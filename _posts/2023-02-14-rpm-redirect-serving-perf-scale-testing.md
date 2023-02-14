---
title: RPM Content Service Performance and Scale Testing
author: Brian Bouterse
tags:
    - pulp_rpm, performance
---

## Goals

1. Help pulp_rpm users plan the number of pulp-content apps to serve a given traffic rate for their 
   object-storage backed installation.

2. Determine what the architectural scalability limit is for pulp_rpm content serving.

To achieve these goals in this blog post, I'll be doing two things:

* Characterize the relationship between sustained requests / sec to the pulp-content app and the 
  number of pulp-content apps without increases in latency.

* Identify rate limiting components in various test scenarios.


## Conclusions

We'll unpack how I reached these conclusions, but for those interested in just those, let's get them
stated right up front:

1. Using Redis caching for Pulp should allow for a cluster to horizontally scale to the size most if
   not all req / sec rates.
    * In doing so, just add more CPU for additional pulp-content processes
    * Make sure your Redis also doesn't run out of CPU. You could horizontally scale this component
      too.

2. The response times in all tests are well within the 2 second yum/dnf response timeout, which
   should ensure their redirects at least without any issue.


## Results - With Redis Caching

* Each pulp-content gunicorn worker process having exclusive access to an idle 2.50GHz Xeon CPU can
  serve 625 req / sec.
* The pulp-content gunicorn worker process uses a stable 16MB each.
* The pulp-content gunicorn worker is CPU bound with CPU directly correlating to req / sec.
* A single container Redis was able to serve about 3K req / sec with 20% CPU in use.
* postgreSQL was almost entirely unloaded and never spawned more than 1 process and never registered
  with `top` during any caching-enabled run.
* The maximum median response time in almost all runs was 25ms.
* The 95th percentile response time in almost all runs was 70ms.


## Results - Without Redis Caching

* Each pulp-content gunicorn worker process having exclusive access to an idle 2.50GHz Xeon CPU can
  serve 108 req / sec.
* The pulp-content gunicorn worker process uses a stable 16MB each.
* The pulp-content gunicorn worker is both 3/4 CPU and 1/4 DB bound with both resources closely
  correlating to req / sec.
* postgreSQL forked workers and began loading them.
* The maximum median response time in almost all runs was 16ms.
* The 95th percentile response time in almost all runs was 28ms.


## System Under Test

Now that we've seen the results, let's go through the system that produced them...

### Physical hardware All on AWS in EU-West-2

1 Load generator - m4.2xlarge instance:
* 8 64-bit CPUs, Intel(R) Xeon(R) Platinum 8259CL CPU @ 2.50GHz
* 32GB Ram

1 Pulp test machine - m4.2xlarge instance:
* 8 64-bit CPUs, Intel(R) Xeon(R) Platinum 8259CL CPU @ 2.50GHz
* 32GB Ram

Minio providing artifact storage also running on the system under test.


### Software Under Test

Pulp will be deployed as containers on the "Pulp Test Machine" hardware above. This was done using
the generic [docker-compose scale](https://github.com/pulp/pulp-oci-images/tree/latest/images/compose#running-with-docker-and-scaling)
feature.

* 1 container for pulp-api
* N containers for pulp-content
* 2 containers for pulp-worker
* 1 redis container
* 1 postgresql container

Pulp was deploying with commands like:

```shell
sudo docker-compose up --scale pulp_content=1 -d --force-recreate
sudo docker-compose up --scale pulp_content=2 -d --force-recreate
sudo docker-compose up --scale pulp_content=4 -d --force-recreate
```

Minio was deployed using its [docker-compose example](https://github.com/minio/minio/blob/master/docs/orchestration/docker-compose/docker-compose.yaml)
on the same machine as the system under test. Deploying minio separately as it is not actually under
test would have been better, however, these artifacts aren't going to actually be served during
testing, so running effectively unused containers should use a minimum amount of hardware resources
on the system under test.

The Pulp settings were configured to connect to minio by applying the diff below to [this file](https://github.com/pulp/pulp-oci-images/blob/latest/images/compose/assets/settings.py).
These changes are mostly taken from [these docs](https://docs.pulpproject.org/pulpcore/installation/storage.html#amazon-s3).
Note you need to manually create the Minio bucket named `pulp3` as specified by the config.

```diff
diff --git a/images/compose/assets/settings.py b/images/compose/assets/settings.py
index 54fcfbf..651873b 100644
--- a/images/compose/assets/settings.py
+++ b/images/compose/assets/settings.py
@@ -16,3 +16,16 @@ PUBLIC_KEY_PATH = "/etc/pulp/keys/container_auth_public_key.pem"
 PRIVATE_KEY_PATH = "/etc/pulp/keys/container_auth_private_key.pem"
 TELEMETRY = False
 STATIC_ROOT = "/var/lib/operator/static/"
+
+DEFAULT_FILE_STORAGE = "storages.backends.s3boto3.S3Boto3Storage"
+MEDIA_ROOT = ""
+AWS_ACCESS_KEY_ID = 'minioadmin'
+AWS_SECRET_ACCESS_KEY = 'minioadmin'
+AWS_S3_REGION_NAME = "eu-central-1"
+AWS_S3_ADDRESSING_STYLE = "path"
+S3_USE_SIGV4 = True
+AWS_S3_SIGNATURE_VERSION = "s3v4"
+AWS_STORAGE_BUCKET_NAME = "pulp3"
+AWS_S3_ENDPOINT_URL = 'http://nginx:9000'
+AWS_DEFAULT_ACL = "@none None"
```

The versions under test are: pulpcore 3.22.0 and pulp_rpm 3.18.9


### Architecture

Locust ---HTTP-Requests---> pulp-content

Locust <---302-Redirect--- pulp-content

Components:

* The load generator ([locust](https://locust.io/) with fasthttp backend)
* The pulp-content processes

Notes:

* The Locust test script will not follow the redirect. Minio is not the software under test, only 
  pulp-content is.
* The reverse proxy is entirely bypassed as we do not want to test nginx, only Pulp.


## Metrics

* sustained requests / sec - The number of redirects issued per second.
* latency median - Pulp's typical 302 redirect response time.
* latency 95th percentiles paired with the requests / sec - Pulp's majority case 302 redirect
  response time.
* redis container CPU percentage - Indicates how heavily loaded Redis is.
* PostgreSQL Process count - Indicates how heavily loaded PostgreSQL is. It forks more when
  postgreSQL is processing more load.
* PostgreSQL average CPU usage across all PostgreSQL processes. - Indicates how heavily loaded 
  postgreSQL is.
* Number of pulp-content Gunicorn workers. In practice, how many did gunicorn fork. - Indicates the
  actual number of processes handling requests.
* Average pulp-content Gunicorn workers CPU. Averaged across all the pulp-content gunicorn
  workers. - Indicates how heavily loaded the pulp-content processes are.


## Scenarios

For each scenario increase the number of Locust users until the pulp-content app no longer shows
increases in req/sec with additional increases in locust users.

1. 1 pulp-content process (2 gunicorn processes when heavily loaded). Redis caching enabled.
2. 2 pulp-content process (4 gunicorn processes when heavily loaded). Redis caching enabled.
3. 4 pulp-content process (8 gunicorn processes when heavily loaded). Redis caching enabled. This is
   the maximum number of cpus on the machine.
4. 2 pulp-content process (4 gunicorn processes when heavily loaded). Redis caching disabled. This
   is useful for comparing against scenario (2) to determine the impact of caching.


## Data Collected

See [this spreadsheet](../../../../files/pulp_rpm_redirection_perf_and_scale_blog_post/Sheet1.html)
for the raw data collected.

A few comments on the data:

* The number of requests per CPU is computed in Column T as (req / sec) / process count / CPU load
  coefficient. For example cell T2 is computed as `=M2/I2/(J2/100)`. This is what drives the
  conclusions of 625 and 108 req / sec for cache-enabled and cache-disabled respectively.
* The tests with up to 2 or 4 actual gunicorn workers (rows 2-25) are able to push the CPU averages
  a lot higher than 8 gunicorn workers (rows 27 - 34) because other services like docker overhead,
  Redis, etc. are able to run on the remaining 4 unused processors. Thus, the rows 2-25 are likely a
  better test because pulp isn't competing for hardware resources in those scenarios. The overall
  conclusions are built on these as these are expected to generalize to other systems more
  accurately.
* Postgresql percentage is missing in many places. `top` did not even register in these scenarios,
  so it's effectively zero.
* I didn't track Redis percentage initially, but given the direction relationship between traffic
  volume and Redis cpu %, and a maximum in testing of 22% at max req/sec, we can deduce the unfilled
  columns are lower than that.
* Locust users make a request, wait for a response, and the make another request. Keep in mind that
  as user count increases, latency also likely increases, and so linear increases in Locust user
  count produce sublinear increases in req / sec.
* The load generator hardware is never the rate limiting component. The low CPU percentage and
  memory fields confirm this.


### Appendix A: Test Setup

Sync with policy="immediate" a RHEL9 baseos. To do this:

1. Create a debug CDN cert with [these instructions](https://source.redhat.com/groups/public/release-engineering/release_engineering_rcm_wiki/how_to_generate_cdn_debug_certs).

2. Create and sync rhel9 baseos with these commands

```
pulp rpm remote create --name rhel9_baseos --url https://cdn.redhat.com/content/dist/rhel9/9/x86_64/baseos/os/ --policy immediate --client-cert @cdn_1056.pem --client-key @cdn_1056.pem --ca-cert @redhat-uep.pem
pulp rpm repository create --name rhel9_baseos --autopublish --remote rhel9_baseos
pulp rpm distribution create --base-path rhel9_baseos --name rhel9_baseos --repository rhel9_baseos
pulp rpm repository sync --name rhel9_baseos
```



### Appendix B: Locust Configuration

Locust is deployed on the load generator maching using its [docker-compose documentation](https://docs.locust.io/en/stable/running-in-docker.html)
and run with the command: `sudo docker-compose up --scale worker=16`.

The urls from the RHEL9 baseos are gathered into a text file using this script:

```
import argparse
import requests
from bs4 import BeautifulSoup
from urllib.parse import urljoin

def process_page(url):
    page = requests.get(url)
    soup = BeautifulSoup(page.content, "html.parser")
    links = soup.find_all("a")
    for link in links:
        if link.text == "../":
            continue
        link_url = urljoin(url, link["href"])
        if link_url.endswith(".rpm"):
            print(link_url)
        elif "://" in link_url:
            process_page(link_url)

parser = argparse.ArgumentParser(description='Get the full URLs of .rpm files at a specific url.')
parser.add_argument('base_url', help='The base url to fetch links from')
args = parser.parse_args()

base_url = args.base_url
process_page(base_url)
```

Here is the locustfile.py

```
import random
import re
import time

from locust import FastHttpUser, task


NUM_CONTENT_APPS = 2

START_PORT = 24737
END_PORT = START_PORT + NUM_CONTENT_APPS


class QuickstartUser(FastHttpUser):

    def on_start(self):
        with open("/mnt/locust/urls.txt") as file:
            self.urls = file.readlines()

    @task
    def random_rpm(self):
        original_url = random.choice(self.urls)
        new_port = str(random.randint(START_PORT, END_PORT - 1))
        new_url = re.sub(r':\d+', ':' + new_port, original_url)
        self.client.get(
            new_url,
            allow_redirects=False
        )
```


### Appendix C: Notes

#### Commands on system under test

Install docker https://docs.docker.com/engine/install/fedora/#install-using-the-repository
sudo dnf install docker-compose git
setup portainer via docker-compose
setup pulp via docker compose
install minio and configure pulp to talk to it
Make sure to create the bucket manually for pulp to use also


#### Commands on load generator

Install docker https://docs.docker.com/engine/install/fedora/#install-using-the-repository
sudo dnf install docker-compose
setup portainer via docker-compose
setup locust via docker-compose  https://docs.locust.io/en/stable/running-in-docker.html
