---
title: Azure HDInsight a Storm aaaTroubleshoot |} Microsoft Docs
description: "Válaszok toocommon kérdések Azure HDInsight alatt futó Apache Storm használatával kapcsolatban."
keywords: "Az Azure HDInsight alatt futó Storm, gyakori kérdések hibaelhárítási útmutatója, gyakori problémák"
services: Azure HDInsight
documentationcenter: na
author: raviperi
manager: 
editor: 
ms.assetid: 74E51183-3EF4-4C67-AA60-6E12FAC999B5
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/7/2017
ms.author: raviperi
ms.openlocfilehash: 51bcb3dc28eff5ee7bb33252fb2ec71a88ed8e09
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-storm-by-using-azure-hdinsight"></a>Azure HDInsight alatt futó Storm hibaelhárításáról

További tudnivalók a hello legfőbb problémákat és azok megoldásait Apache Ambari az Apache Storm hasznos adatot való munkához.

## <a name="how-do-i-access-hello-storm-ui-on-a-cluster"></a>Hogyan érhetem el az egy fürtön található Storm felhasználói felülete hello
Hello Storm felhasználói felülete eléréséhez a böngészőben két lehetőség közül választhat:

### <a name="ambari-ui"></a>Ambari felhasználói felület
1. Nyissa meg toohello Ambari irányítópult.
2. Válassza a szolgáltatások hello lista **Storm**.
3. A hello **Gyorshivatkozások** menü **Storm felhasználói felülete**.

### <a name="direct-link"></a>A közvetlen hivatkozás
Hello Storm felhasználói felülete hello URL-cím a következő címen érhető el:

https://\<fürt DNS-név\>/stormui

Példa:

 https://stormcluster.azurehdinsight.NET/stormui

## <a name="how-do-i-transfer-storm-event-hub-spout-checkpoint-information-from-one-topology-tooanother"></a>Hogyan Storm esemény hub spout ellenőrzőpont információinak átvitele egy topológia tooanother

Olvassa el az Azure Event Hubs hello segítségével a HDInsight alatt futó Storm event hub spout található .jar fájl topológiák fejlesztésekor a topológia, amelyen azonos nevet az új fürt hello kell telepítenie. Azonban meg kell őrizni hello ellenőrzőpont adat véglegesített tooApache ZooKeeper hello régi fürtön.

### <a name="where-checkpoint-data-is-stored"></a>Ellenőrzőpont-adatok tárolására
Az eltolások ellenőrzőpont adatait hello event hub spout ZooKeeper a két legfelső szintű elérési útjában tárolja:
- Nem tranzakciós spout ellenőrzőpontokat /eventhubspout vannak tárolva.
- Be- / tranzakciós tranzakciós spout ellenőrzőpont adatait tárolja.

### <a name="how-toorestore"></a>Hogyan toorestore
tooget hello parancsfájlok és a szalagtárak ZooKeeper tooexport adatokat használni, és hello adatok hátsó tooZooKeeper egy új nevet, majd importálja [HDInsight alatt futó Storm példák](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/tools/zkdatatool-1.0).

hello lib mappa hello megvalósítási hello exportálási/importálási művelethez tartalmazó .jar fájlokat tartalmaz. hello bash mappa példa parancsfájl bemutatja, hogyan tooexport adatait hello ZooKeeper server hello régi fürt tartozik, és importálhatja vissza toohello ZooKeeper server hello új fürtön.

Futtassa a hello [stormmeta.sh](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/tools/zkdatatool-1.0/bash/stormmeta.sh) hello ZooKeeper csomópontok tooexport parancsfájl, és importálja az adatokat. Frissítés hello parancsfájl toohello megfelelő Hortonworks Data Platform (HDP) verziójával. (Jelenleg is dolgozunk a parancsfájlokat a Hdinsightban általános elvégzése. Általános parancsfájlok futtathatók bármely olyan csomópontról hello fürt hello felhasználó módosítások nélkül.)

hello exportálás parancs ír hello metaadatok tooan Apache Hadoop elosztott fájlrendszerrel (HDFS) útvonal (az Azure Blob Storage vagy az Azure Data Lake Store áruházban), amely egy helyen.

### <a name="examples"></a>Példák

#### <a name="export-offset-metadata"></a>Az eltolási metaadatok exportálása
1. Mely hello ellenőrzőponttól eltolás kell exportált toobe hello fürtön SSH toogo toohello ZooKeeper-fürt használatára.
2. Futtatási hello következő parancsot (frissítés után is használni hello HDP verzió-karakterlánca) tooexport ZooKeeper eltolás adatok toohello /stormmetadta/zkdata HDFS elérési útja:

    ```apache   
    java -cp ./*:/etc/hadoop/conf/*:/usr/hdp/2.5.1.0-56/hadoop/*:/usr/hdp/2.5.1.0-56/hadoop/lib/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/lib/*:/etc/failover-controller/conf/*:/etc/hadoop/* com.microsoft.storm.zkdatatool.ZkdataImporter export /eventhubspout /stormmetadata/zkdata
    ```

#### <a name="import-offset-metadata"></a>Az eltolási metaadatok importálása
1. Mely hello ellenőrzőponttól eltolás kell exportált toobe hello fürtön SSH toogo toohello ZooKeeper-fürt használatára.
2. Futtatási hello (frissítés után is használni hello HDP verzió-karakterlánca) parancs következő tooimport ZooKeeper eltolás hello HDFS elérési útja/stormmetadata/zkdata toohello ZooKeeper server hello célfürtön adatait:

    ```apache
    java -cp ./*:/etc/hadoop/conf/*:/usr/hdp/2.5.1.0-56/hadoop/*:/usr/hdp/2.5.1.0-56/hadoop/lib/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/lib/*:/etc/failover-controller/conf/*:/etc/hadoop/* com.microsoft.storm.zkdatatool.ZkdataImporter import /eventhubspout /home/sshadmin/zkdata
    ```
   
#### <a name="delete-offset-metadata-so-that-topologies-can-start-processing-data-from-hello-beginning-or-from-a-timestamp-that-hello-user-chooses"></a>Így a topológiák adatfeldolgozás hello elejétől elindításához, vagy az időbélyeg hello felhasználó úgy dönt, eltolási metaadatok törlése
1. Mely hello ellenőrzőponttól eltolás kell exportált toobe hello fürtön SSH toogo toohello ZooKeeper-fürt használatára.
2. Futtatási hello (frissítés után is használni hello HDP verzió-karakterlánca) parancs következő összes toodelete ZooKeeper eltolás hello aktuális fürt adatokat:

    ```apache
    java -cp ./*:/etc/hadoop/conf/*:/usr/hdp/2.5.1.0-56/hadoop/*:/usr/hdp/2.5.1.0-56/hadoop/lib/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/lib/*:/etc/failover-controller/conf/*:/etc/hadoop/* com.microsoft.storm.zkdatatool.ZkdataImporter delete /eventhubspout
    ```

## <a name="how-do-i-locate-storm-binaries-on-a-cluster"></a>Hogyan keresse meg a Storm bináris fürt
A Storm bináris hello aktuális HDP verem /usr/hdp/current/storm-client szerepelnek. hello helyre van hello azonos átjárócsomópontokkal, valamint az ezen csomópontokhoz.
 
Előfordulhat, hogy több bináris fájljait (például /usr/hdp/2.5.0.1233/storm) /usr/hdp adott HDP verzióihoz. hello /usr/hdp/current/storm-client mappa symlinked toohello legújabb verziójára, amely hello fürtön fut.

További információkért lásd: [Connect tooan HDInsight-fürthöz SSH segítségével](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) és [Storm](http://storm.apache.org/).
 
## <a name="how-do-i-determine-hello-deployment-topology-of-a-storm-cluster"></a>Hogyan állapítható meg hello telepítési topológia a Storm-fürt
Először azonosítsa a HDInsight alatt futó Storm, telepített összetevők. A Storm-fürt négy csomópont kategóriák áll:

* Átjárócsomópontok
* HEAD csomópontok
* ZooKeeper csomópontok
* Munkavégző csomópontokhoz
 
### <a name="gateway-nodes"></a>Átjárócsomópontok
Átjáró csomópont egy átjáró és a fordított proxy szolgáltatás, amely lehetővé teszi, hogy a nyilvános hozzáférés tooan active Ambari management szolgáltatás. Azt is kezeli az Ambari vezető választás.
 
### <a name="head-nodes"></a>HEAD csomópontok
A Storm átjárócsomópontokkal futtassa a következő szolgáltatások hello:
* Nimbus
* Ambari kiszolgáló
* Ambari metrikák kiszolgáló
* Ambari metrikákat gyűjtő
 
### <a name="zookeeper-nodes"></a>ZooKeeper csomópontok
HDInsight egy három csomópontos ZooKeeper kvórum tartalmaz. hello kvórum mérete rögzített, és nem kell újrakonfigurálni.
 
A Storm szolgáltatások hello fürtben konfigurált tooautomatically használata hello ZooKeeper kvórum.
 
### <a name="worker-nodes"></a>Munkavégző csomópontokhoz
Storm munkavégző csomópontokhoz futtassa a következő szolgáltatások hello:
* Felügyeleti
* Munkavégző Java virtuális gépek (JVMs), a futó topológiák
* Ambari ügynök
 
## <a name="how-do-i-locate-storm-event-hub-spout-binaries-for-development"></a>Hogyan keresse meg a Storm event hub spout bináris fejlesztési
 
A topológia a Storm event hub spout .jar fájlok használatával kapcsolatos további információkért tekintse meg a következő erőforrások hello.
 
### <a name="java-based-topology"></a>Java-alapú topológia
[Az Azure Event Hubs (Java) futó Storm eseményeinek](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-storm-develop-java-event-hub-topology)
 
### <a name="c-based-topology-mono-on-hdinsight-34-linux-storm-clusters"></a>C#-alapú topológia (a HDInsight 3.4 + Linux Storm-fürtök monó)
[Az Azure Event Hubs-eseményközpontok eseményeinek feldolgozása a HDInsight alatt futó Stormmal (C#)](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-storm-develop-csharp-event-hub-topology)
 
### <a name="latest-storm-event-hub-spout-binaries-for-hdinsight-35-linux-storm-clusters"></a>Legújabb Storm event hub spout binárist 3.5 + HDInsight Linux Storm-fürtök
toolearn hogyan toouse hello legújabb Storm event hub spout, amely kompatibilis a HDInsight 3.5 + Linux Storm-fürtök, tekintse meg a hello mvn-tárház [információs fájl](https://github.com/hdinsight/mvn-repo/blob/master/README.md).
 
### <a name="source-code-examples"></a>Forrás-kódpéldák
Lásd: [példák](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub) , hogy hogyan tooread és az Azure Event Hubs egy Apache Storm-topológia (Java nyelven írt) használata az Azure HDInsight-fürtök írása.
 
## <a name="how-do-i-locate-storm-log4j-configuration-files-on-clusters"></a>Hogyan keresse meg a Storm Log4J konfigurációs fájlok fürtökön
 
a Storm szolgáltatások tooidentify Apache Log4J konfigurációs fájljait.
 
### <a name="on-head-nodes"></a>Az átjárócsomópontokkal
hello Nimbus Log4J konfiguráció olvasása az/usr/hdp/\<HDP verzió\>/storm/log4j2/cluster.xml.
 
### <a name="on-worker-nodes"></a>A feldolgozó csomópontok
hello felügyelő Log4J konfiguráció olvasása az/usr/hdp/\<HDP verzió\>/storm/log4j2/cluster.xml.
 
hello munkavégző Log4J konfigurációs fájl olvasása az/usr/hdp/\<HDP verzió\>/storm/log4j2/worker.xml.
 
Példák: /usr/hdp/2.6.0.2-76/storm/log4j2/cluster.xml /usr/hdp/2.6.0.2-76/storm/log4j2/worker.xml

