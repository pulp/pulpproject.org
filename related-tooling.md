---
title: Related Tooling
sidebar: home_sidebar
permalink: /related-tooling/
toc: false
---

Open Source tooling, such as for managing the state of Pulp and integrating it with other projects, that is either maintained by the commnunity or other projects, or that is developed by the Pulp Project but not yet in production.

## Foreman / Katello

[Foreman](https://theforeman.org/) is a complete lifecycle management tool for physical and virtual servers. [Katello](https://theforeman.org/plugins/katello/) brings the full power of content management alongside the provisioning and configuration capabilities of Foreman.

Foreman and Katello use Pulp for managing repositories of software packages. It provides a graphical WebUI for Pulp and integrates it with its workflows, but does not expose every underlying feature and content type of Pulp.

Foreman and Katello are currently in the incremental [process of migrating from Pulp 2 to Pulp 3.](https://community.theforeman.org/t/update-on-katello-pulp3-integration-and-upcoming-roadmap/15899)

## Ansible Modules for Pulp aka Squeezer

[This project](https://github.com/mdellweg/ansible_modules_pulp) provides Ansible modules (currently only for pulp-file) to control a Pulp 3 server in a descriptive way. This is neither to be confused with the [Pulp 3 Ansible Installer pulp_installer](https://github.com/pulp/pulp_installer) (a collection of Ansible roles to install Pulp), nor [pulp_ansible](https://github.com/pulp/pulp_ansible) (a Pulp content plugin to manage Ansible content in Pulp.)

## pulp_rpm_repos

pulp_rpm_repos ([GitHub](https://github.com/juan-cabrera/pulp_rpm_repos), [Galaxy](https://galaxy.ansible.com/juan_cabrera/pulp_rpm_repos)): Use Pulp API client to manage repositories in a Pulp 3 server

This Ansible role interacts with a Pulp 3 server. It helps to create and manage rpm repositories.

## pulp-operator: Kubernetes Operator for Pulp 3

[A Kubernetes Operator for Pulp 3](https://github.com/pulp/pulp-operator/), under active development (not production ready yet) by the Pulp team. The goal is to provide a scalable and robust cluster for Pulp 3. [Pre-built images are hosted on quay.io](https://quay.io/repository/pulp/pulp-operator).

Note that it utilizes a single container image from the pulpcore repo, to run 4 different types of service containers (like pulpcore-api & pulpcore-content.) currently manually built and [hosted on quay.io](https://quay.io/repository/pulp/pulp).

It is currently working towards [Phase 1 of the Kubernetes Operator Capability Model](https://blog.openshift.com/top-kubernetes-operators-advancing-across-the-operator-capability-model/) before being published on OperatorHub, including compatibility with more clusters.

See [latest slide deck](http://people.redhat.com/mdepaulo/presentations/Introduction%20to%20pulp-operator.pdf) for more info.

## docker-pulp: Pulp Docker images

[A set of Docker images for Pulp 3, with a Docker Compose.](https://github.com/fpytloun/docker-pulp)
