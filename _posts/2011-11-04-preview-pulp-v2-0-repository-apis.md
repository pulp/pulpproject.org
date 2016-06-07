---
id: 492
title: 'Preview: Pulp v2.0 Repository APIs'
date: 2011-11-04T20:29:33+00:00
author: Jay Dobies
layout: post
guid: http://blog.pulpproject.org/?p=492
permalink: /2011/11/04/preview-pulp-v2-0-repository-apis/
categories:
  - 2.0
  - Uncategorized
---
[Last post](/2011/11/02/preview-creating-a-pulp-plugin/) talked about the process of adding a v2.0 plugin to a Pulp server. This post will provide an overview of the currently available APIs. These APIs were developed with the focus of a sprint demo, so there are some holes in functionality. There is a story this sprint to revisit these and flush them out (along with correcting some conventions), but for now I wanted to provide a quick list for those interested in playing around with the plugin model.

The APIs can be found after the jump.
  
<!--more-->

# Repository Create, Delete, Retrieve, and Configure

## Create Repository

<table class="api">
  <tr>
    <td>
      <strong>Method</strong>
    </td>
    
    <td>
      POST
    </td>
  </tr>
  
  <tr>
    <td>
      <strong>Path</strong>
    </td>
    
    <td>
      <code>/v2/repositories/[id]/</code>
    </td>
  </tr>
  
  <tr>
    <td>
      <strong>Parameters</strong>
    </td>
    
    <td>
      <ul>
        <li>
          <strong>id</strong> &#8211; uniquely identifies the repository (specified in the URL)
        </li>
        <li>
          <strong>display_name</strong> &#8211; (optional) user-friendly name of the repository
        </li>
        <li>
          <strong>description</strong> &#8211; (optional) user-friendly description of the repository
        </li>
      </ul>
    </td>
  </tr>
  
  <tr>
    <td>
      <strong>Return</strong>
    </td>
    
    <td>
      True if the repository was created
    </td>
  </tr>
  
  <tr>
    <td>
      <strong>Sample Request Body</strong>
    </td>
    
    <td>
      <div class="wp_syntax">
        <table>
          <tr>
            <td class="code">
              <pre class="json" style="font-family:monospace;">{"display_name": "Demo 1 Repository"}</pre>
            </td>
          </tr>
        </table>
      </div>
    </td>
  </tr>
  
  <tr>
    <td>
      <strong>Sample Response</strong>
    </td>
    
    <td>
      <div class="wp_syntax">
        <table>
          <tr>
            <td class="code">
              <pre class="json" style="font-family:monospace;">True</pre>
            </td>
          </tr>
        </table>
      </div>
    </td>
  </tr>
</table>

## Delete Repository

<table class="api">
  <tr>
    <td>
      <strong>Method</strong>
    </td>
    
    <td>
      DELETE
    </td>
  </tr>
  
  <tr>
    <td>
      <strong>Path</strong>
    </td>
    
    <td>
      <code>/v2/repositories/[id]/</code>
    </td>
  </tr>
  
  <tr>
    <td>
      <strong>Parameters</strong>
    </td>
    
    <td>
      <ul>
        <li>
          <strong>id</strong> &#8211; identifies the repository (specified in the URL)
        </li>
      </ul>
    </td>
  </tr>
  
  <tr>
    <td>
      <strong>Return</strong>
    </td>
    
    <td>
      True if the repository was deleted
    </td>
  </tr>
  
  <tr>
    <td>
      <strong>Sample Request Body</strong>
    </td>
    
    <td>
      None
    </td>
  </tr>
  
  <tr>
    <td>
      <strong>Sample Response</strong>
    </td>
    
    <td>
      <div class="wp_syntax">
        <table>
          <tr>
            <td class="code">
              <pre class="json" style="font-family:monospace;">True</pre>
            </td>
          </tr>
        </table>
      </div>
    </td>
  </tr>
</table>

## List Repositories

<table class="api">
  <tr>
    <td>
      <strong>Method</strong>
    </td>
    
    <td>
      GET
    </td>
  </tr>
  
  <tr>
    <td>
      <strong>Path</strong>
    </td>
    
    <td>
      <code>/v2/repositories/</code>
    </td>
  </tr>
  
  <tr>
    <td>
      <strong>Parameters</strong>
    </td>
    
    <td>
      None
    </td>
  </tr>
  
  <tr>
    <td>
      <strong>Return</strong>
    </td>
    
    <td>
      List of repositories and their details
    </td>
  </tr>
  
  <tr>
    <td>
      <strong>Sample Request Body</strong>
    </td>
    
    <td>
      None
    </td>
  </tr>
  
  <tr>
    <td>
      <strong>Sample Response</strong>
    </td>
    
    <td>
      <div class="wp_syntax">
        <table>
          <tr>
            <td class="code">
              <pre class="json" style="font-family:monospace;">[{'content_unit_count': 0,
  'description': None,
  'display_name': 'Demo 1 Repository',
  'id': 'demo-1-repo',
  'notes': {}}]</pre>
            </td>
          </tr>
        </table>
      </div>
    </td>
  </tr>
</table>

## Add an Importer to a Repository

<table class="api">
  <tr>
    <td>
      <strong>Method</strong>
    </td>
    
    <td>
      POST
    </td>
  </tr>
  
  <tr>
    <td>
      <strong>Path</strong>
    </td>
    
    <td>
      <code>/v2/repositories/[id]/importers/</code>
    </td>
  </tr>
  
  <tr>
    <td>
      <strong>Parameters</strong>
    </td>
    
    <td>
      <ul>
        <li>
          <strong>id</strong> &#8211; identifies the repository (specified in the URL)
        </li>
        <li>
          <strong>importer_type_id</strong> &#8211; identifies the type of importer being added; corresponds to the ID of the importer plugin
        </li>
        <li>
          <strong>importer_config</strong> &#8211; dictionary of configuration values to pass to the importer that will be used only for the given repository; the contents will vary depending on the type of importer being added
        </li>
      </ul>
    </td>
  </tr>
  
  <tr>
    <td>
      <strong>Return</strong>
    </td>
    
    <td>
      True if the importer was added
    </td>
  </tr>
  
  <tr>
    <td>
      <strong>Sample Request Body</strong>
    </td>
    
    <td>
      <div class="wp_syntax">
        <table>
          <tr>
            <td class="code">
              <pre class="json" style="font-family:monospace;">{"importer_type_id": "local-rpm",
 "importer_config": {"rpm_dir": "/pulp-gc-demo/rpms"}
}</pre>
            </td>
          </tr>
        </table>
      </div>
    </td>
  </tr>
  
  <tr>
    <td>
      <strong>Sample Response</strong>
    </td>
    
    <td>
      <div class="wp_syntax">
        <table>
          <tr>
            <td class="code">
              <pre class="json" style="font-family:monospace;">True</pre>
            </td>
          </tr>
        </table>
      </div>
    </td>
  </tr>
</table>

## Add an Distributor to a Repository

<table class="api">
  <tr>
    <td>
      <strong>Method</strong>
    </td>
    
    <td>
      POST
    </td>
  </tr>
  
  <tr>
    <td>
      <strong>Path</strong>
    </td>
    
    <td>
      <code>/v2/repositories/[id]/distributors/</code>
    </td>
  </tr>
  
  <tr>
    <td>
      <strong>Parameters</strong>
    </td>
    
    <td>
      <ul>
        <li>
          <strong>id</strong> &#8211; identifies the repository (specified in the URL)
        </li>
        <li>
          <strong>distributor_type_id</strong> &#8211; identifies the type of distributor being added; corresponds to the ID of the distributor plugin
        </li>
        <li>
          <strong>distributor_config</strong> &#8211; dictionary of configuration values to pass to the distributor that will be used only for the given repository; the contents will vary depending on the type of distributor being added
        </li>
        <li>
          <strong>auto_publish</strong> &#8211; boolean; if true, this distributor will be published at the end of every successful sync; if false, the only way to publish this distributor is through an explicit call
        </li>
        <li>
          <strong>distributor_id</strong> &#8211; (optional) since more than one distributor may be added to a repository, this is used to uniquely identify the distributor being added; if not specified Pulp will automatically generate one
        </li>
      </ul>
    </td>
  </tr>
  
  <tr>
    <td>
      <strong>Return</strong>
    </td>
    
    <td>
      True if the distributor was added
    </td>
  </tr>
  
  <tr>
    <td>
      <strong>Sample Request Body</strong>
    </td>
    
    <td>
      <div class="wp_syntax">
        <table>
          <tr>
            <td class="code">
              <pre class="json" style="font-family:monospace;">{"distributor_id": "http",
 "distributor_type_id": "http-rpm",
 "distributor_config": {},
 "auto_publish": true
}</pre>
            </td>
          </tr>
        </table>
      </div>
    </td>
  </tr>
  
  <tr>
    <td>
      <strong>Sample Response</strong>
    </td>
    
    <td>
      <div class="wp_syntax">
        <table>
          <tr>
            <td class="code">
              <pre class="json" style="font-family:monospace;">True</pre>
            </td>
          </tr>
        </table>
      </div>
    </td>
  </tr>
</table>

# Repository Actions

## Synchronize Repository

<table class="api">
  <tr>
    <td>
      <strong>Method</strong>
    </td>
    
    <td>
      POST
    </td>
  </tr>
  
  <tr>
    <td>
      <strong>Path</strong>
    </td>
    
    <td>
      <code>/v2/repositories/[id]/sync/</code>
    </td>
  </tr>
  
  <tr>
    <td>
      <strong>Parameters</strong>
    </td>
    
    <td>
      <ul>
        <li>
          <strong>id</strong> &#8211; identifies the repository (specified in the URL)
        </li>
        <li>
          <strong>override_config</strong> &#8211; (optional) if specified, values within will override the importer_config specified when it was added to the repository; these values will take effect <em>only</em> for this sync operation
        </li>
      </ul>
    </td>
  </tr>
  
  <tr>
    <td>
      <strong>Return</strong>
    </td>
    
    <td>
      None
    </td>
  </tr>
  
  <tr>
    <td>
      <strong>Sample Request Body</strong>
    </td>
    
    <td>
      <div class="wp_syntax">
        <table>
          <tr>
            <td class="code">
              <pre class="json" style="font-family:monospace;">{"override_config": {"rpm_dir": "/pulp-gc-demo/override"}}</pre>
            </td>
          </tr>
        </table>
      </div>
    </td>
  </tr>
  
  <tr>
    <td>
      <strong>Sample Response</strong>
    </td>
    
    <td>
      None
    </td>
  </tr>
</table>

## Publish Repository

<table class="api">
  <tr>
    <td>
      <strong>Method</strong>
    </td>
    
    <td>
      POST
    </td>
  </tr>
  
  <tr>
    <td>
      <strong>Path</strong>
    </td>
    
    <td>
      <code>/v2/repositories/[id]/publish/[distributor_id]/</code>
    </td>
  </tr>
  
  <tr>
    <td>
      <strong>Parameters</strong>
    </td>
    
    <td>
      <ul>
        <li>
          <strong>id</strong> &#8211; identifies the repository (specified in the URL)
        </li>
        <li>
          <strong>distributor_id</strong> &#8211; identifies which distributor assigned to the repository should be published
        </li>
        <li>
          <strong>override_config</strong> &#8211; (optional) if specified, values within will override the distributor_config specified when it was added to the repository; these values will take effect <em>only</em> for this publish operation
        </li>
      </ul>
    </td>
  </tr>
  
  <tr>
    <td>
      <strong>Return</strong>
    </td>
    
    <td>
      None
    </td>
  </tr>
  
  <tr>
    <td>
      <strong>Sample Request Body</strong>
    </td>
    
    <td>
      <div class="wp_syntax">
        <table>
          <tr>
            <td class="code">
              <pre class="json" style="font-family:monospace;">{"override_config": {"publish_option_1": "override_value"}}</pre>
            </td>
          </tr>
        </table>
      </div>
    </td>
  </tr>
  
  <tr>
    <td>
      <strong>Sample Response</strong>
    </td>
    
    <td>
      <div class="wp_syntax">
        <table>
          <tr>
            <td class="code">
              <pre class="json" style="font-family:monospace;">True</pre>
            </td>
          </tr>
        </table>
      </div>
    </td>
  </tr>
</table>