---
title: Pulp Community Survey Results
author: bizhang
---

About three weeks ago we launched the first Pulp Community Survey. Our goal was to poll the
community and use the data to create a well-received Pulp2 -> Pulp3 transition plan, and figure our
priorities after getting the Pulp3 MVP out the door.

We had a total of **27** responses, thank you everyone that replied!

Here's some statistics and insights we got out of the survey.

## Usage

![How long have you been using Pulp?](/images/communitysurvey/2017/how-long-using.png)

The majority of our users have been using Pulp for more than 2 years. It will be interesting to
see how this number changes in future surveys.

![What version of pulp do you use?](/images/communitysurvey/2017/pulp-version.png)

As the time of the survey 2.13 is the latest version of Pulp. We officially support the last two
.y releases. 63% of people are on a supported version of Pulp. Pulp is in an interesting position
since we are released with Katello, Foreman, and Satellite. Satellite 6.2 in particular is using
Pulp 2.8, which explains the uptick in 2.8 users. If there is some bug preventing you from
upgrading to a supported version of Pulp let us know on the #pulp channel on freenode.

![Upgrade Frequency](/images/communitysurvey/2017/upgrade-frequency.png)

This supports the data from the pulp version graph. 52% our users upgrades pulp when new releases
come out, and 28% are pinned to downstream products.


![How do you interact with Pulp?](/images/communitysurvey/2017/how-interact.png)

More people use the CLI over the REST API. For Pulp3 we plan on generating the CLI from the REST
API schema, so everyone should see a more feature complete CLI.


![Pulp Plugin Usage](/images/communitysurvey/2017/plugin-usage.png)

No surprises here, 96% of users uses the RPM plugins. Most people use 1-2 plugins. And one person
exclusively uses the debian plugin.

## Workflows

The most popular workflow is using Pulp to promote content; content is copied from dev repos to
testing repo to production repos.

There is an interesting variation where instead of coping content between repos, a symlink is
changed from current to previous, so the yum.conf files on the clients are not changed.


Other workflows include:

* Managing custom content
* repo mirroring
* Importing multiple repos from a central config file via https://github.com/nbetm/python-pulpadm
* Content Management with Katello, using Pulp for status checks and listing object details.


## Database

### How big is your /var/lib/pulp directory?

/var/lib/pulp varied in size, from 2GB to 2.8TB. The mean size (after trimming max and min values)
is 233GB with a standard deviation of 198. This data supports our planned strategy of not touching
/var/lib/pulp during the Pulp2 -> Pulp3 migration and leaving the Pulp2 symlinks in place without
a cleanup until we have a confirmed successful migration to Pulp 3, since doing such during
migration would be too time insensitive and they may be necessary for rollback.  Removing symlinks
should be painless after a successful migration to Pulp 3.


### How big is your Pulp database?

MongoDB size on disk ranged from 56KB to 52GB, averaging (after trimming) 18.8GB with a standard
deviation of 8.9.

### Migrations

There's an equal amount of people who wants in-place migration and side-by-side migration. We plan
on testing and supporting both approaches. It appears that most users who want to do an in-place
migration find about 2 hours of full downtime (0 pulp interaction), and 8 hours of content serving
downtime is acceptable. We will keep these numbers in mind while testing the Pulp2 -> Pulp3
migration. Expected downtime for people who prefer side-by-side migrations is 0 :)


## Pulp3

### What missing features would prevent your adoption of Pulp3?

It appears that the MVP features is viable for most users. The most requested supported plugin
types is RPM (of course), and (surprisingly) Debian.


### What is the one feature you would really like to see added in Pulp 3?

The number one requested feature is a separate Web UI. This would definitely not be prioritized
anytime soon, unless someone wants to contribute something (hint hint).

Some other popular feature requests:

* Database other than Mongo :)
* Auto orphan cleanup
* Smarter sync for specific packages and dependencies from large RPM repos
* More plugins (Rubygems, NPM, Java)


Big thanks to everyone that filled out the survey! It's really great to get some use cases and
hard data to help us make decisions with Pulp 3.