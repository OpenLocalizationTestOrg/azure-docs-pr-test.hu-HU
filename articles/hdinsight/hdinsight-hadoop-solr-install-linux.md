---
title: "aaaUse parancsfájlművelet tooinstall a Linux-alapú HDInsight - Azure Solr |} Microsoft Docs"
description: "Ismerje meg, hogyan tooinstall Solr a Linux-alapú HDInsight Hadoop clusters Parancsfájlműveletek használatával."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: cc93ed5c-a358-456a-91a4-f179185c0e98
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/07/2017
ms.author: larryfr
ms.openlocfilehash: 4c179032b95ae187f1830d8927f8796372fa8ebe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-use-solr-on-hdinsight-hadoop-clusters"></a>Telepítheti és használhatja Solr HDInsight Hadoop-fürtök

Megtudhatja, hogyan tooinstall Solr on Azure HDInsight-parancsfájlművelet használatával. Solr egy hatékony keresési platform, és a Hadoop által kezelt adatok vállalati szintű keresési funkciókat biztosít.

> [!IMPORTANT]
    > hello jelen dokumentumban leírt lépések egy HDInsight-fürt által használt Linux igényelnek. Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).

> [!IMPORTANT]
> a dokumentumban használt hello mintaparancsfájl Solr 4.9 egy adott konfigurációval telepíti. Ha azt szeretné, hogy tooconfigure hello Solr fürt különböző gyűjteményeket, szilánkok, sémákat, replikákat, stb., módosítania kell hello parancsfájl és Solr bináris fájljait.

## <a name="whatis"></a>Mi az a Solr

[Apache Solr](http://lucene.apache.org/solr/features.html) egy vállalati keresési platform, amely lehetővé teszi az adatok hatékony teljes szöveges keresés. Hadoop lehetővé teszi a tárolásához és kezeléséhez az adatok óriási mennyiségben, Apache Solr hello keresési képességeket biztosít tooquickly lekérése hello adatokat.

> [!WARNING]
> A Microsoft hello HDInsight-fürt összetevői teljes mértékben támogatja.
>
> Egyéni összetevők, például Solr, minden üzleti szempontból ésszerű támogatási toohelp fogadni, toofurther hello problémával kapcsolatos hibaelhárítás elősegítéséhez. A Microsoft támogatási nem lehet egyéni összetevőkkel tudja tooresolve kapcsolatos problémák. Szükség lehet tooengage hello nyílt forráskódú Közösségek segítségért. Például nincsenek sok közösségi webhelyek használható, például: [MSDN fórum hdinsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Is Apache projektek rendelkezik projekt helyek [http://apache.org](http://apache.org), például: [Hadoop](http://hadoop.apache.org/).

## <a name="what-hello-script-does"></a>Milyen hello parancsprogram

Ezt a parancsfájlt a következő módosításokat toohello HDInsight-fürt hello teszi:

* 4.9 Solr történő telepítése`/usr/hdp/current/solr`
* Létrehoz egy felhasználói **solrusr**, vagyis használt toorun hello Solr szolgáltatás
* Készletek **solruser** hello tulajdonosaként`/usr/hdp/current/solr`
* Hozzáad egy [Upstart](http://upstart.ubuntu.com/) konfigurációjához, amely Solr automatikusan elindul.

## <a name="install"></a>A Parancsfájlműveletek segítségével Solr telepítése

Egy minta parancsfájlt tooinstall Solr egy HDInsight-fürt hello a következő helyen érhető el:

    https://hdiconfigactions.blob.core.windows.net/linuxsolrconfigactionv01/solr-installer-v01.sh

toocreate telepítve Solr rendelkező fürtnek, használjon hello szükséges lépések hello [HDInsight-fürtök létrehozása](hdinsight-hadoop-create-linux-clusters-portal.md) dokumentum. Hello létrehozási folyamat során a következő lépéseket tooinstall Solr hello használata:

1. A hello __fürt összefoglaló__ panelen, select__Advanced settings__, majd __parancsfájl-műveletek__. A következő információk toopopulate hello űrlap hello használata:

   * **NÉV**: Adjon meg egy rövid nevet hello parancsfájlművelet.
   * **PARANCSFÁJL URI azonosítója**: https://hdiconfigactions.blob.core.windows.net/linuxsolrconfigactionv01/solr-installer-v01.sh
   * **HEAD**: ezt a beállítást
   * **MUNKAVÉGZŐ**: ezt a beállítást
   * **ZOOKEEPER**: Ez a beállítás tooinstall hello Zookeeper csomóponton ellenőrzése
   * **PARAMÉTEREK**: ezt a mezőt hagyja üresen

2. Hello hello alján **parancsfájl-műveletek** panelen, használjon hello **válasszon** gombok toosave hello beállítása. Végül, használja a hello **következő** gomb tooreturn toohello __fürt összegzése__

3. A hello __fürt összefoglaló__ lapon jelölje be __létrehozása__ toocreate hello fürt.

## <a name="usesolr"></a>Solr használata a Hdinsightban

> [!IMPORTANT]
> Ebben a szakaszban található lépéseket hello alapszolgáltatásai Solr bemutatása. Solr használatával kapcsolatos további információkért lásd: hello [Apache Solr hely](http://lucene.apache.org/solr/).

### <a name="index-data"></a>Index adatok

A következő lépéseket tooadd példa adatok tooSolr hello használja, és majd lekérdezése:

1. Csatlakozzon az SSH használatával toohello HDInsight-fürt:

    ```bash
    ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

     > [!IMPORTANT]
     > A jelen dokumentum lépéseit az SSL protokollbújtatás tooconnect toohello Solr webes felhasználói felület használja. Ezek a lépések toouse, kell létesítenie az SSL protokollbújtatás, és konfigurálja a böngésző toouse azt.
     >
     > További információkért lásd: hello [használata SSH Tunneling hdinsight](hdinsight-linux-ambari-ssh-tunnel.md) dokumentum.

2. A következő parancsok toohave Solr index mintaadatok hello használata:

    ```bash
    cd /usr/hdp/current/solr/example/exampledocs
    java -jar post.jar solr.xml monitor.xml
    ```

    hello következő kimenetet visszaadja toohello konzol:

        POSTing file solr.xml
        POSTing file monitor.xml
        2 files indexed.
        COMMITting Solr index changes toohttp://localhost:8983/solr/update..
        Time spent: 0:00:01.624

    Hello `post.jar` segédprogram hozzáadja hello **solr.xml** és **monitor.xml** dokumentumok toohello index.
  
3. A következő parancs tooquery hello Solr REST API hello használata:

    ```bash
    curl "http://localhost:8983/solr/collection1/select?q=*%3A*&wt=json&indent=true"
    ```

    Ez a parancs keres **collection1** megfelelő dokumentumokhoz  **\*:\***  (kódolva \*% 3A\* hello a lekérdezési karakterláncban). a következő JSON-dokumentum hello hello válasz példája:

            "response": {
                "numFound": 2,
                "start": 0,
                "maxScore": 1,
                "docs": [
                  {
                    "id": "SOLR1000",
                    "name": "Solr, hello Enterprise Search Server",
                    "manu": "Apache Software Foundation",
                    "cat": [
                      "software",
                      "search"
                    ],
                    "features": [
                      "Advanced Full-Text Search Capabilities using Lucene",
                      "Optimized for High Volume Web Traffic",
                      "Standards Based Open Interfaces - XML and HTTP",
                      "Comprehensive HTML Administration Interfaces",
                      "Scalability - Efficient Replication tooother Solr Search Servers",
                      "Flexible and Adaptable with XML configuration and Schema",
                      "Good unicode support: héllo (hello with an accent over hello e)"
                    ],
                    "price": 0,
                    "price_c": "0,USD",
                    "popularity": 10,
                    "inStock": true,
                    "incubationdate_dt": "2006-01-17T00:00:00Z",
                    "_version_": 1486960636996878300
                  },
                  {
                    "id": "3007WFP",
                    "name": "Dell Widescreen UltraSharp 3007WFP",
                    "manu": "Dell, Inc.",
                    "manu_id_s": "dell",
                    "cat": [
                      "electronics and computer1"
                    ],
                    "features": [
                      "30\" TFT active matrix LCD, 2560 x 1600, .25mm dot pitch, 700:1 contrast"
                    ],
                    "includes": "USB cable",
                    "weight": 401.6,
                    "price": 2199,
                    "price_c": "2199,USD",
                    "popularity": 6,
                    "inStock": true,
                    "store": "43.17614,-90.57341",
                    "_version_": 1486960637584081000
                  }
                ]
              }

### <a name="using-hello-solr-dashboard"></a>Hello Solr irányítópult használata

hello Solr irányítópult a webes felhasználói felület, amely lehetővé teszi a Solr toowork webböngésző segítségével. hello Solr irányítópult közvetlenül a hello Internet a HDInsight-fürtjéhez nem lesz közzétéve. Egy SSH-alagút tooaccess használhatja azt. További információ az SSH-alagút használatával, lásd: hello [használata SSH Tunneling hdinsight](hdinsight-linux-ambari-ssh-tunnel.md) dokumentum.

Az SSH-alagút létesítése, használja a következő lépéseket toouse hello Solr irányítópult hello:

1. Hello host name hello elsődleges headnode határozzák meg:

   1. SSH tooconnect toohello átjárócsomóponthoz használja. Például: `ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`.

       További információ az SSH használatával, lásd: hello [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

   2. A következő parancs tooget hello teljesen minősített állomásnév hello használata:

        ```bash
        hostname -f
        ```

        Ez a parancs visszaadja a gazdagép neve a következő érték hasonló toohello:

            hn0-myhdi-nfebtpfdv1nubcidphpap2eq2b.ex.internal.cloudapp.net

        Hello érték lett visszaadva, akkor menteni, mert a rendszer később.

2. A böngészőben csatlakozzon túl**http://HOSTNAME:8983/solr / #/**, ahol **ÁLLOMÁSNÉV** hello előző lépésben maghatározott hello neve.

    hello kérelem hello SSH alagút toohello Solr webes felhasználói felület a fürt keresztül történik. hello lap jelenik meg a következő kép hasonló toohello:

    ![Solr irányítópult képe](./media/hdinsight-hadoop-solr-install-linux/solrdashboard.png)

3. Hello bal oldali ablaktáblán, használja a hello **Core választó** legördülő tooselect **collection1**. Több bejegyzést kell őket alatt jelennek meg **collection1**.

4. Az alábbi hello bejegyzései **collection1**, jelölje be **lekérdezés**. A következő értékek toopopulate hello keresőoldalt hello használata:

   * A hello **q** szöveget adja meg a  **\*:**\*. Ez a lekérdezés minden olyan hello dokumentumok indexelt Solr adja vissza. Ha azt szeretné, toosearch hello dokumentumok belül egy adott karakterláncot, karakterláncokat Itt adhatja meg.
   * A hello **wt** szövegmezőben, jelölje be hello kimeneti formátum. Alapértelmezett érték a **json**.

     Végül válassza ki a hello **lekérdezés végrehajtása** hello keresési pate hello alján gombra.

     ![Parancsfájlművelet toocustomize egy fürt használja.](./media/hdinsight-hadoop-solr-install-linux/hdi-solr-dashboard-query.png)

     hello kimeneti toohello hozzáadásának két dokumentumok korábbi index hello adja vissza. a kimeneti hello van a hasonló toohello következő JSON-dokumentum:

           "response": {
               "numFound": 2,
               "start": 0,
               "maxScore": 1,
               "docs": [
                 {
                   "id": "SOLR1000",
                   "name": "Solr, hello Enterprise Search Server",
                   "manu": "Apache Software Foundation",
                   "cat": [
                     "software",
                     "search"
                   ],
                   "features": [
                     "Advanced Full-Text Search Capabilities using Lucene",
                     "Optimized for High Volume Web Traffic",
                     "Standards Based Open Interfaces - XML and HTTP",
                     "Comprehensive HTML Administration Interfaces",
                     "Scalability - Efficient Replication tooother Solr Search Servers",
                     "Flexible and Adaptable with XML configuration and Schema",
                     "Good unicode support: héllo (hello with an accent over hello e)"
                   ],
                   "price": 0,
                   "price_c": "0,USD",
                   "popularity": 10,
                   "inStock": true,
                   "incubationdate_dt": "2006-01-17T00:00:00Z",
                   "_version_": 1486960636996878300
                 },
                 {
                   "id": "3007WFP",
                   "name": "Dell Widescreen UltraSharp 3007WFP",
                   "manu": "Dell, Inc.",
                   "manu_id_s": "dell",
                   "cat": [
                     "electronics and computer1"
                   ],
                   "features": [
                     "30\" TFT active matrix LCD, 2560 x 1600, .25mm dot pitch, 700:1 contrast"
                   ],
                   "includes": "USB cable",
                   "weight": 401.6,
                   "price": 2199,
                   "price_c": "2199,USD",
                   "popularity": 6,
                   "inStock": true,
                   "store": "43.17614,-90.57341",
                   "_version_": 1486960637584081000
                 }
               ]
             }

### <a name="starting-and-stopping-solr"></a>Indítása és leállítása Solr

A következő parancsok toomanually állítsa le és indítsa el a Solr hello használata:

```bash
sudo stop solr
sudo start solr
```

## <a name="backup-indexed-data"></a>Biztonsági mentési indexelt adatokat

A következő lépéseket tooback Solr adatok toohello alapértelmezett szolgáltatás a fürt telepítése hello használata:

1. Csatlakozzon az SSH használatával toohello fürt, majd a következő parancs tooget hello host name hello átjárócsomópont hello használata:

    ```bash
    hostname -f
    ```

2. A következő parancs toocreate indexelt hello adatok pillanatképe hello használata. Cserélje le **ÁLLOMÁSNÉV** hello előző parancs által visszaadott hello nevű:

    ```bash
    curl http://HOSTNAME:8983/solr/replication?command=backup
    ```

    a rendszer a következő XML hasonló toohello hello választ:

        <?xml version="1.0" encoding="UTF-8"?>
        <response>
          <lst name="responseHeader">
            <int name="status">0</int>
            <int name="QTime">9</int>
          </lst>
          <str name="status">OK</str>
        </response>

3. Módosítsa a könyvtárat túl`/usr/hdp/current/solr/example/solr`. Nincs gyűjtemény itt alkönyvtárat. Minden gyűjtemény könyvtár neve tartalmazza a `data` hello pillanatkép hello gyűjtemény tartalmazó könyvtárat.

4. a tömörített archívum hello pillanatkép mappa, a következő parancs használata hello toocreate:

    ```bash
    tar -zcf snapshot.20150806185338855.tgz snapshot.20150806185338855
    ```

    Cserélje le a hello `snapshot.20150806185338855` hello nevet a gyűjtemény hello pillanatkép értékeket.

    Ez a parancs létrehozza az archívumot nevű **snapshot.20150806185338855.tgz**, hello hello tartalmát tartalmazó **snapshot.20150806185338855** könyvtár.

5. Majd tárolhatja elsődleges tárolóhely hello archív toohello fürt hello a következő parancsot:

    ```bash
    hdfs dfs -put snapshot.20150806185338855.tgz /example/data
    ```

Solr biztonsági mentés és visszaállítás használatához további információkért lásd: [https://cwiki.apache.org/confluence/display/solr/Making+and+Restoring+Backups](https://cwiki.apache.org/confluence/display/solr/Making+and+Restoring+Backups).

## <a name="next-steps"></a>Következő lépések

* [Giraph telepíthető a HDInsight-fürtök](hdinsight-hadoop-giraph-install-linux.md). Fürt testreszabási tooinstall Giraph a HDInsight Hadoop-fürtök használata. Giraph lehetővé teszi a Hadoop használatával tooperform diagramfeldolgozás, és az Azure HDInsight használható.

* [A HDInsight-fürtökön Hue telepítése](hdinsight-hadoop-hue-linux.md). HDInsight Hadoop-fürtök fürt testreszabási tooinstall Hue használja. Hue, a webalkalmazások a használt toointeract egy Hadoop-fürthöz.

[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
