The project includes running Kafka with docker-compose.


## To run this project, simply do,

docker-compose up


<b>Understanding the Docker-Compose.yml</b>

Compose will start two services zookeeper and kafka as listed in the yml file . 

The first image is zookeeper which Kafka requires to keep track of various brokers, the network topology as well as synchronizing other information. Kafka broker can talk to zookeeper and that’s all the communication zookeeper needs.

The second service is kafka itself and we are just running a single instance of it, that is to say one broker.  The service listens on port 9092 which is mapped onto the same port number on the Docker Host and that’s how the service communicates with the outside world.


## The second service also has a couple of environment variables. 

a)KAFKA_ADVERTISED_HOST_NAME  default set to localhost. This is the address at which Kafka is running, and where producers and consumers can find it. Once again, this should be the set to localhost but rather to the IP address or the hostname with this the servers can be reached in your network. 

b)KAFKA_CREATE_TOPICS: Topics which you want to be created by default. Pattern includes : "topic1Name:no ofpartitions:replication factor,topic2Name:no ofpartitions:replication factor,..."

c)KAFKA_ZOOKEEPER_CONNECT: represents host ip of the zookeeper server

d)KAFKA_ADVERTISED_PORT: port where zookeeper is running



## <b>Running a simple message flow</b>

In order for Kafka to start working, we need to create a topic within it. The producer clients can then publish streams of data (messages) to the said topic and consumers can read the said datastream, if they are subscribed to that particular topic.

To do this we need to start a interactive terminal with the Kafka container. List the containers to retrieve the kafka container’s name. For example, lets assume our container is named apache-kafka_kafka_1


$ docker ps
With kafka container’s name, we can now drop inside this container.

$ docker exec -it apache-kafka_kafka_1 bash
bash-4.4#

Open two such different terminals to use one as consumer and another producer.


## Producer Side

In one of the prompts (the one you choose to be producer), enter the following commands:
 To start a producer that publishes datastream from standard input to kafka using topic mentioned in config

bash-4.4# kafka-console-producer.sh --broker-list localhost:9092 --topic UploadFile
>
The producer is now ready to take input from keyboard and publish it.



## Consumer Side
Move on the to the second terminal connected to your kafka container. The following command starts a consumer which feeds on test topic:

$ kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic UploadFile


Back to Producer
You can now type messages in the new prompt and everytime you hit return the new line is printed in the consumer prompt. For example:


> Test message produced.
This message gets transmitted to the consumer, through Kafka, and you can see it printed at the consumer prompt.

