---
title: Content Plugins
sidebar: home_sidebar
permalink: /content-plugins/
toc: true
---

## Content Plugins

After you install Pulp, you must add a content plugin for each content type that you want to manage. The following sections contain information about the available content plugins for both Pulp 2 and Pulp 3. If you do not find the plugin for the content type you want to manage, consider [writing a plugin](https://docs.pulpproject.org/plugins/plugin-writer/index.html).

## Plugin Changes between Pulp 2 and Pulp 3

* The ISO content support found in the Pulp 2 RPM plugin is now provided by the File plugin.
* The Docker plugin in Pulp 2 has been replaced by the Container plugin.
* Currently, there are no Pulp 3 plugins for Puppet and OSTree content.

<h2 class="page-header">Pulp 3 Content Plugin Features</h2>

Click the relevant tab of the content plugin that you want to learn more about.

<div class="row">
    <div class="col-lg-12">
        <ul id="myTab1" class="nav nav-tabs nav-justified">
            <li class="active"><a href="#rpm" data-toggle="tab"><i class="fa fa-cube"></i> RPM</a>
            </li>
            <li class=""><a href="#file" data-toggle="tab"><i class="fa fa-floppy-o"></i> File</a>
            </li>
            <li class=""><a href="#container" data-toggle="tab"><i class="fa fa-ship"></i> Container</a>
            </li>
            <li class=""><a href="#ansible" data-toggle="tab"><i class="fa fa-life-bouy"></i> Ansible</a>
            </li>
            <li class=""><a href="#deb" data-toggle="tab"><i class="fa fa-stop-circle"></i> Debian</a>
            </li>
            <li class=""><a href="#python" data-toggle="tab"><i class="fa fa-heart"></i> Python</a>
            </li>
            <li class=""><a href="#gem" data-toggle="tab"><i class="fa fa-play"></i> Gem</a>
            </li>
            <li class=""><a href="#cookbook" data-toggle="tab"><i class="fa fa-cutlery"></i> Cookbook</a>
            </li>
            <li class=""><a href="#maven" data-toggle="tab"><i class="fa fa-book"></i> Maven</a>
            </li>
        </ul>
        <div id="myTabContent" class="tab-content">
            <div class="tab-pane fade active in" id="rpm">
                <p>Manage RPM content</p>
                <ul>
                    <li>Create, sync and publish a repository with RPM Content including RPMs, Advisories, Modularity, and Comps.</li>
                    <li>Version content and rollback if necessary.</li>
                    <li>Download content on-demand when requested by clients to reduce disk space.</li>
                    <li>Upload local RPM content in chunks.</li>
                    <li>Add, remove, copy, and organize RPM content into various repositories.</li>
                    <li>Host content either locally or on S3.</li>
                    <li>View distributions served by pulpcore-content in a browser.</li>
                    <li>De-duplication of all saved content. </li>
                </ul>
            </div>
            <div class="tab-pane fade" id="file">
                <p>Manage File content</p>
                <ul>
                    <li>Sync File packages from a remote to local repository.</li>
                    <li>Upload your own files.</li>
                    <li>Publish and organize files.</li>
                </ul>
               </div>
            <div class="tab-pane fade" id="container">
                <p>Manage Container content</p>
                <ul>
                    <li>Synchronize container image repositories hosted on Docker-hub, Google Container Registry, Quay.io, and others in mirror or additive mode.</li>
                    <li>Version content and rollback if necessary.</li>
                    <li>Download content on-demand when requested by clients to reduce disk space.</li>
                    <li>Perform docker/podman pull from a container distribution served by Pulp.</li>
                    <li>Curate container images by whitelisting what is mirrored from an external repository.</li>
                    <li>Curate container images by creating repository versions with a specific set of images.</li>
                    <li> De-duplication of all saved content. </li>
                </ul>
            </div>
            <div class="tab-pane fade" id="ansible">
                <p>Manage Ansible roles and collections</p>

                <ul>
                    <li>Mirror a subset of roles on-premise.</li>
                    <li>Mirror all of Galaxy’s roles on-premise.</li>
                    <li>Store private Ansible roles on-premise.</li>
                    <li>Install roles from pulp_ansible using the ansible-galaxy CLI.</li>
                    <li>Version content and rollback if necessary.</li>
                    <li>Support for the new multi-role content type from Galaxy.</li>
                </ul>
            </div>
            <div class="tab-pane fade" id="deb">
                <p>Manage Debian content</p>
                <ul>
                    <li>Synchronize remote repository content and metadata locally.</li>
                    <li>Upload your own content.</li>
                    <li>Publish content to one or more repositories.</li>
                  </ul>
            </div>
            <div class="tab-pane fade" id="python">
                <p>Manage Python content</p>
                <ul>
                    <li>Synchronize Python packages from PyPI locally.</li>
                    <li>Upload your own Python packages.</li>
                    <li>Publish and organize Python packages just like PyPI.</li>
                    <li>Install Python packages using pip as published by Pulp.</li>
                </ul>
            </div>
            <div class="tab-pane fade" id="gem">
                <p>Manage RubyGem content</p>
                <ul>
                    <li>Synchronize remote repository content and metadata locally.</li>
                    <li>Upload your own content.</li>
                    <li>Publish content to one or more repositories.</li>
                  </ul>
            </div>
            <div class="tab-pane fade" id="maven">
                <p>Manage Maven content</p>
                <ul>
                <li>Synchronize packages from a remote to local repository.</li>
                <li>Upload your own Maven content.</li>
                <li>Publish and organize packages.</li>
                  </ul>
            </div>
            <div class="tab-pane fade" id="cookbook">
                <p>Manage Chef Cookbook content</p>
                <ul>
                    <li>Sync Cookbook content from a remote to local repository.</li>
                    <li>Upload your own content.</li>
                    <li>Publish and organize content.</li>
                </ul>
            </div>
        </div>
    </div>
</div>

## Pulp 3 Content Plugins Information

This table contains links to information and sources for all Pulp 3 content plugins. If a plugin is missing [contact us](https://www.redhat.com/mailman/listinfo/pulp-list).

| Pulp Plugin | Docs | Source | Tracker | Install with PyPI | Install with RPM |
|-------|--------|---------|--------|---------|-------- |--------- |
| Ansible plugin | <a href="https://docs.pulpproject.org/pulp_ansible/">Ansible plugin docs</a> | <a href="https://github.com/pulp/pulp_ansible">Ansible plugin source</a> | <a href="https://pulp.plan.io/projects/ansible_plugin?jump=welcome">Ansible plugin tracker</a> | <a href="https://pypi.org/project/pulp-ansible/">Yes</a> | No |
| Chef cookbook plugin | <a href="https://github.com/pulp/pulp_cookbook/blob/master/README.rst">Cookbook plugin docs</a> | <a href="https://github.com/pulp/pulp_cookbook">Cookbook plugin source</a> | <a href="https://github.com/pulp/pulp_cookbook/issues">Cookbook plugin tracker</a> | <a href="https://pypi.org/project/pulp-cookbook/">Yes</a> | No |
| Debian plugin | <a href="https://pulp-deb.readthedocs.io/en/latest/">DEB plugin docs</a> | <a href="https://github.com/pulp/pulp_deb/tree/master">DEB plugin source</a> | <a href="https://pulp.plan.io/projects/pulp_deb?jump=welcome">DEB plugin tracker</a> | <a href="https://pypi.org/project/pulp-deb/">Yes</a> | No |
| Container plugin | <a href="https://docs.pulpproject.org/pulp_container/">Container plugin docs</a> | <a href="https://github.com/pulp/pulp_container">Container plugin source</a> | <a href="https://pulp.plan.io/projects/pulp_container?jump=welcome">Container plugin tracker</a> | <a href="https://pypi.org/project/pulp-container/">Yes</a> | No |
| File | <a href="https://docs.pulpproject.org/pulp_file/">File plugin docs</a> | <a href="https://github.com/pulp/pulp_file">File plugin source</a> | <a href="https://pulp.plan.io/projects/pulp_file?jump=welcome">File plugin tracker</a> | <a href="https://pypi.org/project/pulp-file/">Yes<a/> | No |
| GalaxyNG plugin | <a href="https://github.com/ansible/galaxy_ng/blob/master/README.md">GalaxyNG plugin docs</a> | <a href="https://github.com/ansible/galaxy_ng">GalaxyNG plugin source</a> | <a href="https://github.com/ansible/galaxy_ng/issues">GalaxyNG tracker</a> | <a href="https://pypi.org/project/galaxy-ng/">Yes</a> | No |
| Gem plugin | <a href="https://github.com/pulp/pulp_gem/blob/master/README.rst">Gem plugin docs</a> | <a href="https://github.com/pulp/pulp_gem">Gem plugin source</a> | <a href="https://github.com/pulp/pulp_gem/issues">Gem plugin tracker</a> | <a href="https://pypi.org/project/pulp-gem/">Yes</a> | No |
| Maven plugin | <a href="https://github.com/pulp/pulp_maven/blob/master/README.rst">Maven plugin docs</a> | <a href="https://github.com/pulp/pulp_maven">Maven plugin source</a> | <a href="https://pulp.plan.io/projects/maven-plugin/">Maven plugin tracker</a> | <a href="https://pypi.org/project/pulp-maven/">Yes</a> | No |
| Python plugin | <a href="http://pulp-python.readthedocs.io/en/latest/">Python plugin docs</a> | <a href="https://github.com/pulp/pulp_python/">Python plugin source</a> | <a href="https://pulp.plan.io/projects/pulp_python?jump=welcome">Python plugin tracker</a> | <a href="https://pypi.org/project/pulp-python/">Yes</a> | No |
| RPM plugin | <a href="https://docs.pulpproject.org/pulp_rpm/">RPM plugin docs</a> | <a href="https://github.com/pulp/pulp_rpm/">RPM plugin source</a> | <a href="https://pulp.plan.io/projects/pulp_rpm?jump=welcome">RPM plugin tracker</a> | <a href="https://pypi.org/project/pulp-rpm/">Yes</a> | No |



<h2 class="page-header">Pulp 2 Content Plugins Features</h2>

At some point, Pulp 2 will become End of Life. For more information about migrating from Pulp 2 to Pulp 3, see the [migration plugin](https://pulp-2to3-migration.readthedocs.io/en/latest) documentation.

<div class="row">
    <div class="col-lg-12">
        <ul id="myTab" class="nav nav-tabs nav-justified">
            <li class="active"><a href="#rpm2" data-toggle="tab"><i class="fa fa-cube"></i> RPM</a>
            </li>
            <li class=""><a href="#deb2" data-toggle="tab"><i class="fa fa-stop-circle"></i> Debian</a>
            </li>
            <li class=""><a href="#python2" data-toggle="tab"><i class="fa fa-heart"></i> Python</a>
            </li>
            <li class=""><a href="#puppet2" data-toggle="tab"><i class="fa fa-tasks"></i> Puppet</a>
            </li>
            <li class=""><a href="#docker2" data-toggle="tab"><i class="fa fa-ship"></i> Docker</a>
            </li>
            </ul>
        <div id="myTabContent1" class="tab-content">
            <div class="tab-pane fade active in" id="rpm2">
                <p>Manage RPM content</p>
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
            <div class="tab-pane fade" id="deb2">
                <p>Manage DEB content</p>
                <ul>
                    <li>Supports DEB packages and metadata</li>
                    <li>Sync remote repository content and metadata locally</li>
                    <li>Upload your own content</li>
                    <li>Publish content to one or more repositories</li>
                    <li>Published content is installable with apt-get</li>
                    <li>Sign repository metadata </li>
                </ul>
            </div>
            <div class="tab-pane fade" id="python2">
                <p>Manage Python content</p>
                <ul>
                    <li>Sync Python packages from PyPI locally</li>
                    <li>Upload your own Python packages</li>
                    <li>Publish and organize Python packages just like PyPI</li>
                    <li>Install Python packages using pip as published by Pulp</li>
                </ul>
            </div>
            <div class="tab-pane fade" id="puppet2">
                <p>Manage Puppet content</p>
                <ul>
                    <li>Sync Puppet modules from Puppet Forge locally</li>
                    <li>Upload your own Puppet modules</li>
                    <li>Publish and organize Puppet modules just like Puppet Forge</li>
                    <li>Use the Puppet client to install Puppet modules published by Pulp</li>
                </ul>
            </div>
            <div class="tab-pane fade" id="docker2">
                <p>Manage Docker content</p>
                <ul>
                    <li>Sync Docker modules from the Docker Registry</li>
                    <li>Publish and organize Docker containers</li>
                    <li>Install Docker containers as published by Pulp and served with <a href="http://docs.pulpproject.org/plugins/crane/index.html">Crane</a></li>
                </ul>
            </div>
          </div>
    </div>
</div>

## Pulp 2 Content Plugins Information

This table contains links to information and sources for all Pulp 2 content plugins. If a plugin is missing [contact us](https://www.redhat.com/mailman/listinfo/pulp-list).

| Pulp Plugin | Docs | Source | Tracker | Install with PyPI | Install with RPM |
|-------|--------|---------|--------|---------|-------- |--------- |
| RPM plugin | <a href="https://docs.pulpproject.org/en/2.21/plugins/index.html#rpm">RPM plugin docs</a> | <a href="https://github.com/pulp/pulp_rpm/tree/2-master">RPM plugin source</a> | <a href="https://pulp.plan.io/projects/pulp_rpm?jump=welcome">RPM plugin tracker</a> | No | Yes |
| Debian plugin | <a href="https://github.com/pulp/pulp_deb/blob/2-master/README.md">DEB plugin docs</a> | <a href="https://github.com/pulp/pulp_deb/tree/2-master">DEB plugin source</a> | <a href="https://pulp.plan.io/projects/pulp_deb?jump=welcome">DEB plugin tracker</a> | No | Yes |
| Docker plugin | <a href="https://docs.pulpproject.org/en/2.21/plugins/pulp_docker/user-guide/installation.html">Docker plugin docs</a> | <a href="https://github.com/pulp/pulp_docker">Docker plugin source</a> | <a href="https://pulp.plan.io/projects/pulp_docker">Docker plugin tracker</a> | No | Yes |
| Python plugin | <a href="https://docs.pulpproject.org/en/2.21/plugins/pulp_python/user-docs/getting_started.html">Python plugin docs</a> | <a href="https://github.com/pulp/pulp_python/tree/2-master">Python plugin source</a> | <a href="https://pulp.plan.io/projects/pulp_python?jump=welcome">Python plugin tracker</a> | No | Yes |
| Puppet plugin | <a href="https://docs.pulpproject.org/en/2.21/plugins/pulp_puppet/user-guide/installation.html">Puppet plugin docs</a> | <a href="https://github.com/pulp/pulp_puppet">Puppet plugin source</a> | <a href="https://pulp.plan.io/projects/pulp_puppet">Puppet plugin tracker</a> | No | Yes |

Are we missing a plugin? Let us know via the pulp-dev@redhat.com mailing list.
