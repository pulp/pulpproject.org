---
id: 644
title: 'Pulp v2 Preview: Unit Copy'
date: 2012-06-15T15:37:10+00:00
author: Jay Dobies
layout: post
guid: http://blog.pulpproject.org/?p=644
permalink: /2012/06/15/pulp-v2-preview-unit-copy/
categories:
  - 2.0
  - Spotlight
---
<div style="font-style:italic">
  The Pulp team is gearing up for the first community release of the v2 code base. Over the next few weeks I&#8217;ll be highlighting some of the new features in the v2 release. As with any blog entry, this is scoped to a point in time and the specific details are subject to some change before release. The overall concepts, however, should remain consistent despite any tweaks we make.
</div>

# Introduction

The cloning and filtering functionality is one of the most used features in Pulp v1. In v2, I wanted to expand on the capabilities based on feedback I&#8217;ve been hearing from the community and their use of cloning. The result is that the v1 notion of cloning is gone entirely.

The v2 model supports a much less tight coupling to the yum repository on the filesystem. Instead of having a single working area on disk where changes are instantly made visible, the driving operation in Pulp has been split between the process of importing RPMs into a repository (synchronization) and the process by which they are made available to outside consumers (publishing).

This split provides an opportunity for a significant manipulation of the contents of a repository before it is ever made live. RPMs imported into the server can be treated as building blocks that are easily manipulated in the database to quickly build up entirely new repositories before they are published.

Additionally, the v2 model has a stronger understanding of the relationship between a package and a repository, allowing us to track metadata about the association itself, such as when it was associated to a repository. This model comes into play in the unit copy feature and gives us a basis for more advanced association management in the future (for instance, the ability to version a repository at a point in time).

The unit copy functionality works by specifying a source repository and criteria to use to match packages in it. Matching packages will be associated with a specified destination directory. Unlike v1 cloning, there is no restriction on the source repository; multiple copy calls can be issued against different source repositories to populate the destination repository. Criteria can be specified as regular expressions or greater than/less than comparisons against any field in the package, everything from the name and version fields to vendor and license values. Once the destination repository is fully populated, it can be published to make the repository contents publicly accessible.

Before I go on it&#8217;s important to highlight that the idea of unit copy refers to its association with a repository. Pulp will still maintain a single storage on the filesystem of an RPM; what is copied is only the database&#8217;s understanding of which packages are associated with which repositories.

# Demo Setup

For this demo, I created two repositories.

The &#8220;pulp-f17&#8221; repository was created using Pulp&#8217;s testing repository for Fedora 17 as the feed. It has been synchronized multiple times over the course of a few days. The pulp-* RPMs had new versions released in that time, however the dependencies (for example, python-okaara) have not changed.

The &#8220;dest&#8221; repository was created without a feed. It doesn&#8217;t need one; it will be populated by units selectively copied from the pulp-f17 repository and then manually published to make its contents available.

Below is a list of the names and versions of packages in the pulp-f17 repository for reference.

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="none" style="font-family:monospace;">gofer-0.69
gofer-package-0.69
grinder-0.1.3
m2crypto-0.21.1.pulp
m2crypto-debuginfo-0.21.1.pulp
mod_wsgi-3.3
mod_wsgi-debuginfo-3.3
pulp-0.0.295
pulp-admin-0.0.295
pulp-cds-0.0.295
pulp-client-lib-0.0.295
pulp-common-0.0.295
pulp-consumer-0.0.295
pulp-selinux-server-0.0.295
python-gofer-0.69
python-isodate-0.4.4
python-oauth2-1.5.170
python-okaara-1.0.18
python-qpid-0.7.946106
python-rhsm-0.96.4
python-webpy-0.32</pre>
      </td>
    </tr>
  </table>
</div>

# Usage

Unit copies are done through the `pulp-admin repo copy` commands. Beneath the copy section is a separate command by type of unit being copied. At the time of this article, only RPMs, SRPMs, and DRPMs are supported, but in the future errata and potentially kickstart trees will be supported.

At minimum there are two required parameters: the source repository (`--from-repo-id`) and the destination (`--to-repo-id`). Without any criteria, this will have the effect of copying all of the RPMs in the source into the destination. If an RPM to be copied already exists in the destination, it is ignored.

That&#8217;s not terribly interesting in and of itself. The power of this feature comes in the criteria available to select units to copy. Before I get to that, the `--dry-run` or `-d` flag will display a list of packages that match the criteria without actually performing the copy. Also keep in mind that packages can be copied from multiple different source repositories using multiple calls to the copy commands; for expediency this demo will only cover a single source repository.

## Regular Expression Matching

The simplest form of criteria is a regular expression match against a field in each RPM. The format of this argument is to pass in the field name and associated regular expression. For instance, to match on RPM names that start with &#8220;p&#8221;, the full argument string would be `--match "name=^p.*"`.

Multiple fields can be matched by specifying as many arguments as necessary. For example, to match on names that start with &#8220;p&#8221; whose architecture is &#8220;noarch&#8221;, two arguments would be specified: `--match "name=^p.*" --match "arch=noarch"`. A full (dry run) example of this can be seen below:

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="bash" style="font-family:monospace;">$ pulp-admin repo copy rpm <span style="color: #660033;">--from-repo-id</span> pulp-f17 <span style="color: #660033;">--to-repo-id</span> dest <span style="color: #660033;">-d</span> <span style="color: #660033;">--match</span> <span style="color: #ff0000;">"name=^p.*"</span> <span style="color: #660033;">--match</span> <span style="color: #ff0000;">"arch=noarch"</span>
+----------------------------------------------------------------------+
                             Matching Units
+----------------------------------------------------------------------+
&nbsp;
Filename: pulp-0.0.295-<span style="color: #000000;">1</span>.fc17.noarch.rpm
Filename: pulp-admin-0.0.295-<span style="color: #000000;">1</span>.fc17.noarch.rpm
Filename: pulp-cds-0.0.295-<span style="color: #000000;">1</span>.fc17.noarch.rpm
Filename: pulp-client-lib-0.0.295-<span style="color: #000000;">1</span>.fc17.noarch.rpm
Filename: pulp-common-0.0.295-<span style="color: #000000;">1</span>.fc17.noarch.rpm
Filename: pulp-consumer-0.0.295-<span style="color: #000000;">1</span>.fc17.noarch.rpm
Filename: pulp-selinux-server-0.0.295-<span style="color: #000000;">1</span>.fc17.noarch.rpm
Filename: python-gofer-<span style="color: #000000;">0.69</span>-<span style="color: #000000;">1</span>.fc17.noarch.rpm
Filename: python-isodate-0.4.4-<span style="color: #000000;">4</span>.pulp.fc17.noarch.rpm
Filename: python-oauth2-1.5.170-<span style="color: #000000;">2</span>.pulp.fc17.noarch.rpm
Filename: python-okaara-1.0.18-<span style="color: #000000;">1</span>.fc17.noarch.rpm
Filename: python-qpid-0.7.946106-<span style="color: #000000;">1</span>.fc17.noarch.rpm
Filename: python-rhsm-0.96.4-<span style="color: #000000;">1</span>.fc17.noarch.rpm
Filename: python-webpy-<span style="color: #000000;">0.32</span>-<span style="color: #000000;">8</span>.fc17.noarch.rpm</pre>
      </td>
    </tr>
  </table>
</div>

Selecting packages that do **not** match a regular expression uses the same syntax to the `--not` argument. For example, to list all packages in the repository that are dependencies of Pulp but not actually part of Pulp itself:

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="bash" style="font-family:monospace;">$ pulp-admin repo copy rpm <span style="color: #660033;">--from-repo-id</span> pulp-f17 <span style="color: #660033;">--to-repo-id</span> dest <span style="color: #660033;">-d</span> <span style="color: #660033;">--not</span> <span style="color: #ff0000;">"name=^pulp.*"</span>
+----------------------------------------------------------------------+
                             Matching Units
+----------------------------------------------------------------------+
&nbsp;
Filename: gofer-<span style="color: #000000;">0.69</span>-<span style="color: #000000;">1</span>.fc17.noarch.rpm
Filename: gofer-package-<span style="color: #000000;">0.69</span>-<span style="color: #000000;">1</span>.fc17.noarch.rpm
Filename: grinder-0.1.3-<span style="color: #000000;">1</span>.fc17.noarch.rpm
Filename: m2crypto-0.21.1.pulp-<span style="color: #000000;">7</span>.fc17.x86_64.rpm
Filename: m2crypto-debuginfo-0.21.1.pulp-<span style="color: #000000;">7</span>.fc17.x86_64.rpm
Filename: mod_wsgi-<span style="color: #000000;">3.3</span>-<span style="color: #000000;">3</span>.pulp.fc17.x86_64.rpm
Filename: mod_wsgi-debuginfo-<span style="color: #000000;">3.3</span>-<span style="color: #000000;">3</span>.pulp.fc17.x86_64.rpm
Filename: python-gofer-<span style="color: #000000;">0.69</span>-<span style="color: #000000;">1</span>.fc17.noarch.rpm
Filename: python-isodate-0.4.4-<span style="color: #000000;">4</span>.pulp.fc17.noarch.rpm
Filename: python-oauth2-1.5.170-<span style="color: #000000;">2</span>.pulp.fc17.noarch.rpm
Filename: python-okaara-1.0.18-<span style="color: #000000;">1</span>.fc17.noarch.rpm
Filename: python-qpid-0.7.946106-<span style="color: #000000;">1</span>.fc17.noarch.rpm
Filename: python-rhsm-0.96.4-<span style="color: #000000;">1</span>.fc17.noarch.rpm
Filename: python-webpy-<span style="color: #000000;">0.32</span>-<span style="color: #000000;">8</span>.fc17.noarch.rpm</pre>
      </td>
    </tr>
  </table>
</div>

## Comparisons

RPM fields can be compared against a value as well. The client supports greater than (`--gt`), greater than or equal to (`--gte`), less than (`--lt`), and less than or equal to (`--lte`) comparisons. The format is the same, specifying both the field name and the value to use as the comparison.

For example, to copy only packages whose version is at least 1.0:

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="bash" style="font-family:monospace;">$ pulp-admin repo copy rpm <span style="color: #660033;">--from-repo-id</span> pulp-f17 <span style="color: #660033;">--to-repo-id</span> dest <span style="color: #660033;">-d</span> <span style="color: #660033;">--gt</span> <span style="color: #ff0000;">"version=1.0"</span>
+----------------------------------------------------------------------+
                             Matching Units
+----------------------------------------------------------------------+
&nbsp;
Filename: mod_wsgi-<span style="color: #000000;">3.3</span>-<span style="color: #000000;">3</span>.pulp.fc17.x86_64.rpm
Filename: mod_wsgi-debuginfo-<span style="color: #000000;">3.3</span>-<span style="color: #000000;">3</span>.pulp.fc17.x86_64.rpm
Filename: python-oauth2-1.5.170-<span style="color: #000000;">2</span>.pulp.fc17.noarch.rpm
Filename: python-okaara-1.0.18-<span style="color: #000000;">1</span>.fc17.noarch.rpm</pre>
      </td>
    </tr>
  </table>
</div>

## Association Criteria

As I mentioned above, we have a much stronger understanding of the association between a repository and a package. Part of that is tracking when it was added to the repository. This allows us to specify criteria such as &#8220;Copy all packages added in the last 30 days.&#8221;

In my demo setup, the Pulp version was updated on June 8th, however many of the dependencies did not have new versions released in that build. For the purposes of this demo, let&#8217;s assume that I promote content from the live repository (pulp-f17) into an unstable repository (dest) once a month. To see what qualifies for promotion in June, I&#8217;d run:

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="bash" style="font-family:monospace;">pulp-admin repo copy rpm <span style="color: #660033;">--from-repo-id</span> pulp-f17 <span style="color: #660033;">--to-repo-id</span> dest <span style="color: #660033;">-d</span> <span style="color: #660033;">--after</span> <span style="color: #000000;">2012</span>-06-01
+----------------------------------------------------------------------+
                             Matching Units
+----------------------------------------------------------------------+
&nbsp;
Filename: pulp-0.0.295-<span style="color: #000000;">1</span>.fc17.noarch.rpm
Filename: pulp-admin-0.0.295-<span style="color: #000000;">1</span>.fc17.noarch.rpm
Filename: pulp-cds-0.0.295-<span style="color: #000000;">1</span>.fc17.noarch.rpm
Filename: pulp-client-lib-0.0.295-<span style="color: #000000;">1</span>.fc17.noarch.rpm
Filename: pulp-common-0.0.295-<span style="color: #000000;">1</span>.fc17.noarch.rpm
Filename: pulp-consumer-0.0.295-<span style="color: #000000;">1</span>.fc17.noarch.rpm
Filename: pulp-selinux-server-0.0.295-<span style="color: #000000;">1</span>.fc17.noarch.rpm</pre>
      </td>
    </tr>
  </table>
</div>

Again, that example matches packages added to the repository after June 1st. New versions of a package qualify as new packages and will be picked up as part of this criteria. The format to `--after` (and its opposite, `--before`) is an iso8601 formatted timestamp; I simply omitted the time component.

## Finishing Up

Once the desired packages have been copied (don&#8217;t forget to remove the `-d` flag to actually commit the copy), the destination repository can be published to build and expose the new repository (or simply update the repository if it was already published). I&#8217;ll cover publishing in v2 in a separate post, but for completeness the command is: `pulp-admin repo publish run --repo-id dest`.

# Future Work

As usual, don&#8217;t quote me on any of this. I don&#8217;t have an idea yet on where any of this would fit in to our priorities, but I can at least say a few words about how I want to see this evolve.

The first step would be adding the ability for the server to saved named criteria. While this can somewhat be achieved by client-side scripts (or a CLI extension, but again, more on that in a separate article), I think it makes sense as a server-side concept. The idea would be to define regular criteria values and use an identifier to run that criteria again in the future.

Our scheduler in v2 is significantly more flexible in terms of the amount of work it takes on the server to use it, so I&#8217;d like to see copy calls get scheduled and incorporated into a normal scheduled sync workflow. Along the same lines, I&#8217;d like to be able to batch the copy call at the end of a sync, which would greatly aid in automation. The idea would be to tell the Pulp server to sync a repository, for example, every Sunday at 1am and then copy all newly synchronized packages into an unstable repository.

# References

The user guide entry for unit copy can be found at: <a href="http://pulpproject.org/v2/rpm-user-guide/admin-client/repo/units.html#copy-packages-between-repositories" target="new">http://pulpproject.org/v2/rpm-user-guide/admin-client/repo/units.html#copy-packages-between-repositories</a>