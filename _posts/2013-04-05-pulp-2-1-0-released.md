---
id: 1001
title: Pulp 2.1.0 Released
date: 2013-04-05T15:54:46+00:00
author: Jay Dobies
layout: post
guid: http://www.pulpproject.org/?p=1001
permalink: /2013/04/05/pulp-2-1-0-released/
categories:
  - Releases
---
The Pulp team is proud to announce the availability of its 2.1.0 release. Highlights from this release include&#8230;

  * Pulp Nodes functionality, allowing one Pulp server to manage and push content to other Pulp servers (previously known as a CDS in Pulp v1).
  * Support for Puppet Master consumers to have modules remotely installed and upgraded through the Pulp admin client.
  * Significant speed and memory consumption improvements when synchronizing yum repositories.
  * Support for upgrading from Pulp 1.1 installations.
  * A number of <a href="https://bugzilla.redhat.com/buglist.cgi?resolution=---&#038;resolution=CURRENTRELEASE&#038;classification=Community&#038;target_release=2.1.0&#038;query_format=advanced&#038;bug_status=VERIFIED&#038;bug_status=CLOSED&#038;component=admin-client&#038;component=bindings&#038;component=consumer-client%2Fagent&#038;component=consumers&#038;component=coordinator&#038;component=documentation&#038;component=events&#038;component=nodes&#038;component=okaara&#038;component=rel-eng&#038;component=repositories&#038;component=rest-api&#038;component=selinux&#038;component=upgrade&#038;component=users&#038;component=z_other&#038;product=Pulp&#038;list_id=1253761" target="new">bug fixes</a>. </ul> 
    The [full release notes](https://pulp-user-guide.readthedocs.org/en/pulp-2.1/release-notes.html#pulp-2-1-0) can be found in our user guide. 
    
    The release can be found in our stable repositories (pulp-v2-stable in the .repo file if you’ve downloaded it). Installation and upgrade instructions can be found in the [installation section of the user guide](https://pulp-user-guide.readthedocs.org/en/pulp-2.1/installation.html). All of our documentation can be accessed from [this list](/docs/).
    
    ## Pulp 2.1.1
    
    Work is nearing completion for our 2.1.1 release. This release is primarily to support an upcoming <a href="http://katello.org" target="new">Katello</a> release. Of particular interest will be the inclusion of another round of performance improvements when synchronizing yum repositories. Beta builds for this release are scheduled to begin next week with the release coming in early May.