---
id: 473
title: 'Preview: Creating a Pulp Plugin'
date: 2011-11-02T15:46:53+00:00
author: Jay Dobies
layout: post
guid: http://blog.pulpproject.org/?p=473
permalink: /2011/11/02/preview-creating-a-pulp-plugin/
categories:
  - 2.0
  - Uncategorized
---
Buried in Community Release 17 is the first working implementation of the new plugin mechanism for Pulp 2.0. I didn&#8217;t make a big deal about it since, well, it&#8217;s still really, really early. I figured I&#8217;d wait to start to highlight it until we got some more work done.

That changed with the introduction of the <a href="http://www.boredomandlaziness.org/search/label/pulpdist" target="new">pulpdist</a> project which is looking to leverage Pulp for synchronizing and publishing mirrors within Red Hat. It&#8217;s awesome to have such a flushed out use case for Pulp that&#8217;s going to be written at the same time we&#8217;re designing the features and should prove really helpful in getting an outside contributors view on our approach.

Since I needed to write up some information for the pulpdist team on how to get started writing a plugin, I figured I&#8217;d do it here in case others are interested in playing with the new architecture as it takes shape. Again, it&#8217;s still really early in terms of code and API stability, so realize things are likely to still be tweaked. Feedback is appreciated if you do start to poke around.

This post will cover the basics in terms of creating and installing Pulp plugins. A later post will go into more details on the plugin and conduit APIs with some examples of the sorts of things I&#8217;ve done as demo plugins.

# Type Definitions

The first step is to make sure _type definitions_ exist for the types of content you are looking to manage with Pulp. A type definition informs Pulp of the details of how to store and manage _content units_ of that type. This metadata can be user-focused (display name, description) or used by Pulp to optimize storage of a content unit&#8217;s metadata (search indexes). A type definition may also reference other types to establish an aggregate relationship. For instance, the errata type will reference the RPM type to express that an errata may contain one or more RPM units.

Type definitions are written in _type descriptors_ which are located in `/var/lib/pulp/plugins/types`. A type descriptor may contain more than one definition; the intention is that related definitions will be defined in the same file for ease of managing them. Type descriptors are JSON documents.

Let&#8217;s start with an example and break it down. To be clear, this is a slimmed down version of what RPM and errata definitions would look like, so don&#8217;t get caught up in what may be missing.

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="json" style="font-family:monospace;">{"types": [
    {"id" : "rpm",
     "display_name" : "RPM",
     "description" : "RPM Package",
     "unique_indexes" : ["name", "version"],
     "search_indexes" : [
         ["name", "epoch", "version", "release", "architecture"]
      ]},
    {"id" : "errata",
     "display_name" : "Errata",
     "description" : "Errata",
     "unique_indexes" : "id",
     "search_indexes" : [
         "title",
         "type",
         "severity"
      ],
     "child_types" : ["rpm"]}
   ]}</pre>
      </td>
    </tr>
  </table>
</div>

The above example contains two type definitions. When the Pulp server is started, it will parse the type definitions and ensure syntax validity and integrity (e.g. does a type reference a child type that doesn&#8217;t exist). Pulp will alter its database accordingly to be able to store units of these types.

The fields in a type definition are as follows:

  * **id** &#8211; Programmatically identifies the content type. This must be unique across all type definitions.
  * **display_name** &#8211; User-friendly name for the type.
  * **description** &#8211; User-friendly details on what the type represents.
  * **unique_indexes** &#8211; Identifies the set of fields that dictate uniqueness for units of this type. This may be either a single field (e.g. &#8220;name&#8221;) or a compound index for uniqueness when paired together (e.g. &#8220;name&#8221;, &#8220;version&#8221;). Pulp will configure the database with unique indexes on these fields and will enforce uniqueness accordingly when handling units of this type.
  * **search_indexes** &#8211; A list of additional indexes to create for units of this type. This allows the type definition to optimize Pulp&#8217;s storage of units for expected queries. Again, entries in here may either be a single list or a list of indexes for a compound index.
  * **child_types** &#8211; _(optional)_ List of type IDs that may be referenced from units of this type. Pulp uses this information to create the necessary links in the database to track this relationship.

The `unique_indexes` and `search_indexes` fields map pretty closely to the same concepts in MongoDB. More information on how compound indexes are handled in MongoDB can be found in their documentation and should be consulted when defining these indexes in a type definition.

If a type definition cannot be parsed or loaded, the Pulp application will fail to start. Information on what went wrong can be found in `/var/log/httpd/error_log` rather than the Pulp logs.

# Plugins

Writing importers and distributors follow a similar model; the most notable difference is the use of different APIs depending on what you want to do. For the purpose of this post, I&#8217;ll describe how to write an importer and add a note at the end as to the differences in writing a distributor.

## Importers

Each importer is a separate directory located in `/var/lib/pulp/plugins/importers`. This directory must be a Python package (simply drop `__init__.py` in the directory). Pulp will attempt to load the plugin by looking for a file in that directory named `importer.py`. The Pulp hook into the plugin must be located in that module; all other code can be organized into separate modules and packages.

Pulp will look in `importer.py` for any classes that subclass the `pulp.server.content.plugins.importer.Importer` class. The `Importer` class defines the API the plugin may implement (I still have to go back in and clean up the docs in there).

At very least, subclasses must implement the class method `metadata` to allow Pulp to retrieve the required metadata describing the importer. This method must return a dictionary containing the following information:

  * **id** &#8211; Programmatically identifies the importer. This must be unique across all importers.
  * **display_name** &#8211; User-friendly identification of the importer.
  * **types** &#8211; List of type IDs to inform Pulp of the types of content handled by the plugin. Pulp application boot will fail if a plugin references a type ID that does not exist.

Below is the metadata implementation for example importer:

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="python" style="font-family:monospace;"><span style="color: #ff7700;font-weight:bold;">class</span> LocalRpmImporter<span style="color: black;">&#40;</span>Importer<span style="color: black;">&#41;</span>:
&nbsp;
    <span style="color: #66cc66;">@</span><span style="color: #008000;">classmethod</span>
    <span style="color: #ff7700;font-weight:bold;">def</span> metadata<span style="color: black;">&#40;</span>cls<span style="color: black;">&#41;</span>:
        metadata <span style="color: #66cc66;">=</span> <span style="color: black;">&#123;</span>
            <span style="color: #483d8b;">'id'</span>           : <span style="color: #483d8b;">'local-rpm'</span><span style="color: #66cc66;">,</span>
            <span style="color: #483d8b;">'display_name'</span> : <span style="color: #483d8b;">'Local RPM Importer'</span><span style="color: #66cc66;">,</span>
            <span style="color: #483d8b;">'types'</span>        : <span style="color: black;">&#91;</span><span style="color: #483d8b;">'rpm'</span><span style="color: black;">&#93;</span><span style="color: #66cc66;">,</span>
        <span style="color: black;">&#125;</span>
        <span style="color: #ff7700;font-weight:bold;">return</span> metadata</pre>
      </td>
    </tr>
  </table>
</div>

At that point, the only thing left to do is override the base class methods to provide the actual functionality of the plugin. Again, I&#8217;ll cover those APIs more thoroughly in a future post. For now, the documentation in the plugin base classes themselves should be consulted for more information. These classes may be viewed <a href="http://git.fedorahosted.org/git/?p=pulp.git;a=tree;f=src/pulp/server/content/plugins;h=93b7ed68ef0d99493c3b78eb1e1a4d891fd19569;hb=HEAD" target="new">in our git repository here.</a>

## Distributors

Distributor plugins follow the same model as importers. A distributor must also implement the `metadata` class method and the same fields are required. The following differences are probably self-explanatory, but I&#8217;ll list them for completeness:

  * Distributors are installed to </code>/var/lib/pulp/plugins/distributors</code>.
  * The distributor package must include a file named `distributor.py`.
  * Distributors must subclass `pulp.server.content.plugins.distributor.Distributor`.

One last note, since each plugin directory is itself a Python package that will be loaded into the classpath, an importer and distributor may not share the exact same plugin package name even though they are stored in separate directories.

## Summary

I tend to get long-winded when I write, so for reference here is the short version of the above wall of text:

  * Create type definitions 
      * Installed to `/var/lib/pulp/plugins/types/` (file name and extension do not matter).
      * JSON documents; see explanation above for required fields.
  * Create importer plugins 
      * Each plugin is a python package under `/var/lib/pulp/plugins/importers/`.
      * Each plugin module must contain a file named `importer.py`.
      * The `importer.py` module must define a class that subclasses `pulp.server.content.plugins.importer.Importer`.
      * The importer subclass must implement the class method `metadata` (required fields can be found above).
      * The plugin implementation is provided by overriding the methods in the Importer base class.
  * Create distributor plugins 
      * Each plugin is a python package under `/var/lib/pulp/plugins/distributors/`.
      * Each plugin module must contain a file named `distributor.py`.
      * The `distributor.py` module must define a class that subclasses `pulp.server.content.plugins.distributor.Distributor`.
      * The distributor subclass must implement the class method `metadata` (required fields can be found above).
      * The plugin implementation is provided by overriding the methods in the Distributor base class.

# Next Steps

I&#8217;ll be flushing out the documentation in the plugin base classes over the next few days. When I do, I&#8217;ll do another post that outlines the sorts of hooks provided to plugins.

Currently, none of the functionality for creating v2.0 repositories is exposed through the CLI. REST APIs exist for repository creation and configuration, triggering synchronize and publish operations, and querying for content units in Pulp. Unfortunately, none of that is documented yet, so I&#8217;ll be posting on that in the next few days as well.