# Types

The following table maps [DuckDB](https://duckdb.org/docs/sql/data_types/overview) types to [Apache Iceberg Types](https://iceberg.apache.org/docs/latest/schemas/):

| DuckDB | Description  | Iceberg |
| ------ | ------------ | ------- |
| `BIGINT` | Signed eight-byte integer | `long` |
| `BOOLEAN`	 | Logical boolean (true \| false) | `boolean` |
| `BLOB` | Variable-length binary data | `binary` |
| `DATE` | Calendar date (year, month day) | `date` |
| `DOUBLE` | Double precision floating-point number (8 bytes) | `double` |
| `DECIMAL` | Fixed-precision floating point number with the given scale and precision | `decimal` |
| `HUGEINT` | Signed sixteen-byte integer | Cast to `long` (with loss) |
| `INTEGER` | Signed four-byte integer | `int` |
| `INTERVAL` | Date \| time delta | Cast to `long` |
| `LIST` | List with elements of any data type | `list` |
| `MAP` | Map with keys and values of any data type | `map` |
| `STRUCT` | Record with named fields of any data type | `struct` |
| `REAL` | Single precision floating-point number (4 bytes) | `float` |
| `SMALLINT` | Signed two-byte integer | Cast to `int` |
| `TIME` | Time of day (no time zone) | `time` |
| `TIMESTAMP` | Combination of time and date | `timestamp` |
| `TIMESTAMPTZ` | Combination of time and date that uses the current time zone | `timestamptz` |
| `TINYINT` | Signed one-byte integer | Cast to `int` |
| `UBIGINT` | Unsigned eight-byte integer | Cast to `long` |
| `UINTEGER` | Unsigned four-byte integer | Cast to `int` |
| `USMALLINT` | Unsigned two-byte integer | Cast to `int` |
| `UTINYINT` | Unsigned one-byte integer | Cast to `int` |
| `UUID` | UUID data type | Cast to `fixed` |
| `VARCHAR` | Variable-length character string | `string` |
| Cast to `VARCHAR` | Fixed-length character string | `fixed` |
