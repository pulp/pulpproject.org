---
title: Why Does Copy Use Lots of Memory?
author: Michael Hrivnak
tags:
  - platform
---

Pulp user Richard Gray [wrote](https://www.redhat.com/archives/pulp-list/2017-February/msg00008.html)
to our [email list](https://www.redhat.com/mailman/listinfo/pulp-list)
to ask about high memory use during copy operations. He wrote:

> I have a local RPM repository that syncs from an internet mirror of the
> CentOS 7.3 updates repository. When I try to create a local copy of this
> mirror, the celery processes balloon, using all available memory until the
> job eventually fails

He included this helpful example:

```
$ pulp-admin rpm repo copy all --from-repo-id centos-7-updates-x86_64-live --to-repo-id centos-7-updates-x86_64-snapshots-20170216a
This command may be exited via ctrl+c without affecting the request.

[-]
Running...
An internal error occurred on the Pulp server:

RequestException: GET request on
/pulp/api/v2/tasks/776894cc-b524-4f2f-bd30-c8c4d8237c31/ failed with 500 -
[Errno 12] Cannot allocate memory
```

## Explanation

Thankfully this behavior can be explained, and there is even a work-around
available. Both are hinted at in the [REST API documentation](
http://docs.pulpproject.org/dev-guide/integration/rest-api/content/associate.html#copying-units-between-repositories).

The full explanation is rather lame, but honest. When you do a copy, you can
give pulp a [Criteria](http://docs.pulpproject.org/dev-guide/conventions/criteria.html#search-criteria)
object, which lets you match specific units to copy based on filters,
limit/skip, etc. Pulp evaluates that Criteria into a result set, and hands that
to the plugin (yum_importer in Richard's case) to do the actual work. That's
the API between the core of Pulp and a plugin, so it's difficult to change.

One of the things you can specify in a Criteria is a list of fields to load. By
default, it selects all of them. See where this is going? If you just tell Pulp
to copy all RPMs from repo A to B, it will load all of the data it has on each
RPM document into memory at the same time.

## Work-Around

The work-around is to pass a list of fields with your copy request to the REST
API. In most cases, passing just the list of unit key fields will suffice.

pulp-admin does this for you, but only if you are copying a specific type of
content. If you use the ``rpm repo copy all`` command as Richard did, the
request will match multiple types of content (RPM, DRPM, Errata, etc.).
pulp-admin can no longer pass a set of fields that will be appropriate for all
matched types.

We can see this in action by running a fake copy command with pulp-admin. I am
making up two repository names, because we just want to see the body of the
request sent to Pulp's REST API.

```
$ pulp-admin -vv rpm repo copy rpm -f foo -t bar
2017-02-17 14:21:30,384 - INFO - POST request to /pulp/api/v2/repositories/bar/actions/associate/ with parameters {"source_repo_id": "foo", "override_config": {}, "criteria": {"fields": {"unit": ["name", "epoch", "version", "release", "arch", "checksum", "checksumtype", "downloaded", "signing_key", "filename"]}, "type_ids": ["rpm"], "filters": {"unit": {}}}}
```

You can see the list of fields that pulp-admin specified.

## TL;DR

Copy one unit type at a time. Avoid ``pulp-admin rpm repo copy all``.

If using pulp-admin to copy a single type, it will ensure the field list is
appropriately limited.

If using the REST API, specify a list of fields starting with the unit key
fields for the content type you are copying. Add additional fields as needed.
