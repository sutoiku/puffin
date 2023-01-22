# Logs

Query logs will include the following information:

| ID | Type | Description |
| -- | ---- | ----------- |
| `id` | [`ULID`](https://github.com/ulid/spec) | Unique identifier (includes timestamp) |
| `query` | `string` | SQL query |
| `cache` | `uri` | URI of cached query result on Object Store |
