---
title: ElasticSearch Concepts Part 01
---
**Introduction**

Elasticsearch does much of the heavy lifting on handling horizontal scalability for us, managing failures, nodes, shards.  

**cluster.name**

Nodes with the same name belong to same cluster

The cluster reorganizes itself as we add or remove data, meaning it manages moving data between nodes if needed.

**Master Node**

He is not involved in searching - one node is elected as master node, it's going to be in charge of adding and deleting indexes, adding/removing nodes from cluster.  He is just a manager.

**Any Node**

You can talk to any node for searching and indexing including the master.  The **entry point node** (any node) knows where data resides so it will communicate with it to get and set data and it will get back to us (the entry point node) with the results.

**Index**

Logical namespace that points to one or more shards.  It's like a database in a relational ddatabase.  Index groups together mone or more shards.

```bash
1 index -> multi shards # => one index can have one or more multi shards it's like a database.
shard # => documents are stored in shards. single instance of lucene.  a complete search engine in it's own right.
application -> index -> shard # => applications talk to shards via indexes which are logical namespaces pointers to shards.
cluster grows # => move shards between nodes.
primary shard # => document is on a **single** primary shard.  data is only on one primary shard.
replica shard # => in case of hardware failure on primary shard, serve read requests (read/get).
number of shards # => you can have multiple primary shards for an index.
who handles what # => Read / Search is handled by either primary or replica, the more copies the higher the throughput.
concurrency # => if conflict two proesses read 50 and increase to one and store we can end up with 51 and not 52. elasticsearch is using optimistic concurrency control (versioning).
```

**Distributed Document Store**

When you index a document it is stored on a **single primary shard**.

```shard = hash(routing) % number_of_primary_shards```

```
This explains why the number of primary shards can be set only when an index is created and never changed: if the number of primary shards ever changed in the future, all previous routing values would be invalid and documents would never be found.
```

```
coordinating node # => the node got our request, forwards to correct node for read/write.
```

Create, index, and delete requests are write operations, which must be successfully completed on the **primary shard before ** they can be copied to any associated replica shards.  The client will get OK only if finished successfully on primary shard.

**parameters/configuration**

```
replication # => sync: wait for successull response from replicas.  async: success as soon as primary finished.  avoid sync...
quorum # => By default primary shards requires a quorum (shards majority) to be **available** before attermting write.
read miss # => it is possible that while a document is indexed document is in primary but not yet copied to replica, replica will return that document does not exist, while the primary would return the document successfully.  in that sense read is not consistent but eventual consistent.
```

**ElasticSearch GET Read Consistency**

Elasticsearch read consistency is eventually consistent but it can also be consistent :).  The realtime flag is per shard, so if we have a replicated shard which did not get the data yet, while it may still be realtime we won't get the most recent data, at most we would get the data on it's transaction log.

realtime:true + reaplication: sync ==> read consistent # => because replication true means master waits for the written data to be replicated to all replicas.


