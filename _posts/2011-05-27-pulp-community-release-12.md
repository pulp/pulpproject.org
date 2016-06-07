---
id: 294
title: Pulp Community Release 12
date: 2011-05-27T19:15:40+00:00
author: Jay Dobies
layout: post
guid: http://blog.pulpproject.org/?p=294
permalink: /2011/05/27/pulp-community-release-12/
categories:
  - Releases
---
<img src="http://website-pulp.rhcloud.com/wp-content/uploads/2011/05/cr-12-2.png" alt="" title="cr-12-2" width="451" height="167" class="alignnone size-full wp-image-295" srcset="http://www.pulpproject.org/wp-content/uploads/2011/05/cr-12-2-300x111.png 300w, http://www.pulpproject.org/wp-content/uploads/2011/05/cr-12-2.png 451w" sizes="(max-width: 451px) 100vw, 451px" />

The Pulp team is proud to announce the availability of Community Release 12.

## Enhancements

  * [Repo Discovery](http://pulpproject.org/ug/UGRepo.html#discovery) feature added which includes new APIs to schedule repository discovery and check discovery status. Functionality added to the CLI as well through the `pulp-admin repo discovery` command.
  * Feed URLs for managing repositories [have been changed](http://pulpproject.org/ug/UGRepo.html#crud) to remove the `type:url` format. The feed type is inferred from the URL itself.
  * Legacy RHN support removed from Pulp.
  * Added support for text only errata.
  * Enhanced errata delete to check for references before allowing the delete to take place.
  * [Package details](https://fedorahosted.org/pulp/wiki/PackageDetails) enhanced to track more metadata.
  * Standardized date/time input and output to [ISO8601 format](https://fedorahosted.org/pulp/wiki/DateAndTimeFormats)
  * Server/Agent communications secured by a [shared secret authorization scheme.](https://fedorahosted.org/pulp/wiki/Agent#Authorization)
  * Over 30 Bugs resolved, accessible through [bugzilla.](https://bugzilla.redhat.com/buglist.cgi?target_release=Sprint%2023&#038;query_format=advanced&#038;product=Pulp&#038;classification=Other)

## Upgrade Notes

  * Run `pulp-migrate` to upgrade to latest DB after upgrading the Pulp packages and before running the `pulp-server` script.

## Build Versions

  * Pulp: 0.181
  * Grinder: 0.99
  * Gofer: 0.37

## Where&#8217;s the Fedora 15 Support?

It&#8217;s coming. We were too far into QE by the Fedora 15 release and didn&#8217;t have the time to verify the build to our satisfaction. Starting with next week&#8217;s testing releases, builds should be available for Fedora 15 as well. Pulp&#8217;s policy is to support the current and current &#8211; 1 Fedora releases, so this Community Release is the last Pulp Fedora 13 builds that will be supported.