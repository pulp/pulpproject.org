---
title: The pulp-2to3-migration 0.3.0 plugin is Generally Available
author: Melanie Corr
tags:
  - release
---

The Pulp team is pleased to announce the release of the 0.3.0 version of the Pulp 2-to-3 Migration plugin.

You can use this plugin to migrate content from Pulp 2 into Pulp 3. At the moment, the migration plugin covers the following scenarios:

* Pulp 2 ISO content can be migrated into Pulp 3 File.
* Pulp 2 Docker content can be migrated into Pulp 3 Container.
* Pulp 2 RPM content can be migrated into Pulp 3 RPM.

This release is compatible with Pulpcore 3.6 and the following content plugins:

* Pulp File 1.2
* Pulp Container 2.0
* Pulp RPM 3.6

You can migrate plugin by plugin, or all plugins together.

## Release Features

This release includes the following features:

* A `GroupProgressReport` entry has been added so that users can track the progress of the migration. [#6769](https://pulp.plan.io/issues/6769)
* Compatibility changes were added to match the recent release of Pulp Container 2.0 plugin. [#7365](https://pulp.plan.io/issues/7365)

## Bug Fixes

* Added significant performance improvements to partial migrations of additional content. [#6111](https://pulp.plan.io/issues/6111)
* Fixed a problem that caused a migration failure for a distribution tree if it has a treeinfo and not `.treeinfo` . [#6951](https://pulp.plan.io/issues/6951)
* Fixed an issue with the labeling of Pulp 2 Lazy Content catalog entries to prevent false negatives and migration failures on re-migration. [#7260](https://pulp.plan.io/issues/7260)

## Warning

You must migrate Pulp 2 content to a Pulp 3 instance that has not been used for the content plugin that you want to migrate. The migration process can remove some Pulp 3 data related to the plugin that is being migrated. Naming and distribution paths of migrated and Pulp 3 content can clash.

#### Safe Example

If you use Pulp 3 with the `pulp_file` plugin and now want to migrate RPM content from Pulp 2 to Pulp 3. If you haven't used the Pulp 3 RPM plugin, then you can safely migrate and start using the RPM plugin.

#### Unsafe Example

If you use Pulp 3 with the RPM plugin (version 3.x) and decide to migrate Pulp 2 RPM content to Pulp 3, you will encounter problems.
