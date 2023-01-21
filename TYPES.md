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
| `HUGEINT` | signed sixteen-byte integer | `N/A` |
| `INTEGER` | signed four-byte integer | `int` |
| `INTERVAL` | date / time delta | `foo` |
| `REAL` | single precision floating-point number (4 bytes) | `foo` |
| `SMALLINT` | signed two-byte integer | `foo` |
| `TIME` | time of day (no time zone) | `foo` |
| `TIMESTAMP` | combination of time and date | `foo` |
| `TIMESTAMPTZ` | combination of time and date that uses the current time zone | `foo` |
| `TINYINT` | signed one-byte integer | `foo` |
| `UBIGINT` | unsigned eight-byte integer | `foo` |
| `UINTEGER` | unsigned four-byte integer | `foo` |
| `USMALLINT` | unsigned two-byte integer | `foo` |
| `UTINYINT` | unsigned one-byte integer | `foo` |
| `UUID` | UUID data type | `foo` |
| `VARCHAR` | variable-length character string | `foo` |
