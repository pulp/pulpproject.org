---
title: Django14 Retirement From EPEL 6
layout: post
author: Brian Bouterse
tags:
  - platform
  - rpm
---
Running Pulp on EL6 depends on the `Django14` package provided by the Extra Packages for Enterprise
Linux (EPEL) 6. It was recently decided that this package will be retired no earlier than Jan 1 2017
and no later than March 31, 2017.

This was decided at a EPEL steering commitee meeting on Nov 9. It did not make
[the offical minutes](https://meetbot.fedoraproject.org/fedora-meeting/2016-11-09/epel.2016-11-09-18.29.html)
but it is in [the full log](https://meetbot.fedoraproject.org/fedora-meeting/2016-11-09/epel.2016-11-09-18.29.log.html).

Please give feedback via pulp-list in
[this thread](https://www.redhat.com/archives/pulp-list/2016-November/msg00022.html).


### Impact on Pulp EL6 Users

Once `Django14` is removed from EPEL6, Pulp EL6 packages will no longer install without the
`Django14` package being manually installed first. This will mostly affect new installations since
existing EL6 installations already have the `Django14` from before it was retired. Users can receive
the reitred `Django14` bits directly from
[the Koji build](http://koji.fedoraproject.org/koji/buildinfo?buildID=744751).

At some point, upstream Pulp will stop being built for EL6. This will be discussed on pulp-list to
identify a coordinated timeline with the Pulp user community. When this occurs, running Pulp on EL6
will be unsupported.

Pulp will continue to support managing packages for EL6, but the Pulp Server cannot itself be run on
EL6.


### Why is it being retired?

It's being retired due to support and security concerns: 

* Upstream Django is no longer maintaining Django 1.4.z. When an upstream community stop supporting
  an EPEL package, the responsibility falls to the EPEL maintainer to decide if they want to support
  it. The `Django14` package maintainer recommends retiring the package due to security concerns.

* There are several CVEs against Django 1.4 that are unfixed. Additionally, since it's no longer
  supported there are likely additional CVEs that are applicable but not being tracked.


### Preparation

Proactively upgrading your Pulp server OS to EL7 is the recommendation.
