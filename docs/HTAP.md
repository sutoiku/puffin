# HTAP

PuffinDB uses a dual strategy for supporting hybrid transactional/analytical processing ([HTAP](https://en.wikipedia.org/wiki/Hybrid_transactional/analytical_processing)) with lakehouse tables:

- [Amazon Athena](https://aws.amazon.com/athena/) for low-frequency | high-latency updates
- [Amazon EBS](https://aws.amazon.com/ebs/) for high-frequency | low-latency updates

By default, updates on lakehouse tables are made using Amazon Athena (*Cf.* [Updating Iceberg table data](https://docs.aws.amazon.com/athena/latest/ug/querying-iceberg-updating-iceberg-table-data.html))
