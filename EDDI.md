# Edge-Driven Data Integration

Edge-Driven Data Integration (EDDI) is an inversion of control proposed by the PuffinDB project. Its main idea is that data integration should be driven at the edge by dynamic user-driven integration scenarios, rather than on the cloud by static data integration pipelines, yet without sacrificing proper architecture design and data governance.

## Assumptions
EDDI assumes that most organizations have a data lake with one or several lakehouses like [Apache Iceberg](https://iceberg.apache.org/), [Apache Hudi](https://hudi.apache.org/), or [Delta Lake](https://delta.io/). Why multiple lakehouses instead of just one? Because the real world is always more complex and messy than a pretty architecture diagram would like you to believe. And because competitive markets cannot tolerate single-player consolidation (for good reasons).

EDDI assumes that such a data lake is powered by an infinitely elastic Object Store ([Amazon S3](https://aws.amazon.com/s3/), [Azure Blob Storage](https://azure.microsoft.com/en-us/products/storage/blobs), [Google Cloud Storage](https://cloud.google.com/storage)) capable of handling a virtually unlimited number of concurrent connections for read queries.

Finally, EDDI assumes that such a data lake is surrounded by a wide range of complementary query engines, themselves being also elastically scalable and capable of handling a virtually unlimited number of concurrent connections for read queries, on datasets of virtually unlimited size. Some of these engines excel with relational queries, others with graph-oriented queries, others with queries on time series, and others with queries on geospatial data. Each brings a unique set of optimization techniques and query syntaxes, but all share a common query protocol.
