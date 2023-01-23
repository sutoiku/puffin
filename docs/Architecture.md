# Architecture

PuffinDB is architected around the following components:

- [Engine](../functions/engine/README.md) Node.js serverless function implementing the core query engine
- [Planner](../functions/planner/README.md) Java serverless function implementing the distributed query planner
- [AWS EMR](https://aws.amazon.com/emr/)) for executing write queries on lakehouse tables
- [Amazon ElastiCache for Redis](https://aws.amazon.com/elasticache/redis/)) for logging, queuing, and synchronization
