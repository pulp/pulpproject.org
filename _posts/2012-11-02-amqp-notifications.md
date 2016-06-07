---
id: 873
title: AMQP Notifications
date: 2012-11-02T17:37:33+00:00
author: Michael Hrivnak
layout: post
guid: http://www.pulpproject.org/?p=873
permalink: /2012/11/02/amqp-notifications/
categories:
  - 2.0
  - Spotlight
---
# Introduction

[Last week](/2012/10/22/email-notifications/) I talked about Pulp&#8217;s ability to send notifications via email upon occurrence of specified events. This week I am going to show you how Pulp can send those same notifications to an AMQP message broker.

# How Is This Valuable?

[AMQP](http://en.wikipedia.org/wiki/Advanced_Message_Queuing_Protocol) is an industry standard for integrating separate systems, applications, or even components within an application through asynchronous messages. If you want to take programmatic action based on an event that happened inside Pulp, this feature is for you.

# How It Works

  1. When an event fires inside Pulp, the notification system applies whatever handlers have been setup for it. One of those handlers is for AMQP. The body of the event becomes the body of an AMQP message, and the type of the event becomes the &#8220;subject&#8221; (that&#8217;s AMQP lingo) of that message. The message is then sent over the network to a message broker.
  2. The message broker delivers the message to a [topic exchange](https://access.redhat.com/knowledge/docs/en-US/Red_Hat_Enterprise_MRG/1.1/html/Messaging_User_Guide/chap-Messaging_User_Guide-Exchanges.html#sect-Messaging_User_Guide-Exchange_Types-Topic_Exchange) (later I will show you how to specify a named exchange to use).
  3. The message is then sent to any clients that are subscribe to this exchange through queues.

# Setup

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="none" style="font-family:monospace;">$ pulp-admin event listener amqp create --help
Command: create
Description: create a listener
&nbsp;
Available Arguments:
&nbsp;
  --event-type - (required) one of "repo.sync.start", "repo.sync.finish",
                 "repo.publish.start", "repo.publish.finish". May be specified
                 multiple times. To match all types, use value "*"
  --exchange   - optional name of an exchange that overrides the setting
                 from server.conf</pre>
      </td>
    </tr>
  </table>
</div>

We have already talked about how to specify an event type, but here you can also specify an exchange name. If you don&#8217;t specify one, it will default to the value pulled from /etc/pulp/server.conf in the &#8220;[messaging]&#8221; section. If you don&#8217;t set one there either, Pulp will default to &#8220;amq.topic&#8221;, which is an exchange guaranteed to be available on any broker. Regardless of what name you choose (we suggest &#8220;pulp&#8221; as a reasonable choice), you do not need to create the exchange or take any action on the AMQP broker. Pulp will automatically create the exchange if it does not yet exist.

As for selecting event types, if you are unsure, I suggest going with &#8220;*&#8221; to select all of them. The client can choose which types of messages they want to subscribe to based on hierarchically matching against the event type (called a &#8220;subject&#8221; in AMQP). It is cheap and fast to send a message to a broker, so I like to fire and forget. Let the clients decide which subjects they care about. More on subject matching [here](https://access.redhat.com/knowledge/docs/en-US/Red_Hat_Enterprise_MRG/1.1/html/Messaging_User_Guide/chap-Messaging_User_Guide-Exchanges.html#sect-Messaging_User_Guide-Exchange_Types-Topic_Exchange).

Pulp already uses [Qpid](http://qpid.apache.org/) as an AMQP message broker for communication with consumers, so your Pulp installation probably has a broker setup and ready to go.

# Listening In

In the playpen of the Pulp repository, I created a [small utility](https://github.com/pulp/pulp/blob/master/playpen/qpid/listen.py) that lets you connect to a topic exchange and print messages to stdout.

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="none" style="font-family:monospace;">$ ./listen.py -h
usage: listen.py [-h] [-e EXCHANGE] [-s SUBJECT] [-a ADDRESS] [-p PORT] [-q]
&nbsp;
print messages sent to a qpid exchange
&nbsp;
optional arguments:
  -h, --help            show this help message and exit
  -e EXCHANGE, --exchange EXCHANGE
                        name of a qpid exchange (default: amq.topic)
  -s SUBJECT, --subject SUBJECT
                        message subject to bind to
  -a ADDRESS, --address ADDRESS
                        hostname to connect to (default:localhost)
  -p PORT, --port PORT  port to connect to (default: 5672)
  -q, --quiet           show message subject only</pre>
      </td>
    </tr>
  </table>
</div>

Let&#8217;s fire up the listener and see some messages come across. For our purposes, I am going to use quiet mode which will only show the subjects. Event bodies can get very detailed, and they haven&#8217;t changed since you saw them in my last post. After starting listen.py, I will start a repo sync, and then we will see events come across the topic exchange.

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="none" style="font-family:monospace;">$ ./listen.py -e pulp -q
pulp.server.repo.sync.start
pulp.server.repo.sync.finish
pulp.server.repo.publish.start
pulp.server.repo.publish.finish</pre>
      </td>
    </tr>
  </table>
</div>

All four events fired. What if we only care about publish events? Let&#8217;s specify a subject with a wildcard.

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="none" style="font-family:monospace;">$ ./listen.py -e pulp -q -s pulp.server.repo.publish.*
pulp.server.repo.publish.start
pulp.server.repo.publish.finish</pre>
      </td>
    </tr>
  </table>
</div>

All four events still hit the &#8220;pulp&#8221; exchange, but the client&#8217;s subscription only matched these two. Now let&#8217;s get fancy and only match &#8220;finish&#8221; events, whether for sync or publish.

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="none" style="font-family:monospace;">$ ./listen.py -e pulp -q -s pulp.server.repo.*.finish
pulp.server.repo.sync.finish
pulp.server.repo.publish.finish</pre>
      </td>
    </tr>
  </table>
</div>

As you can see, wildcard matching within a topic exchange could be very valuable.

# Summary

This feature allows the community to integrate their own systems and applications with Pulp in ways we might not even imagine using an open industry-standard protocol. As with last time, I invite you to please share with us if you have a use case for AMQP messaging with Pulp. Leave a comment here, post on our [email list](https://www.redhat.com/mailman/listinfo/pulp-list), or catch us on IRC (#pulp on freenode).