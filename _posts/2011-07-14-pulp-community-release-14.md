---
id: 373
title: Pulp Community Release 14
date: 2011-07-14T15:09:54+00:00
author: Jay Dobies
layout: post
guid: http://blog.pulpproject.org/?p=373
permalink: /2011/07/14/pulp-community-release-14/
categories:
  - Releases
---
<img src="http://website-pulp.rhcloud.com/wp-content/uploads/2011/07/cr-14.png" alt="" title="Community Release 14" width="442" height="161" class="alignnone size-full  wp-image-374" srcset="http://www.pulpproject.org/wp-content/uploads/2011/07/cr-14-300x109.png 300w, http://www.pulpproject.org/wp-content/uploads/2011/07/cr-14.png 442w" sizes="(max-width: 442px) 100vw, 442px" />

## Installation &#038; Upgrade

Installation instructions can be found in the [Pulp User Guide](http://pulpproject.org/ug/UGInstallation.html#installation) at the [Pulp project web site.](http://www.pulpproject.org)

As usual, upgraded environments must run `pulp-migrate` to upgrade to the latest database changes.

### mod_wsgi

We&#8217;ve known for a while that we&#8217;ve been pushing our luck using both mod\_wsgi and mod\_python. As of this release, we&#8217;ve removed the dependency on mod_python, which was previously used for our repository authentication.

The caveat is that the current mod\_wsgi version doesn&#8217;t support access to the client&#8217;s SSL certificate, which we need. To get around this, we&#8217;re building a patched version of mod\_wsgi (`mod_wsgi-3.2-3.sslpatch`) which is available in our repository. The Pulp RPMs have a dependency on this version and should upgrade it automatically for you.

## Enhancements

The majority of the team was focused on an internal project that uses Pulp as the backend. We also revisited our unit tests and cleaned up the testing framework. That resulted in a number of (in many cases very tricky) bug fixes and less focus on new features. That said, we were still able to make progress on new features:

  * Added the infrastructure to support union and intersection for queries that specify more than one parameter. Support for this has been added to repository-related queries and will be extended across the rest of Pulp in the future.
  * Errata search has been enhanced to provide queries based on CVE and BZ number.
  * Fixes LDAP support and updated to interact with Pulp&#8217;s authentication subsystem. Full changes can be found in the [bugzilla entry 712859](https://bugzilla.redhat.com/show_bug.cgi?id=712859#c3) (patch submitted by contributor, see below).
  * Pulp&#8217;s sync library, Grinder, now uses a process-based approach instead of thread-based to correct a number of concurrency issues dealing with the underlying security libraries.
  * Support added for custom yum metadata files to be included in repositories. More information on the extent of this feature can be found [on the wiki design page](https://fedorahosted.org/pulp/wiki/CustomMetadata).
  * Design and planning for the next major evolution of Pulp which will support non-RPM content (more on that in an upcoming post).
  * 30 bug fixes, accessible through [bugzilla.](https://bugzilla.redhat.com/buglist.cgi?target_release=Sprint%2025&#038;query_format=advanced&#038;bug_status=MODIFIED&#038;bug_status=ON_DEV&#038;bug_status=ON_QA&#038;bug_status=VERIFIED&#038;product=Pulp&#038;classification=Community)

## Contributors

Special thanks to Chris St. Pierre for his contributions to Pulp. In addition to feedback on usage and direction for the product, he submitted a number of patches to the Pulp team. Beefy patches too, not just fixing a typo here or there, but things like significant fixes to our LDAP support.

## Build Versions

  * Pulp: 0.206
  * Grinder: 0.105
  * Gofer: 0.42