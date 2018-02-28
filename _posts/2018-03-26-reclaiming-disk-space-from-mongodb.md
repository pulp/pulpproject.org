---
title: Reclaiming Disk Space from MongoDB
author: David Davis
---
MongoDB can often eat up large amount of disk space especially in deployments that are under heavy
load. It's possible to reclaim some of this disk space by using Mongo's repair utility. Before you
start this process though, **it is highly recommended you back up your MongoDB database**.

First, check the amount of disk space being used by MongoDB by connecting to MongoDB and running the
db stats command:

```
$ mongo pulp_database
MongoDB shell version: 3.2.12
connecting to: pulp_database
> db.stats()
{
        "db" : "pulp_database",
        "collections" : 44,
        "objects" : 698,
        "avgObjSize" : 639.756446991404,
        "dataSize" : 446550,
        "storageSize" : 811008,
        "numExtents" : 0,
        "indexes" : 147,
        "indexSize" : 1474560,
        "ok" : 1
}
```

Here you can see my database is using only about 800 kilobytes as this is a new installation of
Pulp. Now the command we want to run to reclaim disk space is [the repairDatabase
command](https://docs.mongodb.com/manual/reference/command/repairDatabase/).

There are a couple important things to note though. First, depending on the size of your database,
this could take a long time. The second is that you'll need free space on your disk equal to the
size of your current database plus 2 GB.

With that out of the way, the next step is to stop your pulp services:

```
sudo systemctl stop goferd httpd pulp_workers pulp_celerybeat pulp_resource_manager pulp_streamer
```

Now, there are two ways to run the repair command. The first way is to use the MongoDB shell:

```
$ mongo pulp_database
MongoDB shell version: 3.2.12
connecting to: pulp_database
> db.repairDatabase()
{ "ok" : 1 }
```

The alternative is to run the repair command from the command line. Make sure to run this command as
the mongodb user instead of root to avoid problems with permissions. Also, you'll need to figure out
the path to your database which should be set in `/etc/mongod.conf`.

```
sudo -u mongodb mongod --dbpath /var/lib/mongodb --repairpath
```

After you run the repair command, you can check the size of the database again using `db.stats()`.
