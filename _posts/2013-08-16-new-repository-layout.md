---
id: 1040
title: New Repository Layout
date: 2013-08-16T19:34:15+00:00
author: Michael Hrivnak
layout: post
guid: http://www.pulpproject.org/?p=1040
permalink: /2013/08/16/new-repository-layout/
categories:
  - Uncategorized
---
In preparation for the upcoming release of Pulp 2.2, we have deployed a new layout for our yum repository. The new layout will make it much easier for us to make multiple releases of Pulp available at the same time, and it will give users the ability to choose what level of updates to receive. This also means that stable Pulp releases starting from 2.1 will continue to be available in our repository indefinitely.

We also will be making source RPMs available in our repository for all new releases.

The base of our stable repository is <a href="http://repos.fedorapeople.org/repos/pulp/pulp/stable/" target="_blank">here</a>.

We do our best to follow <a href="http://semver.org/" target="_blank">semantic versioning.</a> Thinking about versions in terms of X.Y.Z components, to stay on the current X.Y stable release and only receive Z updates (which should be bug fixes only), you would use the `2.1/` path.

To stay on the current X stable release and get major new releases such as 2.2 and 2.3 when they come out, you would use the `2/` path. This lets you automatically get major new features in the v2 line, but will stop short of moving you to v3 until you are ready.

To be on the latest stable release all the time, and even get updates to 3.0 and beyond, you would use the `latest/` path.

Beta releases will have a similar layout, but without the upgrade options. So for example we may have 2.3 and 2.4 beta builds available at the same time in `beta/2.3/` and `beta/2.4/`, but there will never be a `beta/2/` or `beta/latest/`. Beta releases are not guaranteed to persist for any period of time and will definitely get trimmed.

The old `v2/stable/` link will continue to work. It will mirror the behavior of the new `stable/2/` link. However, this is a great time to re-evaluate what upgrades you want to receive automatically, and choose a new repository link to match your expectations.

Our repo files have been updated, so please head over to our <a href="https://pulp-user-guide.readthedocs.org/en/pulp-2.1/installation.html#repositories" target="_blank">installation docs</a> to get the new file.