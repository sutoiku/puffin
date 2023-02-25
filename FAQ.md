# Frequently Asked Questions

Please ask new questions as a `Q&A` in [discussions](https://github.com/sutoiku/puffin/discussions).

## What is PuffinDB?
PuffinDB is a serverless [HTAP](https://en.wikipedia.org/wiki/Hybrid_transactional/analytical_processing) data mesh platform powered by [Arrow](https://arrow.apache.org/) × [DuckDB](https://duckdb.org/) × [Iceberg](https://iceberg.apache.org/).

## Why should I use PuffinDB?
If you are using DuckDB client-side with [any client application](docs/Clientless.md), adding the [PuffinDB extension](docs/Extension.md) will let you:
- Distribute queries across thousands of serverless functions and a [Monostore](docs/Monostore.md)
- Read from and write to hundreds of applications using any [Airbyte connector](https://airbyte.com/connectors)
- Collaborate on the same [Iceberg tables](https://iceberg.apache.org/spec/) with other users
- Write back to an Iceberg table with [ACID](https://en.wikipedia.org/wiki/ACID) transactional integrity
- Execute [cross-database joins](docs/Query%20Proxy.md#query-delegation) (*Cf.* [Edge-Driven Data Integration](EDDI.md))
- Translate between 19 [SQL dialects](docs/Query%20Proxy.md#dialect-translation)
- Invoke [remote query generators](docs/Query%20Proxy.md)
- Invoke [curl](https://curl.se/) commands
- Accelerate and | or schedule the downloading of large tables to your client
- Cache tables and run computations at the edge ([Amazon CloudFront](https://aws.amazon.com/cloudfront/) × [Lambda@Edge](https://aws.amazon.com/lambda/edge/))
- Log queries on your data lake

## Why develop yet another distributed SQL engine?
There are many excellent distributed SQL engines currently available on the market. Why do we need yet another one?

- [True serverless architecture](../RATIONALE.md/#true-serverless-architecture)
- [Future-proof architecture](../RATIONALE.md/#future-proof-architecture)
- [Designed for VPC deployment](../RATIONALE.md/#designed-for-vpc-deployment)
- [Designed for real-time analytics](../RATIONALE.md/#designed-for-real-time-analytics)
- [Designed for interactive analytics](../RATIONALE.md/#designed-for-interactive-analytics)
- [Designed for transformation and analytics](../RATIONALE.md/#designed-for-transformation-and-analytics)
- [Designed for analytics and transactions](../RATIONALE.md/#designed-for-analytics-and-transactions)
- [Designed for next-generation query engine](../RATIONALE.md/#designed-for-next-generation-query-engine)
- [Designed for next-generation file formats](../RATIONALE.md/#designed-for-next-generation-file-formats)
- [Designed for data lakes](../RATIONALE.md/#designed-for-data-lakes)
- [Designed for data mesh integration](../RATIONALE.md/#designed-for-data-mesh-integration)
- [Designed for all users](../RATIONALE.md/#designed-for-all-users)
- [Designed for extensibility](../RATIONALE.md/#designed-for-extensibility)
- [Designed for embedability](../RATIONALE.md/#designed-for-embedability)
- [Optimized for machine-generated queries](../RATIONALE.md/#optimized-for-machine-generated-queries)
- [Scalable through large user bases](../RATIONALE.md/#scalable-through-large-user-bases)

## What is Cloud Data?
[Cloud Data](CLOUD.md) is what comes after Big Data.

## What is Edge-Driven Data Integration?
[Edge-Driven Data Integration](EDDI.md) (EDDI) is an inversion of control proposed by the PuffinDB project. Its main idea is that data integration should be driven at the edge by dynamic user-driven integration scenarios, rather than on the cloud with static data integration pipelines, yet without sacrificing solid architecture design and proper data governance ([read more](EDDI.md)).

## Why should I run DuckDB on the cloud instead of my personal computer?
While running [DuckDB](https://duckdb.org/) on your personal computer will work great in many instances, running it on the cloud can bring several benefits:
- No need to download large datasets from the cloud to your local computer.
- Ability to work with larger datasets by taking advantage of fleets of [AWS Lambda](https://aws.amazon.com/lambda/) functions and | or large [Amazon EC2](https://aws.amazon.com/ec2/) instances.
- Enforcement of column and | or row-level access control policies.

## Why should I run DuckDB both client-side and cloud-side?
- Low-latency analytics on whatever amount of data you are willing to cache on your client.
- Ability to handle datasets that are too large for your client.
- Concurrent users editing the same cloud-side table.

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

**Credits**: feature descriptions courtesy of [Apache Iceberg](https://iceberg.apache.org/).

## Won't DuckDB support Iceberg soon?
Yes, it will (or so we've [read](https://twitter.com/tabulario/status/1616467434772533250)). Nevertheless, this support is likely to be limited to [DuckDB](https://duckdb.org/) running on a single host (client, function, or VM). PuffinDB is designed to support DuckDB running on multiple functions in parallel, or multiple functions and a VM coordinating their work. Consequently, while DuckDB can directly request a [table scan](https://iceberg.apache.org/docs/latest/api/#scanning) through [Iceberg](https://iceberg.apache.org/)'s [Java API](https://iceberg.apache.org/docs/latest/api/) when running in single-host mode, this scan must be performed outside of DuckDB when running in multi-host mode. Furthermore, PuffinDB supports the optional execution of some read queries on [Amazon Athena](https://aws.amazon.com/athena/), whenever such delegated execution would be faster or more cost-effective.

## Why not just use Amazon Athena?
[Amazon Athena](https://aws.amazon.com/athena/) can be used to read | write any [Iceberg tables](https://iceberg.apache.org/spec/), but many read queries will be faster and more cost-effective with [DuckDB](https://duckdb.org/).

## Do I need Amazon Athena to query Iceberg tables with PuffinDB?
If you just make read queries on [Iceberg tables](https://iceberg.apache.org/spec/), you do not. But you will need [Amazon Athena](https://aws.amazon.com/athena/) for write queries on these tables.

Alternatively, you will be able to use [Icecap](docs/Icecap.md) once it becomes available.

## Will I get billed for Amazon Athena when executing read queries?
No. You will only be billed for [AWS Lambda](https://aws.amazon.com/lambda/) functions, thanks to the fact that the [Iceberg Java API](https://iceberg.apache.org/docs/latest/api/) used for [table scans](https://iceberg.apache.org/docs/latest/api/#scanning) is packaged as a standalone [AWS Lambda](https://aws.amazon.com/lambda/) function running without [Apache Spark](https://spark.apache.org/). This ensures the lowest possible costs, and a much lower table scan marginal latency under 500 ms. You will get billed for [Amazon Athena](https://aws.amazon.com/athena/) only when doing write queries on [Iceberg tables](https://iceberg.apache.org/spec/). Finally, write queries made directly on Object Store objects do not use [Apache Iceberg](https://iceberg.apache.org/), hence do not require [Amazon Athena](https://aws.amazon.com/athena/).

## Which cloud platforms will be supported?
Initially, [AWS](https://aws.amazon.com/). Support for [Microsoft Azure](https://azure.microsoft.com/en-us) and [Google Cloud](https://cloud.google.com/) will be added in future releases.

## Which Lakehouse platforms will be supported?
[Apache Iceberg](https://iceberg.apache.org/), [Apache Hudi](https://hudi.apache.org/), and [Delta Lake](https://delta.io/).

## Why support so many deployment options?
So that you can pick the one that will work best for you:
- **Hard**: [Bun](https://bun.sh/) and [Python](https://www.python.org/) modules deeply integrated within your own tool or application
- **Easy**: [AWS Lambda functions](functions/) deployed within your own cloud platform
- **Easier**: [AWS CloudFormation](https://aws.amazon.com/cloudformation/) template deployed within your own [VPC](https://aws.amazon.com/vpc/)
- **Easiest**: [AWS Marketplace](https://aws.amazon.com/marketplace) product added to your own cloud environment

## Can PuffinDB be deployed on EC2 instances or Fargates?
PuffinDB is deployed on [AWS Lambda](https://aws.amazon.com/lambda/) functions and one [Amazon EC2](https://aws.amazon.com/ec2/) instance. Support for [AWS Fargate](https://aws.amazon.com/fargate/) will be added later on.

## How is PuffinDB serverless if it is deployed on EC2 instances?
Fair question. Whenever PuffinDB is deployed on [Amazon EC2](https://aws.amazon.com/ec2/) instances, it still uses fleets of [AWS Lambda](https://aws.amazon.com/lambda/) for loading objects from the Object Store and for processing some parts of distributed queries, while using the EC2 instances for [reduction](https://en.wikipedia.org/wiki/Reduction_operator) purposes. The goal here is to be as serverless as possible, while remaining pragmatic (serverless Fargates remain much less powerful than the largest EC2 instances). Eventually, we expect Fargates to become more and more powerful, so much so that we will not need to rely on EC2 instances anymore for the vast majority of applications and workloads.

## Do I need a specific client to use PuffinDB?
No. PuffinDB has a [clientless](docs/Clientless.md) architecture and can be used from any application embedding the [DuckDB](https://duckdb.org/) engine, using a simple [extension](docs/Extension.md).

## Why use a Redis cluster for distributed query orchestration?
Distributed query orchestration requires low latency and high throughput. A large [Amazon ElastiCache for Redis](https://aws.amazon.com/elasticache/redis/) cluster can provide:
- Submillisecond latency
- Tens of millions of transactions per second
- Up to 340 TB of in-memory storage

## Why use a Redis cluster for storing query logs?
[Query logs](docs/Logs.md) must be accessed with very low latency for looking up the Object Store URI where an earlier query result might be cached. [Amazon ElastiCache for Redis](https://aws.amazon.com/elasticache/redis/) provides submillisecond latency for such queries, at a very reasonable cost. Furthermore, Redis can also be used as a low-latency broker for queuing and synchronization, which is required for the execution of distributed queries.

## What about Dragonfly or KeyDB?
[Dragonfly](https://dragonflydb.io/) and [KeyDB](https://docs.keydb.dev/) are promising alternatives to [Redis](https://redis.io/). All three use the same API and should be supported equally well.

## Why embed PRQL and Malloy?
PuffinDB embeds a [PRQL](https://prql-lang.org/) to SQL translator and a [Malloy](https://www.malloydata.dev/) to SQL translator. Many such translators are available today, for many different applications and syntaxes, but PuffinDB selected these two, because they address two complementary needs: PRQL is great for data preparation, while Malloy is unparalleled for data analytics. Both have been embedded to stress the platform in different ways, and to provide ready-to-use "applications" that can be used to convincingly showcase the platform's capabilities. Nevertheless, they are packaged as optional modules that can be omitted if so desired. Later on, they will be dynamically injected using the [Query Proxy](docs/Query%20Proxy.md).

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

## Where can I learn more about SQL query engines?
These [reference materials](docs/References.md) are a solid starting point.

## Why is STOIC initiating and funding this open source project?
[STOIC](https://stoic.com/) is in the business of developing and selling a progressive data platform allowing any data citizen to interact with datasets of any size, from kilobytes to petabytes. In that context, we need to perform read | write SQL queries on large datasets, with low latency (2s or less) and low cost (one or two orders of magnitude lower than conventional solutions). And we need the required query engine to properly support the top-three cloud platforms and the top-three table formats. Furthermore, we want it powered by [Arrow](https://arrow.apache.org/) and [DuckDB](https://duckdb.org/), because there are no better technologies available today. We could not find such an engine distributed under a liberal open source license, so we decided to build one. And because this component is a means to an end for us, yet could benefit countless other projects and organizations, we decided to develop it as an open source project.

## What is STOIC?
[STOIC](https://stoic.com/) is a progressive data platform allowing any data citizen to interact with [datasets of any size](https://github.com/stoic-doc/Community/discussions/905), through an intuitive spreadsheet-like user experience. STOIC is built on top of [PuffinDB](http://PuffinDB.io), with a modern [cloud-native architecture](https://github.com/stoic-doc/Community/discussions/908) and a unique [hierarchical caching stack](https://github.com/stoic-doc/Community/discussions/907). Its user interface is designed for a very wide range of [personas](https://github.com/stoic-doc/Community/discussions/752), including business analysts, data architects, and software engineers. It combines a [table editor](https://github.com/stoic-doc/Community/discussions/534), [sheet editor](https://github.com/stoic-doc/Community/discussions/535), and [notebook editor](https://github.com/stoic-doc/Community/discussions/536) into a fully integrated and coherent user experience. It supports [data pipelines](https://github.com/stoic-doc/Community/discussions/904), [data editing](https://github.com/stoic-doc/Community/discussions/903), [workflow automation](https://github.com/stoic-doc/Community/discussions/796), and [insight-driven data visualization](https://github.com/stoic-doc/Community/discussions/469). For more information, please check this [presentation](https://www.linkedin.com/feed/update/urn:li:activity:7032716700101873664/).

## Will STOIC offer hosting services for PuffinDB?
Not anytime soon. Instead, you can use your [Amazon VPC](https://aws.amazon.com/vpc/) and add PuffinDB from the [AWS Marketplace](https://aws.amazon.com/marketplace), for free.

## Why should my organization become a sponsor?
Vendors and users alike should consider sponsoring the PuffinDB project, for several reasons outlined on our [sponsors](SPONSORS.md) page.

## When will PuffinDB be released?
A first version with a naive [distributed query engine](docs/Query%20Engine.md) (filter pushdown to serverless functions) should be released in Q2 (*Cf.* [roadmap](ROADMAP.md)).

## How may I contribute?
Please send an email to info at puffindb dot io.
