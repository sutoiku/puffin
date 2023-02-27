# Types

The following table maps [DuckDB types](https://duckdb.org/docs/sql/data_types/overview) to [Apache Arrow](https://arrow.apache.org/docs/python/api/datatypes.html#factory-functions) and [Apache Iceberg](https://iceberg.apache.org/docs/latest/schemas/) types:

| DuckDB | Description  | Arrow | Iceberg |
| ------ | ------------ | ----- |------- |
| `BIGINT` | Signed eight-byte integer | `tbd` | `long` |
| `BOOLEAN`	 | Logical boolean (true \| false) | `tbd` | `boolean` |
| `BLOB` | Variable-length binary data | `tbd` | `binary` |
| `DATE` | Calendar date (year, month day) | `tbd` | `date` |
| `DOUBLE` | Double precision floating-point number (8 bytes) | `tbd` | `double` |
| `DECIMAL` | Fixed-precision floating point number | `tbd` | `decimal` |
| `HUGEINT` | Signed sixteen-byte integer | `tbd` | Cast to `long` ([with loss](https://github.com/sutoiku/puffin/issues/2)) |
| `INTEGER` | Signed four-byte integer | `tbd` | `int` |
| `INTERVAL` | Date \| time delta | `tbd` | Cast to `long` |
| `LIST` | List with elements of any data type | `tbd` | `list` |
| `MAP` | Map with keys and values of any data type | `tbd` | `map` |
| `REAL` | Single precision floating-point number (4 bytes) | `tbd` | `float` |
| `SMALLINT` | Signed two-byte integer | `tbd` | Cast to `int` |
| `STRUCT` | Record with named fields of any data type | `tbd` | `struct` |
| `TIME` | Time of day (no time zone) | `tbd` | `time` |
| `TIMESTAMP` | Combination of time and date | `tbd` | `timestamp` |
| `TIMESTAMPTZ` | Combination of time and date with time zone | `tbd` | `timestamptz` |
| `TINYINT` | Signed one-byte integer | `tbd` | Cast to `int` |
| `UBIGINT` | Unsigned eight-byte integer | `tbd` | Cast to `long` |
| `UINTEGER` | Unsigned four-byte integer | `tbd` | Cast to `int` |
| `USMALLINT` | Unsigned two-byte integer | `tbd` | Cast to `int` |
| `UTINYINT` | Unsigned one-byte integer | `tbd` | Cast to `int` |
| `UUID` | UUID data type | `tbd` | Cast to `fixed` |
| `VARCHAR` | Variable-length character string | `tbd` | `string` |
| Cast to `VARCHAR` | Fixed-length character string | `tbd` | `fixed` |

**Note**: A `FIXED` type for fixed-length character strings [might be added](README.md#credits) to [DuckDB](https://duckdb.org/docs/sql/data_types/overview).
