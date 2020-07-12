---
title: "NoSQL solutions: Membase, Redis, CouchDB and MongoDB"
date: "2011-01-25"
tags: [databases,open-source,software-architecture]
---

Each database has specific use cases and every solution has a sweet spot in terms of data, hardware, setup and operation. Here are some of the most popular key-value and document data stores:

### Key-value

[Membase](http://membase.org/)

- Developed by members of the memcached core team.
- Simple (key value store), fast (low, predictable latency) and elastic (effortlessly grow or shrink a cluster).
- Extensions are possible through a plug-in architecture (full-text search, backup, etc).
- Supports Memcached ASCII and Binary protocols (uses existent Memcached libraries and clients).
- Guarantees data consistency.
- High-speed failover (server failures recoverable in under 100ms).
- User management, alerts and logging and audit trail.

[Redis](http://code.google.com/p/redis/)

- Developed by Salvatore Sanfilippo and acquired by VMWare in 2010.
- [Very fast](http://code.google.com/p/redis/wiki/Benchmarks). Non-blocking I/O. Single threaded.
- Data is held in memory but can be persisted by written to disk asynchronously.
- Values can be strings, lists or sets.
- Built-in support for master/slave replication.
- Distributes the dataset across multiple Redis instances.

### Document-oriented

The major benefit of using a document database comes from the fact that while it has all the benefits of a key/value store, you aren't limited to just querying by key. However, documented-oriented databases and MapReduce aren't appropriate for every situation.

[CouchDB](http://couchdb.apache.org/)

- High read performance.
- Supports bulk inserts.
- Good for consistent master-master replica databases that are geographically distributed and often offline.
- Good for intense versioning.
- Android, MeeGo and WebOS include services for syncing locally stored data with a CouchDB non-relational database in the cloud.
- Better than MongoDB at durability.
- Uses REST as its interface to the database. It doesn't have "queries" but instead uses "views".
- Makes heavy use of the file system cache (so more RAM is always better).
- The database must be compacted periodically.
- Conflicts on transactions must be handled by the programmer manually (e.g. if someone else has updated the document since it was fetched, then CouchDB relies on the application to resolve versioning issues).
- Scales through asynchronous replication but lacks an auto-sharding mechanism. Reads are distributed to any server while writes must be propagated to all servers.

[MongoDB](http://www.mongodb.org/)

- High write performance. Good for systems with very high update rates.
- It has the flexibility to replace a relational database in a wider range of scenarios.
- Supports auto-sharding.
- More oriented towards master/slave replication.
- Compaction of the database is not necessary.
- Both CouchDB and MongoDB support map/reduce operations.
- Supports dynamic ad hoc queries via a JSON-style query language.
- The pre-filtering provided by the query attribute doesn't have a direct counterpart in CouchDB. It also allows post-filtering of aggregated values.
- Relies on language-specific database drivers for access to the database.

### Links

- [NoSQL Patterns](http://horicky.blogspot.com/2009/11/nosql-patterns.html) by Ricky Ho
- [Redis interactive tutorial](http://try.redis-db.com/)
- [Redis tutorial](http://simonwillison.net/static/2010/redis-tutorial/) by Simon Willison (NoSQL Europe Conference)
- [MongoDB interactive tutorial](http://try.mongodb.org/)
