---
id: 639
title: Pulp v1.1 on Fedora 17
date: 2012-06-13T14:42:51+00:00
author: Jay Dobies
layout: post
guid: http://blog.pulpproject.org/?p=639
permalink: /2012/06/13/pulp-v1-1-on-fedora-17/
categories:
  - Releases
---
The Pulp v1.1 release is now available for Fedora 17. The existing repo file (<http://repos.fedorapeople.org/repos/pulp/pulp/fedora-pulp.repo>) will correctly substitute in the running version of Fedora.

The only difference in this version is that the python-simplejson package must be updated for Pulp to work. The update already exists in the Fedora repositories (python-simplejson-2.5.2-1.fc17), just make sure to update it before running Pulp.

## Quick Links

  * [Installation Instructions](http://pulpproject.org/ug/UGInstallation.html#installation)
  * [User Guide](http://pulpproject.org/ug/index.html)