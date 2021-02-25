---
title: Pulp Community Update (February 2021)
author: Melanie Corr
tags:
  - newsletter
---

_If you have any questions or updates you would like to share, we would be delighted to hear from you at `pulp-list@redhat.com`._

## FOSDEM & Devconf

The Pulp community had a sizeable presence at both FOSDEM and DevConf, giving talks and workshops, as well as welcoming questions from anyone in the community at our virtual booths. We created some video content for people new to Pulp and about updates in the Pulp community over the last year. If you miss attending conferences, and want a quick summary of the main headlines in the Pulp community since we last met, take a look at this video:

<iframe width="560" height="315" src="https://www.youtube.com/embed/Gcr2lslMny8" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

While DevConf was hosted using the hopin platform, FOSDEM was an entirely self-hosted experience that demonstrated the might of open source tooling. Matrix have an interesting [blog post](https://matrix.org/blog/2021/02/15/how-we-hosted-fosdem-2021-on-matrix) about hosting FOSDEM. We enjoyed our day in the Infra management devroom!

### Host Your Own Ansible Galaxy

In an on-premise environment, discovering and sharing Ansible content between different parts of your organization is a major problem. At FOSDEM, Pulp's Brian Bouterse delivered a talk on how the Galaxy_NG plugin can take the pain away from self-hosting Ansible Galaxy:

<iframe width="560" height="315" src="https://video.fosdem.org/2021/D.infra/hostyourownansiblegalaxy.mp4" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

If this video isn't loading for you, click [here](https://video.fosdem.org/2021/D.infra/hostyourownansiblegalaxy.mp4). The FOSDEM talks and servers are still processing all of the content and a refresh should load the video for you.

Brian also hosted a workshop at DevConf on the same topic. When that video becomes available, you will find a link to it on our new [Media](https://pulpproject.org/media/) page on the Pulp website.

### Registry Native Delivery of Software Content

With containers becoming an increasingly popular method for software content delivery, Pulp's Ina Panova talked about how Pulp can help you manage not only the different software components you use to build your container images, but also the entire container management workflow that includes hosting a local cache of a container registry, building OCI images from containerfiles, curating container content, and distributing containers throughout your organization:

<iframe width="560" height="315" src="https://video.fosdem.org/2021/D.infra/registrynativedeliverysoftwarecontentpulp3.mp4" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

If this video isn't loading for you, click [here](https://video.fosdem.org/2021/D.infra/registrynativedeliverysoftwarecontentpulp3.mp4). The FOSDEM talks and servers are still processing all of the content and a refresh should load the video for you.

For more information about Pulp Container, see the [Pulp Container documentation](https://docs.pulpproject.org/pulp_container/role-based-access-control.html).

### Don’t Get Stuck on Pulp 2!

Each month, we do our best to draw attention to the fact that [Pulp 2 is in maintenance mode](https://pulpproject.org/migrate-to-pulp-3/) and Pulp 3 is growing more powerful and robust with every passing week. If you're still unconvinced, let Pulp's Tanya Tereshchenko walk you through several reasons why it's a good idea to get off Pulp 2 as soon as possible, and how you can do that:

<iframe width="560" height="315" src="https://video.fosdem.org/2021/D.infra/dontgetstuckonpulp2.mp4" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

If this video isn't loading for you, click [here](https://video.fosdem.org/2021/D.infra/dontgetstuckonpulp2.mp4). The FOSDEM talks and servers are still processing all of the content and a refresh should load the video for you.

If you have any questions or concerns about migrating, please write to us at `pulp-list@redhat.com`.


### Host Your Own PyPI

At DevConf, Pavel Picka and Daniel Alley hosted a workshop that went through the main workflows involved in hosting your own PyPI using the Pulp Python plugin.
At the time of writing this newsletter, the video of this workshop has not been made available, but we have a new [Media](https://pulpproject.org/media/) page on the website, where you can check for the latest talks and if you'd like to submit a talk or workshop recording of your own, please write to `pulp-list@redhat.com`.   

If you want to read more about Pulp Python, check out the [Manage Your Python Content with the Pulp Python plugin](https://pulpproject.org/2021/01/13/pulp-python-3.0-release/) post on our blog from earlier this year.

## Pulp Squeezer Debian modules

Over the weekend at FOSDEM, and somewhat as part of a discussion we had at our virtual booth, there has been a release of Pulp Squeezer, now with added Debian modules!
You can read all about this release in our [Automating Pulp Debian workflows with Pulp Squeezer](https://pulpproject.org/2021/02/08/pulp-squeezer-0.0.7/) post.

## Pulp 3.10 release

In February, Pulpcore 3.10 was released. You can read more about the headline features in our [release announcement](https://pulpproject.org/2021/02/04/pulpcore-3.10-is-now-generally-available/).

## Pulp 3 CLI progress

Development of the Pulp 3 CLI is progressing rapidly. In the last month alone, there have been three beta releases. These releases include adding support for the new [Labels](https://docs.pulpproject.org/pulpcore/workflows/labels.html) feature from Pulpcore 3.10, easier handling of config files and support for SSL client certificates, as well as Pulp 2-to-3 migration support, multiple config support, a worker command, and better validation of task states when filtering.

For more information on the latest updates to the Pulp 3 CLI, check out the Pulp 3 CLI [changelog](https://github.com/pulp/pulp-cli/blob/develop/CHANGES.rst).

If you take the time to evaluate the current Pulp 3 CLI in its beta form, [we would love to hear from you](https://docs.google.com/forms/d/121M3w_dyDkSWSh8YI2EGVVW483CzXPYBV5-ecEtnJc4/edit), and send you some SWAG as a thank you.

## Pulp 2.3.0/2.3.1 Container release

With this release, every Pulp Container and Container Registry API call is covered by a default access policy. For the moment, RBAC functionality is in tech preview mode.

Shortly after the 2.3.0 release, 2.3.1 was released with an important bug fix that prevents pulp_container from crashing when running alongside other pulp plugins that override the default user authentication models.

For more information about RBAC in Pulp Container, check out the [role based access control](https://docs.pulpproject.org/pulp_container/role-based-access-control.html) section of the Pulp Container documentation.

## Pulp 2-to-3 Migration releases

In February, there were two releases of the Pulp 2-to-3 Migration plugin, which include performance improvements and bug fixes as a result of extensive testing:

With the 0.7.0 release, here are some highlights:
* performance improvements for simple migration plans, some parts run in multiple tasks in parallel
* improved reliability of the re-migration in case of any interruptions or problems at early stages
* bug fixes for RPM plugin migration, mostly related to modular or distribution tree content
* documentation moved to docs.pulpproject.org, see the link below
* improved test coverage

And for the 0.8.0 release:
 * added the ability to migrate additional Debian content types needed for structured publishing.
 * performance improvements for migration re-runs
 * provided a default configuration for `ALLOWED_CONTENT_CHECKSUMS` setting
 * various bug fixes for re-migration cases

For more information about the Pulp 2-to-3 Migration plugin, check out the [Pulp 2-to-3 Migration documentation](https://docs.pulpproject.org/pulp_2to3_migration/index.html).

## Pulp Ansible 0.7.0 release

The latest Ansible plugin released is as feature-packed as ever. Here are some of the highlights:


* Ansible export/import is now available as a tech preview feature
* Improve sync performance with no-op when possible. To disable the no-op optimization use the optimize=False option on the sync call.
* Efficient sync with unpaginated metadata endpoints if they are available.

New endpoints:

* a `v3/` endpoint returning publication time
* a `v3/collections/all/` endpoint returning all collections unpaginated
* a `v3/collection_versions/all/` endpoint returning all collections versions unpaginated

Modified/improved endpoints:
* Expose `MANIFEST.json` and `FILES.json` at `CollectionVersion` endpoint
* Adds the `requires_ansible` attribute to the Galaxy V3 `CollectionVersion` APIs. This documents the version of Ansible required to use the collection.
* Field `updated_at` from Galaxy v3 Collections endpoint using latest instead of highest version


## Interning with Pulp during a pandemic

In early 2020, Pulp's Gerrod Ubben internship plans were derailed by the pandemic. He took some time to write about his experiences working as a remote intern and working in both the Pulp Python and Python Bandersnatch communities over on [opensource.com](https://opensource.com/article/21/2/python-pulp-internship).

## Get Involved

### Meetings

**Open Floor** - every Tuesday and Friday 10:30 ET (either EST or EDT) in #pulp-meeting on Freenode (IRC). It’s a time for developer discussion, feedback, and decisions on code/issues/PRs/process relating to pulpcore or plugins. To participate, put a topic on the [agenda](https://hackmd.io/@pulp/triage/edit). Minutes will be sent to the `pulp-dev` mailing list.

**Bug Triage** - every Tuesday and Friday immediately after Open Floor in #pulp-meeting on Freenode (IRC). Come and participate in real-time. See the triage process for more details. You can also read through the triage archives.

### Make Sure You’re Registered On IRC

If you want to chat with us on any of the channels on Freenode IRC, please be aware that you must register your username with Freenode. If you're not getting any answers from us, it could be because we are not seeing your messages.
