---                                                                                                                    
title: Pulp 2 Enters Maintenance Mode
author: Melanie Corr
tags:
  - announcement
---

In 2013, the release of Pulp 2 made huge strides in creating an extendable framework for managing several content types. Over the last number of years, the Pulp team have examined user deployments to continue improving Pulp's capabilities as a content management platform. This led to the release of Pulp 3 in December 2019, which includes a multitude of new features, performance improvements, and increased flexibility. For more information on the differences between Pulp 2 and Pulp 3, see [About Pulp 3](https://pulpproject.org/about-pulp-3/).

The Pulp team's main focus is to continue developing the best open source repository management platform available. This means prioritizing the Pulp 3 road map. At this point, apart from critical bug fixes and security issues, there will be no further feature enhancements or bug fixes for Pulp 2. Much of the technical debt of the Pulp 2 project and many of the feature requests have been or will be implemented in Pulp 3.

## Migrate to Pulp 3

If you are an existing Pulp 2 user, start working on a migration plan today.

You can use the migration plugin to migrate your content from Pulp 2 to Pulp 3. The development of this plugin is a work in progress. You can help support the development of this plugin by providing feedback on the different scenarios that you require to complete your migration. Currently, the migration plugin supports the following scenarios:

*  Pulp 2 ISO content can be migrated into Pulp 3 File.
*  Pulp 2 Docker can be migrated into Pulp 3 Container.
*  RPM plugin migration functionality is almost feature complete. A [0.2.0 beta 2](https://pulp-2to3-migration.readthedocs.io/en/latest/changes.html#b2-2020-04-22) release is available.

For more up-to-date information, see the [changelog](https://pulp-2to3-migration.readthedocs.io/en/latest/changes.html).

Take the time to familiarize yourself with the migration plugin, make a plan, and test the migration. For more information, see the [Migration Plugin documentation](https://pulp-2to3-migration.readthedocs.io/en/latest/index.html).

If you want to extend the existing functionality, feel free to raise a [pull request](https://github.com/pulp/pulp-2to3-migration).

To view current progress and outstanding issues, see the [Migration plugin tracker](https://pulp.plan.io/projects/migration).

If you have any questions or feedback, write to the pulp-list@redhat.com mailing list.
