---
title: Pulp 3 Concurrency Testing
author: Grant Gainey
tags:
  - blog
---

Syncing content into Pulp 3 is a time-consuming process. Fortunately, you can specify that content-artifacts are retrieved in parallel to minimize time by maximizing bandwidth utilization. This parallelism is accomplished by specifying the `download_concurrency` value on a given Remote.

Pulp 3’s default value for `download_concurrency` is 20. This can put an undue burden on the server the remote is retrieving content from - sometimes to the point of causing such a server to begin throttling or outright failing Pulp 3’s requests.

Rather than blindly changing that default, we would like to have some idea of the performance impact of various settings of `download_concurrency`, as noted in Redmine issue [7212](https://pulp.plan.io/issues/7212)

This document describes a set of tests, their environment, and some results, in search of that impact.

## Methodology

* Pick a repository of significant size and complexity, for example, [https://mirror.fileplanet.com/centos/7/os/x86_64/](https://mirror.fileplanet.com/centos/7/os/x86_64/)
* Sync its content, using `sync-immediate`, into a ‘clean’ Pulp 3 instance, at `download_concurrency` values of 20, 15, 10, 7, 5, and 3
* For each level, repeat the sync at least three times to average out general Internet non-determinism
* Find the average sync-time for a given concurrency level

## Environment

### Hardware

* Intel Core i7-6700 (4Ghzx8)
* 32 GB memory
* 1 TB Western Digital 7200RPM drive
* Google Fibre network connection, ~900Mbps up/down
* Local network has frequent high internet use, for example, Twitch streaming, video, high-bandwidth gaming, etc.
* System under test was not dedicated to the test process, and was running other loads

### Software

* Fedora 31
* Tests run on the `pulplift` vagrant box, `pulp3-source-fedora31`
* `pulpcore` and `pulp_rpm master` as of 2020-07-25

## Caveats

* The higher the concurrency, the heavier the load on the Pulp 3 instance
* Connection-bandwidth of the Pulp 3 instance is a limiting factor no matter how high concurrency is set
* Connection-bandwidth of the source server is a limiting factor no matter how high concurrency is set
* Internet connectivity is subject to arbitrary changes

## Results

Runs were executed on 2020-07-25 and 07-26 and resulted in the following observations:

* Decreasing concurrency by a factor of four (from twenty to five) did not quite double the sync-time (10:39 to 18:15).
* Reducing it to three results in nearly tripling the sync-time (to 29:12)


![Concurrency Testing Graph](/images/concurrency-testing/pulp3-concurrency-testing-graph.png)

Horizontal axis ticks are 20/15/10/7/5/3 concurrency.

As can be seen from the chart, while reducing concurrency does impact performance, there is a distinct knee below `download_concurrency=5`.

## Conclusion

In the context of this specific test, on this specific weekend, with this specific hardware - it would seem that a concurrency of seven or five, while having an impact on sync-time, still results in acceptable results. Going below five starts to have a disproportionate effect.

It would therefore appear to be useful to reduce Pulp 3’s default concurrency to, but not lower than, five, based on these results.

## Further Experiments

* Repeat the experiment using on_demand instead of immediate
* Monitor the load-average of the Pulp 3 instance under various concurrency levels
* Monitor the Pulp 3 instance’s memory usage under different concurrency levels
* Monitor the performance of postgresql under different concurrency levels (although the load of moving the actual bits across the network is likely to swamp that metric)
* Test the same sync against different source sites, for example, what impact does latency have? are there source-bandwidth limitations?
* Test when syncing onto an SSD instead of HDD

## Raw Data

```
concurrency,avg duration,duration,start,finish
3,,0:29:07,2020-07-25 16:48:11,2020-07-25 17:17:18
3,,0:27:32,2020-07-25 17:21:37,2020-07-25 17:49:09
3,0:29:12,0:30:56,2020-07-25 19:00:45,2020-07-25 19:31:41
5,,0:18:29,2020-07-24 20:45:03,2020-07-24 21:03:32
5,,0:17:47,2020-07-24 21:41:11,2020-07-24 21:58:59
5,0:18:15,0:18:29,2020-07-25 13:07:23,2020-07-25 13:25:52
7,,0:15:00,2020-07-25 23:15:42,2020-07-25 23:30:42
7,,0:15:36,2020-07-26 0:45:38,2020-07-26 1:01:14
7,0:15:30,0:15:53,2020-07-26 3:01:19,2020-07-26 3:17:12
10,,0:11:56,2020-07-25 14:38:27,2020-07-25 14:50:23
10,,0:15:00,2020-07-25 15:01:04,2020-07-25 15:16:04
10,0:13:33,0:13:43,2020-07-25 15:53:02,2020-07-25 16:06:45
15,,0:12:46,2020-07-25 21:43:20,2020-07-25 21:56:06
15,,0:12:44,2020-07-25 22:20:47,2020-07-25 22:33:31
15,0:12:31,0:12:04,2020-07-25 22:37:57,2020-07-25 22:50:01
20,,0:10:36,2020-07-24 18:51:45,2020-07-24 19:02:21
20,,0:10:58,2020-07-24 20:14:53,2020-07-24 20:25:50
20,0:10:39,0:10:23,2020-07-24 20:27:51,2020-07-24 20:38:14
```
