# Extension

PuffinDB includes a [DuckDB Extension](https://duckdb.org/docs/extensions/overview.html) implementing the following features:

- Distribute queries across thousands of serverless functions and a [Monostore](Monostore.md)
- Read from and write to hundreds of applications using any [Airbyte connector](https://airbyte.com/connectors)
- Collaborate on the same [Iceberg tables](https://iceberg.apache.org/spec/) with other users
- Write back to an Iceberg table with [ACID](https://en.wikipedia.org/wiki/ACID) transactional integrity
- Execute [cross-database joins](Query%20Proxy.md#query-delegation) (*Cf.* [Edge-Driven Data Integration](../EDDI.md))
- Translate between 19 [SQL dialects](Query%20Proxy.md#dialect-translation)
- Invoke [remote query generators](Query%20Proxy.md)
- Invoke [curl](https://curl.se/) commands
- Support the [Lance](https://github.com/eto-ai/lance) file format for 100× faster random access
- Accelerate and | or schedule the downloading of large tables to your client
- Cache tables and run computations at the edge ([Amazon CloudFront](https://aws.amazon.com/cloudfront/) × [Lambda@Edge](https://aws.amazon.com/lambda/edge/))
- Log queries on your data lake

## `curl` Invocation
The PuffinDB extension allows the invocation of [curl](https://curl.se/) directly from [DuckDB](https://duckdb.org/), using the following syntax:

```sql
SELECT * FROM curl(url="https://myURL", content-type="text/jsonl");
```
