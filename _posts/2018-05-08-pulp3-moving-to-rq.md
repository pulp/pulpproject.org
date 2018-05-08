---
title: Pulp 3 Moving to RQ for Tasking
author: Brian Bouterse
tags:
  - 3.0
---

Pulp adopted the multi-processing tasking system in Pulp 2.4. Since then, users have benefited from
the throughput and availability it provides, but core developers regularly receive bug reports with
symptoms like:

* Pulp stops processing tasks entirely
* A single task never starts

Ensuring the reliability of Pulp's tasking system and therefore Pulp itself is a paramount concern.
After careful consideration, the maintainers of Pulp's tasking system believe the best way to
resolve these reliability issues is by porting the Pulp 3.0 tasking system from using
[Celery](http://celery.readthedocs.io/en/latest/) to using [RQ](http://python-rq.org/docs/).

This change will be made on May 15, 2018, and will be released with pulpcore Beta 4 on May 16, 2018.

Pulp 2.y is being left as-is because the change is backwards incompatible, so we can't introduce
them due to [Semantic Versioning](http://semver.org/).

This post is divided into three sections: Reasoning, User Changes, Plugin Writer Changes

## Reasoning

Four primary reasons stood out motivating this decision.

1. Users are experiencing task-halting issues which users and developers alike cannot reproduce. We
   have resolved some of these, but there are some that are unresolved. New ones are reported
   periodically, so there is no end in sight.

2. The more time core developers spend fixing Celery, the less time we are improving Pulp and its
   plugins.

3. It's epically complicated for little benefit. The code of Celery itself is spread across Celery,
   Kombu, pyamqp, billiard, qpid.messaging, and other libraries. With RQ it's just a lot simpler.

4. RQ fulfills 100% of the feature needs of Pulp with no gaps.

More discussion that went into this decision can be found in the "Port Pulp3 to use RQ" thread on
the pulp-dev mailing list archives for
[March](https://www.redhat.com/archives/pulp-dev/2018-March/thread.html),
[April](https://www.redhat.com/archives/pulp-dev/2018-April/thread.html), and
[May](https://www.redhat.com/archives/pulp-dev/2018-May/thread.html).


## User Changes

### Redis replaces RabbitMQ/Qpid

RQ uses Redis as its backend, so with this change, Redis replaces RabbitMQ or Qpid in the Pulp
architecture.

### Run workers differently

With Celery you would run workers with:

```commandline
$ /path/to/python/bin/celery worker -A pulpcore.tasking.celery_app:celery -n resource_manager@%%h -Q resource_manager -c 1 --events --umask 18
$ /path/to/python/bin/celery worker -A pulpcore.tasking.celery_app:celery -n reserved_resource_worker-1@%%h -Q reserved_resource_worker-1 -c  --events --umask 18
```

With RQ, you'll run those same works with this instead:

```commandline
rq worker -n 'resource_manager@%h' -w 'pulpcore.tasking.worker.PulpWorker'
rq worker -n 'reserved_resource_worker_1@%h' -w 'pulpcore.tasking.worker.PulpWorker'
```

You'll want to update your process launcher scripts, e.g. systemd scripts to use these new worker
commands instead.

### Ensure DJANGO_SETTINGS_MODULE is set

Anywhere the worker is run, it explicitly needs the environment variable DJANGO_SETTINGS_MODULE set.
For example something like this in your environment or process launcher (e.g. systemd) definition:

```commandline
export DJANGO_SETTINGS_MODULE=pulpcore.app.settings
``` 

### Error handling differences

RQ workers exit when Redis is not available either on startup of a worker or if there is an existing
connection. This is a behavioral difference from Celery. It is recommended that your process
launcher, e.g. systemd be configured to wait-and-retry to restart the service if you want your RQ
workers to automatically recover and reconnect when Redis has an availability issue.

### TLS and Authentication

If you are using TLS or authentication with RabbitMQ/Qpid, you'll need to port that to Redis using
the Redis documentation. Pulp's settings.yaml file also contains Redis configuration settings to
set. 


## Plugin Writer Changes

### Dispatching Tasks

With Celery plugin writers would dispatch tasks using the `apply_async_with_reservation()` method on
the task object itself. It returns a `celery.result.AsyncResult`.

```python
import mytask
result = mytask.apply_async_with_reservation([reservations], args=(...), kwargs={...})
# result is a celery.result.AsyncResult instance tracking the task dispatched to the tasking system
```

With RQ, plugin writers dispatch tasks using the `enqueue_with_reservation` function provided by
`pulpcore.tasking.tasks`. This takes any Python function as its first argument, with all subsequent
arguments being similar to before. Here is an equivalent example:

```python
from pulpcore.tasking.tasks import enqueue_with_reservation
import mytask
result = enqueue_with_reservation(mytask, [reservations], args=(...), kwargs={...})
# result is a rq.job.Job instance tracking the task dispatched to the tasking system
```

### Defining Tasks

With Celery you have to decorate or subclass their objects to formally define something as a Task.
When used with Pulp, this was done using the `@shared_task(base=UserFacingTask)` as follows:

```commandline
@shared_task(base=UserFacingTask)
def mytask(arg1, arg2):
    # task code here
``` 

With RQ you don't have to indicate it's a task at all and it will behave the same. Here is the
equivalent example:

```commandline
def mytask(arg1, arg2):
    # task code here
```

### Travis Changes

You'll need to make a few changes to Travis like these:

* Use `redis-server` not `rabbitmq`
* Explicitly set `export DJANGO_SETTINGS_MODULE=pulpcore.app.settings` before starting the workers 
* Update the workers to use the new command 

NOTE: that we are waiting on an upstream release of RQ containing a necessary patch. In the meantime
manually install RQ *before* pulpcore is installed in Travis using:

`pip install git+https://github.com/rq/rq.git@3133d94b58e59cb86e8f4677492d48b2addcf5f8`

### Example

See the [pulp_file changes](https://github.com/pulp/pulp_file/pull/72) used to port pulp_file to be
compatible with RQ. It contains an example of all necessary plugin changes.


## Questions or feedback

Reach out via pulp-list or pulp-dev email lists, via @pulpproj on Twitter, or via #pulp or #pulp-dev
on Freenode.
