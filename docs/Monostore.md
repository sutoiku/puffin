# Monostore

One of PuffinDB's main insights is to complement its [mostly-serverless architecture](Architecture.md) with what we call a **Monostore**.

## What is a Monostore?
A Monostore is a single server-based container (*e.g.* [EC2](https://aws.amazon.com/ec2/) instance) deployed alongside a fleet of serverless functions (*e.g.* [AWS Lambda](https://aws.amazon.com/lambda/)).

The Monostore serves multiple critical functions:

- Reducing the results of partial queries executed on fleets of serverless functions.
- Executing parts of queries that cannot be distributed efficiently.
- Offering a large directly-addressable in-memory cache for operations like sorting or shuffling.

## What is the lifecycle of a Monostore?
For most deployments, a Monostore is instantiated for a particular team (group of users), just before the start of the business day, and is shut down soon after its end (this provisioning process can be fully automated of course). It is sized according to the team's planned needs for the day, based on historical usage patterns and occasional changes in business activity (up or down). This daily provisioning plays a criticol role in maximizing the value derived from limited cloud infrastructure budgets.

## How should a Monostore be sized?
The minimum size of a Monostore should be set above the uncompressed size of the datasets that must be cached on it, with a 20% to 50% extra capacity for headroom. Using a larger monostore will accelerate most queries, but will increase costs. If budgets are limited, its size can be reduced to the size of compressed datasets, but this will slow queries down by a factor of 2 to 5.

In addition, a Monostore should be sized in such a way that it can hold an entire column of the tallest table that needs to be analyzed, alongside a column of pointers. For columns encoded on 64 bits, this means 128 bits (16 Bytes) per row. For example, if your tallest table has 100 billion rows, you will want a Monostore with at least 1.92 TB of RAM (1.6 TB + 20%). This will make it possible to compute most univariate statistics on all columns of your table, including exact quantiles that require full sorting.

## How large a Monostore can I get?
When using [Amazon Web Services](https://aws.amazon.com/), the [`u-24tb1.112xlarge`](https://aws.amazon.com/ec2/instance-types/high-memory/) instances will offer: 
- 448 vCPUs
- 24 TiB of RAM
- 100 Gbps of network bandwidth
- 169 seconds (less than 3 minutes) to fill 80% of RAM with data uncompressed
- $218.40/hour (on-demand) — $9.1/TiB·hour

This is sufficient for reduced datasets of 20 TB in size compressed, or 100 TB to 200 TB in size uncompressed.

A **reduced dataset** is a dataset produced by reducing the results of partial queries produced by a fleet of serverless functions.

## What is the most powerful instance for a 1 TB Monostore?
If your dataset is 1 TB or smaller, the most powerful instance available on [Amazon Web Services](https://aws.amazon.com/) is the [`p4de.24xlarge`](https://aws.amazon.com/ec2/instance-types/p4/):
- 96 vCPUs
- 1,152 GiB of RAM
- 8 × 1 TB NVMe SSD
- 8 × NVIDIA A100 GPUs
- 640 GB of HBM2e GPU RAM
- 400 Gbps ENA and EFA network bandwidth
- 2 seconds to fill 80% of RAM with data uncompressed
- $40.96/hour (on-demand) — $36.40/TiB·hour
