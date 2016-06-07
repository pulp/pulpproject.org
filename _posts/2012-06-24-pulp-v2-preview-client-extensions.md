---
id: 668
title: 'Pulp v2 Preview: Client Extensions'
date: 2012-06-24T17:21:54+00:00
author: Jay Dobies
layout: post
guid: http://blog.pulpproject.org/?p=668
permalink: /2012/06/24/pulp-v2-preview-client-extensions/
categories:
  - 2.0
  - Spotlight
---
<div style="font-style:italic">
  The Pulp team is gearing up for the first community release of the v2 code base. Over the next few weeks I&#8217;ll be highlighting some of the new features in the v2 release. As with any blog entry, this is scoped to a point in time and the specific details are subject to some change before release. The overall concepts, however, should remain consistent despite any tweaks we make.
</div>

# Introduction

While the separation of content types makes for a significantly more flexible Pulp installation, it also makes the user experience trickier to handle. Without a priori knowledge of the types supported by a Pulp server, any client provided with the Pulp platform will inevitably lack the usability a custom client would provide. At the same time, it&#8217;s cumbersome to require an entirely new client customized for each content type.

The v2 client supports an extension mechanism for adding new functionality to the client. Extensions benefit from the setup and configuration performed by the client, allowing them to focus directly on the UI functionality. Extensions are normal Python code and benefit from all of the abilities of the language.

A side effect of this architecture is that it allows users to augment the client for their specific needs. These additions can be as simple as how data are rendered or as complex as scripting multiple REST API calls into a single client command.

Before I go on, let me start with some simple terminology. A &#8220;command&#8221; is the actual executable portion of a client call. Commands are organized into &#8220;sections&#8221;, where sections can be nested for more fine grained organization. Commands can accept &#8220;options&#8221; from the user to drive its execution. So given the following command:

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="none" style="font-family:monospace;">pulp-admin repo sync run --repo-id demo</pre>
      </td>
    </tr>
  </table>
</div>

The &#8220;repo&#8221; and &#8220;sync&#8221; portions represent sections; sync is a subsection of repo. The &#8220;run&#8221; component is the actual command that is invoked. The execution method for that command will be handed a dictionary mapping &#8220;repo-id&#8221; to &#8220;demo&#8221; to indicate the user&#8217;s intentions.

The remainder of this article describes a simple extension. More information will be avaliable in a separate extension development guide as v2 gets closer to release.

# CSV Support Demo

One of the simplest extension examples revolves around new display formats for data in the Pulp server. While the built in extensions attempt to format data returned from queries against the server in a reasonable fashion, inevitably there are desired views that aren&#8217;t supported out of the box. One of the driving use cases around the extension mechanism is to provide a simple way for the end user to provide such views.

## Creating the Extension

Extensions for the admin client are found under `/usr/lib/pulp/admin/extensions`. Each subdirectory within there is a Python package (the simplest way to achieve this is to add an empty file named `__init__.py` in the directory).

There is a single entry point into the extension, a file named `pulp_cli.py` (evntually a different entry point will be provided when we provide an interactive shell, but I&#8217;m getting ahead of myself). The client will load that module and call the method with the following signature:

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="python" style="font-family:monospace;"><span style="color: #ff7700;font-weight:bold;">def</span> initialize<span style="color: black;">&#40;</span>context<span style="color: black;">&#41;</span>:
   <span style="color: #808080; font-style: italic;"># Extension implementation here</span></pre>
      </td>
    </tr>
  </table>
</div>

## The Context

A copy of the client context is provided to each extension when it is initialized. The context is meant to provide the extension with everything it needs to function. For example, below are a few of the things available in the context:

  * Server Bindings &#8211; Bindings, pre-configured from the client configuration, used to make calls against the server.
  * CLI &#8211; The CLI framework itself, allowing new sections/commands to be added or existing ones to be removed, allowing user-written extensions to appear and act in the same fashion as Pulp provided functionality.
  * Prompt &#8211; A utility for reading user input and formatting data with Pulp&#8217;s common look and feel.

## Adding Commands to an Existing Section

While entirely new sections and subsections can be added to the CLI, the simplest example is adding a command to an existing section. This demo will add a &#8220;csv&#8221; command to the existing search section of commands (found at &#8220;repo units&#8221;). The command will require a single option to indicate the repository being searched. The relevant code is below:

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="python" style="font-family:monospace;">repo_section <span style="color: #66cc66;">=</span> context.<span style="color: black;">cli</span>.<span style="color: black;">find_section</span><span style="color: black;">&#40;</span><span style="color: #483d8b;">'repo'</span><span style="color: black;">&#41;</span>
units_section <span style="color: #66cc66;">=</span> repo_section.<span style="color: black;">find_subsection</span><span style="color: black;">&#40;</span><span style="color: #483d8b;">'units'</span><span style="color: black;">&#41;</span>
&nbsp;
command <span style="color: #66cc66;">=</span> units_section.<span style="color: black;">create_command</span><span style="color: black;">&#40;</span><span style="color: #483d8b;">'csv'</span><span style="color: #66cc66;">,</span> <span style="color: #483d8b;">'repo contents in csv format'</span><span style="color: #66cc66;">,</span> display_csv<span style="color: black;">&#41;</span>
command.<span style="color: black;">create_option</span><span style="color: black;">&#40;</span><span style="color: #483d8b;">'--repo-id'</span><span style="color: #66cc66;">,</span> <span style="color: #483d8b;">'repository to search'</span><span style="color: #66cc66;">,</span> required<span style="color: #66cc66;">=</span><span style="color: #008000;">True</span><span style="color: black;">&#41;</span></pre>
      </td>
    </tr>
  </table>
</div>

In the above example, the &#8220;display_csv&#8221; argument refers to the method that will be invoked when the command is run. For now, that method is stubbed out:

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="python" style="font-family:monospace;"><span style="color: #ff7700;font-weight:bold;">def</span> display_csv<span style="color: black;">&#40;</span>**kwargs<span style="color: black;">&#41;</span>:
    <span style="color: #ff7700;font-weight:bold;">pass</span></pre>
      </td>
    </tr>
  </table>
</div>

The kwargs is used to capture the user specified options; in this case, &#8220;repo-id&#8221;.

At this point, the command is installed and accessible to the client. The `pulp-admin --map` command can be used to display the full structure of sections and commands available and should reflect the csv command:

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="none" style="font-family:monospace;">...
  units: list/search for RPM-related content in a repository
    rpm:          search for RPMs in a repository
    srpm:         search for SRPMs in a repository
    drpm:         search for DRPMs in a repository
    errata:       search errata in a repository
    distribution: list distributions in a repository
    csv:          repo contents in csv format
    all:          search for all content in a repository
...</pre>
      </td>
    </tr>
  </table>
</div>

The client framework will take care of rendering an appropriate usage based on the command&#8217;s configuration (for instance, the `--repo-id` argument is described and flagged as required):

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="none" style="font-family:monospace;">$ pulp-admin repo units csv --help
Command: csv
Description: repo contents in csv format
&nbsp;
Available Arguments:
&nbsp;
  --repo-id - (required) repository to search</pre>
      </td>
    </tr>
  </table>
</div>

## Server Calls

The display_csv method will need access to the context (more specfically, the server bindings) to retrieve the server data. There are a number of ways to do this; the command base class could be subclassed and handed the context on instantiation for a cleaner approach. For simplicity in this demo, the context is stored in a global variable in the module:

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="python" style="font-family:monospace;">CONTEXT <span style="color: #66cc66;">=</span> <span style="color: #008000;">None</span>
&nbsp;
<span style="color: #ff7700;font-weight:bold;">def</span> initialize<span style="color: black;">&#40;</span>context<span style="color: black;">&#41;</span>:
    <span style="color: #ff7700;font-weight:bold;">global</span> CONTEXT
    CONTEXT <span style="color: #66cc66;">=</span> context</pre>
      </td>
    </tr>
  </table>
</div>

This call will use the unit associations API. To be honest, I&#8217;m not terribly happy with how these calls look currently, but even with some API tweaks the concept will remain the same.

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="python" style="font-family:monospace;">repo_id <span style="color: #66cc66;">=</span> kwargs<span style="color: black;">&#91;</span><span style="color: #483d8b;">'repo-id'</span><span style="color: black;">&#93;</span>
units <span style="color: #66cc66;">=</span> CONTEXT.<span style="color: black;">server</span>.<span style="color: black;">repo_search</span>.<span style="color: black;">search</span><span style="color: black;">&#40;</span>repo_id<span style="color: #66cc66;">,</span> <span style="color: black;">&#123;</span><span style="color: black;">&#125;</span><span style="color: black;">&#41;</span>.<span style="color: black;">response_body</span></pre>
      </td>
    </tr>
  </table>
</div>

The empty dictionary passed to this call is the query criteria; for this demo, all units in the repository will be returned.

## Displaying the Data

At this point, it&#8217;s up to the extension writer&#8217;s imagination. The data have been retrieved from the server and the extension is normal Python code, so anything is possible. Below is a simple CSV implementation that will contain the RPM&#8217;s name and version:

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="python" style="font-family:monospace;"><span style="color: #ff7700;font-weight:bold;">for</span> u <span style="color: #ff7700;font-weight:bold;">in</span> units:
    name <span style="color: #66cc66;">=</span> u<span style="color: black;">&#91;</span><span style="color: #483d8b;">'metadata'</span><span style="color: black;">&#93;</span><span style="color: black;">&#91;</span><span style="color: #483d8b;">'name'</span><span style="color: black;">&#93;</span>
    version <span style="color: #66cc66;">=</span> u<span style="color: black;">&#91;</span><span style="color: #483d8b;">'metadata'</span><span style="color: black;">&#93;</span><span style="color: black;">&#91;</span><span style="color: #483d8b;">'version'</span><span style="color: black;">&#93;</span>
    CONTEXT.<span style="color: black;">prompt</span>.<span style="color: black;">write</span><span style="color: black;">&#40;</span><span style="color: #483d8b;">'%s,%s'</span> % <span style="color: black;">&#40;</span>name<span style="color: #66cc66;">,</span> version<span style="color: black;">&#41;</span><span style="color: black;">&#41;</span></pre>
      </td>
    </tr>
  </table>
</div>

## Putting It All Together

Below is the full body of the `pulp_cli.py` module written during this demo:

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="python" style="font-family:monospace;">CONTEXT <span style="color: #66cc66;">=</span> <span style="color: #008000;">None</span>
&nbsp;
<span style="color: #ff7700;font-weight:bold;">def</span> initialize<span style="color: black;">&#40;</span>context<span style="color: black;">&#41;</span>:
    <span style="color: #ff7700;font-weight:bold;">global</span> CONTEXT
    CONTEXT <span style="color: #66cc66;">=</span> context
&nbsp;
    repo_section <span style="color: #66cc66;">=</span> context.<span style="color: black;">cli</span>.<span style="color: black;">find_section</span><span style="color: black;">&#40;</span><span style="color: #483d8b;">'repo'</span><span style="color: black;">&#41;</span>
    units_section <span style="color: #66cc66;">=</span> repo_section.<span style="color: black;">find_subsection</span><span style="color: black;">&#40;</span><span style="color: #483d8b;">'units'</span><span style="color: black;">&#41;</span>
&nbsp;
    command <span style="color: #66cc66;">=</span> units_section.<span style="color: black;">create_command</span><span style="color: black;">&#40;</span><span style="color: #483d8b;">'csv'</span><span style="color: #66cc66;">,</span> <span style="color: #483d8b;">'repo contents in csv format'</span><span style="color: #66cc66;">,</span> display_csv<span style="color: black;">&#41;</span>
    command.<span style="color: black;">create_option</span><span style="color: black;">&#40;</span><span style="color: #483d8b;">'--repo-id'</span><span style="color: #66cc66;">,</span> <span style="color: #483d8b;">'repository to search'</span><span style="color: #66cc66;">,</span> required<span style="color: #66cc66;">=</span><span style="color: #008000;">True</span><span style="color: black;">&#41;</span>
&nbsp;
<span style="color: #ff7700;font-weight:bold;">def</span> display_csv<span style="color: black;">&#40;</span>**kwargs<span style="color: black;">&#41;</span>:
    repo_id <span style="color: #66cc66;">=</span> kwargs<span style="color: black;">&#91;</span><span style="color: #483d8b;">'repo-id'</span><span style="color: black;">&#93;</span>
    units <span style="color: #66cc66;">=</span> CONTEXT.<span style="color: black;">server</span>.<span style="color: black;">repo_search</span>.<span style="color: black;">search</span><span style="color: black;">&#40;</span>repo_id<span style="color: #66cc66;">,</span> <span style="color: black;">&#123;</span><span style="color: black;">&#125;</span><span style="color: black;">&#41;</span>.<span style="color: black;">response_body</span>
&nbsp;
    <span style="color: #ff7700;font-weight:bold;">for</span> u <span style="color: #ff7700;font-weight:bold;">in</span> units:
        name <span style="color: #66cc66;">=</span> u<span style="color: black;">&#91;</span><span style="color: #483d8b;">'metadata'</span><span style="color: black;">&#93;</span><span style="color: black;">&#91;</span><span style="color: #483d8b;">'name'</span><span style="color: black;">&#93;</span>
        version <span style="color: #66cc66;">=</span> u<span style="color: black;">&#91;</span><span style="color: #483d8b;">'metadata'</span><span style="color: black;">&#93;</span><span style="color: black;">&#91;</span><span style="color: #483d8b;">'version'</span><span style="color: black;">&#93;</span>
        CONTEXT.<span style="color: black;">prompt</span>.<span style="color: black;">write</span><span style="color: black;">&#40;</span><span style="color: #483d8b;">'%s,%s'</span> % <span style="color: black;">&#40;</span>name<span style="color: #66cc66;">,</span> version<span style="color: black;">&#41;</span><span style="color: black;">&#41;</span></pre>
      </td>
    </tr>
  </table>
</div>

To test this, the Pulp server has a repository named &#8220;pulp&#8221; which contains the RPMs found in Pulp&#8217;s Fedora 17 v2 testing repository. Sample usage of the CSV command for this repository is as follows:

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="none" style="font-family:monospace;">$ pulp-admin repo units csv --repo-id pulp
python-pulp-common,0.0.305
gofer,0.70
python-pulp-rpm-common,0.0.305
pulp-builtins-admin-extensions,0.0.305
pulp-rpm-yumplugins,0.0.305
mod_wsgi-debuginfo,3.3
grinder,0.1.4
pulp-rpm-consumer-client,0.0.305
pulp-server,0.0.305
python-isodate,0.4.4
python-qpid,0.7.946106
python-pulp-client-lib,0.0.305
mod_wsgi,3.3
python-pulp-agent-lib,0.0.305
pulp-admin-client,0.0.305
python-oauth2,1.5.170
pulp-builtins-consumer-extensions,0.0.305
pulp-consumer-client,0.0.305
python-webpy,0.32
pulp-rpm-handlers,0.0.305
gofer-package,0.70
pulp-rpm-admin-client,0.0.305
pulp-rpm-server,0.0.305
python-okaara,1.0.18
m2crypto-debuginfo,0.21.1.pulp
pulp-rpm-consumer-extensions,0.0.305
python-gofer,0.70
python-pulp-bindings,0.0.305
pulp-rpm-admin-extensions,0.0.305
python-rhsm,0.96.4
pulp-agent,0.0.305
pulp-rpm-plugins,0.0.305
m2crypto,0.21.1.pulp
pulp-rpm-agent,0.0.305</pre>
      </td>
    </tr>
  </table>
</div>

# Summary

This demonstration is a small subset of the possibilities of the client&#8217;s extension framework. All of the functionality in the v2 client is based on the extension framework and uses the features available in the client context. Both the admin and consumer clients use this framework, allowing the consumer client experience to be customized in the same way.

By release, the goal is to provide a more complete extensions guide, including extensions accessing the client&#8217;s configuration, a description of the client&#8217;s exception middleware for handling common server errors, and API documentation for all of the server bindings. In the meantime, feel free to ping me (jdob in #pulp on Freenode) with any questions.