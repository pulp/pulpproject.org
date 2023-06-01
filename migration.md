---
title: Migrate to Pulp 3
sidebar: home_sidebar
permalink: /migrate-to-pulp-3/
toc: false
---

## Migrate from Pulp 2 to Pulp 3

For many years, the Pulp team have worked to gather and implement feedback from user deployments of Pulp 2. The solution to much of the feedback lay outside of the constraints of the Pulp 2 architecture. This led to the development of Pulp 3, which is not only an update of Pulp 2, but a complete technical overhaul to provide the most effective repository management platform.

Pulp 2 is now considered EOL, and is not supported.

If you are still a Pulp 2 user, plan your migration to Pulp 3.

## Pulp 2 to Pulp 3 Migration Plugin

You can use the migration plugin to migrate your content from Pulp 2 to Pulp 3. The migration plugin covers the following scenarios:

*  Pulp 2 ISO content can be migrated into Pulp 3 File.
*  Pulp 2 Docker can be migrated into Pulp 3 Container.
*  Pulp 2 RPM can be migrated into Pulp 3 RPM.
*  Pulp 2 Debian can be migrated into Pulp 3 Debian.

You can watch Tanya discuss the migration process in this video. Since the creation of this video, the migration plugin has been enhanced and you can use the Pulp 3 CLI to complete the migration workflows.

<iframe width="560" height="315" src="https://video.fosdem.org/2021/D.infra/dontgetstuckonpulp2.mp4" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

For more information, see the [Migration Plugin documentation](https://docs.pulpproject.org/pulp_2to3_migration/).
Pulp migration plugin is now considered EOL, and is not supported.

