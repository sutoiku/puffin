# PuffinDB 🐧
Serverless data lake query engine powered by [Arrow](https://arrow.apache.org/) × [DuckDB](https://duckdb.org/) × [Iceberg](https://iceberg.apache.org/)

## Introduction
This is a proposal for an open source project [sponsored](FAQ.md#why-is-stoic-initiating-and-funding-this-open-source-project) by [STOIC](https://stoic.com/). Its purpose is to make it easier to run [DuckDB](https://duckdb.org/) on serverless functions ([AWS Lambda](https://aws.amazon.com/lambda/), [Azure Function](https://learn.microsoft.com/en-us/azure/azure-functions/functions-overview), [Google Cloud Function](https://cloud.google.com/functions)) for executing read | write queries against objects managed by an Object Store ([Amazon S3](https://aws.amazon.com/s3/), [Azure Blob Storage](https://azure.microsoft.com/en-us/products/storage/blobs), [Google Cloud Storage](https://cloud.google.com/storage)) and tables managed by a Lakehouse ([Apache Iceberg](https://iceberg.apache.org/), [Apache Hudi](https://hudi.apache.org/), [Delta Lake](https://delta.io/)).

If you are using DuckDB client-side with [any client application](docs/Clientless.md), adding PuffinDB will let you:
- Collaborate on the same [Iceberg tables](https://iceberg.apache.org/spec/) with other users
- Write back to an Iceberg table with [ACID](https://en.wikipedia.org/wiki/ACID) transactional integrity
- Handle datasets that are too large for your client
- Accelerate queries that run too slow on your client
- Integrate with external data sources (*Cf.* [Edge-Driven Data Integration](EDDI.md))
- Accelerate the downloading of large tables to your client
- Schedule fetching and client-side caching of [remote datasets](docs/Clientless.md#scheduled-remote-data-fetching-and-local-caching)
- Cache tables and run computations at the edge ([Amazon CloudFront](https://aws.amazon.com/cloudfront/) × [Lambda@Edge](https://aws.amazon.com/lambda/edge/))

All it takes is an [AWS](https://aws.amazon.com/) account and a few clicks on the [AWS Marketplace](https://aws.amazon.com/marketplace) (PuffinDB is free and [runs on your VPC](FAQ.md#why-support-so-many-deployment-options)).

## Beliefs
- Nothing beats [SQL](https://en.wikipedia.org/wiki/SQL) because nothing can beat [maths](https://en.wikipedia.org/wiki/Relational_algebra)
- [Arrow](https://arrow.apache.org/) × [DuckDB](https://duckdb.org/) × [Iceberg](https://iceberg.apache.org/) are game changers
- [Edge-Driven Data Integration](EDDI.md) is the way forward

## Outline
- [Serverless architecture](docs/Architecture.md)
- Implemented in [Node.js](https://nodejs.org/en/)
- Powered by [Arrow](https://arrow.apache.org/) × [DuckDB](https://duckdb.org/) × [Iceberg](https://iceberg.apache.org/)
- Integrated with [Apache Iceberg](https://iceberg.apache.org/) first, then [Apache Hudi](https://hudi.apache.org/) and [Delta Lake](https://delta.io/)
- Deployed on [AWS](https://aws.amazon.com/) first, then [Microsoft Azure](https://azure.microsoft.com/en-us) and [Google Cloud](https://cloud.google.com/)
- Invoked through an HTTP endpoint served by [Amazon API Gateway](https://aws.amazon.com/api-gateway/)
- Deployed as two [AWS Lambda](https://aws.amazon.com/lambda/) functions
- Integrated with [AWS EMR](https://aws.amazon.com/emr/) (EKS or Serverless)
- Packaged as an [AWS CloudFormation](https://aws.amazon.com/cloudformation/) template
- Released as a free [AWS Marketplace](https://aws.amazon.com/marketplace) product
- Running on your [Amazon VPC](https://aws.amazon.com/vpc/)
- Licensed under [MIT License](https://opensource.org/licenses/MIT)

## Features
- [Distributed SQL query planner](docs/Query%20Planner.md) powered by [DuckDB](https://duckdb.org/)
- Distributed SQL query execution coordinated by [Redis](https://redis.io/) (using [Amazon ElastiCache for Redis](https://aws.amazon.com/elasticache/redis/))
- Read queries executed by [DuckDB](https://duckdb.org/) (on [AWS Lambda](https://aws.amazon.com/lambda/)) or [Spark SQL](https://spark.apache.org/sql/) (on [AWS EMR](https://aws.amazon.com/emr/))
- Write queries against Object Store objects executed by or [DuckDB](https://duckdb.org/)
- Write queries against Lakehouse tables executed by [Spark SQL](https://spark.apache.org/sql/)
- Built-in SQL dialect converter
- Built-in SQL parser | stringifier
- Sub-500ms table scanning API (fetch table partitions from filter predicates) running on standalone function
- Concurrent support for multiple table formats ([Apache Iceberg](https://iceberg.apache.org/) first, then [Apache Hudi](https://hudi.apache.org/) and [Delta Lake](https://delta.io/))
- Concurrent suport for multiple Lakehouse instances
- Native support for all Lakehouse Catalogs ([AWS Glue Data Catalog](https://docs.aws.amazon.com/glue/latest/dg/catalog-and-crawler.html), [Amazon DynamoDB](https://aws.amazon.com/dynamodb/), and [Amazon RDS](https://aws.amazon.com/rds/))
- Support for authentication and authorization
- Support for synchronous and asynchronous invocations
- Support for cascading remote invocations with [`SELECT THROUGH`](docs/Clientless.md) syntax
- Joins across heterogenous tables using different table formats
- Joins across tables managed by different Lakehouse instances
- Small filtered partitions [cached](FAQ.md#how-does-partition-caching-work) on [AWS Lambda](https://aws.amazon.com/lambda/) function
- Query results returned as HTTP response, serialized on Object Store, or streamed through [Apache Arrow](https://arrow.apache.org/)
- Query results [cached](FAQ.md#how-does-query-result-caching-work) on Object Store ([Amazon S3](https://aws.amazon.com/s3/)) and CDN ([Amazon CloudFront](https://aws.amazon.com/cloudfront/))
- [Query logs](docs/Logs.md) recorded as [JSON](https://redis.io/docs/stack/json/) values in [Redis](https://redis.io/) cluster (using [Amazon ElastiCache for Redis](https://aws.amazon.com/elasticache/redis/))
- Transparent support for all file formats supported by [DuckDB](https://duckdb.org/) and the Lakehouse
- Transparent support for all table lifecycle features offered by the Lakehouse
- Planned support for deployment on [Amazon EC2](https://aws.amazon.com/ec2/) and [AWS Fargate](https://aws.amazon.com/fargate/)
- Planned support for deployment across fleet of [AWS Lambda](https://aws.amazon.com/lambda/) functions

## Deployment
PuffinDB will support four [complementary deployment options](FAQ.md#why-support-so-many-deployment-options):
- [Node.js](https://nodejs.org/en/) module deeply integrated within your own tool or application
- [AWS Lambda](https://aws.amazon.com/lambda/) deployed within your own cloud platform
- [AWS CloudFormation](https://aws.amazon.com/cloudformation/) template deployed within your own [VPC](https://aws.amazon.com/vpc/)
- [AWS Marketplace](https://aws.amazon.com/marketplace) product added to your own cloud environment

## Philosophy
- **Developer-first** — no non-sense, zero friction
- **Minimalist architecture** — less dependencies is better
- **Lowest latency** — every millisecond counts
- **Elastic design** — from kilobytes to petabytes

## FAQ
Please check our [Frequently Asked Questions](FAQ.md).

## Roadmap
Please check our [Roadmap](ROADMAP.md).

## Credits
This project leverages several [DuckDB](https://duckdb.org/) features implemented by [DuckDB Labs](https://duckdblabs.com/) and funded by [STOIC](https://stoic.com/):

- Support for [Apache Arrow](https://arrow.apache.org/) streaming when using [Node.js](https://nodejs.org/en/) deployment (released)
- Support for user-defined functions when using [Node.js](https://nodejs.org/en/) deployment (released)
- Support for map-reduced queries with binary map results using new [`COMBINE`](https://github.com/duckdb/duckdb/pull/2998) function (released)
- Support for import of Hive partitions (released)
- Support for [partitioned exports](https://github.com/duckdb/duckdb/pull/5964) with `COPY ... TO ... PARTITION_BY` (released)
- Support for SQL query parsing | stringifying through standard query API (development starting soon)
- Support for [Azure Blob Storage](https://azure.microsoft.com/en-us/products/storage/blobs) (development to start a bit later)

We are also considering funding the following projects:

- Expose core methods for [distributed query planner](docs/Query%20Planner.md).
- Support for `SELECT THROUGH 'https://myPuffinDB.com/' * FROM remoteTable` syntax (*Cf.* [EDDI](EDDI.md)).
- Support for `FIXED` fixed-length character strings (*Cf.* [#3](https://github.com/sutoiku/puffin/issues/3)).

This project was initially inspired by this excellent [article](https://towardsdatascience.com/boost-your-cloud-data-applications-with-duckdb-and-iceberg-api-67677666fbd3) from [Alon Agmon](https://medium.com/@alon.agmon).

## Notes

PuffinDB should not be confused with the [Puffin file format](https://iceberg.apache.org/puffin-spec/).

Ⓒ [Sutoiku, Inc.](https://stoic.com/)
