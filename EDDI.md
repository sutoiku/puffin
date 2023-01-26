# Edge-Driven Data Integration

Edge-Driven Data Integration (EDDI) is an inversion of control proposed by the [PuffinDB](http://PuffinDB.io) project. Its main idea is that data integration should be driven at the edge by dynamic user-driven integration scenarios, rather than on the cloud with static data integration pipelines, yet without sacrificing solid architecture design and proper data governance.

## Assumptions
EDDI assumes that most organizations have a data lake with one or several lakehouses like [Apache Iceberg](https://iceberg.apache.org/), [Apache Hudi](https://hudi.apache.org/), or [Delta Lake](https://delta.io/). Why multiple lakehouses instead of just one? Because the real world is always more complex and messy than a pretty architecture diagram would like you to believe. And because competitive markets cannot tolerate single-player consolidation (for good reasons).

EDDI assumes that such a data lake is powered by an infinitely elastic Object Store ([Amazon S3](https://aws.amazon.com/s3/), [Azure Blob Storage](https://azure.microsoft.com/en-us/products/storage/blobs), [Google Cloud Storage](https://cloud.google.com/storage)) capable of handling a virtually unlimited number of concurrent object scans for read queries.

Finally, EDDI assumes that such a data lake is surrounded by a wide range of complementary query engines, themselves being also elastically scalable and capable of handling a virtually unlimited number of concurrent connections for read queries, on datasets of virtually unlimited sizes. Some of these engines excel with relational queries, others with graph-oriented queries, others with queries on time series, and others with queries on geospatial data. Each brings a unique set of optimization techniques and query syntaxes, but all share a common query protocol, making it possible to perform joins across multiple large datasets with excellent performance.

## Question
The question then becomes: **how should we express and execute queries involving multiple datasets made of multiple tables managed by multiple lakehouses, and queried across multiple query engines?** We can either adopt a conventional cloud-centric approach with static data integration pipelines, or embrace a radical edge-driven approach with dynamic user-driven integration scenarios.

Think pipeline *vs.* tanker — EDDI advocates for the latter.

## Answer
The answer consists in leveraging the tens of millions of powerful query engines that will very soon be found at the edge, following the explosive adoption of [DuckDB](https://duckdb.org/). But instead of leveraging them for their local querying capabilities, we should leverage them for their ability to originate complex queries that will be executed on the cloud for the most part, in a massively-distributed fashion. There is a very good reason for that: data is getting heavier and heavier, and the more it does, the less it can escape the cloud's gravitational pull.

One should not think in terms of **big data** anymore. Instead, one should think in terms of **heavy data**.

## Implementation
While the vision outlined above might seem very ambitious, it could be implemented with a relatively-simple extension to the SQL syntax:

```
SELECT THROUGH 'https://queryEngine.com/' * FROM remoteTable;
```

With that syntax, `remoteTable` is local to `https://myPuffinDB.com/`, which itself is nothing more than an HTTP endpoint exposing a query engine's API. Initially, this query engine will only use the SQL syntax, but it should be possible to support complementary query syntaxes down the road, such as the upcoming [Graph Query Language](https://www.gqlstandards.org/) (GQL), while offering the ability to nest one into the other, in both correlated and uncorrelated fashions.

While similar results could be achieved with alternative syntaxes, using a `SELECT THROUGH` statement would allow this kind of query:

```
SELECT *
  FROM
    localTable AS local,
    (SELECT THROUGH 'https://myPuffinDB.com/' * FROM remoteTable) AS remote
  WHERE local.key = remote.key;
```

And if the remote query engine were to support this syntax as well, we would gain some kind of **Cascading Query Language** (CQL):

```
SELECT *
  FROM
    localTable AS local,
    (SELECT THROUGH 'https://myFirstPuffinDB.com/' *
        FROM
          firstTable AS first,
          (SELECT THROUGH 'https://mySecondPuffinDB.com/' * FROM secondTable) AS second
        WHERE first.key = second.key
    ) AS remote
  WHERE local.key = remote.key;
```

In this example, the client calls a first remote query engine, which in turns calls a second remote query engine, hence the cascade.

## Components
For this architecture to work, we need two things:
1. The client-side query engine and first remote query engine must support this new `SELECT THROUGH` syntax.
2. Both remote query engines must expose themselves as HTTP endpoints.

The former will require a fairly sophisticated query planner, while the latter will just need a relatively-simple protocol. For performance and scalability reasons, this protocol should support both synchronous and asynchronous requests. With a synchronous request, the query's result should be returned as a simple response to the HTTP request. With an asynchronous request, the response should just include an Object Store URI from which the query's result could be fetched at a later time (with polling or notification).

This architecture should be based on [Apache Arrow Flight SQL](https://arrow.apache.org/docs/format/FlightSql.html).

## Bottomline
PuffinDB's primary goal is to execute on that vision.
