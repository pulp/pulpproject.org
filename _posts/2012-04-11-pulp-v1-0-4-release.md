---
id: 611
title: Pulp v1.0.4 Release
date: 2012-04-11T20:55:50+00:00
author: Jay Dobies
layout: post
guid: http://blog.pulpproject.org/?p=611
permalink: /2012/04/11/pulp-v1-0-4-release/
categories:
  - Releases
---
A bug fix release has been built to the v1 stable repository (<a href="http://repos.fedorapeople.org/repos/pulp/pulp/v1/stable/" target="new">http://repos.fedorapeople.org/repos/pulp/pulp/v1/stable/</a>). The fixed bug list can be found below.

## Upgrade

One of the bug fixes present in this release is to address a situation that may arise when two similarly named packages have similar checksums (a small chance however we actually managed to run into it). As part of that fix, existing packages need to be migrated to a new directory structure. This release contains a script to perform that migration. Instructions on running that script can be found [on the Pulp wiki.](https://fedorahosted.org/pulp/wiki/PackagePathUpdate#Howtorunmigration)

**The migration script must be run before attempting to run a sync with the new release to avoid duplicating content.**

## Quick Links

  * [Installation Instructions](http://pulpproject.org/ug/UGInstallation.html#installation)
  * [User Guide](http://pulpproject.org/ug/index.html)
  * [Project Site](http://pulpproject.org/)
  * [Mailing List](https://www.redhat.com/mailman/listinfo/pulp-list)
  * Chat: #pulp on Freenode

## Bug Fixes

  * [809628](https://bugzilla.redhat.com/show_bug.cgi?id=809628) &#8211; fixed error message formatting
  * [809195](https://bugzilla.redhat.com/show_bug.cgi?id=809195) &#8211; added post_sync dequeue hook to scheduled syncs
  * [807332](https://bugzilla.redhat.com/show_bug.cgi?id=807332) &#8211; the continuing story of the post sync url firing off actuall POST
  * [807516](https://bugzilla.redhat.com/show_bug.cgi?id=807516) &#8211; deleting a repository does not affect repoids associated with a package
  * [806976](https://bugzilla.redhat.com/show_bug.cgi?id=806976) &#8211; fix overlap in /etc/pulp files between main and sub-packages
  * [805740](https://bugzilla.redhat.com/show_bug.cgi?id=805740) &#8211; generate metadata on re-promotion with filters
  * [805922](https://bugzilla.redhat.com/show_bug.cgi?id=805922) &#8211; adding sync logic to look for server directory during clone/local
  * [802454](https://bugzilla.redhat.com/show_bug.cgi?id=802454) &#8211; added post\_sync\_url and post_sync dequeue hook for POSTing task
  * [798656](https://bugzilla.redhat.com/show_bug.cgi?id=798656) &#8211; include full checksum when constructing package paths
  * [801184](https://bugzilla.redhat.com/show_bug.cgi?id=801184) &#8211; error messages when installing selinux RPM after its already been installed
  * [799120](https://bugzilla.redhat.com/show_bug.cgi?id=799120) &#8211; added package filtering at package import stage
  * [797929](https://bugzilla.redhat.com/show_bug.cgi?id=797929) &#8211; add requirement on semanage command
  * [795819](https://bugzilla.redhat.com/show_bug.cgi?id=795819) &#8211; account for the relativepath from the metadata location field

## Version 1.1 In Development

Bug/RFE triage and planning for our 1.1 release has completed and work will begin this sprint towards the included fixes. Aligned bugs can be found [using this bugzilla query](https://bugzilla.redhat.com/buglist.cgi?query_format=advanced&#038;bug_status=NEW&#038;bug_status=ASSIGNED&#038;bug_status=MODIFIED&#038;bug_status=ON_DEV&#038;bug_status=ON_QA&#038;version=1.1.0&#038;product=Pulp&#038;classification=Community) and while there are no known changes to be made to that list, it is, as always, subject to change. Current plans are to target a June 1 release date. More information will be available on this blog as it becomes available.