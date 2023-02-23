# Serverless Architecture

PuffinDB has a radical serverless and cloud-native architecture. Deployment on "private clouds" is not a priority.

## Core Principles
- Do as much as possible with serverless functions ([AWS Lambda](https://aws.amazon.com/lambda/)).
- Do as much as possible of the remaining parts with serverless containers ([AWS Fargates](https://aws.amazon.com/fargate/)).
- Do the last bits with a single server-based container ([Monostore](Monostore.md)) vwith as much capacity as possible ([Amazon EC2](https://aws.amazon.com/ec2/)).
- Cache data in memory as aggressively as possible.
- Use an auto-scaling [Redis](https://redis.io/) cluster for synchronization (submillisecond transactions, millions of transactions per second).
- Use [NAT hole punching](https://github.com/spcl/tcpunch) for data shuffles.

## Why Serverless?
The largest on-demand [Amazon EC2](https://aws.amazon.com/ec2/) instance ([`u-24tb1.112xlarge`](https://aws.amazon.com/ec2/instance-types/high-memory/)) has 448 vCPUs, 24 TB of RAM, and 100 Gbps of network bandwidth. In comparison, 10,000 [AWS Lambda](https://aws.amazon.com/lambda/) functions offer an aggregated 60,000 vCPUs (134×), 200 TB of RAM (8×), and 8 Tbps of actual network bandwidth from [Amazon S3](https://aws.amazon.com/s3/) (80×). Furthermore, EC2 instances are billed from instantiation to termination (usually hours at a time), while Lambda functions are billed by the millisecond, and only for the time during which they are actually used. As a result, a true serverless architecture can offer one to two orders of magnitude greater performance, for one to two orders of magnitude lower costs.

## Serverless Components
PuffinDB is architected around the following serverless components:

- [Catalog](../functions/catalog/README.md) — [Java](https://en.wikipedia.org/wiki/Java_(programming_language)) serverless function packaging [Iceberg's Java API](https://iceberg.apache.org/docs/latest/api/)
- [Engine](../functions/engine/README.md) — [Bun](https://bun.sh/) serverless function packaging the query handler, [query planner](Query%20Planner.md), [query engine](Query%20Engine.md), and [CPython](https://github.com/python/cpython) runtime
- [Amazon Athena](https://aws.amazon.com/athena/) for executing write queries on lakehouse tables (eventually replaced by [Icecap](Icecap.md))
- [Amazon ElastiCache for Redis](https://aws.amazon.com/elasticache/redis/) for logging, queuing, and synchronization
- [Amazon S3](https://aws.amazon.com/s3/) for object storage
- [Amazon CloudFront](https://aws.amazon.com/cloudfront/) for cached query result distribution

**Note**: Technically-speaking, [Amazon ElastiCache for Redis](https://aws.amazon.com/elasticache/redis/) is not serverless, yet is a fully managed service.

## CloudFormation Templates
These components are packaged into a pair of complementary [AWS CloudFormation](https://aws.amazon.com/cloudformation/) templates:
- [Lakehouse Template](../templates/lakehouse/README.md) — usually instantiated once
- [Engine Template](../templates/engine/README.md) — usually instantiated multiple times

## Clientless Interface
PuffinDB has a [clientless](Clientless.md) architecture and can be used from any application embedding the [DuckDB](https://duckdb.org/) engine, using a simple [extension](Extension.md).
