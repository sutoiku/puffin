# Edge-Driven Data Integration

Edge-Driven Data Integration (EDDI) is an inversion of control proposed by the PuffinDB project. Its main idea is that data integration should be driven at the edge by dynamic user-driven integration scenarios, rather than on the cloud by static data integration pipelines, yet without sacrificing proper architecture design and data governance.

## Assumptions
EDDI assumes that most organizations have a data lake with one or several lakehouses like [Apache Iceberg](https://iceberg.apache.org/), [Apache Hudi](https://hudi.apache.org/), or [Delta Lake](https://delta.io/). Why multiple lakehouses instead of just one? Because the real world is always more complex and messy than a pretty architecture diagram would like you to believe. And because competitive markets cannot tolerate single-player consolidation (for good reasons).

EDDI assumes that such a data lake is powered by an infinitely elastic Object Store ([Amazon S3](https://aws.amazon.com/s3/), [Azure Blob Storage](https://azure.microsoft.com/en-us/products/storage/blobs), [Google Cloud Storage](https://cloud.google.com/storage)) capable of handling a virtually unlimited number of concurrent connections for read queries.

Finally, EDDI assumes that such a data lake is surrounded by a wide range of complementary query engines, themselves being also elastically scalable and capable of handling a virtually unlimited number of concurrent connections for read queries, on datasets of virtually unlimited size. Some of these engines excel with relational queries, others with graph-oriented queries, others with queries on time series, and others with queries on geospatial data. Each brings a unique set of optimization techniques and query syntaxes, but all share a common query protocol, making it possible to perform joins across multiple large datasets with excellent performance.

## Question
The question then becomes: **how should we express and execute queries involving multiple datasets made of multiple tables managed by multiple lakehouses, and queried across multiple query engines?** We can either adopt a conventional cloud-centric approach with static data ingration pipelines, or embrace a radical edge-driven approach with dynamic user-driven integration scenarios.

EDDI advocates for the latter.

## Answer
The answer consists in leveraging the tens or hundreds of millions of powerful query engines that will very soon be found at the edge, following the explosive adoption of [DuckDB](https://duckdb.org/). But instead of leveraging them for their local querying capabilities, we should leverage them for their ability to originate queries that will be executed on the cloud, in a massively-distributed fashion. There is a very good reason for that: data is getting heavier and heavier, and the more it does, the less it can escape the cloud's gravitational pull. One should not think in terms of **big data** anymore. Instead, one should think in terms of **heavy data**.

## Implementation
While the vision outlined above might seem very ambitious, it could be implemented with a relatively-simple extension to the SQL syntax:

`SELECT REMOTE 'https://myPuffinService.com/' * FROM myRemoteTable;`

With that syntax, `myRemoteTable` would be local to `https://myPuffinService.com/`, which itself would be nothing more than an HTTP endpoint exposing a query engine's API. Initially, this query engine would use the SQL syntax, but it should be possible to support alternative query syntaxes down the road, such as the upcoming [Graph Query Language](https://www.gqlstandards.org/) (GQL), while offering the ability to nest one into the other, in both correlated and uncorrelated manners.
