---
title: Manage Your Python Content with the Pulp Python plugin
author: Melanie Corr
tags:
  - release
---

The long awaited, Pulp Python 3.0 has been released!

With this release, it is now possible to [mirror the whole of PyPI](https://pulp-python.readthedocs.io/en/latest/workflows/sync.html#creating-a-remote-to-sync-all-of-pypi) in just under one hour.

To celebrate this release, let's take a look at how the Pulp Python plugin can help you manage your Python content.

## Break the dependency on connectivity

Interacting with content on a platform like PyPI requires internet access. If PyPI or your own network connection drops for any reason, you lose access to the content you need. Your business might require restrictive downloading policies or an air-gapped environment. Using the Pulp Python plugin, you can mirror and manage an on-premise version of your Python content, and distribute the content you need throughout your organization.

## Manage all your content in one place

With Pulp, you are in full control of all the content your project depends on. If you develop in-house Python content and want to control and secure the distribution of that content, you can use Pulp to upload and distribute that content throughout your organization. If you also mirror public content from a platform like PyPI, you can distribute a blend of public and private content throughout your organization.  

## Never lose anything you need

Python packages that you rely on can disappear from a remote source. Losing access to the package version can cause dependency issues. When you manage content with Pulp, you have full control over the repository versions that your systems might depend upon.

## Manage dependencies

Dependencies for your software can change very often. Package compatibility issues can lead to breaking changes. While you might need to work through these problems in the development stage, if package-compatibility issues creep into your production environment, you might face unwanted downtime. Pulp ensures that you can curate your content to maintain a safe and stable production environment.

## Roll back whenever you need

Develop software safe in the knowledge that every time you add or remove content, Pulp creates an immutable repository version. If something doesn’t work out, you can roll back to the earlier version and continue, guaranteeing the safety and stability of your operation. Pulp maintains a record of all changes to help with troubleshooting and monitoring.

## Distribute and promote content with ease

Depending on your setup, content distribution can be laden with difficulties and require more than one tool. Pulp has been built with package distribution and promotion in mind. When the content that you’ve been developing is ready, you can easily distribute this content to a new environment for testing, staging and production.

## Try Pulp Python 3.0 now!

Pulp Python can be installed from [PyPI](https://pypi.org/project/pulp-python/3.0.0/) or [source](https://github.com/pulp/pulp_python/), click [here](https://pulp-python.readthedocs.io/en/latest/installation.html) for installation instructions. Client bindings for [Python](https://pypi.org/project/pulp-python-client/3.0.0/) and [Ruby](https://rubygems.org/gems/pulp_python_client/versions/3.0.0) are also available.
