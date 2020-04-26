> #### [ElasticCache](https://docs.aws.amazon.com/elasticache/?id=docs_gateway)

- Amazon ElastiCache makes it easy to set up, manage, and scale distributed in-memory cache environments in the AWS Cloud. It provides a high performance, resizable, and cost-effective in-memory cache, while removing complexity associated with deploying and managing a distributed cache environment. ElastiCache works with both the Redis and Memcached engines

ElastiCache for Redis Components and Features

ElastiCache Nodes
- A node is the smallest building block of an ElastiCache deployment. A node can exist in isolation from or in some relationship to other nodes.
- A node is a fixed-size chunk of secure, network-attached RAM. Each node runs an instance of the engine and version that was chosen when you created your cluster

ElastiCache for Redis Shards
- A Redis shard (called a node group in the API and CLI) is a grouping of one to six related nodes. A Redis (cluster mode disabled) cluster always has one shard. A Redis (cluster mode enabled) cluster can have 1–90 shards
- A multiple node shard implements replication by have one read/write primary node and 1–5 replica nodes.

ElastiCache for Redis Clusters
- A Redis cluster is a logical grouping of one or more ElastiCache for Redis Shards. Data is partitioned across the shards in a Redis (cluster mode enabled) cluster.
- A Redis (cluster mode enabled) cluster contains 1–90 shards (in the API and CLI, called node groups). Redis (cluster mode disabled) clusters always contain just one shard (in the API and CLI, one node group). A Redis shard contains one to six nodes. If there is more than one node in a shard, the shard supports replication with one node being the read/write primary node and the others read-only replica nodes.
-

Caching Strategies
- Lazy Loading
  - loads data into the cache only when necessary.
  - Advantages
    - Only requested data is cached.
    - Node failures are not fatal.
  - Disadvantages
    - There is a cache miss penalty.
    - Stale data
- Write Through
  - adds data or updates data in the cache whenever data is written to the database.
  - Advantages
    - Data in the cache is never stale.
    - Write penalty vs. Read penalty
    - Every write involves two trips:
      - A write to the cache
      - A write to the database
  - Disadvantages
    - Missing data - In the case of spinning up a new node, whether due to a node failure or scaling out, there is missing data which continues to be missing until it is added or updated on the database.
    - Cache churn - Since most data is never read, there can be a lot of data in the cluster that is never read. This is a waste of resources
- Adding TTL
  - Lazy loading allows for stale data, but won't fail with empty nodes. Write through ensures that data is always fresh, but may fail with empty nodes and may populate the cache with superfluous data. By adding a time to live (TTL) value to each write, we are able to enjoy the advantages of each strategy and largely avoid cluttering up the cache with superfluous data
  - Time to live (TTL) is an integer value that specifies the number of seconds (Redis can specify seconds or milliseconds) until the key expires. When an application attempts to read an expired key, it is treated as though the key is not found, meaning that the database is queried for the key and the cache is updated. This does not guarantee that a value is not stale, but it keeps data from getting too stale and requires that values in the cache are occasionally refreshed from the database.


Other important points
- Security
  - Redis supports Redis AUTH ( usr/pwd )
  - SSL in-flight encryption must be enabled and used
  - Memcached supports SASL authentication (advanced)
  - None of the caches support IAM authentication
  - IAM policies on EC  are only used for AWS API-level security
- Patterns for EC
  - Lazy loading
  - Write through
  - session store : temporary session data in cache using TTL
- Quote : There are only two hard things in Computer science : cache invalidation and naming things
