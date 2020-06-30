---
title: Ansible Installer
sidebar: home_sidebar
permalink: /ansible-installer/
toc: false
---

If you have a basic knowledge of Ansible, you can use the `pulp_installer` to automate the installation of Pulp 3. The `pulp_installer` is a collection of Ansible roles hosted on [Ansible Galaxy](https://galaxy.ansible.com/pulp/pulp_installer). Each Ansible role installs and configures a component of Pulp. If you run the `pulp_installer` on an existing Pulp 3 deployment, it will upgrade it to the latest version. You can also use the `pulp_installer` to add or reconfigure plugins for an existing Pulp installation.  

Note that to install Pulp 3 using `pulp_installer`'s collection of Ansible roles hosted on Ansible Galaxy, you must use Ansible 2.9 or higher.

For information about system requirements, recommended workflows and instructions on how to install Pulp 3 using the Ansible `pulp_installer`, see the [Pulp installer documentation](https://pulp-installer.readthedocs.io/).
