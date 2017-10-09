---
title: "aaaApache Storm például Java-topológiák - Azure HDInsight |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate Apache Storm-topológiák Java nyelven hozzon létre egy példa word száma topológia."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
keywords: "Apache storm, apache storm például storm java, a storm topológia – példa"
ms.assetid: a8838f29-9c08-4fd9-99ef-26655d1bf6d7
ms.service: hdinsight
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/07/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive,hdiseo17may2017
ms.openlocfilehash: 54fa9dc3c93ddad83ac861f3101f50f80117d804
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-apache-storm-topology-in-java"></a>Hozzon létre egy Apache Storm-topológia a Java nyelven

Megtudhatja, hogyan toocreate alatt futó Apache Storm egy Java-alapú topológiáját. A Storm-topológia, amely egy word-count alkalmazást hoz létre. Maven toobuild és a csomag hello projekt használja. Ezt követően megismerheti, hogyan hello fluxus keretrendszer toodefine hello topológia használatával.

> [!NOTE]
> hello fluxus keretrendszer elérhető Storm 0.10.0-s vagy újabb verzió. A Storm 0.10.0-s HDInsight 3.3 és 3.4 érhető el.

Ebben a dokumentumban hello lépések végrehajtását követően hello topológia tooApache HDInsight alatt futó Storm telepítheti.

> [!NOTE]
> A befejezett hello Storm-topológia példák létre ebben a dokumentumban verziója érhető el [https://github.com/Azure-Samples/hdinsight-java-storm-wordcount](https://github.com/Azure-Samples/hdinsight-java-storm-wordcount).

## <a name="prerequisites"></a>Előfeltételek

* [Java fejlesztői készlet (JDK) 7-es verzió](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)

* [Maven (https://maven.apache.org/download.cgi)](https://maven.apache.org/download.cgi): Maven rendszer project build Java-projektek esetében.

* Egy szövegszerkesztőben, vagy IDE.

## <a name="configure-environment-variables"></a>Környezeti változók konfigurálása

hello következő környezeti változó lehet beállítani a Java és hello JDK telepítésekor. Azonban ellenőrizni kell, hogy léteznek, illetve hello a rendszer tartozó helyes értékeket tartalmaz.

* **JAVA_HOME** -hello Java-futtatókörnyezet (JRE) futtató toohello directory kell mutatnia. Például egy Unix vagy Linux terjesztési nem rendelkezhet hasonló érték túl`/usr/lib/jvm/java-7-oracle`. A Windows rendszerben kellene hasonló érték túl`c:\Program Files (x86)\Java\jre1.7`

* **Elérési út** -elérési utak a következő hello tartalmaznia kell:

  * **JAVA_HOME** (vagy ezzel egyenértékű elérési hello)

  * **JAVA_HOME\bin** (vagy ezzel egyenértékű elérési hello)

  * hello mappát, ahová a Maven telepítve van

## <a name="create-a-maven-project"></a>Maven-projekt létrehozása

Hello parancssorból használható hello következő parancsot a toocreate nevű Maven-projektté **WordCount**:

```bash
mvn archetype:generate -DarchetypeArtifactId=maven-archetype-quickstart -DgroupId=com.microsoft.example -DartifactId=WordCount -DinteractiveMode=false
```

> [!NOTE]
> Ha a PowerShell használata esetén meg kell helyezze a`-D` az idézőjelek közé foglalt paraméterek.
>
> `mvn archetype:generate "-DarchetypeArtifactId=maven-archetype-quickstart" "-DgroupId=com.microsoft.example" "-DartifactId=WordCount" "-DinteractiveMode=false"`

Ezzel a paranccsal létrejön egy nevű könyvtár `WordCount` hello aktuális helyen, amely tartalmazza egy alapszintű Maven project. Hello `WordCount` könyvtár hello a következő elemeket tartalmazza:

* `pom.xml`: Hello Maven project beállításait tartalmazza.
* `src\main\java\com\microsoft\example`: Az alkalmazás kódját tartalmazza.
* `src\test\java\com\microsoft\example`: Az alkalmazás a tesztek tartalmazza. 

### <a name="remove-hello-generated-example-code"></a>Távolítsa el a generált hello példakód

Generált hello tesztelése és hello alkalmazásfájlok törlése:

* **src\test\java\com\microsoft\example\AppTest.Java**
* **src\main\java\com\microsoft\example\App.Java**

## <a name="add-maven-repositories"></a>Adja hozzá a Maven tárházak

A HDInsight alatt futó Apache Storm projektjeikbe hello Hortonworks tárház toodownload függőségek használatát javasoljuk, hello Hortonworks Data Platform (HDP) alapul. A hello __pom.xml__ fájlt, adja hozzá az XML hello után a következő hello `<url>http://maven.apache.org</url>` sor:

```xml
<repositories>
    <repository>
        <releases>
            <enabled>true</enabled>
            <updatePolicy>always</updatePolicy>
            <checksumPolicy>warn</checksumPolicy>
        </releases>
        <snapshots>
            <enabled>false</enabled>
            <updatePolicy>never</updatePolicy>
            <checksumPolicy>fail</checksumPolicy>
        </snapshots>
        <id>HDPReleases</id>
        <name>HDP Releases</name>
        <url>http://repo.hortonworks.com/content/repositories/releases/</url>
        <layout>default</layout>
    </repository>
    <repository>
        <releases>
            <enabled>true</enabled>
            <updatePolicy>always</updatePolicy>
            <checksumPolicy>warn</checksumPolicy>
        </releases>
        <snapshots>
            <enabled>false</enabled>
            <updatePolicy>never</updatePolicy>
            <checksumPolicy>fail</checksumPolicy>
        </snapshots>
        <id>HDPJetty</id>
        <name>Hadoop Jetty</name>
        <url>http://repo.hortonworks.com/content/repositories/jetty-hadoop/</url>
        <layout>default</layout>
    </repository>
</repositories>
```

## <a name="add-properties"></a>Tulajdonságok hozzáadása

Maven lehetővé teszi tulajdonságok nevű toodefine projektszintű értékeket. A hello __pom.xml__, adja hozzá a következő szöveg után hello hello `</repositories>` sor:

```xml
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <!--
    This is a version of Storm from hello Hortonworks repository that is compatible with HDInsight.
    -->
    <storm.version>1.0.1.2.5.3.0-37</storm.version>
</properties>
```

Ezután már használhatja ezt az értéket a többi szakasza pedig hello `pom.xml`. Például Storm-összetevőket hello verziójának meghatározásakor használhatja `${storm.version}` rögzített kódolási érték helyett.

## <a name="add-dependencies"></a>Adja hozzá a függőségek

Vegyen fel egy függőséget Storm-összetevőket. Nyissa meg hello `pom.xml` fájlt, és adja hozzá a következő kódot a hello hello `<dependencies>` szakasz:

```xml
<dependency>
    <groupId>org.apache.storm</groupId>
    <artifactId>storm-core</artifactId>
    <version>${storm.version}</version>
    <!-- keep storm out of hello jar-with-dependencies -->
    <scope>provided</scope>
</dependency>
```

Fordítási időben Maven ezen információk toolook felhasználásra kerül `storm-core` hello Maven tárházban. Először a jelek hello tárházban a helyi számítógépen. Ha hello fájlokat nem létezik, a Maven adattárból hello nyilvános Maven letölti azokat, és tárolja őket a hello helyi tárházban.

> [!NOTE]
> Értesítés hello `<scope>provided</scope>` sor ebben a szakaszban. Ez a beállítás arról értesíti a Maven tooexclude **storm-core** bármely JAR fájlok jönnek létre, mert azt hello rendszer biztosítja.

## <a name="build-configuration"></a>Konfiguráció

Maven beépülő modulok lehetővé teszik a hello projekt toocustomize hello build fázisból áll. Például hogyan hello projekt lefordított vagy hogyan toopackage JAR-fájlra be azt. Nyissa meg hello `pom.xml` fájlt, és adja hozzá a következő kódot közvetlenül felett hello hello `</project>` sor.

```xml
<build>
    <plugins>
    </plugins>
    <resources>
    </resources>
</build>
```

Ebben a szakaszban használt tooadd beépülő modulok, erőforrások és egyéb build-konfigurációs beállítások. A hello teljes körű referenciáért **pom.xml** fájl című [http://maven.apache.org/pom.html](http://maven.apache.org/pom.html).

### <a name="add-plug-ins"></a>Beépülő modulok hozzáadása

Apache Storm-topológiák Java megvalósított, hello [Exec Maven beépülő modul](http://www.mojohaus.org/exec-maven-plugin/) akkor hasznos, mivel a tooeasily hello topológia futtassa helyileg a fejlesztési környezetet. Adja hozzá a következő toohello hello `<plugins>` hello szakasza `pom.xml` tooinclude hello Exec Maven beépülő modul fájlt:

```xml
<plugin>
    <groupId>org.codehaus.mojo</groupId>
    <artifactId>exec-maven-plugin</artifactId>
    <version>1.4.0</version>
    <executions>
    <execution>
    <goals>
        <goal>exec</goal>
    </goals>
    </execution>
    </executions>
    <configuration>
    <executable>java</executable>
    <includeProjectDependencies>true</includeProjectDependencies>
    <includePluginDependencies>false</includePluginDependencies>
    <classpathScope>compile</classpathScope>
    <mainClass>${storm.topology}</mainClass>
    <cleanupDaemonThreads>false</cleanupDaemonThreads> 
    </configuration>
</plugin>
```

Egy másik hasznos beépülő modul az hello [Apache Maven fordító beépülő modul](http://maven.apache.org/plugins/maven-compiler-plugin/), mely van használt toochange fordítási lehetőségeket. hello módosítások hello Maven által hello forrása és célja az alkalmazás a Java-verziót.

* A HDInsight __3.4 vagy korábbi__, hello forrás beállítása és a Java-verzió too__1.7__ céloz.

* A HDInsight __3.5__, hello forrás beállítása és a Java-verzió too__1.8__ céloz.

Adja hozzá a következő szöveget: hello hello `<plugins>` hello szakasza `pom.xml` tooinclude hello Apache Maven fordító beépülő modul fájlt. Ez a példa 1.8, határozza meg, így hello HDInsight célverzió 3.5-ös verzióját.

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-compiler-plugin</artifactId>
    <version>3.3</version>
    <configuration>
    <source>1.8</source>
    <target>1.8</target>
    </configuration>
</plugin>
```

### <a name="configure-resources"></a>Erőforrások konfigurálása

hello erőforrások szakasz lehetővé teszi tooinclude nem kód erőforrások például összetevők hello topológia számára szükséges konfigurációs fájlokat. Ennél a példánál adja hozzá a következő szöveget: hello hello `<resources>` hello szakasza "pom.xml fájlt.

```xml
<resource>
    <directory>${basedir}/resources</directory>
    <filtering>false</filtering>
    <includes>
        <include>log4j2.xml</include>
    </includes>
</resource>
```

Ez a példa hello erőforrások könyvtárat ad hello hello projekt gyökerében található (`${basedir}`) olyan erőforrásokat tartalmaz, és a hello fájlt tartalmazó helyként `log4j2.xml`. A fájl használt tooconfigure, milyen információt naplózta hello topológia.

## <a name="create-hello-topology"></a>Hozzon létre hello topológia

A Java-alapú Apache Storm-topológia áll meg kell írni három összetevő (vagy hivatkozás) a függőség beállításához.

* **Spoutok**: olvassa be a külső adatokat adatforrásokat, és megfelelően kibocsát adatstreamek hello topológia be.

* **Boltok**: feldolgozási végez spoutokkal kapcsolatban, vagy más boltokhoz által kibocsátott adatfolyamokat, és megfelelően kibocsát egy vagy több adatfolyamokat.

* **Topológia**: hogyan hello spoutok boltokhoz vannak rendezve, és és hello belépési pontot nyújt hello topológia meghatározása.

### <a name="create-hello-spout"></a>Hello spout létrehozása

külső adatforrások, tooreduce követelményei hello következő spout egyszerűen véletlenszerű mondat bocsát ki. Egy spout hello mellékelt módosított változatát [Storm-kezdőpéldák](https://github.com/apache/storm/blob/0.10.x-branch/examples/storm-starter/src/jvm/storm/starter).

> [!NOTE]
> Például egy olyan spout egy külső adatforrásból olvasó tekintse meg a következő példák hello egyikét:
>
> * [TwitterSampleSPout](https://github.com/apache/storm/blob/0.10.x-branch/examples/storm-starter/src/jvm/storm/starter/spout/TwitterSampleSpout.java): egy példa spout, amely Twitter olvassa be
> * [A Storm-Kafka](https://github.com/apache/storm/tree/0.10.x-branch/external/storm-kafka): egy spout, amely Kafka olvassa be

A hello spout, hozzon létre egy fájlt `RandomSentenceSpout.java` a hello `src\main\java\com\microsoft\example` Java-kóddal hello tartalmát, a következő könyvtárra, és használja hello:

```java
package com.microsoft.example;

import org.apache.storm.spout.SpoutOutputCollector;
import org.apache.storm.task.TopologyContext;
import org.apache.storm.topology.OutputFieldsDeclarer;
import org.apache.storm.topology.base.BaseRichSpout;
import org.apache.storm.tuple.Fields;
import org.apache.storm.tuple.Values;
import org.apache.storm.utils.Utils;

import java.util.Map;
import java.util.Random;

//This spout randomly emits sentences
public class RandomSentenceSpout extends BaseRichSpout {
  //Collector used tooemit output
  SpoutOutputCollector _collector;
  //Used toogenerate a random number
  Random _rand;

  //Open is called when an instance of hello class is created
  @Override
  public void open(Map conf, TopologyContext context, SpoutOutputCollector collector) {
  //Set hello instance collector toohello one passed in
    _collector = collector;
    //For randomness
    _rand = new Random();
  }

  //Emit data toohello stream
  @Override
  public void nextTuple() {
  //Sleep for a bit
    Utils.sleep(100);
    //hello sentences that are randomly emitted
    String[] sentences = new String[]{ "hello cow jumped over hello moon", "an apple a day keeps hello doctor away",
        "four score and seven years ago", "snow white and hello seven dwarfs", "i am at two with nature" };
    //Randomly pick a sentence
    String sentence = sentences[_rand.nextInt(sentences.length)];
    //Emit hello sentence
    _collector.emit(new Values(sentence));
  }

  //Ack is not implemented since this is a basic example
  @Override
  public void ack(Object id) {
  }

  //Fail is not implemented since this is a basic example
  @Override
  public void fail(Object id) {
  }

  //Declare hello output fields. In this case, an sentence
  @Override
  public void declareOutputFields(OutputFieldsDeclarer declarer) {
    declarer.declare(new Fields("sentence"));
  }
}
```

> [!NOTE]
> Bár ez a topológia csak egy spout, mások is rendelkezhet, amely adatokat különböző forrásokból történő hello topológia több.

### <a name="create-hello-bolts"></a>Hello boltokhoz létrehozása

Szögek hello adatfeldolgozási kezelni. Ez a topológia két boltokhoz használja:

* **SplitSentence**: hello mondat által kibocsátott felosztja **RandomSentenceSpout** az egyes szavakat.

* **WordCount**: hányszor történt minden szó található.

> [!NOTE]
> Szögek is végrehajthat, például számítási, adatmegőrzés vagy tooexternal összetevők van szó.

Hozzon létre két új fájl `SplitSentence.java` és `WordCount.java` a hello `src\main\java\com\microsoft\example` könyvtár. Szöveg hello fájlok hello tartalmát, a következő hello használata:

#### <a name="splitsentence"></a>SplitSentence

```java
package com.microsoft.example;

import java.text.BreakIterator;

import org.apache.storm.topology.BasicOutputCollector;
import org.apache.storm.topology.OutputFieldsDeclarer;
import org.apache.storm.topology.base.BaseBasicBolt;
import org.apache.storm.tuple.Fields;
import org.apache.storm.tuple.Tuple;
import org.apache.storm.tuple.Values;

//There are a variety of bolt types. In this case, use BaseBasicBolt
public class SplitSentence extends BaseBasicBolt {

  //Execute is called tooprocess tuples
  @Override
  public void execute(Tuple tuple, BasicOutputCollector collector) {
    //Get hello sentence content from hello tuple
    String sentence = tuple.getString(0);
    //An iterator tooget each word
    BreakIterator boundary=BreakIterator.getWordInstance();
    //Give hello iterator hello sentence
    boundary.setText(sentence);
    //Find hello beginning first word
    int start=boundary.first();
    //Iterate over each word and emit it toohello output stream
    for (int end=boundary.next(); end != BreakIterator.DONE; start=end, end=boundary.next()) {
      //get hello word
      String word=sentence.substring(start,end);
      //If a word is whitespace characters, replace it with empty
      word=word.replaceAll("\\s+","");
      //if it's an actual word, emit it
      if (!word.equals("")) {
        collector.emit(new Values(word));
      }
    }
  }

  //Declare that emitted tuples contain a word field
  @Override
  public void declareOutputFields(OutputFieldsDeclarer declarer) {
    declarer.declare(new Fields("word"));
  }
}
```

#### <a name="wordcount"></a>WordCount

```java
package com.microsoft.example;

import java.util.HashMap;
import java.util.Map;
import java.util.Iterator;

import org.apache.storm.Constants;
import org.apache.storm.topology.BasicOutputCollector;
import org.apache.storm.topology.OutputFieldsDeclarer;
import org.apache.storm.topology.base.BaseBasicBolt;
import org.apache.storm.tuple.Fields;
import org.apache.storm.tuple.Tuple;
import org.apache.storm.tuple.Values;
import org.apache.storm.Config;

// For logging
import org.apache.logging.log4j.Logger;
import org.apache.logging.log4j.LogManager;

//There are a variety of bolt types. In this case, use BaseBasicBolt
public class WordCount extends BaseBasicBolt {
  //Create logger for this class
  private static final Logger logger = LogManager.getLogger(WordCount.class);
  //For holding words and counts
  Map<String, Integer> counts = new HashMap<String, Integer>();
  //How often tooemit a count of words
  private Integer emitFrequency;

  // Default constructor
  public WordCount() {
      emitFrequency=5; // Default too60 seconds
  }

  // Constructor that sets emit frequency
  public WordCount(Integer frequency) {
      emitFrequency=frequency;
  }

  //Configure frequency of tick tuples for this bolt
  //This delivers a 'tick' tuple on a specific interval,
  //which is used tootrigger certain actions
  @Override
  public Map<String, Object> getComponentConfiguration() {
      Config conf = new Config();
      conf.put(Config.TOPOLOGY_TICK_TUPLE_FREQ_SECS, emitFrequency);
      return conf;
  }

  //execute is called tooprocess tuples
  @Override
  public void execute(Tuple tuple, BasicOutputCollector collector) {
    //If it's a tick tuple, emit all words and counts
    if(tuple.getSourceComponent().equals(Constants.SYSTEM_COMPONENT_ID)
            && tuple.getSourceStreamId().equals(Constants.SYSTEM_TICK_STREAM_ID)) {
      for(String word : counts.keySet()) {
        Integer count = counts.get(word);
        collector.emit(new Values(word, count));
        logger.info("Emitting a count of " + count + " for word " + word);
      }
    } else {
      //Get hello word contents from hello tuple
      String word = tuple.getString(0);
      //Have we counted any already?
      Integer count = counts.get(word);
      if (count == null)
        count = 0;
      //Increment hello count and store it
      count++;
      counts.put(word, count);
    }
  }

  //Declare that this emits a tuple containing two fields; word and count
  @Override
  public void declareOutputFields(OutputFieldsDeclarer declarer) {
    declarer.declare(new Fields("word", "count"));
  }
}
```

### <a name="define-hello-topology"></a>Hello topológia meghatározása

hello topológia kötelékek hello spoutokkal kapcsolatban, és a grafikon, amely meghatározza, hogyan közötti adatáramlás hello összetevők együttesen boltok. Párhuzamossági mutatók Storm használó hello fürtön belül hello összetevők példányai létrehozásakor is tartalmazza.

hello példánycsoportokat egy egyszerű diagram hello gráf ebben a topológiában az összetevőt.

![diagram ábrázoló hello spoutok és boltok elrendezése](./media/hdinsight-storm-develop-java-topology/wordcount-topology.png)

tooimplement hello topológia, hozzon létre egy fájlt `WordCountTopology.java` a hello `src\main\java\com\microsoft\example` könyvtár. Java-kóddal hello hello fájl tartalmát, a következő hello használata:

```java
package com.microsoft.example;

import org.apache.storm.Config;
import org.apache.storm.LocalCluster;
import org.apache.storm.StormSubmitter;
import org.apache.storm.topology.TopologyBuilder;
import org.apache.storm.tuple.Fields;

import com.microsoft.example.RandomSentenceSpout;

public class WordCountTopology {

  //Entry point for hello topology
  public static void main(String[] args) throws Exception {
  //Used toobuild hello topology
    TopologyBuilder builder = new TopologyBuilder();
    //Add hello spout, with a name of 'spout'
    //and parallelism hint of 5 executors
    builder.setSpout("spout", new RandomSentenceSpout(), 5);
    //Add hello SplitSentence bolt, with a name of 'split'
    //and parallelism hint of 8 executors
    //shufflegrouping subscribes toohello spout, and equally distributes
    //tuples (sentences) across instances of hello SplitSentence bolt
    builder.setBolt("split", new SplitSentence(), 8).shuffleGrouping("spout");
    //Add hello counter, with a name of 'count'
    //and parallelism hint of 12 executors
    //fieldsgrouping subscribes toohello split bolt, and
    //ensures that hello same word is sent toohello same instance (group by field 'word')
    builder.setBolt("count", new WordCount(), 12).fieldsGrouping("split", new Fields("word"));

    //new configuration
    Config conf = new Config();
    //Set toofalse toodisable debug information when
    // running in production on a cluster
    conf.setDebug(false);

    //If there are arguments, we are running on a cluster
    if (args != null && args.length > 0) {
      //parallelism hint tooset hello number of workers
      conf.setNumWorkers(3);
      //submit hello topology
      StormSubmitter.submitTopology(args[0], conf, builder.createTopology());
    }
    //Otherwise, we are running locally
    else {
      //Cap hello maximum number of executors that can be spawned
      //for a component too3
      conf.setMaxTaskParallelism(3);
      //LocalCluster is used toorun locally
      LocalCluster cluster = new LocalCluster();
      //submit hello topology
      cluster.submitTopology("word-count", conf, builder.createTopology());
      //sleep
      Thread.sleep(10000);
      //shut down hello cluster
      cluster.shutdown();
    }
  }
}
```

### <a name="configure-logging"></a>Naplózás konfigurálása

A Storm Apache Log4j toolog információkat használja. Ha nem konfigurálja a naplózási, hello topológia diagnosztikai adatokat bocsát ki. Mi kerül toocontrol hozzon létre egy fájlt `log4j2.xml` a hello `resources` könyvtár. XML hello hello fájl tartalmát, a következő hello használata.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Configuration>
<Appenders>
    <Console name="STDOUT" target="SYSTEM_OUT">
        <PatternLayout pattern="%d{HH:mm:ss} [%t] %-5level %logger{36} - %msg%n"/>
    </Console>
</Appenders>
<Loggers>
    <Logger name="com.microsoft.example" level="trace" additivity="false">
        <AppenderRef ref="STDOUT"/>
    </Logger>
    <Root level="error">
        <Appender-Ref ref="STDOUT"/>
    </Root>
</Loggers>
</Configuration>
```

Az XML-kód konfigurálja egy új naplózó a hello `com.microsoft.example` osztályt, amely ebben a példában topológiában hello összetevőket tartalmazza. hello szintje tootrace a a tranzakciónaplókat tartalmazó, amely minden ebben a topológiában-összetevők által kibocsátott naplózási információkat rögzíti.

Hello `<Root level="error">` szakasz hello legfelső szintű a naplózási szint konfigurálása (nem a minden `com.microsoft.example`) tooonly naplózási hiba adatok.

Log4j naplózásának konfigurálásáról további információkért lásd: [http://logging.apache.org/log4j/2.x/manual/configuration.html](http://logging.apache.org/log4j/2.x/manual/configuration.html).

> [!NOTE]
> Storm verzióját 0.10.0-s és magasabb használata Log4j 2.x. A storm régebbi verzióit használja Log4j 1.x, a naplózási konfiguráció más formátumú használt. Hello régebbi konfigurációtól tudnivalókért lásd: [http://wiki.apache.org/logging-log4j/Log4jXmlFormat](http://wiki.apache.org/logging-log4j/Log4jXmlFormat).

## <a name="test-hello-topology-locally"></a>Teszt hello helyileg topológia

Hello fájlok mentése után használja a következő parancs tootest hello topológia helyileg hello.

```bash
mvn compile exec:java -Dstorm.topology=com.microsoft.example.WordCountTopology
```

Futtatja, a hello topológia indítási információkat jeleníti meg. hello következő szövege hello word-count kimeneti példát:

    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word snow
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word white
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 112 for word seven
    17:33:27 [Thread-16-count] INFO  com.microsoft.example.WordCount - Emitting a count of 195 for word the
    17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 113 for word and
    17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word dwarfs
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word snow

Ez a példa napló azt jelzi, hogy hello word "és" 113 alkalommal kibocsátását. hello száma továbbra is toogo be, amíg hello topológia fut, mert hello spout folyamatosan hello bocsát ki ugyanazt a mondatok.

Egy 5 másodperces időköze van szó kibocsátási és a számok között. Hello **WordCount** -összetevő tooonly adatok létrehozása, ha egy rekordot érkezik. Adott listának csak kézbesítési öt másodpercenként osztásjelek kéri.

## <a name="convert-hello-topology-tooflux"></a>Hello topológia tooFlux átalakítása

Fluxus egy új keretében elérhető Storm 0.10.0-s vagy újabb, amely lehetővé teszi a megvalósítás tooseparate konfigurációja a rendszer. Az összetevők továbbra is Java vannak definiálva, de hello topológia definíciója YAM-fájllal. Csomagdefiníció egy alapértelmezett topológia a projektet, vagy egy önálló fájlt használja, hello topológia elküldésekor. Hello topológia tooStorm elküldésekor hello YAM topológia meghatározása környezeti változókat vagy konfigurációs fájlok toopopulate értékek használhatja.

hello YAM fájl hello összetevők toouse hello topológia és a közöttük hello adatfolyama határozza meg. Megadhat egy YAM fájl hello jar-fájl részeként, vagy egy külső YAM-fájl használatával.

Fluxus további információkért lásd: [fluxus keretrendszer (https://storm.apache.org/releases/0.10.0/flux.html)](https://storm.apache.org/releases/0.10.0/flux.html).

> [!WARNING]
> Esedékes tooa [hiba (https://issues.apache.org/jira/browse/STORM-2055)](https://issues.apache.org/jira/browse/STORM-2055) Storm 1.0.1-es, szükség lehet a tooinstall egy [Storm fejlesztőkörnyezet](https://storm.apache.org/releases/1.0.1/Setting-up-development-environment.html) toorun fluxus topológiák helyileg.

1. Helyezze át a hello `WordCountTopology.java` fájlt kívüli hello projekt. Korábban a fájl a hello topológia definiálva, de a fluxus nem szükséges.

2. A hello `resources` könyvtár, hozzon létre egy fájlt `topology.yaml`. Szöveg hello a fájl tartalmát, a következő hello használata.

        name: "wordcount"       # friendly name for hello topology
        
        config:                 # Topology configuration
        topology.workers: 1     # Hint for hello number of workers toocreate
        
        spouts:                 # Spout definitions
        - id: "sentence-spout"
            className: "com.microsoft.example.RandomSentenceSpout"
            parallelism: 1      # parallelism hint
        
        bolts:                  # Bolt definitions
        - id: "splitter-bolt"
            className: "com.microsoft.example.SplitSentence"
            parallelism: 1
         
        - id: "counter-bolt"
            className: "com.microsoft.example.WordCount"
            constructorArgs:
                - 10
            parallelism: 1
        
        streams:                # Stream definitions
            - name: "Spout --> Splitter" # name isn't used (placeholder for logging, UI, etc.)
            from: "sentence-spout"       # hello stream emitter
            to: "splitter-bolt"          # hello stream consumer
            grouping:                    # Grouping type
                type: SHUFFLE
          
            - name: "Splitter -> Counter"
            from: "splitter-bolt"
            to: "counter-bolt"
            grouping:
            type: FIELDS
                args: ["word"]           # field(s) toogroup on

3. Ellenőrizze a következő módosításokat toohello hello `pom.xml` fájlt.
   
   * Adja hozzá a következő új függőséghez hello hello `<dependencies>` szakasz:
     
        ```xml
        <!-- Add a dependency on hello Flux framework -->
        <dependency>
            <groupId>org.apache.storm</groupId>
            <artifactId>flux-core</artifactId>
            <version>${storm.version}</version>
        </dependency>
        ```
   * Adja hozzá a következő beépülő modul toohello hello `<plugins>` szakasz. A beépülő modul hello projekt hello létrehozása (jar-fájlt) csomag kezeli, és néhány átalakítások adott tooFlux vonatkozik hello csomag létrehozásakor.
     
        ```xml
        <!-- build an uber jar -->
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-shade-plugin</artifactId>
            <version>2.3</version>
            <configuration>
                <transformers>
                    <!-- Keep us from getting a "can't overwrite file error" -->
                    <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer" />
                    <transformer implementation="org.apache.maven.plugins.shade.resource.ServicesResourceTransformer" />
                    <!-- We're using Flux, so refer tooit as main -->
                    <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                        <mainClass>org.apache.storm.flux.Flux</mainClass>
                    </transformer>
                </transformers>
                <!-- Keep us from getting a bad signature error -->
                <filters>
                    <filter>
                        <artifact>*:*</artifact>
                        <excludes>
                            <exclude>META-INF/*.SF</exclude>
                            <exclude>META-INF/*.DSA</exclude>
                            <exclude>META-INF/*.RSA</exclude>
                        </excludes>
                    </filter>
                </filters>
            </configuration>
            <executions>
                <execution>
                    <phase>package</phase>
                    <goals>
                        <goal>shade</goal>
                    </goals>
                </execution>
            </executions>
        </plugin>
        ```

   * A hello **exec-maven-beépülő modul** `<configuration>` területen hello értékének módosításához `<mainClass>` túl`org.apache.storm.flux.Flux`. Ez a beállítás lehetővé teszi, hogy a hello topológia helyben fut a fejlesztési fluxus toohandle.

   * A hello `<resources>` területen írja be a következő toohello hello `<includes>`. Az XML-kód hello YAM definícióját tartalmazó hello topológia hello projekt részeként tartalmazza.

        ```xml
        <include>topology.yaml</include>
        ```

## <a name="test-hello-flux-topology-locally"></a>Hello fluxus topológia helyi tesztelése

1. A következő toocompile hello használja, és hajtsa végre a hello fluxus topológia Maven használatával:

    ```bash
    mvn compile exec:java -Dexec.args="--local -R /topology.yaml"
    ```

    PowerShell, a következő parancs használata hello használata:

    ```bash
    mvn compile exec:java "-Dexec.args=--local -R /topology.yaml"
    ```

    > [!WARNING]
    > Ha a topológia a Storm 1.0.1-es bits használ, ez a parancs sikertelen lesz. Ez a hiba oka [https://issues.apache.org/jira/browse/STORM-2055](https://issues.apache.org/jira/browse/STORM-2055). Ehelyett [Storm a fejlesztési környezet telepítése](http://storm.apache.org/releases/0.10.0/Setting-up-development-environment.html) és hello használja a következő információkat.

    Ha rendelkezik [Storm a fejlesztési környezetben telepített](http://storm.apache.org/releases/0.10.0/Setting-up-development-environment.html), ehelyett a következő parancsok hello használhatja:

    ```bash
    mvn compile package
    storm jar target/WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local -R /topology.yaml
    ```

    Hello `--local` paraméter hello topológia módban fut helyi a fejlesztési környezetet. Hello `-R /topology.yaml` paramétert használ hello `topology.yaml` hello jar fájl toodefine hello topológia erőforrás fájlt.

    Futtatja, a hello topológia indítási információkat jeleníti meg. a következő szöveg hello hello kimeneti példája:

        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word snow
        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word white
        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 112 for word seven
        17:33:27 [Thread-16-count] INFO  com.microsoft.example.WordCount - Emitting a count of 195 for word the
        17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 113 for word and
        17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word dwarfs

    Van egy 10 másodperces késleltetési kötegek naplózott adatok között.

2. Másolatot készít hello `topology.yaml` fájl hello projektből. Nevű hello új fájl `newtopology.yaml`. A hello `newtopology.yaml` fájlt, keresse meg hello következő szakaszt, és hello értékének módosítása `10` túl`5`. A módosítás módosítások hello időközétől kibocsátó Word kötegek száma 10 másodperc too5 a.

    ```yaml
    - id: "counter-bolt"
    className: "com.microsoft.example.WordCount"
    constructorArgs:
    - 5
    parallelism: 1
    ```yaml

3. toorun hello topology, use hello following command:

    ```bash
    mvn exec:java -Dexec.args="--local /path/to/newtopology.yaml"
    ```

    Vagy ha a fejlesztési környezet Storm rendelkezik:

    ```bash
    storm jar target/WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local /path/to/newtopology.yaml
    ```

    Változás hello `/path/to/newtopology.yaml` toohello elérési út toohello newtopology.yaml fájl hello előző lépésben létrehozott. Ez a parancs hello newtopology.yaml hello topológia definíciófrissítések használja. Mivel nem magában foglalja az hello `compile` paraméter, a Maven hello projektet egy előző lépésekben hello verzióját használja.

    Egyszer hello topológia elindul, kell figyelje meg, hogy a hello idő közötti kibocsátott kötegek newtopology.yaml tooreflect hello értéke megváltozott. Így tudja módosítani a konfigurációs YAM fájl anélkül, hogy toorecompile hello topológia látható.

Ezeket és más szolgáltatások hello fluxus keretrendszer további információkért lásd: [fluxus (https://storm.apache.org/releases/0.10.0/flux.html)](https://storm.apache.org/releases/0.10.0/flux.html).

## <a name="trident"></a>Trident

Trident egy magas szintű absztrakció, Storm által biztosított. Állapot-nyilvántartó feldolgozási támogatja. hello elsődleges Trident előnye, hogy azt is garantálni hello topológia kerül minden üzenetet csak egyszer dolgozza fel. A Trident ezzel szemben nélkül a topológia is csak garantálja, hogy az üzenetek legalább egyszer fel. Is más különbségek vannak, például a beépített összetevők boltokhoz létrehozása helyett használható. Valójában boltokhoz kevésbé általános összetevők, például a szűrőket, a leképezések és a funkciók helyébe lép.

Trident alkalmazások Maven-projektek használatával is létrehozható. Hello használata azonos basic az ebben a cikkben bemutatott lépések – csak hello kód nem egyezik. Trident is nem (jelenleg) használható hello fluxus keretrendszer.

A Trident kapcsolatos további információkért lásd: hello [Trident API – áttekintés](http://storm.apache.org/documentation/Trident-API-Overview.html).

A Trident alkalmazás példáért lásd: [trendekkel kapcsolatos témakörök a HDInsight alatt futó Apache Storm Twitter](hdinsight-storm-twitter-trending.md).

## <a name="next-steps"></a>Következő lépések

Megtanulta, hogyan toocreate a Storm-topológia Java használatával. Most megtudhatja, hogyan:

* [Központi telepítése és kezelése a HDInsight alatt futó Apache Storm-topológiák](hdinsight-storm-deploy-monitor-topology.md)

* [Visual Studio használatával HDInsight alatt futó Apache Storm a C#-topológiák fejlesztése](hdinsight-storm-develop-csharp-visual-studio-topology.md)

Található további példa Storm-topológiák ellátogatva [a HDInsight alatt futó Storm](hdinsight-storm-example-topology.md).

