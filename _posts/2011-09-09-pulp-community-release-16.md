---
id: 444
title: Pulp Community Release 16
date: 2011-09-09T20:23:41+00:00
author: Jay Dobies
layout: post
guid: http://blog.pulpproject.org/?p=444
permalink: /2011/09/09/pulp-community-release-16/
categories:
  - Releases
---
<img src="http://website-pulp.rhcloud.com/wp-content/uploads/2011/09/cr-16.png" alt="" title="Pulp Community Release 16" width="442" height="161" class="aligncenter size-full wp-image-445" srcset="http://www.pulpproject.org/wp-content/uploads/2011/09/cr-16-300x109.png 300w, http://www.pulpproject.org/wp-content/uploads/2011/09/cr-16.png 442w" sizes="(max-width: 442px) 100vw, 442px" />

## Installation &#038; Upgrade

Installation instructions can be found in the [Pulp User Guide](http://pulpproject.org/ug/UGInstallation.html#installation) at the [Pulp project web site.](http://www.pulpproject.org)

As usual, upgraded environments must run `pulp-migrate` to upgrade to the latest database changes.

### M2Crypto

Pulp now provides a patched version of m2crypto (`m2crypto-0.21.1.pulp-3.*.rpm`). The changes within are required for the newly added certificate revocation list support. The RPM can be found in the Pulp repositories along side the rest of the Pulp RPMs.

## Enhancements

  * Certificate revocation lists (CRLs) are now available for protected repositories. CRLs can be installed on a per-CA basis as necessary given the authenticated repository setup. Instructions for configuring a Pulp server to use a CRL can be found in the [Repository Authentication](http://pulpproject.org/ug/UGRepoAuth.html) section of the user guide.
  * Support has been added for installing a package group to a consumer group.
  * Custom metadata types can now be removed from existing repository metadata (only in Fedora 14 and 15; RHEL support requires a change to createrepo that is in progress).

## Build Versions

  * Pulp: 0.230-3
  * Grinder: 0.112
  * Gofer: 0.47