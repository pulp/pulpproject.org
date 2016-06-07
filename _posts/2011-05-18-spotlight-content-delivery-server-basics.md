---
id: 727
title: 'Spotlight: Content Delivery Server Basics'
date: 2011-05-18T15:23:30+00:00
author: Jay Dobies
layout: post
guid: http://pulp.noopenblockers.com/?p=80
permalink: /2011/05/18/spotlight-content-delivery-server-basics/
enclosure:
  - |
    |
        http://website-pulp.rhcloud.com/wp-content/uploads/2011/05/cds-spotlight.ogv
        7604658
        video/ogg
        
categories:
  - Spotlight
---
One of the more recent features in Pulp is the ability to use the Pulp server to push repositories out to be hosted on external servers. These Content Delivery Servers (CDS) can be used to distribute content across firewalls and geographic locations, as well as providing an added layer of security by selectively deploying only a subset of the Pulp server&#8217;s content to a specific instance.

## Installation and Configuration

A server is configured to run as a CDS by first installing the Pulp CDS package and its dependencies:

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="bash" style="font-family:monospace;"><span style="color: #7a0874; font-weight: bold;">&#91;</span>root<span style="color: #000000; font-weight: bold;">@</span>localhost<span style="color: #7a0874; font-weight: bold;">&#93;</span> <span style="color: #c20cb9; font-weight: bold;">yum install</span> pulp-cds</pre>
      </td>
    </tr>
  </table>
</div>

If not already installed, httpd will be installed as part of the installation process. The virtual host for the CDS is also installed through the `/etc/httpd/conf.d/pulp-cds.conf` file.

The CDS will need to be able to resolve the hostname of the Pulp server in order to be able to download content from it. This will typically be done through DNS, but in development environments this often means needing to edit `/etc/hosts` to add an entry for the Pulp server.

Additionally, the CDS will have to be configured to use the messaging broker on the Pulp server so it can be manipulated from the server itself. This is done through the `/etc/pulp/cds.conf` file:

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="ini" style="font-family:monospace;"><span style="color: #000066; font-weight:bold;"><span style="">&#91;</span>server<span style="">&#93;</span></span>
<span style="color: #000099;">host</span> <span style="color: #000066; font-weight:bold;">=</span><span style="color: #660066;"> pulp.example.com</span></pre>
      </td>
    </tr>
  </table>
</div>

Like the Pulp server itself, the CDS uses Apache to host its repositories. The typical steps to configure the Apache instance with an SSL certificate should be taken at this point.

Once the configuration changes have been made, the CDS processes are started through the init script:

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="bash" style="font-family:monospace;"><span style="color: #7a0874; font-weight: bold;">&#91;</span>root<span style="color: #000000; font-weight: bold;">@</span>localhost ~<span style="color: #7a0874; font-weight: bold;">&#93;</span> service pulp-cds start
Starting goferd                                            <span style="color: #7a0874; font-weight: bold;">&#91;</span>  OK  <span style="color: #7a0874; font-weight: bold;">&#93;</span>
Starting httpd:                                            <span style="color: #7a0874; font-weight: bold;">&#91;</span>  OK  <span style="color: #7a0874; font-weight: bold;">&#93;</span></pre>
      </td>
    </tr>
  </table>
</div>

## Usage

Once the CDS server is running, it must be registered to a Pulp server. A CDS may only be registered to one Pulp server at a given time. Once registered, the CDS will only accept commands from the Pulp server that it is currently registered to. When a CDS is unregistered from a Pulp server, it is once again open to be registered by a different Pulp server.

### Registration

Registration is done through the `pulp-admin cds` commands. The registration command requires the hostname of the CDS as identification. Keep in mind, however, that the Pulp server itself does not need to be able to resolve the CDS hostname to an IP address. Rather, the hostname is used to determine the unique message bus ID for that CDS. A display name can also be specified using the `--name` argument.

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="bash" style="font-family:monospace;"><span style="color: #7a0874; font-weight: bold;">&#91;</span>root<span style="color: #000000; font-weight: bold;">@</span>localhost ~<span style="color: #7a0874; font-weight: bold;">&#93;</span> pulp-admin cds register <span style="color: #660033;">--hostname</span> cds.example.com
Successfully registered CDS <span style="color: #7a0874; font-weight: bold;">&#91;</span>cds.example.com<span style="color: #7a0874; font-weight: bold;">&#93;</span></pre>
      </td>
    </tr>
  </table>
</div>

Registered CDS instances can be displayed through `pulp-admin cds list` command:

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="bash" style="font-family:monospace;"><span style="color: #7a0874; font-weight: bold;">&#91;</span>root<span style="color: #000000; font-weight: bold;">@</span>localhost ~<span style="color: #7a0874; font-weight: bold;">&#93;</span> pulp-admin cds list
+------------------------------------------+
                CDS Instances
+------------------------------------------+
&nbsp;
Name                	cds.example.com           
Hostname            	cds.example.com           
Description         	None                     
Repos               	None                     
Last Sync           	Never                    
Status:
   Responding       	Yes                      
   Last Heartbeat   	<span style="color: #000000;">2011</span>-05-<span style="color: #000000;">12</span> <span style="color: #000000;">17</span>:07:<span style="color: #000000;">21.834959</span>+00:00</pre>
      </td>
    </tr>
  </table>
</div>

### Repository Association

A registered CDS is neat, but not very useful. Repositories are _associated_ with a CDS to customize the content they serve. Repositories that are protected on the Pulp server using repository authentication will also be protected on any CDS instance they are associated with (more on that in a future blog).

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="bash" style="font-family:monospace;"><span style="color: #7a0874; font-weight: bold;">&#91;</span>root<span style="color: #000000; font-weight: bold;">@</span>localhost ~<span style="color: #7a0874; font-weight: bold;">&#93;</span> pulp-admin cds associate_repo <span style="color: #660033;">--hostname</span> cds.example.com <span style="color: #660033;">--repoid</span> demo
Successfully associated CDS <span style="color: #7a0874; font-weight: bold;">&#91;</span>cds.example.com<span style="color: #7a0874; font-weight: bold;">&#93;</span> with repo <span style="color: #7a0874; font-weight: bold;">&#91;</span>demo<span style="color: #7a0874; font-weight: bold;">&#93;</span></pre>
      </td>
    </tr>
  </table>
</div>

### CDS Synchronization

Repository association configures the server&#8217;s knowledge of which repositories belong on which CDS instances. In order to get the bits to the CDS instance, it must be synchronized. The `pulp-admin cds sync` command is used to trigger this process and the `pulp-admin cds status` command is used to check its progress (in the future, this will be enhanced to more closely resemble the abilities for monitoring repo syncs).

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="bash" style="font-family:monospace;"><span style="color: #7a0874; font-weight: bold;">&#91;</span>root<span style="color: #000000; font-weight: bold;">@</span>localhost ~<span style="color: #7a0874; font-weight: bold;">&#93;</span> pulp-admin cds <span style="color: #c20cb9; font-weight: bold;">sync</span> <span style="color: #660033;">--hostname</span> cds.example.com
Sync <span style="color: #000000; font-weight: bold;">for</span> CDS <span style="color: #7a0874; font-weight: bold;">&#91;</span>rhino.marvel.u<span style="color: #7a0874; font-weight: bold;">&#93;</span> started
Use <span style="color: #ff0000;">"cds status"</span> to check on the progress
&nbsp;
<span style="color: #7a0874; font-weight: bold;">&#91;</span>root<span style="color: #000000; font-weight: bold;">@</span>localhost ~<span style="color: #7a0874; font-weight: bold;">&#93;</span> pulp-admin cds status <span style="color: #660033;">--hostname</span> cds.example.com
+------------------------------------------+
                 CDS Status
+------------------------------------------+
&nbsp;
Name                	cds.example.com           
Hostname            	cds.example.com           
Description         	None                     
Repos               	demo                     
Last Sync           	<span style="color: #000000;">2011</span>-05-<span style="color: #000000;">12</span> <span style="color: #000000;">13</span>:<span style="color: #000000;">20</span>:<span style="color: #000000;">35</span>-04:00
Status:
   Responding       	Yes                      
   Last Heartbeat   	<span style="color: #000000;">2011</span>-05-<span style="color: #000000;">12</span> <span style="color: #000000;">17</span>:<span style="color: #000000;">23</span>:<span style="color: #000000;">54.328154</span>+00:00</pre>
      </td>
    </tr>
  </table>
</div>

Repository contents are stored in the `/var/lib/pulp-cds` directory on the CDS instance:

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="bash" style="font-family:monospace;"><span style="color: #7a0874; font-weight: bold;">&#91;</span>root<span style="color: #000000; font-weight: bold;">@</span>localhost<span style="color: #7a0874; font-weight: bold;">&#93;</span> ll <span style="color: #660033;">-R</span> <span style="color: #000000; font-weight: bold;">/</span>var<span style="color: #000000; font-weight: bold;">/</span>lib<span style="color: #000000; font-weight: bold;">/</span>pulp-cds<span style="color: #000000; font-weight: bold;">/</span>
<span style="color: #000000; font-weight: bold;">/</span>var<span style="color: #000000; font-weight: bold;">/</span>lib<span style="color: #000000; font-weight: bold;">/</span>pulp-cds<span style="color: #000000; font-weight: bold;">/</span>:
total <span style="color: #000000;">8</span>
-rw-r--r--. <span style="color: #000000;">1</span> root root    <span style="color: #000000;">5</span> May <span style="color: #000000;">12</span> <span style="color: #000000;">13</span>:<span style="color: #000000;">23</span> cds_repo_list
drwxr-xr-x. <span style="color: #000000;">3</span> root root <span style="color: #000000;">4096</span> May <span style="color: #000000;">12</span> <span style="color: #000000;">13</span>:<span style="color: #000000;">23</span> demo
&nbsp;
<span style="color: #000000; font-weight: bold;">/</span>var<span style="color: #000000; font-weight: bold;">/</span>lib<span style="color: #000000; font-weight: bold;">/</span>pulp-cds<span style="color: #000000; font-weight: bold;">/</span>demo:
total <span style="color: #000000;">8</span>
-rw-r--r--. <span style="color: #000000;">1</span> root root <span style="color: #000000;">2184</span> May <span style="color: #000000;">12</span> <span style="color: #000000;">13</span>:<span style="color: #000000;">23</span> pulp-demo-<span style="color: #000000;">1.0</span>-<span style="color: #000000;">1</span>.fc14.x86_64.rpm
drwxr-xr-x. <span style="color: #000000;">2</span> root root <span style="color: #000000;">4096</span> May <span style="color: #000000;">12</span> <span style="color: #000000;">13</span>:<span style="color: #000000;">23</span> repodata
&nbsp;
<span style="color: #000000; font-weight: bold;">/</span>var<span style="color: #000000; font-weight: bold;">/</span>lib<span style="color: #000000; font-weight: bold;">/</span>pulp-cds<span style="color: #000000; font-weight: bold;">/</span>demo<span style="color: #000000; font-weight: bold;">/</span>repodata:
total <span style="color: #000000;">28</span>
-rw-r--r--. <span style="color: #000000;">1</span> root root  <span style="color: #000000;">745</span> May <span style="color: #000000;">12</span> <span style="color: #000000;">13</span>:<span style="color: #000000;">23</span> filelists.sqlite.bz2
-rw-r--r--. <span style="color: #000000;">1</span> root root  <span style="color: #000000;">308</span> May <span style="color: #000000;">12</span> <span style="color: #000000;">13</span>:<span style="color: #000000;">23</span> filelists.xml.gz
-rw-r--r--. <span style="color: #000000;">1</span> root root  <span style="color: #000000;">736</span> May <span style="color: #000000;">12</span> <span style="color: #000000;">13</span>:<span style="color: #000000;">23</span> other.sqlite.bz2
-rw-r--r--. <span style="color: #000000;">1</span> root root  <span style="color: #000000;">357</span> May <span style="color: #000000;">12</span> <span style="color: #000000;">13</span>:<span style="color: #000000;">23</span> other.xml.gz
-rw-r--r--. <span style="color: #000000;">1</span> root root <span style="color: #000000;">1721</span> May <span style="color: #000000;">12</span> <span style="color: #000000;">13</span>:<span style="color: #000000;">23</span> primary.sqlite.bz2
-rw-r--r--. <span style="color: #000000;">1</span> root root  <span style="color: #000000;">662</span> May <span style="color: #000000;">12</span> <span style="color: #000000;">13</span>:<span style="color: #000000;">23</span> primary.xml.gz
-rw-r--r--. <span style="color: #000000;">1</span> root root <span style="color: #000000;">2685</span> May <span style="color: #000000;">12</span> <span style="color: #000000;">13</span>:<span style="color: #000000;">23</span> repomd.xml</pre>
      </td>
    </tr>
  </table>
</div>

Repositories on a CDS are hosted at the same URL as for the Pulp server itself:

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="bash" style="font-family:monospace;"><span style="color: #7a0874; font-weight: bold;">&#91;</span>root<span style="color: #000000; font-weight: bold;">@</span>localhost<span style="color: #7a0874; font-weight: bold;">&#93;</span> <span style="color: #c20cb9; font-weight: bold;">wget</span> <span style="color: #660033;">--no-check-certificate</span> https:<span style="color: #000000; font-weight: bold;">//</span>cds.example.com<span style="color: #000000; font-weight: bold;">/</span>pulp<span style="color: #000000; font-weight: bold;">/</span>repos<span style="color: #000000; font-weight: bold;">/</span>demo<span style="color: #000000; font-weight: bold;">/</span>pulp-demo-<span style="color: #000000;">1.0</span>-<span style="color: #000000;">1</span>.fc14.x86_64.rpm
HTTP request sent, awaiting response... <span style="color: #000000;">200</span> OK
Length: <span style="color: #000000;">2184</span> <span style="color: #7a0874; font-weight: bold;">&#40;</span>2.1K<span style="color: #7a0874; font-weight: bold;">&#41;</span> <span style="color: #7a0874; font-weight: bold;">&#91;</span>text<span style="color: #000000; font-weight: bold;">/</span>plain<span style="color: #7a0874; font-weight: bold;">&#93;</span>
Saving to: “pulp-demo-<span style="color: #000000;">1.0</span>-<span style="color: #000000;">1</span>.fc14.x86_64.rpm”
&nbsp;
<span style="color: #000000;">100</span><span style="color: #000000; font-weight: bold;">%</span><span style="color: #7a0874; font-weight: bold;">&#91;</span>==============================================================<span style="color: #000000; font-weight: bold;">&gt;</span><span style="color: #7a0874; font-weight: bold;">&#93;</span> <span style="color: #000000;">2</span>,<span style="color: #000000;">184</span>       --.-K<span style="color: #000000; font-weight: bold;">/</span>s   <span style="color: #000000; font-weight: bold;">in</span> 0s      
&nbsp;
<span style="color: #000000;">2011</span>-05-<span style="color: #000000;">12</span> <span style="color: #000000;">13</span>:<span style="color: #000000;">28</span>:<span style="color: #000000;">21</span> <span style="color: #7a0874; font-weight: bold;">&#40;</span><span style="color: #000000;">24.3</span> MB<span style="color: #000000; font-weight: bold;">/</span>s<span style="color: #7a0874; font-weight: bold;">&#41;</span> - “pulp-demo-<span style="color: #000000;">1.0</span>-<span style="color: #000000;">1</span>.fc14.x86_64.rpm” saved <span style="color: #7a0874; font-weight: bold;">&#91;</span><span style="color: #000000;">2184</span><span style="color: #000000; font-weight: bold;">/</span><span style="color: #000000;">2184</span><span style="color: #7a0874; font-weight: bold;">&#93;</span></pre>
      </td>
    </tr>
  </table>
</div>

### Binding and Distribution

When a consumer is bound to a repository, the Pulp server takes into account any CDS instances that host the repository in question. A list of all locations at which the repository is hosted is returned to the consumer and used as a mirror list for the repository. The Pulp server keeps track of these distributions and will organize these mirror lists to distribute consumer load across all hosts of a given repository. In the future, this distribution can be enhanced to take into account other factors than simple round-robin distribution and make the determination of the &#8220;best&#8221; CDS for a given repository for each consumer.

## Demo

<embed src="http://website-pulp.rhcloud.com/wp-content/uploads/2011/05/cds-spotlight.ogv" autostart="false" width="640" height="480" />


  
[Open in New Window](http://website-pulp.rhcloud.com/wp-content/uploads/2011/05/cds-spotlight.ogv)

## Conclusion

In the middle of writing this, my boss pinged me to tell me he set up a demo environment using a Pulp server in Boston and a CDS instance in Tel Aviv. Geographic issues are just one of the issues addressed by the CDS functionality. CDS instances can be configured with only a subset of repositories to better control which content is available to which clients.

The next big CDS related feature on the horizon is the introduction of CDS groups. The implications extend beyond the simple administration benefits of being able to affect more than a single CDS at a time. The intention is that CDS instances will be able to use other CDS instances in their same group for load balancing and fail over scenarios.

In the meantime, the basic story for Content Delivery Servers is implemented and available in Pulp. This story includes the ability to register CDS instances, associate and synchronize repositories on a per-CDS basis, and passing this information to consumers when they are bound to balance repository accesses across all possible sources.