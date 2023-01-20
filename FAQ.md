# Frequently Asked Questions

Please ask unanswered questions by creating a [new issue](https://github.com/sutoiku/pafin/issues).

## Can I use Pafin without a Lakehouse?
Yes, you can use Pafin with just an Object Store like [Amazon S3](https://aws.amazon.com/s3/).

## Why should I use a Lakehouse like Iceberg instead of just an Object Store like S3?
A Lakehouse like [Apache Iceberg](https://iceberg.apache.org/), [Apache Hudi](https://hudi.apache.org/), and [Delta Lake](https://delta.io/) offers many critical features:
- [Schema evolution](https://iceberg.apache.org/docs/latest/evolution/#schema-evolution) supports add, drop, update, or rename, and has [no side-effects](https://iceberg.apache.org/docs/latest/evolution/#correctness).
- [Hidden partitioning](https://iceberg.apache.org/docs/latest/partitioning/) prevents user mistakes that cause silently incorrect results or extremely slow queries.
- [Partition layout evolution](https://iceberg.apache.org/docs/latest/evolution/#partition-evolution) can update the layout of a table as data volume or query patterns change.
- [Time travel](https://iceberg.apache.org/docs/latest/spark-queries/#time-travel) enables reproducible queries that use exactly the same table snapshot.
- [Table version rollback](https://iceberg.apache.org/docs/latest/) allows users to quickly correct problems by resetting tables to a good state.
- [Advanced filtering](https://iceberg.apache.org/docs/latest/performance/#data-filtering) prunes data files with partition and column-level stats, using table metadata.
- [Serializable isolation](https://iceberg.apache.org/docs/latest/reliability/) makes table changes atomic and ensures that readers never see partial or uncommitted changes.
- [Multiple concurrent writers](https://iceberg.apache.org/docs/latest/reliability/#concurrent-write-operations) use optimistic concurrency and transaction retry.

Note: feature deacriptions courtesy of [Apache Iceberg](https://iceberg.apache.org/)

## What does *Pafin* mean?
*Pafin* (not to be confused with [Puffin](https://iceberg.apache.org/puffin-spec/)) is the Japanese [romanization](https://en.wikipedia.org/wiki/Romanization) of [*puffin*](https://en.wikipedia.org/wiki/Puffin), much like [*sutoiku*](https://github.com/sutoiku) is to [*stoic*](https://stoic.com/).
