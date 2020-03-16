---
title: Pulp 3 in One Container
author: Dennis Kliban
tags:
  - 3.0
---
Installing Pulp 3 and getting all the the services running can be challenging. In an effort to make
these steps optional, we have been working on creating a single container image that has all the
services needed to run Pulp 3. The initial version of this image is now available under my
namespace on Dockerhub. This image includes Ansible, Container, File, Maven, and RPM plugins. The
following commands can be used to start up Pulp 3.2.0 with `podman 1.8.0` (available on Fedora 31):

```
$ mkdir settings pulp_storage pgsql containers
$ echo "CONTENT_ORIGIN='$(hostname):8080'
ANSIBLE_API_HOSTNAME='http://$(hostname):8080'
ANSIBLE_CONTENT_HOSTNAME='http://$(hostname):8080/pulp/content'
TOKEN_AUTH_DISABLED=True" >> settings/settings.py
```

## With SELinux

For systems with SELinux in enforcing mode, use the following command:

```
$ podman run -v ./settings:/etc/pulp:Z \
             -v ./pulp_storage:/var/lib/pulp:Z \
             -v ./pgsql:/var/lib/pgsql:Z \
             -v ./containers:/var/lib/containers:Z \
             --device /dev/fuse \
             --publish 8080:80 \
             --detach \
             dkliban/pulp-fedora31
```

## Or Without SELinux

If your system is not running with SELinux enforcing, use the following command to start Pulp:

```
$ podman run -v ./settings:/etc/pulp \
             -v ./pulp_storage:/var/lib/pulp \
             -v ./pgsql:/var/lib/pgsql \
             -v ./containers:/var/lib/containers \
             --device /dev/fuse \
             --publish 8080:80 \
             --detach \
             dkliban/pulp-fedora31
```

In both cases you will see the following output. The last line will be the container ID that you
can use to execute commands inside the container.

```
Trying to pull docker.io/dkliban/pulp-fedora31...
Getting image source signatures
Copying blob 67e3038dc3b7 done
Copying blob 8c32f98c5416 done
Copying blob b9a1b34b38d0 done
Copying blob ffcb1be59aa1 done
Copying blob 0dd8f5bb4607 done
Copying blob f328d15d69fa done
Copying blob 480fa1c3c6c7 done
Copying blob aedc9c68d4e4 done
Copying blob 8687353e82d2 done
Copying blob 0afd62b6efa8 done
Copying blob fd678a636c2e done
Copying blob 2b22f05f3504 done
Copying blob 6428bfef03ee done
Copying blob 8789cabc8467 done
Copying blob 4f23d0cdd7fb done
Copying blob 821e68e48acd done
Copying blob a93e05ff7e2e done
Copying blob 12619077dc44 done
Copying blob 187cecb3d6f1 done
Copying blob 98ef548b2fbe done
Copying blob 51d6413c000a done
Copying blob 100bc44eaf85 done
Copying blob f0011e27d2a2 done
Copying blob f864f6b109f7 done
Copying blob e232ae39190c done
Copying blob 653675dc0bae done
Copying blob dd65a560a32d done
Copying blob 4895124090aa done
Copying config a92ff9bf42 done
Writing manifest to image destination
Storing signatures
38fe951a6f169653dca86259f2480dc47b0b1fa77875290d44ccbad3b9e15a70
```

Now use the container ID to reset the 'admin' user's password. `podman` also accepts the first part
of the ID for convenience.

```
$ podman exec -it 38fe951a6f16965 bash -c 'pulpcore-manager reset-admin-password'
Please enter new password for user "admin": 
Please enter new password for user "admin" again: 
Successfully set password for "admin" user.
```

At this point both the REST API and the content app will be available on your host's port 8080.

## References

The Containerfile and all other assets used to build the `dkliban/pulp-fedora31` container image
are available on [GitHub](https://github.com/dkliban/pulp-fedora31).
