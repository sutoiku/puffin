# Query Planner

Cross-platform distributed SQL planner powered by [DuckDB](https://duckdb.org/)

## Problem
Multiple options have been considered for implementing the distributed query planner, including [Apache Calcite](https://calcite.apache.org/) and [DataFusion](https://github.com/apache/arrow-datafusion) (*Cf.* [#7](https://github.com/sutoiku/puffin/issues/7)). Apache Calcite has a very rich API and an unparalleled track record, but requires a JVM, which would make integration very challenging. DataFusion has been used successfully for projects like [`dask-sql`](https://github.com/dask-contrib/dask-sql), but would require migrating from [Node.js](https://nodejs.org/en/) to [Rust](https://www.rust-lang.org/), as well as integrating a secondary query engine, which would dramatically increase the footprint of our [Engine](../functions/engine/README.md) serverless function.

## Solution
A much more attractive option would be to use [DuckDB](https://duckdb.org/) itself, by exposing its relational algebra and some of its query planning routines through a public API. DuckDB already supports [Substrait](https://substrait.io/) for both producing and consuming substrait query plans (*Cf.* [documentation](https://duckdb.org/docs/extensions/substrait)), and its SQL parser | stringifier will soon be exposed through the SQL API (project funded by [STOIC](http://stoic.com/)). If implemented properly, this API would provide a very attractive cross-platform alternative to Apache Calcite.

## Benefits
- Cross-platform ([C](https://duckdb.org/docs/api/c/overview), [C++](https://duckdb.org/docs/api/cpp), [Java](https://duckdb.org/docs/api/java), [Node.js](https://duckdb.org/docs/api/nodejs/overview), [Python](https://duckdb.org/docs/api/python/overview), [R](https://duckdb.org/docs/api/r), [Rust](https://duckdb.org/docs/api/rust.html), [WASM](https://duckdb.org/docs/api/wasm))
- Straightforward [integration](https://duckdb.org/docs/extensions/substrait) with [Substrait](https://substrait.io/)
- Aligned with constraints of [serverless architecture](Architecture.md)
- Aligned with target SQL dialect and primary query engine
- Built-in query engine to lookup metadata related to remote tables
- Built-in query engine for dynamic cascading replanning at the edges
- Lowest-possible latency through co-location of query handler, query planner, and query engine
- Zero additional dependencies
- In-browser deployment option
- Access to a rapidly-growing community of [contributors](https://github.com/duckdb/duckdb/graphs/contributors)
- Commercial support from [DuckDBLabs](https://duckdblabs.com/) through [STOIC](https://stoic.com/)'s sponsorship.

## Ideas
The following techniques are being considered:
- Piggybacking of [DuckDB](https://duckdb.org/)'s optimizer with simulated cost metrics for outlining logical distributed query plan
- Implementation of [multirelational algebra](https://dl.acm.org/doi/pdf/10.1145/319996.320009)
- Domain Specific Language (DSL) for [rule-based query optimization](https://www.querifylabs.com/blog/rule-based-query-optimization)
- Dynamic injection of optimizer rules through standard SQL API
- Memoization for [cost-based optimization](https://www.querifylabs.com/blog/memoization-in-cost-based-optimizers)
- Parallelization of query planning across multiple serverless functions
- Parallelization of [metadata](https://www.querifylabs.com/blog/metadata-management-in-apache-calcite) lookups through concurrent invocations of the query engine
- Dynamic cascading replanning at the edges

## Optimizer
- Optimizer rules written with a Domain Specific Language (DSL) similar to the one developed for [CockroachDB](https://www.cockroachlabs.com/blog/building-cost-based-sql-optimizer/)
- Initial set of optimizer rules bootstrapped by porting [Trinio's rules](https://github.com/trinodb/trino/tree/master/core/trino-main/src/main/java/io/trino/sql/planner/iterative/rule) from Java to DSL
- Rule interpreter implemented in [Rust](https://www.rust-lang.org/)

## Engine
The distributed query planner will be used by the [distributed query engine](Query%20Engine.md).

## Credits

Many thanks to [Jacques Nadeau](https://github.com/jacques-n) and [Andy Grove](https://github.com/andygrove) for their [help](https://github.com/sutoiku/puffin/issues/7) in giving us a better understanding of [Substrait](https://substrait.io/)'s awesome goodness.
