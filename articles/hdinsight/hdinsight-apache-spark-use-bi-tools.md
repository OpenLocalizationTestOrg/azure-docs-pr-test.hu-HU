---
title: "aaaSpark BI adatok képi megjelenítés eszközökkel on Azure HDInsight |} Microsoft Docs"
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
ms.openlocfilehash: ba4bfff737ce80ffca5c24f1c2c82a1447f467fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="apache-spark-bi-using-data-visualization-tools-with-azure-hdinsight"></a><span data-ttu-id="199cf-104">Apache Spark BI adatok képi megjelenítés eszközökkel Azure hdinsightban</span><span class="sxs-lookup"><span data-stu-id="199cf-104">Apache Spark BI using data visualization tools with Azure HDInsight</span></span>

<span data-ttu-id="199cf-105">Ismerje meg, hogyan toouse adatábrázolási eszközök, például a Power BI és a Tableau tooanalyze a nyers minta adatkészletet, Apache Spark BI használata a HDInsight-fürtökön.</span><span class="sxs-lookup"><span data-stu-id="199cf-105">Learn how toouse data visualization tools such as Power BI and Tableau tooanalyze a raw sample data set using Apache Spark BI on HDInsight clusters.</span></span>

> [!NOTE]
> <span data-ttu-id="199cf-106">Ebben a cikkben leírt Üzletiintelligencia-eszközök kapcsolattal nem támogatott Spark 2.1 a Azure HDInsight 3.6 Preview-ban.</span><span class="sxs-lookup"><span data-stu-id="199cf-106">Connectivity with BI tools described in this article is not supported on Spark 2.1 on Azure HDInsight 3.6 Preview.</span></span> <span data-ttu-id="199cf-107">Csak a Spark verziók 1.6-os és 2.0 (HDInsight 3.4, 3.5 rendre) támogatottak.</span><span class="sxs-lookup"><span data-stu-id="199cf-107">Only Spark versions 1.6 and 2.0 (HDInsight 3.4, 3.5 respectively) are supported.</span></span>
>

<span data-ttu-id="199cf-108">Ez az oktatóanyag egy HDInsight Spark-fürt Jupyter notebook, is érhető el.</span><span class="sxs-lookup"><span data-stu-id="199cf-108">This tutorial is also available as a Jupyter notebook on an HDInsight Spark cluster.</span></span> <span data-ttu-id="199cf-109">hello notebook élmény lehetővé teszi a hello Python kódtöredékek futtassa hello notebook magát.</span><span class="sxs-lookup"><span data-stu-id="199cf-109">hello notebook experience lets you run hello Python snippets from hello notebook itself.</span></span> <span data-ttu-id="199cf-110">tooperform hello oktatóprogram belül a notebook Spark-fürt létrehozása, indítsa el a Jupyter notebook (`https://CLUSTERNAME.azurehdinsight.net/jupyter`), és futtassa a hello notebook **használata BI eszközök az Apache Spark on HDInsight.ipynb** alatt hello **Python**  mappa.</span><span class="sxs-lookup"><span data-stu-id="199cf-110">tooperform hello tutorial from within a notebook, create a Spark cluster, launch a Jupyter notebook (`https://CLUSTERNAME.azurehdinsight.net/jupyter`), and then run hello notebook **Use BI tools with Apache Spark on HDInsight.ipynb** under hello **Python** folder.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="199cf-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="199cf-111">Prerequisites</span></span>

* <span data-ttu-id="199cf-112">A HDInsight az Apache Spark-fürt.</span><span class="sxs-lookup"><span data-stu-id="199cf-112">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="199cf-113">Útmutatásért lásd: [létrehozása az Apache Spark on Azure hdinsight clusters](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="199cf-113">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>


## <span data-ttu-id="199cf-114"><a name="hivetable"></a>Adatok előkészítése Spark adatábrázolási</span><span class="sxs-lookup"><span data-stu-id="199cf-114"><a name="hivetable"></a>Prepare data for Spark data visualization</span></span>

<span data-ttu-id="199cf-115">Ebben a szakaszban hello használjuk [Jupyter](https://jupyter.org) notebook egy HDInsight Spark-fürt toorun feladatokból, amely a nyers mintaadatok feldolgozni, és mentse egy tábla.</span><span class="sxs-lookup"><span data-stu-id="199cf-115">In this section, we use hello [Jupyter](https://jupyter.org) notebook from an HDInsight Spark cluster toorun jobs that process your raw sample data and save it as a table.</span></span> <span data-ttu-id="199cf-116">hello mintaadatokat egy CSV-fájlt (hvac.csv) elérhető alapértelmezés szerint minden fürtön.</span><span class="sxs-lookup"><span data-stu-id="199cf-116">hello sample data is a .csv file (hvac.csv) available on all clusters by default.</span></span> <span data-ttu-id="199cf-117">Miután az adatok mentése táblázatként hello a következő szakaszban azt BI eszközök tooconnect toohello tábla, és hajtsa végre az adatmegjelenítéseket.</span><span class="sxs-lookup"><span data-stu-id="199cf-117">Once your data is saved as a table, in hello next section we use BI tools tooconnect toohello table and perform data visualizations.</span></span>

> [!NOTE]
> <span data-ttu-id="199cf-118">Akkor, ha hello szükséges lépések Ez a cikk utasításait hello befejezése után [interaktív lekérdezések futtatására egy HDInsight Spark-fürt](hdinsight-apache-spark-load-data-run-query.md), az alábbi 8 tooStep kihagyhatja.</span><span class="sxs-lookup"><span data-stu-id="199cf-118">If you are performing hello steps in this article after completing hello instructions in [Run interactive queries on an HDInsight Spark cluster](hdinsight-apache-spark-load-data-run-query.md), you can skip tooStep 8 below.</span></span>
>

1. <span data-ttu-id="199cf-119">A hello [Azure-portálon](https://portal.azure.com/), hello kezdőpulton, kattintson a Spark-fürt hello csempére (ha toohello kezdőpulton rögzítette azt).</span><span class="sxs-lookup"><span data-stu-id="199cf-119">From hello [Azure portal](https://portal.azure.com/), from hello startboard, click hello tile for your Spark cluster (if you pinned it toohello startboard).</span></span> <span data-ttu-id="199cf-120">Tooyour fürt alapján is megtalálhatja **összes tallózása** > **a HDInsight-fürtök**.</span><span class="sxs-lookup"><span data-stu-id="199cf-120">You can also navigate tooyour cluster under **Browse All** > **HDInsight Clusters**.</span></span>   

2. <span data-ttu-id="199cf-121">Hello Spark-fürt panelén kattintson **fürt irányítópult**, és kattintson a **Jupyter Notebook**.</span><span class="sxs-lookup"><span data-stu-id="199cf-121">From hello Spark cluster blade, click **Cluster Dashboard**, and then click **Jupyter Notebook**.</span></span> <span data-ttu-id="199cf-122">Ha a rendszer kéri, adja meg hello fürt hello rendszergazdai hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="199cf-122">If prompted, enter hello admin credentials for hello cluster.</span></span>

   > [!NOTE]
   > <span data-ttu-id="199cf-123">A fürt URL-címet a böngészőben a következő megnyitásakor hello által is hello Jupyter Notebook lehet elérni.</span><span class="sxs-lookup"><span data-stu-id="199cf-123">You may also reach hello Jupyter Notebook for your cluster by opening hello following URL in your browser.</span></span> <span data-ttu-id="199cf-124">Cserélje le **CLUSTERNAME** hello néven a fürt:</span><span class="sxs-lookup"><span data-stu-id="199cf-124">Replace **CLUSTERNAME** with hello name of your cluster:</span></span>
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >

3. <span data-ttu-id="199cf-125">Hozzon létre egy notebookot.</span><span class="sxs-lookup"><span data-stu-id="199cf-125">Create a notebook.</span></span> <span data-ttu-id="199cf-126">Kattintson a **New** (Új), majd a **PySpark** elemre.</span><span class="sxs-lookup"><span data-stu-id="199cf-126">Click **New**, and then click **PySpark**.</span></span>

    <span data-ttu-id="199cf-127">![Jupyter notebook létrehozása az Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/create-jupyter-notebook-for-spark-bi.png "Apache Spark bi Jupyter notebook létrehozása")</span><span class="sxs-lookup"><span data-stu-id="199cf-127">![Create a Jupyter notebook for Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/create-jupyter-notebook-for-spark-bi.png "Create a Jupyter notebook for Apache Spark BI")</span></span>

4. <span data-ttu-id="199cf-128">Új notebook létrejött, és Untitled.pynb hello nevű.</span><span class="sxs-lookup"><span data-stu-id="199cf-128">A new notebook is created and opened with hello name Untitled.pynb.</span></span> <span data-ttu-id="199cf-129">Hello notebook neve hello tetején kattintson, és adjon meg egy rövid nevet.</span><span class="sxs-lookup"><span data-stu-id="199cf-129">Click hello notebook name at hello top, and enter a friendly name.</span></span>

    <span data-ttu-id="199cf-130">![Nevezze el a hello notebook Apache Spark bi](./media/hdinsight-apache-spark-use-bi-tools/jupyter-notebook-name-for-spark-bi.png "nevezze el a hello notebook Apache Spark BI")</span><span class="sxs-lookup"><span data-stu-id="199cf-130">![Provide a name for hello notebook for Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/jupyter-notebook-name-for-spark-bi.png "Provide a name for hello notebook for Apache Spark BI")</span></span>

5. <span data-ttu-id="199cf-131">Mivel a notebook PySpark kernelt hello hozott létre, nem kell toocreate semmilyen tartalmat explicit módon.</span><span class="sxs-lookup"><span data-stu-id="199cf-131">Because you created a notebook using hello PySpark kernel, you do not need toocreate any contexts explicitly.</span></span> <span data-ttu-id="199cf-132">hello Spark és Hive-környezetek automatikusan létrejönnek hello első kódcella futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="199cf-132">hello Spark and Hive contexts are automatically created for you when you run hello first code cell.</span></span> <span data-ttu-id="199cf-133">Ehhez a forgatókönyvhöz szükséges hello típusok importálásával megkezdése.</span><span class="sxs-lookup"><span data-stu-id="199cf-133">You can start by importing hello types required for this scenario.</span></span> <span data-ttu-id="199cf-134">toodo tehát kurzorral hello hello cellába, majd nyomja le az **SHIFT + ENTER**.</span><span class="sxs-lookup"><span data-stu-id="199cf-134">toodo so, place hello cursor in hello cell and press **SHIFT + ENTER**.</span></span>

        from pyspark.sql import *

6. <span data-ttu-id="199cf-135">Töltse be a mintaadatokat egy ideiglenes táblába.</span><span class="sxs-lookup"><span data-stu-id="199cf-135">Load sample data into a temporary table.</span></span> <span data-ttu-id="199cf-136">Spark-fürt hdinsightban történő hello mintaadatfájlokat, létrehozásakor **hvac.csv**, a másolt toohello kapcsolódó tárfiók **\HdiSamples\HdiSamples\SensorSampleData\hvac**.</span><span class="sxs-lookup"><span data-stu-id="199cf-136">When you create a Spark cluster in HDInsight, hello sample data file, **hvac.csv**, is copied toohello associated storage account under **\HdiSamples\HdiSamples\SensorSampleData\hvac**.</span></span>

    <span data-ttu-id="199cf-137">A cella üres, illessze be a hello alábbi kódrészletet, és nyomja le az **SHIFT + ENTER**.</span><span class="sxs-lookup"><span data-stu-id="199cf-137">In an empty cell, paste hello following snippet and press **SHIFT + ENTER**.</span></span> <span data-ttu-id="199cf-138">Ezt a kódrészletet a következő táblába regisztrálja a hello adatok **hvac**.</span><span class="sxs-lookup"><span data-stu-id="199cf-138">This snippet registers hello data into a table called **hvac**.</span></span>

        # Create an RDD from sample data
        hvacText = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

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

7. <span data-ttu-id="199cf-139">Győződjön meg arról, hogy hello tábla létrehozása sikeresen megtörtént.</span><span class="sxs-lookup"><span data-stu-id="199cf-139">Verify that hello table was successfully created.</span></span> <span data-ttu-id="199cf-140">Használhatja a hello `%%sql` magic toorun Hive-lekérdezéseket közvetlenül.</span><span class="sxs-lookup"><span data-stu-id="199cf-140">You can use hello `%%sql` magic toorun Hive queries directly.</span></span> <span data-ttu-id="199cf-141">Hello kapcsolatos további információk `%%sql` magic és hello PySpark kernellel elérhető egyéb magics [Spark HDInsight-fürtökkel használt Jupyter notebookokban elérhető kernelek](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).</span><span class="sxs-lookup"><span data-stu-id="199cf-141">For more information about hello `%%sql` magic, and other magics available with hello PySpark kernel, see [Kernels available on Jupyter notebooks with Spark HDInsight clusters](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).</span></span>

        %%sql
        SHOW TABLES

    <span data-ttu-id="199cf-142">Hasonló kimenetnek látja, lent látható módon:</span><span class="sxs-lookup"><span data-stu-id="199cf-142">You see an output like shown below:</span></span>

        +---------------+-------------+
        |tableName      |isTemporary  |
        +---------------+-------------+
        |hvactemptable  |true        |
        |hivesampletable|false        |
        |hvac           |false        |
        +---------------+-------------+

    <span data-ttu-id="199cf-143">Csak a tábla, amelyen a hello hamis hello **isTemporary** oszlop hello metaadattárhoz tárolódnak, és elérhető az hello Üzletiintelligencia-eszközök hive táblákat.</span><span class="sxs-lookup"><span data-stu-id="199cf-143">Only hello tables that have false under hello **isTemporary** column are hive tables that are stored in hello metastore and can be accessed from hello BI tools.</span></span> <span data-ttu-id="199cf-144">Ebben az oktatóanyagban a Microsoft connect toohello **hvac** létrehozott tábla.</span><span class="sxs-lookup"><span data-stu-id="199cf-144">In this tutorial, we connect toohello **hvac** table we created.</span></span>

8. <span data-ttu-id="199cf-145">Győződjön meg arról, hogy hello tábla szánt hello adatokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="199cf-145">Verify that hello table contains hello intended data.</span></span> <span data-ttu-id="199cf-146">Egy üres cellába hello notebook, másolja a hello alábbi kódrészletet, és nyomja le az **SHIFT + ENTER**.</span><span class="sxs-lookup"><span data-stu-id="199cf-146">In an empty cell in hello notebook, copy hello following snippet and press **SHIFT + ENTER**.</span></span>

        %%sql
        SELECT * FROM hvac LIMIT 10

9. <span data-ttu-id="199cf-147">Állítsa le a hello notebook toorelease hello erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="199cf-147">Shut down hello notebook toorelease hello resources.</span></span> <span data-ttu-id="199cf-148">toodo Igen, a hello **fájl** hello notebook menüjében kattintson **zárja be és Halt**.</span><span class="sxs-lookup"><span data-stu-id="199cf-148">toodo so, from hello **File** menu on hello notebook, click **Close and Halt**.</span></span>

## <span data-ttu-id="199cf-149"><a name="powerbi"></a>A Power BI használata Spark adatábrázolási</span><span class="sxs-lookup"><span data-stu-id="199cf-149"><a name="powerbi"></a>Use Power BI for Spark data visualization</span></span>

> [!NOTE]
> <span data-ttu-id="199cf-150">Ez a szakasz esetén csak a HDInsight 3.4 Spark 1.6-os és Spark 2.0 a HDInsight 3.5 alkalmazható.</span><span class="sxs-lookup"><span data-stu-id="199cf-150">This section is applicable only for Spark 1.6 on HDInsight 3.4 and Spark 2.0 on HDInsight 3.5.</span></span>
>
>

<span data-ttu-id="199cf-151">Ha hello adatok táblázatként mentette, használja a Power BI tooconnect toohello adatokat, és megjelenítheti azt toocreate jelentések irányítópultok stb.</span><span class="sxs-lookup"><span data-stu-id="199cf-151">Once you have saved hello data as a table, you can use Power BI tooconnect toohello data and visualize it toocreate reports, dashboards, etc.</span></span>

1. <span data-ttu-id="199cf-152">Győződjön meg arról, hogy hozzáférési tooPower BI.</span><span class="sxs-lookup"><span data-stu-id="199cf-152">Make sure you have access tooPower BI.</span></span> <span data-ttu-id="199cf-153">A Power bi-ban az ingyenes kiadásra előfizetői kaphat [http://www.powerbi.com/](http://www.powerbi.com/).</span><span class="sxs-lookup"><span data-stu-id="199cf-153">You can get a free preview subscription of Power BI from [http://www.powerbi.com/](http://www.powerbi.com/).</span></span>

2. <span data-ttu-id="199cf-154">Jelentkezzen be a túl[Power BI](http://www.powerbi.com/).</span><span class="sxs-lookup"><span data-stu-id="199cf-154">Sign in too[Power BI](http://www.powerbi.com/).</span></span>

3. <span data-ttu-id="199cf-155">Hello aljáról hello bal oldali panelen, kattintson a **adatok beolvasása**.</span><span class="sxs-lookup"><span data-stu-id="199cf-155">From hello bottom of hello left pane, click **Get Data**.</span></span>

4. <span data-ttu-id="199cf-156">A hello **adatok beolvasása** lap **importálni, vagy csatlakoztassa a tooData**, a **adatbázisok**, kattintson a **beolvasása**.</span><span class="sxs-lookup"><span data-stu-id="199cf-156">On hello **Get Data** page, under **Import or Connect tooData**, for **Databases**, click **Get**.</span></span>

    <span data-ttu-id="199cf-157">![Adatok beolvasása a Power bi-bA az Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-import-data-power-bi.png "adatok beszerzése Apache Spark BI Power BI-bA")</span><span class="sxs-lookup"><span data-stu-id="199cf-157">![Get data into Power BI for Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-import-data-power-bi.png "Get data into Power BI for Apache Spark BI")</span></span>

5. <span data-ttu-id="199cf-158">Hello következő képernyőn kattintson a **a Spark on Azure HDInsight** majd **Connect**.</span><span class="sxs-lookup"><span data-stu-id="199cf-158">On hello next screen, click **Spark on Azure HDInsight** and then click **Connect**.</span></span> <span data-ttu-id="199cf-159">Amikor a rendszer kéri, adja meg a hello fürt URL-CÍMÉT (`mysparkcluster.azurehdinsight.net`) és hello hitelesítő adatok tooconnect toohello fürt.</span><span class="sxs-lookup"><span data-stu-id="199cf-159">When prompted, enter hello cluster URL (`mysparkcluster.azurehdinsight.net`) and hello credentials tooconnect toohello cluster.</span></span>

    <span data-ttu-id="199cf-160">![Csatlakozás tooApache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/connect-to-apache-spark-bi.png "tooApache Spark BI csatlakozás")</span><span class="sxs-lookup"><span data-stu-id="199cf-160">![Connect tooApache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/connect-to-apache-spark-bi.png "Connect tooApache Spark BI")</span></span>

    <span data-ttu-id="199cf-161">Hello kapcsolat létrejötte után a Power BI adatok importálását a HDInsight Spark-fürt hello kezdődik.</span><span class="sxs-lookup"><span data-stu-id="199cf-161">After hello connection is established, Power BI starts importing data from hello Spark cluster on HDInsight.</span></span>

6. <span data-ttu-id="199cf-162">A Power BI hello adatok importálására, és hozzáadja a **Spark** adatkészlet alapján hello **adatkészletek** fejléc.</span><span class="sxs-lookup"><span data-stu-id="199cf-162">Power BI imports hello data and adds a **Spark** dataset under hello **Datasets** heading.</span></span> <span data-ttu-id="199cf-163">Kattintson a hello adatkészlet tooopen egy új munkalapra toovisualize hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="199cf-163">Click hello data set tooopen a new worksheet toovisualize hello data.</span></span> <span data-ttu-id="199cf-164">Hello munkalap jelentést is mentheti.</span><span class="sxs-lookup"><span data-stu-id="199cf-164">You can also save hello worksheet as a report.</span></span> <span data-ttu-id="199cf-165">a munkalapra, hello toosave **fájl** menüben kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="199cf-165">toosave a worksheet, from hello **File** menu, click **Save**.</span></span>

    <span data-ttu-id="199cf-166">![A Power BI irányítópult Apache Spark BI csempéjén](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-tile-dashboard.png "Apache Spark BI csempét a Power BI-irányítópulton")</span><span class="sxs-lookup"><span data-stu-id="199cf-166">![Apache Spark BI tile on Power BI dashboard](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-tile-dashboard.png "Apache Spark BI tile on Power BI dashboard")</span></span>
7. <span data-ttu-id="199cf-167">Figyelje meg, hogy hello **mezők** hello lista meg hello jobb **hvac** korábban létrehozott tábla.</span><span class="sxs-lookup"><span data-stu-id="199cf-167">Notice that hello **Fields** list on hello right lists hello **hvac** table you created earlier.</span></span> <span data-ttu-id="199cf-168">Hello tábla toosee hello mezők hello tábla, bontsa ki a korábban meghatározott jegyzetfüzet.</span><span class="sxs-lookup"><span data-stu-id="199cf-168">Expand hello table toosee hello fields in hello table, as you defined in notebook earlier.</span></span>

      <span data-ttu-id="199cf-169">![Apache Spark BI irányítópulton lévő táblák listázása](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-display-tables.png "Apache Spark BI irányítópulton lévő táblák listázása")</span><span class="sxs-lookup"><span data-stu-id="199cf-169">![List tables on Apache Spark BI dashboard](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-display-tables.png "List tables on Apache Spark BI dashboard")</span></span>

8. <span data-ttu-id="199cf-170">Build a képi megjelenítés tooshow hello eltérés cél hőmérséklet és az egyes épület tényleges hőmérséklet között.</span><span class="sxs-lookup"><span data-stu-id="199cf-170">Build a visualization tooshow hello variance between target temperature and actual temperature for each building.</span></span> <span data-ttu-id="199cf-171">toovisualize regisztrációhoz adatokat, válassza ki **területdiagram** (vörös téglalappal látható).</span><span class="sxs-lookup"><span data-stu-id="199cf-171">toovisualize yoru data, select **Area Chart** (shown in red box).</span></span> <span data-ttu-id="199cf-172">toodefine hello tengely fogd és vidd hello **BuildingID** eleménél **tengely**, és **ActualTemp**/**TargetTemp** a mezők **érték**.</span><span class="sxs-lookup"><span data-stu-id="199cf-172">toodefine hello axis, drag-and-drop hello **BuildingID** field under **Axis**, and **ActualTemp**/**TargetTemp** fields under **Value**.</span></span>

    <span data-ttu-id="199cf-173">![Hozzon létre a Spark Apache Spark BI használatával adatmegjelenítésekkel](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-add-value-columns.png "Spark létrehozása az adatmegjelenítéseket Apache Spark BI használatával")</span><span class="sxs-lookup"><span data-stu-id="199cf-173">![Create Spark data visualizations using Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-add-value-columns.png "Create Spark data visualizations using Apache Spark BI")</span></span>

9. <span data-ttu-id="199cf-174">Alapértelmezés szerint hello képi megjelenítés mutatja hello összegük **ActualTemp** és **TargetTemp**.</span><span class="sxs-lookup"><span data-stu-id="199cf-174">By default hello visualization shows hello sum for **ActualTemp** and **TargetTemp**.</span></span> <span data-ttu-id="199cf-175">Mindkét hello mezőit hello legördülő menüben válasszon **átlagos** tooget tényleges átlagosan és a cél-hőmérsékletek mindkét épületek.</span><span class="sxs-lookup"><span data-stu-id="199cf-175">For both hello fields, from hello drop-down, select **Average** tooget an average of actual and target temperatures for both buildings.</span></span>

    <span data-ttu-id="199cf-176">![Hozzon létre a Spark Apache Spark BI használatával adatmegjelenítésekkel](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-average-of-values.png "Spark létrehozása az adatmegjelenítéseket Apache Spark BI használatával")</span><span class="sxs-lookup"><span data-stu-id="199cf-176">![Create Spark data visualizations using Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-average-of-values.png "Create Spark data visualizations using Apache Spark BI")</span></span>

10. <span data-ttu-id="199cf-177">Az adatok vizuális hasonló toohello egy hello a képernyőfelvételen látható kell lennie.</span><span class="sxs-lookup"><span data-stu-id="199cf-177">Your data visualization should be similar toohello one in hello screenshot.</span></span> <span data-ttu-id="199cf-178">Vigye a kurzort hello képi megjelenítés tooget eszköztipp vonatkozó adatokat tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="199cf-178">Move your cursor over hello visualization tooget tool tips with relevant data.</span></span>

    <span data-ttu-id="199cf-179">![Hozzon létre a Spark Apache Spark BI használatával adatmegjelenítésekkel](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-area-graph.png "Spark létrehozása az adatmegjelenítéseket Apache Spark BI használatával")</span><span class="sxs-lookup"><span data-stu-id="199cf-179">![Create Spark data visualizations using Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-area-graph.png "Create Spark data visualizations using Apache Spark BI")</span></span>

11. <span data-ttu-id="199cf-180">Kattintson a **mentése** hello a felső menüben, és adja meg a jelentés nevét.</span><span class="sxs-lookup"><span data-stu-id="199cf-180">Click **Save** from hello top menu and provide a report name.</span></span> <span data-ttu-id="199cf-181">Hello visual is rögzítheti.</span><span class="sxs-lookup"><span data-stu-id="199cf-181">You can also pin hello visual.</span></span> <span data-ttu-id="199cf-182">Képi megjelenítés rögzíti, amikor tárolása az irányítópulton, hello legutolsó értékét egy pillanat alatt követheti nyomon.</span><span class="sxs-lookup"><span data-stu-id="199cf-182">When you pin a visualization, it is stored on your dashboard so you can track hello latest value at a glance.</span></span>

   <span data-ttu-id="199cf-183">A kívánt annyi képi megjelenítések is hozzáadhat ugyanahhoz az adatkészlethez hello és az adatok pillanatképe toohello irányítópulthoz rögzítheti őket.</span><span class="sxs-lookup"><span data-stu-id="199cf-183">You can add as many visualizations as you want for hello same dataset and pin them toohello dashboard for a snapshot of your data.</span></span> <span data-ttu-id="199cf-184">A HDInsight Spark-fürtjei is BI közvetlen kapcsolódás csatlakoztatott tooPower.</span><span class="sxs-lookup"><span data-stu-id="199cf-184">Also, Spark clusters on HDInsight are connected tooPower BI with direct connect.</span></span> <span data-ttu-id="199cf-185">Ez biztosítja, hogy a Power BI mindig rendelkezik hello legtöbb naprakész adatokat a fürt így tooschedule frissítések hello az adatkészlethez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="199cf-185">This ensures that Power BI always has hello most up-to-date data from your cluster so you do not need tooschedule refreshes for hello dataset.</span></span>

## <span data-ttu-id="199cf-186"><a name="tableau"></a>A Spark adatábrázolási Tableau asztal használata</span><span class="sxs-lookup"><span data-stu-id="199cf-186"><a name="tableau"></a>Use Tableau Desktop for Spark data visualization</span></span>

> [!NOTE]
> <span data-ttu-id="199cf-187">Ez a szakasz esetén csak az Azure HDInsight létrehozott Spark 1.5.2-fürtök alkalmazható.</span><span class="sxs-lookup"><span data-stu-id="199cf-187">This section is applicable only for Spark 1.5.2 clusters created in Azure HDInsight.</span></span>
>
>

1. <span data-ttu-id="199cf-188">Telepítés [Tableau asztali](http://www.tableau.com/products/desktop) hello az Apache Spark BI oktatóanyag futtató számítógépen.</span><span class="sxs-lookup"><span data-stu-id="199cf-188">Install [Tableau Desktop](http://www.tableau.com/products/desktop) on hello computer where you are running this Apache Spark BI tutorial.</span></span>

2. <span data-ttu-id="199cf-189">Győződjön meg arról, hogy számítógépen is van telepítve a Microsoft Spark ODBC-illesztőprogram.</span><span class="sxs-lookup"><span data-stu-id="199cf-189">Make sure that computer also has Microsoft Spark ODBC driver installed.</span></span> <span data-ttu-id="199cf-190">A hello illesztőprogram telepítése [Itt](http://go.microsoft.com/fwlink/?LinkId=616229).</span><span class="sxs-lookup"><span data-stu-id="199cf-190">You can install hello driver from [here](http://go.microsoft.com/fwlink/?LinkId=616229).</span></span>

1. <span data-ttu-id="199cf-191">Tableau asztal indításakor.</span><span class="sxs-lookup"><span data-stu-id="199cf-191">Launch Tableau Desktop.</span></span> <span data-ttu-id="199cf-192">Hello bal oldali ablaktáblán, a kiszolgáló tooconnect hello listája kattintson **Spark SQL**.</span><span class="sxs-lookup"><span data-stu-id="199cf-192">In hello left pane, from hello list of server tooconnect to, click **Spark SQL**.</span></span> <span data-ttu-id="199cf-193">Ha hello bal oldali ablaktáblán alapértelmezés szerint nem szerepel a Spark SQL, megtalálhatja azt kattintson **több kiszolgálók**.</span><span class="sxs-lookup"><span data-stu-id="199cf-193">If Spark SQL is not listed by default in hello left pane, you can find it by click **More Servers**.</span></span>
2. <span data-ttu-id="199cf-194">A hello Spark SQL kapcsolati párbeszédpanelen adja meg hello hello képernyőfelvételen látható módon, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="199cf-194">In hello Spark SQL connection dialog box, provide hello values as shown in hello screenshot, and then click **OK**.</span></span>

    <span data-ttu-id="199cf-195">![Csatlakozás tooa fürt az Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/connect-to-tableau-apache-spark-bi.png "Connect tooa fürt az Apache Spark BI")</span><span class="sxs-lookup"><span data-stu-id="199cf-195">![Connect tooa cluster for Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/connect-to-tableau-apache-spark-bi.png "Connect tooa cluster for Apache Spark BI")</span></span>

    <span data-ttu-id="199cf-196">hitelesítési legördülő listákból hello **Microsoft Azure HDInsight szolgáltatás** lehetőség csak akkor, ha telepítette a hello [Microsoft Spark ODBC-illesztőprogram](http://go.microsoft.com/fwlink/?LinkId=616229) hello számítógépen.</span><span class="sxs-lookup"><span data-stu-id="199cf-196">hello authentication drop-down lists **Microsoft Azure HDInsight Service** as an option, only if you installed hello [Microsoft Spark ODBC Driver](http://go.microsoft.com/fwlink/?LinkId=616229) on hello computer.</span></span>
3. <span data-ttu-id="199cf-197">Hello következő képernyőn, a hello **séma** legördülő menüben kattintson a hello **található** ikonra, végül **alapértelmezett**.</span><span class="sxs-lookup"><span data-stu-id="199cf-197">On hello next screen, from hello **Schema** drop-down, click hello **Find** icon, and then click **default**.</span></span>

    <span data-ttu-id="199cf-198">![Található séma a következő Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/tableau-find-schema-apache-spark-bi.png "Apache Spark BI keresés sémája")</span><span class="sxs-lookup"><span data-stu-id="199cf-198">![Find schema for Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/tableau-find-schema-apache-spark-bi.png "Find schema for Apache Spark BI")</span></span>
4. <span data-ttu-id="199cf-199">A hello **tábla** , majd kattintson a hello **található** ikon újra összes hello toolist Hive táblák hello fürt érhető el.</span><span class="sxs-lookup"><span data-stu-id="199cf-199">For hello **Table** field, click hello **Find** icon again toolist all hello Hive tables available in hello cluster.</span></span> <span data-ttu-id="199cf-200">Megtekintheti az hello **hvac** , korábban létrehozott hello notebook tábla.</span><span class="sxs-lookup"><span data-stu-id="199cf-200">You should see hello **hvac** table you created earlier using hello notebook.</span></span>

    <span data-ttu-id="199cf-201">![Az Apache Spark BI tábla található](./media/hdinsight-apache-spark-use-bi-tools/tableau-find-table-apache-spark-bi.png "Apache Spark bi keresési tábla")</span><span class="sxs-lookup"><span data-stu-id="199cf-201">![Find table for Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/tableau-find-table-apache-spark-bi.png "Find table for Apache Spark BI")</span></span>
5. <span data-ttu-id="199cf-202">Áthúzása hello tábla toohello felső mezőbe a megfelelő hello.</span><span class="sxs-lookup"><span data-stu-id="199cf-202">Drag and drop hello table toohello top box on hello right.</span></span> <span data-ttu-id="199cf-203">Tableau hello adatok importálására és hello séma példának piros hello mező alapján jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="199cf-203">Tableau imports hello data and displays hello schema as highlighted by hello red box.</span></span>

    <span data-ttu-id="199cf-204">![Adja hozzá a táblák tooTableau Apache Spark bi](./media/hdinsight-apache-spark-use-bi-tools/tableau-add-table-apache-spark-bi.png "Apache Spark bi táblák tooTableau hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="199cf-204">![Add tables tooTableau for Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/tableau-add-table-apache-spark-bi.png "Add tables tooTableau for Apache Spark BI")</span></span>
6. <span data-ttu-id="199cf-205">Kattintson a hello **Munka1** hello bal alsó lapot.</span><span class="sxs-lookup"><span data-stu-id="199cf-205">Click hello **Sheet1** tab at hello bottom left.</span></span> <span data-ttu-id="199cf-206">Ellenőrizze a képi megjelenítés hello átlagos cél- és a tényleges összes épület-hőmérsékletek jelennek meg. minden egyes dátum.</span><span class="sxs-lookup"><span data-stu-id="199cf-206">Make a visualization that shows hello average target and actual temperatures for all buildings for each date.</span></span> <span data-ttu-id="199cf-207">A csomóponthúzási **dátum** és **épület azonosító** túl**oszlopok** és **tényleges Temp**/**cél Temp**túl**sorok**.</span><span class="sxs-lookup"><span data-stu-id="199cf-207">Drag **Date** and **Building ID** too**Columns** and **Actual Temp**/**Target Temp** too**Rows**.</span></span> <span data-ttu-id="199cf-208">A **jelek**, jelölje be **terület** toouse Spark adatábrázolási terület interaktív.</span><span class="sxs-lookup"><span data-stu-id="199cf-208">Under **Marks**, select **Area** toouse an area map for Spark data visualization.</span></span>

     <span data-ttu-id="199cf-209">![Adja hozzá a mezőket a Spark adatábrázolási](./media/hdinsight-apache-spark-use-bi-tools/spark-data-visualization-add-fields.png "Spark adatábrázolási mezők hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="199cf-209">![Add fields for Spark data visualization](./media/hdinsight-apache-spark-use-bi-tools/spark-data-visualization-add-fields.png "Add fields for Spark data visualization")</span></span>
7. <span data-ttu-id="199cf-210">Alapértelmezés szerint megjelenítendő hello hőmérséklet mezők szerint összesítést.</span><span class="sxs-lookup"><span data-stu-id="199cf-210">By default, hello temperature fields are shown as aggregate.</span></span> <span data-ttu-id="199cf-211">Ha inkább tooshow hello átlaghőmérséklet, akkor teheti hello legördülő, az alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="199cf-211">If you want tooshow hello average temperatures instead, you can do so from hello drop-down, as shown below.</span></span>

    <span data-ttu-id="199cf-212">![Átlagos Spark adatok vizuális megjelenítéshez tartozó hőmérséklet érvénybe](./media/hdinsight-apache-spark-use-bi-tools/spark-data-visualization-average-temperature.png "átlagos Spark adatok vizuális megjelenítéshez tartozó hőmérséklet igénybe")</span><span class="sxs-lookup"><span data-stu-id="199cf-212">![Take average of temperature for Spark data visualization](./media/hdinsight-apache-spark-use-bi-tools/spark-data-visualization-average-temperature.png "Take average of temperature for Spark data visualization")</span></span>

8. <span data-ttu-id="199cf-213">Akkor is Szuper-adhat egy hőmérséklet-leképezést több mint hello más tooget jobb arculatának cél és a tényleges hőmérsékletek közötti különbség.</span><span class="sxs-lookup"><span data-stu-id="199cf-213">You can also super-impose one temperature map over hello other tooget a better feel of difference between target and actual temperatures.</span></span> <span data-ttu-id="199cf-214">Helyezze át a hello egér toohello sarkában hello alacsonyabb terület térkép keretein belül hello leíró alakzat kiemelt piros kör látható.</span><span class="sxs-lookup"><span data-stu-id="199cf-214">Move hello mouse toohello corner of hello lower area map till you see hello handle shape highlighted in a red circle.</span></span> <span data-ttu-id="199cf-215">Húzza hello térkép toohello más térkép hello top és a kiadási hello egér piros téglalap kiemelt hello alakzat megjelenésekor.</span><span class="sxs-lookup"><span data-stu-id="199cf-215">Drag hello map toohello other map on hello top and release hello mouse when you see hello shape highlighted in red rectangle.</span></span>

    <span data-ttu-id="199cf-216">![A Spark adatábrázolási maps egyesítése](./media/hdinsight-apache-spark-use-bi-tools/spark-data-visualization-merge-maps.png "egyesítési leképezi a Spark adatábrázolási")</span><span class="sxs-lookup"><span data-stu-id="199cf-216">![Merge maps for Spark data visualization](./media/hdinsight-apache-spark-use-bi-tools/spark-data-visualization-merge-maps.png "Merge maps for Spark data visualization")</span></span>

     <span data-ttu-id="199cf-217">Az adatok vizuális hello képernyőfelvételen látható módon kell módosítani:</span><span class="sxs-lookup"><span data-stu-id="199cf-217">Your data visualization should change as shown in hello screenshot:</span></span>

    <span data-ttu-id="199cf-218">![A Spark adatábrázolási tableau kimeneti](./media/hdinsight-apache-spark-use-bi-tools/spark-data-visualization-tableau-output.png "Spark adatábrázolási Tableau kimenete")</span><span class="sxs-lookup"><span data-stu-id="199cf-218">![Tableau output for Spark data visualization](./media/hdinsight-apache-spark-use-bi-tools/spark-data-visualization-tableau-output.png "Tableau output for Spark data visualization")</span></span>
9. <span data-ttu-id="199cf-219">Kattintson a **mentése** toosave hello munkalapra.</span><span class="sxs-lookup"><span data-stu-id="199cf-219">Click **Save** toosave hello worksheet.</span></span> <span data-ttu-id="199cf-220">Irányítópultok létrehozása, és adja hozzá egy vagy több lapok tooit.</span><span class="sxs-lookup"><span data-stu-id="199cf-220">You can create dashboards and add one or more sheets tooit.</span></span>

## <a name="next-steps"></a><span data-ttu-id="199cf-221">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="199cf-221">Next steps</span></span>

<span data-ttu-id="199cf-222">Amennyiben megtanulta, hogyan toocreate egy fürtbe, Spark adatok keretek tooquery adatok létrehozása, és majd érheti el az adatok Üzletiintelligencia-eszközök.</span><span class="sxs-lookup"><span data-stu-id="199cf-222">So far you learned how toocreate a cluster, create Spark data frames tooquery data, and then access that data from BI tools.</span></span> <span data-ttu-id="199cf-223">Most már megtekintheti toomanage fürterőforrások hello és a HDInsight Spark-fürtben lévő éppen futó feladatok hibakeresési utasításokat.</span><span class="sxs-lookup"><span data-stu-id="199cf-223">You can now look at instructions on how toomanage hello cluster resources and debug jobs that are running in an HDInsight Spark cluster.</span></span>

* [<span data-ttu-id="199cf-224">Az Azure HDInsight hello Apache Spark-fürt erőforrásainak kezelése</span><span class="sxs-lookup"><span data-stu-id="199cf-224">Manage resources for hello Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="199cf-225">Apache Spark-fürtön futó feladatok nyomon követése és hibakeresése a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="199cf-225">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)

