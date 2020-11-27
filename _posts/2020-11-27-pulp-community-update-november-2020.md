---
title: Pulp Community Update (November 2020)
author: Melanie Corr
tags:
  - newsletter
---


## Pulp Community Survey Results

This Autumn, we requested feedback from the Pulp community to learn what matters to you and how your Pulp experience has been. A summary of the survey results is [now available](). If there isn't a live survey, we still hope to hear from you. You're always welcome to start a discussion about anything Pulp-related on `pulp-list@redhat.com`.

## Python project to manage Pulp repos: pulp_operations

Community member [Erik Whitesides](https://github.com/ewhitesides) shared [pulp_operations](https://github.com/ewhitesides/pulp_operations) - a Python project that you can use to help manage many aspects of Pulp repositories.

## Pulp 3.9 Release Date Update

The preliminary release date for Pulp 3.9 was moved forward to December 7th, to allow for the migration from Travis. GitHub actions will be used for future Pulp releases.

## Plugin Updates

### Pulp RPM 3.8 is now Generally Available

As well as a number of bug fixes, this release includes adds new fields so that users can customize `gpgcheck` signature options in a publication.
For more information, check out the [changelog](https://pulp-rpm.readthedocs.io/en/latest/changes.html#id1).

### Pulp Ansible 0.5.2

This month, there have been two z-stream updates to the Pulp Ansible plugin.

These updates bring a number of bug fixes, which include providing better error messages.

For more information, see the [changelog](https://pulp-ansible.readthedocs.io/en/0.5.2/changes.html#id1)

Many of these changes factor into the [Ansible Galaxy_NG](https://github.com/ansible/galaxy_ng) project.

### Pulp Python 3.0.0's 12th Beta is Available!

We are edging closer to the release of the Pulp Python 3.0, and this beta release demonstrates some of the major features that Pulp Python users have to look forward to.

Here is some information from Daniel Alley in the release announcement:

* It is now possible to perform a mirror sync of all of PyPI, without specifying any packages up-front. As a complete clone of PyPI would take multiple terabytes of disk space, we recommend that this option be used in conjunction with "on_demand" syncing, and we have made this the default for the Python plugin, differentiating it from many other plugins.
* A basic PyPI-like JSON API for package metadata has been implemented. It should be considered a "tech preview" and does not currently provide the full set of metadata that PyPI does.

For more information, check out the [Pulp Python changelog](https://pulp-python.readthedocs.io/en/latest/changes.html).

## Katello Pulp 3 - Applicability Differences & Troubleshooting

If you're interested in following the progress of the Katello project and its migration from Pulp 2 to Pulp 3, you can watch a recent update here:

<iframe width="560" height="315" src="https://www.youtube.com/embed/qxJ4sPV3FVE" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
