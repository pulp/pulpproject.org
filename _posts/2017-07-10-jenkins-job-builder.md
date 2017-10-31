---
title: Developing Jenkins Jobs with Jenkins Job Builder
author: Elijah DeLee
tags:
  - pulpqe
---

The power of Jenkins
----------------

I first was exposed to Jenkins while working for an open source software
project out of my university that wanted to use continuous integration as a
part of their workflow. While services like [Travis](https://travis-ci.org/)
and [Semaphore](https://semaphoreci.com/) can be
excellent if you can accomplish your build within the constraints of the
systems they provide, they could not meet the needs of that project. Enter
[Jenkins](https://jenkins.io/), "the leading open source automation server".

Jenkins is an powerful tool, and much of this power comes from the fact that it
is very customizable. Configuring jobs via the web UI that Jenkins provides
leaves many things to be desired, however. First, the task is fraught with
human error. It is very easy to forget to expand that one nested menu and set a
critical value. Small mistakes like this can lead to faulty jobs that lead to
misleading results or a broken CI/CD pipeline causing a headache for all
involved.  Secondly, once you have constructed your house of cards, you have
only to experience a hard drive failure to lose all your work. Even more
likely, if something changes in your set up you may have to hand update dozens
of jobs so carefully configured with hours of mouse clicks.

Jenkins Job Builder (JJB)
----------------

To scale the process of Jenkins job definition it becomes obvious that one must
be able to generate Jenkins jobs based on templates. This allows the use of
variables so that you might test multiple versions against multiple OSes, or
vary other conditions while allowing yourself to share common steps and code
among jobs. Moreover, these assets should be under version control and stored
in some location other than the machine Jenkins is running on. The good news is,
this tool already exists!

[Jenkins Job Builder](https://docs.openstack.org/infra/jenkins-job-builder/),
installable via [pip](https://pypi.python.org/pypi/pip) allows the user to
generate the job definitions from YAML files and upload the XML job definitions
to your Jenkins instance.

PulpQE stores the YAML files and scripts used to generate the pulp jenkins jobs
with jenkins-job-builder in the
[pulp-ci](https://github.com/pulp/pulp-ci) repo.

While working on changes to these files, it is desirable to be able to try
them out locally, so I decided to give it a shot!

I have [Docker](https://www.docker.com/) set up on my Fedora 25 machine, so I spun up a Jenkins instance to
test it out. There is an official [Jenkins image on
DockerHub](https://hub.docker.com/_/jenkins/) so I pulled it down and served it
up on my localhost. _(More detailed guides for setting up jenkins from this
docker image are available on the Jenkins DockerHub page)_

    docker pull jenkins
    docker run -p 8080:8080 -p 50000:50000 jenkins

I then visited the Jenkins web UI at <code>http://localhost:8080</code> and was
directed to login with an admin account created during the install. After
changing the admin password, I chose to "download recommended plugins".

JJB documentation guided me to create a virtual environment to download and run
jenkins job builder in.

    cd $HOME
    python3 -m venv jjb-env
    source ~/jjb-env/bin/activate
    pip install jenkins-job-builder

Then, I needed to set up a configuration file for jenkins-job-builder.

    cat >> $HOME/jjb.ini << 'EOF'
    > [jenkins]
    > user=myadminaccount
    > password=myterribleplaintextpassword
    > url=http://localhost:8080
    > query_plugins_info=False
    >
    > [job_builder]
    > include_path=ci/jobs/scripts
    > EOF

With those pieces in place, I cloned the
[pulp-ci](https://github.com/pulp/pulp-ci) repo and tried to
upload a job to my local Jenkins instance.

    cd $HOME
    git clone https://github.com/pulp/pulp-ci
    cd pulp-ci
    jenkins-jobs --conf $HOME/jjb.ini --ignore-cache update ci/jobs pulp-installer

Happily, I was greeted with the messages:

    INFO:root:Updating jobs in ['ci/jobs'] (['pulp-installer'])
    ... output omitted ...
    INFO:jenkins_jobs.builder:Number of jobs generated:  1
    INFO:jenkins_jobs.builder:Creating jenkins job pulp-installer
    INFO:root:Number of jobs updated: 1

And lo and behold, there was a new job that popped up in the Jenkins dashboard!
Now I am ready to work on creating my own Jenkins jobs and getting them fine
tuned before shipping them off to our real Jenkins server.

Limitations
----------------

While Jenkins Job Builder does a lot to remedy concerns around being able to
migrate jobs from one instance of Jenkins to another without data loss or
excessive manual configuration, it does not manage all aspects of a complete
Jenkins set up. One should not, for example, manage credential migration from
one Jenkins to another with JJB. When credentials are stored in Jenkins, they
can then be referenced in JJB YAML files via their ID. If you set up a new
Jenkins instance you will have to put your credentials in place on the Jenkins
manually and then update the ID references in your JJB job definitions. If you
fail to update SSH key information or other data stored on Jenkins and mapped
to JJB via its ID, and your jobs are broken because of this incorrect data, JJB
has no way of knowing about this or informing you of the error.

Furthermore, this may be a case where in a local install of Jenkins you would
have to modify this data for testing purposes, and then switch it back to the
correct data for your official Jenkins instance.
