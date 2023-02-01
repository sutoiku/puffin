# Icecap

**⚠ WARNING**: This project is under consideration and has not been confirmed.

Icecap would be an alternative implementation of [Iceberg tables](https://iceberg.apache.org/spec/) using the native [DuckDB](https://duckdb.org/) file format.

## Overview

- Optimized for low latency
- Supporting edits in place
- Powered by serverless functions, [Redis](https://redis.io/), and [DuckDB](https://duckdb.org/) (no Spark)
