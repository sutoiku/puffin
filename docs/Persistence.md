# Persistence

PuffinDB uses multiple engines and serverless services for persistence and transient state management:

- **Lakehouse Catalog**: [Amazon DynamoDB](https://aws.amazon.com/dynamodb/)
- **Key-Value Store**: [Amazon DynamoDB](https://aws.amazon.com/dynamodb/)
- **Object Store**: [Amazon S3](https://aws.amazon.com/s3/) (using [Parquet](https://parquet.apache.org/) files)
- **Block Store**: [Amazon EBS](https://aws.amazon.com/ebs/) (using native [DuckDB](https://duckdb.org/) files)
- **In-Memory Metastore**: [Redis](https://redis.io/) instance running within [Monostore](Monostore.md)

**Note**: Block store volumes can be provisioned on the fly when the Monostore gets started (scale-to-zero).

These are used for the following entities:

- **Table Metadata**: Lakehouse Catalog
- **Table Summaries**: Object Store (*Cf.* [Metastore Statistics](Metastore.md))
- **Sharings**: Key-Value Store
- **Edits**: Block Store during [Monostore](Monostore.md)'s life, then Object Store
- **Persistent Logs**: Block Store during [Monostore](Monostore.md)'s life, then Object Store
- **Table Caches Metadata**: In-Memory Metastore
- **Transient Logs**: In-Memory Metastore

Some logs are both persistent and transient (*e.g.* sequences and steps for [data pipelines](Pipeline%20Engine.md)).

## Benefits

- 100% automated provisioning and maintenance
- High elasticity
- Scale-to-zero
- Super low cost
- Ultra-low latency for entities that really need it (transient logs)
- Unlimited capacity (never run out of memory or storage)
