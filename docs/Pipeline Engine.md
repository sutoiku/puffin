# Pipeline Engine

PuffinDB embeds a powerful data pipeline engine with the following features:

- Integration with data lakes like [Iceberg](https://iceberg.apache.org/), [Delta Lake](https://delta.io/), and [Hudi](https://hudi.apache.org/)
- Execution of multi-step data transformation pipelines orchestred by [Redis](https://redis.io/)
- Sequential steps executed on multiple table partitions in parallel across 10,000 serverless functions or more
- Multi-threaded execution of individual steps
- Blocking and non-blocking step execution
- Direct function-to-function communication through [NAT hole punching](https://github.com/spcl/tcpunch)
- Pipelines defined using [JSON](https://www.json.org/) or [YAML](https://yaml.org/) syntax including [Python](https://www.python.org/) and [TypeScript](https://www.typescriptlang.org/) scripting
- Steps defined with [SQL](Query%20Proxy.md#dialect-translation) or [PRQL](https://prql-lang.org/) extended with user-defined functions powered by [Python](https://www.python.org/), [TypeScript](https://www.typescriptlang.org/), or [WebAssembly](https://webassembly.org/)
- Support for steps invoking [curl](https://curl.se/) commands or any [Airbyte connector](https://airbyte.com/connectors)
- Incremental pipeline execution
- Real-time observability
