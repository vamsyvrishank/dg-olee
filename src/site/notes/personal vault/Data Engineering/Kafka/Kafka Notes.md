---
{"dg-publish":true,"permalink":"/personal-vault/data-engineering/kafka/kafka-notes/"}
---

Apache Kafka is an event streaming platform.

Kafka combines three key capabilities so you can implement [your use cases](https://kafka.apache.org/powered-by) for event streaming end-to-end with a single battle-tested solution:

1. To **publish** (write) and **subscribe to** (read) streams of events, including continuous import/export of your data from other systems.
2. To **store** streams of events durably and reliably for as long as you want.
3. To **process** streams of events as they occur or retrospectively.

And all this functionality is provided in a distributed, highly scalable, elastic, fault-tolerant, and secure manner. Kafka can be deployed on bare-metal hardware, virtual machines, and containers, and on-premises as well as in the cloud. You can choose between self-managing your Kafka environments and using fully managed services offered by a variety of vendors.

#### How Does Kafka work in a nutshell

Kafka is a distributed system consisting of **servers** and **clients** that communicate via a high-performance [TCP network protocol](https://kafka.apache.org/protocol.html). It can be deployed on bare-metal hardware, virtual machines, and containers in on-premise as well as cloud environments.

#### Concepts and Important Terminology

##### 1. The Data

- **Event (or Message):** This is the actual piece of data flowing through Kafka. It represents something that happened in the real world or your system (e.g., a "buy" order on Binance, a user clicking a button, or a temperature reading). It usually consists of a key, a value (the actual data payload, often in JSON or Avro format), and a timestamp.

##### 2. Organizing the Data

- **Topic:** Think of a topic as a **category name, a folder, or a specific radio channel**. All messages of the same type go into the same topic. For example, you might have a topic named `binance-trades` and another named `user-logins`. Producers write to topics, and Consumers read from topics.
    
- **Partition:** This is the secret to Kafka's massive speed and scalability. A single topic can hold so much data that it won't fit on one server. So, Kafka breaks a topic into smaller chunks called **Partitions**. These partitions can be spread across multiple different servers.
    
- **Offset:** Inside a partition, every single message is assigned a unique, sequential ID number called an **Offset** (e.g., 0, 1, 2, 3...). This acts like a bookmark. When a Consumer reads data, it remembers its "offset" so if it crashes, it knows exactly where to resume reading without skipping or repeating messages.
##### 3. The Infrastructure (The Middleman)

- **Broker:** A single Kafka server. It is responsible for receiving messages, storing them on its hard drive, and serving them to consumers when requested.
    
- **Cluster:** A group of Brokers working together as one giant system. If you have 5 Kafka Brokers running together, that is a Kafka Cluster. It ensures that if one server dies, the others keep the system running.
##### 4. The Clients (The Senders and Receivers)

- **Producer:** The application that creates and sends (publishes) messages _to_ a Kafka topic. (e.g.,  Binance Market Feed Handler).
    
- **Consumer:** The application that requests and reads (subscribes to) messages _from_ a Kafka topic. (e.g., Your Trading Bot or Database).
    
- **Consumer Group:** A team of Consumers working together to read from a topic. If a topic has millions of messages pouring in, one Consumer might not be fast enough. You can spin up 5 Consumers in a "Consumer Group," and Kafka will automatically divide the work so each one reads a different partition of the topic.
##### 5. The Manager

- **ZooKeeper / KRaft:** Kafka clusters need a "manager" to keep track of which brokers are alive, which broker holds which partitions, and cluster configuration.
    
    - Historically, Kafka used a separate software tool called **ZooKeeper** to do this.
        
    - In newer versions of Kafka, this has been replaced by a built-in system called **KRaft** (Kafka Raft), making Kafka entirely self-managed.

**Summary in a sentence:** A **Producer** sends an **Event** to a specific **Topic**; the **Cluster** of **Brokers** stores that event inside a **Partition** and assigns it an **Offset**; finally, a **Consumer** (often part of a **Consumer Group**) reads that event at its own pace.


![Pasted image 20260316173124.png](/img/user/personal%20vault/Data%20Engineering/Kafka/attachments/Pasted%20image%2020260316173124.png)


Figure: This example topic has four partitions P1–P4. Two different producer clients are publishing, independently from each other, new events to the topic by writing events over the network to the topic’s partitions. Events with the same key (denoted by their color in the figure) are written to the same partition. Note that both producers can write to the same partition if appropriate.

To make your data fault-tolerant and highly-available, every topic can be **replicated** , even across geo-regions or datacenters, so that there are always multiple brokers that have a copy of the data just in case things go wrong, you want to do maintenance on the brokers, and so on. A common production setting is a replication factor of 3, i.e., there will always be three copies of your data. This replication is performed at the level of topic-partitions

This primer should be sufficient for an introduction. The [Design](https://kafka.apache.org/documentation/#design) section of the documentation explains Kafka’s various concepts in full detail, if you are interested.


#### Kafka API's

- The [Admin API](https://kafka.apache.org/documentation.html#adminapi) to manage and inspect topics, brokers, and other Kafka objects.
- The [Producer API](https://kafka.apache.org/documentation.html#producerapi) to publish (write) a stream of events to one or more Kafka topics.
- The [Consumer API](https://kafka.apache.org/documentation.html#consumerapi) to subscribe to (read) one or more topics and to process the stream of events produced to them.
- The [Kafka Streams API](https://kafka.apache.org/documentation/streams) to implement stream processing applications and microservices. It provides higher-level functions to process event streams, including transformations, stateful operations like aggregations and joins, windowing, processing based on event-time, and more. Input is read from one or more topics in order to generate output to one or more topics, effectively transforming the input streams to output streams.
- The [Kafka Connect API](https://kafka.apache.org/documentation.html#connect) to build and run reusable data import/export connectors that consume (read) or produce (write) streams of events from and to external systems and applications so they can integrate with Kafka. For example, a connector to a relational database like PostgreSQL might capture every change to a set of tables. However, in practice, you typically don’t need to implement your own connectors because the Kafka community already provides hundreds of ready-to-use connectors.

#### Kafka Use cases

**Messaging & Event Streaming**

- Decouples producers and consumers, replacing traditional message queues (RabbitMQ, ActiveMQ) with higher throughput and durability
- Pub/sub model where multiple consumers can independently read the same stream

**Real-Time Data Pipelines**

- Moves data reliably between systems (databases, data warehouses, lakes) with low latency
- Used as the backbone for ETL/ELT pipelines at scale

**Log Aggregation**

- Centralizes logs from distributed services into one place for monitoring, alerting, and analysis
- Common pairing: Kafka → Elasticsearch/Splunk

**Stream Processing**

- Powers real-time analytics and transformations using Kafka Streams or frameworks like Flink and Spark Streaming
- Examples: fraud detection, live dashboards, anomaly detection

**Event Sourcing**

- Stores state changes as an immutable log of events, which downstream systems replay to reconstruct state
- Natural fit for microservices architectures

**Change Data Capture (CDC)**

- Captures row-level changes from databases (via Debezium) and streams them to other systems
- Keeps caches, search indexes, and replicas in sync without tight coupling

**Metrics & Monitoring**

- Collects operational metrics from distributed systems in real time
- Feeds into time-series databases like InfluxDB or ClickHouse

**Financial & Trading Systems**

- Order flow, trade events, and market data distribution — relevant to your market data pipeline work with Kafka + KDB+
- Point-in-time correctness is critical here, which is exactly the problem you're solving

The common thread across all of these is **high-throughput, fault-tolerant, ordered event delivery** — Kafka's core strength over alternatives.


Great question. Short answer: **no, offsets are not lost**. Here's the full picture:

#### Data Retention

Kafka retains messages based on policies you configure — it is **not** a traditional queue where messages disappear after consumption.

### Two Retention Modes

**1. Time-based (default)**

```properties
log.retention.hours=168   # 7 days default
log.retention.minutes=...
log.retention.ms=...
```

Messages older than this are deleted regardless of whether they were consumed.

**2. Size-based**

```properties
log.retention.bytes=1073741824  # 1 GB per partition
```

When the partition log exceeds this size, oldest segments are deleted.

Both can be combined — whichever limit is hit first triggers deletion.

---

### Log Compaction (Special Mode)

Instead of deleting by time/size, Kafka can retain only the **latest value per key**:

```properties
log.cleanup.policy=compact
```

- Useful for event sourcing / CDC — you keep the latest state per entity
- Old records with the same key get garbage collected
- The log shrinks but **no key is ever fully lost**
- This is how `__consumer_offsets` itself works internally

---

### Retention Scope — Topic Level vs Broker Level

You can override retention per topic:

```bash
kafka-configs.sh --alter --topic my-topic \
  --add-config retention.ms=3600000  # 1 hour for this topic only
```

Broker-level configs are defaults; topic-level configs override them.

