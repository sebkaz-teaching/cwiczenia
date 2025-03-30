# Test kafka streaming 

1. Check topics list
```bash
cd ~ 
kafka/bin/kafka-topics.sh --list --bootstrap-server broker:9092
```

2. Create new topic `mytopic`
```bash
kafka/bin/kafka-topics.sh --create --topic mytopic  --bootstrap-server broker:9092
```

3. Create producer
```bash
kafka/bin/kafka-console-producer.sh --bootstrap-server broker:9092 --topic mytopic --property "parse.key=true" --property "key.separator=:"
```

4. Run spark connsumer with stream data
```bash
spark-submit --packages org.apache.spark:spark-sql-kafka-0-10_2.12:3.3.0  test_key_value.py
```

5. Type any text on producer termina

```bash
jan:45
alicja:20
```
ctrl+c  to end