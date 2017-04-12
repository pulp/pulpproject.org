---
title: How to make a Pulp Vagrant env with a very large disk
layout: post
author: Dennis Kliban
---
Vagrant has significantly lowered the barrier to starting on Pulp development. However, when I
needed to reproduce an error that only occured when syncing a large repository, I discovered that
the default size of the the virtual machine created from the Vagrantfile in the devel repository
did not have a large enough disk to handle such a use case. The first step in resolving this
problem was an update to my Vagrantfile specifying a larger disk size. Once I did that I
discovered that the default location where libvirt stores disk images backing virtual machines
did not have the amount of space I needed. However, my home directory did have the space. So I
configured libvirt to use my home directory for storage. Below are the notes that I saved from
that experience.

{% highlight bash %}
# change user that libvirt runs as
vim /etc/libvirt/qemu.conf

   user = "dkliban"
   group= "dkliban"

# update the default storage pool for libvirt
sudo virsh pool-edit default

# Edit the xml in the editor that is opened by above command
# Update the path, user, group, and label tags based on the example below
# user and group 1000 are 'dkliban' on my machine

<pool type='dir'>
  <name>default</name>
  <uuid>412c1740-7d48-427c-9ab7-4fcea6305670</uuid>
  <capacity unit='bytes'>192459673600</capacity>
  <allocation unit='bytes'>111755284480</allocation>
  <available unit='bytes'>80704389120</available>
  <source>
  </source>
  <target>
    <path>/home/dkliban/libvirtimages</path>
    <permissions>
      <mode>0711</mode>
      <owner>1000</owner>
      <group>1000</group>
      <label>unconfined_u:object_r:user_home_t:s0</label>
    </permissions>
  </target>
</pool>

# exit the editor and look output from the command. If validation passes, you should see a message saying the pool has been updated.
# restart libvirtd
sudo systemctl restart libvirtd
{% endhighlight %}

At this point every time I start a virtual machine using libvirt, the disk image backing that
machine will be stored in `/home/dkliban/libvirtimages`.