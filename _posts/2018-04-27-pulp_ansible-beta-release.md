---
title: Pulp Ansible Plugin Beta 1 Release
author: David Davis
tags:
  - release
  - 3.0
---

A few days ago, [we announced the beta 1 release of Pulp 3.0 Core](/2018/04/25/beta-release/). To
coincide with this release, today we are announcing the beta 1 release of the pulp_ansible plugin.
This is a brand new plugin that allows you to manage your Ansible content using Pulp. Below you will
find information about the plugin, its features, how to install it, and a demo video.

**Please note that this plugin is available for Pulp 3.0 only and not Pulp 2.0.**

## Pulp Ansible Plugin Features

The pulp_ansible plugin supports the following features:

- Syncing Ansible Roles to Pulp from galaxy.ansible.com
- Managing Ansible Roles with repositories and repository versions
- Uploading Ansible Roles to Pulp through the REST API
- Publishing and distributing Ansible Roles using the Galaxy API
- Installation of Ansible Roles from Pulp using the ansible-galaxy CLI client

## Documentation

To find out how to install pulp_ansible or to read our docs around how to use pulp_ansible, check
out the [pulp_ansible repository on Github](https://github.com/pulp/pulp_ansible).

## Getting involved

To view our progress, report bugs, or request new features, check out our issue tracker at
[https://pulp.plan.io/projects/ansible_plugin](https://pulp.plan.io/projects/ansible_plugin).

## Demo video

<iframe width="560" height="315" src="https://www.youtube.com/embed/EnD8BGcNcjU" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>
