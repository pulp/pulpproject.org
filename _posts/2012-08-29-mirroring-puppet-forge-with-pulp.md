---
id: 751
title: Mirroring Puppet Forge with Pulp
date: 2012-08-29T01:50:47+00:00
author: Jay Dobies
layout: post
guid: http://blog.pulpproject.org/?p=751
permalink: /2012/08/29/mirroring-puppet-forge-with-pulp/
categories:
  - 2.0
  - Spotlight
---
# Introduction

One of the biggest changes in Pulp v2 is the ability to manage non-RPM content types. The first use case we had in mind for this is to inventory Puppet modules and serve them from the Pulp server. This includes both uploading modules into the Pulp server as well as selectively downloading and keeping up to date modules served at <a href="http://forge.puppetlabs.com/" target="new">Puppet Forge</a>.

Keep in mind that this is an early preview of code I&#8217;m still working on and is subject to changes in the coming weeks.

# Repository Creation & Configuration

Pulp organizes content into _repositories_. The granularity of a repository is up to the user. A typical usage is to stage content from a live stream (such as Puppet Forge), to a testing environment, ultimately migrating the tested modules into a production usage. This paradigm can be modeled using separate Pulp repositories and copying the modules between them as appropriate.

Pulp will use the modules.json file located at Puppet Forge to determine which modules should be downloaded. Technically speaking, there is nothing that limits Pulp to using Puppet Forge. Any host can be used so long as the modules.json format is supported. Additionally, modules.json offers a limited query syntax for scoping which modules are included in its metadata. Pulp uses this query syntax to limit a repository to contain only certain modules.

For existing Pulp users, the pulp-admin client is undergoing some changes to support both RPM and Puppet content (and any further additions we support in the future). All of the Puppet repository related commands are located under the `puppet` section in the root of the client.

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="none" style="font-family:monospace;">$ pulp-admin puppet repo
Usage: pulp-admin repo [SUB_SECTION, ..] COMMAND
Description: repository lifecycle commands
&nbsp;
Available Sections:
  copy    - copy modules from one repository into another
  group   - repository group lifecycle commands
  publish - run, schedule, or view the status of publish tasks
  remove  - remove copied or uploaded modules from a repository
  sync    - run, schedule, or view the status of sync tasks
  uploads - upload modules into a repository
&nbsp;
Available Commands:
  create - creates a new repository
  delete - deletes a repository
  list   - lists repositories on the Pulp server
  search - searches for Puppet repositories on the server
  update - changes metadata on an existing repository</pre>
      </td>
    </tr>
  </table>
</div>

Creating a repository is done, not surprisingly, through the `create` command. This command requires a ID to be specified to uniquely identify the repository in Pulp. While all other configuration is optional, when mirroring Puppet Forge there are two options in particular of interest:

The `feed` option is used to indicate the URL of the host from which to download modules. This must refer to the location in which the modules.json file resides. For Puppet Forge, this is simply `http://forge.puppetlabs.com`.

The `query` argument is used to limit which modules are downloaded into the repository. It may be specified multiple times if a single query is not enough to fully describe all of the desired modules.

For example, the following command will create a repository that will download Apache and MySQL related modules:

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="none" style="font-family:monospace;">$ pulp-admin puppet repo create --repo-id blog-repo --feed http://forge.puppetlabs.com/ --query httpd --query mysql
Successfully created repository [blog-repo]</pre>
      </td>
    </tr>
  </table>
</div>

There are two options related to the serving of Puppet modules from the Pulp server itself. The `serve-http` and `serve-https` options are used to indicate which of these protocols the Pulp server will make the repository available over. These may be enabled individually or both may be active at once (technically, none can be active in the event a repository should contain modules for use in copying to other repositories but never itself be exposed). By default, Puppet repositories will be served over HTTP but not HTTPS.

The list of repositories is retrieved using the `list` command. A more advanced repository search is supported but I won&#8217;t cover it here.

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="none" style="font-family:monospace;">$ pulp-admin puppet repo list
+----------------------------------------------------------------------+
                              Repositories
+----------------------------------------------------------------------+
&nbsp;
Id:                 blog-repo
Display Name:       blog-repo
Description:        None
Content Unit Count: 0</pre>
      </td>
    </tr>
  </table>
</div>

The repository reflects that there are zero content units (where &#8220;unit&#8221; is the generic Pulp term for a piece of content it manages) because the modules have not been downloaded yet. This is done through the Pulp &#8220;sync&#8221; process.

# Sync and Publish

A repository sync operation is the process through which the external feed for a repository is contacted to check for changes to its content. On the initial sync, all modules (matching any queries if specified) will be downloaded to the Pulp server and inventoried into its database. On subsequent sync operations, only new modules and new versions of existing modules will be downloaded. Additionally, any modules that were once present in the feed but have been removed will be removed from the Pulp repository as well.

The publish process is the opposite. When Pulp publishes a Puppet repository, it takes the current modules in the repository and serves them over HTTP and/or HTTPS from the Pulp server. This includes modules added to a repository from a sync operation as well as those uploaded by a user or copied from another Pulp repository.

By default, this publish operation happens automatically on the tail end of a successful sync operation. Users may trigger a publish explicitly in cases where only local changes to a repository have been made (upload, copy) or if there is no external feed configured for the repository.

A sync can be run immediately using the `sync run` command. By default, the command will continue to poll the Pulp server and display the progress as it synchronizes and publishes the repository. This output can be skipped using the `--bg` command or simply by pressing ctrl+c; the sync will continue on the Pulp server.

Below is a sample output from the sync command. Remember that the repository was configured with two queries (httpd and mysql) and is set to only publish the modules over HTTP.

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="none" style="font-family:monospace;">$ pulp-admin puppet repo sync run --repo-id blog-repo
+----------------------------------------------------------------------+
                  Synchronizing Repository [blog-repo]
+----------------------------------------------------------------------+
&nbsp;
This command may be exited by pressing ctrl+c without affecting the actual
operation on the server.
&nbsp;
Downloading metadata...
[==================================================] 100%
Metadata Query: 2/2 items
... completed
&nbsp;
Downloading new modules...
[==================================================] 100%
Module: 20/20 items
... completed
&nbsp;
Publishing modules...
[==================================================] 100%
Module: 20/20 items
... completed
&nbsp;
Generating repository metadata...
[-]
... completed
&nbsp;
Publishing repository over HTTP...
... completed
&nbsp;
Publishing repository over HTTPS...
... skipped</pre>
      </td>
    </tr>
  </table>
</div>

Each repository has a separate location under `/pulp/puppet`. Within that directory, the published repository will use the same format as Puppet Forge. Modules are located under a directory with the first letter of the author&#8217;s name and then futher under the author&#8217;s name. The module name will be&#8211;.tar.gz. Additionally, Pulp will generate a modules.json file in the root of the repository, allowing systems that use this for parsing to use a Pulp server instead of Puppet Forge. The modules.json file will be customized to include only modules in the repository when it was published.

Below are some screenshots of navigating the published repository. Firefox hides the &#8220;http&#8221; portion, but the port 80 line shows the serve-http flag was honored.

<img class="alignnone size-full wp-image-763" title="repo-root" src="http://website-pulp.rhcloud.com/wp-content/uploads/2012/08/repo-root.png" alt="" width="770" height="601" srcset="http://www.pulpproject.org/wp-content/uploads/2012/08/repo-root-300x234.png 300w, http://www.pulpproject.org/wp-content/uploads/2012/08/repo-root.png 770w" sizes="(max-width: 770px) 100vw, 770px" />

Digging into the system and releases directories shows the breakdown by the first character of each author name:

<img class="alignnone size-full wp-image-765" title="authors" src="http://website-pulp.rhcloud.com/wp-content/uploads/2012/08/authors.png" alt="" width="770" height="601" srcset="http://www.pulpproject.org/wp-content/uploads/2012/08/authors-300x234.png 300w, http://www.pulpproject.org/wp-content/uploads/2012/08/authors.png 770w" sizes="(max-width: 770px) 100vw, 770px" />

Finally, digging all the way down the hierarchy reveals the modules themselves:

<img class="alignnone size-full wp-image-771" title="modules" src="http://website-pulp.rhcloud.com/wp-content/uploads/2012/08/modules.png" alt="" width="770" height="601" srcset="http://www.pulpproject.org/wp-content/uploads/2012/08/modules-300x234.png 300w, http://www.pulpproject.org/wp-content/uploads/2012/08/modules.png 770w" sizes="(max-width: 770px) 100vw, 770px" />

# Conclusion

This article only covered the basic concepts for mirroring Puppet Forge: repository creation, configuration, and synchronization/publishing. Pulp offers a number of other features in the areas of searching for modules, uploading user-defined modules, and copying modules between repositories. Future work will involve registering a puppet master with the Pulp server and using Pulp to control the deployment of modules to the puppet master.

This functionality is not yet available in a released version of Pulp (to be honest, I just finished writing the bulk of it in the past few days). However, there are options for playing with this functionality before it makes it into a formal Community Release. See the <a href="http://repos.fedorapeople.org/repos/pulp/pulp" target="new">repository definition files in our repositories</a> for information on how to enable the weekly testing builds or Community Release candidate builds. This early in the feature development, all input is welcome and will likely be incorporated.

For more information, see <a href="https://www.redhat.com/mailman/listinfo/pulp-list" target="new">the Pulp mailing lists</a> or ping me in our chat room (jdob in #pulp on Freenode).