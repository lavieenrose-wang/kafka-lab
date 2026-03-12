# Kafka-Lab



## Initialization kafka cluster on Docker
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

