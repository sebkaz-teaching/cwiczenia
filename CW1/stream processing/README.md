# Test kafka streaming 

1. Sprawdź listę topiców
```bash
cd ~ 
kafka/bin/kafka-topics.sh --list --bootstrap-server broker:9092
```

2. Ustaw topic `mytopic`
```bash
kafka/bin/kafka-topics.sh --create --topic mytopic  --bootstrap-server broker:9092
```

3. Utwórz producenta 
```bash
kafka/bin/kafka-console-producer.sh --bootstrap-server broker:9092 --topic mytopic --property "parse.key=true" --property "key.separator=:"
```

4. Otwórz nowy terminal w miejscu gdzie znajduje się plik test_key_value.py i uruchom program Consumenta na Sparku
```bash
spark-submit --packages org.apache.spark:spark-sql-kafka-0-10_2.12:3.3.0  test_key_value.py
```

5. W terminalu z uruchominionym producentem wpisz teskt w postaci: 
```bash
jan:45
alicja:20
```
i sprawdz co pojawia się w oknie aplikacji spark 

Po zakończeniu pokazu ctrl+c zamyka zarówno okno producenta jak i aplikacje sparkową