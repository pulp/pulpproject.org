---
title: Pulp Community Update (June 2021)
author: Melanie Corr
tags:
  - release
---

This is a summary of the Pulp community goings-on  over the last few weeks!

## Pulp Community Survey

This year, we decided to pare the survey down to seven simple open questions. We've been delighted with the responses so far. We don't collect usage data on the project, so the more information you share with us, the more we can factor your scenarios and considerations into our plans. Given the size of the Pulp project, your feedback can have a very real impact on the future of Pulp, so please take the time to [complete the short survey](https://forms.gle/C3QwT9SVncXETipu9).

## Pulpcore 3.14 has been released!

The major feature of this release was the move to the new tasking system. In the lead-up to this release, Matthias Dellweg and Brian Bouterse hosted an information session in the form of a YouTube livestream, you can rewatch the event here:

<iframe width="560" height="315" src="https://www.youtube.com/embed/YWKw4RYluPM" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

For a look at the main headline features of the release, you can check out the [release announcement blog](https://pulpproject.org/2021/07/01/pulpcore-3.14-release-announcement/).

For a comprehensive list of all bugfixes, deprecations, removals, and enhancements, see the [Pulpcore changelog](https://docs.pulpproject.org/pulpcore/changes.html#id1).

The following plugins are compatible with Pulpcore 3.14:

* pulp_file 1.8.1
* pulp_rpm 3.13.2
* pulp_container 2.7.0
* pulp_ansible 0.8.0
* pulp_certguard 1.4.0
* pulp_2to3_migration 0.12.0


## Pulp Container 2.7.0

With this update, it's now possible for users to update their container push repositories.

As well as this, there was a number of bug fixes around RBAC functionality.

For a full list of updates, check out the [changelog](https://docs.pulpproject.org/pulp_container/en/2.7.0/changes.html).

## Pulp Certguard 1.4.0

As well as compatibility with 3.14, this release has extended the CertGuard.ca_certificate to accept a cert-bundle in addition to a single cert.

For more information, see the [changelog](https://docs.pulpproject.org/pulp_certguard/en/1.4.0/changes.html#id1)

## Pulp Ansible Tests

Pulp's Brian Bouterse hosted a session on adding tests for any additions to the Pulp Ansible plugin. You can rewatch on our YouTube channel:

<iframe width="560" height="315" src="https://www.youtube.com/embed/gqxqvQ5dKlY" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Pulp 3 session for Katello users

At the Foreman Birthday Party on July 1, Matthias Dellweg gave a presentation entitled **Pulp 3 introduction for Katello users - exploring the backend and tracing issues**. This session was the most popular event at the Foreman birthday party probably because the Foreman community are either in the midst of migrating from Pulp 2 to 3, or are now using Pulp 3. If you use Pulp through Katello, this session might be of interest to you.

<iframe width="560" height="315" src="https://www.youtube.com/embed/k7YOvCZmX0U?start=3716" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## GitHub Discussions

Over the last few weeks, we have been trialing moving away from mailing lists and towards a more modern and accessible forum for discussing issues related to the Pulp community. For the last while, we have tried [GitHub Discussions](https://github.com/pulp/community/discussions/).

Here are some conversations of note going on over the last few weeks:

* [Dropping support for Python 3.6 and Django 2](https://github.com/pulp/community/discussions/3)
* [Making pulp logging more configurable](https://github.com/pulp/community/discussions/20)
* [Optimizing the database fields key generation](https://github.com/pulp/community/discussions/47)


We've asked for opinions on using GitHub Discussions and heard that Pulp users might struggle to keep up to date because GitHub notifications are quite noisy and filtering notification emails can be hard. We're looking into other trial options, like Discourse. We will update our website and blog this happens! We would love to hear what you think. If you have any opinions, feedback or suggestions, let us know!
