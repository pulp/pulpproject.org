---
id: 426
title: 'Pulp Rearchitecture &#8211; Sprint Update'
date: 2011-09-06T18:20:38+00:00
author: Jay Dobies
layout: post
guid: http://blog.pulpproject.org/?p=426
permalink: /2011/09/06/pulp-rearchitecture-sprint-update/
categories:
  - Uncategorized
---
As much as I wish the new architecture would be in place overnight, it&#8217;s not gonna happen. So instead of living in a silo for multiple sprints, I wanted to give a quick update of where we got in the last sprint and how Pulp is going to look in the future.

<p align="center">
  <a href="http://website-pulp.rhcloud.com/wp-content/uploads/2011/09/pulp-2-arch-sprint-1.png"><img src="http://website-pulp.rhcloud.com/wp-content/uploads/2011/09/pulp-2-arch-sprint-1.png" alt="" title="Pulp Rearchitecture - Sprint 1"class="aligncenter size-full wp-image-430" /></a>
</p>

The above graphics shows things at a few levels, ranging from internal guts to plugin-developer interaction to client. The scope feels a bit odd, but it was the simplest way to summarize the work for the sprint.

## Type Definitions

We decided on a first pass at a [type definition format](https://fedorahosted.org/pulp/wiki/TypeDescriptor). The idea was not to include a ton of things we _may_ need eventually, but rather to get the infrastructure in place to load the definitions and begin to configure the server accordingly.

For instance, one area we went back and forth on was defining the metadata that will be tracked for units of a given type. Eventually I can see a use case for it in terms of type discovery, but for now it was omitted. We will flush it out in a future iteration.

The important part about the current implementation is that it provides clues to the server on how to configure the database to help optimize queries. I won&#8217;t go into the detail here but will post something in the future.

## Importers &#038; Distributors

The basics of the importer and distributor plugins were implemented this sprint, including a first pass at their APIs and the server-side code to load and track them. Each importer/distributor consists of Python code implementing the necessary APIs and an optional JSON configuration file for the plugin as a whole.

The consumer-side of things, tentatively called &#8220;trackers&#8221;, still needs some whiteboard time and is not included.

## Repository Functions

The biggest changes coming to Pulp in terms of public-facing APIs revolve around repository lifecycle. The client box on the right side of the graphic above describes some of the new calls available which illustrate the new approach to repository creation.

The following describes the new workflow for the creation of a new repository. Keep in mind that this workflow is at the API level; the CLI will eventually simplify this for the end user.

  * A new Pulp repository is created with minimal metadata (unique ID, display name, description, etc.). At this point, the repository is largely a placeholder in Pulp as it doesn&#8217;t have any type-specific functionality attached to it.
  * An importer is configured for the repository. The type of importer and its configuration are associated with the repository. This effectively governs what content types a repository can contain. The current thinking is to limit a repository to a single importer due to the sheer amount of potential complications a multiple-importer model would introduce (that said, we&#8217;ll see how things go over the next few sprints).
  * One or more distributors are configured for the repository. These indicate the means through which content in the repository will be published. The common example I keep citing here is that a repository may have an HTTP-based distributor to make the content available through the web and an ISO-based distributor that will package up the contents of the repository for offline distribution.

The other major change is that previous concept of &#8220;repo sync&#8221; is now split into two concepts: sync and publish. It&#8217;s still possible to combine the two at execution time, but conceptually there is a much bigger divide between the idea of bringing content into a repository (sync) and making it available to end users or other systems (publish).

## REST APIs

Ya, so, they&#8217;re on the picture, but not exposed yet. All of the above functionality is implemented in the server code but I didn&#8217;t quite get around to wiring up the REST APIs for them yet. That part should be reasonably simple and should be addressed next sprint, at which point I can actually demo all of this new functionality in action.

That said, the graphic does highlight a few conceptual areas the APIs are focusing on in the near future:

  * Repository lifecycle (create, set\_importer, add\_distributor) have already been discussed in this post. The only note to make is that set\_importer versus add\_distributor was intentional to convey a single importer per repository v. multiple distributors.
  * Repository sync and distribute. Again, these were discussed previously so I won&#8217;t go into any more detail.
  * Query functionality is becoming a highly requested feature in Pulp. Both the repository and content unit search capabilities are being looked at for flexibility and performance in the coming sprints.
  * Ultimately a Pulp server will have an array of discovery APIs to determine the sorts of capabilities installed on it, such as type definitions and importer/distributor plugins loaded.

## Conclusion

There&#8217;s still much work to be done which means now is the perfect time to help guide Pulp towards your needs. We&#8217;re always looking for feedback at any of the normal channels ([mailing list](https://www.redhat.com/mailman/listinfo/pulp-list), #pulp on freenode).