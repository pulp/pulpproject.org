---
title: Encouraging New Contributors
author: Brian Bouterse
---
As an open source project, Pulp wants to attract as many contributors as possible. Encouraging and
supporting new contributors is a key strategy to achieve that goal. This post shares some of the
approaches and activities the Pulp community is using to grow the contributor base of Pulp.

* **Have an easy way to get started**. Having a developer environment like Vagrant is a great way to
  reduce the amount of time and complexity it takes to go from nothing to a functional developer
  setup. The [Pulp developer environment](http://docs.pulpproject.org/dev-guide/contributing/dev_setup.html#vagrant)
  is an easy way to get a complete and highly featured developer environment. We also have a
  [Vagrant video](https://youtu.be/0LDoNPIZyzk) which talks about how to setup this environment as
  well.

* **Support developers the way they want to develop**. Not everyone develops the same way, and if a
  new contributor prefers to do things another way, we don't want to convince them otherwise without
  a good reason. For example, a new contributor preferred to use Docker instead of VMs with
  libvirtd. At the time, the Pulp developer environment only had support for libvirtd. By exploring
  another way of doing things, the new contributor's first contribution was adding Docker support to
  the developer environment. Now Pulp's dev environment has both libvirtd and Docker support and the
  contributor is able to develop in a way that aligns with their preferences.

* **Recognize and thank new contributors.** Every first time contribution made to Pulp should
  be recognized with a tweet from [@pulpproj](https://twitter.com/pulpproj) thanking them for their
  first contribution. We also send a Pulp tshirt and stickers to them if they want. Additionally, we
  highlight community contributions in the community update portion of the sprint demos.

* **Be responsive**. From questions via the developer mailing list, to newly opened Pull Requests
  (PRs), we want to engage new contributors who take their first step without any delay. We could
  improve some in this area, but ideally we would have comments and review on any new PR within a
  few hours and no more than 24 hours. In practice, I've observed that delays responding to a
  question or new PR significantly increase the likelihood that PR will not get finished or merged.

* **Recognize different kinds of contributions**. Not all contributions are code. For example,
  documentation improvements are very valuable and can be contributed by users. This is a great way
  to have non-programmers contribute.

* **Don't let contributor licenses get in the way.** A few years ago formal, contract-like
  Contributor License Agreements (CLAs) were becoming very common. Pulp does not require a CLA. By
  not requiring one, friction during the contribution process is reduced.

Have more ideas or feedback for us? We'd love to hear from you by tweeting at [@pulpproj account](https://twitter.com/pulpproj).