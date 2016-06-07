---
id: 387
title: Pulp in Production
date: 2011-08-01T14:32:43+00:00
author: Jay Dobies
layout: post
guid: http://blog.pulpproject.org/?p=387
permalink: /2011/08/01/pulp-in-production/
categories:
  - Uncategorized
---
Last Friday, Red Hat Update Infrastructure (RHUI) 2.0 went GA. This release marks a huge step forward for Pulp as the RHUI codebase has been rebased on Pulp in this version. Naturally this has been a source of pride for the Pulp team, but it&#8217;s also been a huge benefit to Pulp as a project due to the increased focus on QA and bug fixing.

As a quick summary, the [Red Hat Update Infrastructure](http://docs.redhat.com/docs/en-US/Red_Hat_Update_Infrastructure/2.0/html/Installation_Guide/index.html) is a product (currently) available to certified cloud providers for the management and distribution of Red Hat and custom content within the cloud. RHUI offers load balancing and fail over capabilities that allow the cloud provider to meet increasing client demand. All of these features are built on top of Pulp as the backend and a custom CLI using Pulp&#8217;s REST APIs.

Now that this release is finished, the next big focus for the team will be moving to support non-RPM content (I&#8217;ll be blogging more about that in the next few days).