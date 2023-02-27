# Types

The following table maps [DuckDB types](https://duckdb.org/docs/sql/data_types/overview) to [Apache Arrow](https://arrow.apache.org/docs/python/api/datatypes.html#factory-functions) and [Apache Iceberg](https://iceberg.apache.org/docs/latest/schemas/) types:

| DuckDB | Description  | Arrow | Iceberg |
| ------ | ------------ | ----- |------- |
| `BIGINT` | Signed eight-byte integer | `int64` | `long` |
| `BOOLEAN`	 | Logical boolean (true \| false) | `bool` | `boolean` |
| `BLOB` | Variable-length binary data | `binary` | `binary` |
| `DATE` | Calendar date (year, month day) | `date32` | `date` |
| `DOUBLE` | Double precision floating-point number (8 bytes) | `float64` | `double` |
| `DECIMAL` | Fixed-precision floating point number | `decimal128` | `decimal` |
| `HUGEINT` | Signed sixteen-byte integer | Cast to `int64` (with loss) | Cast to `long` ([with loss](https://github.com/sutoiku/puffin/issues/2)) |
| `INTEGER` | Signed four-byte integer | `int32` | `int` |
| `INTERVAL` | Date \| time delta | `month_day_nano_interval` | Cast to `long` |
| `LIST` | List with elements of any data type | `list_` | `list` |
| `MAP` | Map with keys and values of any data type | `map_` | `map` |
| `REAL` | Single precision floating-point number (4 bytes) | `float32` | `float` |
| `SMALLINT` | Signed two-byte integer | `int16` | Cast to `int` |
| `STRUCT` | Record with named fields of any data type | `struct` | `struct` |
| `TIME` | Time of day (no time zone) | `time32` | `time` |
| `TIMESTAMP` | Combination of time and date | `timestamp` | `timestamp` |
| `TIMESTAMPTZ` | Combination of time and date with time zone | `timestamp` | `timestamptz` |
| `TINYINT` | Signed one-byte integer | `int8` | Cast to `int` |
| `UBIGINT` | Unsigned eight-byte integer | `uint64` | Cast to `long` |
| `UINTEGER` | Unsigned four-byte integer | `uint32` | Cast to `int` |
| `USMALLINT` | Unsigned two-byte integer | `uint16` | Cast to `int` |
| `UTINYINT` | Unsigned one-byte integer | `uint8` | Cast to `int` |
| `UUID` | UUID data type | Cast to `string` | Cast to `fixed` |
| `VARCHAR` | Variable-length character string | `string` | `string` |
| Cast to `INTERVAL` | Duration in nanoseconds | `duration` | Cast to `long` |
| Cast to `VARCHAR` | Fixed-length character string | Cast to `string` | `fixed` |

**Note**: A `FIXED` type for fixed-length character strings [might be added](README.md#credits) to [DuckDB](https://duckdb.org/docs/sql/data_types/overview).

**Note**: The [`dictionary`](https://arrow.apache.org/docs/python/generated/pyarrow.dictionary.html#pyarrow.dictionary), [`field`](https://arrow.apache.org/docs/python/generated/pyarrow.field.html#pyarrow.field), and [`schema`](https://arrow.apache.org/docs/python/generated/pyarrow.schema.html#pyarrow.schema) Apache Arrow types are ignored for now.
