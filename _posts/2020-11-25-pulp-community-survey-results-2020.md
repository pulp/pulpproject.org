---
title: Pulp Community Survey Results 2020
author: Melanie Corr
tags:
  - survey
---

The Pulp community survey was open to submissions during August and September of 2020.
In the survey, we looked at the release of Pulp 3, as well as those who continue to use Pulp 2.

In total, **20** Pulp users participated in the survey.

## General Pulp Usage

Of the respondents, there was an even 50/50 split of those using Pulp 2 and Pulp 3. If respondents were using Pulp 2, they were asked some additional migration-related questions, while Pulp 3 users were asked specifically about their Pulp 3 experience.

At the beginning of the survey, participants were asked a number of general Pulp questions before we delved into the different versions.


![Which version of Pulp is your main deployment?](/images/communitysurvey/2020/pulp2vpulp3.png)

![Which operating system have you installed Pulp on?](/images/communitysurvey/2020/operating-system-pulp.png)


### Workflows

* Mirroring and syncing of repos to promote through the different stages of the content lifecycle environment
* Distributing custom content from different sources throughout an organization
* Content Management with Katello.

### Where does your content come from and how do you get it into Pulp (e.g. sync/upload/etc)?

* Synced content from repositories.
* Mirroring repositories.
* The additional respondents uploaded content from internal systems.

### Do you horizontally scale (cluster) Pulp and if so, which components?

The overwhelming majority answered no to this question.
Of the few that answered yes, said only for HA for a fail-over partner, clustering for resilience not performance.

### How big is your /var/lib/pulp directory?

The `/var/lib/pulp `directory ranged from 250 GB to 2.5 TB in size.

### What type of storage do you use for /var/lib/pulp? For example, NFS.

There were somewhat diverse answers to this question:

* NFS
* XFS
* SAN
* Ext4 FS
* Virtual disk with LVM

![](/images/communitysurvey/2020/upgrade-frequency.png)


## Pulp 2 users

Those whose primary deployment is Pulp 2 were asked a number of Pulp 2-specific and Pulp 2-to-3 migration related questions.

### Pulp 2 plugins


Of the Pulp 2 respondents:
* 100% use the Pulp 2 RPM plugin.
* 60% of respondents use more than one plugin.
* As well as the RPM plugin, the most popular plugin is the Python plugin, with 30% of respondents using it in addition to RPM.

Pulp 2 users reported also using the Debian, Plugin, and Docker plugins.


### Do you rely on the Pulp 2 CLI or API? If so, can you explain why?

80% of Pulp 2 respondents reported that they use the Pulp 2 CLI exclusively and depend on it for daily tasks. Some users wrote that they script against it and find it essential for sysadmin tasks.

10% said that they use both the CLI and API.

10% said that they use only the Pulp 2 API.

![Pulp 2 is now in maintenance mode. When do you plan to upgrade to Pulp 3?](/images/communitysurvey/2020/migration-plans.png)


![To what extent have you tried Pulp 3?](/images/communitysurvey/2020/experience-with-pulp3.png)

### Are there any other obstacles or problems that would prevent you from upgrading to Pulp 3 soon, for example, the lack of a plugin or particular feature?

There were many reasons cited as challenges to migrating from Pulp 2 to Pulp 3, but most responses were not related to the actual process of migrating.
The largest grouping, which included 30% of users, reported that the lack of a Pulp 3 CLI was a blocker for them.

Other than the CLI, the following reasons were cited:
* The lack of a production-ready Pulp 3 container image.
* A perception that there was a much broader skillset required to use Pulp 3, with references in the documentation, for example, to Ansible and Python.
* A lack of a Puppet plugin.
* On-premise installation via RPMs because of security restrictions.
* Rewriting of internal tooling.


### Reason for not planning to migrate to Pulp 3

100% answered the lack of a Pulp 3 CLI, with an additional response, as above, that the lack of on-premise RPMs was also a blocker.

![Do you have enough information to migrate from Pulp 2 to Pulp 3?](/images/communitysurvey/2020/migration-information.png)


Of the 33% who reported no, the following reasons were cited:

* No CLI
* Can the migration work with Pulp 3 in a container?


### Have you tried the Pulp 2 to 3 Migration plugin?

* 56% reported that they had yet to try the migration plugin.
* 20% reported that they plan to do "clean-slate" installations of Pulp 3.
* Additional respondents reported having done provisional testing and are waiting for additional features. These features were not named.

The [migration plugin](https://pulpproject.org/migrate-to-pulp-3/) is still in a beta phase, so these results are not surprising.

It'll be interesting to measure this against the results of next year's survey.

## Pulp 3 users

Those who selected that their primary deployment was Pulp 3 were asked specific questions about their Pulp 3 experience.

Of the Pulp 3 users, none reported migrating from Pulp 2 to Pulp 3. All deployments were fresh Pulp 3 installations.
None reported using an Amazon S3 storage backend for their Pulp 3 deployment, with one user reporting that they plan to move to S3 soon.

![How did you install Pulp 3?](/images/communitysurvey/2020/pulp3-installation-method.png)

![Which version of Pulp 3 are you using?](/images/communitysurvey/2020/pulp3version.png)

The latest version of Pulp 3 at that time was 3.5, with 3.6 being released during the weeks that the survey was open.


## Which Pulp 3 plugins do you use?

* 91% reported using the Pulp RPM plugin.
* Over half of Pulp 3 users use 3 or more plugins.
* Of the users that use only one plugin, those users use only the RPM plugin.
* After RPM, the most popular plugins are File and Container plugins.

## Which plugin is Pulp missing and you would like to see developed?

All questions in the form were optional, and users did not widely respond to this question. One user replied that they were content with what is available.
The answers to this question were in single figures, with a small majority saying a Debian plugin, which is great because a Debian plugin is now available.

Other responses were as follows:

* StackStorm exchange
* An updated Pulp Python plugin
* A "working" Pulp Container plugin. A respondent reported problems with the Container plugin when used with a custom Docker image.

### Pulp 3 provides a plugin API and a plugin template that makes it easier to write plugins. Are you interested in writing a plugin?

* 81.8% reported no desire to write a plugin.
* 21.2 reported that they are interested.

Of the 21.2% that is interested in writing a plugin, the main obstacle to writing one was "time".

### Would any potential feature(s) make it easier for you to run and maintain Pulp?

50% cited a Pulp 3 CLI would make their Pulp 3 experience easier. Users reported having to write 1000s of lines of script just for basic Pulp 3 functionality.

Of the other responses, here are what Pulp users cited:

* Easier installation process
* A Stackstorm content pack for maintaining Pulp via StackStorm workflows
* An Ansible plugin (what this means is unclear)
* A web UI

![Do you need multiple users on your Pulp 3 installation?](/images/communitysurvey/2020/multi-users.png)

![Do you need to run Pulp 3 on Kubernetes or Openshift?](/images/communitysurvey/2020/kube-or-openshift.png)

![What existing identity management do you want to integrate with Pulp 3?](/images/communitysurvey/2020/pulp-idm.png.png)

### If a full deployment of Pulp in one container was available, would that be useful for you?

66% of respondents answered yes to this question.

### Is there another storage service backend that you want to use with Pulp 3? If so, which one?

The answers to this question were diverse:

* A Container data store.
* Azure blob storage
* NFS
* Ceph


### A note on need versus want in 2020

For some of the questions in the 2020 survey, "need" was used to ask questions about potential features. For example, do you need a web UI, do you need a Pulp 3 CLI.
In previous surveys, the word "want" was used. For example, do you want a web UI, do you want a Pulp 3 CLI.
As with many aspects of life, what one needs and what one wants can be very different things. This change of verbiage might have caused a sharp reversal on some answers between 2019 and 2020.
On discussing this issue with a data analyst, these types of questions might be best avoided altogether in future surveys. Surveys are most useful when measuring the lived experiences of the community.

![Do you need a Pulp 3 CLI for your environment?](/images/communitysurvey/2020/pulpcli-requirement.png)

![Do you need a Pulp 3 web UI for your environment?](/images/communitysurvey/2020/pulp-web-ui.png)

### If you answered Yes to the previous questions, please provide any information about the workflows that would improve with access to a CLI and/or Web UI.

* Sync external report, promote copies through dev->QA->prod life cycle.
* Updating/syncing current repositories
* Configuration, and quick status queries. It is not convenient to craft the queries with httpie, and the squeezer modules only cover a limited set of APIs.
* Integrating / triggering jobs via Ansible.

![Have you ever found it difficult to find the information you needed about a Pulp feature you were trying to use?](/images/communitysurvey/2020/finding-information.png)

### Feedback on difficulties with docs/information

From reviewing the answers, some of the users have not interacted recently with Pulp docs.
Although their feedback was accurate, things might have improved over the last year or so.

Some of the reasons cited were:

* Documentation very developer-focused
* Getting started was very hard
* API documentation vague in places
* Documentation difficult to find

![Have you contributed to Pulp?](/images/communitysurvey/2020/contributing-to-pulp.png)

![Interested in contributing to Pulp?](/images/communitysurvey/2020/interest-in-contributing.png)

A big thank to everyone who participated in the Pulp Community Survey this year!
