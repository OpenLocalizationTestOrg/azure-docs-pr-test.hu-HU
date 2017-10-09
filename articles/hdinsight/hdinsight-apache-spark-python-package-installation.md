---
title: "aaaScript művelet – telepítése Python-csomagokat, az Azure hdinsight Jupyter |} Microsoft Docs"
description: "Lépésenként hogyan toouse parancsfájl művelet tooconfigure Jupyter notebookok érhető el a HDInsight Spark-fürtök toouse külső python-csomagokat."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 21978b71-eb53-480b-a3d1-c5d428a7eb5b
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: nitinme
ms.openlocfilehash: 54db79e67995dee7ca00abff979f7d74ae5ab9cd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-script-action-tooinstall-external-python-packages-for-jupyter-notebooks-in-apache-spark-clusters-on-hdinsight"></a>Parancsfájlművelet tooinstall külső Python-csomagok használata Jupyter notebookok Apache Spark-fürt a HDInsight
> [!div class="op_single_selector"]
> * [Cella magic használatával](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
> * [Parancsfájl művelet segítségével](hdinsight-apache-spark-python-package-installation.md)
>
>

Ismerje meg, hogyan toouse Parancsfájlműveletek tooconfigure Apache Spark-fürt a HDInsight (Linux) toouse külső, közösségi-hozzájárult **python** csomagok, amelyek nem szerepelnek hello fürt out-of-az-box.

> [!NOTE]
> Jupyter notebook használatával is konfigurálhatja `%%configure` magic toouse külső csomagok. Útmutatásért lásd: [külső csomagok használata Jupyter notebookok Apache Spark-fürt a HDInsight](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md).
> 
> 

Hello kereshet [csomagindexet](https://pypi.python.org/pypi) hello csomagok rendelkezésre álló teljes listáját. Más forrásból is megkapható elérhető csomagok listáját. Például telepíthet keresztül elérhető csomagok [Anaconda](https://docs.continuum.io/anaconda/pkg-docs) vagy [conda-forge](https://conda-forge.github.io/feedstocks.html).

Ebből a cikkből megtudhatja, hogyan tooinstall hello [TensorFlow](https://www.tensorflow.org/) csomag parancsfájl Actoin használatával a fürtön, és keresztül hello Jupyter notebook használni.

## <a name="prerequisites"></a>Előfeltételek
Hello következő kell rendelkeznie:

* Azure-előfizetés. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* A HDInsight az Apache Spark-fürt. Útmutatásért lásd: [létrehozása az Apache Spark on Azure hdinsight clusters](hdinsight-apache-spark-jupyter-spark-sql.md).

   > [!NOTE]
   > Ha még nem rendelkezik egy Spark-fürt HDInsight Linux rendszeren, Parancsfájlműveletek futtathatja a fürt létrehozása során. Keresse fel a hello dokumentáció [hogyan toouse egyéni parancsfájl-műveletek](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux).
   > 
   > 

## <a name="use-external-packages-with-jupyter-notebooks"></a>Külső csomagok használata Jupyter notebookokkal

1. A hello [Azure Portal](https://portal.azure.com/), hello kezdőpulton, kattintson a Spark-fürt hello csempére (ha toohello kezdőpulton rögzítette azt). Tooyour fürt alapján is megtalálhatja **összes tallózása** > **a HDInsight-fürtök**.   

2. Hello Spark-fürt panelén kattintson **Parancsfájlműveletek** alatt **használati**. Futtassa a következő egyéni művelet hello telepítésére TensorFlow hello átjárócsomópontokkal és hello munkavégző csomópontokhoz. hello bash parancsfájlok lehet hivatkozni: https://hdiconfigactions.blob.core.windows.net/linuxtensorflow/tensorflowinstall.sh látogasson el hello dokumentációja [hogyan toouse egyéni parancsfájl-műveletek](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux).

   > [!NOTE]
   > Nincsenek két python hello fürt telepítése. Spark fogja használni a hello Anaconda python telepítési helye: `/usr/bin/anaconda/bin`. Hivatkozás a telepítést, az egyéni műveletek keresztül a `/usr/bin/anaconda/bin/pip` és `/usr/bin/anaconda/bin/conda`.
   > 
   > 

3. Nyissa meg a PySpark Jupyter notebook

    ![Új Jupyter notebook létrehozása](./media/hdinsight-apache-spark-python-package-installation/hdinsight-spark-create-notebook.png "Új Jupyter notebook létrehozása")

4. Új notebook létrejött, és Untitled.pynb hello nevű. Hello notebook neve hello tetején kattintson, és adjon meg egy rövid nevet.

    ![Adjon meg egy nevet hello notebook](./media/hdinsight-apache-spark-python-package-installation/hdinsight-spark-name-notebook.png "adjon meg egy nevet hello notebook")

5. Most fog `import tensorflow` , és futtassa a hello world példa. 

    Kód toocopy:

        import tensorflow as tf
        hello = tf.constant('Hello, TensorFlow!')
        sess = tf.Session()
        print(sess.run(hello))

    hello eredményt fog kinézni:
    
    ![TensorFlow kód végrehajtása](./media/hdinsight-apache-spark-python-package-installation/execution.png "hajtható végre TensorFlow kódot")



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
* [Külső csomagok használata Jupyter notebookok Apache Spark-fürt a HDInsight](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Toocreate IntelliJ IDEA HDInsight-eszközei beépülő használja, és küldje el a Spark Scala-alkalmazások](hdinsight-apache-spark-intellij-tool-plugin.md)
* [IntelliJ IDEA toodebug Spark-alkalmazások HDInsight-eszközei beépülő távolról használni](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Zeppelin notebookok használata Spark-fürttel HDInsighton](hdinsight-apache-spark-zeppelin-notebook.md)
* [Jupyter notebookokhoz elérhető kernelek a HDInsight Spark-fürtjében](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Jupyter telepítse a számítógépre, és csatlakozzon a HDInsight Spark-fürt tooan](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Erőforrások kezelése
* [Az Azure HDInsight hello Apache Spark-fürt erőforrásainak kezelése](hdinsight-apache-spark-resource-manager.md)
* [Apache Spark-fürtön futó feladatok nyomon követése és hibakeresése a HDInsightban](hdinsight-apache-spark-job-debugging.md)
