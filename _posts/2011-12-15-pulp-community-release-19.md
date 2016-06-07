---
id: 548
title: Pulp Community Release 19
date: 2011-12-15T18:02:39+00:00
author: Jay Dobies
layout: post
guid: http://blog.pulpproject.org/?p=548
permalink: /2011/12/15/pulp-community-release-19/
categories:
  - Releases
---
<img src="http://website-pulp.rhcloud.com/wp-content/uploads/2011/12/cr-19.png" alt="" title="Pulp Community Release 19" width="442" height="161" class="aligncenter size-full wp-image-550" srcset="http://www.pulpproject.org/wp-content/uploads/2011/12/cr-19-300x109.png 300w, http://www.pulpproject.org/wp-content/uploads/2011/12/cr-19.png 442w" sizes="(max-width: 442px) 100vw, 442px" />

## Installation &#038; Upgrade

Installation instructions can be found in the [Pulp User Guide](http://pulpproject.org/ug/UGInstallation.html#installation) at the [Pulp project web site.](http://www.pulpproject.org) As usual, upgraded environments must run `pulp-migrate` to upgrade to the latest database changes. This release includes upgraded versions of both the server and client RPMs (admin and consumer) and should be upgraded in conjunction with each other.

### Upgrade Database Migration

Last release, changes were made with respect to requiring the relative path of a repository to be unique. This sprint saw further refinements that prevent one repository from being nested within another. For example, hosting a repository at &#8220;/foo/bar&#8221; and another at &#8220;/foo/bar/baz&#8221; causes a number of issues and will be prevented going forward. However, repositories &#8220;/foo/bar/wombat&#8221; and &#8220;/foo/bar/zombie&#8221; are valid as long as &#8220;/foo/bar&#8221; is not itself a repository.

If your installation contains repositories with relative paths that violate this rule, the `pulp-migrate` script will display a warning. The migration will complete and the Pulp server will continue to run, however the repositories in question should be manually fixed as soon as possible.

### Custom Built Dependencies

Due to a bug in python-oauth, the Pulp repositories now contain a patched version: `python-oauth2-1.5.170-2.pulp.fc16.noarch.rpm`. This version is a dependency on the Pulp RPM and the version from the Fedora repositories (1.5.170-2.fc16) will be upgraded during the Pulp installation. Appropriate care should be taken for systems using this library.

Additionally, the Pulp built version of python-isodate has been upgraded to `ython-isodate-0.4.4-4.pulp.fc16.noarch.rpm`.

### Supported Platforms

In keeping with Pulp&#8217;s policy of building for Fedora&#8217;s current and previous releases, builds for Fedora 14 will no longer be provided. Builds for this Community Release are provided for Fedora 15 and Fedora 16 (RHEL builds remain unchanged).

## Features &#038; Changes

  * Updated SELinux Policy 
      * Improved upon security rules from previous releases.
      * Pulp files are now labled with the `httpd_sys_content_t` context.
      * Renamed semodule name from `pulp` to `pulp-server`.
      * More details can be found on <a href="https://fedorahosted.org/pulp/wiki/SELinux" target="new">the SELinux design wiki page.</a>
  * Repository Filters Enahncements 
      * Repository filters are now applied to manually uploaded content in addition to synchronized content.
  * Bulk Repository Status APIs 
      * Added API calls to retrieve sync status and history for all or a specified set of repositories in a single API call.
      * More details can be found on <a href="https://fedorahosted.org/pulp/wiki/RepoBulkAPIEnhancements" target="new">the Repository Bulk API wiki page.</a>
  * Repository Enhancements &#038; Changes 
      * Added the ability to update checksum type on a repository. Doing so will cause the repository metadata to be regenerated.
      * Added uniqueness constraint to ensure relative paths are unique across repositories.
      * Added checks to ensure that the relative path of a new repository will not allow the repository to be nested within another repository. See the user guide for more information.
  * Repository Clone Enhancements &#038; Changes 
      * Overall performance tweaks to greatly improve the speed of the clone operation.
      * Cloning now has the ability to resolve duplicate packages, distribution tree missing sub-directories, and metadata checksum mismatches between filesystem and database.
      * Changed cloned repository symlinks to refer to the central package location instead of the parent repository.
      * Added the clone operation to the persistent task framework, allowing an in process clone to be resumed on server restart.
  * Distribution Enhancements 
      * Added an `arch` field to the distribution model.
      * Distribution API root changed from `/distribution` to `/distributions` to be consistent with the Pulp API conventions.
      * A number of changes have been made to the distribution API to be more REST-like.
  * Added a new AMQP event that is raised when a task is dequeued. Currently, this only applies to tasks related to repository synchronization. More information can be found on <a href="https://fedorahosted.org/pulp/wiki/QpidEvents" target="new">the Pulp&#8217;s AMQP events wiki page.</a>
  * Added support for updating packages on a consumer. Package update is modeled after yum behavior in that when packages are specified, only those packages are updated. When no packages are specified, all upgradable pacakges are updated. See the User Guide and API documentation for more details.
  * <a href="https://bugzilla.redhat.com/buglist.cgi?target_release=Sprint%2030&#038;query_format=advanced&#038;bug_status=VERIFIED&#038;product=Pulp&#038;classification=Community" target="new">Fixed 21 bugs.</a>