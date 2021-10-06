---
title: Pulpcore 3.16 is now Generally Available!
author: Melanie Corr
tags:
  - release
---

We are happy to announce the release of Pulpcore 3.16!

In this blog, we will look at the headline features of the release. However, as well as the headline features, this release contains a myriad of bugfixes, enhancements, as well as a host of new features for the plugin API. for a full list of changes, take a look at the [changelog](https://docs.pulpproject.org/pulpcore/changes.html#id1).

## Pulpcore 3.16 features

Let's take a look at what's new in Pulpcore 3.16.

## Support for Alternative Content Sources [#8749](https://pulp.plan.io/issues/8749), [#9375](https://pulp.plan.io/issues/9375)

While the 3.15 release introduced some basic modeling, with 3.16 we now see support added for Alternative Content Sources.

Alternate Content Sources provide a way to sync content from an alternate source so you can sync from a remote repository but use a faster or more reliable source of content such as your local file system. This release provides the functionality to prioritize remote content provided by Alternate Content Sources over regular content when syncing and in the content application.

Validation has also been added to ensure that  the remote type can be used with the Alternative Content Source.

At the moment, this functionality is supported in `pulp_file 1.10` and `pulp_rpm 3.16` plugins. Support is also available in the CLI.

### Tasking system performance improvements [#9326](https://pulp.plan.io/issues/9326)

To improve performance, read-only task resources are now shared for concurrent use.

## Removals

## Bye bye old tasking system [#9157](https://pulp.plan.io/issues/9157)

A new tasking system was introduced in Pulpcore 3.14. Since then, the old tasking system was deprecated and has now been removed.
This also included the removal of the `USE_NEW_WORKER_TYPE` setting.

## OpenAPI schema view removal [#9322](https://pulp.plan.io/issues/9322)

The browseable OpenAPI schema view had sub-optimal performance issues. It had multiple dependencies, and often displayed errors rather than the expected information. For this reason, it was removed and replaced with a less costly, clearer schema view available at `/pulp/api/v3/`.

## pulp import endpoint change [#9382](https://pulp.plan.io/issues/9382)

Previous to this release, the endpoint returned a task. This left the user with the added step of finding the task group based on the task. With this update, the endpoint returns the task group. 
