---
id: 20
title: 'Spotlight: Pulp Protected Repositories'
date: 2011-05-18T15:42:27+00:00
author: Todd Sanders
layout: post
guid: http://pulp.noopenblockers.com/?p=20
permalink: /2011/05/18/pulp-protected-repositories/
enclosure:
  - |
    |
        http://website-pulp.rhcloud.com/wp-content/uploads/2011/05/repoauth.ogv
        21017578
        video/ogg
        
categories:
  - Spotlight
---
Have you ever wanted to control access to your yum repositories?  Well, with Pulp you can!  This entry will explore the steps necessary to configure the Pulp Server and Consumer for [Repository Authentication](http://www.pulpproject.org/ug/UGRepoAuth.html "Repository Authentication"); including creating the necessary PKI infrastructure.

## Steps:

### 1.  Enable Repo Auth on Pulp Server

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="bash" style="font-family:monospace;"><span style="color: #666666;">$ </span><span style="color: #c20cb9; font-weight: bold;">vi</span> <span style="color: #000000; font-weight: bold;">/</span>etc<span style="color: #000000; font-weight: bold;">/</span>pulp<span style="color: #000000; font-weight: bold;">/</span>repo_auth.conf</pre>
      </td>
    </tr>
  </table>
</div>

This is essentially the on/off switch for protected repos. If this is set to false, **all** repositories on the Pulp Server will be available publicly for consumption.

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="ini" style="font-family:monospace;"><span style="color: #000066; font-weight:bold;"><span style="">&#91;</span>main<span style="">&#93;</span></span>
enabled:  true
&nbsp;
<span style="color: #000066; font-weight:bold;"><span style="">&#91;</span>repos<span style="">&#93;</span></span>
cert_location: /etc/pki/content/
global_cert_location: /etc/pki/content/
protected_repo_listing_file: /etc/pki/content/pulp-protected-repos</pre>
      </td>
    </tr>
  </table>
</div>

### 2. Create a CA Certificate

A Certificate Authority is required for issuing and validating entitlement certificates.  I will use openssl to create my CA. First the key, then the CA itself.

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="bash" style="font-family:monospace;">$ openssl genrsa <span style="color: #660033;">-out</span> caPulp.key <span style="color: #000000;">2048</span>
&nbsp;
Generating RSA private key, <span style="color: #000000;">2048</span> bit long modulus
....+++
......................+++
e is <span style="color: #000000;">65537</span> <span style="color: #7a0874; font-weight: bold;">&#40;</span>0x10001<span style="color: #7a0874; font-weight: bold;">&#41;</span></pre>
      </td>
    </tr>
  </table>
</div>

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="none" style="font-family:monospace;">$ openssl req -new -x509 -days 365 -key caPulp.key -out caPulp.crt
&nbsp;
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [XX]:US
State or Province Name (full name) []:North Carolina
Locality Name (eg, city) [Default City]:Raleigh
Organization Name (eg, company) [Default Company Ltd]:Red Hat Inc
Organizational Unit Name (eg, section) []:Engineering
Common Name (eg, your name or your server's hostname) []:Pulp Server CA
Email Address []:tsanders@redhat.com</pre>
      </td>
    </tr>
  </table>
</div>

### 3. Create the Entitlement Certificate

The Entitlement Certificate is the credential that will be used by yum clients for accessing the protected repository. Again, the first step here is to create a key for signing.

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="bash" style="font-family:monospace;">$ openssl genrsa <span style="color: #660033;">-out</span> client.key <span style="color: #000000;">2048</span>
&nbsp;
Generating RSA private key, <span style="color: #000000;">2048</span> bit long modulus
......................................+++
.....................................+++
e is <span style="color: #000000;">65537</span> <span style="color: #7a0874; font-weight: bold;">&#40;</span>0x10001<span style="color: #7a0874; font-weight: bold;">&#41;</span></pre>
      </td>
    </tr>
  </table>
</div>

Next we need to create the [extensions](https://fedorahosted.org/pulp/wiki/RepoAuth#Extensions "Extensions") file that contains the entitlements you wish to include. The most import oid is 1.3.6.1.4.1.2312.9.2.0000.1.6. This oid contains the relative path of the pulp repository that you intend to protect.

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="bash" style="font-family:monospace;"><span style="color: #666666;">$ </span><span style="color: #c20cb9; font-weight: bold;">vi</span> extensions.txt</pre>
      </td>
    </tr>
  </table>
</div>

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="ini" style="font-family:monospace;"><span style="color: #000066; font-weight:bold;"><span style="">&#91;</span>myRepo<span style="">&#93;</span></span>
<span style="color: #000099;">basicConstraints</span><span style="color: #000066; font-weight:bold;">=</span><span style="color: #660066;">CA:FALSE</span>
1.3.6.1.4.1.2312.9.2.0000.1.1<span style="color: #000066; font-weight:bold;">=</span><span style="color: #660066;">ASN1:UTF8:Pulp Production MyRepo x86_64</span>
1.3.6.1.4.1.2312.9.2.0000.1.2<span style="color: #000066; font-weight:bold;">=</span><span style="color: #660066;">ASN1:UTF8:pulp-prod-myrepo-64</span>
1.3.6.1.4.1.2312.9.2.0000.1.6<span style="color: #000066; font-weight:bold;">=</span><span style="color: #660066;">ASN1:UTF8:repos/myRepo/</span></pre>
      </td>
    </tr>
  </table>
</div>

And finally we create the entitlement certificate .csr and sign it.

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="none" style="font-family:monospace;">$ openssl req -new -key client.key -out client.csr
&nbsp;
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [XX]:US
State or Province Name (full name) []:North Carolina
Locality Name (eg, city) [Default City]:Raleigh
Organization Name (eg, company) [Default Company Ltd]:Red Hat Inc
Organizational Unit Name (eg, section) []:Engineering
Common Name (eg, your name or your server's hostname) []:Pulp Entitlement Certificate
Email Address []:tsanders@redhat.com
&nbsp;
Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:
An optional company name []:</pre>
      </td>
    </tr>
  </table>
</div>

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="none" style="font-family:monospace;">$ openssl x509 -req -days 365 -CA caPulp.crt -CAkey caPulp.key -CAcreateserial -extfile extensions.txt 
  -extensions myRepo -in client.csr -out client.crt
&nbsp;
Signature ok
subject=/C=US/ST=North Carolina/L=Raleigh/O=Red Hat Inc
/OU=Engineering/CN=Pulp Entitlement Certificate/emailAddress=tsanders@redhat.com
Getting CA Private Key</pre>
      </td>
    </tr>
  </table>
</div>

**Notes:** 
  
1. The name in the extensions.txt file [ ] is what is passed to the -extensions argument when signing the request. This allows you to pick which batch of entitlements to include.
  
2. The -CAcreateserial option will create a serial number file to allow openssl to manage the serial number incrementing for each successive signing. In this case the file will be caPulp.srl. Once this file exists, use the -CAserial option to supply this file when signing.

### 4. Create the Repository

We are now ready to create our repository using the CA and Entitlement Certificate that we created above.

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="bash" style="font-family:monospace;">$ pulp-admin repo create <span style="color: #660033;">--id</span>=myRepo <span style="color: #660033;">--name</span>=myRepo --consumer_ca=caPulp.crt  --consumer_cert=client.crt  
   --consumer_key=client.key
&nbsp;
Successfully created repository <span style="color: #7a0874; font-weight: bold;">&#91;</span> myRepo <span style="color: #7a0874; font-weight: bold;">&#93;</span></pre>
      </td>
    </tr>
  </table>
</div>

### 5. Upload Content

In our case I am choosing to upload an rpm that I created locally, however, this could have also easily as been a feeded repository mirroring content from a remote location.

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="none" style="font-family:monospace;">$ pulp-admin content upload --repoid=myRepo --nosig pulp-demo-1.0-1.fc14.x86_64.rpm
&nbsp;
* Starting Content Upload operation. See /var/log/pulp/client.log for more verbose output
&nbsp;
* Performing Content Uploads to Pulp server
&nbsp;
* Performing Repo Associations 
&nbsp;
* Content Upload complete.</pre>
      </td>
    </tr>
  </table>
</div>

### 6. Create Consumer and Bind to Repository

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="none" style="font-family:monospace;">$ pulp-client -u admin -p admin consumer create --id=myConsumer
&nbsp;
Successfully created consumer [ myConsumer ]</pre>
      </td>
    </tr>
  </table>
</div>

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="none" style="font-family:monospace;">$ pulp-client consumer bind --repoid=myRepo
&nbsp;
Successfully subscribed consumer [myConsumer] to repo [myRepo]</pre>
      </td>
    </tr>
  </table>
</div>

### 7. Manually update /etc/yum.repos.d/pulp.repo on Consumer

Currently, Pulp doesn&#8217;t handle automatically setting the appropriate PKI attributes in the yum.repos.d configuration during bind. This is coming in a future sprint, so for now we&#8217;ll make these mods by hand. Without this added configuration, as you&#8217;ll see in the demo below, yum will not be able to access the repository.

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="bash" style="font-family:monospace;"><span style="color: #666666;">$ </span><span style="color: #c20cb9; font-weight: bold;">vi</span> <span style="color: #000000; font-weight: bold;">/</span>etc<span style="color: #000000; font-weight: bold;">/</span>yum.repos.d<span style="color: #000000; font-weight: bold;">/</span>pulp.repo</pre>
      </td>
    </tr>
  </table>
</div>

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="ini" style="font-family:monospace;">#
# Pulp Repositories
# Managed by Pulp client
#
&nbsp;
<span style="color: #000066; font-weight:bold;"><span style="">&#91;</span>myRepo<span style="">&#93;</span></span>
<span style="color: #000099;">name</span> <span style="color: #000066; font-weight:bold;">=</span><span style="color: #660066;"> myRepo</span>
<span style="color: #000099;">enabled</span> <span style="color: #000066; font-weight:bold;">=</span><span style="color: #660066;"> 1</span>
<span style="color: #000099;">sslverify</span> <span style="color: #000066; font-weight:bold;">=</span><span style="color: #660066;"> 0</span>
<span style="color: #000099;">gpgcheck</span> <span style="color: #000066; font-weight:bold;">=</span><span style="color: #660066;"> 0</span>
<span style="color: #000099;">baseurl</span> <span style="color: #000066; font-weight:bold;">=</span><span style="color: #660066;"> https://localhost/pulp/repos/myRepo</span></pre>
      </td>
    </tr>
  </table>
</div>

Next you need to copy the ca, entitlement certificate and key (from steps 2 & 3) to the /etc/pki/content directory on the consumer. Then add the following three attributes to the [myRepo] section:

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="ini" style="font-family:monospace;"><span style="color: #000099;">sslclientkey</span><span style="color: #000066; font-weight:bold;">=</span><span style="color: #660066;">/etc/pki/content/client.key</span>
<span style="color: #000099;">sslclientcert</span><span style="color: #000066; font-weight:bold;">=</span><span style="color: #660066;">/etc/pki/content/client.crt</span>
<span style="color: #000099;">sslcacert</span><span style="color: #000066; font-weight:bold;">=</span><span style="color: #660066;">/etc/pki/content/caPulp.crt</span></pre>
      </td>
    </tr>
  </table>
</div>

That&#8217;s it! You should now be able to yum install packages from the authenticated repository on your pulp server.

## Demo

A simple screen-cast walking you through steps 4-7 from above.

<embed width="640" height="480" src="http://website-pulp.rhcloud.com/wp-content/uploads/2011/05/repoauth.ogv" autostart="false">
</embed>


  
[Open in New Window](http://website-pulp.rhcloud.com/wp-content/uploads/2011/05/repoauth.ogv)

&nbsp;