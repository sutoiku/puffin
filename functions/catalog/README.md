# Catalog

The **Catalog** is a [Java](https://en.wikipedia.org/wiki/Java_(programming_language)) serverless function exposing the [Lakehouse Catalog](https://iceberg.apache.org/docs/latest/spark-configuration/#catalogs). It embeds [Iceberg's Java API](https://iceberg.apache.org/docs/latest/api/).

**Note**: The Catalog and [Engine](../engine/README.md) run on two separate serverless functions because the former uses Java while the latter uses Rust.
