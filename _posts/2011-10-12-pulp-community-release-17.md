---
id: 454
title: Pulp Community Release 17
date: 2011-10-12T13:23:18+00:00
author: Todd Sanders
layout: post
guid: http://blog.pulpproject.org/?p=454
permalink: /2011/10/12/pulp-community-release-17/
categories:
  - Releases
---
[<img class="size-full wp-image-457 aligncenter" title="Pulp Community Release 17" src="http://website-pulp.rhcloud.com/wp-content/uploads/2011/10/cr-17.png" alt="Pulp Community Release 17" width="447" height="161" srcset="http://www.pulpproject.org/wp-content/uploads/2011/10/cr-17-300x108.png 300w, http://www.pulpproject.org/wp-content/uploads/2011/10/cr-17.png 447w" sizes="(max-width: 447px) 100vw, 447px" />](http://website-pulp.rhcloud.com/wp-content/uploads/2011/10/cr-17.png)

## Installation & Upgrade

Installation instructions can be found in the [Pulp User Guide](http://pulpproject.org/ug/UGInstallation.html#installation) at the [Pulp project web site.](http://www.pulpproject.org)

As usual, upgraded environments must run `pulp-migrate` to upgrade to the latest database changes.

## Enhancements

### Pulp Exporter

  * Ability to export repository content (rpms, errata, packagegroups, distributions and custom metadata) 
  * User facing API to invoke exports on a repository
  * CLI support to invoke exports via \`pulp-admin repo export\`
  * Ability to cancel on going exports on a repository

### Distributions

  * New fields added to distribution model &#8211; &#8220;Family&#8221;, &#8220;Variant&#8221; and &#8220;Version&#8221;

### Cancel Clone

  * Ability to gracefully cancel a running clone using &#8216;pulp-admin repo cancel_clone&#8217; command 
  * Ability to snapshot and restart a running clone in case of server restart 

## Build Versions

  * Pulp: 0.237-4
  * Grinder: 0.119
  * Gofer: 0.50