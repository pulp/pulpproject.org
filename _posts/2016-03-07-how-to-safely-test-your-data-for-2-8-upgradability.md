---
id: 1237
title: How to Safely Test Your Data for 2.8 Upgradability
date: 2016-03-07T04:09:24+00:00
author: Michael Hrivnak
layout: post
guid: http://www.pulpproject.org/?p=1237
permalink: /2016/03/07/how-to-safely-test-your-data-for-2-8-upgradability/
categories:
  - Uncategorized
---
Help us help you! The Pulp team added stronger data validation in 2.8. To ensure that everyone has a smooth upgrade experience, we want your help testing your data in advance. In case there is any data &#8220;in the wild&#8221; that does not meet the stronger validation requirements, we want to know now so we can adjust the validation before release.

All we need you to do is use `mongodump` to export a copy of your data, and then run the [small script that we are providing](https://github.com/pulp/pulp/tree/master/playpen/mongoengine).

See the [README](https://raw.githubusercontent.com/pulp/pulp/master/playpen/mongoengine/README) for full details. The script will start two docker images; one running mongodb, and the other to run the upgrade test itself. The test will automatically load your data into the mongodb container, run the 2.8 migrations, and then attempt to read and save each record. At the end, both containers are cleaned up, and you&#8217;re left with a log of what happened.

Please let us know that you ran the tests, and especially if you encountered any problems. Leave a comment here, [send an email](https://www.redhat.com/mailman/listinfo/pulp-list), or [file a bug report](https://pulp.plan.io/projects/pulp/issues/new).

We are hoping to release 2.8.0 very soon, so your prompt feedback is appreciated. Thank you in advance for your help!