# Distributed Query Engine
The distributed query engine is designed around the following components:
- Deployed using standard [DuckDB](https://duckdb.org/) engines supercharged with the `puffindb` [extension](https://duckdb.org/docs/extensions/overview)
- Abstracted with the [`SELECT ... THROUGH`](../EDDI.md#implementation) syntax to avoid unnecessary data copies
- Powered by [Apache Arrow](https://arrow.apache.org/) to accelerate data transfers between DuckDB engines

## Why use `SELECT THROUGH`?
The proposed [`SELECT ... THROUGH`](../EDDI.md#implementation) syntax would allow a [distributed query plan](Query%20Planner.md) to be executed in a cascaded fashion by supercharged DuckDB engines, without the intermediation of any middleware. Using this syntax, distributed DuckDB engines could communicate with each other directly, thereby reducing latencies and avoiding any unnecessary data copies. This would not prevent some DuckDB engines from serializing streamed intermediate results on an Object Store (*e.g.* [Amazon S3](https://aws.amazon.com/s3/)), but this serialization (and the decision to perform it) could be made by the DuckDB engine itself. This syntax would also allow any DuckDB engine to join locally-cached data with intermediate query results produced by remote DuckDB engines.

## Core Engine
The core [engine](../functions/engine/README.md) is packaged as a serverless function and includes the following components:
- Query handler responsible for handling inbound queries
- [Query planner](Query%20Planner.md) responsible for creating a distributed query plan
- Query engine (DuckDB) responsible for executing the local query plan

Combining all three components within the same serverless function reduces latencies and removes any unnecessary data copies.

## Physical Deployment
The distributed query engine is distributed across three main tiers:
1. Client (web browser, native client application, or cloud-side client)
2. Monostore (single-host cloud-side container)
3. Fleet of serverless functions

In future releases, support for an auto-scaling cluster of serverless containers (*e.g.* [AWS Fargates](https://aws.amazon.com/fargate/)) might be added.

The execution model defined by the [distributed query planner](Query%20Planner.md) is pretty straightforward:
1. Pushdown as much of the SQL query as possible onto the serverless functions (and cache data there by making them stateful)
2. Pullup and cache as much data as possible on the Monostore and Client (through reactive caching)
3. Execute as much of the SQL query as possible on cached data

The Monostore is a single-host cloud-side container (server or serverless) used to execute parts of SQL queries that we cannot distribute efficiently across fleets of serverless functions or clusters of serverless containers. It is statically sized based on historical usage patterns. Best-in-class cloud providers like [Amazon Web Services](https://aws.amazon.com/) can offer very large Monostores on demand. For example, the [`u-24tb1.112xlarge`](https://aws.amazon.com/ec2/instance-types/high-memory/) instance comes with 448 vCPUs and 24 TB of RAM.

This physical deployment model brings the following benefits:
- Maximum elasticity with serverless functions
- Maximum performance for complex SQL queries with large Monostore
- Lowest latency thanks to multi-layer reactive caching

## Reactive Caching
Data stored on the lake ([Apache Iceberg](https://iceberg.apache.org/), [Apache Hudi](https://hudi.apache.org/), [Delta Lake](https://delta.io/)) is automatically cached across the following layers:
- Serverless functions (yes indeed, these can be made stateful)
- Monostore Solid State Drive (compressed or uncompressed)
- Monostore CPU RAM (compressed or uncompressed)
- Monostore GPU RAM (if GPU available, uncompressed)
- Client Solid State Drive (compressed or uncompressed)
- Client CPU RAM (compressed or uncompressed)
- Client GPU RAM (if GPU available, uncompressed)

Whenever a query is executed, its distributed physical plan is generated to take advantage of cached data, by using caches that are as close to the client as possible. Once the query has been executed, data is automatically copied in a reactive manner onto higher caching layers in order to accelerate subsequent queries that might be made on the same dataset. 

## Registry
The distributed execution of a query is made possible by the use of a low-latency **Registry** powered by [Redis](https://redis.io/). This allows small intermediate results to be cached with submillisecond latency. Using a large [Amazon ElastiCache for Redis](https://aws.amazon.com/elasticache/redis/) cluster, tens of millions of such intermediate results can be cached per second, for up to 340 TB of cached data (large intermediate results should use the Object Store). The registry can be used to trigger the Reduce phase of a MapReduce process once all Map operations have completed successfully, or to enable asynchronous communication between two DuckDB engines.
