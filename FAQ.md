# Frequently Asked Questions

Please ask unanswered questions by creating a [new issue](https://github.com/sutoiku/pafin/issues).

## What is Pafin?
Patfin is a serverless lakehouse query engine powered by [Iceberg](https://iceberg.apache.org/) × [DuckDB](https://duckdb.org/) × [Arrow](https://arrow.apache.org/).

## What does *Pafin* mean?
*Pafin* (not to be confused with [Puffin](https://iceberg.apache.org/puffin-spec/)) is the Japanese [romanization](https://en.wikipedia.org/wiki/Romanization) of [*puffin*](https://en.wikipedia.org/wiki/Puffin), much like [*sutoiku*](https://github.com/sutoiku) is to [*stoic*](https://stoic.com/).

## Why should I use Pafin?
Pafin's purpose is to make it easier to run [DuckDB](https://duckdb.org/) on a serverless function ([AWS Lambda](https://aws.amazon.com/lambda/), [Azure Function](https://learn.microsoft.com/en-us/azure/azure-functions/functions-overview), [Google Cloud Function](https://cloud.google.com/functions)) for executing read | write queries against objects managed by an Object Store ([Amazon S3](https://aws.amazon.com/s3/), [Azure Blob Storage](https://azure.microsoft.com/en-us/products/storage/blobs), [Google Cloud Storage](https://cloud.google.com/storage)) and tables managed by a Lakehouse ([Apache Iceberg](https://iceberg.apache.org/), [Apache Hudi](https://hudi.apache.org/), [Delta Lake](https://delta.io/)).

## Why should I run DuckDB on the cloud instead of my personal computer?
While running DuckDB on your personal computer will work great in some instances, running it on the cloud can bring many benefits:
- No need to download large datasets from the cloud to your local computer.
- Ability to work with larger datasets by taking advantage of fleets of [AWS Lambda](https://aws.amazon.com/lambda/) functions and | or large [Amazon EC2](https://aws.amazon.com/ec2/) instances.
- Enforcement of column and | or row-level access control policies.

## Can I use Pafin without a Lakehouse?
Yes, you can use Pafin with just an Object Store like [Amazon S3](https://aws.amazon.com/s3/).

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

## Do I need a specific client to use Pafin?
No, all you need is an HTTP client, which you should be able to find for virtually any platform.

## Will you offer hosting services for Pafin?
We will not. Instead, you can use your own [Amazon VPC](https://aws.amazon.com/vpc/) and add Pufin from the [AWS Marketplace](https://aws.amazon.com/marketplace), for free. Others might though.

## When will Pafin be released?
We do not know yet, but a tentative roadmap will be released before the end of January 2023.

## How may I contribute?
Please send an email to info at stoic dot com.
