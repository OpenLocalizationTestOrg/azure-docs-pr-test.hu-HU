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
# <a name="run-interactive-queries-on-an-hdinsight-spark-cluster"></a><span data-ttu-id="ac3ff-104">Interaktív lekérdezések futtatására egy HDInsight Spark-fürt</span><span class="sxs-lookup"><span data-stu-id="ac3ff-104">Run interactive queries on an HDInsight Spark cluster</span></span>

<span data-ttu-id="ac3ff-105">Ebben a cikkben a Jupyter notebook toorun interaktív Spark SQL lekérdezések Spark-fürt használatára.</span><span class="sxs-lookup"><span data-stu-id="ac3ff-105">In this article, you use a Jupyter notebook toorun interactive Spark SQL queries against a Spark cluster.</span></span> <span data-ttu-id="ac3ff-106">Jupyter notebook egy webböngésző-alapú alkalmazás, amely kiterjeszti a hello interaktivitási élményének Konzolalapú toohello webes.</span><span class="sxs-lookup"><span data-stu-id="ac3ff-106">Jupyter notebook is a browser-based application that extends hello console-based interactive experience toohello Web.</span></span> <span data-ttu-id="ac3ff-107">További információkért lásd: [hello Jupyter notebook](http://jupyter-notebook.readthedocs.io/en/latest/notebook.html).</span><span class="sxs-lookup"><span data-stu-id="ac3ff-107">For more information, see [hello Jupyter notebook](http://jupyter-notebook.readthedocs.io/en/latest/notebook.html).</span></span>

<span data-ttu-id="ac3ff-108">Ebben az oktatóanyagban hello használata **PySpark** kernel a hello Jupyter notebook toorun egy interaktív Spark SQL-lekérdezésben.</span><span class="sxs-lookup"><span data-stu-id="ac3ff-108">For this tutorial, you use hello **PySpark** kernel in hello Jupyter notebook toorun an interactive Spark SQL query.</span></span> <span data-ttu-id="ac3ff-109">A HDInsight-fürtökön Jupyter notebookok is támogatja a két más kernelek - **PySpark3** és **Spark**.</span><span class="sxs-lookup"><span data-stu-id="ac3ff-109">Jupyter notebooks on HDInsight clusters also support two other kernels - **PySpark3** and **Spark**.</span></span> <span data-ttu-id="ac3ff-110">További információ a hello kernelek és hello előnyei a **PySpark**, lásd: [használata Jupyter notebook kernelek az Apache Spark hdinsight-fürtök](hdinsight-apache-spark-jupyter-notebook-kernels.md).</span><span class="sxs-lookup"><span data-stu-id="ac3ff-110">For more information about hello kernels, and hello benefits of using **PySpark**, see [Use Jupyter notebook kernels with Apache Spark clusters in HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ac3ff-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ac3ff-111">Prerequisites</span></span>

* <span data-ttu-id="ac3ff-112">**Egy Azure HDInsight Spark-fürt**.</span><span class="sxs-lookup"><span data-stu-id="ac3ff-112">**An Azure HDInsight Spark cluster**.</span></span> <span data-ttu-id="ac3ff-113">Útmutatásért lásd: [Apache Spark-fürt létrehozása az Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="ac3ff-113">For instructions, see [Create an Apache Spark cluster in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>

## <a name="create-a-jupyter-notebook-toorun-interactive-queries"></a><span data-ttu-id="ac3ff-114">Jupyter notebook toorun interaktív lekérdezések létrehozása</span><span class="sxs-lookup"><span data-stu-id="ac3ff-114">Create a Jupyter notebook toorun interactive queries</span></span>

<span data-ttu-id="ac3ff-115">toorun lekérdezések használjuk a mintaadatok, akkor hello-fürthöz tartozó hello Storage alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="ac3ff-115">toorun queries, we use sample data that is by default available in hello storage associated with hello cluster.</span></span> <span data-ttu-id="ac3ff-116">Azonban meg kell először adott adatok betöltése az Spark, a dataframe.</span><span class="sxs-lookup"><span data-stu-id="ac3ff-116">However, you must first load that data into Spark as a dataframe.</span></span> <span data-ttu-id="ac3ff-117">Miután hello dataframe, lekérdezéseket is futtathat a hello Jupyter notebook használatával.</span><span class="sxs-lookup"><span data-stu-id="ac3ff-117">Once you have hello dataframe, you can run queries on it using hello Jupyter notebook.</span></span> <span data-ttu-id="ac3ff-118">Ebben a szakaszban, olvassa el:</span><span class="sxs-lookup"><span data-stu-id="ac3ff-118">In this section, you look at how to:</span></span>

* <span data-ttu-id="ac3ff-119">A Spark dataframe minta adatkészlet regisztrálásához.</span><span class="sxs-lookup"><span data-stu-id="ac3ff-119">Register a sample data set as a Spark dataframe.</span></span>
* <span data-ttu-id="ac3ff-120">Hello dataframe kapcsolatos lekérdezések futtatása.</span><span class="sxs-lookup"><span data-stu-id="ac3ff-120">Run queries on hello dataframe.</span></span>

1. <span data-ttu-id="ac3ff-121">Nyissa meg hello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="ac3ff-121">Open hello [Azure portal](https://portal.azure.com/).</span></span> <span data-ttu-id="ac3ff-122">Ha toopin hello fürt toohello irányítópult választotta, kattintson a hello fürt csempe hello irányítópult toolaunch hello fürt paneljén.</span><span class="sxs-lookup"><span data-stu-id="ac3ff-122">If you opted toopin hello cluster toohello dashboard, click hello cluster tile from hello dashboard toolaunch hello cluster blade.</span></span>

    <span data-ttu-id="ac3ff-123">Ha nem volt rögzíti hello fürt toohello irányítópult, hello bal oldali ablaktáblában kattintson **a HDInsight-fürtök**, majd kattintson a létrehozott hello fürt.</span><span class="sxs-lookup"><span data-stu-id="ac3ff-123">If you did not pin hello cluster toohello dashboard, from hello left pane, click **HDInsight clusters**, and then click hello cluster you created.</span></span>

3. <span data-ttu-id="ac3ff-124">A **Gyorshivatkozások** menüben kattintson a **Fürt irányítópultjai** lehetőségre, majd a **Jupyter Notebook** elemre.</span><span class="sxs-lookup"><span data-stu-id="ac3ff-124">From **Quick links**, click **Cluster dashboards**, and then click **Jupyter Notebook**.</span></span> <span data-ttu-id="ac3ff-125">Ha a rendszer kéri, adja meg hello fürt hello rendszergazdai hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="ac3ff-125">If prompted, enter hello admin credentials for hello cluster.</span></span>

   <span data-ttu-id="ac3ff-126">![Nyissa meg Jupyter notebook toorun interaktív Spark SQL-lekérdezés](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-start-jupyter-interactive-spark-sql-query.png "nyitott Jupyter notebook toorun interaktív Spark SQL-lekérdezés")</span><span class="sxs-lookup"><span data-stu-id="ac3ff-126">![Open Jupyter notebook toorun interactive Spark SQL query](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-start-jupyter-interactive-spark-sql-query.png "Open Jupyter notebook toorun interactive Spark SQL query")</span></span>

   > [!NOTE]
   > <span data-ttu-id="ac3ff-127">A fürt URL-címet a böngészőben a következő megnyitásakor hello által hello Jupyter notebook is elérhetők.</span><span class="sxs-lookup"><span data-stu-id="ac3ff-127">You may also access hello Jupyter notebook for your cluster by opening hello following URL in your browser.</span></span> <span data-ttu-id="ac3ff-128">Cserélje le **CLUSTERNAME** hello néven a fürt:</span><span class="sxs-lookup"><span data-stu-id="ac3ff-128">Replace **CLUSTERNAME** with hello name of your cluster:</span></span>
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >
3. <span data-ttu-id="ac3ff-129">Hozzon létre egy notebookot.</span><span class="sxs-lookup"><span data-stu-id="ac3ff-129">Create a notebook.</span></span> <span data-ttu-id="ac3ff-130">Kattintson a **New** (Új), majd a **PySpark** elemre.</span><span class="sxs-lookup"><span data-stu-id="ac3ff-130">Click **New**, and then click **PySpark**.</span></span>

   <span data-ttu-id="ac3ff-131">![A Jupyter notebook toorun interaktív Spark SQL-lekérdezés létrehozása](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-create-jupyter-interactive-Spark-SQL-query.png "a Jupyter notebook toorun interaktív Spark SQL-lekérdezés létrehozása")</span><span class="sxs-lookup"><span data-stu-id="ac3ff-131">![Create a Jupyter notebook toorun interactive Spark SQL query](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-create-jupyter-interactive-Spark-SQL-query.png "Create a Jupyter notebook toorun interactive Spark SQL query")</span></span>

   <span data-ttu-id="ac3ff-132">Új notebook létrejött, és hello nevű Untitled(Untitled.pynb).</span><span class="sxs-lookup"><span data-stu-id="ac3ff-132">A new notebook is created and opened with hello name Untitled(Untitled.pynb).</span></span>

4. <span data-ttu-id="ac3ff-133">Hello notebook neve hello tetején kattintson, és adja meg egy rövid nevet, ha szeretné.</span><span class="sxs-lookup"><span data-stu-id="ac3ff-133">Click hello notebook name at hello top, and enter a friendly name if you want.</span></span>

    <span data-ttu-id="ac3ff-134">![Adjon meg egy nevet hello Jupter notebook toorun interaktív Spark-lekérdezést](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-jupyter-notebook-name.png "adjon meg egy nevet hello Jupter notebook toorun interaktív Spark lekérdezése")</span><span class="sxs-lookup"><span data-stu-id="ac3ff-134">![Provide a name for hello Jupter notebook toorun interactive Spark query from](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-jupyter-notebook-name.png "Provide a name for hello Jupter notebook toorun interactive Spark query from")</span></span>

5. <span data-ttu-id="ac3ff-135">Beillesztés hello következő kód egy üres cellába, és nyomja le az **SHIFT + ENTER** toorun hello kódot.</span><span class="sxs-lookup"><span data-stu-id="ac3ff-135">Paste hello following code in an empty cell, and then press **SHIFT + ENTER** toorun hello code.</span></span> <span data-ttu-id="ac3ff-136">hello kód importálja az ehhez a forgatókönyvhöz szükséges hello típusok:</span><span class="sxs-lookup"><span data-stu-id="ac3ff-136">hello code imports hello types required for this scenario:</span></span>

        from pyspark.sql.types import *

    <span data-ttu-id="ac3ff-137">Mivel a notebook PySpark kernelt hello hozott létre, nem kell toocreate semmilyen tartalmat explicit módon.</span><span class="sxs-lookup"><span data-stu-id="ac3ff-137">Because you created a notebook using hello PySpark kernel, you do not need toocreate any contexts explicitly.</span></span> <span data-ttu-id="ac3ff-138">hello Spark és Hive-környezetek automatikusan létrejönnek hello első kódcella futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="ac3ff-138">hello Spark and Hive contexts are automatically created for you when you run hello first code cell.</span></span>

    <span data-ttu-id="ac3ff-139">![Az interaktív Spark SQL-lekérdezés állapota](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-interactive-spark-query-status.png "Az interaktív Spark SQL-lekérdezés állapota")</span><span class="sxs-lookup"><span data-stu-id="ac3ff-139">![Status of interactive Spark SQL query](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-interactive-spark-query-status.png "Status of interactive Spark SQL query")</span></span>

    <span data-ttu-id="ac3ff-140">Minden alkalommal, amikor az interaktív lekérdezések futtatása a Jupyter, a webböngésző ablakának címsorában látható egy **(foglalt)** állapot hello notebook neve mellett.</span><span class="sxs-lookup"><span data-stu-id="ac3ff-140">Every time you run an interactive query in Jupyter, your web browser window title shows a **(Busy)** status along with hello notebook title.</span></span> <span data-ttu-id="ac3ff-141">Egy teli kör következő toohello is látni **PySpark** hello jobb felső sarokban lévő szöveg.</span><span class="sxs-lookup"><span data-stu-id="ac3ff-141">You also see a solid circle next toohello **PySpark** text in hello top-right corner.</span></span> <span data-ttu-id="ac3ff-142">Hello feladat befejezése után tooa jel üres körre változik.</span><span class="sxs-lookup"><span data-stu-id="ac3ff-142">After hello job is completed, it changes tooa hollow circle.</span></span>

6. <span data-ttu-id="ac3ff-143">Mielőtt hello adatok betölthető Spark-fürt, tudassa velünk keresse meg a pillanatképe.</span><span class="sxs-lookup"><span data-stu-id="ac3ff-143">Before you load hello data into a Spark cluster, let us look a snapshot of it.</span></span> <span data-ttu-id="ac3ff-144">hello ebben az oktatóanyagban használt mintaadatok érhető el az összes HDInsight Spark-fürtök a CSV-fájlként **\HdiSamples\HdiSamples\SensorSampleData\hvac\hvac.csv**.</span><span class="sxs-lookup"><span data-stu-id="ac3ff-144">hello sample data used in this tutorial is available as a CSV file on all HDInsight Spark clusters at **\HdiSamples\HdiSamples\SensorSampleData\hvac\hvac.csv**.</span></span> <span data-ttu-id="ac3ff-145">hello adatok hello hőmérséklet változata épület rögzíti.</span><span class="sxs-lookup"><span data-stu-id="ac3ff-145">hello data captures hello temperature variations of a building.</span></span> <span data-ttu-id="ac3ff-146">Az alábbiakban hello hello adatok első néhány sor.</span><span class="sxs-lookup"><span data-stu-id="ac3ff-146">Here are hello first few rows of hello data.</span></span>

    <span data-ttu-id="ac3ff-147">![Az adatok interaktív Spark SQL-lekérdezés pillanatkép](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-sample-data-interactive-spark-sql-query.png "pillanatkép adatok interaktív Spark SQL-lekérdezés")</span><span class="sxs-lookup"><span data-stu-id="ac3ff-147">![Snapshot of data for interactive Spark SQL query](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-sample-data-interactive-spark-sql-query.png "Snapshot of data for interactive Spark SQL query")</span></span>

6. <span data-ttu-id="ac3ff-148">Hozzon létre egy dataframe és egy ideiglenes tábla (**hvac**) a következő kód hello futtatásával.</span><span class="sxs-lookup"><span data-stu-id="ac3ff-148">Create a dataframe and a temporary table (**hvac**) by running hello following code.</span></span> <span data-ttu-id="ac3ff-149">Ebben az oktatóanyagban nem létrehozni minden hello oszlopok hello ideiglenes tábla hello CSV nyersadatok összehasonlított toohello oszlopként.</span><span class="sxs-lookup"><span data-stu-id="ac3ff-149">For this tutorial, we do not create all hello columns in hello temporary table as compared toohello columns in hello raw CSV data.</span></span> 

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

7. <span data-ttu-id="ac3ff-150">Hello tábla létrehozása után interaktív lekérdezés futtatása hello adatokat, használja a következő kód hello.</span><span class="sxs-lookup"><span data-stu-id="ac3ff-150">Once hello table is created, run interactive query on hello data, use hello following code.</span></span>

        %%sql
        SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"

   <span data-ttu-id="ac3ff-151">Mivel PySpark kernelt használ, akkor most közvetlenül futtathat SQL interaktív lekérdezés hello ideiglenes táblán **hvac** hello segítségével létrehozott `%%sql` magic.</span><span class="sxs-lookup"><span data-stu-id="ac3ff-151">Because you are using a PySpark kernel, you can now directly run an interactive SQL query on hello temporary table **hvac** that you created by using hello `%%sql` magic.</span></span> <span data-ttu-id="ac3ff-152">Hello kapcsolatos további információk `%%sql` magic és hello PySpark kernellel elérhető egyéb magics [Spark HDInsight-fürtökkel használt Jupyter notebookokban elérhető kernelek](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).</span><span class="sxs-lookup"><span data-stu-id="ac3ff-152">For more information about hello `%%sql` magic, and other magics available with hello PySpark kernel, see [Kernels available on Jupyter notebooks with Spark HDInsight clusters](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).</span></span>

   <span data-ttu-id="ac3ff-153">a következő táblázatos kimenet hello alapértelmezés szerint megjelenik.</span><span class="sxs-lookup"><span data-stu-id="ac3ff-153">hello following tabular output is displayed by default.</span></span>

     <span data-ttu-id="ac3ff-154">![Az interaktív Spark-lekérdezési eredmény táblázati kimenete](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result.png "Az interaktív Spark-lekérdezési eredmény táblázati kimenete")</span><span class="sxs-lookup"><span data-stu-id="ac3ff-154">![Table output of interactive Spark query result](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result.png "Table output of interactive Spark query result")</span></span>

    <span data-ttu-id="ac3ff-155">Hello eredményeket egyéb megjelenítési formákban is megtekintheti.</span><span class="sxs-lookup"><span data-stu-id="ac3ff-155">You can also see hello results in other visualizations as well.</span></span> <span data-ttu-id="ac3ff-156">Például tartozó területgrafikon hello azonos kimenethez hello következő jelenne meg.</span><span class="sxs-lookup"><span data-stu-id="ac3ff-156">For example, an area graph for hello same output would look like hello following.</span></span>

    <span data-ttu-id="ac3ff-157">![Az interaktív Spark-lekérdezési eredmény területgrafikonja](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result-area-chart.png "Az interaktív Spark-lekérdezési eredmény területgrafikonja")</span><span class="sxs-lookup"><span data-stu-id="ac3ff-157">![Area graph of interactive Spark query result](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result-area-chart.png "Area graph of interactive Spark query result")</span></span>

9. <span data-ttu-id="ac3ff-158">Hello notebook toorelease hello fürterőforrások leállítása hello alkalmazást futtató befejezése után.</span><span class="sxs-lookup"><span data-stu-id="ac3ff-158">Shut down hello notebook toorelease hello cluster resources after you have finished running hello application.</span></span> <span data-ttu-id="ac3ff-159">toodo Igen, a hello **fájl** hello notebook menüjében kattintson **zárja be és Halt**.</span><span class="sxs-lookup"><span data-stu-id="ac3ff-159">toodo so, from hello **File** menu on hello notebook, click **Close and Halt**.</span></span>

## <a name="next-step"></a><span data-ttu-id="ac3ff-160">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="ac3ff-160">Next step</span></span>

<span data-ttu-id="ac3ff-161">Az e cikkben megtanulta, hogyan meg interaktív lekérdezések toorun Spark Jupyter notebook használatával.</span><span class="sxs-lookup"><span data-stu-id="ac3ff-161">In this article you learned how toorun interactive queries in Spark using Jupyter notebook.</span></span> <span data-ttu-id="ac3ff-162">Előzetes toohello hogyan kell húzni Spark regisztrált hello adatok tovább cikk toosee olyan BI analytics eszközt, például a Power bi-ban és a Tableau.</span><span class="sxs-lookup"><span data-stu-id="ac3ff-162">Advance toohello next article toosee how hello data you registered in Spark can be pulled into a BI analytics tool such as Power BI and Tableau.</span></span> 

> [!div class="nextstepaction"]
>[<span data-ttu-id="ac3ff-163">Adatok képi megjelenítés eszközökkel rendelkező Azure HDInsight Spark BI</span><span class="sxs-lookup"><span data-stu-id="ac3ff-163">Spark BI using data visualization tools with Azure HDInsight</span></span>](hdinsight-apache-spark-use-bi-tools.md)




