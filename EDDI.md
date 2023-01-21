# Edge-Driven Data Integration

Edge-Driven Data Integration (EDDI) is an inversion of control proposed by the PuffinDB project. Its main idea is that data integration should be driven at the edge by dynamic user-driven integration scenarios, rather than on the cloud by static data integration pipelines, yet without sacrificing proper architecture design and data governance.

## Assumptions
EDDI assumes that organizations have a data lake with one or several lakehouses like [Apache Iceberg](https://iceberg.apache.org/), [Apache Hudi](https://hudi.apache.org/), or [Delta Lake](https://delta.io/). Why multiple lakehouses instead of just one? Because the real world is always more complex and messy than a pretty architecture diagram would like you to believe.
