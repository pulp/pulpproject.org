---
title: Pulp 3 CLI - Call for Feedback
author: Melanie Corr
tags:
  - announcement
---

On the `pulp-dev` mailing list, we have discussed how the lack of a CLI might be a limitation or potential blocker to using Pulp 3. In our recent community survey, many community members expressed the desire for a Pulp 3 CLI also. At PulpCon 2020, there was a session dedicated to a Pulp 3 CLI discussion. In that session, there was a demo the initial POC for the Pulp 3 CLI. At this point, we need your help to progress with the development of a Pulp 3 CLI.

<script id="asciicast-vu3qzlzmpDEFV5SCz2WDO1psb" src="https://asciinema.org/a/vu3qzlzmpDEFV5SCz2WDO1psb.js" async></script>

We want to build a Pulp 3 CLI to suit the needs and requirements of the wider Pulp community. To do this, we need your feedback, thoughts, and suggestions on what we have put together so far. We want to avoid developing features without knowing if it will help you and your use cases.

If you're willing to install and test the current Pulp 3 CLI POC and let us know your feedback, for example, whether this would suit your deployment or whether there are specific gaps or pitfalls etc, this would greatly help shape the future direction of the Pulp 3 CLI development. For anyone who participates and provides feedback on the Pulp 3 CLI PoC, we would be very happy to ship some Pulp SWAG as a thank you!

## How to Help

To install the Pulp 3 CLI, complete the following instructions. If you spot any bugs or would like to request certain features, please [submit an issue](https://pulp.plan.io/projects/pulp-cli/issues). Please submit your [feedback](https://forms.gle/Qc3TDDuL6zeART649) and SWAG information in this form.

The source code for the Pulp CLI POC is [here](https://github.com/pulp/pulp-cli).

## Install the Pulp 3 CLI

Ensure that you are running Python 3.6 or higher and have the [virtual environment wrapper](https://pypi.org/project/virtualenvwrapper/).

Ensure that you are running Pulp 3.6 or higher.

To install the Pulp 3 CLI, complete the following steps.

Clone the pulp-cli git repository:

```
git clone https://github.com/pulp/pulp-cli.git
```

Create a virtual environment:

```
mkvirtualenv pulp-cli
```

Install the CLI in edit mode in that virtual environment:

```
pip install -e .
```

At this point, you should be able to use the `pulp` command.

## Pulp 3 CLI at PulpCon

If you're interested, you can watch the discussion of the Pulp CLI at PulpCon 2020 for more information:

<iframe width="560" height="315" src="https://www.youtube.com/embed/5bovhlosrPA" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
