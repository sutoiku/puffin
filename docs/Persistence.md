# Persistence

PuffinDB uses multiple engines and serverless services for persistence and transient state management:

- **Table Metadata**: Lakehouse Catalog
- **Table Summaries**: Object Store
- **Edits**: Key-Value Store
- **Sharings**: Key-Value Store
- **Persistent Logs**: Block Store during [Monostore](Monostore.md)'s life, then Object Store
- **Table Caches Metadata**: In-Memory Metastore
- **Transient Logs**: In-Memory Metastore

Some logs are both persistent and transient (sequences and steps for [data pipelines](Pipeline%20Engine.md)).

## Implementations
Engines and services are implemented in the following fashion:

- **Lakehouse Catalog**: [Amazon DynamoDB](https://aws.amazon.com/dynamodb/)
- **Key-Value Store**: [Amazon DynamoDB](https://aws.amazon.com/dynamodb/)
- **Object Store**: [Amazon S3](https://aws.amazon.com/s3/) (using [Parquet](https://parquet.apache.org/) files)
- **Block Store**: [Amazon EBS](https://aws.amazon.com/ebs/) (using native [DuckDB](https://duckdb.org/) files)
- **In-Memory Metastore**: [Redis](https://redis.io/) instance running within [Monostore](Monostore.md)
