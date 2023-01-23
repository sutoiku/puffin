# Engine

The **Engine** is a [Rust]([https://nodejs.org/en/](https://docs.aws.amazon.com/sdk-for-rust/latest/dg/lambda.html)) serverless function implementing the query engine. It embeds [DataFusion](https://arrow.apache.org/datafusion/) and [DuckDB](https://duckdb.org/docs/api/nodejs/overview.html).

**Note**: The Engine and [Catalog](../catalog/README.md) run on two separate serverless functions because the former uses Rust while the latter uses Java.
