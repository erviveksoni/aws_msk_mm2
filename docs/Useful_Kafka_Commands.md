# Useful Kafka Commands

### Regular Kafka Commands 
###### Get Cluster Details
```shell
aws kafka describe-cluster --region us-east-1 --cluster-arn "CLUSTER_ARN"
```

###### List Topics
```shell
./bin/kafka-topics.sh --list --zookeeper "<ZOOKEEPER_NODES_ADDRESS>"
```

###### Create Topic
```shell
./bin/kafka-topics.sh --create --zookeeper "<ZOOKEEPER_NODES_ADDRESS>" --replication-factor 3 --partitions 3 --topic MyTopic
```

###### Get Bootstrap Servers
```shell
aws kafka get-bootstrap-brokers --region us-east-1 --cluster-arn "CLUSTER_ARN"
```

###### Create Kafka Producer and Consumer
```shell
./bin/kafka-console-producer.sh --broker-list "<BROKER_NODES_ADDRESS>" --topic MyTopic --producer.config client_sasl.properties

./bin/kafka-console-consumer.sh --bootstrap-server "<BROKER_NODES_ADDRESS>"" --topic MyTopic --group mytopic_cg  --consumer.config client_sasl.properties
```

###### Kafka Consumer Groups Details
```shell
./bin/kafka-consumer-groups.sh --bootstrap-server "<BROKER_NODES_ADDRESS>" --group mytopic_cg --describe --command-config cg_client_sasl.properties
```

### Performance Test Kafka

###### Performance Test Kafka Producer
```shell
./bin/kafka-producer-perf-test.sh \
--topic perf-test-topic \
--throughput -1 \
--num-records 3000000 \
--record-size 1024 \
--producer-props acks=all bootstrap.servers=<BROKER_NODES_ADDRESS>
--producer.config client_sasl.properties
```

###### Performance Test Kafka Consumer
```shell
# Format the output
sudo yum install jq
```

```shell
./bin/kafka-consumer-perf-test.sh \
--topic perf-test-topic \
--broker-list <BROKER_NODES_ADDRESS>
--messages 3000000 \
--consumer.config client_sasl.properties | \
jq -R .|jq -sr 'map(./",")|transpose|map(join(": "))[]'
```

###### End to end Latency Test
To produce and consume 10k messages of 1 KiB each in with acks value set to all and encrypted data transfer via SSL:
```shell
./bin/kafka-run-class.sh kafka.tools.EndToEndLatency \
"<BROKER_NODES_ADDRESS>" \
perf-test-topic 10000 all 1024 client_sasl.properties
```


#### References
* [Perf Test Kafka Examples](https://gist.github.com/jkreps/c7ddb4041ef62a900e6c)
* [Apache kafka how to test performance](https://medium.com/metrosystemsro/apache-kafka-how-to-test-performan)
* [Perf Test Kafka Reference Commands](https://gist.github.com/rajkrrsingh/bc93471e82fe6806388939988f1d6358)
