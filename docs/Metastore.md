# Metastore

The Metastore is a metadata store for PuffinDB tables. It extends the data lake's store with metadata that cannot be stored in the latter.

## Partition-Level Column Statistics
The Metastore manages partition-level column statistics that are physically stored on the Object Store (*e.g.* [Amazon S3](https://aws.amazon.com/s3/)). These statistics are critical for helping the [distributed query planner](Query%20Planner.md) perform certain optimizations, or for allowing user interfaces to rapidly display quantitatively-rich previews of tables before querying (*e.g.* [STOIC Table Editor](https://github.com/stoic-doc/Community/discussions/534)).

### Requirements
- Support tables with fairly large numbers of columns (hundreds or even thousands)
- Support user-defined summary statistics
- Optimize partial lookups for subsets of columns
- Optimize partial lookups for top frequencies
- Optimize partial lookups for histogram subsets

In order to fulfill these requirements, statistics are captured across three files per partition:
- A Parquet file for summary statistics (minimum, maximum, mean, etc.), with one column per summary statistic and one row per column
- A parquet file the the frequencies of discrete column, with columns for the column, value, and frequency, and one row per column·value
- A parquet file for the histograms of numerical columns, with one column per column and one row per bin (1,000 or 10,000)

This approach would offer the following benefits over the proposed [Iceberg Pufin](https://iceberg.apache.org/puffin-spec/) file format:
- No need to develop and maintain a new parser | serializer
- Low-latency lookup of statistics for specific columns
- Much smaller file size
- Totally independent from [Iceberg Table Format](https://iceberg.apache.org/spec/), therefore compatible with [Delta Lake](https://delta.io/) and [Hudi](https://hudi.apache.org/) table formats.

This approach would offer the following benefits over a key-value store like [DynamoDB](https://aws.amazon.com/dynamodb/) or [Redis](https://redis.io/):
- Lower cost
- Higher scalability (when serving large numbers of concurrent requests)
- Higher throughput (when using up to one serverless function per partition)

### `statistics.parquet`
- One column per summary statistic (minimum, maximum, mean, *etc.*)
- One row per column of the related table
- Ordered as columns are ordered in the related table

This table format is optimized for storing both simple and complex summary statistics (*e.g.* Percentiles).

### `frequencies.parquet`
- Columns for column, value, and frequency
- One row per pair of column·value
- Ordered by column (as columns are ordered in the related table) and decreasing frequency

This table format is optimized for columns with large numbers of distinct values.

### `histograms.parquet`
- One column per column of the related table
- One row per bin (1,000 ot 10,000 bins)
- Ordered by increasing bin minimum value

This table format is optimized for storing high-resolution histograms.
