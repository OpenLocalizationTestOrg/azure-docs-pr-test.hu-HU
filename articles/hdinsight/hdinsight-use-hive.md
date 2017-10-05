---
title: "Mi az Apache Hive és a HiveQL - Azure HDInsight |} Microsoft Docs"
description: "A Hadoop adatraktárrendszer Apache Hive. A Hive használata a HiveQL, adataihoz lekérheti amely Transact-SQL hasonló. Ebből a dokumentumból megtudhatja, hogyan Azure HDInsight Hive és a HiveQL használandó."
keywords: "hiveql, mi az hive, hadoop hiveql a hive használata kapcsolatos további tudnivalók a hive, mi az hive"
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 2c10f989-7636-41bf-b7f7-c4b67ec0814f
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/03/2017
ms.author: larryfr
ms.openlocfilehash: 6b3ee17141f773bec07cf40e0b6d63363e9b5164
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="what-is-apache-hive-and-hiveql-on-azure-hdinsight"></a><span data-ttu-id="cce9f-106">Mi az Apache Hive és a Azure HDInsight HiveQL?</span><span class="sxs-lookup"><span data-stu-id="cce9f-106">What is Apache Hive and HiveQL on Azure HDInsight?</span></span>

<span data-ttu-id="cce9f-107">[Apache Hive](http://hive.apache.org/) egy adatraktárrendszer van a Hadoop.</span><span class="sxs-lookup"><span data-stu-id="cce9f-107">[Apache Hive](http://hive.apache.org/) is a data warehouse system for Hadoop.</span></span> <span data-ttu-id="cce9f-108">Hive lehetővé teszi, hogy adatainak összefoglalója lekérdezése és az adatok elemzését.</span><span class="sxs-lookup"><span data-stu-id="cce9f-108">Hive enables data summarization, querying, and analysis of data.</span></span> <span data-ttu-id="cce9f-109">Hive-lekérdezések HiveQL, amely hasonló SQL lekérdezésnyelvet nyelven íródtak.</span><span class="sxs-lookup"><span data-stu-id="cce9f-109">Hive queries are written in HiveQL, which is a query language similar to SQL.</span></span>

<span data-ttu-id="cce9f-110">Hive lehetővé teszi a nagy mértékben strukturálatlan adatok szerkezetének.</span><span class="sxs-lookup"><span data-stu-id="cce9f-110">Hive allows you to project structure on largely unstructured data.</span></span> <span data-ttu-id="cce9f-111">A struktúra meghatározása után HiveQL használatával Java vagy MapReduce ismerete nélkül a lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="cce9f-111">After you define the structure, you can use HiveQL to query the data without knowledge of Java or MapReduce.</span></span>

<span data-ttu-id="cce9f-112">A HDInsight fürt számos különböző, amelyek adott munkaterhelés konkrét hangolt biztosít.</span><span class="sxs-lookup"><span data-stu-id="cce9f-112">HDInsight provides several cluster types, which are tuned for specific workloads.</span></span> <span data-ttu-id="cce9f-113">A következő fürttípusok leggyakrabban használt a Hive-lekérdezéseket:</span><span class="sxs-lookup"><span data-stu-id="cce9f-113">The following cluster types are most often used for Hive queries:</span></span>

* <span data-ttu-id="cce9f-114">__Interaktív Hive__: A Hadoop-fürt biztosító [alacsony késleltetésű analitikus feldolgozási (LLAP)](https://cwiki.apache.org/confluence/display/Hive/LLAP) funkciót a javíthatja interaktív lekérdezések válaszidejét.</span><span class="sxs-lookup"><span data-stu-id="cce9f-114">__Interactive Hive__: A Hadoop cluster that provides [Low Latency Analytical Processing (LLAP)](https://cwiki.apache.org/confluence/display/Hive/LLAP) functionality to improve response times for interactive queries.</span></span> <span data-ttu-id="cce9f-115">További információkért lásd: a [interaktív Hive hdinsight kezdődnie](hdinsight-hadoop-use-interactive-hive.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="cce9f-115">For more information, see the [Start with Interactive Hive in HDInsight](hdinsight-hadoop-use-interactive-hive.md) document.</span></span>

* <span data-ttu-id="cce9f-116">__Hadoop__: A Hadoop-fürt, amely a kötegelt feldolgozáson munkaterhelések van beállítva.</span><span class="sxs-lookup"><span data-stu-id="cce9f-116">__Hadoop__: A Hadoop cluster that is tuned for batch processing workloads.</span></span> <span data-ttu-id="cce9f-117">További információkért lásd: a [indítsa el a HDInsight Hadoop](hdinsight-hadoop-linux-tutorial-get-started.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="cce9f-117">For more information, see the [Start with Hadoop in HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md) document.</span></span>

* <span data-ttu-id="cce9f-118">__Spark__: Apache Spark rendelkezik beépített funkcióval Hive használata.</span><span class="sxs-lookup"><span data-stu-id="cce9f-118">__Spark__: Apache Spark has built-in functionality for working with Hive.</span></span> <span data-ttu-id="cce9f-119">További információkért lásd: a [indítsa el a Spark on HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="cce9f-119">For more information, see the [Start with Spark on HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md) document.</span></span>

* <span data-ttu-id="cce9f-120">__A HBase__: HiveQL a HBase lekérdezés adataihoz is használható.</span><span class="sxs-lookup"><span data-stu-id="cce9f-120">__HBase__: HiveQL can be used to query data stored in HBase.</span></span> <span data-ttu-id="cce9f-121">További információkért lásd: a [indítsa el a HDInsight HBase](hdinsight-hbase-tutorial-get-started-linux.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="cce9f-121">For more information, see the [Start with HBase on HDInsight](hdinsight-hbase-tutorial-get-started-linux.md) document.</span></span>

## <a name="how-to-use-hive"></a><span data-ttu-id="cce9f-122">Hive használata</span><span class="sxs-lookup"><span data-stu-id="cce9f-122">How to use Hive</span></span>

<span data-ttu-id="cce9f-123">A következő táblázat segítségével miképpen Hive használata a Hdinsightban:</span><span class="sxs-lookup"><span data-stu-id="cce9f-123">Use the following table to discover how to use Hive with HDInsight:</span></span>

| <span data-ttu-id="cce9f-124">**Ezzel a módszerrel** Ha azt szeretné...</span><span class="sxs-lookup"><span data-stu-id="cce9f-124">**Use this method** if you want...</span></span> | <span data-ttu-id="cce9f-125">.. .an **interaktív** rendszerhéj</span><span class="sxs-lookup"><span data-stu-id="cce9f-125">...an **interactive** shell</span></span> | <span data-ttu-id="cce9f-126">... **kötegelt** feldolgozása</span><span class="sxs-lookup"><span data-stu-id="cce9f-126">...**batch** processing</span></span> | <span data-ttu-id="cce9f-127">és mivel ez **fürt operációs rendszer**</span><span class="sxs-lookup"><span data-stu-id="cce9f-127">...with this **cluster operating system**</span></span> | <span data-ttu-id="cce9f-128">.. .from ez **ügyfél operációs rendszer**</span><span class="sxs-lookup"><span data-stu-id="cce9f-128">...from this **client operating system**</span></span> |
|:--- |:---:|:---:|:--- |:--- |
| [<span data-ttu-id="cce9f-129">Hive nézete</span><span class="sxs-lookup"><span data-stu-id="cce9f-129">Hive View</span></span>](hdinsight-hadoop-use-hive-ambari-view.md) |<span data-ttu-id="cce9f-130">✔</span><span class="sxs-lookup"><span data-stu-id="cce9f-130">✔</span></span> |<span data-ttu-id="cce9f-131">✔</span><span class="sxs-lookup"><span data-stu-id="cce9f-131">✔</span></span> |<span data-ttu-id="cce9f-132">Linux</span><span class="sxs-lookup"><span data-stu-id="cce9f-132">Linux</span></span> |<span data-ttu-id="cce9f-133">Bármely (böngészőalapú)</span><span class="sxs-lookup"><span data-stu-id="cce9f-133">Any (browser based)</span></span> |
| [<span data-ttu-id="cce9f-134">Beeline ügyfél</span><span class="sxs-lookup"><span data-stu-id="cce9f-134">Beeline client</span></span>](hdinsight-hadoop-use-hive-beeline.md) |<span data-ttu-id="cce9f-135">✔</span><span class="sxs-lookup"><span data-stu-id="cce9f-135">✔</span></span> |<span data-ttu-id="cce9f-136">✔</span><span class="sxs-lookup"><span data-stu-id="cce9f-136">✔</span></span> |<span data-ttu-id="cce9f-137">Linux</span><span class="sxs-lookup"><span data-stu-id="cce9f-137">Linux</span></span> |<span data-ttu-id="cce9f-138">Linux, Unix, Mac OS X vagy Windows</span><span class="sxs-lookup"><span data-stu-id="cce9f-138">Linux, Unix, Mac OS X, or Windows</span></span> |
| [<span data-ttu-id="cce9f-139">REST API</span><span class="sxs-lookup"><span data-stu-id="cce9f-139">REST API</span></span>](hdinsight-hadoop-use-hive-curl.md) |&nbsp; |<span data-ttu-id="cce9f-140">✔</span><span class="sxs-lookup"><span data-stu-id="cce9f-140">✔</span></span> |<span data-ttu-id="cce9f-141">Linux vagy a Windows</span><span class="sxs-lookup"><span data-stu-id="cce9f-141">Linux or Windows*</span></span> |<span data-ttu-id="cce9f-142">Linux, Unix, Mac OS X vagy Windows</span><span class="sxs-lookup"><span data-stu-id="cce9f-142">Linux, Unix, Mac OS X, or Windows</span></span> |
| [<span data-ttu-id="cce9f-143">A HDInsight tools for Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cce9f-143">HDInsight tools for Visual Studio</span></span>](hdinsight-hadoop-use-hive-visual-studio.md) |&nbsp; |<span data-ttu-id="cce9f-144">✔</span><span class="sxs-lookup"><span data-stu-id="cce9f-144">✔</span></span> |<span data-ttu-id="cce9f-145">Linux vagy a Windows</span><span class="sxs-lookup"><span data-stu-id="cce9f-145">Linux or Windows*</span></span> |<span data-ttu-id="cce9f-146">Windows</span><span class="sxs-lookup"><span data-stu-id="cce9f-146">Windows</span></span> |
| [<span data-ttu-id="cce9f-147">A Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="cce9f-147">Windows PowerShell</span></span>](hdinsight-hadoop-use-hive-powershell.md) |&nbsp; |<span data-ttu-id="cce9f-148">✔</span><span class="sxs-lookup"><span data-stu-id="cce9f-148">✔</span></span> |<span data-ttu-id="cce9f-149">Linux vagy a Windows</span><span class="sxs-lookup"><span data-stu-id="cce9f-149">Linux or Windows*</span></span> |<span data-ttu-id="cce9f-150">Windows</span><span class="sxs-lookup"><span data-stu-id="cce9f-150">Windows</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="cce9f-151">\*Linux az egyetlen operációs rendszer használt a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="cce9f-151">\* Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="cce9f-152">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="cce9f-152">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>
>
> <span data-ttu-id="cce9f-153">Ha egy Windows-alapú HDInsight-fürtöt használ, akkor használhatja a [lekérdezés konzol](hdinsight-hadoop-use-hive-query-console.md) a böngészőből vagy [távoli asztal](hdinsight-hadoop-use-hive-remote-desktop.md) Hive-lekérdezések futtatásához.</span><span class="sxs-lookup"><span data-stu-id="cce9f-153">If you are using a Windows-based HDInsight cluster, you can use the [Query console](hdinsight-hadoop-use-hive-query-console.md) from your browser or [Remote Desktop](hdinsight-hadoop-use-hive-remote-desktop.md) to run Hive queries.</span></span>

## <a name="hiveql-language-reference"></a><span data-ttu-id="cce9f-154">HiveQL nyelvi referencia</span><span class="sxs-lookup"><span data-stu-id="cce9f-154">HiveQL language reference</span></span>

<span data-ttu-id="cce9f-155">HiveQL nyelvi dokumentáció áll rendelkezésre a [nyelvi manuális (https://cwiki.apache.org/confluence/display/Hive/LanguageManual)](https://cwiki.apache.org/confluence/display/Hive/LanguageManual).</span><span class="sxs-lookup"><span data-stu-id="cce9f-155">HiveQL language reference is available in the [language manual (https://cwiki.apache.org/confluence/display/Hive/LanguageManual)](https://cwiki.apache.org/confluence/display/Hive/LanguageManual).</span></span>

## <a name="hive-and-data-structure"></a><span data-ttu-id="cce9f-156">Hive és az adatok szerkezete</span><span class="sxs-lookup"><span data-stu-id="cce9f-156">Hive and data structure</span></span>

<span data-ttu-id="cce9f-157">Hive együttműködik a strukturált és félig strukturált adatok használata.</span><span class="sxs-lookup"><span data-stu-id="cce9f-157">Hive understands how to work with structured and semi-structured data.</span></span> <span data-ttu-id="cce9f-158">Például szövegfájlok ahol a mezők határolja különleges karaktereket.</span><span class="sxs-lookup"><span data-stu-id="cce9f-158">For example, text files where the fields are delimited by specific characters.</span></span> <span data-ttu-id="cce9f-159">A következő HiveQL-utasítás táblázatot hoz létre szóközökkel elválasztott kötetnevek adatok:</span><span class="sxs-lookup"><span data-stu-id="cce9f-159">The following HiveQL statement creates a table over space-delimited data:</span></span>

```hiveql
CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
STORED AS TEXTFILE LOCATION '/example/data/';
```

<span data-ttu-id="cce9f-160">Hive is támogatja az egyéni **szerializáló/deserializers (SerDe)** túl összetett vagy szabálytalan strukturált adatok számára.</span><span class="sxs-lookup"><span data-stu-id="cce9f-160">Hive also supports custom **serializer/deserializers (SerDe)** for complex or irregularly structured data.</span></span> <span data-ttu-id="cce9f-161">További információkért lásd: a [egyéni JSON-SerDe használata a HDInsight](http://blogs.msdn.com/b/bigdatasupport/archive/2014/06/18/how-to-use-a-custom-json-serde-with-microsoft-azure-hdinsight.aspx) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="cce9f-161">For more information, see the [How to use a custom JSON SerDe with HDInsight](http://blogs.msdn.com/b/bigdatasupport/archive/2014/06/18/how-to-use-a-custom-json-serde-with-microsoft-azure-hdinsight.aspx) document.</span></span>

<span data-ttu-id="cce9f-162">A Hive támogatott fájlformátumok további információkért lásd: a [nyelvi manuális (https://cwiki.apache.org/confluence/display/Hive/LanguageManual)](https://cwiki.apache.org/confluence/display/Hive/LanguageManual)</span><span class="sxs-lookup"><span data-stu-id="cce9f-162">For more information on file formats supported by Hive, see the [Language manual (https://cwiki.apache.org/confluence/display/Hive/LanguageManual)](https://cwiki.apache.org/confluence/display/Hive/LanguageManual)</span></span>

## <a name="hive-internal-tables-vs-external-tables"></a><span data-ttu-id="cce9f-163">Belső tábla és a külső táblákra struktúra</span><span class="sxs-lookup"><span data-stu-id="cce9f-163">Hive internal tables vs external tables</span></span>

<span data-ttu-id="cce9f-164">Olyan táblázatot, amely a Hive hozhat létre két típusa van:</span><span class="sxs-lookup"><span data-stu-id="cce9f-164">There are two types of tables that you can create with Hive:</span></span>

* <span data-ttu-id="cce9f-165">__Belső__: adatokat a Hive-adatraktárban tárolja.</span><span class="sxs-lookup"><span data-stu-id="cce9f-165">__Internal__: Data is stored in the Hive data warehouse.</span></span> <span data-ttu-id="cce9f-166">Az adatraktár itt található: `/hive/warehouse/` az alapértelmezett tároló, a fürt számára.</span><span class="sxs-lookup"><span data-stu-id="cce9f-166">The data warehouse is located at `/hive/warehouse/` on the default storage for the cluster.</span></span>

    <span data-ttu-id="cce9f-167">Belső használja táblák esetén:</span><span class="sxs-lookup"><span data-stu-id="cce9f-167">Use internal tables when:</span></span>

    * <span data-ttu-id="cce9f-168">Adatok csak átmenetileg létezik.</span><span class="sxs-lookup"><span data-stu-id="cce9f-168">Data is temporary.</span></span>
    * <span data-ttu-id="cce9f-169">Azt szeretné, hogy a struktúra a táblázat és az adatok az életciklus kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="cce9f-169">You want Hive to manage the lifecycle of the table and data.</span></span>

* <span data-ttu-id="cce9f-170">__Külső__: kívül az adatraktár tárolja.</span><span class="sxs-lookup"><span data-stu-id="cce9f-170">__External__: Data is stored outside the data warehouse.</span></span> <span data-ttu-id="cce9f-171">Az adatok a fürt által elérhető minden tárterület tárolható.</span><span class="sxs-lookup"><span data-stu-id="cce9f-171">The data can be stored on any storage accessible by the cluster.</span></span>

    <span data-ttu-id="cce9f-172">Használjon külső táblák esetén:</span><span class="sxs-lookup"><span data-stu-id="cce9f-172">Use external tables when:</span></span>

    * <span data-ttu-id="cce9f-173">Az adatok Hive kívül is használható.</span><span class="sxs-lookup"><span data-stu-id="cce9f-173">The data is also used outside of Hive.</span></span> <span data-ttu-id="cce9f-174">Például az adatfájlok frissítése egy másik folyamat (vagyis nem zárolja a fájlokat.)</span><span class="sxs-lookup"><span data-stu-id="cce9f-174">For example, the data files are updated by another process (that does not lock the files.)</span></span>
    * <span data-ttu-id="cce9f-175">Adatoknak kell alapul szolgáló helyét, a tábla eldobása után is megmaradnak.</span><span class="sxs-lookup"><span data-stu-id="cce9f-175">Data needs to remain in the underlying location, even after dropping the table.</span></span>
    * <span data-ttu-id="cce9f-176">Egyéni helyen, például egy nem alapértelmezett tárfiók van szüksége.</span><span class="sxs-lookup"><span data-stu-id="cce9f-176">You need a custom location, such as a non-default storage account.</span></span>
    * <span data-ttu-id="cce9f-177">Nem a hive kezeli az adatformátum, hely, stb.</span><span class="sxs-lookup"><span data-stu-id="cce9f-177">A program other than hive manages the data format, location, etc.</span></span>

<span data-ttu-id="cce9f-178">További információkért lásd: a [Hive belső és külső táblák bevezetés] [ cindygross-hive-tables] blogbejegyzést.</span><span class="sxs-lookup"><span data-stu-id="cce9f-178">For more information, see the [Hive Internal and External Tables Intro][cindygross-hive-tables] blog post.</span></span>

## <a name="user-defined-functions-udf"></a><span data-ttu-id="cce9f-179">Felhasználói függvény (UDF)</span><span class="sxs-lookup"><span data-stu-id="cce9f-179">User-defined functions (UDF)</span></span>

<span data-ttu-id="cce9f-180">Hive is kiterjeszthető keresztül **felhasználói függvény (UDF)**.</span><span class="sxs-lookup"><span data-stu-id="cce9f-180">Hive can also be extended through **user-defined functions (UDF)**.</span></span> <span data-ttu-id="cce9f-181">Egy UDF funkció vagy logika, amely nem egyszerű modellezve megvalósítását a HiveQL teszi lehetővé.</span><span class="sxs-lookup"><span data-stu-id="cce9f-181">A UDF allows you to implement functionality or logic that isn't easily modeled in HiveQL.</span></span> <span data-ttu-id="cce9f-182">Például egy felhasználó által megadott függvények használata a Hive lásd a következő dokumentumokat:</span><span class="sxs-lookup"><span data-stu-id="cce9f-182">For an example of using UDFs with Hive, see the following documents:</span></span>

* [<span data-ttu-id="cce9f-183">A Java-felhasználó által definiált függvény használata struktúra</span><span class="sxs-lookup"><span data-stu-id="cce9f-183">Use a Java user-defined function with Hive</span></span>](hdinsight-hadoop-hive-java-udf.md)

* [<span data-ttu-id="cce9f-184">A Python felhasználói függvény használata a Hive és a Pig használatával</span><span class="sxs-lookup"><span data-stu-id="cce9f-184">Use a Python user-defined function with Hive and Pig</span></span>](hdinsight-python.md)

* [<span data-ttu-id="cce9f-185">A C# felhasználó által definiált függvény használata Hive és a Pig használatával</span><span class="sxs-lookup"><span data-stu-id="cce9f-185">Use a C# user-defined function with Hive and Pig</span></span>](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)

* [<span data-ttu-id="cce9f-186">HDInsight egyéni Hive felhasználó által definiált függvény hozzáadása</span><span class="sxs-lookup"><span data-stu-id="cce9f-186">How to add a custom Hive user-defined function to HDInsight</span></span>](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/14/how-to-add-custom-hive-udfs-to-hdinsight.aspx)

* [<span data-ttu-id="cce9f-187">Példa Hive felhasználó által definiált függvény Dátum-/ időformátumok átalakítása Hive időbélyeg</span><span class="sxs-lookup"><span data-stu-id="cce9f-187">An example Hive user-defined function to convert date/time formats to Hive timestamp</span></span>](https://github.com/Azure-Samples/hdinsight-java-hive-udf)

## <span data-ttu-id="cce9f-188"><a id="data"></a>Példa adatok</span><span class="sxs-lookup"><span data-stu-id="cce9f-188"><a id="data"></a>Example data</span></span>

<span data-ttu-id="cce9f-189">A HDInsight Hive előre betöltött tartalmaz egy belső tábla nevű `hivesampletable`.</span><span class="sxs-lookup"><span data-stu-id="cce9f-189">Hive on HDInsight comes pre-loaded with an internal table named `hivesampletable`.</span></span> <span data-ttu-id="cce9f-190">HDInsight Hive használható például adatkészleteket is biztosít.</span><span class="sxs-lookup"><span data-stu-id="cce9f-190">HDInsight also provides example data sets that can be used with Hive.</span></span> <span data-ttu-id="cce9f-191">Ezek az adathalmazok tárolódnak a `/example/data` és `/HdiSamples` könyvtárak.</span><span class="sxs-lookup"><span data-stu-id="cce9f-191">These data sets are stored in the `/example/data` and `/HdiSamples` directories.</span></span> <span data-ttu-id="cce9f-192">Ezeket a könyvtárakat az alapértelmezett tároló, a fürt szerepel.</span><span class="sxs-lookup"><span data-stu-id="cce9f-192">These directories exist in the default storage for your cluster.</span></span>

## <span data-ttu-id="cce9f-193"><a id="job"></a>Példa Hive-lekérdezések</span><span class="sxs-lookup"><span data-stu-id="cce9f-193"><a id="job"></a>Example Hive query</span></span>

<span data-ttu-id="cce9f-194">A következő hiveql-projekt oszlopok alakzatot a `/example/data/sample.log` fájlt:</span><span class="sxs-lookup"><span data-stu-id="cce9f-194">The following HiveQL statements project columns onto the `/example/data/sample.log` file:</span></span>

    set hive.execution.engine=tez;
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION '/example/data/';
    SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

<span data-ttu-id="cce9f-195">Az előző példában a HiveQL utasításokat a következő műveleteket:</span><span class="sxs-lookup"><span data-stu-id="cce9f-195">In the previous example, the HiveQL statements perform the following actions:</span></span>

* <span data-ttu-id="cce9f-196">`set hive.execution.engine=tez;`: A-végrehajtó motor Tez használatára állítja be.</span><span class="sxs-lookup"><span data-stu-id="cce9f-196">`set hive.execution.engine=tez;`: Sets the execution engine to use Tez.</span></span> <span data-ttu-id="cce9f-197">Tez helyett MapReduce biztosíthat a lekérdezési teljesítmény növelését.</span><span class="sxs-lookup"><span data-stu-id="cce9f-197">Using Tez instead of MapReduce can provide an increase in query performance.</span></span> <span data-ttu-id="cce9f-198">Tez további információkért tekintse meg a [Apache Tez használja a jobb teljesítmény](#usetez) szakasz.</span><span class="sxs-lookup"><span data-stu-id="cce9f-198">For more information on Tez, see the [Use Apache Tez for improved performance](#usetez) section.</span></span>

    > [!NOTE]
    > <span data-ttu-id="cce9f-199">Az utasítás csak egy Windows-alapú HDInsight-fürt használata esetén szükséges.</span><span class="sxs-lookup"><span data-stu-id="cce9f-199">This statement is only required when using a Windows-based HDInsight cluster.</span></span> <span data-ttu-id="cce9f-200">Tez a Linux-alapú hdinsight alapértelmezett-végrehajtó motor.</span><span class="sxs-lookup"><span data-stu-id="cce9f-200">Tez is the default execution engine for Linux-based HDInsight.</span></span>

* <span data-ttu-id="cce9f-201">`DROP TABLE`: Ha a tábla már létezik, törölje azt.</span><span class="sxs-lookup"><span data-stu-id="cce9f-201">`DROP TABLE`: If the table already exists, delete it.</span></span>

* <span data-ttu-id="cce9f-202">`CREATE EXTERNAL TABLE`: Létrehoz egy új **külső** Hive táblát.</span><span class="sxs-lookup"><span data-stu-id="cce9f-202">`CREATE EXTERNAL TABLE`: Creates a new **external** table in Hive.</span></span> <span data-ttu-id="cce9f-203">Külső táblák csak tárolja a tábladefiníció struktúra.</span><span class="sxs-lookup"><span data-stu-id="cce9f-203">External tables only store the table definition in Hive.</span></span> <span data-ttu-id="cce9f-204">Az adatok marad az eredeti helyen és az eredeti formátumban.</span><span class="sxs-lookup"><span data-stu-id="cce9f-204">The data is left in the original location and in the original format.</span></span>

* <span data-ttu-id="cce9f-205">`ROW FORMAT`: Az adatok formázását Hive jelzi.</span><span class="sxs-lookup"><span data-stu-id="cce9f-205">`ROW FORMAT`: Tells Hive how the data is formatted.</span></span> <span data-ttu-id="cce9f-206">Ebben az esetben a mezőket az egyes naplókon szóközzel elválasztva.</span><span class="sxs-lookup"><span data-stu-id="cce9f-206">In this case, the fields in each log are separated by a space.</span></span>

* <span data-ttu-id="cce9f-207">`STORED AS TEXTFILE LOCATION`: Az adatok tárolására Hive jelzi (a `example/data` könyvtár) és szövegként tárolt.</span><span class="sxs-lookup"><span data-stu-id="cce9f-207">`STORED AS TEXTFILE LOCATION`: Tells Hive where the data is stored (the `example/data` directory) and that it is stored as text.</span></span> <span data-ttu-id="cce9f-208">Az adatok egyetlen fájlban vagy több fájl a könyvtárban lévő elosztva.</span><span class="sxs-lookup"><span data-stu-id="cce9f-208">The data can be in one file or spread across multiple files within the directory.</span></span>

* <span data-ttu-id="cce9f-209">`SELECT`: Választja ki az összes sor száma ha az oszlop **t4** értéke **[hiba]**.</span><span class="sxs-lookup"><span data-stu-id="cce9f-209">`SELECT`: Selects a count of all rows where the column **t4** contains the value **[ERROR]**.</span></span> <span data-ttu-id="cce9f-210">A jelen nyilatkozat értéket ad vissza, **3** mert három ezt az értéket tartalmazó sorok.</span><span class="sxs-lookup"><span data-stu-id="cce9f-210">This statement returns a value of **3** because there are three rows that contain this value.</span></span>

* <span data-ttu-id="cce9f-211">`INPUT__FILE__NAME LIKE '%.log'`-Hive megkísérli a séma alkalmazása a könyvtárban található összes fájl.</span><span class="sxs-lookup"><span data-stu-id="cce9f-211">`INPUT__FILE__NAME LIKE '%.log'` - Hive attempts to apply the schema to all files in the directory.</span></span> <span data-ttu-id="cce9f-212">Ebben az esetben a directory nem egyeznek meg a séma fájlokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="cce9f-212">In this case, the directory contains files that do not match the schema.</span></span> <span data-ttu-id="cce9f-213">Szemétgyűjtési adatok a eredmények elkerülése érdekében a jelen nyilatkozat közli struktúra, hogy azt kell csak vissza adatokat fájlok végződése. napló.</span><span class="sxs-lookup"><span data-stu-id="cce9f-213">To prevent garbage data in the results, this statement tells Hive that we should only return data from files ending in .log.</span></span>

> [!NOTE]
> <span data-ttu-id="cce9f-214">Külső táblák kell használni, amikor külső forrásból frissítenie kell az alapul szolgáló adatokat várt.</span><span class="sxs-lookup"><span data-stu-id="cce9f-214">External tables should be used when you expect the underlying data to be updated by an external source.</span></span> <span data-ttu-id="cce9f-215">Például egy automatizált adatok feltöltési folyamat vagy MapReduce művelethez.</span><span class="sxs-lookup"><span data-stu-id="cce9f-215">For example, an automated data upload process, or MapReduce operation.</span></span>
>
> <span data-ttu-id="cce9f-216">A külső tábla eldobása does **nem** törli az adatokat, csak a tábla definíciójában törli.</span><span class="sxs-lookup"><span data-stu-id="cce9f-216">Dropping an external table does **not** delete the data, it only deletes the table definition.</span></span>

<span data-ttu-id="cce9f-217">Létrehozásához egy **belső** helyett külső tábla, használja a következő HiveQL:</span><span class="sxs-lookup"><span data-stu-id="cce9f-217">To create an **internal** table instead of external, use the following HiveQL:</span></span>

    set hive.execution.engine=tez;
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs
    SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]';

<span data-ttu-id="cce9f-218">Ezekre az utasításokra hajtsa végre a következő műveleteket:</span><span class="sxs-lookup"><span data-stu-id="cce9f-218">These statements perform the following actions:</span></span>

* <span data-ttu-id="cce9f-219">`CREATE TABLE IF NOT EXISTS`: Ha a tábla nem létezik, hozza létre.</span><span class="sxs-lookup"><span data-stu-id="cce9f-219">`CREATE TABLE IF NOT EXISTS`: If the table does not exist, create it.</span></span> <span data-ttu-id="cce9f-220">Mivel a **külső** kulcsszó nem használható, a jelen nyilatkozat egy belső táblát hoz létre.</span><span class="sxs-lookup"><span data-stu-id="cce9f-220">Because the **EXTERNAL** keyword is not used, this statement creates an internal table.</span></span> <span data-ttu-id="cce9f-221">A tábla a Hive-adatraktárban tárolja, és Hive teljesen kezeli.</span><span class="sxs-lookup"><span data-stu-id="cce9f-221">The table is stored in the Hive data warehouse and is managed completely by Hive.</span></span>

* <span data-ttu-id="cce9f-222">`STORED AS ORC`: Tárolja az adatokat optimalizált sor oszlopos (ORC) formátumban.</span><span class="sxs-lookup"><span data-stu-id="cce9f-222">`STORED AS ORC`: Stores the data in Optimized Row Columnar (ORC) format.</span></span> <span data-ttu-id="cce9f-223">ORC formátuma egy magas optimalizált és hatékony Hive adatainak tárolásához.</span><span class="sxs-lookup"><span data-stu-id="cce9f-223">ORC is a highly optimized and efficient format for storing Hive data.</span></span>

* <span data-ttu-id="cce9f-224">`INSERT OVERWRITE ... SELECT`: Azon sorait kiválasztja a **log4jLogs** tartalmazó tábla **[hiba]**, majd beilleszti az adatokat a **errorLogs** tábla.</span><span class="sxs-lookup"><span data-stu-id="cce9f-224">`INSERT OVERWRITE ... SELECT`: Selects rows from the **log4jLogs** table that contains **[ERROR]**, and then inserts the data into the **errorLogs** table.</span></span>

> [!NOTE]
> <span data-ttu-id="cce9f-225">Külső táblák eltérően eldobását egy belső tábla is törli az alapul szolgáló adatokat.</span><span class="sxs-lookup"><span data-stu-id="cce9f-225">Unlike external tables, dropping an internal table also deletes the underlying data.</span></span>

## <a name="improve-hive-query-performance"></a><span data-ttu-id="cce9f-226">Hive-lekérdezések teljesítményének növelése</span><span class="sxs-lookup"><span data-stu-id="cce9f-226">Improve Hive query performance</span></span>

### <span data-ttu-id="cce9f-227"><a id="usetez"></a>Apache Tez</span><span class="sxs-lookup"><span data-stu-id="cce9f-227"><a id="usetez"></a>Apache Tez</span></span>

<span data-ttu-id="cce9f-228">[Apache Tez](http://tez.apache.org) egy keretrendszer, amely lehetővé teszi az adatok alkalmazások, például a Hive, a méretekben sokkal hatékonyabban futtatható.</span><span class="sxs-lookup"><span data-stu-id="cce9f-228">[Apache Tez](http://tez.apache.org) is a framework that allows data intensive applications, such as Hive, to run much more efficiently at scale.</span></span> <span data-ttu-id="cce9f-229">A Linux-alapú HDInsight-fürtökön alapértelmezés szerint engedélyezve van a Tez.</span><span class="sxs-lookup"><span data-stu-id="cce9f-229">Tez is enabled by default for Linux-based HDInsight clusters.</span></span>

> [!NOTE]
> <span data-ttu-id="cce9f-230">Tez jelenleg ki alapértelmezés szerint a Windows-alapú HDInsight-fürtök és engedélyezni kell.</span><span class="sxs-lookup"><span data-stu-id="cce9f-230">Tez is currently off by default for Windows-based HDInsight clusters and it must be enabled.</span></span> <span data-ttu-id="cce9f-231">Tez kihasználását, a következő értéket kell beállítani a Hive-lekérdezések:</span><span class="sxs-lookup"><span data-stu-id="cce9f-231">To take advantage of Tez, the following value must be set for a Hive query:</span></span>
>
> `set hive.execution.engine=tez;`
>
> <span data-ttu-id="cce9f-232">Tez a Linux-alapú HDInsight-fürtök alapértelmezett motor.</span><span class="sxs-lookup"><span data-stu-id="cce9f-232">Tez is the default engine for Linux-based HDInsight clusters.</span></span>

<span data-ttu-id="cce9f-233">A [Hive Tez tervezési dokumentumok](https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez) megvalósítási és hangolási konfigurációkkal kapcsolatos részleteket tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="cce9f-233">The [Hive on Tez design documents](https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez) contains details about the implementation choices and tuning configurations.</span></span>

<span data-ttu-id="cce9f-234">A feladatok hibakeresés futtatta a Tez használatával, a következő web UI, amelyek lehetővé teszik a Tez feladatok részletes adatainak megtekintéséhez a HDInsight lehetővé:</span><span class="sxs-lookup"><span data-stu-id="cce9f-234">To aid in debugging jobs ran using Tez, HDInsight provides the following web UIs that allow you to view details of Tez jobs:</span></span>

* [<span data-ttu-id="cce9f-235">Az Ambari Tez nézetben a Linux-alapú HDInsight-on</span><span class="sxs-lookup"><span data-stu-id="cce9f-235">Use the Ambari Tez view on Linux-based HDInsight</span></span>](hdinsight-debug-ambari-tez-view.md)

* [<span data-ttu-id="cce9f-236">A Tez felhasználói felület használata a Windows-alapú HDInsight-on</span><span class="sxs-lookup"><span data-stu-id="cce9f-236">Use the Tez UI on Windows-based HDInsight</span></span>](hdinsight-debug-tez-ui.md)

### <a name="low-latency-analytical-processing-llap"></a><span data-ttu-id="cce9f-237">Kis késleltetésű analitikus feldolgozási (LLAP)</span><span class="sxs-lookup"><span data-stu-id="cce9f-237">Low Latency Analytical Processing (LLAP)</span></span>

<span data-ttu-id="cce9f-238">[LLAP](https://cwiki.apache.org/confluence/display/Hive/LLAP) (más néven hosszú Live és a folyamat), amely lehetővé teszi, hogy a memóriában történő gyorsítótárazás lekérdezések Hive 2.0 új szolgáltatása.</span><span class="sxs-lookup"><span data-stu-id="cce9f-238">[LLAP](https://cwiki.apache.org/confluence/display/Hive/LLAP) (sometimes known as Live Long and Process) is a new feature in Hive 2.0 that allows in-memory caching of queries.</span></span> <span data-ttu-id="cce9f-239">LLAP lehetővé teszi a Hive-lekérdezések sokkal gyorsabb, legfeljebb [26 x gyorsabb, mint a Hive 1.x bizonyos esetekben](https://hortonworks.com/blog/announcing-apache-hive-2-1-25x-faster-queries-much/).</span><span class="sxs-lookup"><span data-stu-id="cce9f-239">LLAP makes Hive queries much faster, up to [26x faster than Hive 1.x in some cases](https://hortonworks.com/blog/announcing-apache-hive-2-1-25x-faster-queries-much/).</span></span>

<span data-ttu-id="cce9f-240">HDInsight fürt interaktív Hive típusban LLAP biztosít.</span><span class="sxs-lookup"><span data-stu-id="cce9f-240">HDInsight provides LLAP in the Interactive Hive cluster type.</span></span> <span data-ttu-id="cce9f-241">További információkért lásd: a [interaktív Hive kezdődnie](hdinsight-hadoop-use-interactive-hive.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="cce9f-241">For more information, see the [Start with Interactive Hive](hdinsight-hadoop-use-interactive-hive.md) document.</span></span>

## <a name="hive-jobs-and-sql-server-integration-services"></a><span data-ttu-id="cce9f-242">Hive-feladatok és az SQL Server Integration Services</span><span class="sxs-lookup"><span data-stu-id="cce9f-242">Hive jobs and SQL Server Integration Services</span></span>

<span data-ttu-id="cce9f-243">SQL Server Integration Services (SSIS) segítségével egy Hive-feladat futtatása.</span><span class="sxs-lookup"><span data-stu-id="cce9f-243">You can use SQL Server Integration Services (SSIS) to run a Hive job.</span></span> <span data-ttu-id="cce9f-244">Az Azure funkciócsomag SSIS biztosít a következő összetevők hdinsight Hive-feladatok együtt használható.</span><span class="sxs-lookup"><span data-stu-id="cce9f-244">The Azure Feature Pack for SSIS provides the following components that work with Hive jobs on HDInsight.</span></span>

* <span data-ttu-id="cce9f-245">[Az Azure HDInsight Hive feladat][hivetask]</span><span class="sxs-lookup"><span data-stu-id="cce9f-245">[Azure HDInsight Hive Task][hivetask]</span></span>

* <span data-ttu-id="cce9f-246">[Az Azure előfizetés Csatlakozáskezelő][connectionmanager]</span><span class="sxs-lookup"><span data-stu-id="cce9f-246">[Azure Subscription Connection Manager][connectionmanager]</span></span>

<span data-ttu-id="cce9f-247">További információk az Azure funkciócsomag SSIS [Itt][ssispack].</span><span class="sxs-lookup"><span data-stu-id="cce9f-247">Learn more about the Azure Feature Pack for SSIS [here][ssispack].</span></span>

## <span data-ttu-id="cce9f-248"><a id="nextsteps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cce9f-248"><a id="nextsteps"></a>Next steps</span></span>

<span data-ttu-id="cce9f-249">Most, hogy megismerte az Hive van, és a hadooppal a Hdinsightban használatával, az alábbi hivatkozások segítségével más módjai Azure HDInsight használata.</span><span class="sxs-lookup"><span data-stu-id="cce9f-249">Now that you've learned what Hive is and how to use it with Hadoop in HDInsight, use the following links to explore other ways to work with Azure HDInsight.</span></span>

* <span data-ttu-id="cce9f-250">[Adatok feltöltése a HDInsightba][hdinsight-upload-data]</span><span class="sxs-lookup"><span data-stu-id="cce9f-250">[Upload data to HDInsight][hdinsight-upload-data]</span></span>
* <span data-ttu-id="cce9f-251">[A Pig használata a HDInsightban][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="cce9f-251">[Use Pig with HDInsight][hdinsight-use-pig]</span></span>
* <span data-ttu-id="cce9f-252">[MapReduce-feladatok használata a hdinsight eszközzel][hdinsight-use-mapreduce]</span><span class="sxs-lookup"><span data-stu-id="cce9f-252">[Use MapReduce jobs with HDInsight][hdinsight-use-mapreduce]</span></span>

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/
[hivetask]: http://msdn.microsoft.com/library/mt146771(v=sql.120).aspx
[connectionmanager]: http://msdn.microsoft.com/library/mt146773(v=sql.120).aspx
[ssispack]: http://msdn.microsoft.com/library/mt146770(v=sql.120).aspx

[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md


[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md

[Powershell-install-configure]: /powershell/azureps-cmdlets-docs
[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx


[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx
