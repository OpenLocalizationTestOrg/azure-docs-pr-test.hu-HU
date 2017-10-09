---
title: "aaaWhat az Apache Hive és a HiveQL - Azure HDInsight |} Microsoft Docs"
description: "A Hadoop adatraktárrendszer Apache Hive. Lekérheti a Hive használata a HiveQL, mely hasonló tooTransact SQL adataihoz. Ebből a dokumentumból megtudhatja, hogyan toouse struktúra és az Azure HDInsight HiveQL."
keywords: "hiveql, mi az hive, hadoop hiveql, hogyan toouse struktúra, ismerje meg a hive, mi az hive"
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
ms.openlocfilehash: a2772312263895ff99b499898264c2e6d5e816e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-apache-hive-and-hiveql-on-azure-hdinsight"></a><span data-ttu-id="c6cfa-106">Mi az Apache Hive és a Azure HDInsight HiveQL?</span><span class="sxs-lookup"><span data-stu-id="c6cfa-106">What is Apache Hive and HiveQL on Azure HDInsight?</span></span>

<span data-ttu-id="c6cfa-107">[Apache Hive](http://hive.apache.org/) egy adatraktárrendszer van a Hadoop.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-107">[Apache Hive](http://hive.apache.org/) is a data warehouse system for Hadoop.</span></span> <span data-ttu-id="c6cfa-108">Hive lehetővé teszi, hogy adatainak összefoglalója lekérdezése és az adatok elemzését.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-108">Hive enables data summarization, querying, and analysis of data.</span></span> <span data-ttu-id="c6cfa-109">Hive-lekérdezések HiveQL, amely egy lekérdezési nyelv hasonló tooSQL nyelven íródtak.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-109">Hive queries are written in HiveQL, which is a query language similar tooSQL.</span></span>

<span data-ttu-id="c6cfa-110">Hive lehetővé teszi tooproject struktúra nagymértékben strukturálatlan adatokon.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-110">Hive allows you tooproject structure on largely unstructured data.</span></span> <span data-ttu-id="c6cfa-111">Hello struktúra meghatározása után HiveQL tooquery hello adatok Java vagy MapReduce ismerete nélkül is használhatja.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-111">After you define hello structure, you can use HiveQL tooquery hello data without knowledge of Java or MapReduce.</span></span>

<span data-ttu-id="c6cfa-112">A HDInsight fürt számos különböző, amelyek adott munkaterhelés konkrét hangolt biztosít.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-112">HDInsight provides several cluster types, which are tuned for specific workloads.</span></span> <span data-ttu-id="c6cfa-113">a következő fürttípusok hello leggyakrabban használt a Hive-lekérdezéseket:</span><span class="sxs-lookup"><span data-stu-id="c6cfa-113">hello following cluster types are most often used for Hive queries:</span></span>

* <span data-ttu-id="c6cfa-114">__Interaktív Hive__: A Hadoop-fürt biztosító [alacsony késleltetésű analitikus feldolgozási (LLAP)](https://cwiki.apache.org/confluence/display/Hive/LLAP) interaktív lekérdezések funkció tooimprove válaszidejét.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-114">__Interactive Hive__: A Hadoop cluster that provides [Low Latency Analytical Processing (LLAP)](https://cwiki.apache.org/confluence/display/Hive/LLAP) functionality tooimprove response times for interactive queries.</span></span> <span data-ttu-id="c6cfa-115">További információkért lásd: hello [interaktív Hive hdinsight kezdődnie](hdinsight-hadoop-use-interactive-hive.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-115">For more information, see hello [Start with Interactive Hive in HDInsight](hdinsight-hadoop-use-interactive-hive.md) document.</span></span>

* <span data-ttu-id="c6cfa-116">__Hadoop__: A Hadoop-fürt, amely a kötegelt feldolgozáson munkaterhelések van beállítva.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-116">__Hadoop__: A Hadoop cluster that is tuned for batch processing workloads.</span></span> <span data-ttu-id="c6cfa-117">További információkért lásd: hello [indítsa el a HDInsight Hadoop](hdinsight-hadoop-linux-tutorial-get-started.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-117">For more information, see hello [Start with Hadoop in HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md) document.</span></span>

* <span data-ttu-id="c6cfa-118">__Spark__: Apache Spark rendelkezik beépített funkcióval Hive használata.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-118">__Spark__: Apache Spark has built-in functionality for working with Hive.</span></span> <span data-ttu-id="c6cfa-119">További információkért lásd: hello [indítsa el a Spark on HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-119">For more information, see hello [Start with Spark on HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md) document.</span></span>

* <span data-ttu-id="c6cfa-120">__A HBase__: HiveQL lehet a HBase használt tooquery adataihoz.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-120">__HBase__: HiveQL can be used tooquery data stored in HBase.</span></span> <span data-ttu-id="c6cfa-121">További információkért lásd: hello [indítsa el a HDInsight HBase](hdinsight-hbase-tutorial-get-started-linux.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-121">For more information, see hello [Start with HBase on HDInsight](hdinsight-hbase-tutorial-get-started-linux.md) document.</span></span>

## <a name="how-toouse-hive"></a><span data-ttu-id="c6cfa-122">Hogyan toouse struktúra</span><span class="sxs-lookup"><span data-stu-id="c6cfa-122">How toouse Hive</span></span>

<span data-ttu-id="c6cfa-123">A következő hogyan toouse hdinsight Hive tábla toodiscover hello használata:</span><span class="sxs-lookup"><span data-stu-id="c6cfa-123">Use hello following table toodiscover how toouse Hive with HDInsight:</span></span>

| <span data-ttu-id="c6cfa-124">**Ezzel a módszerrel** Ha azt szeretné...</span><span class="sxs-lookup"><span data-stu-id="c6cfa-124">**Use this method** if you want...</span></span> | <span data-ttu-id="c6cfa-125">.. .an **interaktív** rendszerhéj</span><span class="sxs-lookup"><span data-stu-id="c6cfa-125">...an **interactive** shell</span></span> | <span data-ttu-id="c6cfa-126">... **kötegelt** feldolgozása</span><span class="sxs-lookup"><span data-stu-id="c6cfa-126">...**batch** processing</span></span> | <span data-ttu-id="c6cfa-127">és mivel ez **fürt operációs rendszer**</span><span class="sxs-lookup"><span data-stu-id="c6cfa-127">...with this **cluster operating system**</span></span> | <span data-ttu-id="c6cfa-128">.. .from ez **ügyfél operációs rendszer**</span><span class="sxs-lookup"><span data-stu-id="c6cfa-128">...from this **client operating system**</span></span> |
|:--- |:---:|:---:|:--- |:--- |
| [<span data-ttu-id="c6cfa-129">Hive nézete</span><span class="sxs-lookup"><span data-stu-id="c6cfa-129">Hive View</span></span>](hdinsight-hadoop-use-hive-ambari-view.md) |<span data-ttu-id="c6cfa-130">✔</span><span class="sxs-lookup"><span data-stu-id="c6cfa-130">✔</span></span> |<span data-ttu-id="c6cfa-131">✔</span><span class="sxs-lookup"><span data-stu-id="c6cfa-131">✔</span></span> |<span data-ttu-id="c6cfa-132">Linux</span><span class="sxs-lookup"><span data-stu-id="c6cfa-132">Linux</span></span> |<span data-ttu-id="c6cfa-133">Bármely (böngészőalapú)</span><span class="sxs-lookup"><span data-stu-id="c6cfa-133">Any (browser based)</span></span> |
| [<span data-ttu-id="c6cfa-134">Beeline ügyfél</span><span class="sxs-lookup"><span data-stu-id="c6cfa-134">Beeline client</span></span>](hdinsight-hadoop-use-hive-beeline.md) |<span data-ttu-id="c6cfa-135">✔</span><span class="sxs-lookup"><span data-stu-id="c6cfa-135">✔</span></span> |<span data-ttu-id="c6cfa-136">✔</span><span class="sxs-lookup"><span data-stu-id="c6cfa-136">✔</span></span> |<span data-ttu-id="c6cfa-137">Linux</span><span class="sxs-lookup"><span data-stu-id="c6cfa-137">Linux</span></span> |<span data-ttu-id="c6cfa-138">Linux, Unix, Mac OS X vagy Windows</span><span class="sxs-lookup"><span data-stu-id="c6cfa-138">Linux, Unix, Mac OS X, or Windows</span></span> |
| [<span data-ttu-id="c6cfa-139">REST API</span><span class="sxs-lookup"><span data-stu-id="c6cfa-139">REST API</span></span>](hdinsight-hadoop-use-hive-curl.md) |&nbsp; |<span data-ttu-id="c6cfa-140">✔</span><span class="sxs-lookup"><span data-stu-id="c6cfa-140">✔</span></span> |<span data-ttu-id="c6cfa-141">Linux vagy a Windows</span><span class="sxs-lookup"><span data-stu-id="c6cfa-141">Linux or Windows*</span></span> |<span data-ttu-id="c6cfa-142">Linux, Unix, Mac OS X vagy Windows</span><span class="sxs-lookup"><span data-stu-id="c6cfa-142">Linux, Unix, Mac OS X, or Windows</span></span> |
| [<span data-ttu-id="c6cfa-143">A HDInsight tools for Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c6cfa-143">HDInsight tools for Visual Studio</span></span>](hdinsight-hadoop-use-hive-visual-studio.md) |&nbsp; |<span data-ttu-id="c6cfa-144">✔</span><span class="sxs-lookup"><span data-stu-id="c6cfa-144">✔</span></span> |<span data-ttu-id="c6cfa-145">Linux vagy a Windows</span><span class="sxs-lookup"><span data-stu-id="c6cfa-145">Linux or Windows*</span></span> |<span data-ttu-id="c6cfa-146">Windows</span><span class="sxs-lookup"><span data-stu-id="c6cfa-146">Windows</span></span> |
| [<span data-ttu-id="c6cfa-147">A Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="c6cfa-147">Windows PowerShell</span></span>](hdinsight-hadoop-use-hive-powershell.md) |&nbsp; |<span data-ttu-id="c6cfa-148">✔</span><span class="sxs-lookup"><span data-stu-id="c6cfa-148">✔</span></span> |<span data-ttu-id="c6cfa-149">Linux vagy a Windows</span><span class="sxs-lookup"><span data-stu-id="c6cfa-149">Linux or Windows*</span></span> |<span data-ttu-id="c6cfa-150">Windows</span><span class="sxs-lookup"><span data-stu-id="c6cfa-150">Windows</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="c6cfa-151">\*Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-151">\* Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="c6cfa-152">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="c6cfa-152">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>
>
> <span data-ttu-id="c6cfa-153">Ha egy Windows-alapú HDInsight-fürtöt használ, használhatja a hello [lekérdezés konzol](hdinsight-hadoop-use-hive-query-console.md) a böngészőből vagy [távoli asztal](hdinsight-hadoop-use-hive-remote-desktop.md) toorun Hive-lekérdezéseket.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-153">If you are using a Windows-based HDInsight cluster, you can use hello [Query console](hdinsight-hadoop-use-hive-query-console.md) from your browser or [Remote Desktop](hdinsight-hadoop-use-hive-remote-desktop.md) toorun Hive queries.</span></span>

## <a name="hiveql-language-reference"></a><span data-ttu-id="c6cfa-154">HiveQL nyelvi referencia</span><span class="sxs-lookup"><span data-stu-id="c6cfa-154">HiveQL language reference</span></span>

<span data-ttu-id="c6cfa-155">HiveQL nyelvi dokumentáció áll rendelkezésre a hello [nyelvi manuális (https://cwiki.apache.org/confluence/display/Hive/LanguageManual)](https://cwiki.apache.org/confluence/display/Hive/LanguageManual).</span><span class="sxs-lookup"><span data-stu-id="c6cfa-155">HiveQL language reference is available in hello [language manual (https://cwiki.apache.org/confluence/display/Hive/LanguageManual)](https://cwiki.apache.org/confluence/display/Hive/LanguageManual).</span></span>

## <a name="hive-and-data-structure"></a><span data-ttu-id="c6cfa-156">Hive és az adatok szerkezete</span><span class="sxs-lookup"><span data-stu-id="c6cfa-156">Hive and data structure</span></span>

<span data-ttu-id="c6cfa-157">Hive tisztában van azzal, hogyan toowork a strukturált és félig strukturált adatok.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-157">Hive understands how toowork with structured and semi-structured data.</span></span> <span data-ttu-id="c6cfa-158">Például szövegfájlok ahol hello mezők határolja különleges karaktereket.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-158">For example, text files where hello fields are delimited by specific characters.</span></span> <span data-ttu-id="c6cfa-159">a következő HiveQL utasítás hello táblázatot hoz létre szóközökkel elválasztott kötetnevek adatok:</span><span class="sxs-lookup"><span data-stu-id="c6cfa-159">hello following HiveQL statement creates a table over space-delimited data:</span></span>

```hiveql
CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
STORED AS TEXTFILE LOCATION '/example/data/';
```

<span data-ttu-id="c6cfa-160">Hive is támogatja az egyéni **szerializáló/deserializers (SerDe)** túl összetett vagy szabálytalan strukturált adatok számára.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-160">Hive also supports custom **serializer/deserializers (SerDe)** for complex or irregularly structured data.</span></span> <span data-ttu-id="c6cfa-161">További információkért lásd: hello [hogyan toouse a hdinsightban egyéni JSON SerDe](http://blogs.msdn.com/b/bigdatasupport/archive/2014/06/18/how-to-use-a-custom-json-serde-with-microsoft-azure-hdinsight.aspx) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-161">For more information, see hello [How toouse a custom JSON SerDe with HDInsight](http://blogs.msdn.com/b/bigdatasupport/archive/2014/06/18/how-to-use-a-custom-json-serde-with-microsoft-azure-hdinsight.aspx) document.</span></span>

<span data-ttu-id="c6cfa-162">A Hive támogatott fájlformátumok további információkért lásd: hello [nyelvi manuális (https://cwiki.apache.org/confluence/display/Hive/LanguageManual)](https://cwiki.apache.org/confluence/display/Hive/LanguageManual)</span><span class="sxs-lookup"><span data-stu-id="c6cfa-162">For more information on file formats supported by Hive, see hello [Language manual (https://cwiki.apache.org/confluence/display/Hive/LanguageManual)](https://cwiki.apache.org/confluence/display/Hive/LanguageManual)</span></span>

## <a name="hive-internal-tables-vs-external-tables"></a><span data-ttu-id="c6cfa-163">Belső tábla és a külső táblákra struktúra</span><span class="sxs-lookup"><span data-stu-id="c6cfa-163">Hive internal tables vs external tables</span></span>

<span data-ttu-id="c6cfa-164">Olyan táblázatot, amely a Hive hozhat létre két típusa van:</span><span class="sxs-lookup"><span data-stu-id="c6cfa-164">There are two types of tables that you can create with Hive:</span></span>

* <span data-ttu-id="c6cfa-165">__Belső__: hello Hive adatraktár adatokat tárolja.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-165">__Internal__: Data is stored in hello Hive data warehouse.</span></span> <span data-ttu-id="c6cfa-166">hello adatraktár itt található: `/hive/warehouse/` hello alapértelmezett tároló hello fürthöz.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-166">hello data warehouse is located at `/hive/warehouse/` on hello default storage for hello cluster.</span></span>

    <span data-ttu-id="c6cfa-167">Belső használja táblák esetén:</span><span class="sxs-lookup"><span data-stu-id="c6cfa-167">Use internal tables when:</span></span>

    * <span data-ttu-id="c6cfa-168">Adatok csak átmenetileg létezik.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-168">Data is temporary.</span></span>
    * <span data-ttu-id="c6cfa-169">Azt szeretné, hogy a struktúra toomanage hello életciklus hello táblázat és az adatok.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-169">You want Hive toomanage hello lifecycle of hello table and data.</span></span>

* <span data-ttu-id="c6cfa-170">__Külső__: kívül hello adatraktár tárolja.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-170">__External__: Data is stored outside hello data warehouse.</span></span> <span data-ttu-id="c6cfa-171">hello adatok hello fürt által elérhető minden tárterület tárolható.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-171">hello data can be stored on any storage accessible by hello cluster.</span></span>

    <span data-ttu-id="c6cfa-172">Használjon külső táblák esetén:</span><span class="sxs-lookup"><span data-stu-id="c6cfa-172">Use external tables when:</span></span>

    * <span data-ttu-id="c6cfa-173">hello adatok Hive kívül is használható.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-173">hello data is also used outside of Hive.</span></span> <span data-ttu-id="c6cfa-174">Például hello adatfájlok frissítése egy másik folyamat (vagyis nem zárolja hello fájlokat.)</span><span class="sxs-lookup"><span data-stu-id="c6cfa-174">For example, hello data files are updated by another process (that does not lock hello files.)</span></span>
    * <span data-ttu-id="c6cfa-175">Adatoknak az alapul szolgáló hely, hello tábla eldobása után is hello tooremain kell.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-175">Data needs tooremain in hello underlying location, even after dropping hello table.</span></span>
    * <span data-ttu-id="c6cfa-176">Egyéni helyen, például egy nem alapértelmezett tárfiók van szüksége.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-176">You need a custom location, such as a non-default storage account.</span></span>
    * <span data-ttu-id="c6cfa-177">Nem a hive kezeli a hello adatformátum hely, stb.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-177">A program other than hive manages hello data format, location, etc.</span></span>

<span data-ttu-id="c6cfa-178">További információkért lásd: hello [Hive belső és külső táblák bevezetés] [ cindygross-hive-tables] blogbejegyzést.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-178">For more information, see hello [Hive Internal and External Tables Intro][cindygross-hive-tables] blog post.</span></span>

## <a name="user-defined-functions-udf"></a><span data-ttu-id="c6cfa-179">Felhasználói függvény (UDF)</span><span class="sxs-lookup"><span data-stu-id="c6cfa-179">User-defined functions (UDF)</span></span>

<span data-ttu-id="c6cfa-180">Hive is kiterjeszthető keresztül **felhasználói függvény (UDF)**.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-180">Hive can also be extended through **user-defined functions (UDF)**.</span></span> <span data-ttu-id="c6cfa-181">Egy UDF lehetővé teszi tooimplement funkcionalitással kapcsolatban, vagy nem könnyen modellezve a HiveQL logika.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-181">A UDF allows you tooimplement functionality or logic that isn't easily modeled in HiveQL.</span></span> <span data-ttu-id="c6cfa-182">Például egy felhasználó által megadott függvények használata a Hive tekintse meg a következő dokumentumok hello:</span><span class="sxs-lookup"><span data-stu-id="c6cfa-182">For an example of using UDFs with Hive, see hello following documents:</span></span>

* [<span data-ttu-id="c6cfa-183">A Java-felhasználó által definiált függvény használata struktúra</span><span class="sxs-lookup"><span data-stu-id="c6cfa-183">Use a Java user-defined function with Hive</span></span>](hdinsight-hadoop-hive-java-udf.md)

* [<span data-ttu-id="c6cfa-184">A Python felhasználói függvény használata a Hive és a Pig használatával</span><span class="sxs-lookup"><span data-stu-id="c6cfa-184">Use a Python user-defined function with Hive and Pig</span></span>](hdinsight-python.md)

* [<span data-ttu-id="c6cfa-185">A C# felhasználó által definiált függvény használata Hive és a Pig használatával</span><span class="sxs-lookup"><span data-stu-id="c6cfa-185">Use a C# user-defined function with Hive and Pig</span></span>](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)

* [<span data-ttu-id="c6cfa-186">A felhasználó által definiált egyéni struktúra tooadd tooHDInsight működési módját</span><span class="sxs-lookup"><span data-stu-id="c6cfa-186">How tooadd a custom Hive user-defined function tooHDInsight</span></span>](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/14/how-to-add-custom-hive-udfs-to-hdinsight.aspx)

* [<span data-ttu-id="c6cfa-187">Egy példa Hive felhasználó által definiált függvény tooconvert dátum és idő formátumú tooHive időbélyeg</span><span class="sxs-lookup"><span data-stu-id="c6cfa-187">An example Hive user-defined function tooconvert date/time formats tooHive timestamp</span></span>](https://github.com/Azure-Samples/hdinsight-java-hive-udf)

## <span data-ttu-id="c6cfa-188"><a id="data"></a>Példa adatok</span><span class="sxs-lookup"><span data-stu-id="c6cfa-188"><a id="data"></a>Example data</span></span>

<span data-ttu-id="c6cfa-189">A HDInsight Hive előre betöltött tartalmaz egy belső tábla nevű `hivesampletable`.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-189">Hive on HDInsight comes pre-loaded with an internal table named `hivesampletable`.</span></span> <span data-ttu-id="c6cfa-190">HDInsight Hive használható például adatkészleteket is biztosít.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-190">HDInsight also provides example data sets that can be used with Hive.</span></span> <span data-ttu-id="c6cfa-191">Ezek az adathalmazok tárolódnak hello `/example/data` és `/HdiSamples` könyvtárak.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-191">These data sets are stored in hello `/example/data` and `/HdiSamples` directories.</span></span> <span data-ttu-id="c6cfa-192">Ezeket a könyvtárakat hello alapértelmezett tárolási a fürt szerepel.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-192">These directories exist in hello default storage for your cluster.</span></span>

## <span data-ttu-id="c6cfa-193"><a id="job"></a>Példa Hive-lekérdezések</span><span class="sxs-lookup"><span data-stu-id="c6cfa-193"><a id="job"></a>Example Hive query</span></span>

<span data-ttu-id="c6cfa-194">a következő HiveQL utasítások projektoszlopok alakzatot hello hello `/example/data/sample.log` fájlt:</span><span class="sxs-lookup"><span data-stu-id="c6cfa-194">hello following HiveQL statements project columns onto hello `/example/data/sample.log` file:</span></span>

    set hive.execution.engine=tez;
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION '/example/data/';
    SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

<span data-ttu-id="c6cfa-195">Hello előző példában hello HiveQL utasítások hajtsa végre a következő műveletek hello:</span><span class="sxs-lookup"><span data-stu-id="c6cfa-195">In hello previous example, hello HiveQL statements perform hello following actions:</span></span>

* <span data-ttu-id="c6cfa-196">`set hive.execution.engine=tez;`: Készletek hello végrehajtó motor toouse Tez.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-196">`set hive.execution.engine=tez;`: Sets hello execution engine toouse Tez.</span></span> <span data-ttu-id="c6cfa-197">Tez helyett MapReduce biztosíthat a lekérdezési teljesítmény növelését.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-197">Using Tez instead of MapReduce can provide an increase in query performance.</span></span> <span data-ttu-id="c6cfa-198">Tez további információkért lásd: hello [Apache Tez használja a jobb teljesítmény](#usetez) szakasz.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-198">For more information on Tez, see hello [Use Apache Tez for improved performance](#usetez) section.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c6cfa-199">Az utasítás csak egy Windows-alapú HDInsight-fürt használata esetén szükséges.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-199">This statement is only required when using a Windows-based HDInsight cluster.</span></span> <span data-ttu-id="c6cfa-200">Tez hello alapértelmezett végrehajtó motorja a Linux-alapú HDInsight.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-200">Tez is hello default execution engine for Linux-based HDInsight.</span></span>

* <span data-ttu-id="c6cfa-201">`DROP TABLE`: Ha hello tábla már létezik, törölje azt.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-201">`DROP TABLE`: If hello table already exists, delete it.</span></span>

* <span data-ttu-id="c6cfa-202">`CREATE EXTERNAL TABLE`: Létrehoz egy új **külső** Hive táblát.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-202">`CREATE EXTERNAL TABLE`: Creates a new **external** table in Hive.</span></span> <span data-ttu-id="c6cfa-203">Külső táblák csak tárolása Hive hello tábla definíciójában.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-203">External tables only store hello table definition in Hive.</span></span> <span data-ttu-id="c6cfa-204">hello adatok marad az eredeti helyükre hello és hello eredeti formátumban.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-204">hello data is left in hello original location and in hello original format.</span></span>

* <span data-ttu-id="c6cfa-205">`ROW FORMAT`: Közli a Hive hello adatok formázását.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-205">`ROW FORMAT`: Tells Hive how hello data is formatted.</span></span> <span data-ttu-id="c6cfa-206">Ebben az esetben az egyes naplókon hello mezők szóközzel elválasztva.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-206">In this case, hello fields in each log are separated by a space.</span></span>

* <span data-ttu-id="c6cfa-207">`STORED AS TEXTFILE LOCATION`: Közli Hive adott hello tárolja (hello `example/data` könyvtár) és szövegként tárolt.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-207">`STORED AS TEXTFILE LOCATION`: Tells Hive where hello data is stored (hello `example/data` directory) and that it is stored as text.</span></span> <span data-ttu-id="c6cfa-208">hello adatok egyetlen fájlban vagy több fájl hello könyvtárban lévő elosztva.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-208">hello data can be in one file or spread across multiple files within hello directory.</span></span>

* <span data-ttu-id="c6cfa-209">`SELECT`: Az összes sorok számát választ adott hello oszlop **t4** hello értéket tartalmaz **[hiba]**.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-209">`SELECT`: Selects a count of all rows where hello column **t4** contains hello value **[ERROR]**.</span></span> <span data-ttu-id="c6cfa-210">A jelen nyilatkozat értéket ad vissza, **3** mert három ezt az értéket tartalmazó sorok.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-210">This statement returns a value of **3** because there are three rows that contain this value.</span></span>

* <span data-ttu-id="c6cfa-211">`INPUT__FILE__NAME LIKE '%.log'`-Hive megpróbál tooapply hello tooall sémafájlok hello könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-211">`INPUT__FILE__NAME LIKE '%.log'` - Hive attempts tooapply hello schema tooall files in hello directory.</span></span> <span data-ttu-id="c6cfa-212">Hello directory ebben az esetben nem egyeznek meg a hello séma fájlokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-212">In this case, hello directory contains files that do not match hello schema.</span></span> <span data-ttu-id="c6cfa-213">tooprevent szemétgyűjtési adatok hello eredmények között, a jelen nyilatkozat közli struktúra, hogy azt kell csak vissza adatokat fájlok végződése. napló.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-213">tooprevent garbage data in hello results, this statement tells Hive that we should only return data from files ending in .log.</span></span>

> [!NOTE]
> <span data-ttu-id="c6cfa-214">Külső táblák kell használni, amikor hello frissíteni az külső forrás alapjául szolgáló adatok toobe várt.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-214">External tables should be used when you expect hello underlying data toobe updated by an external source.</span></span> <span data-ttu-id="c6cfa-215">Például egy automatizált adatok feltöltési folyamat vagy MapReduce művelethez.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-215">For example, an automated data upload process, or MapReduce operation.</span></span>
>
> <span data-ttu-id="c6cfa-216">A külső tábla eldobása does **nem** hello adatok törlése csak törli a hello tábla definíciójában.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-216">Dropping an external table does **not** delete hello data, it only deletes hello table definition.</span></span>

<span data-ttu-id="c6cfa-217">toocreate egy **belső** helyett külső tábla, a következő HiveQL hello használata:</span><span class="sxs-lookup"><span data-stu-id="c6cfa-217">toocreate an **internal** table instead of external, use hello following HiveQL:</span></span>

    set hive.execution.engine=tez;
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs
    SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]';

<span data-ttu-id="c6cfa-218">Ezekre az utasításokra hajtsa végre a következő műveletek hello:</span><span class="sxs-lookup"><span data-stu-id="c6cfa-218">These statements perform hello following actions:</span></span>

* <span data-ttu-id="c6cfa-219">`CREATE TABLE IF NOT EXISTS`: Ha hello tábla nem létezik, hozza létre.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-219">`CREATE TABLE IF NOT EXISTS`: If hello table does not exist, create it.</span></span> <span data-ttu-id="c6cfa-220">Mivel hello **külső** kulcsszó nem használható, a jelen nyilatkozat egy belső táblát hoz létre.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-220">Because hello **EXTERNAL** keyword is not used, this statement creates an internal table.</span></span> <span data-ttu-id="c6cfa-221">hello tábla hello Hive-adatraktárban tárolja, és Hive teljesen kezeli.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-221">hello table is stored in hello Hive data warehouse and is managed completely by Hive.</span></span>

* <span data-ttu-id="c6cfa-222">`STORED AS ORC`: A hello adatot tárol a optimalizált sor oszlopos (ORC) formátumban.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-222">`STORED AS ORC`: Stores hello data in Optimized Row Columnar (ORC) format.</span></span> <span data-ttu-id="c6cfa-223">ORC formátuma egy magas optimalizált és hatékony Hive adatainak tárolásához.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-223">ORC is a highly optimized and efficient format for storing Hive data.</span></span>

* <span data-ttu-id="c6cfa-224">`INSERT OVERWRITE ... SELECT`: Sorok kiválaszt hello **log4jLogs** tartalmazó tábla **[hiba]**, majd beszúrása hello hello az adatok és **errorLogs** tábla.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-224">`INSERT OVERWRITE ... SELECT`: Selects rows from hello **log4jLogs** table that contains **[ERROR]**, and then inserts hello data into hello **errorLogs** table.</span></span>

> [!NOTE]
> <span data-ttu-id="c6cfa-225">Ellentétben a külső táblákhoz eldobását egy belső tábla is törli hello alapul szolgáló adatokat.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-225">Unlike external tables, dropping an internal table also deletes hello underlying data.</span></span>

## <a name="improve-hive-query-performance"></a><span data-ttu-id="c6cfa-226">Hive-lekérdezések teljesítményének növelése</span><span class="sxs-lookup"><span data-stu-id="c6cfa-226">Improve Hive query performance</span></span>

### <span data-ttu-id="c6cfa-227"><a id="usetez"></a>Apache Tez</span><span class="sxs-lookup"><span data-stu-id="c6cfa-227"><a id="usetez"></a>Apache Tez</span></span>

<span data-ttu-id="c6cfa-228">[Apache Tez](http://tez.apache.org) egy keretrendszer, amely lehetővé teszi az adatok alkalmazások, például a Hive, sokkal hatékonyabban léptékű toorun.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-228">[Apache Tez](http://tez.apache.org) is a framework that allows data intensive applications, such as Hive, toorun much more efficiently at scale.</span></span> <span data-ttu-id="c6cfa-229">A Linux-alapú HDInsight-fürtökön alapértelmezés szerint engedélyezve van a Tez.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-229">Tez is enabled by default for Linux-based HDInsight clusters.</span></span>

> [!NOTE]
> <span data-ttu-id="c6cfa-230">Tez jelenleg ki alapértelmezés szerint a Windows-alapú HDInsight-fürtök és engedélyezni kell.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-230">Tez is currently off by default for Windows-based HDInsight clusters and it must be enabled.</span></span> <span data-ttu-id="c6cfa-231">Tez, a következő érték hello tootake előnyeit be kell állítani a Hive-lekérdezést:</span><span class="sxs-lookup"><span data-stu-id="c6cfa-231">tootake advantage of Tez, hello following value must be set for a Hive query:</span></span>
>
> `set hive.execution.engine=tez;`
>
> <span data-ttu-id="c6cfa-232">Tez hello alapértelmezett motor a Linux-alapú HDInsight-fürtök.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-232">Tez is hello default engine for Linux-based HDInsight clusters.</span></span>

<span data-ttu-id="c6cfa-233">Hello [Hive Tez tervezési dokumentumok](https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez) hello megvalósítási döntéseknek és hangolási konfigurációk részleteit tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-233">hello [Hive on Tez design documents](https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez) contains details about hello implementation choices and tuning configurations.</span></span>

<span data-ttu-id="c6cfa-234">a feladatok hibakeresés tooaid futott Tez használatával, a HDInsight lehetővé hello web UI, amelyek lehetővé teszik Tez feladatokhoz tooview részleteit a következő:</span><span class="sxs-lookup"><span data-stu-id="c6cfa-234">tooaid in debugging jobs ran using Tez, HDInsight provides hello following web UIs that allow you tooview details of Tez jobs:</span></span>

* [<span data-ttu-id="c6cfa-235">A Linux-alapú HDInsight Ambari Tez nézet hello használata</span><span class="sxs-lookup"><span data-stu-id="c6cfa-235">Use hello Ambari Tez view on Linux-based HDInsight</span></span>](hdinsight-debug-ambari-tez-view.md)

* [<span data-ttu-id="c6cfa-236">A Windows-alapú HDInsight hello Tez felhasználói felület használata</span><span class="sxs-lookup"><span data-stu-id="c6cfa-236">Use hello Tez UI on Windows-based HDInsight</span></span>](hdinsight-debug-tez-ui.md)

### <a name="low-latency-analytical-processing-llap"></a><span data-ttu-id="c6cfa-237">Kis késleltetésű analitikus feldolgozási (LLAP)</span><span class="sxs-lookup"><span data-stu-id="c6cfa-237">Low Latency Analytical Processing (LLAP)</span></span>

<span data-ttu-id="c6cfa-238">[LLAP](https://cwiki.apache.org/confluence/display/Hive/LLAP) (más néven hosszú Live és a folyamat), amely lehetővé teszi, hogy a memóriában történő gyorsítótárazás lekérdezések Hive 2.0 új szolgáltatása.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-238">[LLAP](https://cwiki.apache.org/confluence/display/Hive/LLAP) (sometimes known as Live Long and Process) is a new feature in Hive 2.0 that allows in-memory caching of queries.</span></span> <span data-ttu-id="c6cfa-239">LLAP teszi fel sokkal gyorsabb Hive-lekérdezések túl[26 x gyorsabb, mint a Hive 1.x bizonyos esetekben](https://hortonworks.com/blog/announcing-apache-hive-2-1-25x-faster-queries-much/).</span><span class="sxs-lookup"><span data-stu-id="c6cfa-239">LLAP makes Hive queries much faster, up too[26x faster than Hive 1.x in some cases](https://hortonworks.com/blog/announcing-apache-hive-2-1-25x-faster-queries-much/).</span></span>

<span data-ttu-id="c6cfa-240">A HDInsight fürt típusa interaktív Hive hello LLAP nyújt.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-240">HDInsight provides LLAP in hello Interactive Hive cluster type.</span></span> <span data-ttu-id="c6cfa-241">További információkért lásd: hello [interaktív Hive kezdődnie](hdinsight-hadoop-use-interactive-hive.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-241">For more information, see hello [Start with Interactive Hive](hdinsight-hadoop-use-interactive-hive.md) document.</span></span>

## <a name="hive-jobs-and-sql-server-integration-services"></a><span data-ttu-id="c6cfa-242">Hive-feladatok és az SQL Server Integration Services</span><span class="sxs-lookup"><span data-stu-id="c6cfa-242">Hive jobs and SQL Server Integration Services</span></span>

<span data-ttu-id="c6cfa-243">Használhatja az SQL Server Integration Services (SSIS) toorun egy Hive-feladatot.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-243">You can use SQL Server Integration Services (SSIS) toorun a Hive job.</span></span> <span data-ttu-id="c6cfa-244">hello Azure funkciócsomag SSIS a Hive-feladatok együttműködik a HDInsight összetevők a következő hello biztosít.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-244">hello Azure Feature Pack for SSIS provides hello following components that work with Hive jobs on HDInsight.</span></span>

* <span data-ttu-id="c6cfa-245">[Az Azure HDInsight Hive feladat][hivetask]</span><span class="sxs-lookup"><span data-stu-id="c6cfa-245">[Azure HDInsight Hive Task][hivetask]</span></span>

* <span data-ttu-id="c6cfa-246">[Az Azure előfizetés Csatlakozáskezelő][connectionmanager]</span><span class="sxs-lookup"><span data-stu-id="c6cfa-246">[Azure Subscription Connection Manager][connectionmanager]</span></span>

<span data-ttu-id="c6cfa-247">További információk hello Azure funkciócsomag SSIS [Itt][ssispack].</span><span class="sxs-lookup"><span data-stu-id="c6cfa-247">Learn more about hello Azure Feature Pack for SSIS [here][ssispack].</span></span>

## <span data-ttu-id="c6cfa-248"><a id="nextsteps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c6cfa-248"><a id="nextsteps"></a>Next steps</span></span>

<span data-ttu-id="c6cfa-249">Most, hogy megismerte Hive van, és hogyan hadooppal a Hdinsightban, a következő használatát hello hivatkozik tooexplore más módokon toowork Azure hdinsightban toouse.</span><span class="sxs-lookup"><span data-stu-id="c6cfa-249">Now that you've learned what Hive is and how toouse it with Hadoop in HDInsight, use hello following links tooexplore other ways toowork with Azure HDInsight.</span></span>

* <span data-ttu-id="c6cfa-250">[Adatok tooHDInsight feltöltése][hdinsight-upload-data]</span><span class="sxs-lookup"><span data-stu-id="c6cfa-250">[Upload data tooHDInsight][hdinsight-upload-data]</span></span>
* <span data-ttu-id="c6cfa-251">[A Pig használata a HDInsightban][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="c6cfa-251">[Use Pig with HDInsight][hdinsight-use-pig]</span></span>
* <span data-ttu-id="c6cfa-252">[MapReduce-feladatok használata a hdinsight eszközzel][hdinsight-use-mapreduce]</span><span class="sxs-lookup"><span data-stu-id="c6cfa-252">[Use MapReduce jobs with HDInsight][hdinsight-use-mapreduce]</span></span>

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
