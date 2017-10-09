---
title: "a Storm aaaApache írási tooStorage/Data Lake Store - Azure HDInsight |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello a HDInsight alatt futó Apache Storm toowrite toohello HDFS-kompatibilis tároló. Az Azure Storage vagy az Azure Data Lake Store a HDFS-comptabile hello tárhely biztosításához a HDInsight számára. A dokumentum, és a hello társított példában bemutatják, hogyan hello HdfsBolt összetevő lehet használt toowrite toohello alapértelmezett egy Storm on HDInsight-fürt tárolására."
services: hdinsight
documentationcenter: na
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 1df98653-a6c8-4662-a8c6-5d288fc4f3a6
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/19/2017
ms.author: larryfr
ms.openlocfilehash: d76159a9ecd1be18e519511cfdb3bcfd18ae4d33
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="write-toohdfs-from-apache-storm-on-hdinsight"></a>A HDInsight alatt futó Apache Storm tooHDFS írása

Ismerje meg, hogyan toouse Storm toowrite adatok toohello HDFS-kompatibilis storage a HDInsight alatt futó Apache Storm használja. HDInsight is használhat HDFS-comptabile tárolóként tárolásához Azure Storage és az Azure Data Lake. Storm lehetővé egy [HdfsBolt](http://storm.apache.org/releases/1.1.0/javadocs/org/apache/storm/hdfs/bolt/HdfsBolt.html) írja az adatokat tooHDFS összetevő. Ez a dokumentum tájékoztatást nyújt a hello HdfsBolt tooeither a tároló típusa szerinti írása. 

> [!IMPORTANT]
> hello példában topológia a HDInsight alatt futó Storm részét képező összetevők támaszkodik. Megkövetelheti, hogy a módosítás toowork az Azure Data Lake Store más alatt futó Apache Storm-fürtök használata esetén.

## <a name="get-hello-code"></a>Hello kód beolvasása

Ez a topológia tartalmazó hello projekt egy letölthető [https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store](https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store).

toocompile ebben a projektben van szüksége a fejlesztési környezet konfigurációját a következő hello:

* [Java JDK 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) vagy újabb verzióját. HDInsight 3.5-ös vagy újabb rendszer szükséges Java 8.

* [Maven 3.x](https://maven.apache.org/download.cgi)

hello következő környezeti változó lehet beállítani a fejlesztő munkaállomás Java és hello JDK telepítésekor. Azonban ellenőrizni kell, hogy léteznek, illetve hello a rendszer tartozó helyes értékeket tartalmaz.

* `JAVA_HOME`-toohello rendszert tartalmazó könyvtár hello JDK kell mutatnia.
* `PATH`-a következő elérési utak hello tartalmaznia kell:
  
    * `JAVA_HOME`(vagy ezzel egyenértékű elérési hello).
    * `JAVA_HOME\bin`(vagy ezzel egyenértékű elérési hello).
    * hello mappát, ahová a Maven telepítve van.

## <a name="how-toouse-hello-hdfsbolt-with-hdinsight"></a>Hogyan toouse hello HdfsBolt a hdinsight eszközzel

> [!IMPORTANT]
> A HDInsight alatt futó Storm hello HdfsBolt használatához először használnia kell egy parancsfájl művelet szükséges toocopy jar fájlok be hello `extpath` alatt futó Storm. További információkért lásd: hello [konfigurálása hello fürt](#configure) szakasz.

hello HdfsBolt hello fájl sémát, hogy megadja toounderstand hogyan használja toowrite tooHDFS. A hdinsight eszközzel használja a következő rendszerek hello egyikét:

* `wasb://`: Egy Azure Storage-fiókot használni.
* `adl://`: Az Azure Data Lake Store használt.

hello következő táblázat példákat tartalmaz hello fájl séma használatával különböző helyzetek kezelésére.

| séma | Megjegyzések |
| ----- | ----- |
| `wasb:///` | hello alapértelmezett tárfiók egy blob tároló, az Azure Storage-fiók |
| `adl:///` | hello alapértelmezett tárfiók út egy könyvtár az Azure Data Lake Store. Fürt létrehozása során meg hello directory Data Lake Store, amely hello fürt HDFS hello gyökérmappájában. Például hello `/clusters/myclustername/` könyvtár. |
| `wasb://CONTAINER@ACCOUNT.blob.core.windows.net/` | Egy nem alapértelmezett (további) Azure-tárfiók hello-fürthöz tartozó. |
| `adl://STORENAME/` | Data Lake Store hello fürt által használt hello hello gyökere. A rendszer lehetővé teszi tooaccess adatok hello fürt fájlrendszer tartalmazó hello könyvtár kívül helyezkedik el. |

További információkért lásd: hello [HdfsBolt](http://storm.apache.org/releases/1.1.0/javadocs/org/apache/storm/hdfs/bolt/HdfsBolt.html) hivatkozás az Apache.org webhelyen.

### <a name="example-configuration"></a>Példa konfiguráció

hello következő YAM fájlból hello `resources/writetohdfs.yaml` hello példában szereplő fájlt. Ez a fájl határozza meg a hello Storm-topológia hello segítségével [fluxus](https://storm.apache.org/releases/1.1.0/flux.html) keretrendszere, amely alatt futó Apache Storm.

```yaml
components:
  - id: "syncPolicy"
    className: "org.apache.storm.hdfs.bolt.sync.CountSyncPolicy"
    constructorArgs:
      - 1000

  - id: "rotationPolicy"
    className: "org.apache.storm.hdfs.bolt.rotation.NoRotationPolicy"

  - id: "fileNameFormat"
    className: "org.apache.storm.hdfs.bolt.format.DefaultFileNameFormat"
    configMethods:
      - name: "withPath"
        args: ["${hdfs.write.dir}"]
      - name: "withExtension"
        args: [".txt"]

  - id: "recordFormat"
    className: "org.apache.storm.hdfs.bolt.format.DelimitedRecordFormat"
    configMethods:
      - name: "withFieldDelimiter"
        args: ["|"]

# spout definitions
spouts:
  - id: "tick-spout"
    className: "com.microsoft.example.TickSpout"
    parallelism: 1


# bolt definitions
bolts:
  - id: "hdfs-bolt"
    className: "org.apache.storm.hdfs.bolt.HdfsBolt"
    configMethods:
      - name: "withConfigKey"
        args: ["hdfs.config"]
      - name: "withFsUrl"
        args: ["${hdfs.url}"]
      - name: "withFileNameFormat"
        args: [ref: "fileNameFormat"]
      - name: "withRecordFormat"
        args: [ref: "recordFormat"]
      - name: "withRotationPolicy"
        args: [ref: "rotationPolicy"]
      - name: "withSyncPolicy"
        args: [ref: "syncPolicy"]
```

A YAM meghatározza, hogy a következő elemek hello:

* `syncPolicy`: Határozza meg, mikor fájlok legyenek szinkronizálva/kiürített toohello fájlrendszer. A példában minden 1000 rekordokat.
* `fileNameFormat`: Hello elérési útját és nevét mintát toouse meghatározza a fájlok írása közben. Ebben a példában a szűrő használata futásidőben a hello elérési út van megadva, és hello fájl kiterjesztése `.txt`.
* `recordFormat`: Az írt hello fájlok hello belső formátuma határozza meg. Ebben a példában szereplő mezők hello határolja `|` karakter.
* `rotationPolicy`: Meghatározza, hogy mikor toorotate fájlokat. Ebben a példában elforgatás nélkül történik.
* `hdfs-bolt`: Használja az előző összetevők működését hello hello konfigurációs paraméterei, `HdfsBolt` osztály.

Hello fluxus keretrendszerre további információkért lásd: [https://storm.apache.org/releases/1.1.0/flux.html](https://storm.apache.org/releases/1.1.0/flux.html).

## <a name="configure-hello-cluster"></a>Hello fürt konfigurálása

Alapértelmezés szerint a HDInsight alatt futó Storm nem tartalmaz, hogy HdfsBolt használ toocommunicate Azure Storage vagy a Data Lake Store a Storm tartozó classpath hello összetevők. Használjon hello alábbi parancsfájl művelet tooadd ezen összetevők toohello `extlib` könyvtár alatt futó Storm a fürt:

| A parancsfájl URI |} Csomópontok tooapply úgy, hogy |} Paraméterek. `https://000aarperiscus.blob.core.windows.net/certs/stormextlib.sh` | Nimbus, a felügyelő |} Nincs |}

Ezt a parancsfájlt a fürt használatáról információkért lásd: hello [Parancsfájlműveletek segítségével testre szabhatja HDInsight-fürtök](./hdinsight-hadoop-customize-cluster-linux.md) dokumentum.

## <a name="build-and-package-hello-topology"></a>Buildelés és a csomag hello topológia

1. Töltse le a hello példaprojekt [https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store ](https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store) tooyour fejlesztési környezet.

2. Egy parancs parancssori futtatásával Terminálszolgáltatások vagy a rendszerhéj munkamenet módosítása könyvtárak toohello gyökérmappájában hello letöltése. toobuild és a csomag hello topológia, a következő parancs hello használata:
   
        mvn compile package
   
    Miután hello build és csomagolt befejeződött, nincs-e egy új nevű könyvtár `target`, nevű fájlt tartalmaz, amelyek `StormToHdfs-1.0-SNAPSHOT.jar`. Ez a fájl fordítása hello topológia tartalmazza.

## <a name="deploy-and-run-hello-topology"></a>Regisztrálhat és futtathat hello topológia

1. A következő parancs toocopy hello topológia toohello HDInsight-fürt hello használata. Cserélje le **felhasználói** az SSH-felhasználónév hello hello fürt létrehozásakor használt. Cserélje le **CLUSTERNAME** hello nevű hello fürt.
   
        scp target\StormToHdfs-1.0-SNAPSHOT.jar USER@CLUSTERNAME-ssh.azurehdinsight.net:StormToHdfs1.0-SNAPSHOT.jar
   
    Amikor a rendszer kéri, adja meg az SSH-felhasználó hello hello fürt létrehozásakor használt hello jelszót. Ha a jelszó helyett használt nyilvános kulcsot, szükség lehet a toouse hello `-i` paraméter toospecify hello elérési toohello titkos kulccsal.
   
   > [!NOTE]
   > További információk az `scp` a hdinsight eszközzel, lásd: [az SSH a Hdinsighttal](./hdinsight-hadoop-linux-use-ssh-unix.md).

2. Hello feltöltése után használja a következő tooconnect toohello HDInsight-fürtjéhez SSH hello. Cserélje le **felhasználói** az SSH-felhasználónév hello hello fürt létrehozásakor használt. Cserélje le **CLUSTERNAME** hello nevű hello fürt.
   
        ssh USER@CLUSTERNAME-ssh.azurehdinsight.net
   
    Amikor a rendszer kéri, adja meg az SSH-felhasználó hello hello fürt létrehozásakor használt hello jelszót. Ha a jelszó helyett használt nyilvános kulcsot, szükség lehet a toouse hello `-i` paraméter toospecify hello elérési toohello titkos kulccsal.
   
   További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

3. A csatlakozás után használata hello következő parancsot a toocreate nevű fájl `dev.properties`:

        nano dev.properties

4. Szöveg hello hello tartalmát, a következő használatát hello `dev.properties` fájlt:

        hdfs.write.dir: /stormdata/
        hdfs.url: wasb:///

    > [!IMPORTANT]
    > Ez a példa feltételezi, hogy a fürt egy Azure Storage-fiókot használja, mint hello alapértelmezett tároló. Ha a fürt használ az Azure Data Lake Store, `hdfs.url: adl:///` helyette.
    
    toosave hello fájl használata __Ctrl + X__, majd __Y__, és végül __Enter__. hello ebben a fájlban értéke hello Data Lake store URL-cím és adatot ír hello könyvtárnevet.

3. A következő parancs toostart hello topológia hello használata:
   
        storm jar StormToHdfs-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /writetohdfs.yaml --filter dev.properties

    Hello topológia továbbítás toohello Nimbus csomópont hello fürt hello fluxus keretrendszer használatával indítja el. hello topológia definiált hello `writetohdfs.yaml` hello jar fájl. Hello `dev.properties` fájl szűrőként átadása, és hello topológia olvassák hello értékek hello fájlban találhatók.

## <a name="view-output-data"></a>Kimeneti adatok megtekintése

tooview hello adatokat, a következő parancs hello használata:

    hdfs dfs -ls /stormdata/

Ez a topológia által létrehozott hello fájlok listája jelenik meg.

hello alábbiakban látható egy példa hello előző parancsok által retuned hello adatok:

    Found 30 items
    -rw-r-----+  1 sshuser sshuser       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-0-1488568403092.txt
    -rw-r-----+  1 sshuser sshuser       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-1-1488568404567.txt
    -rw-r-----+  1 sshuser sshuser       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-10-1488568408678.txt
    -rw-r-----+  1 sshuser sshuser       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-11-1488568411636.txt
    -rw-r-----+  1 sshuser sshuser       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-12-1488568411884.txt
    -rw-r-----+  1 sshuser sshuser       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-13-1488568412603.txt
    -rw-r-----+  1 sshuser sshuser       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-14-1488568415055.txt

## <a name="stop-hello-topology"></a>Hello topológia leállítása

Storm-topológiák csak a futtatása vagy hello fürtök törlése. toostop hello topológia, a következő parancs használata hello:

    storm kill hdfswriter

## <a name="delete-your-cluster"></a>A fürt törlése

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Következő lépések

Most, hogy megtanulta, hogyan toouse Storm toowrite tooAzure tárolási és az Azure Data Lake Store felderítése más [példák a HDInsight Storm](hdinsight-storm-example-topology.md).

