# Monostore

One of PuffinDB's main insights is to complement its [mostly-serverless architecture](Architecture.md) with what we call a **Monostore**.

## What is a Monostore?
A Monostore is a single server-based container (*e.g.* [EC2](https://aws.amazon.com/ec2/) instance) deployed alongside a fleet of serverless functions (*e.g.* [AWS Lambda](https://aws.amazon.com/lambda/)).
