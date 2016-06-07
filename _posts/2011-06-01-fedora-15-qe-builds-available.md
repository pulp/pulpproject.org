---
id: 313
title: Fedora 15 QE Builds Available
date: 2011-06-01T13:38:50+00:00
author: Jay Dobies
layout: post
guid: http://blog.pulpproject.org/?p=313
permalink: /2011/06/01/fedora-15-qe-builds-available/
categories:
  - Releases
---
We finished setting up our build process and have started producing Fedora 15 RPMs for our Wednesday/Friday QE builds. With this change, Fedora 13 builds are no longer provided in the QE releases.

As usual with the QE builds, they are not tested to the same level as community releases. The next community release (which is estimated to be the third week in June) will be built on Fedora 14 and Fedora 15 (and RHEL 5 and 6, no change there).

The QE builds can be found at <http://repos.fedorapeople.org/repos/pulp/pulp/testing>. The [fedora-pulp.repo](http://repos.fedorapeople.org/repos/pulp/pulp/fedora-pulp.repo) file that is used for community release installations already has a repo definition for the QE builds. Simply enable the repo and the QE build versions will be used instead of the community releases.

As usual, feel free to contact us through the normal channels (Freenode: #pulp or [mailing list](https://www.redhat.com/mailman/listinfo/pulp-list)) if you run into any issues.