
# Starting Kafka

In `docker-compose.yaml`:

```yaml
version: '2'
services:
  zookeeper:
    image: wurstmeister/zookeeper
    ports:
      - "2181:2181"
  kafka:
    build: .
    ports:
      - 9092:9092
    environment:
      KAFKA_ZOOKEEPER_CONNECT: zookeeper:2181
      HOSTNAME_COMMAND: "route -n | awk '/UG[ \t]/{print $$2}'"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
```

Then `docker-compose up -d` to start Kafka.


# Create Topic

docker exec -it kafka-docker_kafka_1 kafka-topics.sh --create --zookeeper zookeeper:2181 --replication-factor 1 --partitions 1 --topic my-topic-here

# Produce message

echo 'blah' | docker exec -i kafka-docker_kafka_1 kafka-console-producer.sh --bootstrap-server localhost:9092 --topic my-topic-here

# Consume message

docker exec -it kafka-docker_kafka_1 kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic my-topic-here --from-beginning
