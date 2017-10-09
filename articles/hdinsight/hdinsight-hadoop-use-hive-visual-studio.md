---
title: a Visual Studio - Azure HDInsight (Hadoop) a Data Lake tools aaaHive |} Microsoft Docs
description: "Ismerje meg, hogyan toouse hello Data Lake tools az Apache Hive Visual Studio toorun az Azure HDInsight az Apache Hadoop-lekérdezések."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 2b3e672a-1195-4fa5-afb7-b7b73937bfbe
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/07/2017
ms.author: larryfr
ms.openlocfilehash: dc76974c02cf68bcf701b2b155842c9e9c5cb988
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="run-hive-queries-using-hello-data-lake-tools-for-visual-studio"></a><span data-ttu-id="a49a1-103">Futtathat Hive-lekérdezéseket hello Data Lake tools for Visual Studio használatával</span><span class="sxs-lookup"><span data-stu-id="a49a1-103">Run Hive queries using hello Data Lake tools for Visual Studio</span></span>

<span data-ttu-id="a49a1-104">Ismerje meg, hogyan toouse hello Data Lake Visual Studio tooquery Apache Hive eszközei.</span><span class="sxs-lookup"><span data-stu-id="a49a1-104">Learn how toouse hello Data Lake tools for Visual Studio tooquery Apache Hive.</span></span> <span data-ttu-id="a49a1-105">hello Data Lake tools lehetővé teszik tooeasily létrehozása, küldje el, és Azure hdinsight Hive-lekérdezések tooHadoop figyelése.</span><span class="sxs-lookup"><span data-stu-id="a49a1-105">hello Data Lake tools allow you tooeasily create, submit, and monitor Hive queries tooHadoop on Azure HDInsight.</span></span>

## <span data-ttu-id="a49a1-106"><a id="prereq"></a>Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a49a1-106"><a id="prereq"></a>Prerequisites</span></span>

* <span data-ttu-id="a49a1-107">(A HDInsight Hadoop) Azure HDInsight-fürtök</span><span class="sxs-lookup"><span data-stu-id="a49a1-107">An Azure HDInsight (Hadoop on HDInsight) cluster</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="a49a1-108">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="a49a1-108">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="a49a1-109">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="a49a1-109">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="a49a1-110">A Visual Studio (hello a következő verziók egyike):</span><span class="sxs-lookup"><span data-stu-id="a49a1-110">Visual Studio (one of hello following versions):</span></span>

    * <span data-ttu-id="a49a1-111">A Visual Studio 2013 Community/Professional/Premium/Ultimate Update 4</span><span class="sxs-lookup"><span data-stu-id="a49a1-111">Visual Studio 2013 Community/Professional/Premium/Ultimate with Update 4</span></span>

    * <span data-ttu-id="a49a1-112">A Visual Studio 2015 (minden kiadás)</span><span class="sxs-lookup"><span data-stu-id="a49a1-112">Visual Studio 2015 (any edition)</span></span>

    * <span data-ttu-id="a49a1-113">A Visual Studio 2017 (minden kiadás)</span><span class="sxs-lookup"><span data-stu-id="a49a1-113">Visual Studio 2017 (any edition)</span></span>

* <span data-ttu-id="a49a1-114">A HDInsight tools for Visual Studio vagy az Azure Data Lake tools for Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a49a1-114">HDInsight tools for Visual Studio or Azure Data Lake tools for Visual Studio.</span></span> <span data-ttu-id="a49a1-115">Lásd: [első lépések a Visual Studio Hadoop tools for HDInsight használatával](hdinsight-hadoop-visual-studio-tools-get-started.md) telepítésének és konfigurálásának hello eszközök információt.</span><span class="sxs-lookup"><span data-stu-id="a49a1-115">See [Get started using Visual Studio Hadoop tools for HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md) for information on installing and configuring hello tools.</span></span>

## <span data-ttu-id="a49a1-116"><a id="run"></a>Futtathat Hive-lekérdezéseket hello Visual Studio használatával</span><span class="sxs-lookup"><span data-stu-id="a49a1-116"><a id="run"></a> Run Hive queries using hello Visual Studio</span></span>

1. <span data-ttu-id="a49a1-117">Nyissa meg **Visual Studio** válassza **új** > **projekt** > **Azure Data Lake** > **HIVE** > **Hive alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="a49a1-117">Open **Visual Studio** and select **New** > **Project** > **Azure Data Lake** > **HIVE** > **Hive Application**.</span></span> <span data-ttu-id="a49a1-118">Adja meg a projekt nevét.</span><span class="sxs-lookup"><span data-stu-id="a49a1-118">Provide a name for this project.</span></span>

2. <span data-ttu-id="a49a1-119">Nyissa meg hello **Script.hql** jön létre a projektet, és illessze be a következő HiveQL utasítások hello:</span><span class="sxs-lookup"><span data-stu-id="a49a1-119">Open hello **Script.hql** file that is created with this project, and paste in hello following HiveQL statements:</span></span>

   ```hiveql
   set hive.execution.engine=tez;
   DROP TABLE log4jLogs;
   CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
   ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
   STORED AS TEXTFILE LOCATION '/example/data/';
   SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND  INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;
   ```

    <span data-ttu-id="a49a1-120">Ezekre az utasításokra hajtsa végre a következő műveletek hello:</span><span class="sxs-lookup"><span data-stu-id="a49a1-120">These statements perform hello following actions:</span></span>

   * <span data-ttu-id="a49a1-121">`DROP TABLE`: Ha hello tábla létezik, a jelen nyilatkozatban törli őket.</span><span class="sxs-lookup"><span data-stu-id="a49a1-121">`DROP TABLE`: If hello table exists, this statement deletes it.</span></span>

   * <span data-ttu-id="a49a1-122">`CREATE EXTERNAL TABLE`: Táblát hoz létre egy új "külső" struktúra.</span><span class="sxs-lookup"><span data-stu-id="a49a1-122">`CREATE EXTERNAL TABLE`: Creates a new 'external' table in Hive.</span></span> <span data-ttu-id="a49a1-123">Külső táblák hello tábla definíciójában csak struktúra (hello adatok marad az eredeti helyükre hello) tárolja.</span><span class="sxs-lookup"><span data-stu-id="a49a1-123">External tables only store hello table definition in Hive (hello data is left in hello original location).</span></span>

     > [!NOTE]
     > <span data-ttu-id="a49a1-124">Külső táblák kell használni, amikor hello frissíteni az külső forrás alapjául szolgáló adatok toobe várt.</span><span class="sxs-lookup"><span data-stu-id="a49a1-124">External tables should be used when you expect hello underlying data toobe updated by an external source.</span></span> <span data-ttu-id="a49a1-125">Például a MapReduce feladatot vagy Azure-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="a49a1-125">For example, a MapReduce job or Azure service.</span></span>
     >
     > <span data-ttu-id="a49a1-126">A külső tábla eldobása does **nem** törölni hello adatokat, csak a hello tábla definíciójában.</span><span class="sxs-lookup"><span data-stu-id="a49a1-126">Dropping an external table does **not** delete hello data, only hello table definition.</span></span>

   * <span data-ttu-id="a49a1-127">`ROW FORMAT`: Közli a Hive hello adatok formázását.</span><span class="sxs-lookup"><span data-stu-id="a49a1-127">`ROW FORMAT`: Tells Hive how hello data is formatted.</span></span> <span data-ttu-id="a49a1-128">Ebben az esetben az egyes naplókon hello mezők szóközzel elválasztva.</span><span class="sxs-lookup"><span data-stu-id="a49a1-128">In this case, hello fields in each log are separated by a space.</span></span>

   * <span data-ttu-id="a49a1-129">`STORED AS TEXTFILE LOCATION`: Közli Hive adott hello tárolja (hello példaadatokat/könyvtár) és szövegként tárolt.</span><span class="sxs-lookup"><span data-stu-id="a49a1-129">`STORED AS TEXTFILE LOCATION`: Tells Hive where hello data is stored (hello example/data directory) and that it is stored as text.</span></span>

   * <span data-ttu-id="a49a1-130">`SELECT`: Jelölje ki az összes sorok számát ahol oszlop `t4` hello értéket tartalmaz `[ERROR]`.</span><span class="sxs-lookup"><span data-stu-id="a49a1-130">`SELECT`: Select a count of all rows where column `t4` contains hello value `[ERROR]`.</span></span> <span data-ttu-id="a49a1-131">A jelen nyilatkozat értéket ad vissza, `3` mert három ezt az értéket tartalmazó sorok.</span><span class="sxs-lookup"><span data-stu-id="a49a1-131">This statement returns a value of `3` because there are three rows that contain this value.</span></span>

   * <span data-ttu-id="a49a1-132">`INPUT__FILE__NAME LIKE '%.log'`-Közli struktúra, hogy azt kell csak vissza adatokat fájlok végződése. napló.</span><span class="sxs-lookup"><span data-stu-id="a49a1-132">`INPUT__FILE__NAME LIKE '%.log'` - Tells Hive that we should only return data from files ending in .log.</span></span> <span data-ttu-id="a49a1-133">Ehhez a záradékhoz hello keresési toohello sample.log adatait tartalmazó fájl hello korlátozza.</span><span class="sxs-lookup"><span data-stu-id="a49a1-133">This clause restricts hello search toohello sample.log file that contains hello data.</span></span>

3. <span data-ttu-id="a49a1-134">Hello eszköztáron válassza a hello **HDInsight-fürt** toouse szeretné az ehhez a lekérdezéshez.</span><span class="sxs-lookup"><span data-stu-id="a49a1-134">From hello toolbar, select hello **HDInsight Cluster** that you want toouse for this query.</span></span> <span data-ttu-id="a49a1-135">Válassza ki **Submit** toorun hello utasításokra egy Hive-feladatot.</span><span class="sxs-lookup"><span data-stu-id="a49a1-135">Select **Submit** toorun hello statements as a Hive job.</span></span>

   ![Küldje el sáv](./media/hdinsight-hadoop-use-hive-visual-studio/toolbar.png)

4. <span data-ttu-id="a49a1-137">Hello **Hive feladat összegzése** és hello feladat futtatásával kapcsolatos információkat jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="a49a1-137">hello **Hive Job Summary** appears and displays information about hello running job.</span></span> <span data-ttu-id="a49a1-138">Használjon hello **frissítése** toorefresh hello feladatinformációkat, amíg hello hivatkozás **feladat állapota** túl változik**befejezve**.</span><span class="sxs-lookup"><span data-stu-id="a49a1-138">Use hello **Refresh** link toorefresh hello job information, until hello **Job Status** changes too**Completed**.</span></span>

   ![a befejezett feladatok megjelenítése a feladat összegzése](./media/hdinsight-hadoop-use-hive-visual-studio/jobsummary.png)

5. <span data-ttu-id="a49a1-140">Használjon hello **Job Output** tooview hello kimenetet a feldolgozás hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="a49a1-140">Use hello **Job Output** link tooview hello output of this job.</span></span> <span data-ttu-id="a49a1-141">Megjeleníti `[ERROR] 3`, ez a lekérdezés által visszaadott hello értéket.</span><span class="sxs-lookup"><span data-stu-id="a49a1-141">It displays `[ERROR] 3`, which is hello value returned by this query.</span></span>

6. <span data-ttu-id="a49a1-142">Hive-lekérdezéseket a projekt létrehozása nélkül is futtathatja.</span><span class="sxs-lookup"><span data-stu-id="a49a1-142">You can also run Hive queries without creating a project.</span></span> <span data-ttu-id="a49a1-143">Használatával **Server Explorer**, bontsa ki a **Azure** > **HDInsight**, kattintson a jobb gombbal a HDInsight-kiszolgáló, és válassza **Hive lekérdezés írása**.</span><span class="sxs-lookup"><span data-stu-id="a49a1-143">Using **Server Explorer**, expand **Azure** > **HDInsight**, right-click your HDInsight server, and then select **Write a Hive Query**.</span></span>

7. <span data-ttu-id="a49a1-144">A hello **temp.hql** dokumentumot, amely akkor jelenik meg, vegye fel a következő HiveQL utasítások hello:</span><span class="sxs-lookup"><span data-stu-id="a49a1-144">In hello **temp.hql** document that appears, add hello following HiveQL statements:</span></span>

   ```hiveql
   set hive.execution.engine=tez;
   CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
   INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';
   ```

    <span data-ttu-id="a49a1-145">Ezekre az utasításokra hajtsa végre a következő műveletek hello:</span><span class="sxs-lookup"><span data-stu-id="a49a1-145">These statements perform hello following actions:</span></span>

   * <span data-ttu-id="a49a1-146">`CREATE TABLE IF NOT EXISTS`: Létrehoz egy táblát, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="a49a1-146">`CREATE TABLE IF NOT EXISTS`: Creates a table if it does not already exist.</span></span> <span data-ttu-id="a49a1-147">Mivel hello `EXTERNAL` kulcsszó nem használható, a jelen nyilatkozat egy belső táblát hoz létre.</span><span class="sxs-lookup"><span data-stu-id="a49a1-147">Because hello `EXTERNAL` keyword is not used, this statement creates an internal table.</span></span> <span data-ttu-id="a49a1-148">Belső tábla hello Hive adatraktár tárolódnak, és Hive kezeli.</span><span class="sxs-lookup"><span data-stu-id="a49a1-148">Internal tables are stored in hello Hive data warehouse and are managed by Hive.</span></span>

     > [!NOTE]
     > <span data-ttu-id="a49a1-149">Eltérően `EXTERNAL` táblák, egy belső tábla is eldobása hello alapjául szolgáló adatok törlése.</span><span class="sxs-lookup"><span data-stu-id="a49a1-149">Unlike `EXTERNAL` tables, dropping an internal table also deletes hello underlying data.</span></span>

   * <span data-ttu-id="a49a1-150">`STORED AS ORC`: A tárolók hello adatok optimalizált sor oszlopos (ORC) formátumban.</span><span class="sxs-lookup"><span data-stu-id="a49a1-150">`STORED AS ORC`: Stores hello data in optimized row columnar (ORC) format.</span></span> <span data-ttu-id="a49a1-151">ORC formátuma egy magas optimalizált és hatékony Hive adatainak tárolásához.</span><span class="sxs-lookup"><span data-stu-id="a49a1-151">ORC is a highly optimized and efficient format for storing Hive data.</span></span>

   * <span data-ttu-id="a49a1-152">`INSERT OVERWRITE ... SELECT`: Sorok kiválaszt hello `log4jLogs` tartalmazó `[ERROR]`, majd beszúrása adatok hello be hello `errorLogs` tábla.</span><span class="sxs-lookup"><span data-stu-id="a49a1-152">`INSERT OVERWRITE ... SELECT`: Selects rows from hello `log4jLogs` table that contain `[ERROR]`, then inserts hello data into hello `errorLogs` table.</span></span>

8. <span data-ttu-id="a49a1-153">Hello eszköztárán válassza **Submit** toorun hello feladat.</span><span class="sxs-lookup"><span data-stu-id="a49a1-153">From hello toolbar, select **Submit** toorun hello job.</span></span> <span data-ttu-id="a49a1-154">Használjon hello **feladatállapot** toodetermine hello feladat sikeresen befejeződött.</span><span class="sxs-lookup"><span data-stu-id="a49a1-154">Use hello **Job Status** toodetermine that hello job has completed successfully.</span></span>

9. <span data-ttu-id="a49a1-155">amely a feladat létrehozva hello tábla, használjon hello tooverify **Server Explorer** csomópontot **Azure** > **HDInsight** > a HDInsight-fürt > **Hive adatbázisokat** > **alapértelmezett**.</span><span class="sxs-lookup"><span data-stu-id="a49a1-155">tooverify that hello job created hello table, use **Server Explorer** and expand **Azure** > **HDInsight** > your HDInsight cluster > **Hive Databases** > **default**.</span></span> <span data-ttu-id="a49a1-156">Hello **errorLogs** tábla és hello **log4jLogs** táblázatban vannak felsorolva.</span><span class="sxs-lookup"><span data-stu-id="a49a1-156">hello **errorLogs** table and hello **log4jLogs** table are listed.</span></span>

## <span data-ttu-id="a49a1-157"><a id="nextsteps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a49a1-157"><a id="nextsteps"></a>Next steps</span></span>

<span data-ttu-id="a49a1-158">Ahogy látja, hello a HDInsight tools for Visual Studio egy egyszerűen toowork a Hive-lekérdezéseket a HDInsight adja meg.</span><span class="sxs-lookup"><span data-stu-id="a49a1-158">As you can see, hello HDInsight tools for Visual Studio provide an easy way toowork with Hive queries on HDInsight.</span></span>

<span data-ttu-id="a49a1-159">Általános információk a hdinsight Hive:</span><span class="sxs-lookup"><span data-stu-id="a49a1-159">For general information about Hive in HDInsight:</span></span>

* [<span data-ttu-id="a49a1-160">A Hive használata a hdinsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="a49a1-160">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)

<span data-ttu-id="a49a1-161">Más módszerekkel kapcsolatos információk a HDInsight Hadoop dolgozhat:</span><span class="sxs-lookup"><span data-stu-id="a49a1-161">For information about other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="a49a1-162">A Pig használata a HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="a49a1-162">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)

* [<span data-ttu-id="a49a1-163">A HDInsight Hadoop MapReduce használata</span><span class="sxs-lookup"><span data-stu-id="a49a1-163">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)

<span data-ttu-id="a49a1-164">Hello kapcsolatos további információk a HDInsight tools for Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="a49a1-164">For more information about hello HDInsight tools for Visual Studio:</span></span>

* [<span data-ttu-id="a49a1-165">Kezdeti lépések a HDInsight tools for Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a49a1-165">Getting started with HDInsight tools for Visual Studio</span></span>](hdinsight-hadoop-visual-studio-tools-get-started.md)

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md



[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx

[image-hdi-hive-powershell]: ./media/hdinsight-use-hive/HDI.HIVE.PowerShell.png
[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png
[image-hdi-hive-architecture]: ./media/hdinsight-use-hive/HDI.Hive.Architecture.png
