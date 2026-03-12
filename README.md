# Kafka-Lab

This is a Lab of Data Engineering, which contains a Kafka cluster and a Spark cluster, the version of the Kafka cluster is 3.7.2, and the version of the spark cluster is 3.5.1.



## Initialize kafka cluster on Docker
STEP 0: Create directories
```BASH
mkdir -p data/kafka1
mkdir -p data/kafka2
mkdir -p data/kafka3
```


STEP 1: Get a random cluster id from a temporary kafka container

```bash
docker run --rm apache/kafka:3.7.2 /opt/kafka/bin/kafka-storage.sh random-uuid
```

e.g.

```
RrowE1AQR-KE0hMzIvv7XQ
```

and then, the temporary container will be removed automatically.



STEP2: Startup all containers

```BASH
docker compose up -d
```

then you will have all containers which have been described in docker-compose.yml file.



STEP3: Login to **each Kafka node**

```bash
docker exec -it kafka1 bash
```

Initialize KRaft components or the Kafka cluster will crash later

```BASH
/opt/kafka/bin/kafka-storage.sh format \
-t CLUSTER_ID \
-c /opt/kafka/config/kraft/server.properties
```

use the cluster id that have been already recieved from the temporary container, e.g.

```BASH
/opt/kafka/bin/kafka-storage.sh format \
-t RrowE1AQR-KE0hMzIvv7XQ \
-c /opt/kafka/config/kraft/server.properties
```

please note that the path "/opt/kafka/config/kraft/server.properties" have been attached to kafka-lab/config/server.x.properties which is located at local filesystem through docker volumn. Finally, repeat STEP 3 until all containers have been initialized or the whole cluster will crash.



STEP4: Restart all containers running on docker

```BASH
docker compose restart
```

cluster has been correctly configured.



## The details of nodes

This is a cluster with 3 kafka nodes and 3 spark nodes in the docker container environment. For all spark nodes, 1 master node and 2 worker nodes have been configured while all ports have been correctly reflected to local ports. About the reflection, see the table below

| Role         | Port GP-#1 (GUI)            | Port GP-#2 (Communication)  |
| ------------ | --------------------------- | --------------------------- |
| Spark-Master | Local: 8080 => Docker: 8080 | Local: 7077 => Docker: 7077 |
| Kafka-#1     | -                           | Local: 9092 => Docker: 9092 |
| Kafka-#2     | -                           | Local: 9093 => Docker: 9093 |
| Kafka-#3     | -                           | Local: 9094 => Docker: 9094 |
