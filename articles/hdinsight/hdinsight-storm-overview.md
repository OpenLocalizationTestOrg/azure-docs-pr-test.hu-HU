---
title: aaaWhat az Apache Storm - Azure HDInsight |} Microsoft Docs
description: "Apache Storm lehetővé teszi a valós idejű tooprocess adatstreamek. Az Azure HDInsight lehetővé teszi a tooeasily hello Azure felhőalapú Storm-fürtök létrehozása. A Visual Studio használatával C# Storm megoldások létrehozásához, majd telepítse a HDInsight alatt futó Storm-fürtök tooyour."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
keywords: "apache storm használati esetek,storm-fürt,mi az apache storm"
ms.assetid: 72d54080-1e48-4a5e-aa50-cce4ffc85077
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/03/2017
ms.author: larryfr
ms.openlocfilehash: 6c6b2925ef3e5666dfecc3fb3c835bb362902c51
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-apache-storm-on-azure-hdinsight"></a>Mi az Azure HDInsight alatt futó Apache Storm?

Az [Apache Storm](http://storm.apache.org/) egy elosztott, nagy hibatűrésű, nyílt forráskódú számítási rendszer. A Storm tooprocess adatfolyamok valós idejű adatok Hadoop használható. A Storm megoldások garantált adatfeldolgozást is megadhatja, hello képességét tooreplay az adat nem sikeresen feldolgozott hello első alkalommal.

A HDInsight alatt futó Storm főbb előnyei a következő hello biztosítja:

* Felügyelt szolgáltatásként működik, szolgáltatásiszint-szerződésben garantált 99,9%-os elérhetőséggel.

* Könnyen testre szabható a szkriptek Storm-fürtön történő futtatásával a létrehozás során vagy után. További információ: [HDInsight-fürtök testre szabása szkriptműveletekkel](hdinsight-hadoop-customize-cluster-linux.md).

* Többféle nyelvet támogat. Egy szerkesztőprogramban, például Java C# vagy Python nyelven hello írhat Storm-összetevőket.

    * Integrálható a HDInsight Visual Studio hello fejlesztési, a felügyeleti és a C#-topológiák figyelését. További információkért lásd: [fejlesztésére C# Storm-topológiák a hello a HDInsight Tools for Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).

    * Támogatja a hello Trident Java kezelőfelülettel. Olyan Storm-topológiákat hozhat létre, amelyek támogatják az üzenetek pontosan egyszeri feldolgozását, a tranzakciós adattároló-megőrzést és számos gyakori Stream Analytics-műveletet.

*  Könnyen le- és felskálázhat Storm-fürtöket. Adja hozzá, vagy távolítsa el a munkavégző csomópont nincs hatása toorunning Storm-topológiák.

* Integrálható a hello Azure-szolgáltatások a következő:

    * Azure Event Hubs

    * Azure Virtual Network

    * Azure SQL Database

    * Azure Storage

    * Azure Cosmos DB

* Több HDInsight-fürtök hello képességeit biztonságosan egyesíti a virtuális hálózaton keresztül. Létrehozhat Storm-, Kafka-, Spark-, HBase- vagy Hadoop-fürtöket használó elemzőfolyamatokat.

A valós idejű elemzési megoldásaikhoz Apache Stormot használó vállalatok listája itt található: [Az Apache Stormot használó vállalatok](https://storm.apache.org/documentation/Powered-By.html).

Storm, használatának tooget lásd [beolvasása használatába a HDInsight alatt futó Storm][gettingstarted].

## <a name="how-does-storm-work"></a>Hogyan működik a Storm?

Storm futtatása helyett hello topológiák előfordulhat, hogy ismeri a MapReduce-feladatok. A Storm-topológiák több összetevőből állnak, amelyek egy irányított aciklikus gráfba (DAG) vannak rendezve. Hello Graph hello összetevők közötti adatáramlás. Minden összetevőbe egy vagy több stream érkezik be, valamint egy vagy több streamet sugároz. hello a következő ábra bemutatja, hogyan közötti adatáramlás egy alapszintű word-count topológiához összetevőket:

![Példa az összetevők elrendezésére egy Storm-topológiában](./media/hdinsight-storm-overview/example-apache-storm-topology-diagram.png)

* A spout-összetevők adatokat importálnak a topológiába. Ezek adatfolyamokat a hello topológia hozható létre.

* A bolt-összetevők a spout- vagy más bolt-összetevők által sugárzott streameket dolgozzák fel. Boltokhoz szükség lehet, hogy kibocsátás adatfolyamok hello topológia be. Szögek megtalálhatók tooexternal adatszolgáltatások vagy tárhelyen, például a HDFS, Kafka vagy HBase beírásáért felelős.

## <a name="ease-of-creation"></a>Könnyű létrehozás

Az új Storm-fürtök percek alatt kiépíthetők a HDInsightban. További információkat a Storm-fürtök létrehozásáról [a HDInsight alatt futó Storm bemutatásában](hdinsight-apache-storm-tutorial-get-started-linux.md) talál.

## <a name="ease-of-use"></a>Könnyű használat

* __Secure Shell (SSH) kapcsolat__: hello átjárócsomópontokkal a Storm-fürt hello interneten keresztül való hozzáféréshez SSH használatával. Az SSH használatával közvetlenül futtathat parancsokat a fürtön.

  További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

* __Webalkalmazás-kapcsolat__: az összes HDInsight-fürtök hello Ambari webes felhasználói felület adja meg. Könnyedén figyelheti, konfigurálása és kezelése a szolgáltatások a fürt hello Ambari webes felhasználói felület használatával. Storm-fürtök is hello Storm felhasználói felületén adja meg. Figyelheti, és kezelheti a futó Storm-topológiák a böngészőből hello Storm felhasználói felülete segítségével.

  További információkért lásd: hello [kezelése Hdinsightban az Ambari webes felhasználói felületén hello](hdinsight-hadoop-manage-ambari.md) és [figyelő és kezelhet a Storm felhasználói felülete hello](hdinsight-storm-deploy-monitor-topology-linux.md#monitor-and-manage-storm-ui) dokumentumok.

* __Az Azure PowerShell és az Azure parancssori felület__: PowerShell és a parancssori felületen egyaránt adja meg a parancssori segédeszközök is használhatja az ügyfél rendszer toowork HDInsight és más Azure-szolgáltatásokkal.

* __Visual Studio-integráció__: Azure Data Lake Tools for Visual Studio projektsablonjai a C# Storm-topológiák létrehozása hello SCP.Net keretrendszer használatával tartalmazza. A Data Lake Tools is biztosítanak az eszközök toodeploy, figyelheti és kezelheti a HDInsight alatt futó Storm-megoldások.

  További információkért lásd: [fejlesztésére C# Storm-topológiák a hello a HDInsight Tools for Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).

## <a name="integration-with-other-azure-services"></a>Integráció más Azure-szolgáltatásokkal

* __Azure Data Lake Store__: Egy példát találhat a Data Lake Store és egy Storm-fürt együttes használatára [az Azure Data Lake a HDInsight alatt futó Apache Stormmal történő használatát](hdinsight-storm-write-data-lake-store.md) bemutató cikkben.

* __Az Event Hubs__: példa egy Storm-fürt Event Hubs használatával, tekintse meg a következő dokumentumok hello:

    * [Java-alapú topológia fejlesztése HDInsight alatt futó Stormhoz](hdinsight-storm-develop-java-topology.md)

    * [Az Azure Event Hubs-eseményközpontok eseményeinek feldolgozása a HDInsight alatt futó Stormmal (C#)](hdinsight-storm-develop-csharp-event-hub-topology.md)

* __SQL-adatbázis__, __Cosmos DB__, __Event Hubs__, és __HBase__: sablon példákat szerepelnek hello Data Lake Tools for Visual Studio. További információk: [C#-topológiák fejlesztése a HDInsight alatt futó Stormmal](hdinsight-storm-develop-csharp-visual-studio-topology.md).

## <a name="reliability"></a>Megbízhatóság

Apache Storm garantálja, hogy minden bejövő üzenet mindig teljes mértékben fel, akkor is, ha hello adatok elemzése több száz csomópont között oszlik.

hello Nimbus csomópont biztosít funkció hasonló toohello Hadoop JobTracker, és ki feladatokat a fürt Zookeeper keresztül tooother csomópontján. Zookeeper csomópontok fürt koordinációt biztosítanak, és elősegítik a kommunikációt a Nimbus és hello hello munkavégző csomópontokhoz felügyelő folyamat között. Ha egy feldolgozó csomópont leáll, hello Nimbus csomópont tájékoztatást kapjon, és kiosztja hello feladat és a hozzájuk kapcsolódó adatok tooanother csomópont.

az Apache Storm-fürtök hello alapértelmezett konfigurációjának toohave csak egyetlen Nimbus csomóponttal. A HDInsight alatt futó Storm két Nimbus csomóponttal rendelkezik. Ha hello elsődleges csomópont meghibásodik, hello Storm-fürt toohello másodlagos csomópont vált, amíg az elsődleges csomópont hello helyre lett állítva. hello következő diagram azt ábrázolja, hello feladat Attribútumfolyam konfigurációjának alatt futó Storm példatopológiái:

![Diagram: Nimbus, Zookeeper és Supervisor](./media/hdinsight-storm-overview/nimbus.png)

## <a name="scale"></a>Méretezés

A HDInsight-fürtök dinamikusan méretezhetők munkavégző csomópontok hozzáadásával vagy eltávolításával. Ezt a műveletet adatfeldolgozás közben hajthatja végre.

> [!IMPORTANT]
> a méretezés során hozzáadott új csomópontok kihasználásához tootake toorebalance Storm-topológiák elindítása előtt lett megnövelve. hello foglalásiegység-méret van szüksége.

## <a name="support"></a>Támogatás

A HDInsight alatt futó Stormhoz teljes körű, vállalati szintű, folyamatos támogatás áll rendelkezésre. A HDInsight alatt futó Storm emellett szolgáltatásiszint-szerződésben garantált 99,9%-os elérhetőséggel rendelkezik. Ez azt jelenti, hogy garantáljuk, hogy egy Storm-fürt külső kapcsolattal rendelkeznek hello idő legalább 99,9 %.

További információk: [Azure-ügyfélszolgálat](https://azure.microsoft.com/support/options/).

## <a name="apache-storm-use-cases"></a>Apache Storm használati esetek

Az alábbiakban hello olyan gyakori forgatókönyveket tartalmaz, amelynek a HDInsight alatt futó Storm előfordulhat, hogy használja:

* Eszközök internetes hálózata (IoT)
* Csalások észlelése
* Közösségi hálók adatainak elemzése
* Kinyerés, átalakítás és betöltés (ETL)
* Hálózatfigyelés
* Keresés
* Mobilmarketing

Valós forgatókönyvekkel kapcsolatos további információkért lásd: hello [vállalatok hogyan használják a Storm](https://storm.apache.org/documentation/Powered-By.html) dokumentum.

## <a name="development"></a>Fejlesztés

A Data Lake Tools for Visual Studio használatával a .NET-fejlesztők C# nyelven tervezhetnek és valósíthatnak meg topológiákat. Létrehozhatók Java- és C#-összetevőket egyaránt használó hibrid topológiák is.

További információk: [C#-topológiák fejlesztése HDInsight alatt futó Stormra a Visual Studio használatával](hdinsight-storm-develop-csharp-visual-studio-topology.md)

Java megoldásokkal hello az Ön által választott IDE használatával is készíthet. További információk: [Java-topológiák fejlesztése a HDInsight alatt futó Stormmal](hdinsight-storm-develop-java-topology.md).

Python használt toodevelop Storm-összetevőket is lehet. További információkért lásd [a Storm-topológiák Python segítségével a HDInsighton való fejlesztésével](hdinsight-storm-develop-python-topology.md) kapcsolatos témakört.

## <a name="common-development-patterns"></a>Gyakori fejlesztési minták

### <a name="guaranteed-message-processing"></a>Garantált üzenetfeldolgozás

Az Apache Storm különböző szinteken biztosít garantált üzenetfeldolgozást. Például egy alapszintű Storm-alkalmazás „legalább egyszeri” feldolgozást tud garantálni, míg a Trident „pontosan egyszeri” feldolgozást.

További információk: [Adatfeldolgozási garancia](https://storm.apache.org/about/guarantees-data-processing.html) az apache.org webhelyen.

### <a name="ibasicbolt"></a>IBasicBolt

egy bemeneti rekord olvasása, nulla vagy több rekordokat kibocsátó mintáját hello és majd nyugtázása hello bemeneti rekord azonnal végén hello hello metódus végrehajtása közös. Storm lehetővé hello [IBasicBolt](https://storm.apache.org/releases/1.0.3/javadocs/org/apache/storm/topology/IBasicBolt.html) csatoló tooautomate ebben a mintában.

### <a name="joins"></a>Illesztések

Az adatstreamek alkalmazások közötti csatlakoztatásának különböző módjai. Például összeillesztheti több stream minden rekordját egy új streammé, vagy összeilleszthet csupán rekordkötegeket egy bizonyos ablak alapján. Az illesztés mindkét módszer esetén a [fieldsGrouping](http://storm.apache.org/releases/current/javadocs/org/apache/storm/topology/InputDeclarer.html#fieldsGrouping-java.lang.String-org.apache.storm.tuple.Fields-) használatával történik. Csoportosítás mezője egy mód annak definiálására, hogyan listának irányított toobolts.

A Java-példában a következő hello fieldsGrouping használt tooroute rekordokat eredő "1", "2" és "3" összetevők toohello MyJoiner bolt:

    builder.setBolt("join", new MyJoiner(), parallelism) .fieldsGrouping("1", new Fields("joinfield1", "joinfield2")) .fieldsGrouping("2", new Fields("joinfield1", "joinfield2")) .fieldsGrouping("3", new Fields("joinfield1", "joinfield2"));

### <a name="batches"></a>Kötegek

A Storm rendelkezik egy belső időzítő mechanizmussal, az úgynevezett „tick tuple” rekordórajellel. Ennek segítségével beállíthatja, hogy a topológia milyen gyakran bocsásson ki egy-egy rekordórajelet.

Példa egy rekordórajel C#-összetevőből való használatára: [PartialBoltCount.cs](https://github.com/hdinsight/hdinsight-storm-examples/blob/3b2c960549cac122e8874931df4801f0934fffa7/EventCountExample/EventCountTopology/src/main/java/com/microsoft/hdinsight/storm/examples/PartialCountBolt.java).

### <a name="caches"></a>Gyorsítótárak

A memóriában történő gyorsítótárazás gyakran használatos a feldolgozást felgyorsító mechanizmusként, mivel a memóriában tartja a gyakran használt objektumokat. Mivel a topológiák több csomópont, és az egyes csomópontokon belül is több folyamat között oszlanak meg, érdemes megfontolni a [fieldsGrouping](http://storm.apache.org/releases/current/javadocs/org/apache/storm/topology/InputDeclarer.html#fieldsGrouping-java.lang.String-org.apache.storm.tuple.Fields-) használatát. Használjon `fieldsGrouping` győződjön meg arról, hogy hello a gyorsítótárban való kereséshez használt mezőket tartalmazó rekordok mindig irányított toohello tooensure ugyanezt a folyamatot. Ezzel a csoportosítási funkcióval elkerülhető, hogy a különböző folyamatok ismétlődő gyorsítótár-bejegyzéseket hozzanak létre.

### <a name="stream-top-n"></a>A „legfelső N” számú elem streamelése

Ha a topológia a legfelső n számú értéket kiszámításától függ, kiszámíthatja hello legfelső n számú értéket párhuzamosan. Majd egyesítési hello e számítások eredményét összesíti egy globális értékben. Ez a művelet használatával végezhető [fieldsGrouping](http://storm.apache.org/releases/current/javadocs/org/apache/storm/topology/InputDeclarer.html#fieldsGrouping-java.lang.String-org.apache.storm.tuple.Fields-) tooroute párhuzamos feldolgozás mező szerint. Majd irányíthatja a tooa bolthoz, amely globálisan meghatározza a hello legfelső n számú értéket.

A legfelső n számú értéket kiszámítása példáért lásd: hello [RollingTopWords](https://github.com/apache/storm/blob/master/examples/storm-starter/src/jvm/org/apache/storm/starter/RollingTopWords.java) példa.

## <a name="logging"></a>Naplózás

A Storm Apache Log4j toolog információkat használja. Alapértelmezés szerint egy nagy mennyiségű adatot kerül, és nehéz toosort hello információk között lehet. Megadhat egy naplózási konfigurációs fájlt a Storm-topológia toocontrol naplózási viselkedését részeként.

Egy példa topológia azt mutatja be, hogyan naplózás tooconfigure: [Java-alapú WordCount](hdinsight-storm-develop-java-topology.md) HDInsight alatt futó Storm példa.

## <a name="next-steps"></a>Következő lépések

További információk a HDInsight alatt futó Storm valós idejű elemzési megoldásairól:

* [Bevezetés a HDInsight-alapú Apache Storm használatába][gettingstarted]
* [HDInsight alatt futó Apache Storm példatopológiái](hdinsight-storm-example-topology.md)

[stormtrident]: https://storm.apache.org/documentation/Trident-API-Overview.html
[samoa]: http://yahooeng.tumblr.com/post/65453012905/introducing-samoa-an-open-source-platform-for-mining
[apachetutorial]: https://storm.apache.org/documentation/Tutorial.html
[gettingstarted]: hdinsight-apache-storm-tutorial-get-started-linux.md
