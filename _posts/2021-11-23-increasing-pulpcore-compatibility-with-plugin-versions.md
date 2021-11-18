---
title: Increasing Pulpcore Compatibility with Plugin Versions
author: Brian Bouterse
---

## Tightly coupled plugins and pulpcore

One challenge users and plugin maintainers face is a close coupling between plugin releases and the
corresponding pulpcore versions. For example, pulp_rpm 3.15.z supports pulpcore 3.15 and 3.16, but
not 3.17. Users wanting to upgrade to pulpcore 3.17 need to upgrade pulp_rpm too which makes
upgrades harder. For plugin maintainers, e.g. pulp_rpm maintainers when pulpcore 3.17 is released
they must release a pulp_rpm even if it has no meaningful changes, it has to be done in order to
declare its safe to use with 3.17.

## Solution
At pulpcon 2021 we discussed various options and summarized them in [this discourse thread](https://discourse.pulpproject.org/t/what-to-do-to-reduce-unecessary-plugin-compatibility-releases/236/).
As a solution, we want to try including breaking changes to the Plugin API only in specific pulpcore
releases. We're going to try this idea out with pulpcore==3.20 and hold any breaking changes until
that release.

The next pulpcore release is 3.17, and this time instead of plugins declaring safety with 3.17 and
3.18, they can declare compatibility with 3.17, 3.18, and 3.19.

## Benefits

There are likely three benefits:

* Users can stay on plugin versions for longer and safely upgrade to newer pulpcore
versions when they come out.

* Plugin maintainers can avoid putting out so many releases simply to declare pulpcore
  compatibility.

* Users will enjoy more meaningful plugin releases with new releases containing new features, not
  just "compatibility declarations".

## Downsides

The main downside is that the plugin API cannot remove deprecated interfaces as quickly. This will
be a bit more painful for the pulpcore team.

## What happens after 3.20?

We'll pick another release, e.g. 3.25 perhaps, that will be the next pulpcore release that can
contain breaking Plugin API changes. We'll evaluate this more based on what we've learned between
now and 3.20.

## Feedback

Please send feedback to https://discourse.pulpproject.org/ You can also consider posting directly
on [this thread](https://discourse.pulpproject.org/t/what-to-do-to-reduce-unecessary-plugin-compatibility-releases/236).
Additionally, feedback can be [given via Matrix channels](https://pulpproject.org/help/#chat-to-us).
