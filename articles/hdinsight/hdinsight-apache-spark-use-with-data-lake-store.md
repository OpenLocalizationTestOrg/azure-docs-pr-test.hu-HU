---
title: az Apache Spark on tooanalyze adatokat az Azure Data Lake Store aaaUse |} Microsoft Docs
description: "A Spark-feladatok futtatása az Azure Data Lake Store-ban tárolt tooanalyze adatok"
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 1f174323-c17b-428c-903d-04f0e272784c
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: nitinme
ms.openlocfilehash: 3b7f628f7a8114d2ca6f3f9219ce107905f1c818
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hdinsight-spark-cluster-tooanalyze-data-in-data-lake-store"></a>HDInsight Spark-fürt tooanalyze adatok használata a Data Lake Store-ban

Ebben az oktatóanyagban használata Jupyter notebook elérhető HDInsight Spark-fürtök toorun egy feladatot, amely egy Data Lake Store-fiók olvassa be az adatokat.

## <a name="prerequisites"></a>Előfeltételek

* Azure Data Lake Store-fiók. Hajtsa végre a hello található utasítások segítségével: [Ismerkedés az Azure Data Lake Store használatának hello Azure Portal](../data-lake-store/data-lake-store-get-started-portal.md).

* Az Azure HDInsight Spark-fürt a Data Lake Store tárolóként Hajtsa végre a hello található utasítások segítségével: [HDInsight-fürtök létrehozása az Azure portál használatával a Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).

    
## <a name="prepare-hello-data"></a>Hello adatok előkészítése

> [!NOTE]
> Nem kell tooperform ebben a lépésben Ha hello HDInsight-fürt hozott létre a Data Lake Store alapértelmezett tárolóként. hello fürt csoportlétrehozási folyamatait néhány mintaadatokkal hello fürt létrehozásakor megadott hello Data Lake Store-fiókban. Hagyja ki a szakaszt toohello [használata a HDInsight Spark-fürt a Data Lake Store](#use-an-hdinsight-spark-cluster-with-data-lake-store).
>
>

HDInsight-fürtök Data Lake Store további tárhely és az Azure Storage-Blobba alapértelmezett tárolóként hozta létre, ha először másolja át néhány minta adatok toohello Data Lake Store-fiók. Hello HDInsight-fürthöz társított Azure Storage-Blobba hello hello minta adatait is használhatja. Használhatja a hello [ADLCopy eszköz](http://aka.ms/downloadadlcopy) toodo így. Töltse le, és telepítse hello eszköz hello hivatkozásra.

1. Nyisson meg egy parancssort, és keresse meg a toohello rendszert tartalmazó könyvtár AdlCopy, általában `%HOMEPATH%\Documents\adlcopy`.

2. Futtassa a következő parancs toocopy hello egy adott blob hello forrás tároló tooa Data Lake Store:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>

    Másolás hello **HVAC.csv** minta adatfájl **/HdiSamples/HdiSamples/SensorSampleData/hvac/** toohello Azure Data Lake Store-fiók. hello kódrészletet hasonlóan kell kinéznie:

        AdlCopy /Source https://mydatastore.blob.core.windows.net/mysparkcluster/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv /dest swebhdfs://mydatalakestore.azuredatalakestore.net/hvac/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

   > [!WARNING]
   > Győződjön meg arról, hogy hello fájl, és útvonalneveket hello megfelelő esetben.
   >
   >
3. Fogja felszólító tooenter hello hitelesítő adatai hello, amely alatt a Data Lake Store-fiók rendelkezik Azure-előfizetés. Egy kimeneti hasonló toohello következő jelenik meg:

        Initializing Copy.
        Copy Started.
        100% data copied.
        Copy Completed. 1 file copied.

    hello adatfájl (**HVAC.csv**) egy mappában kerülnek **/hvac** a hello Data Lake Store-fiók.

## <a name="use-an-hdinsight-spark-cluster-with-data-lake-store"></a>A Data Lake Store HDInsight Spark-fürt használatára

1. A hello [Azure Portal](https://portal.azure.com/), hello kezdőpulton, kattintson a Spark-fürt hello csempére (ha toohello kezdőpulton rögzítette azt). Tooyour fürt alapján is megtalálhatja **összes tallózása** > **a HDInsight-fürtök**.

2. Hello Spark-fürt panelén kattintson **Gyorshivatkozások**, és majd a hello **fürt irányítópult** panelen kattintson a **Jupyter Notebook**. Ha a rendszer kéri, adja meg hello fürt hello rendszergazdai hitelesítő adataival.

   > [!NOTE]
   > A fürt URL-címet a böngészőben a következő megnyitásakor hello által is hello Jupyter Notebook lehet elérni. Cserélje le **CLUSTERNAME** hello néven a fürt:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >

3. Hozzon létre új notebookot. Kattintson a **New** (Új), majd a **PySpark** elemre.

    ![Új Jupyter notebook létrehozása](./media/hdinsight-apache-spark-use-with-data-lake-store/hdinsight-create-jupyter-notebook.png "Új Jupyter notebook létrehozása")

4. Mivel a notebook PySpark kernelt hello hozott létre, nem kell toocreate semmilyen tartalmat explicit módon. hello Spark és Hive-környezetek automatikusan létrehozza az Ön hello első kódcella futtatásakor. Ehhez a forgatókönyvhöz szükséges hello típusok importálásával megkezdése. toodo Igen, illessze be a következő kódrészletet a cellába, majd nyomja le az hello **SHIFT + ENTER**.

        from pyspark.sql.types import *

    Minden alkalommal, amikor a Jupyter futtat egy feladatot, a webböngésző ablakának címsorában megjelenik egy **(foglalt)** állapot hello notebook neve mellett. Ezenkívül megjelenik egy teli kör következő toohello **PySpark** hello jobb felső sarokban lévő szöveg. Hello feladat befejezése után ez tooa jel üres körre változik.

     ![A Jupyter notebook feladat állapota](./media/hdinsight-apache-spark-use-with-data-lake-store/hdinsight-jupyter-job-status.png "A Jupyter notebook feladat állapota")

5. Mintaadatokat tölthet be egy ideiglenes táblába hello segítségével **HVAC.csv** toohello Data Lake Store-fiók másolt fájl. Hello adatok hello Data Lake Store-fiók URL-minta a következő hello segítségével érheti el.

    * Ha a Data Lake Store alapértelmezett tárolóként, HVAC.csv hello elérési hasonló toohello URL-cím a következő lesz:

            adl://<data_lake_store_name>.azuredatalakestore.net/<cluster_root>/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv

        Vagy egy rövidített formátumban, például hello következő is használhatja:

            adl:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv

    * Ha Data Lake Store további tárolóként, HVAC.csv lesz hello helyét, ahová másolta, például:

            adl://<data_lake_store_name>.azuredatalakestore.net/<path_to_file>

     Cserélje le egy üres cellába, az alábbi kódpéldát, Beillesztés hello **MYDATALAKESTORE** a Data Lake Store-fiók nevét, és nyomja le az **SHIFT + ENTER**. Ez a Kódpélda nevű ideiglenes táblába regisztrálja a hello adatok **hvac**.

            # Load hello data. hello path below assumes Data Lake Store is default storage for hello Spark cluster
            hvacText = sc.textFile("adl://MYDATALAKESTORE.azuredatalakestore.net/cluster/mysparkcluster/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

            # Create hello schema
            hvacSchema = StructType([StructField("date", StringType(), False),StructField("time", StringType(), False),StructField("targettemp", IntegerType(), False),StructField("actualtemp", IntegerType(), False),StructField("buildingID", StringType(), False)])

            # Parse hello data in hvacText
            hvac = hvacText.map(lambda s: s.split(",")).filter(lambda s: s[0] != "Date").map(lambda s:(str(s[0]), str(s[1]), int(s[2]), int(s[3]), str(s[6]) ))

            # Create a data frame
            hvacdf = sqlContext.createDataFrame(hvac,hvacSchema)

            # Register hello data fram as a table toorun queries against
            hvacdf.registerTempTable("hvac")

6. Mivel PySpark kernelt használ, akkor most közvetlenül futtathat SQL-lekérdezést hello ideiglenes táblán **hvac** imént létrehozott hello segítségével `%%sql` magic. További információ a hello `%%sql` magic, valamint hello PySpark kernellel elérhető egyéb magics lásd: [Spark HDInsight-fürtökkel használt Jupyter notebookokban elérhető kernelek](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).

        %%sql
        SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"

7. Ha hello feladat sikeresen befejeződött, a következő táblázatos kimenet hello alapértelmezés szerint megjelenik.

      ![A lekérdezési eredmény táblázati kimenete](./media/hdinsight-apache-spark-use-with-data-lake-store/jupyter-tabular-output.png "A lekérdezési eredmény táblázati kimenete")

     Hello eredményeket egyéb megjelenítési formákban is megtekintheti. Például tartozó területgrafikon hello azonos kimenethez hello következő jelenne meg.

     ![A lekérdezési eredmény területgrafikonja](./media/hdinsight-apache-spark-use-with-data-lake-store/jupyter-area-output.png "A lekérdezési eredmény területgrafikonja")

8. Miután befejezte a hello alkalmazást futtat, akkor leállítási hello notebook toorelease hello erőforrásokat. toodo Igen, a hello **fájl** hello notebook menüjében kattintson **zárja be és Halt**. Ezzel leállítja és Bezárás hello notebookot.


## <a name="next-steps"></a>Következő lépések

* [Hozzon létre egy önálló Scala alkalmazás toorun az Apache Spark-fürt](hdinsight-apache-spark-create-standalone-application.md)
* [Az IntelliJ toocreate Spark-alkalmazások HDInsight Spark Linux-fürt eszköztára Azure HDInsight eszközök használata](hdinsight-apache-spark-intellij-tool-plugin.md)
* [HDInsight-eszközök használata az Eclipse toocreate Spark-alkalmazások HDInsight Spark Linux-fürt Azure eszköztára](hdinsight-apache-spark-eclipse-tool-plugin.md)
