---
title: Homepage
sidebar: home_sidebar
type: homepage
toc: false
homepage: true
---

<div style="width:70%;text-align:center;margin:0 auto;margin-bottom:60px;">
  <p>
    <h2>Fetch, Upload, Organize, and Distribute Software Packages.</h2>
  </p>
</div>

Pulp is a platform for managing repositories of software packages and making them available to a
large number of consumers. Pulp can locally mirror all or part of a repository, host your own
software packages in repositories, and manage many types of content from multiple sources in one
place.

Pulp has a [REST API](https://docs.pulpproject.org/dev-guide/integration/rest-api/index.html) and
[command line interface](https://docs.pulpproject.org/user-guide/admin-client/index.html) for
management.

Pulp is free and open-source, and we invite you to [join us on GitHub](https://github.com/pulp/).

Follow us on [Twitter](https://twitter.com/pulpproj) or check out our [Youtube
channel](https://www.youtube.com/PulpProject).

&nbsp;

<table style="width:70%;text-align:left;margin:0 auto;margin-bottom:10px;">
  <col style="width:35%">
  <col style="width:35%">
  <thead>
    <tr>
      <th>Quickly Try Pulp</th>
      <th>Fully Install Pulp</th>
    </tr>
  </thead>
  <tr>
    <td>Pulp 3 RC: Download and run<br />
        <a href="https://raw.githubusercontent.com/pulp/pulp-operator/master/insta-demo/pulp-insta-demo.sh">pulp-insta-demo.sh</a></td>
    <td>Pulp 2 Stable: <a href="https://docs.pulpproject.org/en/2.21/user-guide/installation/index.html">Instructions</a><br />
        Pulp 3: <a href="https://docs.pulpproject.org/installation/index.html">Instructions</a></td>
  </tr>
</table>

<h2 class="page-header">Supported Types</h2>
<div class="row">
    <div class="col-lg-12">
        <ul id="myTab" class="nav nav-tabs nav-justified">
            <li class="active"><a href="#rpm" data-toggle="tab"><i class="fa fa-cube"></i> RPM</a>
            </li>
            <li class=""><a href="#deb" data-toggle="tab"><i class="fa fa-stop-circle"></i> Debian</a>
            </li>
            <li class=""><a href="#python" data-toggle="tab"><i class="fa fa-heart"></i> Python</a>
            </li>
            <li class=""><a href="#puppet" data-toggle="tab"><i class="fa fa-tasks"></i> Puppet</a>
            </li>
            <li class=""><a href="#docker" data-toggle="tab"><i class="fa fa-ship"></i> Docker</a>
            </li>
            <li class=""><a href="#ostree" data-toggle="tab"><i class="fa fa-code-fork"></i> OSTree</a>
            </li>
        </ul>
        <div id="myTabContent" class="tab-content">
            <div class="tab-pane fade active in" id="rpm">
                <p>Manage and deliver RPM content</p>
                <ul>
                    <li>Supports RPMs, DRPMs, SRPMs, Errata, Kickstart Trees, and repository metadata</li>
                    <li>Sync remote repository content and metadata locally</li>
                    <li>Upload your own content</li>
                    <li>Publish content to one or more repositories</li>
                    <li>Published content is installable with yum/dnf</li>
                    <li>On demand fetching of content allowing repositories to be synced and published without storing everything locally</li>
                    <li>Fetch content protected by basic or certificate based authentication</li>
                    <li>Protect content with certificates to be used by yum/dnf clients</li>
                </ul>
            </div>
            <div class="tab-pane fade" id="deb">
                <p>Manage and deliver DEB content</p>
                <ul>
                    <li>Supports DEB packages and metadata</li>
                    <li>Sync remote repository content and metadata locally</li>
                    <li>Upload your own content</li>
                    <li>Publish content to one or more repositories</li>
                    <li>Published content is installable with apt-get</li>
                    <li>Sign repository metadata </li>
                </ul>
            </div>
            <div class="tab-pane fade" id="python">
                <p>Manage and deliver Python content</p>
                <ul>
                    <li>Sync Python packages from PyPI locally</li>
                    <li>Upload your own Python packages</li>
                    <li>Publish and organize Python packages just like PyPI</li>
                    <li>Install Python packages using pip as published by Pulp</li>
                </ul>
            </div>
            <div class="tab-pane fade" id="puppet">
                <p>Manage and deliver Puppet content</p>
                <ul>
                    <li>Sync Puppet modules from Puppet Forge locally</li>
                    <li>Upload your own Puppet modules</li>
                    <li>Publish and organize Puppet modules just like Puppet Forge</li>
                    <li>Use the Puppet client to install Puppet modules published by Pulp</li>
                </ul>
            </div>
            <div class="tab-pane fade" id="docker">
                <p>Manage and deliver Docker content</p>
                <ul>
                    <li>Sync Docker modules from the Docker Registry</li>
                    <li>Publish and organize Docker containers</li>
                    <li>Install Docker containers as published by Pulp and served with <a href="http://docs.pulpproject.org/plugins/crane/index.html">Crane</a></li>
                </ul>
            </div>
            <div class="tab-pane fade" id="ostree">
                <p>Manage and deliver OSTree content</p>
                <ul>
                    <li>Sync OSTree branches from remote OSTrees</li>
                    <li>Publish and organize OSTree branches</li>
                    <li>Use OSTree compatible clients to install OSTree repositories published by Pulp</li>
                </ul>
            </div>
        </div>
    </div>
</div>
