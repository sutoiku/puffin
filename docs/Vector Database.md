# Vector Database

PuffinDB is turning [DuckDB](https://duckdb.org/) into a full-blown vector database with the following features:

- Support for the [Lance](https://github.com/eto-ai/lance) file format
- Acceleration of [Faiss](https://github.com/facebookresearch/faiss) on GPU
- Custom SQL functions for vector processing
- Data pipeline automation with support for [PRQL](https://prql-lang.org/)
- Storage of very large vectors on data lakes such as [Iceberg](https://iceberg.apache.org/), [Delta Lake](https://delta.io/), and [Hudi](https://hudi.apache.org/).

PuffinDB will support most [NVIDIA](https://www.nvidia.com/) GPU accelerators, while developing specific optimizations for the [Grace Hopper Superchip](https://www.nvidia.com/en-us/data-center/grace-hopper-superchip/).
