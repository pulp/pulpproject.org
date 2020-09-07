---
title: Pulp Roadmap Update
author: Melanie Corr
tags:
  - planning
---

Over the coming months, the Pulp team will focus on the following areas. As with all plans, these are subject to change. Many of the items in this plan factor into [Katello](https://community.theforeman.org/t/new-katello-release-schedule-pulp-3-migration-update/19884?u=mcorr) and [Galaxy NG](https://github.com/ansible/galaxy_ng/blob/master/ROADMAP.rst) plans.  

Further updates will be announced on Pulp blog and mailing lists. If you have any questions or feedback, feel free to write to pulp-list@redhat.com

## Pulpcore

* Implementing Role Based Access Control (RBAC)
* Working towards making Pulp multi-user safe
* Working towards making Pulp FIPS-compliant
* Improving import/export functionality

## Pulp Installer

* Pulp in a single CentOS 8 container
* SELinux - installer integration & EL8 Policy Fixes
* Cluster installations
* Pulp operator

## Pulp 2-to-3 Migration Plugin

* Migrating in parallel when a simplified migration plan is selected
* Performance testing for Pulp Container and File migration
* Test writing

## Pulp File Plugin

* Implementing Role Based Access Control (RBAC)

## Pulp RPM Plugin

* Implementing Role Based Access Control (RBAC)
* Adding the ability to set and update `repo_gpgcheck` `gpg_check` options [#6926](https://pulp.plan.io/issues/6926)

## Pulp Ansible & Galaxy_NG Plugin

* Finalizing features to demo at Ansiblefest
* Further planning

## Pulp Container Plugin

* Implementing Role Based Access Control (RBAC)
* Finalizing OCI Image Builder
