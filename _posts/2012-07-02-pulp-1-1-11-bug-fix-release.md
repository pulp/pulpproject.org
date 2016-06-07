---
id: 691
title: Pulp 1.1.11 Bug Fix Release
date: 2012-07-02T14:19:27+00:00
author: Jay Dobies
layout: post
guid: http://blog.pulpproject.org/?p=691
permalink: /2012/07/02/pulp-1-1-11-bug-fix-release/
categories:
  - Releases
---
A new release of Pulp has been built to the v1 stable repositories to fix the following critical bug involving CDS sync (<https://bugzilla.redhat.com/show_bug.cgi?id=836640>). There are no extra steps involved in upgrading to this version, simply update the RPMs and restart the `pulp-cds` daemon.