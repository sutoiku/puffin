# Metastore

The Metastore is a metadata store for PuffinDB tables. It extends the data lake's store with metadata that cannot be stored in the latter.

## Partition-Level Column Statistics
The Metastore manages partition-level column statistics that are physically stored on the Object Store (*e.g.* [Amazon S3](https://aws.amazon.com/s3/)).

These statistics are managed across three files per partition:
- A Parquet file for summary statistics (minimum, maximum, mean, etc.), with one column per summary statistic and one row per column
- A parquet file for the histograms of numerical columns, with one column per column and one row per bin (1,000 or 10,000)
- A parquet file the the frequencies of discrete column, with columns for the column, value, and frequency, and one row per column·value

This approach is a lot more purpose driven that the proposed [Iceberg Pufin](https://iceberg.apache.org/puffin-spec/) format, and would offer the following benefits:
- No need to develop and maintain a new parser | serializer
- Low-latency lookup of statistics for specific columns
- Much smaller file size
- Totally independent from [Iceberg Table Format](https://iceberg.apache.org/spec/) (compatible with [Delta Lake](https://delta.io/) and [Hudi](https://hudi.apache.org/) table formats).
