# HTAP

PuffinDB uses a dual strategy for supporting hybrid transactional/analytical processing ([HTAP](https://en.wikipedia.org/wiki/Hybrid_transactional/analytical_processing)) with lakehouse tables:

- [Amazon Athena](https://aws.amazon.com/athena/) for low-frequency | high-latency updates (default)
- [Amazon EBS](https://aws.amazon.com/ebs/) for high-frequency | low-latency updates

By default, updates on lakehouse tables are made using Amazon Athena (*Cf.* [Updating Iceberg table data](https://docs.aws.amazon.com/athena/latest/ug/querying-iceberg-updating-iceberg-table-data.html)). But for higher performance (more IOPS and lower latency), lakehouse tables can be transiently copied to an Amazon EBS volume mounted onto the [Monostore](Monostore.md). When doing so, the table is locked on the lakehouse, and all read | write transactions are performed on Amazon EBS by the [DuckDB](https://duckdb.org/) running on the Monostore.
