# Query Proxy

PuffinDB is built upon the strong belief that [SQL](https://en.wikipedia.org/wiki/SQL) is [Cloud Data](../CLOUD.md)'s preferred language, and that countless wonderful SQL query generators like [Malloy](https://www.malloydata.dev/), [PRQL](https://prql-lang.org/), [Baxter](https://baxterhq.com/), or [WHERE TRUE](https://www.wheretrue.com/) will emerge over the coming years. Furthermore, this trend will be accelerated by the advent of large [language models](https://en.wikipedia.org/wiki/Language_model) like OpenAI's [ChatGPT](https://openai.com/blog/chatgpt/). These query generators will all share a common set of requirements that could be factorized by PuffinDB, with the intent of reducing R&D efforts for vendors, and delivering a better overall experience to users:

- Integration with the user's client
- Integration with the user's data lake
- SQL parsing | serializing
- [Scale-out and scale-up](../CLOUD.md#scale-out-and-scale-up)
- IP protection

## Architecture
To achieve these goals, query generation is architected around four main components:
- A client-side [DuckDB extension](Extension.md)
- A cloud-side **Query Proxy** operated by PuffinDB
- A cloud-side **Query Generator** running on the vendor's cloud or the user's VPC (depending on IP protection requirements)
- A cloud-side **Registry** of query generators managed by PuffinDB

The `puffindb` DuckDB extension is installed once by the user. From there, support for any number of query generators can be added from DuckDB's SQL API, using the public **Registry** managed by PuffinDB. Once a query is submitted by the user through DuckDB's SQL API, it is sent to the cloud-side **Query Proxy**, which forwards it to the **Query Generator**. The query is then translated into SQL and sent back to the user's DuckDB engine for execution.

Several options are currently being considered for the packaging of the Query Generator. One of them is to use [WebAssembly](https://webassembly.org/), which would allow both client-side and cloud-side execution. This proposed meta-extension mechanism would also make it easier to develop certain DuckDB extensions, without having to compile them for different platforms (to be confirmed).

## Benefits for Vendors
- No need to develop and distribute any proprietary DuckDB extension
- No need to develop yet another SQL parser | serializer
- Access to the metadata of tables managed by the most popular data lakes ([Apache Iceberg](https://iceberg.apache.org/), [Apache Hudi](https://hudi.apache.org/), [Delta Lake](https://delta.io/))
- Direct integration with the user's VPC
- [Scale-out and scale-up](../CLOUD.md#scale-out-and-scale-up) of complex queries through [distributed SQL engine](Query%20Engine.md)
- IP protection when running query generator on vendor's cloud

## Benefits for Users
- No need to install one DuckDB extension for every query generator
- Data and metadata confidentiality when running query generator on user's VPC
- Cloud-side acceleration of query generation and query optimization
