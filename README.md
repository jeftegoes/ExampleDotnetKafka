## What is this project

This is a simple implementation follow reference https://docs.confluent.io/kafka-clients/dotnet/current/overview.html.

## Docker compose

```
version: "3.9"
services:
  zookeeper:
    container_name: zookeeper_dev
    image: confluentinc/cp-zookeeper:latest
    volumes:
      - V:/zookeeper_home/data:/var/lib/zookeeper/data
      - V:/zookeeper_home/log:/var/lib/zookeeper/log
    environment:
      ZOOKEEPER_CLIENT_PORT: 2181
      ZOOKEEPER_TICK_TIME: 2000
    ports:
      - 2181:2181
    networks:
      - net
  kafka:
    container_name: kafka_dev
    image: confluentinc/cp-kafka:latest
    depends_on:
      - zookeeper
    environment:
      KAFKA_BROKER_ID: 1
      KAFKA_ZOOKEEPER_CONNECT: zookeeper:2181
      KAFKA_ADVERTISED_LISTENERS: PLAINTEXT://kafka:29092,PLAINTEXT_HOST://localhost:9092
      KAFKA_LISTENER_SECURITY_PROTOCOL_MAP: PLAINTEXT:PLAINTEXT,PLAINTEXT_HOST:PLAINTEXT
      KAFKA_INTER_BROKER_LISTENER_NAME: PLAINTEXT
      KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR: 1
    ports:
      - 9092:9092
    volumes:
      - V:/kafka_home:/var/lib/kafka
    networks:
      - net
networks:
  net:
```

## Commands

- kafka-topics --bootstrap-server localhost:9092 --create --topic demo_topic -- partitions 3

## How is it organized

- [Consumer](Consumer//) (.NET 6.0 / Console)
- [Producer](Producer/) (.NET 6.0 / Console)
