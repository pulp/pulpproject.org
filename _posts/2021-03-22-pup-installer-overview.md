---
title: Pulp Installer - Information & Overview
author: Melanie Corr, Mike DePaulo, Brian Bouterse
tags:
  - update
---

Over on the #pulp channel on IRC, a user asked about how the Pulp community came to the decision to move towards using an Ansible installer for Pulp 3 rather than RPM system packages that were used for Pulp 2 installations. He wanted to understand why Pulp took this direction so that he could explain the advantages of such an approach to the community of another project he was involved in.

Since our code and community is open, we thought it might be useful to post about the advantages of replacing system packages with a pulp-installer tool, so that if any other community is thinking of doing the same, they might benefit from the answer to this question.

If you’ve ever any questions or feedback about Pulp, feel free to write to `pulp-list@redhat.com`

## What is the Pulp installer?

The Pulp installer is a collection of Ansible roles that you can use to install or upgrade Pulp 3, as well as install additional content plugins that you need. The Pulp installer also changes the installation experience for Pulp users that just want to install Pulp on one node. The introduction of the Pulp installer was one of the many user-experience improvements when compared to Pulp 2.

## What motivated Pulp to change the primary installation method?

In planning Pulp 3, we looked at Pulp 2 and realized a few problems stemmed from using an RPM-based installation method:

* Pulp was installable only in the Red Hat ecosystem. It could not be cross-distribution. Users wanted to deploy Pulp on other operating systems.
* Pulp ended up being on the delivery path of large portions of our dependency tree. This was, practically speaking, taking a lot of Pulp developer time.
* Pulp had to repackage CVEs in the dependency tree of Pulp, for example, Django. This lowered the security of user environments. In many cases, we would only deliver these CVE-patched dependencies to the __latest__ one or two Pulp repositories, leaving older Pulpcore user environments totally exposed.
* We could not automate the upgrade process because applying migrations to the database was not a good fit for packaged RPMs.
* Configuring Pulp’s dependencies was mostly beyond our control via RPM methods. For example, the database. We didn’t own the database RPMs, yet we needed to apply configurations onto it.
* We could not handle clustered installations.
* We could not use the RPMs to the right components on different machines. If the RPMs declared a dependency on mongod for example it would get installed on every node in clustered installations. If we did not declare a dependency on mongod, single node installs would not even receive mongod automatically.
* Maintaining config files across the cluster was not practical when every RPM declares a configuration file and tries to apply changes to it over time.
* There was a lot of divergence between developer systems and user systems because user systems used the RPM packaging, but developer systems never did.

## What is the main difference for the user between Pulp installer and system package installation?

Previously, users installed packages, but then had to determine which commands to run afterwards, and which config files to modify and how, which was overall more complex.

Now, the Pulp installer automates these parts of the installation process that would otherwise have been left to the user to complete.

## How do I customize my Pulp deployment?

While the Pulp installer handles the automation of the installation, if you want to customize your deployment, you must determine which variables to provide before running the installer. However, once this is done, running the installer once completely performs the installation in an automated fashion.

Using variables makes the process much more repeatable. For example, if you install Pulp in your test environment, and want to repeat this installation in production, you can copy the Ansible variable file, adjust one or two variables for the environment, and re-run the installer.

For every Ansible role in the Pulp installer, there is a list of variables that you can use to customize the installation. Each Ansible role documents its own variables. You can also find this information in the [Pulp installer documentation](https://pulp-installer.readthedocs.io). For example, if you want to customize your firewall settings, you can do so using the `pulp_configure_firewall` variable in the [pulp_webserver](https://pulp-installer.readthedocs.io/en/latest/roles/pulp_webserver/#role-variables) role.

## What do you think of the Pulp installer?

Have you tried the installer? Have you customized your deployment using Ansbile variables? Let us know what you think at `pulp-list@redhat.com`
