---
title: ElasticSearch Concepts Part 01
---

Elasticsearch does much of the heavy lifting on handling horizontal scalability for us, managing failures, nodes, shards.  

`cluster.name # => nodes with the same name belong to same cluster`

The cluster reorganizes itself as we add or remove data, meaning it manages moving data between nodes if needed.

**Master Node**

He is not involved in searching - one node is elected as master node, it's going to be in charge of adding and deleting indexes, adding/removing nodes from cluster.  He is just a manager.

**Any Node**

You can talk to any node for searching and indexing including the master.  The **entry point node** (any node) knows where data resides so it will communicate with it to get and set data and it will get back to us (the entry point node) with the results.
