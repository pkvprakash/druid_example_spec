# Druid Realtime spec to read from multiple topics to multiple data sources
Given below are the steps to follow to index data from multiple topics to corresponding data sources. The example below reads data from two kafka topics ```wikipedia_realtime``` and ```wiki_realtime```. It then indexes them into datasources ```wikipedia``` and ```wiki```.

1. Start kafka topics
----------------------
Start two kafka topics ( ```wikipedia_realtime``` and ```wiki_realtime``` ) for the datasources  ```wikipedia``` and ```wiki```. 
```bash 
$ bin/kafka-console-producer.sh --broker-list localhost:9092 --topic wikipedia_realtime
```
and 
```bash 
$ bin/kafka-console-producer.sh --broker-list localhost:9092 --topic wiki_realtime
```

2. Start realtime nodes
--------------------
Start real time nodes using the realtime spec given in [realtime.spec](examples/realtime.spec). This will listen to the two topics we just started in the 1st step.

```bash 
$ java -Xmx512m -Duser.timezone=UTC -Dfile.encoding=UTF-8 -Ddruid.realtime.specFile=/path/to/realtime.spec -classpath "config/_common:config/realtime:lib/*"  io.druid.cli.Main server realtime
```
3. Generate data
----------------
Now generate some example data as per the schema given in realtime spec using  [generate-example-metrics](bin/generate-example-metrics). This is a simple python program that generates 25 records by default.

```bash 
$ ./generate-example-metrics --count 25
```

4. Publish data to topics
-------------------------
After generating data, publish data to kafka topics ```wikipedia_realtime``` and ```wiki_realtime```. Druid will index these records to corresponding datasource segments.

