---
title: Pulp 3.0 Core Beta 1 Release
author: Austin Macdonald
tags:
  - release
  - 3.0
---

## pulpcore 3.0 beta 1

Pulpcore 3.0 beta 1 has been released, and can be [installed from
PyPI](https://docs.pulpproject.org/en/3.0/nightly/installation/instructions.html). We recommend
getting started with the [file plugin](https://github.com/pulp/pulp_file/blob/master/README.rst).

[Pulp 3 documentation](https://docs.pulpproject.org/en/3.0/nightly/index.html) is built nightly.
Pulp 3 concepts and changes are covered in our [overview documentation](https://docs.pulpproject.org/en/3.0/nightly/overview/index.html).
If you find problems or have features you’d like to request please [file an issue](https://docs.pulpproject.org/en/3.0/nightly/bugs-features.html)

## Pulp 3 Highlighted New Features

- Relational database. We designed Pulp 3 to be SQL agnostic, it was developed with SQLite and
  PostgreSQL.
- Pulp 3 is installable with pip, and can be run on most linux systems.
- The set of content in each repository is versioned.
- Promotion and rollback are fast, synchronous calls.
- Auto-generated, always up-to-date [API reference
  documentation](https://docs.pulpproject.org/en/3.0/nightly/integration-guide/rest-api/index.html#pulpcore-rest-api)

## What to expect

#### Weekly Releases to PyPI

Releases of [pulpcore](https://pypi.org/project/pulpcore/) and
[pulpcore-plugin](https://pypi.org/project/pulpcore-plugin/) will continue to be announced on the
lists.

#### Changes will be communicated

All of the Pulp 3 APIs may change during the beta process. Breaking changes will be clearly
communicated in release notes, and on mailing lists when applicable.

Betas are not part of a supported upgrade path. Migrations from Pulp 2 are planned, but not yet
implemented. We do not support upgrades between betas.


#### Plugin betas

Keep an eye on pulp-list for announcements of plugins entering beta.
