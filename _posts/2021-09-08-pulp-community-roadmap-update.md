---
title: Pulp Community Roadmap Update
author: Melanie Corr
tags:
  - roadmap
---

Over the coming months, the Pulp team will focus on the following areas. As with all plans, this roadmap is subject to change. Many of the items in this plan factor into Katello and GalaxyNG roadmaps.
Further updates will be announced on the Pulp blog and community. If you have any questions or feedback, feel free to write or post to our community channels!

### OSTree plugin

While Pulp 2 had an OSTree plugin, Pulp 3 was released without any OSTree functionality.
We are happy to announce that a basic proof-of-concept OSTree plugin is now in the works.
The initial release of the plugin will focus on providing functionality for the basic Pulp workflow that includes synchronizing, importing, and consuming OSTree content in Pulp.
We will publish updates on this blog as the plugin takes shape.
If you would like to contribute to our efforts, or would like to see what we’re working on, you can take a look at our [github repo](https://github.com/pulp/pulp_ostree).

### Pulpcore

* RBAC - roles for Pulp Container and File plugins
* RBAC - content guard
* Alternate Content Sources - [check out the epic](https://pulp.plan.io/issues/7832) to learn more about what this involves
* Performance & Hardening
* Removing the legacy tasking system in favor of the distributed one introduced in Pulpcore 3.14
* Content signing and evaluating Sigstore integration


### RPM Plugin

* Alternate Content Sources -  [check out the epic](https://pulp.plan.io/issues/7832) to learn more about what this involves.

### Ansible Plugin

* Import/export functionality for disconnected environments
* Token support
* RBAC support
* Moving sync lists to the client side

### Pulp Container

* Continuing the efforts to enhance import/export workflow
* Token support enhancements

### Pulp CLI

* Improving documentation
* Alternate Content Source support

### Pulp Installer

* Better support for High Availability deployments
* Python 3.8 installation (EL8 module, Debian 11)
* pulp_installer 3.16.z plans to support installing any Pulpcore 3.16.z release

### Community/CI

* Planning Virtual PulpCon Meetup in Nov 2021
* Redmine → GitHub Issues migration
* [Discourse evaluation](https://discourse.pulpproject.org/) as a replacement for mailing list discussions on design, decision, and support topics
* Conference Prep (DevConf.us/cz, ConfigMgmtCamp, etc.)
* Container and OSTree related community articles
