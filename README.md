# kafka-docs

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

### 1. Produce Messages

To produce messages to a topic:

```bash
./bin/kafka-console-producer.sh --bootstrap-server localhost:9092 --topic my-topic
```

- Type messages directly into the console and press `Enter` to send each message.

### 2. Consume Messages

To consume messages from a topic:

```bash
./bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic my-topic --from-beginning
```

- **Options:**
  - `--from-beginning`: Reads all messages from the beginning of the topic.

---

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

- Each message in a partition has an offset, a unique sequential ID used by consumers to track their progress.

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