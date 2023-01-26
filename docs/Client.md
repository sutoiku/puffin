# Client

From the client-side (browser, standalone application, or online service), PuffinDB can be used through any HTTP client or through [DuckDB](https://duckdb.org/).

## DuckDB Extension
PuffinDB will provide a simple extension that can be added to any DuckDB engine (client-side or cloud-side), directly from the SQL API:

```
INSTALL 'puffindb';
LOAD 'puffindb';
```

This extension will implement the `SELECT THROUGH` syntax:

```
SELECT THROUGH 'https://myPuffinDB.com/' * FROM remoteTable;
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

## Scheduled Remote Data Fetching and Local Caching
Personal computers are becoming increasingly powerful, and DuckDB offers great performance on datasets that are 10GB to 100GB in size. Unfortunately, downloading that much data from the cloud to the local client can take a long time, especially if such large datasets change on a regular basis (*e.g.* Yesterday's transactions). Fortunately, if the user's local DuckDB client can be invoked through some kind of API by another client-side application like a scheduler, the user can easily create a schedule that would automatically trigger the download of large datasets at night, allowing interactive queries on local data during the day.

If remote data is available as files, it can be fetched through the standard `httpfs` extension. If it is available as files that need to be filtered, it can be fetched using a standard `SELECT` statement. And if it requires a complex query that must be excuted cloud-side, it can be fectched using the proposed `SELECT THROUGH` syntax. Either way, this mechanism will work out of the box, on any platform (Linux, MacOS, Windows), with any client (*e.g.* Excel, Jupyter, RStudio).
