---
title: Features
sidebar: home_sidebar
permalink: /features/
toc: false
---

### One tool for different content types

With Pulp, you can fetch, upload, and distribute content from a wide variety of [content types](/content-plugins/). Add the plugins for the different content you want to work with and use Pulp to manage them all.  

### Safely roll back with repository versioning

Every time you add or remove content, Pulp creates an immutable repository version so that you can roll back to earlier versions and thereby guarantee the safety and stability of your operation.

### Optimizing disk storage and speed during remote synchronization

Download and store only what you need from remote repositories. You can select from three modes to optimize disk speed and storage when synchronizing. While the _default_ download option downloads all content, you can enable either the _on_demand_ or _streamed_ option. The _on_demand_ option saves disk space by downloading and saving only the content that clients request. With the _streamed_ option, you download also on client request without saving the content in Pulp. This is ideal for synchronizing content, for example, from a nightly repository and saves having to perform a disk clean up at a later stage.

### Multiple storage options

As well as local file storage, Pulp supports a range of cloud storage options, such as Amazon S3 and Azure, to ensure that you can scale to meet the demands of your deployment.
