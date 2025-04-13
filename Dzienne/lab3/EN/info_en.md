## 1️⃣ Check topic list
Go to the home directory:
```sh
cd ~
kafka/bin/kafka-topics.sh --list --bootstrap-server broker:9092
```

## 2️⃣ Create new topis with name `mytopic`
```sh
kafka/bin/kafka-topics.sh --create --topic mytopic --bootstrap-server broker:9092
```

## 3 Recheck topic list
```sh
kafka/bin/kafka-topics.sh --list --bootstrap-server broker:9092 | grep mytopic
```