# Serverless Architecture

PuffinDB is architected around the following serverless components:

- [Engine](../functions/engine/README.md) Node.js serverless function implementing the core query engine powered by [DuckDB](https://duckdb.org/)
- [Planner](../functions/planner/README.md) Java serverless function implementing the distributed query planner
- [AWS EMR](https://aws.amazon.com/emr/) for executing write queries on lakehouse tables
- [Amazon ElastiCache for Redis](https://aws.amazon.com/elasticache/redis/) for logging, queuing, and synchronization
- [Amazon S3](https://aws.amazon.com/s3/) for object storage
- [Amazon CloudFront](https://aws.amazon.com/cloudfront/) for cached query result distribution

**Note**: Technically-speaking, [Amazon ElastiCache for Redis](https://aws.amazon.com/elasticache/redis/) is not serverless, yet is a fully managed service.
