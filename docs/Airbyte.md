# Airbyte

The [PuffinDB extension](Extension.md) for [DuckDB](https://duckdb.org/) will make it possible to use any [Airbyte Connector](https://airbyte.com/connectors) directly from DuckDB.

## Architecture
Support for Airbyte connectors will be offered using the following components:
- [Airbyte Protocol](https://docs.airbyte.com/understanding-airbyte/airbyte-protocol/)
- [Airbyte CDK](https://airbyte.com/connector-development-kit)
- [Airbyte Connectors](https://github.com/airbytehq/airbyte/tree/fd13d43a13abc028657e0af4584d912f57d86382/airbyte-integrations/connectors)

If DuckDB is used wihtout the standard [DuckDB Python API](https://duckdb.org/docs/api/python/overview.html) ([C](https://duckdb.org/docs/api/c/overview), [C++](https://duckdb.org/docs/api/cpp), [Java](https://duckdb.org/docs/api/java), [Node.js](https://duckdb.org/docs/api/nodejs/overview), [R](https://duckdb.org/docs/api/r), [Rust](https://duckdb.org/docs/api/rust.html), or [WASM](https://duckdb.org/docs/api/wasm)), this feature will require the installation of a collocated [CPython](https://github.com/python/cpython) runtime (barebone or via [Pyodide](https://pyodide.org/en/stable/)). Once the PuffinDB extension and the Python runtime have been installed, individual [connectors](https://github.com/airbytehq/airbyte/tree/fd13d43a13abc028657e0af4584d912f57d86382/airbyte-integrations/connectors) will be installed through DuckDB's SQL API.

This feature will work with DuckDB deployed either client-side or cloud-side.

Airbyte Core will not be used.

## Syntax
Read operations performed through Airbyte connectors will use the following syntax:

```sql
SELECT * FROM airbyte(...)
```

The syntax for write operations is currently under development.

## Why Airbyte?
- Solid architecture
- Best collection of read-write connectors currently available under a liberal open source license
- Great company ready to provide commercial support if necessary

## Benefits
- No need to develop and | or install one DuckDB extension per application
- Mature and robust framework for developing new connectors
- Ability to perform joins across applications
- Ability to integrate with hundreds of applications through SQL
- Ability to use SQL query generators like [PRQL](https://prql-lang.org/) or [Malloy](https://www.malloydata.dev/) (*Cf.* [Query Proxy](Query%20Proxy.md))
- Ability to execute on the [Edge-Driven Data Integration](../EDDI.md) vision
