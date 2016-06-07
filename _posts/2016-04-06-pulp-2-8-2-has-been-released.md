---
id: 1254
title: Pulp 2.8.2 has been released!
date: 2016-04-06T21:39:58+00:00
author: Sean Myers
layout: post
guid: http://www.pulpproject.org/?p=1254
permalink: /2016/04/06/pulp-2-8-2-has-been-released/
categories:
  - Uncategorized
---
Pulp 2.8.2 has been published to the stable repositories:

<https://repos.fedorapeople.org/repos/pulp/pulp/stable/2.8/>

## Changes

Pulp 2.8.2 addresses a security vulnerability that was found after the
  
announcement of the 2.8.1 release candidate. More information about this
  
vulnerability, including upgrade instructions, can be found at
  
the following address:

<http://www.openwall.com/lists/oss-security/2016/04/06/3>

Satellite users are unaffected by this vulnerability:

<https://access.redhat.com/security/cve/cve-2016-3095>

(It&#8217;s currently in the reserved state, but should be opened up shortly.)

From the access.redhat.com CVE page:

&#8220;This issue did not affect the versions of pulp as shipped with Red Hat
  
Satellite 6.x and Red Hat Update Infrastructure 2.x as they did not include
  
support for pulp-gen-ca-certificate.&#8221;

## Notes

This was discovered after the 2.8.1 release candidate was published, so
  
the fastest way for us to get this out the door was a rapid-fire hotfix
  
release. This is a unique situation; two releases of the same pulp minor
  
version in two days is exceptional and should not become the norm.</pre>