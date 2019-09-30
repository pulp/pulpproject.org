---
title: Pulp 3.0 Release Timeline
author: Brian Bouterse
tags:
  - 3.0
---

Pulp3 has been receiving lots of testing from users and various other communities which use Pulp2.
We want to share a plan to put Pulp3 on a “time-based release with blockers”.

## When will Pulp 3.0 be Generally Available?

* Final Development freeze  --   Nov 12th 2019
* Final GA release       --   Dec 3rd 2019

On Nov 12th, if all blockers are completed, we will branch the 3.0 release and publish it as the
final Release Candidate. From Nov 12th to Dec 3rd, only bugfixes and documentation changes will be
merged to the 3.0 branch. On Dec 3rd, Pulp 3.0 will be Generally Available.

If not all blockers are completed, we will publish a delayed schedule including a new GA release
date on Nov 12th based on remaining work.


## What is Included?

Plugins:
* `pulp_container` at 4.0.0  (`pulp_docker` will be renamed `pulp_container`)
* `pulp_rpm` at 3.0.0
* `pulp_file` at 0.1.0

Core components:
* `pulpcore` at 3.0.0 (the user facing API)
* `pulpcore-plugin` at 0.1.0 (the plugin API)

Migration tooling:
* Container content migration
* File content migration


## What are the Blockers?
`pulpcore`, `pulpcore-plugin`, and `pulp_file` blockers are tracked in this Redmine query:
[https://pulp.plan.io/issues?query_id=77](https://pulp.plan.io/issues?query_id=77).

`pulp_rpm` blocker query is here: [https://pulp.plan.io/projects/pulp_rpm/issues?query_id=139](
https://pulp.plan.io/projects/pulp_rpm/issues?query_id=139)

`pulp_docker` blockers query is pending.


## What does this mean for me?

Pulp 2.y users who manage Docker, iso, or file content should be able to migrate their production
systems to Pulp 3.0. RPM users can begin using Pulp3, but cannot migrate their data from their Pulp
2.y systems yet.


## Getting Help

The pulp-list@redhat.com mailing list is the best place for questions:
[https://www.redhat.com/mailman/listinfo/pulp-list](
https://www.redhat.com/mailman/listinfo/pulp-list)

To report bugs or request features use the issue tracker for the project you are interested in:

* pulp_rpm - [https://pulp.plan.io/projects/pulp_rpm/issues/new](
      https://pulp.plan.io/projects/pulp_rpm/issues/new)
* pulp_docker - [https://pulp.plan.io/projects/pulp_docker/issues/new](
      https://pulp.plan.io/projects/pulp_docker/issues/new)
* pulp_file - [https://pulp.plan.io/projects/pulp_file/issues/new](
      https://pulp.plan.io/projects/pulp_file/issues/new)
* pulpcore and pulpcore-plugin - [https://pulp.plan.io/issues/new](https://pulp.plan.io/issues/new)


## Stay Connected

Follow us on [twitter at @pulpproj](https://twitter.com/pulpproj) to receive updates on the “Road to
GA” progress and blockers. We’ll advertise this both on the pulp-list mailing list and twitter.
