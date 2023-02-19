# Extension

PuffinDB includes a [DuckDB Extension](https://duckdb.org/docs/extensions/overview.html) implementing the following features:

## Without Cloud-Side Template
- Read-Write data lake connectivity ([Apache Iceberg](https://iceberg.apache.org/), [Apache Hudi](https://hudi.apache.org/), [Delta Lake](https://delta.io/))
- Integration with [other database engines](Query%20Proxy.md#query-delegation) (Athena, Databricks, Snowflake, *etc.*)
- [SQL dialect translation](Query%20Proxy.md#dialect-translation) (powered by [SQLGlot](https://github.com/tobymao/sqlglot))
- Remote [curl](https://curl.se/) invocation
- [Remote query generation](Query%20Proxy.md)
- Query Logging

## With Cloud-Side Template
- [Remote query optimization](Query%20Proxy.md#query-optimization)
- [Scale-out and scale-up](../CLOUD.md#scale-out-and-scale-up)
- Query result sharing
