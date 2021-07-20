---
title: New User? Start Here
sidebar: home_sidebar
permalink: /installation-introduction/
toc: false
---

If you're a new user and are unsure where to begin, here are the available options.

## Want to evaluate Pulp?

The quickest way to evaluate Pulp is by using Pulp in One Container. This image contains all the services that you need to evaluate Pulp and is perfect for an initial evaluation. Eventually, this containerized version of Pulp will be enhanced and made available for production environments, but it's not quite there yet.

For installation instructions, see [Pulp in One Container](/pulp-in-one-container/).


## What is the quickest way to set up a production-ready Pulp environment?

The `pulp_installer` is a set of Ansible roles that automates the installation of Pulp 3. This is the preferred method of installation and removes a lot of the complexity of the installation process.

For installation instructions, see [Pulp installer](https://docs.pulpproject.org/pulp_installer/).


## Is there a manual installation option available?

You can manually install Pulp using `pip` or from [source](https://github.com/pulp/pulpcore).

For `pip` installation instructions, see [PyPI installation](https://docs.pulpproject.org/pulpcore/en/master/nightly/installation/instructions.html#pypi-installation).


## Is there a Kubernetes operator deployment option?

There is a [proof of concept](https://raw.githubusercontent.com/pulp/pulp-operator/master/insta-demo/pulp-insta-demo.sh) script that deploys installs K3s Lightweight Kubernetes and then deploys the Pulp operator. This is not production ready. If you're interested in providing feedback or contributing to making this better, see the [Pulp operator repo](https://github.com/pulp/pulp-operator) on GitHub.

## Do you need something else?

If you are blocked and don't find an option that you need, please write to `pulp-list@redhat.com` and let us know what problem you encountered or what scenario you're missing.
