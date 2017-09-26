---
title: Pulp 3 Plugin API Alpha
author: Michael Hrivnak
tags:
    -3.0
---

You are invited to try the first alpha release of the Pulp 3 Plugin API! Pulp 3
includes a dedicated python package for the plugin API. This package is
versioned separately to communicate backward-compatibility, and it enables
developers to write plugins in as simple and straight-forward of a way as
possible. That package should contain everything you need to create a plugin.

The API is complete enough to try, and we would love your feedback. It is
possible to synchronize and publish repositories, and there are two examples of
working plugins for you to reference.

In particular, we would appreciate feedback on both the low-level and
high-level Importer APIs. The [pulp_example](https://github.com/pulp/pulp_example)
plugin demonstrates how to implement a synchronization workflow end-to-end that
uses the low-level API, with the plugin writer controlling the details. The
[pulp_file](https://github.com/pulp/pulp_file) plugin demonstrates an
importer with equivalent behavior that uses the high-level API, in which case
the API handles many details and the plugin writer is responsible for much
less.

Scope
-----

Pulp 3 has enough core functionality in place to exercise these plugins through
the REST API. But we would like to emphasize that this alpha release is focused
on the plugin writer API only. Please consider the rest of Pulp 3’s behavior to
be “under construction” for now, and we will invite broader feedback with a
future alpha release.

Also please note that the plugin API itself is not yet complete. In particular,
the “copy” feature is missing, and “upload” will see major improvements. But
sync and publish are ready to try!

Frequency
---------

Look for alpha releases to appear regularly [on PyPI](https://pypi.python.org/pypi/pulpcore-plugin).
We will be targeting a weekly cadence during the alpha phase.

Get Started
-----------

Ready to learn more? Start by
[installing Pulp core](https://docs.pulpproject.org/en/3.0/nightly/installation/instructions.html)
to just work on plugins, or set up a
[full dev environment](https://docs.pulpproject.org/en/3.0/nightly/contributing/dev-setup/index.html)
to also possibly work on Pulp Core.

Then visit the
[plugin writer guide](https://docs.pulpproject.org/en/3.0/nightly/plugins/plugin-writer/index.html).

We encourage sending questions and feedback to the
[pulp-dev@redhat.com](https://www.redhat.com/mailman/listinfo/pulp-dev) email
list. From discussions there, we can collaborate on filing bug reports and RFEs
as appropriate.
