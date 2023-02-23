# Airbyte

The [PuffinDB Extension](Extension.md) for [DuckDB](https://duckdb.org/) will make it possible to use any [Airbyte Connector](https://airbyte.com/connectors) from DuckDB.

## Architecture
Support for Airbyte connectors will be offered using the following components:
- [Airbyte Protocol](https://docs.airbyte.com/understanding-airbyte/airbyte-protocol/)
- [Airbyte CDK](https://airbyte.com/connector-development-kit)
- [Airbyte Connectors](https://github.com/airbytehq/airbyte/tree/fd13d43a13abc028657e0af4584d912f57d86382/airbyte-integrations/connectors)

If DuckDB is used wihtout the standard [DuckDB Python API](https://duckdb.org/docs/api/python/overview.html) ([C](https://duckdb.org/docs/api/c/overview), [C++](https://duckdb.org/docs/api/cpp), [Java](https://duckdb.org/docs/api/java), [Node.js](https://duckdb.org/docs/api/nodejs/overview), [R](https://duckdb.org/docs/api/r), [Rust](https://duckdb.org/docs/api/rust.html), [WASM](https://duckdb.org/docs/api/wasm)), this feature will require the installation of a collocated [CPython](https://github.com/python/cpython) runtime (barebone or via [Pyodide](https://pyodide.org/en/stable/)).
