---
title: Pulp Community Roadmap Update
author: Melanie Corr
tags:
  - roadmap
---

Over the coming months, the Pulp team will focus on the following areas. As with all plans, this roadmap is subject to change. Many of the items in this plan factor into Katello and GalaxyNG roadmaps.
Further updates will be announced on the Pulp blog and community. If you have any questions or feedback, feel free to write to post to our community channels!

### OSTree plugin

While Pulp 2 had an OSTree plugin, Pulp 3 was released without any OSTree functionality.
The Pulp 2 OSTree plugin primarily managed Fedora Atomic Host content that has been EOL for several years.
We are happy to announce that a basic proof-of-concept OSTree plugin is now in the works.
We will publish updates on this blog as the plugin takes shape.
If you would like to contribute to our efforts, or would like to see what we’re working on, you can take a look at our [github repo](https://github.com/pulp/pulp_ostree).

### Pulpcore

* RBAC - roles for Pulp Container and File plugins
* RBAC - content guard
* Encrypted fields
* Alternative Content Sources - [check out the epic](https://pulp.plan.io/issues/7832) to learn more about what this involves.
* Disk usage
* Orphan cleanup
* Django 3 and Python 3.7+ support
* Performance & Hardening


### RPM Plugin

* Alternative Content Sources -  [check out the epic](https://pulp.plan.io/issues/7832) to learn more about what this involves.

### Ansible Plugin

* Import/export functionality for disconnected environments
* Token support
* RBAC support
* Moving sync lists to the client side

### Pulp Container

* DRF token support
* Continuing the efforts to enhance import/export workflow

### Pulp CLI

* Improving documentation
* ACS support

### Pulp Installer

* Better support for High Availability deployments
* Python 3.8 installation (EL8 module, Debian 11)
* Installer for mirror branch model

### Community/CI

* Planning Virtual PulpCon Meetup in Nov 2021
* Redmine → GitHub Issues migration
* Discourse evaluation as replacement of mailing list for design, decision, and support discussions
* Conference Prep (DevConf.us/cz, ConfigMgmtCamp, etc.)
