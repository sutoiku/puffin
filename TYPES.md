# Types

The following table maps [DuckDB](https://duckdb.org/docs/sql/data_types/overview) types to [Apache Iceberg Types](https://iceberg.apache.org/docs/latest/schemas/):

| DuckDB | Description  | Iceberg |
| ------ | ------------ | ------- |
| `BIGINT` | signed eight-byte integer | `long` |
| `BOOLEAN`	 | logical boolean (true \| false) | `boolean` |
| `BLOB` | variable-length binary data | `binary` |
| `DATE` | calendar date (year, month day) | `date` |
| `DOUBLE` | double precision floating-point number (8 bytes) | `double` |
| `DECIMAL` | fixed-precision floating point number with the given scale and precision | `decimal` |
| `HUGEINT` | signed sixteen-byte integer | Cast to `long` (with loss) |
| `INTEGER` | signed four-byte integer | `int` |
| `INTERVAL` | date \| time delta | Cast to `long` |
| `LIST` | list with elements of any data type | `list` |
| `MAP` | map with keys and values of any data type | `map` |
| `STRUCT` | record with named fields of any data type | `struct` |
| `REAL` | single precision floating-point number (4 bytes) | `float` |
| `SMALLINT` | signed two-byte integer | Cast to `int` |
| `TIME` | time of day (no time zone) | `time` |
| `TIMESTAMP` | combination of time and date | `timestamp` |
| `TIMESTAMPTZ` | combination of time and date that uses the current time zone | `timestamptz` |
| `TINYINT` | signed one-byte integer | Cast to `int` |
| `UBIGINT` | unsigned eight-byte integer | Cast to `long` |
| `UINTEGER` | unsigned four-byte integer | Cast to `int` |
| `USMALLINT` | unsigned two-byte integer | Cast to `int` |
| `UTINYINT` | unsigned one-byte integer | Cast to `int` |
| `UUID` | UUID data type | Cast to `fixed` |
| `VARCHAR` | variable-length character string | `string` |
| Cast to `VARCHAR` | fixed-length character string | `fixed` |
