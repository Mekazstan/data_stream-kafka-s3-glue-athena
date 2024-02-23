![alt text](https://github.com/Mekazstan/data_stream-kafka-s3-glue-athena/blob/main/architecture.png)


# Real Time Data Streaming Application
This project implements a real-time data streaming application using various services such as Kafka, AWS EC2 instance, S3 bucket, Glue, and Athena to simulate an ETL (Extract, Transform, Load) process.

## About the Dataset
The dataset used contains a list of video games with sales greater than 100,000 copies. It was generated by a scrape of vgchartz.com.
Fields include:

1) Rank - Ranking of overall sales

2) Name - The games name

3) Platform - Platform of the games release (i.e. PC,PS4, etc.)

4) Year - Year of the game's release

5) Genre - Genre of the game

6) Publisher - Publisher of the game

7) NA_Sales - Sales in North America (in millions)

8) EU_Sales - Sales in Europe (in millions)

9) JP_Sales - Sales in Japan (in millions)

10) Other_Sales - Sales in the rest of the world (in millions)

11) Global_Sales - Total worldwide sales.

## Producer Overview
The producer.py script generates simulated transaction details for video game sales and streams them in real-time. It uses Kafka to produce events with transaction details, such as the name of the game, to the 'order_transactions' topic. The script reads the dataset and sends a randomly selected record as a JSON-formatted message to Kafka every two seconds.

## Consumer Overview
The consumer.py script acts as a consumer of the 'order_transactions' topic in Kafka. It retrieves the messages, processes them, and uploads the data to an S3 bucket. The consumer uses the S3 filesystem library to interact with the S3 bucket. Each message is stored in a separate JSON file within the specified S3 bucket and prefix. The script includes error handling to address issues during the JSON dump process.

## AWS Glue + Athena
The AWS Glue service is utilized to perform the ETL process on the data stored in the S3 bucket. A Glue crawler is configured to create a schema for the dataset in the S3 bucket. After running the Glue crawler, it creates a schema that can be queried using Athena. Athena allows for SQL-based queries on the dataset, making it easy to analyze and gain insights from the real-time streaming data.

Note: The Glue crawler needs to be run only once to create the schema. If encountering an error related to output location in Athena, allocate an S3 bucket to save the output.









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
- bin/kafka-server-start.sh config/server.properties
- sudo nano config/server.properties (change ADVERTISED_LISTENERS to public ip of the ec2 instance)

#### Creating the Kafka Topic on console 3
- cd kafka_2.13-3.6.1
- bin/kafka-topics.sh --create --topic order_transactions --bootstrap-server {public_ip}:9092 --replication-factor 1 --partitions 1 
- bin/kafka-topics.sh --bootstrap-server {public_ip}:9092 --list (To verify topic creation successfull)

#### Starting the Kafka Producer
- cd kafka_2.13-3.6.1
- bin/kafka-console-producer.sh --topic order_transactions --bootstrap-server {public_ip}:9092

#### Starting the Kafka Consumer on console 4
- cd kafka_2.13-3.6.1
- bin/kafka-console-consumer.sh --topic order_transactions --bootstrap-server {public_ip}:9092 ==> Statt
