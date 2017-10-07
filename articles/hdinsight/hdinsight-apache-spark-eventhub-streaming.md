---
title: aaaUse Apache Spark streaming az Event Hubs az Azure HDInsight |} Microsoft Docs
description: "Az Apache Spark streamelési minta build toosend egy adatok tooAzure Eseményközpont-adatfolyam és majd scala-alkalmazások segítségével HDInsight Spark-fürt kap, ezeket az eseményeket."
keywords: "Apache spark streaming, spark streamelési, spark minta, apache spark streamelési, például az esemény hub azure-minta, spark-minta"
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 68894e75-3ffa-47bd-8982-96cdad38b7d0
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: nitinme
ms.openlocfilehash: 10cc5884047b3b8249fe8a8822a16a19780a4af3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="apache-spark-streaming-process-data-from-azure-event-hubs-with-spark-cluster-on-hdinsight"></a>Apache Spark streaming: a HDInsight fürt adatfeldolgozásra Spark az Azure Event hubs

Ebben a cikkben az Apache Spark streaming mintát, amely magában foglalja az alábbi lépésekkel hello hoz létre:

1. Az Azure Event Hubs egy önálló alkalmazás tooingest üzenetek használhatja.

2. A két különböző szempontok, akkor hello üzeneteket beolvasni az Event Hubs a valós idejű Azure HDInsight Spark-fürtön futó alkalmazást használ.

3. Adatfolyam-továbbítási elemzőfolyamatokat toopersist adatok toodifferent tárolási rendszereket építhetnek, vagy mélyebb betekintés az adatok hello menet közben.

## <a name="prerequisites"></a>Előfeltételek

* Azure-előfizetés. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

* A HDInsight az Apache Spark-fürt. Útmutatásért lásd: [létrehozása az Apache Spark on Azure hdinsight clusters](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="spark-streaming-concepts"></a>Spark Streaming fogalmak

A Spark streamelési részletes ismertetése, [Apache Spark streaming áttekintése](http://spark.apache.org/docs/latest/streaming-programming-guide.html#overview). HDInsight során az Azure-fürt azonos adatfolyam-továbbítási funkciók tooa Spark hello.  

## <a name="what-does-this-solution-do"></a>Mire ehhez a megoldáshoz?

Ebben a cikkben, Spark streamelési példa toocreate hajtsa végre a lépéseket követve hello:

1. Hozzon létre egy Azure Event hubs Eseményközpontot, akik szintén megkapják az események adatfolyam.

2. Futtassa egy helyi önálló alkalmazás, amely eseményeket hoz létre, és leküldéses értesítések az Azure Event Hubs toohello. hello mintaalkalmazás, amely ezt a közzétett [https://github.com/hdinsight/spark-streaming-data-persistence-examples](https://github.com/hdinsight/spark-streaming-data-persistence-examples).

3. Távolról futtatni egy adatfolyam-továbbítási alkalmazást egy Spark-fürt, amely az Azure Event Hubs olvassa be az esemény streamelését és végezhetnek különböző adatfeldolgozási /.

## <a name="create-an-azure-event-hub"></a>Hozzon létre egy Azure Event Hubs

1. Jelentkezzen be toohello [Azure Portal](https://ms.portal.azure.com), és kattintson **új** hello: bal felső részén üdvözlő képernyőt.

2. Kattintson az **Eszközök internetes hálózata**, majd az **Event Hubs** lehetőségre.

    ![Spark streaming példa eseményközpont létrehozása](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-create-event-hub-for-spark-streaming.png "Spark streamelési példa eseményközpont létrehozása")

3. A hello **névtér létrehozása** panelen adja meg a névtér nevét. Válassza ki a hello tarifacsomag (alapszintű vagy Standard). Válassza ki, egy Azure-előfizetéssel, erőforráscsoportot és helyet az erőforrás-toocreate hello. Kattintson a **létrehozása** toocreate hello névtér.

      ![Adjon meg a Spark streamelési példa event hub nevet](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-provide-event-hub-name-for-spark-streaming.png "adja meg az eseményközpont neveként a Spark streamelési – példa")

    > [!NOTE]
    > Meg kell válasszon hello azonos **hely** a Apache Spark-fürt a HDInsight tooreduce késleltetés és a költségek.
    >
    >

4. Hello Event Hubs-névtér listájában kattintson hello újonnan létrehozott névtér.      


5. Hello névtér paneljén kattintson **Event Hubs**, és kattintson a **+ Eseményközpont** toocreate egy új Eseményközpont.
   
    ![Spark streaming példa eseményközpont létrehozása](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-open-event-hubs-blade-for-spark-streaming-example.png "Spark streamelési példa eseményközpont létrehozása")

6. Adjon meg egy nevet az Eseményközpont set hello partíció száma too10 és üzenet megőrzési too1. Jelenleg nem archiválni köszönőüzenetei ebben a megoldásban, az alapértelmezett hello rest hagyja, és kattintson **létrehozása**.
   
    ![Adja meg a Spark streaming példa event hub-adatok](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-provide-event-hub-details-for-spark-streaming-example.png "Spark streamelési példa event hub-adatok megadása")

7. az újonnan létrehozott Eseményközpont hello hello Event Hub panel jelenik meg.
    
     ![Az Event Hubs megtekintése hello Spark streamelési például](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-view-event-hub-for-spark-streaming-example.png "nézet Eseményközpont hello Spark streamelési – példa")

8. Hello névtér (nem hello adott Event Hub panel) panelen, kattintson **megosztott elérési házirendek**, és kattintson a **RootManageSharedAccessKey**.
    
     ![Az Event Hubs házirendbe hello Spark streamelési példa](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-set-event-hub-policies-for-spark-streaming-example.png "hello szabályzatok beállítása az Event Hubs Spark streamelési – példa")

9. Kattintson a hello Másolás gombra toocopy hello **RootManageSharedAccessKey** elsődleges kulcs és a kapcsolati karakterlánc toohello vágólapra. Mentse a toouse hello oktatóanyag későbbi részében.
    
     ![Az Event Hubs házirend kulcsok hello Spark streamelési például megtekintése](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-view-event-hub-policy-keys.png "nézet Eseményközpont házirend kulcsokat hello Spark streamelési – példa")

## <a name="send-messages-tooazure-event-hub-using-a-sample-scala-application"></a>Az Event Hubs egy mintaalkalmazás Scala használatával üzenetek tooAzure küldése

Ebben a szakaszban egy különálló helyi Scala alkalmazás, amely események adatfolyam hoz létre, és elküldi azt korábban létrehozott Eseményközpont tooAzure használja. Ez az alkalmazás érhető el a Githubon: [https://github.com/hdinsight/eventhubs-sample-event-producer](https://github.com/hdinsight/eventhubs-sample-event-producer). Itt a hello lépések azt feltételezik, hogy Ön rendelkezik már ágazik el a GitHub-tárházban.

1. Ellenőrizze, hogy a hello következő hello ezt az alkalmazást futtató számítógépre telepített.

    * Oracle Java fejlesztői készlet. A későbbiekben telepítheti az [Itt](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).
    * Apache Maven. Letöltheti a [Itt](https://maven.apache.org/download.cgi). Útmutatás tooinstall Maven érhetők el [Itt](https://maven.apache.org/install.html).

2. Nyisson meg egy parancssort, és keresse meg a klónozott hello GitHub-tárház hello Scala mintaalkalmazás toohello helyét, és futtassa a következő parancs toobuild hello alkalmazás hello.

        mvn package

3. hello kimeneti jar hello alkalmazás **com-microsoft-azure-eventhubs-client-example-0.2.0.jar**, alatt létrejön **/target** könyvtár. Ez a cikk tootest hello teljes megoldás később JAR használhatja.

## <a name="create-application-tooreceive-messages-from-event-hub-into-a-spark-cluster"></a>Az Event Hubs tooreceive alkalmazásüzeneteket a Spark-fürt létrehozása 

Spark Streaming két megközelítés tooconnect és az Azure Event Hubs, a fogadó-alapú kapcsolat és a közvetlen DStream-alapú kapcsolat van. Közvetlen DStream-alapú van a Jan 2017 hello 2.0.3 kiadásban bevezetett. Azt kellene tooreplace hello eredeti fogadó-alapú kapcsolat, mert az több performant és erőforrás-hatékony. További részletek találhatók [https://github.com/hdinsight/spark-eventhubs](https://github.com/hdinsight/spark-eventhubs). Közvetlen DStream csak a Spark 2.0 + támogatja.

### <a name="build-applications-with-hello-dependency-toospark-eventhubs-connector"></a>Hello függőségi toospark-eventhubs összekötőn keresztül használó alkalmazások

Azt is közzétesz hello átmeneti Spark-EventHubs a Githubon verzióját. toouse hello átmeneti a Spark-EventHubs, hello első lépéseként verziója tooindicate GitHub, a forrás-tárház hello adja hozzá a következő bejegyzés toopom.xml hello:

```xml
<repository>
      <id>spark-eventhubs</id>
      <url>https://raw.github.com/hdinsight/spark-eventhubs/maven-repo/</url>
      <snapshots>
        <enabled>true</enabled>
        <updatePolicy>always</updatePolicy>
      </snapshots>
</repository>
```

A következő függőségi tooyour projekt tootake hello előzetes kiadását hello majd adhat hozzá.

Maven-függőség

```xml
<!-- https://mvnrepository.com/artifact/com.microsoft.azure/spark-streaming-eventhubs_2.11 -->
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>spark-streaming-eventhubs_2.11</artifactId>
    <version>2.0.4</version>
</dependency>
```

SBT függőség

```
// https://mvnrepository.com/artifact/com.microsoft.azure/spark-streaming-eventhubs_2.11
libraryDependencies += "com.microsoft.azure" % "spark-streaming-eventhubs_2.11" % "2.0.4"
```

### <a name="direct-dstream-connection"></a>Közvetlen DStream kapcsolat

A letölthető egy előre elkészített jar-fájlt tartalmazó példák segítségével közvetlen DStream [http://central.maven.org/maven2/com/microsoft/azure/spark-streaming-eventhubs_2.11/2.0.4/spark-streaming-eventhubs_2.11-2.0.4.jar](http://central.maven.org/maven2/com/microsoft/azure/spark-streaming-eventhubs_2.11/2.0.4/spark-streaming-eventhubs_2.11-2.0.4.jar).

hello jar-fájlt tartalmaz három példák, amelyek a forráskód érhetők el [https://github.com/hdinsight/spark-eventhubs/tree/master/examples/src/main/scala/com/microsoft/spark/streaming/examples/directdstream](https://github.com/hdinsight/spark-eventhubs/tree/master/examples/src/main/scala/com/microsoft/spark/streaming/examples/directdstream).

Véve [WindowingWordCount](https://github.com/hdinsight/spark-eventhubs/blob/master/examples/src/main/scala/com/microsoft/spark/streaming/examples/directdstream/WindowingWordCount.scala) példa:

```scala
private def createStreamingContext(
  sparkCheckpointDir: String,
  batchDuration: Int,
  namespace: String,
  eventHunName: String,
  eventhubParams: Map[String, String],
  progressDir: String) = {
val ssc = new StreamingContext(new SparkContext(), Seconds(batchDuration))
ssc.checkpoint(sparkCheckpointDir)
val inputDirectStream = EventHubsUtils.createDirectStreams(
  ssc,
  namespace,
  progressDir,
  Map(eventHunName -> eventhubParams))

inputDirectStream.map(receivedRecord => (new String(receivedRecord.getBody), 1)).
  reduceByKeyAndWindow((v1, v2) => v1 + v2, (v1, v2) => v1 - v2, Seconds(batchDuration * 3),
    Seconds(batchDuration)).print()

ssc
}

def main(args: Array[String]): Unit = {

if (args.length != 8) {
  println("Usage: program progressDir PolicyName PolicyKey EventHubNamespace EventHubName" +
    " BatchDuration(seconds) Spark_Checkpoint_Directory maxRate")
  sys.exit(1)
}

val progressDir = args(0)
val policyName = args(1)
val policykey = args(2)
val namespace = args(3)
val name = args(4)
val batchDuration = args(5).toInt
val sparkCheckpointDir = args(6)
val maxRate = args(7)

val eventhubParameters = Map[String, String] (
  "eventhubs.policyname" -> policyName,
  "eventhubs.policykey" -> policykey,
  "eventhubs.namespace" -> namespace,
  "eventhubs.name" -> name,
  "eventhubs.partition.count" -> "32",
  "eventhubs.consumergroup" -> "$Default",
  "eventhubs.maxRate" -> s"$maxRate"
)

val ssc = StreamingContext.getOrCreate(sparkCheckpointDir,
  () => createStreamingContext(sparkCheckpointDir, batchDuration, namespace, name,
    eventhubParameters, progressDir))

ssc.start()
ssc.awaitTermination()
}
```

A fenti példában hello `eventhubParameters` vannak hello paraméterek adott tooa EventHubs egypéldányos, és rendelkezik toopass azt toohello `createDirectStreams` API-hoz létre egy közvetlen DStream objektum leképezési tooa Event Hubs névtér. Spark Streamelési API keretrendszer által biztosított DStream API-k hívása hello közvetlen DStream objektum. Ebben a példában a Microsoft hello utolsó 3 micro kötegelt időközök belül minden szó hello gyakorisága számítható ki.

### <a name="receiver-based-connection"></a>Fogadó-alapú kapcsolat

A Spark streaming események és útvonal hello toodifferent célok kap, scalában írt mintaalkalmazás érhető el: [https://github.com/hdinsight/spark-streaming-data-persistence-examples](https://github.com/hdinsight/spark-streaming-data-persistence-examples). Kövesse az alábbi tooupdate hello alkalmazásban az Eseményközpont konfigurációja hello lépéseket, és hozzon létre hello kimeneti jar.

1. Indítsa el az IntelliJ IDEA, és a hello indítsa el a képernyőn válassza ki **tekintse meg a verziókövetés** majd **Git**.
   
    ![Apache Spark streaming példa - get forrásokból származó Git](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-get-source-from-git.png "Apache Spark streaming példa - get forrásokból származó Git")

2. A hello **klónozott tárház** párbeszédpanelen adja meg a hello URL-cím toohello Git tárház tooclone származó, adja meg a hello directory tooclone, és kattintson **Klónozás**.
   
    ![Apache Spark streaming példa - klón a Git](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-clone-from-git.png "Apache Spark streaming példa - klón a Git")
3. Hello útmutatást követve keretein belül hello projekt teljesen klónozták. Nyomja le az **Alt + 1** tooopen hello **Projektnézetében**. Az alábbi hello kell hasonlítania.
   
    ![Apache Spark streaming példa - projekt nézetben](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-project-view.png "Apache Spark streaming példa - projekt nézetben")
4. Ellenőrizze, hogy hello alkalmazáskód fordítása során Java8. tooensure, kattintson **fájl**, kattintson a **szerkezetének**, és a hello **projekt** lapra, győződjön meg arról, hogy a projekt nyelvi szintje túl**8 - lambda kifejezések, típusa jegyzetek stb**.
   
    ![Apache Spark streaming példa - beállítása fordító](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-java-8-compiler.png "Apache Spark streaming példa - beállítása fordító")
5. Nyissa meg hello **pom.xml** és ellenőrizze, hogy hello Spark verziója megfelelő. A `<properties>` csomópont, keresse meg a következő kódrészletet hello és hello Spark verziójának ellenőrzése.

        <scala.version>2.11.8</scala.version>
        <scala.compat.version>2.11.8</scala.compat.version>
        <scala.binary.version>2.11</scala.binary.version>
        <spark.version>2.0.0</spark.version>

6. hello az alkalmazáshoz egy függőségi jar nevű **JDBC illesztőprogram jar**. Ez az Event Hubs kapott az Azure SQL adatbázishoz szükséges toowrite köszönőüzenetei. Letöltheti a jar (v4.1-es vagy újabb) a [Itt](https://msdn.microsoft.com/sqlserver/aa937724.aspx). Adja hozzá a hivatkozás toothis jar hello projekt könyvtárában. Hajtsa végre az alábbi lépésekkel hello:
     
     1. IntelliJ IDEA ablakban nyissa meg a hello alkalmazás esetében, kattintson a **fájl**, kattintson a **szerkezetének**, és kattintson a **szalagtárak**. 
     2. Hello kattintson hozzáadása ikon (![hozzáadása ikon](./media/hdinsight-apache-spark-eventhub-streaming/add-icon.png)), kattintson a **Java**, és navigáljon a hello JDBC illesztőprogram jar kezelőportálon toohello helyét. Hajtsa végre a hello kér tooadd hello jar fájl toohello projekt könyvtár.

         ![Adja hozzá a Hiányzó függőségek](./media/hdinsight-apache-spark-eventhub-streaming/add-missing-dependency-jars.png "adja hozzá a hiányzó függőség JAR-fájlok kivételével")
     3. Kattintson az **Alkalmaz** gombra.

7. Hozzon létre hello kimeneti jar-fájlra. Hajtsa végre a következő lépéseket hello.

   1. A hello **szerkezetének** párbeszédpanel, kattintson a **összetevők** , majd a hello plusz jelre. A hello előugró párbeszédpanelen kattintson az **JAR**, és kattintson a **a függőségekkel rendelkező modulok**.      
       
       ![Apache Spark streamelési példa - JAR létrehozása](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-create-jar.png "Apache Spark streamelési példa - JAR létrehozása")
   2. A hello **modulokban létrehozása JAR** párbeszédpanel kattintson hello három pont (![három pont](./media/hdinsight-apache-spark-eventhub-streaming/ellipsis.png)) hello elleni **fő osztály**.
   3. A hello **fő osztály kiválasztása** párbeszédpanel mezőben, válassza ki valamelyik hello elérhető osztályok, és kattintson a **OK**.
      
       ![Apache Spark streaming példa - jar válassza osztály](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-select-class-for-jar.png "Apache Spark streaming példa - jar válassza osztály")
   4. A hello **modulokban létrehozása JAR** párbeszédpanelen győződjön meg arról, hogy hello beállítás túl**toohello cél JAR kibontása** van kiválasztva, és kattintson **OK**. Ez minden függőség egyetlen JAR hoz létre.
      
       ![Apache Spark streamelési példa - modulok jar létrehozása](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-create-jar-from-modules.png "Apache Spark streamelési példa - modulok jar létrehozása")
   5. Hello **kimeneti elrendezés** lap felsorolja az összes hello JAR-fájlok kivételével, amelyek tartalmazzák a hello Maven project. Kiválaszthatja, és azokat, amelyeken hello Scala alkalmazás törlése hello nincs közvetlen függőség van. Itt létrehozzuk hello alkalmazáshoz, akkor eltávolíthatja az összes, de az utolsó hello (**spark-adatfolyam-adatok-adatmegőrzési-példák fordítási kimeneti**). Hello JAR-fájlok kivételével toodelete válasszon, majd kattintson a hello **törlése** ikon (![törlés ikonja](./media/hdinsight-apache-spark-eventhub-streaming/delete-icon.png)).
      
       ![Apache Spark streaming példa - kibontott delete JAR-fájlok kivételével](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-delete-output-jars.png "Apache Spark streaming - kibontott delete JAR-fájlok kivételével – példa")
      
       Győződjön meg arról, hogy **győződjön Build** be van jelölve, amely biztosítja, hogy hello jar minden alkalommal létrejön hello projekt beépített vagy frissíteni. Kattintson az **Alkalmaz** gombra.
   6. A hello **kimeneti elrendezés** lapon jobb alján hello hello **rendelkezésre álló elemek** mezőben van hello SQL JDBC jar, hogy hozzáadta a korábbi toohello projekt könyvtár. Hozzá kell adni a toohello **kimeneti elrendezés** fülre. Kattintson a jobb gombbal a hello jar-fájlra, és kattintson **bontsa ki a kimeneti gyökér**.
      
       ![Apache Spark streaming példa - kivonat függőségi jar](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-extract-dependency-jar.png "Apache Spark streaming példa - kivonat függőségi jar")  
      
       Hello **kimeneti elrendezés** lapon most példához hasonló.
      
       ![Apache Spark streaming példa - végső kimenetet lapon](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-final-output-tab.png "Apache Spark streaming példa - végső kimenetet lap")        
      
       A hello **szerkezetének** párbeszédpanel, kattintson a **alkalmaz** majd **OK**.    
   7. Hello menüsorában kattintson **Build**, és kattintson a **ellenőrizze projekt**. Is **Build összetevők** toocreate hello jar. hello kimeneti jar alatt létrejön **\classes\artifacts**.
      
       ![Apache Spark streamelési példa - kimeneti JAR](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-output-jar.png "Apache Spark streamelési példa - kimeneti JAR")

## <a name="run-hello-application-remotely-on-a-spark-cluster-using-livy"></a>Hello alkalmazás távoli futtatása Spark-fürtön Livy használatával

Ez a cikk használhatja Livy toorun hello Apache Spark streamelési alkalmazás távolról Spark-fürtön. A hogyan toouse Livy HDInsight Spark a fürt részletes leírását lásd: [küldés feladatok távolról tooan Apache Spark fürtön Azure HDInsight](hdinsight-apache-spark-livy-rest-interface.md). Előtt hello Spark adatfolyam-továbbítási alkalmazást futtat, van néhány dolgot kell tennie:

1. Indítsa el a hello helyi önálló toogenerate eseményeket, és tooEvent Hub küldött. Ezért a következő parancs toodo hello használata:

        java -cp com-microsoft-azure-eventhubs-client-example-0.2.0.jar com.microsoft.eventhubs.client.example.EventhubsClientDriver --eventhubs-namespace "mysbnamespace" --eventhubs-name "myeventhub" --policy-name "mysendpolicy" --policy-key "<policy key>" --message-length 32 --thread-count 32 --message-count -1

2. Másolás hello streaming jar (**spark-adatfolyam-adatok-adatmegőrzési-examples.jar**) toohello hello-fürthöz tartozó Azure Blob Storage tárolóban. Így hello jar elérhető tooLivy. Használhat [ **AzCopy**](../storage/common/storage-use-azcopy.md), a parancs így sor segédprogram, toodo. Nincsenek sokkal más ügyfelek tooupload adatok használatával. A rájuk vonatkozó további található [feltölteni az adatokat a HDInsight Hadoop-feladatok](hdinsight-upload-data.md).
3. Telepítse a CURL hello számítógépen az alkalmazások futtatása során. CURL tooinvoke hello Livy végpontok toorun hello feladatok távolról használjuk.

### <a name="run-hello-spark-streaming-application-tooreceive-hello-events-into-an-azure-storage-blob-as-text"></a>Egy Azure Storage-Blobba való hello Spark streamelési tooreceive hello alkalmazásesemények futtató szöveg

Nyisson meg egy parancssort, keresse meg a toohello könyvtárba CURL telepítette, és futtassa a következő parancsot (a név felülírandó felhasználónév/jelszó és a fürt nevét) hello:

    curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\inputBlob.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

hello fájlban paraméterek hello **inputBlob.txt** az alábbiak szerint definiáltuk:

    { "file":"wasb:///example/jars/spark-streaming-data-persistence-examples.jar", "className":"com.microsoft.spark.streaming.examples.workloads.EventhubsEventCount", "args":["--eventhubs-namespace", "mysbnamespace", "--eventhubs-name", "myeventhub", "--policy-name", "myreceivepolicy", "--policy-key", "<put-your-key-here>", "--consumer-group", "$default", "--partition-count", 10, "--batch-interval-in-seconds", 20, "--checkpoint-directory", "/EventCheckpoint", "--event-count-folder", "/EventCount/EventCount10"], "numExecutors":20, "executorMemory":"1G", "executorCores":1, "driverMemory":"2G" }

Ossza meg velünk megismeréséhez hello paraméterek hello bemeneti fájlban:

* **fájl** hello elérési toohello alkalmazás jar fájl hello hello-fürthöz tartozó Azure storage-fiók.
* **Osztálynév** hello hello jar hello osztály neve.
* **argumentum** hello osztály kötelező argumentumok hello listája
* **numExecutors** mag, Spark toorun hello adatfolyam-alkalmazás által használt hello száma. Mindig legyen az Event Hubs partíciók legalább két alkalommal hello száma.
* **executorMemory**, **executorCores**, **driverMemory** használt paraméterek tooassign szükséges erőforrások toohello adatfolyam-továbbítási alkalmazások vannak.

> [!NOTE]
> Nem kell toocreate hello kimeneti mappák (EventCheckpoint, EventCount/EventCount10), amelyek paraméterekként. adatfolyam-alkalmazás hello létrehozza azt.
>
>

Hello parancs futtatásakor hello hasonló kimenetnek kell megjelennie:

    < HTTP/1.1 201 Created
    < Content-Type: application/json; charset=UTF-8
    < Location: /18
    < Server: Microsoft-IIS/8.5
    < X-Powered-By: ARR/2.5
    < X-Powered-By: ASP.NET
    < Date: Tue, 01 Dec 2015 05:39:10 GMT
    < Content-Length: 37
    <
    {"id":1,"state":"starting","log":[]}* Connection #0 toohost mysparkcluster.azurehdinsight.net left intact

Jegyezze fel a hello Kötegazonosító hello utolsó sorában hello kimeneti (a példában az értéke "1"). sikeresen lefutott, az alkalmazás hello tooverify, megtalálhatja a hello-fürthöz tartozó Azure-tárfiókot és hello kell megjelennie **/EventCount/EventCount10** létrehozott mappába. Ebben a mappában kell tartalmaznia, amely rögzíti a megadott ideig tartó hello paraméter hello végrehajtva események hello száma blobok **batch-időköz-a-(másodperc)**.

hello Spark streamelési alkalmazás toorun folytatódik, amíg állítsa le. toodo Igen, használja a következő parancs hello:

    curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/1"

### <a name="run-hello-applications-tooreceive-hello-events-into-an-azure-storage-blob-as-json"></a>Hello alkalmazások futtatásához tooreceive hello eseményeket az Azure Storage-Blobba JSON-fájlként
Nyisson meg egy parancssort, keresse meg a toohello könyvtárba CURL telepítette, és futtassa a következő parancsot (a név felülírandó felhasználónév/jelszó és a fürt nevét) hello:

    curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\inputJSON.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

hello fájlban paraméterek hello **inputJSON.txt** az alábbiak szerint definiáltuk:

    { "file":"wasb:///example/jars/spark-streaming-data-persistence-examples.jar", "className":"com.microsoft.spark.streaming.examples.workloads.EventhubsToAzureBlobAsJSON", "args":["--eventhubs-namespace", "mysbnamespace", "--eventhubs-name", "myeventhub", "--policy-name", "myreceivepolicy", "--policy-key", "<put-your-key-here>", "--consumer-group", "$default", "--partition-count", 10, "--batch-interval-in-seconds", 20, "--checkpoint-directory", "/EventCheckpoint", "--event-count-folder", "/EventCount/EventCount10", "--event-store-folder", "/EventStore10"], "numExecutors":20, "executorMemory":"1G", "executorCores":1, "driverMemory":"2G" }

hello paraméterei hello szöveges kimenet hello előző lépésben megadott hasonló toowhat. Ebben az esetben nem kell toocreate hello kimeneti mappák (EventCheckpoint, EventCount/EventCount10), amelyek paraméterekként. adatfolyam-alkalmazás hello létrehozza azt.

 Miután hello parancs futtatása, megtalálhatja a hello-fürthöz tartozó Azure-tárfiók és hello kell megjelennie **/EventStore10** létrehozott mappába. Nyissa meg a fájl a következő előtaggal **rész -** és hello események feldolgozása JSON formátumban kell megjelennie.

### <a name="run-hello-applications-tooreceive-hello-events-into-a-hive-table"></a>A Hive tábla tooreceive hello események hello alkalmazások futtatásához
toorun hello adatfolyamok események egy Hive tábla, Spark streamelési alkalmazás kell néhány további összetevőket. Ezek a következők:

* datanucleus-api-jdo-3.2.6.jar
* datanucleus-rdbms-3.2.9.jar
* datanucleus-core-3.2.10.jar
* Hive-site.xml

Hello **.jar** fájlok érhetők el: a HDInsight Spark-fürt `/usr/hdp/current/spark-client/lib`. Hello **hive-site.xml** érhető el `/usr/hdp/current/spark-client/conf`.

Használhat [WinScp](http://winscp.net/eng/download.php) toocopy ezeket a fájlokat hello fürt tooyour helyi számítógépről. Ezek a fájlok tooyour tárfiók keresztül hello-fürthöz tartozó eszközök toocopy használhatja. Hogyan tooupload fájlok toohello tárfiók további információkért lásd: [feltölteni az adatokat a HDInsight Hadoop-feladatok](hdinsight-upload-data.md).

Miután átmásolta a keresztül hello fájlok tooyour Azure storage-fiók, nyisson meg egy parancssort, keresse meg a CURL telepítési toohello directory és futtassa a következő parancsot (a név felülírandó felhasználónév/jelszó és a fürt nevét) hello:

    curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\inputHive.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

hello fájlban paraméterek hello **inputHive.txt** az alábbiak szerint definiáltuk:

    { "file":"wasb:///example/jars/spark-streaming-data-persistence-examples.jar", "className":"com.microsoft.spark.streaming.examples.workloads.EventhubsToHiveTable", "args":["--eventhubs-namespace", "mysbnamespace", "--eventhubs-name", "myeventhub", "--policy-name", "myreceivepolicy", "--policy-key", "<put-your-key-here>", "--consumer-group", "$default", "--partition-count", 10, "--batch-interval-in-seconds", 20, "--checkpoint-directory", "/EventCheckpoint", "--event-count-folder", "/EventCount/EventCount10", "--event-hive-table", "EventHiveTable10" ], "jars":["wasb:///example/jars/datanucleus-api-jdo-3.2.6.jar", "wasb:///example/jars/datanucleus-rdbms-3.2.9.jar", "wasb:///example/jars/datanucleus-core-3.2.10.jar"], "files":["wasb:///example/jars/hive-site.xml"], "numExecutors":20, "executorMemory":"1G", "executorCores":1, "driverMemory":"2G" }

hello paraméterei hello szöveges kimenet hello előző lépésben megadott hasonló toowhat. Ebben az esetben nincs szükség toocreate hello kimeneti mappák (EventCheckpoint, EventCount/EventCount10) vagy hello kimeneti paraméterként használt Hive tábla (EventHiveTable10). adatfolyam-alkalmazás hello létrehozza azt. Vegye figyelembe, hogy hello **JAR-fájlok kivételével** és **fájlok** beállítás elérési utak toohello .jar fájlok és hello hive-site.xml toohello tárfiók keresztül másolt tartalmaz.

SSH alkalmas hello fürt és a Hive-lekérdezések futtatása, tooverify, amely hello hive tábla létrehozása sikeresen megtörtént. Útmutatásért lásd: [használja az SSH a HDInsight Hadoop Hive](hdinsight-hadoop-use-hive-ssh.md). Miután csatlakozott SSH használatával, futtathatja a következő parancs tooverify hello hello Hive táblát, **EventHiveTable10**, jön létre.

    show tables;

Egy kimeneti hasonló toohello következő kell megjelennie:

    OK
    eventhivetable10
    hivesampletable

A SELECT lekérdezés hello tábla tooview hello tartalmát is futtathatja.

    SELECT * FROM eventhivetable10 LIMIT 10;

Hello hasonló kimenetnek kell megjelennie:

    ZN90apUSQODDTx7n6Toh6jDbuPngqT4c
    sor2M7xsFwmaRW8W8NDwMneFNMrOVkW1
    o2HcsU735ejSi2bGEcbUSB4btCFmI1lW
    TLuibq4rbj0T9st9eEzIWJwNGtMWYoYS
    HKCpPlWFWAJILwR69MAq863nCWYzDEw6
    Mvx0GQOPYvPR7ezBEpIHYKTKiEhYammQ
    85dRppSBSbZgThLr1s0GMgKqynDUqudr
    5LAWkNqorLj3ZN9a2mfWr9rZqeXKN4pF
    ulf9wSFNjD7BZXCyunozecov9QpEIYmJ
    vWzM3nvOja8DhYcwn0n5eTfOItZ966pa
    Time taken: 4.434 seconds, Fetched: 10 row(s)


### <a name="run-hello-applications-tooreceive-hello-events-into-an-azure-sql-database-table"></a>Egy Azure SQL adatbázis táblába tooreceive hello események hello alkalmazások futtatásához
Ez a lépés futtatása előtt győződjön meg arról, hogy az Azure SQL-adatbázis létrehozása. Útmutatásért lásd: [SQL-adatbázis létrehozása percben](../sql-database/sql-database-get-started.md). toocomplete Ez részben, értékek adatbázisnév, adatbázis-kiszolgáló nevét és hello adatbázis rendszergazdai hitelesítő adatok paraméterként van szüksége. Toocreate hello adatbázistábla azonban nem kell. hello Spark adatfolyam-továbbítási alkalmazást hoz létre, amely meg.

Nyisson meg egy parancssort, keresse meg a toohello könyvtárba CURL telepítette, és futtassa a következő parancs hello:

    curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\inputSQL.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

hello fájlban paraméterek hello **inputSQL.txt** az alábbiak szerint definiáltuk:

    { "file":"wasb:///example/jars/spark-streaming-data-persistence-examples.jar", "className":"com.microsoft.spark.streaming.examples.workloads.EventhubsToAzureSQLTable", "args":["--eventhubs-namespace", "mysbnamespace", "--eventhubs-name", "myeventhub", "--policy-name", "myreceivepolicy", "--policy-key", "<put-your-key-here>", "--consumer-group", "$default", "--partition-count", 10, "--batch-interval-in-seconds", 20, "--checkpoint-directory", "/EventCheckpoint", "--event-count-folder", "/EventCount/EventCount10", "--sql-server-fqdn", "<database-server-name>.database.windows.net", "--sql-database-name", "mysparkdatabase", "--database-username", "sparkdbadmin", "--database-password", "<put-password-here>", "--event-sql-table", "EventContent" ], "numExecutors":20, "executorMemory":"1G", "executorCores":1, "driverMemory":"2G" }

sikeresen lefutott, az alkalmazás hello tooverify, toohello Azure SQL-adatbázis SQL Server Management Studio használatával is elérheti. Útmutatást, lásd: toodo [tooSQL adatbázis csatlakozzon az SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md). Ha csatlakoztatott toohello adatbázis, navigálhat toohello **EventContent** hello adatfolyam-alkalmazás által létrehozott tábla. Hello táblából futtatása egy gyors tooget hello adatait kérdezi le. Futtassa a következő lekérdezés hello:

    SELECT * FROM EventCount

Kimeneti hasonló toohello következő kell megjelennie:

    00046b0f-2552-4980-9c3f-8bba5647c8ee
    000b7530-12f9-4081-8e19-90acd26f9c0c
    000bc521-9c1b-4a42-ab08-dc1893b83f3b
    00123a2a-e00d-496a-9104-108920955718
    0017c68f-7a4e-452d-97ad-5cb1fe5ba81b
    001KsmqL2gfu5ZcuQuTqTxQvVyGCqPp9
    001vIZgOStka4DXtud0e3tX7XbfMnZrN
    00220586-3e1a-4d2d-a89b-05c5892e541a
    0029e309-9e54-4e1b-84be-cd04e6fce5ec
    003333cf-874f-4045-9da3-9f98c2b4ea49
    0043c07e-8d73-420a-9af7-1fcb94575356
    004a11a9-0c2c-4bc0-a7d5-2e0ebd947ab9


## <a name="seealso"></a>Lásd még:
* [Overview: Apache Spark on Azure HDInsight (Áttekintés: Apache Spark on Azure HDInsight)](hdinsight-apache-spark-overview.md)
* [Fogadó-alapú kapcsolat és a közvetlen DStream tervezése](https://www.slideshare.net/NanZhu/seattle-sparkmeetup032317)

### <a name="scenarios"></a>Forgatókönyvek
* [Spark és BI: Interaktív adatelemzés végrehajtása a Spark on HDInsight használatával, BI-eszközökkel](hdinsight-apache-spark-use-bi-tools.md)
* [Spark és Machine Learning: A Spark on HDInsight használata az épület-hőmérséklet elemzésére HVAC-adatok alapján](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark és Machine Learning: használja a Spark on HDInsight toopredict élelmiszervizsgálati eredmények](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [A webhelynapló elemzése a Spark on HDInsight használatával](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Alkalmazások létrehozása és futtatása
* [Önálló alkalmazás létrehozása a Scala használatával](hdinsight-apache-spark-create-standalone-application.md)
* [Feladatok távoli futtatása Spark-fürtön a Livy használatával](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Eszközök és bővítmények
* [Toocreate IntelliJ IDEA HDInsight-eszközei beépülő használja, és küldje el a Spark Scala applicatons](hdinsight-apache-spark-intellij-tool-plugin.md)
* [IntelliJ IDEA toodebug Spark-alkalmazások HDInsight-eszközei beépülő távolról használni](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Zeppelin notebookok használata Spark-fürttel HDInsighton](hdinsight-apache-spark-zeppelin-notebook.md)
* [Jupyter notebookokhoz elérhető kernelek a HDInsight Spark-fürtjében](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Külső csomagok használata Jupyter notebookokkal](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter telepítse a számítógépre, és csatlakozzon a HDInsight Spark-fürt tooan](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Erőforrások kezelése
* [Az Azure HDInsight hello Apache Spark-fürt erőforrásainak kezelése](hdinsight-apache-spark-resource-manager.md)
* [Apache Spark-fürtön futó feladatok nyomon követése és hibakeresése a HDInsightban](hdinsight-apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-create-storageaccount]: ../storage-create-storage-account/
