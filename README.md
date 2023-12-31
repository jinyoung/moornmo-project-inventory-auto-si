# topping-isVanillaK8s

## How to Test



Spin up microcks with async api enabled:
```
cd infra
docker compose -f docker-compose-microks.yml up -d
```

Check whether the event is properly consumed:
```
docker run -i --rm --network=host\
        edenhill/kcat:1.7.1 \
                -b localhost:9092 \
                -C \
                -t UsersignedupAPI-0.1.1-user-signedup \
                -f 'Headers: %h: Message value: %s\n'
```


docker run -i --rm --network=host\
        edenhill/kcat:1.7.1 \
                -b localhost:9092 \
                -C \
                -t microcks-services-updates \
                -f 'Headers: %h: Message value: %s\n'

list topics
```
docker run -it --network=host edenhill/kcat:1.7.1 -b localhost:9092 -L
```


To publish an OrderPlaced Event manually:
```
docker run -i --rm --network=host\
        edenhill/kcat:1.7.1 \
                -b localhost:9092 \
                -t moornmo.project \
                -H "type=OrderCreated" -H "contentType=application/json" \
                -K: \
                -P <<EOF
1:{"eventType":"OrderCreated","timestamp":1694009759996,"orderId":"0001","customerId":null,"totalAmount":null,"shippingAddress":null}
EOF
```



To see logs of kafka inside the microcks:
```
docker exec -it microcks-kafka /bin/bash
cd /opt/kafka/bin

./kafka-topics.sh --list --bootstrap-server localhost:9092
./kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic microcks-services-updates --from-beginning
```
