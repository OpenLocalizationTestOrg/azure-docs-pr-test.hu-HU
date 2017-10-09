---
title: "a Spark - Azure Python könyvtárak aaaAnalyze webhelyek naplóinak |} Microsoft Docs"
description: "A notebook bemutatja, hogyan tooanalyze naplózni a Spark on Azure Hdinsighttal egyéni szalagtárat használ."
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
ms.openlocfilehash: 29e4308b2a359aee6d69494a98307d4da07f7909
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-website-logs-using-a-custom-python-library-with-spark-cluster-on-hdinsight"></a><span data-ttu-id="e226f-103">Egy egyéni Python kódtár használata a HDInsight Spark-fürt webhelyek naplóinak elemzése</span><span class="sxs-lookup"><span data-stu-id="e226f-103">Analyze website logs using a custom Python library with Spark cluster on HDInsight</span></span>

<span data-ttu-id="e226f-104">A notebook bemutatja, hogyan tooanalyze naplózni a Spark on HDInsight az egyéni szalagtárat használ.</span><span class="sxs-lookup"><span data-stu-id="e226f-104">This notebook demonstrates how tooanalyze log data using a custom library with Spark on HDInsight.</span></span> <span data-ttu-id="e226f-105">hello egyéni tárát használjuk egy Python kódtár nevű **iislogparser.py**.</span><span class="sxs-lookup"><span data-stu-id="e226f-105">hello custom library we use is a Python library called **iislogparser.py**.</span></span>

> [!TIP]
> <span data-ttu-id="e226f-106">Ez az oktatóanyag, Ön által létrehozott hdinsight (Linux) Spark-fürtön Jupyter notebook is érhető el.</span><span class="sxs-lookup"><span data-stu-id="e226f-106">This tutorial is also available as a Jupyter notebook on a Spark (Linux) cluster that you create in HDInsight.</span></span> <span data-ttu-id="e226f-107">hello notebook élmény lehetővé teszi a hello Python kódtöredékek futtassa hello notebook magát.</span><span class="sxs-lookup"><span data-stu-id="e226f-107">hello notebook experience lets you run hello Python snippets from hello notebook itself.</span></span> <span data-ttu-id="e226f-108">tooperform hello oktatóprogram belül a notebook Spark-fürt létrehozása, indítsa el a Jupyter notebook (`https://CLUSTERNAME.azurehdinsight.net/jupyter`), majd futtassa a hello notebook **naplóinak elemzése a Spark egy egyéni library.ipynb használatával** hello alatt  **PySpark** mappa.</span><span class="sxs-lookup"><span data-stu-id="e226f-108">tooperform hello tutorial from within a notebook, create a Spark cluster, launch a Jupyter notebook (`https://CLUSTERNAME.azurehdinsight.net/jupyter`), and then run hello notebook **Analyze logs with Spark using a custom library.ipynb** under hello **PySpark** folder.</span></span>
>
>

<span data-ttu-id="e226f-109">**Előfeltételek:**</span><span class="sxs-lookup"><span data-stu-id="e226f-109">**Prerequisites:**</span></span>

<span data-ttu-id="e226f-110">Hello következő kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="e226f-110">You must have hello following:</span></span>

* <span data-ttu-id="e226f-111">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="e226f-111">An Azure subscription.</span></span> <span data-ttu-id="e226f-112">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="e226f-112">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>

* <span data-ttu-id="e226f-113">A HDInsight az Apache Spark-fürt.</span><span class="sxs-lookup"><span data-stu-id="e226f-113">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="e226f-114">Útmutatásért lásd: [létrehozása az Apache Spark on Azure hdinsight clusters](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="e226f-114">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>

## <a name="save-raw-data-as-an-rdd"></a><span data-ttu-id="e226f-115">Nyers adatok elmentse egy RDD</span><span class="sxs-lookup"><span data-stu-id="e226f-115">Save raw data as an RDD</span></span>
<span data-ttu-id="e226f-116">Ebben a szakaszban hello használjuk [Jupyter](https://jupyter.org) Apache Spark-fürt a HDInsight toorun feladatok, amely a nyers mintaadatok feldolgozni, és mentse egy Hive tábla társított notebook.</span><span class="sxs-lookup"><span data-stu-id="e226f-116">In this section, we use hello [Jupyter](https://jupyter.org) notebook associated with an Apache Spark cluster in HDInsight toorun jobs that process your raw sample data and save it as a Hive table.</span></span> <span data-ttu-id="e226f-117">hello mintaadatokat egy CSV-fájlt (hvac.csv) elérhető alapértelmezés szerint minden fürtön.</span><span class="sxs-lookup"><span data-stu-id="e226f-117">hello sample data is a .csv file (hvac.csv) available on all clusters by default.</span></span>

<span data-ttu-id="e226f-118">Miután az adatok mentése Hive tábla hello a következő szakaszban azt csatlakoztassa toohello Hive tábla a Power BI és a Tableau Üzletiintelligencia-eszközök segítségével.</span><span class="sxs-lookup"><span data-stu-id="e226f-118">Once your data is saved as a Hive table, in hello next section we will connect toohello Hive table using BI tools such as Power BI and Tableau.</span></span>

1. <span data-ttu-id="e226f-119">A hello [Azure-portálon](https://portal.azure.com/), hello kezdőpulton, kattintson a Spark-fürt hello csempére (ha toohello kezdőpulton rögzítette azt).</span><span class="sxs-lookup"><span data-stu-id="e226f-119">From hello [Azure portal](https://portal.azure.com/), from hello startboard, click hello tile for your Spark cluster (if you pinned it toohello startboard).</span></span> <span data-ttu-id="e226f-120">Tooyour fürt alapján is megtalálhatja **összes tallózása** > **a HDInsight-fürtök**.</span><span class="sxs-lookup"><span data-stu-id="e226f-120">You can also navigate tooyour cluster under **Browse All** > **HDInsight Clusters**.</span></span>   
2. <span data-ttu-id="e226f-121">Hello Spark-fürt panelén kattintson **fürt irányítópult**, és kattintson a **Jupyter Notebook**.</span><span class="sxs-lookup"><span data-stu-id="e226f-121">From hello Spark cluster blade, click **Cluster Dashboard**, and then click **Jupyter Notebook**.</span></span> <span data-ttu-id="e226f-122">Ha a rendszer kéri, adja meg hello fürt hello rendszergazdai hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="e226f-122">If prompted, enter hello admin credentials for hello cluster.</span></span>

   > [!NOTE]
   > <span data-ttu-id="e226f-123">A fürt URL-címet a böngészőben a következő megnyitásakor hello által is hello Jupyter Notebook lehet elérni.</span><span class="sxs-lookup"><span data-stu-id="e226f-123">You may also reach hello Jupyter Notebook for your cluster by opening hello following URL in your browser.</span></span> <span data-ttu-id="e226f-124">Cserélje le **CLUSTERNAME** hello néven a fürt:</span><span class="sxs-lookup"><span data-stu-id="e226f-124">Replace **CLUSTERNAME** with hello name of your cluster:</span></span>
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >
3. <span data-ttu-id="e226f-125">Hozzon létre új notebookot.</span><span class="sxs-lookup"><span data-stu-id="e226f-125">Create a new notebook.</span></span> <span data-ttu-id="e226f-126">Kattintson a **New** (Új), majd a **PySpark** elemre.</span><span class="sxs-lookup"><span data-stu-id="e226f-126">Click **New**, and then click **PySpark**.</span></span>

    <span data-ttu-id="e226f-127">![Új Jupyter notebook létrehozása](./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdinsight-create-jupyter-notebook.png "Új Jupyter notebook létrehozása")</span><span class="sxs-lookup"><span data-stu-id="e226f-127">![Create a new Jupyter notebook](./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdinsight-create-jupyter-notebook.png "Create a new Jupyter notebook")</span></span>
4. <span data-ttu-id="e226f-128">Új notebook létrejött, és Untitled.pynb hello nevű.</span><span class="sxs-lookup"><span data-stu-id="e226f-128">A new notebook is created and opened with hello name Untitled.pynb.</span></span> <span data-ttu-id="e226f-129">Hello notebook neve hello tetején kattintson, és adjon meg egy rövid nevet.</span><span class="sxs-lookup"><span data-stu-id="e226f-129">Click hello notebook name at hello top, and enter a friendly name.</span></span>

    <span data-ttu-id="e226f-130">![Adjon meg egy nevet hello notebook](./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdinsight-name-jupyter-notebook.png "adjon meg egy nevet hello notebook")</span><span class="sxs-lookup"><span data-stu-id="e226f-130">![Provide a name for hello notebook](./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdinsight-name-jupyter-notebook.png "Provide a name for hello notebook")</span></span>
5. <span data-ttu-id="e226f-131">Mivel a notebook PySpark kernelt hello hozott létre, nem kell toocreate semmilyen tartalmat explicit módon.</span><span class="sxs-lookup"><span data-stu-id="e226f-131">Because you created a notebook using hello PySpark kernel, you do not need toocreate any contexts explicitly.</span></span> <span data-ttu-id="e226f-132">hello Spark és Hive-környezetek automatikusan létrehozza az Ön hello első kódcella futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="e226f-132">hello Spark and Hive contexts will be automatically created for you when you run hello first code cell.</span></span> <span data-ttu-id="e226f-133">Ehhez a forgatókönyvhöz szükséges típusok hello importálásával megkezdése.</span><span class="sxs-lookup"><span data-stu-id="e226f-133">You can start by importing hello types that are required for this scenario.</span></span> <span data-ttu-id="e226f-134">Illessze be az alábbi részlet egy üres cellába hello, és nyomja le az **SHIFT + ENTER**.</span><span class="sxs-lookup"><span data-stu-id="e226f-134">Paste hello following snippet in an empty cell, and then press **SHIFT + ENTER**.</span></span>

        from pyspark.sql import Row
        from pyspark.sql.types import *


1. <span data-ttu-id="e226f-135">Hozzon létre egy minta hello napló már rendelkezésre álló adatok használatával hello fürtön RDD.</span><span class="sxs-lookup"><span data-stu-id="e226f-135">Create an RDD using hello sample log data already available on hello cluster.</span></span> <span data-ttu-id="e226f-136">Hello hello fürthöz tartozó alapértelmezett tárfiók hello az adatok eléréséhez **\HdiSamples\HdiSamples\WebsiteLogSampleData\SampleLog\909f2b.log**.</span><span class="sxs-lookup"><span data-stu-id="e226f-136">You can access hello data in hello default storage account associated with hello cluster at **\HdiSamples\HdiSamples\WebsiteLogSampleData\SampleLog\909f2b.log**.</span></span>

        logs = sc.textFile('wasb:///HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b.log')


1. <span data-ttu-id="e226f-137">Kérje le egy mintanaplót beállítása tooverify hello a korábbi lépés sikeresen befejeződött.</span><span class="sxs-lookup"><span data-stu-id="e226f-137">Retrieve a sample log set tooverify that hello previous step completed successfully.</span></span>

        logs.take(5)

    <span data-ttu-id="e226f-138">Egy kimeneti hasonló toohello következő kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="e226f-138">You should see an output similar toohello following:</span></span>

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [u'#Software: Microsoft Internet Information Services 8.0',
         u'#Fields: date time s-sitename cs-method cs-uri-stem cs-uri-query s-port cs-username c-ip cs(User-Agent) cs(Cookie) cs(Referer) cs-host sc-status sc-substatus sc-win32-status sc-bytes cs-bytes time-taken',
         u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step2.png X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 53175 871 46',
         u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step3.png X-ARR-LOG-ID=9eace870-2f49-4efd-b204-0d170da46b4a 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 51237 871 32',
         u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step4.png X-ARR-LOG-ID=4bea5b3d-8ac9-46c9-9b8c-ec3e9500cbea 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 72177 871 47']

## <a name="analyze-log-data-using-a-custom-python-library"></a><span data-ttu-id="e226f-139">Egy egyéni Python kódtár használatával napló adatok elemzése</span><span class="sxs-lookup"><span data-stu-id="e226f-139">Analyze log data using a custom Python library</span></span>
1. <span data-ttu-id="e226f-140">A fenti hello kimenetében hello első néhány sor hello fejléc-információ tartalmazza, és minden fennmaradó sor megfelel, hogy a fejléc ismertetett hello séma.</span><span class="sxs-lookup"><span data-stu-id="e226f-140">In hello output above, hello first couple lines include hello header information and each remaining line matches hello schema described in that header.</span></span> <span data-ttu-id="e226f-141">Ilyen naplók elemzése bonyolult lehet.</span><span class="sxs-lookup"><span data-stu-id="e226f-141">Parsing such logs could be complicated.</span></span> <span data-ttu-id="e226f-142">Igen, egy egyéni Python kódtár használjuk (**iislogparser.py**), amely lehetővé teszi az ilyen sokkal könnyebben naplók elemzése.</span><span class="sxs-lookup"><span data-stu-id="e226f-142">So, we use a custom Python library (**iislogparser.py**) that makes parsing such logs much easier.</span></span> <span data-ttu-id="e226f-143">Alapértelmezés szerint ezt a szalagtárat megtalálható a következő hdinsight Spark-fürt **/HdiSamples/HdiSamples/WebsiteLogSampleData/iislogparser.py**.</span><span class="sxs-lookup"><span data-stu-id="e226f-143">By default, this library is included with your Spark cluster on HDInsight at **/HdiSamples/HdiSamples/WebsiteLogSampleData/iislogparser.py**.</span></span>

    <span data-ttu-id="e226f-144">Azonban ez a tár nincs hello `PYTHONPATH` , nem tudjuk használni, például import utasítás segítségével `import iislogparser`.</span><span class="sxs-lookup"><span data-stu-id="e226f-144">However, this library is not in hello `PYTHONPATH` so we cannot use it by using an import statement like `import iislogparser`.</span></span> <span data-ttu-id="e226f-145">toouse ezt a tárat, azt el kell osztania az tooall hello munkavégző csomópontokhoz.</span><span class="sxs-lookup"><span data-stu-id="e226f-145">toouse this library, we must distribute it tooall hello worker nodes.</span></span> <span data-ttu-id="e226f-146">Futtassa a következő kódrészletet hello.</span><span class="sxs-lookup"><span data-stu-id="e226f-146">Run hello following snippet.</span></span>

        sc.addPyFile('wasb:///HdiSamples/HdiSamples/WebsiteLogSampleData/iislogparser.py')


1. <span data-ttu-id="e226f-147">`iislogparser`funkció `parse_log_line` , amely visszaadja a `None` Ha egy napló sor a fejlécsor lesz, és hello példányának `LogLine` osztály, ha egy napló sor ütközik.</span><span class="sxs-lookup"><span data-stu-id="e226f-147">`iislogparser` provides a function `parse_log_line` that returns `None` if a log line is a header row, and returns an instance of hello `LogLine` class if it encounters a log line.</span></span> <span data-ttu-id="e226f-148">Használjon hello `LogLine` osztály tooextract csak hello hello RDD napló sorok:</span><span class="sxs-lookup"><span data-stu-id="e226f-148">Use hello `LogLine` class tooextract only hello log lines from hello RDD:</span></span>

        def parse_line(l):
            import iislogparser
            return iislogparser.parse_log_line(l)
        logLines = logs.map(parse_line).filter(lambda p: p is not None).cache()
2. <span data-ttu-id="e226f-149">Kibontott napló sorok tooverify, amely hello sikeresen befejeződött. lépés néhány beolvasása.</span><span class="sxs-lookup"><span data-stu-id="e226f-149">Retrieve a couple of extracted log lines tooverify that hello step completed successfully.</span></span>

       logLines.take(2)

   <span data-ttu-id="e226f-150">hello kimeneti hasonló toohello következő legyen:</span><span class="sxs-lookup"><span data-stu-id="e226f-150">hello output should be similar toohello following:</span></span>

       # -----------------
       # THIS IS AN OUTPUT
       # -----------------

       [2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step2.png X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 53175 871 46,
        2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step3.png X-ARR-LOG-ID=9eace870-2f49-4efd-b204-0d170da46b4a 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 51237 871 32]
3. <span data-ttu-id="e226f-151">Hello `LogLine` osztály, viszont az például néhány hasznos metódusokkal rendelkezik `is_error()`, amely adja vissza, hogy rendelkezik-e a naplóbejegyzés hibakódot.</span><span class="sxs-lookup"><span data-stu-id="e226f-151">hello `LogLine` class, in turn, has some useful methods, like `is_error()`, which returns whether a log entry has an error code.</span></span> <span data-ttu-id="e226f-152">Hibák száma a toocompute hello kibontott hello napló sorok használja, és ezután minden hello hibák tooa másik fájlt.</span><span class="sxs-lookup"><span data-stu-id="e226f-152">Use this toocompute hello number of errors in hello extracted log lines, and then log all hello errors tooa different file.</span></span>

       errors = logLines.filter(lambda p: p.is_error())
       numLines = logLines.count()
       numErrors = errors.count()
       print 'There are', numErrors, 'errors and', numLines, 'log entries'
       errors.map(lambda p: str(p)).saveAsTextFile('wasb:///HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b-2.log')

   <span data-ttu-id="e226f-153">Hello hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="e226f-153">You should see an output like hello following:</span></span>

       # -----------------
       # THIS IS AN OUTPUT
       # -----------------

       There are 30 errors and 646 log entries
4. <span data-ttu-id="e226f-154">Is **Matplotlib** tooconstruct hello adatok képi megjelenítés.</span><span class="sxs-lookup"><span data-stu-id="e226f-154">You can also use **Matplotlib** tooconstruct a visualization of hello data.</span></span> <span data-ttu-id="e226f-155">Például ha azt szeretné, hogy hosszú ideig futó kérelmek tooisolate hello okait, érdemes toofind hello fájlok, amelyek hello legtöbb idő tooserve átlagosan.</span><span class="sxs-lookup"><span data-stu-id="e226f-155">For example, if you want tooisolate hello cause of requests that run for a long time, you might want toofind hello files that take hello most time tooserve on average.</span></span>
   <span data-ttu-id="e226f-156">az alábbi hello részlet hello felső 25 erőforrást, amelyre szükség volt a legtöbb idő tooserve kérelmet kéri le.</span><span class="sxs-lookup"><span data-stu-id="e226f-156">hello snippet below retrieves hello top 25 resources that took most time tooserve a request.</span></span>

       def avgTimeTakenByKey(rdd):
           return rdd.combineByKey(lambda line: (line.time_taken, 1),
                                   lambda x, line: (x[0] + line.time_taken, x[1] + 1),
                                   lambda x, y: (x[0] + y[0], x[1] + y[1]))\
                     .map(lambda x: (x[0], float(x[1][0]) / float(x[1][1])))

       avgTimeTakenByKey(logLines.map(lambda p: (p.cs_uri_stem, p))).top(25, lambda x: x[1])

   <span data-ttu-id="e226f-157">Hello hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="e226f-157">You should see an output like hello following:</span></span>

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
5. <span data-ttu-id="e226f-158">Ez az információ használatát a tartománynevekben rajzolási hello is jelenthet.</span><span class="sxs-lookup"><span data-stu-id="e226f-158">You can also present this information in hello form of plot.</span></span> <span data-ttu-id="e226f-159">Egy első lépés toocreate rajzot, ossza meg velünk először hozzon létre egy ideiglenes tábla **AverageTime**.</span><span class="sxs-lookup"><span data-stu-id="e226f-159">As a first step toocreate a plot, let us first create a temporary table **AverageTime**.</span></span> <span data-ttu-id="e226f-160">Ha bármely szokatlan késés igényeiben jelentkező történt egy adott időpontban hello tábla csoportok hello által idő toosee naplózza.</span><span class="sxs-lookup"><span data-stu-id="e226f-160">hello table groups hello logs by time toosee if there were any unusual latency spikes at any particular time.</span></span>

       avgTimeTakenByMinute = avgTimeTakenByKey(logLines.map(lambda p: (p.datetime.minute, p))).sortByKey()
       schema = StructType([StructField('Minutes', IntegerType(), True),
                            StructField('Time', FloatType(), True)])

       avgTimeTakenByMinuteDF = sqlContext.createDataFrame(avgTimeTakenByMinute, schema)
       avgTimeTakenByMinuteDF.registerTempTable('AverageTime')
6. <span data-ttu-id="e226f-161">Ezután futtassa a következő SQL-lekérdezés tooget hello bejegyzésekkel hello a hello **AverageTime** tábla.</span><span class="sxs-lookup"><span data-stu-id="e226f-161">You can then run hello following SQL query tooget all hello records in hello **AverageTime** table.</span></span>

       %%sql -o averagetime
       SELECT * FROM AverageTime

   <span data-ttu-id="e226f-162">Hello `%%sql` magic követ `-o averagetime` biztosítja, hogy hello lekérdezés kimenetét hello hello (általában hello headnode hello fürt) Jupyter kiszolgálón helyileg megőrződjenek.</span><span class="sxs-lookup"><span data-stu-id="e226f-162">hello `%%sql` magic followed by `-o averagetime` ensures that hello output of hello query is persisted locally on hello Jupyter server (typically hello headnode of hello cluster).</span></span> <span data-ttu-id="e226f-163">hello kimeneti tárolva a [Pandas](http://pandas.pydata.org/) dataframe hello a megadott név **averagetime**.</span><span class="sxs-lookup"><span data-stu-id="e226f-163">hello output is persisted as a [Pandas](http://pandas.pydata.org/) dataframe with hello specified name **averagetime**.</span></span>

   <span data-ttu-id="e226f-164">Hello hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="e226f-164">You should see an output like hello following:</span></span>

   <span data-ttu-id="e226f-165">![SQL-lekérdezés kimeneti](./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdinsight-jupyter-sql-qyery-output.png "SQL-lekérdezés kimenete")</span><span class="sxs-lookup"><span data-stu-id="e226f-165">![SQL query output](./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdinsight-jupyter-sql-qyery-output.png "SQL query output")</span></span>

   <span data-ttu-id="e226f-166">További információ a hello `%%sql` magic, lásd: [hello támogatott paraméterek %% sql magic](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).</span><span class="sxs-lookup"><span data-stu-id="e226f-166">For more information about hello `%%sql` magic, see [Parameters supported with hello %%sql magic](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).</span></span>
7. <span data-ttu-id="e226f-167">Ezután már használhatja a Matplotlib marad, a szalagtár tooconstruct képi megjelenítés adatainak, toocreate rajzot használja.</span><span class="sxs-lookup"><span data-stu-id="e226f-167">You can now use Matplotlib, a library used tooconstruct visualization of data, toocreate a plot.</span></span> <span data-ttu-id="e226f-168">Mert a rajzolási hello hello helyileg kell létrehozni a megőrzött **averagetime** dataframe, hello kódrészletet a következővel kell kezdődnie hello `%%local` magic.</span><span class="sxs-lookup"><span data-stu-id="e226f-168">Because hello plot must be created from hello locally persisted **averagetime** dataframe, hello code snippet must begin with hello `%%local` magic.</span></span> <span data-ttu-id="e226f-169">Ez biztosítja, hogy hello kód hello Jupyter kiszolgálón helyileg futnak.</span><span class="sxs-lookup"><span data-stu-id="e226f-169">This ensures that hello code is run locally on hello Jupyter server.</span></span>

       %%local
       %matplotlib inline
       import matplotlib.pyplot as plt

       plt.plot(averagetime['Minutes'], averagetime['Time'], marker='o', linestyle='--')
       plt.xlabel('Time (min)')
       plt.ylabel('Average time taken for request (ms)')

   <span data-ttu-id="e226f-170">Hello hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="e226f-170">You should see an output like hello following:</span></span>

   <span data-ttu-id="e226f-171">![Matplotlib kimeneti](./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdinsight-apache-spark-web-log-analysis-plot.png "Matplotlib kimeneti")</span><span class="sxs-lookup"><span data-stu-id="e226f-171">![Matplotlib output](./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdinsight-apache-spark-web-log-analysis-plot.png "Matplotlib output")</span></span>
8. <span data-ttu-id="e226f-172">Miután befejezte a hello alkalmazást futtat, akkor leállítási hello notebook toorelease hello erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="e226f-172">After you have finished running hello application, you should shutdown hello notebook toorelease hello resources.</span></span> <span data-ttu-id="e226f-173">toodo Igen, a hello **fájl** hello notebook menüjében kattintson **zárja be és Halt**.</span><span class="sxs-lookup"><span data-stu-id="e226f-173">toodo so, from hello **File** menu on hello notebook, click **Close and Halt**.</span></span> <span data-ttu-id="e226f-174">Ezzel leállítja és Bezárás hello notebookot.</span><span class="sxs-lookup"><span data-stu-id="e226f-174">This will shutdown and close hello notebook.</span></span>

## <span data-ttu-id="e226f-175"><a name="seealso"></a>Lásd még:</span><span class="sxs-lookup"><span data-stu-id="e226f-175"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="e226f-176">Overview: Apache Spark on Azure HDInsight (Áttekintés: Apache Spark on Azure HDInsight)</span><span class="sxs-lookup"><span data-stu-id="e226f-176">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a><span data-ttu-id="e226f-177">Forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="e226f-177">Scenarios</span></span>
* [<span data-ttu-id="e226f-178">Spark és BI: Interaktív adatelemzés végrehajtása a Spark on HDInsight használatával, BI-eszközökkel</span><span class="sxs-lookup"><span data-stu-id="e226f-178">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="e226f-179">Spark és Machine Learning: A Spark on HDInsight használata az épület-hőmérséklet elemzésére HVAC-adatok alapján</span><span class="sxs-lookup"><span data-stu-id="e226f-179">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="e226f-180">Spark és Machine Learning: használja a Spark on HDInsight toopredict élelmiszervizsgálati eredmények</span><span class="sxs-lookup"><span data-stu-id="e226f-180">Spark with Machine Learning: Use Spark in HDInsight toopredict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="e226f-181">Spark Streaming: A Spark on HDInsight használata valós idejű streamelési alkalmazások összeállítására</span><span class="sxs-lookup"><span data-stu-id="e226f-181">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)

### <a name="create-and-run-applications"></a><span data-ttu-id="e226f-182">Alkalmazások létrehozása és futtatása</span><span class="sxs-lookup"><span data-stu-id="e226f-182">Create and run applications</span></span>
* [<span data-ttu-id="e226f-183">Önálló alkalmazás létrehozása a Scala használatával</span><span class="sxs-lookup"><span data-stu-id="e226f-183">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="e226f-184">Feladatok távoli futtatása Spark-fürtön a Livy használatával</span><span class="sxs-lookup"><span data-stu-id="e226f-184">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="e226f-185">Eszközök és bővítmények</span><span class="sxs-lookup"><span data-stu-id="e226f-185">Tools and extensions</span></span>
* [<span data-ttu-id="e226f-186">Toocreate IntelliJ IDEA HDInsight-eszközei beépülő használja, és küldje el a Spark Scala-alkalmazások</span><span class="sxs-lookup"><span data-stu-id="e226f-186">Use HDInsight Tools Plugin for IntelliJ IDEA toocreate and submit Spark Scala applications</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="e226f-187">IntelliJ IDEA toodebug Spark-alkalmazások HDInsight-eszközei beépülő távolról használni</span><span class="sxs-lookup"><span data-stu-id="e226f-187">Use HDInsight Tools Plugin for IntelliJ IDEA toodebug Spark applications remotely</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="e226f-188">Zeppelin notebookok használata Spark-fürttel HDInsighton</span><span class="sxs-lookup"><span data-stu-id="e226f-188">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="e226f-189">Jupyter notebookokhoz elérhető kernelek a HDInsight Spark-fürtjében</span><span class="sxs-lookup"><span data-stu-id="e226f-189">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="e226f-190">Külső csomagok használata Jupyter notebookokkal</span><span class="sxs-lookup"><span data-stu-id="e226f-190">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="e226f-191">Jupyter telepítse a számítógépre, és csatlakozzon a HDInsight Spark-fürt tooan</span><span class="sxs-lookup"><span data-stu-id="e226f-191">Install Jupyter on your computer and connect tooan HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a><span data-ttu-id="e226f-192">Erőforrások kezelése</span><span class="sxs-lookup"><span data-stu-id="e226f-192">Manage resources</span></span>
* [<span data-ttu-id="e226f-193">Az Azure HDInsight hello Apache Spark-fürt erőforrásainak kezelése</span><span class="sxs-lookup"><span data-stu-id="e226f-193">Manage resources for hello Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="e226f-194">Apache Spark-fürtön futó feladatok nyomon követése és hibakeresése a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="e226f-194">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)
