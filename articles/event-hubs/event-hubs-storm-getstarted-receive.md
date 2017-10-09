---
title: "az Azure Event Hubs Apache Storm használatának aaaReceive események |} Microsoft Docs"
description: "Fogadását az Event Hubs Apache Storm használatának első lépései"
services: event-hubs
documentationcenter: 
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: java
ms.devlang: multiple
ms.topic: article
ms.date: 08/15/2017
ms.author: sethm
ms.openlocfilehash: a0ab860ee8d504a28aac380c504c928f0d6dbc1e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="receive-events-from-event-hubs-using-apache-storm"></a>Események fogadása Apache Storm használatának az Event Hubs

[Apache Storm](https://storm.incubator.apache.org) egy elosztott, valós idejű számítási rendszer, amely egyszerűbbé teszi a megbízható unbounded adatstreamek feldolgozására. Ez a szakasz bemutatja, hogyan toouse az Azure Event Hubs Storm spout tooreceive az Event Hubs eseményeit. Apache Storm használatának fel események különböző csomópontokon üzemelnek folyamatok között. hello Event Hubs-integráció a Storm leegyszerűsíti események felhasználásához transzparens módon ellenőrzőpontok használata a telepítés előrehaladását, Storm tartozó Zookeeper telepítést használ, kezeli az állandó ellenőrzőpontokat és párhuzamos fogadásokat az Event Hubs.

További információ az Event Hubs fogadni a mintákat, lásd: hello [Event Hubs – áttekintés][Event Hubs overview].

## <a name="create-project-and-add-code"></a>Projekt létrehozása, és adja hozzá a kódot

Ez az oktatóanyag használja egy [HDInsight alatt futó Storm] [ HDInsight Storm] telepítése, amely rendelkezik az Event Hubs spout hello már elérhető.

1. Hajtsa végre a hello [HDInsight alatt futó Storm - első lépések](../hdinsight/hdinsight-storm-overview.md) eljárás toocreate egy új HDInsight fürt, és csatlakozzon a távoli asztalon keresztül tooit.
2. Másolás hello `%STORM_HOME%\examples\eventhubspout\eventhubs-storm-spout-0.9-jar-with-dependencies.jar` fájl tooyour helyi fejlesztési környezet. Események-storm-spout hello tartalmazza.
3. Hello parancs tooinstall hello csomag következő hello helyi Maven tárolóhoz használni. Ez lehetővé teszi ezt a hello Storm projekt egy későbbi lépésben tooadd.

    ```shell
    mvn install:install-file -Dfile=target\eventhubs-storm-spout-0.9-jar-with-dependencies.jar -DgroupId=com.microsoft.eventhubs -DartifactId=eventhubs-storm-spout -Dversion=0.9 -Dpackaging=jar
    ```
4. Az eclipse-ben hozzon létre egy új Maven project (kattintson **fájl**, majd **új**, majd **projekt**).
   
    ![][12]
5. Válassza ki **alapértelmezett munkaterület helyet**, majd kattintson a **következő**
6. Jelölje be hello **maven-archetype – gyors üzembe helyezés** archetype, majd kattintson a **következő**
7. Helyezze be a **GroupId** és **artifactid szakaszát**, majd kattintson **Befejezés**
8. A **pom.xml**, adja hozzá a következő függőséget hello hello `<dependency>` csomópont.

    ```xml  
    <dependency>
        <groupId>org.apache.storm</groupId>
        <artifactId>storm-core</artifactId>
        <version>0.9.2-incubating</version>
        <scope>provided</scope>
    </dependency>
    <dependency>
        <groupId>com.microsoft.eventhubs</groupId>
        <artifactId>eventhubs-storm-spout</artifactId>
        <version>0.9</version>
    </dependency>
    <dependency>
        <groupId>com.netflix.curator</groupId>
        <artifactId>curator-framework</artifactId>
        <version>1.3.3</version>
        <exclusions>
            <exclusion>
                <groupId>log4j</groupId>
                <artifactId>log4j</artifactId>
            </exclusion>
            <exclusion>
                <groupId>org.slf4j</groupId>
                <artifactId>slf4j-log4j12</artifactId>
            </exclusion>
        </exclusions>
        <scope>provided</scope>
    </dependency>
    ```

9. A hello **src** mappa, nevű fájl létrehozása **Config.properties** és a következő tartalmat, és hello másolási hello `receive rule key` és `event hub name` értékeket:

    ```java
    eventhubspout.username = ReceiveRule
    eventhubspout.password = {receive rule key}
    eventhubspout.namespace = ioteventhub-ns
    eventhubspout.entitypath = {event hub name}
    eventhubspout.partitions.count = 16
       
    # if not provided, will use storm's zookeeper settings
    # zookeeper.connectionstring=localhost:2181
       
    eventhubspout.checkpoint.interval = 10
    eventhub.receiver.credits = 10
    ```
    a következő hello **eventhub.receiver.credits** határozza meg, hány események vannak kötegelni címek felszabadításával azok újból toohello Storm folyamat előtt. Hello szakét az egyszerűség, az ebben a példában ez érték too10 állítja be. Éles általában állítva toohigher értékek; például: 1024.
10. Hozzon létre egy új osztályt **LoggerBolt** a hello a következő kódot:
    
    ```java
    import java.util.Map;
    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;
    import backtype.storm.task.OutputCollector;
    import backtype.storm.task.TopologyContext;
    import backtype.storm.topology.OutputFieldsDeclarer;
    import backtype.storm.topology.base.BaseRichBolt;
    import backtype.storm.tuple.Tuple;
    
    public class LoggerBolt extends BaseRichBolt {
        private OutputCollector collector;
        private static final Logger logger = LoggerFactory
                  .getLogger(LoggerBolt.class);
    
        @Override
        public void execute(Tuple tuple) {
            String value = tuple.getString(0);
            logger.info("Tuple value: " + value);
   
            collector.ack(tuple);
        }
   
        @Override
        public void prepare(Map map, TopologyContext context, OutputCollector collector) {
            this.collector = collector;
            this.count = 0;
        }
        
        @Override
        public void declareOutputFields(OutputFieldsDeclarer declarer) {
            // no output fields
        }
    
    }
    ```
    
    A Storm bolt naplózza hello fogadott események hello tartalmát. Ez egyszerűen bővíthető toostore rekordokat a storage szolgáltatásban. Hello [HDInsight érzékelő elemzés oktatóanyag] a azonos megközelítés toostore adatokat használ a HBase.
11. Hozzon létre egy osztályt **LogTopology** a hello a következő kódot:
    
    ```java
    import java.io.FileReader;
    import java.util.Properties;
    import backtype.storm.Config;
    import backtype.storm.LocalCluster;
    import backtype.storm.StormSubmitter;
    import backtype.storm.generated.StormTopology;
    import backtype.storm.topology.TopologyBuilder;
    import com.microsoft.eventhubs.samples.EventCount;
    import com.microsoft.eventhubs.spout.EventHubSpout;
    import com.microsoft.eventhubs.spout.EventHubSpoutConfig;
        
    public class LogTopology {
        protected EventHubSpoutConfig spoutConfig;
        protected int numWorkers;
        
        protected void readEHConfig(String[] args) throws Exception {
            Properties properties = new Properties();
            if (args.length > 1) {
                properties.load(new FileReader(args[1]));
            } else {
                properties.load(EventCount.class.getClassLoader()
                        .getResourceAsStream("Config.properties"));
            }
        
            String username = properties.getProperty("eventhubspout.username");
            String password = properties.getProperty("eventhubspout.password");
            String namespaceName = properties
                    .getProperty("eventhubspout.namespace");
            String entityPath = properties.getProperty("eventhubspout.entitypath");
            String zkEndpointAddress = properties
                    .getProperty("zookeeper.connectionstring"); // opt
            int partitionCount = Integer.parseInt(properties
                    .getProperty("eventhubspout.partitions.count"));
            int checkpointIntervalInSeconds = Integer.parseInt(properties
                    .getProperty("eventhubspout.checkpoint.interval"));
            int receiverCredits = Integer.parseInt(properties
                    .getProperty("eventhub.receiver.credits")); // prefetch count
                                                                // (opt)
            System.out.println("Eventhub spout config: ");
            System.out.println("  partition count: " + partitionCount);
            System.out.println("  checkpoint interval: "
                    + checkpointIntervalInSeconds);
            System.out.println("  receiver credits: " + receiverCredits);
     
            spoutConfig = new EventHubSpoutConfig(username, password,
                    namespaceName, entityPath, partitionCount, zkEndpointAddress,
                    checkpointIntervalInSeconds, receiverCredits);
        
            // set hello number of workers toobe hello same as partition number.
            // hello idea is toohave a spout and a logger bolt co-exist in one
            // worker tooavoid shuffling messages across workers in storm cluster.
            numWorkers = spoutConfig.getPartitionCount();
        
            if (args.length > 0) {
                // set topology name so that sample Trident topology can use it as
                // stream name.
                spoutConfig.setTopologyName(args[0]);
            }
        }
        
        protected StormTopology buildTopology() {
            TopologyBuilder topologyBuilder = new TopologyBuilder();
       
            EventHubSpout eventHubSpout = new EventHubSpout(spoutConfig);
            topologyBuilder.setSpout("EventHubsSpout", eventHubSpout,
                    spoutConfig.getPartitionCount()).setNumTasks(
                    spoutConfig.getPartitionCount());
            topologyBuilder
                    .setBolt("LoggerBolt", new LoggerBolt(),
                            spoutConfig.getPartitionCount())
                    .localOrShuffleGrouping("EventHubsSpout")
                    .setNumTasks(spoutConfig.getPartitionCount());
            return topologyBuilder.createTopology();
        }
        
        protected void runScenario(String[] args) throws Exception {
            boolean runLocal = true;
            readEHConfig(args);
            StormTopology topology = buildTopology();
            Config config = new Config();
            config.setDebug(false);
        
            if (runLocal) {
                config.setMaxTaskParallelism(2);
                LocalCluster localCluster = new LocalCluster();
                localCluster.submitTopology("test", config, topology);
                Thread.sleep(5000000);
                localCluster.shutdown();
            } else {
                config.setNumWorkers(numWorkers);
                StormSubmitter.submitTopology(args[0], config, topology);
            }
        }
        
        public static void main(String[] args) throws Exception {
            LogTopology topology = new LogTopology();
            topology.runScenario(args);
        }
    }
    ```

    Ez az osztály hoz létre egy új Event Hubs spout, hello tulajdonságok használatát hello konfigurációs fájl tooinstantiate azt. Fontos, amely ebben a példában hoz annyi toonote spoutok feladatok hello eseményközpont, a partíciók számának hello rendelés toouse hello maximális párhuzamossági adott eseményközpont által engedélyezett.

## <a name="next-steps"></a>Következő lépések
További információ az Event Hubs érhetők el a következő hivatkozások hello:

* [Event Hubs – áttekintés][Event Hubs overview]
* [Eseményközpont létrehozása](event-hubs-create.md)
* [Event Hubs – gyakori kérdések](event-hubs-faq.md)

<!-- Links -->
[Event Hubs overview]: event-hubs-what-is-event-hubs.md
[HDInsight Storm]: ../hdinsight/hdinsight-storm-overview.md
[HDInsight érzékelő elemzés oktatóanyag]: ../hdinsight/hdinsight-storm-sensor-data-analysis.md

<!-- Images -->

[12]: ./media/event-hubs-get-started-receive-storm/create-storm1.png
