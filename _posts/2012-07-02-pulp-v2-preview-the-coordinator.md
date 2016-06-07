---
id: 687
title: 'Pulp v2 Preview: The Coordinator'
date: 2012-07-02T19:18:39+00:00
author: Jay Dobies
layout: post
guid: http://blog.pulpproject.org/?p=687
permalink: /2012/07/02/pulp-v2-preview-the-coordinator/
categories:
  - 2.0
  - Spotlight
---
<div style="font-style:italic">
  The Pulp team is gearing up for the first community release of the v2 code base. Over the next few weeks I&#8217;ll be highlighting some of the new features in the v2 release. As with any blog entry, this is scoped to a point in time and the specific details are subject to some change before release. The overall concepts, however, should remain consistent despite any tweaks we make.
</div>

# Introduction

One of the most important new additions to Pulp v2 is also one of the least visible. Pulp has a number of challenges in handling incoming requests related to both its support of multiple, concurrent users and the fact that there is the potential for very long running operations on the server (repo sync, for instance). We needed a way to prevent users from issuing commands that could corrupt the server without completely locking them out from making any changes until the resource became free.

The coordinator is a layer on top of our tasking subsystem that understands user requests and ensures that conflicting requests do not execute concurrently. In most cases, when a resource is locked, an incoming request will be postponed until it is safe to run. For example, while a repository is executing a sync, an incoming configuration update on that repository will be postponed until after the sync completes. The server still stores the update request and will automatically execute it when it is safe to do so; the user does not need to come back later and re-issue the request.

In rare cases, the coordinator will outright reject a request if it will never safely execute. The common example is if a request on a resource, for instance a repo sync or configuration update, is made while a delete request is queued for the resource. Since the coordinator will execute requests in the order in which they are made, the delete will execute before the newly requested operation. In such a case, the coordinator knows the second request will fail and will reject it entirely at request time rather than queueing it up.

Conflicts are not the only reason a task may not immediately execute. In the event the Pulp server is overly busy, a task may be postponed until the server has the available resources to complete it. The server is configured in such a way that this should only occur for intensive operations such as repository sync.

# Effect on the Client

What this translates to in terms of a user experience is the possibility that calls to the client will not execute immediately but rather be queued to run in the future. For example, if the repository named &#8220;demo&#8221; is in the process of synchronizing and an attempt is made to update its configuration, the following is displayed in the client:

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="none" style="font-family:monospace;">$ pulp-admin repo update --repo-id demo --only-newest false
Repository update postponed due to another operation. Progress on this task can
be viewed using the commands under "repo tasks".
&nbsp;
Resource:  repository - demo
Operation: update</pre>
      </td>
    </tr>
  </table>
</div>

It is possible that an operation will be blocked by more than one resource or resources of different types. In this case, the request is blocked because the repository &#8220;demo&#8221; is executing an operation that is performing some sort of &#8220;update&#8221; on the repository.

Given the same scenario, the following is displayed when an attempt is made to delete the repository:

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="none" style="font-family:monospace;">$ pulp-admin repo delete --repo-id demo                                  
Repository delete postponed due to another operation. Progress on this task can
be viewed using the commands under "repo tasks".
&nbsp;
Resource:  repository - demo
Operation: update</pre>
      </td>
    </tr>
  </table>
</div>

Again, the delete is blocked because the &#8220;demo&#8221; repository is in the process of updating the repository in some way.

At this point, the task queue for the repository contains, in order, a configuration update and a repository delete. If another operation is requested on the repository, such as a configuration update or another sync request, the server will outright reject the request; it is impossible for it to run because the delete will resolve before the new request would be fielded. The client output is below:

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="none" style="font-family:monospace;">$ pulp-admin repo update --repo-id demo --display-name "Rejected Update"
The requested operation conflicts with one or more operations already queued for
the resource. The following operations on the specified resources caused the
request to be rejected:
&nbsp;
Resource:  repository - demo
Operation: delete</pre>
      </td>
    </tr>
  </table>
</div>

This is considered an error condition to the client and a non-zero exit code will be returned. The error message indicates the operation was rejected because there is a task in the queue for repository &#8220;demo&#8221; that will execute an operation with &#8220;delete&#8221; permissions.

# Viewing Queued Tasks

While ensuring the integrity of the Pulp server, this functionality also complicates the user experience. With the possibility to queue up multiple operations to execute on a repository, it becomes necessary to view and potentially cancel queued tasks. The `repo tasks` section of commands provides these abilities.

From the earlier example, below is the output of the task list for the repository:

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="none" style="font-family:monospace;">$ pulp-admin repo tasks list --repo-id demo                
+----------------------------------------------------------------------+
                                 Tasks
+----------------------------------------------------------------------+
&nbsp;
Operations:  sync
Resources:   demo (repository)
State:       Running
Start Time:  2012-07-02T14:49:55Z
Finish Time: Incomplete
Result:      Incomplete
Task Id:     300046b3-c455-11e1-93f1-72d46940eff3
&nbsp;
Operations:  update
Resources:   demo (repository)
State:       Waiting
Start Time:  Unstarted
Finish Time: Incomplete
Result:      Incomplete
Task Id:     3001afa3-c455-11e1-93fa-72d46940eff3
&nbsp;
Operations:  delete
Resources:   demo (repository)
State:       Waiting
Start Time:  Unstarted
Finish Time: Incomplete
Result:      Incomplete
Task Id:     30029630-c455-11e1-9400-72d46940eff3</pre>
      </td>
    </tr>
  </table>
</div>

The first entry is the running task. The remainder of the tasks are listed in the order in which they will execute. Completed tasks are removed from this list.

More details on a task can be viewed by passing its task ID to the `repo tasks details` command. The extra information is primarily in the form of the progress report on the task (not all tasks support this). The sync task, for instance, will include its progress report when viewed through the details command:

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="none" style="font-family:monospace;">$ pulp-admin repo tasks details --task-id bbb155d4-c473-11e1-8312-72d46940eff3
+----------------------------------------------------------------------+
                              Task Details
+----------------------------------------------------------------------+
&nbsp;
Operations:   sync
Resources:    demo (repository)
State:        Running
Start Time:   2012-07-02T18:28:34Z
Finish Time:  Incomplete
Result:       Incomplete
Task Id:      bbb155d4-c473-11e1-8312-72d46940eff3
Progress:     
  Importer: 
    Content:  
      Details:       
        Delta Rpm: 
          Items Left:  0
          Items Total: 0
          Num Error:   0
          Num Success: 0
          Size Left:   0
          Size Total:  0
        File:      
          Items Left:  0
          Items Total: 0
          Num Error:   0
          Num Success: 0
          Size Left:   0
          Size Total:  0
        Rpm:       
          Items Left:  2731
          Items Total: 3107
          Num Error:   0
          Num Success: 376
          Size Left:   3147523487
          Size Total:  3455015673
        Tree File: 
          Items Left:  6
          Items Total: 6
          Num Error:   0
          Num Success: 0
          Size Left:   0
          Size Total:  0
      Error Details: 
      Items Left:    2737
      Items Total:   3113
      Num Error:     0
      Num Success:   376
      Size Left:     3147523487
      Size Total:    3455015673
      State:         IN_PROGRESS
    Errata:   
      State: NOT_STARTED
    Metadata: 
      State: FINISHED</pre>
      </td>
    </tr>
  </table>
</div>

Any non-running task may be cancelled by passing its task ID to the `repo tasks cancel` command as well.

# Summary

There are two important lessons from this article:

  1. Pulp is awesome and is smart enough to prevent you from screwing your server up.
  2. Most operations in the client may not run immediately but instead queue up a task on the server. The progress of these tasks can be viewed using the <core>repo tasks list</code> command.