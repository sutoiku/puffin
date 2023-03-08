# HTAP

PuffinDB uses a dual strategy for supporting hybrid transactional/analytical processing ([HTAP](https://en.wikipedia.org/wiki/Hybrid_transactional/analytical_processing)) with lakehouse tables:

- [Amazon Athena](https://aws.amazon.com/athena/) for low-frequency | high-latency updates (default)
- [Amazon EBS](https://aws.amazon.com/ebs/) for high-frequency | low-latency updates

By default, updates on lakehouse tables are made using Amazon Athena (*Cf.* [Updating Iceberg table data](https://docs.aws.amazon.com/athena/latest/ug/querying-iceberg-updating-iceberg-table-data.html)). But for higher performance (more IOPS and lower latency), a lakehouse table can be transiently copied to an Amazon EBS volume mounted onto the [Monostore](Monostore.md), using the native [DuckDB](https://duckdb.org/) file format. When doing so, the table is locked on the lakehouse, and all read | write transactions are performed on Amazon EBS by the DuckDB engine running on the Monostore. This transient copy of a table from the lakehouse to the block storage is usually done for the duration of an HTAP session lasting for a few hours, and which lifecycle is mirrored by the Monostore's lifecycle. Once the Monostore is stopped, the table is written back to the lakehouse, then finally unlocked.
