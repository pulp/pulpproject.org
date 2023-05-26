---
title: Pulp in One Container
sidebar: home_sidebar
permalink: /pulp-in-one-container/
toc: false
---

The quickest way to get started with Pulp is [multi-process image](https://docs.pulpproject.org/pulp_oci_images/multi-process-images/) that has all necessary services to run Pulp 3. It is intended to cover as many use cases for small-scale Pulp deployments.

If you prefer multiple containers, we have [pulp-operator](https://docs.pulpproject.org/pulp_operator/), which is robust but requires a Kubernetes Infrastructure. We also have a [Podman Compose / Docker Compose](/podman-compose/) deployment option to run the [single-process image](https://docs.pulpproject.org/pulp_oci_images/multi-process-images/), however it is intended as a proof-of-concept, and you will likely need to modify the Compose file for your needs.

The multi-process image is available under the `pulp` namespace on [Dockerhub](https://hub.docker.com/repository/docker/pulp/pulp/). This image includes the Ansible, Container, Deb, File, Maven, Python, and RPM plugins. A new version is published every time there is a plugin update available. You can update your environment to the latest version of the container using `podman pull`.

If you experience any problems, check the [Known Issues](https://docs.pulpproject.org/pulp_oci_images/multi-process-images/#known-issues) section for workarounds, and file an [issue on Github](https://github.com/pulp/pulp-oci-images/issues). If you have any questions, feel free to reach out to us on `pulp-list@redhat.com` or the [`pulp`](/help/#chat-to-us) channel on Matrix.

You can use either `podman` or `docker`. If you use `docker`, substitute `docker` for `podman` in the following examples.

The following commands can be used to start Pulp 3.4.1 with `podman 1.6.4`, which is available on
CentOS/RHEL 7 and later

```
$ mkdir settings pulp_storage pgsql containers
$ echo "CONTENT_ORIGIN='http://$(hostname):8080'
ANSIBLE_API_HOSTNAME='http://$(hostname):8080'
ANSIBLE_CONTENT_HOSTNAME='http://$(hostname):8080/pulp/content'
CACHE_ENABLED=True" >> settings/settings.py
```

## With SELinux

For systems with SELinux in enforcing mode, use the following command to start Pulp:

```
$ podman run --detach \
             --publish 8080:80 \
             --name pulp \
             --volume "$(pwd)/settings":/etc/pulp:Z \
             --volume "$(pwd)/pulp_storage":/var/lib/pulp:Z \
             --volume "$(pwd)/pgsql":/var/lib/pgsql:Z \
             --volume "$(pwd)/containers":/var/lib/containers:Z \
             --device /dev/fuse \
             pulp/pulp
```

## Without SELinux

For systems without SELinux in enforcing mode, use the following command to start Pulp:

```
$ podman run --detach \
             --publish 8080:80 \
             --name pulp \
             --volume "$(pwd)/settings":/etc/pulp \
             --volume "$(pwd)/pulp_storage":/var/lib/pulp \
             --volume "$(pwd)/pgsql":/var/lib/pgsql \
             --volume "$(pwd)/containers":/var/lib/containers \
             --device /dev/fuse \
             pulp/pulp
```

In both cases, you will see the following output. The last line contains the container ID that you can use to execute commands inside the container.

```
Trying to pull quay.io/pulp/pulp...
Getting image source signatures
Copying blob 67e3038dc3b7 done
Copying blob ff23023c8c8d done
Copying blob 8c32f98c5416 done
Copying blob f328d15d69fa done
Copying blob ffcb1be59aa1 done
Copying blob b9a1b34b38d0 done
Copying blob 443cb276d4ae done
Copying blob d4bcf1091bc0 done
Copying blob f8e0ce13e5f4 done
Copying blob 0c976302c80e done
Copying blob d69fddf60189 done
Copying blob dba42db3b9a9 done
Copying blob 63e0efa76d65 done
Copying blob 355e3ca36a59 done
Copying blob 9854ce01af4c done
Copying blob d2e7008e82d0 done
Copying blob 7d16b80a34a6 done
Copying blob 219dcbdc1826 done
Copying blob 62370638e9ec done
Copying blob 01ceabebb047 done
Copying blob a1881de732d0 done
Copying blob 0e76fdd96f40 done
Copying blob 673f8ea740ee done
Copying blob bfce4aba4f42 done
Copying blob eb6e7e68f0d6 done
Copying blob 0f6b1bb45cd3 done
Copying config 2692cc3736 done
Writing manifest to image destination
Storing signatures
f7044dec1e9d1b9dd66d44e0056280a4399d6eac5c258f03a3a8ccd2a7e6effc
```

Note that the container ID is unique to your deployment.

Use the container ID to reset the `admin` user's password. `podman` also accepts the first part
of the ID for convenience.

```
$ podman exec -it pulp bash -c 'pulpcore-manager reset-admin-password'
Please enter new password for user "admin":
Please enter new password for user "admin" again:
Successfully set password for "admin" user.
```

At this point, both the REST API and the content app are available on your host's port `8080`. Try
hitting the pulp status endpoint to confirm:

```
curl localhost:8080/pulp/api/v3/status/
```

To start working with Pulp, check out the [Workflows and Use Cases](https://docs.pulpproject.org/workflows/index.html).
For individual plugin documentation, see [Pulp 3 Content Plugin Documentation](/docs/#pulp-3-content-plugin-documentation).


## Pulp CLI

We recommend using [pulp-cli](https://github.com/pulp/pulp-cli) to interact with Pulp. If you have
Python 3 installed on the host OS, you can run these commands to get started:

```
pip install pulp-cli[pygments]
pulp config create --username admin --base-url http://localhost:8080 --password <admin password>
```

Then you should be able to execute commands such as `pulp status`.

## References

The Container file and all other assets used to build the container image
are available on [GitHub](https://github.com/pulp/pulp-oci-images).