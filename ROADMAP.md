# Roadmap

- **Q1**: Preliminary [DuckDB extension](docs/Extension.md) (with limited subset of planned features)
- **Q2**: PuffinDB 0.1 with basic [distributed query planner](docs/Query%20Planner.md) (filter pushdown)
- **Q4**: PuffinDB 1.0 with advanced distributed query planner (including support for full [TPC-H](https://www.tpc.org/tpch/) benchmark)
- **2024**: PuffinDB 2.0 (including support for full [TPC-DS](https://www.tpc.org/tpcds/) benchmark)

## Features
Features will be implemented in the following order. Please start an `Idea` [discussion](https://github.com/sutoiku/puffin/discussions) to discuss any element of this proposed roadmap.

### Project Framework
- [x] Temporary [GitHub repository](https://github.com/sutoiku/puffin)
- [x] [Domain name](http://PuffinDB.io/)
- [x] [Logo](https://github.com/sutoiku/puffin/blob/main/media/PuffinDB.svg) design
- [x] [Core architecture design](docs/Architecture.md)
- [x] Permanent [GitHub organization](https://github.com/PuffinDB)
- [ ] Core project framework
- [ ] Continuous integration framework
- [ ] Unit testing framework
- [ ] Integration testing framework

### DuckDB Extension
- [ ] Project website
- [ ] [DuckDB extension](docs/Extension.md) (with minimal set of features)
- [ ] [Airbyte](https://airbyte.com/) connector framework
- [ ] Support for [`SELECT THROUGH`](docs/Clientless.md#select-through) syntax
- [ ] Authentication
- [ ] Authorization

### Iceberg Integration
- [ ] [Catalog serverless function](functions/catalog/README.md)
- [ ] [Lakehouse template](templates/lakehouse/README.md)
- [ ] Lakehouse catalog integration ([AWS Glue Data Catalog](https://docs.aws.amazon.com/glue/latest/dg/catalog-and-crawler.html), [Amazon DynamoDB](https://aws.amazon.com/dynamodb/), and [Amazon RDS](https://aws.amazon.com/rds/))
- [ ] Read queries against Lakehouse tables executed by [DuckDB](https://duckdb.org/)
- [ ] Write queries against Lakehouse tables executed by [Amazon Athena](https://aws.amazon.com/athena/)

### Basic Distributed Query Engine
- [ ] [Engine serverless function](functions/engine/README.md)
- [ ] [Engine template](templates/engine/README.md)
- [ ] [Remote query engine](docs/Clientless.md) running on [AWS Lambda](https://aws.amazon.com/lambda/) functions
- [ ] [Query logs](docs/Logs.md) recorded as [JSON](https://redis.io/docs/stack/json/) values in [Redis](https://redis.io/) cluster (using [Amazon ElastiCache for Redis](https://aws.amazon.com/elasticache/redis/))

### Query Proxy
- [ ] [Query proxy](docs/Query%20Proxy.md)
- [ ] [PRQL](https://prql-lang.org/) to SQL translator
- [ ] [Malloy](https://github.com/malloydata/malloy/tree/main/packages/malloy) to SQL translator
- [ ] SQL dialect converter

### Advanced Distributed Query Engine
- [ ] [Remote query engine](docs/Clientless.md) running on [Monostore](docs/Monostore.md)
- [ ] Basic [distributed query planner](docs/Query%20Planner.md)
- [ ] [Partition caching](FAQ.md#how-does-partition-caching-work) on [AWS Lambda](https://aws.amazon.com/lambda/) function
- [ ] [Query result caching](FAQ.md#how-does-query-result-caching-work) on Object Store
- [ ] [Query result caching](FAQ.md#how-does-query-result-caching-work) on CDN ([Amazon CloudFront](https://aws.amazon.com/cloudfront/))

### Other Features
- [ ] [AWS Marketplace](https://aws.amazon.com/marketplace) provisioning
- [ ] [Microsoft Azure](https://azure.microsoft.com/en-us) support
- [ ] Joins across tables managed by different Lakehouse instances
- [ ] Asynchronous invocations over [Apache Arrow](https://arrow.apache.org/)
- [ ] [Arrow Database Connectivity](https://arrow.apache.org/docs/dev/format/ADBC.html) support
- [ ] Concurrent suport for multiple Lakehouse instances
- [ ] Advanced [distributed query planner](docs/Query%20Planner.md)
- [ ] [Delta Lake](https://delta.io/) support
- [ ] [Apache Hudi](https://hudi.apache.org/) support
- [ ] Joins across heterogenous tables using different table formats
- [ ] [AWS Fargate](https://aws.amazon.com/fargate/) support
- [ ] [Google Cloud](https://cloud.google.com/) support
