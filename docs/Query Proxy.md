# Query Proxy

PuffinDB is built upon the strong belief that [SQL](https://en.wikipedia.org/wiki/SQL) is [Cloud Data](../CLOUD.md)'s preferred language, and that countless wonderful SQL query generators like [Malloy](https://www.malloydata.dev/), [PRQL](https://prql-lang.org/), [Baxter](https://baxterhq.com/), or [WHERE TRUE](https://www.wheretrue.com/) will emerge over the coming years. Furthermore, this trend will be accelerated by the advent of large [language models](https://en.wikipedia.org/wiki/Language_model) like OpenAI's [ChatGPT](https://openai.com/blog/chatgpt/). These query generators will all share a common set of requirements that could be factorized by PuffinDB, with the intent of reducing R&D efforts for vendors, and delivering a better overall experience to users:

- Integration with the user's client
- Integration with the user's data lake
- SQL parsing | serializing
- SQL query optimization
- [Scale-out and scale-up](../CLOUD.md#scale-out-and-scale-up)
- IP protection

## Architecture
To achieve these goals, query generation is architected around four main components:
- A client-side [DuckDB extension](Extension.md)
- A cloud-side **Query Proxy** operated by PuffinDB
- A cloud-side **Query Generator** running on the vendor's cloud or the user's VPC (depending on IP protection requirements)
- A cloud-side **Registry** of supported query generators managed by PuffinDB

The `puffindb` DuckDB extension is installed once by the user. From there, support for any number of query generators can be added from DuckDB's SQL API, using the public **Registry** managed by PuffinDB. Once a query is submitted by the user through DuckDB's SQL API, it is sent to the cloud-side **Query Proxy**, which forwards it to the **Query Generator**. The query is then translated into SQL and sent back to the user's DuckDB engine for execution.

Several options are currently being considered for the packaging of Query Generators. One of them is to use [WebAssembly](https://webassembly.org/), which would allow both client-side and cloud-side execution. This proposed meta-extension mechanism would also make it easier to develop certain DuckDB extensions, without having to compile them for different platforms. Another option is to provide bindings between DuckDB and a collocated [CPython](https://github.com/python/cpython) runtime (barebone or via [Pyodide](https://pyodide.org/)).

## Query Optimization
The exact same mechanism used for remote query generation will be used for remote query optimization by the [distributed query planner](Query%20Planner.md). Furthermore, the integration of technologies like [WeTune](https://ipads.se.sjtu.edu.cn/_media/publications/wetune_final.pdf) within the distributed query planner will ensure that machine-generated queries are properly optimized, even in cases where DuckDB's built-in optimizer might prove suboptimal (**query pre-optimization**).

## Query Delegation
The exact same architecture will be used for delegating queries to third-party engines with [`SELECT THROUGH`](Clientless.md#select-through). This feature can be used for:

- Data integration
- Data validation
- Outlier detection
- Error correction
- Prediction
- Recommendation
- Synthetic data generation
- *etc.*

Out of the box, PuffinDB will provide integration with the following databases:

- [Athena](https://aws.amazon.com/athena/)
- [Databricks](https://www.databricks.com/)
- [DuckDB](https://duckdb.org/)
- [EMR Spark SQL](https://aws.amazon.com/emr/)
- [RDS for Aurora](https://aws.amazon.com/rds/aurora/)
- [RDS for MariaDB](https://aws.amazon.com/rds/mariadb/)
- [RDS for MySQL](https://aws.amazon.com/rds/mysql/)
- [RDS for Oracle](https://aws.amazon.com/rds/oracle/)
- [RDS for PostgreSQL](https://aws.amazon.com/rds/postgresql/)
- [RDS for SQL Server](https://aws.amazon.com/rds/sqlserver/)
- [Redshift](https://aws.amazon.com/redshift/)
- [Snowflake](https://www.snowflake.com/en/)
- [SQLite](https://www.sqlite.org/)


## Dialect Translation
When delegating a subquery to a third-party SQL engine with [`SELECT THROUGH`](Clientless.md#select-through), the PuffinDB extension will handle SQL dialect translation, using [SQLGlot](https://github.com/tobymao/sqlglot) deployed on a collocated [CPython](https://github.com/python/cpython) runtime. This will make it easy to write composite queries involving multiple SQL engines while using a single dialect. The following dialects are currently supported:

- BigQuery
- ClickHouse
- Databricks
- Drill
- DuckDB
- Hive
- MySQL
- Oracle
- Postgres
- Presto
- Redshift
- Snowflake
- Spark
- SQLite
- StarRocks
- Tableau
- Teradata
- Trino
- T-SQL

## Benefits for Vendors
- No need to develop and distribute any proprietary DuckDB extension
- No need to develop yet another SQL parser | serializer
- Access to the metadata of tables managed by the most popular data lakes ([Apache Iceberg](https://iceberg.apache.org/), [Apache Hudi](https://hudi.apache.org/), [Delta Lake](https://delta.io/))
- Direct integration with the user's VPC
- [Scale-out and scale-up](../CLOUD.md#scale-out-and-scale-up) of complex and | or large queries through [distributed SQL engine](Query%20Engine.md)
- IP protection when running query generator on vendor's cloud

## Benefits for Users
- No need to install one DuckDB extension for every query generator
- Cloud-side acceleration of query generation and query optimization
- Faster queries (through query pre-optimization)
- Data and metadata confidentiality when running query generator on user's VPC or user's client
