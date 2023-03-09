# Persistence

PuffinDB uses multiple engines and serverless services for persistence and transient state management:

- **Table Metadata**: Lakehouse Catalog
- **Table Summaries**: Object Store
- **Edits**: Key-Value Store
- **Sharings**: Key-Value Store
- **Persistent Logs**: Block Storage during [Monostore](Monostore.md)'s life, then Object Store
- **Table Caches Metadata**: In-Memory Metastore
- **Transient Logs**: In-Memory Metastore

Some logs are both persistent and transient (sequences and steps for [data pipelines](Pipeline%20Engine.md)).
