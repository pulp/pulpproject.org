---
id: 535
title: Importer Sync APIs
date: 2011-11-21T22:05:32+00:00
author: Jay Dobies
layout: post
guid: http://blog.pulpproject.org/?p=535
permalink: /2011/11/21/importer-sync-apis/
categories:
  - 2.0
---
One of my stories for the sprint is to revisit the the APIs for interacting back and forth between a plugin and the Pulp server during a sync operation. The goal is too make sure they are not only sufficient from a functionality perspective, but designed in such a way as to not absolutely destroy performance. As you can imagine, that last part takes a little bit of thought and creativity. I want to avoid making an API that is awesome for RPMs and near unusable for everything else (I had to work with a guy once who wrote APIs that only he would want to use, I&#8217;m not getting back into that situation again), so I figured I&#8217;d lay out how I see most sync operations taking place and use that as a starting point to talk with current plugin writers on their needs.

Below is what I imagine is a rough outline of what most importer plugins will look like. It will reference how the APIs from importer back into Pulp* look today, but realize the driving force behind this is refining those APIs, so they&#8217;ll be changing in the next few days as I digest this.

_* The term &#8220;conduit&#8221; is used to refer to the object passed to the plugin that exposes the Pulp functionality it will want to use. Each conduit instance is scoped to the repository being synchronized, so don&#8217;t be surprised to not see the repository ID in any of the conduit signatures below._

## Step 1: Query External Feed

I figure the first step just about any importer will want to do is query the external source from which units will be imported. Already in place is the ability for each importer type to accept a custom set of configuration options on a per repo basis, so the importer should already have everything it needs to find out what work it has to do.

In the RPM world, this involves fetching the repository&#8217;s metadata and starting to break down the file list.

## Step 2: Current State of the Repository

Once we know what the external source says the repository should look like, we need to know what the repository _currently_ looks like. On first sync this will be empty, but the common case is incremental updates to a previously synchronized repository.

**Conduit Call: get\_unit\_keys\_for\_repo**
  
A unit key is a dictionary of key-value pairs that uniquely identifies a content unit in the context of its content type. The keys contained within will vary by content type depending on what makes sense. I suppose you could call it the &#8220;natural&#8221; key as compared to the unit ID which is talked about later.

This is a single call the plugin will use to determine the unit keys of all units currently associated with the repository. The &#8220;single call&#8221; part of that is important since I&#8217;m trying to keep plugin writers from having to utterly smash the database, a theme you&#8217;ll see repeated throughout this write up.

## Step 3: Resolve Repository Changes

Using the above two pieces of information (what should be in the repository and what currently is), the plugin will want to resolve the differences. It should be noted that I&#8217;m expecting this to happen almost entirely in memory at this point and not incur massive amounts of Pulp database hits (this is the driving reason behind the single call in step 2 rather than giving the plugin writer calls to the database to test units on a case by case basis).

In some cases, I expect this to be an early exit. If you know there&#8217;s no chance of content metadata changing and needing to update the unit&#8217;s metadata, you can punch out early if there are no new units.

By now, I expect the plugin writer to have two lists: content units to be added (and updated, you&#8217;ll see what I mean in a minute) in the repository and units to be unassociated from the repository.

## Step 4: Add or Update Units

My gut reaction* is that in most cases, the plugin won&#8217;t care if a unit is being added or updated. It just wants the correct metadata in the database and the unit associated to the repository. So the current API has a single idempotent call that does either and add or update; the Pulp server makes the call and the plugin writer&#8217;s life is easier.

_* My gut may be totally wrong, so feel free to argue this point._

I suspect I&#8217;ll be changing this to provide both fine-grained semantics as well as this sort of utility combination call. For now though, the idea is that the plugin will:

  1. Add (or update) the unit to Pulp. This will create an entry in the database for the unit if one didn&#8217;t already exist, but it is orphaned (doesn&#8217;t belong to a repository) by default.
  2. Associate the unit to the repository being synchronized.

Even now as I look at the above list I wonder why I didn&#8217;t just take the extra step of adding another aggregate call that will add/update and associate all in one, leveraging the fact that they are all idempotent so the plugin writer only really needs to concern themselves with the post-conditions and not the correct incantation of conduit calls to get there.

The other big operation taking place in this step is the downloading of the unit&#8217;s bits. This is left entirely to the plugin itself to implement, though in the future if there are any utilities we can provide they may be in some form of supplemental package.

The noteworthy part of the download operation is that the plugin asks Pulp for the final say on where to store the unit. The plugin determines the relative path that will keep one unit&#8217;s bits from conflicting with another&#8217;s (segregated by type), but Pulp is asked for the actual location on disk.

Also notice that the download bits part is optional. If a plugin wants to use Pulp strictly for cataloging content and not actually doing any bits movement, that&#8217;s totally possible with these APIs.

This area is my biggest concern in terms of Pulp overhead&#8217;s influence on the overall performance. So far it seems like the add/update call can&#8217;t cleanly be batched, but I&#8217;m also ok with the concept of 2 database hits per unit being added (at some point we&#8217;re going to have to actually use the database). What we have batched are the associate calls, which means that until that uber add/update/associate call exists, it&#8217;s only a single call to associate the running list of added/updated units to the repository.

**Conduit Call: request\_unit\_filename(content\_type, relative\_path)**
  
Not much else to explain here. This prevents the plugin from having to care where the admin configured Pulp to store content and lets the plugin focus on the important part: uniquely storing unit bits without duplication.

**Conduit Call: add\_or\_update\_content\_unit(type\_id, unit\_key, filename, custom\_unit\_data)**
  
(This isn&#8217;t exactly how it appears in git right now, but the git version is definitely busted, so I&#8217;ll talk to where I want to go with it.)

The unit\_key has been discussed earlier. The custom\_unit_data field is a dictionary containing whatever metadata the plugin wants to store for that unit; there is no set schema for what goes in here. The filename may seem odd, but I&#8217;m expecting the purging of orphaned packages to be a Pulp operation (in other words, not contacting a plugin). That means Pulp needs to know where on disk the bits are stored. In the case of metadata-only units, this will be None and Pulp will simply remove the database entry on orphaned unit cleanup.

**Conduit Call: associate\_content\_unit(type\_id, unit\_id)**
  
The unit ID is Pulp&#8217;s unique ID for the content unit (as compared to the unit key which is the natural key). This is returned from the add/update call which gives that method the side effect of translating unit_key into database-level ID. Again, a batch operation will reduce the database hits for making all of these associations.

## Step 5: Unassociate Removed Units

Using the data from early on, the plugin will know which units are in the repository that are not in the external source. I still have to figure out a solution for how to handle units manually associated with a repository; either the plugin should support a flag that says do not unassociate unknown units or the manually associated units will have a flag indicating they were explicitly associated and the plugin should use that information and not undo user-initiated changes.

**Conduit Call: unassociate\_content\_unit(type\_id, unit\_id)**
  
As I write this I realize that, given my explanation, the plugin writer doesn&#8217;t have the unit\_id for units that are supposed to be removed at this point. My expectation is that they used the get\_unit\_keys\_for\_repo to determine which units need to be removed, but that doesn&#8217;t include the database IDs. So I either need to enhance these APIs to be able to unassociate by unit\_key or give an explicit ID lookup call (that call has the smell of giving a plugin writer enough rope to hang himself with). Like I said, this is a work in progress and I&#8217;m half using this to think out loud <img src="https://www.pulpproject.org/wp-includes/images/smilies/simple-smile.png" alt=":)" class="wp-smiley" style="height: 1em; max-height: 1em;" />

## Step 6: Return a Report

A successful sync_repo call is expected to return an instance of SyncReport. That&#8217;s stored in the newly added sync history tracking on the repository and can be accessed through the REST API. The sync report currently includes three pieces of data specified by the plugin:

  * Number of units added
  * Number of units removed
  * Arbitrary log of the sync

The log is meant to give the user visibility into the sync process itself. Added/removed counts are meant to, among other things, be used as triggers for other things that may need to happen when a repository&#8217;s contents change.

## Other: Repository Working Directory

I didn&#8217;t know where to fit this in, but Pulp will provide each importer with a working directory for each repository. This is meant to store temporary files needed during the synchronize process. For instance, our current RPM sync operation utilizes the repository metadata which can be downloaded and unpacked to this directory. Pulp will take care of cleaning up this directory on repository delete.

# Next Steps

I&#8217;m focused on this for the next day or so, depending on how early I mentally check out for Thanksgiving. I can&#8217;t stress enough how any input is appreciated. Ping me in chat (jdob @ #pulp on Freenode), e-mail pulp-list, comment to this blog, send me a carrier pigeon&#8230; whatever it takes to let me know what you think.