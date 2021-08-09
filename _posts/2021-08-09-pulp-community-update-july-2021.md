---
title: Pulp Community Update (July 2021)
author: Melanie Corr
tags:
  - newsletter
---

## Pulp Community Survey 2021

Thank you so much for your responses to the Pulp Community Survey.

Based on trends among survey comments, I have already opened several documentation-related bugs.

I have started a [discussion](https://github.com/pulp/community/discussions/66) about how we can provide better _Getting Started_ information about Pulp's Architecture.

We have some work to do to make Pulp more accessible and will use your feedback to guide us as much as possible.

In the coming weeks, I will publish a summary of what we have learned from your responses on our blog. Just because the survey is closed does not mean that you cannot send us feedback. It means a lot to us when people get in touch and tell us about their experiences with Pulp. The more we know, the better decisions we can make! [Get in touch!](https://pulpproject.org/help/#chat-to-us)

## Pulpcore 3.14 and High Availability

With the release of Pulpcore 3.14 last month brought new high availability capabilities.

In this video, Brian explains the changes to the Pulp architecture, including the new tasking system that has been introduced in Pulpcore 3.14.

Brian gave a presentation on the high availability options that became available last month.

Brian talks through the new architecture and the options that you can use to build out your HA infrastructure.

Watch the full talk here:

<iframe width="560" height="315" src="https://www.youtube.com/embed/vjRvCuo7p4I?start=82" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


## Plugin releases

Here's a roundup of the plugin releases since last time.


### Pulp Container 2.8.0

Exciting times for the Pulp Container plugin!

With the completion of [#6636](https://pulp.plan.io/issues/6636), the plugin now has support for native importing and exporting to facilitate disconnected and air-gapped environment scenarios for synced container repositories.

This release also prepares the Pulp Container plugin for a feature that will be available with Pulpcore 3.15: once you upgrade to that release, you can reclaim disk space for blobs and manifests [#9169](https://pulp.plan.io/issues/9169).

This release is compatible with Pulpcore 3.14 and the forthcoming Pulpcore 3.15.

For more information, see the [release annoucement](https://github.com/pulp/community/discussions/81).

### Pulp Certguard 1.5.0

This release dropped support for Python 3.6 and 3.7. pulp-certguard now supports Python 3.8 and higher. It is compatible with Pulpcore 3.10 through to 3.15.

For more information, see the [release announcement](https://github.com/pulp/community/discussions/79)

### Pulp Debian 2.14.1

The release is compatible with pulpcore 3.14 and the future 3.15 release.

2.14.1 contains two important bug fixes:
* Added a missing **Size** field in publications
* Fixed an `arch=all` package indices that were not syncing when filtering by architecture

The 2.14.0 release initially dropped support for Python 3.6 and 3.7, but this caused problems that were then corrected in the 2.14.1 release with Python 3.6 and 3.7 re-enabled. Pulp Debian plugin now also supports versions Python 3.8 and higher.

For more information, see the [release announcement](https://github.com/pulp/community/discussions/71).

## Pulp Ansible 0.9.0

This release renames bindings to be compatible with Pulpcore 3.14 and higher. For more information, see the [release announcement](https://github.com/pulp/community/discussions/68).
