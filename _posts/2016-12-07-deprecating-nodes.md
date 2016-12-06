---
title: Deprecating Nodes
author: David Davis
tags:
  - release
  - platform
  - 3.0
---

Nodes were introduced in Pulp in 2.1.0 and provided a way to keep multiple Pulp
servers synchronized. However, Nodes uses separate codepaths from the normal
sync+publish codepaths which creates a few problems:

* Nodes is less tested than sync+publish. This greatly reduces the user-to-user
  usage and testing benefit.
* Fixing bugs in sync+publish does not fix bugs in Nodes and vice-versa
* Nodes has a good amount of bugs already reported that are still unfixed

Given that much of the functionality of Nodes can be achieved by running Pulp
with sync+publish workflow, the core team is removing Nodes in the next major
release, Pulp 3.0. With Pulp 2.12, Nodes will be officially deprecated.

In this blog post, I want to outline how users can sync content across Pulp
servers without using Nodes and how tools like cron and systemd can be used to
keep content updated across Pulp servers. I am going to illustrate a two-server
example in which I'll call one Pulp server the parent and the other the child.
However, this distinction is arbitrary as each server is a standard Pulp server
installation without Nodes packages installed. The child Pulp server is called
that because it is the one we want to mirror content to.

Syncing Content to a Child Server
---------------------------------

Suppose I have an rpm repo called 'centos-base' on the parent Pulp server.
First, I would want to publish that repository to make it available for the
child Pulp server to sync:

    pulp-admin rpm repo publish run --repo-id centos-base

This repository should then be available via http or https. You can check its
relative path via `pulp-admin rpm repo list --repo-id centos-base --details`.
In my case, it's at "https://example.com/pulp/repos/centos-base". Now on the
child node, I can create and sync the repository:

    pulp-admin rpm repo create --repo-id centos-base --feed https://example.com/pulp/repos/centos-base
    pulp-admin rpm repo sync run --repo-id centos-base

Now the 'centos-base' repository from the parent Pulp server has been mirrored
onto a child server.


Automatically Updating Content
------------------------------

By using cron or a systemd timer, we can keep content on the child Pulp server
up-to-date automatically. To make content available, you will need to call
publish on the parent repository. This can be done automatically after each
sync via the "auto-publish" feature, or periodically with a crontab entry such
as:

    @hourly pulp-admin rpm repo publish run --repo-id centos-base

On on the child server, we could then sync our 'centos-base' repository hourly
with:

    @hourly pulp-admin rpm repo sync run --repo-id centos-base

It's worth noting that you'll want to ensure that pulp-admin is pointing to
the correct URL on each server. One option would be to edit the crontab for
each server and have pulp-admin configured to point to Pulp via localhost.

Also, by default, a pulp-admin certificate only lasts for 7 days before you
must login again. To extend this period, configure `user_cert_expiration` in
server.conf.
