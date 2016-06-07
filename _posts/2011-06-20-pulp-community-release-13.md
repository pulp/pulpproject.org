---
id: 351
title: Pulp Community Release 13
date: 2011-06-20T14:17:02+00:00
author: Jay Dobies
layout: post
guid: http://blog.pulpproject.org/?p=351
permalink: /2011/06/20/pulp-community-release-13/
categories:
  - Releases
---
<img src="http://website-pulp.rhcloud.com/wp-content/uploads/2011/06/cr-13-2.png" alt="" title="cr-13-2" width="442" height="161" class="alignnone size-full wp-image-352" srcset="http://www.pulpproject.org/wp-content/uploads/2011/06/cr-13-2-300x109.png 300w, http://www.pulpproject.org/wp-content/uploads/2011/06/cr-13-2.png 442w" sizes="(max-width: 442px) 100vw, 442px" />

## Installation

Installation instructions can be found [in the Pulp User Guide](http://pulpproject.org/ug/UGInstallation.html#installation) at the [Pulp project web site.](http://www.pulpproject.org)

## Enhancements

  * [Errata enhancements](https://fedorahosted.org/pulp/wiki/PulpCli#ErrataActions) including: 
      * Ability to search errata using various parameters such as id, type, and repository association.
      * Ability to list all errata including orphaned errata filtered by type.
  * Tasks are now persisted to the database instead of simply being stored in memory. 
      * Unscheduled tasks will now resume if interrupted by a Pulp crash or restart.
      * Task history for repository and CDS syncs available.
      * Administrative transparency into the task subsystem can be found in the `pulp-admin`. To enable support for this command, edit `/etc/pulp/client.conf` and set `[admin] expose_task_command = True`. </ul> 
      * Repository discovery enhancements from the previous sprint&#8217;s implementation: 
          * Support added for discovering authenticated repositories.
          * Support added for discovering mirror lists and redirected URLs.
          * Increased process visibility through incremental feedback.
      * A yum plugin has been added to automatically update consumer profile information in the Pulp server after all yum transactions.
      * [CDS Clusters](http://pulpproject.org/ug/UGCDS.html#clusters) have been added, which will automatically maintain consistency of repository associations across all CDS instances in the cluster, allowing for one-step provisioning of CDS instances to support high load scenarios.
      * Consumers now have support for protected repositories. When a consumer certificate is specified for a repository in Pulp, it is downloaded to the consumer and referenced in the repository definition.
      * Fixed 22 bugs, accessible through [bugzilla.](https://bugzilla.redhat.com/buglist.cgi?target_release=Sprint%2024&#038;query_format=advanced&#038;product=Pulp&#038;classification=Other)
    ## Upgrade Notes
    
      * Run `pulp-migrate` to upgrade to the latest database changes.
      * Fedora 13 is no longer supported.
      * The x.509 private key and certificates were consolidated to be stored as a single, combined file. Any x.509 `--key` CLI options are optional so long as the file referenced in `--cert` option contains both the private key and certificate. After upgrading Pulp, existing certificates stored on the file system will no longer be valid unless the private key and certificates are combined. This has the following effects on upgraded systems: 
          * Repositories that have been defined with feed or consumer certificates will need to be updated with the certificate information again. Even if the optional `--key` support is used, this step needs to occur again so the Pulp server will correctly combine and store the certificate and private key on the server.
          * Consumer certificates may be manually combined (alternatively, the consumer can be deleted and re-registered). To manually upgrade a consumer, combine the `key.pem` and `cert.pem` files in the `/etc/pki/consumer` directory into a single file named `cert.pem`.
          * Users will need to regenerate their user certificate by logging out and then logging back in (`pulp-admin auth logout` and `pulp-admin auth login -u [user] -p [password]` respectively.
    ## Fedora 15 Support
    
    The Fedora 15 build requires mongo 1.7.5 due to an issue in version 1.8 that can cause the mongo shell to hang. Users installing or upgrading pulp on an F15 instance that already has mongo 1.8 installed will need to manually downgrade to mongo 1.7.5.
    
    ## Build Versions
    
      * Pulp: 0.191
      * Grinder: 0.103
      * Gofer: 0.41