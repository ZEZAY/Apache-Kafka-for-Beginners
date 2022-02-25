# Apache Kafka for Beginners

[Youtube Tutorial](https://www.youtube.com/watch?v=CU44hKLMg7khttps://www.youtube.com/watch?v=CU44hKLMg7k)

Apache Kafka is `distributed publish-subscribe` messaging system.

```txt
Producer -> Broker -> Consumer
```

Producer

- write msg to partition (in Topic)

Consumer

- read msg to partition (in Topic)
- 2 ways: waiting for new, from beginning

Kafka Broker

- receive msg from producer
- `store msg`
- give ability for consumers to read msg
- 1 broker can have many topics

Topic

- have specific name
- default duration: 7 days
- `queue` structure (msg)
- can be in many brokers at the same time
- part of a topic (in a broker) called `partition`

Partition

- 1 partition has 2+ nodes (2 types): leader & follower
- leader: handle partition read/write operation
- if leader fails, one of followers be come leader
- have many nodes to back up msg

Message (structure)

- timestamp
- offset number (unique across partition)
- key (optional)
- value (sequence of bytes)

```txt
Broker <-> Zookeeper
```

Zookeeper

- maintains list of active brokers
- manage config of topics and partitions
- elects controller

Zookeeper Cluster (ensemble)

- many zookeepers
- recommended to have `odd` number of zookeepers in an ensemble

## Installation

- [Apache Kafka](https://kafka.apache.org/downloads) (Binary)
- [Ubuntu](https://ubuntu.com/#download)
- [VirtualBox](https://www.virtualbox.org/wiki/Downloads)

## Setup Ubuntu on VirtualBox

- open VirtualBox, click "New"
- Setup following:
  - Name: Ubuntu Kafka
  - Type: Linux
  - Version: Ubuntu (64 bit)
  - memory: 2048 MB (2 GB)
  - hardware: create a hardware, VDI, Dynamically allocated

After save there will be a folder created

note. /Users/ZaGA/VirtualBox VMs/Ubuntu Kafka/

- click "Settings", go to Storage
- click Empty (Controller IDE)
  - Optical Drive: the `ubuntu-xxx-desktop-amd64.iso` file, we downloaded from [above](#installation)
- in the VM (small window), click "Install Ubuntu", finish the settings (it will restart)

## Setup Ubuntu on Droplet (Digital Ocean)

- in [Digital Ocean](https://cloud.digitalocean.com), create new Droplet (Ubuntu)
- copy `IP Address`

On Computer, to connect to the Droplet

```bash
$ ssh root@<IP_ADDRESS>
# enter pwd (send to email)
```

## Install Apache Kafka on Sever

```bash
# install java
$ sudo apt install openjdk-11-jdk

# check java version
$ java --version
```

```bash
# if Downloads not exist create one
$ mkdir Downloads

# download Apache Kafka
$ curl https://dlcdn.apache.org/kafka/3.1.0/kafka_2.13-3.1.0.tgz -o Downloads/kafka.tgz

# extract file
$ mkdir kafka
$ cd kafka
$ tar -xvzf ~/Downloads/kafka.tgz --strip 1
```

## Zookeeper and Broker

Kafka Broker can have multiple clusters

1 Kafka cluster = 1 Zookeeper + 1 Broker

Run Zookeeper

```bash
# default -> localhost:2181
$ bin/zookeeper-server-start.sh config/zookeeper.properties
```

Run Server/Broker (keep Zookeeper running)

```bash
# default -> localhost:9092
$ bin/kafka-server-start.sh config/server.properties
```

## Topic

```bash
# create, delete, describe, or change a topic
$ bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --topic <TOPIC_NAME>
```

```bash
# list topics belong to a zookeeper
$ bin/kafka-topics.sh --list --zookeeper localhost:2181

# get a topic detail
$ bin/kafka-topics.sh --describe --zookeeper localhost:2181 --topic <TOPIC_NAME>
```

## Producer and Consumer

in 1 Kafka cluster can have multiple Producers and Consumers

```bash
# start sending messages to a topic
$ bin/kafka-console-producer.sh --broker-list localhost:9092 --topic <TOPIC_NAME>
```

```bash
# start receiving messages from a topic
$ bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic <TOPIC_NAME>

# start receiving (and also show the earlier messages)
$ bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic <TOPIC_NAME> --from-beginning
```
