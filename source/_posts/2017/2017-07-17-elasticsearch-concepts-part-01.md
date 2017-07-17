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
primary/replica shard # => document is on a **single** primary shard.
replica shard # => in case of hardware failure on primary shard, serve read requests (read/get).
number of shards # => you can have multiple primary shards for an index.
who handles what # => Read / Search is handled by either primary or replica, the more copies the higher the throughput.
concurrency # => if conflict two proesses read 50 and increase to one and store we can end up with 51 and not 52. elasticsearch is using optimistic concurrency control (versioning).
```

**Distributed Document Store**


