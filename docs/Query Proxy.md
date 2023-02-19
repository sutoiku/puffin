# Query Proxy

PuffinDB is built upon the strong belief that [SQL](https://en.wikipedia.org/wiki/SQL) is [Cloud Data](../CLOUD.md)'s preferred language, and that countless wonderful SQL query generators like [Malloy](https://www.malloydata.dev/), [PRQL](https://prql-lang.org/), [Baxter](https://baxterhq.com/), or [WHERE TRUE](https://www.wheretrue.com/) will emerge over the coming years. These will all share a set of common requirements that could be factorized by PuffinDB, with the intent of reducing R&D efforts for vendors, and delivering a better user experience for end users:

- Integration with the user's client
- Integration with the user's data lake
- SQL parsing | serializing
- [Scale-out and scale-up](../CLOUD.md#scale-out-and-scale-up)
- IP protection
