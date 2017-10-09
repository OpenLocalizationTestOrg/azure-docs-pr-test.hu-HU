---
title: "aaaWhat HDInsight, hello Hadoop technológiai területekre & fürtök? - Azure | Microsoft Docs"
description: "Egy bevezető tooHDInsight és hello Hadoop technológiai területekre és összetevőket, például a Spark, a Kafka, a Hive, a HBase a big Data típusú adatok elemzésére."
keywords: "az Azure hadoop, hadoop azure, hadoop bevezetés, hadoop bemutatása, hadoop technológiai területekre, bevezetés toohadoop, bevezetés toohadoop, mi az, hogy mi az hadoop-fürt hadoop-fürt hadoop alkalmazott"
services: hdinsight
documentationcenter: 
author: cjgronlund
manager: jhubbard
editor: cgronlun
ms.assetid: e56a396a-1b39-43e0-b543-f2dee5b8dd3a
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/20/2017
ms.author: cgronlun
ms.openlocfilehash: 0aed3d1a6cf3c0cdc3b036cfa4865a2e0b58e2c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooazure-hdinsight-hello-hadoop-technology-stack-and-hadoop-clusters"></a>A HDInsight bemutatása tooAzure hello Hadoop technológiai területekre és Hadoop-fürtök
 Ez a cikk egy bevezető tooAzure HDInsight, a felhőalapú terjesztési hello Hadoop technológia verem tartalmazza. Itt olvashat arról is, hogy mi a Hadoop-fürt, és mikor lehet használni. 

## <a name="what-is-hdinsight-and-hello-hadoop-technology-stack"></a>Mi az HDInsight és a Hadoop technológiai területekre hello? 
Az Azure HDInsight Hadoop-összetevőt hello hello felhőalapú terjesztési [Hortonworks Data Platform (HDP)](https://hortonworks.com/products/data-center/hdp/). [Apache Hadoop](http://hadoop.apache.org/) hello eredeti nyílt forráskódú keretrendszere, amely elosztott feldolgozási és a számítógépek fürtjein big data készletek elemzésének volt. 


hello Hadoop technológiai területekre kapcsolódó szoftver- és segédprogramok, többek között az Apache Hive, HBase, Spark, Kafka és sok más tartalmazza. toosee elérhető Hadoop technológia verem összetevői a HDInsight, lásd: [összetevők és a hdinsight eszközzel verziók][component-versioning]. tooread Hadoop a Hdinsightban, kapcsolatos további információkért lásd: hello [Azure-szolgáltatások hdinsight lapon](https://azure.microsoft.com/services/hdinsight/).

## <a name="what-is-a-hadoop-cluster-and-when-do-you-use-it"></a>Mi a Hadoop-fürt, és mikor lehet használni?
A *Hadoop* egy fürttípus is, amely a következőkkel rendelkezik:

* YARN a feladatütemezéshez és az erőforrás-kezeléshez
* MapReduce a párhuzamos feldolgozáshoz
* hello Hadoop elosztott fájlrendszer (HDFS)
  
A Hadoop-fürtöket leggyakrabban a tárolt adatok kötegelt feldolgozására használják. A HDInsightban található egyéb típusú fürtök további képességekkel rendelkeznek: A Spark a gyorsabb, memórián belüli feldolgozás miatt egyre népszerűbb. További részletek: [Fürttípusok a HDInsightban](#overview).

## <a name="what-is-big-data"></a>Mik azok a big data típusú adatok?
A big data kifejezés bármilyen nagyobb digitális információhalmazra alkalmazható, mint például:

* Ipari berendezés érzékelőadatai
* Webhelyről gyűjtött, felhasználói tevékenységre vonatkozó információ
* Twitter-hírcsatorna

A big data gyűjtése egyre nagyobb mennyiségben és sebességgel, többféle formátumban történik. Lehet, hogy a korábbi (azaz tárolt) vagy a valós idejű (azaz a hello forrásból streamelt). 

## <a name="overview"></a>Fürttípusok a HDInsightban
A HDInsight adott fürttípusokat és fürttestreszabási képességeket is tartalmaz, például lehetővé teszi összetevők, segédprogramok és nyelvek hozzáadását.

### <a name="spark-kafka-interactive-hive-hbase-customized-and-other-cluster-types"></a>Spark, Kafka, Interactive Hive, HBase, egyéni és egyéb fürttípusok
HDInsight fürt típusa a következő hello kínálja:

* **[Apache Hadoop](https://wiki.apache.org/hadoop)**: használ [HDFS](#hdfs), [YARN](#yarn) erőforrás-kezelést és egy egyszerű [MapReduce](#mapreduce) programozási modell tooprocess és adatok kötegelt párhuzamos elemzése.
* **[Az Apache Spark on](http://spark.apache.org/)**: egy párhuzamos feldolgozást végző keretrendszer, amely támogatja a memórián belüli feldolgozást tooboost hello teljesítmény a big data elemző alkalmazások, az SQL, a streamelési adatok Spark folyamatok, és a gépi tanulás. Lásd: [Mi a HDInsight-alapú Apache Spark?](hdinsight-apache-spark-overview.md)
* **[Apache HBase](http://hbase.apache.org/)**: egy Hadoopra épülő NoSQL-adatbázis, amely közvetlen hozzáférést és nagymértékű következetességet biztosít a nagy mennyiségű strukturálatlan és félig strukturált adatok számára, akár egy több milliárd sorból és több millió oszlopból álló táblázat esetén is. Lásd: [Mi a HDInsight-alapú HBase?](hdinsight-hbase-overview.md)
* **[Microsoft R Server](https://msdn.microsoft.com/microsoft-r/rserver)**: párhuzamos, elosztott R-folyamatok üzemeltetésére és felügyeletére használt kiszolgáló. Az igény szerinti hozzáférés tooscalable, elemzés hdinsight platformon történő elosztott módszerek adatelemzők, statisztikusok és R-t használó programozók biztosít. Lásd: [A HDInsight-alapú R Server áttekintése](hdinsight-hadoop-r-server-overview.md).
* **[Apache Storm](https://storm.incubator.apache.org/)**: egy elosztott, valós idejű számítási rendszer a nagy méretű adatfolyamok gyors feldolgozására. A Storm a HDInsightban felügyelt fürtként érhető el. Lásd: [Analyze real-time sensor data using Storm and Hadoop](hdinsight-storm-sensor-data-analysis.md) (Valós idejű érzékelőadatok elemzése a Storm és a Hadoop segítségével).
* **[Apache Interaktív Hive – előzetes verzió (avagy Hosszú és eredményes feldolgozást!)](https://cwiki.apache.org/confluence/display/Hive/LLAP)**: memóriában történő gyorsítótárazás az interaktív és gyorsabb Hive-lekérdezésekhez. Lásd: [Az Interactive Hive használata a HDInsightban](hdinsight-hadoop-use-interactive-hive.md).
* **[Apache Kafka](https://kafka.apache.org/)**: nyílt forráskódú platform streamelt adatfolyamatok és alkalmazások létrehozásához. Kafka, amely lehetővé teszi a toopublish és toodata adatfolyamok előfizetés üzenet-várólista funkciókat is biztosít. Lásd: [Kafka a HDInsight bemutatása tooApache](hdinsight-apache-kafka-introduction.md).

A következő módszerek hello-fürtök is konfigurálhatja:
* **[Tartományhoz csatlakozó fürtök előzetes](hdinsight-domain-joined-introduction.md)**: A fürt tooan Active Directory-tartományhoz csatlakoztatott, hogy a hozzáférést, és az adatok házirendhatóság.
* **[Egyéni fürtök parancsfájlműveletekkel](hdinsight-hadoop-customize-cluster-linux.md)**: Olyan fürtök, amelyek az üzembe helyezés során parancsfájlok futtatásával telepítenek további összetevőket.

### <a name="example-cluster-customization-scripts"></a>Szkriptpéldák a fürtök testreszabásához
A Parancsfájlműveletek olyan Bash parancsfájlok Linux rendszerű fürtök kiépítése során, és, amely lehet használt tooinstall további összetevők hello fürtön. 

hello alábbi parancsfájlpéldákat biztosítja hello HDInsight csapat:

* **[Hue](hdinsight-hadoop-hue-linux.md)**: A webes alkalmazások használt toointeract egy fürttel készletét. Csak Linux fürtökön használható.
* **[Giraph](hdinsight-hadoop-giraph-install-linux.md)**: Diagramfeldolgozás dolgok vagy személyek közötti toomodel kapcsolatokat.
* **[Solr](hdinsight-hadoop-solr-install-linux.md)**: vállalati szintű keresési platform, amely teljes szöveges keresést tesz lehetővé az adatokon.

Az egyéni parancsfájlművelet-fejlesztéssel kapcsolatos további információkért lásd: [Script Action development with HDInsight](hdinsight-hadoop-script-actions-linux.md) (Parancsfájlműveletek fejlesztése a HDInsighttal).

## <a name="components-and-utilities-on-hdinsight-clusters"></a>A HDInsight-fürtök összetevői és segédprogramjai
hello alábbi összetevőket és segédprogramokat tartalmazzák a HDInsight-fürtök:

* **[Ambari](#ambari)**: fürtkiépítés, kezelés, megfigyelés és segédprogramok.
* **[Az Avro](#avro)**  (Microsoft .NET könyvtár az avro-hoz): adatszerializálás a Microsoft .NET-környezet hello. 
* **[Hive és HCatalog](#hive)**: SQL-szerű lekérdezés, valamint egy táblázat- és tárolókezelési réteg.
* **[Mahout](#mahout)**: Méretezhető Machine Learning alkalmazásokhoz.
* **[MapReduce](#mapreduce)**: örökölt keretrendszer a Hadoop által elosztott feldolgozáshoz és erőforrás-kezeléshez. Lásd: [YARN](#yarn).
* **[Oozie](#oozie)**: munkafolyamat-kezelés.
* **[Phoenix](#phoenix)**: HBase-en alapuló relációs adatbázisréteg.
* **[Pig](#pig)**: egyszerűbb parancsfájlkezelés MapReduce-átalakításokhoz.
* **[Sqoop](#sqoop)**: adatok importálása és exportálása.
* **[Tez](#tez)**: lehetővé teszi, hogy az adatigényes folyamatok toorun hatékonyan méretekben.
* **[YARN](#yarn)**: hello Hadoop alap függvénytárának része erőforrás-kezelést.
* **[ZooKeeper](#zookeeper)**: elosztott rendszerek folyamatait koordinálja.

> [!NOTE]
> Hello meghatározott összetevőkre vonatkozó és fájlverzió-információkat lásd: [Hadoop-összetevők és a HDInsight-verziói][component-versioning]
>
>

### <a name="ambari"></a>Ambari
Az Apache Ambari az Apache Hadoop-fürtök kiépítésére, kezelésére és figyelésére szolgál. Operátori eszközök intuitív gyűjteményét és egy robusztus API-k, amelyek elfedik hello Hadoop összetettségét, és a fürtök működését hello egyszerűsítése tartalmaz. A HDInsight-fürtök Linux rendszeren adja meg, mindkét hello Ambari webes felhasználói felület, és hello Ambari REST API-t. Az Ambari Views on HDInsight fürtök beépülő felhasználói felületi képességeket kínálnak.
Lásd: [HDInsight-fürtök kezelése az Ambari használatával](hdinsight-hadoop-manage-ambari.md) és <a target="_blank" href="https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md">referencia az Apache Ambari API-hoz</a>.

### <a name="avro"></a>Avro (Microsoft .NET könyvtár az Avro-hoz)
a Microsoft .NET könyvtár az Avro hello hello Apache Avro kompakt bináris data interchange formátum hello Microsoft .NET-környezet szerializációkhoz valósítja meg. Nyelvfüggetlen sémát határoz meg annak érdekében, hogy az egyik nyelven szerializált adatok egy másik nyelven is olvashatók legyenek. Hello részletes információk találhatók hello < a target = _ "blank" href = "http://avro.apache.org/docs/current/spec.html" > Apache Avro specifikációjában</a>. az Avro-fájlok támogatja hello hello formátuma elosztott MapReduce programozási modell: a fájl bármely pontot kikereshet, és az egy adott blokktól kezdheti fájlok "feloszthatók",. toofind, hogyan, lásd: [szerializálni az adatokat a hello Microsoft .NET könyvtár az avro-hoz](hdinsight-dotnet-avro-serialization.md). Linux-alapú fürt támogatási toocome.

### <a name="hdfs"></a>HDFS
Hadoop elosztott fájlrendszerrel (HDFS) rendszert, amely, és a YARN a MapReduce, Hadoop technológiák hello core rendszer. Ez a szabványos fájlrendszere hello a HDInsight Hadoop-fürtök. Lásd: [Adatok lekérdezése HDFS-kompatibilis tárhelyről](hdinsight-hadoop-use-blob-storage.md).

### <a name="hive"></a>Hive és HCatalog
<a target="_blank" href="http://hive.apache.org/">Apache Hive</a> az adatok szoftver, amely lehetővé teszi tooquery Hadoopra épülő adatraktár és az elosztott tárolókban található nagy adatkészletek kezelése a HiveQL nevű SQL-szerű nyelv használatával. A Hive a Pighez hasonlóan egy MapReduce-ra épülő absztrakció, amely a lekérdezéseket MapReduce-feladatok sorozatára fordítja le. Hive szorosabb tooa relációs adatbázis-kezelő rendszer mint a Pig, és használt strukturáltabb adatokhoz. Strukturálatlan adatok esetén a Pig hello megfelelőbb választás. Lásd: [Use Hive with Hadoop in HDInsight](hdinsight-use-hive.md) (A Hive és a Hadoop együttes használata a HDInsightban).

Az <a target="_blank" href="https://cwiki.apache.org/confluence/display/Hive/HCatalog/">Apache HCatalog</a> a Hadoop táblázat- és tárolókezelési rétege, amely relációs nézetben mutatja be az adatokat. A HCatalogban bármely olyan formátumban olvashat és írhat fájlokat, amely használható a Hive SerDe-vel (szerializáló-deszerializáló).

### <a name="mahout"></a>Mahout
Az <a target="_blank" href="https://mahout.apache.org/">Apache Mahout</a> a Hadoopon futó gépi tanulási algoritmusok könyvtára. Statisztikai alapelvek segítségével, a gépi tanulást segítő alkalmazások rendszerek toolearn adatokból és a múltbeli eredményekkel toodetermine későbbi viselkedési toouse mutatja meg. Lásd: [Generate movie recommendations using Mahout on Hadoop](hdinsight-mahout.md) (Filmajánlók létrehozása a Hadoop-alapú Mahout segítségével).

### <a name="mapreduce"></a>MapReduce
MapReduce a Hadoop örökölt szoftver-keretrendszere hello párhuzamosan toobatch folyamat big data készletek alkalmazások írásához. A MapReduce feladatot felosztja a nagy adatkészleteket, és hello adatok rendezi a kulcs-érték párok feldolgozásra. A MapReduce-feladatok a [YARN](#yarn) rendszerén futnak. Lásd: <a target="_blank" href="http://wiki.apache.org/hadoop/MapReduce">MapReduce</a> a Hadoop Wiki hello.

### <a name="oozie"></a>Oozie
Az <a target="_blank" href="http://oozie.apache.org/">Apache Oozie</a> egy munkafolyamat-koordinációs rendszer, amely a Hadoop-feladatokat kezeli. Hadoop-veremmel hello integrálva van, és támogatja a Hadoop-feladatokat a MapReduce, a Pig, a Hive és a Sqoop. Azt is használt tooschedule feladatok adott tooa rendszer, például Java programok vagy héjparancsfájlok ütemezésére. Lásd: [Use Oozie with Hadoop](hdinsight-use-oozie-linux-mac.md) (Az Oozie és a Hadoop együttes használata).

### <a name="phoenix"></a>Phoenix
Az <a  target="_blank" href="http://phoenix.apache.org/">Apache Phoenix</a> egy relációs adatbázisréteg, amely a HBase-re épül. Phoenix, amely lehetővé teszi a tooquery és SQL-táblák közvetlen kezelése JDBC-illesztőt tartalmaz. A Phoenix a MapReduce használata helyett natív NoSQL API-hívásokká alakítja a lekérdezéseket és egyéb kifejezéseket, ezáltal gyorsabb alkalmazásokat tesz lehetővé a NoSQL-tárolókon. Lásd: [Use Apache Phoenix and SQuirreL with HBase clusters](hdinsight-hbase-phoenix-squirrel.md) (Az Apache Phoenix és az SQuirreL használata HBase-fürtökkel).

### <a name="pig"></a>Pig
<a  target="_blank" href="http://pig.apache.org/">Apache Pig</a> egy magas szintű platform, amely lehetővé teszi összetett MapReduce-átalakításokhoz tooperform nagy adatkészletek a Pig Latin nevezetű egyszerű parancsnyelv használatával. A Pig lefordítja hello Pig Latin parancsfájlokat, így azok Hadoop is futtathatók. Felhasználó által definiált függvényeket (UDF) tooextend Pig Latin hozhat létre. Lásd: [Use Pig with Hadoop](hdinsight-use-pig.md) (A Pig és a Hadoop együttes használata).

### <a name="sqoop"></a>Sqoop
Az <a  target="_blank" href="http://sqoop.apache.org/">Apache Sqoop</a> olyan eszköz, amely a lehető leghatékonyabban biztosít tömeges adatátvitelt a Hadoop és a relációs adatbázisok (például SQL) vagy más strukturált adattárolók között. Lásd: [Use Sqoop with Hadoop](hdinsight-use-sqoop.md) (A Sqoop és a Hadoop együttes használata).

### <a name="tez"></a>Tez
Az <a  target="_blank" href="http://tez.apache.org/">Apache Tez</a> a Hadoop YARN rendszerre épülő alkalmazás-keretrendszer, amely végrehajtja az általános adatfeldolgozás összetett, aciklikus gráfjait. Egy rugalmasabb és hatékonyabb követő toohello MapReduce keretrendszer, amely lehetővé teszi az adatigényes folyamatok, például a Hive, hatékonyabb léptékű toorun. Lásd a [Use Apache Tez for improved performance](hdinsight-use-hive.md#usetez) (Teljesítménynövelés az Apache Tez használatával) című szakaszt a Hive és a HiveQL használatával foglalkozó témakörben.

### <a name="yarn"></a>YARN
Apache YARN hello következő generációja (MapReduce 2.0 vagy MRv2) MapReduce és adatfeldolgozási forgatókönyveket MapReduce kötegelt feldolgozáson nagyobb méretezhetőséggel és valós idejű feldolgozással támogatja. A YARN erőforrás-kezelést és egy elosztott alkalmazás-keretrendszert biztosít. A MapReduce-feladatok a YARN rendszerén futnak. Lásd: <a target="_blank" href="http://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html">Apache Hadoop NextGen MapReduce (YARN)</a>.

### <a name="zookeeper"></a>ZooKeeper
Az <a  target="_blank" href="http://zookeeper.apache.org/">Apache ZooKeeper</a> adatregiszterek (znode-ok) közös hierarchikus névterével koordinálja a nagy méretű elosztott rendszerek folyamatait. Znode tartalmaz kis mennyiségű metaadatot szükséges információkat toocoordinate folyamatok: állapot, hely, konfiguráció és így tovább. Tekintsen meg egy példát [a ZooKeeper, egy HBase-fürt és az Apache Phoenix](hdinsight-hbase-phoenix-squirrel-linux.md) használatára. 

## <a name="programming-languages-on-hdinsight"></a>Programozási nyelvek a HDInsight rendszerében
A HDInsight-fürtök, mint például a Spark, a HBase, a Kafka, és a Hadoop, számos programozási nyelvet támogatnak, de ezek közül nem mindegyik van alapértelmezés szerint telepítve. A könyvtárak, modulok vagy alapértelmezés szerint nem telepített csomagok [egy parancsfájl művelet tooinstall hello összetevővel](hdinsight-hadoop-script-actions-linux.md). 

### <a name="default-programming-language-support"></a>Alapértelmezés szerint támogatott programozási nyelvek
Alapértelmezés szerint a HDInsight-fürtök a következőket támogatják:

* Java
* Python

[Szkriptműveletek](hdinsight-hadoop-script-actions-linux.md) használatával további nyelvek is telepíthetők.

### <a name="java-virtual-machine-jvm-languages"></a>JVM (Java virtuális gép) nyelvek
Java nyelven sok futtathatja a Java virtuális gép (JVM); azonban fut az egyes nyelvek szükség lehet további összetevők hello fürtön telepítve.

A HDInsight-fürtök az alábbi JVM-alapú nyelveket támogatják:

* Clojure
* Jython (Python a Javához)
* Scala

### <a name="hadoop-specific-languages"></a>Hadoop-specifikus nyelvek
A HDInsight-fürtök támogatása a következő nyelveket, amelyek adott toohello Hadoop technológiai területekre hello:

* Pig Latin a Pig-feladatokhoz
* HiveQL a Hive-feladatokhoz és a SparkSQL-hez

## <a name="hdinsight-standard-and-hdinsight-premium"></a>HDInsight Standard és HDInsight Prémium
A HDInsight a big data felhőajánlatokat kétféle – Standard és Prémium – kategóriában biztosítja. HDInsight Standard egy vállalati szintű fürtöt biztosít, hogy a szervezetek használhatja toorun a big data számítási feladatokat. A Standard képességekre épülő HDInsight Prémium magasabb szintű elemzési és biztonsági képességeket biztosít a HDInsight-fürt számára. További információért lásd: [Azure HDInsight Prémium](hdinsight-component-versioning.md#hdinsight-standard-and-hdinsight-premium)

## <a name="microsoft-business-intelligence-and-hdinsight"></a>A Microsoft üzleti intelligencia és a HDInsight
Az ismerős üzletiintelligencia-(BI) eszközök beolvasni, elemzése, és jelentést vagy Power Query beépülő modul hello segítségével a HDInsight rendszerébe integrált adatokat vagy a Microsoft Hive ODBC-illesztő hello:

* [Power Query az Excel tooHadoop csatlakozás](hdinsight-connect-excel-power-query.md): megtudhatja, hogyan tooconnect Excel toohello Azure Storage-fiókban tárolt hello segítségével a Microsoft Power Query az Excel programhoz a HDInsight-fürt adatait. Használatához Windows-munkaállomás szükséges. 
* [Csatlakozás a Microsoft Hive ODBC-illesztő hello Excel tooHadoop](hdinsight-connect-excel-hive-odbc-driver.md): megtudhatja, hogyan tooimport adatokat a HDInsight-ból hello Microsoft Hive ODBC-illesztő. Használatához Windows-munkaállomás szükséges. 
* [A Microsoft Cloud Platform](http://www.microsoft.com/server-cloud/solutions/business-intelligence/default.aspx): további információk a Power BI az Office 365, töltse le a hello SQL Server próbaverzióját, és a SharePoint Server 2013 és az SQL Server BI beállítása.
* [SQL Server Analysis Services](http://msdn.microsoft.com/library/hh231701.aspx)
* [SQL Server Reporting Services](http://msdn.microsoft.com/library/ms159106.aspx)


## <a name="next-steps"></a>Következő lépések

* [A Hadoop használatának első lépései a HDInsightban](hdinsight-hadoop-linux-tutorial-get-started.md): gyors üzembe helyezési útmutató HDInsight Hadoop-fürtök kiépítéséhez és Hive-mintalekérdezések futtatásához.
* [A Spark használatának első lépései a HDInsightban](hdinsight-apache-spark-jupyter-spark-sql.md): gyors üzembe helyezési útmutató Spark-fürt létrehozásához és interaktív Spark SQL-lekérdezések futtatásához.
* [R Server használata a HDInsighton](hdinsight-hadoop-r-server-get-started.md): Az R Server használatának megkezdése a HDInsight Premiumban.
* [A HDInsight-fürtök kiépítése](hdinsight-hadoop-provision-linux-clusters.md): megtudhatja, hogyan egy HDInsight Hadoop-fürt keresztül tooprovision hello Azure-portál, Azure CLI vagy Azure PowerShell.


[component-versioning]: hdinsight-component-versioning.md
[zookeeper]: http://zookeeper.apache.org/