# Logs

Logs of all queries

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

Logs are serialized using an [Iceberg table](https://iceberg.apache.org/spec/).
