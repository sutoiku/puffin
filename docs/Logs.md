# Logs

Logs of all queries executed by the query engine

## Schema
Every log entry includes the following information:

| ID | Type | Description |
| -- | ---- | ----------- |
| `id` | [`ulid`](https://github.com/ulid/spec) | Unique identifier (includes timestamp) |
| `query` | `string` | Query executed by query engine |
| `duration` | `integer` | Duration of query execution in milliseconds |
| `cache` | `uri` | URI of cached query result on Object Store |
| `error` | `string` | Error message |

## Serialization

Logs are serialized using an [Iceberg table](https://iceberg.apache.org/spec/) managed by the Spark cluster.

**Note**: The serialization of logs will not use the query engine, therefore will still work even when the query engine does not.
