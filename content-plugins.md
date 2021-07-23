---
title: Content Plugins
sidebar: home_sidebar
permalink: /content-plugins/
toc: true
---

## Content Plugins

As part of the Pulp installation, you must add a content plugin for each content type that you want
to manage.
The following sections contain information about the available content plugins for both Pulp 2 and
Pulp 3. If you do not find the plugin for the content type you want to manage, consider
[writing a plugin](https://docs.pulpproject.org/plugins/plugin-writer/index.html).

## Plugin Changes between Pulp 2 and Pulp 3

* The ISO content support found in the Pulp 2 RPM plugin is now provided by the File plugin.
* The Docker plugin in Pulp 2 has been replaced by the Container plugin.
* Currently, there are no Pulp 3 plugins for Puppet and OSTree content.

## Pulp 3 CLI Coverage

The [Pulp 3 CLI](https://github.com/pulp/pulp-cli) is a work in progress.
The CLI is implemented by the plugin writers, so the workflows that are possible with the CLI vary
from plugin to plugin.
At the moment, these plugins have the following coverage:

* **RPM**: Sync workflow
* **File**: Sync workflow; content upload
* **Ansible**: Sync workflow (role & collection)
* **Container**: Sync workflow
* **Python**: Sync workflow

## Cloud Storage

It is possible to configure Pulp to use
[cloud storage](https://docs.pulpproject.org/pulpcore/installation/storage.html).
However, plugins can introduce changes that are incompatible with, for example, S3 requirements.
The Pulp Ansible, RPM, File, Debian, Python, and Container plugins are regularly tested to ensure they remain compatible but the
level of coverage is lower for other plugins.

## Pulp 3 Content Plugin Features


### RPM

* Create, sync and publish a repository with RPM Content including RPMs, Advisories, Modularity, and
 Comps.
* Version content and rollback if necessary.
* Download content on-demand when requested by clients to reduce disk space.
* Upload local RPM content in chunks.
* Add, remove, copy, and organize RPM content into various repositories.
* Host content either locally or on S3.
* View distributions served by pulpcore-content in a browser.
* De-duplication of all saved content.

### File

* Sync File packages from a remote to local repository.
* Upload your own files.
* Publish and organize files.

### Container

For an in-depth look at the available workflows, see the
[Pulp Container workflow documentation](https://docs.pulpproject.org/pulp_container/workflows).

You can read about Pulp container's Role Based Access Control(RBAC) support
[here](https://docs.pulpproject.org/pulp_container/role-based-access-control.html).

* Synchronize container image repositories hosted on Docker-hub, Google Container Registry,
Quay.io, and any other that is Docker Registry HTTP API V2-compatible in mirror or additive mode.
* Version content and rollback if necessary.
* Download content on-demand when requested by clients to reduce disk space.
* Host content either locally or on S3.
* Perform docker/podman pull from a container distribution served by Pulp.
* Perform docker/podman push to the Pulp Registry.
* Support for registry token authentication.
* Curate container images by filtering what is mirrored from an external repository.
* Curate container images by creating repository versions with a specific set of images.
* Build an OCI format image from a Containerfile and make it available from the Pulp Registry.
* De-duplication of all saved content.

### Ansible

* Mirror a subset of roles on-premise.
* Mirror all of Galaxy’s roles on-premise.
* Store private Ansible roles on-premise.
* Install roles from pulp_ansible using the ansible-galaxy CLI.
* Version content and rollback if necessary.
* Support for the new multi-role content type from Galaxy.


### Debian

* Synchronize remote repository content and metadata locally.
* Upload your own content.
* Publish content to one or more repositories.

### Python

* Synchronize Python packages from PyPI locally.
* Upload your own Python packages.
* Publish and organize Python packages just like PyPI.
* Install Python packages using pip as published by Pulp.

### Ruby Gem

* Synchronize remote repository content and metadata locally.
* Upload your own content.
* Publish content to one or more repositories.

### Chef Cookbook

* Sync Cookbook content from a remote to local repository.
* Upload your own content.
* Publish and organize content.

### Maven

* Synchronize packages from a remote to local repository.
* Upload your own Maven content.
* Publish and organize packages.


## Pulp 3 Content Plugins Information

This table contains links to information and sources for all Pulp 3 content plugins. If a plugin is missing [contact us](https://www.redhat.com/mailman/listinfo/pulp-list).

| Pulp Plugin | Docs | Source | Tracker | Install with PyPI | Install with RPM |
|-------|--------|---------|--------|---------|-------- |--------- |
| Ansible plugin | <a href="https://docs.pulpproject.org/pulp_ansible/">Ansible plugin docs</a> | <a href="https://github.com/pulp/pulp_ansible">Ansible plugin source</a> | <a href="https://pulp.plan.io/projects/ansible_plugin?jump=welcome">Ansible plugin tracker</a> | <a href="https://pypi.org/project/pulp-ansible/">Yes</a> | No |
| Chef cookbook plugin | <a href="https://github.com/pulp/pulp_cookbook/blob/master/README.rst">Cookbook plugin docs</a> | <a href="https://github.com/pulp/pulp_cookbook">Cookbook plugin source</a> | <a href="https://github.com/pulp/pulp_cookbook/issues">Cookbook plugin tracker</a> | <a href="https://pypi.org/project/pulp-cookbook/">Yes</a> | No |
| Debian plugin | <a href="https://docs.pulpproject.org/pulp_deb/">DEB plugin docs</a> | <a href="https://github.com/pulp/pulp_deb/tree/master">DEB plugin source</a> | <a href="https://pulp.plan.io/projects/pulp_deb?jump=welcome">DEB plugin tracker</a> | <a href="https://pypi.org/project/pulp-deb/">Yes</a> | No |
| Container plugin | <a href="https://docs.pulpproject.org/pulp_container/">Container plugin docs</a> | <a href="https://github.com/pulp/pulp_container">Container plugin source</a> | <a href="https://pulp.plan.io/projects/pulp_container?jump=welcome">Container plugin tracker</a> | <a href="https://pypi.org/project/pulp-container/">Yes</a> | No |
| File | <a href="https://docs.pulpproject.org/pulp_file/">File plugin docs</a> | <a href="https://github.com/pulp/pulp_file">File plugin source</a> | <a href="https://pulp.plan.io/projects/pulp_file?jump=welcome">File plugin tracker</a> | <a href="https://pypi.org/project/pulp-file/">Yes<a/> | No |
| GalaxyNG plugin | <a href="https://github.com/ansible/galaxy_ng/blob/master/README.md">GalaxyNG plugin docs</a> | <a href="https://github.com/ansible/galaxy_ng">GalaxyNG plugin source</a> | <a href="https://github.com/ansible/galaxy_ng/issues">GalaxyNG tracker</a> | <a href="https://pypi.org/project/galaxy-ng/">Yes</a> | No |
| Gem plugin | <a href="https://github.com/pulp/pulp_gem/blob/master/README.rst">Gem plugin docs</a> | <a href="https://github.com/pulp/pulp_gem">Gem plugin source</a> | <a href="https://github.com/pulp/pulp_gem/issues">Gem plugin tracker</a> | <a href="https://pypi.org/project/pulp-gem/">Yes</a> | No |
| Maven plugin | <a href="https://github.com/pulp/pulp_maven/blob/master/README.rst">Maven plugin docs</a> | <a href="https://github.com/pulp/pulp_maven">Maven plugin source</a> | <a href="https://pulp.plan.io/projects/maven-plugin/">Maven plugin tracker</a> | <a href="https://pypi.org/project/pulp-maven/">Yes</a> | No |
| Python plugin | <a href="http://pulp-python.readthedocs.io/en/latest/">Python plugin docs</a> | <a href="https://github.com/pulp/pulp_python/">Python plugin source</a> | <a href="https://pulp.plan.io/projects/pulp_python?jump=welcome">Python plugin tracker</a> | <a href="https://pypi.org/project/pulp-python/">Yes</a> | No |
| RPM plugin | <a href="https://docs.pulpproject.org/pulp_rpm/">RPM plugin docs</a> | <a href="https://github.com/pulp/pulp_rpm/">RPM plugin source</a> | <a href="https://pulp.plan.io/projects/pulp_rpm?jump=welcome">RPM plugin tracker</a> | <a href="https://pypi.org/project/pulp-rpm/">Yes</a> | No |


## Pulp 2 Content Plugins Information

Pulp 2 will become End of Life in November 2021. For more information about migrating from Pulp 2 to Pulp 3, see the [migration plugin](https://pulp-2to3-migration.readthedocs.io/en/latest) documentation.


This table contains links to information and sources for all Pulp 2 content plugins. If a plugin is missing [contact us](https://www.redhat.com/mailman/listinfo/pulp-list).

| Pulp Plugin | Docs | Source | Tracker | Install with PyPI | Install with RPM |
|-------|--------|---------|--------|---------|-------- |--------- |
| RPM plugin | <a href="https://docs.pulpproject.org/en/2.21/plugins/index.html#rpm">RPM plugin docs</a> | <a href="https://github.com/pulp/pulp_rpm/tree/2-master">RPM plugin source</a> | <a href="https://pulp.plan.io/projects/pulp_rpm?jump=welcome">RPM plugin tracker</a> | No | Yes |
| Debian plugin | <a href="https://github.com/pulp/pulp_deb/blob/2-master/README.md">DEB plugin docs</a> | <a href="https://github.com/pulp/pulp_deb/tree/2-master">DEB plugin source</a> | <a href="https://pulp.plan.io/projects/pulp_deb?jump=welcome">DEB plugin tracker</a> | No | Yes |
| Docker plugin | <a href="https://docs.pulpproject.org/en/2.21/plugins/pulp_docker/user-guide/installation.html">Docker plugin docs</a> | <a href="https://github.com/pulp/pulp_docker">Docker plugin source</a> | <a href="https://pulp.plan.io/projects/pulp_docker">Docker plugin tracker</a> | No | Yes |
| Python plugin | <a href="https://docs.pulpproject.org/en/2.21/plugins/pulp_python/user-docs/getting_started.html">Python plugin docs</a> | <a href="https://github.com/pulp/pulp_python/tree/2-master">Python plugin source</a> | <a href="https://pulp.plan.io/projects/pulp_python?jump=welcome">Python plugin tracker</a> | No | Yes |
| Puppet plugin | <a href="https://docs.pulpproject.org/en/2.21/plugins/pulp_puppet/user-guide/installation.html">Puppet plugin docs</a> | <a href="https://github.com/pulp/pulp_puppet">Puppet plugin source</a> | <a href="https://pulp.plan.io/projects/pulp_puppet">Puppet plugin tracker</a> | No | Yes |

Are we missing a plugin? Let us know via the pulp-dev@redhat.com mailing list.
