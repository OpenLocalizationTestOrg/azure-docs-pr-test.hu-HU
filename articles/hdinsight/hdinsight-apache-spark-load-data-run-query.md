---
title: "egy Azure HDInsight Spark-fürt aaaRun interaktív lekérdezések |} Microsoft Docs"
description: "Hogyan toocreate az Apache Spark on hdinsight fürt a HDInsight Spark gyorsindítási."
keywords: "spark gyorsútmutató,interaktív spark,interaktív lekérdezés,hdinsight spark,azure spark"
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: nitinme
ms.openlocfilehash: 3864eba50eb3828a9ecb657ded88080e1974585f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="run-interactive-queries-on-an-hdinsight-spark-cluster"></a>Interaktív lekérdezések futtatására egy HDInsight Spark-fürt

Ebben a cikkben a Jupyter notebook toorun interaktív Spark SQL lekérdezések Spark-fürt használatára. Jupyter notebook egy webböngésző-alapú alkalmazás, amely kiterjeszti a hello interaktivitási élményének Konzolalapú toohello webes. További információkért lásd: [hello Jupyter notebook](http://jupyter-notebook.readthedocs.io/en/latest/notebook.html).

Ebben az oktatóanyagban hello használata **PySpark** kernel a hello Jupyter notebook toorun egy interaktív Spark SQL-lekérdezésben. A HDInsight-fürtökön Jupyter notebookok is támogatja a két más kernelek - **PySpark3** és **Spark**. További információ a hello kernelek és hello előnyei a **PySpark**, lásd: [használata Jupyter notebook kernelek az Apache Spark hdinsight-fürtök](hdinsight-apache-spark-jupyter-notebook-kernels.md).

## <a name="prerequisites"></a>Előfeltételek

* **Egy Azure HDInsight Spark-fürt**. Útmutatásért lásd: [Apache Spark-fürt létrehozása az Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="create-a-jupyter-notebook-toorun-interactive-queries"></a>Jupyter notebook toorun interaktív lekérdezések létrehozása

toorun lekérdezések használjuk a mintaadatok, akkor hello-fürthöz tartozó hello Storage alapértelmezés szerint. Azonban meg kell először adott adatok betöltése az Spark, a dataframe. Miután hello dataframe, lekérdezéseket is futtathat a hello Jupyter notebook használatával. Ebben a szakaszban, olvassa el:

* A Spark dataframe minta adatkészlet regisztrálásához.
* Hello dataframe kapcsolatos lekérdezések futtatása.

1. Nyissa meg hello [Azure-portálon](https://portal.azure.com/). Ha toopin hello fürt toohello irányítópult választotta, kattintson a hello fürt csempe hello irányítópult toolaunch hello fürt paneljén.

    Ha nem volt rögzíti hello fürt toohello irányítópult, hello bal oldali ablaktáblában kattintson **a HDInsight-fürtök**, majd kattintson a létrehozott hello fürt.

3. A **Gyorshivatkozások** menüben kattintson a **Fürt irányítópultjai** lehetőségre, majd a **Jupyter Notebook** elemre. Ha a rendszer kéri, adja meg hello fürt hello rendszergazdai hitelesítő adataival.

   ![Nyissa meg Jupyter notebook toorun interaktív Spark SQL-lekérdezés](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-start-jupyter-interactive-spark-sql-query.png "nyitott Jupyter notebook toorun interaktív Spark SQL-lekérdezés")

   > [!NOTE]
   > A fürt URL-címet a böngészőben a következő megnyitásakor hello által hello Jupyter notebook is elérhetők. Cserélje le **CLUSTERNAME** hello néven a fürt:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >
3. Hozzon létre egy notebookot. Kattintson a **New** (Új), majd a **PySpark** elemre.

   ![A Jupyter notebook toorun interaktív Spark SQL-lekérdezés létrehozása](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-create-jupyter-interactive-Spark-SQL-query.png "a Jupyter notebook toorun interaktív Spark SQL-lekérdezés létrehozása")

   Új notebook létrejött, és hello nevű Untitled(Untitled.pynb).

4. Hello notebook neve hello tetején kattintson, és adja meg egy rövid nevet, ha szeretné.

    ![Adjon meg egy nevet hello Jupter notebook toorun interaktív Spark-lekérdezést](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-jupyter-notebook-name.png "adjon meg egy nevet hello Jupter notebook toorun interaktív Spark lekérdezése")

5. Beillesztés hello következő kód egy üres cellába, és nyomja le az **SHIFT + ENTER** toorun hello kódot. hello kód importálja az ehhez a forgatókönyvhöz szükséges hello típusok:

        from pyspark.sql.types import *

    Mivel a notebook PySpark kernelt hello hozott létre, nem kell toocreate semmilyen tartalmat explicit módon. hello Spark és Hive-környezetek automatikusan létrejönnek hello első kódcella futtatásakor.

    ![Az interaktív Spark SQL-lekérdezés állapota](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-interactive-spark-query-status.png "Az interaktív Spark SQL-lekérdezés állapota")

    Minden alkalommal, amikor az interaktív lekérdezések futtatása a Jupyter, a webböngésző ablakának címsorában látható egy **(foglalt)** állapot hello notebook neve mellett. Egy teli kör következő toohello is látni **PySpark** hello jobb felső sarokban lévő szöveg. Hello feladat befejezése után tooa jel üres körre változik.

6. Mielőtt hello adatok betölthető Spark-fürt, tudassa velünk keresse meg a pillanatképe. hello ebben az oktatóanyagban használt mintaadatok érhető el az összes HDInsight Spark-fürtök a CSV-fájlként **\HdiSamples\HdiSamples\SensorSampleData\hvac\hvac.csv**. hello adatok hello hőmérséklet változata épület rögzíti. Az alábbiakban hello hello adatok első néhány sor.

    ![Az adatok interaktív Spark SQL-lekérdezés pillanatkép](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-sample-data-interactive-spark-sql-query.png "pillanatkép adatok interaktív Spark SQL-lekérdezés")

6. Hozzon létre egy dataframe és egy ideiglenes tábla (**hvac**) a következő kód hello futtatásával. Ebben az oktatóanyagban nem létrehozni minden hello oszlopok hello ideiglenes tábla hello CSV nyersadatok összehasonlított toohello oszlopként. 

        # Create an RDD from sample data
        hvacText = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

        # Create a schema for our data
        Entry = Row('Date', 'Time', 'TargetTemp', 'ActualTemp', 'BuildingID')

        # Parse hello data and create a schema
        hvacParts = hvacText.map(lambda s: s.split(',')).filter(lambda s: s[0] != 'Date')
        hvac = hvacParts.map(lambda p: Entry(str(p[0]), str(p[1]), int(p[2]), int(p[3]), int(p[6])))
        
        # Infer hello schema and create a table       
        hvacTable = sqlContext.createDataFrame(hvac)
        hvacTable.registerTempTable('hvactemptable')
        dfw = DataFrameWriter(hvacTable)
        dfw.saveAsTable('hvac')

7. Hello tábla létrehozása után interaktív lekérdezés futtatása hello adatokat, használja a következő kód hello.

        %%sql
        SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"

   Mivel PySpark kernelt használ, akkor most közvetlenül futtathat SQL interaktív lekérdezés hello ideiglenes táblán **hvac** hello segítségével létrehozott `%%sql` magic. Hello kapcsolatos további információk `%%sql` magic és hello PySpark kernellel elérhető egyéb magics [Spark HDInsight-fürtökkel használt Jupyter notebookokban elérhető kernelek](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).

   a következő táblázatos kimenet hello alapértelmezés szerint megjelenik.

     ![Az interaktív Spark-lekérdezési eredmény táblázati kimenete](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result.png "Az interaktív Spark-lekérdezési eredmény táblázati kimenete")

    Hello eredményeket egyéb megjelenítési formákban is megtekintheti. Például tartozó területgrafikon hello azonos kimenethez hello következő jelenne meg.

    ![Az interaktív Spark-lekérdezési eredmény területgrafikonja](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result-area-chart.png "Az interaktív Spark-lekérdezési eredmény területgrafikonja")

9. Hello notebook toorelease hello fürterőforrások leállítása hello alkalmazást futtató befejezése után. toodo Igen, a hello **fájl** hello notebook menüjében kattintson **zárja be és Halt**.

## <a name="next-step"></a>Következő lépés

Az e cikkben megtanulta, hogyan meg interaktív lekérdezések toorun Spark Jupyter notebook használatával. Előzetes toohello hogyan kell húzni Spark regisztrált hello adatok tovább cikk toosee olyan BI analytics eszközt, például a Power bi-ban és a Tableau. 

> [!div class="nextstepaction"]
>[Adatok képi megjelenítés eszközökkel rendelkező Azure HDInsight Spark BI](hdinsight-apache-spark-use-bi-tools.md)




