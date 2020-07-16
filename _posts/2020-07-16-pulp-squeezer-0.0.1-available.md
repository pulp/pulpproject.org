---
title: Announcing Pulp Squeezer 0.0.1
author: Melanie Corr
tags:
  - release
---

Pulp Squeezer, formerly known as Pulp Ansible Modules, is a collection of Ansible modules that you can use to manage Pulp.

Previously, you could use Ansible modules only to manage File content in Pulp. With this 0.0.1 release, you can fetch, upload, organize, and distribute File, Ansible, and Python content.

Installing collections with `ansible-galaxy` is only supported in Ansible 2.9 and higher.

Check out Pulp Squeezer on [Ansible Galaxy](https://galaxy.ansible.com/pulp/squeezer).

## Available modules

For Ansible content:

*  `ansible_distribution`
*  `ansible_remote`
*  `ansible_repository`
*  `ansible_role`
*  `ansible_sync`

For File content:

*  `file_content`
*  `file_distribution`
*  `file_publication`
*  `file_remote`
*  `file_repository`
*  `file_sync`

For Python content:

*  `python_distribution`
*  `python_publication`
*  `python_remote`
*  `python_repository`
*  `python_sync`

Pulp general:

*  `artifact`
*  `delete_orphans`
*  `status`
*  `task`
