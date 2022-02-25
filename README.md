# Apache Kafka for Beginners

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
