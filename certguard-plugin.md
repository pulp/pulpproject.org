---
title: Certguard Plugin
sidebar: home_sidebar
permalink: /certguard-plugin/
toc: false
---

The Certguard plugin provides a X.509 capable `ContentGuard` that requires clients to submit a certificate proving their entitlement to content before receiving content from Pulp.

Any client presenting an X.509 or Red Hat Subscription Management-based certificate at request time will be authorized as long as the client cert is not expired and is signed by the Certificate Authority Certificate stored on the Certguard when it was created. The client presents the certificate using TLS, which proves that the client not only has the certificate but also possesses the key for the certificate.

If you use Pulpcore versions 3.3.z or higher, you can use the existing installer to install or upgrade this plugin. For more information, see [Example Playbook for Installing Plugins](https://docs.pulpproject.org/pulp_installer/quickstart/#example-playbook-for-installing-plugins).

The Certguard plugin is available on [PyPi](https://pypi.org/project/pulp-certguard/).


For more information about how to set up and use the Certguard plugin, see the [Pulp Certguard documentation](https://docs.pulpproject.org/pulp_certguard).

To view the Pulp Certguard source, see the [Pulp Certguard GitHub](https://github.com/pulp/pulp-certguard) page.

To report an issue or track future progress, see the [Certguard plugin issue tracker](https://github.com/pulp/pulp-certguard/issues).
