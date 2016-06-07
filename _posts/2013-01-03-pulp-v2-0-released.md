---
id: 927
title: Pulp v2.0 Released
date: 2013-01-03T14:41:24+00:00
author: Jay Dobies
layout: post
guid: http://www.pulpproject.org/?p=927
permalink: /2013/01/03/pulp-v2-0-released/
categories:
  - Releases
---
<img src="http://www.pulpproject.org/wp-content/uploads/2013/01/v2.png" alt="v2" width="309" height="161" class="alignnone size-full wp-image-928" srcset="http://www.pulpproject.org/wp-content/uploads/2013/01/v2-300x156.png 300w, http://www.pulpproject.org/wp-content/uploads/2013/01/v2.png 309w" sizes="(max-width: 309px) 100vw, 309px" />

I am extremely excited to announce the immediate availability of Pulp 2.0. This version of Pulp represents an entirely new approach, evolving Pulp into an extendable framework for content management. No longer limited to RPM-related content, users have the ability to customize their Pulp installation to handle their specific content management needs. This release sees the first usage of this ability, providing support for both RPM-related content (RPMs, errata, etc.) as well as handling Puppet modules, including synchronizing all or a subset of modules hosted at Puppet Forge. 

## Quick Links

  * [Documentation Home](http://www.pulpproject.org/docs/)
  * [Mailing List](https://www.redhat.com/mailman/listinfo/pulp-list)
  * Chat: #pulp on Freenode

## Existing Users

Pulp v2 beta users can upgrade to the release version. The .repo files have been updated with the v2 stable repository as the default enabled repository. Alternatively, simply enable the `[pulp-v2-stable]` repository definition in your existing copy.

Unfortunately, this release does not support upgrading from a Pulp v1 installation. Work is being done to support this in the next two months, but it didn&#8217;t make it into this release.

## What is Pulp?

<span style="font-style: italic">(I shamelessly copied this directly from www.pulpproject.org, so don&#8217;t be surprised if this looks familiar)</span>

Pulp is a platform for managing repositories of content, such as software packages, and pushing that content out to large numbers of consumers. If you want to locally mirror all or part of a repository, host your own content in a new repository, manage content from multiple sources in one place, and push content you choose out to large numbers of clients in one simple operation, Pulp is for you!

Pulp has a well-documented REST API and command line interface for management.

Pulp is completely free and open-source, and we invite you to join us on GitHub!

### With Pulp Server, you can&#8230;

  * Pull in content from existing repositories to the Pulp server. Do it manually one-time-only or on a recurring schedule.
  * Upload new content to the Pulp server.
  * Mix and match uploaded and imported content to create new repositories, then publish and host them with Pulp.
  * Publish your content as a web-based repository, to a series of ISOs, or in any other way that meets your needs.

### Pulp Consumers can&#8230;.

  * Register with Pulp server, bind to a repository, then have their installed content managed from the server.
  * Pulp server centrally manages and reports on what content is on each consumer.