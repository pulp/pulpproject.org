---
title: Features
sidebar: home_sidebar
permalink: /features/
toc: false
---

### One tool for different content types

With Pulp, you can fetch, upload, and distribute content from a wide variety of content types. Add the plugins for the different content you want to work with and use Pulp to manage them all.  

### Safely roll back with repository versioning

Every time you add or remove content, Pulp creates an immutable repository version so that you can roll back to earlier versions and guarantee the safety and stability of your operation. Pulp maintains a record of all changes to help with troubleshooting and monitoring.

### Optimizing disk storage and speed during remote synchronization

Download and store only what you need from remote repositories. You can select from three modes to optimize disk speed and storage when synchronizing. While the default download option downloads all content, you can enable either the on_demand or streamed option. The on_demand option saves disk space by downloading and saving only the content that clients request. With the streamed option, you can download on request without saving the content in Pulp. This is ideal for synching content that you might require in the moment but not for the long term, for example, synchronizing from a nightly repository no longer requires disk clean up at a later stage.

### Multiple Storage options

As well as local file storage, Pulp supports a range of cloud storage options, such as Amazon S3 and Azure, to ensure that you can scale to meet the demands of your deployment.

### Enabling Ansible collaboration on premise

Using the Pulp Ansible plugin, you can create an on-premise platform to manage and distribute a scalable blend of public and private Ansible roles and collections across your organization.
