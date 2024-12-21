# Kafka

Kafka is a distributed event streaming platform used for building real-time data pipelines and streaming applications. It is designed to handle large volumes of data at high throughput, with low latency and fault tolerance. Kafka allows you to publish, subscribe to, store, and process streams of records (or messages) in real time.

Apache Kafka is the open-source, distributed version of Kafka, developed by the Apache Software Foundation used for building real-time data pipelines and streaming applications. 
<br>

Here are several reasons why Kafka is widely used in modern data-driven systems:
## 1. **High Throughput**
Kafka can handle a large volume of data with high throughput, making it suitable for applications that need to process large amounts of data quickly. It can handle millions of messages per second, both for publishing and subscribing.

## 2. **Low Latency**
Kafka provides low-latency message delivery, allowing for near real-time data processing. It ensures that messages are delivered with minimal delay, making it ideal for systems that require quick data updates, such as real-time analytics, monitoring, or notifications.

## 3. **Scalability**
Kafka is horizontally scalable, meaning it can scale to accommodate high throughput and large amounts of data by adding more broker nodes to the cluster. It can handle massive data streams without compromising performance.

## 4. **Durability**
Kafka ensures durability by persisting messages to disk, and it supports replication to prevent data loss. Even if a broker fails, Kafka guarantees that the messages are not lost, ensuring reliable message delivery.

## 5. **Fault Tolerance**
Kafka's architecture is designed for fault tolerance. It replicates data across multiple nodes, and in the event of a failure, it can recover and continue processing without loss of data. This ensures that your system remains available even in the face of hardware failures.

## 6. **Real-Time Processing**
Kafka is built to process data in real-time, which is ideal for use cases that require immediate insights or actions. With Kafka, you can stream data and process it in real-time as it arrives.

## 7. **Decoupling of Data Producers and Consumers**
Kafka provides a decoupled architecture where producers (who send data) and consumers (who process the data) do not need to know about each other. This enables independent scaling of producers and consumers and simplifies data integration across systems.

## 8. **Log Aggregation**
Kafka is often used for log aggregation, where log data from multiple sources (such as servers, microservices, and applications) is collected and made available for analysis and monitoring in real-time.

## 9. **Stream Processing**
Kafka enables powerful stream processing by allowing developers to write applications that process data as it flows through Kafka topics. This allows real-time transformation, aggregation, and filtering of data streams.

## 10. **Integration with Other Systems**
Kafka integrates well with a variety of data systems such as relational databases, NoSQL stores, and data lakes. It can also be used alongside stream processing frameworks like Apache Flink, Apache Spark, and Kafka Streams for more advanced data processing.

## 11. **Distributed and Highly Available**
Kafka is a distributed system, meaning it can run on multiple nodes across a network, ensuring that data is replicated and available even in the event of a node failure. This ensures high availability and resilience.

## Use Cases for Kafka

- **Real-Time Analytics**: Kafka is used in applications that need to perform real-time analytics on streaming data.
- **Data Pipelines**: Kafka enables the building of reliable and scalable data pipelines for transferring data between different services or systems.
- **Event Sourcing**: Kafka can be used to store and publish event logs that represent state changes in systems, allowing event-driven architectures.
- **Messaging System**: Kafka can act as a high-throughput, low-latency messaging system between services or components in a distributed architecture.

Kafka’s combination of performance, low latency, scalability, fault tolerance, and ease of use makes it a go-to tool for building modern data architectures and real-time data processing applications.



# Apache Kafka Local Setup Documentation

This guide provides detailed instructions for setting up Apache Kafka locally, managing topics, producing and consuming messages, and understanding Kafka’s key concepts.

---

## Prerequisites

1. **Java Development Kit (JDK):** Kafka requires Java. Ensure that you have JDK 8 or above installed. You can check your Java version with:

   ```bash
   java -version
   ```
![Java Version Image](https://github.com/kafleprakash96/kafka-docs/blob/screenshots/images/image1.png?raw=true)

2. **Kafka Installation:** Download the latest Kafka release from the [Apache Kafka website](https://kafka.apache.org/downloads) and extract the archive to a directory of your choice.

---

## Steps to Run Kafka Locally

### 1. Start Zookeeper


Kafka requires Zookeeper to coordinate distributed components.    
Zookeeper is a centralized service for maintaining configuration information, naming, providing distributed synchronization, and providing group services. It is used by Kafka to manage and coordinate the Kafka brokers.  
Use the following command to start Zookeeper:

```bash
./bin/zookeeper-server-start.sh config/zookeeper.properties
```

- **Configuration File:** `config/zookeeper.properties`
  - This file contains Zookeeper configuration, such as data directory and client port.
- **Default Zookeeper Port:** `2181`

### 2. Start Kafka Broker

Once Zookeeper is running, start the Kafka broker:

```bash
./bin/kafka-server-start.sh config/server.properties
```

- **Configuration File:** `config/server.properties`
  - This file contains Kafka broker configuration, such as the port, log directory, and broker ID.
- **Default Kafka Broker Port:** `9092`

### 3. Verify Kafka Setup

To ensure Kafka is running, list the topics currently available:

```bash
./bin/kafka-topics.sh --bootstrap-server localhost:9092 --list
```

If Kafka is correctly configured, this command should return an empty list if no topics are created yet.

---

## Managing Kafka Topics

### 1. Create a Topic

To create a topic named `my-topic`:

```bash
./bin/kafka-topics.sh --bootstrap-server localhost:9092 --create --topic my-topic
```

```bash
./bin/kafka-topics.sh --bootstrap-server localhost:9092 --create --topic my-topic --partitions 1 --replication-factor 1
```

- **Options:**
  - `--partitions`: Number of partitions for the topic (default: 1).
  - `--replication-factor`: Number of replicas for fault tolerance (default: 1 for local setup).

### 2. List Topics

To list all available topics:

```bash
./bin/kafka-topics.sh --bootstrap-server localhost:9092 --list
```

### 3. Describe a Topic

To describe details of a topic:

```bash
./bin/kafka-topics.sh --bootstrap-server localhost:9092 --describe --topic my-topic
```

- This provides information such as partitions, replicas, and in-sync replicas (ISR).

### 4. Delete a Topic

To delete a topic:

```bash
./bin/kafka-topics.sh --bootstrap-server localhost:9092 --delete --topic my-topic
```

- Ensure `delete.topic.enable=true` is set in the `config/server.properties` file.
- Restart the Kafka broker if this setting was not enabled previously.

---

## Producing and Consuming Messages

### 1. Producer: Produce/Push Messages

To produce messages:

```bash
./bin/kafka-console-producer.sh --bootstrap-server localhost:9092 --topic my-topic
```

- Type messages directly into the console and press `Enter` to send each message.

### 2.Consumer: Consume/Pull Messages

To consume messages:

```bash
./bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic my-topic --from-beginning
```

- **Options:**
  - `--from-beginning`: Reads all messages from the beginning of the topic.
---
<h2>Batch Data Production:</h2>
<p>To improve throughput, the producer collects multiple data points in batches and publishes them together. By default, the producer accumulates data for 1000 milliseconds (1 sec) before publishing.

You can customize this behavior using the --timeout option to specify a different time interval for batching. The timeout value can be set in milliseconds to fine-tune the batching duration.<br> For example:
</p>

```bash
./bin/kafka-console-producer.sh --bootstrap-server localhost:9092 --topic my-topic --timeout 500
```
- **Options:**
    - `--timeout <milliseconds>`: Specifies the time interval (in milliseconds) to wait for data before publishing the batch.

The producer pushes messages to Kafka brokers, sending data to topics as quickly as possible based on configurations like `batch.size` and `linger.ms`.

The consumer pulls messages from Kafka brokers, deciding when to fetch data based on internal logic, with control over how many records to fetch at a time using settings like `max.poll.records`.

---

# Serialization and Deserialization

In Kafka, **serialization** and **deserialization** refer to the processes of converting data into a byte stream for transmission (serialization) and converting the byte stream back into usable data (deserialization). These processes are crucial in Kafka for ensuring that the data being produced and consumed is in the correct format.
<br>
<br>
<b><i>It is Producer responsibility to serialize the data and Consumer responsibilty to deserialize the data. </i></b>
## 1. **Serialization in Kafka**

Serialization is the process of converting an object or data structure into a byte stream so that it can be transmitted over the network or stored in Kafka. Kafka producers serialize the data (messages) before sending it to Kafka brokers.

Kafka supports several serialization formats, including:
- **StringSerializer**: Converts strings into a byte array.
- **IntegerSerializer**: Converts integers into a byte array.
- **ByteArraySerializer**: Passes byte arrays as they are, without any modification.
- **AvroSerializer**: Used for serializing Avro data (used with schema registry).
- **JsonSerializer**: Used for serializing JSON objects.

Serialization ensures that data can be efficiently transmitted over the network, stored in Kafka topics, and retrieved by consumers in the right format.

## 2.Deserialization in Kafka

Deserialization is the process of converting a byte stream back into an object or data structure that can be used by the application. Kafka consumers deserialize the data (messages) they receive from Kafka brokers, converting the byte data back into a usable format.

Kafka supports several deserialization formats, including:

- **StringDeserializer**: Converts byte arrays into strings.
- **IntegerDeserializer**: Converts byte arrays into integers.
- **ByteArrayDeserializer**: Passes byte arrays as they are, without any modification.
- **AvroDeserializer**: Used for deserializing Avro data (used with schema registry).
- **JsonDeserializer**: Used for deserializing JSON objects.

Deserialization ensures that the data received from Kafka can be converted back into a usable format and processed by the consumer application, allowing for seamless communication between producers and consumers in Kafka-based systems.

## Additional Key Concepts

### 1. **Broker**

- A Kafka server is called a broker. It handles storage and retrieval of messages for clients.
- In a distributed environment, multiple brokers form a Kafka cluster.

### 2. **Topic**

- A topic is a category where records are published. Producers send data to topics, and consumers read data from topics.
- Topics are divided into partitions for scalability.

### 3. **Partitions**

- Each topic is split into one or more partitions. Messages within a partition are ordered.
- Partitions allow parallel processing and load balancing.

### 4. **Replication**

- Kafka replicates partitions across brokers to ensure reliability and fault tolerance.
- The replication factor determines how many copies of a partition exist.

### 5. **Producer**

- Producers are clients that send data to Kafka topics.
- They can choose which partition to send data to or let Kafka decide based on a hashing function.

### 6. **Consumer**

- Consumers are clients that read data from Kafka topics.
- They belong to consumer groups for scalability and load balancing.

### 7. **Zookeeper**

- Zookeeper manages Kafka’s metadata, such as configurations, partition assignments, and leader elections.

### 8. **Offsets**

Each message in a partition has an offset, a unique sequential ID used by consumers to track their progress.

- **Offset** is the unique identifier for each message in a Kafka partition.
- Consumers use offsets to track which messages they have processed.
- Offsets are managed by consumer groups, and Kafka provides both automatic and manual offset management.
- Kafka stores offsets in an internal topic, allowing consumers to resume from where they left off.

Use the following command to print the offset along with the consumed messages:

```bash
kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic my-topic --from-beginning --property print.offset=true
```
- **Options:**
    - `--property print.offset=true` will print the offset of each consumed message in the output.


Use the following command to print the timestamp of the consumed messages:

```bash
kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic my-topic --from-beginning --property print.timestamp=true
```
- **Options:**
    - `--property print.timestamp=true` will print the timestamp of each message when consumed.

### 9. **Log Retention Policy**

- The **log retention policy** controls when log segments are eligible for deletion, based on either the age of the logs or their accumulated size. These policies ensure efficient disk usage and prevent logs from growing indefinitely.
<br>
<br>
Check `server.properties` file for log retention properties. <br> <br>

    `log.retention.hours`: Determines the age of log segments for deletion based on time (default: 168 hours or 7 days).

    `log.retention.bytes` : Defines the maximum size for retained logs, pruning older segments when the size exceeds the limit.

    `log.segment.bytes`: Sets the maximum size of each log segment file before a new segment is created.

    `log.retention.check.interval.ms`: Sets the frequency of log retention checks, by default every 5 minutes.

---

## Debugging Tips

### Check Kafka Logs

Kafka logs provide insight into any issues:

```bash
cat logs/kafka-server.log
```

### Test Connectivity

Use the CLI producer and consumer to ensure Kafka is functioning properly before integrating with applications.

### Increase Verbosity

For more detailed output, add `--verbose` to Kafka CLI commands.

---

## Example Workflow

1. Start Zookeeper and Kafka:

   ```bash
   ./bin/zookeeper-server-start.sh config/zookeeper.properties
   ./bin/kafka-server-start.sh config/server.properties
   ```

2. Create a topic:

   ```bash
   ./bin/kafka-topics.sh --bootstrap-server localhost:9092 --create --topic test-topic --partitions 2 --replication-factor 1
   ```

3. Produce messages:

   ```bash
   ./bin/kafka-console-producer.sh --bootstrap-server localhost:9092 --topic test-topic
   ```
4. Consume messages:

   ```bash
   ./bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test-topic --from-beginning
   ```

5. Delete the topic:

   ```bash
   ./bin/kafka-topics.sh --bootstrap-server localhost:9092 --delete --topic test-topic
   ```

---

## Conclusion

This guide provides a comprehensive overview of running Kafka locally, managing topics, and using Kafka's CLI tools. With this knowledge, you can experiment with Kafka’s powerful features and integrate it into your applications
