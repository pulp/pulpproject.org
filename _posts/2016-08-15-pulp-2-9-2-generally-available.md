---
title: Pulp 2.9.2 Generally Available
author: Sean Myers
tags:
  - release
  - rpm
categories:
  - Releases
---
The Pulp 2.9.2 is now available, and can be downloaded from the 2.9 stable repositories:

<https://repos.fedorapeople.org/repos/pulp/pulp/stable/2.9/>

This release includes fixes to Pulp Platform and the RPM Plugin.

## Issues Addressed

These critical issues are fixed in Pulp 2.9.2:

### Pulp
- 1863    Exception using regex in filters against associate endpoint
- 1894    unassociate rpms API request using unit_ids removes all rpm from repo
- 1728    Please relax input validation on --login for 'pulp-admin user create'

###  RPM Support
- 2135    Repo publish error Unclosed tags: endverbatim
- 2093    errors importing some rpms after upgrading to pulp 2.9
- 2077    publish fails if Django Template Syntax in changelog

View this list [in Redmine](http://bit.ly/2aygdsB).
