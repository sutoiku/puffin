# Distributed Query Engine
The distributed query engine is designed with the following requirements in mind:
- Deployed using standard [DuckDB](https://duckdb.org/) engines supercharged with the `puffindb` [extension](https://duckdb.org/docs/extensions/overview)
- Abstracted with the [`SELECT THROUGH`](../EDDI.md#implementation) syntax to avoind unnecessary data copies
- Powered by [Apache Arrow](https://arrow.apache.org/) to accelerate data transfers between DuckDB engines

## Why use `SELECT THROUGH`?
The proposed [`SELECT THROUGH`](../EDDI.md#implementation) syntax would allow a [distributed query plan](Query%20Planner.md) to be executed in a cascaded fashion by supercharged DuckDB engines, without the intermediation of any middleware. Using this syntax, distributed DuckDB engines could communicate with each other directly, thereby reducing latencies and avoiding any unnecessary data copies. This would not prevent some DuckDB engines from serializing stremed intermediary results on an Object Store (*e.g.* [Amazon S3](https://aws.amazon.com/s3/)), but this serialization (and the decision to perform it) could be made by the DuckDB engine itself. This syntax would also allow any DuckDB engine to join locally-cached data with intermediate query results produced by remote DuckDB engines.
