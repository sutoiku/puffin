# Engine

The **Engine** is a [Bun](https://bun.sh/) serverless function providing a cloud-side [JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript) runtime. It embeds the following components:

- [DuckDB](https://duckdb.org/) with [Node.js API](https://duckdb.org/docs/api/nodejs/overview.html)
- [PuffinDB Extension](../../docs/Extension.md) with [CPython](https://github.com/python/cpython) binding
- [CPython](https://github.com/python/cpython) Python runtime
- [TCPunch](https://github.com/spcl/tcpunch) NAT hole puncher
- [Distributed Query Planner](../../docs/Query%20Planner.md)
- [Distributed Query Engine](../../docs/Query%20Engine.md)
- [SQLGlot](https://github.com/tobymao/sqlglot) SQL parser and transpiler
- [Ibis](https://ibis-project.org/) interface for data wrangling
- [Airbyte](https://github.com/airbytehq/airbyte) connector framework ([Protocol](https://docs.airbyte.com/understanding-airbyte/airbyte-protocol/), [CDK](https://airbyte.com/connector-development-kit), and [Connectors](https://github.com/airbytehq/airbyte/tree/fd13d43a13abc028657e0af4584d912f57d86382/airbyte-integrations/connectors))
- [scikit-learn](https://scikit-learn.org/) for maching learning
- [Modal client](https://github.com/modal-labs/modal-client) for GPU acceleration
