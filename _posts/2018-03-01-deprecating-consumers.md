---
title: Deprecating Consumers
author: David Davis
tags:
  - release
  - platform
  - 3.0
---
One of the core tenets in the [Unix Philosophy](https://en.wikipedia.org/wiki/Unix_philosophy) is
that programs should do one thing well. It's something we on the Pulp team believe in. In line with
this philosophy, there have been a couple features in Pulp 2 that are being removed in Pulp 3 to
better allow Pulp to focus on its primary goal of managing software packages.
[Nodes](https://pulpproject.org/2016/12/07/deprecating-nodes/) were one feature of Pulp 2 that got
cut. Pulp's support for Consumers is another feature that we're not planning on supporting in Pulp
3.

Pulp 2 provided the ability to manage Consumers of Pulp content. With Pulp 2, users could use Pulp
to install and track content on machines (called Consumers). However, there are number of tools
available today that can do a much better job automating the installation/updating/removal of
packages on Consumers. One such tool is Ansible which, for instance, has a [DNF
module](http://docs.ansible.com/ansible/latest/dnf_module.html) for managing packages with the dnf
package manager.

By focusing on our primary mission of managing software, we believe we can build a stronger product.
That said, we'd love to hear from you, our Pulp users, about how you use Pulp and how this change
will affect you. Feel free [to reach out to us](https://pulpproject.org/help/) via our mailing lists
or IRC to let us know.
