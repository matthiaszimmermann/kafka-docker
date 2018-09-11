## Prepare your Setup

In order to properly work the environment variable *KAFKA_ADVERTISED_HOST_NAME* in the file *docker-compose.yml* should match the ip address of your computer. 
On a Mac/Linux box you may use the following shell command to get your ip address

```
ifconfig | grep inet
...
inet 192.168.0.105 netmask 0xffffff00 broadcast 192.168.0.255
...
```

for the example case above update the *KAFKA_ADVERTISED_HOST_NAME* accordingly:

```
...
    environment:
      KAFKA_ADVERTISED_HOST_NAME: 192.168.0.105
...
```

## Start Kafka (and Zookeeper)

Inside of your directory *kafka-docker* start kafka with the following command
```
docker-compose up -d
```

you can then check that your kafka instance is up and running
```
docker-compose ps
          Name                        Command               State                         Ports                       
----------------------------------------------------------------------------------------------------------------------
kafka-docker_kafka_1       start-kafka.sh                   Up      0.0.0.0:32771->9092/tcp                           
kafka-docker_zookeeper_1   /bin/sh -c /usr/sbin/sshd  ...   Up      0.0.0.0:2181->2181/tcp, 22/tcp, 2888/tcp, 3888/tcp
```

you are now ready to start using your kafka instance.

## Control Kafka via Kafka Shell

The steps below describe how to use the Kafka shell commands to create, list and delete a topic

### Run Kafka Shell
```
$ ./start-kafka-shell.sh
```

this starts docker container with kafka installed.
kafka shell commands are then located inside directory */opt/kafka/bin/*

### Create Topic *dummy*
```
bash-4.4# /opt/kafka/bin/kafka-topics.sh --zookeeper 192.168.0.105:2181 --create --topic dummy --partitions 3 --replication-factor 2
Error while executing topic command : Replication factor: 2 larger than available brokers: 1.
[2018-09-11 15:08:01,065] ERROR org.apache.kafka.common.errors.InvalidReplicationFactorException: Replication factor: 2 larger than available brokers: 1.
 (kafka.admin.TopicCommand$)

bash-4.4# /opt/kafka/bin/kafka-topics.sh --zookeeper 192.168.0.105:2181 --create --topic dummy --partitions 3 --replication-factor 1
Created topic "dummy".
```

### Show List of Topics
```
bash-4.4# /opt/kafka/bin/kafka-topics.sh --zookeeper 192.168.0.105:2181 --list
dummy
```

### Delete Topic *dummy*
```
bash-4.4# /opt/kafka/bin/kafka-topics.sh --zookeeper 192.168.0.105:2181 --delete --topic dummy
Topic dummy is marked for deletion.
Note: This will have no impact if delete.topic.enable is not set to true.

bash-4.4# /opt/kafka/bin/kafka-topics.sh --zookeeper 192.168.0.105:2181 --list
bash-4.4# 
```

### Exit Kafka Shell
```
bash-4.4# exit
exit
```

## Stop and remove Kafka (and Zookeeper)

```
docker-compose stop
docker-compose rm
```



