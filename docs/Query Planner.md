# Query Planner

Cross-platform distributed SQL planner powered for [Cloud Data](../CLOUD.md) by [DuckDB](https://duckdb.org/)

## Problem
Multiple options have been considered for implementing the distributed query planner, including [Apache Calcite](https://calcite.apache.org/) and [DataFusion](https://github.com/apache/arrow-datafusion) (*Cf.* [#7](https://github.com/sutoiku/puffin/issues/7)). Apache Calcite has a very rich API and an unparalleled track record, but requires a JVM, which would make integration very challenging. DataFusion has been used successfully for projects like [`dask-sql`](https://github.com/dask-contrib/dask-sql), but would require migrating from [Node.js](https://nodejs.org/en/) to [Rust](https://www.rust-lang.org/), as well as integrating a secondary query engine, which would dramatically increase the footprint of our [Engine](../functions/engine/README.md) serverless function.

## Solution
A much more attractive option is to use [DuckDB](https://duckdb.org/) itself, by exposing its relational algebra and some of its query planning routines through a public API. DuckDB already supports [Substrait](https://substrait.io/) for both producing and consuming substrait query plans (*Cf.* [documentation](https://duckdb.org/docs/extensions/substrait)), and its SQL parser | stringifier will soon be exposed through the SQL API (project funded by [STOIC](http://stoic.com/)). If implemented properly, this API would provide a very attractive cross-platform alternative to Apache Calcite.

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
- Piggybacking of [DuckDB](https://duckdb.org/)'s optimizer with simulated cost metrics for outlining logical query plan
- Implementation of [multi-relational algebra](https://dl.acm.org/doi/pdf/10.1145/319996.320009)
- Domain Specific Language (DSL) for [rule-based query optimization](https://www.querifylabs.com/blog/rule-based-query-optimization)
- Rule scripting powered by [TypeScript](https://www.typescriptlang.org/) for dynamic rule injection and client-side + cloud-side execution
- Initial set of optimizer rules bootstrapped by porting [Trinio's rules](https://github.com/trinodb/trino/tree/master/core/trino-main/src/main/java/io/trino/sql/planner/iterative/rule) from Java to DSL
- Automatic generation of optimizer rules using [WeTune](https://ipads.se.sjtu.edu.cn/_media/publications/wetune_final.pdf)
- Dynamic injection of optimizer rules through standard SQL API
- Rule interpreter implemented in [Rust](https://www.rust-lang.org/)
- Memoization for [cost-based optimization](https://www.querifylabs.com/blog/memoization-in-cost-based-optimizers)
- Parallelization of query planning across multiple serverless functions
- Parallelization of [metadata](https://www.querifylabs.com/blog/metadata-management-in-apache-calcite) lookups through concurrent invocations of the query engine
- Dynamic cascading replanning at the edges

## Strategy
Because PuffinDB's [distributed query engine](Query%20Engine.md) will be deployed across [multiple tiers](Query%20Engine.md#physical-deployment) and might run across tens of thousands of serverless functions with [reactive caching](Query%20Engine.md#reactive-caching), its query planner will be quite sophisticated. There are three main ways to approach that challenge:
- Manufally curating hundreds of query optimizer rules
- Using deep learning to automate the generation of such rules
- Using first-order logic (FOL) to automate the generation of such rules (*à la* [WeTune](https://ipads.se.sjtu.edu.cn/_media/publications/wetune_final.pdf))

We fundamentally believe that first-order logic is the best approach, for several reasons:
- It is cost-effective, fast, and sustainable (unlike manual curation)
- It is efficient (unlike deep learning)
- It leverages the fact that SQL is based on solid mathematical foundations ([relational algebra](https://en.wikipedia.org/wiki/Relational_algebra))

Therefore, PuffinDB's distributed query planner will do as much as possible with FOL, then add manually-curated rules for specific cases.

## Serverless Architecture
PuffinDB's [distributed query engine](Query%20Engine.md) is mostly serverless (with the exception of the Monostore, for good reasons) and will be deployed on up to tens of thousands of serverless functions (*e.g.* [AWS Lambda](https://aws.amazon.com/lambda/)), with [reactive caching](Query%20Engine.md#reactive-caching). This demands a radical rethinking of distributed SQL query planning, for several reasons:

## Query Plan Lifecycle
1. Query translated from non-SQL dialect (*e.g.* [Malloy](https://github.com/malloydata/malloy/tree/main/packages/malloy), [PRQL](https://prql-lang.org/)) to SQL
2. Abstract syntax tree, relational tree, and logical query plan produced by [DuckDB](https://duckdb.org/)
3. Non-distributed logical query plan optimized by [DuckDB](https://duckdb.org/)
4. Non-distributed logical query plan further optimized by [WeTune](https://dl.acm.org/doi/10.1145/3514221.3526125)
5. Set of Object Store partitions looked-up from data lake (using [Iceberg Java API](https://iceberg.apache.org/docs/latest/api/) packaged as a serverless function)
6. Set of cached partitions looked-up from [Registry](Query%20Engine.md#Registry) (powered by [Redis](https://redis.io/))
7. Distributed logical query plan generated with [multi-relational algebra](https://dl.acm.org/doi/pdf/10.1145/319996.320009) and [SMT](https://en.wikipedia.org/wiki/Satisfiability_modulo_theories) solver (using [Z3](https://github.com/Z3Prover/z3) theorem prover)
8. Distributed physical query plan produced by assigning operations to serverless functions and containers
9. Distributed physical query plan executed by [distributed query engine](Query%20Engine.md)

**Note**: #4 can be executed in parallel with #5 and #6. #4 and #7 might be executed in parallel across many serverless functions.

## Credits

Many thanks to [Jacques Nadeau](https://github.com/jacques-n) and [Andy Grove](https://github.com/andygrove) for their [help](https://github.com/sutoiku/puffin/issues/7) in giving us a better understanding of [Substrait](https://substrait.io/)'s awesome goodness.
