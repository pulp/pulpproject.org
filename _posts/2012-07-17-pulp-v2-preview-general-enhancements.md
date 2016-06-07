---
id: 712
title: 'Pulp v2 Preview: General Enhancements'
date: 2012-07-17T18:20:21+00:00
author: Jay Dobies
layout: post
guid: http://blog.pulpproject.org/?p=712
permalink: /2012/07/17/pulp-v2-preview-general-enhancements/
categories:
  - 2.0
  - Spotlight
---
<div style="font-style:italic">
  The Pulp team is gearing up for the first community release of the v2 code base. Over the next few weeks I&#8217;ll be highlighting some of the new features in the v2 release. As with any blog entry, this is scoped to a point in time and the specific details are subject to some change before release. The overall concepts, however, should remain consistent despite any tweaks we make.
</div>

## HTTP and HTTPS Support

Pulp now supports serving repositories over both HTTP and HTTPS. This is selected on a per repository basis as part of its configuration and can be changed at any time. Repositories can be served over HTTP, HTTPS, or both (only repositories served over HTTPS can be configured as protected repositories due to the protection scheme used).

## Feed URL Changing

A fix that has already gotten a lot of good karma for Pulp, the restrictions on changing a feed URL have been removed. The feed URL can be changed at any time regardless of whether or not the repository has been synchronized.

## Multiple Schedule Support

Pulp&#8217;s sync schedule support has been modified to support multiple sync schedules per repository. Intervals can still be specified (now including increments of months or years), however for desired behavior that can not be broken down into intervals, multiple schedules may be created. For example, synchronizing a repository on the 7th and 21st of every month can now be achieved by defining two schedules: 2012-01-07T00:00:00Z/P1M and 2012-01-21T00:00:00Z/P1M.

## REST API Improvements

The REST APIs in v2 follow REST conventions much more closely than in the past. The REST API documentation has been rewritten using reST and Sphinx and is significantly more flushed out, including example request and response bodies. The REST API documentation can be found at: <a href="http://pulpproject.org/v2/rest-api/" target="new">http://pulpproject.org/v2/rest-api/</a>

## Improved Metadata Regeneration

To be clear, this is not scheduled to be included in the initial community release but will be in a subsequent release. We have investigated replacing our usage of createrepo with a new, faster implementation named <a href="http://kojipkgs.fedoraproject.org/packages/createrepo_c/0.1.5/" target="new">createrepo_c</a>. The new implementation is nearly complete and, once it is, Pulp will integrate support for it to greatly increase metadata generation tasks.