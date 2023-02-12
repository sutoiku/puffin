# Monostore

One of PuffinDB's main insights is to complement its [mostly-serverless architecture](Architecture.md) with what we call a **Monostore**.

## What is a Monostore?
A Monostore is a single server-based container (*e.g.* [EC2](https://aws.amazon.com/ec2/) instance) deployed alongside a fleet of serverless functions (*e.g.* [AWS Lambda](https://aws.amazon.com/lambda/)).

The Monostore serves multiple critical functions:

- Reducing the results of partial queries executed on fleets of serverless functions.
- Executing parts of queries that cannot be distributed efficiently.
- Offering a large directly-addressable in-memory cache for operations like sorting or shuffling.

## What is the lifecycle of a Monostore?
For most deployments, a Monostore is instantiated for a particular team (group of users), just before the start of the business day, and is shut down soon after its end. It is sized according to the team's planned needs for the day, based on historical usage patterns and occasional changes in business activity (up or down). This daily provisioning plays a criticol role in maximizing the value derived from limited cloud infrastructure budgets.

## How should a Monostore be sized?
The minimum size of a Monostore should be set above the uncompressed size of the datasets that must be cached on it, with a 20% to 50% extra capacity for headroom. Using a larger monostore will accelerate most queries, but will increase costs. If budgets are limited, its size can be reduced to the size of compressed datasets, but this will slow queries down by a factor of 2 to 5.

## How large a Monostore can I get?
When using [Amazon Web Services](), the [`u-24tb1.112xlarge`](https://aws.amazon.com/ec2/instance-types/high-memory/) instances will offer: 
- 448 vCPUs
- 24 TB of RAM
- 100 Gbps of network bandwidth

This is sufficient for reduced datasets of 20 TB in size compressed, or 100 TB to 200 TB in size uncompressed.

A **reduced dataset** is the intermediary dataset produced by reducing the results of partial queries produced by the fleet of serverless functions.
