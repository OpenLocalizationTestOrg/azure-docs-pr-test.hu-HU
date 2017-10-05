---
title: "Művelet – telepítése Python-csomagokat, az Azure hdinsight Jupyter parancsfájl |} Microsoft Docs"
description: "Lépésenkénti útmutató parancsfájlművelet használatára konfigurálhatja a Jupyter notebookok érhető el a HDInsight Spark-fürtjei külső python-csomagok használata."
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
ms.openlocfilehash: 20cf384c96d4ff4eaf064c8880ad128d521fb9bf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-script-action-to-install-external-python-packages-for-jupyter-notebooks-in-apache-spark-clusters-on-hdinsight"></a>Parancsfájlművelet használata Jupyter notebookok külső Python-csomagokat telepíteni az Apache Spark on hdinsight-fürtök
> [!div class="op_single_selector"]
> * [Cella magic használatával](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
> * [Parancsfájl művelet segítségével](hdinsight-apache-spark-python-package-installation.md)
>
>

Útmutató Apache Spark-fürt konfigurálása a HDInsight (Linux) használatához a külső, közösségi része volt a Parancsfájlműveletek segítségével **python** csomagok, amelyek nem tartalmazza a fürt, a-kész.

> [!NOTE]
> Jupyter notebook használatával is konfigurálhatja `%%configure` külső csomagok használata a Bűvös. Útmutatásért lásd: [külső csomagok használata Jupyter notebookok Apache Spark-fürt a HDInsight](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md).
> 
> 

Kereshet az [csomagindexet](https://pypi.python.org/pypi) csomagok rendelkezésre álló teljes listáját. Más forrásból is megkapható elérhető csomagok listáját. Például telepíthet keresztül elérhető csomagok [Anaconda](https://docs.continuum.io/anaconda/pkg-docs) vagy [conda-forge](https://conda-forge.github.io/feedstocks.html).

Ebből a cikkből megtanulhatja, hogyan telepítheti a [TensorFlow](https://www.tensorflow.org/) csomag parancsfájl Actoin használatával a fürtön, és a Jupyter notebook keresztül használni.

## <a name="prerequisites"></a>Előfeltételek
Az alábbiakkal kell rendelkeznie:

* Azure-előfizetés. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* A HDInsight az Apache Spark-fürt. Útmutatásért lásd: [létrehozása az Apache Spark on Azure hdinsight clusters](hdinsight-apache-spark-jupyter-spark-sql.md).

   > [!NOTE]
   > Ha még nem rendelkezik egy Spark-fürt HDInsight Linux rendszeren, Parancsfájlműveletek futtathatja a fürt létrehozása során. Látogasson el a dokumentációt a [egyéni parancsfájl-műveletek használata](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux).
   > 
   > 

## <a name="use-external-packages-with-jupyter-notebooks"></a>Külső csomagok használata Jupyter notebookokkal

1. Az [Azure portál](https://portal.azure.com/) kezdőpultján kattintson a Spark-fürthöz tartozó csempére (ha rögzítette azt a kezdőpulton). A fürtöt a következő helyről is megkeresheti: **Browse All (Összes tallózása)** > **HDInsight Clusters** (HDInsight-fürtök).   

2. A Spark-fürt panelén kattintson **Parancsfájlműveletek** alatt **használati**. Az egyéni művelet, amely telepíti TensorFlow az átjárócsomópontokkal és a feldolgozó csomópontok. A bash parancsfájlok lehet hivatkozni: https://hdiconfigactions.blob.core.windows.net/linuxtensorflow/tensorflowinstall.sh látogassa meg a dokumentáció a [egyéni parancsfájl-műveletek használata](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux).

   > [!NOTE]
   > Nincsenek két python telepítések a fürtben. Spark fogja használni a Anaconda python telepítési helye: `/usr/bin/anaconda/bin`. Hivatkozás a telepítést, az egyéni műveletek keresztül a `/usr/bin/anaconda/bin/pip` és `/usr/bin/anaconda/bin/conda`.
   > 
   > 

3. Nyissa meg a PySpark Jupyter notebook

    ![Új Jupyter notebook létrehozása](./media/hdinsight-apache-spark-python-package-installation/hdinsight-spark-create-notebook.png "Új Jupyter notebook létrehozása")

4. Az új notebook létrejött, és Untitled.pynb néven nyílt meg. A felső részen kattintson a notebook nevére, és adjon meg egy könnyen megjegyezhető nevet.

    ![Adjon nevet a notebooknak](./media/hdinsight-apache-spark-python-package-installation/hdinsight-spark-name-notebook.png "Adjon nevet a notebooknak")

5. Most fog `import tensorflow` , és futtassa a hello world példa. 

    Kód másolása:

        import tensorflow as tf
        hello = tf.constant('Hello, TensorFlow!')
        sess = tf.Session()
        print(sess.run(hello))

    Az eredmény fog kinézni:
    
    ![TensorFlow kód végrehajtása](./media/hdinsight-apache-spark-python-package-installation/execution.png "hajtható végre TensorFlow kódot")



## <a name="seealso"></a>Lásd még:
* [Overview: Apache Spark on Azure HDInsight (Áttekintés: Apache Spark on Azure HDInsight)](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Forgatókönyvek
* [Spark és BI: Interaktív adatelemzés végrehajtása a Spark on HDInsight használatával, BI-eszközökkel](hdinsight-apache-spark-use-bi-tools.md)
* [Spark és Machine Learning: A Spark on HDInsight használata az épület-hőmérséklet elemzésére HVAC-adatok alapján](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark és Machine Learning: A Spark on HDInsight használata az élelmiszervizsgálati eredmények előrejelzésére](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Spark Streaming: A Spark on HDInsight használata valós idejű streamelési alkalmazások összeállítására](hdinsight-apache-spark-eventhub-streaming.md)
* [A webhelynapló elemzése a Spark on HDInsight használatával](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Alkalmazások létrehozása és futtatása
* [Önálló alkalmazás létrehozása a Scala használatával](hdinsight-apache-spark-create-standalone-application.md)
* [Feladatok távoli futtatása Spark-fürtön a Livy használatával](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Eszközök és bővítmények
* [Külső csomagok használata Jupyter notebookok Apache Spark-fürt a HDInsight](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Az IntelliJ IDEA HDInsight-eszközei beépülő moduljának használata Spark Scala-alkalmazások létrehozásához és elküldéséhez](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Az IntelliJ IDEA HDInsight-eszközei beépülő moduljának használata Spark-alkalmazások távoli hibaelhárításához](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Zeppelin notebookok használata Spark-fürttel HDInsighton](hdinsight-apache-spark-zeppelin-notebook.md)
* [Jupyter notebookokhoz elérhető kernelek a HDInsight Spark-fürtjében](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [A Jupyter telepítése a számítógépre, majd csatlakozás egy HDInsight Spark-fürthöz](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Erőforrások kezelése
* [Apache Spark-fürt erőforrásainak kezelése az Azure HDInsightban](hdinsight-apache-spark-resource-manager.md)
* [Apache Spark-fürtön futó feladatok nyomon követése és hibakeresése a HDInsightban](hdinsight-apache-spark-job-debugging.md)
