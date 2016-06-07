---
id: 722
title: Pulp v2 Community Release 1
date: 2012-07-26T13:27:28+00:00
author: Jay Dobies
layout: post
guid: http://blog.pulpproject.org/?p=722
permalink: /2012/07/26/pulp-v2-community-release-1/
categories:
  - 2.0
  - Releases
---
<img src="http://website-pulp.rhcloud.com/wp-content/uploads/2012/07/cr-1.png" alt="" title="Pulp v2 Community Release 1" width="410" height="161" class="alignnone size-full wp-image-726" />

The first community release of the v2 codebase is now available. It can be accessed by enabling the pulp-v2-stable repository in the *-pulp.repo files (due to the versioning scheme, all other repositories should be disabled to ensure the v2 builds are installed).

# What It Is

Pulp has been completely rearchitected from its v1 releases. Instead of a tight coupling to RPM-related content, the v2 architecture has been redesigned around an open plugin model. The result is the ability to use the inventorying, searching, and repository and consumer management features with custom defined types of content. Plugins are provided for RPM concepts (RPM, errata, kickstart trees, etc.) on top of the core Pulp Platform to provide functional parity with the v1 releases.

This release is the work thus far on the v2 architecture. It is intended as a preview for what the eventual 2.0 release will be in order to solicit input from the community.

# What It Is Not

First and foremost, this community release is not meant for production. It is the first release on a nearly entirely new codebase and, as such, has not been subject to the levels of QA the 1.x releases have been. Furthermore, it&#8217;s not yet 100% feature complete with respect to what was offered in v1. Work continues on reaching that goal, but in the meantime don&#8217;t be surprised to find certain features missing from this release (most notably, CDS support is missing entirely).

# Installation &#038; Upgrade

## Installation

Installation has changed slightly in the 2.x versions. The short explanation is that the packages to install have been renamed. Installation instructions can be found in our <a href="http://pulpproject.org/v2/rpm-user-guide/installation.html" target="new">Pulp+RPM User Guide</a>.

The 2.0 and 1.x releases cannot be installed at the same time.

In this release, the SELinux policy RPM is not installed automatically with the server. SELinux support can be added once the server is installed by installing the `pulp-selinux` RPM.

## Upgrades

Upgrades from 1.x are not currently supported. This will be added in a future release.

# Features

Over the past few weeks, I&#8217;ve written about a number of new features in 2.0. These articles include:

  * The 1.x notion of cloning has been removed in favor of the more flexible and powerful [unit copy](http://blog.pulpproject.org/2012/06/15/pulp-v2-preview-unit-copy/) functionality.
  * The client has been rearchitected to support easily installed [extensions](http://blog.pulpproject.org/2012/06/24/pulp-v2-preview-client-extensions/) to customize and specialize it&#8217;s behavior.
  * The server now employs a [significantly stronger mechanism](http://blog.pulpproject.org/2012/07/02/pulp-v2-preview-the-coordinator/) for ensuring integrity across concurrent requests and queuing operations requested on busy resources.
  * A number of other [general enhancements](http://blog.pulpproject.org/2012/07/17/pulp-v2-preview-general-enhancements/) have been made based on community feedback.

The [v2 user guide](http://pulpproject.org/v2/rpm-user-guide/), which continues to be flushed out, contains more information on using this release.

# Support

Any and all comments, suggestions, and bug reports are appreciated. In the coming months we&#8217;ll be working to polish the new codebase and every bit helps. As usual we can be reached on our [mailing list](https://www.redhat.com/mailman/listinfo/pulp-list) and in #pulp on Freenode.