---
title: Installing Pulp3 on Openshift
author: Brian Bouterse
---
Here is information on how to deploy Pulp3 on Openshift with a plugin like
[pulp_ansible](https://github.com/pulp/pulp_ansible). I posted the resources we will use in a repo:
[pulp3-openshift](https://github.com/bmbouter/pulp3-openshift).


# Overview

The minimal install has 5 containers:

* Webserver. I use `pulp-manager runserver`.
* Resource manager
* Worker
* Postgresql
* Redis


# Postgresql and Redis config

The Postgresql and Redis services are deployed by hand from your Openshift Service Catalog. It's
assumed your Postgresql database is named 'pulp' and the user accessing the db is also named 'pulp'.

After deploying Redis and Postgresql be sure you have the following settings available.

REDIS_IP
REDIS_PORT
REDIS_PASSWORD
POSTGRESQL_HOST
POSTGRESQL_PORT
POSTGRESQL_PASSWORD

Clone the [pulp3-openshift](https://github.com/bmbouter/pulp3-openshift/) repo and enter all of the
values of ^ variables into your
[pulp.json template file](https://github.com/bmbouter/pulp3-openshift/blob/master/deployment_config/pulp.json).
Note each variable is set in multiple places.


# Make a Persistant Storage

Pulp needs persistant storage, typically mounted at /var/lib/pulp/. I was able to apply this
[pv.yaml](https://github.com/bmbouter/pulp3-openshift/blob/master/pv.yaml) to create the
PersistentVolumeClaim.

The names in pv.yaml are then referenced in the deployment config [here](https://github.com/bmbouter/pulp3-openshift/blob/master/deployment_config/pulp.json#L28)
so you may need to update your deployment config to match.
 

# Additional settings

Make up a value for SECRET_KEY with the snippet below and set it also in both the
[pulp.json template file](https://github.com/bmbouter/pulp3-openshift/blob/master/deployment_config/pulp.json)
and the [Dockerfile](https://github.com/bmbouter/pulp3-openshift/blob/master/pulp_ansible/Dockerfile#L19).

```
cat /dev/urandom | tr -dc 'a-z0-9!@#$%^&*(\-_=+)' | head -c 50

```

Invent your admin password and specify it [here](https://github.com/bmbouter/pulp3-openshift/blob/master/pulp_ansible/s2i/bin/run#L39)
so it will run every time one of the pulp component starts.


# Configuring What Is Installed

To include other plugins, source installs, or custom code you'll want to modify the pip commands in
the Dockerfile where Pulp is installed [here](https://github.com/bmbouter/pulp3-openshift/blob/master/pulp_ansible/Dockerfile#L26-L28.

```
RUN pip install -e 'git://github.com/pulp/pulp#egg=pulpcore&subdirectory=pulpcore'
RUN pip install -e 'git://github.com/pulp/pulp#egg=pulpcore-plugin&subdirectory=plugin'
RUN pip install -e 'git://github.com/pulp/pulp_ansible#egg=pulp_ansible'
```

Migrations are made a image-build time. Make sure to `makemigrations` for any django apps that you
are installing
[here](https://github.com/bmbouter/pulp3-openshift/blob/master/pulp_ansible/Dockerfile#L30-L31). 

```
RUN pulp-manager makemigrations pulp_app
RUN pulp-manager makemigrations pulp_ansible
```

Migrations are run at image-start time because its there that there is a connection to the real DB.
Have the migrations apply upon image startup
[here](https://github.com/bmbouter/pulp3-openshift/blob/master/pulp_ansible/s2i/bin/run#L38).


# Build your Image

Within the [pulp_ansible](https://github.com/bmbouter/pulp3-openshift/blob/master/pulp_ansible/) run
`sudo make` to build the image. This will run all the commands in the Dockerfile. Note you will need
the docker daemon running locally for this to work.


# Tag and Publish your Image

For now I get my images onto openshift through dockerhub. I tag and push the image like this:

```
sudo docker tag f897f98fd9 bmbouter/pulp_ansible:0.1.b2.34
sudo docker push bmbouter/pulp_ansible:0.1.b2.34
```

Then I use this name in my deployment config [here](https://github.com/bmbouter/pulp3-openshift/blob/master/deployment_config/pulp.json#L35).

Everytime I make a change I increment the number otherwise openshift uses the already downloaded and
cached image from before.


# Deploy your Image

With your deployment config, e.g. [pulp.json](https://github.com/bmbouter/pulp3-openshift/blob/master/deployment_config/pulp.json)
customized you are ready to deploy Pulp using the 'Add to Project' dropdown and selecting 'Import
YAML/JSON' then paste in your deployment config.


# Expose a Service and Route

The last step is to expose a Service and then a Route to the Pulp webserver. I was able to deploy
from configs for both the [Service](https://github.com/bmbouter/pulp3-openshift/blob/master/service.yaml)
and the [Route](https://github.com/bmbouter/pulp3-openshift/blob/master/route.yaml).

These thing should match up with the port and pod labels in the deployment config.


# Test Pulp with the status API

For example in one test deployment I could receive my status at:

```
http example.com:24817/pulp/api/v3/status/
```


# Pics or it Didn't Happen

![The Deployment Config?](/images/install-on-openshift/deployment_config.png)

![The Services?](/images/install-on-openshift/services.png)


# Get Involved

I'm tracking bugs related to this mini-effort [here](https://github.com/bmbouter/pulp3-openshift/issues/).
You can see some existing issues or file a new one for help.

PRs and collaboration is welcome. I'm 'bmbouter' on Freenode.
