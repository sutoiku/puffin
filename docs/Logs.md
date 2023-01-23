# Logs

Logs of all queries executed by the query engine

## Schema
Every log entry includes the following information:

| ID | Type | Description |
| -- | ---- | ----------- |
| `id` | [`ulid`](https://github.com/ulid/spec) | Unique identifier (includes timestamp) |
| `query` | `string` | Query executed by query engine |
| `duration` | `integer` | Duration of the query execution in milliseconds |
| `cache` | `uri` | URI of cached query result on Object Store |
| `error` | `string` | Error message |

## Serialization

Logs are serialized as [JSON](https://redis.io/docs/stack/json/) values in a [Redis](https://redis.io/) cluster (using [Amazon ElastiCache for Redis](https://aws.amazon.com/elasticache/redis/)).

**Related FAQ**: [Why use a Redis cluster for storing query logs?](../FAQ.md#why-use-a-redis-cluster-for-storing-query-logs)

**Note**: For simpler or smaller deployments that do not require distributed queries, we could consider using [Amazon DynamoDB](https://aws.amazon.com/dynamodb/) as an alternative option.
