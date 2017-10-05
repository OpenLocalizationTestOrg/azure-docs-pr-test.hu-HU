---
title: "Zeppelin notebookok használata Azure HDInsight az Apache Spark-fürt |} Microsoft Docs"
description: "Részletes útmutatók Apache Spark-fürtök on Azure HDInsight Zeppelin notebookok használata."
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
ms.openlocfilehash: 7fe5e3ec68e82945b972d2dd44f2cc3b8cf395d1
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="use-zeppelin-notebooks-with-apache-spark-cluster-on-azure-hdinsight"></a><span data-ttu-id="f659f-103">Zeppelin notebookok használata Azure HDInsight az Apache Spark-fürt</span><span class="sxs-lookup"><span data-stu-id="f659f-103">Use Zeppelin notebooks with Apache Spark cluster on Azure HDInsight</span></span>

<span data-ttu-id="f659f-104">HDInsight Spark-fürtjei Zeppelin notebookok használó feladatok futtatása Spark tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="f659f-104">HDInsight Spark clusters include Zeppelin notebooks that you can use to run Spark jobs.</span></span> <span data-ttu-id="f659f-105">Ebből a cikkből megismerheti a Zeppelin notebook használata a HDInsight-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="f659f-105">In this article, you learn how to use the Zeppelin notebook on an HDInsight cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="f659f-106">Zeppelin notebookok csak 1.6.3 Spark on HDInsight 3.5-ös és 2.1.0 Spark on HDInsight 3.6 érhetők el.</span><span class="sxs-lookup"><span data-stu-id="f659f-106">Zeppelin notebooks are available only for Spark 1.6.3 on HDInsight 3.5 and Spark 2.1.0 on HDInsight 3.6.</span></span>
>

<span data-ttu-id="f659f-107">**Előfeltételek:**</span><span class="sxs-lookup"><span data-stu-id="f659f-107">**Prerequisites:**</span></span>

* <span data-ttu-id="f659f-108">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="f659f-108">An Azure subscription.</span></span> <span data-ttu-id="f659f-109">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="f659f-109">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="f659f-110">A HDInsight az Apache Spark-fürt.</span><span class="sxs-lookup"><span data-stu-id="f659f-110">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="f659f-111">Útmutatásért lásd: [létrehozása az Apache Spark on Azure hdinsight clusters](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="f659f-111">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>

## <a name="launch-a-zeppelin-notebook"></a><span data-ttu-id="f659f-112">Indítsa el a Zeppelin notebook</span><span class="sxs-lookup"><span data-stu-id="f659f-112">Launch a Zeppelin notebook</span></span>
1. <span data-ttu-id="f659f-113">A Spark-fürt panelén kattintson **fürt irányítópult**, és kattintson a **Zeppelin Notebook**.</span><span class="sxs-lookup"><span data-stu-id="f659f-113">From the Spark cluster blade, click **Cluster Dashboard**, and then click **Zeppelin Notebook**.</span></span> <span data-ttu-id="f659f-114">Ha a rendszer felkéri rá, adja meg a fürthöz tartozó rendszergazdai hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="f659f-114">If prompted, enter the admin credentials for the cluster.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="f659f-115">Előfordulhat, hogy is eléri a Zeppelin Notebook a fürt a böngészőben nyissa meg a következő URL-címet.</span><span class="sxs-lookup"><span data-stu-id="f659f-115">You may also reach the Zeppelin Notebook for your cluster by opening the following URL in your browser.</span></span> <span data-ttu-id="f659f-116">Cserélje le a **CLUSTERNAME** elemet a fürt nevére:</span><span class="sxs-lookup"><span data-stu-id="f659f-116">Replace **CLUSTERNAME** with the name of your cluster:</span></span>
   > 
   > `https://CLUSTERNAME.azurehdinsight.net/zeppelin`
   > 
   > 
2. <span data-ttu-id="f659f-117">Hozzon létre új notebookot.</span><span class="sxs-lookup"><span data-stu-id="f659f-117">Create a new notebook.</span></span> <span data-ttu-id="f659f-118">A fejléc ablaktáblában kattintson **Notebook**, és kattintson a **hozzon létre új megjegyzés**.</span><span class="sxs-lookup"><span data-stu-id="f659f-118">From the header pane, click **Notebook**, and then click **Create New Note**.</span></span>
   
    <span data-ttu-id="f659f-119">![Létrehozhat egy újat Zeppelin](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-create-zeppelin-notebook.png "egy új Zeppelin notebook létrehozása")</span><span class="sxs-lookup"><span data-stu-id="f659f-119">![Create a new Zeppelin notebook](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-create-zeppelin-notebook.png "Create a new Zeppelin notebook")</span></span>
   
    <span data-ttu-id="f659f-120">Adja meg a notebook nevét, és kattintson **létrehozása Megjegyzés**.</span><span class="sxs-lookup"><span data-stu-id="f659f-120">Enter a name for the notebook, and then click **Create Note**.</span></span>
3. <span data-ttu-id="f659f-121">Ellenőrizze azt is, a notebook fejléc egy csatlakoztatott állapotát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="f659f-121">Also, make sure the notebook header shows a connected status.</span></span> <span data-ttu-id="f659f-122">A jobb felső sarokban, zöld pontot helyén.</span><span class="sxs-lookup"><span data-stu-id="f659f-122">It is denoted by a green dot in the top-right corner.</span></span>
   
    <span data-ttu-id="f659f-123">![Zeppelin notebook állapot](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-connected.png "Zeppelin notebook állapota")</span><span class="sxs-lookup"><span data-stu-id="f659f-123">![Zeppelin notebook status](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-connected.png "Zeppelin notebook status")</span></span>
4. <span data-ttu-id="f659f-124">Töltse be a mintaadatokat egy ideiglenes táblába.</span><span class="sxs-lookup"><span data-stu-id="f659f-124">Load sample data into a temporary table.</span></span> <span data-ttu-id="f659f-125">Spark-fürt hdinsightban a mintaadatfájlokat létrehozásakor **hvac.csv**, másolja a kapcsolódó tárfiók alatt **\HdiSamples\SensorSampleData\hvac**.</span><span class="sxs-lookup"><span data-stu-id="f659f-125">When you create a Spark cluster in HDInsight, the sample data file, **hvac.csv**, is copied to the associated storage account under **\HdiSamples\SensorSampleData\hvac**.</span></span>
   
    <span data-ttu-id="f659f-126">Az üres bekezdés, amely az új notebook alapértelmezés szerint létrejön illessze be a következő kódrészletet.</span><span class="sxs-lookup"><span data-stu-id="f659f-126">In the empty paragraph that is created by default in the new notebook, paste the following snippet.</span></span>
   
        %livy.spark
        //The above magic instructs Zeppelin to use the Livy Scala interpreter
   
        // Create an RDD using the default Spark context, sc
        val hvacText = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
   
        // Define a schema
        case class Hvac(date: String, time: String, targettemp: Integer, actualtemp: Integer, buildingID: String)
   
        // Map the values in the .csv file to the schema
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
   
    <span data-ttu-id="f659f-127">Nyomja le az **SHIFT + ENTER** , vagy kattintson a **lejátszása** gomb a bekezdés, a részlet futtatásához.</span><span class="sxs-lookup"><span data-stu-id="f659f-127">Press **SHIFT + ENTER** or click the **Play** button for the paragraph to run the snippet.</span></span> <span data-ttu-id="f659f-128">A bekezdés jobb-sarkában állapotának a kész, függőben lévő, FUTÓ befejezett kell előrehaladás.</span><span class="sxs-lookup"><span data-stu-id="f659f-128">The status on the right-corner of the paragraph should progress from READY, PENDING, RUNNING to FINISHED.</span></span> <span data-ttu-id="f659f-129">Egyazon paragraph alján megjelenik a kimenetet.</span><span class="sxs-lookup"><span data-stu-id="f659f-129">The output shows up at the bottom of the same paragraph.</span></span> <span data-ttu-id="f659f-130">A képernyőfelvételen a következőképpen néznek:</span><span class="sxs-lookup"><span data-stu-id="f659f-130">The screenshot looks like the following:</span></span>
   
    <span data-ttu-id="f659f-131">![Hozzon létre egy ideiglenes táblából a nyers adatoktól](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-load-data.png "a nyers adatoktól ideiglenes tábla létrehozása")</span><span class="sxs-lookup"><span data-stu-id="f659f-131">![Create a temporary table from raw data](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-load-data.png "Create a temporary table from raw data")</span></span>
   
    <span data-ttu-id="f659f-132">Minden bekezdéséhez címet is megadhatja.</span><span class="sxs-lookup"><span data-stu-id="f659f-132">You can also provide a title to each paragraph.</span></span> <span data-ttu-id="f659f-133">Kattintson a jobb oldali sarokban a **beállítások** ikonra, végül **cím megjelenítése a**.</span><span class="sxs-lookup"><span data-stu-id="f659f-133">From the right-hand corner, click the **Settings** icon, and then click **Show title**.</span></span>
5. <span data-ttu-id="f659f-134">Most futtathatja a Spark SQL-utasítások a **hvac** tábla.</span><span class="sxs-lookup"><span data-stu-id="f659f-134">You can now run Spark SQL statements on the **hvac** table.</span></span> <span data-ttu-id="f659f-135">Illessze be a következő lekérdezést egy új bekezdésben szereplő esetekben.</span><span class="sxs-lookup"><span data-stu-id="f659f-135">Paste the following query in a new paragraph.</span></span> <span data-ttu-id="f659f-136">A lekérdezés lekéri az épület-Azonosítót és a különbség a cél és a tényleges hőmérsékletek minden készítéséhez egy adott időpontban.</span><span class="sxs-lookup"><span data-stu-id="f659f-136">The query retrieves the building ID and the difference between the target and actual temperatures for each building on a given date.</span></span> <span data-ttu-id="f659f-137">Nyomja le az **SHIFT + ENTER**.</span><span class="sxs-lookup"><span data-stu-id="f659f-137">Press **SHIFT + ENTER**.</span></span>
   
        %sql
        select buildingID, (targettemp - actualtemp) as temp_diff, date from hvac where date = "6/1/13" 
   
    <span data-ttu-id="f659f-138">A **% sql** nyilatkozat elején közli a notebook használatához a Livy Scala értelmező.</span><span class="sxs-lookup"><span data-stu-id="f659f-138">The **%sql** statement at the beginning tells the notebook to use the Livy Scala interpreter.</span></span>
   
    <span data-ttu-id="f659f-139">Az alábbi képernyőfelvételen látható a kimenetet.</span><span class="sxs-lookup"><span data-stu-id="f659f-139">The following screenshot shows the output.</span></span>
   
    <span data-ttu-id="f659f-140">![A notebook használata Spark SQL-utasítás futtatása](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-spark-query-1.png "a notebook használata Spark SQL-utasítás futtatása")</span><span class="sxs-lookup"><span data-stu-id="f659f-140">![Run a Spark SQL statement using the notebook](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-spark-query-1.png "Run a Spark SQL statement using the notebook")</span></span>
   
     <span data-ttu-id="f659f-141">Kattintson a megjelenítési beállítások (kiemelt téglalap) az azonos kimenethez tartozó különböző felelősséget közötti váltáshoz.</span><span class="sxs-lookup"><span data-stu-id="f659f-141">Click the display options (highlighted in rectangle) to switch between different representations for the same output.</span></span> <span data-ttu-id="f659f-142">Kattintson a **beállítások** választhat, milyen consitutes a kulcs értékeit a kimenetben.</span><span class="sxs-lookup"><span data-stu-id="f659f-142">Click **Settings** to choose what consitutes the key and values in the output.</span></span> <span data-ttu-id="f659f-143">A fenti képernyőfelvételen használ **buildingID** átlaga és a kulcs, **temp_diff** értékeként.</span><span class="sxs-lookup"><span data-stu-id="f659f-143">The screen capture above uses **buildingID** as the key and the average of **temp_diff** as the value.</span></span>
6. <span data-ttu-id="f659f-144">A lekérdezési változók használata Spark SQL-utasítások is futtathatja.</span><span class="sxs-lookup"><span data-stu-id="f659f-144">You can also run Spark SQL statements using variables in the query.</span></span> <span data-ttu-id="f659f-145">A következő kódrészletet bemutatja, hogyan adhat meg egy változót **Temp**, a lehetséges értékek a le szeretné kérdezni a lekérdezésben.</span><span class="sxs-lookup"><span data-stu-id="f659f-145">The next snippet shows how to define a variable, **Temp**, in the query with the possible values you want to query with.</span></span> <span data-ttu-id="f659f-146">A lekérdezés első futtatásakor egy legördülő lista automatikusan töltődik változó megadott értékekre.</span><span class="sxs-lookup"><span data-stu-id="f659f-146">When you first run the query, a drop-down is automatically populated with the values you specified for the variable.</span></span>
   
        %sql
        select buildingID, date, targettemp, (targettemp - actualtemp) as temp_diff from hvac where targettemp > "${Temp = 65,65|75|85}" 
   
    <span data-ttu-id="f659f-147">Ezt a kódrészletet illessze be egy új bekezdés, és nyomja le az **SHIFT + ENTER**.</span><span class="sxs-lookup"><span data-stu-id="f659f-147">Paste this snippet in a new paragraph and press **SHIFT + ENTER**.</span></span> <span data-ttu-id="f659f-148">Az alábbi képernyőfelvételen látható a kimenetet.</span><span class="sxs-lookup"><span data-stu-id="f659f-148">The following screenshot shows the output.</span></span>
   
    <span data-ttu-id="f659f-149">![A notebook használata Spark SQL-utasítás futtatása](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-spark-query-2.png "a notebook használata Spark SQL-utasítás futtatása")</span><span class="sxs-lookup"><span data-stu-id="f659f-149">![Run a Spark SQL statement using the notebook](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-spark-query-2.png "Run a Spark SQL statement using the notebook")</span></span>
   
    <span data-ttu-id="f659f-150">A lekérdezések egy új értéket a legördülő listán válasszon, és futtassa újból a lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="f659f-150">For subsequent queries, you can select a new value from the drop-down and run the query again.</span></span> <span data-ttu-id="f659f-151">Kattintson a **beállítások** választhat, milyen consitutes a kulcs értékeit a kimenetben.</span><span class="sxs-lookup"><span data-stu-id="f659f-151">Click **Settings** to choose what consitutes the key and values in the output.</span></span> <span data-ttu-id="f659f-152">A fenti képernyőfelvételen használ **buildingID** átlaga kulcsként **temp_diff** értékeként, és **targettemp** csoportot.</span><span class="sxs-lookup"><span data-stu-id="f659f-152">The screen capture above uses **buildingID** as the key, the average of **temp_diff** as the value, and **targettemp** as the group.</span></span>
7. <span data-ttu-id="f659f-153">Indítsa újra a Livy értelmező kilép az alkalmazásból.</span><span class="sxs-lookup"><span data-stu-id="f659f-153">Restart the Livy interpreter to exit the application.</span></span> <span data-ttu-id="f659f-154">Ehhez az szükséges, parancsértelmező beállításai megnyitásához a bejelentkezett felhasználó nevében a jobb felső sarokban a, és kattintson a **parancsértelmező**.</span><span class="sxs-lookup"><span data-stu-id="f659f-154">To do so, open interpreter settings by clicking the logged in user name from the top-right corner, and then click **Interpreter**.</span></span>
   
    <span data-ttu-id="f659f-155">![Indítsa el a parancsértelmező](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "kimeneti struktúra")</span><span class="sxs-lookup"><span data-stu-id="f659f-155">![Launch interpreter](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Hive output")</span></span>
8. <span data-ttu-id="f659f-156">Görgessen a Livy parancsértelmező beállításait, és kattintson a **indítsa újra a**.</span><span class="sxs-lookup"><span data-stu-id="f659f-156">Scroll to Livy interpreter settings and then click **Restart**.</span></span>
   
    <span data-ttu-id="f659f-157">![Indítsa újra a Livy intepreter](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-restart-interpreter.png "indítsa újra a Zeppelin intepreter")</span><span class="sxs-lookup"><span data-stu-id="f659f-157">![Restart the Livy intepreter](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-restart-interpreter.png "Restart the Zeppelin intepreter")</span></span>

## <a name="how-do-i-use-external-packages-with-the-notebook"></a><span data-ttu-id="f659f-158">Hogyan külső csomagok használata a notebook?</span><span class="sxs-lookup"><span data-stu-id="f659f-158">How do I use external packages with the notebook?</span></span>
<span data-ttu-id="f659f-159">Konfigurálhatja a Zeppelin notebook az Apache Spark-fürt a HDInsight (Linux), amelyek nem szerepel az a-kész a fürt külső, közösségi hozzájárult csomagok használata.</span><span class="sxs-lookup"><span data-stu-id="f659f-159">You can configure the Zeppelin notebook in Apache Spark cluster on HDInsight (Linux) to use external, community-contributed packages that are not included out-of-the-box in the cluster.</span></span> <span data-ttu-id="f659f-160">Kereshet az [Maven-tárház](http://search.maven.org/) csomagok rendelkezésre álló teljes listáját.</span><span class="sxs-lookup"><span data-stu-id="f659f-160">You can search the [Maven repository](http://search.maven.org/) for the complete list of packages that are available.</span></span> <span data-ttu-id="f659f-161">Más forrásból is megkapható elérhető csomagok listáját.</span><span class="sxs-lookup"><span data-stu-id="f659f-161">You can also get a list of available packages from other sources.</span></span> <span data-ttu-id="f659f-162">Például közösségi hozzájárult csomagok teljes listája megtalálható [Spark csomagok](http://spark-packages.org/).</span><span class="sxs-lookup"><span data-stu-id="f659f-162">For example, a complete list of community-contributed packages is available at [Spark Packages](http://spark-packages.org/).</span></span>

<span data-ttu-id="f659f-163">Ebből a cikkből látni fogja a használata a [spark-fürt megosztott kötetei szolgáltatás](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) a Jupyter notebookkal csomag.</span><span class="sxs-lookup"><span data-stu-id="f659f-163">In this article, you will see how to use the [spark-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) package with the Jupyter notebook.</span></span>

1. <span data-ttu-id="f659f-164">Nyissa meg a parancsértelmező beállításait.</span><span class="sxs-lookup"><span data-stu-id="f659f-164">Open interpreter settings.</span></span> <span data-ttu-id="f659f-165">A jobb felső sarokban, kattintson a bejelentkezett felhasználó nevében, majd kattintson **parancsértelmező**.</span><span class="sxs-lookup"><span data-stu-id="f659f-165">From the top-right corner, click the logged in user name, and then click **Interpreter**.</span></span>
   
    <span data-ttu-id="f659f-166">![Indítsa el a parancsértelmező](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "kimeneti struktúra")</span><span class="sxs-lookup"><span data-stu-id="f659f-166">![Launch interpreter](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Hive output")</span></span>
2. <span data-ttu-id="f659f-167">Görgessen a Livy parancsértelmező beállításait, és kattintson a **szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="f659f-167">Scroll to Livy interpreter settings and then click **Edit**.</span></span>
   
    <span data-ttu-id="f659f-168">![Parancsértelmező beállításainak módosítása](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-use-external-package-1.png "parancsértelmező beállításainak módosítása")</span><span class="sxs-lookup"><span data-stu-id="f659f-168">![Change interpreter settings](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-use-external-package-1.png "Change interpreter settings")</span></span>
3. <span data-ttu-id="f659f-169">Adja hozzá egy új kulcsot, úgynevezett **livy.spark.jars.packages** és az értékét állítsa a formátumban `group:id:version`.</span><span class="sxs-lookup"><span data-stu-id="f659f-169">Add a new key, called **livy.spark.jars.packages** and set its value in the format `group:id:version`.</span></span> <span data-ttu-id="f659f-170">Ha szeretné használni a [spark-fürt megosztott kötetei szolgáltatás](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) csomag, meg kell adni a kulcs értékének `com.databricks:spark-csv_2.10:1.4.0`.</span><span class="sxs-lookup"><span data-stu-id="f659f-170">So, if you want to use the [spark-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) package, you must set the value of the key to `com.databricks:spark-csv_2.10:1.4.0`.</span></span>
   
    <span data-ttu-id="f659f-171">![Parancsértelmező beállításainak módosítása](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-use-external-package-2.png "parancsértelmező beállításainak módosítása")</span><span class="sxs-lookup"><span data-stu-id="f659f-171">![Change interpreter settings](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-use-external-package-2.png "Change interpreter settings")</span></span>
   
    <span data-ttu-id="f659f-172">Kattintson a **mentése** , majd indítsa újra a Livy értelmező.</span><span class="sxs-lookup"><span data-stu-id="f659f-172">Click **Save** and then restart the Livy interpreter.</span></span>
4. <span data-ttu-id="f659f-173">**Tipp**: Ha azt szeretné, hogy hogyan megérkeznek a kulcsnak az értéke a fent megadott, hogyan.</span><span class="sxs-lookup"><span data-stu-id="f659f-173">**Tip**: If you want to understand how to arrive at the value of the key entered above, here's how.</span></span>
   
    <span data-ttu-id="f659f-174">a.</span><span class="sxs-lookup"><span data-stu-id="f659f-174">a.</span></span> <span data-ttu-id="f659f-175">Keresse meg a csomagot a Maven-tárházat.</span><span class="sxs-lookup"><span data-stu-id="f659f-175">Locate the package in the Maven Repository.</span></span> <span data-ttu-id="f659f-176">Ebben az oktatóanyagban használtuk [spark-fürt megosztott kötetei szolgáltatás](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).</span><span class="sxs-lookup"><span data-stu-id="f659f-176">For this tutorial, we used [spark-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).</span></span>
   
    <span data-ttu-id="f659f-177">b.</span><span class="sxs-lookup"><span data-stu-id="f659f-177">b.</span></span> <span data-ttu-id="f659f-178">A tárház gyűjtse össze a értékeinek **GroupId**, **artifactid szakaszát**, és **verzió**.</span><span class="sxs-lookup"><span data-stu-id="f659f-178">From the repository, gather the values for **GroupId**, **ArtifactId**, and **Version**.</span></span>
   
    <span data-ttu-id="f659f-179">![Külső csomagok használata Jupyter notebook](./media/hdinsight-apache-spark-zeppelin-notebook/use-external-packages-with-jupyter.png "külső csomagok használata Jupyter notebook")</span><span class="sxs-lookup"><span data-stu-id="f659f-179">![Use external packages with Jupyter notebook](./media/hdinsight-apache-spark-zeppelin-notebook/use-external-packages-with-jupyter.png "Use external packages with Jupyter notebook")</span></span>
   
    <span data-ttu-id="f659f-180">c.</span><span class="sxs-lookup"><span data-stu-id="f659f-180">c.</span></span> <span data-ttu-id="f659f-181">A következő három érték, egymástól kettősponttal összefűzésére (**:**).</span><span class="sxs-lookup"><span data-stu-id="f659f-181">Concatenate the three values, separated by a colon (**:**).</span></span>
   
        com.databricks:spark-csv_2.10:1.4.0

## <a name="where-are-the-zeppelin-notebooks-saved"></a><span data-ttu-id="f659f-182">A Zeppelin notebookok tároló?</span><span class="sxs-lookup"><span data-stu-id="f659f-182">Where are the Zeppelin notebooks saved?</span></span>
<span data-ttu-id="f659f-183">A fürt headnodes a Zeppelin notebookok kerülnek.</span><span class="sxs-lookup"><span data-stu-id="f659f-183">The Zeppelin notebooks are saved to the cluster headnodes.</span></span> <span data-ttu-id="f659f-184">Ezért, ha törli a fürtöt, a notebookok is törlődik.</span><span class="sxs-lookup"><span data-stu-id="f659f-184">So, if you delete the cluster, the notebooks will be deleted as well.</span></span> <span data-ttu-id="f659f-185">Ha meg szeretné őrizni későbbi használatra más fürtökön jegyzetfüzetek, exportálnia kell őket futó a feladatok befejezése után.</span><span class="sxs-lookup"><span data-stu-id="f659f-185">If you want to preserve your notebooks for later use on other clusters, you must export them after you have finished running the jobs.</span></span> <span data-ttu-id="f659f-186">A notebook exportálásához kattintson a **exportálása** ikon az alábbi ábrán látható módon.</span><span class="sxs-lookup"><span data-stu-id="f659f-186">To export a notebook, click the **Export** icon as shown in the image below.</span></span>

<span data-ttu-id="f659f-187">![Töltse le a notebook](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-download-notebook.png "töltse le a notebook")</span><span class="sxs-lookup"><span data-stu-id="f659f-187">![Download notebook](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-download-notebook.png "Download the notebook")</span></span>

<span data-ttu-id="f659f-188">Ez a notebook egy JSON-fájl a letöltési helyre menti.</span><span class="sxs-lookup"><span data-stu-id="f659f-188">This saves the notebook as a JSON file in your download location.</span></span>

## <a name="livy-session-management"></a><span data-ttu-id="f659f-189">Livy munkamenet-kezelés</span><span class="sxs-lookup"><span data-stu-id="f659f-189">Livy session management</span></span>
<span data-ttu-id="f659f-190">A kód első bekezdésben szereplő esetekben a Zeppelin jegyzetfüzet futtatásakor a új Livy munkamenet létrehozása a HDInsight Spark-fürtön.</span><span class="sxs-lookup"><span data-stu-id="f659f-190">When you run the first code paragraph in your Zeppelin notebook, a new Livy session is created in your HDInsight Spark cluster.</span></span> <span data-ttu-id="f659f-191">A munkamenet által megosztott minden Zeppelin notebookok ezt követően hozzon létre.</span><span class="sxs-lookup"><span data-stu-id="f659f-191">This session is shared across all Zeppelin notebooks that you subsequently create.</span></span> <span data-ttu-id="f659f-192">Ha valamilyen okból a munkamenet Livy leállítása (fürt újraindítás, stb.), nem kell a Zeppelin notebook származó feladatok futtathatók.</span><span class="sxs-lookup"><span data-stu-id="f659f-192">If for some reason the Livy session is killed (cluster reboot, etc.), you will not be able to run jobs from the Zeppelin notebook.</span></span>

<span data-ttu-id="f659f-193">Ilyen esetben a következő lépéseket kell elvégezni, a Zeppelin jegyzetfüzet futó feladatok megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="f659f-193">In such a case, you must perform the following steps before you can start running jobs from a Zeppelin notebook.</span></span> 

1. <span data-ttu-id="f659f-194">Indítsa újra a Livy értelmező a Zeppelin notebook a.</span><span class="sxs-lookup"><span data-stu-id="f659f-194">Restart the Livy interpreter from the Zeppelin notebook.</span></span> <span data-ttu-id="f659f-195">Ehhez az szükséges, parancsértelmező beállításai megnyitásához a bejelentkezett felhasználó nevében a jobb felső sarokban a, és kattintson a **parancsértelmező**.</span><span class="sxs-lookup"><span data-stu-id="f659f-195">To do so, open interpreter settings by clicking the logged in user name from the top-right corner, and then click **Interpreter**.</span></span>
   
    <span data-ttu-id="f659f-196">![Indítsa el a parancsértelmező](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "kimeneti struktúra")</span><span class="sxs-lookup"><span data-stu-id="f659f-196">![Launch interpreter](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Hive output")</span></span>
2. <span data-ttu-id="f659f-197">Görgessen a Livy parancsértelmező beállításait, és kattintson a **indítsa újra a**.</span><span class="sxs-lookup"><span data-stu-id="f659f-197">Scroll to Livy interpreter settings and then click **Restart**.</span></span>
   
    <span data-ttu-id="f659f-198">![Indítsa újra a Livy intepreter](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-restart-interpreter.png "indítsa újra a Zeppelin intepreter")</span><span class="sxs-lookup"><span data-stu-id="f659f-198">![Restart the Livy intepreter](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-restart-interpreter.png "Restart the Zeppelin intepreter")</span></span>
3. <span data-ttu-id="f659f-199">Futtassa a kódcella meglévő Zeppelin jegyzetfüzet.</span><span class="sxs-lookup"><span data-stu-id="f659f-199">Run a code cell from an existing Zeppelin notebook.</span></span> <span data-ttu-id="f659f-200">Ez létrehoz egy új Livy munkamenetet a HDInsight-fürt.</span><span class="sxs-lookup"><span data-stu-id="f659f-200">This creates a new Livy session in the HDInsight cluster.</span></span>

## <span data-ttu-id="f659f-201"><a name="seealso"></a>Lásd még:</span><span class="sxs-lookup"><span data-stu-id="f659f-201"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="f659f-202">Overview: Apache Spark on Azure HDInsight (Áttekintés: Apache Spark on Azure HDInsight)</span><span class="sxs-lookup"><span data-stu-id="f659f-202">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a><span data-ttu-id="f659f-203">Forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="f659f-203">Scenarios</span></span>
* [<span data-ttu-id="f659f-204">Spark és BI: Interaktív adatelemzés végrehajtása a Spark on HDInsight használatával, BI-eszközökkel</span><span class="sxs-lookup"><span data-stu-id="f659f-204">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="f659f-205">Spark és Machine Learning: A Spark on HDInsight használata az épület-hőmérséklet elemzésére HVAC-adatok alapján</span><span class="sxs-lookup"><span data-stu-id="f659f-205">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="f659f-206">Spark és Machine Learning: A Spark on HDInsight használata az élelmiszervizsgálati eredmények előrejelzésére</span><span class="sxs-lookup"><span data-stu-id="f659f-206">Spark with Machine Learning: Use Spark in HDInsight to predict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="f659f-207">Spark Streaming: A Spark on HDInsight használata valós idejű streamelési alkalmazások összeállítására</span><span class="sxs-lookup"><span data-stu-id="f659f-207">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="f659f-208">A webhelynapló elemzése a Spark on HDInsight használatával</span><span class="sxs-lookup"><span data-stu-id="f659f-208">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a><span data-ttu-id="f659f-209">Alkalmazások létrehozása és futtatása</span><span class="sxs-lookup"><span data-stu-id="f659f-209">Create and run applications</span></span>
* [<span data-ttu-id="f659f-210">Önálló alkalmazás létrehozása a Scala használatával</span><span class="sxs-lookup"><span data-stu-id="f659f-210">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="f659f-211">Feladatok távoli futtatása Spark-fürtön a Livy használatával</span><span class="sxs-lookup"><span data-stu-id="f659f-211">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="f659f-212">Eszközök és bővítmények</span><span class="sxs-lookup"><span data-stu-id="f659f-212">Tools and extensions</span></span>
* [<span data-ttu-id="f659f-213">Az IntelliJ IDEA HDInsight-eszközei beépülő moduljának használata Spark Scala-alkalmazások létrehozásához és elküldéséhez</span><span class="sxs-lookup"><span data-stu-id="f659f-213">Use HDInsight Tools Plugin for IntelliJ IDEA to create and submit Spark Scala applicatons</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="f659f-214">Az IntelliJ IDEA HDInsight-eszközei beépülő moduljának használata Spark-alkalmazások távoli hibaelhárításához</span><span class="sxs-lookup"><span data-stu-id="f659f-214">Use HDInsight Tools Plugin for IntelliJ IDEA to debug Spark applications remotely</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="f659f-215">Jupyter notebookokhoz elérhető kernelek a HDInsight Spark-fürtjében</span><span class="sxs-lookup"><span data-stu-id="f659f-215">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="f659f-216">Külső csomagok használata Jupyter notebookokkal</span><span class="sxs-lookup"><span data-stu-id="f659f-216">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="f659f-217">A Jupyter telepítése a számítógépre, majd csatlakozás egy HDInsight Spark-fürthöz</span><span class="sxs-lookup"><span data-stu-id="f659f-217">Install Jupyter on your computer and connect to an HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a><span data-ttu-id="f659f-218">Erőforrások kezelése</span><span class="sxs-lookup"><span data-stu-id="f659f-218">Manage resources</span></span>
* [<span data-ttu-id="f659f-219">Apache Spark-fürt erőforrásainak kezelése az Azure HDInsightban</span><span class="sxs-lookup"><span data-stu-id="f659f-219">Manage resources for the Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="f659f-220">Apache Spark-fürtön futó feladatok nyomon követése és hibakeresése a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="f659f-220">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md 







