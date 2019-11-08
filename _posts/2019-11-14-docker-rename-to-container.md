---
title: Pulp Docker plugin will be renamed to Pulp Container plugin in Pulp3
author: Ina Panova
tags:
  - docker
  - container
---

Version 1.0 of the pulp_container plugin will be released to coincide with Pulp 3 GA..

## Why rename pulp_docker to pulp_container in Pulp3?

* pulp_docker plugin implies management of Docker format images only. This plugin naming convention will remain in Pulp 2.
* pulp_container won't be tied to Docker format images, this will enable us to add support for the OCI format in the future.

## What does this mean for me?

Pulp 2.y users who manage Docker content should be able to migrate their production
systems to Pulp 3.0 with the Pulp Container plugin installed.
Pulp 3 Container plugin has full feature parity with Pulp 2 Docker plugin.

Pulp 3 users will need to uninstall the pulp_docker plugin and install the pulp_container one.
Don't be alaramed by the low version number, this plugin release is stable.

## Where to report issues?

To report bugs or request features use the issue tracker for pulp_container:

* pulp_container - [https://pulp.plan.io/projects/pulp_container/issues/new](
      https://pulp.plan.io/projects/pulp_container/issues/new)

If you are facing issues in Pulp 2 use pulp_docker tracker:

* pulp_docker - [https://pulp.plan.io/projects/pulp_docker/issues/new](
      https://pulp.plan.io/projects/pulp_docker/issues/new)

## Where is the source code?

The source code for Pulp Container plugin can be found on GitHub:

[https://github.com/pulp/pulp_container](https://github.com/pulp/pulp_container)
