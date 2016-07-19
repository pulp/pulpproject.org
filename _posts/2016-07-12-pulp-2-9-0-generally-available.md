---
id: 1400
title: Pulp 2.9.0 Generally Available
date: 2016-07-12T20:38:48+00:00
author: Sean Myers
layout: post
guid: http://www.pulpproject.org/?p=1400
permalink: /2016/07/12/pulp-2-9-0-generally-available/
categories:
  - Releases
---
The Pulp 2.9.0 is now available, and can be downloaded from the 2.9 stable repositories:

<https://repos.fedorapeople.org/repos/pulp/pulp/stable/2.9/>

The Pulp &#8220;2&#8221; and &#8220;latest&#8221; repositories have also been updated to point to 2.9.0:

<https://repos.fedorapeople.org/repos/pulp/pulp/stable/2/>
  
<https://repos.fedorapeople.org/repos/pulp/pulp/stable/latest/>

This release includes many new features, as well as bug fixes. Owing to the more rapid time-based release of 2.9, this release should consist of fewer new features compared to the release of 2.8.

This release was slightly delayed by two blocking issues (2035 and 2037, seen below) which were fixed in Beta 2. The release timeline of 2.10 has not been impacted by these delays.

More information about our release schedule can be found on the Pulp wiki:
  
<https://pulp.plan.io/projects/pulp/wiki/Release_Schedule>

# Known Issues

## Possible Errata Sync Failure

Several issues were reported against Pulp 2.8 that were not included in the Pulp 2.9.0 release as a result of release timing. The [list of issues fixed in 2.8.6](http://bit.ly/29vs1bE) outlines these bugs, but there is one issue in particular that can potentially break RPM repository syncing after upgrading: <https://pulp.plan.io/issues/2048>

This issue is related to resyncing errata from some repositories, and in a pulp-admin sync operation looks like this:

<pre>Task Failed
Could not parse errata `updated` field: expected format '%Y-%m-%d %H:%M:%S'.
 Fail to update the existing erratum SOME_ERRATUM_ID.</pre>

As a workaround, you can choose to skip errata in the feed repository. To do this, you can update the repo to skip errata:

<pre>pulp-admin rpm repo update --repo-id  --skip=erratum</pre>

This will be fixed in Pulp 2.9.1. If you require errata to be synced from a feed repository, consider delaying an upgrade to Pulp 2.9 until 2.9.1 is released.

## RPM Publish Directory

[A story](https://pulp.plan.io/issues/1976) related to specifying different package publish directories inside an RPM repo, e.g. packages go in a configured packages dir, was included in previous betas and the release candidate for 2.9.0. Testing revealed that the feature didn&#8217;t perform as expected, so that feature has been pulled from this release. It is likely to return in a future release.

# Upgrading

The Pulp 2 stable repository (pulp-2-stable) is included in the pulp repo files:

  * <https://repos.fedorapeople.org/repos/pulp/pulp/fedora-pulp.repo> for fedora 22 & 23
  * <https://repos.fedorapeople.org/repos/pulp/pulp/rhel-pulp.repo> for RHEL 6 & 7

After enabling the pulp-2-stable repository, you&#8217;ll want to follow the standard
  
upgrade path, with migrations, to get to 2.9:

<pre style="padding-left: 30px;">$ sudo systemctl stop httpd pulp_workers pulp_resource_manager pulp_celerybeat
$ sudo yum upgrade
$ sudo -u apache pulp-manage-db
$ sudo systemctl start httpd pulp_workers pulp_resource_manager pulp_celerybeat</pre>

# Supported Fedora Versions

Fedora 22 is no longer a supported Fedora release. We will not be supporting Fedora 22 for Pulp 2.9 and onward. Supported Fedora 24 builds will be starting as soon as possible.

We&#8217;ll be updating [our docs](http://docs.pulpproject.org/user-guide/installation/index.html#supported-operating-systems) soon with more details about our Fedora support policy than what&#8217;s currently there[2]. In the meantime, Pulp users on Fedora 22 are encouraged to upgrade to Fedora 23 to continue to receive supported releases.

# New Features

New features for Pulp 2.9 include optimizations in publishing, repoview support for yum repos, langpacks support for yum repos, and more:

## Pulp

  * 1724 Publish should be a no-op if no units and no settings have changed since the last successful publish

## RPM Support

  * 1716 As a user, I can have better memory performance on Publish by using SAX instead of etree for comps and updateinfo XML production
  * 1543 As a user, I would like incremental export to be the same format as full export
  * 1367 comps langpacks support
  * 1158 As a user, I can force full/fresh publish of rpms and not do an incremental publish
  * 1003 As a user, I can use pulp-admin to upload of package_environment
  * 189 Repoview-like functionality for browsing repositories via the web interface

View this list [in redmine](http://bit.ly/1XXtIps).

More information about these new features can be found in the release notes:
  
<http://docs.pulpproject.org/en/2.9/user-guide/release-notes/2.9.x.html>
  
<http://docs.pulpproject.org/en/2.9/plugins/pulp_rpm/user-guide/release-notes/2.9.x.html>

# Issues Addressed

All bug fixes from Pulp 2.8.5 and earlier are included in Pulp 2.9.

Here are the bug fixes specific to 2.9:

## Pulp

  * 2015 rpm repo publish fails with &#8220;Incorrect length of data produced&#8221; error
  * 1928 Publish should be operational if override config values were specified
  * 1844 pulp-admin &#8211;config and/or api_prefix option appears to be ignored

## RPM Support

  * 2037 Migration failing after 2.9 upgrade
  * 2035 Errata are published incrementally
  * 2001 Make sure sqlite files are generated if repoview is enabled
  * 1969 as user, I can export a repo with a specified checksum type
  * 1938 force\_full is not supported by export\_distributor
  * 1876 Add example on how to generate PULP_MANIFEST file
  * 1787 None in comps.xml
  * 1619 as user, I can export repo groups with different checksum than sha256
  * 1618 &#8211;checksum-type is broken

View this list [in redmine](http://bit.ly/1Q5bSOs).