# Extension

PuffinDB includes a [DuckDB Extension](https://duckdb.org/docs/extensions/overview.html) implementing the following features:

## Without Cloud-Side Template
The following features are available without having to install any [CloudFormation](https://aws.amazon.com/cloudformation/) template on your VPC:
- Read-Write data lake connectivity ([Apache Iceberg](https://iceberg.apache.org/), [Apache Hudi](https://hudi.apache.org/), [Delta Lake](https://delta.io/))
- [Cross-database](Query%20Proxy.md#query-delegation) joins (Athena, Databricks, Snowflake, *etc.*)
- [SQL dialect translation](Query%20Proxy.md#dialect-translation) (powered by [SQLGlot](https://github.com/tobymao/sqlglot))
- Remote [curl](https://curl.se/) invocation
- [Remote query generation](Query%20Proxy.md)
- Query Logging

## With Cloud-Side Template
The following features become available after having installed the PuffinDB [CloudFormation](https://aws.amazon.com/cloudformation/) template on your VPC:
- [Remote query optimization](Query%20Proxy.md#query-optimization)
- [Scale-out and scale-up](../CLOUD.md#scale-out-and-scale-up)
- Query result sharing
