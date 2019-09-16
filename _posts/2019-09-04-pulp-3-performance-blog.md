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

##### pulp-plugins version

| Plugin | Version |
| ------------- | ------------- |
| pulpcore | 3.0.0rc4 |
| pulpcore-plugin | 0.1.0rc4  |
| pulp_file  | 0.1.0b1  |


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

| Tests | Run of | Time Taken(seconds) |
| ------------- | ------------- | ------------- |
| Single sync | Single Sync pulp_file repo w/ 100k units (run of 1) | 3944 |
| Single sync | Single Sync pulp_file repo w/ 100k units (run of 2) | 10150 |
| Single sync | Single Sync pulp_file repo w/ 100k units (run of 3) | 11152 |
| Concurrent sync  | 5 repos ,2 workers @ 24 CPUs  | 65497 |
| Concurrent sync  | 5 repos ,5 workers @ 24 CPUs  | 52372 |
| Single resync  | Sync repository which was synced before (run of 1)  | 36513 |
| Single resync  | Sync repository which was synced before (run of 2)  | 35694 |
| Single resync  | Sync repository which was synced before (run of 3)  | 35354 |
| Concurrent resync | 5 repos ,2 workers @ 24 CPUs  | 123091 |
| Concurrent resync | 5 repos ,5 workers @ 24 CPUs  | 43442 |
| Single publish | Single Publication of 100k unit repo version (run of 1)  | 226 |
| Single publish | Single Publication of 100k unit repo version (run of 2)  | 228 |
| Single publish | Single Publication of 100k unit repo version (run of 3)  | 230 |
| Concurrent publish  | 5 repos ,2 workers @ 24 CPUs  | 679 |
| Concurrent publish  | 5 repos ,5 workers @ 24 CPUs  | 248 |
| Single download  | Single unit Download Content Test (run of 1)  | 227 |
| Single download  | Single unit Download Content Test (run of 2)  | 228 |
| Single download  | Single unit Download Content Test (run of 3)  | 227 |
| Concurrent download | 5 processes  | 69605 |
| Concurrent download | 10 processes  | 69627 |
| Listing repo and units | 1 processes  | 33390 |
| Listing repo and units | 5 processes  | 33340 |
| Listing repo and units | 10 processes  | 33343 |
| Listing repo and units | 20 processes  | 33203 |

## Conclusion

After some quick inspection, would say we have found two concerns:

1. Syncing is getting slower with amount of data in the database (see Single sync results - 1st repo sync = 4k seconds, 2nd repo sync = failed to measure, 3rd repo sync = 10k seconds, 4th repo sync = 11k seconds)

2. Resyncing file repository that was already synced is very expensive (see Single resync - 36k seconds for 100k repo)



