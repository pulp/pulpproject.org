---
title: Migrate to Pulp 3
sidebar: home_sidebar
permalink: /migrate-to-pulp-3/
toc: false
---

## Migrate from Pulp 2 to Pulp 3

For many years, the Pulp team have worked to gather and implement feedback from user deployments of Pulp 2. The solution to much of the feedback lay outside of the constraints of the Pulp 2 architecture. This led to the development of Pulp 3, which is not only an update of Pulp 2, but a complete technical overhaul to provide the most effective repository management platform.

At the moment, the Pulp team supports both Pulp 2 and Pulp 3. However, at some point in the future, support will end for Pulp 2 and focus exclusively on Pulp 3.  

If you are a Pulp 2 user, start planning your migration to Pulp 3.

## Pulp 2 to Pulp 3 Migration Plugin

You can use the migration plugin to migrate your content from Pulp 2 to Pulp 3. The development of this plugin is a work in progress. You can help support the development of this plugin by providing feedback on the different scenarios that you require to complete your migration. Currently, the migration plugin supports the following scenarios:

*  Pulp 2 ISO content can be migrated into Pulp 3 File.
*  Pulp 2 Docker can be migrated into Pulp 3 Container.
*  RPM plugin migration functionality is almost feature complete. A [0.2.0 beta 2](https://pulp-2to3-migration.readthedocs.io/en/latest/changes.html#b2-2020-04-22) release is available.

For more up-to-date information, see the [changelog](https://pulp-2to3-migration.readthedocs.io/en/latest/changes.html).

Take the time to familiarize yourself with the migration plugin, make a plan, and test the migration. For more information, see the [Migration Plugin documentation](https://pulp-2to3-migration.readthedocs.io/en/latest/index.html).

If you want to extend the existing functionality, feel free to raise a [pull request](https://github.com/pulp/pulp-2to3-migration).

To view current progress and outstanding issues, see the [Migration plugin tracker](https://pulp.plan.io/projects/migration).

If you have any questions or feedback, write to the pulp-dev@redhat.com mailing list.
