# Planner

The **Planner** is a serverless function implementing the distributed SQL query planner. It embeds [Apache Calcite](https://calcite.apache.org/) and [Iceberg Java API](https://iceberg.apache.org/docs/latest/api/).

## Methods
The Planner serverless functions exposes the following public methods:
- `parseQuery`
- `planQuery`
- `scanTable`
