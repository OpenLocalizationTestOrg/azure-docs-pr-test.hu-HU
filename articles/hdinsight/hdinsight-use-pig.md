---
title: hdinsight Hadoop Pig aaaUse |} Microsoft Docs
description: "Megtudhatja, hogyan toouse a HDInsight hadooppal sertésfelmérés."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: acfeb52b-4b81-4a7d-af77-3e9908407404
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/15/2017
ms.author: larryfr
ms.openlocfilehash: 90850f2c742b8954c66ce277127e01b14fc3906f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-pig-with-hadoop-on-hdinsight"></a><span data-ttu-id="91044-103">A Pig használata a HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="91044-103">Use Pig with Hadoop on HDInsight</span></span>

<span data-ttu-id="91044-104">Megtudhatja, hogyan toouse [Apache Pig](http://pig.apache.org/) a hdinsight eszközzel...</span><span class="sxs-lookup"><span data-stu-id="91044-104">Learn how toouse [Apache Pig](http://pig.apache.org/) with HDInsight...</span></span>

<span data-ttu-id="91044-105">A Pig egy platform programok létrehozásához a Hadoop által ismert eljárási nyelv használatával *Pig Latin*.</span><span class="sxs-lookup"><span data-stu-id="91044-105">Pig is a platform for creating programs for Hadoop by using a procedural language known as *Pig Latin*.</span></span> <span data-ttu-id="91044-106">A Pig egy alternatív tooJava létrehozásához *MapReduce* megoldások, és az Azure HDInsight része.</span><span class="sxs-lookup"><span data-stu-id="91044-106">Pig is an alternative tooJava for creating *MapReduce* solutions, and it is included with Azure HDInsight.</span></span> <span data-ttu-id="91044-107">A következő tábla toodiscover hello módszert arról, hogy használható-e a Pig with HDInsight hello használata:</span><span class="sxs-lookup"><span data-stu-id="91044-107">Use hello following table toodiscover hello various ways that Pig can be used with HDInsight:</span></span>

| <span data-ttu-id="91044-108">**Ezzel** Ha azt szeretné...</span><span class="sxs-lookup"><span data-stu-id="91044-108">**Use this** if you want...</span></span> | <span data-ttu-id="91044-109">.. .an **interaktív** rendszerhéj</span><span class="sxs-lookup"><span data-stu-id="91044-109">...an **interactive** shell</span></span> | <span data-ttu-id="91044-110">... **kötegelt** feldolgozása</span><span class="sxs-lookup"><span data-stu-id="91044-110">...**batch** processing</span></span> | <span data-ttu-id="91044-111">és mivel ez **fürt operációs rendszer**</span><span class="sxs-lookup"><span data-stu-id="91044-111">...with this **cluster operating system**</span></span> | <span data-ttu-id="91044-112">.. .from ez **ügyfél operációs rendszer**</span><span class="sxs-lookup"><span data-stu-id="91044-112">...from this **client operating system**</span></span> |
|:--- |:---:|:---:|:--- |:--- |
| [<span data-ttu-id="91044-113">SSH</span><span class="sxs-lookup"><span data-stu-id="91044-113">SSH</span></span>](hdinsight-hadoop-use-pig-ssh.md) |<span data-ttu-id="91044-114">✔</span><span class="sxs-lookup"><span data-stu-id="91044-114">✔</span></span> |<span data-ttu-id="91044-115">✔</span><span class="sxs-lookup"><span data-stu-id="91044-115">✔</span></span> |<span data-ttu-id="91044-116">Linux</span><span class="sxs-lookup"><span data-stu-id="91044-116">Linux</span></span> |<span data-ttu-id="91044-117">Linux, Unix, Mac OS X vagy Windows</span><span class="sxs-lookup"><span data-stu-id="91044-117">Linux, Unix, Mac OS X, or Windows</span></span> |
| [<span data-ttu-id="91044-118">REST API</span><span class="sxs-lookup"><span data-stu-id="91044-118">REST API</span></span>](hdinsight-hadoop-use-pig-curl.md) |&nbsp; |<span data-ttu-id="91044-119">✔</span><span class="sxs-lookup"><span data-stu-id="91044-119">✔</span></span> |<span data-ttu-id="91044-120">Linux- vagy Windows</span><span class="sxs-lookup"><span data-stu-id="91044-120">Linux or Windows</span></span> |<span data-ttu-id="91044-121">Linux, Unix, Mac OS X vagy Windows</span><span class="sxs-lookup"><span data-stu-id="91044-121">Linux, Unix, Mac OS X, or Windows</span></span> |
| [<span data-ttu-id="91044-122">.NET SDK a Hadoophoz</span><span class="sxs-lookup"><span data-stu-id="91044-122">.NET SDK for Hadoop</span></span>](hdinsight-hadoop-use-pig-dotnet-sdk.md) |&nbsp; |<span data-ttu-id="91044-123">✔</span><span class="sxs-lookup"><span data-stu-id="91044-123">✔</span></span> |<span data-ttu-id="91044-124">Linux- vagy Windows</span><span class="sxs-lookup"><span data-stu-id="91044-124">Linux or Windows</span></span> |<span data-ttu-id="91044-125">(Egyelőre) Windows</span><span class="sxs-lookup"><span data-stu-id="91044-125">Windows (for now)</span></span> |
| [<span data-ttu-id="91044-126">A Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="91044-126">Windows PowerShell</span></span>](hdinsight-hadoop-use-pig-powershell.md) |&nbsp; |<span data-ttu-id="91044-127">✔</span><span class="sxs-lookup"><span data-stu-id="91044-127">✔</span></span> |<span data-ttu-id="91044-128">Linux- vagy Windows</span><span class="sxs-lookup"><span data-stu-id="91044-128">Linux or Windows</span></span> |<span data-ttu-id="91044-129">Windows</span><span class="sxs-lookup"><span data-stu-id="91044-129">Windows</span></span> |
| <span data-ttu-id="91044-130">[A távoli asztal](hdinsight-hadoop-use-pig-remote-desktop.md) (HDInsight 3.2-es és 3.3-as)</span><span class="sxs-lookup"><span data-stu-id="91044-130">[Remote Desktop](hdinsight-hadoop-use-pig-remote-desktop.md) (HDInsight 3.2 and 3.3)</span></span> |<span data-ttu-id="91044-131">✔</span><span class="sxs-lookup"><span data-stu-id="91044-131">✔</span></span> |<span data-ttu-id="91044-132">✔</span><span class="sxs-lookup"><span data-stu-id="91044-132">✔</span></span> |<span data-ttu-id="91044-133">Windows</span><span class="sxs-lookup"><span data-stu-id="91044-133">Windows</span></span> |<span data-ttu-id="91044-134">Windows</span><span class="sxs-lookup"><span data-stu-id="91044-134">Windows</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="91044-135">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="91044-135">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="91044-136">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="91044-136">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <span data-ttu-id="91044-137"><a id="why"></a>Miért érdemes használni a Pig</span><span class="sxs-lookup"><span data-stu-id="91044-137"><a id="why"></a>Why use Pig</span></span>

<span data-ttu-id="91044-138">A feldolgozó logika csak térkép és a reduce függvény használatával végrehajtó egyik hello kihívásai adatfeldolgozás MapReduce a Hadoop használatával.</span><span class="sxs-lookup"><span data-stu-id="91044-138">One of hello challenges of processing data by using MapReduce in Hadoop is implementing your processing logic by using only a map and a reduce function.</span></span> <span data-ttu-id="91044-139">Gyakran összetett feldolgozási nincs több MapReduce műveleteket történő feldolgozás toobreak összeláncolt tooachieve szükséges hello eredménye.</span><span class="sxs-lookup"><span data-stu-id="91044-139">For complex processing, you often have toobreak processing into multiple MapReduce operations that are chained together tooachieve hello desired result.</span></span>

<span data-ttu-id="91044-140">A Pig lehetővé teszi, hogy az adatfolyamok keresztül tooproduce szükséges hello kimeneti hello átalakítások sorozataként feldolgozása toodefine.</span><span class="sxs-lookup"><span data-stu-id="91044-140">Pig allows you toodefine processing as a series of transformations that hello data flows through tooproduce hello desired output.</span></span>

<span data-ttu-id="91044-141">hello Pig Latin nyelv lehetővé teszi, hogy Ön toodescribe hello adatfolyama nyers bemeneti, egy vagy több átalakítások, tooproduce szükséges hello kimeneti keresztül.</span><span class="sxs-lookup"><span data-stu-id="91044-141">hello Pig Latin language allows you toodescribe hello data flow from raw input, through one or more transformations, tooproduce hello desired output.</span></span> <span data-ttu-id="91044-142">A Pig Latin programok kövesse az ebben a mintában általános:</span><span class="sxs-lookup"><span data-stu-id="91044-142">Pig Latin programs follow this general pattern:</span></span>

* <span data-ttu-id="91044-143">**Betöltési**: hello fájlrendszerből választéka adatok toobe olvasása</span><span class="sxs-lookup"><span data-stu-id="91044-143">**Load**: Read data toobe manipulated from hello file system</span></span>

* <span data-ttu-id="91044-144">**Átalakítás**: kezelhetők hello adatok</span><span class="sxs-lookup"><span data-stu-id="91044-144">**Transform**: Manipulate hello data</span></span>

* <span data-ttu-id="91044-145">**Memóriakép, vagy tárolja**: kimeneti adatok toohello képernyőjén, vagy tárolja el azt a feldolgozás</span><span class="sxs-lookup"><span data-stu-id="91044-145">**Dump or store**: Output data toohello screen or store it for processing</span></span>

### <a name="user-defined-functions"></a><span data-ttu-id="91044-146">Felhasználó által definiált függvények</span><span class="sxs-lookup"><span data-stu-id="91044-146">User-defined functions</span></span>

<span data-ttu-id="91044-147">A Pig Latin is támogatja a felhasználói függvény (UDF), amely lehetővé teszi a Pig Latin nehéz toomodel logikát megvalósító tooinvoke külső összetevők.</span><span class="sxs-lookup"><span data-stu-id="91044-147">Pig Latin also supports user-defined functions (UDF), which allows you tooinvoke external components that implement logic that is difficult toomodel in Pig Latin.</span></span>

<span data-ttu-id="91044-148">A Pig Latin kapcsolatos további információkért lásd: [Pig Latin hivatkozás manuális 1](http://pig.apache.org/docs/r0.7.0/piglatin_ref1.html) és [Pig Latin hivatkozás manuális 2](http://pig.apache.org/docs/r0.7.0/piglatin_ref2.html).</span><span class="sxs-lookup"><span data-stu-id="91044-148">For more information about Pig Latin, see [Pig Latin Reference Manual 1](http://pig.apache.org/docs/r0.7.0/piglatin_ref1.html) and [Pig Latin Reference Manual 2](http://pig.apache.org/docs/r0.7.0/piglatin_ref2.html).</span></span>

<span data-ttu-id="91044-149">Például egy felhasználó által megadott függvények használata a Pig tekintse meg a következő dokumentumok hello:</span><span class="sxs-lookup"><span data-stu-id="91044-149">For an example of using UDFs with Pig, see hello following documents:</span></span>

* <span data-ttu-id="91044-150">[DataFu használata a Hdinsightban Pig](hdinsight-hadoop-use-pig-datafu-udf.md) -DataFu hasznos felhasználó által megadott függvények Apache által fenntartott gyűjteménye</span><span class="sxs-lookup"><span data-stu-id="91044-150">[Use DataFu with Pig in HDInsight](hdinsight-hadoop-use-pig-datafu-udf.md) - DataFu is a collection of useful UDFs maintained by Apache</span></span>
* [<span data-ttu-id="91044-151">Python az Pig és a Hive HDInsight használata</span><span class="sxs-lookup"><span data-stu-id="91044-151">Use Python with Pig and Hive in HDInsight</span></span>](hdinsight-python.md)
* [<span data-ttu-id="91044-152">C# az Hive és a hdinsight a Pig használata</span><span class="sxs-lookup"><span data-stu-id="91044-152">Use C# with Hive and Pig in HDInsight</span></span>](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)

## <span data-ttu-id="91044-153"><a id="data"></a>Példa adatok</span><span class="sxs-lookup"><span data-stu-id="91044-153"><a id="data"></a>Example data</span></span>

<span data-ttu-id="91044-154">A HDInsight lehetővé különböző példa adatkészletek, hello tárolt `/example/data` és `/HdiSamples` könyvtárak.</span><span class="sxs-lookup"><span data-stu-id="91044-154">HDInsight provides various example data sets, which are stored in hello `/example/data` and `/HdiSamples` directories.</span></span> <span data-ttu-id="91044-155">Ezeket a könyvtárakat hello alapértelmezett tárolási a fürt szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="91044-155">These directories are in hello default storage for your cluster.</span></span> <span data-ttu-id="91044-156">hello Pig ebben a dokumentumban példában hello *log4j* fájlként `/example/data/sample.log`.</span><span class="sxs-lookup"><span data-stu-id="91044-156">hello Pig example in this document uses hello *log4j* file from `/example/data/sample.log`.</span></span>

<span data-ttu-id="91044-157">Egyes naplókon belül hello fájl tartalmaz egy mezőket tartalmazó sor egy `[LOG LEVEL]` tooshow hello típusát és hello súlyossági, például mezőben:</span><span class="sxs-lookup"><span data-stu-id="91044-157">Each log inside hello file consists of a line of fields that contains a `[LOG LEVEL]` field tooshow hello type and hello severity, for example:</span></span>

    2012-02-03 20:26:41 SampleClass3 [ERROR] verbose detail for id 1527353937

<span data-ttu-id="91044-158">Hello előző példában hello naplózási szint: Hiba történt.</span><span class="sxs-lookup"><span data-stu-id="91044-158">In hello previous example, hello log level is ERROR.</span></span>

> [!NOTE]
> <span data-ttu-id="91044-159">Hello segítségével is létrehozhat egy log4j fájl [Apache Log4j](http://en.wikipedia.org/wiki/Log4j) eszköz naplózása és az, hogy a fájl tooyour blob majd feltölteni.</span><span class="sxs-lookup"><span data-stu-id="91044-159">You can also generate a log4j file by using hello [Apache Log4j](http://en.wikipedia.org/wiki/Log4j) logging tool and then upload that file tooyour blob.</span></span> <span data-ttu-id="91044-160">Lásd: [adatok feltöltése tooHDInsight](hdinsight-upload-data.md) utasításokat.</span><span class="sxs-lookup"><span data-stu-id="91044-160">See [Upload Data tooHDInsight](hdinsight-upload-data.md) for instructions.</span></span> <span data-ttu-id="91044-161">Az Azure storage blobs használata a hdinsight eszközzel kapcsolatos további információkért lásd: [az Azure Blob Storage a hdinsight eszközzel](hdinsight-hadoop-use-blob-storage.md).</span><span class="sxs-lookup"><span data-stu-id="91044-161">For more information about how blobs in Azure storage are used with HDInsight, see [Use Azure Blob Storage with HDInsight](hdinsight-hadoop-use-blob-storage.md).</span></span>

## <span data-ttu-id="91044-162"><a id="job"></a>Példa feladat</span><span class="sxs-lookup"><span data-stu-id="91044-162"><a id="job"></a>Example job</span></span>

<span data-ttu-id="91044-163">hello következő Pig Latin feladat betölti hello `sample.log` hello alapértelmezett tárolási fájlból a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="91044-163">hello following Pig Latin job loads hello `sample.log` file from hello default storage for your HDInsight cluster.</span></span> <span data-ttu-id="91044-164">Végrehajtja eredményező hogyan számítja sokszor minden naplózási szint keletkezett hello bemeneti adatok átalakítások sorozata.</span><span class="sxs-lookup"><span data-stu-id="91044-164">Then it performs a series of transformations that result in a count of how many times each log level occurred in hello input data.</span></span> <span data-ttu-id="91044-165">hello eredmények tooSTDOUT készültek.</span><span class="sxs-lookup"><span data-stu-id="91044-165">hello results are written tooSTDOUT.</span></span>

    LOGS = LOAD 'wasb:///example/data/sample.log';
    LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
    FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
    GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
    FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
    RESULT = order FREQUENCIES by COUNT desc;
    DUMP RESULT;

<span data-ttu-id="91044-166">hello következő kép bemutatja, milyen minden átalakítása does toohello adatok összegzését.</span><span class="sxs-lookup"><span data-stu-id="91044-166">hello following image shows a summary of what each transformation does toohello data.</span></span>

![Hello átalakítások grafikus ábrázolása][image-hdi-pig-data-transformation]

## <span data-ttu-id="91044-168"><a id="run"></a>Hello Pig Latin feladat futtatása</span><span class="sxs-lookup"><span data-stu-id="91044-168"><a id="run"></a>Run hello Pig Latin job</span></span>

<span data-ttu-id="91044-169">A Pig Latin feladatok futtatásával a HDInsight többféle módszerrel.</span><span class="sxs-lookup"><span data-stu-id="91044-169">HDInsight can run Pig Latin jobs by using a variety of methods.</span></span> <span data-ttu-id="91044-170">Használja a következő tábla toodecide, melyik módszert részesíti az Ön számára legmegfelelőbb hello, majd kövesse az útmutatást hello hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="91044-170">Use hello following table toodecide which method is right for you, then follow hello link for a walkthrough.</span></span>

| <span data-ttu-id="91044-171">**Ezzel** Ha azt szeretné...</span><span class="sxs-lookup"><span data-stu-id="91044-171">**Use this** if you want...</span></span> | <span data-ttu-id="91044-172">.. .an **interaktív** rendszerhéj</span><span class="sxs-lookup"><span data-stu-id="91044-172">...an **interactive** shell</span></span> | <span data-ttu-id="91044-173">... **kötegelt** feldolgozása</span><span class="sxs-lookup"><span data-stu-id="91044-173">...**batch** processing</span></span> | <span data-ttu-id="91044-174">és mivel ez **fürt operációs rendszer**</span><span class="sxs-lookup"><span data-stu-id="91044-174">...with this **cluster operating system**</span></span> | <span data-ttu-id="91044-175">.. .from ez **ügyfél**</span><span class="sxs-lookup"><span data-stu-id="91044-175">...from this **client**</span></span> |
|:--- |:---:|:---:|:--- |:--- |
| [<span data-ttu-id="91044-176">SSH</span><span class="sxs-lookup"><span data-stu-id="91044-176">SSH</span></span>](hdinsight-hadoop-use-pig-ssh.md) |<span data-ttu-id="91044-177">✔</span><span class="sxs-lookup"><span data-stu-id="91044-177">✔</span></span> |<span data-ttu-id="91044-178">✔</span><span class="sxs-lookup"><span data-stu-id="91044-178">✔</span></span> |<span data-ttu-id="91044-179">Linux</span><span class="sxs-lookup"><span data-stu-id="91044-179">Linux</span></span> |<span data-ttu-id="91044-180">Linux, Unix, Mac OS X vagy Windows</span><span class="sxs-lookup"><span data-stu-id="91044-180">Linux, Unix, Mac OS X, or Windows</span></span> |
| [<span data-ttu-id="91044-181">Curl</span><span class="sxs-lookup"><span data-stu-id="91044-181">Curl</span></span>](hdinsight-hadoop-use-pig-curl.md) |&nbsp; |<span data-ttu-id="91044-182">✔</span><span class="sxs-lookup"><span data-stu-id="91044-182">✔</span></span> |<span data-ttu-id="91044-183">Linux- vagy Windows</span><span class="sxs-lookup"><span data-stu-id="91044-183">Linux or Windows</span></span> |<span data-ttu-id="91044-184">Linux, Unix, Mac OS X vagy Windows</span><span class="sxs-lookup"><span data-stu-id="91044-184">Linux, Unix, Mac OS X, or Windows</span></span> |
| [<span data-ttu-id="91044-185">.NET SDK a Hadoophoz</span><span class="sxs-lookup"><span data-stu-id="91044-185">.NET SDK for Hadoop</span></span>](hdinsight-hadoop-use-pig-dotnet-sdk.md) |&nbsp; |<span data-ttu-id="91044-186">✔</span><span class="sxs-lookup"><span data-stu-id="91044-186">✔</span></span> |<span data-ttu-id="91044-187">Linux- vagy Windows</span><span class="sxs-lookup"><span data-stu-id="91044-187">Linux or Windows</span></span> |<span data-ttu-id="91044-188">(Egyelőre) Windows</span><span class="sxs-lookup"><span data-stu-id="91044-188">Windows (for now)</span></span> |
| [<span data-ttu-id="91044-189">A Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="91044-189">Windows PowerShell</span></span>](hdinsight-hadoop-use-pig-powershell.md) |&nbsp; |<span data-ttu-id="91044-190">✔</span><span class="sxs-lookup"><span data-stu-id="91044-190">✔</span></span> |<span data-ttu-id="91044-191">Linux- vagy Windows</span><span class="sxs-lookup"><span data-stu-id="91044-191">Linux or Windows</span></span> |<span data-ttu-id="91044-192">Windows</span><span class="sxs-lookup"><span data-stu-id="91044-192">Windows</span></span> |
| <span data-ttu-id="91044-193">[A távoli asztal](hdinsight-hadoop-use-pig-remote-desktop.md) (HDInsight 3.2-es és 3.3-as)</span><span class="sxs-lookup"><span data-stu-id="91044-193">[Remote Desktop](hdinsight-hadoop-use-pig-remote-desktop.md) (HDInsight 3.2 and 3.3)</span></span> |<span data-ttu-id="91044-194">✔</span><span class="sxs-lookup"><span data-stu-id="91044-194">✔</span></span> |<span data-ttu-id="91044-195">✔</span><span class="sxs-lookup"><span data-stu-id="91044-195">✔</span></span> |<span data-ttu-id="91044-196">Windows</span><span class="sxs-lookup"><span data-stu-id="91044-196">Windows</span></span> |<span data-ttu-id="91044-197">Windows</span><span class="sxs-lookup"><span data-stu-id="91044-197">Windows</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="91044-198">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="91044-198">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="91044-199">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="91044-199">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="pig-and-sql-server-integration-services"></a><span data-ttu-id="91044-200">A Pig és az SQL Server Integration Services</span><span class="sxs-lookup"><span data-stu-id="91044-200">Pig and SQL Server Integration Services</span></span>

<span data-ttu-id="91044-201">SQL Server Integration Services (SSIS) toorun a Pig feladatot is használhatja.</span><span class="sxs-lookup"><span data-stu-id="91044-201">You can use SQL Server Integration Services (SSIS) toorun a Pig job.</span></span> <span data-ttu-id="91044-202">hello Azure funkciócsomag SSIS biztosít, amelyek használhatók a Pig-feladatokhoz a HDInsight összetevők a következő hello.</span><span class="sxs-lookup"><span data-stu-id="91044-202">hello Azure Feature Pack for SSIS provides hello following components that work with Pig jobs on HDInsight.</span></span>

* <span data-ttu-id="91044-203">[Az Azure HDInsight a Pig feladatot][pigtask]</span><span class="sxs-lookup"><span data-stu-id="91044-203">[Azure HDInsight Pig Task][pigtask]</span></span>

* <span data-ttu-id="91044-204">[Az Azure előfizetés Csatlakozáskezelő][connectionmanager]</span><span class="sxs-lookup"><span data-stu-id="91044-204">[Azure Subscription Connection Manager][connectionmanager]</span></span>

<span data-ttu-id="91044-205">További információk hello Azure funkciócsomag SSIS [Itt][ssispack].</span><span class="sxs-lookup"><span data-stu-id="91044-205">Learn more about hello Azure Feature Pack for SSIS [here][ssispack].</span></span>

## <span data-ttu-id="91044-206"><a id="nextsteps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="91044-206"><a id="nextsteps"></a>Next steps</span></span>
<span data-ttu-id="91044-207">Most, hogy megtanulta, hogyan toouse Pig with HDInsight, a következő használatát hello hivatkozásait tooexplore más módokon toowork Azure hdinsightban.</span><span class="sxs-lookup"><span data-stu-id="91044-207">Now that you have learned how toouse Pig with HDInsight, use hello following links tooexplore other ways toowork with Azure HDInsight.</span></span>

* <span data-ttu-id="91044-208">[Adatok tooHDInsight feltöltése][hdinsight-upload-data]</span><span class="sxs-lookup"><span data-stu-id="91044-208">[Upload data tooHDInsight][hdinsight-upload-data]</span></span>
* <span data-ttu-id="91044-209">[A Hive használata a HDInsightban][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="91044-209">[Use Hive with HDInsight][hdinsight-use-hive]</span></span>
* [<span data-ttu-id="91044-210">Use Sqoop with HDInsight</span><span class="sxs-lookup"><span data-stu-id="91044-210">Use Sqoop with HDInsight</span></span>](hdinsight-use-sqoop.md)
* [<span data-ttu-id="91044-211">Oozie használata a hdinsight eszközzel</span><span class="sxs-lookup"><span data-stu-id="91044-211">Use Oozie with HDInsight</span></span>](hdinsight-use-oozie.md)
* <span data-ttu-id="91044-212">[MapReduce-feladatok használata a hdinsight eszközzel][hdinsight-use-mapreduce]</span><span class="sxs-lookup"><span data-stu-id="91044-212">[Use MapReduce jobs with HDInsight][hdinsight-use-mapreduce]</span></span>

[apachepig-home]: http://pig.apache.org/
[putty]: http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html
[curl]: http://curl.haxx.se/
[pigtask]: http://msdn.microsoft.com/library/mt146781(v=sql.120).aspx
[connectionmanager]: http://msdn.microsoft.com/library/mt146773(v=sql.120).aspx
[ssispack]: http://msdn.microsoft.com/library/mt146770(v=sql.120).aspx


[hdinsight-upload-data]: hdinsight-upload-data.md

[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md#mapreduce-sdk

[Powershell-install-configure]: /powershell/azureps-cmdlets-docs

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx


[image-hdi-pig-data-transformation]: ./media/hdinsight-use-pig/HDI.DataTransformation.gif
