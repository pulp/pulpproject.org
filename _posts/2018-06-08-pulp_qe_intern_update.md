---
title: 2018 Pulp QE Intern Update
author: Ragavendran Balakrishnan
tags:
  - pulpqe
---

## How my journey started

   The second week of February'18 was a very exciting period, as that's when I got to know I am
gonna be a Red Hatter. Fast forward four months, Here I am joining as a QE in the pulp project in
Red Hat. My role and responsibilities as a QE were very well delineated on a spreadsheet which I was
presented on the first day. The first week, was very much a getting started phase. So here are the
things which I learned.

   Pulp is a content management plugin that is a major cog in the Satellite Project machinery.  It
is responsible for syncing and managing contents in the yum repository (Oops! Its much more than
that - It can work with contents in the python, docker, ostree, puppets and pretty much any files ).
There are two main versions of the pulp project -  *version 2.x and version 3*.  Each has its own
set of interfaces. The v_2 can be accessed via the CLI or API and v_3 just via the API.

   I also enrolled myself into the Linux RHCSA fast-track course, on which I will be doing a
certification, as a part of this internship. The very first module taught me about ssh-keygen and
passwordless boots to remote VMs. This is something which I learned, will be extensively used in the
pulp setup. I also completed the CEE (Customer Experience and Engagement ) course, which gave a
high-level description about Red Hat's internal org structure.

## Virtual Machines and not containers

   With bits and pieces of information gathered in first week, and with two modules completed in
RHCSA and a fedora machine installed from scratch, I moved into the second week( forgot to mention a
couple of beers over the weekend!!). The second week turned out to be a very productive and
informative week. It is at this point I got to learn about pulp smash ( which is a test suite for
running tests against an installed pulp). Since it needs to be tested against matrix of Operating
Systems, a multi OS virtualization is required. This helps to spawn a separate instance of a box(
Like Fedora 25,26,27 and 28), that can be installed with its own version of pulp.

   Virtual Machine setup can be either a KVM + libvirt  with the aid of a client(Virtual Machine
Manager).  Or, it can be a Beaker machine,(an internal physical machine )which can be provisioned.
It can also be through an utility called as 'Vagrant'. Also, One can provision a machine via the
Jenkins - node pool - OpenStack setup. Though all these methods exist, for the purpose of testing
and not depending on internet, I was advised to use the machine's KVM which was simpler and readily
available.

   Another interesting question that bugged me during this time was, why not containers, as they
are lightweight, easily spawnable and are very expendable. However, I got to learn that, containers
are more suitable for running single process services. The pulp project has several services, that
has multiple processes on it is like MongoDB, Python, and others. This makes it less likely as a
candidate for container friendly. However, this is still a marker for the future, as containers are
still capable of handling these drawbacks.


## Installing Pulp

   Depending on the versions 2 or 3, the installation also varies. The version 2 installer is an
[ansible script](https://github.com/pulp/pulp-ci.git). This ansible script requires a virtual
machine (can be done locally too) with OS installed and python enabled. Once the VM(guest machine)
is up and running, it can be installed by cloning the script in the local and running against the
guest VM or cloning and running directly against the guest vm. This will be much faster when we can
cache connections to the server, inside the .ssh/controlmasters repo.

   Once the ansible script completes running, Pulp is installed.This can be checked by hitting the
command pulp-admin in the remote VM. The version 3 installation is also almost similar which uses
another [ansible script](https://github.com/pulp/devel.git). This requires the server to be turned
up and firewall to be turned down. Since this is an API-only framework, one can check it by hitting
http://hostname:8000/pulp/api/v3/status/ .

## Use Pulp

   Once installed, pulp can be used for managing repos. I was able to create a new repo, feed it
with a remote RPM URL, and then sync the content to my local. I accomplished all these with [the CLI
client](https://docs.pulpproject.org/plugins/pulp_rpm/user-guide/quick-start.html).

   I also managed to write a python script that was capable of cloning an RPM repo, Syncing and
publishing content in the repo and finally deleting it. This was the basic CRUD functionality of it,
that was accomplished by hitting the necessary APIs.


## Pulp Smash ( The Pulp Testing Framework)

   Pulp-smash is a comprehensive testing framework for running testcases against pulp. It has tests
for both version 2 and 3 testing. It has tests for both CLI and API and also for the different type
of plugins - RPMs, Python, docker etc. Also,it is available as a Pypi package, so installing it is
as easy as running a `pip install` command.
	
   It has a [git source](https://github.com/PulpQE/pulp-smash.git) too, that can be cloned and
installed with pip locally. It works on basis of a settings file, that points it to the location of
pulp. As a part of my internship, during this point of time, I created my very own test case, that
ran against pulp and verified whether it was working successfully. I also created a pull request for
the same and got it reviewed and merged with the upstream master.

   By the end of my second week, I was very elated as I not only covered the things mentioned in
the agenda initially, but also few extra exercises that gave me a good learning experience in python
packaging, pulp API automation and writing testcases and that got merged into the upstream.

## Next Steps

   The main agenda for my Internship was to write scripts for testing Pulp in a
[FIPS](https://en.wikipedia.org/wiki/Federal_Information_Processing_Standards) enabled RHEL box.
Over the next few weeks, I will be learning much more about FIPS, and how to enable it in a RHEL
box. Once figured out with the installation in a FIPS machine, I will be writing much more testcases
to ensure its working in FIPS.

   So, Things are going out great so far, learning new things each and every day, have a wonderful
team to work with. With lots of positives, I am looking forward to more joyous and fruitful weeks. I
will be back with another blog post with more updates and more exciting news to share. Until then
take care !!!