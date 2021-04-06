---
title: Pulp Community Update (March 2021)
author: Melanie Corr
tags:
  - newsletter
---

__If you have any questions or updates you would like to share, we would be delighted to hear from you at `pulp-list@redhat.com`.__


## DevConf.cz videos are now available!

While DevConf.cz took place at the end of February, it took a while for the videos from the sessions to be added to YouTube.

If you didn't get a chance, you can watch Pavel and Daniel host a workshop on hosting your own on-premise PyPI.

<iframe width="560" height="315" src="https://www.youtube.com/embed/yxPHEHNJwO4" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### Host your own on-premise Ansible Galaxy

Brian also hosted a workshop where he demoed how you can host your very own on-premise Ansible Galaxy. Throughout the workshop, Brian also demoed the power of the new Pulp 3 CLI.

<iframe width="560" height="315" src="https://www.youtube.com/embed/GjrWYMfjGrs" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Pulpcore 3.11 has been released!

The headline features from this release include New Artifacts Checksum Reports, Proxy Authorization Updates, and many more updates and enhancements to existing functionality.

For more information about the new features and removals as part of this release, see the [release announcement blog](https://pulpproject.org/2021/03/15/pulpcore-3.11-is-now-generally-available/).

## Pulp 3 CLI now has Pulp Python commands!

Pulp 3 CLI 0.7.0 was released, and includes CLI commands for Pulp Python. The Pulp Python documentation has been updated to include CLI commands in their [workflows](https://pulp-python.readthedocs.io/en/latest/workflows/index.html)

As part of the Pulp 3 CLI release, the Container namespace command has also been added.
For more information, check out the [0.7.0 changelog](https://github.com/pulp/pulp-cli/blob/develop/CHANGES.rst#070-2021-03-15).

#### Want some SWAG?

As always, if you have tried the Pulp 3 CLI and would like to tell us your thoughts, [complete the survey](https://forms.gle/zUwcVf9i9erxm9Zr5) and we'll send you some SWAG as a thank you!

## Pulp Container 2.4.0

As well as the 2.4.0 release, this month there were some minor releases of Pulp Container versions [2.2.1](https://listman.redhat.com/archives/pulp-list/2021-March/msg00039.html) and [2.1.1](https://listman.redhat.com/archives/pulp-list/2021-March/msg00012.html).

Some 2.4.0 release highlights:
* The registry API has been updated to include pagination for the `_catalog` and the `tags/list` endpoint.
* Users have a new API endpoint they can use to remove an image using a digest from a push repository.
* Functionality has been added so that admins can allow users without permissions to create the namespace that matches their login.

For for a full list of changes, see the [changelog](https://docs.pulpproject.org/pulp_container/en/2.4.0/changes.html)


## Pulp 2-to-3 Migration plugin updates

In the last month, versions 0.9.0 and 0.10.0 of the Pulp 2-to-3 migration plugin have been released.

As well as performance improvements and bug fixes mainly for migration reruns, the example documentation have been updated to include Pulp 3 CLI examples. You can see the new Pulp CLI in action in the [workflow documentation](https://docs.pulpproject.org/pulp_2to3_migration/workflows.html).

For more information, check out the [changelog](https://docs.pulpproject.org/pulp_2to3_migration/changes.html).

And remember, Pulp 2 will sunset in late 2022, so [migrate to Pulp 3](//migrate-to-pulp-3/) today!

## Pulp RPM 3.10.0

This release fixes several bugs related to Advisories (Errata) as well as a bug relating to "sha" used as an alias for "sha1"

For a full list of changes, see the [changelog](https://pulp-rpm.readthedocs.io/en/latest/changes.html#id1).


## Pulp Debian updates

This month, Pulp Debian 2.10.0 and 2.11.0 were released.

Version 2.11.0 is compatible with Pulpcore 3.11.

The most important Debian update is that starting with Pulpcore 3.11, the MD5 checksum algorithm will be disabled by default.
Providing MD5 checksums in repository metadata is almost universal practice within the APT ecosystem.
For information about re-enabling MD5, or otherwise handling this change, see the new [workflow documentation](https://docs.pulpproject.org/pulp_deb/workflows/checksums.html).

## Pulp Ansible updates

This month, there were several minor updates to Pulp Ansible with the releases of [0.5.7](https://listman.redhat.com/archives/pulp-list/2021-March/msg00002.html), [0.5.8](https://listman.redhat.com/archives/pulp-list/2021-March/msg00010.html), [0.6.2](https://listman.redhat.com/archives/pulp-list/2021-March/msg00003.html), and [0.7.1](https://listman.redhat.com/archives/pulp-list/2021-March/msg00004.html).

Each of these releases contained several bugfixes. For more information, check out the release announcements and the [changelog](https://docs.pulpproject.org/pulp_ansible/en/0.7.1/changes.html#id1).

## PulpCon 2021

PulpCon 2021 will be held online. As sad as we are not to meet for another year, hosting PulpCon online has proven to open the floor to more community members than ever before. The tentative date for PulpCon 2021 is November 8-12. Please keep an eye on our mailing list `pulp-list@redhat.com` for further updates.

## The evolution of the Pulp installer

As a result of questions asked in the community this month, we put together a post outlining the rationale behind investing time and energy in an Ansible installer to automate the installation of Pulp 3. If you want to learn more about our installer, read [Pulp Installer - Information & Overview](https://pulpproject.org/2021/03/22/pup-installer-overview/).

## Get Involved

### Meetings

**Open Floor** - every Tuesday and Friday 10:30 ET (either EST or EDT) in #pulp-meeting on Freenode (IRC). It’s a time for developer discussion, feedback, and decisions on code/issues/PRs/process relating to pulpcore or plugins. To participate, put a topic on the agenda. Minutes will be sent to the pulp-dev mailing list.

**Bug Triage** - every Tuesday and Friday immediately after Open Floor in #pulp-meeting on Freenode (IRC). Come and participate in real-time. See the triage process for more details. You can also read through the triage archives.

### Make Sure You’re Registered On IRC

If you want to chat with us on any of the channels on Freenode IRC, please be aware that you must register your username with Freenode. If you’re not getting any answers from us, it could be because we are not seeing your messages.
