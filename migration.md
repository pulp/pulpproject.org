---
title: Migrate to Pulp 3
sidebar: home_sidebar
permalink: /migrate-to-pulp-3/
toc: false
---

## Migrate from Pulp 2 to Pulp 3

For many years, the Pulp team have worked to gather and implement feedback from user deployments of Pulp 2. The solution to much of the feedback lay outside of the constraints of the Pulp 2 architecture. This led to the development of Pulp 3, which is not only an update of Pulp 2, but a complete technical overhaul to provide the most effective repository management platform.

At the moment, Pulp 2 has entered maintenance mode. This means that apart from critical bug fixes and security issues, there will be no further feature enhancements or bug fixes for Pulp 2. Much of the technical debt of the Pulp 2 project and many of the feature requests have been or will be implemented in Pulp 3.

If you are a Pulp 2 user, start planning your migration to Pulp 3.

## Pulp 2 to Pulp 3 Migration Plugin

You can use the migration plugin to migrate your content from Pulp 2 to Pulp 3. The development of this plugin is a work in progress. You can help support the development of this plugin by providing feedback on the different scenarios that you require to complete your migration. Currently, the migration plugin supports the following scenarios:

*  Pulp 2 ISO content can be migrated into Pulp 3 File.
*  Pulp 2 Docker can be migrated into Pulp 3 Container.
*  Pulp 2 RPM can be migrated into Pulp 3 RPM. A beta release is available.
*  Pulp Debian migration is currently available as a tech preview.

You can watch Tanya discuss the migration process in this video. Since the creation of this video, the migration plugin has been enhanced and you can use the Pulp 3 CLI to complete the migration workflows. At the time of writing, via the Pulp 3 CLI you can conduct plan management and the investigation of Pulp 2 content and repositories.

<iframe width="560" height="315" src="https://video.fosdem.org/2021/D.infra/dontgetstuckonpulp2.mp4" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

For more up-to-date information, see the [changelog](https://docs.pulpproject.org/pulp_2to3_migration/changes.html).

Take the time to familiarize yourself with the migration plugin, make a plan, and test the migration. For more information, see the [Migration Plugin documentation](https://docs.pulpproject.org/pulp_2to3_migration/).

If you want to extend the existing functionality, feel free to raise a [pull request](https://github.com/pulp/pulp-2to3-migration).

To view current progress and outstanding issues, see the [Migration plugin tracker](https://github.com/pulp/pulp-2to3-migration/issues).

If you have any questions or feedback, write to the pulp-dev@redhat.com mailing list.
