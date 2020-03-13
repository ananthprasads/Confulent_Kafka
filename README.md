**Confulent\_Kafka**

docker-compose file for a 3 node Kafka cluster (using Confluent Kafka) with a 3 node Zookeeper.  The stack has persistent data and logs. Also, the Kafka service usable from the host network.

**Pre-requiste / Assumption:**

1. Ubuntu 16.04 installed.

2. Install docker using following link

https://docs.docker.com/install/linux/docker-ce/ubuntu/

3. Install docker compose

```

sudo curl -L &quot;https://github.com/docker/compose/releases/download/1.25.4/docker-compose-$(uname -s)-$(uname -m)&quot; -o /usr/local/bin/docker-compose

```

Apply executable permissions to the binary:

```

sudo chmod +x /usr/local/bin/docker-compose

```

Test the installation.

```

$ docker-compose --version

docker-compose version 1.25.4, build 1110ad01

```



**Usage:**

1. Create folders on host and provide permission (This path will be used for data/logs store).

```

sudo mkdir -p /vol/zk-data1 /vol/zk-txn-logs1 /vol/kafka-data1 /vol/zk-data2 /vol/zk-txn-logs2 /vol/kafka-data2 /vol/zk-data3 /vol/zk-txn-logs3 /vol/kafka-data3

sudo chmod 777 -R /vol/\*

```

2. Clone Repository

```

git clone [https://github.com/ananthprasads/Confulent\_Kafka.git](https://github.com/ananthprasads/Confulent_Kafka.git)

```

3. Get into cloned directory and perform docker compose up

```

$ sudo docker-compose up

```

**Validation:**

1. Perform docker compose ps

```

$ sudo docker-compose ps

```

Should see following output

```

@ubuntu16:~/Confulent\_Kafka$ sudo docker-compose ps

            Name                         Command            State   Ports

-------------------------------------------------------------------------

confulent\_kafka\_kafka-1\_1       /etc/confluent/docker/run   Up

confulent\_kafka\_kafka-2\_1       /etc/confluent/docker/run   Up

confulent\_kafka\_kafka-3\_1       /etc/confluent/docker/run   Up

confulent\_kafka\_zookeeper-1\_1   /etc/confluent/docker/run   Up

confulent\_kafka\_zookeeper-2\_1   /etc/confluent/docker/run   Up

confulent\_kafka\_zookeeper-3\_1   /etc/confluent/docker/run   Up

```

2. Check the ZooKeeper logs to verify that ZooKeeper is up. For example, for service zookeeper-1:

```

$ sudo docker-compose logs zookeeper-1

  ```

3. Install kafkacat

```

@ubuntu16:~/Confulent\_Kafka$ sudo apt-get install kafkacat

```

4. Run the following command to list all available brokers in the cluster:

```

@ubuntu16:~/Confulent\_Kafka$ kafkacat -L -b kafka-1:19092

Metadata for all topics (from broker -1: kafka-1:19092/bootstrap):

 3 brokers:

  broker 2 at localhost:29092

  broker 3 at localhost:39092

  broker 1 at localhost:19092

 2 topics:

  topic &quot;\_\_confluent.support.metrics&quot; with 1 partitions:

    partition 0, leader 2, replicas: 2,3,1, isrs: 2,3,1

  topic &quot;helloworld\_topic&quot; with 1 partitions:

    partition 0, leader 3, replicas: 3, isrs: 3

```

5. Produce message to helloworld\_topic

```

@ubuntu16:/vol$ kafkacat -P -b kafka-1:19092 -t helloworld\_topic

a

b

c

```

6. Consume message from helloworld\_topic

```

@ubuntu16:/vol$ kafkacat -P -b kafka-1:19092 -t helloworld\_topic

a

b

c

```

7. Consume message from helloworld\_topic using localhost

```

@ubuntu16:/vol$ kafkacat -C -b localhost:39092 -t helloworld\_topic

a

b

c

```
