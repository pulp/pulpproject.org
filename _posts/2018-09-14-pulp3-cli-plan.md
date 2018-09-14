---
title: Pulp3 CLI Plan
author: Brian Bouterse
---
The Command Line Interface (CLI) is a useful tool that Pulp2 users have come to rely on. As Pulp3
works towards a Release Candidate and finally a Generally Available release, we want to communicate
our recommendations on the CLI usage.


# Current State

Recently for Pulp3, a prototype auto-generating CLI
[was created for Pulp3](https://www.redhat.com/archives/pulp-dev/2018-June/msg00025.html). It was
very useful in the sense that we learned a lot about how the Pulp API is being described by OpenAPI
v2. However since the [next steps](https://www.redhat.com/archives/pulp-dev/2018-July/msg00017.html)
were identified, we realized the remaining work required to bring the prototype to a usable point is
a non-trivial level of effort.

Another option that is available is to use [HTTPie](https://httpie.org/) as the CLI. You can see
examples of HTTPie usage, .i.e. `http` in the
[pulp_file instructions](https://github.com/pulp/pulp_file#create-a-repository-foo). Using HTTPie is
similar to the auto-generating CLI because both have each command be a single API call.


# Recommendation to Pulp3 users

So for the time being, at least through the Pulp 3.0 General Availability, it's recommended to use
[HTTPie](https://httpie.org/) for your command line scripting needs. You can see examples of its
usage in the [pulp_file instructions](https://github.com/pulp/pulp_file#create-a-repository-foo).
Other Pulp3 plugins have similar instructions on how to interact with their APIs using httpie in
their README files


# Workflow Based CLI

The Pulp2 CLI, `pulp-admin` is primarily a workflow tool. Pulp3 does several steps differently than
Pulp2, so as users approach Pulp3, we don't yet know the workflow use cases that are of broad use to
many users. Given that requirements gap, the current approach is to wait to hear from users about
what their workflow CLI needs are.


# Contribute Your Use Cases

Do you want a specific workflow for a Pulp3 CLI? Write a story
[here](https://pulp.plan.io/issues/new) with details of the workflow that would be useful to you.
