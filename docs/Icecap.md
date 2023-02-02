# Icecap

Icecap is an implementation of [Iceberg tables](https://iceberg.apache.org/spec/) using [Redis](https://redis.io/) and [DuckDB](https://duckdb.org/) as an alternative to [Spark SQL](https://spark.apache.org/sql/).

## Overview
- Optimized for low latency
- Supporting updates in place
- Powered by serverless functions, [Redis](https://redis.io/), and [DuckDB](https://duckdb.org/) (*sans* Spark)

## Redis
- Used as table catalog and transactional orchestrator allowing multiple DuckDB engines to read | write the same tables
- Accelerated with [Dragonfly](https://dragonflydb.io/) or [KeyDB](https://docs.keydb.dev/) (optional)

## Updates
Object Stores like [Amazon S3](https://aws.amazon.com/s3/) do not currenlty support updates in place. Therefore, a serverless function must `GET` an entire object before applying updates to it and before it can be `PUT` back on the Object Store. Nevertheless, if the object uses a file format natively designed to support updates in place (such as DuckDB's native file format), this process can be accelerated. Furthermore, the serverless function can cache the object on its local filesystem, thereby allowing updates in place during the object's caching lifespan.

Down the road, we hope Object Stores will add native support for updates in place when using certain file formats such as DuckDB's.

In the meantime, updates will be managed in the following fashion:

1. Updates on table buffered on Redis
2. Partitions of tables loaded from object store and cached on serverless functions
3. Updates applied in place by serverless functions
4. Partitions serialized back onto object store

According to this model, the DuckDB file format could be used on both object store and serverless functions, or just the latter.

## File Format Replication
Icecap will make it possible to replicate every partition stored on the Object Store across multiple file formats. For example, the same partition could be stored in both DuckDB and Parquet formats. This will allow any Parquet-compatible tool to query tables, while making it faster for Icecap to update tables by leveraging the fact that DuckDB's native file format supports updates in place. Considering that storage costs on the Object Store usually represents a tiny fraction of the overall cost of operating a data lake, this transparent replication will probably be attractive to many organizations.

## FAQ
**Why not use Spark SQL?**.  
Because it's too slow and too expensive to deploy and operate.

**Will Icecap support the Parquet file format?**.  
Yes. Icecap will support any file format supported by [Apache Iceberg](https://iceberg.apache.org/), alongside the native DuckDB file format for updates in place.

**Will Icecap support the Iceberg table format?**.  
Yes, Iceberg will support both the [Iceberg](https://iceberg.apache.org/) and [Delta Lake](https://delta.io/) table formats (not to be confused with file formats).

**Which file formats will be supported on the underlying Object Store?**   
All of them.

**Which file formats will be supported in cache?**   
All of them.
