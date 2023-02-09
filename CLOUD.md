# Cloud Data

Data lives in the cloud, it has weight, and PuffinDB is the **Cloud Data Engine** working against the cloud's gravitational pull.

## Data Weight
When thinking about data, one should not think in terms of small data or big data. This volumetric classification isn't really helpful anymore. Instead, one should think in terms of **weight**. Data has mass (the larger the dataset, the larger its mass), and lives within a cloud that exerts a gravitational pull (weight) on the data, with a fixed [gravitational constant](https://en.wikipedia.org/wiki/Gravitational_constant). The larger data gets, the stronger a gravitational pull is exerted on it by the cloud it resides in. According to Newton's second law of motion:

```
W = m·g
```

Following this analogy, a cloud's gravitational constant `g` is a factor of internal bandwidth (how fast can you move data from S3 to EC2), external bandwidth (how fast can you download data from the cloud to your client computer), and data egress cost (how much does it cost to do the latter). Of course, like any other analogy, ours is imperfect, but it should be helpful to illustrate certain important points.

## Data at Rest
At rest, data lives in a Lake ([Iceberg](https://iceberg.apache.org/), [Delta Lake](https://delta.io/), [Hudi](https://hudi.apache.org/)) backed by an Object Store ([Amazon S3](https://aws.amazon.com/s3/), [Azure Blob Storage](https://azure.microsoft.com/en-us/products/storage/blobs), [Google Cloud Storage](https://cloud.google.com/storage)). According to our analogy, it is laying still on Earth's surface, wasting very little energy to do so (translation: object stores are dirt cheap).

## Data in Motion
Because the object store's advent established a clear separation between storage and compute, data cannot be queried or updated directly from the object store. Instead, it needs to be moved from the object store to a compute node (serverless function, serverless container, container, or client), then queried or updated from there.

This data movement is expensive, for two mains reasons: first, storage and compute capacities increase faster than network bandwidth; second, bits cannot move faster than the speed of light (they move quite a bit slower actually, and never in a straight line). This means that **bandwidth** and **latency** are a cloud's main limiting factors. They are facts of life that we must learn to live with.

With that in mind, one could think of moving data from the object store to a compute node within the same [availability zone](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.RegionsAndAvailabilityZones.html) as taking a flight from Paris to Tokyo. It is no doubt expensive, but nowhere near as much as putting a satellite on orbit around the earth, or going to Mars.
