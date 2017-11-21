---
layout: post
title: Migrating ElasticSearch across versions and clusters
author: Michał "rysiek" Woźniak
tags: ["ElasticSearch", "Migration"]
# categories: ["Sysadmin"]
---

Migrating data between ES clusters might seem like a simple thing -- after all, there are [dedicated tools](http://tech.taskrabbit.com/blog/2014/01/06/elasticsearch-dump/) for that. Or one could [use logstash with a simple config](https://stackoverflow.com/questions/17884581/elasticsearch-how-to-copy-data-to-another-cluster/26295832#26295832).

Things get a bit hairy, however, when the source and destination cluster versions differ wildly. Say, like `0.90.3` and `1.7.3`. And when you don't happen to have any admin access to the source cluster (only via [`HTTP`](https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-http.html) and [`transport`](https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-transport.html) interfaces).And when the source cluster is misconfigured in just a slight but annoying manner...

## What did not work

 - `elasticdump`

[ElasticDump](http://tech.taskrabbit.com/blog/2014/01/06/elasticsearch-dump/) was the first and obvious thing to try, but apparently it only supports migrations between clusters running ElasticSearch 1.2 and higher. So, that's a no-go.

 - `logstash`

Apparently [one can use logstash](http://stackoverflow.com/a/26295832) to migrate data between clusters. Unfortunately [this solution did not work, either](https://discuss.elastic.co/t/migrating-data-off-of-es-0-90-3-and-into-es-1-7-x/35471/).

## What did work

Please keep in mind that this procedure worked for us, but it doesn't have to work for you. Specifically, if the source is a cluster of more than one node, you might need to do some fancy [shard allocation](https://www.elastic.co/guide/en/elasticsearch/reference/current/allocation-filtering.html) to make sure that all shards and all indices are copied over to the new node.

### 1. Create a new node

Why not cluster with that source ES server (running a single-node cluster) by creating a new node that we do control, and thus get the data? Getting the `docker` container to run an ES version `0.90.3` was just a bit of manual fiddling with the Dockerfile. Changing the versions everywhere worked well, but here's hint: up until `1.0` or so, `elasticsearch` [ran in background by default](https://github.com/elastic/elasticsearch/issues/4392). Thus, the docker container stopped immediately after starting `elasticsearch`, for no apparent reason...

So if you're dealing with a pre-`1.0` ES just add a `-f` (for "foregroud") to the command in Dockerfile to save yourself a bit of frustration.

### 2. Cluster with the source server

Once we have this running, it's time to cluster with the source node. What could be easier? Disable [multicast discovery](https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-discovery-zen.html), enable unicast with a specified host and we're good, right?

Wrong. Remember the *"misconfigured in just a slight but annoying manner"* thing? The source IP server turned out to be behind a NAT and the IP that we could connect to differed from the IP the server published. Hence, our new node discovered the source node as master, but then -- based on the info gotten from it -- tried connecting to it via its internal (`10.x.y.z`) IP address. Which obviously did not work.

### 3. iptables to the rescue

As we had no way of changing the configuration of the source node, the only thing we could do was mangle the IP packets, so that packets going to `10.x.y.z` would have the [destination address modified](http://linux-ip.net/html/nat-dnat.html) to the public IP of the source node (and those coming from the public IP of the source node would get modified to have the source address of `10.x.y.z`, but `iptables` handled that for us automagically):

```
iptables -t nat -I PREROUTING -d 10.x.y.z -j DNAT --to-destination <external_IP_of_the_source_node>
```

We love one-liners, don't you?

### 4. Get the data

Once we had this working and [confirmed that the cluster is now two nodes](https://www.elastic.co/guide/en/elasticsearch/reference/current/cluster-state.html) (souce node and our new node), we just sat back and watched the ~9GiB of data flow in. Once that was done, it was time to down the new node, disabled discovery altogether, and up it again to [verify that we now have the data in there](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-count.html), on a node that we actually control.

### 5. Upgrade

[Full cluster restart upgrade](https://www.elastic.co/guide/en/elasticsearch/reference/current/restart-upgrade.html) is what we had to do next, but for our single-node cluster that's just a fancy way of saying:

 - down the node;
 - upgrade ES to whatever version needed (`1.7.3` in our case);
 - up the node;
 - verify everything is AOK.

### 6. Migrate the data to the production node

Since we were doing all this on a new node, created only to get the data off of the source ES `0.90.3` server, we needed to shunt the data off of it and into our production server (changing the index name in the process for good measure). This is where we turned back to `elasticdump`, and using a simple script were able to successfuly migrate the data off of the new node and onto our production ES `1.7.3` server.

Of course things could not got smooth and easy here, either. The dump kept being interrupted by [an EADDRNOTAVAIL error](https://github.com/taskrabbit/elasticsearch-dump/issues/138); a quick work-around was to use the `--skip` command-line argument of `elasticdump` to skip rows that have already been migrated.
