---
title: New User? Start Here
sidebar: home_sidebar
permalink: /installation-introduction/
toc: false
---

If you're a new user and are unsure where to begin, this page outlines the different options available, as well as limitations and requirements for those options.
Each sections has links to corresponding documentation for each installation method.
If you've any questions or feedback about installing Pulp, we would love to hear from you on our [Pulp Community Discourse](https://discourse.pulpproject.org/).

## Want to evaluate Pulp?

The quickest way to evaluate Pulp is by using Pulp in One Container.
This image contains all the services that you need to run Pulp and is perfect for an initial evaluation.
This container is not production-ready and cannot scale, for example, to provide for high availability scenarios.

For installation instructions, see [Pulp in One Container](/pulp-in-one-container/).


## What is the quickest way to set up a production-ready Pulp environment?

The `pulp_installer` is a set of Ansible roles that automates the installation of Pulp 3.
This is the preferred method of installation and removes a lot of the complexity of the installation process.
When updates to Pulpcore are released, the newest Pulp version is available through the `pulp_installer`.
The `pulp_installer` provides options for high availability scenarios.
For more information, see [Pulp & High Availability](/pulp-ha/).

For installation instructions, see [Pulp installer](https://docs.pulpproject.org/pulp_installer/).


## Is there a manual installation option available?

You can manually install Pulp using `pip` or from [source](https://github.com/pulp/pulpcore).

For `pip` installation instructions, see [PyPI installation](https://docs.pulpproject.org/pulpcore/en/master/nightly/installation/instructions.html#pypi-installation).


## Is there a Kubernetes operator deployment option?

Pulp operator is an [Ansible operator](https://www.ansible.com/blog/ansible-operator) that is under active development.
Pulp operator endeavours to provide a scalable and robust cluster for Pulp 3.
Pulp can be installed from [OperatorHub](https://operatorhub.io/operator/pulp-operator).
If you're interested in providing feedback or contributing to making this better, see the [Pulp operator repo](https://github.com/pulp/pulp-operator) on GitHub.
For more information about using Pulp operator, see [Pulp on Openshift](https://pulpproject.org/insta-demo/)

## Is there a podman compose deployment option?

Based on community feedback from the survey and PulpCon 2021, we have reused the Pulp operator images to create a podman compose option for deploying Pulp.
If you're familiar with podman compose, you can customize the configuration to suit your deployment needs and to deploy at scale.
If you do use it, [let us know your feedback](https://discourse.pulpproject.org/).
For more information see our [podman compose page](/podman-compose/).

## Is there a docker compose deployment option?

[Kong](https://konghq.com) maintain a Docker compose option for deploying Pulp.
They have diligently documented known issues and the Pulpcore version in the compose is _generally_ one release behind the latest Pulpcore release.
For more information, see [Kong's Github repo](https://github.com/Kong/docker-pulp).

## Do you need something else?

If you are blocked and don't find an option that you need, please post to our [Pulp Community Discourse](https://discourse.pulpproject.org) and let us know what problem you encountered or what scenario you're missing. Feel free to introduce yourself, what you're trying to achieve, where you ran into problems or didn't understand something. We're always happy to hear how people are using Pulp!

You can also find us on [**pulp** on Matrix](https://matrix.to/#/!HWvLQmBGVPfJfTQBAu:matrix.org) for user support.

## Project history

Moving from Pulp 2 to Pulp 3, the Pulp community decided to stop with RPM-based installations in favour of the Ansible installer.
If you're interested in understanding the motivations for such a change, see [What motivated Pulp to change the primary installation method?](https://pulpproject.org/2021/03/22/pup-installer-overview/#what-motivated-pulp-to-change-the-primary-installation-method).
