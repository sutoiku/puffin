# Engine

The **Engine** is a [Node.js](https://nodejs.org/en/) serverless function implementing the query engine. It embeds [DuckDB](https://duckdb.org/docs/api/nodejs/overview.html).

**Note**: The Engine and [Planner](../planner/README.md) run on two separate serverless functions because the former uses Node.js while the latter uses Java.
