---
title: Pulp Community Update (April 2021)
author: Melanie Corr
tags:
  - newsletter
---

__If you have any questions or updates you would like to share, we would be delighted to hear from you at `pulp-list@redhat.com`.__


## Pulpcore 3.12 is Generally Available!

One of the headline features of this release is an enhancement to Pulpcore so that when the content of a repository is updated, the repository can be automatically published and redistributed. This feature paves the way for future content plugin releases to add support for automatically publishing or distributing the new repository version after it has been created. Soon after the release of 3.12, the Pulp File plugin was updated to enable this feature.

Autopublish and autodistribute support has since been added to the Pulp CLI 0.8.0 release for Pulp RPM and Pulp File content.

One minor release followed that adds a missing field `RepositoryVersionRelatedField` in the plugin API that plugin writers need when moving from RepositoryVersionDistribution to the new Distribution model.

For a full list of headline features, see the [release announcement](https://pulpproject.org/2021/04/08/pulp-3.12-is-generally-available/)

Pulp 3.12.1 is available on [PyPI](https://pypi.org/project/pulpcore/3.12.1)
You can find the latest version of the Pulp installer [on Ansible Galaxy](https://galaxy.ansible.com/pulp/pulp_installer).

This month, there was also a backport to Pulpcore 3.7.6 that fixes the handling of repositories with non-sha256 checksums. For more information, see the [release announcement](https://listman.redhat.com/archives/pulp-list/2021-April/msg00058.html).


## Pulp 2-to-3 Migration plugin

With the 0.11.0 release, some performance improvements have been added as well as a new  `CONTENT_PREMIGRATION_BATCH_SIZE` option that you can use to adjust the content batch size, so that you have better control while fine tuning a slow migration or if you've been dealing with timeout issues.

The default value is set to `1000`. If you're working with a slow system, consider readjusting the batch size to somewhere in the 50-100 range.

A reminder that you can use the Pulp 3 CLI with the migration plugin, so that its easier and faster than ever to progress through the workflows. The [docs](https://docs.pulpproject.org/pulp_2to3_migration/workflows.html) have also been updated with CLI examples.

You can download the latest release from [PyPI](https://pypi.org/project/pulp-2to3-migration/0.11.0/) and find the latest [Python bindings](https://pypi.org/project/pulp-2to3-migration-client/0.11.0) and [Ruby bindings](https://rubygems.org/gems/pulp_2to3_migration_client/versions/0.11.0)

## Pulp 3 CLI 0.8.0

The Pulp 3 CLI effort has been going from strength to strength, and the latest release is no exception.
The 0.8.0 release is packed with features.

Among the new features of this release are added support for autopublish and autodistribute in pulp_file and pulp_rpm, as well as a new interactive-shell mode to pulp-cli. For a full list, check out the [changelog](https://github.com/pulp/pulp-cli/blob/develop/CHANGES.rst)

The Pulp 3 CLI is still considered beta, and we would very much like to know what you think about the effort so far. If you have any feedback, please [let us know](https://forms.gle/fmh2wgfarjrqWDkf6) and we'd be glad to send some SWAG as a thank you!

## Pulp Python 3.2.0

The Pulp Python release contains three new sync filters. With the `package_types` and `exclude_platforms`, you can refine the package types you want to sync from a remote source. With the `keep_latest_packages` filter, you can specify how many latest versions of packages you want to retain. This is another method of optimizing disk space on your deployment.

For more information about the new sync workflows, check out the [A More Complex Remote](https://pulp-python.readthedocs.io/en/latest/workflows/sync.html#a-more-complex-remote) in the Pulp Python documentation. These workflows have also been incorporated into the Pulp 3 CLI support for Pulp Python.

You can find this release on [PyPI](https://pypi.org/project/pulp-python/3.2.0/)
as well as the [Python bindings](https://pypi.org/project/pulp-python-client/3.2.0/) and
[Ruby bindings](https://rubygems.org/gems/pulp_python_client/versions/3.2.0).

## Pulp Container 2.5.0

As well as some enhancements and bug fixes, RBAC is no longer in tech-preview mode!
Also with this release, users and groups who are authorized can now access repositories they have permissions for in the catalog endpoint. For more information, check out the [full changelog](https://docs.pulpproject.org/pulp_container/en/2.5.0/changes.html).

You can find the latest release on [PyPI](https://pypi.org/project/pulp-container/2.5.0), as well as
[Python bindings](https://pypi.org/project/pulp-container-client/2.5.0/). You can also find [Ruby bindings](https://rubygems.org/gems/pulp_container_client/versions/2.5.0/).


## Pulp Debian

Several versions of the Pulp Debian plugin were updated to fix a bug that was preventing synchronization of repositories containing translation files (as well as the synchronization of the translation files themselves).

The following versions have been updated: 2.9.1, 2.10.1, and 2.11.1.

For more information, see the [release announcement](https://listman.redhat.com/archives/pulp-list/2021-April/msg00032.html).

## PulpCon 2021 - Save the Date!

PulpCon 2021 will be held online. As sad as we are not to meet for another year, hosting PulpCon online has proven to open the floor to more community members than ever before. The tentative date for PulpCon 2021 is November 8-12. Please keep an eye on our mailing list `pulp-list@redhat.com` for further updates.


## Thank You!

Thank you to everyone who came along to our virtual booth at Red Hat Summit, asked questions and provided feedback on the Pulp mailing list and Freenode IRC over the last few weeks. We love hearing how you're using Pulp, and are looking forward to more questions and insights!

## Get Involved!

As well as the mailing lists, there are a number of ways to get involved in the Pulp community!

### Meetings

**Open Floor** - every Tuesday and Friday 10:30 ET (either EST or EDT) in #pulp-meeting on Freenode (IRC). It’s a time for developer discussion, feedback, and decisions on code/issues/PRs/process relating to pulpcore or plugins. To participate, put a topic on the agenda. Minutes will be sent to the pulp-dev mailing list.

**Bug Triage** - every Tuesday and Friday immediately after Open Floor in #pulp-meeting on Freenode (IRC). Come and participate in real-time. See the triage process for more details. You can also read through the triage archives.

### Make Sure You’re Registered On IRC

If you want to chat with us on any of the channels on Freenode IRC, please be aware that you must register your username with Freenode. If you’re not getting any answers from us, it could be because we are not seeing your messages.

### Matrix users

If you're accessing the #pulp channel on Freenode using the Element client for Matrix, [this article](https://amitosh.medium.com/staying-always-online-in-irc-using-riot-the-right-way-d4c4ff2f43d0) helps get setup so that everyone can see your messages.
