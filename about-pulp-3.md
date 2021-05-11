---
title: About Pulp 3
sidebar: home_sidebar
permalink: /about-pulp-3/
toc: false
---

Pulp 3 builds on the achievements of Pulp 2, with a renewed focus on repository management. Pulp 3 is more than an update of Pulp 2. Pulp 3 is the result of years of feedback from Pulp 2 production scenarios and serves to address key concerns that could not be resolved within the existing Pulp 2 architecture.

Pulp 3 reduces the challenges and dependencies that users experienced with Pulp 2. Built on Django, and using open standards such as OpenAPI, Pulp 3 has increased reliability and flexibility while reducing the code base. Plugin writing has become easier thanks to the improved Plugin API. Performance speeds, data integrity, and safety features, such as roll back to specific versions, have guided the development of Pulp 3.

Pulp 3 and Pulp 2 have several conceptual differences. For more information about conceptual and terminology differences between Pulp 3 and Pulp 2, see [Renamed concepts](https://docs.pulpproject.org/from-pulp-2.html#renamed-concepts).

## Pulp 3 vs Pulp 2 At A Glance

Pulp 3 has the following advantages and features when compared to Pulp 2:

* Improved performance and data integrity. Pulp 3 moves from MongoDB to PostgreSQL.
* Faster performance. On testing, synchronizing in Pulp 3 ranges from two to eight times faster than Pulp 2.
* Versioned repositories feature. New ability to rollback safely to earlier versions.
* Disk storage and speed solutions. Select from [multiple synchronization](https://docs.pulpproject.org/workflows/on-demand-downloading.html) options for all content types to suit specific environmental requirements.
* More storage options. Store content in the cloud with [storage services like S3 and Azure](https://docs.pulpproject.org/installation/storage.html).
* Simplified installation. Ansible can automate Pulp 3 installation.
* [Client bindings](https://docs.pulpproject.org/client_bindings.html) make Pulp’s API language agnostic, requiring no customization.
* Browseable, auto-documented, OpenAPI.
* Python’s PyPI replaces RPMs as the main packaging method, which allows for easy installation across Linux distributions.
* Improvements to the Plugin API ensures compatibility with Pulp core and has led to [an increasing list of Plugins](/content-plugins/).

### Functionality that has been dropped or replaced entirely with Pulp 3

System management features, which were present in Pulp 2, have been removed so that Pulp 3 maintains focus on providing the best and most stable repository management experience. Other tools, such as Ansible, can be used for system management. Some other services have been dropped as part of Pulp 3. For more information, see [Deprecating Consumers](/2018/03/01/deprecating-consumers/).

* Pulp agent
* Scheduled tasks
* The entire Celery stack has been replaced by [RQ](/2018/05/08/pulp3-moving-to-rq/)
* Pulp 2’s nodes concept has been removed in favor of Pulp server-to-server syncing

### Functionality that does not currently exist but might be subject to change

* Role-based access control is not available
* The [Pulp 3 CLI](https://github.com/pulp/pulp-cli) is in beta and a work in progress. 
* No Puppet or OSTree plugins are available


## What do you think?

Pulp 3 continues to provide the Pulp 2 core features while providing several key improvements along with an updated tech stack. If you have any questions or feedback, or would like to know more about the key changes, write to the [Pulp mailing list](https://www.redhat.com/mailman/listinfo/pulp-list) or join the conversation on [IRC](/help/#irc).
