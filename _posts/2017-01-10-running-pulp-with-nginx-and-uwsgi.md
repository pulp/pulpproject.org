---
title: Running Pulp with Nginx and uWSGI
author: Brian Bouterse
tags:
  - platform
---
Recently while troubleshooting a [slow memory leak](https://pulp.plan.io/issues/2492), I wanted to
run Pulp with a webserver other than [httpd](https://httpd.apache.org/). I decided to look at
[nginx](http://nginx.org/) and [uWSGI](http://uwsgi-docs.readthedocs.io/en/latest/) to give some
memory comparisons versus httpd. I've always run Pulp using httpd so this was new for me.

Note, this example does not serve content; it only serves the Pulp API. httpd usually serves both
the API and content like rpms. For my purposes, I only needed to test the API, and I think serving
content will take a little bit of work (see below). Note also that this is an **experimental
setup**, so **use at your own risk**.

Usually, Pulp is being served with Apache httpd and the plugin
[mod_wsgi](https://code.google.com/archive/p/modwsgi/). The httpd is the web server, and mod_wsgi
contains the Python interpreter handling a WSGI call to return a response. In this case we want
nginx to be the webserver replacing Apache httpd, and we want uWSGI to be the WSGI handler.


#### Install Packages, Stop httpd

```
$ sudo dnf install uwsgi uwsgi-plugin-python nginx
$ sudo systemctl stop httpd
```

#### Configure Nginx

I configured TLS for the webserver directly in nginx.conf. I probably shouldn't do this, but Vagrant
makes it so easy to reset my dev environment. In the nginx.conf I enabled the TLS block and added
two custom parts.

First are the SSL certs:

```
        ssl_certificate "/etc/pki/tls/certs/apachecert.pem";  # the private key for the TLS conn
        ssl_certificate_key "/etc/pki/tls/private/apachekey.pem";  # key for the private cert
        ssl_client_certificate "/etc/pki/pulp/ca.crt";  # The CA cert to validate the client
        ssl_verify_client optional;  # Sets auth headers that Pulp expects
```

Second, a definition for the API url handler itself.:
 
```
        location /pulp/api/ {
            rewrite /pulp/api/(.*) /$1 break;
            include uwsgi_params;
            uwsgi_pass 127.0.0.1:8000;
            uwsgi_param REMOTE_USER $remote_user if_not_empty;
            uwsgi_param HTTP_AUTHORIZATION $http_authorization;
            uwsgi_param SSL_CLIENT_CERT $ssl_client_raw_cert if_not_empty;
        }
```

The headers cause nginx to set headers in specific ways that Pulp cares about. With the above config
pulp-admin allows for both basic and cert based auth. Basic auth work for requests work when using
<code>pulp-admin -u xxxxxx -p xxxxxx</code>. You can also <code>pulp-admin login -u admin</code>
and your certificate will allow additional calls to be made. After calling
<code>pulp-admin logout</code> you will not be able to make additional calls.

For the complete example (similar to vanilla nginx.conf), here is the whole TLS block in nginx.conf:

```
# Settings for a TLS enabled server.

    server {
        listen       443 ssl http2 default_server;
        listen       [::]:443 ssl http2 default_server;
        server_name  _;
        root         /usr/share/nginx/html;

        ssl_certificate "/etc/pki/tls/certs/apachecert.pem";
        ssl_certificate_key "/etc/pki/tls/private/apachekey.pem";
        ssl_client_certificate "/etc/pki/pulp/ca.crt";
        ssl_verify_client optional;
        ssl_session_cache shared:SSL:1m;
        ssl_session_timeout  10m;
        ssl_ciphers HIGH:!aNULL:!MD5;
        ssl_prefer_server_ciphers on;

        # Load configuration files for the default server block.
        include /etc/nginx/default.d/*.conf;

        location /pulp/api/ {
            rewrite /pulp/api/(.*) /$1 break;
            include uwsgi_params;
            uwsgi_pass 127.0.0.1:8000;
            uwsgi_param REMOTE_USER $remote_user if_not_empty;
            uwsgi_param HTTP_AUTHORIZATION $http_authorization;
            uwsgi_param SSL_CLIENT_CERT $ssl_client_raw_cert if_not_empty;
        }

        error_page 404 /404.html;
            location = /40x.html {
        }

        error_page 500 502 503 504 /50x.html;
            location = /50x.html {
        }
    }

```

#### Start nginx

After applying configuration changes be sure to restart nginx.

```
$ sudo systemctl restart nginx
```


#### uWSGI Config

I configured uWSGI with the following <code>pulp_uwsgi.ini</code> file:

```
[uwsgi]
socket = 127.0.0.1:8000
wsgi-file = /usr/share/pulp/wsgi/webservices.wsgi
env = DJANGO_SETTINGS_MODULE=pulp.server.webservices.settings
uid = apache
gid = apache
plugin = python
```

I then run the webserver in the foreground with <code>sudo uwsgi pulp_uwsgi.ini</code> which creates
this process tree:

```
[vagrant@dev ~]$ sudo ps -awfux | grep uwsgi
root     12109  0.6  0.2 118792  5700 pts/4    S+   23:47   0:00    |   \_ sudo uwsgi pulp_uwsgi.ini
apache   12110 15.6  3.6 665884 75564 pts/4    Sl+  23:47   0:00    |       \_ uwsgi pulp_uwsgi.ini
```

This could be daemonized as a next step. I did not do that. Docs could also be written on how to
configure this manually or with Vagrant. You can also increase the number of processes and workers,
but I went with the defaults which only spawns one worker.


#### What about Content?
To allow stored content to be served, there should be an additional location block added for nginx
to serve statically. To have feature parity with httpd it also needs to be able to run the custom
access scripts which check for cert based authorization access to content. This should be do-able
but its going to take some work.

Additionally, Pulp makes use of [mod_xsendfile](https://tn123.org/mod_xsendfile/) to serve content,
would also need replacing. I'm not sure what nginx has in that area, but it likely has something
also.

Also nginx configs would need to be put together to support pulp_streamer but that should also be
possibly by making correct configurations (no code changes).


#### Todos

Here are some todos I thought of in case others are interested in seeing this work continue.

* Creating a Pulp config for nginx (not in nginx.conf) and committing it to a repo
* Creating a pulp_uwsgi.ini file in a repo
* Adding docs on other webservers
* Testing. Things always need testing ;-)
