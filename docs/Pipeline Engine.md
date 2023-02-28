# Pipeline Engine

PuffinDB embeds a powerful data pipeline engine with the following features:

- Execution of multi-step data transformation pipelines orchestred by [Redis](https://redis.io/)
- Sequential steps executed on multiple table partitions in parallel across 10,000 serverless functions or more
- Multi-threaded execution of sequential steps
- Direct function-to-function communication through [NAT hole punching](https://github.com/spcl/tcpunch)
- Pipelines defined using [JSON](https://www.json.org/) or [YAML](https://yaml.org/) syntax including [Python](https://www.python.org/) and [TypeScript](https://www.typescriptlang.org/) scripting
- Incremental pipeline execution
- Real-time observability
