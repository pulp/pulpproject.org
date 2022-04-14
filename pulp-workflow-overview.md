---
title: Pulp 101
sidebar: home_sidebar
permalink: /pulp-workflow-overview/
toc: false
---

This page aims to provide you with an understanding of the major considerations so that you can then build a Pulp deployment that suits your needs.

First, let's look at [Pulp's primary concepts](/pulp-workflow-overview//#pulp---from-such-great-heights) and how they [fit together](/pulp-workflow-overview//#pulp-for-cicd).
Then, we'll walk through Pulp's architecture and how that can be made highly available if required.


## Scope

Pulp is highly flexible and configurable.
This document explains concepts and general workflows but does not instruct you how to set them up.
For more comprehensive information, see our [documentation](/docs/).

## Pulp - from such great heights

The following diagram illustrates a highly simplified overview of Pulp.

![](/images/pulp-workflow-architecture-ha/pulp-101.png)

1. You can sync packages from remote sources such as public repositories like PyPI, or, for example, from another Pulp server.
2. You can host these packages in repositories in a Pulp server.
You can organize the packages in whatever way you want.
3. You can make the repositories available for systems to consume the packages.
You can connect a server or CI to a particular set of repositories, for example, repositories that contain packages that have been tested and are ready for deployment to production environments. You can also push local changes to Pulp.

### Content plugins

Pulp has a plugin-based architecture.
Depending on what content you want to manage, you can add [content plugins](/content-plugins/) either during installation or at a later stage.

Let's take a closer look at each concept in the process.

### Remotes

A remote defines the external source of content that you want to sync to Pulp.

![](/images/pulp-workflow-architecture-ha/pulp-remote.png)

For example, you can mirror some or all Python packages from PyPI to your Pulp instance.

To do this, you must install the correct content plugin to sync a particular content type to Pulp.
For example, Python content requires the Pulp Python plugin.

This content is then synced to a repository in Pulp.

For a list of available plugins see our [content plugins](/content-plugins/) page.

For an example of how to create and sync Python content using remotes, see [Synchronize a Repository](https://docs.pulpproject.org/pulp_python/workflows/sync.html) in the Pulp Python plugin documentation.


### Repositories

A repository is a versioned set of specific packages in Pulp.

![](/images/pulp-workflow-architecture-ha/pulp-repository-versioning.png)

Every time you add or remove content, a new version is automatically created.

In this example, a set of packages is synced from PyPi, and then later, a file is removed, creating another version.
Each time, a new repository version is created.
You also have options to manage how many versions of a given repository you want to retain.
You can create new repositories and promote packages across those repositories.

### Distributions

A distribution makes a specific version of a repository available to users by exposing the repository through a base path.

![](/images/pulp-workflow-architecture-ha/pulp-distribution.png)

For example, you can make multiple versions of a repository available to different users at different base paths.

For some content types like RPM and Python, you also need a **Publication**.
Publications provide the necessary metadata for particular content types.

### Key Concepts in action

#### Keep working when public sources go offline
Despite everyone's best efforts, external content sources can go offline unexpectedly. For example, PyPI or Dockerhub.
With this setup, you can ensure stability and continuity of your deployment:
* make a remote to sync all of PyPI
* Create a repository
* Configure the repository to sync nightly
* Configure pip on my servers to pull from my Pulp server instead of PyPI.

In this way, you can maintain stability of your workflows.

#### Reduce rate limiting

Some platforms, such as Dockerhub have rate-limiting.

* Make a remote and sync all of Dockerhub
* Create a repository
* Configure docker to pull from my Pulp server instead of Dockerhub
* Create a distribution so that everyone in your organization can pull Docker imags from Pulp rather than Dockerhub
* When a user tries to pull an image, first check the Pulp server for the image and only pull from Dockerhub if the image doesn't exist in Pulp

#### Distribute content privately

If you develop content in-house that you need to distribute but do not want to share on external sites.

* Create a repository in Pulp
* Push the local content that you create to Pulp
* Create a distribution for this repository and share that with anyone who needs to consume the content.


#### Pulp for CI/CD

This example assumes packages already have been synced to Pulp from an external repository.

If you want to see a real-world application of Pulp, check out StackHPC's [All Aboard the Release Train](https://stackhpc.com/all-aboard-the-release-train.html).

![](/images/pulp-workflow-architecture-ha/pulp-cicd-example.png)

This is the workflow illustrated in this example:

1. Pulp is configured to have three separate repositories: _Dev_, _Test_, and _Prod_.
2. A package is built and uploaded into the _Dev_ repository.
3. Packages in the _Dev_ repository are then tested, for example, using integration tests.
There might be some back and forth, which creates different repository versions.
4. When all tests have passed, the packages are promoted to the _Test_ repository.
5. Quality engineering (QE) syncs packages from the _Test_ repository. QE passes/fails certain tests.
6. Devs provide patches that they then promote new versions to the Test Repository.
7. When tests complete, QE promotes the packages to the _Production_ (_Prod_) repository.
8. Members of your organization or your customers install packages from the Production repository.


### Further reading

Pulp can be deployed and scaled to meet high availability requirements. For more information, see [Pulp & High Availability](/pulp-ha/)
If you want a look at what happens beneath the surface, take a look at our [Under the hood](/under-the-hood/) page.
