# Serverless Architecture

PuffinDB has a radical serverless and cloud-native architecture. Deployment on "private clouds" is not a priority.

## Core Principles
- Do as much as possible with serverless functions ([AWS Lambda](https://aws.amazon.com/lambda/)).
- Do as much as possible of the remaining parts with serverless containers ([AWS Fargates](https://aws.amazon.com/fargate/)).
- Do the last bits with a single server-based container with as much capacity as possible ([Amazon EC2](https://aws.amazon.com/ec2/)).
- Cache data in memory as aggressively as possible.
- Use an auto-scaling [Redis](https://redis.io/) cluster for synchronization (submillisecond transactions, millions of transactions per second).
- Use the same [Redis](https://redis.io/) cluster for small shuffles.
- Use the Object Store ([Amazon S3](https://aws.amazon.com/s3/)) for larger shuffles.

## Serverless Components
PuffinDB is architected around the following serverless components:

- [Engine](../functions/engine/README.md) — Node.js serverless function packaging the query handler, [query planner](Query%20Planner.md), and [DuckDB](https://duckdb.org/) query engine
- [Catalog](../functions/catalog/README.md) — Java serverless function packaging [Iceberg's Java API](https://iceberg.apache.org/docs/latest/api/)
- [Python](../functions/python/README.md) — Python serverless function providing a cloud-side Python runtime and [SQLGlot](https://github.com/tobymao/sqlglot) SQL Parser and Transpiler
- [Amazon Athena](https://aws.amazon.com/athena/) for executing write queries on lakehouse tables (eventually replaced by [Icecap](Icecap.md))
- [Amazon ElastiCache for Redis](https://aws.amazon.com/elasticache/redis/) for logging, queuing, and synchronization
- [Amazon S3](https://aws.amazon.com/s3/) for object storage
- [Amazon CloudFront](https://aws.amazon.com/cloudfront/) for cached query result distribution

**Note**: Technically-speaking, [Amazon ElastiCache for Redis](https://aws.amazon.com/elasticache/redis/) is not serverless, yet is a fully managed service.

## CloudFormation Templates
These components are packaged into a pair of complementary [AWS CloudFormation](https://aws.amazon.com/cloudformation/) templates:
- [Lakehouse Template](../templates/lakehouse/README.md) — usually instantiated once
- [Engine Template](../templates/engine/README.md) — usually instantiated multiple times

## Clientless Interface
From the client side (browser, local application, online service), PuffinDB can be used through any HTTP client, or through [DuckDB](https://duckdb.org/) ([more](Clientless.md)).
