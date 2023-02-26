# Metastore

The Metastore is a metadata store for PuffinDB tables. It extends the data lake's store with metadata that cannot be stored in the latter.

## Partition-Level Column Statistics
The Metastore manages partition-level column statistics that are physically stored on the Object Store (*e.g.* [Amazon S3](https://aws.amazon.com/s3/)).

These statistics are managed across three files per partition:
- A Parquet file for summary statistics (minimum, maximum, mean, etc.) with one column per summary statistic and one row per column of the original table
- 
