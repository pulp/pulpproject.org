---
title: Under the hood
sidebar: home_sidebar
permalink: /under-the-hood/
toc: false
---

Let's take a look at the different components that Pulp is composed of.

This section aims to provide a beginner's level overview.
For more detailed descriptions of each component, see the [architecture](https://docs.pulpproject.org/pulpcore/components.html#) documentation.

![](/images/pulp-workflow-architecture-ha/pulp-architecture-components.png)

All of the components in this diagram can be run on one or more hosts, depending on your requirements.
The `/pulp/api/v3/status` API is used to check that all services are up and available.

### Pulpcore API

This application serves the REST API that your HTTPS and Pulp CLI requests interact with at `/pulp/api/v3/`.

### Pulpcore Content Application

The Pulpcore content application serves the binary data via `/pulp/content/filepath/content.rpm`.
For example, this application fulfils  `yum/apt/pip/podman/ansible collection` requests to the Pulp server from clients.

### Pulp Workers

The Pulp workers are part of the tasking system.
Because syncing large amounts of content takes time, a lot of the work in Pulp runs longer than is suitable for webservers.
You can deploy as many workers as you need.

Together, the Pulpcore API, the Pulpcore Content Application, and the Pulp Workers/Tasking system iare known as **Pulpcore**.

### Postgresql

Pulp requires a Postgresql database.
The tasking system runs on top of Postgresql.
For high availability setups, you'll need clustered Postgres.  

### Redis

Redis helps to increase the speed at which the content app caches information about requests.
This cache makes answering subsequent content-app requests easier.
However, it's optional, you are free not to use it.
If it fails, Pulp will continue to work.  

### Webserver

The webserver, such as Apache or Nginx, functions to take inbound requests to the Pulpcore API service and the Pulpcore content application service.
You can deploy multiple or clustered webservers to suit your requirements.
You can configure load balancers like HA Proxy or DNS load balancing between the webserver and the Pulpcore API service, or between the webserver and the Pulpcore content application service.  

### Storage backend

Pulp requires a storage backend, such as a filesystem like NFS, Samba or cloud storage, to read and write data that is then shared between Pulpcore processes.
The default mount point is `/var/lib/pulp`.
The Pulpcore services will then be configured to run on top of this.
