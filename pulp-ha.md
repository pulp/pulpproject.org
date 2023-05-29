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

### Pulp Operator

Pulp operator endeavours to provide a scalable and robust cluster for Pulp 3.
Pulp can be installed from [OperatorHub](https://operatorhub.io/operator/pulp-operator).
For more information about using Pulp operator, see [Pulp on Openshift](https://pulpproject.org/insta-demo/)
