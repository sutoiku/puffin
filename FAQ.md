# Frequently Asked Questions

Please ask unanswered questions by creating a [new issue](https://github.com/sutoiku/pafin/issues).

## What is Pafin?
Patfin is a serverless lakehouse query engine powered by [Iceberg](https://iceberg.apache.org/) × [DuckDB](https://duckdb.org/) × [Arrow](https://arrow.apache.org/).

## What does *Pafin* mean?
*Pafin* (not to be confused with [Puffin](https://iceberg.apache.org/puffin-spec/)) is the Japanese [romanization](https://en.wikipedia.org/wiki/Romanization) (パフィン) of [*puffin*](https://en.wikipedia.org/wiki/Puffin), much like [*sutoikku*](https://github.com/sutoiku) (ストイック) is to [*stoic*](https://stoic.com/).

## Why should I use Pafin?
Pafin's purpose is to make it easier to run [DuckDB](https://duckdb.org/) on a serverless function ([AWS Lambda](https://aws.amazon.com/lambda/), [Azure Function](https://learn.microsoft.com/en-us/azure/azure-functions/functions-overview), [Google Cloud Function](https://cloud.google.com/functions)) for executing read | write queries against objects managed by an Object Store ([Amazon S3](https://aws.amazon.com/s3/), [Azure Blob Storage](https://azure.microsoft.com/en-us/products/storage/blobs), [Google Cloud Storage](https://cloud.google.com/storage)) and tables managed by a Lakehouse ([Apache Iceberg](https://iceberg.apache.org/), [Apache Hudi](https://hudi.apache.org/), [Delta Lake](https://delta.io/)).

## Why should I run DuckDB on the cloud instead of my personal computer?
While running DuckDB on your personal computer will work great in some instances, running it on the cloud can bring many benefits:
- No need to download large datasets from the cloud to your local computer.
- Ability to work with larger datasets by taking advantage of fleets of [AWS Lambda](https://aws.amazon.com/lambda/) functions and | or large [Amazon EC2](https://aws.amazon.com/ec2/) instances.
- Enforcement of column and | or row-level access control policies.

## Can I use Pafin without a Lakehouse?
Yes, you can use Pafin with just an Object Store like [Amazon S3](https://aws.amazon.com/s3/). But you should still take a look at [Iceberg](https://iceberg.apache.org/), for the following reasons:

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

Note: feature deacriptions courtesy of [Apache Iceberg](https://iceberg.apache.org/)

## Won't DuckDB support Iceberg soon?
Yes, it will (or so we've [read](https://twitter.com/tabulario/status/1616467434772533250)). Nevertheless, this support is likely to be limited to [DuckDB](https://duckdb.org/) running on a single host (client, function, or VM). Pafin is designed to support DuckDB running on multiple functions in parallel, or multiple functions and a VM coordinating their work. Consequently, while DuckDB can directly request a [table scan](https://iceberg.apache.org/docs/latest/api/#scanning) through [Iceberg](https://iceberg.apache.org/)'s [Java API](https://iceberg.apache.org/docs/latest/api/) when running in single-host mode, this scan must be performed outside of DuckDB when running in multi-host mode. Furthermore, Pafin supports the optional execution of some read queries on [Spark SQL](), whenever such delegated execution would be faster or more cost-effective.

## Why not just use Spark SQL?
[Spark SQL](https://spark.apache.org/sql/) can be use to read | write any [Iceberg tables](https://iceberg.apache.org/docs/latest/configuration/), but many read queries will be faster and more cost-effective when using [DuckDB](https://duckdb.org/).

## Do I need Amazon EMR to query Iceberg tables with Pafin?
If you just make read queries on [Iceberg tables](https://iceberg.apache.org/docs/latest/configuration/), you do not. But you will need [Amazon EMR](https://aws.amazon.com/emr/) (EKS or Serverless), or any suitable alternative [Apache Spark](https://spark.apache.org/) deployment running [Apache Iceberg](https://iceberg.apache.org/) if you want to make write queries on these tables. Thankfully, [Amazon EMR Serverless](https://aws.amazon.com/emr/serverless/) comes pre-configured with the default [AWS CloudFormation](https://aws.amazon.com/cloudformation/) template that is part of Pufin. Furthermore, [Amazon EMR Serverless](https://aws.amazon.com/emr/serverless/) is only 30% more expensive than running barebone [AWS Lambda](https://aws.amazon.com/lambda/) functions, is billed by the second, and is fully managed by [AWS](https://aws.amazon.com/), therefore, we cannot think of many reasons *not* to use it.

## Will I get billed for EMR when executing read queries?
No. You will only be billed for [AWS Lambda](https://aws.amazon.com/lambda/) functions, thanks to the fact that the [Iceberg Java API](https://iceberg.apache.org/docs/latest/api/) used for [table scans](https://iceberg.apache.org/docs/latest/api/#scanning) is packaged as a standalone [AWS Lambda](https://aws.amazon.com/lambda/) function running without [Apache Spark](https://spark.apache.org/). This ensures the lowest possible costs, and a much lower table scan marginal latency under 500 ms. You will get billed for EMR only when doing write queries on [Iceberg tables](https://iceberg.apache.org/docs/latest/configuration/). Finally, queries made directly on Object Store objects do not use [Apache Iceberg](https://iceberg.apache.org/), hence do not require [Apache Spark](https://spark.apache.org/).

## Which cloud platforms will be supported?
Initially, [AWS](https://aws.amazon.com/). Support for [Microsoft Azure](https://azure.microsoft.com/en-us) and [Google Cloud](https://cloud.google.com/) will be added in future releases.

## Which lakehouse platforms will be supported?
Initially, [Apache Iceberg](https://iceberg.apache.org/). Support for [Apache Hudi](https://hudi.apache.org/) and [Delta Lake](https://delta.io/) will be added in future releases.

## Why use Node.js?
Why not? [Node.js](https://nodejs.org/en/) is a great development platform for this type of project, and we're familiar with it. But it should not matter to users.

## Do I need a specific client to use Pafin?
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

## How does query result caching work?
Whenever you execute a read query, its result can be cached on the Object Store ([Amazon S3](https://aws.amazon.com/s3/)), and this cache is automatically referenced in the query logs table. Recent entries in this table are cached in the main [AWS Lambda](https://aws.amazon.com/lambda/) function for fast lookup. Whenever the same query is requested again, its cached result can be returned instead of re-executing the query. This helps reduce latency and cost, while freeing limited resources (most [AWS](https://aws.amazon.com/) accounts are limited to 3,000 concurrent [Lambda](https://aws.amazon.com/lambda/) functions) for other queries.

## Will you offer hosting services for Pafin?
Not anytime soon. Instead, you can use your [Amazon VPC](https://aws.amazon.com/vpc/) and add Pufin from the [AWS Marketplace](https://aws.amazon.com/marketplace), for free. Others are encouraged to though.

## Why is STOIC initiating and funding this open source project?
[STOIC](https://stoic.com/) is in the business of developing and selling a progressive data platform allowing any data citizen to interact with datasets of any size, from kilobytes to petabytes. In that context, we need to perform read | write SQL queries on large datasets, with low latency (2s or less) and low cost (one or two orders of magnitude more cost-effective than conventional solutions). And we need the required query engine to support the top-three cloud platforms and the top-three table formats. Furthermore, we want it powered by [DuckDB](https://duckdb.org/) and [Arrow](https://arrow.apache.org/), because there are no better technologies available today. We could not find such an engine available under a liberal open source license, so we decided to build one. But because this engine is a means to an end for us, we decided to develop it as an open source project.

## When will Pafin be released?
We do not know yet, but a tentative roadmap will be released soon.

## How may I contribute?
Please send an email to info at pafin dot org.
