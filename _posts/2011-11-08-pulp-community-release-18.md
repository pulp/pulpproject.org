---
id: 521
title: Pulp Community Release 18
date: 2011-11-08T18:24:23+00:00
author: Jay Dobies
layout: post
guid: http://blog.pulpproject.org/?p=521
permalink: /2011/11/08/pulp-community-release-18/
categories:
  - Releases
---
<img src="http://website-pulp.rhcloud.com/wp-content/uploads/2011/11/cr-18.png" alt="" title="Pulp Community Release 18" width="444" height="161" class="aligncenter size-full wp-image-522" />

## Installation &#038; Upgrade

Installation instructions can be found in the [Pulp User Guide](http://pulpproject.org/ug/UGInstallation.html#installation) at the [Pulp project web site.](http://www.pulpproject.org) As usual, upgraded environments must run `pulp-migrate` to upgrade to the latest database changes.

### Upgrade Database Migration

The `pulp-migrate` script might warn about repositories with the same relative paths. Relative paths need to be unique across repositories in Pulp. Please make sure to clean up these repositories as this functionality is deprecated and will be unsupported in upcoming releases.

### Server Configuration

The configuration option:

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="ini" style="font-family:monospace;"><span style="color: #000066; font-weight:bold;"><span style="">&#91;</span>tasking<span style="">&#93;</span></span>
max_concurrent</pre>
      </td>
    </tr>
  </table>
</div>

has changed to:

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="ini" style="font-family:monospace;"><span style="color: #000066; font-weight:bold;"><span style="">&#91;</span>tasking<span style="">&#93;</span></span>
concurrency_threshold</pre>
      </td>
    </tr>
  </table>
</div>

This is in preparation for the administrator to &#8220;weight&#8221; tasks. Currently, the semantics of the property have not changed. Details can be found here <a href="https://fedorahosted.org/pulp/wiki/WeightedTasks" target="new">on the weighted tasks design wiki.</a>

### Consumers

The pulp consumer certificate location is now configurable. As part of this change, the default location was changed from:

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="bash" style="font-family:monospace;"><span style="color: #000000; font-weight: bold;">/</span>etc<span style="color: #000000; font-weight: bold;">/</span>pki<span style="color: #000000; font-weight: bold;">/</span>consumer<span style="color: #000000; font-weight: bold;">/</span></pre>
      </td>
    </tr>
  </table>
</div>

to:

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="bash" style="font-family:monospace;"><span style="color: #000000; font-weight: bold;">/</span>etc<span style="color: #000000; font-weight: bold;">/</span>pki<span style="color: #000000; font-weight: bold;">/</span>consumer<span style="color: #000000; font-weight: bold;">/</span>pulp</pre>
      </td>
    </tr>
  </table>
</div>

to avoid collisions with RHSM certificates. After upgrading Pulp on consumer systems, the `pulp-agent` daemon must be restarted and the consumer re-registered. Alternatively, the certificate may be manually migrated to the new directory.

## Features &#038; Changes

  * Added support for package uninstallation through the API and CLI.
  * Added support for package group uninstallation through the API and CLI.
  * Enhanced progress reporting of repository synchronization. Progress is now reported on each individual item being downloaded.
  * Exporter (continued from last sprint): 
      * Added Support for deltarpms and prestodelta metadata export.
      * Added ability to export repository groups to target location.
  * Distribution Enhancements: 
      * Distributions now use a new id format: `ks-$family-$variant-$version-$arch`
      * New selective sync support to add and remove distributions between repositories.
      * Added new timestamp field to distribution model to capture when the treeinfo was last updated.
      * Pulp now stores distributions in a central location (`/var/lib/pulp/distribution`) for space optimization and to allow a distribution to be associated with multiple repositories.
      * The distribution `relativepath` to be path of the distro location.
      * The distribution URL will now use the repository&#8217;s relative path. A distribution can be associated to multiple repositories using same distribution ID.
  * Local Discovery: Support to discover repositories from a local file based URL paths.
  * Selective operations such as add/remove packages or errata on a repository will not automatically trigger a metadata generation task. Users need to manually trigger by calling the generate metadata on the repository after the associations are complete (`pulp-admin repo generate_metadata`).
  * Repository Notes: Repository notes are similar to key-value pairs on consumers. The ability to add, update and delete notes from a repository has been added. Repositories may also be queried on note data. For more information, see the <a href="https://pulpproject.org/ug/UGRepo.html#notes" target="new">repository section of the user guide.</a>
  * Repo Synchronization Schedule 
      * REST API changed to separate sync schedule management from repository creation and update. A new sub-collection has been added (`/pulp/api/repositories/<id>/schedules/sync/`) that accepts `GET`, `PUT`, and `DELETE` calls. Details can be found inthe <a href="https://pulpproject.org/ug/UGRepo.html" target="new">repository section of the user guide.</a>
      * The `pulp-admin` script has been changed to utilize the new REST API. Schedule creation and update are no longer part of the `pulp-admin repo create` and `pulp-admin repo update` actions. Repository scheduled syncs can now be viewed and managed with the `pulp-admin repo sync` action. The `--help` flag should be used for more details. Details can be found here: https://fedorahosted.org/pulp/wiki/UGRepo#sync