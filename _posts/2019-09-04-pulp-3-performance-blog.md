---
title: Pulp 3 Performance Blog
author: Imaanpreet Kaur
team: Red Hat Performance Team
members: Pradeep Surisetty, Jan Hutař, Imaanpreet Kaur
---

## Motivation

We want to investigate the performance of various Pulp3 operations including: Sync, Publish, Download, Content Listing, and Copy operations.Pulp team came up with a test plan and Red Hat Performance team tested these test cases. So, let’s discuss about the pulp3 performance in brief.

## Test environment

Pulp was installed on one CentOS7 (CentOS Linux release 7.5.1804 (Core)) system using this [playbook](https://github.com/pulp/ansible-pulp/blob/master/example-use/playbook.yml). 

##### pulp version

![version](https://user-images.githubusercontent.com/32102000/64325263-1a1c9380-cfc8-11e9-809c-210229c85887.png)

In order to monitor the pulp3 performance, grafana monitoring has been done with the help of custom [script](https://github.com/redhat-performance/satellite-monitoring/blob/master/adhoc-scripts/get_stats_from_grafana.py). 

## Tests

| Tests | Run of |
| ------------- | ------------- |
| Single sync | 3 |
| Concurrent sync  | 2  |
| Single resync  | 3  |
| Concurrent resync | 2  |
| Single publish | 3  |
| Concurrent publish  | 2  |
| Single download  | 3  |
| Concurrent download | 2  |
| Listing repo and units | 4  |
| Copy version | 2  |

## Methodology

Repos to test having files-count 100000 and created using this [script](https://github.com/redhat-performance/pulpperf/tree/master/scrips). In order to test the performance of pulp3, simple .py scripts are used and can be found [here](https://github.com/redhat-performance/pulpperf/tree/master/tests). Testing can be done on the pulp3 system using commands by specifying the test along with repos manually.

For Example:  Single Sync pulp_file repo w/ 100k units

#### Output 

~~~
Sync tasks
==========
/pulp/api/v3/tasks/4e04d054-e2ca-4a0e-8f88-3809a15d6484/
    name:    pulp_file.app.tasks.synchronizing.synchronize
    created:    2019-08-07T09:59:16.766175Z
    started:    2019-08-07T09:59:16.897303Z
    finished:    2019-08-07T11:05:00.733855Z
    state:    completed

   field                            min                            max
   _created    2019-08-07T09:59:16.766175Z    2019-08-07T09:59:16.766175Z
 started_at    2019-08-07T09:59:16.897303Z    2019-08-07T09:59:16.897303Z
finished_at    2019-08-07T11:05:00.733855Z    2019-08-07T11:05:00.733855Z

Sync tasks waiting time: {'samples': 1, 'min': 0.13, 'max': 0.13, 'mean': 0.13, 'stdev': 0.0}

Sync tasks service time: {'samples': 1, 'min': 3943.84, 'max': 3943.84, 'mean': 3943.84, 'stdev': 0.0}
~~~

To get basic monitoring overview, we have used [get_stats_from_grafana.py](https://github.com/redhat-performance/satellite-monitoring/blob/master/adhoc-scripts/get_stats_from_grafana.py) script which connects to Grafana and formats some statistical measures. To get the more details on the output of all the commands, refer to this [document](https://docs.google.com/document/d/1Tt3WiQUaugYFkvuxVHkCjRngcMxqeYdMEKer_eqWOxE/edit#).

#### Result-

![results](https://user-images.githubusercontent.com/32102000/64327344-cf9d1600-cfcb-11e9-81eb-bec84adc11c8.png)

Similarly, for all the test cases results are noted down [here](https://docs.google.com/spreadsheets/d/1i9YUNMjZH3I9vqfito4Hf7c-jJpptvsN4tDwlp4HvnU/edit#gid=0).

## Conclusion

After some quick inspection, would say we have found two concerns:

1. Syncing is getting slower with amount of data in the database (see Single sync results - 1st repo sync = 4k seconds, 2nd repo sync = failed to measure, 3rd repo sync = 10k seconds, 4th repo sync = 11k seconds)

2. Resyncing file repository that was already synced is very expensive (see Single resync - 36k seconds for 100k repo seems too long)



