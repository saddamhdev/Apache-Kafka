///////////////////////// Kafka Initial setup///////////////////////////////////////////////
1->  In server.properties file, change log.dirs path
#log.dirs=/tmp/kraft-combined-logs (change this path)
log.dirs=D:/ApacheKafka/kafka/logs

2-> Create logs folder in kafka installation folder( only if logs folder not available)
D:\ApacheKafka\kafka\logs

3-> Write this command in server.properties (set controller.quorum.voters)
controller.quorum.voters=1@localhost:9093

(Note: 1 = node.id and localhost:9093 = controller listener port)

4-> run this command to generate random UUID
.\bin\windows\kafka-storage.bat random-uuid

5-> run formate command
.\bin\windows\kafka-storage.bat format -t <UUID> -c .\config\server.properties --ignore-formatted

6-> run this command to Start Kafka Server in KRaft mode
.\bin\windows\kafka-server-start.bat .\config\server.properties

///////////////////// Create topic, producer and consumer////////////////////////////////////////

7-> Create Kafka topic
.\bin\windows\kafka-topics.bat --create --topic my-topic --bootstrap-server localhost:9092

8-> Create Producer(open new cmd for it)
.\bin\windows\kafka-console-producer.bat --topic my-topic --bootstrap-server localhost:9092


9-> Create consumer(open new cmd for it)
.\bin\windows\kafka-console-consumer.bat --topic my-topic --from-beginning --bootstrap-server localhost:9092


////////////Advance Kafka topic management commands in KRaft mode////////////////////////////////////
10-> Create a kafka topic with some advance configurations
.\bin\windows\kafka-topics.bat --create --topic my-topic --partitions 1 --replication-factor 1 --bootstrap-server localhost:9092

(//explainations
--partitions = Number of partitions (logical shards), usually between 1-3
--replication-factor = 1 (Keep 1 in KRaft single-node mode))



11-> Describe a topic
.\bin\windows\kafka-topics.bat --describe --topic my-topic --bootstrap-server localhost:9092

12->To see available List Topics
.\bin\windows\kafka-topics.bat --list --bootstrap-server localhost:9092

13-> Delete a topic
.\bin\windows\kafka-topics.bat --delete --topic my-topic --bootstrap-server localhost:9092

(to enable this command , add this code in server.properties
delete.topic.enable=true
)

14-> To increate topic partition(we can only increase the topic partition, can't decrease it)
.\bin\windows\kafka-topics.bat --alter --topic my-topic --partitions 3 --bootstrap-server localhost:9092
