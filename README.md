# Puffin
Serverless lakehouse query engine powered by [Apache Arrow](https://arrow.apache.org/) and [DuckDB](https://duckdb.org/)


## Introduction
This is a proposal for an Open Source project sponsored by [STOIC](https://stoic.com/). Its purpose is to make it easier to run [DuckDB](https://duckdb.org/) on a serverless function ([AWS Lambda](https://aws.amazon.com/lambda/), [Azure Function](https://learn.microsoft.com/en-us/azure/azure-functions/functions-overview), [Google Cloud Function](https://cloud.google.com/functions)) for executing read/write queries against objects managed by an Object Store ([Amazon S3](https://aws.amazon.com/s3/), [Azure Blob Storage](https://azure.microsoft.com/en-us/products/storage/blobs), [Google Cloud Storage](https://cloud.google.com/storage)) and tables managed by a Lakehouse ([Apache Iceberg](https://iceberg.apache.org/), [Apache Hudi](https://hudi.apache.org/), [Delta Lake](https://delta.io/)).

## Outline
- Written in [TypeScript](https://www.typescriptlang.org/)
- Deployed on [Node.js](https://nodejs.org/en/) (or [Bun](https://bun.sh/))
- Powered by [Apache Arrow](https://arrow.apache.org/) and [DuckDB](https://duckdb.org/)
- Integrated with [Apache Iceberg](https://iceberg.apache.org/) first, then [Apache Hudi](https://hudi.apache.org/), and [Delta Lake](https://delta.io/)
- Designed for [AWS](https://aws.amazon.com/) (with planned support for [Microsoft Azure](https://azure.microsoft.com/en-us) and [Google Cloud](https://cloud.google.com/))
- Invoked through an HTTP endpoint served by [Amazon API Gateway](https://aws.amazon.com/api-gateway/)
- Deployed as an [AWS Lambda](https://aws.amazon.com/lambda/) function (or a couple of functions)
- Integrated with [AWS EMR](https://aws.amazon.com/emr/) (EKS and Serverless)
- Packaged as an [AWS CloudFormation](https://aws.amazon.com/cloudformation/) template
- Released as a free [AWS Marketplace](https://aws.amazon.com/marketplace) product
- Running on your [Amazon VPC](https://aws.amazon.com/vpc/)
- Licensed under [MIT License](https://opensource.org/licenses/MIT)

## Features
- Read queries executed by [DuckDB](https://duckdb.org/) (running on [AWS Lambda](https://aws.amazon.com/lambda/)) or [Spark SQL](https://spark.apache.org/sql/) (running on [AWS EMR](https://aws.amazon.com/emr/))
- Write queries on Object Store executed by [DuckDB](https://duckdb.org/)
- Write queries on Lakehouse executed by [Spark SQL](https://spark.apache.org/sql/)
- Built-in [DuckDB](https://duckdb.org/) to [Spark SQL](https://spark.apache.org/sql/) SQL dialect converter for write queries on Lakehouse
- Built-in SQL parser/stringifier using native [DuckDB](https://duckdb.org/) SQL parser/stringifier
- Low-latency table scanning API (fetch table partitions from filter predicates) running on standalone function
- Concurrent support for multiple table formats ([Apache Iceberg](https://iceberg.apache.org/) first, then [Apache Hudi](https://hudi.apache.org/), [Delta Lake](https://delta.io/))
- Concurrent suport for multiple Lakehouse instances
- Native support for all Lakehouse Catalogs ([AWS Glue Data Catalog](https://docs.aws.amazon.com/glue/latest/dg/catalog-and-crawler.html), [Amazon DynamoDB](https://aws.amazon.com/dynamodb/), and [Amazon RDS](https://aws.amazon.com/rds/))
- Support for both synchronous and asynchronous invocations
- Query results returned as HTTP reponse, serialized on Object Store, or streamed through [Apache Arrow](https://arrow.apache.org/)
- Query logs recorded on Lakehouse table in [Apache Parquet](https://parquet.apache.org/) format
- Transparent support for all file formats supported by [DuckDB](https://duckdb.org/) and the Lakehouse ([Apache Parquet](https://parquet.apache.org/) strongly recommended for now)
- Transparent support for all table lifecycle features offered by the Lakehouse

## Credits
This project was initially inspired by this excellent [article](https://towardsdatascience.com/boost-your-cloud-data-applications-with-duckdb-and-iceberg-api-67677666fbd3) from [Alon Agmon](https://medium.com/@alon.agmon).
