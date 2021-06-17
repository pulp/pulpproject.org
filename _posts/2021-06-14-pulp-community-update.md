---
title: Pulp Community Update (May/June 2021)
author: Melanie Corr
tags:
  - newsletter
---

## 7 Questions for you!

We know your time is precious, so this year we thinned out our survey to comprise of only seven questions. The seventh question is just an open box, where we hope you can tell us anything that you might think is useful to help us improve Pulp.

[Take the survey now](https://forms.gle/r7qrbgE8RsxjdmCT8)!

## Pulp is Leaving Freenode

This probably isn't the first time you're hearing that many FOSS communities have left Freenode IRC and moved elsewhere. Pulp is no different. We now consider Matrix our Primary channel for communication. If you'd like to hang out and chat with the Pulp community, you can do so on the following Matrix channels:

If you want to use IRC, you can find corresponding rooms on Libera.Chat that are bridged to our new Matrix rooms.

You can find an up-to-date list of our channels and their purpose on our [Chat to us](/help/#chat-to-us) page.

## GitHub Discussions

After a recent Pulp Open Floor meeting, we decided to test the possibility of retiring our mailing lists. User feedback has suggested that the barrier of entry is high (you need to join the list to ask one question) and the mode of communication is outdated. We want to remove as many barriers of entry for users and contributors as possible. For these reasons, we are centralizing all of our discussions and meeting notes on a [Community Github Discussions](https://github.com/pulp/community/discussions). We would love to know what you think about it! If it works out, we might decommission our mailing lists.

## Move to GitHub Issues?

In a further effort to incorporate feedback from the Pulp community about our contribution process, we plan to migrate from our Pulp Redmine to GitHub Issues. We hope that with this move, we will reduce some of the overhead of contributing to Pulp. If you'd like to join in the discussion and share your experience, see [Github Issues migration meeting](https://github.com/pulp/community/discussions/9).

## Installing Pulp on Openshift

Fabricio has shared an end-to-end demo of how you can set up and deploy Pulp on Openshift. You can also find corresponding documentation for this process [here](https://pulp-operator.readthedocs.io/en/latest/). If you're interested to see this in action, you can watch here:

<iframe width="560" height="315" src="https://www.youtube.com/embed/quUdQ1j56I4" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

If you've any questions or feedback, [start a discussion](https://github.com/pulp/community/discussions).

## 5 reasons to host your container registry with Pulp

If you're looking for a better way to manage your containers, check out this [opensource.com article](https://opensource.com/article/21/5/container-management-pulp) about how Pulp can help you take control of your container management!

## Latest Releases

### Pulpcore 3.13

3.13 contained several bugfixes and the ability to control how many repo versions are retained. You can watch a demo here, and read all about the [release announcement](https://pulpproject.org/2021/05/24/pulpcore-3.13-is-generally-available/).

<a href="https://asciinema.org/a/412393" target="_blank"><img src="https://asciinema.org/a/412393.svg" /></a>

The Pulpcore 3.14 release is scheduled for the end of June. You can find updates on releaste dates and progress [here](https://github.com/pulp/community/discussions/22).

### Pulp RPM 3.12.0

The main feature of this release is support for automatic publishing and distributing. As well as this, the Pulp RPM plugin now has support for synching **Oracle ULN** repositories using ULN remotes. You can set an instance wide ULN server base URL using the `DEFAULT_ULN_SERVER_BASE_URL` setting.  

Earlier in the month, 3.11 was released and included support for the new changes in modularity (`static_context`), as well as some important bug fixes, such as a fix that was causing high memory consumption during syncs of large repositories. An update, 3.11.1, came later in the month and provided additional bugfixes. For more information, see the [changelog](https://docs.pulpproject.org/pulp_rpm/en/3.11.1/changes.html#id1).

### Pulp Debian 2.13.0

Pulp Debian 2.13.0 was primarily a compatibility release for Pulpcore 3.13 and for the future Pulpcore 3.14 release. 2.13.0 also contains some bug fixes. For more information, see the [changelog](https://docs.pulpproject.org/pulp_deb/changes.html#id2).

This month, translation file synchronization was disabled from the Debian plugin. The feature was incomplete and the cause of various sync failures  This update was ported to and released in the following versions: 2.9.2, 2.10.2, 2.11.2, and 2.12.1.

Translation file synchronization may be reintroduced in the future.

### Pulp Python 3.3.0

The 3.3.0 release adds support for automatic publishing and distributing. You can find the full [changelog here](https://pulp-python.readthedocs.io/en/latest/changes.html). [Python](https://pypi.org/project/pulp-python-client/3.3.0/) and [Ruby](https://rubygems.org/gems/pulp_python_client/versions/3.3.0) bindings are also available.

### Pulp Container 2.6.0

The 2.6.0 release adds support for adding a default remote to a repository for syncing, as well as a slew of bug fixes. For more information, see the [changlog](https://docs.pulpproject.org/pulp_container/en/2.6.0/changes.html).
