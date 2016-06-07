---
id: 1094
title: Upcoming Pulp Releases
date: 2014-11-19T19:31:46+00:00
author: Brian Bouterse
layout: post
guid: http://www.pulpproject.org/?p=1094
permalink: /2014/11/19/upcoming-pulp-releases/
categories:
  - Uncategorized
---
2.5.0 <&#8211; A release candidate is available in Pulp&#8217;s beta repo in the 2.5 release stream.

2.6.0 <&#8211; Not yet built, but an alpha should be available soon.

**Why is the release after 2.5.0 going to be 2.6.0 and not 2.5.1?**
  
Since the version 2.4.0 release, Pulp is working to adhere to <a href="http://semver.org/" target="_blank">semantic versioning</a>. Semantic versioning is important so that users can upgrade to a given version and have a correct expectation about what is in that new version ie: bugfix, features, or backwards incompatible changes. There are new features that are ready to be included in a release, so the next release planned will be 2.6.0.

**Will 2.6.0 fix bugs that that exist in 2.5.0?**
  
Yes. There are some new features but also a lot of bugfixes. One of the main bugs resolved is that 2.6.0 should work with RabbitMQ.

**I integrate against Pulp (ie: HTTP API, plugin API), is it safe for me to upgrade from 2.5.0 -> 2.6.0?**
  
Yes, any Pulp release that starts with a 2 should be backwards compatible from an API perspective. Sometimes there are good reasons (ie: security) that cause a X.Y release to be incompatible, but 2.5.0 -> 2.6.0 should be completely backwards compatible. 2.4.Z -> 2.5.Z should also be completely reverse compatible.

**I develop or contribute to Pulp or Pulp plugins; how does this affect me?**
  
You should do one thing. Go delete 2.5-dev from your fork and local checkouts of pulp, pulp\_rpm, and pulp\_puppet. We made sure nothing was lost when we deleted 2.5-dev, but you still need to delete your versions of those branches.

Send questions or thoughts to pulp-list; you can also discuss on #pulp on freenode