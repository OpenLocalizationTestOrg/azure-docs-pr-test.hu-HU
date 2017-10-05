---
title: "Egy Azure HDInsight Spark-fürt interaktív lekérdezések futtatására |} Microsoft Docs"
description: "HDInsight Spark rövid útmutató az Apache Spark-fürtök HDInsightban történő létrehozásáról."
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
ms.openlocfilehash: ada1c3d1482c68834dbbf5eabbd045a7e0c01f9f
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/29/2017
---
# <a name="run-interactive-queries-on-an-hdinsight-spark-cluster"></a><span data-ttu-id="af7f0-104">Interaktív lekérdezések futtatására egy HDInsight Spark-fürt</span><span class="sxs-lookup"><span data-stu-id="af7f0-104">Run interactive queries on an HDInsight Spark cluster</span></span>

<span data-ttu-id="af7f0-105">Ebben a cikkben segítségével Jupyter notebook interaktív Spark SQL-lekérdezések futtatása Spark-fürt.</span><span class="sxs-lookup"><span data-stu-id="af7f0-105">In this article, you use a Jupyter notebook to run interactive Spark SQL queries against a Spark cluster.</span></span> <span data-ttu-id="af7f0-106">Jupyter notebook egy webböngésző-alapú alkalmazás, amely a konzol alapú interaktivitási élményének kibővíti az interneten.</span><span class="sxs-lookup"><span data-stu-id="af7f0-106">Jupyter notebook is a browser-based application that extends the console-based interactive experience to the Web.</span></span> <span data-ttu-id="af7f0-107">További információkért lásd: [a Jupyter notebook](http://jupyter-notebook.readthedocs.io/en/latest/notebook.html).</span><span class="sxs-lookup"><span data-stu-id="af7f0-107">For more information, see [The Jupyter notebook](http://jupyter-notebook.readthedocs.io/en/latest/notebook.html).</span></span>

<span data-ttu-id="af7f0-108">A jelen oktatóanyag esetében használja a **PySpark** interaktív Spark SQL-lekérdezés futtatása a Jupyter notebook a kernel.</span><span class="sxs-lookup"><span data-stu-id="af7f0-108">For this tutorial, you use the **PySpark** kernel in the Jupyter notebook to run an interactive Spark SQL query.</span></span> <span data-ttu-id="af7f0-109">A HDInsight-fürtökön Jupyter notebookok is támogatja a két más kernelek - **PySpark3** és **Spark**.</span><span class="sxs-lookup"><span data-stu-id="af7f0-109">Jupyter notebooks on HDInsight clusters also support two other kernels - **PySpark3** and **Spark**.</span></span> <span data-ttu-id="af7f0-110">További információ a kernelek, és a használatának előnyeit **PySpark**, lásd: [használata Jupyter notebook kernelek az Apache Spark hdinsight-fürtök](hdinsight-apache-spark-jupyter-notebook-kernels.md).</span><span class="sxs-lookup"><span data-stu-id="af7f0-110">For more information about the kernels, and the benefits of using **PySpark**, see [Use Jupyter notebook kernels with Apache Spark clusters in HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="af7f0-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="af7f0-111">Prerequisites</span></span>

* <span data-ttu-id="af7f0-112">**Egy Azure HDInsight Spark-fürt**.</span><span class="sxs-lookup"><span data-stu-id="af7f0-112">**An Azure HDInsight Spark cluster**.</span></span> <span data-ttu-id="af7f0-113">Útmutatásért lásd: [Apache Spark-fürt létrehozása az Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="af7f0-113">For instructions, see [Create an Apache Spark cluster in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>

## <a name="create-a-jupyter-notebook-to-run-interactive-queries"></a><span data-ttu-id="af7f0-114">Interaktív lekérdezések futtatása Jupyter notebook létrehozása</span><span class="sxs-lookup"><span data-stu-id="af7f0-114">Create a Jupyter notebook to run interactive queries</span></span>

<span data-ttu-id="af7f0-115">Lekérdezések futtatása, amely elérhető a fürthöz rendelt tárolási alapértelmezés szerint mintaadatok használjuk.</span><span class="sxs-lookup"><span data-stu-id="af7f0-115">To run queries, we use sample data that is by default available in the storage associated with the cluster.</span></span> <span data-ttu-id="af7f0-116">Azonban meg kell először adott adatok betöltése az Spark, a dataframe.</span><span class="sxs-lookup"><span data-stu-id="af7f0-116">However, you must first load that data into Spark as a dataframe.</span></span> <span data-ttu-id="af7f0-117">Miután a dataframe, rajta a Jupyter notebook használatával lekérdezéseket is futtathat.</span><span class="sxs-lookup"><span data-stu-id="af7f0-117">Once you have the dataframe, you can run queries on it using the Jupyter notebook.</span></span> <span data-ttu-id="af7f0-118">Ebben a szakaszban, olvassa el:</span><span class="sxs-lookup"><span data-stu-id="af7f0-118">In this section, you look at how to:</span></span>

* <span data-ttu-id="af7f0-119">A Spark dataframe minta adatkészlet regisztrálásához.</span><span class="sxs-lookup"><span data-stu-id="af7f0-119">Register a sample data set as a Spark dataframe.</span></span>
* <span data-ttu-id="af7f0-120">A dataframe kapcsolatos lekérdezések futtatása.</span><span class="sxs-lookup"><span data-stu-id="af7f0-120">Run queries on the dataframe.</span></span>

1. <span data-ttu-id="af7f0-121">Nyissa meg az [Azure portált](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="af7f0-121">Open the [Azure portal](https://portal.azure.com/).</span></span> <span data-ttu-id="af7f0-122">Ha rögzítette a fürtöt az irányítópulton, a fürt paneljének megnyitásához kattintson a fürt csempéjére az irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="af7f0-122">If you opted to pin the cluster to the dashboard, click the cluster tile from the dashboard to launch the cluster blade.</span></span>

    <span data-ttu-id="af7f0-123">Ha nem rögzítette a fürtöt az irányítópulton, a bal oldali panelen kattintson a **HDInsight-fürtök** elemre, majd a létrehozott fürtre.</span><span class="sxs-lookup"><span data-stu-id="af7f0-123">If you did not pin the cluster to the dashboard, from the left pane, click **HDInsight clusters**, and then click the cluster you created.</span></span>

3. <span data-ttu-id="af7f0-124">A **Gyorshivatkozások** menüben kattintson a **Fürt irányítópultjai** lehetőségre, majd a **Jupyter Notebook** elemre.</span><span class="sxs-lookup"><span data-stu-id="af7f0-124">From **Quick links**, click **Cluster dashboards**, and then click **Jupyter Notebook**.</span></span> <span data-ttu-id="af7f0-125">Ha a rendszer felkéri rá, adja meg a fürthöz tartozó rendszergazdai hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="af7f0-125">If prompted, enter the admin credentials for the cluster.</span></span>

   <span data-ttu-id="af7f0-126">![A Jupyter notebook megnyitása interaktív Spark SQL-lekérdezés futtatásához](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-start-jupyter-interactive-spark-sql-query.png "A Jupyter notebook megnyitása interaktív Spark SQL-lekérdezés futtatásához")</span><span class="sxs-lookup"><span data-stu-id="af7f0-126">![Open Jupyter notebook to run interactive Spark SQL query](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-start-jupyter-interactive-spark-sql-query.png "Open Jupyter notebook to run interactive Spark SQL query")</span></span>

   > [!NOTE]
   > <span data-ttu-id="af7f0-127">A fürthöz tartozó Jupyter notebookot az alábbi URL-cím böngészőben történő megnyitásával is elérheti.</span><span class="sxs-lookup"><span data-stu-id="af7f0-127">You may also access the Jupyter notebook for your cluster by opening the following URL in your browser.</span></span> <span data-ttu-id="af7f0-128">Cserélje le a **CLUSTERNAME** elemet a fürt nevére:</span><span class="sxs-lookup"><span data-stu-id="af7f0-128">Replace **CLUSTERNAME** with the name of your cluster:</span></span>
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >
3. <span data-ttu-id="af7f0-129">Hozzon létre egy notebookot.</span><span class="sxs-lookup"><span data-stu-id="af7f0-129">Create a notebook.</span></span> <span data-ttu-id="af7f0-130">Kattintson a **New** (Új), majd a **PySpark** elemre.</span><span class="sxs-lookup"><span data-stu-id="af7f0-130">Click **New**, and then click **PySpark**.</span></span>

   <span data-ttu-id="af7f0-131">![Jupyter notebook létrehozása interaktív Spark SQL-lekérdezés futtatásához](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-create-jupyter-interactive-Spark-SQL-query.png "Jupyter notebook létrehozása interaktív Spark SQL-lekérdezés futtatásához")</span><span class="sxs-lookup"><span data-stu-id="af7f0-131">![Create a Jupyter notebook to run interactive Spark SQL query](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-create-jupyter-interactive-Spark-SQL-query.png "Create a Jupyter notebook to run interactive Spark SQL query")</span></span>

   <span data-ttu-id="af7f0-132">Az új notebook létrejött, és Untitled(Untitled.pynb) néven nyílt meg.</span><span class="sxs-lookup"><span data-stu-id="af7f0-132">A new notebook is created and opened with the name Untitled(Untitled.pynb).</span></span>

4. <span data-ttu-id="af7f0-133">Ha a felső részen a notebook nevére kattint, megadhat egy könnyen megjegyezhető nevet.</span><span class="sxs-lookup"><span data-stu-id="af7f0-133">Click the notebook name at the top, and enter a friendly name if you want.</span></span>

    <span data-ttu-id="af7f0-134">![A Jupyter notebook elnevezése, amelyből interaktív Spark lekérdezést futtat](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-jupyter-notebook-name.png "A Jupyter notebook elnevezése, amelyből interaktív Spark lekérdezést futtat")</span><span class="sxs-lookup"><span data-stu-id="af7f0-134">![Provide a name for the Jupter notebook to run interactive Spark query from](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-jupyter-notebook-name.png "Provide a name for the Jupter notebook to run interactive Spark query from")</span></span>

5. <span data-ttu-id="af7f0-135">Illessze be a következő kódot egy üres cellába, majd nyomja le a **SHIFT + ENTER** billentyűkombinációt annak futtatásához.</span><span class="sxs-lookup"><span data-stu-id="af7f0-135">Paste the following code in an empty cell, and then press **SHIFT + ENTER** to run the code.</span></span> <span data-ttu-id="af7f0-136">A kód importálja az alábbi forgatókönyvhöz szükséges típusokat:</span><span class="sxs-lookup"><span data-stu-id="af7f0-136">The code imports the types required for this scenario:</span></span>

        from pyspark.sql.types import *

    <span data-ttu-id="af7f0-137">Mivel a notebook PySpark kernel használatával jött létre, explicit módon semmilyen tartalmat nem kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="af7f0-137">Because you created a notebook using the PySpark kernel, you do not need to create any contexts explicitly.</span></span> <span data-ttu-id="af7f0-138">Az első kódcella futtatásakor a Spark- és Hive-környezetek automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="af7f0-138">The Spark and Hive contexts are automatically created for you when you run the first code cell.</span></span>

    <span data-ttu-id="af7f0-139">![Az interaktív Spark SQL-lekérdezés állapota](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-interactive-spark-query-status.png "Az interaktív Spark SQL-lekérdezés állapota")</span><span class="sxs-lookup"><span data-stu-id="af7f0-139">![Status of interactive Spark SQL query](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-interactive-spark-query-status.png "Status of interactive Spark SQL query")</span></span>

    <span data-ttu-id="af7f0-140">Minden alkalommal, amikor a Jupyterben interaktív lekérdezést futtat, a webböngésző ablakának címsorában **(Foglalt)** állapot jelenik meg a notebook neve mellett.</span><span class="sxs-lookup"><span data-stu-id="af7f0-140">Every time you run an interactive query in Jupyter, your web browser window title shows a **(Busy)** status along with the notebook title.</span></span> <span data-ttu-id="af7f0-141">A jobb felső sarokban lévő **PySpark** felirat mellett ekkor egy teli kör is megjelenik.</span><span class="sxs-lookup"><span data-stu-id="af7f0-141">You also see a solid circle next to the **PySpark** text in the top-right corner.</span></span> <span data-ttu-id="af7f0-142">A feladat befejezése után ez a jel üres körre változik.</span><span class="sxs-lookup"><span data-stu-id="af7f0-142">After the job is completed, it changes to a hollow circle.</span></span>

6. <span data-ttu-id="af7f0-143">A Spark-fürt betölteni az adatokat, mielőtt tudassa velünk keresse meg a pillanatképe.</span><span class="sxs-lookup"><span data-stu-id="af7f0-143">Before you load the data into a Spark cluster, let us look a snapshot of it.</span></span> <span data-ttu-id="af7f0-144">Ebben az oktatóanyagban használt mintaadatok érhető el az összes HDInsight Spark-fürtök a CSV-fájlként **\HdiSamples\HdiSamples\SensorSampleData\hvac\hvac.csv**.</span><span class="sxs-lookup"><span data-stu-id="af7f0-144">The sample data used in this tutorial is available as a CSV file on all HDInsight Spark clusters at **\HdiSamples\HdiSamples\SensorSampleData\hvac\hvac.csv**.</span></span> <span data-ttu-id="af7f0-145">Az adatok egy épületet hőmérséklet változatait rögzíti.</span><span class="sxs-lookup"><span data-stu-id="af7f0-145">The data captures the temperature variations of a building.</span></span> <span data-ttu-id="af7f0-146">Az alábbiakban az adatok első néhány sor.</span><span class="sxs-lookup"><span data-stu-id="af7f0-146">Here are the first few rows of the data.</span></span>

    <span data-ttu-id="af7f0-147">![Az adatok interaktív Spark SQL-lekérdezés pillanatkép](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-sample-data-interactive-spark-sql-query.png "pillanatkép adatok interaktív Spark SQL-lekérdezés")</span><span class="sxs-lookup"><span data-stu-id="af7f0-147">![Snapshot of data for interactive Spark SQL query](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-sample-data-interactive-spark-sql-query.png "Snapshot of data for interactive Spark SQL query")</span></span>

6. <span data-ttu-id="af7f0-148">Hozzon létre egy dataframe és egy ideiglenes tábla (**hvac**) a következő kód futtatásával.</span><span class="sxs-lookup"><span data-stu-id="af7f0-148">Create a dataframe and a temporary table (**hvac**) by running the following code.</span></span> <span data-ttu-id="af7f0-149">Ebben az oktatóanyagban nem létrehozni az oszlopokat az ideiglenes tábla át a nyers adatok CSV képest.</span><span class="sxs-lookup"><span data-stu-id="af7f0-149">For this tutorial, we do not create all the columns in the temporary table as compared to the columns in the raw CSV data.</span></span> 

        # Create an RDD from sample data
        hvacText = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

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

7. <span data-ttu-id="af7f0-150">A tábla létrehozása után az adatok interaktív lekérdezés futtatása a következő kódot használja.</span><span class="sxs-lookup"><span data-stu-id="af7f0-150">Once the table is created, run interactive query on the data, use the following code.</span></span>

        %%sql
        SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"

   <span data-ttu-id="af7f0-151">Mivel PySpark kernelt használ, most közvetlenül futtathat interaktív SQL-lekérdezést az imént létrehozott **hvac** ideiglenes táblán, a `%%sql` funkció használatával.</span><span class="sxs-lookup"><span data-stu-id="af7f0-151">Because you are using a PySpark kernel, you can now directly run an interactive SQL query on the temporary table **hvac** that you created by using the `%%sql` magic.</span></span> <span data-ttu-id="af7f0-152">A `%%sql` funkcióval, illetve a PySpark kernellel elérhető egyéb funkciókkal kapcsolatos további információkat [A Spark HDInsight-fürtökkel használt Jupyter notebookokban elérhető kernelek](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic) című részben talál.</span><span class="sxs-lookup"><span data-stu-id="af7f0-152">For more information about the `%%sql` magic, and other magics available with the PySpark kernel, see [Kernels available on Jupyter notebooks with Spark HDInsight clusters](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).</span></span>

   <span data-ttu-id="af7f0-153">Alapértelmezés szerint az alábbi táblázatos kimenet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="af7f0-153">The following tabular output is displayed by default.</span></span>

     <span data-ttu-id="af7f0-154">![Az interaktív Spark-lekérdezési eredmény táblázati kimenete](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result.png "Az interaktív Spark-lekérdezési eredmény táblázati kimenete")</span><span class="sxs-lookup"><span data-stu-id="af7f0-154">![Table output of interactive Spark query result](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result.png "Table output of interactive Spark query result")</span></span>

    <span data-ttu-id="af7f0-155">Az eredményeket egyéb megjelenítési formákban is megtekintheti.</span><span class="sxs-lookup"><span data-stu-id="af7f0-155">You can also see the results in other visualizations as well.</span></span> <span data-ttu-id="af7f0-156">Az azonos kimenethez tartozó területgrafikon például az alábbihoz hasonlóan fog kinézni.</span><span class="sxs-lookup"><span data-stu-id="af7f0-156">For example, an area graph for the same output would look like the following.</span></span>

    <span data-ttu-id="af7f0-157">![Az interaktív Spark-lekérdezési eredmény területgrafikonja](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result-area-chart.png "Az interaktív Spark-lekérdezési eredmény területgrafikonja")</span><span class="sxs-lookup"><span data-stu-id="af7f0-157">![Area graph of interactive Spark query result](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result-area-chart.png "Area graph of interactive Spark query result")</span></span>

9. <span data-ttu-id="af7f0-158">Az alkalmazás futtatása után állítsa le a notebookot a fürt erőforrásainak felszabadítása érdekében.</span><span class="sxs-lookup"><span data-stu-id="af7f0-158">Shut down the notebook to release the cluster resources after you have finished running the application.</span></span> <span data-ttu-id="af7f0-159">Ehhez a notebook **File** (Fájl) menüjében kattintson a **Close and Halt** (Bezárás és leállítás) elemre.</span><span class="sxs-lookup"><span data-stu-id="af7f0-159">To do so, from the **File** menu on the notebook, click **Close and Halt**.</span></span>

## <a name="next-step"></a><span data-ttu-id="af7f0-160">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="af7f0-160">Next step</span></span>

<span data-ttu-id="af7f0-161">Ebben a cikkben megtanulta, interaktív lekérdezések futtatása Spark Jupyter notebook használatával.</span><span class="sxs-lookup"><span data-stu-id="af7f0-161">In this article you learned how to run interactive queries in Spark using Jupyter notebook.</span></span> <span data-ttu-id="af7f0-162">A következő cikk megjelenítéséhez, hogyan lehet-e a Spark regisztrált adatok lekért olyan BI analytics eszközt, például a Power bi-ban és a Tableau továbblépés.</span><span class="sxs-lookup"><span data-stu-id="af7f0-162">Advance to the next article to see how the data you registered in Spark can be pulled into a BI analytics tool such as Power BI and Tableau.</span></span> 

> [!div class="nextstepaction"]
>[<span data-ttu-id="af7f0-163">Adatok képi megjelenítés eszközökkel rendelkező Azure HDInsight Spark BI</span><span class="sxs-lookup"><span data-stu-id="af7f0-163">Spark BI using data visualization tools with Azure HDInsight</span></span>](hdinsight-apache-spark-use-bi-tools.md)




