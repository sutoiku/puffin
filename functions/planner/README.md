# Planner

The **Planner** is a [Java](https://en.wikipedia.org/wiki/Java_(programming_language)) serverless function implementing the distributed query planner. It embeds [Apache Calcite](https://calcite.apache.org/) and [Iceberg's Java API](https://iceberg.apache.org/docs/latest/api/).

**Note**: The Planner and [Engine](../engine/README.md) run on two separate serverless functions because the former uses Java while the latter uses Node.js.

## Methods
The Planner serverless functions exposes the following public methods:
- `parseQuery`
- `planQuery`
- `scanTable`
