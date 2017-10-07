---
title: "a Spark on Azure HDInsight Jupyter aaaUse egyéni Maven csomagok |} Microsoft Docs"
description: "Hogyan tooconfigure Jupyter notebookok, amely a HDInsight Spark-fürtök toouse egyéni Maven-csomagok részletes ismertetése."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 2a8bc545-064e-436f-8b5f-e67c26cfbf98
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: ba8ac13716bc94ab082a18fe02d4a40b2f1e09e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-external-packages-with-jupyter-notebooks-in-apache-spark-clusters-on-hdinsight"></a>Külső csomagok használata Jupyter notebookok Apache Spark-fürt a HDInsight
> [!div class="op_single_selector"]
> * [Cella magic használatával](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
> * [Parancsfájl művelet segítségével](hdinsight-apache-spark-python-package-installation.md)
>
>

Ismerje meg, hogyan tooconfigure Jupyter notebook az Apache Spark-fürt a HDInsight toouse külső, közösségi-hozzájárult **maven** csomagok, amelyek nem szerepelnek hello fürt out-of-az-box. 

Hello kereshet [Maven-tárház](http://search.maven.org/) hello csomagok rendelkezésre álló teljes listáját. Más forrásból is megkapható elérhető csomagok listáját. Például közösségi hozzájárult csomagok teljes listája megtalálható [Spark csomagok](http://spark-packages.org/).

Ebből a cikkből megtudhatja, hogyan toouse hello [spark-fürt megosztott kötetei szolgáltatás](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) hello Jupyter notebook számára.



## <a name="prerequisites"></a>Előfeltételek
Hello következő kell rendelkeznie:

* A HDInsight az Apache Spark-fürt. Útmutatásért lásd: [létrehozása az Apache Spark on Azure hdinsight clusters](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="use-external-packages-with-jupyter-notebooks"></a>Külső csomagok használata Jupyter notebookokkal
1. A hello [Azure Portal](https://portal.azure.com/), hello kezdőpulton, kattintson a Spark-fürt hello csempére (ha toohello kezdőpulton rögzítette azt). Tooyour fürt alapján is megtalálhatja **összes tallózása** > **a HDInsight-fürtök**.   
2. Hello Spark-fürt panelén kattintson **Gyorshivatkozások**, és majd a hello **fürt irányítópult** panelen kattintson a **Jupyter Notebook**. Ha a rendszer kéri, adja meg hello fürt hello rendszergazdai hitelesítő adataival.

    > [!NOTE]
    > A fürt URL-címet a böngészőben a következő megnyitásakor hello által is hello Jupyter Notebook lehet elérni. Cserélje le **CLUSTERNAME** hello néven a fürt:
    > 
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
    > 

   

3. Hozzon létre új notebookot. Kattintson a **új**, és kattintson a **Spark**.
   
    ![Új Jupyter notebook létrehozása](./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/hdinsight-spark-create-notebook.png "Új Jupyter notebook létrehozása")

4. Új notebook létrejött, és Untitled.pynb hello nevű. Hello notebook neve hello tetején kattintson, és adjon meg egy rövid nevet.
   
    ![Adjon meg egy nevet hello notebook](./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/hdinsight-spark-name-notebook.png "adjon meg egy nevet hello notebook")

5. Hello használandó `%%configure` magic tooconfigure hello notebook toouse egy külső csomagot. Külső csomagok használata jegyzetfüzetek, győződjön meg arról, meghívja a hello `%%configure` magic az első kódcella hello. Ez biztosítja, hogy hello kernel konfigurált toouse hello csomag hello munkamenet indítása előtt.

    >[!IMPORTANT] 
    >Ha elfelejti tooconfigure hello kernel hello első cellába, használhatja a hello `%%configure` a hello `-f` paraméter, de hello munkamenet újraindul, és az összes folyamatban lévő elvesznek.

    | HDInsight-verzió | Parancs |
    |-------------------|---------|
    |A HDInsight 3.3-as és a HDInsight 3.4 | `%%configure` <br>`{ "packages":["com.databricks:spark-csv_2.10:1.4.0"] }`|
    | A HDInsight 3.5 | `%%configure`<br>`{ "conf": {"spark.jars.packages": "com.databricks:spark-csv_2.10:1.4.0" }}`|

6. a fenti hello részlet hello maven koordináták hello külső package Maven központi tárházban vár. Ezt a kódrészletet a `com.databricks:spark-csv_2.10:1.4.0` hello maven koordináta a van **spark-fürt megosztott kötetei szolgáltatás** csomag. Itt látható, hogyan hozható létre csomag hello koordinátáit.
   
    a. Keresse meg hello csomag hello Maven-tárházat. Ebben az oktatóanyagban használjuk [spark-fürt megosztott kötetei szolgáltatás](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).
   
    b. Adattárból hello gyűjtse össze a hello értékeinek **GroupId**, **artifactid szakaszát**, és **verzió**. Győződjön meg arról, hogy gyűjtse össze a hello értékek egyeznek-e a fürtön. Ebben az esetben a Scala 2.10 és Spark 1.4.0 csomag használjuk, de szükséges tooselect különböző verziói hello megfelelő Scala vagy Spark-verziót a fürtben. Talál hello Scala verzió a fürtön futó `scala.util.Properties.versionString` hello Spark Jupyter kernel vagy Spark elküldése. Talál hello Spark verzió a fürtön futó `sc.version` használt Jupyter notebookokban.
   
    ![Külső csomagok használata Jupyter notebook](./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/use-external-packages-with-jupyter.png "külső csomagok használata Jupyter notebook")
   
    c. Összefűzésére hello három értékek, egymástól kettősponttal (**:**).
   
        com.databricks:spark-csv_2.10:1.4.0

7. Futtassa a hello kódcella hello `%%configure` magic. Ezzel konfigurálja a hello alapul szolgáló Livy munkamenet toouse hello csomag megadott. A hello későbbi levő hello jegyzetfüzet, most már használhatja hello csomag alább látható módon.
   
        val df = sqlContext.read.format("com.databricks.spark.csv").
        option("header", "true").
        option("inferSchema", "true").
        load("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

8. Ezt követően futtathatja hello kódtöredékek, például a lent látható módon tooview hello hello dataframe adatait hello előző lépésben létrehozott.
   
        df.show()
   
        df.select("Time").count()

## <a name="seealso"></a>Lásd még:
* [Overview: Apache Spark on Azure HDInsight (Áttekintés: Apache Spark on Azure HDInsight)](hdinsight-apache-spark-overview.md)

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

* [Külső python-csomagok használata Jupyter notebookok Apache Spark-fürtök HDInsight Linux rendszeren](hdinsight-apache-spark-python-package-installation.md)
* [Toocreate IntelliJ IDEA HDInsight-eszközei beépülő használja, és küldje el a Spark Scala-alkalmazások](hdinsight-apache-spark-intellij-tool-plugin.md)
* [IntelliJ IDEA toodebug Spark-alkalmazások HDInsight-eszközei beépülő távolról használni](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Zeppelin notebookok használata Spark-fürttel HDInsighton](hdinsight-apache-spark-zeppelin-notebook.md)
* [Jupyter notebookokhoz elérhető kernelek a HDInsight Spark-fürtjében](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Jupyter telepítse a számítógépre, és csatlakozzon a HDInsight Spark-fürt tooan](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Erőforrások kezelése
* [Az Azure HDInsight hello Apache Spark-fürt erőforrásainak kezelése](hdinsight-apache-spark-resource-manager.md)
* [Apache Spark-fürtön futó feladatok nyomon követése és hibakeresése a HDInsightban](hdinsight-apache-spark-job-debugging.md)

