# Architecture

PuffinDB has a radical cloud-native architecture. Deployment on "private clouds" is not a priority.

## Serverless Components
PuffinDB is architected around the following serverless components:

- [Engine](../functions/engine/README.md) Node.js serverless function implementing the core query engine powered by [DuckDB](https://duckdb.org/) and [Apache Arrow](https://arrow.apache.org/)
- [Planner](../functions/planner/README.md) Java serverless function implementing the distributed query planner powered by [Apache Calcite](https://calcite.apache.org/) and [Iceberg's Java API](https://iceberg.apache.org/docs/latest/api/)
- [AWS EMR Serverless](https://aws.amazon.com/emr/serverless/) for executing write queries on lakehouse tables
- [Amazon ElastiCache for Redis](https://aws.amazon.com/elasticache/redis/) for logging, queuing, and synchronization
- [Amazon S3](https://aws.amazon.com/s3/) for object storage
- [Amazon CloudFront](https://aws.amazon.com/cloudfront/) for cached query result distribution

**Note**: Technically-speaking, [Amazon ElastiCache for Redis](https://aws.amazon.com/elasticache/redis/) is not serverless, yet is a fully managed service.

## CloudFormation Templates
These components are packaged into these complementary [AWS CloudFormation](https://aws.amazon.com/cloudformation/) templates:
- [Lakehouse Template](../templates/lakehouse/README.md)
- [Engine Template](../templates/engine/README.md)
