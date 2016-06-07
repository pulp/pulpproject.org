---
id: 410
title: Pulp Community Release 15
date: 2011-08-15T14:22:02+00:00
author: Todd Sanders
layout: post
guid: http://blog.pulpproject.org/?p=410
permalink: /2011/08/15/pulp-community-release-15/
categories:
  - Releases
---
## Installation & Upgrade

Installation instructions can be found in the [Pulp User Guide](http://pulpproject.org/ug/UGInstallation.html#installation) at the [Pulp project web site.](http://www.pulpproject.org)

As usual, upgraded environments must run `pulp-migrate` to upgrade to the latest database changes.

### Client Tools

As mentioned below, the client tools have been refactored. This affects the client configuration files in /etc/pulp which have to be fixed manually after you upgrade.

### SELinux

After the upgrade,  SELinux no longer has to be set to permissive/disabled. Although, no change is required.

## Enhancements

  * File Based Syncs
  * Support to sync file based content repos. Requires a PULP-MANIFEST metadata file
  * New option content_type added to repo creation cli and api. default is &#8216;yum&#8217;; supported types: &#8216;yum&#8217; & &#8216;file&#8217;
  * Pulp synchronizer has been re-architected to support generic content model
  * Interrupt/Resume Downloads
  * Pulp and Grinder now supports resuming partial download files

  * Client Refactoring
  * Split the pulp-client tool into logical components:
  * pulp-consumer &#8211; Consumer client tool
  * pulp-admin &#8211; Admin client tool
  * pulp-client-lib &#8211; Common client libraries

  * Since the pulp-client package has been renamed, pulp-admin and/or pulp-consumer will have to be installed, not updated. These new packages obsolete pulp-client, so it will be removed when the new packages are installed.
  * The previous client config file has been split into a pulp-consumer specific config file and a pulp-admin specific config file. Any values that were set in /etc/pulp/client.conf will need to be manually reset in the new config files:
  * /etc/pulp/consumer/consumer.conf &#8211; pulp-consumer config
  * /etc/pulp/admin/admin.conf &#8211; pulp-admin config

  * Added SELinux rules
  * Pulp will run with SELinux enabled on RHEL-6 and Fedora
  * Pulp and SELinux will not be supported on RHEL-5

  * Improved cancel of repository synchronization
  * Previous implementation required pulp wait for the current item to finish downloading before synchronization would be interrupted.
  * New implementation will interrupt a transfer, useful for interrupting transfer of larger ISOs.

  * Added support for jobs which are a collection of tasks.
  * The REST API was expanded to include job queries. Also, pulp-admin CLI contains job command. To enable, edit /etc/pulp/admin/job.conf and change: enabled=True.
  * All consumer group operations updated to use jobs for better status and result feedback for each consumer.
  * Install package on consumer group returns job instead of task.
  * Install errata on consumer group return job instead of task.

## Known Issues

Years and months can not be used when specifying timeout values in the cli, due to an incompatibility in a library and will be fixed in a future release. See <https://bugzilla.redhat.com/show_bug.cgi?id=729496> for a workaround if they are specified and pulp-server can&#8217;t be restarted.

## Build Versions

  * Pulp: 0.223-4
  * Grinder: 0.110
  * Gofer: 0.44