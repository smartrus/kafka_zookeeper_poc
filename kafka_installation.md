# Installing a Kafka broker

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
