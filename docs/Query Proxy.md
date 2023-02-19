# Query Proxy

PuffinDB is built upon the strong belief that [SQL](https://en.wikipedia.org/wiki/SQL) is [Cloud Data](../CLOUD.md)'s preferred language, and that countless wonderful SQL query generators like [Malloy](https://www.malloydata.dev/), [PRQL](https://prql-lang.org/), [Baxter](https://baxterhq.com/), or [WHERE TRUE](https://www.wheretrue.com/) will emerge over the coming years. These will all share a common set of requirements that could be factorized by PuffinDB, with the intent of reducing R&D efforts for vendors, and delivering a better user experience for end users:

- Integration with the user's client
- Integration with the user's data lake
- SQL parsing | serializing
- [Scale-out and scale-up](../CLOUD.md#scale-out-and-scale-up)
- IP protection

## Architecture
To achieve these goals, PuffinDB is architected around two main components:
- A [DuckDB extension](Extension.md)
- A cloud-side **PuffinDB Proxy** operated by PuffinDB
- A cloud-side **Query Generator** running on the vendor's cloud or the end-user's VPC (depending on IP protection requirements)

The `puffindb` DuckDB extension is installed once by the end-user. From there, support for any number of query generators can be added from DuckDB's SQL API, using a public registry managed by PuffinDB. Once a query is submitted by the end-user through DuckDB's SQL API, it is sent to the cloud-side **PuffinDB Proxy**, which forwards it to the **Query Generator**. The query is then translated into SQL and sent back to the end-user's DuckDB engine for execution.
