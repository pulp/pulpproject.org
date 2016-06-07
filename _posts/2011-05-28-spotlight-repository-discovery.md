---
id: 271
title: 'Spotlight: Repository Discovery'
date: 2011-05-28T14:06:24+00:00
author: Todd Sanders
layout: post
guid: http://blog.pulpproject.org/?p=271
permalink: /2011/05/28/spotlight-repository-discovery/
categories:
  - Spotlight
---
Community Release 12 introduces [Repository Discovery](http://pulpproject.org/ug/UGRepo.html#discovery). This is a simple and fast way of identifying all of the yum repositories in a given tree, and importing them into Pulp. Let&#8217;s explore this feature through a simple example; mirroring the Pulp Project repositories.

Let&#8217;s take a look at the **repo discovery** command:

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="none" style="font-family:monospace;">$ pulp-admin repo discovery --help
Usage: pulp-admin  repo discovery 
&nbsp;
Options:
  -h, --help            show this help message and exit
  -u URL, --url=URL     root url to perform discovery (required)
  -g GROUPID, --groupid=GROUPID
                        groupids to associate the discovered repos (optional)
  -y, --assumeyes       assume yes; automatically create candidate repos for
                        discovered urls (optional)
  -t TYPE, --type=TYPE  content type to look for during discovery(required);
                        supported types: ['yum',]</pre>
      </td>
    </tr>
  </table>
</div>

Now let&#8217;s run repo discovery for Pulp; we&#8217;ll use the -g option to add any repositories we import to a &#8220;pulp&#8221; group.

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="none" style="font-family:monospace;">$ pulp-admin repo discovery -u http://repos.fedorapeople.org/repos/pulp/ -g pulp -t yum
Discovering urls with yum metadata, This could take some time...
Waiting /
+------------------------------------------+
 Repository Urls discovered @ [http://repos.fedorapeople.org/repos/pulp/]
+------------------------------------------+
(-)  [1] http://repos.fedorapeople.org/repos/pulp/pulp/testing/fedora-14/x86_64
(-)  [2] http://repos.fedorapeople.org/repos/pulp/pulp/testing/fedora-14/i386
(-)  [3] http://repos.fedorapeople.org/repos/pulp/pulp/testing/fedora-13/x86_64
(-)  [4] http://repos.fedorapeople.org/repos/pulp/pulp/testing/fedora-13/i386
(-)  [5] http://repos.fedorapeople.org/repos/pulp/pulp/testing/6Server/x86_64
(-)  [6] http://repos.fedorapeople.org/repos/pulp/pulp/testing/6Server/i386
(-)  [7] http://repos.fedorapeople.org/repos/pulp/pulp/testing/5Server/x86_64
(-)  [8] http://repos.fedorapeople.org/repos/pulp/pulp/testing/5Server/i386
(-)  [9] http://repos.fedorapeople.org/repos/pulp/pulp/fedora-14/x86_64
(-)  [10] http://repos.fedorapeople.org/repos/pulp/pulp/fedora-14/i386
(-)  [11] http://repos.fedorapeople.org/repos/pulp/pulp/fedora-13/x86_64
(-)  [12] http://repos.fedorapeople.org/repos/pulp/pulp/fedora-13/i386
(-)  [13] http://repos.fedorapeople.org/repos/pulp/pulp/demo_repos/test_bandwidth_repo_smaller
(-)  [14] http://repos.fedorapeople.org/repos/pulp/pulp/demo_repos/test_bandwidth_repo
(-)  [15] http://repos.fedorapeople.org/repos/pulp/pulp/6Server/x86_64
(-)  [16] http://repos.fedorapeople.org/repos/pulp/pulp/6Server/i386
(-)  [17] http://repos.fedorapeople.org/repos/pulp/pulp/5Server/x86_64
(-)  [18] http://repos.fedorapeople.org/repos/pulp/pulp/5Server/i386
&nbsp;
Select urls for which candidate repos should be created; use `y` to confirm (h for help):</pre>
      </td>
    </tr>
  </table>
</div>

Then it is as simple as selecting the repositories I want to import. In this case, I am interested in Fedora only; both testing and community releases. We&#8217;ll select 1:4 and 9:12.

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="none" style="font-family:monospace;">(+)  [1] http://repos.fedorapeople.org/repos/pulp/pulp/testing/fedora-14/x86_64
(+)  [2] http://repos.fedorapeople.org/repos/pulp/pulp/testing/fedora-14/i386
(+)  [3] http://repos.fedorapeople.org/repos/pulp/pulp/testing/fedora-13/x86_64
(+)  [4] http://repos.fedorapeople.org/repos/pulp/pulp/testing/fedora-13/i386
(-)  [5] http://repos.fedorapeople.org/repos/pulp/pulp/testing/6Server/x86_64
(-)  [6] http://repos.fedorapeople.org/repos/pulp/pulp/testing/6Server/i386
(-)  [7] http://repos.fedorapeople.org/repos/pulp/pulp/testing/5Server/x86_64
(-)  [8] http://repos.fedorapeople.org/repos/pulp/pulp/testing/5Server/i386
(+)  [9] http://repos.fedorapeople.org/repos/pulp/pulp/fedora-14/x86_64
(+)  [10] http://repos.fedorapeople.org/repos/pulp/pulp/fedora-14/i386
(+)  [11] http://repos.fedorapeople.org/repos/pulp/pulp/fedora-13/x86_64
(+)  [12] http://repos.fedorapeople.org/repos/pulp/pulp/fedora-13/i386
(-)  [13] http://repos.fedorapeople.org/repos/pulp/pulp/demo_repos/test_bandwidth_repo_smaller
(-)  [14] http://repos.fedorapeople.org/repos/pulp/pulp/demo_repos/test_bandwidth_repo
(-)  [15] http://repos.fedorapeople.org/repos/pulp/pulp/6Server/x86_64
(-)  [16] http://repos.fedorapeople.org/repos/pulp/pulp/6Server/i386
(-)  [17] http://repos.fedorapeople.org/repos/pulp/pulp/5Server/x86_64
(-)  [18] http://repos.fedorapeople.org/repos/pulp/pulp/5Server/i386
&nbsp;
Select urls for which candidate repos should be created; use `y` to confirm (h for help):y
&nbsp;
Creating candidate repos for selected urls..
Successfully created repo [repos-pulp-pulp-testing-fedora-14-x86_64]
Successfully created repo [repos-pulp-pulp-testing-fedora-14-i386]
Successfully created repo [repos-pulp-pulp-testing-fedora-13-x86_64]
Successfully created repo [repos-pulp-pulp-testing-fedora-13-i386]
Successfully created repo [repos-pulp-pulp-fedora-14-x86_64]
Successfully created repo [repos-pulp-pulp-fedora-14-i386]
Successfully created repo [repos-pulp-pulp-fedora-13-x86_64]
Successfully created repo [repos-pulp-pulp-fedora-13-i386]</pre>
      </td>
    </tr>
  </table>
</div>

That&#8217;s it. Now all that&#8217;s left to be done is sync content from the remote feeds. Currently, Pulp is limited in it&#8217;s Repository Group operations; only supporting listing by group. For example to list the repos we just created:

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="none" style="font-family:monospace;">$ pulp-admin repo list --groupid=pulp</pre>
      </td>
    </tr>
  </table>
</div>

In the future, we will be expanding the set of group operations to support update, setting re-occurring sync schedules, sync, etc.