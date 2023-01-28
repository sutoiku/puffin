# Distributed Query Engine
The distributed query engine is designed with the following requirements in mind:
- Deployed using the standard [DuckDB](https://duckdb.org/) engine supercharged with a PuffinDB [extension](https://duckdb.org/docs/extensions/overview)
- Abstracted with the [`SELECT THROUGH`](../EDDI.md#implementation) syntax
- Powered by [Apache Arrow](https://arrow.apache.org/)
