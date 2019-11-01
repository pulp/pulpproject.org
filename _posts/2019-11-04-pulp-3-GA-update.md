---
title: Pulp 3.0 Release Update
author: Brian Bouterse
tags:
  - 3.0
---
As announced in [an earlier blogpost](https://pulpproject.org/2019/09/30/pulp-3-GA-timeline/) Pulp
3.0 is scheduled to be frozen on Nov 12th and Generally Available on Dec 3rd, 2019. Here are details
on how we'll be building and curating releases for Pulp 3, including 3.0.0 and other releases like
3.0.1, 3.0.2, ..., 3.1.0, 3.1.1, etc.

### Terminology

* **release**: is a x.y.z number. It's specific because it always includes the "z"
* **y-release**: is a release where the z portion equals 0, e.g. 3.0.0, 3.1.0, 3.2.0, etc.
* **z-release**: is a release where the z portion is not 0, e.g. 3.0.1, 3.0.2, 3.1.1, etc.
* **z-stream**: The set of z releases following a specific y-release.


### Release Lead

Each 3.y release will have a release lead who performs the cherry picking of fixes to be included in
the 3.y.z releases. Brian Bouterse will be the release lead for pulpcore for the 3.0, which includes
releases 3.0.0, 3.0.1, and any other 3.0 z-stream releases. For 3.1 and its z-stream, another release
lead will be identified.

Each plugin team is encouraged to have a release lead who performs similar functions for that
plugin's y-release.


### Branching

When 3.0 is frozen (or any y-release), it will be branched from the master branch. The branch name
will be the y-release name, e.g. 3.0, 3.1, 3.2, etc. All changes merge to the master first, so the
y-release branch includes all merged changes.


### Cherry Picking

The z-stream is curated by cherry picking changes from the master branch to the y-release branch,
e.g. 3.0. The release lead performs the cherry picking as needed and with input from developers and
users.


### Tagging a Release

When a release is ready, it is tagged as the x.y.z version number. For the 3.0 y-release this will
be 3.0.0. This tagging is expected on Dec 3rd. Once tagged Travis tests things one final time before
pushing the bits to PyPI and the bindings to PyPI and rubygems.org.


### What changed in release 3.y.z?

While the git-history is the ultimate source of truth, it's too large and has too much change to
easily see the meaningful changes that occurred in a release. As such, the changelog for the release
is the recommended way to see what changed in a specific release.

The changelog is formally generated as part of the tagging process and will be available in the
documentation for that release. To see the changelog before it's generated browse the CHANGES folder
at the root of the repo, which is where changelog snippets are stored. You could also generate the
changelog locally using ``towncrier --draft`` in a local checkout of the repository.

Changelog entries are provided by developers along with their changes, so the cherry picking process
will maintain the changelog.


### What about pulp.plan.io?

All changelog entries require an associated [pulp.plan.io](https://pulp.plan.io) issue, and each
generated changelog line links to its pulp.plan.io issue. Users can read these pulp.plan.io issues
for more detailed information on the change.

The commits themselves are also associated with the pulp.plan.io issue as "associated commits". This
serves various downstream users who want to perform their own cherry picking in their source tree.
For this reason it's recommended that all commit changes are associated with a single issue, even in
the case of a followup adjustment pre-release.

Issues will remain at `MODIFIED` as long as they are unreleased. Once released, either in a
y-release or z-release, whichever contains that fix and occurs first, the release lead will move the
issue to `CLOSED - CURRENT RELEASE`.
