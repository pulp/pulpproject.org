---
id: 792
title: Customizing Puppet Module Repositories in Pulp
date: 2012-09-25T14:34:04+00:00
author: Jay Dobies
layout: post
guid: http://website-pulp.rhcloud.com/?p=792
permalink: /2012/09/25/customizing-puppet-module-repositories-in-pulp/
categories:
  - 2.0
  - Spotlight
---
In my last post, I covered [mirroring Puppet Forge modules in Pulp.](/2012/08/29/mirroring-puppet-forge-with-pulp/) Pulp offers much more than the simple mirroring of content. This article covers creating custom repositories from both existing and newly uploaded content.

# Uploading Puppet Modules

The ability to synchronize modules from an external source such as Puppet Forge is extremely useful in a number of use cases. However, there are also situations where a module does not exist in a source containing the proper structure to be imported by Pulp. In those cases, Pulp offers the ability to upload modules into a repository.

All upload-related commands are found under the `pulp-admin puppet repo uploads` section of the admin client. In addition to the upload command itself, that section contains commands to pause an upload and resume it at a later time or see a list of in progress and paused uploads.

Uploading a module is done through the `pulp-admin puppet repo uploads` command. The ID of the repository into which the modules will be uploaded is required. Modules to upload are indicated either by individual file (`--file`) or by directory (`--dir`). Either argument may be specified more than once and may be used in conjunction with each other. In the case of uploading a directory, only files ending in `.tar.gz` will be uploaded, so it is safe to have non-module content in the directory as well.

For the example below, a new repository is created to house the uploaded modules. Note that it does not have a feed; this repository cannot be synchronized against an external source, however it may still have modules uploaded or copied into it.

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="none" style="font-family:monospace;">$ pulp-admin puppet repo create --repo-id upload-repo
Successfully created repository [upload-repo]</pre>
      </td>
    </tr>
  </table>
</div>

A directory of modules will be uploaded as an example. The output updates in regular intervals as the upload proceeds. At any time, the upload may be paused by pressing `ctrl+c`. The queued files that have not completed uploading will be remembered and can be resumed at a later point. Files that have been partially uploaded will retain the offset and begin uploading where they left off.

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="none" style="font-family:monospace;">$ ll /home/jdob/vault/code/data/puppet-modules
total 12
-rw-rw-r--. 1 500 500 1895 Aug 30 14:21 adob-good-2.0.0.tar.gz
-rw-rw-r--. 1 500 500 1949 Aug 30 14:22 jdob-valid-1.0.0.tar.gz
-rw-rw-r--. 1 500 500 1952 Aug 30 14:22 jdob-valid-1.1.0.tar.gz
&nbsp;
$ pulp-admin puppet repo uploads upload --repo-id upload-repo --dir /home/jdob/vault/code/data/puppet-modules
+----------------------------------------------------------------------+
                          Puppet Module Upload
+----------------------------------------------------------------------+
&nbsp;
Extracting necessary metdata for each file...
[==================================================] 100%
Analyzing: jdob-valid-1.1.0.tar.gz
... completed
&nbsp;
Creating upload requests on the server...
[==================================================] 100%
Initializing: jdob-valid-1.1.0.tar.gz
... completed
&nbsp;
Starting upload of selected packages. If this process is stopped through ctrl+c,
the uploads will be paused and may be resumed later using the resume command or
cancelled entirely using the cancel command.
&nbsp;
Uploading: adob-good-2.0.0.tar.gz
[==================================================] 100%
1895/1895 bytes
... completed
&nbsp;
Importing into the repository...
... completed
&nbsp;
Deleting the upload request...
... completed
&nbsp;
Uploading: jdob-valid-1.0.0.tar.gz
[==================================================] 100%
1949/1949 bytes
... completed
&nbsp;
Importing into the repository...
... completed
&nbsp;
Deleting the upload request...
... completed
&nbsp;
Uploading: jdob-valid-1.1.0.tar.gz
[==================================================] 100%
1952/1952 bytes
... completed
&nbsp;
Importing into the repository...
... completed
&nbsp;
Deleting the upload request...
... completed</pre>
      </td>
    </tr>
  </table>
</div>

Displaying the list of repositories reflects the three modules that have been uploaded:

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="none" style="font-family:monospace;">pulp-admin puppet repo list
+----------------------------------------------------------------------+
                              Repositories
+----------------------------------------------------------------------+
&nbsp;
Id:                 upload-repo
Display Name:       upload-repo
Description:        None
Content Unit Count: 3</pre>
      </td>
    </tr>
  </table>
</div>

# Searching for Puppet Modules in a Repository

Before covering copying modules between repositories, it makes sense to explain Pulp&#8217;s query syntax.

Pulp isn&#8217;t merely meant as a pass through for content as it comes into and leaves the system. Pulp provides an extremely powerful and flexible query syntax for analyzing content in the system.

One usage of this query capability is seen when searching for modules in a repository. The simplest example is to list all modules in the repository (excuse the lame data, these are some test modules I threw together for unit tests):

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="none" style="font-family:monospace;">$ pulp-admin puppet repo modules --repo-id upload-repo
Name:         good
Version:      2.0.0
Author:       adob
Dependencies: 
Description:  Good Module Description
License:      Apache License, Version 2.0
License :     None
Project Page: http://example.org/adob-good
Source:       http://example.org/adob-good/source
Summary:      Good Module Summary
Tag List:     None
Types:        
&nbsp;
Name:         valid
Version:      1.0.0
Author:       jdob
Dependencies: 
  Name:                jdob/dep-alpha
  Version Requirement: &gt;= 1.0.0
  Name:                ldob/dep-beta
  Version Requirement: &gt;= 2.0.0
Description:  Valid Module Description
License:      Apache License, Version 2.0
License :     None
Project Page: http://example.org/jdob-valid
Source:       http://example.org/jdob-valid/source
Summary:      Valid Module Summary
Tag List:     None
Types:        
&nbsp;
Name:         valid
Version:      1.1.0
Author:       jdob
Dependencies: 
  Name:                jdob/dep-alpha
  Version Requirement: &gt;= 1.1.0
  Name:                ldob/dep-beta
  Version Requirement: &gt;= 2.1.0
Description:  Valid Module Description
License:      Apache License, Version 2.0
License :     None
Project Page: http://example.org/jdob-valid
Source:       http://example.org/jdob-valid/source
Summary:      Valid Module Summary
Tag List:     None
Types:</pre>
      </td>
    </tr>
  </table>
</div>

All of the fields in the module&#8217;s Moduleinfo file are inventoried in Pulp (checksums are there as well, but hidden to make the client more readable). The tag list is not found in the Moduleinfo; my plan is to eventually support specifying these at upload time. The tag list is, however, captured when synchronizing modules from Puppet Forge and is a candidate for querying against with Pulp.

Before going into the query abilities, the examples will read better if the output data is limited. The `--fields` option can be used to indicate which fields should be displayed:

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="none" style="font-family:monospace;">$ pulp-admin puppet repo modules --repo-id upload-repo --fields name,version
Name:    good
Version: 2.0.0
&nbsp;
Name:    valid
Version: 1.0.0
&nbsp;
Name:    valid
Version: 1.1.0</pre>
      </td>
    </tr>
  </table>
</div>

Any field in the module&#8217;s metadata is available for searching. Values to match can be expressed in a number of ways:

  * Literals: &#8220;name is httpd&#8221;
  * Regular Expressions: &#8220;name matches mysql.*&#8221;
  * Enumerated Values (including negation): &#8220;author is one of &#8216;jdob&#8217; or &#8216;adob'&#8221;
  * Comparisons: &#8220;version is greater than 1.0.0&#8221;
  * Association Information: &#8220;modules added to the repository since August 1, 2012&#8221;

Any of the above can be combined to form more complex queries. A few examples are as follows:

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="none" style="font-family:monospace;">$ pulp-admin puppet repo modules --repo-id upload-repo --fields name,version --str-eq "name=valid"
Name:    valid
Version: 1.0.0
&nbsp;
Name:    valid
Version: 1.1.0
&nbsp;
$ pulp-admin puppet repo modules --repo-id upload-repo --fields name,version --gt "version=1.0.0"
Name:    good
Version: 2.0.0
&nbsp;
Name:    valid
Version: 1.1.0
&nbsp;
$ pulp-admin puppet repo modules --repo-id upload-repo --fields name,version,author --match "author=.*dob$"
Name:    good
Version: 2.0.0
Author:  adob
&nbsp;
Name:    valid
Version: 1.0.0
Author:  jdob
&nbsp;
Name:    valid
Version: 1.1.0
Author:  jdob</pre>
      </td>
    </tr>
  </table>
</div>

# Copying Modules Between Repositories

Puppet Modules in Pulp are meant to serve as building blocks for repositories. Once a module is in Pulp, be it via a sync from an external feed or uploaded, it can be copied or moved between repositories freely. The term &#8220;copying&#8221; is slightly misleading; to save disk space, the module is only ever stored once on the filesystem, it is simply associated with more than one repository.

A common usage of this ability is the promotion of content through a live -> unstable -> stable workflow (or dev -> qa -> prod depending on your terminology). Since each repository is served at its own URL, modules can be kept separate from production environments until they are tested and copied to the production repositories in Pulp.

The syntax for specifying which modules to copy is identical to the search syntax above, providing both the ability to easily copy an entire repository as well as specifically choosing a subset of modules for inclusion. Copied modules are not available at the hosted repository URL until the repository is explicitly published, allowing users to make a number of changes and validate the new contents before the modules are ever made available to the repository&#8217;s consumers.

For the example below, a new repository (again, feedless) is created as the destination for the copied modules. This new repository can be thought of as a higher repository in the workflow chains described above. Only a subset of modules &#8212; those written by jdob &#8212; will be promoted to the newly created repository. The final command in the example is used to verify only the desired modules have been copied.

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="none" style="font-family:monospace;">$ pulp-admin puppet repo create --repo-id dest-repo   
Successfully created repository [dest-repo]
&nbsp;
$ pulp-admin puppet repo list                      
+----------------------------------------------------------------------+
                              Repositories
+----------------------------------------------------------------------+
&nbsp;
Id:                 upload-repo
Display Name:       upload-repo
Description:        None
Content Unit Count: 3
&nbsp;
Id:                 dest-repo
Display Name:       dest-repo
Description:        None
Content Unit Count: 0
&nbsp;
$ pulp-admin puppet repo copy --from-repo-id upload-repo --to-repo-id dest-repo --str-eq "author=jdob"
Progress on this task can be viewed using the commands under "repo tasks".
&nbsp;
$ pulp-admin puppet repo modules --repo-id dest-repo --fields name,version,author
Name:    valid
Version: 1.0.0
Author:  jdob
&nbsp;
Name:    valid
Version: 1.1.0
Author:  jdob</pre>
      </td>
    </tr>
  </table>
</div>

I won&#8217;t go into too much detail on the &#8220;repo tasks&#8221; message in the example above. The short version is that the Pulp server has a number of checks to ensure the integrity of a repository is maintained. In this case, the copy will execute asynchronously as soon as the repository is available. This is to prevent, for instance, conflicts that could arise if the repository was currently in the process of synchronizing with the external source. A link to an article with more information on this functionality, called the Pulp Coordinator, can be found in the References section below.

# References

None of the above functionality is specific to Puppet. Pulp (the platform) is a framework that supports all of these features. Puppet is merely the latest type of content the team is flushing out support for. I&#8217;ve written about a number of these features in more detail with regard to their usage in the RPM support in Pulp. These articles, despite their RPM focus, provide more detail on the capabilities of Pulp.

  * [Unit Copy](/2012/06/15/pulp-v2-preview-unit-copy/)
  * [Coordinator](/2012/07/02/pulp-v2-preview-the-coordinator/)
  * [Writing Custom Client Extensions](/2012/06/24/pulp-v2-preview-client-extensions/)