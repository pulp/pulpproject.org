---
id: 1328
title: 'Getting Started on Pulp and Pulp Smash &#8211; A PulpQE Intern Summary'
date: 2016-07-05T21:31:44+00:00
author: Chongrui Zhang
layout: post
guid: http://www.pulpproject.org/?p=1328
permalink: /2016/07/05/getting-started-on-pulp-and-pulp-smash-a-pulpqe-intern-summary/
categories:
  - Uncategorized
tags:
  - pulpqe-intern
---
# Getting started with Pulp and Pulp-smash

<p dir="ltr">
  This summer, I have joined the PulpQE team in Red Hat as a summer internship. My main job is to contribute automated CLI/API-based tests through <a href="https://github.com/PulpQE/pulp-smash">Pulp-Smash</a>, a test suite for Pulp. Through the summer, I’d like to record what I have learned from a perspective of “newbie”, thus providing a fresh idea to young developers like me for learning Pulp and Pulp-Smash. This post will cover how I install pulp and pulp-smash in a step-by-step process.
</p>

# Install the development environment

<p dir="ltr">
  The very first step is to install Pulp on my host. I’m using Fedora 23, and I want to setup a virtual machine for Pulp to avoid messing up my local environment. It’s great that the Pulp team has already provided a Vagrantfile that can “automatically deploy a virtual machine on your host with a Pulp development environment configured”. The reference of the Developer Setup Documentation is here on the official documentation:
</p>

<p dir="ltr">
  <a href="http://docs.pulpproject.org/dev-guide/contributing/dev_setup.html">http://docs.pulpproject.org/dev-guide/contributing/dev_setup.html</a>
</p>

<p dir="ltr">
  Here I just extracted out the commands from that doc as my notes for installation:
</p>

  1. <span dir="ltr">Install vagrant, ansible, and nfs-utils. NFS will be used to share your code directory with the deployed virtual machine:<br /> </span></p> 
    > <span dir="ltr">$ sudo dnf install &#8211;best &#8211;allowerasing ansible nfs-utils vagrant-libvirt</span>

  2. <span dir="ltr">Grant the nfsnobody user rx access to the folder to check out code with, especially in Fedora; since $HOME typically does not allow such access to other users. So in the future, the working directory will become $HOME/devel or similar in the vm, which is sharable to my local filesystem. For example, the path to my folder of Pulp is /home/<user>/Documents/pulp, so in vm it is actually /home/vagrant/devel/pulp. This is really nice and convenient for development.<br /> </span></p> 
    > <span dir="ltr">$ setfacl -m user:nfsnobody:r-x $HOME</span><span dir="ltr"><br /> </span>

  3. <span dir="ltr">Start and enable the nfs-server service:<br /> </span></p> 
    > <span dir="ltr">$ sudo systemctl enable nfs-server && sudo systemctl start nfs-server</span><span dir="ltr"><br /> </span>

  4. <span dir="ltr">Allow NFS services through your firewall:<br /> </span></p> 
    > <span dir="ltr">$ sudo firewall-cmd &#8211;permanent &#8211;add-service=nfs</span><span dir="ltr"><br /> $ sudo firewall-cmd &#8211;permanent &#8211;add-service=rpc-bind<br /> $ sudo firewall-cmd &#8211;permanent &#8211;add-service=mountd<br /> $ sudo firewall-cmd &#8211;reload</span>

<ol start="5">
  <li>
    <span dir="ltr">Check out the Pulp code<br /> </span></p> <blockquote>
      <p>
        <span dir="ltr">$ cd $HOME/devel</span><span dir="ltr"><br /> $ git clone <a href="mailto:git@github.com">git@github.com</a>:pulp/pulp.git</span>
      </p>
    </blockquote>
  </li>
  
  <li>
    <span dir="ltr">Change the Vagrantfile in the pulp directory<br /> The Pulp project provides an example Vagrantfile that you can use as a starting point by copying it. Change<a href="https://github.com/pulp/pulp/blob/master/Vagrantfile.example#L40"> this line</a> to something like this:<br /> </span></p> <blockquote>
      <p>
        <span dir="ltr">config.vm.provision &#8220;shell&#8221;, inline: &#8220;sudo dnf upgrade -y || sudo dnf upgrade -y&#8221;</span><span dir="ltr"><br /> </span>
      </p>
    </blockquote>
  </li>
  
  <li>
    <span dir="ltr">Begin provisioning your Vagrant environment. We will finish by running vagrant reload. This allows the machine to reboot after provisioning<br /> </span></p> <blockquote>
      <p>
        <span dir="ltr">$ cd pulp</span><span dir="ltr"><br /> $ cp Vagrantfile.example Vagrantfile<br /> $ vagrant up<br /> $ vagrant reload<br /> # Reboot the machine at the end to apply kernel updates</span>
      </p>
    </blockquote>
  </li>
</ol>

<ol start="8">
  <li>
    ssh into your Vagrant environment:<br /> <blockquote>
      <p>
        $ vagrant ssh
      </p>
    </blockquote>
  </li>
  
  <li>
    <span dir="ltr">Verify the installation through the status of Pulp admin and Pulp API:<br /> </span></p> <blockquote>
      <p>
        <span dir="ltr">$ pulp-admin status<br /> +&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;-+<br /> Status of the server<br /> +&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;-+</span>
      </p>
      
      <p>
        Api Version:           2<br /> Database Connection:<br /> Connected: True<br /> ……<br /> Messaging Connection:<br /> Connected: True<br /> Versions:<br /> Platform Version: 2.9.0b1  # Note the Pulp’s version here.
      </p>
    </blockquote>
    
    <blockquote>
      <p>
        <span dir="ltr">$ phttp <a href="/external-link.jspa?url=https%3A%2F%2Fdev%2Fpulp%2Fapi%2Fv2%2Fstatus%2F" target="_blank" rel="nofollow">https://dev/pulp/api/v2/status/</a></span>
      </p>
    </blockquote>
  </li>
  
  <li>
    <span dir="ltr">You can migrate the database after a fresh install:<br /> </span></p> <blockquote>
      <p>
        <span dir="ltr">$ sudo -u apache pulp-manage-db</span>
      </p>
    </blockquote>
  </li>
  
  <li>
    The Pulp development environment also provides a `pclean` to restore database. But it will destroy all user data. You can refer to the man page of it through phelp. It will also provide several other useful commands such as phttp, pjournal, prestart, etc.
  </li>
</ol>

<p dir="ltr">
  And now you have a running deployed Pulp development machine! Pretty easy and convenient to deploy Pulp in such an automatic way. Please take note that every time you halt the vm you need to `vagrant up` again. So I just played around in the Pulp with the help of official docs. And some of the excerpts will be listed in the next section.
</p>

* * *

## Start Pulp and Related Services manually.

<p dir="ltr">
  Several services can be accessed through systemctl manually, according to the developer’s setup doc:
</p>

> <p dir="ltr">
>   Start the broker:          sudo systemctl start qpidd
> </p>
> 
> <p dir="ltr">
>   Start the agent:           sudo systemctl start goferd
> </p>
> 
> <p dir="ltr">
>   Start the database:        sudo systemctl start mongod
> </p>
> 
> <p dir="ltr">
>   Synchronize db:            sudo -u apache pulp-manage-db
> </p>
> 
> <p dir="ltr">
>   Start the server:          sudo systemctl start httpd
> </p>
> 
> <p dir="ltr">
>   Start pulp workers:        sudo systemctl start pulp_workers;
> </p>
> 
> <p dir="ltr">
>   Start pulp celerybeat:     sudo systemctl start pulp_celerybeat;
> </p>
> 
> <p dir="ltr">
>   Start pulp resource manager: sudo systemctl start pulp_resource_manager
> </p>

<p dir="ltr">
  Well, are these pulp services’ names the major components for Pulp? Yes, and here’s a detailed introduction to some the components listed: <a href="https://docs.pulpproject.org/user-guide/server.html">https://docs.pulpproject.org/user-guide/server.html</a>
</p>

## How to Install a plugin

<p dir="ltr">
  Pulp has a feature called Pulp Platform, which can manage content/software packages extensible to nearly any type of digital content. Here’s the excerpt from <a href="http://docs.pulpproject.org/user-guide/introduction.html#plugins">here</a>:
</p>

<p dir="ltr">
  Pulp manages content, and its original focus was software packages such as RPMs and Puppet modules. The core features of Pulp, such as syncing and publishing repositories, have been implemented in a generic way that can be extended by plugins to support specific content types. We refer to the core implementation as the Pulp Platform. With this flexible design, Pulp can be extended to manage nearly any type of digital content.
</p>

<p dir="ltr">
  And here’s an example of installing pulp-rpm. At least one plugin must be installed for every pulp instance.
</p>

> <p dir="ltr">
>   $ git clone <a href="/external-link.jspa?url=https%3A%2F%2Fgithub.com%2Fpulp%2Fpulp_rpm.git" target="_blank" rel="nofollow">https://github.com/pulp/pulp_rpm.git</a><br /> $ cd pulp_rpm<br /> $ sudo yum install pulp-rpm-plugins<br /> $ sudo ./manage_setup_pys.sh develop<br /> $ sudo python ./pulp-dev.py -I<br /> $ sudo -u apache pulp-manage-db
> </p>

## Issues after installing pulp-rpm plugin

<p dir="ltr">
  I encountered an error “The importer type yum_importer does not exist” after the fresh install of pulp.
</p>

<p dir="ltr">
  To solve this, I tried to execute a pclean to restore the database. However, another error came up as “ImportError: No module named createrepo”&#8230;
</p>

<p dir="ltr">
  Finally I got a clue from <a href="https://www.redhat.com/archives/pulp-list/2015-June/msg00013.html">https://www.redhat.com/archives/pulp-list/2015-June/msg00013.html</a>. It looks like there’s a missing yum package on the Vagrant’s system. After installing the package through `sudo yum install pulp-rpm-plugins`, everything is ok now. I have also added this command into the above code snippet. Please change to pulp-puppet-plugins in order to install pulp-puppet plugin.
</p>

<p dir="ltr">
  You can test the repo’s functionality manually through this snippet FYI:
</p>

> <p dir="ltr">
>   $ pulp-admin -u admin -p admin rpm repo create &#8211;repo-id repo1 &#8211;relative-url zoo &#8211;feed \<br /> <a href="/external-link.jspa?url=http%3A%2F%2Frepos.fedorapeople.org%2Frepos%2Fpulp%2Fpulp%2Fdemo_repos%2Frepo1" target="_blank" rel="nofollow">http://repos.fedorapeople.org/repos/pulp/pulp/demo_repos/repo1</a><br /> $ pulp-admin -u admin -p admin rpm repo sync run &#8211;repo-id repo1<br /> $ pulp-admin -u admin -p admin rpm repo delete &#8211;repo-id repo1
> </p>

<p dir="ltr">
  Note that a newer version of pclean automatically creates a repository called zoo with the same feed. Please check that with `pulp-admin repo list`. Now that I have successfully deployed a Pulp on my vm, it is time to further install pulp-smash. The next section of this blog covers how to configure the pulp-smash after a fresh installation of Pulp under the Vagrant environment.
</p>

* * *

&nbsp;

# Introduction to Pulp Smash

<p dir="ltr">
  Before installation, I will briefly give an introduction to Pulp Smash. All tests in Pulp Smash are automated and suited for use in a continuous integration environment, providing both CLI- and API-based test cases for the Pulp platform. It can thus talk to multiple systems via their API and CLI, and it can stop and start system services, drop databases, delete files from their filesystems, and so on.
</p>

<p dir="ltr">
  According to the comments <a href="https://github.com/PulpQE/pulp-smash/issues/311#issuecomment-227759897">here</a>, users can tell Pulp Smash what system(s) are available via (by default) a `~/.config/pulp_smash/settings.json` file, and Pulp Smash can choose which tests to run based on what&#8217;s available via `python -m unittest2 pulp_smash.tests`. Pulp Smash can be executed on any types of target systems, such as VMs backed by libvirt, virtualbox, vmware, xen, etc, or docker containers, or bare-metal systems, or the local system, or even a mixture of these. This imparts a great deal of flexibility.
</p>

# Install the Pulp-Smash on the VM

## Step 1. Check out pulp-smash from github and install required packages.

> <p dir="ltr">
>   $ sudo pip install -r requirements.txt -r requirements-dev.txt<br /> $ # After fresh install of pulp-smash, run the following command to see guidance:<br /> $ python -m pulp_smash
> </p>

## Step 2. Create a configuration file in the vm environment.

<p dir="ltr">
  The configuration file at /home/vagrant/.config/pulp_smash/settings.json will be automatically created after the execution of `python -m pulp_smash`. The configuration file should have this structure:
</p>

> <p dir="ltr">
>   {
> </p>
> 
> <p dir="ltr">
>       &#8220;pulp&#8221;: {
> </p>
> 
> <p dir="ltr">
>           &#8220;base_url&#8221;: &#8220;<a href="/external-link.jspa?url=https%3A%2F%2Fpulp.example.com" target="_blank" rel="nofollow">https://pulp.example.com</a>&#8220;,
> </p>
> 
> <p dir="ltr">
>           &#8220;auth&#8221;: [&#8220;username&#8221;, &#8220;password&#8221;],
> </p>
> 
> <p dir="ltr">
>           &#8220;verify&#8221;: true,
> </p>
> 
> <p dir="ltr">
>           &#8220;version&#8221;: &#8220;2.7.5&#8221;,
> </p>
> 
> <p dir="ltr">
>           &#8220;cli_transport&#8221;: &#8220;local&#8221;
> </p>
> 
> <p dir="ltr">
>       },
> </p>
> 
> <p dir="ltr">
>       &#8220;pulp agent&#8221;: {
> </p>
> 
> <p dir="ltr">
>           &#8220;base_url&#8221;: &#8220;<a href="/external-link.jspa?url=https%3A%2F%2Fpulp-agent.example.com" target="_blank" rel="nofollow">https://pulp-agent.example.com</a>&#8220;
> </p>
> 
> <p dir="ltr">
>       }<br /> }
> </p>

<p dir="ltr">
  My only responsibility as a user is to put the right information (including version) in ~/.config/pulp_smash/settings.json. And here’s my working example of that file:
</p>

> <p dir="ltr">
>   [vagrant@dev ~]$ cat .config/pulp_smash/settings.json<br /> {
> </p>
> 
> <p dir="ltr">
>       &#8220;pulp&#8221;: {
> </p>
> 
> <p dir="ltr">
>               &#8220;base_url&#8221;: &#8220;<a href="/external-link.jspa?url=https%3A%2F%2Fdev" target="_blank" rel="nofollow">https://dev</a>&#8220;,
> </p>
> 
> <p dir="ltr">
>               &#8220;auth&#8221;: [&#8220;admin&#8221;, &#8220;admin&#8221;],
> </p>
> 
> <p dir="ltr">
>               &#8220;verify&#8221;: true,
> </p>
> 
> <p dir="ltr">
>               &#8220;version&#8221;: &#8220;2.9.0b1&#8221;,
> </p>
> 
> <p dir="ltr">
>               &#8220;cli_transport&#8221;: &#8220;local&#8221;
> </p>
> 
> <p dir="ltr">
>       }
> </p>
> 
> <p dir="ltr">
>   }
> </p>

## Step 3. Call python -m unittest2 discover pulp_smash.tests.

<p dir="ltr">
  This step may take a while to execute. Have a cup of coffee :  )
</p>

* * *

## Get handy with automated scripts

<p dir="ltr">
  In this section, I’ll also list some useful excerpts from the official documentation. The author was very nice to provide a starting point to learn pulp-smash:
</p>

<p dir="ltr">
  <a href="http://pulp-smash.readthedocs.io/en/latest/about.html#learning-pulp-smash">http://pulp-smash.readthedocs.io/en/latest/about.html#learning-pulp-smash</a>
</p>

<p dir="ltr">
  For example, to verify the sanity of your development environment, cd into the Pulp Smash source code directory and execute `make all`. The author has also suggested to me that, before sending out a Pull Request on github, it is useful to execute this command again to pass Travis CI tests. Also, please copy & paste the output from the test run as a comment in the GitHub pull request: `python -m unittest2 pulp_smash.tests`
</p>

<p dir="ltr">
  In case that you are not familiar with Python’s unittest module, its official documentation would be another good material to learn:
</p>

<p dir="ltr">
  <a href="https://docs.python.org/3/library/unittest.html#command-line-interface">https://docs.python.org/3/library/unittest.html#command-line-interface</a>
</p>

<p dir="ltr">
  This is also suggested by the author that I may run a single test case such as `python -m unittest2 pulp_smash.tests.platform.api_v2.test_login`; and I can consult the source code to see which other test modules are available.
</p>

## Package Structure

<p dir="ltr">
  From the doc, I also found it helpful to read through all commented code on this module:
</p>

<p dir="ltr">
  <a href="http://pulp-smash.readthedocs.io/en/latest/introductory-module.html">http://pulp-smash.readthedocs.io/en/latest/introductory-module.html</a>
</p>

# Errors encountered for Beginners of Pulp-Smash

<p dir="ltr">
  There’s no doubt that I’ll meet several errors by the first time I install pulp-smash. But it really matters how you view/treat those barriers. Do you just feel frustrated by being blocked for couple of hours? Sure it is&#8230;but when you finally solved it, it is also a good chance to improve, right? And more importantly, don’t miss the chance to consult the authors on IRC’s freenode!
</p>

## MissingSchema

> <p dir="ltr">
>   requests.exceptions.MissingSchema: Invalid URL &#8216;/pulp/api/v2/plugins/types/&#8217;: No schema supplied. Perhaps you meant<a href="http://pulp/api/v2/plugins/types/"> http://pulp/api/v2/plugins/types/</a>
> </p>

<p dir="ltr">
  How it is solved: without that <a href="/external-link.jspa?url=https%3A%2F%2F" target="_blank" rel="nofollow">https://</a>, Pulp Smash doesn&#8217;t know whether to contact Pulp on port 80, 443, or what.
</p>

## ConnectionError

> <p dir="ltr">
>   requests.exceptions.ConnectionError: HTTPSConnectionPool(host=&#8217;127.0.0.1&#8242;, port=443): Max retries exceeded with url: /pulp/api/v2/plugins/types/ (Caused by NewConnectionError(&#8216;<requests.packages.urllib3.connection.VerifiedHTTPSConnection object at 0x7f22e8f0c510>: Failed to establish a new connection: [Errno 111] Connection refused&#8217;,))
> </p>

<p dir="ltr">
  How it is solved: First I’ll check if pulp is running:
</p>

> <p dir="ltr">
>   $ systemctl status &#8216;*pulp*&#8217;
> </p>

<p dir="ltr">
  It turns out that I need to start httpd service manually after the vm is halted: sudo systemctl start httpd
</p>

## SSLError

> <p dir="ltr">
>   requests.exceptions.SSLError: hostname &#8216;localhost&#8217; doesn&#8217;t match either of &#8216;dev&#8217;, &#8216;<a href="http://dev.example.com/">dev.example.com</a>&#8216;
> </p>

<p dir="ltr">
  How it is solved:<br /> 1. My solution 1 is to execute `sudo hostnamectl set-hostname &#8220;localhost&#8221;` in order to change the hostname. However, changing hostname only doesn&#8217;t help since it still needs default hostname of dev.
</p>

<p dir="ltr">
  2. My solution 2 is to set &#8220;<a href="/external-link.jspa?url=https%3A%2F%2Fdev" target="_blank" rel="nofollow">https://dev</a>&#8221; as base_url for the configuration. I found out what&#8217;s happening here is that we&#8217;re telling our client to make an SSL-secured connection to the &#8220;localhost&#8221; domain (as configured). The client starts doing that, but when it discovers that the server&#8217;s SSL certificate isn&#8217;t valid for the &#8220;localhost&#8221; domain, but is instead valid for &#8220;dev&#8221; and &#8220;<a href="http://dev.example.com/">dev.example.com</a>&#8221; (by default), the client freaks out.
</p>