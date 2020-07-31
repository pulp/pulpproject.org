---
title: Pulp Community Update (July 2020)
author: Melanie Corr
tags:
  - newsletter
---

If you have any questions or updates you would like to share, write to us at pulp-list@redhat.com.

## 2020 Community Survey

If you are a Pulp user, please complete the [annual community survey](https://forms.gle/2Q3Ta2n1AKAHiev9A) so that the Pulp team can better understand how the community is using Pulp, what the community needs, and would like to see.

## Managing Pulp with Ansible - Pulp Squeezer

[Pulp Squeezer](/pulp-squeezer/), formerly known as Pulp Ansible Modules, is a collection of Ansible modules that you can use to manage Pulp. This month, versions 0.0.1 and a follow up 0.0.2 have been released.

<script id="asciicast-amFEhvEprDLB2UPgDcgJg4knz" src="https://asciinema.org/a/amFEhvEprDLB2UPgDcgJg4knz.js" async></script>

Check out Pulp Squeezer on [Ansible Galaxy](https://galaxy.ansible.com/pulp/squeezer).

## Pulp 3 at the Foreman Birthday Party

As part of the Foreman's 11th Birthday Party celebrations, Justin Sherrill spoke about the performance improvements Katello will enjoy by migrating from Pulp 2 to Pulp 3.

<iframe width="560" height="315" src="https://www.youtube.com/embed/b6HsSm10DLk" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Pulpcore 3.5 is available

As well as a number of bug fixes, this release contains major updates to the Pulp installer to achieve Service Oriented Architecture (SOA). With this release, there is one role per service, which means that you can install each service on an individual server. Because of the scale of these changes, this release introduces some breaking changes. As part of the upgrade, you must also update existing playbooks.  For more information, see [Pulp Installer 3.5 new roles and new deployment scenarios](https://pulpproject.org/2020/07/09/pulp-3.5-installer-roles/)

The following features have been added to the REST API:

* By default, Pulp now provides a user agent with all `aiohttp` requests to identify itself.
* A `start_versions=`  has been added so export a specific set of repository versions. For more information, see [Exporting Incrementally](https://docs.pulpproject.org/en/3.5/workflows/import-export.html?highlight=start_versions#exporting-incrementally) in the Pulp documentation.
* GroupProgressReport has been added to track the progress of each task in that group. For more information, see *GroupProgressReport can* in the [Tasks](https://docs.pulpproject.org/plugins/plugin-writer/concepts/index.html#tasks) concept section of the Plugin Writers Guide

The following features have been added to the Plugin API:

* OpenAPI Tags have been added to group operations into logical categories. You can also customize tags. For more information. see [OpenAPI Tags](https://docs.pulpproject.org/plugins/reference/how-to-doc-api.html?highlight=group%20operations%20logically%20categories#openapi-tags)
* GroupProgressReport has been added to track the progress of each task in that group. For more information, see *GroupProgressReport can* in the [Tasks](https://docs.pulpproject.org/plugins/plugin-writer/concepts/index.html#tasks) concept section of the Plugin Writers Guide
* SingleContentArtifactField and PulpTemporaryUploadedFile have been exported to the Plugin API

For information about installing or updating to Pulpcore 3.5, see the [full announcement](https://www.redhat.com/archives/pulp-list/2020-July/msg00010.html).

## Pulp 2.21.3 is available

For more information about the issues addressed see the [release announcement](/2020/07/13/pulp-2.21.3-generally-available/).

Pulp 2 is in [maintenance mode](/2020/06/15/pulp-2-enters-maintenance-mode/). Start planning your [migration](/migrate-to-pulp-3/) to Pulp 3 today!

## Content Plugin Releases

### Pulp RPM 3.5 is available

After a 3.4.2 bug-fix-focussed release earlier this month, the Pulp RPM team has released a feature-packed Pulp RPM 3.5 plugin. As well as a number of bug fixes, this release provides the following features:

* A new retention policy feature that you can use to specify the latest _N_ versions of each package that you want to keep while purging older versions
* Support for comparing packages by epoch, version, release (EVR)
* Support for synchronizing from a mirror list feed
* The comps file types `PackageCategory`, `PackageEnvironment`, and `PackageGroup` can copy their children
* Support for synchronizing SUSE enterprise repositories with an authentication token

For more information, see the [changelog](https://pulp-rpm.readthedocs.io/en/3.5/changes.html#id1) and [release announcement](https://www.mail-archive.com/pulp-list@redhat.com/msg05832.html).


### Pulp File 1.1.0 is available

This bug fix release has added the `requirements.txt` file to the Pulp File 1.1.0 package that was not included in the Pulp File 1.0.0 package.

For more information, see the [changelog](https://pulp-file.readthedocs.io/en/latest/changes.html#id1) and the [release announcement](https://www.mail-archive.com/pulp-list@redhat.com/msg05832.html).


###  Pulp Container 1.4.2 is available

The Pulp Container team has produced a bug fix release of 1.4 that improves performance synchronization. For more information, see the [changelog](https://pulp-container.readthedocs.io/en/1.4/changes.html#id1) and the [release announcement](https://www.mail-archive.com/pulp-list@redhat.com/msg05841.html).

## Content Plugin Beta Releases

### Pulp Debian 2.5.0 Beta 1 is available

This release includes adding more metadata fields to published Release files, as well as some bug fixes.

For more information see the [changelog](https://pulp-deb.readthedocs.io/en/latest/changes.html#b1-2020-07-15) and the [release announcement](https://www.redhat.com/archives/pulp-list/2020-July/msg00015.html).

### Pulp Ansible Plugin 0.2.0 Beta 15 is available

This release provides the ability to enable token authentication for synchronizing Ansible Collections.

For more information, see the [changelog](https://pulp-ansible.readthedocs.io/en/latest/changes.html#b15-2020-07-14) and the [release announcement](https://www.mail-archive.com/pulp-list@redhat.com/msg05836.html).

### Pulp Container 2.0.0 Beta 3 is available

This beta update contains a number of bug fixes as well as the following features:

* Schema conversion, which was available in Container plugin 1.4, is now available in this beta release. This feature works with both the local filesystem and S3 storage backends.
* A new repository type `ContainerPushRepository` has been added to support writable container registries.
`ContentRedirectContentGuard` has been added to facilitate authentication.
* Push access has been restricted to an admin user.  

For more information, see the [changelog](https://pulp-container.readthedocs.io/en/2.0.0b3/changes.html#b3-2020-07-16) and [release announcement](https://www.mail-archive.com/pulp-list@redhat.com/msg05841.html).

## Pulp Certguard 1.0.0 is available

The Certguard plugin provides a X.509 capable `ContentGuard` that requires clients to submit a certificate proving their entitlement to content before receiving content from Pulp.

For more information, see [Certguard plugin](/certguard-plugin/)

## Pulp 3 Concurrency Testing

Syncing content into Pulp 3 is a time-consuming process. Grant Gainey conducted some evaluations of sync-performance at various concurrency settings. Read more in his [blog](/2020/07/31/concurrency-testing/).

## Work in Progress

### Role Based Access Control (RBAC) Proof of Concept is ready for review and feedback.

Brian Bouterse hosted an open meeting where he gave an overview of the RBAC Proof of Concept:

<iframe width="560" height="315" src="https://www.youtube.com/embed/tWEHGJmBqCk" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### Import Export Keys

Grant Gainey hosted an open meeting where he outlined the issues around Pulp 3 import/export handling of entities without a ‘natural’ key. You can find the document that was discussed [here](https://hackmd.io/@ggainey/importexport_lowlevel_naturalkeys)

<iframe width="560" height="315" src="https://www.youtube.com/embed/Vk5qiM9yPHQ" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


If you have any feedback, submissions, or comments on anything discussed here, feel free to write to us at pulp-list@redhat.com
