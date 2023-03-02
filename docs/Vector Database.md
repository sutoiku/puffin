# Vector Database

PuffinDB is turning [DuckDB](https://duckdb.org/) into a next-generation vector database with the following features:

- Integration with all client applications embedding [DuckDB](https://duckdb.org/) through [clientless architecture](Clientless.md)
- Storage of very large vectors on data lakes such as [Iceberg](https://iceberg.apache.org/), [Delta Lake](https://delta.io/), and [Hudi](https://hudi.apache.org/)
- Support for the [Lance](https://github.com/eto-ai/lance) file format
- GPU acceleration
- Custom SQL functions for vector processing
- Data pipeline automation with support for [PRQL](https://prql-lang.org/)

PuffinDB will support most [NVIDIA](https://www.nvidia.com/) GPU accelerators, while developing specific optimizations for the [Grace Hopper Superchip](https://www.nvidia.com/en-us/data-center/grace-hopper-superchip/)

## Benefits over conventional vector database
- [Full SQL support](Query%20Proxy.md#dialect-translation)
- [Clientless architecture](Clientless.md)
- [Serverless architecture](Architecture.md)
- [Data pipeline engine](Pipeline%20Engine.md)
- [Connector framework](Airbyte.md)
- Easier integration
- Lower costs
