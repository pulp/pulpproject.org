---
id: 1409
title: Pulp 2.8.6 Generally Available
date: 2016-07-18T19:47:54+00:00
author: Sean Myers
layout: post
guid: http://www.pulpproject.org/?p=1409
permalink: /2016/07/18/pulp-2-8-6-generally-available/
categories:
  - Releases
---
Pulp 2.8.6 is now Generally Available in the stable repositories with bug fixes:

<https://repos.fedorapeople.org/repos/pulp/pulp/stable/2.8/>

This release includes a small number of fixes to severe bugs in Pulp Platform,
  
the RPM plugin, and the Docker plugin.

Of particular interest to RPM users are issues #1949 and #2048, two issues
  
that were severely impacting errata-related syncing and publishing. See the
  
list of issues below for more details.

# Upgrading

The Pulp 2 and Pulp stable repositories are included in the pulp repo files:
  
<https://repos.fedorapeople.org/repos/pulp/pulp/fedora-pulp.repo> for fedora 22 & 23
  
<https://repos.fedorapeople.org/repos/pulp/pulp/rhel-pulp.repo> for RHEL 6 & 7

After enabling the desired repository (pulp-stable or pulp-2-stable), you&#8217;ll want
  
to follow the standard upgrade path, with migrations:

<pre>$ sudo systemctl stop httpd pulp_workers pulp_resource_manager pulp_celerybeat
 $ sudo yum upgrade
 $ sudo -u apache pulp-manage-db
 $ sudo systemctl start httpd pulp_workers pulp_resource_manager pulp_celerybeat</pre>

# Issues Addressed

Pulp

  * 2056 Distribution storage path migration re-links .treeinfo files incorrectly.
  * 2012 alternate content source refresh fails with json serialization error
  * 2031 possible incorrect URL param parsing by streamer

RPM Support

  * 2048 Errata update failure during sync or upload
  * 1949 re-publish takes longer than expected
  * 2032 pulp sets reboot_suggested to True for all errata when syncing from published repo

View the full list in Redmine:
  
<http://bit.ly/29vs1bE>