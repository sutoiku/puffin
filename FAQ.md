# Frequently Asked Questions

Please ask new questions as a `Q&A` in [discussions](https://github.com/sutoiku/puffin/discussions).

## What is PuffinDB?
PuffinDB is a serverless data lake query engine powered by [Iceberg](https://iceberg.apache.org/) × [DuckDB](https://duckdb.org/) × [Arrow](https://arrow.apache.org/).

## Why should I use PuffinDB?
PuffinDB makes it much easier to run [DuckDB](https://duckdb.org/) on serverless functions ([AWS Lambda](https://aws.amazon.com/lambda/), [Azure Function](https://learn.microsoft.com/en-us/azure/azure-functions/functions-overview), [Google Cloud Function](https://cloud.google.com/functions)) for executing read | write queries against objects managed by an Object Store ([Amazon S3](https://aws.amazon.com/s3/), [Azure Blob Storage](https://azure.microsoft.com/en-us/products/storage/blobs), [Google Cloud Storage](https://cloud.google.com/storage)) and tables managed by a Lakehouse ([Apache Iceberg](https://iceberg.apache.org/), [Apache Hudi](https://hudi.apache.org/), [Delta Lake](https://delta.io/)).

## What is Edge-Driven Data Integration?
[Edge-Driven Data Integration](EDDI.md) (EDDI) is an inversion of control proposed by the PuffinDB project. Its main idea is that data integration should be driven at the edge by dynamic user-driven integration scenarios, rather than on the cloud with static data integration pipelines, yet without sacrificing solid architecture design and proper data governance ([read more](EDDI.md)).

## Why should I run DuckDB on the cloud instead of my personal computer?
While running [DuckDB](https://duckdb.org/) on your personal computer will work great in many instances, running it on the cloud can bring several benefits:
- No need to download large datasets from the cloud to your local computer.
- Ability to work with larger datasets by taking advantage of fleets of [AWS Lambda](https://aws.amazon.com/lambda/) functions and | or large [Amazon EC2](https://aws.amazon.com/ec2/) instances.
- Enforcement of column and | or row-level access control policies.

## Can I still run DuckDB client-side while using PuffinDB cloud-side?
Of course! In fact, this is probably the best way to take advantage of PuffinDB. To do so with a plain-vanilla client-side version of [DuckDB](https://duckdb.org/), simply export your PuffinDB query results to your Object Store as [Apache Parquet](https://parquet.apache.org/) files, then download these files onto your client so that you can query them using your local DuckDB engine. Down the road, it is quite likely that DuckDB will directly support more advanced protocols to make this integration totally seamless (*C.f.* [related issue](https://github.com/sutoiku/puffin/issues/4)).

## Why use both DataFusion and DuckDB as cloud-side query engines?
[DuckDB](https://duckdb.org/) is the absolute best client-side query engine, while being a great option cloud-side as well. Nevertheless, [DataFusion](https://arrow.apache.org/datafusion/) and [Ballista](https://github.com/apache/arrow-ballista) provide some critical building blocks for the development of a distributed SQL query planner, as discussed in this [issue](https://github.com/sutoiku/puffin/issues/7). Therefore, the [`Engine`](functions/engine/README.md) packages the two into a single [Rust](https://github.com/awslabs/aws-lambda-rust-runtime) serverless function. This function is used both as query handler and query engine, in a fully recursive manner. Having both engines also makes it possible to use the fastest engine for a particular query, or to benefit from a specific API, file format, or data type that is not supported by the other. For example, DataFusion supports [Arrow Flight SQL](https://arrow.apache.org/docs/format/FlightSql.html) and [Avro](https://avro.apache.org/docs/1.2.0/), while DuckDB supports a richer set of [data types](TYPES.md) (including `LIST`, `MAP`, and `STRUCT`).

## Can I use PuffinDB without a Lakehouse?
Yes, you can use PuffinDB with just an Object Store like [Amazon S3](https://aws.amazon.com/s3/). But you should still take a look at [Iceberg](https://iceberg.apache.org/), for the following reasons:

## Why should I use a Lakehouse like Iceberg instead of just an Object Store like S3?
A Lakehouse like [Apache Iceberg](https://iceberg.apache.org/), [Apache Hudi](https://hudi.apache.org/), and [Delta Lake](https://delta.io/) offers many critical features:
- [Schema evolution](https://iceberg.apache.org/docs/latest/evolution/#schema-evolution) supports add, drop, update, or rename, and has [no side-effects](https://iceberg.apache.org/docs/latest/evolution/#correctness).
- [Hidden partitioning](https://iceberg.apache.org/docs/latest/partitioning/) prevents user mistakes that cause silently incorrect results or extremely slow queries.
- [Partition layout evolution](https://iceberg.apache.org/docs/latest/evolution/#partition-evolution) can update the layout of a table as data volume or query patterns change.
- [Time travel](https://iceberg.apache.org/docs/latest/spark-queries/#time-travel) enables reproducible queries that use exactly the same table snapshot.
- [Table version rollback](https://iceberg.apache.org/docs/latest/) allows users to quickly correct problems by resetting tables to a good state.
- [Advanced filtering](https://iceberg.apache.org/docs/latest/performance/#data-filtering) prunes data files with partition and column-level stats, using table metadata.
- [Serializable isolation](https://iceberg.apache.org/docs/latest/reliability/) makes table changes atomic and ensures that readers never see partial or uncommitted changes.
- [Multiple concurrent writers](https://iceberg.apache.org/docs/latest/reliability/#concurrent-write-operations) use optimistic concurrency and transaction retry.

Credits: feature descriptions courtesy of [Apache Iceberg](https://iceberg.apache.org/).

## Won't DuckDB support Iceberg soon?
Yes, it will (or so we've [read](https://twitter.com/tabulario/status/1616467434772533250)). Nevertheless, this support is likely to be limited to [DuckDB](https://duckdb.org/) running on a single host (client, function, or VM). PuffinDB is designed to support DuckDB running on multiple functions in parallel, or multiple functions and a VM coordinating their work. Consequently, while DuckDB can directly request a [table scan](https://iceberg.apache.org/docs/latest/api/#scanning) through [Iceberg](https://iceberg.apache.org/)'s [Java API](https://iceberg.apache.org/docs/latest/api/) when running in single-host mode, this scan must be performed outside of DuckDB when running in multi-host mode. Furthermore, PuffinDB supports the optional execution of some read queries on [Spark SQL](), whenever such delegated execution would be faster or more cost-effective.

## Why not just use Spark SQL?
[Spark SQL](https://spark.apache.org/sql/) can be use to read | write any [Iceberg tables](https://iceberg.apache.org/spec/), but many read queries will be faster and more cost-effective when using [DuckDB](https://duckdb.org/).

## Do I need Amazon EMR to query Iceberg tables with PuffinDB?
If you just make read queries on [Iceberg tables](https://iceberg.apache.org/spec/), you do not. But you will need [Amazon EMR](https://aws.amazon.com/emr/) (EKS or Serverless), or any suitable alternative [Apache Spark](https://spark.apache.org/) deployment running [Apache Iceberg](https://iceberg.apache.org/) if you want to make write queries on these tables. Thankfully, [Amazon EMR Serverless](https://aws.amazon.com/emr/serverless/) comes pre-configured with the default [AWS CloudFormation](https://aws.amazon.com/cloudformation/) template that is part of Pufin. Furthermore, [Amazon EMR Serverless](https://aws.amazon.com/emr/serverless/) is only 30% more expensive than running barebone [AWS Lambda](https://aws.amazon.com/lambda/) functions, is billed by the second, and is fully managed by [AWS](https://aws.amazon.com/), therefore, we cannot think of many reasons *not* to use it.

## Will I get billed for EMR when executing read queries?
No. You will only be billed for [AWS Lambda](https://aws.amazon.com/lambda/) functions, thanks to the fact that the [Iceberg Java API](https://iceberg.apache.org/docs/latest/api/) used for [table scans](https://iceberg.apache.org/docs/latest/api/#scanning) is packaged as a standalone [AWS Lambda](https://aws.amazon.com/lambda/) function running without [Apache Spark](https://spark.apache.org/). This ensures the lowest possible costs, and a much lower table scan marginal latency under 500 ms. You will get billed for EMR only when doing write queries on [Iceberg tables](https://iceberg.apache.org/spec/). Finally, queries made directly on Object Store objects do not use [Apache Iceberg](https://iceberg.apache.org/), hence do not require [Apache Spark](https://spark.apache.org/).

## Which cloud platforms will be supported?
Initially, [AWS](https://aws.amazon.com/). Support for [Microsoft Azure](https://azure.microsoft.com/en-us) and [Google Cloud](https://cloud.google.com/) will be added in future releases.

## Which lakehouse platforms will be supported?
Initially, [Apache Iceberg](https://iceberg.apache.org/). Support for [Apache Hudi](https://hudi.apache.org/) and [Delta Lake](https://delta.io/) will be added in future releases.

## Why support so many deployment options?
So that you can pick the one that will work best for you:
- **Hard**: [Rust](https://www.rust-lang.org/) module deeply integrated within your own tool or application
- **Easy**:[AWS Lambda](https://aws.amazon.com/lambda/) deployed within your own cloud platform
- **Easier**: [AWS CloudFormation](https://aws.amazon.com/cloudformation/) template deployed within your own [VPC](https://aws.amazon.com/vpc/)
- **Easiest**: [AWS Marketplace](https://aws.amazon.com/marketplace) product added to your own cloud environment

## Can PuffinDB be deployed on EC2 instances or Fargates?
Initially, PuffinDB will be deployed on [AWS Lambda](https://aws.amazon.com/lambda/) functions, but support for [Amazon EC2](https://aws.amazon.com/ec2/) and [AWS Fargate](https://aws.amazon.com/fargate/) will be added soon after.

## How is PuffinDB serverless if it is deployed on EC2 instances?
Fair question. Whenever PuffinDB is deployed on [Amazon EC2](https://aws.amazon.com/ec2/) instances, it still uses fleets of [AWS Lambda](https://aws.amazon.com/lambda/) for loading objects from the Object Store and for processing some parts of distributed queries, while using the EC2 instances for [reduction](https://en.wikipedia.org/wiki/Reduction_operator) purposes. The goal here is to be as serverless as possible, while remaining pragmatic (serverless Fargates remain much less powerful than the largest EC2 instances). Eventually, we expect Fargates to become more and more powerful, so much so that we do not need to rely on EC2 instances for the vast majority of applications and workloads.

## Why use Rust?
Because [developers love it](https://insights.stackoverflow.com/survey/2021#technology-most-loved-dreaded-and-wanted).

## Do I need a specific client to use PuffinDB?
No, all you need is the [AWS SDK](https://aws.amazon.com/developer/tools/) for the programming language of your choice:
- C++
- Go
- Java
- JavaScript
- Kotlin
- .NET
- Node.js
- PHP
- Python
- Ruby
- Rust
- Swift

## Why use a Redis cluster for storing query logs?
[Query logs](docs/Logs.md) must be accessed with very low latency for looking up the Object Store URI where an earlier query result might be cached. [Amazon ElastiCache for Redis](https://aws.amazon.com/elasticache/redis/) provides submillisecond latency for such queries, at a very reasonable cost. Furthermore, Redis can also be used as a low-latency broker for queuing and synchronization, which is required for the execution of distributed queries.

## How are read queries on Lakehouse tables executed?
1. Parse SQL query using native [DuckDB](https://duckdb.org/) parser and outline table filter predicates.
2. Scan table(s) using low-latency [Lakehouse Catalog API](https://iceberg.apache.org/docs/latest/api/#scanning) running on [AWS Lambda](https://aws.amazon.com/lambda/) function.
3. Replace reference(s) to table(s) with references to partitions returned by table scan(s).
4. Stringify SQL query using native [DuckDB](https://duckdb.org/) stringifier.
5. Execute query using [DuckDB](https://duckdb.org/) running on [AWS Lambda](https://aws.amazon.com/lambda/) function.

## How does partition caching work?
Whenever you execute a query on a table (be it backed by objects on the Object Store or a table in the Lakehouse) and are planning to make multiple queries on the same subset of the table, its partitions can be cached within the memory of the [AWS Lambda](https://aws.amazon.com/lambda/) function(s) used to execute the query. During subsequent queries on the same partitions, the function has a 99% probability of enjoying a hot start, giving it instant access to previously-cached partitions. This helps reduce latency and cost, since downloading partitions from the Object Store to the function is usually the longest step in the end-to-end query process (at least for relatively simple queries). The only drawback of such an optimization is that it slows the initial query down, because it requires an extra copy of the data in memory, but this is a small price to pay if many queries are to be made on the same table partitions.

## How does query result caching work?
Whenever you execute a read query, its result can be cached on the Object Store ([Amazon S3](https://aws.amazon.com/s3/)), and this cache is automatically referenced in the query logs table. Recent entries in this table are cached in the main [AWS Lambda](https://aws.amazon.com/lambda/) function for fast lookup. Whenever the same query is requested again, its cached result can be returned instead of re-executing the query. This helps reduce latency and cost, while freeing limited resources (most [AWS](https://aws.amazon.com/) accounts are limited to 3,000 concurrent [Lambda](https://aws.amazon.com/lambda/) functions) for other queries. Cached query results can be served through a Content Delivery Network ([Amazon CloudFront](https://aws.amazon.com/cloudfront/)) using [Object Store Origins](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/DownloadDistS3AndCustomOrigins.html#using-s3-as-origin) to further reduce latency and remove any potential bottleneck on the Object Store.

## Why is STOIC initiating and funding this open source project?
[STOIC](https://stoic.com/) is in the business of developing and selling a progressive data platform allowing any data citizen to interact with datasets of any size, from kilobytes to petabytes. In that context, we need to perform read | write SQL queries on large datasets, with low latency (2s or less) and low cost (one or two orders of magnitude lower than conventional solutions). And we need the required query engine to properly support the top-three cloud platforms and the top-three table formats. Furthermore, we want it powered by [Arrow](https://arrow.apache.org/) and [DuckDB](https://duckdb.org/), because there are no better technologies available today. We could not find such an engine distributed under a liberal open source license, so we decided to build one. And because this component is a means to an end for us, yet could benefit countless other projects and organizations, we decided to develop it as an open source project.

## What is STOIC?
[STOIC](https://stoic.com/) is a progressive data platform allowing any data citizen to interact with [datasets of any size](https://github.com/stoic-doc/Community/discussions/905), through an intuitive spreadsheet-like user experience. STOIC is built on top of [PuffinDB](http://PuffinDB.io), with a modern [cloud-native architecture](https://github.com/stoic-doc/Community/discussions/908) and a unique [hierarchical caching stack](https://github.com/stoic-doc/Community/discussions/907). Its user interface is designed for a very wide range of [personas](https://github.com/stoic-doc/Community/discussions/752), including business analysts, data architects, and software engineers. It combines a [table editor](https://github.com/stoic-doc/Community/discussions/534), [sheet editor](https://github.com/stoic-doc/Community/discussions/535), and [notebook editor](https://github.com/stoic-doc/Community/discussions/536) into a fully integrated and coherent user experience. It supports [data pipelines](https://github.com/stoic-doc/Community/discussions/904), [data editing](https://github.com/stoic-doc/Community/discussions/903), [workflow automation](https://github.com/stoic-doc/Community/discussions/796), and [insight-driven data visualization](https://github.com/stoic-doc/Community/discussions/469).

## Will STOIC offer hosting services for PuffinDB?
Not anytime soon. Instead, you will use your [Amazon VPC](https://aws.amazon.com/vpc/) and add PuffinDB from the [AWS Marketplace](https://aws.amazon.com/marketplace), for free. But others might.

## When will PuffinDB be released?
We do not know yet, but here is our [prioritized roadmap](ROADMAP.md).

## How may I contribute?
Please send an email to info at stoic dot com.
