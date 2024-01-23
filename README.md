# Real Time Data Streaming Application
This service incorporates the following services Kafka, AWS EC2 instance, S3 bucket, Glue, Athena


### Setting up Apache Kafka
#### Downloading Kafka
- wget https://downloads.apache.org/kafka/3.6.1/kafka_2.13-3.6.1.tgz
- tar -xvf kafka_2.13-3.6.1.tgz

#### Installing Java
- java -version
- sudo apt install java-1.8.0-openjdk
- java -version

#### Start Zookeeper on console 1
- cd kafka_2.13-3.6.1
- bin/zookeeper-server-start.sh config/zookeeper.properties

#### Start Kafka Server on console 2
- export KAFKA_HEAP_OPTS="-Xmx256M -Xms128M" (Assigning memory to it)
- cd kafka_2.13-3.6.1
- bin/kafka-server-start.sh config/zookeeper.properties
- sudo nano config/server.properties (change ADVERTISED_LISTENERS to public ip of the ec2 instance)

#### Creating the Kafka Topic on console 3
- cd kafka_2.13-3.6.1
- bin/kafka-topics.sh --create --topic order_transactions --bootstrap-server {public_ip}:9092 --replication-factor 1 --partitions 1

#### Starting the Kafka Producer
- cd kafka_2.13-3.6.1
- bin/kafka-console-producer.sh --topic order_transactions --bootstrap-server {public_ip}:9092

#### Starting the Kafka Consumer on console 4
- cd kafka_2.13-3.6.1
- bin/kafka-console-consumer.sh --topic order_transactions --bootstrap-server {public_ip}:9092