# Future-Proofing

PuffinDB is designed to take advantage of the very latest serverless technologies (*C.f.* [Architecture](Architecture.md)), but we also anticipate future developments:

- Serverless containers ([AWS Fargates](https://aws.amazon.com/fargate/)) that could start in 10s instead of 60 to 90s.
- Serverless functions ([AWS Lambda](https://aws.amazon.com/lambda/) that could "officially" be used in a stateful manner.
- Serverless containers and functions with GPU support.
- In-memory tier for Object Store ([Amazon S3](https://aws.amazon.com/s3/))
- [DuckDB](https://duckdb.org/) embedded within Object Store (as alternative to [`SelectObjectContent`](https://docs.aws.amazon.com/AmazonS3/latest/API/API_SelectObjectContent.html))
