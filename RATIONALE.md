# Rationale for a new distributed SQL engine

There are many excellent distributed SQL engines currently available on the market. Why do we need yet another one?

## True serverless architecture
Most distributed SQL engines are designed to be deployed on conventional containers (*e.g.* [Amazon EC2](https://aws.amazon.com/ec2/) instances). PuffinDB is designed to run on serverless functions (*e.g.* [AWS Lambda](https://aws.amazon.com/lambda/) functions). The largest currently-available [Amazon EC2](https://aws.amazon.com/ec2/) instance ([`u-24tb1.112xlarge`](https://aws.amazon.com/ec2/instance-types/high-memory/)) has 448 vCPUs, 24 TB of RAM, and 100 Gbps of network bandwidth, and its on-demand vailability is not guaranteed. In comparison, 10,000 [AWS Lambda](https://aws.amazon.com/lambda/) functions offer an aggregated 60,000 vCPUs (134×), 200 TB of RAM (8×), and 8 Tbps of network bandwidth (80×). Furthermore, EC2 instances are billed from instantiation to termination (usually several hours at a time), while Lambda functions are billed by the millisecond, and only for the time during which they are actually used.

**Benefits**:
- Lower query processing times
- Lower costs
- Higher availability
- Higher elasticity

## Future-proof architecture
Coming soon...

## Designed for VPC deployment
Coming soon...

## Designed for real-time analytics
Coming soon...

## Designed for interactive analytics
Coming soon...

## OLTP + OLAP = HTAP
Coming soon...

## Transformation and Analysis
Coming soon...

## Designed for data lakes
Coming soon...

## Optimized for machine-generated queries
Coming soon...

## Designed for extensibility
Coming soon...

## Designed for embedability
Coming soon...

## Scalable through large user bases
Coming soon...
