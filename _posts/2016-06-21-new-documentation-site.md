---
id: 1294
title: New Documentation Site
date: 2016-06-21T14:21:37+00:00
author: Brian Bouterse
layout: post
guid: http://www.pulpproject.org/?p=1294
permalink: /2016/06/21/new-documentation-site/
categories:
  - Uncategorized
---
The Pulp team is happy to announce the availability of a new documentation site for Pulp and its plugins: <https://docs.pulpproject.org/>

**Improvements**

  * Finding the documentation you need is easier with everything on one site. Previously the Pulp platform and plugin docs each lived on its own site.
  * GA, RC, Beta, and Nightly documentation is now available.
  * Trimmed down the top-level content allows new users to find their way easier.

**Old Sites**

The readthedocs.com (RTD) sites will go offline on June 27th. Unfortunately RTD does not provide site-wide redirection so we are relying on search engines to make our new site&#8217;s content findable. Many thanks to ReadTheDocs for hosting our docs for so long.

**Content and URLs**

The new site contains 2.8+ documentation.

GA releases for x.y will live at:   https://docs.pulpproject.org/en/x.y/

The current, stable GA release will also live at https://docs.pulpproject.org/

A given x.y version could have either a Beta or an RC but not both, so those are hosted at the same place:  https://docs.pulpproject.org/en/x.y/testing/

Nightlies for a given x.y line are available at:  https://docs.pulpproject.org/en/x.y/nightly/

**Legacy Docs**

Archived versions of 2.7 docs are available here: https://docs.pulpproject.org/en/2.7/
  
Archived versions of 2.6 docs are available here: https://docs.pulpproject.org/en/2.6/

Pre 2.6 docs are available via source.

**How It Works?**

Platform and each plugin have their own Sphinx project, which can continue to be built, standalone on their own. The docs source lives in the same repos as before; for example, the [pulp_rpm source docs live here](https://github.com/pulp/pulp_rpm/tree/master/docs) and the [pulp_python source docs live here](https://github.com/pulp/pulp_python/tree/master/docs). The Pulp Jenkins infrastructure uses the [platform Sphinx project](https://github.com/pulp/pulp/tree/master/docs) as the single sphinx project to build all documentation. The Jenkins job clones all repos and creates symlinks which cause the plugin documentation source to become discovered by the platform Sphinx project at build time on Jenkins. The Jenkins job adds some [additional content is added](https://github.com/pulp/pulp_packaging/tree/master/ci/docs) to provide more structure for the top level landing pages. The Jenkins job is managed by a [Jenkins Job builder template](https://github.com/pulp/pulp_packaging/blob/master/ci/jobs/docs.yaml). By default the jobs build the active branches each night, but it can also be triggered manually.