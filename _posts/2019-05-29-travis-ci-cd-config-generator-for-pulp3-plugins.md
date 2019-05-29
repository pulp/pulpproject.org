---
title: Travis CI/CD Config Generator for Pulp3 Plugins
author: Brian Bouterse
tags:
  - 3.0
---

Pulp 3 plugins are encouraged to Continuously Integrate (CI) and Continuously Deliver (CD) with
Travis CI, but maintaining a feature-rich CI/CD pipeline across the 9+ repositories is cumbersome.
As additional features and plugins are created, the matrix will only grow. Luckily @dkliban has
created the Travis Config Generator.


## Features

### Continuous Integration (with every commit)

* Install your plugin using the [Pulp Ansible Installer](https://github.com/pulp/ansible-pulp)
* Run all functional and unit tests on a matrix including:
  * Python 3.6
  * Python 3.7
  * PostgreSQL
  * MariaDB
* Test the docs build with `make html`
* Build Python and Ruby bindings and test them

### Continuous Deployment

* Release to PyPI via a pushed tag (after testing occurs)
* Release Bindings to PyPI and rubygems.org nightly (after testing occurs)
* Release Bindings to PyPI and rubygems.org via a pushed tag (after testing occurs)


## How does this work? (high level)

1. Configure the rubygems.org and pypi.org passwords as secrets through the Travis UI

2. You use a command line tool to generate travis files. The tool takes options to disable features.
   By default, all features are enabled.

3. Commit the generated files to your plugin code, and you're done!


## How do I get started?

The tool is included in the plugin_template for new plugins to use, but existing plugins can use it
by checking out the latest version from: https://github.com/pulp/plugin_template/

See the [docs in the plugin template](https://github.com/pulp/plugin_template#travis-configuration).


## How will my pipeline stay up to date?

1. Checkout the latest [https://github.com/pulp/plugin_template](https://github.com/pulp/plugin_template)
2. Rerun the appropriate `generate_travis_config.py` command for your plugin, for example:

```
./generate_travis_config.py ansible --pypi-username pulp
```


## How do I file improvements?

File feature requests or stories in [the issue tracker here](https://pulp.plan.io/issues/new). With
the tool stored in the plugin_template, please add the 'plugin_template' tag when you file.
