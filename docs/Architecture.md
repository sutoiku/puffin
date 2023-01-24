# Architecture

PuffinDB has a radical cloud-native architecture. Deployment on "private clouds" is not a priority.

## Core Principles
- Do as much as possible with serverless functions ([AWS Lambda](https://aws.amazon.com/lambda/)).
- Do as much as possible of the remaining parts with serverless containers ([AWS Fargates](https://aws.amazon.com/fargate/)).
- Do the last bits with a single server-based container with as much capacity as possible ([Amazon EC2](https://aws.amazon.com/ec2/)).
- Cache data in memory as aggressively as possible.
- Use an auto-scaling [Redis](https://redis.io/) cluster for synchronization (submillisecond transactions, millions of transactions per second).
- Use the same [Redis](https://redis.io/) cluster for small shuffles
- Use the Object Store ([Amazon S3](https://aws.amazon.com/s3/)) for larger shuffles

## Serverless Components
PuffinDB is architected around the following serverless components:

- [Engine](../functions/engine/README.md) — Rust serverless function implementing the query handler and packaging the [DataFusion](https://github.com/apache/arrow-datafusion) and [DuckDB](https://duckdb.org/) query engines
- [Catalog](../functions/catalog/README.md) — Java serverless function packaging [Iceberg's Java API](https://iceberg.apache.org/docs/latest/api/)
- [AWS EMR Serverless](https://aws.amazon.com/emr/serverless/) for executing write queries on lakehouse tables
- [Amazon ElastiCache for Redis](https://aws.amazon.com/elasticache/redis/) for logging, queuing, and synchronization
- [Amazon S3](https://aws.amazon.com/s3/) for object storage
- [Amazon CloudFront](https://aws.amazon.com/cloudfront/) for cached query result distribution

**Note**: Technically-speaking, [Amazon ElastiCache for Redis](https://aws.amazon.com/elasticache/redis/) is not serverless, yet is a fully managed service.

## CloudFormation Templates
These components are packaged into a pair of complementary [AWS CloudFormation](https://aws.amazon.com/cloudformation/) templates:
- [Lakehouse Template](../templates/lakehouse/README.md) — usually instantiated once
- [Engine Template](../templates/engine/README.md) — usually instantiated multiple times
