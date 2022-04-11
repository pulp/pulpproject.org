---
title: Ansible Installer
sidebar: home_sidebar
permalink: /ansible-installer/
toc: false
---

If you have a basic knowledge of Ansible, you can use the `pulp_installer` to automate the installation of Pulp 3. The `pulp_installer` is a collection of Ansible roles hosted on [Ansible Galaxy](https://galaxy.ansible.com/pulp/pulp_installer). Each Ansible role installs and configures a component of Pulp. If you run the `pulp_installer` on an existing Pulp 3 deployment, it will upgrade it to the latest version. You can also use the `pulp_installer` to add or reconfigure plugins for an existing Pulp installation.

Note that to install Pulp 3 using `pulp_installer`'s collection of Ansible roles hosted on Ansible Galaxy, you must use Ansible 2.9 or higher.

For information about system requirements, recommended workflows and instructions on how to install Pulp 3 using the Ansible `pulp_installer`, see the [Pulp installer documentation](https://docs.pulpproject.org/pulp_installer/).

Pulp's Mike did a demo of running the installer at [PulpCon 2021](https://youtube.com/playlist?list=PLwm8_O6oKSS3GEl1NHgtY4G-Tq2byivpa)

<iframe width="560" height="315" src="https://www.youtube.com/embed/IjZrOQH7Uqk" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

# Quickstart Guide

For a more comprehensive overview of how the Ansible Installer works, as well as a description of workflows, see the the [Pulp installer documentation](https://docs.pulpproject.org/pulp_installer/).


Installation
------------

Enter the following command to install everything you need from Ansible Galaxy:

```
ansible-galaxy collection install pulp.pulp_installer
```

Using the example playbook
--------------------------

Some of the roles used in the playbook use root privileges on the managed node, so when prompted,
you must provide the password for the managed node user.

```
ansible-playbook playbooks/example-use/playbook.yml -u <managed_node_username> --ask-become-pass -i <managed_node_hostname>,
```

<script id="asciicast-335159" src="https://asciinema.org/a/335159.js" async data-autoplay="true" data-speed="2"></script>

To configure a custom installation, you must set configuration variables. In the simplest case,
they can be set in the playbook. See the Ansible docs for more flexible idiomatic alternatives.


Example Playbook for Installing Plugins
-----------------
The following example is an Ansible playbook that you can use for installing `pulp_container` and `pulp_rpm`.
You can learn more about the variables on the [roles section](https://docs.pulpproject.org/pulp_installer/roles/pulp_common/#role-variables)

1 -  Install the `pulp_installer` collection:
```
ansible-galaxy collection install pulp.pulp_installer
```

2 -  Install the `geerlingguy.postgresql` role:
```
ansible-galaxy install geerlingguy.postgresql
```

3 - Write the following playbook:
```
vim install.yml
```


```yaml
---
- hosts: all
  force_handlers: True
  vars:
    pulp_settings:
      secret_key: << YOUR SECRET HERE >>
      content_origin: "http://{{ ansible_fqdn }}"
    pulp_default_admin_password: << YOUR PASSWORD HERE >>
    pulp_install_plugins:
      # galaxy-ng: {}
      # pulp-2to3-migration: {}
      # pulp-ansible: {}
      # pulp-certguard: {}
      pulp-container: {}
      # pulp-cookbook: {}
      # pulp-deb: {}
      # pulp-file: {}
      # pulp-gem: {}
      # pulp-maven: {}
      # pulp-npm: {}
      # pulp-python: {}
      pulp-rpm: {}
  roles:
    - pulp.pulp_installer.pulp_all_services
  environment:
    DJANGO_SETTINGS_MODULE: pulpcore.app.settings
```
4 - Run the playbook:
```
ansible-playbook install.yml -u <managed_node_username> --ask-become-pass
```
<script id="asciicast-335829" src="https://asciinema.org/a/335829.js" async data-autoplay="true" data-speed="2"></script>
