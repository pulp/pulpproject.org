---
id: 393
title: Generic Content Support
date: 2011-08-08T19:12:33+00:00
author: Jay Dobies
layout: post
guid: http://blog.pulpproject.org/?p=393
permalink: /2011/08/08/generic-content-support/
categories:
  - Uncategorized
---
The aspirations for Pulp have always been larger than simply RPM-related content (&#8220;related&#8221; meaning kickstart trees, errata, etc). Now that we have a strong base of functionality in Pulp, we&#8217;re starting to look at how we can refactor things to make it easier to add support for non-RPM content.

The primary driving need for this is to support [Katello](http://katello.org/) use cases. Of these, the most complicated is the ability to store Puppet modules which consist of a set directory structure and multiple child types. More long-term, however, is the is the goal of supporting user-defined content types. For example, being able to use all of Pulp&#8217;s functionality to support Debian package-based flavors of Linux.

To support all of this, Pulp&#8217;s architecture is changing to a plugin-supported model:

[<img src="http://website-pulp.rhcloud.com/wp-content/uploads/2011/08/pulp-arch.png" alt="" title="Pulp Architecture" width="604" height="242" class="aligncenter size-full wp-image-394" srcset="http://www.pulpproject.org/wp-content/uploads/2011/08/pulp-arch-300x120.png 300w, http://www.pulpproject.org/wp-content/uploads/2011/08/pulp-arch.png 604w" sizes="(max-width: 604px) 100vw, 604px" />](http://website-pulp.rhcloud.com/wp-content/uploads/2011/08/pulp-arch.png)

There&#8217;s a lot going on in that picture, so let&#8217;s start with the colors.

  * Blue or White &#8211; Areas written and supported by the Pulp team. These fall into two categories: the general Pulp platform and the RPM-centric type support.
  * Orange &#8211; The main touch points for developers looking to leverage Pulp for their needs. These include custom administrative tasks through our REST API, custom content retrieval (not necessarily limited to just client systems, but potentially to other systems looking to retrieve Pulp-managed content), and type support.
  * Green &#8211; Interfaces from the outside into the Pulp server.

Starting in the middle, the bulk of Pulp&#8217;s subsystems remain untouched, such as auth and tasking (concurrency, scheduling, etc). The root of the new architecture is found in the type definitions. These indicate to the Pulp server the types of content that will be supported along with some metadata as to what the type represents. The other plugin pieces support one or more different types and together define the scope of what a Pulp installation will do.

The repository handling gets shifted around and split into two main concepts:

  * Import &#8211; The process of taking content and getting it into Pulp. This includes both the ability to load from an external source (whatever that may be) as well as how to process user-uploaded content.
  * Distribution &#8211; The process of taking content from the Pulp server and exposing it. The most easily understood use case is serving content over HTTP, but it&#8217;s also possible to add in a hook that would write to an ISO or be leveraged for use in image creation.

These two pieces function as adapters between Pulp and&#8230; wherever. The importers/distributors can speak whatever protocols are necessary to the outside world and use specific APIs to interact with the Pulp server.

A similar piece is the concept of a Tracker. Much like the importers/distributors provide content management functionality for types, the trackers provide client-side knowledge of types. For instance, the RPM tracker will understand how to read a package profile report for an RPM based system. Another example could be a JBoss-related tracker that understands how to inventory a JBoss installation and its components (JARs, EARs, etc).

The Pulp team plans on providing two admin CLIs. The Reference Implementation (RI) is driven by the types indicated by the Pulp server. This isn&#8217;t intended to be the most efficient UI into using any one particular set of types but rather to showcase the Pulp APIs and functionality.

The RPM Specific CLI is a custom CLI that assumes the default installation of Pulp with RPM support. This CLI expects RPM-related types to be supported and thus will be a much more streamlined UI for RPM needs.

The Katello team is using Pulp&#8217;s APIs to provide a rich front-end experience on top of Pulp&#8217;s backend. And for example purposes, if Debian package type support was added, the same APIs could be used to provide a highly customized experience for managing those packages.

The consumer side of things is similar. The Pulp consumer is being refactored to be a general framework for interacting with the Pulp server. Specific type support can be added depending on what content types should be supported. This support includes things like profiling and installation. And since Pulp&#8217;s APIs are open, the giant &#8220;Other&#8221; block is meant to represent any other wacky usage people can come up with.

These are high aspirations and we recognize that. We&#8217;re not naive to the point of thinking one person drawing an architecture diagram is going to get it all right on the first try. Anything is subject to change as things progress and we&#8217;re continuing to support the current codebase as we work towards getting this in place. The nice part is that we have a number of teams interested in using this for a variety of purposes, so we have a pretty robust pool of use cases to test it out on as we go.

More information can be found on the [Pulp wiki](https://fedorahosted.org/pulp/) near the bottom under &#8220;Generic Content&#8221;.