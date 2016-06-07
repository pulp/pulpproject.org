---
id: 563
title: Unit Associations
date: 2012-01-10T15:27:48+00:00
author: Jay Dobies
layout: post
guid: http://blog.pulpproject.org/?p=563
permalink: /2012/01/10/unit-associations/
categories:
  - 2.0
---
I spent the bulk of last sprint working on the relationship (read: association) between a content unit and a repository. A lot of the changes and functionality were driven by community feedback. Given their use cases, in 2.0 we&#8217;ll be able to support a lot of queries that aren&#8217;t capable in the 1.0 architecture.

## Association Metadata

In 1.0, the association between a repository and a package is maintained by a simple list of packages on the repository. In 2.0, this relationship is its own entity, allowing us to capture metadata about the association itself. This metadata can be easily expanded in the future, but for now we track the following:

### Association Creation Timestamp

Timestamp of when the association between repository and unit was first created.

### Association Update Timestamp

The methods to create an association is idempotent; if the association already exists, a second isn&#8217;t created and no error is raised. The one change that occurs is that the &#8220;updated&#8221; field on the association is changed to reflect when the association call was made.

This can be used as a mechanism for tracking the last time an association was confirmed, which can then be used to ask questions such as &#8220;Show me all units that have not been present in the external source in the last 3 months.&#8221;

### Owner

I&#8217;m starting to work through the scenarios surrounding user associations v. those created through an importer. User associations can be either by a unit upload or by copying the associations from an existing repository. I wanted to be able to track both the type of owner (user v. importer) and the ID of the owner for auditing purposes.

This has other implications as well. An importer can only affect its own associations, preventing the possibility that the importer will undo explicit repository additions made by a user. It is possible that a unit will be associated multiple times with a single repository, each time by a different owner. The unit association query provides the ability to filter on owner metadata or to have Pulp remove the duplicate associations before returning the results.

## Unit Association Query

The unit association query, or more specifically the criteria that drives the query, is used in a number of places beyond showing a user the contents of a repository. At very least, the query is used in the following places:

  * Providing users with the ability to list or search for specific units in a repository.
  * Accessible to importers through the sync conduit to be used when determining the current state of the repository during a sync.
  * Similarly, accessible to distributors through the publish conduit to give the plugin the flexibility it needs to get the data necessary to publish the repository.
  * The criteria document is used in the 2.0 evolution of repository cloning to let users select a custom set of units to migrate to the new repository.

I&#8217;ve mentioned the criteria a number of times now. The criteria refers to a JSON document passed to Pulp in the unit association query to drive the query and returned results. The following capabilities are provided:

  * **Filtering** &#8211; Filtering can be done on both the association metadata as well as the unit metadata itself. MongoDB syntax is used directly in the criteria document, giving users access to useful constructs such as $and, $or, $in, $gt, $lt &#8212; you get the idea. If you can filter on it using Mongo, you can filter on it for the unit association query too.
  * **Sorting** &#8211; Sorting can be done on either the association metadata or the unit metadata; within a particular metadata category, however, multiple sort fields can be provided.
  * **Skip &#038; Limit** &#8211; Used in conjunction with each other, the skip and limit fields can be used to achieve server-side pagination driven at the database level.
  * **Field Limiting** &#8211; A list of association metadata and/or unit metadata fields can be provided to limit the returned data to save on bandwidth and memory costs.

### Examples

While the above list describes the supported functionality, I&#8217;ve found it&#8217;s often easier to think in terms of the sorts of questions that can be supported by it. Below are a few of the query use cases (and the respective JSON criteria document) I wanted to answer with the query functionality.

The NEVRA for all RPMs in the repository, sorted by name (a basic web UI for RPMs in a repo):

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="json" style="font-family:monospace;">{ "type_ids" : ["rpm"],
  "fields" : { "unit" : ["name", "epoch", "version", "release", "arch"] },
  "sort" : { "unit" : { "name" : "ascending"} }
}</pre>
      </td>
    </tr>
  </table>
</div>

All units added to the repository more than a month ago, regardless of type (too lazy to type out the date subtraction code):

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="json" style="font-family:monospace;">{ "filters" : { "association" : {"created" : {"$lt" : &lt;now - 30 days&gt;}}} }</pre>
      </td>
    </tr>
  </table>
</div>

Basic pagination (on page 4 showing 25 items per page):

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="json" style="font-family:monospace;">{ "limit" : 25, "skip" : "75" }</pre>
      </td>
    </tr>
  </table>
</div>

All noarch RPMs:

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="json" style="font-family:monospace;">{ "type_ids" : ["rpm"],
  "filters" : { "unit" : { "arch" : "noarch" }}
}</pre>
      </td>
    </tr>
  </table>
</div>

User selected multiple sort columns to sort RPMs by name with the most recent version first:

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="json" style="font-family:monospace;">{ "type_ids" : ["rpm"],
  "sort" : { "unit" : {"name" : 1, "version" : -1, "epoch" : -1}}
}</pre>
      </td>
    </tr>
  </table>
</div>

All user-uploaded RPMs in a repository, showing the most recently uploaded ones first:

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="json" style="font-family:monospace;">{ "type_ids" : ["rpm"],
  "filter" : { "association" : { "owner_type" : "user" }},
  "sort" : { "association" : { "owner_type" : "descending" }}
}</pre>
      </td>
    </tr>
  </table>
</div>

Units uploaded by a system admin sometime in November:

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="json" style="font-family:monospace;">{ "filters" : { "association" : { "owner_type" : "user", "created" { "$gte" : "November 1", "$lte" : "November 30"}}}}</pre>
      </td>
    </tr>
  </table>
</div>

Units uploaded by a rogue system admin:

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="json" style="font-family:monospace;">{ "filters" : { "association" : "owner_id" : "evil-jdob" } }</pre>
      </td>
    </tr>
  </table>
</div>

## Association from Repository

Let&#8217;s get this out of the way now: I haven&#8217;t thought of a good name for this feature. It&#8217;s a new spin on cloning, allowing the user to specify a criteria document to dictate which units from a source repository should be imported into a destination repository. I&#8217;m open to suggestions <img src="https://www.pulpproject.org/wp-includes/images/smilies/simple-smile.png" alt=":)" class="wp-smiley" style="height: 1em; max-height: 1em;" />

As I&#8217;ve mentioned a few times now, the criteria document format is the same as for the unit association query. This lets a user tweak the criteria and see matching units in the source repository. Once the criteria is acceptable, the same document is then fed back into Pulp to be used to associate those units into a new repository.

It&#8217;s important to note that the destination repository&#8217;s importer is notified of the new associations. This gives the importer a hook to do whatever setup work it needs to do to accomodate the newly associated units. I won&#8217;t go into the details of the plugin implementation here, but the docstrings for the `Importer` base class should provide enough guidance if you&#8217;re curious.

## Next Steps

When I get the time, it really shouldn&#8217;t take long to add the infrastructure to be able to save named copies of criteria documents to the database. Once that is in place, adapting the query to accept a previously saved criteria document instead of respecifying it each time should be a pretty trivial change.

At that point, I&#8217;d like to add some small variable support to the criteria document. This is mostly necessary for any searches that focus around dates. A saved query that shows all units added since January 1st starts to lose its usefulness over time. However, the ability to specify &#8220;in the last 30 days&#8221; and have Pulp resolve that at execution time is much more useful.

I haven&#8217;t gotten any of this on the Pulp backlog yet, much less scheduled for a sprint, but it&#8217;s definitely functionality that&#8217;s in my head and pretty achievable given the way the current code is written.