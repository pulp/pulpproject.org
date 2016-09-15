---
title: Pulp 2.10.0 Generally Available
author: Sean Myers
tags:
  - release
  - rpm
  - docker
categories:
  - Releases
---
Pulp 2.10.0 is now generally available, and can be downloaded from the Pulp 2.10 stable repositories:

<https://repos.fedorapeople.org/repos/pulp/pulp/stable/2.10/>

New features for Pulp 2.10 include the new rsync distributor, new high-availability
features for resource managers, as well as some improvements to RPM publishing and
package validation. Several bugs were also fixed; all the details can be found later
in this announcement.

More information about these new features can be found in the release notes:

- <http://docs.pulpproject.org/en/2.10/testing/user-guide/release-notes/2.10.x.html>
- <http://docs.pulpproject.org/en/2.10/testing/plugins/pulp_rpm/user-guide/release-notes/2.10.x.html>
- <http://docs.pulpproject.org/en/2.10/testing/plugins/pulp_docker/user-guide/release-notes/2.1.x.html>

Try out the new features, and let us know how they work for you!

These two issues were listed as blockers for release candidate 1, and fixed in release
candidate 2. Upgrade testing to 2.10.0 from earlier versions of pulp is complete and has
verified that these issues do not affect 2.10.0:

- [2234](https://pulp.plan.io/issues/2234)
  Upgrading to pulp 2.9 causes sync of repos with SRPM's with a filelist field to fail.

- [2236](https://pulp.plan.io/issues/2236)
  Upgrading hto pulp 2.9 causes sync of repos with DRPM' with a "relativepath" field to fail.

No other blocking issues have been identified, which clears 2.10.0 to become generally
available a little earlier than I indicated in my previous announcement for this release.


## Upgrading

The 2.10 stable repositories are included in the pulp repo files:
https://repos.fedorapeople.org/repos/pulp/pulp/fedora-pulp.repo for fedora 23 & 24
https://repos.fedorapeople.org/repos/pulp/pulp/rhel-pulp.repo for RHEL 6 & 7

After enabling the pulp-stable or pulp-2-stable repository, you'll want to follow
the standard upgrade path, with migrations, to get to 2.10.0:

```sh
$ sudo systemctl stop httpd pulp_workers pulp_resource_manager pulp_celerybeat
$ sudo yum upgrade
$ sudo -u apache pulp-manage-db
$ sudo systemctl start httpd pulp_workers pulp_resource_manager pulp_celerybeat
```


## New Features

Here are the specific stories done for 2.10.0:

### Docker Support
- 1887	As a user, I can use the rsync predistributor with docker web distributor
- 1291	As a user, I can sync feeds that require username/password authentication

### Pulp
- 1990	Rsync distributor
- 1963	As a user, I can use rsync distributor to publish iso repositories
- 1759	As a user, I can use rsync distributor to publish RPM repositores
- 898	As a user I can run multiple pulp_resource_managers concurrently with all of them actively participating

### RPM Support
- 1991	As a user, uploaded units which don't pass the signature check are not imported
- 1982	As a user, I can force a full sync
- 1878	Support for choosing the checksum type in updateinfo
- 1156	As a user, I can have an "signature" attribute stored for RPMs, SRPMs, and DRPMs

View this list in [redmine](http://bit.ly/2axDGKA).


## Issues Addressed

Here are the bug fixes specific to 2.10.0:

### Pulp
- 2082	Cannot add importer to the repository
- 1948	Upgrading RPMs on EL6 sometimes fails during pre script
- 2187	rsync as predistributor logic is wrong
- 2202	predistributor logic in platform does not account for force_full and updated distributor configs
- 2030	importer config is not validated during the update
- 2039	Not full importer config is validated in the update call
- 2118	Reduce runtime of file path migration
- 1606	API call to install consumers raises 500 if body contains wrong data type
- 2106	Pin to flake8 2.6.2

### RPM Support
- 2188	Make GPG signature checking is called "filtering"
- 2042	updateinfo.xml is missing checksums
- 2199	The RPM rsync distributor breaks when SELinux is enabled
- 2115	The 'skip_fast_forward' in RPM rsync config should be renamed to 'force_full'

All bug fixes from Pulp 2.9.2 and earlier are included in Pulp 2.10, as well as the fixes
for the two blockers mentioned above.

View this list in [redmine](http://bit.ly/2awB1h4).


## Fedora Support

Fedora 22 has reached its end-of-life. As a result Pulp 2.10 is being built for (and tested on)
Fedoras 23 and 24.
