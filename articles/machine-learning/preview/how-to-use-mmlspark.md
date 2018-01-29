---
title: "MMLSpark Machine Learning használatával |} Microsoft Docs"
description: "Útmutató az Azure Machine Learning segítségével MMLSpark szalagtár használata."
services: machine-learning
author: rastala
ms.author: roastala
manager: jhubbard
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 01/12/2018
ms.openlocfilehash: f978805f800a35908629a6febb59d7db50d14023
ms.sourcegitcommit: e19f6a1709b0fe0f898386118fbef858d430e19d
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/13/2018
---
# <a name="how-to-use-microsoft-machine-learning-library-for-apache-spark"></a>Microsoft Machine Learning-könyvtárral az Apache Spark használata

## <a name="introduction"></a>Bevezetés

[Microsoft Machine Learning könyvtár az Apache Spark](https://github.com/Azure/mmlspark) (MMLSpark) biztosítja az eszközöket, amelyek lehetővé teszik, hogy könnyedén létre méretezhető machine learning modellek nagy adatkészletek esetében. Ez magában foglalja az SparkML adatcsatornák az integráció a [Microsoft kognitív eszközkészlet ](https://github.com/Microsoft/CNTK) és [OpenCV](http://www.opencv.org/), amely lehetővé teszi, hogy: 
 * Bemenő és előre folyamatok képének adatai
 * Featurize képek és szövegek előre képzett mély tanulási modellek használata
 * Betanítása és implicit featurization használatával besorolási és regressziós modell pontozása.

## <a name="prerequisites"></a>Előfeltételek

Ez az útmutató Útmutató lépéseit, kell:
- [Telepítse az Azure Machine Learning-munkaterület](quickstart-installation.md)
- [Azure HDInsight Spark-fürt beállítása](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-jupyter-spark-sql)

## <a name="run-your-experiment-in-docker-container"></a>Futtassa a kísérletet a Docker-tároló

Az Azure Machine Learning-munkaterület MMLSpark használandó futtatásakor kísérletek Docker-tároló, a helyi vagy távoli virtuális gépre van konfigurálva. Ez a funkció lehetővé teszi, hogy egyszerűen hibakeresését és a PySpark modellek kísérletezhet méretű fürt futtatásához. 

Például az első lépéseiben, hozzon létre egy új projektet, és válassza az example "MMLSpark a felnőtt nyilvántartásba" gyűjteménye. Jelölje ki a számítási környezet a projekt irányítópultján "Docker", és válassza a "Futtatás". Az Azure Machine Learning-munkaterület létrehozza a Docker-tároló a PySpark program futtatásához, és végrehajtja a program.

Miután a Futtatás befejeződött, az eredmények megtekinthetők az Azure Machine Learning-munkaterület futtatási előzmények megtekintése.

## <a name="install-mmlspark-on-azure-hdinsight-spark-cluster"></a>Az Azure HDInsight Spark-fürt MMLSpark telepíthető.

Ez és a következő lépés elvégzéséhez szüksége első [hozzon létre egy Azure HDInsight Spark-fürt](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-jupyter-spark-sql).

Alapértelmezés szerint Azure Machine Learning-munkaterület telepíti MMLSpark a fürtön való futtatásakor a kísérlet során. Ez a viselkedés szabályozhatja és más külső csomagok telepítése nevű fájl szerkesztésével _aml_config/spark_dependencies.yml_ a projekt mappában.

```
# Spark configuration properties.
configuration:
  "spark.app.name": "Azure ML Experiment"
  "spark.yarn.maxAppAttempts": 1

repositories:
  - "https://mmlspark.azureedge.net/maven"
packages:
  - group: "com.microsoft.ml.spark"
    artifact: "mmlspark_2.11"
    version: "0.9.9"
```

Is telepíthető MMLSpark közvetlenül a HDInsight Spark fürt használt [parancsfájlművelet](https://github.com/Azure/mmlspark#hdinsight).

## <a name="set-up-azure-hdinsight-spark-cluster-as-compute-target"></a>A számítási céljaként Azure HDInsight Spark-fürt beállítása

Nyissa meg a parancssori ablakban az Azure Machine Learning-munkaterület a "File" menüben, majd kattintson a "a parancssor megnyitása"

CLI-ablakban futtassa a következő parancsokat:

```
az ml computetarget attach cluster --name <myhdi> --address <myhdi-ssh.azurehdinsight.net> --username <sshusername> --password <sshpwd> 
```

```
az ml experiment prepare -c <myhdi>
```

Most már a fürt érhető el a célként megadott a projekthez tartozó számítási.

## <a name="run-experiment-on-azure-hdinsight-spark-cluster"></a>Futtatási kísérlet az Azure HDInsight Spark-fürt.

Lépjen vissza a projekt irányítópultján "MMLSpark a felnőtt nyilvántartásba" példa. Válassza ki a fürtöt a számítási célként, és kattintson a Futtatás gombra.

Az Azure Machine Learning-munkaterület elküldi a spark feladatot a fürthöz. Az előrehaladást, és tekintse meg az eredményeket futtatási előzmények megtekintése.

## <a name="next-steps"></a>További lépések
További információ a MMLSpark könyvtár és példák: [MMLSpark GitHub-adattár](https://github.com/Azure/mmlspark)

*Apache®, Apache Spark és Spark® bejegyzett védjegye vagy védjegye az Amerikai Egyesült Államokban és/vagy más országokban Apache szoftver alapját.*
