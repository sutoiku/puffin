# Distributed Query Engine
The distributed query engine is designed around the following components:
- Deployed using standard [DuckDB](https://duckdb.org/) engines supercharged with the `puffindb` [extension](https://duckdb.org/docs/extensions/overview)
- Abstracted with the [`SELECT THROUGH`](../EDDI.md#implementation) syntax to avoind unnecessary data copies
- Powered by [Apache Arrow](https://arrow.apache.org/) to accelerate data transfers between DuckDB engines

## Why use `SELECT THROUGH`?
The proposed [`SELECT THROUGH`](../EDDI.md#implementation) syntax would allow a [distributed query plan](Query%20Planner.md) to be executed in a cascaded fashion by supercharged DuckDB engines, without the intermediation of any middleware. Using this syntax, distributed DuckDB engines could communicate with each other directly, thereby reducing latencies and avoiding any unnecessary data copies. This would not prevent some DuckDB engines from serializing streamed intermediate results on an Object Store (*e.g.* [Amazon S3](https://aws.amazon.com/s3/)), but this serialization (and the decision to perform it) could be made by the DuckDB engine itself. This syntax would also allow any DuckDB engine to join locally-cached data with intermediate query results produced by remote DuckDB engines.

## Core Engine
The core [engine](../functions/Engine/README.md) is packaged as a serverless function and includes the following components:
- Query handler responsible for handling inbound queries
- [Query planner](Querry%20Planner.md) responsible for creating a distributed query plan
- Query engine (DuckDB) responsible for executing the local query plan

## Registry
The distributed execution of a query is made possible by the use of a low-latency **Registry** powered by [Redis](https://redis.io/). This allows small intermediate results to be cached with submillisecond latency. Using a large [Amazon ElastiCache for Redis](https://aws.amazon.com/elasticache/redis/) cluster, tens of millions of such intermediate results can be cached per second, for up to 340 TB of cached data (large intermediate results should use the Object Store). The registry can be used to trigger the Reduce phase of a MapReduce process once all Map operations have completed successfully, or to enable asynchronous communication between two DuckDB engines.
