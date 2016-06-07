---
id: 576
title: Pulp v1.0 Release
date: 2012-02-24T19:58:31+00:00
author: Jay Dobies
layout: post
guid: http://blog.pulpproject.org/?p=576
permalink: /2012/02/24/pulp-v1-0-release/
categories:
  - Releases
---
<img src="http://website-pulp.rhcloud.com/wp-content/uploads/2012/02/v1.png" alt="" title="Pulp v1.0" width="295" height="161" class="alignnone size-full wp-image-577" />

The team is proud to announce the availability of the Pulp 1.0 release.

## Quick Links

  * [Installation Instructions](http://pulpproject.org/ug/UGInstallation.html#installation)
  * [User Guide](http://pulpproject.org/ug/index.html)
  * [Project Site](http://pulpproject.org/)
  * [Mailing List](https://www.redhat.com/mailman/listinfo/pulp-list)
  * Chat: #pulp on Freenode

## What is Pulp?

Pulp is a repository and consumer management system. On the repository management side of things, the Pulp server will synchronize yum repositories from external sources, either manually initiated or at recurring scheduled intervals. Repositories can be manually created and populated with user-uploaded content as well in the case there is no external source. These repositories are then hosted by either the Pulp server directly or synchronized to an external server (called a Content Deliver Server or &#8220;CDS&#8221;) to better control the bandwidth required for the repository. Repositories can be made available to anybody or secured, requiring yum clients to present a valid x.509 certificate to be granted access.

Pulp also provides a number of features designed to facilitate the management of clients (called &#8220;consumers&#8221;) using Pulp-provided repositories. Using the Pulp consumer scripts, a client machine registers itself with the Pulp server and uploads a profile describing its installed packages. As the consumer binds itself to Pulp repositories, the server keeps track and can then provide information to administrators about applicable errata available to a given consumer. Each consumer is connected to a message bus that the Pulp server can use to remotely trigger operations on the consumer such as package installations and updates.

## Going Forward

As with any release, we recognize issues are going to arise. We&#8217;ve already set up a <a href="https://bugzilla.redhat.com/buglist.cgi?query_format=advanced&#038;bug_status=NEW&#038;bug_status=ASSIGNED&#038;bug_status=MODIFIED&#038;bug_status=ON_DEV&#038;bug_status=ON_QA&#038;bug_status=VERIFIED&#038;bug_status=RELEASE_PENDING&#038;bug_status=POST&#038;bug_status=CLOSED&#038;version=1.1.0&#038;product=Pulp&#038;classification=Community" target="new">1.1 version in bugzilla</a> so we can begin to plan out what support for the v1 stream will look like.

Outside of that, the team is focused on the re-architecting of Pulp for v2. The theme for v2 is &#8220;Pulp as a Platform&#8221; providing mechanisms for developers to use Pulp infrastructure for non-RPM content types. Of course, Pulp v2 will this architecture to continue to handle its existing content types, including RPMs and errata, as well as many refinements to its existing subsystems. More information and progress on the v2 stream can be found [in the 2.0 category of this blog.](/category/2-0/)

## Thank You

The pulp.spec file was first committed to git on May 20, 2010, which I suppose represents as good a birth date for Pulp as any. In the time since that commit our small 4 person team has over doubled in size. But more importantly, a community has sprung up in the process. Today, the chat room and mailing list are buzzing with comments ranging from using Pulp as a standalone product to embedding Pulp as part of a larger initiative (see below) to hacking up the pieces of Pulp to meet more specific needs.

It&#8217;s been awesome to watch it all take shape. I hesitate to thank individuals by name for fear of missing any, but realize that all contributions, from feature requests to bug reports (and everything in between), are greatly appreciated by the team. The community has helped Pulp become a better product; that&#8217;s simply a fact and there&#8217;s no disputing it. So thank you to everyone for your interest in Pulp, feedback on what you like (and don&#8217;t), keeping us company in the chat room, and general patience as so few balance the needs of so many. Y&#8217;all rock.

### Powered by Pulp

Below are a few examples of projects using Pulp as part of the back end:

  * <a href="https://fedorahosted.org/pulpdist/" target="new">PulpDist</a>
  * <a href="https://github.com/stpierre/sponge" target="new">Sponge</a>
  * <a href="http://katello.org/" target="new">Katello</a>

## Upgrades from Community Releases

Upgrading a Community Release to the 1.0 release is supported. Be sure to run `pulp-migrate` and restart Apache once the new packages are installed.