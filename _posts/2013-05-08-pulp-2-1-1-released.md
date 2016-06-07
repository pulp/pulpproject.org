---
id: 1021
title: Pulp 2.1.1 Released
date: 2013-05-08T13:03:16+00:00
author: Jay Dobies
layout: post
guid: http://www.pulpproject.org/?p=1021
permalink: /2013/05/08/pulp-2-1-1-released/
categories:
  - Releases
---
The Pulp team is proud to announce the availability of its 2.1.1 release. This is a minor release containing the following:

  * Another round of performance improvements to the yum repository sync process. Our testing saw the following changes: 
      * Sync time reduced from ~65 mins to ~35 mins. In Pulp 2.1.0 there is a significant delay as the yum metadata is transferred from Grinder to Pulp. To the user this appeared as if the process was frozen. The metadata retrieval is now done in Pulp itself, greatly reducing this initial metadata processing step.
      * Resyncing an existing repository reduced from ~25-30 mins to ~6-10 mins.
      * Memory footprint reduced by 30%.
  * Enhancements to our applicability API to support the next iteration of <a href="http://www.katello.org/" target="new">Katello</a>.
  * Additional <a href="https://bugzilla.redhat.com/buglist.cgi?list_id=1346169&#038;classification=Community&#038;target_release=2.1.1&#038;query_format=advanced&#038;bug_status=VERIFIED&#038;bug_status=RELEASE_PENDING&#038;bug_status=CLOSED&#038;product=Pulp" target="new">bug fixes and enhancements.</a>

The release can be found in our stable repositories (pulp-v2-stable in the .repo file if you’ve downloaded it). Installation and upgrade instructions can be found in the [installation section of the user guide](https://pulp-user-guide.readthedocs.org/en/pulp-2.1/installation.html). All of our documentation can be accessed from [our documentation summary page](/docs/).

## Pulp 2.2.0

Our May sprint is the our last feature and development sprint for the 2.2.0 release. Starting the first week in June, beta builds for this release will be available. I&#8217;ll provide more of a preview on what to expect when I have a bit more time.