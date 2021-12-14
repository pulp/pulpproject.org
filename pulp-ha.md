---
title: Pulp & High Availability
sidebar: home_sidebar
permalink: /pulp-ha/
toc: false
---

Pulp's architecture is designed to offer scalability and high availability.
You can scale Pulp's architecture in whatever way suits your needs.

With Pulp, the more you increase your availability, you also increase your capability.
The more Pulpcore API processes you deploy, the more API requests you can serve.
The more Pulpcore content applications you deploy, the more binary data requests you can serve.
The more workers you start, the higher the tasking (syncing, publishing) throughput you deployment can handle.

You are free to, for example, deploy all of the components onto separate servers, deploy multiple Pulpcore API, content serving application, Pulpcore worker services on one server, as long as they can communicate with one another, there are no limits.

### Clustering

For high availability deployments, you will need to configure a clustered Postgresql and storage backend.
This is mounted by default on `/var/lib/pulp`.
This is beyond the scope of the Pulp documentation.
You can also configure as many webservers as you need.

### Load balancing

You can connect load balancers like HA Proxy or DNS load balancing between the webserver and the Pulpcore API and/or Pulpcore content serving applications.
With load balancing, you must use the Pulp status API to ensure that all the services that you deploy are online.
For a high availability deployment, consider using a load balancer in front of your webservers to help distribute client requests efficiently.

### High Availability Examples

The Ansible installer documentation has example playbooks for four different deployment scenarios:

* [The default “all-in-one” Pulp server that includes the Postgresql database, redis, and webserver](https://docs.pulpproject.org/pulp_installer/customizing/#all-in-one-pulp-server-example).
* [Pulp on one server, while using existing servers for the database, redis, and webserver](https://docs.pulpproject.org/pulp_installer/customizing/#pulp-server-with-existing-infrastructure).
* [Pulp on one server, with the database, redis & webserver on separate servers](https://docs.pulpproject.org/pulp_installer/customizing/#pulp-with-separate-servers-for-services).
* [Each and every Pulp service on a separate server](https://docs.pulpproject.org/pulp_installer/customizing/#separate-servers-for-each-and-every-service).

You can also use these examples as a base and further extend them to suit your needs.

In the following example, each host contains a webserver, pulpcore-api, pulpcore-content application and pulpcore-worker.

![](/images/pulp-workflow-architecture-ha/pulp-ha-example.png)

The advantages of this setup is its homogeneity. You can keep adding more hosts with the uniform setup to extend your deployment as required.
Your webserver configuration never needs to change as it can always communicate with pulpcore-api, and pulpcore-content applications via localhost.
However, you can configure the webserver settings in the Ansible playbooks for the Pulp installer.
For this to remain highly available, ensure that the hosts are distributed across availability zones in case of a failure scenario.
