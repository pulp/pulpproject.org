---
title: Automating Pulp Debian workflows with Pulp Squeezer
author: Melanie Corr, Matthias Dellweg
tags:
  - release
---

We are happy to announce the release of Pulp Squeezer 0.0.7!

With the [Pulp 3 Debian](https://docs.pulpproject.org/pulp_deb/installation.html) plugin, you can take full control over the management and distribution of your Debian and Ubuntu packages. With this release of [Pulp Squeezer](https://galaxy.ansible.com/pulp/squeezer), you can automate many aspects of the Pulp Debian workflow using Ansible.

[Pulp Squeezer](https://galaxy.ansible.com/pulp/squeezer) is a growing collection of Ansible modules that you can use to manage Pulp.

As part of this release, you can now use Pulp Squeezer to automate workflows with the following new modules:

* `deb_distribution`
* `deb_publication`
* `deb_remote`
* `deb_repository`
* `deb_sync`

The first four in the list, associated with actual resources, provide the `state` parameter to allow for idempotent playbooks the ansible way.
The `deb_sync` module however, being an action module, you might want to register as a [handler](https://docs.ansible.com/ansible/latest/user_guide/playbooks_handlers.html).

This release also includes the automatic refreshing of the API docs in the status module if version discrepancies are detected.

If you’re new to Pulp and looking for a better way to manage your Debian content, take a look at the [introduction to Pulp Debian](https://opensource.com/article/20/10/pulp-debian) article. This article contains an example workflow of how to get started with managing Debian content in Pulp. For more detail, see the [Pulp Debian documentation](https://docs.pulpproject.org/pulp_deb/installation.html).

## Try Pulp Squeezer today

You can find Pulp Squeezer on [Ansible Galaxy](https://galaxy.ansible.com/pulp/squeezer).

You can install with the following command:

```
ansible-galaxy collection install pulp.squeezer
```

If you’ve any questions or feedback, we would love to hear from you on our pulp-list@redhat.com mailing list!
