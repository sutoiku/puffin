# Vector Database

PuffinDB is turning [DuckDB](https://duckdb.org/) into a next-generation vector database with the following features:

- Storage of very large vectors on data lakes such as [Iceberg](https://iceberg.apache.org/), [Delta Lake](https://delta.io/), and [Hudi](https://hudi.apache.org/)
- Support for the [Lance](https://github.com/eto-ai/lance) file format
- Acceleration of [Faiss](https://github.com/facebookresearch/faiss) on GPU
- Custom SQL functions for vector processing
- Data pipeline automation with support for [PRQL](https://prql-lang.org/)
- Integration with all client applications embedding [DuckDB](https://duckdb.org/) through [clientless architecture](Clientless.md)

PuffinDB will support most [NVIDIA](https://www.nvidia.com/) GPU accelerators, while developing specific optimizations for the [Grace Hopper Superchip](https://www.nvidia.com/en-us/data-center/grace-hopper-superchip/).

![Grace Hopper](https://user-images.githubusercontent.com/1074452/221723235-f3299432-5960-49a3-a750-c8b5f2a8d9e2.png)
