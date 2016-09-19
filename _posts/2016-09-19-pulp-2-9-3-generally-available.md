---
title: Pulp 2.9.3 Generally Available
author: Sean Myers
tags:
  - release
  - rpm
  - docker
categories:
  - Releases
---
Pulp 2.9.3 is now Generally Available in the stable repositories:

<https://repos.fedorapeople.org/repos/pulp/pulp/stable/2.9/>

This release includes bug fixes to Pulp Platform, the RPM Plugin,
and the Docker plugin.

No changes have taken place between 2.9.3 Beta 2 and this release.


## Upgrading

The 2.9 stable repositories are not included in the pulp repo files, and must be
installed manually with the following repository baseurls.

Fedora 23: `https://repos.fedorapeople.org/repos/pulp/pulp/stable/2.9/fedora-$releasever/$basearch/`
RHEL/Centos 6 & 7: `https://repos.fedorapeople.org/repos/pulp/pulp/stable/2.9/$releasever/$basearch/`

After enabling the pulp 2.9 repository for your distribution, you'll want to
follow the standard upgrade path with migrations:

```sh
$ sudo systemctl stop httpd pulp_workers pulp_resource_manager pulp_celerybeat
$ sudo yum upgrade
$ sudo -u apache pulp-manage-db
$ sudo systemctl start httpd pulp_workers pulp_resource_manager pulp_celerybeat
```


## Issues Addressed

These issues are fixed in Pulp 2.9.3:

### Docker Support

- 2198: Timestamp not updated for changed docker_tag
- 2142: Units created with 0-byte files when sync runs out of disk space

### Pulp

- 2121: Requires: selinux-policy is broken in spec files
- 2112: Status API does not log Qpid errors
- 2047: directory creation race condition during publish

### RPM Support

- 2236: Upgrading to pulp 2.9 causes sync of repos with DRPM' with a "relativepath" field to fail.
- 2234: Upgrading to pulp 2.9 causes sync of repos with SRPM's with a filelist field to fail.
- 2197: Error report for file size verification is not generated correctly
- 2134: Updating a repo without specifying checksum_type causes KeyError
- 1926: package without epoch in the erratum pkglist is not handled correctly during publish
- 1881: pulp-admin fails to display the erratum if not all metadata is present
- 236: Don't re-download rpms if they exist on disk

View this list in [Redmine](http://bit.ly/2bPVEca).


## Fedora Support

Fedora 22 packages are incidentally included in this release, but are not
supported due to Fedora 22 reaching its end-of-life.
