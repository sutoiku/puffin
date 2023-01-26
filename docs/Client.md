# Client

From the client-side (browser, standalone application, or online service), PuffinDB can be used through any HTTP client or through [DuckDB](https://duckdb.org/).

## DuckDB Extension
PuffinDB will provide a simple extension that can be added to any DuckDB engine, directly from the SQL API:

```
INSTALL 'puffindb';
LOAD 'puffindb';
```

This extension will implement the `SELECT THROUGH` syntax:

```
SELECT THROUGH 'https://queryEngine.com/' * FROM remoteTable;
```

With that syntax, `remoteTable` is local to `https://queryEngine.com/`, which itself is nothing more than an HTTP endpoint exposing a query engine's API. Initially, this query engine will only use the SQL syntax, but it should be possible to support complementary query syntaxes down the road, such as the upcoming [Graph Query Language](https://www.gqlstandards.org/) (GQL), while offering the ability to nest one into the other, in both correlated and uncorrelated fashions.

While similar results could be achieved with alternative syntaxes, using a `SELECT THROUGH` statement would allow this kind of query:

```
SELECT *
  FROM
    localTable AS local,
    (SELECT THROUGH 'https://queryEngine.com/' * FROM remoteTable) AS remote
  WHERE local.key = remote.key;
```

And if the remote query engine were to support this syntax as well, we would gain some kind of **Cascading Query Language** (CQL):

```
SELECT *
  FROM
    localTable AS local,
    (SELECT THROUGH 'https://firstRemoteEngine.com/' *
        FROM
          firstTable AS first,
          (SELECT THROUGH 'https://secondRemoteEngine.com/' * FROM secondTable) AS second
        WHERE first.key = second.key
    ) AS remote
  WHERE local.key = remote.key;
```

In this example, the client calls a first remote query engine, which in turns calls a second remote query engine, hence the cascade.
