# Query Planner

Cross-platform distributed SQL planner for [Cloud Data](../CLOUD.md) powered by [DuckDB](https://duckdb.org/)

## Problem
Multiple options have been considered for implementing the distributed query planner, including [Apache Calcite](https://calcite.apache.org/) and [DataFusion](https://github.com/apache/arrow-datafusion) (*Cf.* [#7](https://github.com/sutoiku/puffin/issues/7)). Apache Calcite has a very rich API and an unparalleled track record, but requires a JVM, which would make integration very challenging. DataFusion has been used successfully for projects like [dask-sql](https://github.com/dask-contrib/dask-sql), but would require migrating from [Node.js](https://nodejs.org/en/) to [Rust](https://www.rust-lang.org/), as well as integrating a secondary query engine, which would dramatically increase the footprint of our [Engine](../functions/engine/README.md) serverless function.

## Solution
A much more attractive option is to use [DuckDB](https://duckdb.org/) itself, by exposing its relational algebra and some of its query planning routines through a public API. DuckDB's SQL parser | stringifier will soon be exposed through the SQL API (project funded by [STOIC](http://stoic.com/)). If implemented properly, this API will provide an attractive cross-platform alternative to some components of Apache Calcite.

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
- Logical query optimization using [DuckDB](https://duckdb.org/)'s optimizer
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
Because PuffinDB's [distributed query engine](Query%20Engine.md) will be deployed across [multiple tiers](Query%20Engine.md#physical-deployment) and might run across tens of thousands of serverless functions with [reactive caching](Query%20Engine.md#reactive-caching), its query planner will be quite sophisticated. There are three main ways to approach such a challenge:
- Manufally curating hundreds of query optimizer rules
- Using deep learning to automate the generation of such rules
- Using first-order logic (FOL) to automate the generation of such rules (*à la* [WeTune](https://ipads.se.sjtu.edu.cn/_media/publications/wetune_final.pdf))

We fundamentally believe that first-order logic is the best approach, for several reasons:
- It is cost-effective, fast, and sustainable (unlike manual curation)
- It is efficient (unlike deep learning)
- It leverages the fact that SQL is based on solid mathematical foundations ([relational algebra](https://en.wikipedia.org/wiki/Relational_algebra))

Therefore, PuffinDB's distributed query planner will do as much as possible with FOL, then add manually-curated rules for specific cases.

## Serverless Architecture
PuffinDB's [distributed query engine](Query%20Engine.md) is mostly serverless (with the exception of the [Monostore](Monostore.md), for good reasons) and will be deployed across tens of thousands or even hundreds of thousands of serverless functions (*e.g.* [AWS Lambda](https://aws.amazon.com/lambda/)), with [reactive caching](Query%20Engine.md#reactive-caching). This demands a radical rethinking of distributed SQL query planning, for several reasons:
- The number of compute nodes is very large (most query planners are optimized for tens or hundreds of compute nodes at most)
- Compute nodes are heterogeneous (serverless functions, serverless containers, server-based containers, and web browsers)
- Serverless functions mandate a clear delineation between static partitioning (object store) and dynamic sharding (functions)

Therefore, the distributed query planner will need to answer three main questions:
- How is data partitioned on the Object Store and sharded across the [reactive caching system](Query%20Engine.md#reactive-caching)?
- How should the query be distributed across [computing and caching tiers](Query%20Engine.md#physical-deployment)?
- How should data be cached next to accelerate subsequent queries?

## Partitioning *vs.* Sharding
Large tables managed by the Data Lake (*e.g.* [Apache Iceberg](https://iceberg.apache.org/)) are partitioned across multiple objects on the Object Store (*e.g.* [Amazon S3](https://aws.amazon.com/s3/)). While serverless functions scan tables directly from the Object Store, the resulting data ends up being cached on the serverless functions, the [Monostore](Monostore.md), or clients. Therefore, it becomes critical to properly shard large tables across serverless functions, be they used in a stateful or stateless manner (with or without caching).

For performance reasons, three types of sharded tables must be supported:

- **Distributed tables**: one serverless function per partition or group of partitions
- **Co-located tables**: partitions of tables sharded across the same dimensions are co-located within the same serverless functions
- **Replicated tables**: small tables are replicated across all severless functions that might need them for joins

**Note**: implementing the last two sharding techniques can be avoided if multi-table datasets are statically denormalized in the Data Lake.

## Cost-Based Optimizer
The distributed query planner will include a cost-based optimizer responsible for deciding which plan will deliver the best performance for a given query. Among other things, this cost-based optimizer will decide whether an existing sharding is sufficient for a given query, or whether tables involved in the query should be sharded another way. Mature cost-based optimizers are currently under review.

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
