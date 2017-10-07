---
title: "aaaUse Zeppelin notebookok az Apache Spark on Azure HDInsight fürt |} Microsoft Docs"
description: "Hogyan toouse Zeppelin notebookok az Apache Spark on Azure HDInsight clusters részletes ismertetése."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: df489d70-7788-4efa-a089-e5e5006421e2
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 3ab479cfccc7fd38a9bf6a9fb4f5928beec8ff7b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-zeppelin-notebooks-with-apache-spark-cluster-on-azure-hdinsight"></a><span data-ttu-id="a6413-103">Zeppelin notebookok használata Azure HDInsight az Apache Spark-fürt</span><span class="sxs-lookup"><span data-stu-id="a6413-103">Use Zeppelin notebooks with Apache Spark cluster on Azure HDInsight</span></span>

<span data-ttu-id="a6413-104">HDInsight Spark-fürtjei Zeppelin notebookok használható toorun Spark feladatok közé tartozik.</span><span class="sxs-lookup"><span data-stu-id="a6413-104">HDInsight Spark clusters include Zeppelin notebooks that you can use toorun Spark jobs.</span></span> <span data-ttu-id="a6413-105">Ebből a cikkből megismerheti, hogyan toouse hello Zeppelin jegyzetfüzet a HDInsight-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="a6413-105">In this article, you learn how toouse hello Zeppelin notebook on an HDInsight cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="a6413-106">Zeppelin notebookok csak 1.6.3 Spark on HDInsight 3.5-ös és 2.1.0 Spark on HDInsight 3.6 érhetők el.</span><span class="sxs-lookup"><span data-stu-id="a6413-106">Zeppelin notebooks are available only for Spark 1.6.3 on HDInsight 3.5 and Spark 2.1.0 on HDInsight 3.6.</span></span>
>

<span data-ttu-id="a6413-107">**Előfeltételek:**</span><span class="sxs-lookup"><span data-stu-id="a6413-107">**Prerequisites:**</span></span>

* <span data-ttu-id="a6413-108">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="a6413-108">An Azure subscription.</span></span> <span data-ttu-id="a6413-109">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="a6413-109">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="a6413-110">A HDInsight az Apache Spark-fürt.</span><span class="sxs-lookup"><span data-stu-id="a6413-110">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="a6413-111">Útmutatásért lásd: [létrehozása az Apache Spark on Azure hdinsight clusters](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="a6413-111">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>

## <a name="launch-a-zeppelin-notebook"></a><span data-ttu-id="a6413-112">Indítsa el a Zeppelin notebook</span><span class="sxs-lookup"><span data-stu-id="a6413-112">Launch a Zeppelin notebook</span></span>
1. <span data-ttu-id="a6413-113">Hello Spark-fürt panelén kattintson **fürt irányítópult**, és kattintson a **Zeppelin Notebook**.</span><span class="sxs-lookup"><span data-stu-id="a6413-113">From hello Spark cluster blade, click **Cluster Dashboard**, and then click **Zeppelin Notebook**.</span></span> <span data-ttu-id="a6413-114">Ha a rendszer kéri, adja meg hello fürt hello rendszergazdai hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="a6413-114">If prompted, enter hello admin credentials for hello cluster.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="a6413-115">A fürt URL-címet a böngészőben a következő megnyitásakor hello által is hello Zeppelin Notebook lehet elérni.</span><span class="sxs-lookup"><span data-stu-id="a6413-115">You may also reach hello Zeppelin Notebook for your cluster by opening hello following URL in your browser.</span></span> <span data-ttu-id="a6413-116">Cserélje le **CLUSTERNAME** hello néven a fürt:</span><span class="sxs-lookup"><span data-stu-id="a6413-116">Replace **CLUSTERNAME** with hello name of your cluster:</span></span>
   > 
   > `https://CLUSTERNAME.azurehdinsight.net/zeppelin`
   > 
   > 
2. <span data-ttu-id="a6413-117">Hozzon létre új notebookot.</span><span class="sxs-lookup"><span data-stu-id="a6413-117">Create a new notebook.</span></span> <span data-ttu-id="a6413-118">Hello fejléc ablaktáblában kattintson **Notebook**, és kattintson a **hozzon létre új megjegyzés**.</span><span class="sxs-lookup"><span data-stu-id="a6413-118">From hello header pane, click **Notebook**, and then click **Create New Note**.</span></span>
   
    <span data-ttu-id="a6413-119">![Létrehozhat egy újat Zeppelin](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-create-zeppelin-notebook.png "egy új Zeppelin notebook létrehozása")</span><span class="sxs-lookup"><span data-stu-id="a6413-119">![Create a new Zeppelin notebook](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-create-zeppelin-notebook.png "Create a new Zeppelin notebook")</span></span>
   
    <span data-ttu-id="a6413-120">Adjon meg egy nevet hello notebook, és kattintson **létrehozása Megjegyzés**.</span><span class="sxs-lookup"><span data-stu-id="a6413-120">Enter a name for hello notebook, and then click **Create Note**.</span></span>
3. <span data-ttu-id="a6413-121">Ellenőrizze azt is, hello notebook fejléc egy csatlakoztatott állapotát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="a6413-121">Also, make sure hello notebook header shows a connected status.</span></span> <span data-ttu-id="a6413-122">Hello jobb felső sarokban, zöld pontot helyén.</span><span class="sxs-lookup"><span data-stu-id="a6413-122">It is denoted by a green dot in hello top-right corner.</span></span>
   
    <span data-ttu-id="a6413-123">![Zeppelin notebook állapot](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-connected.png "Zeppelin notebook állapota")</span><span class="sxs-lookup"><span data-stu-id="a6413-123">![Zeppelin notebook status](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-connected.png "Zeppelin notebook status")</span></span>
4. <span data-ttu-id="a6413-124">Töltse be a mintaadatokat egy ideiglenes táblába.</span><span class="sxs-lookup"><span data-stu-id="a6413-124">Load sample data into a temporary table.</span></span> <span data-ttu-id="a6413-125">Spark-fürt hdinsightban történő hello mintaadatfájlokat, létrehozásakor **hvac.csv**, a másolt toohello kapcsolódó tárfiók **\HdiSamples\SensorSampleData\hvac**.</span><span class="sxs-lookup"><span data-stu-id="a6413-125">When you create a Spark cluster in HDInsight, hello sample data file, **hvac.csv**, is copied toohello associated storage account under **\HdiSamples\SensorSampleData\hvac**.</span></span>
   
    <span data-ttu-id="a6413-126">Hello üres bekezdés, amely alapértelmezés szerint az új notebook hello jön létre illessze be a következő kódrészletet hello.</span><span class="sxs-lookup"><span data-stu-id="a6413-126">In hello empty paragraph that is created by default in hello new notebook, paste hello following snippet.</span></span>
   
        %livy.spark
        //hello above magic instructs Zeppelin toouse hello Livy Scala interpreter
   
        // Create an RDD using hello default Spark context, sc
        val hvacText = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
   
        // Define a schema
        case class Hvac(date: String, time: String, targettemp: Integer, actualtemp: Integer, buildingID: String)
   
        // Map hello values in hello .csv file toohello schema
        val hvac = hvacText.map(s => s.split(",")).filter(s => s(0) != "Date").map(
            s => Hvac(s(0), 
                    s(1),
                    s(2).toInt,
                    s(3).toInt,
                    s(6)
            )
        ).toDF()
   
        // Register as a temporary table called "hvac"
        hvac.registerTempTable("hvac")
   
    <span data-ttu-id="a6413-127">Nyomja le az **SHIFT + ENTER** , vagy kattintson a hello **lejátszása** hello bekezdés toorun hello részlet gombra.</span><span class="sxs-lookup"><span data-stu-id="a6413-127">Press **SHIFT + ENTER** or click hello **Play** button for hello paragraph toorun hello snippet.</span></span> <span data-ttu-id="a6413-128">hello hello bekezdés jobb-sarkában hello állapotának a kész, függőben lévő, FUTÓ tooFINISHED kell előrehaladás.</span><span class="sxs-lookup"><span data-stu-id="a6413-128">hello status on hello right-corner of hello paragraph should progress from READY, PENDING, RUNNING tooFINISHED.</span></span> <span data-ttu-id="a6413-129">hello hello alján megjelenik hello kimeneti egyazon paragraph.</span><span class="sxs-lookup"><span data-stu-id="a6413-129">hello output shows up at hello bottom of hello same paragraph.</span></span> <span data-ttu-id="a6413-130">képernyőfelvétel a hello következőhöz hasonló hello:</span><span class="sxs-lookup"><span data-stu-id="a6413-130">hello screenshot looks like hello following:</span></span>
   
    <span data-ttu-id="a6413-131">![Hozzon létre egy ideiglenes táblából a nyers adatoktól](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-load-data.png "a nyers adatoktól ideiglenes tábla létrehozása")</span><span class="sxs-lookup"><span data-stu-id="a6413-131">![Create a temporary table from raw data](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-load-data.png "Create a temporary table from raw data")</span></span>
   
    <span data-ttu-id="a6413-132">A cím tooeach bekezdés is megadhatja.</span><span class="sxs-lookup"><span data-stu-id="a6413-132">You can also provide a title tooeach paragraph.</span></span> <span data-ttu-id="a6413-133">A hello jobb sarkában, kattintson a hello **beállítások** ikonra, végül **cím megjelenítése a**.</span><span class="sxs-lookup"><span data-stu-id="a6413-133">From hello right-hand corner, click hello **Settings** icon, and then click **Show title**.</span></span>
5. <span data-ttu-id="a6413-134">Most futtathatja a Spark SQL-utasítások hello **hvac** tábla.</span><span class="sxs-lookup"><span data-stu-id="a6413-134">You can now run Spark SQL statements on hello **hvac** table.</span></span> <span data-ttu-id="a6413-135">Illessze be a következő lekérdezés egy új bekezdés hello.</span><span class="sxs-lookup"><span data-stu-id="a6413-135">Paste hello following query in a new paragraph.</span></span> <span data-ttu-id="a6413-136">hello lekérdezés lekéri a hello épület azonosítója és hello különbség hello cél és a tényleges hőmérsékletek minden készítéséhez egy adott időpontban.</span><span class="sxs-lookup"><span data-stu-id="a6413-136">hello query retrieves hello building ID and hello difference between hello target and actual temperatures for each building on a given date.</span></span> <span data-ttu-id="a6413-137">Nyomja le az **SHIFT + ENTER**.</span><span class="sxs-lookup"><span data-stu-id="a6413-137">Press **SHIFT + ENTER**.</span></span>
   
        %sql
        select buildingID, (targettemp - actualtemp) as temp_diff, date from hvac where date = "6/1/13" 
   
    <span data-ttu-id="a6413-138">Hello **% sql** hello elején utasítás hello notebook toouse hello Livy Scala parancsértelmező jelzi.</span><span class="sxs-lookup"><span data-stu-id="a6413-138">hello **%sql** statement at hello beginning tells hello notebook toouse hello Livy Scala interpreter.</span></span>
   
    <span data-ttu-id="a6413-139">hello alábbi képernyőfelvételen látható hello kimeneti.</span><span class="sxs-lookup"><span data-stu-id="a6413-139">hello following screenshot shows hello output.</span></span>
   
    <span data-ttu-id="a6413-140">![Hello notebook használata Spark SQL-utasítás futtatása](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-spark-query-1.png "hello notebook használata Spark SQL-utasítás futtatása")</span><span class="sxs-lookup"><span data-stu-id="a6413-140">![Run a Spark SQL statement using hello notebook](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-spark-query-1.png "Run a Spark SQL statement using hello notebook")</span></span>
   
     <span data-ttu-id="a6413-141">Kattintson a hello megjelenítési beállítások (a kijelölt téglalap) tooswitch között különböző felelősséget a hello azonos kimenethez.</span><span class="sxs-lookup"><span data-stu-id="a6413-141">Click hello display options (highlighted in rectangle) tooswitch between different representations for hello same output.</span></span> <span data-ttu-id="a6413-142">Kattintson a **beállítások** toochoose milyen consitutes hello kulcs és hello kimeneti értékeit.</span><span class="sxs-lookup"><span data-stu-id="a6413-142">Click **Settings** toochoose what consitutes hello key and values in hello output.</span></span> <span data-ttu-id="a6413-143">képernyőfelvétel-készítés fent használ hello **buildingID** hello kulcs és hello átlaga **temp_diff** hello értékként.</span><span class="sxs-lookup"><span data-stu-id="a6413-143">hello screen capture above uses **buildingID** as hello key and hello average of **temp_diff** as hello value.</span></span>
6. <span data-ttu-id="a6413-144">Változók használata hello lekérdezés Spark SQL-utasítások is futtathatja.</span><span class="sxs-lookup"><span data-stu-id="a6413-144">You can also run Spark SQL statements using variables in hello query.</span></span> <span data-ttu-id="a6413-145">hello a következő kódrészletben látható szövegrészt hogyan toodefine egy változó **Temp**, hello lekérdezés hello a lehetséges értékek a tooquery kívánja.</span><span class="sxs-lookup"><span data-stu-id="a6413-145">hello next snippet shows how toodefine a variable, **Temp**, in hello query with hello possible values you want tooquery with.</span></span> <span data-ttu-id="a6413-146">Hello lekérdezés első futtatásakor egy legördülő lista automatikusan töltődik hello változóhoz megadott hello értékek.</span><span class="sxs-lookup"><span data-stu-id="a6413-146">When you first run hello query, a drop-down is automatically populated with hello values you specified for hello variable.</span></span>
   
        %sql
        select buildingID, date, targettemp, (targettemp - actualtemp) as temp_diff from hvac where targettemp > "${Temp = 65,65|75|85}" 
   
    <span data-ttu-id="a6413-147">Ezt a kódrészletet illessze be egy új bekezdés, és nyomja le az **SHIFT + ENTER**.</span><span class="sxs-lookup"><span data-stu-id="a6413-147">Paste this snippet in a new paragraph and press **SHIFT + ENTER**.</span></span> <span data-ttu-id="a6413-148">hello alábbi képernyőfelvételen látható hello kimeneti.</span><span class="sxs-lookup"><span data-stu-id="a6413-148">hello following screenshot shows hello output.</span></span>
   
    <span data-ttu-id="a6413-149">![Hello notebook használata Spark SQL-utasítás futtatása](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-spark-query-2.png "hello notebook használata Spark SQL-utasítás futtatása")</span><span class="sxs-lookup"><span data-stu-id="a6413-149">![Run a Spark SQL statement using hello notebook](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-spark-query-2.png "Run a Spark SQL statement using hello notebook")</span></span>
   
    <span data-ttu-id="a6413-150">A lekérdezések kiválaszthat egy új értéket a hello legördülő, és futtassa újra a hello lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="a6413-150">For subsequent queries, you can select a new value from hello drop-down and run hello query again.</span></span> <span data-ttu-id="a6413-151">Kattintson a **beállítások** toochoose milyen consitutes hello kulcs és hello kimeneti értékeit.</span><span class="sxs-lookup"><span data-stu-id="a6413-151">Click **Settings** toochoose what consitutes hello key and values in hello output.</span></span> <span data-ttu-id="a6413-152">képernyőfelvétel-készítés fent használ hello **buildingID** hello kulcsként hello átlagos **temp_diff** hello értékként és **targettemp** hello csoportként.</span><span class="sxs-lookup"><span data-stu-id="a6413-152">hello screen capture above uses **buildingID** as hello key, hello average of **temp_diff** as hello value, and **targettemp** as hello group.</span></span>
7. <span data-ttu-id="a6413-153">Indítsa újra a hello Livy parancsértelmező tooexit hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="a6413-153">Restart hello Livy interpreter tooexit hello application.</span></span> <span data-ttu-id="a6413-154">toodo tehát parancsértelmező beállításai megnyitásához hello bejelentkezett felhasználó nevét a hello jobb felső sarokban, és kattintson **parancsértelmező**.</span><span class="sxs-lookup"><span data-stu-id="a6413-154">toodo so, open interpreter settings by clicking hello logged in user name from hello top-right corner, and then click **Interpreter**.</span></span>
   
    <span data-ttu-id="a6413-155">![Indítsa el a parancsértelmező](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "kimeneti struktúra")</span><span class="sxs-lookup"><span data-stu-id="a6413-155">![Launch interpreter](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Hive output")</span></span>
8. <span data-ttu-id="a6413-156">Görgessen tooLivy parancsértelmező beállításait, és kattintson a **indítsa újra a**.</span><span class="sxs-lookup"><span data-stu-id="a6413-156">Scroll tooLivy interpreter settings and then click **Restart**.</span></span>
   
    <span data-ttu-id="a6413-157">![Indítsa újra a hello Livy intepreter](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-restart-interpreter.png "hello Zeppelin intepreter újraindítása")</span><span class="sxs-lookup"><span data-stu-id="a6413-157">![Restart hello Livy intepreter](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-restart-interpreter.png "Restart hello Zeppelin intepreter")</span></span>

## <a name="how-do-i-use-external-packages-with-hello-notebook"></a><span data-ttu-id="a6413-158">Hogyan külső csomagok használata hello notebook?</span><span class="sxs-lookup"><span data-stu-id="a6413-158">How do I use external packages with hello notebook?</span></span>
<span data-ttu-id="a6413-159">Az Apache Spark-fürt a HDInsight (Linux) toouse külső, közösségi hozzájárult csomagok, amelyek nem szerepel az a-kész hello fürt hello Zeppelin notebook konfigurálhatja.</span><span class="sxs-lookup"><span data-stu-id="a6413-159">You can configure hello Zeppelin notebook in Apache Spark cluster on HDInsight (Linux) toouse external, community-contributed packages that are not included out-of-the-box in hello cluster.</span></span> <span data-ttu-id="a6413-160">Hello kereshet [Maven-tárház](http://search.maven.org/) hello csomagok rendelkezésre álló teljes listáját.</span><span class="sxs-lookup"><span data-stu-id="a6413-160">You can search hello [Maven repository](http://search.maven.org/) for hello complete list of packages that are available.</span></span> <span data-ttu-id="a6413-161">Más forrásból is megkapható elérhető csomagok listáját.</span><span class="sxs-lookup"><span data-stu-id="a6413-161">You can also get a list of available packages from other sources.</span></span> <span data-ttu-id="a6413-162">Például közösségi hozzájárult csomagok teljes listája megtalálható [Spark csomagok](http://spark-packages.org/).</span><span class="sxs-lookup"><span data-stu-id="a6413-162">For example, a complete list of community-contributed packages is available at [Spark Packages](http://spark-packages.org/).</span></span>

<span data-ttu-id="a6413-163">Ebből a cikkből látni fogja hogyan toouse hello [spark-fürt megosztott kötetei szolgáltatás](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) hello Jupyter notebook számára.</span><span class="sxs-lookup"><span data-stu-id="a6413-163">In this article, you will see how toouse hello [spark-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) package with hello Jupyter notebook.</span></span>

1. <span data-ttu-id="a6413-164">Nyissa meg a parancsértelmező beállításait.</span><span class="sxs-lookup"><span data-stu-id="a6413-164">Open interpreter settings.</span></span> <span data-ttu-id="a6413-165">Hello jobb felső sarokban, kattintson a hello jelentkezni a felhasználónevet, és kattintson a **parancsértelmező**.</span><span class="sxs-lookup"><span data-stu-id="a6413-165">From hello top-right corner, click hello logged in user name, and then click **Interpreter**.</span></span>
   
    <span data-ttu-id="a6413-166">![Indítsa el a parancsértelmező](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "kimeneti struktúra")</span><span class="sxs-lookup"><span data-stu-id="a6413-166">![Launch interpreter](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Hive output")</span></span>
2. <span data-ttu-id="a6413-167">Görgessen tooLivy parancsértelmező beállításait, és kattintson a **szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="a6413-167">Scroll tooLivy interpreter settings and then click **Edit**.</span></span>
   
    <span data-ttu-id="a6413-168">![Parancsértelmező beállításainak módosítása](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-use-external-package-1.png "parancsértelmező beállításainak módosítása")</span><span class="sxs-lookup"><span data-stu-id="a6413-168">![Change interpreter settings](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-use-external-package-1.png "Change interpreter settings")</span></span>
3. <span data-ttu-id="a6413-169">Adja hozzá egy új kulcsot, úgynevezett **livy.spark.jars.packages** és az értékét állítsa hello formátumban `group:id:version`.</span><span class="sxs-lookup"><span data-stu-id="a6413-169">Add a new key, called **livy.spark.jars.packages** and set its value in hello format `group:id:version`.</span></span> <span data-ttu-id="a6413-170">Ha azt szeretné, hogy toouse hello [spark-fürt megosztott kötetei szolgáltatás](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) csomag, akkor értékre kell állítani hello hello kulcs túl`com.databricks:spark-csv_2.10:1.4.0`.</span><span class="sxs-lookup"><span data-stu-id="a6413-170">So, if you want toouse hello [spark-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) package, you must set hello value of hello key too`com.databricks:spark-csv_2.10:1.4.0`.</span></span>
   
    <span data-ttu-id="a6413-171">![Parancsértelmező beállításainak módosítása](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-use-external-package-2.png "parancsértelmező beállításainak módosítása")</span><span class="sxs-lookup"><span data-stu-id="a6413-171">![Change interpreter settings](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-use-external-package-2.png "Change interpreter settings")</span></span>
   
    <span data-ttu-id="a6413-172">Kattintson a **mentése** , majd indítsa újra a hello Livy parancsértelmező.</span><span class="sxs-lookup"><span data-stu-id="a6413-172">Click **Save** and then restart hello Livy interpreter.</span></span>
4. <span data-ttu-id="a6413-173">**Tipp**: Ha azt szeretné, hogyan hello kulcs hello értékénél tooarrive fent, az alábbiakban megadott toounderstand módját.</span><span class="sxs-lookup"><span data-stu-id="a6413-173">**Tip**: If you want toounderstand how tooarrive at hello value of hello key entered above, here's how.</span></span>
   
    <span data-ttu-id="a6413-174">a.</span><span class="sxs-lookup"><span data-stu-id="a6413-174">a.</span></span> <span data-ttu-id="a6413-175">Keresse meg hello csomag hello Maven-tárházat.</span><span class="sxs-lookup"><span data-stu-id="a6413-175">Locate hello package in hello Maven Repository.</span></span> <span data-ttu-id="a6413-176">Ebben az oktatóanyagban használtuk [spark-fürt megosztott kötetei szolgáltatás](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).</span><span class="sxs-lookup"><span data-stu-id="a6413-176">For this tutorial, we used [spark-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).</span></span>
   
    <span data-ttu-id="a6413-177">b.</span><span class="sxs-lookup"><span data-stu-id="a6413-177">b.</span></span> <span data-ttu-id="a6413-178">Adattárból hello gyűjtse össze a hello értékeinek **GroupId**, **artifactid szakaszát**, és **verzió**.</span><span class="sxs-lookup"><span data-stu-id="a6413-178">From hello repository, gather hello values for **GroupId**, **ArtifactId**, and **Version**.</span></span>
   
    <span data-ttu-id="a6413-179">![Külső csomagok használata Jupyter notebook](./media/hdinsight-apache-spark-zeppelin-notebook/use-external-packages-with-jupyter.png "külső csomagok használata Jupyter notebook")</span><span class="sxs-lookup"><span data-stu-id="a6413-179">![Use external packages with Jupyter notebook](./media/hdinsight-apache-spark-zeppelin-notebook/use-external-packages-with-jupyter.png "Use external packages with Jupyter notebook")</span></span>
   
    <span data-ttu-id="a6413-180">c.</span><span class="sxs-lookup"><span data-stu-id="a6413-180">c.</span></span> <span data-ttu-id="a6413-181">Összefűzésére hello három értékek, egymástól kettősponttal (**:**).</span><span class="sxs-lookup"><span data-stu-id="a6413-181">Concatenate hello three values, separated by a colon (**:**).</span></span>
   
        com.databricks:spark-csv_2.10:1.4.0

## <a name="where-are-hello-zeppelin-notebooks-saved"></a><span data-ttu-id="a6413-182">Zeppelin notebookok mentett hol vannak a hello?</span><span class="sxs-lookup"><span data-stu-id="a6413-182">Where are hello Zeppelin notebooks saved?</span></span>
<span data-ttu-id="a6413-183">hello Zeppelin notebookok toohello fürt headnodes kerülnek mentésre.</span><span class="sxs-lookup"><span data-stu-id="a6413-183">hello Zeppelin notebooks are saved toohello cluster headnodes.</span></span> <span data-ttu-id="a6413-184">Így hello fürt törlésekor, hello notebookok is törlődik.</span><span class="sxs-lookup"><span data-stu-id="a6413-184">So, if you delete hello cluster, hello notebooks will be deleted as well.</span></span> <span data-ttu-id="a6413-185">Ha azt szeretné, toopreserve jegyzetfüzetek más fürtökön későbbi használatra, exportálnia kell őket futó hello feladatok befejezése után.</span><span class="sxs-lookup"><span data-stu-id="a6413-185">If you want toopreserve your notebooks for later use on other clusters, you must export them after you have finished running hello jobs.</span></span> <span data-ttu-id="a6413-186">a notebook tooexport kattintson hello **exportálása** ikon az alábbi hello ábrán látható módon.</span><span class="sxs-lookup"><span data-stu-id="a6413-186">tooexport a notebook, click hello **Export** icon as shown in hello image below.</span></span>

<span data-ttu-id="a6413-187">![Töltse le a notebook](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-download-notebook.png "letöltési hello notebook")</span><span class="sxs-lookup"><span data-stu-id="a6413-187">![Download notebook](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-download-notebook.png "Download hello notebook")</span></span>

<span data-ttu-id="a6413-188">Ez hello notebook egy JSON-fájl a letöltési helyre menti.</span><span class="sxs-lookup"><span data-stu-id="a6413-188">This saves hello notebook as a JSON file in your download location.</span></span>

## <a name="livy-session-management"></a><span data-ttu-id="a6413-189">Livy munkamenet-kezelés</span><span class="sxs-lookup"><span data-stu-id="a6413-189">Livy session management</span></span>
<span data-ttu-id="a6413-190">A Zeppelin jegyzetfüzet hello első kód bekezdés futtatásakor a új Livy munkamenet létrehozása a HDInsight Spark-fürtön.</span><span class="sxs-lookup"><span data-stu-id="a6413-190">When you run hello first code paragraph in your Zeppelin notebook, a new Livy session is created in your HDInsight Spark cluster.</span></span> <span data-ttu-id="a6413-191">A munkamenet által megosztott minden Zeppelin notebookok ezt követően hozzon létre.</span><span class="sxs-lookup"><span data-stu-id="a6413-191">This session is shared across all Zeppelin notebooks that you subsequently create.</span></span> <span data-ttu-id="a6413-192">Ha valamilyen okból hello a Livy munkamenet leállítása (fürt újraindítás, stb.), nem fogja tudni toorun feladatokat azok hello Zeppelin notebook.</span><span class="sxs-lookup"><span data-stu-id="a6413-192">If for some reason hello Livy session is killed (cluster reboot, etc.), you will not be able toorun jobs from hello Zeppelin notebook.</span></span>

<span data-ttu-id="a6413-193">Ebben az esetben hello Zeppelin jegyzetfüzet a futó feladatok megkezdése előtt a következő lépéseket kell elvégeznie.</span><span class="sxs-lookup"><span data-stu-id="a6413-193">In such a case, you must perform hello following steps before you can start running jobs from a Zeppelin notebook.</span></span> 

1. <span data-ttu-id="a6413-194">Indítsa újra a hello Zeppelin notebook Livy parancsértelmező hello.</span><span class="sxs-lookup"><span data-stu-id="a6413-194">Restart hello Livy interpreter from hello Zeppelin notebook.</span></span> <span data-ttu-id="a6413-195">toodo tehát parancsértelmező beállításai megnyitásához hello bejelentkezett felhasználó nevét a hello jobb felső sarokban, és kattintson **parancsértelmező**.</span><span class="sxs-lookup"><span data-stu-id="a6413-195">toodo so, open interpreter settings by clicking hello logged in user name from hello top-right corner, and then click **Interpreter**.</span></span>
   
    <span data-ttu-id="a6413-196">![Indítsa el a parancsértelmező](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "kimeneti struktúra")</span><span class="sxs-lookup"><span data-stu-id="a6413-196">![Launch interpreter](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Hive output")</span></span>
2. <span data-ttu-id="a6413-197">Görgessen tooLivy parancsértelmező beállításait, és kattintson a **indítsa újra a**.</span><span class="sxs-lookup"><span data-stu-id="a6413-197">Scroll tooLivy interpreter settings and then click **Restart**.</span></span>
   
    <span data-ttu-id="a6413-198">![Indítsa újra a hello Livy intepreter](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-restart-interpreter.png "hello Zeppelin intepreter újraindítása")</span><span class="sxs-lookup"><span data-stu-id="a6413-198">![Restart hello Livy intepreter](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-restart-interpreter.png "Restart hello Zeppelin intepreter")</span></span>
3. <span data-ttu-id="a6413-199">Futtassa a kódcella meglévő Zeppelin jegyzetfüzet.</span><span class="sxs-lookup"><span data-stu-id="a6413-199">Run a code cell from an existing Zeppelin notebook.</span></span> <span data-ttu-id="a6413-200">Ez létrehoz egy új Livy munkamenetet hello HDInsight-fürt.</span><span class="sxs-lookup"><span data-stu-id="a6413-200">This creates a new Livy session in hello HDInsight cluster.</span></span>

## <span data-ttu-id="a6413-201"><a name="seealso"></a>Lásd még:</span><span class="sxs-lookup"><span data-stu-id="a6413-201"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="a6413-202">Overview: Apache Spark on Azure HDInsight (Áttekintés: Apache Spark on Azure HDInsight)</span><span class="sxs-lookup"><span data-stu-id="a6413-202">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a><span data-ttu-id="a6413-203">Forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="a6413-203">Scenarios</span></span>
* [<span data-ttu-id="a6413-204">Spark és BI: Interaktív adatelemzés végrehajtása a Spark on HDInsight használatával, BI-eszközökkel</span><span class="sxs-lookup"><span data-stu-id="a6413-204">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="a6413-205">Spark és Machine Learning: A Spark on HDInsight használata az épület-hőmérséklet elemzésére HVAC-adatok alapján</span><span class="sxs-lookup"><span data-stu-id="a6413-205">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="a6413-206">Spark és Machine Learning: használja a Spark on HDInsight toopredict élelmiszervizsgálati eredmények</span><span class="sxs-lookup"><span data-stu-id="a6413-206">Spark with Machine Learning: Use Spark in HDInsight toopredict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="a6413-207">Spark Streaming: A Spark on HDInsight használata valós idejű streamelési alkalmazások összeállítására</span><span class="sxs-lookup"><span data-stu-id="a6413-207">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="a6413-208">A webhelynapló elemzése a Spark on HDInsight használatával</span><span class="sxs-lookup"><span data-stu-id="a6413-208">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a><span data-ttu-id="a6413-209">Alkalmazások létrehozása és futtatása</span><span class="sxs-lookup"><span data-stu-id="a6413-209">Create and run applications</span></span>
* [<span data-ttu-id="a6413-210">Önálló alkalmazás létrehozása a Scala használatával</span><span class="sxs-lookup"><span data-stu-id="a6413-210">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="a6413-211">Feladatok távoli futtatása Spark-fürtön a Livy használatával</span><span class="sxs-lookup"><span data-stu-id="a6413-211">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="a6413-212">Eszközök és bővítmények</span><span class="sxs-lookup"><span data-stu-id="a6413-212">Tools and extensions</span></span>
* [<span data-ttu-id="a6413-213">Toocreate IntelliJ IDEA HDInsight-eszközei beépülő használja, és küldje el a Spark Scala applicatons</span><span class="sxs-lookup"><span data-stu-id="a6413-213">Use HDInsight Tools Plugin for IntelliJ IDEA toocreate and submit Spark Scala applicatons</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="a6413-214">IntelliJ IDEA toodebug Spark-alkalmazások HDInsight-eszközei beépülő távolról használni</span><span class="sxs-lookup"><span data-stu-id="a6413-214">Use HDInsight Tools Plugin for IntelliJ IDEA toodebug Spark applications remotely</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="a6413-215">Jupyter notebookokhoz elérhető kernelek a HDInsight Spark-fürtjében</span><span class="sxs-lookup"><span data-stu-id="a6413-215">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="a6413-216">Külső csomagok használata Jupyter notebookokkal</span><span class="sxs-lookup"><span data-stu-id="a6413-216">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="a6413-217">Jupyter telepítse a számítógépre, és csatlakozzon a HDInsight Spark-fürt tooan</span><span class="sxs-lookup"><span data-stu-id="a6413-217">Install Jupyter on your computer and connect tooan HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a><span data-ttu-id="a6413-218">Erőforrások kezelése</span><span class="sxs-lookup"><span data-stu-id="a6413-218">Manage resources</span></span>
* [<span data-ttu-id="a6413-219">Az Azure HDInsight hello Apache Spark-fürt erőforrásainak kezelése</span><span class="sxs-lookup"><span data-stu-id="a6413-219">Manage resources for hello Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="a6413-220">Apache Spark-fürtön futó feladatok nyomon követése és hibakeresése a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="a6413-220">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md 







