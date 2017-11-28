---
title: "A Spark - Azure Python könyvtárak webhelyek naplóinak elemzése |} Microsoft Docs"
description: "A notebook adatelemzés napló segítségével egyéni tárát a Spark on Azure Hdinsighttal mutatja be."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 8c61c70f-fe7f-4f0f-a4ab-0cccee5668c9
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 2a028beb1e9eae89d32238e61b6f67c4c059a94a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="analyze-website-logs-using-a-custom-python-library-with-spark-cluster-on-hdinsight"></a><span data-ttu-id="620fc-103">Egy egyéni Python kódtár használata a HDInsight Spark-fürt webhelyek naplóinak elemzése</span><span class="sxs-lookup"><span data-stu-id="620fc-103">Analyze website logs using a custom Python library with Spark cluster on HDInsight</span></span>

<span data-ttu-id="620fc-104">A notebook adatelemzés napló segítségével egyéni tárát a Spark on HDInsight mutatja be.</span><span class="sxs-lookup"><span data-stu-id="620fc-104">This notebook demonstrates how to analyze log data using a custom library with Spark on HDInsight.</span></span> <span data-ttu-id="620fc-105">Az egyéni könyvtár használjuk egy Python kódtár nevű **iislogparser.py**.</span><span class="sxs-lookup"><span data-stu-id="620fc-105">The custom library we use is a Python library called **iislogparser.py**.</span></span>

> [!TIP]
> <span data-ttu-id="620fc-106">Ez az oktatóanyag, Ön által létrehozott hdinsight (Linux) Spark-fürtön Jupyter notebook is érhető el.</span><span class="sxs-lookup"><span data-stu-id="620fc-106">This tutorial is also available as a Jupyter notebook on a Spark (Linux) cluster that you create in HDInsight.</span></span> <span data-ttu-id="620fc-107">A notebook élmény lehetővé teszi a Python kódtöredékek futtassa a notebook magát.</span><span class="sxs-lookup"><span data-stu-id="620fc-107">The notebook experience lets you run the Python snippets from the notebook itself.</span></span> <span data-ttu-id="620fc-108">A notebook belül az oktatóprogram elvégzéséhez Spark-fürt létrehozása, indítsa el a Jupyter notebook (`https://CLUSTERNAME.azurehdinsight.net/jupyter`), és futtassa a notebook **naplóinak elemzése a Spark egy egyéni library.ipynb használatával** alatt a **PySpark**  mappa.</span><span class="sxs-lookup"><span data-stu-id="620fc-108">To perform the tutorial from within a notebook, create a Spark cluster, launch a Jupyter notebook (`https://CLUSTERNAME.azurehdinsight.net/jupyter`), and then run the notebook **Analyze logs with Spark using a custom library.ipynb** under the **PySpark** folder.</span></span>
>
>

<span data-ttu-id="620fc-109">**Előfeltételek:**</span><span class="sxs-lookup"><span data-stu-id="620fc-109">**Prerequisites:**</span></span>

<span data-ttu-id="620fc-110">Az alábbiakkal kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="620fc-110">You must have the following:</span></span>

* <span data-ttu-id="620fc-111">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="620fc-111">An Azure subscription.</span></span> <span data-ttu-id="620fc-112">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="620fc-112">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>

* <span data-ttu-id="620fc-113">A HDInsight az Apache Spark-fürt.</span><span class="sxs-lookup"><span data-stu-id="620fc-113">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="620fc-114">Útmutatásért lásd: [létrehozása az Apache Spark on Azure hdinsight clusters](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="620fc-114">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>

## <a name="save-raw-data-as-an-rdd"></a><span data-ttu-id="620fc-115">Nyers adatok elmentse egy RDD</span><span class="sxs-lookup"><span data-stu-id="620fc-115">Save raw data as an RDD</span></span>
<span data-ttu-id="620fc-116">Ebben a szakaszban használjuk a [Jupyter](https://jupyter.org) notebook társított Apache Spark-fürt a Hdinsightban, amely a nyers mintaadatok feldolgozni, és mentse egy Hive tábla feladatok futtatásához.</span><span class="sxs-lookup"><span data-stu-id="620fc-116">In this section, we use the [Jupyter](https://jupyter.org) notebook associated with an Apache Spark cluster in HDInsight to run jobs that process your raw sample data and save it as a Hive table.</span></span> <span data-ttu-id="620fc-117">A mintaadatok egy CSV-fájlt (hvac.csv) elérhető alapértelmezés szerint minden fürtön.</span><span class="sxs-lookup"><span data-stu-id="620fc-117">The sample data is a .csv file (hvac.csv) available on all clusters by default.</span></span>

<span data-ttu-id="620fc-118">Miután az adatok mentése Hive tábla a következő szakaszban nem fog csatlakozni a Hive tábla a Power BI és a Tableau Üzletiintelligencia-eszközök.</span><span class="sxs-lookup"><span data-stu-id="620fc-118">Once your data is saved as a Hive table, in the next section we will connect to the Hive table using BI tools such as Power BI and Tableau.</span></span>

1. <span data-ttu-id="620fc-119">Az [Azure portál](https://portal.azure.com/) kezdőpultján kattintson a Spark-fürthöz tartozó csempére (ha rögzítette azt a kezdőpulton).</span><span class="sxs-lookup"><span data-stu-id="620fc-119">From the [Azure portal](https://portal.azure.com/), from the startboard, click the tile for your Spark cluster (if you pinned it to the startboard).</span></span> <span data-ttu-id="620fc-120">A fürtöt a következő helyről is megkeresheti: **Browse All (Összes tallózása)** > **HDInsight Clusters** (HDInsight-fürtök).</span><span class="sxs-lookup"><span data-stu-id="620fc-120">You can also navigate to your cluster under **Browse All** > **HDInsight Clusters**.</span></span>   
2. <span data-ttu-id="620fc-121">A Spark-fürt panelén kattintson a **Fürt irányítópultja Dashboard** lehetőségre, majd a **Jupyter Notebook** elemre.</span><span class="sxs-lookup"><span data-stu-id="620fc-121">From the Spark cluster blade, click **Cluster Dashboard**, and then click **Jupyter Notebook**.</span></span> <span data-ttu-id="620fc-122">Ha a rendszer felkéri rá, adja meg a fürthöz tartozó rendszergazdai hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="620fc-122">If prompted, enter the admin credentials for the cluster.</span></span>

   > [!NOTE]
   > <span data-ttu-id="620fc-123">A fürthöz tartozó Jupyter notebookot az alábbi URL-cím böngészőben történő megnyitásával is elérheti.</span><span class="sxs-lookup"><span data-stu-id="620fc-123">You may also reach the Jupyter Notebook for your cluster by opening the following URL in your browser.</span></span> <span data-ttu-id="620fc-124">Cserélje le a **CLUSTERNAME** elemet a fürt nevére:</span><span class="sxs-lookup"><span data-stu-id="620fc-124">Replace **CLUSTERNAME** with the name of your cluster:</span></span>
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >
3. <span data-ttu-id="620fc-125">Hozzon létre új notebookot.</span><span class="sxs-lookup"><span data-stu-id="620fc-125">Create a new notebook.</span></span> <span data-ttu-id="620fc-126">Kattintson a **New** (Új), majd a **PySpark** elemre.</span><span class="sxs-lookup"><span data-stu-id="620fc-126">Click **New**, and then click **PySpark**.</span></span>

    <span data-ttu-id="620fc-127">![Új Jupyter notebook létrehozása](./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdinsight-create-jupyter-notebook.png "Új Jupyter notebook létrehozása")</span><span class="sxs-lookup"><span data-stu-id="620fc-127">![Create a new Jupyter notebook](./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdinsight-create-jupyter-notebook.png "Create a new Jupyter notebook")</span></span>
4. <span data-ttu-id="620fc-128">Az új notebook létrejött, és Untitled.pynb néven nyílt meg.</span><span class="sxs-lookup"><span data-stu-id="620fc-128">A new notebook is created and opened with the name Untitled.pynb.</span></span> <span data-ttu-id="620fc-129">A felső részen kattintson a notebook nevére, és adjon meg egy könnyen megjegyezhető nevet.</span><span class="sxs-lookup"><span data-stu-id="620fc-129">Click the notebook name at the top, and enter a friendly name.</span></span>

    <span data-ttu-id="620fc-130">![Adjon nevet a notebooknak](./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdinsight-name-jupyter-notebook.png "Adjon nevet a notebooknak")</span><span class="sxs-lookup"><span data-stu-id="620fc-130">![Provide a name for the notebook](./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdinsight-name-jupyter-notebook.png "Provide a name for the notebook")</span></span>
5. <span data-ttu-id="620fc-131">Mivel a notebook PySpark kernel használatával jött létre, explicit módon semmilyen tartalmat nem kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="620fc-131">Because you created a notebook using the PySpark kernel, you do not need to create any contexts explicitly.</span></span> <span data-ttu-id="620fc-132">Az első kódcella futtatásakor a Spark- és Hive-környezetek automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="620fc-132">The Spark and Hive contexts will be automatically created for you when you run the first code cell.</span></span> <span data-ttu-id="620fc-133">Ehhez a forgatókönyvhöz szükséges típusok importálása megkezdése.</span><span class="sxs-lookup"><span data-stu-id="620fc-133">You can start by importing the types that are required for this scenario.</span></span> <span data-ttu-id="620fc-134">Illessze be a következő kódrészletet a cella üres, és nyomja le az **SHIFT + ENTER**.</span><span class="sxs-lookup"><span data-stu-id="620fc-134">Paste the following snippet in an empty cell, and then press **SHIFT + ENTER**.</span></span>

        from pyspark.sql import Row
        from pyspark.sql.types import *


1. <span data-ttu-id="620fc-135">Hozzon létre egy RDD használatával a napló mintaadatok már elérhető a fürtön.</span><span class="sxs-lookup"><span data-stu-id="620fc-135">Create an RDD using the sample log data already available on the cluster.</span></span> <span data-ttu-id="620fc-136">Végezheti el az adatokat a fürthöz tartozó alapértelmezett tárfiókban **\HdiSamples\HdiSamples\WebsiteLogSampleData\SampleLog\909f2b.log**.</span><span class="sxs-lookup"><span data-stu-id="620fc-136">You can access the data in the default storage account associated with the cluster at **\HdiSamples\HdiSamples\WebsiteLogSampleData\SampleLog\909f2b.log**.</span></span>

        logs = sc.textFile('wasb:///HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b.log')


1. <span data-ttu-id="620fc-137">Egy mintanaplót, ellenőrizze, hogy beállítása sikeresen befejeződött az előző lépésben beolvasása.</span><span class="sxs-lookup"><span data-stu-id="620fc-137">Retrieve a sample log set to verify that the previous step completed successfully.</span></span>

        logs.take(5)

    <span data-ttu-id="620fc-138">A következőhöz hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="620fc-138">You should see an output similar to the following:</span></span>

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [u'#Software: Microsoft Internet Information Services 8.0',
         u'#Fields: date time s-sitename cs-method cs-uri-stem cs-uri-query s-port cs-username c-ip cs(User-Agent) cs(Cookie) cs(Referer) cs-host sc-status sc-substatus sc-win32-status sc-bytes cs-bytes time-taken',
         u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step2.png X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 53175 871 46',
         u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step3.png X-ARR-LOG-ID=9eace870-2f49-4efd-b204-0d170da46b4a 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 51237 871 32',
         u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step4.png X-ARR-LOG-ID=4bea5b3d-8ac9-46c9-9b8c-ec3e9500cbea 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 72177 871 47']

## <a name="analyze-log-data-using-a-custom-python-library"></a><span data-ttu-id="620fc-139">Egy egyéni Python kódtár használatával napló adatok elemzése</span><span class="sxs-lookup"><span data-stu-id="620fc-139">Analyze log data using a custom Python library</span></span>
1. <span data-ttu-id="620fc-140">A fenti kimenetben az első néhány sor tartalmazza a fejléc-információ, és minden fennmaradó sor megfelel az adott fejléc ismertetett séma.</span><span class="sxs-lookup"><span data-stu-id="620fc-140">In the output above, the first couple lines include the header information and each remaining line matches the schema described in that header.</span></span> <span data-ttu-id="620fc-141">Ilyen naplók elemzése bonyolult lehet.</span><span class="sxs-lookup"><span data-stu-id="620fc-141">Parsing such logs could be complicated.</span></span> <span data-ttu-id="620fc-142">Igen, egy egyéni Python kódtár használjuk (**iislogparser.py**), amely lehetővé teszi az ilyen sokkal könnyebben naplók elemzése.</span><span class="sxs-lookup"><span data-stu-id="620fc-142">So, we use a custom Python library (**iislogparser.py**) that makes parsing such logs much easier.</span></span> <span data-ttu-id="620fc-143">Alapértelmezés szerint ezt a szalagtárat megtalálható a következő hdinsight Spark-fürt **/HdiSamples/HdiSamples/WebsiteLogSampleData/iislogparser.py**.</span><span class="sxs-lookup"><span data-stu-id="620fc-143">By default, this library is included with your Spark cluster on HDInsight at **/HdiSamples/HdiSamples/WebsiteLogSampleData/iislogparser.py**.</span></span>

    <span data-ttu-id="620fc-144">Nincs a azonban ezt a szalagtárat a `PYTHONPATH` , nem tudjuk használni, például import utasítás segítségével `import iislogparser`.</span><span class="sxs-lookup"><span data-stu-id="620fc-144">However, this library is not in the `PYTHONPATH` so we cannot use it by using an import statement like `import iislogparser`.</span></span> <span data-ttu-id="620fc-145">Szeretné használni ezt a szalagtárat, azt kell terjeszteni az összes munkavégző csomópontokhoz.</span><span class="sxs-lookup"><span data-stu-id="620fc-145">To use this library, we must distribute it to all the worker nodes.</span></span> <span data-ttu-id="620fc-146">Futtassa a következő kódrészletet.</span><span class="sxs-lookup"><span data-stu-id="620fc-146">Run the following snippet.</span></span>

        sc.addPyFile('wasb:///HdiSamples/HdiSamples/WebsiteLogSampleData/iislogparser.py')


1. <span data-ttu-id="620fc-147">`iislogparser`funkció `parse_log_line` , amely visszaadja a `None` Ha egy napló sor a fejlécsor lesz, és egy példányát adja vissza a `LogLine` osztály, ha egy napló sor ütközik.</span><span class="sxs-lookup"><span data-stu-id="620fc-147">`iislogparser` provides a function `parse_log_line` that returns `None` if a log line is a header row, and returns an instance of the `LogLine` class if it encounters a log line.</span></span> <span data-ttu-id="620fc-148">Használja a `LogLine` osztály a napló csak a Sorok kinyerése a RDD:</span><span class="sxs-lookup"><span data-stu-id="620fc-148">Use the `LogLine` class to extract only the log lines from the RDD:</span></span>

        def parse_line(l):
            import iislogparser
            return iislogparser.parse_log_line(l)
        logLines = logs.map(parse_line).filter(lambda p: p is not None).cache()
2. <span data-ttu-id="620fc-149">Néhány kibontott napló sorral annak ellenőrzésére, hogy sikeresen befejeződött. a lépés beolvasása.</span><span class="sxs-lookup"><span data-stu-id="620fc-149">Retrieve a couple of extracted log lines to verify that the step completed successfully.</span></span>

       logLines.take(2)

   <span data-ttu-id="620fc-150">A kimenet az alábbihoz hasonló lesz:</span><span class="sxs-lookup"><span data-stu-id="620fc-150">The output should be similar to the following:</span></span>

       # -----------------
       # THIS IS AN OUTPUT
       # -----------------

       [2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step2.png X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 53175 871 46,
        2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step3.png X-ARR-LOG-ID=9eace870-2f49-4efd-b204-0d170da46b4a 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 51237 871 32]
3. <span data-ttu-id="620fc-151">A `LogLine` osztály, viszont az például néhány hasznos metódusokkal rendelkezik `is_error()`, amely adja vissza, hogy rendelkezik-e a naplóbejegyzés hibakódot.</span><span class="sxs-lookup"><span data-stu-id="620fc-151">The `LogLine` class, in turn, has some useful methods, like `is_error()`, which returns whether a log entry has an error code.</span></span> <span data-ttu-id="620fc-152">Használja ezt a kibontott napló sorokat fellépett hibák száma számítási, és jelentkezzen be a hibák egy másik fájlba.</span><span class="sxs-lookup"><span data-stu-id="620fc-152">Use this to compute the number of errors in the extracted log lines, and then log all the errors to a different file.</span></span>

       errors = logLines.filter(lambda p: p.is_error())
       numLines = logLines.count()
       numErrors = errors.count()
       print 'There are', numErrors, 'errors and', numLines, 'log entries'
       errors.map(lambda p: str(p)).saveAsTextFile('wasb:///HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b-2.log')

   <span data-ttu-id="620fc-153">A következőhöz hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="620fc-153">You should see an output like the following:</span></span>

       # -----------------
       # THIS IS AN OUTPUT
       # -----------------

       There are 30 errors and 646 log entries
4. <span data-ttu-id="620fc-154">Is **Matplotlib** képi megjelenítés az adatok összeállításához.</span><span class="sxs-lookup"><span data-stu-id="620fc-154">You can also use **Matplotlib** to construct a visualization of the data.</span></span> <span data-ttu-id="620fc-155">Például ha el szeretné különíteni a kérelmeket, amelyek hosszú ideig futnak az okait, érdemes található a fájl, amely a legtöbb kiszolgálására átlagos időt vehet igénybe.</span><span class="sxs-lookup"><span data-stu-id="620fc-155">For example, if you want to isolate the cause of requests that run for a long time, you might want to find the files that take the most time to serve on average.</span></span>
   <span data-ttu-id="620fc-156">Az alábbi részlet egy kérelem kiszolgálására legtöbb időt vett felső 25 erőforrásokat kéri le.</span><span class="sxs-lookup"><span data-stu-id="620fc-156">The snippet below retrieves the top 25 resources that took most time to serve a request.</span></span>

       def avgTimeTakenByKey(rdd):
           return rdd.combineByKey(lambda line: (line.time_taken, 1),
                                   lambda x, line: (x[0] + line.time_taken, x[1] + 1),
                                   lambda x, y: (x[0] + y[0], x[1] + y[1]))\
                     .map(lambda x: (x[0], float(x[1][0]) / float(x[1][1])))

       avgTimeTakenByKey(logLines.map(lambda p: (p.cs_uri_stem, p))).top(25, lambda x: x[1])

   <span data-ttu-id="620fc-157">A következőhöz hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="620fc-157">You should see an output like the following:</span></span>

       # -----------------
       # THIS IS AN OUTPUT
       # -----------------

       [(u'/blogposts/mvc4/step13.png', 197.5),
        (u'/blogposts/mvc2/step10.jpg', 179.5),
        (u'/blogposts/extractusercontrol/step5.png', 170.0),
        (u'/blogposts/mvc4/step8.png', 159.0),
        (u'/blogposts/mvcrouting/step22.jpg', 155.0),
        (u'/blogposts/mvcrouting/step3.jpg', 152.0),
        (u'/blogposts/linqsproc1/step16.jpg', 138.75),
        (u'/blogposts/linqsproc1/step26.jpg', 137.33333333333334),
        (u'/blogposts/vs2008javascript/step10.jpg', 127.0),
        (u'/blogposts/nested/step2.jpg', 126.0),
        (u'/blogposts/adminpack/step1.png', 124.0),
        (u'/BlogPosts/datalistpaging/step2.png', 118.0),
        (u'/blogposts/mvc4/step35.png', 117.0),
        (u'/blogposts/mvcrouting/step2.jpg', 116.5),
        (u'/blogposts/aboutme/basketball.jpg', 109.0),
        (u'/blogposts/anonymoustypes/step11.jpg', 109.0),
        (u'/blogposts/mvc4/step12.png', 106.0),
        (u'/blogposts/linq8/step0.jpg', 105.5),
        (u'/blogposts/mvc2/step18.jpg', 104.0),
        (u'/blogposts/mvc2/step11.jpg', 104.0),
        (u'/blogposts/mvcrouting/step1.jpg', 104.0),
        (u'/blogposts/extractusercontrol/step1.png', 103.0),
        (u'/blogposts/sqlvideos/sqlvideos.jpg', 102.0),
        (u'/blogposts/mvcrouting/step21.jpg', 101.0),
        (u'/blogposts/mvc4/step1.png', 98.0)]
5. <span data-ttu-id="620fc-158">Ezt az információt a képernyőn ábra is jelenthet.</span><span class="sxs-lookup"><span data-stu-id="620fc-158">You can also present this information in the form of plot.</span></span> <span data-ttu-id="620fc-159">Első lépésként rajzot létrehozásához, ossza meg velünk először létre kell hoznia egy ideiglenes tábla **AverageTime**.</span><span class="sxs-lookup"><span data-stu-id="620fc-159">As a first step to create a plot, let us first create a temporary table **AverageTime**.</span></span> <span data-ttu-id="620fc-160">A tábla a naplók megtekintéséhez, hogy voltak-e bármilyen szokatlan késés igényeiben jelentkező adott bármikor idő szerint csoportosítja.</span><span class="sxs-lookup"><span data-stu-id="620fc-160">The table groups the logs by time to see if there were any unusual latency spikes at any particular time.</span></span>

       avgTimeTakenByMinute = avgTimeTakenByKey(logLines.map(lambda p: (p.datetime.minute, p))).sortByKey()
       schema = StructType([StructField('Minutes', IntegerType(), True),
                            StructField('Time', FloatType(), True)])

       avgTimeTakenByMinuteDF = sqlContext.createDataFrame(avgTimeTakenByMinute, schema)
       avgTimeTakenByMinuteDF.registerTempTable('AverageTime')
6. <span data-ttu-id="620fc-161">Ezt követően futtathatja a következő SQL-lekérdezést a rekordok lekérése a **AverageTime** tábla.</span><span class="sxs-lookup"><span data-stu-id="620fc-161">You can then run the following SQL query to get all the records in the **AverageTime** table.</span></span>

       %%sql -o averagetime
       SELECT * FROM AverageTime

   <span data-ttu-id="620fc-162">A `%%sql` magic követ `-o averagetime` biztosítja, hogy a lekérdezés kimenetét a Jupyter kiszolgálón (általában a fürt headnode) helyileg megőrződjenek.</span><span class="sxs-lookup"><span data-stu-id="620fc-162">The `%%sql` magic followed by `-o averagetime` ensures that the output of the query is persisted locally on the Jupyter server (typically the headnode of the cluster).</span></span> <span data-ttu-id="620fc-163">A kimeneti tárolva a [Pandas](http://pandas.pydata.org/) a megadott nevű dataframe **averagetime**.</span><span class="sxs-lookup"><span data-stu-id="620fc-163">The output is persisted as a [Pandas](http://pandas.pydata.org/) dataframe with the specified name **averagetime**.</span></span>

   <span data-ttu-id="620fc-164">A következőhöz hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="620fc-164">You should see an output like the following:</span></span>

   <span data-ttu-id="620fc-165">![SQL-lekérdezés kimeneti](./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdinsight-jupyter-sql-qyery-output.png "SQL-lekérdezés kimenete")</span><span class="sxs-lookup"><span data-stu-id="620fc-165">![SQL query output](./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdinsight-jupyter-sql-qyery-output.png "SQL query output")</span></span>

   <span data-ttu-id="620fc-166">További információ a `%%sql` magic, lásd: [támogatott paraméterek a %% sql magic](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).</span><span class="sxs-lookup"><span data-stu-id="620fc-166">For more information about the `%%sql` magic, see [Parameters supported with the %%sql magic](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).</span></span>
7. <span data-ttu-id="620fc-167">Most már használhatja Matplotlib, a szalagtár segítségével hozza létre az adatok, a képi megjelenítés rajzot létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="620fc-167">You can now use Matplotlib, a library used to construct visualization of data, to create a plot.</span></span> <span data-ttu-id="620fc-168">Mert a rajzolási létre kell hozni a helyileg tárolt **averagetime** dataframe, a kódrészletet a következővel kell kezdődnie az `%%local` magic.</span><span class="sxs-lookup"><span data-stu-id="620fc-168">Because the plot must be created from the locally persisted **averagetime** dataframe, the code snippet must begin with the `%%local` magic.</span></span> <span data-ttu-id="620fc-169">Ez biztosítja, hogy a kód a Jupyter kiszolgálón helyileg futnak.</span><span class="sxs-lookup"><span data-stu-id="620fc-169">This ensures that the code is run locally on the Jupyter server.</span></span>

       %%local
       %matplotlib inline
       import matplotlib.pyplot as plt

       plt.plot(averagetime['Minutes'], averagetime['Time'], marker='o', linestyle='--')
       plt.xlabel('Time (min)')
       plt.ylabel('Average time taken for request (ms)')

   <span data-ttu-id="620fc-170">A következőhöz hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="620fc-170">You should see an output like the following:</span></span>

   <span data-ttu-id="620fc-171">![Matplotlib kimeneti](./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdinsight-apache-spark-web-log-analysis-plot.png "Matplotlib kimeneti")</span><span class="sxs-lookup"><span data-stu-id="620fc-171">![Matplotlib output](./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdinsight-apache-spark-web-log-analysis-plot.png "Matplotlib output")</span></span>
8. <span data-ttu-id="620fc-172">Az alkalmazás futtatását követően állítsa le a notebookot az erőforrások felszabadítása érdekében.</span><span class="sxs-lookup"><span data-stu-id="620fc-172">After you have finished running the application, you should shutdown the notebook to release the resources.</span></span> <span data-ttu-id="620fc-173">Ehhez a notebook **File** (Fájl) menüjében kattintson a **Close and Halt** (Bezárás és leállítás) elemre.</span><span class="sxs-lookup"><span data-stu-id="620fc-173">To do so, from the **File** menu on the notebook, click **Close and Halt**.</span></span> <span data-ttu-id="620fc-174">Ezzel leállítja és bezárja a notebookot.</span><span class="sxs-lookup"><span data-stu-id="620fc-174">This will shutdown and close the notebook.</span></span>

## <span data-ttu-id="620fc-175"><a name="seealso"></a>Lásd még:</span><span class="sxs-lookup"><span data-stu-id="620fc-175"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="620fc-176">Overview: Apache Spark on Azure HDInsight (Áttekintés: Apache Spark on Azure HDInsight)</span><span class="sxs-lookup"><span data-stu-id="620fc-176">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a><span data-ttu-id="620fc-177">Forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="620fc-177">Scenarios</span></span>
* [<span data-ttu-id="620fc-178">Spark és BI: Interaktív adatelemzés végrehajtása a Spark on HDInsight használatával, BI-eszközökkel</span><span class="sxs-lookup"><span data-stu-id="620fc-178">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="620fc-179">Spark és Machine Learning: A Spark on HDInsight használata az épület-hőmérséklet elemzésére HVAC-adatok alapján</span><span class="sxs-lookup"><span data-stu-id="620fc-179">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="620fc-180">Spark és Machine Learning: A Spark on HDInsight használata az élelmiszervizsgálati eredmények előrejelzésére</span><span class="sxs-lookup"><span data-stu-id="620fc-180">Spark with Machine Learning: Use Spark in HDInsight to predict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="620fc-181">Spark Streaming: A Spark on HDInsight használata valós idejű streamelési alkalmazások összeállítására</span><span class="sxs-lookup"><span data-stu-id="620fc-181">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)

### <a name="create-and-run-applications"></a><span data-ttu-id="620fc-182">Alkalmazások létrehozása és futtatása</span><span class="sxs-lookup"><span data-stu-id="620fc-182">Create and run applications</span></span>
* [<span data-ttu-id="620fc-183">Önálló alkalmazás létrehozása a Scala használatával</span><span class="sxs-lookup"><span data-stu-id="620fc-183">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="620fc-184">Feladatok távoli futtatása Spark-fürtön a Livy használatával</span><span class="sxs-lookup"><span data-stu-id="620fc-184">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="620fc-185">Eszközök és bővítmények</span><span class="sxs-lookup"><span data-stu-id="620fc-185">Tools and extensions</span></span>
* [<span data-ttu-id="620fc-186">Az IntelliJ IDEA HDInsight-eszközei beépülő moduljának használata Spark Scala-alkalmazások létrehozásához és elküldéséhez</span><span class="sxs-lookup"><span data-stu-id="620fc-186">Use HDInsight Tools Plugin for IntelliJ IDEA to create and submit Spark Scala applications</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="620fc-187">Az IntelliJ IDEA HDInsight-eszközei beépülő moduljának használata Spark-alkalmazások távoli hibaelhárításához</span><span class="sxs-lookup"><span data-stu-id="620fc-187">Use HDInsight Tools Plugin for IntelliJ IDEA to debug Spark applications remotely</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="620fc-188">Zeppelin notebookok használata Spark-fürttel HDInsighton</span><span class="sxs-lookup"><span data-stu-id="620fc-188">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="620fc-189">Jupyter notebookokhoz elérhető kernelek a HDInsight Spark-fürtjében</span><span class="sxs-lookup"><span data-stu-id="620fc-189">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="620fc-190">Külső csomagok használata Jupyter notebookokkal</span><span class="sxs-lookup"><span data-stu-id="620fc-190">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="620fc-191">A Jupyter telepítése a számítógépre, majd csatlakozás egy HDInsight Spark-fürthöz</span><span class="sxs-lookup"><span data-stu-id="620fc-191">Install Jupyter on your computer and connect to an HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a><span data-ttu-id="620fc-192">Erőforrások kezelése</span><span class="sxs-lookup"><span data-stu-id="620fc-192">Manage resources</span></span>
* [<span data-ttu-id="620fc-193">Apache Spark-fürt erőforrásainak kezelése az Azure HDInsightban</span><span class="sxs-lookup"><span data-stu-id="620fc-193">Manage resources for the Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="620fc-194">Apache Spark-fürtön futó feladatok nyomon követése és hibakeresése a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="620fc-194">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)
