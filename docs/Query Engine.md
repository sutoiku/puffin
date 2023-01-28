# Distributed Query Engine
The distributed query engine is designed with the following requirements in mind:
- Deployed using standard [DuckDB](https://duckdb.org/) engines supercharged with a PuffinDB [extension](https://duckdb.org/docs/extensions/overview)
- Abstracted with the [`SELECT THROUGH`](../EDDI.md#implementation) syntax to avoind unnecessary data copies
- Powered by [Apache Arrow](https://arrow.apache.org/) to accelerate data transfers between DuckDB engines
