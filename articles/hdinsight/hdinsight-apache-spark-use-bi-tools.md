---
title: "Adatok képi megjelenítés eszközökkel on Azure HDInsight Spark BI |} Microsoft Docs"
description: "Adatok képi megjelenítés eszközök segítségével analytics Apache Spark BI használata a HDInsight-fürtökön"
keywords: "Apache spark üzleti intelligencia, spark bi, spark adatábrázolási, spark üzleti intelligencia"
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 1448b536-9bc8-46bc-bbc6-d7001623642a
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: nitinme
ms.openlocfilehash: 49dd161049ac442081fe6d26cf8bd3a56a2e0687
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="apache-spark-bi-using-data-visualization-tools-with-azure-hdinsight"></a><span data-ttu-id="6ba67-104">Apache Spark BI adatok képi megjelenítés eszközökkel Azure hdinsightban</span><span class="sxs-lookup"><span data-stu-id="6ba67-104">Apache Spark BI using data visualization tools with Azure HDInsight</span></span>

<span data-ttu-id="6ba67-105">Útmutató adatok képi megjelenítés eszközöket, például a Power BI és a Tableau használatával elemezheti a nyers minta adatkészletet, Apache Spark BI használata a HDInsight-fürtökön.</span><span class="sxs-lookup"><span data-stu-id="6ba67-105">Learn how to use data visualization tools such as Power BI and Tableau to analyze a raw sample data set using Apache Spark BI on HDInsight clusters.</span></span>

> [!NOTE]
> <span data-ttu-id="6ba67-106">Ebben a cikkben leírt Üzletiintelligencia-eszközök kapcsolattal nem támogatott Spark 2.1 a Azure HDInsight 3.6 Preview-ban.</span><span class="sxs-lookup"><span data-stu-id="6ba67-106">Connectivity with BI tools described in this article is not supported on Spark 2.1 on Azure HDInsight 3.6 Preview.</span></span> <span data-ttu-id="6ba67-107">Csak a Spark verziók 1.6-os és 2.0 (HDInsight 3.4, 3.5 rendre) támogatottak.</span><span class="sxs-lookup"><span data-stu-id="6ba67-107">Only Spark versions 1.6 and 2.0 (HDInsight 3.4, 3.5 respectively) are supported.</span></span>
>

<span data-ttu-id="6ba67-108">Ez az oktatóanyag egy HDInsight Spark-fürt Jupyter notebook, is érhető el.</span><span class="sxs-lookup"><span data-stu-id="6ba67-108">This tutorial is also available as a Jupyter notebook on an HDInsight Spark cluster.</span></span> <span data-ttu-id="6ba67-109">A notebook élmény lehetővé teszi a Python kódtöredékek futtassa a notebook magát.</span><span class="sxs-lookup"><span data-stu-id="6ba67-109">The notebook experience lets you run the Python snippets from the notebook itself.</span></span> <span data-ttu-id="6ba67-110">Jegyzetfüzet belül az oktatóprogram elvégzéséhez Spark-fürt létrehozása, indítsa el a Jupyter notebook (`https://CLUSTERNAME.azurehdinsight.net/jupyter`), és futtassa a notebook **használata BI eszközök az Apache Spark on HDInsight.ipynb** alatt a **Python** mappa.</span><span class="sxs-lookup"><span data-stu-id="6ba67-110">To perform the tutorial from within a notebook, create a Spark cluster, launch a Jupyter notebook (`https://CLUSTERNAME.azurehdinsight.net/jupyter`), and then run the notebook **Use BI tools with Apache Spark on HDInsight.ipynb** under the **Python** folder.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6ba67-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="6ba67-111">Prerequisites</span></span>

* <span data-ttu-id="6ba67-112">A HDInsight az Apache Spark-fürt.</span><span class="sxs-lookup"><span data-stu-id="6ba67-112">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="6ba67-113">Útmutatásért lásd: [létrehozása az Apache Spark on Azure hdinsight clusters](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="6ba67-113">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>


## <span data-ttu-id="6ba67-114"><a name="hivetable"></a>Adatok előkészítése Spark adatábrázolási</span><span class="sxs-lookup"><span data-stu-id="6ba67-114"><a name="hivetable"></a>Prepare data for Spark data visualization</span></span>

<span data-ttu-id="6ba67-115">Ebben a szakaszban használjuk a [Jupyter](https://jupyter.org) egy HDInsight Spark-fürt, amely a nyers mintaadatok feldolgozni, és mentse egy tábla feladatok futtatása a notebook.</span><span class="sxs-lookup"><span data-stu-id="6ba67-115">In this section, we use the [Jupyter](https://jupyter.org) notebook from an HDInsight Spark cluster to run jobs that process your raw sample data and save it as a table.</span></span> <span data-ttu-id="6ba67-116">A mintaadatok egy CSV-fájlt (hvac.csv) elérhető alapértelmezés szerint minden fürtön.</span><span class="sxs-lookup"><span data-stu-id="6ba67-116">The sample data is a .csv file (hvac.csv) available on all clusters by default.</span></span> <span data-ttu-id="6ba67-117">Az adatok táblázatként mentése után a következő szakaszban használjuk Üzletiintelligencia-eszközök a tábla csatlakozhat, és hajtsa végre az adatmegjelenítéseket.</span><span class="sxs-lookup"><span data-stu-id="6ba67-117">Once your data is saved as a table, in the next section we use BI tools to connect to the table and perform data visualizations.</span></span>

> [!NOTE]
> <span data-ttu-id="6ba67-118">Ha hajtja végre a lépéseket a cikkben található utasítások végrehajtása után [interaktív lekérdezések futtatására egy HDInsight Spark-fürt](hdinsight-apache-spark-load-data-run-query.md), ugorjon az alábbi 8. lépés.</span><span class="sxs-lookup"><span data-stu-id="6ba67-118">If you are performing the steps in this article after completing the instructions in [Run interactive queries on an HDInsight Spark cluster](hdinsight-apache-spark-load-data-run-query.md), you can skip to Step 8 below.</span></span>
>

1. <span data-ttu-id="6ba67-119">Az [Azure portál](https://portal.azure.com/) kezdőpultján kattintson a Spark-fürthöz tartozó csempére (ha rögzítette azt a kezdőpulton).</span><span class="sxs-lookup"><span data-stu-id="6ba67-119">From the [Azure portal](https://portal.azure.com/), from the startboard, click the tile for your Spark cluster (if you pinned it to the startboard).</span></span> <span data-ttu-id="6ba67-120">A fürtöt a következő helyről is megkeresheti: **Browse All (Összes tallózása)** > **HDInsight Clusters** (HDInsight-fürtök).</span><span class="sxs-lookup"><span data-stu-id="6ba67-120">You can also navigate to your cluster under **Browse All** > **HDInsight Clusters**.</span></span>   

2. <span data-ttu-id="6ba67-121">A Spark-fürt panelén kattintson a **Fürt irányítópultja Dashboard** lehetőségre, majd a **Jupyter Notebook** elemre.</span><span class="sxs-lookup"><span data-stu-id="6ba67-121">From the Spark cluster blade, click **Cluster Dashboard**, and then click **Jupyter Notebook**.</span></span> <span data-ttu-id="6ba67-122">Ha a rendszer felkéri rá, adja meg a fürthöz tartozó rendszergazdai hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="6ba67-122">If prompted, enter the admin credentials for the cluster.</span></span>

   > [!NOTE]
   > <span data-ttu-id="6ba67-123">A fürthöz tartozó Jupyter notebookot az alábbi URL-cím böngészőben történő megnyitásával is elérheti.</span><span class="sxs-lookup"><span data-stu-id="6ba67-123">You may also reach the Jupyter Notebook for your cluster by opening the following URL in your browser.</span></span> <span data-ttu-id="6ba67-124">Cserélje le a **CLUSTERNAME** elemet a fürt nevére:</span><span class="sxs-lookup"><span data-stu-id="6ba67-124">Replace **CLUSTERNAME** with the name of your cluster:</span></span>
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >

3. <span data-ttu-id="6ba67-125">Hozzon létre egy notebookot.</span><span class="sxs-lookup"><span data-stu-id="6ba67-125">Create a notebook.</span></span> <span data-ttu-id="6ba67-126">Kattintson a **New** (Új), majd a **PySpark** elemre.</span><span class="sxs-lookup"><span data-stu-id="6ba67-126">Click **New**, and then click **PySpark**.</span></span>

    <span data-ttu-id="6ba67-127">![Jupyter notebook létrehozása az Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/create-jupyter-notebook-for-spark-bi.png "Apache Spark bi Jupyter notebook létrehozása")</span><span class="sxs-lookup"><span data-stu-id="6ba67-127">![Create a Jupyter notebook for Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/create-jupyter-notebook-for-spark-bi.png "Create a Jupyter notebook for Apache Spark BI")</span></span>

4. <span data-ttu-id="6ba67-128">Az új notebook létrejött, és Untitled.pynb néven nyílt meg.</span><span class="sxs-lookup"><span data-stu-id="6ba67-128">A new notebook is created and opened with the name Untitled.pynb.</span></span> <span data-ttu-id="6ba67-129">A felső részen kattintson a notebook nevére, és adjon meg egy könnyen megjegyezhető nevet.</span><span class="sxs-lookup"><span data-stu-id="6ba67-129">Click the notebook name at the top, and enter a friendly name.</span></span>

    <span data-ttu-id="6ba67-130">![Adjon meg egy nevet a notebook Apache Spark bi](./media/hdinsight-apache-spark-use-bi-tools/jupyter-notebook-name-for-spark-bi.png "adjon meg egy nevet a notebook Apache Spark BI")</span><span class="sxs-lookup"><span data-stu-id="6ba67-130">![Provide a name for the notebook for Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/jupyter-notebook-name-for-spark-bi.png "Provide a name for the notebook for Apache Spark BI")</span></span>

5. <span data-ttu-id="6ba67-131">Mivel a notebook PySpark kernel használatával jött létre, explicit módon semmilyen tartalmat nem kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="6ba67-131">Because you created a notebook using the PySpark kernel, you do not need to create any contexts explicitly.</span></span> <span data-ttu-id="6ba67-132">Az első kódcella futtatásakor a Spark- és Hive-környezetek automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="6ba67-132">The Spark and Hive contexts are automatically created for you when you run the first code cell.</span></span> <span data-ttu-id="6ba67-133">Ennek az első lépése a jelen forgatókönyvhöz szükséges típusok importálása.</span><span class="sxs-lookup"><span data-stu-id="6ba67-133">You can start by importing the types required for this scenario.</span></span> <span data-ttu-id="6ba67-134">Ehhez az szükséges, vigye a kurzort a cella, és nyomja le az **SHIFT + ENTER**.</span><span class="sxs-lookup"><span data-stu-id="6ba67-134">To do so, place the cursor in the cell and press **SHIFT + ENTER**.</span></span>

        from pyspark.sql import *

6. <span data-ttu-id="6ba67-135">Töltse be a mintaadatokat egy ideiglenes táblába.</span><span class="sxs-lookup"><span data-stu-id="6ba67-135">Load sample data into a temporary table.</span></span> <span data-ttu-id="6ba67-136">Spark-fürt HDInsightban történő létrehozásakor a **hvac.csv** mintaadatfájl a hozzá tartozó tárfiókba lesz átmásolva a következő helyre: **\HdiSamples\HdiSamples\SensorSampleData\hvac**.</span><span class="sxs-lookup"><span data-stu-id="6ba67-136">When you create a Spark cluster in HDInsight, the sample data file, **hvac.csv**, is copied to the associated storage account under **\HdiSamples\HdiSamples\SensorSampleData\hvac**.</span></span>

    <span data-ttu-id="6ba67-137">Egy üres cellába, illessze be a következő kódrészletet, és nyomja le az **SHIFT + ENTER**.</span><span class="sxs-lookup"><span data-stu-id="6ba67-137">In an empty cell, paste the following snippet and press **SHIFT + ENTER**.</span></span> <span data-ttu-id="6ba67-138">Ezt a kódrészletet regisztrálja az adatokat a következő táblába **hvac**.</span><span class="sxs-lookup"><span data-stu-id="6ba67-138">This snippet registers the data into a table called **hvac**.</span></span>

        # Create an RDD from sample data
        hvacText = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

        # Create a schema for our data
        Entry = Row('Date', 'Time', 'TargetTemp', 'ActualTemp', 'BuildingID')

        # Parse the data and create a schema
        hvacParts = hvacText.map(lambda s: s.split(',')).filter(lambda s: s[0] != 'Date')
        hvac = hvacParts.map(lambda p: Entry(str(p[0]), str(p[1]), int(p[2]), int(p[3]), int(p[6])))

        # Infer the schema and create a table       
        hvacTable = sqlContext.createDataFrame(hvac)
        hvacTable.registerTempTable('hvactemptable')
        dfw = DataFrameWriter(hvacTable)
        dfw.saveAsTable('hvac')

7. <span data-ttu-id="6ba67-139">Győződjön meg arról, hogy a tábla létrehozása sikeresen megtörtént.</span><span class="sxs-lookup"><span data-stu-id="6ba67-139">Verify that the table was successfully created.</span></span> <span data-ttu-id="6ba67-140">Használhatja a `%%sql` Hive futtatásához magic közvetlenül lekérdezi.</span><span class="sxs-lookup"><span data-stu-id="6ba67-140">You can use the `%%sql` magic to run Hive queries directly.</span></span> <span data-ttu-id="6ba67-141">A `%%sql` funkcióval, illetve a PySpark kernellel elérhető egyéb funkciókkal kapcsolatos további információkat [A Spark HDInsight-fürtökkel használt Jupyter notebookokban elérhető kernelek](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic) című részben talál.</span><span class="sxs-lookup"><span data-stu-id="6ba67-141">For more information about the `%%sql` magic, and other magics available with the PySpark kernel, see [Kernels available on Jupyter notebooks with Spark HDInsight clusters](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).</span></span>

        %%sql
        SHOW TABLES

    <span data-ttu-id="6ba67-142">Hasonló kimenetnek látja, lent látható módon:</span><span class="sxs-lookup"><span data-stu-id="6ba67-142">You see an output like shown below:</span></span>

        +---------------+-------------+
        |tableName      |isTemporary  |
        +---------------+-------------+
        |hvactemptable  |true        |
        |hivesampletable|false        |
        |hvac           |false        |
        +---------------+-------------+

    <span data-ttu-id="6ba67-143">Csak azokat a táblákat, amelyek a FALSE értékre a **isTemporary** oszlop, amely a metaadattárhoz tárolódnak, és elérhető az Üzletiintelligencia-eszközök hive táblák.</span><span class="sxs-lookup"><span data-stu-id="6ba67-143">Only the tables that have false under the **isTemporary** column are hive tables that are stored in the metastore and can be accessed from the BI tools.</span></span> <span data-ttu-id="6ba67-144">Ebben az oktatóanyagban nem csatlakozni a **hvac** létrehozott tábla.</span><span class="sxs-lookup"><span data-stu-id="6ba67-144">In this tutorial, we connect to the **hvac** table we created.</span></span>

8. <span data-ttu-id="6ba67-145">Győződjön meg arról, hogy a tábla tartalmazza-e a kívánt adatokat.</span><span class="sxs-lookup"><span data-stu-id="6ba67-145">Verify that the table contains the intended data.</span></span> <span data-ttu-id="6ba67-146">Egy üres cellába a notebook, másolja a következő kódrészletet, és nyomja le az **SHIFT + ENTER**.</span><span class="sxs-lookup"><span data-stu-id="6ba67-146">In an empty cell in the notebook, copy the following snippet and press **SHIFT + ENTER**.</span></span>

        %%sql
        SELECT * FROM hvac LIMIT 10

9. <span data-ttu-id="6ba67-147">Állítsa le a notebook az erőforrások kijelölése.</span><span class="sxs-lookup"><span data-stu-id="6ba67-147">Shut down the notebook to release the resources.</span></span> <span data-ttu-id="6ba67-148">Ehhez a notebook **File** (Fájl) menüjében kattintson a **Close and Halt** (Bezárás és leállítás) elemre.</span><span class="sxs-lookup"><span data-stu-id="6ba67-148">To do so, from the **File** menu on the notebook, click **Close and Halt**.</span></span>

## <span data-ttu-id="6ba67-149"><a name="powerbi"></a>A Power BI használata Spark adatábrázolási</span><span class="sxs-lookup"><span data-stu-id="6ba67-149"><a name="powerbi"></a>Use Power BI for Spark data visualization</span></span>

> [!NOTE]
> <span data-ttu-id="6ba67-150">Ez a szakasz esetén csak a HDInsight 3.4 Spark 1.6-os és Spark 2.0 a HDInsight 3.5 alkalmazható.</span><span class="sxs-lookup"><span data-stu-id="6ba67-150">This section is applicable only for Spark 1.6 on HDInsight 3.4 and Spark 2.0 on HDInsight 3.5.</span></span>
>
>

<span data-ttu-id="6ba67-151">A mentést követően az adatok táblázatként, a Power BI segítségével csatlakozzon a adataihoz, és jelenítheti meg, hogy hozzon létre a jelentések, irányítópultok, stb.</span><span class="sxs-lookup"><span data-stu-id="6ba67-151">Once you have saved the data as a table, you can use Power BI to connect to the data and visualize it to create reports, dashboards, etc.</span></span>

1. <span data-ttu-id="6ba67-152">Győződjön meg arról, hogy hozzáfér a Power bi-ban.</span><span class="sxs-lookup"><span data-stu-id="6ba67-152">Make sure you have access to Power BI.</span></span> <span data-ttu-id="6ba67-153">A Power bi-ban az ingyenes kiadásra előfizetői kaphat [http://www.powerbi.com/](http://www.powerbi.com/).</span><span class="sxs-lookup"><span data-stu-id="6ba67-153">You can get a free preview subscription of Power BI from [http://www.powerbi.com/](http://www.powerbi.com/).</span></span>

2. <span data-ttu-id="6ba67-154">Jelentkezzen be [a Power BI](http://www.powerbi.com/).</span><span class="sxs-lookup"><span data-stu-id="6ba67-154">Sign in to [Power BI](http://www.powerbi.com/).</span></span>

3. <span data-ttu-id="6ba67-155">A bal oldali ablaktábla alján kattintson **adatok beolvasása**.</span><span class="sxs-lookup"><span data-stu-id="6ba67-155">From the bottom of the left pane, click **Get Data**.</span></span>

4. <span data-ttu-id="6ba67-156">A a **adatok beolvasása** lap **importálása vagy adatok kapcsolódás**, a **adatbázisok**, kattintson a **beolvasása**.</span><span class="sxs-lookup"><span data-stu-id="6ba67-156">On the **Get Data** page, under **Import or Connect to Data**, for **Databases**, click **Get**.</span></span>

    <span data-ttu-id="6ba67-157">![Adatok beolvasása a Power bi-bA az Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-import-data-power-bi.png "adatok beszerzése Apache Spark BI Power BI-bA")</span><span class="sxs-lookup"><span data-stu-id="6ba67-157">![Get data into Power BI for Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-import-data-power-bi.png "Get data into Power BI for Apache Spark BI")</span></span>

5. <span data-ttu-id="6ba67-158">A következő képernyőn kattintson a **a Spark on Azure HDInsight** majd **Connect**.</span><span class="sxs-lookup"><span data-stu-id="6ba67-158">On the next screen, click **Spark on Azure HDInsight** and then click **Connect**.</span></span> <span data-ttu-id="6ba67-159">Amikor a rendszer kéri, adja meg a fürt URL-CÍMÉT (`mysparkcluster.azurehdinsight.net`) és a hitelesítő adatokat. csatlakozzon a fürthöz.</span><span class="sxs-lookup"><span data-stu-id="6ba67-159">When prompted, enter the cluster URL (`mysparkcluster.azurehdinsight.net`) and the credentials to connect to the cluster.</span></span>

    <span data-ttu-id="6ba67-160">![Kapcsolódás az Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/connect-to-apache-spark-bi.png "Apache Spark BI kapcsolódni")</span><span class="sxs-lookup"><span data-stu-id="6ba67-160">![Connect to Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/connect-to-apache-spark-bi.png "Connect to Apache Spark BI")</span></span>

    <span data-ttu-id="6ba67-161">A kapcsolat létrejötte után a Power BI elindítja adatok importálását a HDInsight Spark-fürt.</span><span class="sxs-lookup"><span data-stu-id="6ba67-161">After the connection is established, Power BI starts importing data from the Spark cluster on HDInsight.</span></span>

6. <span data-ttu-id="6ba67-162">A Power BI importálja az adatokat, és hozzáadja a **Spark** adatkészlet alapján a **adatkészletek** fejléc.</span><span class="sxs-lookup"><span data-stu-id="6ba67-162">Power BI imports the data and adds a **Spark** dataset under the **Datasets** heading.</span></span> <span data-ttu-id="6ba67-163">Kattintson az adatkészlet nyisson meg egy új munkalapra lesznek adatok megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="6ba67-163">Click the data set to open a new worksheet to visualize the data.</span></span> <span data-ttu-id="6ba67-164">A munkalap jelentést is mentheti.</span><span class="sxs-lookup"><span data-stu-id="6ba67-164">You can also save the worksheet as a report.</span></span> <span data-ttu-id="6ba67-165">Munkalap mentése a **fájl** menüben kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="6ba67-165">To save a worksheet, from the **File** menu, click **Save**.</span></span>

    <span data-ttu-id="6ba67-166">![A Power BI irányítópult Apache Spark BI csempéjén](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-tile-dashboard.png "Apache Spark BI csempét a Power BI-irányítópulton")</span><span class="sxs-lookup"><span data-stu-id="6ba67-166">![Apache Spark BI tile on Power BI dashboard](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-tile-dashboard.png "Apache Spark BI tile on Power BI dashboard")</span></span>
7. <span data-ttu-id="6ba67-167">Figyelje meg, hogy a **mezők** listáján a jobb oldali listák a **hvac** korábban létrehozott tábla.</span><span class="sxs-lookup"><span data-stu-id="6ba67-167">Notice that the **Fields** list on the right lists the **hvac** table you created earlier.</span></span> <span data-ttu-id="6ba67-168">Bontsa ki az alábbi táblázatban a mezőket a táblázatban, korábban definiált jegyzetfüzet.</span><span class="sxs-lookup"><span data-stu-id="6ba67-168">Expand the table to see the fields in the table, as you defined in notebook earlier.</span></span>

      <span data-ttu-id="6ba67-169">![Apache Spark BI irányítópulton lévő táblák listázása](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-display-tables.png "Apache Spark BI irányítópulton lévő táblák listázása")</span><span class="sxs-lookup"><span data-stu-id="6ba67-169">![List tables on Apache Spark BI dashboard](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-display-tables.png "List tables on Apache Spark BI dashboard")</span></span>

8. <span data-ttu-id="6ba67-170">Cél hőmérséklet és a tényleges hőmérséklet rendszerbeli minden közötti eltérés megjelenítendő képi megjelenítés létrehozása.</span><span class="sxs-lookup"><span data-stu-id="6ba67-170">Build a visualization to show the variance between target temperature and actual temperature for each building.</span></span> <span data-ttu-id="6ba67-171">Adato regisztrációhoz, jelölje be **területdiagram** (vörös téglalappal látható).</span><span class="sxs-lookup"><span data-stu-id="6ba67-171">To visualize yoru data, select **Area Chart** (shown in red box).</span></span> <span data-ttu-id="6ba67-172">A tengely fogd és vidd megadhatók a **BuildingID** eleménél **tengely**, és **ActualTemp**/**TargetTemp** a mezők **érték**.</span><span class="sxs-lookup"><span data-stu-id="6ba67-172">To define the axis, drag-and-drop the **BuildingID** field under **Axis**, and **ActualTemp**/**TargetTemp** fields under **Value**.</span></span>

    <span data-ttu-id="6ba67-173">![Hozzon létre a Spark Apache Spark BI használatával adatmegjelenítésekkel](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-add-value-columns.png "Spark létrehozása az adatmegjelenítéseket Apache Spark BI használatával")</span><span class="sxs-lookup"><span data-stu-id="6ba67-173">![Create Spark data visualizations using Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-add-value-columns.png "Create Spark data visualizations using Apache Spark BI")</span></span>

9. <span data-ttu-id="6ba67-174">Alapértelmezés szerint a képi megjelenítés jeleníti meg az összegük **ActualTemp** és **TargetTemp**.</span><span class="sxs-lookup"><span data-stu-id="6ba67-174">By default the visualization shows the sum for **ActualTemp** and **TargetTemp**.</span></span> <span data-ttu-id="6ba67-175">Mindkét a mezőket, a legördülő menüben válassza a **átlagos** le tényleges átlagosan és a cél hőmérsékletek mindkét épületekhez.</span><span class="sxs-lookup"><span data-stu-id="6ba67-175">For both the fields, from the drop-down, select **Average** to get an average of actual and target temperatures for both buildings.</span></span>

    <span data-ttu-id="6ba67-176">![Hozzon létre a Spark Apache Spark BI használatával adatmegjelenítésekkel](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-average-of-values.png "Spark létrehozása az adatmegjelenítéseket Apache Spark BI használatával")</span><span class="sxs-lookup"><span data-stu-id="6ba67-176">![Create Spark data visualizations using Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-average-of-values.png "Create Spark data visualizations using Apache Spark BI")</span></span>

10. <span data-ttu-id="6ba67-177">Az adatok vizuális kell lennie ezen a képernyőfelvételen hasonló.</span><span class="sxs-lookup"><span data-stu-id="6ba67-177">Your data visualization should be similar to the one in the screenshot.</span></span> <span data-ttu-id="6ba67-178">Helyezze a kurzort a képi megjelenítés eszköztipp vonatkozó adatok eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="6ba67-178">Move your cursor over the visualization to get tool tips with relevant data.</span></span>

    <span data-ttu-id="6ba67-179">![Hozzon létre a Spark Apache Spark BI használatával adatmegjelenítésekkel](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-area-graph.png "Spark létrehozása az adatmegjelenítéseket Apache Spark BI használatával")</span><span class="sxs-lookup"><span data-stu-id="6ba67-179">![Create Spark data visualizations using Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-area-graph.png "Create Spark data visualizations using Apache Spark BI")</span></span>

11. <span data-ttu-id="6ba67-180">Kattintson a **mentése** a felső menüben, és adja meg a jelentés neve.</span><span class="sxs-lookup"><span data-stu-id="6ba67-180">Click **Save** from the top menu and provide a report name.</span></span> <span data-ttu-id="6ba67-181">A Látványelem is rögzítheti.</span><span class="sxs-lookup"><span data-stu-id="6ba67-181">You can also pin the visual.</span></span> <span data-ttu-id="6ba67-182">A képi megjelenítés rögzíti, amikor tárolása az irányítópulton, a legutóbbi értékét egy pillanat alatt követheti nyomon.</span><span class="sxs-lookup"><span data-stu-id="6ba67-182">When you pin a visualization, it is stored on your dashboard so you can track the latest value at a glance.</span></span>

   <span data-ttu-id="6ba67-183">Az ugyanahhoz az adatkészlethez tartozó és az adatok pillanatképe az irányítópulton rögzítheti őket annyi képi megjelenítések adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="6ba67-183">You can add as many visualizations as you want for the same dataset and pin them to the dashboard for a snapshot of your data.</span></span> <span data-ttu-id="6ba67-184">Emellett a HDInsight Spark-fürtök csatlakoznak a Power BI közvetlen kapcsolódás.</span><span class="sxs-lookup"><span data-stu-id="6ba67-184">Also, Spark clusters on HDInsight are connected to Power BI with direct connect.</span></span> <span data-ttu-id="6ba67-185">Ez biztosítja, hogy Power BI mindig rendelkezik a lehető legfrissebb adatokat a fürtről, így Önnek nem kell ütemezhetők be frissítések az adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="6ba67-185">This ensures that Power BI always has the most up-to-date data from your cluster so you do not need to schedule refreshes for the dataset.</span></span>

## <span data-ttu-id="6ba67-186"><a name="tableau"></a>A Spark adatábrázolási Tableau asztal használata</span><span class="sxs-lookup"><span data-stu-id="6ba67-186"><a name="tableau"></a>Use Tableau Desktop for Spark data visualization</span></span>

> [!NOTE]
> <span data-ttu-id="6ba67-187">Ez a szakasz esetén csak az Azure HDInsight létrehozott Spark 1.5.2-fürtök alkalmazható.</span><span class="sxs-lookup"><span data-stu-id="6ba67-187">This section is applicable only for Spark 1.5.2 clusters created in Azure HDInsight.</span></span>
>
>

1. <span data-ttu-id="6ba67-188">Telepítés [Tableau asztali](http://www.tableau.com/products/desktop) az Apache Spark BI oktatóanyag futtató számítógépen.</span><span class="sxs-lookup"><span data-stu-id="6ba67-188">Install [Tableau Desktop](http://www.tableau.com/products/desktop) on the computer where you are running this Apache Spark BI tutorial.</span></span>

2. <span data-ttu-id="6ba67-189">Győződjön meg arról, hogy számítógépen is van telepítve a Microsoft Spark ODBC-illesztőprogram.</span><span class="sxs-lookup"><span data-stu-id="6ba67-189">Make sure that computer also has Microsoft Spark ODBC driver installed.</span></span> <span data-ttu-id="6ba67-190">Az illesztőprogram telepítése [Itt](http://go.microsoft.com/fwlink/?LinkId=616229).</span><span class="sxs-lookup"><span data-stu-id="6ba67-190">You can install the driver from [here](http://go.microsoft.com/fwlink/?LinkId=616229).</span></span>

1. <span data-ttu-id="6ba67-191">Tableau asztal indításakor.</span><span class="sxs-lookup"><span data-stu-id="6ba67-191">Launch Tableau Desktop.</span></span> <span data-ttu-id="6ba67-192">Kattintson a bal oldali panelen való kapcsolódásra, közül **Spark SQL**.</span><span class="sxs-lookup"><span data-stu-id="6ba67-192">In the left pane, from the list of server to connect to, click **Spark SQL**.</span></span> <span data-ttu-id="6ba67-193">Ha a bal oldali ablaktáblán alapértelmezés szerint nem szerepel a Spark SQL, megtalálhatja azt kattintson **több kiszolgálók**.</span><span class="sxs-lookup"><span data-stu-id="6ba67-193">If Spark SQL is not listed by default in the left pane, you can find it by click **More Servers**.</span></span>
2. <span data-ttu-id="6ba67-194">A Spark SQL kapcsolati párbeszédpanelen adja meg az értékeket, a képernyőfelvételen látható módon, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="6ba67-194">In the Spark SQL connection dialog box, provide the values as shown in the screenshot, and then click **OK**.</span></span>

    <span data-ttu-id="6ba67-195">![Csatlakozás az Apache Spark BI fürthöz](./media/hdinsight-apache-spark-use-bi-tools/connect-to-tableau-apache-spark-bi.png "csatlakozás az Apache Spark BI fürthöz")</span><span class="sxs-lookup"><span data-stu-id="6ba67-195">![Connect to a cluster for Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/connect-to-tableau-apache-spark-bi.png "Connect to a cluster for Apache Spark BI")</span></span>

    <span data-ttu-id="6ba67-196">A hitelesítési legördülő listákból **Microsoft Azure HDInsight szolgáltatás** lehetőség csak akkor, ha telepítette a [Microsoft Spark ODBC-illesztőprogram](http://go.microsoft.com/fwlink/?LinkId=616229) azon a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="6ba67-196">The authentication drop-down lists **Microsoft Azure HDInsight Service** as an option, only if you installed the [Microsoft Spark ODBC Driver](http://go.microsoft.com/fwlink/?LinkId=616229) on the computer.</span></span>
3. <span data-ttu-id="6ba67-197">A következő képernyőn a a **séma** legördülő menüben kattintson a **található** ikonra, végül **alapértelmezett**.</span><span class="sxs-lookup"><span data-stu-id="6ba67-197">On the next screen, from the **Schema** drop-down, click the **Find** icon, and then click **default**.</span></span>

    <span data-ttu-id="6ba67-198">![Található séma a következő Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/tableau-find-schema-apache-spark-bi.png "Apache Spark BI keresés sémája")</span><span class="sxs-lookup"><span data-stu-id="6ba67-198">![Find schema for Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/tableau-find-schema-apache-spark-bi.png "Find schema for Apache Spark BI")</span></span>
4. <span data-ttu-id="6ba67-199">Az a **tábla** , majd kattintson a **található** ikonra kattintva ismét elérhető a fürt összes Hive táblák listázása.</span><span class="sxs-lookup"><span data-stu-id="6ba67-199">For the **Table** field, click the **Find** icon again to list all the Hive tables available in the cluster.</span></span> <span data-ttu-id="6ba67-200">Megjelenik a **hvac** létrehozott korábban a notebook tábla.</span><span class="sxs-lookup"><span data-stu-id="6ba67-200">You should see the **hvac** table you created earlier using the notebook.</span></span>

    <span data-ttu-id="6ba67-201">![Az Apache Spark BI tábla található](./media/hdinsight-apache-spark-use-bi-tools/tableau-find-table-apache-spark-bi.png "Apache Spark bi keresési tábla")</span><span class="sxs-lookup"><span data-stu-id="6ba67-201">![Find table for Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/tableau-find-table-apache-spark-bi.png "Find table for Apache Spark BI")</span></span>
5. <span data-ttu-id="6ba67-202">Áthúzással a tábla a jobb felső mezőbe.</span><span class="sxs-lookup"><span data-stu-id="6ba67-202">Drag and drop the table to the top box on the right.</span></span> <span data-ttu-id="6ba67-203">Tableau importálja az adatokat, és a séma példának a vörös téglalappal alapján jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="6ba67-203">Tableau imports the data and displays the schema as highlighted by the red box.</span></span>

    <span data-ttu-id="6ba67-204">![Vegyen fel táblák a Tableau Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/tableau-add-table-apache-spark-bi.png "Tableau Apache Spark bi táblák hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="6ba67-204">![Add tables to Tableau for Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/tableau-add-table-apache-spark-bi.png "Add tables to Tableau for Apache Spark BI")</span></span>
6. <span data-ttu-id="6ba67-205">Kattintson a **Munka1** lap bal alsó.</span><span class="sxs-lookup"><span data-stu-id="6ba67-205">Click the **Sheet1** tab at the bottom left.</span></span> <span data-ttu-id="6ba67-206">Ellenőrizze, hogy a átlagos cél és a tényleges összes épület-hőmérsékletek jelennek meg. minden egyes dátum képi megjelenítés.</span><span class="sxs-lookup"><span data-stu-id="6ba67-206">Make a visualization that shows the average target and actual temperatures for all buildings for each date.</span></span> <span data-ttu-id="6ba67-207">Húzza **dátum** és **azonosító létrehozása** való **oszlopok** és **tényleges Temp**/**céloz Temp** való **sorok**.</span><span class="sxs-lookup"><span data-stu-id="6ba67-207">Drag **Date** and **Building ID** to **Columns** and **Actual Temp**/**Target Temp** to **Rows**.</span></span> <span data-ttu-id="6ba67-208">A **jelek**, jelölje be **terület** Spark adatábrázolási interaktív terület használandó.</span><span class="sxs-lookup"><span data-stu-id="6ba67-208">Under **Marks**, select **Area** to use an area map for Spark data visualization.</span></span>

     <span data-ttu-id="6ba67-209">![Adja hozzá a mezőket a Spark adatábrázolási](./media/hdinsight-apache-spark-use-bi-tools/spark-data-visualization-add-fields.png "Spark adatábrázolási mezők hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="6ba67-209">![Add fields for Spark data visualization](./media/hdinsight-apache-spark-use-bi-tools/spark-data-visualization-add-fields.png "Add fields for Spark data visualization")</span></span>
7. <span data-ttu-id="6ba67-210">Alapértelmezés szerint a hőmérséklet mezők láthatók szerint összesítést.</span><span class="sxs-lookup"><span data-stu-id="6ba67-210">By default, the temperature fields are shown as aggregate.</span></span> <span data-ttu-id="6ba67-211">Ha az átlaghőmérséklet inkább megjeleníteni kívánt, akkor megteheti a legördülő listán, a alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="6ba67-211">If you want to show the average temperatures instead, you can do so from the drop-down, as shown below.</span></span>

    <span data-ttu-id="6ba67-212">![Átlagos Spark adatok vizuális megjelenítéshez tartozó hőmérséklet érvénybe](./media/hdinsight-apache-spark-use-bi-tools/spark-data-visualization-average-temperature.png "átlagos Spark adatok vizuális megjelenítéshez tartozó hőmérséklet igénybe")</span><span class="sxs-lookup"><span data-stu-id="6ba67-212">![Take average of temperature for Spark data visualization](./media/hdinsight-apache-spark-use-bi-tools/spark-data-visualization-average-temperature.png "Take average of temperature for Spark data visualization")</span></span>

8. <span data-ttu-id="6ba67-213">Akkor is super-adhat egy hőmérséklet-leképezést keresztül, a másik cél- és a tényleges hőmérsékletek közötti különbség jobb arculatának eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="6ba67-213">You can also super-impose one temperature map over the other to get a better feel of difference between target and actual temperatures.</span></span> <span data-ttu-id="6ba67-214">Mozgassa az egeret a alacsonyabb terület térkép sarkába, keretein belül a leíró alakzat kiemelt piros kör látható.</span><span class="sxs-lookup"><span data-stu-id="6ba67-214">Move the mouse to the corner of the lower area map till you see the handle shape highlighted in a red circle.</span></span> <span data-ttu-id="6ba67-215">A térkép húzza a többi leképezési felső, és az egérrel kiadási, amikor megjelenik a kijelölt piros téglalap alakú.</span><span class="sxs-lookup"><span data-stu-id="6ba67-215">Drag the map to the other map on the top and release the mouse when you see the shape highlighted in red rectangle.</span></span>

    <span data-ttu-id="6ba67-216">![A Spark adatábrázolási maps egyesítése](./media/hdinsight-apache-spark-use-bi-tools/spark-data-visualization-merge-maps.png "egyesítési leképezi a Spark adatábrázolási")</span><span class="sxs-lookup"><span data-stu-id="6ba67-216">![Merge maps for Spark data visualization](./media/hdinsight-apache-spark-use-bi-tools/spark-data-visualization-merge-maps.png "Merge maps for Spark data visualization")</span></span>

     <span data-ttu-id="6ba67-217">Az adatok vizuális kell módosítani a képernyőfelvételen látható módon:</span><span class="sxs-lookup"><span data-stu-id="6ba67-217">Your data visualization should change as shown in the screenshot:</span></span>

    <span data-ttu-id="6ba67-218">![A Spark adatábrázolási tableau kimeneti](./media/hdinsight-apache-spark-use-bi-tools/spark-data-visualization-tableau-output.png "Spark adatábrázolási Tableau kimenete")</span><span class="sxs-lookup"><span data-stu-id="6ba67-218">![Tableau output for Spark data visualization](./media/hdinsight-apache-spark-use-bi-tools/spark-data-visualization-tableau-output.png "Tableau output for Spark data visualization")</span></span>
9. <span data-ttu-id="6ba67-219">Kattintson a **mentése** a munkalap mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="6ba67-219">Click **Save** to save the worksheet.</span></span> <span data-ttu-id="6ba67-220">Irányítópultok létrehozása, és egy vagy több felvétele.</span><span class="sxs-lookup"><span data-stu-id="6ba67-220">You can create dashboards and add one or more sheets to it.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6ba67-221">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6ba67-221">Next steps</span></span>

<span data-ttu-id="6ba67-222">Amennyiben megtudta, hogyan hozzon létre egy fürtöt, hozzon létre a Spark adatkeretek lekérdezni adatokat, és majd érheti el, hogy az adatok Üzletiintelligencia-eszközök.</span><span class="sxs-lookup"><span data-stu-id="6ba67-222">So far you learned how to create a cluster, create Spark data frames to query data, and then access that data from BI tools.</span></span> <span data-ttu-id="6ba67-223">Most már megtekintheti kezelheti a fürt erőforrásait, és egy HDInsight Spark-fürtben futó összes feladatot hibakeresési utasításokat.</span><span class="sxs-lookup"><span data-stu-id="6ba67-223">You can now look at instructions on how to manage the cluster resources and debug jobs that are running in an HDInsight Spark cluster.</span></span>

* [<span data-ttu-id="6ba67-224">Apache Spark-fürt erőforrásainak kezelése az Azure HDInsightban</span><span class="sxs-lookup"><span data-stu-id="6ba67-224">Manage resources for the Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="6ba67-225">Apache Spark-fürtön futó feladatok nyomon követése és hibakeresése a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="6ba67-225">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)

