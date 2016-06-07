---
id: 832
title: Pulp v2 Community Release 2
date: 2012-10-09T19:31:59+00:00
author: Jay Dobies
layout: post
guid: http://website-pulp.rhcloud.com/?p=832
permalink: /2012/10/09/pulp-v2-community-release-2/
categories:
  - 2.0
  - Releases
---
<img src="http://website-pulp.rhcloud.com/wp-content/uploads/2012/10/cr-2.png" alt="" title="Pulp Community Release 2" width="410" height="161" class="alignnone size-full wp-image-838" />

The second community release of the v2 codebase is now available. It can be accessed by enabling the pulp-v2-stable repository in the *-pulp.repo files (due to the versioning scheme, all other repositories should be disabled to ensure the v2 builds are installed).

# Overview

By now I&#8217;ve talked pretty extensively about how the Pulp&#8217;s v2 codebase is a radical shift from our v1 offering. This release is the second milestone in our reworking of Pulp from its humble beginnings as an RPM repository manager into a much more powerful and extensible platform (while still maintaining its RPM support).

There are a number of new features and bug fixes, but as with [the last community release](/2012/07/26/pulp-v2-community-release-1), this is intended more as a preview release rather than production level responsibilities.

# Installation &#038; Upgrade

## Installation

Installation instructions can be found in our <a href="http://pulp-rpm-user-guide.readthedocs.org/en/latest/installation.html" target="new">Pulp User Guide</a>.

The 2.0 and 1.x releases cannot be installed at the same time.

In this release, the SELinux policy RPM is not installed automatically with the server. SELinux support can be added once the server is installed by installing the `pulp-selinux` RPM.

## Upgrades

Upgrades from 1.x are not currently supported. This will be added in a future release, most likely Community Release 4.

# Features

_I&#8217;ll be completely honest, we cut this branch a few weeks ago. QE took some time to test and there was a period of about a week and a half where it sat ready to be released but was held up for dumb reasons. As such, I&#8217;m a bit fuzzy on what made it into CR-2 v. what will be in CR-3. There&#8217;s a ton of work that&#8217;s been done, all of it awesome, it&#8217;s just a matter of this release coming in the middle of a change to how we want to handle releases which made these release notes tricky to write. We&#8217;re cleaning up our release notes tracking process and this will get better, I promise. &#8211; jdob_

  * **Event Notification System** &#8211; The Pulp server can be configured to send events based on activities within the system. This release includes the basis for that framework as well as the ability to configure events to be sent to a REST API. The next release will include notifications sent by e-mail or posted to an AMQP message bus.
  * **Profiler Plugin Framework** &#8211; A profiler is a server-side plugin that provides type-specific support for operations related to consumers. This release includes the definition, framework, and hooks into the appropriate places in the Pulp server workflows to enhance the capabilities of Pulp&#8217;s remote installation functionality. Also included is the first usage of these plugins in the form of server-side translation of errata installation requests into the appropriate RPM upgrades based on the consumer&#8217;s latest reported package profile. The profiler also includes the ability to calculate applicable errata on a given set of consumers, however this ability is only accessible through the REST APIs and has yet to be exposed in the CLI.
  * **Increased Installation Options** &#8211; In addition to the previously supported installation of RPMs, the client now supports installation of errata and package groups on a consumer.
  * **Repository Group Support** &#8211; Repository groups have been added back to Pulp. In this iteration, Pulp&#8217;s criteria system is supported to greatly enhance the interface for manipulating group membership. Work continues in this area to flush out the operational features around groups.
  * **Standardized Criteria UI** &#8211; In v2, Pulp exposes a strong query syntax for all types of resources in what is referred to as a Pulp criteria. Criteria can include anything from a direct field matching to regular expressions. This release includes the beginning of standardizing the client&#8217;s format for defining a criteria when searching in Pulp.
  * **Client Support for Users** &#8211; Pulp&#8217;s user and roles system has been reworked. The v2 incarnation is included in this release.
  * **Performance and Stability Enhancements** &#8211; I realize this sounds like a generic catch-all, but given the newness of the code and QE&#8217;s ability to settle in and hammer on CR-1, there really was an impressive amount of bug fixes. A number of problem areas, for example the performance of retrieving a large number of repositories, were identified and addressed.

# Future Work

Work has already begun on Community Release 3. That release will include the Puppet support written about in the past few articles. Another notable change is the removal of createrepo from the metadata generation process, greatly increasing the performance of publishing yum repositories. This release should be available within the next two months to get these two major features, along with many others, in the community&#8217;s hands.

# Support

Any and all comments, suggestions, and bug reports are appreciated. In the coming months we&#8217;ll be working to polish the new codebase and every bit helps. As usual we can be reached on our [mailing list](https://www.redhat.com/mailman/listinfo/pulp-list) and in #pulp on Freenode.