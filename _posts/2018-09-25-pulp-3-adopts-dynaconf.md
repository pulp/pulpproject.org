---
title: Pulp 3 adopts dynaconf
author: Dennis Kliban
---
Pulp 3 now uses [dyanconf](https://dynaconf.readthedocs.io/en/latest/) to manage its settings.
dynaconf allows for storing settings in
[multiple file formats](https://dynaconf.readthedocs.io/en/latest/guides/examples.html).
By default, Pulp will look for settings in ``/etc/pulp/settings.py``. To specify another settings
file, set the ``PULP_SETTINGS`` environment variable. Each of the settings can also be set by
prepending ``PULP_`` to the name and setting it as an environment variable.
[Environment variables](https://dynaconf.readthedocs.io/en/latest/guides/environment_variables.html#environment-variables)
take precedence over all other configuration sources. Check out the
[Pulp 3 docs](https://docs.pulpproject.org/en/3.0/nightly/installation/configuration.html) for
more details.
