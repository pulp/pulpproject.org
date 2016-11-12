---
title: Yum Repository Layout Changes
layout: post
author: Brian Bouterse
tags:
  - rpm
---
The upcoming 2.12 release will change the layout of published Yum repositories. In 2.11 or earlier,
when publishing a Yum repository you will get something like the following snippet from a locally
synced [zoo repository](https://repos.fedorapeople.org/repos/pulp/pulp/fixtures/rpm/):

```
[vagrant@dev zoo]$ tree
.
├── bear-4.1-1.noarch.rpm
├── camel-0.1-1.noarch.rpm

<snip>

├── repodata
│   ├── 12c9a3187bccbcbc3d0e1edf0163bfdc6eff1d65fdd5f1353cdcdeb70ed6728f-primary.xml.gz
│   ├── 3d46903cd04e68284f18067ce8c710571eb75ecc3d247686cea1e3326cf9ed6f-filelists.xml.gz
│   ├── 829355cb5d6bb74be7ddc01f04413af713a7780382ad2f0b7c3c77943a7b7c75-other.xml.gz
│   ├── 87cfcf84fbe80f2f8593194797744d0a9256c8769c0d1d74f4ba0337c0e387b5-comps.xml
│   ├── e358bb4bce6b743c0feca58214a4a54b82a9184163e8325c830d8dd0db2c6e4c-updateinfo.xml.gz
│   └── repomd.xml

<snip>

├── walrus-0.71-1.noarch.rpm
├── walrus-5.21-1.noarch.rpm
├── whale-0.2-1.noarch.rpm
├── wolf-9.4-2.noarch.rpm
└── zebra-0.1-2.noarch.rpm
```

The 2.12+ published rpm repository will be different in a few ways:

1. Packages will be published in the `Packages` directory and will no longer be published in the
   root of the repository.

2. Packages will be placed in a folder by the first character of the package name.

3. Metadata files in the `repodata` directory will reference the new relative paths allowing Yum and
   similar clients to continue discovering packages.

Here is the same example from above organized using the new layout:

```
[vagrant@dev zoo]$ tree
.
├── Packages   
│   ├── a
│   ├── b
│   │   └── bear-4.1-1.noarch.rpm
│   ├── c
│   │   ├── camel-0.1-1.noarch.rpm

<snip>

│   ├── w
│   │   ├── walrus-0.71-1.noarch.rpm
│   │   ├── walrus-5.21-1.noarch.rpm
│   │   ├── whale-0.2-1.noarch.rpm
│   │   └── wolf-9.4-2.noarch.rpm
│   ├── x
│   ├── y
│   └── z
│       └── zebra-0.1-2.noarch.rpm
└── repodata
    ├── 12c9a3187bccbcbc3d0e1edf0163bfdc6eff1d65fdd5f1353cdcdeb70ed6728f-primary.xml.gz
    ├── 3d46903cd04e68284f18067ce8c710571eb75ecc3d247686cea1e3326cf9ed6f-filelists.xml.gz
    ├── 829355cb5d6bb74be7ddc01f04413af713a7780382ad2f0b7c3c77943a7b7c75-other.xml.gz
    ├── 87cfcf84fbe80f2f8593194797744d0a9256c8769c0d1d74f4ba0337c0e387b5-comps.xml
    ├── e358bb4bce6b743c0feca58214a4a54b82a9184163e8325c830d8dd0db2c6e4c-updateinfo.xml.gz
    └── repomd.xml
```


### Motivation

* Having all of the packages in one folder limits the ability to publish or copy Pulp repositories 
  on some filesystems.

* The `Packages` folder is a common convention and keeps the repository organized. For example, see
  the [Fedora](http://download.fedoraproject.org/pub/fedora/linux//releases/24/Server/x86_64/os/)
  or [CentOS](http://mirror.centos.org/centos/7/os/x86_64/) repositories.


### Backwards Compatibility

Yum and similar clients are expected to have no interruption with this change. The repository
metadata is consistent with the packages available in both the old and new layout. Repositories
already published prior to 2.12 will retain the old layout with 2.12+ until they are republished. 

Users who have hard-coded package urls to Pulp published repositories should update their urls to
use the new locations. This impact should be minimal with most users using Puppet,
[Ansible's package module](https://docs.ansible.com/ansible/package_module.html), Yum bindings, or
similar.


### Get Involved

If this impacts a use case you have for Pulp, please describe it via
[pulp-list](https://www.redhat.com/mailman/listinfo/pulp-list) or post on
[Issue #1976](https://pulp.plan.io/issues/1976).

It would be great to have a few users test this feature in one of the 2.12 betas or release
candidates.
