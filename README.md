# PuffinDB 🐧
Serverless data lake query engine powered by [Iceberg](https://iceberg.apache.org/) × [DuckDB](https://duckdb.org/) × [Arrow](https://arrow.apache.org/)


## Introduction
This is a proposal for an open source project sponsored by [STOIC](https://stoic.com/). Its purpose is to make it easier to run [DuckDB](https://duckdb.org/) on serverless functions ([AWS Lambda](https://aws.amazon.com/lambda/), [Azure Function](https://learn.microsoft.com/en-us/azure/azure-functions/functions-overview), [Google Cloud Function](https://cloud.google.com/functions)) for executing read | write queries against objects managed by an Object Store ([Amazon S3](https://aws.amazon.com/s3/), [Azure Blob Storage](https://azure.microsoft.com/en-us/products/storage/blobs), [Google Cloud Storage](https://cloud.google.com/storage)) and tables managed by a Lakehouse ([Apache Iceberg](https://iceberg.apache.org/), [Apache Hudi](https://hudi.apache.org/), [Delta Lake](https://delta.io/)).

## Beliefs
- Nothing beats SQL, because nothing can beat maths
- [Iceberg](https://iceberg.apache.org/) × [DuckDB](https://duckdb.org/) × [Arrow](https://arrow.apache.org/) are game changers
- [Edge-Driven Data Integration](./EDDI.md) is the way forward

## Outline
- Written in [TypeScript](https://www.typescriptlang.org/)
- Deployed on [Node.js](https://nodejs.org/en/) (or [Bun](https://bun.sh/))
- Powered by [Iceberg](https://iceberg.apache.org/) × [DuckDB](https://duckdb.org/) × [Arrow](https://arrow.apache.org/)
- Integrated with [Apache Iceberg](https://iceberg.apache.org/) first, then [Apache Hudi](https://hudi.apache.org/) and [Delta Lake](https://delta.io/)
- Designed for [AWS](https://aws.amazon.com/) (with planned support for [Microsoft Azure](https://azure.microsoft.com/en-us) and [Google Cloud](https://cloud.google.com/))
- Invoked through an HTTP endpoint served by [Amazon API Gateway](https://aws.amazon.com/api-gateway/)
- Deployed as an [AWS Lambda](https://aws.amazon.com/lambda/) function (or a couple of functions)
- Integrated with [AWS EMR](https://aws.amazon.com/emr/) (EKS or Serverless)
- Packaged as an [AWS CloudFormation](https://aws.amazon.com/cloudformation/) template
- Released as a free [AWS Marketplace](https://aws.amazon.com/marketplace) product
- Running on your [Amazon VPC](https://aws.amazon.com/vpc/)
- Licensed under [MIT License](https://opensource.org/licenses/MIT)

## Features
- Read queries executed by [DuckDB](https://duckdb.org/) (running on [AWS Lambda](https://aws.amazon.com/lambda/)) or [Spark SQL](https://spark.apache.org/sql/) (running on [AWS EMR](https://aws.amazon.com/emr/))
- Write queries on Object Store executed by [DuckDB](https://duckdb.org/)
- Write queries on Lakehouse executed by [Spark SQL](https://spark.apache.org/sql/)
- Built-in [DuckDB](https://duckdb.org/) to [Spark SQL](https://spark.apache.org/sql/) SQL dialect converter for write queries on Lakehouse
- Built-in SQL parser | stringifier using native [DuckDB](https://duckdb.org/) SQL parser | stringifier
- Sub-500ms table scanning API (fetch table partitions from filter predicates) running on standalone function
- Concurrent support for multiple table formats ([Apache Iceberg](https://iceberg.apache.org/) first, then [Apache Hudi](https://hudi.apache.org/) and [Delta Lake](https://delta.io/))
- Concurrent suport for multiple Lakehouse instances
- Native support for all Lakehouse Catalogs ([AWS Glue Data Catalog](https://docs.aws.amazon.com/glue/latest/dg/catalog-and-crawler.html), [Amazon DynamoDB](https://aws.amazon.com/dynamodb/), and [Amazon RDS](https://aws.amazon.com/rds/))
- Support for authentication and authorization
- Support for both synchronous and asynchronous invocations
- Joins across heterogenous tables using different table formats
- Joins across tables managed by different Lakehouse instances
- Small filtered partitions cached on [AWS Lambda](https://aws.amazon.com/lambda/) function
- Query results returned as HTTP reponse, serialized on Object Store, or streamed through [Apache Arrow](https://arrow.apache.org/)
- Query results cached on Object Store
- Query logs recorded on Lakehouse table in [Apache Parquet](https://parquet.apache.org/) format
- Transparent support for all file formats supported by [DuckDB](https://duckdb.org/) and the Lakehouse
- Transparent support for all table lifecycle features offered by the Lakehouse
- Planned support for deployment on [Amazon EC2](https://aws.amazon.com/ec2/) and [AWS Fargate](https://aws.amazon.com/fargate/)
- Planned support for deployment across fleet of [AWS Lambda](https://aws.amazon.com/lambda/) functions using distributed SQL query planner

## Deployment
PuffinDB will support four complementary deployment models:
- [Node.js](https://nodejs.org/en/) module deeply integrated within your own tool or application
- [AWS Lambda](https://aws.amazon.com/lambda/) deployed within your own cloud platform
- [AWS CloudFormation](https://aws.amazon.com/cloudformation/) template deployed within your own [VPC](https://aws.amazon.com/vpc/)
- [AWS Marketplace](https://aws.amazon.com/marketplace) product added to your own cloud environment

## Philosophy
- **Developer-first** — no non-sense, zero friction
- **Minimalist architecture** — the fewer dependencies the better
- **Lowest latency** — every millisecond counts
- **Elastic design** — from kilobytes to petabytes

## FAQ
Please check our [Frequently Asked Questions](./FAQ.md).

## Credits
This project leverages many [DuckDB](https://duckdb.org/) features implemented by [DuckDB Labs](https://duckdblabs.com/) and funded by [STOIC](https://stoic.com/):

- Support for [Apache Arrow](https://arrow.apache.org/) streaming when using [Node.js](https://nodejs.org/en/) deployment (released)
- Support for user-defined functions when using [Node.js](https://nodejs.org/en/) deployment (released)
- Support for map-reduced queries with binary map results using new [`COMBINE`](https://github.com/duckdb/duckdb/pull/2998) function (released)
- Support for partitioned exports (to be released soon)
- Support for SQL query parsing/stringifying through standard query API (development starting soon)
- Support for [Azure Blob Storage](https://azure.microsoft.com/en-us/products/storage/blobs) (development to start a bit later)

We are also considering funding the following projects:

- Support for `SELECT REMOTE 'https://myPuffinService.com/' * FROM myRemoteTable` syntax (*C.f.* [issue](https://github.com/sutoiku/puffin/issues/4)).
- Support for [Bun](https://bun.sh/) deployment as a faster alternative to [Node.js](https://nodejs.org/en/).
- Support for `FIXED` fixed-length character strings (*C.f.* [issue](https://github.com/sutoiku/puffin/issues/3)).

This project was initially inspired by this excellent [article](https://towardsdatascience.com/boost-your-cloud-data-applications-with-duckdb-and-iceberg-api-67677666fbd3) from [Alon Agmon](https://medium.com/@alon.agmon).

## Notes

PuffinDB should not be confused with the [Puffin file format](https://iceberg.apache.org/puffin-spec/).

Ⓒ [Sutoiku, Inc.](https://stoic.com/)
