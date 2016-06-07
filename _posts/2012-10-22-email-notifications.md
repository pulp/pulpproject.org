---
id: 847
title: Email Notifications
date: 2012-10-22T14:36:26+00:00
author: Michael Hrivnak
layout: post
guid: http://website-pulp.rhcloud.com/?p=847
permalink: /2012/10/22/email-notifications/
categories:
  - 2.0
  - Spotlight
---
# Introduction

Did you know that Pulp has an event system? You can subscribe to specific events and be notified in several ways. This has been a popular feature because it allows other projects to integrate with Pulp, and in cases like email notification, it allows humans to monitor Pulp&#8217;s server activity. Today we are going to look at receiving notifications by email.

# Diving In

Let&#8217;s take a look at the event listener system in the command line interface.

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="none" style="font-family:monospace;">$ pulp-admin event listener --help
Usage: pulp-admin listener [SUB_SECTION, ..] COMMAND
Description: manage server-side event listeners
&nbsp;
Available Sections:
  amqp    - manage amqp listeners
  email   - manage email listeners
  restapi - manage rest-api listeners
&nbsp;
Available Commands:
  delete - delete an event listener
  list   - list all of the event listeners in the system</pre>
      </td>
    </tr>
  </table>
</div>

Next week I will write about the AMQP notification type, but today we will focus on email. From this section, you can list all event listeners and delete specific listeners. To create and update, we drill down by type.

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="none" style="font-family:monospace;">$ pulp-admin event listener email create --help
Command: create
Description: create a listener
&nbsp;
Available Arguments:
&nbsp;
  --event-type - (required) one of "repo.sync.start", "repo.sync.finish",
                 "repo.publish.start", "repo.publish.finish". May be specified
                 multiple times. To match all types, use value "*"
  --subject    - (required) text of the email's subject
  --addresses  - (required) this is a comma separated list of email addresses
                 that should receive these notifications. Do not include spaces.</pre>
      </td>
    </tr>
  </table>
</div>

To add an email notifier, you must specify what types of events to listen to, what the email subject should be, and who should receive the emails. Let&#8217;s create an email listener now.

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="none" style="font-family:monospace;">$ pulp-admin event listener email create --event-type="repo.sync.start" --subject="pulp notification" --addresses=someone@redhat.com
Event listener successfully created
&nbsp;
$ pulp-admin event listener list
Event Types:       repo.sync.start
Id:                5081a42ce19a00ea4300000e
Notifier Config:   
  Addresses: someone@redhat.com
  Subject:   pulp notification
Notifier Type Id:  email</pre>
      </td>
    </tr>
  </table>
</div>

# Settings

Take a look in /etc/pulp/server.conf at the &#8220;email&#8221; section, and read the comments. For testing, I use port 1025 on localhost and run the python command as suggested to run a dummy MTA. (see below for example)

In production, it is strongly recommended that Pulp submit email to an MTA on localhost, or at least a highly available MTA on the local network. Like many applications, Pulp makes no effort to retry if the MTA is unavailable or rejects a message.

# Email Content

The emails themselves are bare-bones and not designed for luxury. In fact, the body of the email is simply a pretty-printed JSON representation of the actual event body. The messages have all of the relevant information that is available, but don&#8217;t expect a friendly greeting. That said, we want to improve this in the future, and we would love to hear about your use case. Let us know what format would be valuable to you.

In another window I ran:

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="none" style="font-family:monospace;">pulp-admin rpm repo sync run --repo-id=pulp2</pre>
      </td>
    </tr>
  </table>
</div>

When the sync started, an email was sent whose output looked like this:

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="none" style="font-family:monospace;">$ python -m smtpd -n -c DebuggingServer localhost:1025
---------- MESSAGE FOLLOWS ----------
Content-Type: text/plain; charset="us-ascii"
MIME-Version: 1.0
Content-Transfer-Encoding: 7bit
Subject: pulp notification
From: no-repy@your.domain
To: someone@redhat.com
X-Peer: 127.0.0.1
&nbsp;
{
  "call_report": {
    "task_group_id": "aaa8f2ec-964c-4d62-bba9-3191aad9c3ea", 
    "exception": null, 
    "task_id": "79be77a8-1a20-11e2-aeb9-1803731e94c4", 
    "tags": [
      "pulp:repository:pulp2", 
      "pulp:action:sync"
    ], 
    "reasons": [], 
    "start_time": "2012-10-19T19:09:16Z", 
    "traceback": null, 
    "schedule_id": null, 
    "finish_time": null, 
    "state": "running", 
    "result": null, 
    "progress": {}, 
    "principal_login": "admin", 
    "response": "accepted"
  }, 
  "event_type": "repo.sync.start", 
  "payload": {
    "repo_id": "pulp2"
  }
}
------------ END MESSAGE ------------</pre>
      </td>
    </tr>
  </table>
</div>

# Summary

Notification by email is a useful way to integrate Pulp with other systems, and it is the method of choice for notifying humans. For more powerful integration options, be sure to read my next post about our AMQP notifier.

Are notification features valuable to you? Tell us about your use case in a comment here, on our [email list](https://www.redhat.com/mailman/listinfo/pulp-list), or on IRC (#pulp on freenode).