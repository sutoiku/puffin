# Rationale for a new distributed SQL engine

There are many excellent distributed SQL engines currently available on the market. Why do we need yet another one?

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
Most OLAP engines are designed to support analytics workloads on datasets that are updated at relatively low frequency (daily or longer). PuffinDB is designed for real-time analytics with much higher update frequencies (sub-second). This dramatically widens the range of use cases for which the engine can be used, while increasing the value of insights it gives access to.

**Benefits**:
- Wider applicability across use cases
- Higher business value

## Designed for interactive analytics
Coming soon...

## OLTP + OLAP = HTAP
Coming soon...

## Transformation and Analysis
Coming soon...

## Designed for data lakes
Coming soon...

## Designed for integration
Coming soon...

## Optimized for machine-generated queries
Coming soon...

## Designed for extensibility
Coming soon...

## Designed for embedability
Coming soon...

## Scalable through large user bases
Coming soon...
