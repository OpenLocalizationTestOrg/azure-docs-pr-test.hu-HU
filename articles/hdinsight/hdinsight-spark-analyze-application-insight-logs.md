---
title: "a Spark - Azure HDInsight naplózza Application Insights aaaAnalyze |} Microsoft Docs"
description: "Ismerje meg, hogy miként naplózza az Application Insights tooexport a tooblob tárolási, és majd elemezheti a Spark on HDInsight hello naplókat."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 883beae6-9839-45b5-94f7-7eb0f4534ad5
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/15/2017
ms.author: larryfr
ms.openlocfilehash: 11ed8cf68dba8d5f9d6e4a65eba0d2b5a950cd00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-application-insights-telemetry-logs-with-spark-on-hdinsight"></a>A Spark on HDInsight az Application Insights telemetria naplóinak elemzése

Megtudhatja, hogyan toouse Spark, a HDInsight tooanalyze Application Insights telemetriai adatokat.

[A Visual Studio Application Insights](../application-insights/app-insights-overview.md) analytics szolgáltatás, amely figyeli a webes alkalmazások. Lehet, hogy az Application Insights által generált telemetriai adatokat exportált tooAzure tároló. Ha hello adatokat az Azure Storage, HDInsight lehet használt tooanalyze azt.

## <a name="prerequisites"></a>Előfeltételek

* Alkalmazás toouse Application Insights konfigurálva.

* Linux-alapú HDInsight-fürt létrehozása ismeretét. További információkért lásd: [hozzon létre a Spark on HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

  > [!IMPORTANT]
  > hello jelen dokumentumban leírt lépések egy HDInsight-fürt által használt Linux igényelnek. Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* Egy webböngészőben.

hello következőket használták a fejlesztés és tesztelés Ez a dokumentum:

* Application Insights telemetria adatok generálta egy [Node.js webalkalmazás konfigurált toouse Application Insights](../application-insights/app-insights-nodejs.md).

* A Linux-alapú a Spark on HDInsight-fürt verziószáma 3.5 használt tooanalyze hello adatok volt.

## <a name="architecture-and-planning"></a>Architektúra és tervezése

a következő diagram hello hello szolgáltatás architektúrája ebben a példában mutatja be:

![az Application Insights tooblob tárolásból áramló adatok jelennek meg, majd a Spark on HDInsight által feldolgozott diagramja](./media/hdinsight-spark-analyze-application-insight-logs/appinsightshdinsight.png)

### <a name="azure-storage"></a>Azure Storage

Az Application Insights konfigurált toocontinuously exportálás telemetriai adatokat tooblobs lehet. HDInsight majd elolvashatják hello blobok tárolt adatokat. Vannak azonban olyan követelményekkel, amelyeket kell követnie:

* **Hely**: Ha hello Tárfiók és a HDInsight különböző helyeken vannak, megnövelheti késés. Költség szempontjából, is vagy kimenő díjak sem régiók közötti áthelyezése alkalmazott toodata növekszik.

    > [!WARNING]
    > A Storage-fiók egy másik helyen, mint a HDInsight használata nem támogatott.

* **BLOB-típusú**: HDInsight csak blokk blobokat támogat. Az Application Insights toousing blokkblobokat, és a HDInsight együttes alapértelmezés szerint kell működnie, alapértelmezés szerint.

További tárhely tooan meglévő HDInsight-fürt hozzáadására vonatkozó információkért lásd: hello [adja hozzá a további tárfiókok](hdinsight-hadoop-add-storage.md) dokumentum.

### <a name="data-schema"></a>Adatséma

Az Application Insights biztosít [exportálja az adatokat az adatmodellbe](../application-insights/app-insights-export-data-model.md) tooblobs exportálják hello telemetriai adatok formátuma. jelen dokumentumban leírt lépések hello használata Spark SQL toowork hello adatokkal. Spark SQL hello Application Insights által naplózott JSON-adatszerkezet sémát automatikusan elő tud állítani.

## <a name="export-telemetry-data"></a>Telemetriai adatok exportálása

Hello kövesse [konfigurálása a folyamatos exportálás](../application-insights/app-insights-export-telemetry.md) tooconfigure az Application Insights tooexport telemetriai adatokat tooan az Azure storage-blob.

## <a name="configure-hdinsight-tooaccess-hello-data"></a>HDInsight tooaccess hello adatok konfigurálása

HDInsight-fürtök létrehozásakor hello tárfiók hozzáadása a fürt létrehozása során.

tooadd hello Azure Storage-fiók tooan meglévő fürt használja hello adatokat hello [adja hozzá a további Tárfiókok](hdinsight-hadoop-add-storage.md) dokumentum.

## <a name="analyze-hello-data-pyspark"></a>Hello adatok elemzése: PySpark

1. A hello [Azure-portálon](https://portal.azure.com), válassza ki a Spark on HDInsight-fürt. A hello **Gyorshivatkozások** szakaszban jelölje be **fürt irányítópultok**, majd válassza ki **Jupyter Notebook** hello fürt Dashboard__ paneljén.

    ![hello fürt irányítópultok](./media/hdinsight-spark-analyze-application-insight-logs/clusterdashboards.png)

2. Hello jobb felső sarkában hello Jupyter lapra, válassza ki **új**, majd **PySpark**. A Python-alapú Jupyter Notebook tartalmazó új böngészőlapon nyílik meg.

3. Első mezőben hello (nevű egy **cella**) hello lapon adja meg a következő szöveg hello:

   ```python
   sc._jsc.hadoopConfiguration().set('mapreduce.input.fileinputformat.input.dir.recursive', 'true')
   ```

    Ez a kód Spark toorecursively hozzáférés hello könyvtárstruktúrát hello bemeneti adatok konfigurálja. Application Insights telemetria naplózott tooa directory szerkezete hasonló toohello `/{telemetry type}/YYYY-MM-DD/{##}/`.

4. Használjon **SHIFT + ENTER** toorun hello kódot. A bal oldalán található hello cella hello egy "\*" hello zárójeleket tooindicate között jelenik meg, hogy ezt a cellát hello kód végrehajtása zajlik. Amint befejeződött, hello "\*" tooa számát, valamint a következő szöveg hasonló toohello hello cella alább látható kimeneti módosítja:

        Creating SparkContext as 'sc'

        ID    YARN Application ID    Kind    State    Spark UI    Driver log    Current session?
        3    application_1468969497124_0001    pyspark    idle    Link    Link    ✔

        Creating HiveContext as 'sqlContext'
        SparkContext and HiveContext created. Executing user code ...
5. Egy új cella alatt hello jön létre először egy. Adja meg a következő szöveget: hello új cella hello. Cserélje le `CONTAINER` és `STORAGEACCOUNT` hello az Azure storage-fiók nevét és az Application Insights-adatokat tartalmazó blob-tároló neve.

   ```python
   %%bash
   hdfs dfs -ls wasb://CONTAINER@STORAGEACCOUNT.blob.core.windows.net/
   ```

    Használjon **SHIFT + ENTER** tooexecute ezt a cellát. Egy hasonló toohello eredményt, a következő szöveg jelenik meg:

        Found 1 items
        drwxrwxrwx   -          0 1970-01-01 00:00 wasb://appinsights@contosostore.blob.core.windows.net/contosoappinsights_2bededa61bc741fbdee6b556571a4831

    hello wasb visszaadott elérési út hello Application Insights telemetria adatok hello helyét. Változás hello `hdfs dfs -ls` sor a hello cella toouse hello wasb visszaadott elérési út, és ezután **SHIFT + ENTER** toorun hello cella újra. Megadott idő hello eredmények megjelenjen-e telemetriai adatokat tartalmazó hello könyvtárak.

   > [!NOTE]
   > A hello hello maradéka ebben a szakaszban ismertetett visszaállítási lépésekkel, hello `wasb://appinsights@contosostore.blob.core.windows.net/contosoappinsights_{ID}/Requests` directory lett megadva. Lehet, hogy a könyvtárstruktúra különböző.

6. A következő cella hello, adja meg a következő kód hello: cserélje le `WASB_PATH` hello előző lépésben hello elérési úttal.

   ```python
   jsonFiles = sc.textFile('WASB_PATH')
   jsonData = sqlContext.read.json(jsonFiles)
   ```

    Ez a kód egy dataframe hello folyamatos exportálási folyamat által exportált hello JSON-fájlokat hoz létre. Használjon **SHIFT + ENTER** toorun ezt a cellát.
7. Hello következő cellában adja meg, és futtassa a következő tooview hello séma, Spark létrehozott hello JSON-fájlok hello:

   ```python
   jsonData.printSchema()
   ```

    az egyes telemetriai hello séma nem egyezik. hello következő példája az webes kérelmek létrehozott hello séma (hello tárolt adatok `Requests` alkönyvtár):

        root
        |-- context: struct (nullable = true)
        |    |-- application: struct (nullable = true)
        |    |    |-- version: string (nullable = true)
        |    |-- custom: struct (nullable = true)
        |    |    |-- dimensions: array (nullable = true)
        |    |    |    |-- element: string (containsNull = true)
        |    |    |-- metrics: array (nullable = true)
        |    |    |    |-- element: string (containsNull = true)
        |    |-- data: struct (nullable = true)
        |    |    |-- eventTime: string (nullable = true)
        |    |    |-- isSynthetic: boolean (nullable = true)
        |    |    |-- samplingRate: double (nullable = true)
        |    |    |-- syntheticSource: string (nullable = true)
        |    |-- device: struct (nullable = true)
        |    |    |-- browser: string (nullable = true)
        |    |    |-- browserVersion: string (nullable = true)
        |    |    |-- deviceModel: string (nullable = true)
        |    |    |-- deviceName: string (nullable = true)
        |    |    |-- id: string (nullable = true)
        |    |    |-- osVersion: string (nullable = true)
        |    |    |-- type: string (nullable = true)
        |    |-- location: struct (nullable = true)
        |    |    |-- city: string (nullable = true)
        |    |    |-- clientip: string (nullable = true)
        |    |    |-- continent: string (nullable = true)
        |    |    |-- country: string (nullable = true)
        |    |    |-- province: string (nullable = true)
        |    |-- operation: struct (nullable = true)
        |    |    |-- name: string (nullable = true)
        |    |-- session: struct (nullable = true)
        |    |    |-- id: string (nullable = true)
        |    |    |-- isFirst: boolean (nullable = true)
        |    |-- user: struct (nullable = true)
        |    |    |-- anonId: string (nullable = true)
        |    |    |-- isAuthenticated: boolean (nullable = true)
        |-- internal: struct (nullable = true)
        |    |-- data: struct (nullable = true)
        |    |    |-- documentVersion: string (nullable = true)
        |    |    |-- id: string (nullable = true)
        |-- request: array (nullable = true)
        |    |-- element: struct (containsNull = true)
        |    |    |-- count: long (nullable = true)
        |    |    |-- durationMetric: struct (nullable = true)
        |    |    |    |-- count: double (nullable = true)
        |    |    |    |-- max: double (nullable = true)
        |    |    |    |-- min: double (nullable = true)
        |    |    |    |-- sampledValue: double (nullable = true)
        |    |    |    |-- stdDev: double (nullable = true)
        |    |    |    |-- value: double (nullable = true)
        |    |    |-- id: string (nullable = true)
        |    |    |-- name: string (nullable = true)
        |    |    |-- responseCode: long (nullable = true)
        |    |    |-- success: boolean (nullable = true)
        |    |    |-- url: string (nullable = true)
        |    |    |-- urlData: struct (nullable = true)
        |    |    |    |-- base: string (nullable = true)
        |    |    |    |-- hashTag: string (nullable = true)
        |    |    |    |-- host: string (nullable = true)
        |    |    |    |-- protocol: string (nullable = true)
8. Használja a következő tooregister hello dataframe ideiglenes táblaként hello és lekérdezésével hello adatokat:

   ```python
   jsonData.registerTempTable("requests")
   df = sqlContext.sql("select context.location.city from requests where context.location.city is not null")
   df.show()
   ```

    A lekérdezés által visszaadott hello Város adatainak hello felső 20 rekordok, ahol context.location.city értéke nem null.

   > [!NOTE]
   > hello környezetben struktúra megtalálható-e az Application Insights által naplózott összes telemetriai adat. hello város elem nem lehet megadni a naplókban. Használjon hello séma tooidentify más elemeket lekérdezhető a naplók adatok szerepelhetnek.

    A lekérdezés által visszaadott adatokat hasonló toohello a következő szöveget:

        +---------+
        |     city|
        +---------+
        | Bellevue|
        |  Redmond|
        |  Seattle|
        |Charlotte|
        ...
        +---------+

## <a name="analyze-hello-data-scala"></a>Hello adatok elemzése: Scala

1. A hello [Azure-portálon](https://portal.azure.com), válassza ki a Spark on HDInsight-fürt. A hello **Gyorshivatkozások** szakaszban jelölje be **fürt irányítópultok**, majd válassza ki **Jupyter Notebook** hello fürt Dashboard__ paneljén.

    ![hello fürt irányítópultok](./media/hdinsight-spark-analyze-application-insight-logs/clusterdashboards.png)
2. Hello jobb felső sarkában hello Jupyter lapra, válassza ki **új**, majd **Scala**. Scala-alapú Jupyter Notebook tartalmazó új böngészőlapon jelenik meg.
3. Első mezőben hello (nevű egy **cella**) hello lapon adja meg a következő szöveg hello:

   ```scala
   sc.hadoopConfiguration.set("mapreduce.input.fileinputformat.input.dir.recursive", "true")
   ```

    Ez a kód Spark toorecursively hozzáférés hello könyvtárstruktúrát hello bemeneti adatok konfigurálja. Az Application Insights telemetria van naplózva tooa könyvtárstruktúrát hasonló túl`/{telemetry type}/YYYY-MM-DD/{##}/`.

4. Használjon **SHIFT + ENTER** toorun hello kódot. A bal oldalán található hello cella hello egy "\*" hello zárójeleket tooindicate között jelenik meg, hogy ezt a cellát hello kód végrehajtása zajlik. Amint befejeződött, hello "\*" tooa számát, valamint a következő szöveg hasonló toohello hello cella alább látható kimeneti módosítja:

        Creating SparkContext as 'sc'

        ID    YARN Application ID    Kind    State    Spark UI    Driver log    Current session?
        3    application_1468969497124_0001    spark    idle    Link    Link    ✔

        Creating HiveContext as 'sqlContext'
        SparkContext and HiveContext created. Executing user code ...
5. Egy új cella alatt hello jön létre először egy. Adja meg a következő szöveget: hello új cella hello. Cserélje le `CONTAINER` és `STORAGEACCOUNT` hello az Azure storage-fiók nevét és a blobtároló neve, amely tartalmazza az Application Insights naplózza.

   ```scala
   %%bash
   hdfs dfs -ls wasb://CONTAINER@STORAGEACCOUNT.blob.core.windows.net/
   ```

    Használjon **SHIFT + ENTER** tooexecute ezt a cellát. Egy hasonló toohello eredményt, a következő szöveg jelenik meg:

        Found 1 items
        drwxrwxrwx   -          0 1970-01-01 00:00 wasb://appinsights@contosostore.blob.core.windows.net/contosoappinsights_2bededa61bc741fbdee6b556571a4831

    hello wasb visszaadott elérési út hello Application Insights telemetria adatok hello helyét. Változás hello `hdfs dfs -ls` sor a hello cella toouse hello wasb visszaadott elérési út, és ezután **SHIFT + ENTER** toorun hello cella újra. Megadott idő hello eredmények megjelenjen-e telemetriai adatokat tartalmazó hello könyvtárak.

   > [!NOTE]
   > A hello hello maradéka ebben a szakaszban ismertetett visszaállítási lépésekkel, hello `wasb://appinsights@contosostore.blob.core.windows.net/contosoappinsights_{ID}/Requests` directory lett megadva. Ez a könyvtár nem létezik, kivéve, ha a telemetriai adatok egy webalkalmazás.

6. A következő cella hello, adja meg a következő kód hello: cserélje le `WASB\_PATH` hello előző lépésben hello elérési úttal.

   ```scala
   var jsonFiles = sc.textFile('WASB_PATH')
   val sqlContext = new org.apache.spark.sql.SQLContext(sc)
   var jsonData = sqlContext.read.json(jsonFiles)
   ```

    Ez a kód egy dataframe hello folyamatos exportálási folyamat által exportált hello JSON-fájlokat hoz létre. Használjon **SHIFT + ENTER** toorun ezt a cellát.

7. Hello következő cellában adja meg, és futtassa a következő tooview hello séma, Spark létrehozott hello JSON-fájlok hello:

   ```scala
   jsonData.printSchema
   ```

    az egyes telemetriai hello séma nem egyezik. hello következő példája az webes kérelmek létrehozott hello séma (hello tárolt adatok `Requests` alkönyvtár):

        root
        |-- context: struct (nullable = true)
        |    |-- application: struct (nullable = true)
        |    |    |-- version: string (nullable = true)
        |    |-- custom: struct (nullable = true)
        |    |    |-- dimensions: array (nullable = true)
        |    |    |    |-- element: string (containsNull = true)
        |    |    |-- metrics: array (nullable = true)
        |    |    |    |-- element: string (containsNull = true)
        |    |-- data: struct (nullable = true)
        |    |    |-- eventTime: string (nullable = true)
        |    |    |-- isSynthetic: boolean (nullable = true)
        |    |    |-- samplingRate: double (nullable = true)
        |    |    |-- syntheticSource: string (nullable = true)
        |    |-- device: struct (nullable = true)
        |    |    |-- browser: string (nullable = true)
        |    |    |-- browserVersion: string (nullable = true)
        |    |    |-- deviceModel: string (nullable = true)
        |    |    |-- deviceName: string (nullable = true)
        |    |    |-- id: string (nullable = true)
        |    |    |-- osVersion: string (nullable = true)
        |    |    |-- type: string (nullable = true)
        |    |-- location: struct (nullable = true)
        |    |    |-- city: string (nullable = true)
        |    |    |-- clientip: string (nullable = true)
        |    |    |-- continent: string (nullable = true)
        |    |    |-- country: string (nullable = true)
        |    |    |-- province: string (nullable = true)
        |    |-- operation: struct (nullable = true)
        |    |    |-- name: string (nullable = true)
        |    |-- session: struct (nullable = true)
        |    |    |-- id: string (nullable = true)
        |    |    |-- isFirst: boolean (nullable = true)
        |    |-- user: struct (nullable = true)
        |    |    |-- anonId: string (nullable = true)
        |    |    |-- isAuthenticated: boolean (nullable = true)
        |-- internal: struct (nullable = true)
        |    |-- data: struct (nullable = true)
        |    |    |-- documentVersion: string (nullable = true)
        |    |    |-- id: string (nullable = true)
        |-- request: array (nullable = true)
        |    |-- element: struct (containsNull = true)
        |    |    |-- count: long (nullable = true)
        |    |    |-- durationMetric: struct (nullable = true)
        |    |    |    |-- count: double (nullable = true)
        |    |    |    |-- max: double (nullable = true)
        |    |    |    |-- min: double (nullable = true)
        |    |    |    |-- sampledValue: double (nullable = true)
        |    |    |    |-- stdDev: double (nullable = true)
        |    |    |    |-- value: double (nullable = true)
        |    |    |-- id: string (nullable = true)
        |    |    |-- name: string (nullable = true)
        |    |    |-- responseCode: long (nullable = true)
        |    |    |-- success: boolean (nullable = true)
        |    |    |-- url: string (nullable = true)
        |    |    |-- urlData: struct (nullable = true)
        |    |    |    |-- base: string (nullable = true)
        |    |    |    |-- hashTag: string (nullable = true)
        |    |    |    |-- host: string (nullable = true)
        |    |    |    |-- protocol: string (nullable = true)

8. Használja a következő tooregister hello dataframe ideiglenes táblaként hello és lekérdezésével hello adatokat:

   ```scala
   jsonData.registerTempTable("requests")
   var city = sqlContext.sql("select context.location.city from requests where context.location.city is not null limit 10").show()
   ```

    A lekérdezés által visszaadott hello Város adatainak hello felső 20 rekordok, ahol context.location.city értéke nem null.

   > [!NOTE]
   > hello környezetben struktúra megtalálható-e az Application Insights által naplózott összes telemetriai adat. hello város elem nem lehet megadni a naplókban. Használjon hello séma tooidentify más elemeket lekérdezhető a naplók adatok szerepelhetnek.
   >
   >

    A lekérdezés által visszaadott adatokat hasonló toohello a következő szöveget:

        +---------+
        |     city|
        +---------+
        | Bellevue|
        |  Redmond|
        |  Seattle|
        |Charlotte|
        ...
        +---------+

## <a name="next-steps"></a>Következő lépések

Adatok és az Azure Spark toowork használatával további példákért lásd a következő dokumentumok hello:

* [Spark és BI: Interaktív adatelemzés végrehajtása a Spark on HDInsight használatával, BI-eszközökkel](hdinsight-apache-spark-use-bi-tools.md)
* [Spark és Machine Learning: A Spark on HDInsight használata az épület-hőmérséklet elemzésére HVAC-adatok alapján](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark és Machine Learning: használja a Spark on HDInsight toopredict élelmiszervizsgálati eredmények](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Spark Streaming: Spark on HDInsight használata az adatfolyam-továbbítási alkalmazások létrehozásához](hdinsight-apache-spark-eventhub-streaming.md)
* [A webhelynapló elemzése a Spark on HDInsight használatával](hdinsight-apache-spark-custom-library-website-log-analysis.md)

A létrehozása és alkalmazások futtatása Spark információkért lásd: a következő dokumentumok hello:

* [Önálló alkalmazás létrehozása a Scala használatával](hdinsight-apache-spark-create-standalone-application.md)
* [Feladatok távoli futtatása Spark-fürtön a Livy használatával](hdinsight-apache-spark-livy-rest-interface.md)
