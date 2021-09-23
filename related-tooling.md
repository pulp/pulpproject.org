---
title: Related Tooling
sidebar: home_sidebar
permalink: /related-tooling/
toc: false
---

Open Source tooling, such as for managing the state of Pulp and integrating it with other projects, that is either maintained by the commnunity or other projects, or that is developed by the Pulp Project but not yet in production.

## Foreman / Katello

[Foreman](https://theforeman.org/) is a complete lifecycle management tool for physical and virtual servers. [Katello](https://theforeman.org/plugins/katello/) brings the full power of content management alongside the provisioning and configuration capabilities of Foreman.

Foreman and Katello use Pulp for managing repositories of software packages. Katello provides a graphical Web UI for Pulp and integrates it with its workflows, but does not expose every underlying feature and content type of Pulp.

The following video gives a comprehensive overview of the workflows available with Katello:

<iframe width="560" height="315" src="https://www.youtube.com/embed/kWbfU_1zseU" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## pulp_rpm_repos

pulp_rpm_repos ([GitHub](https://github.com/juan-cabrera/pulp_rpm_repos), [Galaxy](https://galaxy.ansible.com/juan_cabrera/pulp_rpm_repos)): Use Pulp API client to manage repositories in a Pulp 3 server

This Ansible role interacts with a Pulp 3 server. It helps to create and manage rpm repositories.

## Python project to manage Pulp repos: pulp_operations

`pulp_operations` is a Python project by [Erik Whitesides](https://github.com/ewhitesides) that you can use to help manage many aspects of Pulp repositories.

You can find the `pulp_operations` project and instructions on [GitHub](https://github.com/ewhitesides/pulp_operations).

## pulp-operator: Kubernetes Operator for Pulp 3

[A Kubernetes Operator for Pulp 3](https://github.com/pulp/pulp-operator/), under active development (not production ready yet) by the Pulp team. The goal is to provide a scalable and robust cluster for Pulp 3. [Pre-built images are hosted on quay.io](https://quay.io/repository/pulp/pulp-operator).

Note that it utilizes a single container image from the pulpcore repo, to run 4 different types of service containers (like pulpcore-api & pulpcore-content.) currently manually built and [hosted on quay.io](https://quay.io/repository/pulp/pulp).

It is currently working towards [Phase 1 of the Kubernetes Operator Capability Model](https://blog.openshift.com/top-kubernetes-operators-advancing-across-the-operator-capability-model/) before being published on OperatorHub, including compatibility with more clusters.

See [latest slide deck](http://people.redhat.com/mdepaulo/presentations/Introduction%20to%20pulp-operator.pdf) for more info.


## docker-pulp: Pulp Docker images

[A set of Docker images for Pulp 3, with a Docker Compose.](https://github.com/fpytloun/docker-pulp)
