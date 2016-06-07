---
id: 1156
title: Use Docker to Try Pulp
date: 2015-05-21T17:20:21+00:00
author: Michael Hrivnak
layout: post
guid: http://www.pulpproject.org/?p=1156
permalink: /2015/05/21/use-docker-to-try-pulp/
categories:
  - Development
---
Using Docker, you can now easily and quickly stand up a demo deployment of Pulp. Excluding the time it takes Docker to download the images, it takes only a few seconds to have a brand new deployment running.

    
    $ wget https://raw.githubusercontent.com/pulp/pulp_packaging/master/dockerfiles/centos/start.sh
    $ sudo source start.sh /path/to/lots/of/storage/
    
    

The [README](https://github.com/pulp/pulp_packaging/tree/d292ba6e2cfd7abe1f18bed4a66d8ac164c8aaaf/dockerfiles/centos) explains a variety of security and other concerns that prevent this containerization of pulp from being production-worthy. But it is a great way to try Pulp for the first time, try a new release, or experiment with new features in a throw-away environment.

I&#8217;ve been using these Docker images for a few months to do demos, reproduce bugs, and verify that each Pulp process can be run in isolation. They&#8217;ve been very stable and easy to work with, but let me know what your experience is.

The images themselves are built on CentOS 7. You have the best chance at a good experience if you run them on a distribution in the Red Hat, CentOS and Fedora &#8220;family&#8221;, but I have also had success running them on Ubuntu. There were a couple of hurdles to overcome (for example, for reasons I don&#8217;t understand, qpid&#8217;s persistent message store would not work), and I am interested to know if you encounter any problems running these images on a different distribution.

Just run the `start.sh` script mentioned in the README, and it will get everything configured and running. Then you can log in and start using it. The following shows an rpm sync, which under-the-hood makes use of a container running apache, and at least two containers running worker processes:

    
    $ sudo docker run -it --rm --link pulpapi:pulpapi pulp/admin-client bash
    [root@6610af58a341 /]# pulp-admin login -u admin
    Enter password:
    Successfully logged in. Session certificate will expire at May 19 02:50:16 2015 GMT.
    
    [root@6610af58a341 /]# pulp-admin rpm repo create --repo-id=zoo --feed=https://repos.fedorapeople.org/repos/pulp/pulp/demo_repos/zoo/
    Successfully created repository [zoo]
    
    [root@6610af58a341 /]# pulp-admin rpm repo sync run --repo-id=zoo
    +----------------------------------------------------------------------+
    Synchronizing Repository [zoo]
    +----------------------------------------------------------------------+
    
    This command may be exited via ctrl+c without affecting the request.
    
    Downloading metadata...
    [|]
    ... completed
    
    Downloading repository content...
    [==================================================] 100%
    RPMs: 32/32 items
    Delta RPMs: 0/0 items
    
    ... completed
    
    <snip>