# Query Planner

Distributed SQL Planner powered by [DuckDB](https://duckdb.org/)

## Problem
Multiple options have been considered for implementing the distributed query planner, including [Apache Calcite](https://calcite.apache.org/) and [DataFusion](https://github.com/apache/arrow-datafusion) (*C.f.* [#6](https://github.com/sutoiku/puffin/issues/7)). Apache Calcite has a very rich API and an unparalleled track record, but requires a JVM, which would make integration very challenging. DataFusion has been used successfully for projects like [`dask-sql`](https://github.com/dask-contrib/dask-sql), but would require migrating from [Node.js](https://nodejs.org/en/) to [Rust](https://www.rust-lang.org/), as well as integrating a secondary query engine, which would dramatically increase the footprint of our [Engine](../functions/engine/README.md) serverless function.

## Solution
A much more attractive option would be to use [DuckDB](https://duckdb.org/) itself, by exposing its relational algebra and some of its query planning routines through a public API. DuckDB already supports [Substrait](https://substrait.io/) for both producing and consuming substrait query plans (*C.f.* [documentation](https://duckdb.org/docs/extensions/substrait)), and its SQL parser | stringifier will soon be exposed through the SQL API (project funded by [STOIC](http://stoic.com/)). If implemented properly, this API would provide a very attractive cross-platform alternative to Apache Calcite.

## Benefits
- Cross -platform ([C](https://duckdb.org/docs/api/c/overview), [C++](https://duckdb.org/docs/api/cpp), [Java](https://duckdb.org/docs/api/java), [Node.js](https://duckdb.org/docs/api/nodejs/overview), [Python](https://duckdb.org/docs/api/python/overview), [R](https://duckdb.org/docs/api/r), [Rust](https://duckdb.org/docs/api/rust.html), [WASM](https://duckdb.org/docs/api/wasm))
- Straightforward [integration](https://duckdb.org/docs/extensions/substrait) with [Substrait](https://substrait.io/)
- Perfect alignment with target query engine
- Built-in query engine to lookup metadata related to remote tables
- Zero additional dependencies
- Access to a rapidly-growing community of [contributors](https://github.com/duckdb/duckdb/graphs/contributors)
- Commercial support from [DuckDBLabs](https://duckdblabs.com/) through [STOIC](https://stoic.com/)'s sponsorship.

## Credits

Many thanks to [Jacques Nadeau](https://github.com/jacques-n) and [Andy Grove](https://github.com/andygrove) for their help in giving us a better understanding of [Substrait](https://substrait.io/)'s awesome goodness.
