---
title: Pulp Community Update (October 2020)
author: Melanie Corr
tags:
  - newsletter
---

_If you have any questions or updates you would like to share, we would be delighted to hear from you at `pulp-list@redhat.com`._


## Pulp 3 CLI PoC

A reminder that there's an open call for feedback on the Pulp 3 CLI proof of concept. As a thank you, we will gladly send some Pulp SWAG to anyone who [downloads and evaluates](https://pulpproject.org/2020/09/28/pulp-3-cli-poc-call-for-feedback/) the Pulp 3 CLI. Let us know your thoughts!

## Pulp 3.8 is Generally Available!

In Pulp 3.8.0, there were a number of updates to pulpcore, pulp_installer, and the plugin_template. You can read a summary of the main changes in the [release announcement](https://pulpproject.org/2020/10/20/3.8.0-release-announcement/).

Pulp 3.8.1 was released to add a fix for a bug that could [silently delete an Artifact from the filesystem](https://docs.pulpproject.org/pulpcore/en/3.8.1/changes.html#id1).

If any repositories were affected by this, please use the [repo version repair](https://docs.pulpproject.org/pulpcore/en/3.8.1/workflows/repairing-repository-versions.html?highlight=repo%20version%20repair) feature.

## Pulp 3.7.3 is Generally Available

This release added a critical update to Pulp 3.7 that could [silently delete an Artifact from the filesystem](https://docs.pulpproject.org/pulpcore/en/3.7/changes.html#id1).

## Pulp 2-to-3 Migration plugin 0.5 is Generally Available

In the past month, 0.5.0 and 0.5.1 releases of the Pulp 2-to-3 migration plugin have been made available. These releases included a number of significant performance improvements. You can read details in the [changelog](https://pulp-2to3-migration.readthedocs.io/en/0.5/changes.html).

The 0.5.1 release includes a fix for an [issue](https://pulp-2to3-migration.readthedocs.io/en/latest/changes.html#id1) related to migration of RPM content. Prior to this release, the RPM content metadata was migrated incorrectly. Users that have migrated RPM content using pulp-2to3-migration prior to 0.5.1 will need to remove the migrated RPM content from their Pulp 3 system and run the migration again using pulp-2to3-migration 0.5.1.


## Pulp Ansible 0.5.0 is Generally Available

After an earlier update to [0.4.2](https://www.redhat.com/archives/pulp-list/2020-October/msg00019.html) this month, Pulp Ansible 0.5.0 was released. This release contains a number of bug fixes, as well as adding a new /pulp/api/v3/ansible/copy/ endpoint allowing content to be copied from one AnsibleRepository version to a destination `AnsibleRepository`. For more information, see the full [changelog](https://docs.pulpproject.org/pulp_ansible/en/master/nightly/changes.html#id1).

## Pulp 2 updates

### Pulp 2.21.4 is Generally Available

This release includes bug fixes for Debian Support, Docker Support, Pulp, and RPM Support.
For more information, see the [release announcement](https://pulpproject.org/2020/10/31/pulp-2.4.1-is-generally-available/).

A reminder that Pulp 2 is in [maintenance mode](https://pulpproject.org/2020/06/15/pulp-2-enters-maintenance-mode/). After almost one year of general availability, Pulp 3 is more robust and stable than ever. If you haven't started planning your [migration](https://pulpproject.org/migrate-to-pulp-3/), there's never been a better time.

### pulp-docker 3.2.7 and python-nectar 1.6.2   

Because of an issue with syncing large docker repos from [registry.redhat.io](registry.redhat.io), there was an out-of-band release of pulp-docker 3.2.7 and python-nectar 1.6.2. You can download the updated repositories from [repos.fedorapeople.org/repos/pulp/pulp/stable/2.21/](https://repos.fedorapeople.org/repos/pulp/pulp/stable/2.21/).

## Pulplift has been merged into pulp_installer

During the installer discussions at PulpCon 2020, one of the conclusions was to consolidate the pulplift repository as part of the pulp_installer repository. As a result, pulplift has been archived. This means, you can still use your already provisioned boxes in pulplift, but they will fall out of maintenance shortly, and we encourage you to redeploy all you sandbox and source installations within pulp_installer soon.

For more information, see [Matthias's announcement](https://www.redhat.com/archives/pulp-list/2020-October/msg00040.html).

## opensource.com's Pulp Debian article

As a follow-up to last month's Pulp Debian plugin release, Maximilian Kolb has an article on [opensource.com](https://opensource.com/) entitled [Manage content using Pulp Debian](https://opensource.com/article/20/10/pulp-debian).

## Pulp open floor meetings & communication channels

Every Tuesday and Friday 10:30 ET (either EST or EDT), there is an open meeting in #pulp-meeting on Freenode (IRC).
It's a time for developer discussion, feedback, and decisions on code/issues/PRs/process relating to pulpcore or plugins.
If you have any questions, comments, or feedback relating to Pulp, feel free to put a topic on [the agenda](https://hackmd.io/@pulp/triage/edit) and come along and discuss it with us there.

If you're a Matrix user, you can find a list of Pulp channels [here](https://riot.im/app/#/group/+pulp:matrix.org).
