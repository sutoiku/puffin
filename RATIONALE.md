# Rationale for a new distributed SQL engine

There are many excellent distributed SQL engines available today. Why do we need yet another one?

- [True serverless architecture](#true-serverless-architecture)
- [Future-proof architecture](#future-proof-architecture)
- [Designed for VPC deployment](#designed-for-vpc-deployment)
- [Designed for small to large datasets](#designed-for-small-to-large-datasets)
- [Designed for real-time analytics](#designed-for-real-time-analytics)
- [Designed for interactive analytics](#designed-for-interactive-analytics)
- [Designed for transformation and analytics](#designed-for-transformation-and-analytics)
- [Designed for analytics and transactions](#designed-for-analytics-and-transactions)
- [Designed for next-generation query engine](#designed-for-next-generation-query-engine)
- [Designed for next-generation file formats](#designed-for-next-generation-file-formats)
- [Designed for data lakes](#designed-for-data-lakes)
- [Designed for data mesh integration](#designed-for-data-mesh-integration)
- [Designed for all users](#designed-for-all-users)
- [Designed for extensibility](#designed-for-extensibility)
- [Designed for embedability](#designed-for-embedability)
- [Optimized for machine-generated queries](#optimized-for-machine-generated-queries)
- [Scalable through large user bases](#scalable-through-large-user-bases)

## True serverless architecture
Most distributed SQL engines are designed to be deployed on conventional containers (*e.g.* [Amazon EC2](https://aws.amazon.com/ec2/) instances). PuffinDB is designed to run on serverless functions (*e.g.* [AWS Lambda](https://aws.amazon.com/lambda/) functions). The largest currently-available [Amazon EC2](https://aws.amazon.com/ec2/) instance ([`u-24tb1.112xlarge`](https://aws.amazon.com/ec2/instance-types/high-memory/)) has 448 vCPUs, 24 TB of RAM, and 100 Gbps of network bandwidth, and its on-demand availability is never guaranteed. In comparison, 10,000 [AWS Lambda](https://aws.amazon.com/lambda/) functions offer an aggregated 60,000 vCPUs (134×), 200 TB of RAM (8×), and 8 Tbps of network bandwidth (80×). Furthermore, EC2 instances are billed from instantiation to termination (usually several hours at a time), while Lambda functions are billed by the millisecond, and only for the time during which they are actually used.

**Benefits**:
- Lower query processing times
- Lower costs
- Higher availability
- Higher elasticity

## Future-proof architecture
Most distributed SQL engines have been designed to be deployed on conventional data centers, and the few that were natively designed for public cloud deployment were inspired by decade-old cloud architectures. PuffinDB is designed for contemporary public clouds, and anticipates [future developments](docs/Future-Proofing.md) that are considered likely to happen within the next 3 to 5 years.

**Benefits**:
- Lower query processing times
- Lower costs
- Higher availability
- Higher elasticity

## Designed for VPC deployment
Most distributed SQL engines available on public clouds are designed for multi-tenancy and operated by their vendors. PuffinDB is designed for single-tenancy (with support for multiple teams) and deployed on the customer's Virtual Private Cloud (VPC). This is made possible thanks to a true [serverless architecture](docs/Architecture.md), and an unprecedented level of automation for provisioning, monitoring, and maintenance.

**Benefits**:
- Better security
- Better data confidentiality
- Better integration with existing cloud assets
- More predictable Quality of Service

## Designed for small to large datasets
Most SQL engines are designed for small datasets and optimize for usability, or for large datasets and require advanced technical skills. PuffinDB is designed for [Cloud Data](CLOUD.md) and can scale from kilobytes to petabytes in a very progressive manner, without sacrificing usability.

**Benefits**:
- Broader userbase accessibility
- Lower total cost of operations

## Designed for real-time analytics
Most OLAP engines are designed to support analytics workloads on datasets that are updated at relatively low frequency (daily or longer). PuffinDB is designed for real-time analytics with much higher update frequencies (sub-second). This dramatically broadens the range of use cases for which the engine can be used, while increasing the value of insights it gives access to.

**Benefits**:
- Broader use case applicability
- Higher business value

## Designed for interactive analytics
Most OLAP engines are designed for queries that can take minutes or even hours to complete. While PuffinDB is capable of handling such long-running queries, it is optimized for interactive analytics workloads, with queries completing within seconds. This is made possible thanks to a true [serverless architecture](docs/Architecture.md) offering two to three orders of magnitude more bandwidth and compute power. Going from minutes or hours to seconds increases user satisfaction and allows data analysts to run more queries or more complex queries, on larger datasets.

**Benefits**:
- Increased user satisfaction
- Higher business value

## Designed for transformation and analytics
Most OLAP engines and cloud data warehousing platforms are designed to be used alongside third-party data preparation tools for ETL and reverse ETL. PuffinDB is in and by itself a powerful data extraction, transformation, and loading tool, thanks to its extensive collection of database and application [connectors](docs/Connectors.md), and its built-in data pipeline execution engine.

**Benefits**:
- Lower development costs
- Lower licensing costs
- Faster development cycles
- Lower data update latency
- Better data quality
- Enhanced data governance

## Designed for analytics and transactions
By definition, OLAP (online analytical processing) engines are designed to work on immutable data, and are commonly used downstream of OLTP (online transaction processing) systems. While PuffinDB is not designed to be used as primary system of records for high-frequency transactional applications, it is designed to support real-time updates and manual edits (adjustments) on data. As such, it can be considered an analytics-oriented HTAP (hybrid transaction/analytical processing) engine.

**Benefits**:
- Broader use case applicability
- Lower data update latency
- Lower data integration costs

## Designed for next-generation query engine
Most distributed SQL engines are built using proprietary query engines, or open source tabular query engines like [MySQL](https://www.mysql.com/) or [PostgreSQL](https://www.postgresql.org/). PuffinDB is designed around the next-generation [DuckDB](https://duckdb.org/) in-process columnar query engine, which offers a truly unique set of features:
- In process, serverless
- Columnar and vectorized
- In memory and out-of-core
- Multithreaded
- Multi-version concurrency control (MVCC)
- Support for local and remote persistence
- Support for Arrow and Parquet
- Small footprint and no dependencies

**Benefits**:
- Lower development costs for vendor
- Richer feature set for users

## Designed for next-generation file formats
Most SQL engines are designed for traditional file formats like CSV or Parquet. PuffinDB is designed to take advantage of next-generation file formats like [Puffin](https://iceberg.apache.org/puffin-spec/) (not to be confused with PuffinDB — advanced file metadata), [Lance](https://github.com/eto-ai/lance) (high-performance random access), and the upcoming native DuckDB file format (updates in place).

**Benefits**:
- Lower query processing times and costs through enhanced query planning
- Higher performance for random access
- Lower data update latency

## Designed for data lakes
Most SQL engines targeted at public cloud deployment have been designed for Object Stores only (*e.g.* [Amazon S3](https://aws.amazon.com/s3/)). Instead, PuffinDB has been natively designed to take advantage of Data Lakes such as [Apache Iceberg](https://iceberg.apache.org/), [Apache Hudi](https://hudi.apache.org/), and [Delta Lake](https://delta.io/). This is one of the critical design elements that give it full OLTP capabilities with ACID properties, while allowing it to connect to a wide range of data sources that are being migrated to data lakes at an accelerating pace.

**Benefits**:
- Enhanced transactional integrity
- Lower data integration costs

## Designed for data mesh integration
Most SQL engines are designed as standalone data silos. Instead, PuffinDB embraces the [data mesh philosophy](https://martinfowler.com/articles/data-mesh-principles.html), and is natively designed to distribute SQL queries across heterogeneous and remote databases and applications, thanks to its extensive collection of database and application [connectors](docs/Connectors.md), and its built-in data pipeline execution engine.

**Benefits**:
- Lower data integration costs
- Faster development cycles
- Lower data update latency
- Better data quality
- Enhanced data governance

## Designed for all users
Many SQL engines require advanced technical skills for deployment, monitoring, and maintenance. PuffinDB takes a radically-different approach, allowing direct deployment from any [DuckDB](https://duckdb.org/) client, thanks to a simple [DuckDB extension](docs/Extension.md), and a [Terraform](https://www.terraform.io/) template automatically deployed on the user's virtual private cloud (VPC), from the DuckDB client. Furthermore, its true [serverless architecture](docs/Architecture.md) dramatically reduces efforts required for monitoring and maintenance.

**Benefits**:
- End-user empowerment
- Faster adoption process
- Lower total cost of operations

## Designed for extensibility
Many SQL engines primarily targeted at public cloud deployment have limited extensibility with regards to user-defined functions (UDFs), user-defined aggregation functions (UDAFs), SQL semantics, indexing structures, connectivity, file format support, *etc.* Instead, PuffinDB fully leverages DuckDB's [extension](https://duckdb.org/docs/extensions/overview.html) mechanisms, while embedding runtimes for languages like [JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript), [Python](https://www.python.org/), or [Rust](https://www.rust-lang.org/) within its [serverless function](functions/engine/README.md) and [Monostore](docs/Monostore.md). This makes it possible to develop high-performance UDFs and UDAFs, extend the SQL language with constructs like [`SELECT THROUGH`](docs/Clientless.md#select-through), add advanced indexing structures for use cases like graph analysis, geospatial analysis, or time series analysis, integrate with remote databases and applications, and provide support for specific file formats.

**Benefits**:
- Richer feature set
- Enhanced user experience
- Higher performance
- Lower integration costs

## Designed for embedability
The vast majority of SQL engines primarily targeted at public cloud deployment are designed for standalone deployment. Instead, PuffinDB takes advantage of its true [serverless architecture](docs/Architecture.md) and its packaging as a self-provisioned and self-managed [Terraform](https://www.terraform.io/) template to make itself fully embedable within third-party systems and applications.

**Benefits**:
- Lower development costs
- Faster time to market
- Richer feature set

## Optimized for machine-generated queries
The query optimizers of most SQL engines have been optimized for human-designed SQL queries. Unfortunately, more and more queries are generated by machines, either through the use of [object-relational mappers](https://en.wikipedia.org/wiki/Object%E2%80%93relational_mapping) (ORM), or through the use of higher-level languages like [Malloy](https://github.com/malloydata/malloy/tree/main/packages/malloy) or [PRQL](https://prql-lang.org/). And this trend will accelerate in a dramatic fashion with the mainstream adoption of large language models such as [GPT-3](https://openai.com/blog/gpt-3-apps/). Therefore, PuffinDB incorporates query optimization techniques like [WeTune](http://www.news.cs.nyu.edu/~jinyang/pub/wetune-sigmod22.pdf), which make it possible to fully automate the discovery of query optimizer rules that are highly effective with machine-generated queries (100× acceleration).

**Benefits**:
- Lower development and maintenance costs for vendor
- Lower query processing times for users

## Scalable through large user bases
Most OLAP engines are designed for small groups of data analysts and data scientists, and would be prohibitive to make available to large groups of end users, especially if these have limited technical skills. On the contrary, thanks to its [clientless](docs/Clientless.md) and [serverless](docs/Architecture.md) architectures, and its advanced governor features, PuffinDB can be made available to large groups of users, at low and predictable costs.

**Benefits**:
- Broader userbase accessibility
- Lower risks of runaway charges
- Lower total cost of operations
