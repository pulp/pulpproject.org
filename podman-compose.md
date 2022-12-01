---
title: Podman Compose
sidebar: home_sidebar
permalink: /podman-compose/
toc: false
---

Many members of the community have requested a production-ready container to deploy Pulp.
In an effort to address this feedback, we have re-used images from [Pulp Operator](/pulp-operator/) to provide a podman compose.
Because this podman compose is new, so far it has received limited feedback and testing.
We want to hear from you. Let us know what you think on our [community discourse](https://discourse.pulpproject.org/).

To use Pulp's podman compose, you must have an understanding of podman compose and also how to configure Pulp's podman compose to best suit your deployment requirements.
At the moment, there is minimal documentation for Pulp's podman compose.
If you'd like to add some tips, tricks, or tutorials, feel free to raise a PR against [this page](https://github.com/pulp/pulpproject.org/).

Pulp's podman compose is limited to versions podman 3.x and podman-compose 0.1.x.
It does not work with higher versions.

1. Install Podman Compose:
```
$ pip install podman-compose
```

2. Clone the Pulp Operator repository from Github:
```
$ git clone git@github.com:pulp/pulp-oci-images.git
```

3. In your local clone of the Pulp Operator repo, navigate to the `containers` directory:
```
$ cd pulp-oci-images/images/compose
```

4. To configure Pulp to suit your deployment needs, take the time to familiarize yourself with the default configuration and extend accordingly.
Everything in the `/assets` directory is mounted into the container.

4. To start the podman compose, enter the following command:
```
$ podman-compose up
```
