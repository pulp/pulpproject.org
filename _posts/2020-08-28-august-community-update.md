---
title: Pulp Community Update (August 2020)
author: Melanie Corr
tags:
  - news
---

_If you have any questions or updates you would like to share, write to pulp-list@redhat.com._

## 2020 Community Survey

Thanks to everyone who has taken the time to complete the survey so far. If you haven't yet, please complete the [annual community survey](https://forms.gle/2Q3Ta2n1AKAHiev9A) so that the Pulp team can better understand how you are using Pulp, what you need and what you would like to see.

## PulpCon 2020 - Save the Date

The first virtual PulpCon will take place during the week of September 14th. The sessions will be held over five half-days, Mon-Fri mornings in EST and afternoons in CET. The call for topics is now closed and the finalized timetable will be announced on the Pulp mailing list. Attending PulpCon has never been easier. Block off your calendar and join us from the comfort of your own home!  

## Verifying Contributions to pulp.plan.io

Because of a high volume of spam, Pulp intern Lubos Mjachky has added some checks to ensure that [pulp.plan.io](pulp.plan.io) remains a space for easy collaboration on Pulp features and issues. Users whose contributions contain untrusted outbound links will be required to perform additional follow-up actions to verify their identity.

## Pulpcore 3.6 is Generally Available

This release contains a number of major enhancements:

* Introduction of SSL to ensure the security of Pulp requests.
* Update of the REST API schema from OpenAPI v2 to OpenAPI v3.
* Addition of significant Role Based Access Control (RBAC) machinery plugin writers to enable RBAC in their plugins.  
* Remotes can be associated with a Repository and automatically used when syncing the Repository.
* Improvements to import functionality to better handle the import of split files.

For more information, see the [release announcement](https://pulpproject.org/2020/08/13/pulp-3.6-is-generally-available/).

A [Pulp File plugin 1.2](https://pulp-file.readthedocs.io/en/latest/changes.html#id1) release accompanied the Pulp 3.6 release, and a flurry of other plugin compatibility releases followed.

## Python Plugin Updates

Over the last month, there have been two beta updates to the Pulp Python plugin - [pulp_python 3.0.0b10](https://pulpproject.org/2020/08/06/pulp-python-3.0.0b10-is-generally-available/) and [pulp_python 3.0.0b11](https://www.redhat.com/archives/pulp-list/2020-August/msg00018.html). For the most part, these betas were compatibility releases with Pulpcore 3.5 and 3.6 respectively.

Gerrod Ubben, a Pulp intern, has spent the last number of months working with the Pulp Python team to give the Pulp Python content plugin some much needed TLC. This work involved contributing to the Python Bandersnatch community. For an overview of the improvements coming in an upcoming release of the Pulp Python plugin, watch Gerrod's demo:

<iframe width="560" height="315" src="https://www.youtube.com/embed/dEgyufQLuFA" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Pulp RPM Plugin 3.6.1 is Generally Available

This month, Pulp RPM 3.6 and an update 3.6.1 were released. As well as a number of bug fixes, this release includes the following features and enhancements:

* Up to 40% performance improvement around publishing. [#7289](https://pulp.plan.io/issues/7289)
* Added the ability for users to import and export distribution trees. [#6739](https://pulp.plan.io/issues/6739)
* Added import/export support for remaining advisory-related entities. [#6815](https://pulp.plan.io/issues/6815)
* Added the ability to associate a Remote with a Repository and automatically use when syncing the Repository. [#7159](https://pulp.plan.io/issues/7159)

For more information, see the [release announcement](https://www.mail-archive.com/pulp-list@redhat.com/msg05865.html).

## Pulp Container 2.0.0 is Generally Available

The central feature of this release includes new REST APIs to handle Docker and Podman clients push functionality. [#5027](https://pulp.plan.io/issues/5027)

This release also includes the following features:

* A new `ContainerPushRepository` type has been added to support writeable container registries. [#6825](https://pulp.plan.io/issues/6825)
* Push functionality has been restricted to Pulp admin users. [#6976](https://pulp.plan.io/issues/6976)
* A new `ContentRedirectContentGuard` type has been added to support token authentication for Pulp Container. [#6894](https://pulp.plan.io/issues/6894)
* Schema conversion handling has been added for S3 storage backends. [#6824](https://pulp.plan.io/issues/6824)
* The ability to exclude tags during syncing has been added to allow users to skip tags they don't want to sync. This can save both time and disk space. [#6922](https://pulp.plan.io/issues/6922)
* Clean-up functionality has been added so that when a push repository is deleted, the associated distribution is also deleted. [#7172](https://pulp.plan.io/issues/7172)

For more information, see the [release announcement](https://pulpproject.org/2020/08/18/pulp-container-2.0-is-generally-available/).

## Pulp Ansible 0.2.0 is Generally Available

In this release, the following functionality has been added:

* Added the ability to associate a Remote with a Repository and automatically use when syncing the Repository. [#7194](https://pulp.plan.io/issues/7194)

For more information, see the [release announcement](https://pulpproject.org/2020/08/19/pulp-ansible-0.2.0-is-generally-available/)


## Pulp 2-to-3 Migration Plugin 0.3.0 is Generally Available

This release includes the following features:

* A `GroupProgressReport` entry has been added so that users can track the progress of the migration. [#6769](https://pulp.plan.io/issues/6769)
* Compatibility changes were added to match the recent release of Pulp Container 2.0 plugin. [#7365](https://pulp.plan.io/issues/7365)

For more information, see the [release announcement](https://pulpproject.org/2020/08/26/pulp-2-to-3-migration-0.3.0-is-available/)


## Get Involved

The Pulp team is happy to answer any questions or receive any feedback sent to pulp-list@redhat.com.

You are also welcome to connect with the team to discuss general Pulp topics on the #pulp channel on Freenode IRC.

The Pulp team also hold regular open meetings:

* **Bug Triage** - every Tuesday and Friday 10:30 ET (either EST or EDT) in #pulp-meeting on Freenode (IRC). Come and
participate in real-time. See the [triage process](https://docs.pulpproject.org/bugs-features.html#triage) for
more details. You can also read through the [triage archives](https://logs.pulpproject.org/pulp-meeting/)
* **Open Floor** - every Tuesday and Friday immediately after Bug Triage in #pulp-meeting on Freenode
(IRC). It's a time for developer discussion, feedback, and decisions on code/issues/PRs/process
relating to pulpcore or plugins. To particiapte, put a topic on
[the agenda](https://hackmd.io/@pulp/triage/edit). Minutes will be sent to the pulp-dev mailing
list.
