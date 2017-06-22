---
title: 2017 Pulp QE Intern Update
author: Elijah DeLee
tags:
  - pulpqe
---

Virtualization, docker, automated tests and more! 
----------------

This summer I am interning with the QE team at Redhat for Pulp Project. To do
the work of re-creating bugs and writing tests, I will need to install various
versions and builds of Pulp. To enable me to do this repeatedly and without
endangering my local system or generating a difficult to duplicate state of the
application, this week I was trained on the various tools the Pulp QE team use
to automate their work flow. This included setting up virtual machines on my
local system using [libvirt](https://libvirt.org/), as well as scripting the
cloning of these VM's and using [Ansible](https://www.ansible.com/) to install
various builds of pulp on each VM. Additionally I got introduced to beaker, an
internal tool for provisioning machines, and was shown how to use a Jenkins job
the pulp team uses to install various versions of pulp.

Controlling the pulp-server remotely
----------------

Pulp can be installed in a distributed fashion, as well as be controlled
remotely by the pulp-admin client. I used pulp to both pull in existing RPM
repositories as well and create a new RPM repo with a few RPM files, and enable
a system to install the package via `dnf` from the repository hosted on the
pulp server.

Docker demo
----------------

Midway through the week I took a brief break from working on Pulp while one of
the Satellite6  (downstream project of
[Katello](https://www.theforeman.org/plugins/katello/)) QE team, came
over and led us through a demo of [docker](https://www.docker.com/) and how he
uses it to run an automated test suite against Satellite6 (which uses Pulp).

At one point I had ten containers running this test suite all hammering a
Satellite6 instance! Watching my system monitor was quite entertaining as the
many thousands of tests ran. Then I walked through a demo of how Satellite6
works from a user perspective. This gave me a better idea of what role Pulp
plays in Satellite6. Customizing the content provided to different machines or
groups of machines is a powerful tool for system administrators using
Satellite6, and Pulp is the workhorse behind this functionality!

Pulp-smash
----------------

Finally, I got into working with pulp-smash, which is the test suite for pulp.
After setting up a python virtual environment and installing the developer
requirements, I did some exploration of the project in
[ipython](https://ipython.org/). Armed with this knowledge and our different
pulp VM's, I wrote a test that ensured, if the pulp version being tested was of
a sufficient version, a command could be executed and that it returned with a
successful exit code. By the end of the day Friday I had one pull request
updating the documentation to Pulp, and another adding a test to Pulp-Smash. I
had a lot of fun and I'm looking forward to all I have to learn and the
opportunity to contribute to such an active open source project.
