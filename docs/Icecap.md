# Icecap

Icecap would be an alternative implementation of [Iceberg tables](https://iceberg.apache.org/spec/) using the native [DuckDB](https://duckdb.org/) file format.

## Overview
- Optimized for low latency
- Supporting updates in place
- Powered by serverless functions, [Redis](https://redis.io/), and [DuckDB](https://duckdb.org/) (no Spark)

## Redis
- Used as table catalog and transactional orchestrator allowing multiple DuckDB engines to read | write the same tables
- Accelerated with [Dragonfly](https://dragonflydb.io/) or [KeyDB](https://docs.keydb.dev/) (optional)

## Updates
Object Stores like [Amazon S3](https://aws.amazon.com/s3/) do not currenlty support updates in place. Therefore, a serverless function must `GET` an entire object before applying updates to it and before it can be `PUT` back on the Object Store. Nevertheless, if the object uses a file format natively designed to support updates in place, this process can be accelerated. Furthermore, the serverless function can cache the object on its local filesystem, thereby allowing updates in place during the object's caching lifespan.

Down the road, we expect Object Stores to add native support for updates in place when using certain file formats such as DuckDB's.

In the meantime, updates will be managed in the following fashion:

1. Updates on table buffered on Redis
2. Partitions of tables loaded from object store and cached on serverless functions
3. Updates applied in place by serverless functions
4. Partitions serialized back onto object store

According to this model, the DuckDB file format could be used on both object store and serverless functions, or just the latter.
