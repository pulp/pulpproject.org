---
id: 1079
title: Deploying Pulp in Containers
date: 2014-08-31T15:01:20+00:00
author: Michael Hrivnak
layout: post
guid: http://www.pulpproject.org/?p=1079
permalink: /2014/08/31/deploying-pulp-in-containers/
categories:
  - Uncategorized
---
Mark Lamourine has started an excellent series of blog posts on deploying Pulp in containers using <a href="http://docker.io" target="_blank">docker</a> and <a href="https://github.com/GoogleCloudPlatform/kubernetes" target="_blank">kubernetes</a>. He starts with an <a href="http://cloud-mechanic.blogspot.com/2014/08/intro-to-containerized-applications.html" target="_blank">intro</a> and goes on to describe in detail how to <a href="http://cloud-mechanic.blogspot.com/2014/08/docker-simple-service-container-example.html" target="_blank">deploy mongodb with docker</a>.

Keep your eye out for the rest of this series.

I am especially interested in seeing qpid deployed in a container. Since the qpid-cpp server does not have support for vhosts, running multiple qpid instances is the only way to achieve full isolation between separate components or applications that require a message broker. Docker has the potential to make that very easy to manage. Even within one Pulp deployment, there would be a lot of value in isolating the celery workers&#8217; messaging traffic from consumer messaging traffic by using two qpid instances. Stay tuned for more on that.