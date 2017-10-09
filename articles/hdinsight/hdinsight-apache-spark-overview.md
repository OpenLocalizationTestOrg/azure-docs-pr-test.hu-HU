---
title: az Azure HDInsight aaaIntroduction tooSpark |} Microsoft Docs
description: "Ez a cikk Ez egy bevezető tooSpark HDInsight és hello különböző forgatókönyvek, amelyben a HDInsight Spark-fürt is használja."
keywords: "Mi az apache spark, a spark-fürt, a bevezetés toospark, a spark on hdinsight"
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 82334b9e-4629-4005-8147-19f875c8774e
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/12/2017
ms.author: nitinme
ms.openlocfilehash: 41996e733618b8534469fa239b980ac50161a535
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toospark-on-hdinsight"></a>A HDInsight bemutatása tooSpark

Ez a cikk egy a HDInsight bemutatása tooSpark biztosít. <a href="http://spark.apache.org/" target="_blank">Az Apache Spark on</a> egy nyílt forráskódú párhuzamos feldolgozást végző keretrendszer, amely támogatja a memórián belüli feldolgozását végzi big data elemző alkalmazások tooboost hello teljesítményét. A HDInsight-alapú Spark-fürt kompatibilis az Azure Storage (WASB) és az Azure Data Lake Store szolgáltatással, így a Spark-fürt segítségével könnyen feldolgozhatók az Azure-ban tárolt meglévő adatok.

A HDInsight-alapú Spark-fürt létrehozásával egyben olyan Azure-beli számítási erőforrásokat is létrehoz, amelyeken a Spark telepítve és konfigurálva van. Csak toocreate a Spark on hdinsight fürt nagyjából tíz percet vesz igénybe. Azure Storage vagy az Azure Data Lake Store hello adatok toobe feldolgozott tárolja. Lásd: [Az Azure Storage és a HDInsight együttes használata](hdinsight-hadoop-use-blob-storage.md).

**a HDInsight fürt toocreate egy Spark**, lásd: [gyors üzembe helyezés: a HDInsight Spark-fürt létrehozása és futtatása a Jupyter használatával interaktív lekérdezés](hdinsight-apache-spark-jupyter-spark-sql.md).


## <a name="what-is-apache-spark-on-azure-hdinsight"></a>Mi az Azure HDInsight-alapú Apache Spark?
A HDInsight-alapú Spark-fürtök teljes körűen felügyelt Spark szolgáltatást nyújtanak. A HDInsight-alapú Spark-fürt létrehozásának előnyeit ez a lista foglalja össze.

| Szolgáltatás | Leírás |
| --- | --- |
| A Spark-fürtök könnyen létrehozhatók |Létrehozhat egy új Spark-fürt a HDInsight hello Azure portál, Azure PowerShell vagy hello HDInsight .NET SDK használatával percek alatt. Lásd: [Spark-fürt a HDInsightban – első lépések](hdinsight-apache-spark-jupyter-spark-sql.md) |
| Könnyű használat |A HDInsight-alapú Spark-fürt tartalmazza a Jupyter és a Zeppelin notebookokat. Ezeket interaktív adatfeldolgozásra és -megjelenítésre használhatja.|
| REST API-k |Hdinsight Spark-fürtök [Livy](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server), a REST API-alapú Spark feladat server tooremotely küldés és a figyelő feladatok. |
| Az Azure Data Lake Store támogatása | A HDInsight Spark-fürt konfigurált toouse Azure Data Lake Store, egy további tárterületet, valamint a elsődleges tárolási (csak a 3.5-ös verzióját a HDInsight-fürtök) lehet. További információk a Data Lake Store-ról: [Áttekintés: Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md). |
| Integráció az Azure-szolgáltatásokkal |A HDInsight Spark-fürt rendelkezik egy összekötő tooAzure Event Hubs. Ügyfelek használatával hello Event Hubs, továbbá túl streamelési alkalmazásokat állíthatnak[Kafka](http://kafka.apache.org/), amely már rendelkezésre áll a Spark részeként. |
| Támogatás az R Serverhez | Az R Server on HDInsight Spark-fürt toorun elosztott R-számítások a hello jellemző sebesség mellett egy Spark-fürt állíthat be. További információk: [Get started using R Server on HDInsight](hdinsight-hadoop-r-server-get-started.md) (R Server on HDInsight – első lépések). |
| Integráció külső integrált fejlesztői környezetekkel (IDE) | HDInsight beépülő modulok például IntelliJ IDEA és, hogy toocreate használ, és küldje el az alkalmazások tooan HDInsight Spark-fürt Eclipse IDEs biztosít. További információ: [Az IntelliJ IDEA-hoz készült Azure-eszközkészlet használata](hdinsight-apache-spark-intellij-tool-plugin.md) és [Az Eclipse-hez készült Azure eszközkészlet használata](hdinsight-apache-spark-eclipse-tool-plugin.md).|
| Egyidejű lekérdezések |A HDInsight-alapú Spark-fürtök támogatják az egyidejű lekérdezéseket. Ezzel lehetővé válik, hogy egy felhasználó vagy a különböző felhasználók származó több lekérdezés és alkalmazások tooshare hello ugyanazokat a fürterőforrásokat. |
| Gyorsítótárazás SSD meghajtókon |Választhatja toocache adatok a memóriában vagy SSD meghajtók csatolva toohello fürtcsomópontok. A memóriában történő gyorsítótárazás hello legjobb lekérdezési teljesítményt biztosít, azonban drága lehet; gyorsítótárazás SSD-k hello kell toocreate szükséges toofit hello teljes adatkészlet memória méretének fürtben lekérdezési teljesítmény javításához nagyszerű lehetőséget biztosít. |
| Integráció BI-eszközökkel |A HDInsight-alapú Spark-fürtök összekötőket biztosítanak az olyan adatelemző BI-eszközök számára, mint a [Power BI](http://www.powerbi.com/) és a [Tableau](http://www.tableau.com/products/desktop). |
| Előre betöltött Anaconda-könyvtárak |A HDInsight Spark-fürtjei előre telepített Anaconda-könyvtárakkal rendelkeznek. [Anaconda](http://docs.continuum.io/anaconda/) Bezárás too200 szalagtárak biztosít a gépi tanulás, adatelemzés, a képi megjelenítés, stb. |
| Méretezhetőség |Bár a csomópontok száma hello adhat meg a fürt létrehozása során, toogrow szeretné, vagy hello toomatch fürtmunkaterhelés zsugorítani. HDInsight-fürtök esetében lehetővé teszi a csomópontok toochange hello számát hello fürt. Emellett a Spark-fürtök törölhetők adatvesztés nélkül óta az összes hello adatok Azure Storage vagy a Data Lake Store tárolják. |
| Napi 24 órás támogatás a hét minden napján |A HDInsight-alapú Spark-fürtökhöz a hét minden napján 24 órás vállalati szintű ügyfélszolgálat, valamint az SLA által garantált 99,9%-os üzemidő jár. |

## <a name="what-are-hello-use-cases-for-spark-on-hdinsight"></a>Mik a Spark on HDInsight hello alkalmazási esetei?
A HDInsight Spark-fürtjei főbb forgatókönyvek a következő hello engedélyezése.

### <a name="interactive-data-analysis-and-bi"></a>Interaktív adatelemzés és BI
[Oktatóanyag megtekintése](hdinsight-apache-spark-use-bi-tools.md)

A HDInsight-alapú Apache Spark az Azure Storage vagy az Azure Data Lake Store tárhelyein tárolja az adatokat. Az üzleti szakértők és a legfontosabb döntéshozók elemezheti és jelentéseket állíthatnak adatok és a Microsoft Power BI toobuild interaktív jelentések hello elemzett adatokból. Elemzők indítsa el a fürttároló strukturált strukturálatlan/részben adatokból, definiálhat egy sémát hello adatok notebookok használatával, és majd használatával adatmodelleket építhetnek Microsoft Power bi-ban. A HDInsight-alapú Spark-fürtök számos olyan külső BI-eszközt is támogatnak, mint a Tableau, így ideális platformot jelentenek az adatelemzők, az üzleti szakértők és a fő döntéshozók számára.

### <a name="spark-machine-learning"></a>Spark Machine Learning
[Oktatóanyag megtekintése: Épület-hőmérsékletek előrejelzése HVAC-adatok használatával](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

[Oktatóanyag megtekintése: Élelmiszervizsgálati eredmények előrejelzése](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

Az Apache Spark tartalmazza a Sparkra épülő [MLlib](http://spark.apache.org/mllib/) Machine Learning-könyvtárat, amelyet egy HDInsight-alapú Spark-fürtből használhat. A HDInsight-alapú Spark-fürt tartalmazza a Python által terjesztett, számos gépi tanulási csomaggal rendelkező Anaconda rendszert. Ha ehhez hozzáveszi a Jupyter és Zeppelin notebookok beépített támogatását is, csúcskategóriás környezetet kap a gépi tanulási alkalmazásokhoz.

### <a name="spark-streaming-and-real-time-data-analysis"></a>Spark-alapú streamelés és valós idejű adatelemzés
[Oktatóanyag megtekintése](hdinsight-apache-spark-eventhub-streaming.md)

A HDInsight-alapú Spark széles körű támogatást nyújt a valós idejű elemzési megoldások kiépítéséhez. Spark már összekötők tooingest adatok például Kafka, Flume, Twitter, ZeroMQ vagy TCP-szoftvercsatornák számos más forrásból, amíg a Spark on HDInsight ad osztályú támogatást kínál az Azure Event Hubs eseményközpontokból. Az Event Hubs hello legszélesebb körben használt Azure Üzenetsor-kezelés szolgáltatás. Az azonnal használható Event Hubs-támogatással a HDInsight-alapú Spark-fürtök ideális platformot nyújtanak a valós idejű elemzési folyamatok kiépítéséhez.

## <a name="next-steps"></a>Milyen összetevők találhatóak egy Spark-fürtben?
Hdinsight Spark-fürtjei hello következő hello fürtökön alapértelmezés szerint elérhető összetevőket tartalmazza.

* [Spark mag](https://spark.apache.org/docs/1.5.1/). A következőket tartalmazza: Spark mag, Spark SQL, Spark streamelési API-k, GraphX és MLlib.
* [Anaconda](http://docs.continuum.io/anaconda/)
* [Livy](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server)
* [Jupyter notebook](https://jupyter.org)
* [Zeppelin notebook](http://zeppelin-project.org/)

A HDInsight Spark-fürtjei is adjon meg egy [ODBC-illesztőprogram](http://go.microsoft.com/fwlink/?LinkId=616229) kapcsolat tooSpark fürt a Hdinsightban az Üzletiintelligencia-eszközök a Microsoft Power BI és a Tableau.

## <a name="where-do-i-start"></a>Hogyan kezdjek hozzá?
A HDInsight-alapú Spark-fürt létrehozásának első lépései. Lásd: [Rövid útmutató: Linux rendszerű HDInsight-alapú Spark-fürt létrehozása és interaktív lekérdezés futtatása a Jupyter használatával](hdinsight-apache-spark-jupyter-spark-sql.md). 

## <a name="next-steps"></a>Következő lépések
### <a name="scenarios"></a>Forgatókönyvek
* [Spark és BI: Interaktív adatelemzés végrehajtása a Spark on HDInsight használatával, BI-eszközökkel](hdinsight-apache-spark-use-bi-tools.md)
* [Spark és Machine Learning: A Spark on HDInsight használata az épület-hőmérséklet elemzésére HVAC-adatok alapján](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark és Machine Learning: használja a Spark on HDInsight toopredict élelmiszervizsgálati eredmények](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Spark Streaming: A Spark on HDInsight használata valós idejű streamelési alkalmazások összeállítására](hdinsight-apache-spark-eventhub-streaming.md)
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
* [Az Apache Spark ismert problémái az Azure HDInsightban](hdinsight-apache-spark-known-issues.md).
