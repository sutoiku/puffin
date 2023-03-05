# PuffinDB <img src="https://user-images.githubusercontent.com/1074452/220389778-245dd23e-3f09-4615-bdf1-24cd1eb8b819.png" style="margin-left: 0.25em" width="24">

Serverless [HTAP](https://en.wikipedia.org/wiki/Hybrid_transactional/analytical_processing) data mesh platform powered by [Arrow](https://arrow.apache.org/) × [DuckDB](https://duckdb.org/) × [Iceberg](https://iceberg.apache.org/)

Accelerate DuckDB with 10,000 [AWS Lambda functions](https://aws.amazon.com/lambda/) running on your own VPC

**Note**: This repository only contains preliminary design documents (*Cf.* [Roadmap](ROADMAP.md))

**Kickoff meetup**: [Rovinj, Croatia, March 29-31, 2023](meetup)

## Introduction
<img width="1217" alt="Architecture" src="https://user-images.githubusercontent.com/1074452/221385259-c8b07288-2300-40ef-a6b8-b786ce808cc4.png">

If you are using DuckDB client-side with [any client application](docs/Clientless.md), adding the [PuffinDB extension](docs/Extension.md) will let you:
- Distribute queries across thousands of serverless functions and a [Monostore](docs/Monostore.md)
- Read from and write to hundreds of applications using any [Airbyte connector](https://airbyte.com/connectors)
- Collaborate on the same [Iceberg tables](https://iceberg.apache.org/spec/) with other users
- Write back to an Iceberg table with [ACID](https://en.wikipedia.org/wiki/ACID) transactional integrity
- Execute [cross-database joins](docs/Query%20Proxy.md#query-delegation) (*Cf.* [Edge-Driven Data Integration](EDDI.md))
- Translate between 19 [SQL dialects](docs/Query%20Proxy.md#dialect-translation)
- Invoke [remote query generators](docs/Query%20Proxy.md)
- Invoke [curl](https://curl.se/) commands
- Execute incremental and observable [data pipelines](docs/Pipeline%20Engine.md)
- Turn DuckDB into a next-generation [vector database](docs/Vector%20Database.md)
- Support the [Lance](https://github.com/eto-ai/lance) file format for 100× faster random access
- Accelerate and | or schedule the downloading of large tables to your client
- Cache tables and run computations at the edge ([Amazon CloudFront](https://aws.amazon.com/cloudfront/) × [Lambda@Edge](https://aws.amazon.com/lambda/edge/))
- Log queries on your data lake

PuffinDB is an initiative of [STOIC](https://stoic.com/), and not [DuckDB Labs](https://duckdblabs.com/) or the [DuckDB Foundation](https://duckdb.org/foundation/).

DuckDB and the DuckDB logo are trademarks of the DuckDB Foundation.

PuffinDB and the PuffinDB logo are trademarks of STOIC (Sutoiku, Inc.).

STOIC is a member of the DuckDB Foundation.

## Beliefs
- Nothing beats [SQL](https://en.wikipedia.org/wiki/SQL) because nothing can beat [maths](https://en.wikipedia.org/wiki/Relational_algebra)
- The [public cloud](FAQ.md#why-not-support-private-cloud-deployment) is the only truly elastic platform
- [Arrow](https://arrow.apache.org/) × [DuckDB](https://duckdb.org/) × [Iceberg](https://iceberg.apache.org/) are game changers
- [Edge-Driven Data Integration](EDDI.md) is the way forward
- [Clientless](docs/Clientless.md) + [Serverless](docs/Architecture.md) = [Goodness](CLOUD.md)

## Rationale
Many excellent distributed SQL engines are available today. Why do we need yet another one?

- [True serverless architecture](RATIONALE.md/#true-serverless-architecture)
- [Future-proof architecture](RATIONALE.md/#future-proof-architecture)
- [Designed for virtual private cloud deployment](RATIONALE.md/#designed-for-virtual-private-cloud-deployment)
- [Designed for small to large datasets](RATIONALE.md/#designed-for-small-to-large-datasets)
- [Designed for real-time analytics](RATIONALE.md/#designed-for-real-time-analytics)
- [Designed for interactive analytics](RATIONALE.md/#designed-for-interactive-analytics)
- [Designed for transformation and analytics](RATIONALE.md/#designed-for-transformation-and-analytics)
- [Designed for analytics and transactions](RATIONALE.md/#designed-for-analytics-and-transactions)
- [Designed for next-generation query engines](RATIONALE.md/#designed-for-next-generation-query-engines)
- [Designed for next-generation file formats](RATIONALE.md/#designed-for-next-generation-file-formats)
- [Designed for lakehouses](RATIONALE.md/#designed-for-lakehouses)
- [Designed for data mesh integration](RATIONALE.md/#designed-for-data-mesh-integration)
- [Designed for all users](RATIONALE.md/#designed-for-all-users)
- [Designed for extensibility](RATIONALE.md/#designed-for-extensibility)
- [Designed for embedability](RATIONALE.md/#designed-for-embedability)
- [Optimized for machine-generated queries](RATIONALE.md/#optimized-for-machine-generated-queries)
- [Scalable across large user bases](RATIONALE.md/#scalable-across-large-user-bases)

## Outline
- True [serverless architecture](docs/Architecture.md) (run [DuckDB](https://duckdb.org/) on 10,000 [Lambda functions](https://aws.amazon.com/lambda/))
- Supporting both read and write queries ([HTAP](https://en.wikipedia.org/wiki/Hybrid_transactional/analytical_processing))
- Implemented in [Python](https://www.python.org/), [Rust](https://www.rust-lang.org/), and [TypeScript](https://www.typescriptlang.org/) (using [Bun](https://bun.sh/))
- Powered by [Arrow](https://arrow.apache.org/) × [DuckDB](https://duckdb.org/) × [Iceberg](https://iceberg.apache.org/)
- Powered by [Redis](https://redis.io/) (using [Amazon ElastiCache for Redis](https://aws.amazon.com/elasticache/redis/)) for state management
- Accelerated by [NAT hole punching](https://github.com/spcl/tcpunch) for superfast data shuffles
- Integrated with [Apache Iceberg](https://iceberg.apache.org/), [Apache Hudi](https://hudi.apache.org/), and [Delta Lake](https://delta.io/)
- Deployed on [AWS](https://aws.amazon.com/) first, then [Microsoft Azure](https://azure.microsoft.com/en-us) and [Google Cloud](https://cloud.google.com/)
- Deployed as two [AWS Lambda functions](functions/) and one [Amazon EC2](https://aws.amazon.com/ec2/) instance
- Integrated with [Amazon Athena](https://aws.amazon.com/athena/) (for write queries on lakehouse tables)
- Packaged as an [AWS CloudFormation](https://aws.amazon.com/cloudformation/) template (using [Terraform](https://www.terraform.io/))
- Released as a free [AWS Marketplace](https://aws.amazon.com/marketplace) product
- Running on your [Amazon VPC](https://aws.amazon.com/vpc/)
- Licensed under [MIT License](https://opensource.org/licenses/MIT)

## Features
- [Distributed SQL query planner](docs/Query%20Planner.md) powered by [DuckDB](https://duckdb.org/)
- [Distributed SQL query engine](docs/Query%20Engine.md) powered by [DuckDB](https://duckdb.org/)
- Distributed SQL query execution coordinated by [Redis](https://redis.io/) (using [Amazon ElastiCache for Redis](https://aws.amazon.com/elasticache/redis/))
- Distributed data shuffles enabled by direct Lambda-to-Lambda communication through [NAT hole punching](https://github.com/spcl/tcpunch)
- Read queries executed by [DuckDB](https://duckdb.org/) (on [AWS Lambda](https://aws.amazon.com/lambda/))
- Write queries against Object Store objects executed by [DuckDB](https://duckdb.org/)
- Write queries against Lakehouse tables executed by [Amazon Athena](https://aws.amazon.com/athena/)
- Built-in [Malloy](https://github.com/malloydata/malloy/tree/main/packages/malloy) to SQL translator
- Built-in [PRQL](https://prql-lang.org/) to SQL translator
- Built-in [SQL dialect converter](https://github.com/tobymao/sqlglot)
- Built-in [SQL parser | stringifier](https://twitter.com/ghalimi/status/1625172235895046146)
- Sub-500ms table scanning API (fetch table partitions from filter predicates) running on standalone function
- Advanced table metadata managed by serverless [Metastore](docs/Metastore.md)
- Concurrent support for multiple table formats ([Apache Iceberg](https://iceberg.apache.org/), [Apache Hudi](https://hudi.apache.org/), and [Delta Lake](https://delta.io/))
- Concurrent suport for multiple Lakehouse instances
- Native support for all Lakehouse Catalogs ([AWS Glue Data Catalog](https://docs.aws.amazon.com/glue/latest/dg/catalog-and-crawler.html), [Amazon DynamoDB](https://aws.amazon.com/dynamodb/), and [Amazon RDS](https://aws.amazon.com/rds/))
- Support for authentication and authorization
- Support for synchronous and asynchronous invocations
- Support for cascading remote invocations with [`SELECT THROUGH`](docs/Clientless.md) syntax
- Joins across heterogenous tables using different table formats
- Joins across tables managed by different Lakehouse instances
- Small filtered partitions [cached](FAQ.md#how-does-partition-caching-work) on [AWS Lambda](https://aws.amazon.com/lambda/) functions
- Query results returned as HTTP response, serialized on Object Store, or streamed through [Apache Arrow](https://arrow.apache.org/)
- Query results [cached](FAQ.md#how-does-query-result-caching-work) on Object Store ([Amazon S3](https://aws.amazon.com/s3/)) and CDN ([Amazon CloudFront](https://aws.amazon.com/cloudfront/))
- [Query logs](docs/Logs.md) recorded as [JSON](https://redis.io/docs/stack/json/) values in [Redis](https://redis.io/) cluster or on data lake using Parquet file
- Transparent support for all file formats supported by [DuckDB](https://duckdb.org/) and the Lakehouse
- Transparent support for all table lifecycle features offered by the Lakehouse
- Planned support for deployment on [AWS Fargate](https://aws.amazon.com/fargate/)

## Deployment
PuffinDB will support four [incremental deployment options](FAQ.md#why-support-so-many-deployment-options):
- [Node.js](https://nodejs.org/en/) and [Python](https://www.python.org/) modules deeply integrated within your own tool or application
- [AWS Lambda functions](functions/) deployed within your own cloud platform
- [AWS CloudFormation](https://aws.amazon.com/cloudformation/) template deployed within your own [VPC](https://aws.amazon.com/vpc/)
- [AWS Marketplace](https://aws.amazon.com/marketplace) product added to your own cloud environment

## Philosophy
- **Developer-first** — no non-sense, zero friction
- **Lowest latency** — every millisecond counts
- **Elastic design** — from kilobytes to petabytes

## FAQ
Please check our [Frequently Asked Questions](FAQ.md).

## Roadmap
Please check our [Roadmap](ROADMAP.md).

## Sponsors
This project was initiated and is currently funded by [STOIC](https://stoic.com/).

Please check our [sponsors](SPONSORS.md) page for sponsorship opportunities.

## Credits
This project leverages several [DuckDB](https://duckdb.org/) features implemented by [DuckDB Labs](https://duckdblabs.com/) and funded by [STOIC](https://stoic.com/):

- Support for [Apache Arrow](https://arrow.apache.org/) streaming when using [Node.js](https://nodejs.org/en/) deployment (released)
- Support for user-defined functions when using [Node.js](https://nodejs.org/en/) deployment (released)
- Support for map-reduced queries with binary map results using new [`COMBINE`](https://github.com/duckdb/duckdb/pull/2998) function (released)
- Support for import of Hive partitions (released)
- Support for [partitioned exports](https://github.com/duckdb/duckdb/pull/5964) with `COPY ... TO ... PARTITION_BY` (released)
- Support for SQL query parsing | stringifying through standard query API ([under development](https://twitter.com/ghalimi/status/1625172235895046146))
- Support for [Azure Blob Storage](https://azure.microsoft.com/en-us/products/storage/blobs) (development starting soon)

We are also considering funding the following projects:

- Support for `SELECT * THROUGH 'https://myPuffinDB.com/' FROM remoteTable` syntax (*Cf.* [EDDI](EDDI.md))
- Support for `FIXED` fixed-length character strings (*Cf.* [#3](https://github.com/sutoiku/puffin/issues/3))
- Support for `C` and `S` [`tpch-dbgen`](https://github.com/electrum/tpch-dbgen) options in `tpch` [extension](https://duckdb.org/docs/extensions/overview.html)

This project was initially inspired by this excellent [article](https://towardsdatascience.com/boost-your-cloud-data-applications-with-duckdb-and-iceberg-api-67677666fbd3) from [Alon Agmon](https://medium.com/@alon.agmon).

## Discussions
Most discussions about this project are currently taking place on the [@ghalimi](https://twitter.com/ghalimi) Twitter account.

For a lower-frequency alternative, please follow [@PuffinDB](https://twitter.com/PuffinDB).

## Notes
PuffinDB should not be confused with the [Puffin file format](https://iceberg.apache.org/puffin-spec/).

*Be stoic, be kind, be cool. Like a puffin...*

Ⓒ [Sutoiku, Inc.](https://stoic.com/)
