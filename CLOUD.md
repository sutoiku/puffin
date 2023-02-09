# Cloud Data

Data lives in the cloud and has weight. A **Cloud Data Engine** must work against the cloud's gravitational pull.

## Data Weight
When thinking about data, one should not think in terms of small data or big data. This volumetric classification isn't really helpful anymore. Instead, one should think in terms of **weight** (`W`). Data has **mass** (the larger the dataset, the larger its mass `m`), and lives within a cloud that exerts a gravitational pull (weight) on it, with a fixed [gravitational constant](https://en.wikipedia.org/wiki/Gravitational_constant) `g`. The larger data gets, the stronger the gravitational pull. According to Newton's second law of motion: `W = m·g`

Following this analogy, a cloud's gravitational constant is a factor of its internal bandwidth (how fast can you move data from an object store to a compute engine), its external bandwidth (how fast can you download data from the cloud to your client computer), and its egress cost (how much does it cost to do the latter). Of course, like any other analogy, ours is imperfect, but it should be helpful to illustrate certain important points that follow.

## Data at Rest
At rest, data lives in a Lake ([Iceberg](https://iceberg.apache.org/), [Delta Lake](https://delta.io/), [Hudi](https://hudi.apache.org/)) backed by an Object Store ([Amazon S3](https://aws.amazon.com/s3/), [Azure Blob Storage](https://azure.microsoft.com/en-us/products/storage/blobs), [Google Cloud Storage](https://cloud.google.com/storage)). According to our analogy, it is laying still on Earth's surface, wasting very little energy to do so (translation: object stores are dirt cheap).

## Data in Motion
Because the object store's advent established a clear separation between storage and compute, data cannot be queried or updated directly from the object store (with the exception of trivial queries done with [`SelectObjectContent`](https://docs.aws.amazon.com/AmazonS3/latest/API/API_SelectObjectContent.html)). Instead, it needs to be moved from the object store to a compute node (serverless function, serverless container, container, or client), then queried or updated from there.

This data movement is expensive, for two mains reasons: first, storage and compute capacities increase faster than network bandwidth; second, bits cannot move faster than the speed of light (they move quite a bit slower actually, and never in a straight line). This means that **bandwidth** and **latency** are a cloud's main limiting factors. These are facts of life that we must learn to live with.

With that in mind, one could think of moving data from the object store to a compute node within the same [availability zone](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.RegionsAndAvailabilityZones.html) as taking a flight from Paris to Tokyo. It is no doubt expensive, but nowhere near as much as putting a satellite on orbit around the Earth, or going to Mars. Following our analogy, moving data from the object store to a [Content Delivery Network](https://en.wikipedia.org/wiki/Content_delivery_network) (CDN) for edge computing is akin to putting a satellite on low-Earth orbit, while downloading a full dataset from the object store to your laptop computer for local computing is a bit like establishing a human colony on Mars. It probably can be done, but is it really worth the expense?

Now, can we justify this analogy in any quantitative manner? Absolutely!

If we assume our Paris-Tokyo flight to cost $1,000 and our payload (passenger and luggage) to weigh 100kg, we get a cost of $10/kg. As a point of comparison, SpaceX's [Falcon Heavy](https://www.spacex.com/vehicles/falcon-heavy/) offers low-Earth orbit deliveries for a cool $1,400/kg, which is 140 times more expensive still. And current missions to Mars cost about $200,000/kg, or 142 times more that low-Earth orbit deliveries. In other words, each jump is about two orders of magnitude more expensive than the previous one. Granted, SpaceX promises Starship missions to Mars for as little as $50/kg, but that remains to be seen, and low-cost carriers routinely take passengers across continents for less than $50. Furthermore, sending you and your suitcase to Mars is one thing, but sending along all the equipment and supplies necessary for your survival once you get there is another thing altogether. You get the point...

From the hotel room where I am writing this article, I enjoy (suffer from) a bandwidth of just over 60 Mbps. As a point of comparison, a [`p4d.24xlarge`](https://aws.amazon.com/ec2/instance-types/p4/) instance gives me 400 Gbps of bandwith, which is 6,667 times greater. And the 3,000 Lambda functions that I can provision within a second or less give me an aggregated bandwidth from S3 of 2.4 Tbps, or 40,000 times greater. Therefore, if going to Mars is 20,000 times more expensive than flying from Paris to Tokyo, downloading a dataset to your laptop is indeed akin to going to Mars...

## Data Caching
As a result, since we cannot bring compute to data (until object stores get [upgraded](docs/Future-Proofing.md) with a full SQL engine like [DuckDB](https://duckdb.org/)), we need to bring data to compute, but we need to do so at the lowest possible cost. This is called **caching**, and it is the most important feature of any distributed database engine. Now, when we write about **cost**, we mean two things: first, money; second, latency, because time is money. Said another way, I want to query (or update) my dataset as quickly as possible, for as little money as possible.

As explained earlier, data at rest lives in the object store, therefore will have to be moved from there to some compute engine. We could use a traditional container for that (*e.g.* [EC2](https://aws.amazon.com/ec2/) instance), but its bandwidth is limited (400 Gbps at most). Instead, we are probably better off using a fleet of serverless functions (*e.g.* [AWS Lambda](https://aws.amazon.com/lambda/)): 3,000 of them can be provisioned within less than a second, and offer an aggregated bandwidth from S3 of 2.4 Tbps (6 times more). This is called **scale out** (we will come back to that later in the article).

But when a query is made on a dataset (or a particular subset of a dataset) by a data analyst, it is quite likely that another query will be made on the same dataset soon after. Therefore, we will want to cache onto our compute engine(s) the data that we have just moved from the object store. This is very easy when using a traditional container, but it can also be done using serverless functions and containers.

From there, we will soon realize that scaling out queries across multiple compute nodes works well for certain queries or certain query parts, but is much more difficult for others. Therefore, we will also want to involve one bigger compute engine (we like to call it a **monostore**) that can be used to cache large amounts of data and process really complex queries against them. For example, Amazon Web Services now offer their [`u-24tb1.112xlarge`](https://aws.amazon.com/ec2/instance-types/high-memory/) instance on demand, with 448 vCPUs and 24 TB of RAM.

Unfortunately, it only comes with 100 Gbps of network bandwidth (4 to 8 times more would be more appropriate), which means that filling its RAM with data would take at least 1,920 seconds (32 minutes). Even if you load data compressed at a 5× factor and cache it uncompressed in memory, you will need to wait 384 seconds (over 6 minutes) for the full dataset to be downloaded. And when you do so, you will want to scale out data downloads from the object store to the monostore by using a fleet of serverless functions in between, especially if data can be filtered at the source (*i.e.* **filter pushdown**).

Then, you will realize that caching data on the monostore could be done at four different levels:

1. Compressed on local solid state drives ([NVMe](https://en.wikipedia.org/wiki/NVM_Express) ideally)
2. Compressed in CPU memory
3. Uncompressed in CPU memory
4. Uncompressed in GPU memory (if you are lucky enough to use a GPU-accelerated monostore like the [`p4de.24xlarge`](https://aws.amazon.com/ec2/instance-types/p4/))

After that, you might consider caching closer to their users small to medium datasets (*Cf.* [Dataset Sizes](https://github.com/stoic-doc/Community/discussions/905)) that are used very frequently, using a [Content Delivery Network](https://en.wikipedia.org/wiki/Content_delivery_network) (CDN) and some edge serverless functions for querying. And only when you have exhausted all these options should you consider caching datasets all the way to the client. Welcome to Mars!

This incremental caching of data closer and closer to the client and the distribution of queries across layers of caching is called **scale up**.

## Scale Out and Scale Up
In order to take full advantage of these complementary **scale out** and **scale up** strategies, the [distributed query engine](docs/Query%20Engine.md) will need a very powerful [distributed query planner](docs/Query%20Planner.md). But make no mistake: whether you scale out, scale up, or both, you will need a distributed query planner. And scale up with a monostore without scale out using serverless functions for filter pushdown will deliver poor performance, because the monostore is limited by its network bandwidth, as explained earlier. Therefore, if anyone is selling you scale up as an alternative to scale out, please do not fall for it: this pitch is either shortsighted or misleading.

## Data Analytics on Laptop
You desktop or laptop computer is part of the overall cloud, as a client. But as explained earlier, this client is as far from your cloud data as Mars is from Earth, and moving data to your client is really expensive. By the same token, you are likely to get introduced to [DuckDB](https://duckdb.org/)'s goodness through a client application, be it [Jupyter](https://jupyter.org/), [Posit](https://posit.co/), or countless others. And when you do, you will want to do **less** analytics on your latop rather than **more**, for several reasons:

First, moving data from your laptop to the cloud will make it a lot easier to share your work with others, especially when using DuckDB in combination with a data lake ([Iceberg](https://iceberg.apache.org/), [Delta Lake](https://delta.io/), [Hudi](https://hudi.apache.org/)) and | or a git-based collaboration platform ([GitHub](https://github.com/), [GitLab](https://about.gitlab.com/), [BitBucket](https://bitbucket.org/product)).

Second, contrary to what some might want you to believe, your laptop is **not** faster than your data warehouse. Unless you are stuck with decade-old data warehouse technology, the cloud is orders of magnitude more powerful than your laptop will ever be. And even if you were to switch to a beefed-up desktop computer, I highly doubt that it packs 448 vCPUs, 24 TB of RAM, or 8× NVIDIA A100 GPUs. But if it does, I sure hope that you and your roommates have good ear protection...

Third, contrary to what you might have been told, the cloud is a lot more reliable than your laptop. For sure, every cloud service will fail, and any good cloud application developer knows how to design around this default failure mode. But I have yet to forget my cloud data center on an airliner seat back pocket, or get my cloud data center burglarized from my car, or break my cloud data center because I spilled coffee all over it. Let us be real here...

Fourth, if someone told you that your laptop is more secure than your cloud, you might want to exert some reasonable critical judgement. Considering the number of security experts constantly monitoring best-in-class cloud services like AWS or Azure, your laptop is at a much greater risk of being hacked than these cloud services. In 25 years of working with enterprise customers, I have only met one who had a different opinion: it was an oil and gas company, and their new oil field data was only accessible from dekstop computers, locked in a highly-secured room. But these were never connected to the Internet. And this type of security strategy probably involves less than 100,000 poor souls around the world. If you are part of the 99.998% remaining online population, you should consider the cloud as your data vault, and this is probably how your CISO sees it as well...

DuckDB is growing on the client very rapidly, but it will spread across the cloud even faster.

## Clientless Architecture
The beauty of [DuckDB](https://duckdb.org/) is that you will soon find it on every client, embedded within all kinds of applications, from [Jupyter](https://jupyter.org/) and [Posit](https://posit.co/) to [Tableau](https://www.tableau.com/) and [Excel](https://www.microsoft.com/en-us/microsoft-365/excel). Therefore, make sure to select a Cloud Data Engine that is designed to be [clientless](docs/Clientless.md), in the sense that it works with any client embedding DuckDB, through a simple extension installed from the SQL API. And resist the temptation of using a client that comes with your Cloud Data Engine. Instead, work with your client application's vendor to improve its integration with your Cloud Data Engine.

## Dataset Size
Datasets come in all sizes. Here is how we like to think about them:

| Code | Size | Size Range | Interactivity | Mult. Func. | Cloud | Basic Part. | Container | Adv. Part. | Cluster |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **XS** | 1 MB | 0-10 MB | ✅ | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; |
| **S** | 100 MB | 10 MB - 1 GB | ✅ | ✅ | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; |
| **M** | 10 GB | 1 GB - 100 GB | ✅ | ✅ | ✅ | ✅ | ✅ | &nbsp; | &nbsp; |
| **L** | 1 TB | 100 GB - 10 TB | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | &nbsp; |
| **XL** | 100 TB | 10 TB - 1 PB | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |

- **Interactivity**: Simple queries complete within 2 seconds or less.
- **Multiple Functions**: Requires multiple serverless functions for initial scan.
- **Cloud**: Requires cloud-side processing.
- **Basic Partitions**: Datasets must be partitioned.
- **Advanced Partitions**: Partitions must be defined from actual table columns.
- **Container**: Requires the use of at least one container (serverless functions are not enough).
- **Cluster**: Requires the use of a cluster of containers.

And while it is true that most organizations **produce** datasets that are medium in size or smaller (less than 100 GB), anyone who is online (5.16 B at last count) **consumes** datasets that are much larger. For example, when I make a search on Google Search, I query a dataset that is larger than 100,000,000 gigabytes (that's 100 PB). And while most such queries are handled by a handful of very large online service providers, more and more organizations must directly query third-party datasets that are measured in terabytes or even petabytes, and this trend will be accelerated by two major factors:

First, the explosion of Artificial Intelligence is fueling the need for larger and larger datasets. For example, [GPT-3](https://openai.com/blog/gpt-3-apps/) has 175 billion parameters, and GPT-4 will have 100 trillion of them. While very few organizations will be able to run such very large models at scale, countless ones will run GPT-3-class models for countless users. This is a major opportunity for a serverless Cloud Data Engine.

Second, climate risk is slowly but surely affecting all of us, forcing organizations to implement more and more complex climate risk management strategies. This is especially true for banks and insurance companies, but anyone involved in planning and forecasting will need to up their game, for a simple reason: while climate disasters fueled by climate change tend to have a direct yet short-lived impact, indirect supply-chain disruptions have long-lasting effects. For example, regional conflicts are more and more attributable to the impact of climate change, and these can have consequences lasting decades. Organizations of all sizes and operating across virtually every industry will need to develop more and more sophisticated models for planning and remediating such disruptive events.

Hadoop-style Big Data is dead and should rest in peace. But datasets will keep getting larger and larger, whether we like it or not.

## Virtual Private Cloud
When Amazon launched Amazon Web Services back in 2006, [Virtual Private Clouds](https://aws.amazon.com/vpc/) (VPC) were not available (released in 2009), and neither were provisioning tools like [CloudFormation](https://aws.amazon.com/cloudformation/) (released in 2011). But in 2023, they work really well, and there is no reason for anyone to cede control of their data to any database vendor. Deploying your Cloud Data Engine on your VPC brings the following benefits:

- Lower cost (when running mostly serverless)
- Better billing options (using credits pre-negotiated with your cloud provider)
- Better chargeback visibility (with query-level cost monitoring when running mostly serverless)
- Higher security and stronger confidentiality (one less actor to trust)
- More elasticity (no reliance on a single vendor operating on top of the cloud infrastructure)
- More regional options (deploy in any region supported by your cloud provider)
- More control (customize the provisioning templates to better suit your specific requirements)

## Conclusions
- Data is heavy, so keep it in the cloud where it belongs, across as many layers of caching as possible.
- Scale out and scale up, using serverless options whenever possible.
- Embrace the data lake, and use your laptop for what it was designed: a portable user interface.
- Don't limit yourself to a particular client, go clientless.
- Don't limit yourself to small datasets, large datasets are becoming more and more prevalent.
- Do not give your data to a database vendor, demand to run your Cloud Data Engine on your Virtual Private Cloud.

Be stoic, be kind, be cool. Like a puffin...
