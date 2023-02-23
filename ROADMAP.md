# Roadmap

- **Q2**: PuffinDB 0.1 with basic [distributed query planner](docs/Query%20Planner.md) (filter pushdown)
- **Q4**: PuffinDB 1.0 with advanced distributed query planner (including support for full [TPC-H](https://www.tpc.org/tpch/) benchmark)
- **2024**: PuffinDB 2.0 (including support for full [TPC-DS](https://www.tpc.org/tpcds/) benchmark)

## Features
Features will be implemented in the following order. Please start an `Idea` [discussion](https://github.com/sutoiku/puffin/discussions) to discuss any element of this proposed roadmap.

- [x] [GitHub repository](https://github.com/sutoiku/puffin)
- [x] [Domain name](http://PuffinDB.io/)
- [x] [Logo](https://github.com/sutoiku/puffin/blob/main/media/PuffinDB.svg) design
- [x] [Core architecture design](docs/Architecture.md)
- [ ] Core project framework
- [ ] Unit testing framework
- [ ] Integration testing framework
- [ ] [DuckDB extension](docs/Extension.md)
- [ ] [Airbyte](https://airbyte.com/) connector framework (with [DuckDB Python API](https://duckdb.org/docs/api/python/overview.html) only)
- [ ] [Airbyte](https://airbyte.com/) connector framework (with any [DuckDB](https://duckdb.org/) client)
- [ ] [Cross-database joins](docs/Query%20Proxy.md#query-delegation) powered by [Ibis](https://ibis-project.org/)
- [ ] Support for [`SELECT THROUGH`](docs/Clientless.md#select-through) syntax
- [ ] Authentication
- [ ] Authorization
- [ ] Read queries executed by [DuckDB](https://duckdb.org/) (running on [AWS Lambda](https://aws.amazon.com/lambda/))
- [ ] Read queries executed by [Amazon Athena](https://aws.amazon.com/athena/)
- [ ] Write queries against Object Store objects executed by [DuckDB](https://duckdb.org/)
- [ ] Write queries against Lakehouse tables executed by [Amazon Athena](https://aws.amazon.com/athena/)
- [ ] [Engine serverless function](functions/engine/README.md)
- [ ] [Engine template](templates/engine/README.md)
- [ ] [Catalog serverless function](functions/catalog/README.md)
- [ ] [Catalog template](templates/catalog/README.md)
- [ ] Lakehouse catalog integration ([AWS Glue Data Catalog](https://docs.aws.amazon.com/glue/latest/dg/catalog-and-crawler.html), [Amazon DynamoDB](https://aws.amazon.com/dynamodb/), and [Amazon RDS](https://aws.amazon.com/rds/))
- [ ] [Remote query engine](docs/Clientless.md) running on [AWS Lambda](https://aws.amazon.com/lambda/) functions
- [ ] [Query logs](docs/Logs.md) recorded as [JSON](https://redis.io/docs/stack/json/) values in [Redis](https://redis.io/) cluster (using [Amazon ElastiCache for Redis](https://aws.amazon.com/elasticache/redis/))
- [ ] [PRQL](https://prql-lang.org/) to SQL translator
- [ ] [Malloy](https://github.com/malloydata/malloy/tree/main/packages/malloy) to SQL translator
- [ ] [Lakehouse template](templates/lakehouse/README.md)
- [ ] [Amazon ElastiCache for Redis](https://aws.amazon.com/elasticache/redis/) support
- [ ] [Partition caching](FAQ.md#how-does-partition-caching-work) on [AWS Lambda](https://aws.amazon.com/lambda/) function
- [ ] [Query result caching](FAQ.md#how-does-query-result-caching-work) on Object Store
- [ ] [Query result caching](FAQ.md#how-does-query-result-caching-work) on CDN ([Amazon CloudFront](https://aws.amazon.com/cloudfront/))
- [ ] Basic [distributed query planner](docs/Query%20Planner.md)
- [ ] Distributed query execution across multiple serverless functions
- [ ] [Query proxy](docs/Query%20Proxy.md)
- [ ] [Microsoft Azure](https://azure.microsoft.com/en-us) support
- [ ] Joins across tables managed by different Lakehouse instances
- [ ] Asynchronous invocations over [Apache Arrow](https://arrow.apache.org/)
- [ ] [Arrow Database Connectivity](https://arrow.apache.org/docs/dev/format/ADBC.html) support
- [ ] [Amazon EC2](https://aws.amazon.com/ec2/) support
- [ ] [AWS Fargate](https://aws.amazon.com/fargate/) support
- [ ] [Amazon OpenSearch Service](https://aws.amazon.com/opensearch-service/) support
- [ ] SQL dialect converter
- [ ] Project website
- [ ] [AWS Marketplace](https://aws.amazon.com/marketplace) provisioning
- [ ] Concurrent suport for multiple Lakehouse instances
- [ ] Advanced [distributed query planner](docs/Query%20Planner.md)
- [ ] [Delta Lake](https://delta.io/) support
- [ ] [Apache Hudi](https://hudi.apache.org/) support
- [ ] Joins across heterogenous tables using different table formats
- [ ] [Google Cloud](https://cloud.google.com/) support
