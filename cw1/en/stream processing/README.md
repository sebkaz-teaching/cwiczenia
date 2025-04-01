# Apache Kafka - Introduction

Apache Kafka is a stream processing system (event streaming) that acts as a distributed message broker.
It enables real-time data transmission and processing.

The default address of our broker is `broker:9092`.

In Apache Kafka, data is stored in structures called topics, which serve as communication queues.

Kafka management is performed using scripts. In our case, these will be .sh scripts.

## 1️⃣ Check the list of topics

Remember to navigate to the home directory:
```sh
cd ~
kafka/bin/kafka-topics.sh --list --bootstrap-server broker:9092
```

## 2️⃣ Create a new topic named mytopic
```sh
kafka/bin/kafka-topics.sh --create --topic mytopic --bootstrap-server broker:9092
```
## 3️⃣ Create a producer

This script allows you to manually enter events via the terminal. The `--property` options are additional and used for analysis in this example.

```sh
kafka/bin/kafka-console-producer.sh --bootstrap-server broker:9092 --topic mytopic --property "parse.key=true" --property "key.separator=:"
```

## 4️⃣ Consumer in Spark

Open a new terminal in the directory where the `test_key_value.py` file is located and run the `Consumer` program in Spark.

```python
from pyspark.sql import SparkSession

KAFKA_BROKER = 'broker:9092'
KAFKA_TOPIC = 'mytopic'

spark = SparkSession.builder.getOrCreate()
spark.sparkContext.setLogLevel("WARN")

df = (spark.readStream.format("kafka")
      .option("kafka.bootstrap.servers", KAFKA_BROKER)
      .option("subscribe", KAFKA_TOPIC)
      .option("startingOffsets", "earliest")
      .load()
     )

df.selectExpr("CAST(key AS STRING)", "CAST(value AS STRING)") \
 .writeStream \
 .format("console") \
 .outputMode("append") \
 .start() \
 .awaitTermination()
```
Note that Apache Spark does not have a built-in Kafka connector, so run the process using spark-submit and download the appropriate Scala package:

```sh
spark-submit --packages org.apache.spark:spark-sql-kafka-0-10_2.12:3.3.0 test_key_value.py
```

## 5️⃣ Test data transmission

In the terminal with the running producer, enter text in the following format:
```bash
jan:45
alicja:20
```
Check what appears in the Consumer application window.

## 6️⃣ Terminating the process

After completing the demonstration, use `Ctrl+C` to close both the producer window and the Spark application.

---
Done! Now you have a basic Apache Kafka and Spark setup for stream processing. 🎉