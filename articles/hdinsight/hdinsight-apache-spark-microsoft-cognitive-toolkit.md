---
title: "az Azure HDInsight Spark kognitív eszközkészlet aaaMicrosoft mély biztonságával |} Microsoft Docs"
description: "Ismerje meg, hogyan képzett Microsoft kognitív eszközkészletre mély tanulási modell alkalmazott tooa dataset hello Python API Spark on Azure HDInsight Spark-fürtben lévő használatával lehet."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: nitinme
ms.openlocfilehash: c296d4697f16d4ef6a958fdb55289807d745ea40
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-microsoft-cognitive-toolkit-deep-learning-model-with-azure-hdinsight-spark-cluster"></a>Microsoft kognitív eszközkészlet mély tanulása Azure HDInsight Spark-fürt használatára

Ebben a cikkben szereplő lépéseket követve hello.

1. Egy egyéni parancsfájl tooinstall Microsoft kognitív eszközkészlet futtasson egy Azure HDInsight Spark-fürt.

2. Töltse fel a Jupyter notebook toohello Spark-fürt toosee hogyan tooapply képzett Microsoft kognitív eszközkészletre mély tanulási modell toofiles egy Azure Blob Storage-fiók hello segítségével a [Spark Python API (PySpark)](https://spark.apache.org/docs/0.9.0/python-programming-guide.html)

## <a name="prerequisites"></a>Előfeltételek

* **Azure-előfizetés**. Az oktatóanyag elindításához Azure-előfizetéssel kell rendelkeznie. Lásd: [Ingyenes Azure-fiók létrehozása még ma](https://azure.microsoft.com/free).

* **Az Azure HDInsight Spark-fürt**. Ez a cikk a Spark 2.0 fürt létrehozása. Útmutatásért lásd: [Azure HDInsight létrehozása Apache Spark-fürt](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="how-does-this-solution-flow"></a>Hogyan nem flow ehhez a megoldáshoz?

Ebben a megoldásban ez a cikk és ebben az oktatóanyagban részeként feltöltött Jupyter notebook között oszlik meg. A cikkben végezze el a lépéseket követve hello:

* Futtasson egy parancsfájl műveletet egy HDInsight Spark-fürt tooinstall Microsoft kognitív eszközkészlet és Python-csomagokat.
* Hello Jupyter notebook hello megoldás toohello HDInsight Spark-fürt futtató feltöltése.

hello következő fennmaradó lépéseit lásd: hello Jupyter notebook.

- Egy Spark Resiliant elosztott adatkészlet vagy RDD minta képek betöltése
   - Modulok be, és a készletek meghatározása
   - Töltse le a hello adatkészletre helyileg a hello Spark-fürt
   - Egy RDD hello adatkészlethez konvertálása
- Pontszám hello képek kognitív eszközkészlet betanított modell segítségével
   - Töltse le a betanítása hello kognitív eszközkészlet modell toohello Spark-fürt
   - Adja meg a munkavégző csomópontokhoz által használt funkciók toobe
   - A feldolgozó csomópontok pontszám hello lemezképek
   - Értékelje ki a modell pontosságát


## <a name="install-microsoft-cognitive-toolkit"></a>Microsoft kognitív eszközkészlet telepítése

Telepítheti a Microsoft kognitív eszközkészlet parancsfájlművelet használata Spark-fürtön. Parancsfájlművelet egyéni parancsfájlok tooinstall összetevőket, amelyek nincsenek alapértelmezés szerint elérhető hello fürtön használja. Hello hello Azure portál, egyéni parancsfájlt használhatja a HDInsight .NET SDK használatával, vagy az Azure PowerShell használatával. Hello parancsfájl tooinstall hello eszközkészlet részeként a fürt létrehozása, vagy miután hello fürt megfelelően működik, és vagy is használható. 

Ebben a cikkben használjuk hello portál tooinstall hello eszközkészlet hello fürt létrehozása után. Más módokon toorun hello egyéni parancsfájl, lásd: [testreszabása HDInsight-fürtök használata parancsfájlművelet](hdinsight-hadoop-customize-cluster-linux.md).

### <a name="using-hello-azure-portal"></a>Hello Azure portál használatával

Hogyan toouse hello Azure Portal toorun parancsfájl-e a művelet, lásd: [testreszabása HDInsight-fürtök használata parancsfájlművelet](hdinsight-hadoop-customize-cluster-linux.md#use-a-script-action-during-cluster-creation). Ellenőrizze, hogy a következő bemenet tooinstall kognitív eszközkészlet Microsoft hello megadnia.

* Adjon meg egy értéket hello parancsfájlművelet nevét.

* A **Bash parancsfájlok URI**, adja meg `https://raw.githubusercontent.com/Azure-Samples/hdinsight-pyspark-cntk-integration/master/cntk-install.sh`.

* Ellenőrizze, hogy futtatja hello parancsfájl csak hello head és feldolgozó csomópontokat, és törölje az összes hello más jelölőnégyzeteket.

* Kattintson a **Create** (Létrehozás) gombra.

## <a name="upload-hello-jupyter-notebook-tooazure-hdinsight-spark-cluster"></a>Töltse fel a hello Jupyter notebook tooAzure HDInsight Spark-fürt

Microsoft kognitív eszközkészlet toouse hello hello Azure HDInsight Spark-fürt, be kell tölteni a hello Jupyter notebook **CNTK_model_scoring_on_Spark_walkthrough.ipynb** toohello Azure HDInsight Spark-fürt. A notebook érhető el a Githubon: [https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration](https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration).

1. Klónozás hello GitHub-tárházban [https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration](https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration). További utasítások tooclone: [egy tárház klónozása](https://help.github.com/articles/cloning-a-repository/).

2. Az Azure portál hello, nyissa meg a hello Spark-fürt panelén, amelyre már kiépített, **fürt irányítópult**, és kattintson a **Jupyter notebook**.

    Is elindíthatja a hello Jupyter notebook toohello URL-cím megnyitásával `https://<clustername>.azurehdinsight.net/jupyter/`. Cserélje le \<clustername > hello nevet, a HDInsight-fürthöz.

3. A hello Jupyter notebook, kattintson az **feltöltése** a hello jobb felső sarokban, és navigáljon a toohello helyre, ahol klónozott hello GitHub-tárházban.

    ![Töltse fel a Jupyter notebook tooAzure HDInsight Spark-fürt](./media/hdinsight-apache-spark-microsoft-cognitive-toolkit/hdinsight-microsoft-cognitive-toolkit-load-jupyter-notebook.png "feltöltése Jupyter notebook tooAzure HDInsight Spark-fürt")

4. Kattintson a **feltöltése** újra.

5. Után hello notebook töltheti fel, hello notebook neve hello kattintson, és majd utasítások hello hello jegyzetfüzet maga tooload hello adatkészlet és a hello az oktatóanyag végrehajtása.

## <a name="see-also"></a>Lásd még:
* [Overview: Apache Spark on Azure HDInsight (Áttekintés: Apache Spark on Azure HDInsight)](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Forgatókönyvek
* [Spark és BI: Interaktív adatelemzés végrehajtása a Spark on HDInsight használatával, BI-eszközökkel](hdinsight-apache-spark-use-bi-tools.md)
* [Spark és Machine Learning: A Spark on HDInsight használata az épület-hőmérséklet elemzésére HVAC-adatok alapján](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark és Machine Learning: használja a Spark on HDInsight toopredict élelmiszervizsgálati eredmények](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Spark Streaming: A Spark on HDInsight használata valós idejű streamelési alkalmazások összeállítására](hdinsight-apache-spark-eventhub-streaming.md)
* [A webhelynapló elemzése a Spark on HDInsight használatával](hdinsight-apache-spark-custom-library-website-log-analysis.md)
* [Az Application Insights telemetriai adatainak elemzése a Spark on HDInsight használatával](hdinsight-spark-analyze-application-insight-logs.md)

### <a name="create-and-run-applications"></a>Alkalmazások létrehozása és futtatása
* [Önálló alkalmazás létrehozása a Scala használatával](hdinsight-apache-spark-create-standalone-application.md)
* [Feladatok távoli futtatása Spark-fürtön a Livy használatával](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Eszközök és bővítmények
* [Toocreate IntelliJ IDEA HDInsight-eszközei beépülő használja, és küldje el a Spark Scala-alkalmazások](hdinsight-apache-spark-intellij-tool-plugin.md)
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
[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md
