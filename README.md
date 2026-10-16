# nats-kafka-test
This repo is to p.o.c. locally kafka stream to nats, namespace nats-kafka-test

Use 2 terminals
1. Type a message into the Kafka Console Producer (Terminal 1).
2. Kafka stores that message on the topic test-kafka-stream.
3. NATS-Kafka Bridge detects the message on Kafka (KafkaToNATS).
4. The Bridge forwards that message over to the NATS Server - subject test.nats.received.
5. The NATS Listener (Terminal 2) picks up the message from the NATS Server and displays it

## 🚀 Getting Started

### Installation & Setup
1. Clone the repository
2. kubectl apply -f nats-kafka-test.yaml
3. (on one terminal)   
kubectl exec -it deployment/kafka-broker -n nats-kafka-test -- /opt/kafka/bin/kafka-console-producer.sh --bootstrap-server localhost:9092 --topic test-kafka-stream
4. (on another terminal, listener subscriber)  
kubectl run nats-listener -n nats-kafka-test -it --image=natsio/nats-box:latest --restart=Never --rm -- nats sub -s nats://nats-service:4222 "test.nats.received"
6. enter your test message in the first terminal, and switch to second terminal to see it
