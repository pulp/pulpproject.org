---
title: Community Testing Day on August 5th
layout: post
author: Chongrui Zhang
---

The Pulp 2.10 GA is [scheduled for Aug 16,
2016](https://pulp.plan.io/projects/pulp/wiki/Release_Schedule). To deliver a robust Pulp release,
the Pulp QE and Pulp team have organized a Community Test day where the Pulp community can come
together to test Pulp. This post further details the event and how to participate it.

# What is a Community Testing Day and why host it?

According to the [Fedora blog](https://communityblog.fedoraproject.org/hosting-fedora-test-day/),
organizing a test day "is a great way to help expose your project and bring more testers to try a
new update before it goes live".

The main objective of hosting this Pulp Test Day is to identify as many issues with 2.10 as possible
early on with the help of the community. Once identified, important issues can be fixed before the
GA releases leading to a more stable release.

# What to test?

The focus of the test day will be the new features included in Pulp 2.10. The issues fixed in 2.10
can also be verified that day. A list of these features will be provided, and participants can sign
up for the features they want to test. The attendees are welcomed to test existing features as well.
Members can also participate in the test day by adding tests to our automation test suite (Pulp
Smash) or by submitting fixes to the reported bugs. All issues and bugs will be filed and triaged to
prepare for 2.10 GA.

# When?

The Pulp QE team has tentatively decided to host the day on **Aug 05, 9am-5pm EDT**.

# Where?

The major communication approach will be through IRC. Participants are encouraged to join **channels
#pulp and #pulp-dev on [irc.freenode.net](https://freenode.net/)**. The **Pulp email list
pulp-list@redhat.com** can also be used for communications. In order to expand the audience of this
Test Day, members of both the open source community as well as downstream users will be notified via
email.

# Getting Ready

Here are some instructions for you to install or update your Pulp.

- Install a fresh Pulp 2.10 beta according to the [official documentation](http://docs.pulpproject.org/user-guide/installation/index.html).

- Optionally, you can use Ansible to install Pulp onto Fedora 23 by the following commands:

  ```bash
  #!/usr/bin/bash
  set -euo pipefail
  dnf update -y
  dnf install -y git vim ansible python-dnf libselinux-python
  mkdir code
  cd code
  git clone https://github.com/pulp/pulp_packaging
  cd pulp_packaging
  echo localhost > hosts
  ansible-playbook -i hosts --connection local -e pulp_version='2.9' \
      ci/ansible/pulp_server.yaml
  ```

- You can also update your current, non-production Pulp to Pulp 2.10 beta. Here is a link about how
  to upgrade to 2.8. You can follow the [instructions](http://docs.pulpproject.org/user-guide/release-notes/2.8.x.html#upgrade-instructions) to upgrade to 2.10.

- If you don't have your own instance of Pulp on hand, for internal Red Hat associates, we can
  provide you with a system. Just check in with preethi or chongrui on IRC.

- It is important to not use a production envrionment. Also note, Pulp does not support upgrading
  from a beta or RC to a newer beta, RC, or GA version. In other words you'll end up deleting your
Pulp testing envrionment after you are done.

# How to test

Now that you have a running instance of Pulp 2.10, you can start testing the new features and look
into the existing issues that can be verified.

## Sign up to test a new feature

Please enter your name, targeting features and OS/platform information on the spreadsheet. After
your tests, you can put the results on the
[spreadsheet](https://docs.google.com/spreadsheets/d/1XgD43QI7ELYWFI_eeZdWqHwtCZDywMX-uGTrSZJvPX0/edit#gid=12432565) as well.

# Automate tests

The Pulp QE team uses a python-based testing framework to automate testing features and and
regression testing. This framework is called Pulp Smash, and if you're interested in contributing
with your own automated tests for some of the new features, here's how to get started:

## Install Pulp Smash

Pulp Smash can be easily installed onto your system. Simply follow the [official documentation found
here](http://pulp-smash.readthedocs.io/en/latest/index.html). If you want a different approach to
how to install it, you can also read [this
post](http://www.pulpproject.org/2016/07/05/getting-started-on-pulp-and-pulp-smash-a-pulpqe-intern-summary/)
from our intern.

## Run the tests

Once you have Pulp Smash installed and configured, we highly recommend that you read through
[this](http://pulp-smash.readthedocs.io/en/latest/about.html#contributing) for a quick introduction
to the process of contributing with automated tests. Then you can:

- Manually test on Pulp 2.10 beta with API/CLI features.
- Develop Pulp Smash tests to help automated testing.
- Choose one of the existing test cases that need to be automated
  [here](https://github.com/PulpQE/pulp-smash/issues).

# Reporting bugs and issues

While playing with Pulp 2.10 Beta, you may encounter an issue or something that does not quite match
your expectations or the existing documentation. For these cases we kindly ask that you help us by:

- Opening a Pulp issue: <https://pulp.plan.io/projects/pulp/issues>
- Opening a Pulp Smash issue: <https://github.com/PulpQE/pulp-smash/issues>

# Test Day Report

After the completion of Test Day, Pulp QE  will deliver a summary via email containing information
about:

- What new features were tested?
- What existing features were tested?
- Number of Pulp issues opened
- Number of Pulp Smash issues created
- Number of PR/automation submitted
- Number of Pulp issues closed/reopened

Also, there will be a follow-up blog post on pulpproject.org with this summary.


# Who's available

The following PulpQE members will be available on IRC/freenode to provide help and guidance with any
questions you may have, as well as help with your tests, inform of known workarounds, or just chat
about the awesome new features:

- Chongrui Zhang (chongrui)
- Preethi Thomas (preethi)

The following  PulpQE members can help out with Pulp Smash automation as well:

- Chongrui Zhang (chongrui)
- Elyezer Rezende (elyezer)

The Pulp Development team will also help out by answering questions on IRC.

**We expect that Pulp 2.10 will be an amazing new release, and we look forward to seeing you join
us this Test Day!**

**Pulp QE Team**

