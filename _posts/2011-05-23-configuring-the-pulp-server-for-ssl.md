---
id: 253
title: Configuring the Pulp Server for SSL
date: 2011-05-23T12:55:24+00:00
author: Jay Dobies
layout: post
guid: http://blog.pulpproject.org/?p=253
permalink: /2011/05/23/configuring-the-pulp-server-for-ssl/
categories:
  - Uncategorized
---
Almost everyone has used SSL at one point. What&#8217;s less common is the need to set it up on a server of your own. Since Pulp uses HTTPS for both its API access and repository hosting, most Pulp admin users quickly find themselves with the need to configure their Apache instance beyond the defaults established at installation. This article describes the process of doing so.

I tried to write this in a way that if you know what you&#8217;re doing, you can just skim the article for the pre-formatted sections as command reference. If you&#8217;re new to the concepts, the text should guide you through some high level knowledge to get you oriented.

## What&#8217;s the Issue?

On the surface, it looks like there is nothing to do. When Apache and mod_ssl are installed, HTTPS &#8220;just works.&#8221;

The problem revolves around what SSL really is. An SSL certificate is a pairing of a user&#8217;s (or server&#8217;s) public key with identity information. Clients to that server use the public key in the SSL certificate to encrypt data. The server uses its private key to decrypt that data.

What&#8217;s to stop another server from claiming someone&#8217;s identity? When I get a certificate that says &#8220;Hi, I&#8217;m the server for redhat.com&#8221;, how do I know it&#8217;s telling the truth?

That&#8217;s where the certificate authority (or CA) comes in. A CA _signs_ an SSL certificate to indicate that it has validated the identity of the public key&#8217;s owner. Some CAs, such as VeriSign or GeoTrust, are considered trusted. So if VeriSign signed an SSL certificate, the client can be confident that the certificate is legitimate.

Ever go to a website and get a pop-up from your browser that says the site is using an untrusted SSL certificate? That means it wasn&#8217;t signed by one of the trusted CA certificates included by default in the browser. For example, in Firefox you can view these defaults through Preferences -> Advanced -> View Certificates -> Authorities tab. Notice how you have VeriSign and GeoTrust authorities without having to have installed them manually.

For development purposes, it often doesn&#8217;t make sense to go to an established CA to get every SSL certificate signed. Nor is it economical; I don&#8217;t need to pay for certificates for the half-dozen guests I use for development purposes. In those cases, we can generate our own public key infrastructure (PKI), with its own CA certificates used to sign our SSL certificates.

One last piece of information that often trips people up. Remember that the SSL certificate pairs a public key with an identity. Part of that identity is expressed through the _common name_ (or CN) of the certificate. The CN value of the certificate must match the hostname of the server on which it is used. In other words, if I attempt to connect to foo.com but the SSL certificate it presents to me says it is bar.com, I probably shouldn&#8217;t trust it.

## Let&#8217;s Make Some Certificates

Armed with a high-level understanding of the pieces, it&#8217;s actually pretty simple to create them.

### CA Certificate

We start with making a fake CA. Again, in a production environment we would skip this step and use an established CA. But for development and testing purposes, simulating our own CA is a pretty simple solution.

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="bash" style="font-family:monospace;">$ openssl genrsa <span style="color: #660033;">-out</span> ca.key <span style="color: #000000;">2048</span>
$ openssl req <span style="color: #660033;">-new</span> <span style="color: #660033;">-x509</span> <span style="color: #660033;">-days</span> <span style="color: #000000;">365</span> <span style="color: #660033;">-key</span> ca.key <span style="color: #660033;">-out</span> ca.crt</pre>
      </td>
    </tr>
  </table>
</div>

Let&#8217;s break that down&#8230;

The first command generates a private key. This key is used when signing the SSL certificate that we&#8217;ll generate later. This should be kept secret; if someone gets access to your CA certificate and private key, they can sign certificates on your behalf. That&#8217;s bad.

The second command generates the CA certificate itself. This command will prompt you for a number of different things. The only thing to be careful of is that the CN **cannot** be the same as the server for any SSL certificates you will create. This isn&#8217;t a hard requirement so much as a convention; many clients will assume that if the CA and SSL certificate CNs are the same, it&#8217;s probably someone signing it on their own and shouldn&#8217;t be trusted. Otherwise, the CN can be anything you want, for example &#8220;pulp-ca&#8221;.

When that&#8217;s finished, we have our fake certificate authority. The ca.crt will be used both to sign SSL certificates and by clients to verify that a trusted CA has signed them. Keep in mind that multiple SSL certificates can be signed by the same CA. So given an environment of one Pulp server and multiple CDS instances, all of their SSL certificates can be signed by the same CA.

### SSL Certificates

Generating an SSL certificate is a very similar process, just with an extra step in the middle. The process is as follows:

  * Generate a private key. Ideally this should be unique for each server but isn&#8217;t a technical requirement.
  * Generate a _certificate signing request_. This request uses the public key for the above private key and pairs identity information with it. The term &#8220;signing request&#8221; is important; it&#8217;s not a real certificate yet, just a request for one.
  * Give the signing request to the CA to sign. The CA will do some magic to ensure that you are who you said you are in the signing request. If the CA is happy with that, they will use their CA certificate and private key to sign the request, producing a full-blown certificate.
  * Install the SSL certificate given to you by the CA and the private key (remember to keep the private key) into the web server.

For this example, we&#8217;re going to play the part of the CA, using the CA certificate and private key generated above to do the signing.

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="bash" style="font-family:monospace;">$ openssl genrsa <span style="color: #660033;">-out</span> server.key <span style="color: #000000;">2048</span>
$ openssl req <span style="color: #660033;">-new</span> <span style="color: #660033;">-key</span> server.key <span style="color: #660033;">-out</span> server.csr
$ openssl x509 <span style="color: #660033;">-req</span> <span style="color: #660033;">-days</span> <span style="color: #000000;">365</span> <span style="color: #660033;">-CA</span> ca.crt <span style="color: #660033;">-CAkey</span> ca.key -set_serial 01 <span style="color: #660033;">-in</span> server.csr <span style="color: #660033;">-out</span> server.crt</pre>
      </td>
    </tr>
  </table>
</div>

The first step generates the private key. It&#8217;s possible to encrypt this with a password, however doing so will require you to specify that password each time it&#8217;s used (read: each time you start the web server). That&#8217;s cumbersome, especially in a development environment, so we&#8217;ll leave it unprotected.

The second is the signing request generation. It will prompt you for many of the same fields as before. The extremely important part is that the CN specified matches the server on which the certificate will be installed. Bad mojo happens if they don&#8217;t.

The last step is the process of actually signing the request by the CA. That will produce the certificate itself (server.crt) which we&#8217;ll install in Apache in a bit. One thing to keep in mind is that the `-set_serial` argument is used to uniquely identify each certificate signed by the CA. Be careful to use a different number for each certificate. There are other ways of managing serial numbers that I won&#8217;t get into here.

These three steps need to be repeated for each server that&#8217;s going to be using SSL, supplying the correct CN each time.

## Configuring Apache

Technically speaking, we only need the server&#8217;s SSL certificate and private key to configure Apache. Before we get to that, let me explain why you can&#8217;t just throw away your CA certificate.

When a server hands a client an SSL certificate, the client needs to make sure it&#8217;s legit. The client can use a CA certificate to verify that the SSL certificate was signed by it. In other words, if I were to try to impersonate redhat.com, the client can verify that my SSL certificate is fake since it wasn&#8217;t signed by the CA they expected it to be signed by. Most clients bundle in the trusted CA certificates by default, but since we&#8217;re using a custom CA, we&#8217;ll likely need to provide it to our client.

When mod_ssl is installed, it generates a self-signed SSL certificate using &#8220;localhost&#8221; as the CN. We want to replace that with our custom created SSL certificates by editing `/etc/httpd/conf.d/ssl.conf`

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="none" style="font-family:monospace;">SSLCertificateFile /etc/pki/example/server.crt
SSLCertificateKeyFile /etc/pki/example/server.key</pre>
      </td>
    </tr>
  </table>
</div>

In the above example, I&#8217;ve taken the server SSL certificate and private key we generated and put them in `/etc/pki/example`. When Apache is restarted, it will use those files for its end of the SSL handshake.

## Wrapping Up

At this point, our Apache server is configured to use a CA-signed SSL certificate whose CN is scoped to the machine&#8217;s hostname instead of simply localhost. Depending on the client being used, our CA certificate may need to be made available. An example of how to do this in a yum repo definition can be found at the end of the [Pulp Protected Repositories Spotlight](/2011/05/18/pulp-protected-repositories/) elsewhere on this blog.