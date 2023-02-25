# Rationale for a new distributed SQL engine

There are many excellent distributed SQL engines currently available on the market. Why do we need yet another one?

- [True serverless architecture](#true-serverless-architecture)
- [Future-proof architecture](#future-proof-architecture)
- [Designed for VPC deployment](#designed-for-vpc-deployment)
- [Designed for real-time analytics](#designed-for-real-time-analytics)
- [Designed for interactive analytics](#designed-for-interactive-analytics)
- [Designed for transformation and analytics](#designed-for-transformation-and-analytics)
- [Designed for analytics and transactions](#designed-for-analytics-and-transactions)
- [Designed for next-generation query engine](#designed-for-next-generation-query-engine)
- [Designed for next-generation file formats](#designed-for-next-generation-file-formats)
- [Designed for data lakes](#designed-for-data-lakes)
- [Designed for integration](#designed-for-integration)
- [Designed for all users](#designed-for-all-users)
- [Designed for extensibility](#designed-for-extensibility)
- [Designed for embedability](#designed-for-embedability)
- [Optimized for machine-generated queries](#optimized-for-machine-generated-queries)
- [Scalable through large user bases](#scalable-through-large-user-bases)

## True serverless architecture
Most distributed SQL engines are designed to be deployed on conventional containers (*e.g.* [Amazon EC2](https://aws.amazon.com/ec2/) instances). PuffinDB is designed to run on serverless functions (*e.g.* [AWS Lambda](https://aws.amazon.com/lambda/) functions). The largest currently-available [Amazon EC2](https://aws.amazon.com/ec2/) instance ([`u-24tb1.112xlarge`](https://aws.amazon.com/ec2/instance-types/high-memory/)) has 448 vCPUs, 24 TB of RAM, and 100 Gbps of network bandwidth, and its on-demand availability is not guaranteed. In comparison, 10,000 [AWS Lambda](https://aws.amazon.com/lambda/) functions offer an aggregated 60,000 vCPUs (134×), 200 TB of RAM (8×), and 8 Tbps of network bandwidth (80×). Furthermore, EC2 instances are billed from instantiation to termination (usually several hours at a time), while Lambda functions are billed by the millisecond, and only for the time during which they are actually used.

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
Most OLAP engines and cloud data warehousing platforms are designed to be used alongside third-party data preparation tools for ETL and reverse ETL. PuffinDB is in and by itself a powerful data extraction, transformation, and loading tool, thanks to its extensive collection of database and application connectors, and its built-in data pipeline execution engine.

**Benefits**:
- Lower development costs
- Lower licensing costs
- Faster development cycles
- Lower data update latency
- Better data quality
- Enhanced data governance

## Designed for analytics and transactions
By definition, OLAP (online analytical processing) engines are designed to work on immutable data, and are commonly used downstream of OLTP (online transaction processing) systems. While PuffinDB is not designed to be used as primary system of records for high-frequency transactional applications, it is designed to support real-time updates and manual edits (adjustments) on data. As such, it can be considered as an analytics-oriented HTAP (hybrid transaction/analytical processing) engine.

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
Most SQL engines are designed for conventional file formats like CSV or Parquet. PuffinDB is designed to take advantage of next-generation files formats like [Puffin](https://iceberg.apache.org/puffin-spec/) (not to be confused with PuffinDB) (advanced file metadata), [Lance](https://github.com/eto-ai/lance) (high-performance random access), and the DuckDB native file format (updates in place).

**Benefits**:
- Lower query processing times and costs through enhanced query planning
- Higher performance for random access
- Lower data update latency

## Designed for data lakes
Most SQL engines targeted at public cloud deployment have been designed for Object Stores only (*e.g.* [Amazon S3](https://aws.amazon.com/s3/)). Instead, PuffinDB has been natively designed to take advantage of Data Lakes such as [Apache Iceberg](https://iceberg.apache.org/), [Apache Hudi](https://hudi.apache.org/), and [Delta Lake](https://delta.io/). This is one of the critical design elements that gives it full OLTP capabilities with ACID properties, while allowing it to connect to a wide range of data sources that are being migrated to data lakes.

**Benefits**:
- Enhanced transactional integrity
- Lower data integration costs

## Designed for integration
Coming soon...

## Designed for all users
Coming soon...

## Designed for extensibility
Coming soon...

## Designed for embedability
Coming soon...

## Optimized for machine-generated queries
Coming soon...

## Scalable through large user bases
Coming soon...
