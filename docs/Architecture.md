# Architecture

PuffinDB is architected around the following components:

- [Engine](../functions/engine/README.md) serverless function
- [Planner](../functions/planner/README.md) serverless function
- [Amazon ElastiCache for Redis](https://aws.amazon.com/elasticache/redis/)) for logging, queuing, and synchronization
