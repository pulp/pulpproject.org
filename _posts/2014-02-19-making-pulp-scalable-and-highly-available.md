---
id: 1070
title: Making Pulp Scalable and Highly Available
date: 2014-02-19T20:42:13+00:00
author: Michael Hrivnak
layout: post
guid: http://www.pulpproject.org/?p=1070
permalink: /2014/02/19/making-pulp-scalable-and-highly-available/
categories:
  - Uncategorized
---
Something very exciting is going to happen today to Pulp. We have been working for months to make Pulp scalable (more details below), and recently it has been an all-hands effort to prepare a beta build of 2.4. We have completely replaced the asynchronous tasking system, re-written the scheduled task system, and re-written large portions of our web request handlers and their underlying logic. The sum of that effort is going to be merged into our master branch on <a href="https://github.com/pulp/pulp" target="_blank">GitHub</a> today, because we have finally reached a point where the new code is stable and feature-complete.

In previous releases of Pulp, the REST API has been limited to running in one apache process. No more! Pulp 2.4 will let you handle web requests with multiple processes scaled out to as many machines as you like.

Long-running tasks, like publishing a repository, have previously been performed by the same process that was handling web requests. In 2.4, those jobs have been moved to dedicated &#8220;worker&#8221; processes. You&#8217;ll be able to scale apache processes and worker processes separately based on what type of load your deployment receives.

By scaling out apache processes and worker processes to multiple machines, you&#8217;ll be able to make a single Pulp deployment (mostly) highly-available with the same techniques used for any data-driven web application. (There is one area remaining that includes a single point of failure; more about that later.)

This is all made possible by using the best-in-class <a href="http://www.celeryproject.org/" target="_blank">Celery</a> project, which is a mature management framework for asynchronous and distributed job queues. We&#8217;ve been working closely with the Celery community to add support for Apache Qpid (our preferred messaging broker) using AMQP 1.0, which is another exciting development that we&#8217;ll post more about separately.

There are many additional improvements going into 2.4 such as an all-new yum distributor, support for puppet 3.3+, and more consistent error reporting across the REST API (this will be an ongoing effort). Likewise, some very interesting engineering took place to guarantee the same resource locking we had in previous releases. The reason Pulp has been limited to one process is that it is difficult to guarantee that only one operation (such as a sync or publish) happens at a time on any particular resource. Doing this in a distributed system is even more complex! We came up with an interesting solution that we&#8217;ll post more about separately.

Stay tuned, and we will follow up with more details on the 2.4 release schedule as well as additional posts going into more detail about these changes. Keep an eye out for 2.4 beta builds, and please share your feedback.