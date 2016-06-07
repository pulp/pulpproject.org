---
id: 1251
title: Pulp 2.8.1 has been released!
date: 2016-04-05T20:39:44+00:00
author: Sean Myers
layout: post
guid: http://www.pulpproject.org/?p=1251
permalink: /2016/04/05/pulp-2-8-1-has-been-released/
categories:
  - Uncategorized
---
Pulp 2.8.1 has been published to the stable repositories:

<https://repos.fedorapeople.org/repos/pulp/pulp/stable/2.8/>

## Changes

The following Redmine (pulp.plan.io) issues are addressed in 2.8.1:

  * Pulp 
      * 1734 The Pulp streamer logs a traceback when a ConnectionError occurs.
      * 1764 SELinux denial on Celery attempting to read resolv.conf
      * 1737 The checksum/checksum_type is not being included in the lazy catalog
      * 1735 Downloading the CentOS base repository fails with lazy and &#8211;verify-all
      * 1776 sync_complete response has changed
  * Puppet Support 
      * 1607 Puppet install distributor fails when deleting repository if not published
  * RPM Support 
      * 1138 Install applicable errata consumer task does not report kernel packages were updated
      * 1366 Advisory package list doesn&#8217;t match packages in the repository
      * 1548 published errata contain packages not in repo
      * 1789 Sync fills up <i class="moz-txt-slash"><span class="moz-txt-tag">/</span>var/cache/pulp<span class="moz-txt-tag">/</span></i> with rpms

More details regarding these issues can be found in Redmine at the following URL: <http://bit.ly/1LHtwWp>{.moz-txt-link-freetext}