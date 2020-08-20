---
title: Pulp Ansible 0.2.0 is Generally Available
author: Melanie Corr
tags:
  - release
---

The Pulp team is pleased to announce the release of the Pulp Ansible 0.2.0 plugin.

## About the Pulp Ansible content plugin

If you’re an Ansible user and do not want to host your private content on Ansible Galaxy, you can add the Pulp Ansible plugin to Pulp. You can then mirror the public Ansible content that you require and use Pulp as an on-premise platform to manage and distribute a scalable blend of public and private Ansible roles and collections across your organization.

With the Pulp Ansible plugin, you can complete the following workflows via the Pulp API. Ansible content includes both roles and collections:

* Mirror a subset of Ansible content on-premise
* Mirror all of Ansible Galaxy’s content on-premise
* Create a distribution and use the `ansible-galaxy` command to install content from this distribution

For more information, see the [roles](https://pulp-ansible.readthedocs.io/en/latest/workflows/roles.html) and [collections](https://pulp-ansible.readthedocs.io/en/latest/workflows/collections.html) workflows in the Pulp Ansible documentation.

The Pulp Ansible plugin also has the following features:

* Store private Ansible collections on-premise using the `ansible-galaxy collection publish` command
* Version Ansible content over time and roll back if necessary
* Support for the new multi-role content type from Ansible Galaxy

## Release Features

In this release, which is compatible with Pulp 3.6, the following functionality has been added:

* Users can now associate a remote with a repository and automatically use this remote when syncing the repository. [#7194](https://pulp.plan.io/issues/7194)

As part of this release, artifacts are no longer persisted when importing collections [#6718](https://pulp.plan.io/issues/6718)

For more information, see the [Ansible plugin docs](https://pulp-ansible.readthedocs.io/en/latest/index.html).

For more information about work in progress, see the [Pulp Ansible redmine ](https://pulp.plan.io/projects/ansible_plugin/).

## Get Pulp Ansible 0.2.0

* PyPI: [https://pypi.org/project/pulp-ansible/0.2.0/](https://pypi.org/project/pulp-ansible/0.2.0/)
* Python bindings: [https://pypi.org/project/pulp-ansible-client/0.2.0/](https://pypi.org/project/pulp-ansible-client/0.2.0/)
* Ruby bindings: [https://rubygems.org/gems/pulp_ansible_client/versions/0.2.0](https://rubygems.org/gems/pulp_ansible_client/versions/0.2.0)
