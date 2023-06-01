---
title: Content Plugins
sidebar: home_sidebar
permalink: /content-plugins/
toc: true
---

## Content Plugins

![](/images/pulp-workflow-architecture-ha/pulp-overview.png)

As part of the Pulp installation, you must add a content plugin for each content type that you want
to manage. The following sections contain information about the available content plugins. If you do
not find the plugin for the content type you want to manage, consider 
[writing a plugin](https://docs.pulpproject.org/plugins/plugin-writer/index.html).

## Plugin Changes between Pulp 2 and Pulp 3

* The ISO content support found in the Pulp 2 RPM plugin is now provided by the File plugin.
* The Docker plugin in Pulp 2 has been replaced by the Container plugin.
* Currently, there is no Pulp 3 plugin for Puppet content.

## Pulp 3 CLI Coverage

The [Pulp 3 CLI](https://github.com/pulp/pulp-cli) is continuously being expanded.
The CLI is implemented by the plugin writers, so the workflows that are possible with the CLI vary
from plugin to plugin.
Please check the CLI directly to see the current workflow coverage for the desired plugin.

## Role Based Access Control Support

While the functionality is available in Pulpcore, each plugin must add it separately.
Please check whether the plugin of your interest has already added RBAC support.

## Cloud Storage

It is possible to configure Pulp to use 
[cloud storage](https://docs.pulpproject.org/pulpcore/installation/storage.html). However, plugins
can introduce changes that are incompatible with, for example, S3 requirements. A number of plugins
are regularly tested to ensure they remain compatible but the level of coverage might not cover our
entire matrix of plugins.

## Pulp 3 Content Plugin Features

Here are some highlights of each plugin's capabilities.

### RPM

* Create, sync and publish a repository with RPM Content including RPMs, Advisories, Modularity, and
 Comps.
* Create, sync and publish a repository with Unbreakable Linux Network (ULN) remotes to sync from
ULN servers.
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
* Support disconnected and air-gapped environments with pulp import/export facility for synced container repositories.

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

### OSTree

* Synchronize content from a remote OSTree repository and serve it via Pulp.
* Import new OSTree commits to an existing Pulp repository.
* Consume content imported to Pulp using the ostree utility.

#### Maven

* Use Pulp as a pull through cache for Maven content.

#### Ruby Gem

* Synchronize remote repository content and metadata locally.
* Upload your own content.
* Publish content to one or more repositories.

### Unmaintained plugins

The following plugins have been created and developed to some extent but have not been regularly maintained.
If you're interested in extending or maintaining the basic functionality of these plugins, [let us know](/help/#contribute-as-a-developer).

#### Chef Cookbook

* Sync Cookbook content from a remote to local repository.
* Upload your own content.
* Publish and organize content.


## Pulp 3 Content Plugins Information

This table contains links to information and sources for all Pulp 3 content plugins. If a plugin is missing [contact us](https://discourse.pulpproject.org/).

| Pulp Plugin | Docs | Source | Tracker | Install with PyPI | Install with RPM |
|-------|--------|---------|--------|---------|-------- |--------- |
| Ansible plugin | <a href="https://docs.pulpproject.org/pulp_ansible/">Ansible plugin docs</a> | <a href="https://github.com/pulp/pulp_ansible">Ansible plugin source</a> | <a href="https://github.com/pulp/pulp_ansible/issues">Ansible plugin tracker</a> | <a href="https://pypi.org/project/pulp-ansible/">Yes</a> | No |
| Chef cookbook plugin | <a href="https://github.com/pulp/pulp_cookbook/blob/master/README.rst">Cookbook plugin docs</a> | <a href="https://github.com/pulp/pulp_cookbook">Cookbook plugin source</a> | <a href="https://github.com/pulp/pulp_cookbook/issues">Cookbook plugin tracker</a> | <a href="https://pypi.org/project/pulp-cookbook/">Yes</a> | No |
| Debian plugin | <a href="https://docs.pulpproject.org/pulp_deb/">DEB plugin docs</a> | <a href="https://github.com/pulp/pulp_deb/tree/master">DEB plugin source</a> | <a href="https://github.com/pulp/pulp_deb/issues">DEB plugin tracker</a> | <a href="https://pypi.org/project/pulp-deb/">Yes</a> | No |
| Container plugin | <a href="https://docs.pulpproject.org/pulp_container/">Container plugin docs</a> | <a href="https://github.com/pulp/pulp_container">Container plugin source</a> | <a href="https://github.com/pulp/pulp_container/issues">Container plugin tracker</a> | <a href="https://pypi.org/project/pulp-container/">Yes</a> | No |
| File | <a href="https://docs.pulpproject.org/pulp_file/">File plugin docs</a> | <a href="https://github.com/pulp/pulp_file">File plugin source</a> | <a href="https://github.com/pulp/pulp_file/issues">File plugin tracker</a> | <a href="https://pypi.org/project/pulp-file/">Yes<a/> | No |
| GalaxyNG plugin | <a href="https://github.com/ansible/galaxy_ng/blob/master/README.md">GalaxyNG plugin docs</a> | <a href="https://github.com/ansible/galaxy_ng">GalaxyNG plugin source</a> | <a href="https://github.com/ansible/galaxy_ng/issues">GalaxyNG tracker</a> | <a href="https://pypi.org/project/galaxy-ng/">Yes</a> | No |
| Gem plugin | <a href="https://github.com/pulp/pulp_gem/blob/master/README.rst">Gem plugin docs</a> | <a href="https://github.com/pulp/pulp_gem">Gem plugin source</a> | <a href="https://github.com/pulp/pulp_gem/issues">Gem plugin tracker</a> | <a href="https://pypi.org/project/pulp-gem/">Yes</a> | No |
| Maven plugin | <a href="https://github.com/pulp/pulp_maven/blob/master/README.rst">Maven plugin docs</a> | <a href="https://github.com/pulp/pulp_maven">Maven plugin source</a> | <a href="https://github.com/pulp/pulp_maven/issues">Maven plugin tracker</a> | <a href="https://pypi.org/project/pulp-maven/">Yes</a> | No |
| OSTree plugin | <a href="https://docs.pulpproject.org/pulp_ostree/">OSTree plugin docs</a> | <a href="https://github.com/pulp/pulp_ostree/">OSTree plugin source</a> | <a href="https://github.com/pulp/pulp_ostree/issues">OSTree plugin tracker</a> | <a href="https://pypi.org/project/pulp-ostree/">Yes</a> | No |
| Python plugin | <a href="https://docs.pulpproject.org/pulp_python/">Python plugin docs</a> | <a href="https://github.com/pulp/pulp_python/">Python plugin source</a> | <a href="https://github.com/pulp/pulp_python/issues">Python plugin tracker</a> | <a href="https://pypi.org/project/pulp-python/">Yes</a> | No |
| RPM plugin | <a href="https://docs.pulpproject.org/pulp_rpm/">RPM plugin docs</a> | <a href="https://github.com/pulp/pulp_rpm/">RPM plugin source</a> | <a href="https://github.com/pulp/pulp_rpm/issues">RPM plugin tracker</a> | <a href="https://pypi.org/project/pulp-rpm/">Yes</a> | No |


## Pulp 2 Content Plugins Information

Pulp 2 is EOL. For more information about migrating from Pulp 2 to Pulp 3, see the [migration plugin](https://docs.pulpproject.org/pulp_2to3_migration/) documentation.
