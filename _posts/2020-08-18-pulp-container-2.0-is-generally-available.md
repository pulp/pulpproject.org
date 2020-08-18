---
title: Pulp Container 2.0.0 is Generally Available
author: Melanie Corr
tags:
  - release
---

The Pulp team is pleased to announce the release of Pulp Container 2.0.0.

This release features push API support for Docker and Podman clients. This enhancement greatly simplifies the workflow for managing containers in Pulp.

You can now push a container image to the repositories hosted by the Container Registry. For an example workflow see [Push content to a repository](https://pulp-container.readthedocs.io/en/latest/workflows/push.html#push-content-to-a-repository) in the Pulp Container plugin documentation. This release also includes security features and restrictions to ensure that you can manage container content in a secure manner.

For information about all Pulp Container workflows, see [workflows](https://pulp-container.readthedocs.io/en/latest/workflows/index.html).

You can find Pulp Container 2.0.0 on [PyPI](https://pypi.org/project/pulp-container/2.0.0/).

## Pulp Container 2.0.0 Features

* REST APIs have been added to handle Docker and Podman clients push functionality. [#5027](https://pulp.plan.io/issues/5027)
* A new `ContainerPushRepository` type has been added to support writeable container registries. [#6825](https://pulp.plan.io/issues/6825)
* Push functionality has been restricted to Pulp admin users. [#6976](https://pulp.plan.io/issues/6976)
* A new `ContentRedirectContentGuard` type has been added to support token authentication for Pulp Container. [#6894](https://pulp.plan.io/issues/6894)
* Schema conversion handling has been added for S3 storage backends. [#6824](https://pulp.plan.io/issues/6824)
* The ability to exclude tags during syncing has been added to allow users to skip tags they don't want to sync. This can save both time and disk space. [#6922](https://pulp.plan.io/issues/6922)
* Cleanup functionality has been added so that when a push repository is deleted, the associated distribution is also deleted. [#7172](https://pulp.plan.io/issues/7172)

## Bug Fixes

As part of this release and throughout the different beta iterations, the following bugs were captured and fixed:

* Fixed 500 error when pulling by tag. [#6776](https://pulp.plan.io/issues/6776)
* All relations between content models are properly created. [#6827](https://pulp.plan.io/issues/6827)
* Automatically create repositories and distributions for the container push. [#6878](https://pulp.plan.io/issues/6878)
* Fixed not being able to push tags with periods in them. [#6884](https://pulp.plan.io/issues/6884)
* Fixed problem with the `client_max_body_size` value in the nginx config. [#6916](https://pulp.plan.io/issues/6916)
* Refactored `token_authentication` so that it now happens in the `pulpcore-api` app. [#6894](https://pulp.plan.io/issues/6894)
* Fixed a crash when trying to access content with an unparseable token. [#7124](https://pulp.plan.io/issues/7124)
* Fixed a runtime error which was triggered when a registry client sends an accept header with an inappropriate media type for a manifest and the conversion failed. [7125](https://pulp.plan.io/issues/7125)
* Updated the sync machinery to not store an image manifest as a tag’s artifact. [#6816](https://pulp.plan.io/issues/6816)
* Added a validation, that a push repository cannot be distributed by specifying a version. [#7012](https://pulp.plan.io/issues/7012)
* Restrict the REST API methods PATCH and PUT so as to prevent changes to repositories created via docker/podman push requests. [#7013](https://pulp.plan.io/issues/7013)
* Fixed error rendering in the container registry API. [#7054](https://pulp.plan.io/issues/7054)
* Repaired broken registry with `TOKEN_AUTH_DISABLED=True`. [#7304](https://pulp.plan.io/issues/7304)
