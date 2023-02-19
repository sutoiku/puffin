# Extension

PuffinDB includes a [DuckDB Extension](https://duckdb.org/docs/extensions/overview.html) implementing the following features:

## Without Cloud-Side Templane
- Data lake connectivity ([Apache Iceberg](https://iceberg.apache.org/), [Apache Hudi](https://hudi.apache.org/), [Delta Lake](https://delta.io/))
- Integration with other database engines ([Snowflake](https://www.snowflake.com/en/), [Databricks](https://www.databricks.com/), [Athena](https://aws.amazon.com/athena/), *etc.*)
- [SQL dialect translation](Query%20Proxy.md#dialect-translation)
- Remote [curl](https://curl.se/) invocation
- [Remote query generation](Query%20Proxy.md)

## With Cloud-Side Template
- [Remote query optimization](Query%20Proxy.md#query-optimization)
- [Scale-out and scale-up](../CLOUD.md#scale-out-and-scale-up)
