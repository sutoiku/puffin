# Python Engine

The **Python Engine** is a serverless function providing a cloud-side [Python](https://www.python.org/) runtime. It embeds the following components:

- [DuckDB](https://duckdb.org/) with [Python API](https://duckdb.org/docs/api/python/overview.html)
- [Distributed Query Planner](../../docs/Query%20Planner.md)
- [Distributed Query Engine](../../docs/Query%20Engine.md)
- [Ibis](https://ibis-project.org/) interface for data wrangling
- [Modal client](https://github.com/modal-labs/modal-client) for GPU acceleration
- [scikit-learn](https://scikit-learn.org/) for maching learning
- [SQLGlot](https://github.com/tobymao/sqlglot) SQL Parser and Transpiler
