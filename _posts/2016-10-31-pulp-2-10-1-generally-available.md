---
title: Pulp 2.10.1 ROLLED BACK
# blog post was originally at this permalink; it's defined explicitly here so that
# anything referencing the original GA announcement sees the rollback notice.
permalink: /2016/10/31/pulp-2-10-1-generally-available/
author: Sean Myers
tags:
  - release
  - rpm
  - ostree
categories:
  - Releases
---
## <span style="color: red;">ROLLED BACK</span>

> Pulp 2.10.1 was found to have multiple fatal flaws, most
> notably [this issue](https://pulp.plan.io/issues/2378). As a result, the
> release was rolled back. At this time, pending future release announcements,
> the currently available release of Pulp is 2.10.0, with Pulp 2.10.2 coming
> as soon as possible. Our apologies for the inconvenience.

Pulp 2.10.1 (<span style="color: darkorange" title="Happy Halloween!">Spooky Edition</span>)
is now Generally Available in the stable repositories:

* [pulp-2-stable](https://repos.fedorapeople.org/pulp/pulp/stable/2/)
* [pulp-stable](https://repos.fedorapeople.org/pulp/pulp/stable/latest/)

This release includes bug fixes to Pulp Platform, the RPM Plugin,
and the OSTree plugin.

Pulp 2.10.1 includes a variety of bug fixes to the Pulp Platform, as well
as the OSTree and RPM plugins, outlined below.


## Upgrading

The Pulp stable repositories are included in the pulp repo files:

* <https://repos.fedorapeople.org/repos/pulp/pulp/fedora-pulp.repo> for fedora 23 & 24
* <https://repos.fedorapeople.org/repos/pulp/pulp/rhel-pulp.repo> for RHEL 6 & 7

After enabling the `pulp-stable` or `pulp-2-stable` repository, you'll want to follow the standard
upgrade path with migrations:

```sh
$ sudo systemctl stop httpd pulp_workers pulp_resource_manager pulp_celerybeat
$ sudo yum upgrade
$ sudo -u apache pulp-manage-db
$ sudo systemctl start httpd pulp_workers pulp_resource_manager pulp_celerybeat
```


## Issues Addressed

These issues are fixed in Pulp 2.10.1:

### OSTree Support

* 2237 Published repositories are copied instead of linked
* 2213 Proxy URL for remotes not properly constructed.

### Pulp

* 2344 Streamer PulpHTTPAdapter does not configure the proxy pool manger with certificates resulting in 503s.
* 2328 Repository syncs show all units updated even when there are no changes
* 2287 Cannot get docker v2 repo tags list
* 2278 Remove checksum_type from the srpm and drpm collections
* 2277 Content published using move (instead of copy) causes 404 due to selinux denial.
* 2221 rsync distributor doesn't remove files from remote when rsyncing empty repository with --delete
* 2049 Django RemovedInDjango110Warning in logs for missing TEMPLATES setting
* 1766 Pulp API is incompatible with Django 1.10
* 1392 Misleading nodes quickstart howto

### RPM Support

* 2326 Publishes fail
* 2257 unit test failure: "AppRegistryNotReady: Apps aren't loaded yet."
* 2242 Package signature ID checking is broken when syncing in packages
* 2227 Only first pkglist is synced for erratum even if multiple are present
* 2190 Unit is associated with the repo before it is copied to the final location

View this list [in Redmine](http://bit.ly/2eqCCZe)
