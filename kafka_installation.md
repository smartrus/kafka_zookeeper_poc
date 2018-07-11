# Installing a Kafka broker

## Standalone Kafka broker installation

```
# tar -zxf kafka_2.12-1.1.0.tgz
# mv kafka_2.12-1.1.0 /usr/local/kafka
# mkdir /tmp/kafka-logs
# /usr/local/kafka/config/server.properties
####### Add the following lines:
port = 9092
advertised.host.name = localhost
# /usr/local/kafka/bin/kafka-server-start.sh -daemon /usr/local/kafka/config/server.properties
```

Create and verify a topic:

```
# /usr/local/kafka/bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test
Created topic "test".
# /usr/local/kafka/bin/kafka-topics.sh --zookeeper localhost:2181 --describe --topic test
Topic:test PartitionCount:1 ReplicationFactor:1 Configs:
Topic: test Partition: 0 Leader: 0 Replicas: 0 Isr: 0
```

Produce messages to a test topic:

```
# /usr/local/kafka/bin/kafka-console-producer.sh --broker-list localhost:9092 --topic test
message 1
message 2
message 3
^D
```

Consume messages from a test topic:

```
# /usr/local/kafka/bin/kafka-console-consumer.sh --zookeeper localhost:2181 --topic test --from-beginning
message 1
message 2
message 3
^C
Consumed 3 messages
```

## A broker configuration for a cluster

* broker.id - Kafka broker identifier
* port - default TCP port is 9092
* zookeeper.connect - Zookeeper settings for storing a broker metadata
** hostname
** port
** /path, an optional Zookeeper path to use as a chroot environment for the Kafka
cluster. If it is omitted, the root path is used.
* log.dirs - log directory
* num.recovery.threads.per.data.dir - a configurable pool of threads for handling log segments.
* auto.create.topics.enable - automatically creates a topic 1) when a producer starts writing messages to the topic 2) when a consumer starts reading messages from the topic 3) when any client requests metadata for the topic
* num.partitions - how many partitions a new topic is created with
* log.retention.ms - how long Kafka will retain messages
* log.retention.bytes - expire messages based on the total number of bytes of messages retained
* log.segment.bytes - the current log segment size
* log.segment.ms - the amount of time after which a log segment should be closed
* message.max.bytes - limits the maximum size of a message
