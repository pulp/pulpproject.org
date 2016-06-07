---
id: 1273
title: 'Pulp 2.8.3 Released: Security &#038; Bug fixes'
date: 2016-05-17T19:03:53+00:00
author: Sean Myers
layout: post
guid: http://www.pulpproject.org/?p=1273
permalink: /2016/05/17/pulp-2-8-3-released-security-bug-fixes/
tags: release
categories:
  - Uncategorized
---
Pulp has been updated, along with the Puppet (pulp\_puppet) and RPM (pulp\_rpm) plugins.

This release also includes updates for OSTree plugin (pulp_ostree) version
  
1.1.1, the Docker plugin (pulp_docker) version 2.0.1, and the Python plugin
  
(pulp_python) version 1.1.1.

The release is available in the pulp beta repository for 2.8:
  
 <https://repos.fedorapeople.org/repos/pulp/pulp/stable/2.8/>

Migrations will need to be run for this release. See the Upgrade Instructions
  
below for more details.

## Security Issues Addressed

**CVE-2016-3111** (Low Impact):
  
pulp.spec generates its RSA keys for message signing insecurely
  
 <https://pulp.plan.io/issues/1837>

**CVE-2016-3112** (Moderate Impact):
  
Pulp consumer private keys are world-readable
  
 <https://pulp.plan.io/issues/1834>

**CVE-2016-3107** (Moderate Impact):
  
Node certificate containing private key stored in world-readable file
  
 <https://pulp.plan.io/issues/1833>

**CVE-2016-3108** (Moderate Impact):
  
Insecure temporary file used when generating certificate for Pulp Nodes
  
 <https://pulp.plan.io/issues/1830>

**CVE-2016-3106** (Low Impact):
  
Insecure creation of temporary directory when generating new CA key
  
 <https://pulp.plan.io/issues/1827>

Additionally, **CVE-2013-7450** was announced during this release cycle, even
  
though it was fixed in Pulp 2.3.0. Users who have upgraded from Pulp < 2.3.0
  
may still be vulnerable, action may be required (see below).
  
 <https://bugzilla.redhat.com/show_bug.cgi?id=1003326>

See the upgrade instructions below for more information on addressing these
  
vulnerabilities.

## Known Issues

Changes to the squid package in fedora 22 are causing selinux denials to prevent squid
  
from starting on systems using pulp&#8217;s lazy download features. At this time, all other
  
platforms appear to be working normally: only fedora 22 is affected.

This issue is being tracked in our tracker. Links to upstream issues and workarounds
  
can be found there:

<https://pulp.plan.io/issues/1904>

## Issues Addressed

Docker Support

  * 1818 Add migration &#8211; content units to standard storage path.

Nectar

  * 1820 Fix checking for config.proxy_username

OSTree Support

  * 1106 relative_path should be checked for url collision

Pulp

  * 1576 content type mongo id searches not working
  * 1764 SELinux denial on Celery attempting to read resolv.conf
  * 1771 requests or urllib3 can&#8217;t read a file which causes Nectar to fail mysteriously
  * 1801 Pulp celery\_beat and resource\_manager are running, but logs say they are not running
  * 1802 Pulp 2.8 client no longer supports sha1 RPM checksum type
  * 1809 python 2.6 incompatibility during set_importer
  * 1747 Import upload task has unexpected/missing information on error
  * 1784 regression: &#8220;pulp-admin rpm repo search&#8221; with filters does not work as expected
  * 1834 CVE-2016-3112: Pulp consumer private keys are world-readable
  * 1837 CVE-2016-3111: pulp.spec generates its RSA keys for message signing insecurely
  * 1791 After upgrading from 2.7.1 to pulp 2.8.0 getting 403 error&#8217;s on all my Pulp repo&#8217;s.
  * 1794 A Pulp unit test is failing to find a certificate to be valid
  * 1824 iso repo publish fails for file in subdirectories
  * 1827 CVE-2016-3106: Insecure creation of temporary directory when generating new CA key
  * 1830 CVE-2016-3108: Insecure temporary file used when generating certificate for Pulp Nodes
  * 1833 CVE-2016-3107: Node certificate containing private key stored in world-readable file
  * 1601 Migrate /var/lib/pulp/content to new 2.8 storage paths.
  * 1815 Create a common 2.8 storage path migration to be used by plugins

Puppet Support

  * 1780 PLP0000: Update failed (The dotted field &#8216;thomasmckay-rsync-0.4.1-thomasmckay&#8217;
  * 1817 Add migration &#8211; content units to standard storage path.

Python Support

  * 1855 Upload broken
  * 1819 Add migration &#8211; content units to standard storage path.

RPM Support

  * 1869 Resynchronizing rhel repos seems to be failing after upgrade
  * 1768 Unable to sync RHEL 5 repositories with a distribution
  * 1792 recursive and depsolving unit copy results in PulpExecutionException
  * 1843 Pulp publishes invalid PULP_DISTRIBUTION.xml metadata
  * 1778 Switching a repository to immediate from on_demand doesn&#8217;t download its packages
  * 1828 pulp doesn&#8217;t sync reference title correctly from errata
  * 1835 export fails when units are not downloaded
  * 1782 None in generated XML for unit with no &#8216;reboot_suggested&#8217;
  * 1808 exporting a sufficiently large repo with &#8216;on_demand&#8217; policy results in BSON error
  * 1812 Comps.xml upload succeeds but units are not associated to the repo.
  * 1813 Handle duplicate key error in comps.xml upload
  * 1856 publishing kickstart repo fails on EL6
  * 1816 Add migration &#8211; content units to standard storage path.

Details on these issues can be found [in Redmine](https://pulp.plan.io/issues?c[]=subject&f[]=status_id&f[]=cf_4&f[]=&group_by=project&op[cf_4]=%3D&op[status_id]=%3D&per_page=50&set_filter=1&utf8=%E2%9C%93&v[cf_4][]=2.8.3&v[status_id][]=1&v[status_id][]=2&v[status_id][]=3&v[status_id][]=4&v[status_id][]=5&v[status_id][]=6).

## Upgrade instructions

Some of the CVEs require user interaction to remedy. Begin by upgrading to
  
Pulp 2.8.3, and running migrations:

<pre style="padding-left: 30px;">$ sudo systemctl stop httpd pulp_workers pulp_resource_manager pulp_celerybeat goferd
$ sudo yum upgrade
$ sudo -u apache pulp-manage-db
$ sudo systemctl start httpd pulp_workers pulp_resource_manager pulp_celerybeat goferd</pre>

### CVE-2016-3112 (Part I)

The client certificate for consumers
  
(/etc/pki/pulp/consumer/consumer-cert.pem) was installed world-readable. This
  
issue has been fixed for new certificates issued to consumers, but upgrading
  
to 2.8.3 does not modify the permissions of old certificates. It is
  
recommended that users regenerate the certificates by unregistering and
  
re-registering all consumers. However, the consumers cannot be re-registered
  
until CVE-2013-7450, CVE-2016-3095, CVE-2016-3106, and CVE-2016-3111 have
  
been addressed below. Thus, start by unregistering each of your consumers (we
  
will return to this CVE later to re-register them):

<pre style="padding-left: 30px;">$ sudo pulp-consumer unregister</pre>

### CVE-2013-7450, CVE-2016-3095, and CVE-2016-3106

There are two reasons that you may wish to regenerate Pulp&#8217;s internal
  
certificate authority key and certificate. First, if your Pulp installation
  
started off as a version lower than 2.3.0 and you are still using the default
  
CA certificate and key that was distributed with those versions of Pulp, then
  
you are still vulnerable to CVE-2013-7450 and it is crucial that you generate
  
a new unique CA.

Additionally, CVE-2016-3095 and CVE-2016-3106 made it possible for local
  
attackers to read the CA key during generation (which happens during the
  
initial installation of Pulp or any time an admin ran
  
pulp-gen-ca-certificate). If you are concerned that a local user may have
  
read that CA key during the brief window that it was visible it is
  
recommended that you regenerate the key and cert.

To regenerate the certificate, you should remove the old one and then you may
  
use the provided utility:

<pre style="padding-left: 30px;"># First remove the old files so that the new files get the correct SELinux context.
$ sudo rm /etc/pki/pulp/ca.*
$ sudo pulp-gen-ca-certificate</pre>

If you choose not to perform the CA regeneration, you may wish to apply the
  
correct SELinux type to your existing CA files as versions of Pulp < 2.8.3
  
generated this file with an incorrect SELinux type. You don&#8217;t need to do this
  
if you removed the old file and regenerated it with pulp-gen-ca-certificate.
  
You can run restorecon recursively on the /etc/pki/pulp folder to fix the
  
SELinux label on your existing CA certificate:

<pre style="padding-left: 30px;"># You only need to do this if you didn't regenerate the CA above.
$ sudo restorecon -R /etc/pki/pulp</pre>

### CVE-2016-3107 and CVE-2016-3108

For Nodes users, the /etc/pki/pulp/nodes/node.crt file was installed
  
world-readable. Users are recommended to remove this file and regenerate it
  
by running pulp-gen-nodes-certificate:

<pre style="padding-left: 30px;"># It is important to remove the file so that the new file has the correct permissions.
$ sudo rm /etc/pki/pulp/nodes/node.crt
$ sudo pulp-gen-nodes-certificate</pre>

### CVE-2016-3111

Both the RSA key pair for the Pulp server and RSA key pair for each Pulp
  
consumer was generated during installation in an insecure directory. This
  
vulnerability allowed a local attacker to read the private key portion of the
  
key pair. These keys are used for message authentication between the Pulp
  
server and the Pulp consumers. If you are concerned that a local attacker was
  
able to read these keys, you can regenerate them. We do not ship a script to
  
perform this, but the process is straight-forward. For the Pulp server, do
  
the following as root:

<pre style="padding-left: 30px;">$ cd /etc/pki/pulp/
$ rm rsa.key rsa_pub.key
$ umask 077
$ openssl genrsa -out rsa.key # should be at least 2048
$ openssl rsa -in rsa.key -pubout &gt; rsa_pub.key
$ chgrp apache rsa.key rsa_pub.key
$ chmod 640 rsa.key # Apache must be able to read the private key
$ chmod 644 rsa_pub.key # The public key is world-readable as it is served via Apache</pre>

The Pulp consumer key is similar:

<pre style="padding-left: 30px;">$ cd /etc/pki/pulp/consumer/
$ rm rsa.key rsa_pub.key
$ umask 077
$ openssl genrsa -out rsa.key # should be at least 2048
$ openssl rsa -in rsa.key -pubout &gt; rsa_pub.key</pre>

### CVE-2016-3112 (Part II)

Now that we have regenerated the server&#8217;s CA certificate, we can finish
  
re-registering each consumer to Pulp:

<pre style="padding-left: 30px;">$ sudo pulp-consumer -u register --consumer-id=</pre>

### Restart

Pulp services are now ready to be restarted again to pick up the new
  
certificates. For systemd users:

<pre style="padding-left: 30px;">$ sudo systemctl restart httpd pulp_workers pulp_resource_manager pulp_celerybeat goferd</pre>

### Troubleshooting

Regenerating the CA certificate will invalidate all client certificates that
  
were issued by the old CA. All users will need to login to Pulp again to
  
obtain a new client certificate. If you forget a step, you may see one of the
  
following error messages:

&#8220;pulp.server.managers.auth.authentication:ERROR: Auth certificate with CN
  
[admin:admin:57155b83e779896cb3d634a4] is signed by a foreign CA&#8221; (or
  
similar) in the server log can indicate that httpd has not been restarted
  
since the CA was replaced.
  
&#8220;The specified user does not have permission to execute the given
  
command&#8221; from pulp-admin can mean that the user has not logged in since the
  
new CA was present, or that httpd has not been restarted since the
  
certificate was replaced. More generally, this error message can also mean
  
that the user is not authorized to perform the given action.
  
&#8220;An error occurred attempting to contact the server. More information may
  
be found using the -v flag.&#8221; may be output by pulp-admin if you have
  
restarted httpd but have not logged in again to get a new CA certificate. If
  
you provide that -v flag and see &#8220;ConnectionException: (None, &#8216;tlsv1 alert
  
decrypt error&#8217;, None)&#8221;, this is likely the issue.