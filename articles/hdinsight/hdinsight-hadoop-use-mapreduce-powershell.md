---
title: "aaaUse MapReduce és a Hadoop - Azure HDInsight PowerShell |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse PowerShell tooremotely hibaüzenettel MapReduce-feladatok Hadoop on HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 21b56d32-1785-4d44-8ae8-94467c12cfba
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/16/2017
ms.author: larryfr
ms.openlocfilehash: 59524f0e8813d4c017f92bccb2e50d4c018acf71
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="run-mapreduce-jobs-with-hadoop-on-hdinsight-using-powershell"></a><span data-ttu-id="8b943-103">A PowerShell használatával HDInsight Hadoop MapReduce-feladatok futtassa</span><span class="sxs-lookup"><span data-stu-id="8b943-103">Run MapReduce jobs with Hadoop on HDInsight using PowerShell</span></span>

[!INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

<span data-ttu-id="8b943-104">A dokumentum a egy Hadoop MapReduce feladatot biztosít Azure PowerShell toorun használatának példája a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="8b943-104">This document provides an example of using Azure PowerShell toorun a MapReduce job in a Hadoop on HDInsight cluster.</span></span>

## <span data-ttu-id="8b943-105"><a id="prereq"></a>Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="8b943-105"><a id="prereq"></a>Prerequisites</span></span>

* <span data-ttu-id="8b943-106">**(A HDInsight Hadoop) Azure HDInsight-fürtök**</span><span class="sxs-lookup"><span data-stu-id="8b943-106">**An Azure HDInsight (Hadoop on HDInsight) cluster**</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="8b943-107">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="8b943-107">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="8b943-108">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="8b943-108">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="8b943-109">**Munkaállomás Azure PowerShell-lel**.</span><span class="sxs-lookup"><span data-stu-id="8b943-109">**A workstation with Azure PowerShell**.</span></span>

## <span data-ttu-id="8b943-110"><a id="powershell"></a>A MapReduce feladatot az Azure PowerShell használatával futtassa</span><span class="sxs-lookup"><span data-stu-id="8b943-110"><a id="powershell"></a>Run a MapReduce job using Azure PowerShell</span></span>

<span data-ttu-id="8b943-111">Az Azure PowerShell biztosít *parancsmagok* , amelyek lehetővé teszik MapReduce-feladatok futtatása tooremotely a hdinsight platformon.</span><span class="sxs-lookup"><span data-stu-id="8b943-111">Azure PowerShell provides *cmdlets* that allow you tooremotely run MapReduce jobs on HDInsight.</span></span> <span data-ttu-id="8b943-112">Belsőleg, mindez túl REST-hívások segítségével[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (korábbi nevén lépni a Templeton) futó hello HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="8b943-112">Internally, this is accomplished by using REST calls too[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (formerly called Templeton) running on hello HDInsight cluster.</span></span>

<span data-ttu-id="8b943-113">hello következő parancsmagok használhatók egy távoli HDInsight-fürt MapReduce-feladatok futásakor.</span><span class="sxs-lookup"><span data-stu-id="8b943-113">hello following cmdlets are used when running MapReduce jobs in a remote HDInsight cluster.</span></span>

* <span data-ttu-id="8b943-114">**Login-AzureRmAccount**: hitelesíti az Azure PowerShell tooyour Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="8b943-114">**Login-AzureRmAccount**: Authenticates Azure PowerShell tooyour Azure subscription.</span></span>

* <span data-ttu-id="8b943-115">**Új AzureRmHDInsightMapReduceJobDefinition**: létrehoz egy új *definition feladat* hello segítségével megadott MapReduce információkat.</span><span class="sxs-lookup"><span data-stu-id="8b943-115">**New-AzureRmHDInsightMapReduceJobDefinition**: Creates a new *job definition* by using hello specified MapReduce information.</span></span>

* <span data-ttu-id="8b943-116">**Start-AzureRmHDInsightJob**: hello feladat definition tooHDInsight küld, hello feladat elindul, és adja vissza egy *feladat* objektum, amely használt toocheck hello hello feladat állapota lehet.</span><span class="sxs-lookup"><span data-stu-id="8b943-116">**Start-AzureRmHDInsightJob**: Sends hello job definition tooHDInsight, starts hello job, and returns a *job* object that can be used toocheck hello status of hello job.</span></span>

* <span data-ttu-id="8b943-117">**Várjon, amíg-AzureRmHDInsightJob**: hello objektum toocheck hello feladatállapot hello feladat használja.</span><span class="sxs-lookup"><span data-stu-id="8b943-117">**Wait-AzureRmHDInsightJob**: Uses hello job object toocheck hello status of hello job.</span></span> <span data-ttu-id="8b943-118">Arra vár, amíg hello feladat befejeződik, vagy hello várakozási ideje lejár.</span><span class="sxs-lookup"><span data-stu-id="8b943-118">It waits until hello job completes or hello wait time is exceeded.</span></span>

* <span data-ttu-id="8b943-119">**Get-AzureRmHDInsightJobOutput**: hello feladat eredményének tooretrieve hello használt.</span><span class="sxs-lookup"><span data-stu-id="8b943-119">**Get-AzureRmHDInsightJobOutput**: Used tooretrieve hello output of hello job.</span></span>

<span data-ttu-id="8b943-120">hello következő lépések bemutatják, hogyan toouse ezen parancsmagok toorun egy feladat a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="8b943-120">hello following steps demonstrate how toouse these cmdlets toorun a job in your HDInsight cluster.</span></span>

1. <span data-ttu-id="8b943-121">Egy szerkesztővel, mentse a következő kódot hello **mapreducejob.ps1**.</span><span class="sxs-lookup"><span data-stu-id="8b943-121">Using an editor, save hello following code as **mapreducejob.ps1**.</span></span>

    [!code-powershell[main](../../powershell_scripts/hdinsight/use-mapreduce/use-mapreduce.ps1?range=5-69)]

2. <span data-ttu-id="8b943-122">Nyisson meg egy új **Azure PowerShell** parancssort.</span><span class="sxs-lookup"><span data-stu-id="8b943-122">Open a new **Azure PowerShell** command prompt.</span></span> <span data-ttu-id="8b943-123">Hello könyvtárak toohello módosítani **mapreducejob.ps1** fájlt, majd a következő parancsfájl toorun hello hello használata:</span><span class="sxs-lookup"><span data-stu-id="8b943-123">Change directories toohello location of hello **mapreducejob.ps1** file, then use hello following command toorun hello script:</span></span>

        .\mapreducejob.ps1

    <span data-ttu-id="8b943-124">Hello parancsprogram futtatásakor hello hello HDInsight-fürt nevét és a hello HTTPS/rendszergazda fiók nevét és a jelszó hello fürt kéri.</span><span class="sxs-lookup"><span data-stu-id="8b943-124">When you run hello script, you are prompted for hello name of hello HDInsight cluster and hello HTTPS/Admin account name and password for hello cluster.</span></span> <span data-ttu-id="8b943-125">Azure-előfizetés. kért tooauthenticate tooyour is lehet.</span><span class="sxs-lookup"><span data-stu-id="8b943-125">You may also be prompted tooauthenticate tooyour Azure subscription.</span></span>

3. <span data-ttu-id="8b943-126">Hello feladat befejeződik, a szöveg a következő kimeneti hasonló toohello jelenhet meg:</span><span class="sxs-lookup"><span data-stu-id="8b943-126">When hello job completes, you receive output similar toohello following text:</span></span>

        Cluster         : CLUSTERNAME
        ExitCode        : 0
        Name            : wordcount
        PercentComplete : map 100% reduce 100%
        Query           :
        State           : Completed
        StatusDirectory : f1ed2028-afe8-402f-a24b-13cc17858097
        SubmissionTime  : 12/5/2014 8:34:09 PM
        JobId           : job_1415949758166_0071

    <span data-ttu-id="8b943-127">A kimenet hello feladat sikeresen befejeződött.</span><span class="sxs-lookup"><span data-stu-id="8b943-127">This output indicates that hello job completed successfully.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8b943-128">Ha hello **ExitCode** értéke csak 0, lásd: [hibaelhárítás](#troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="8b943-128">If hello **ExitCode** is a value other than 0, see [Troubleshooting](#troubleshooting).</span></span>

    <span data-ttu-id="8b943-129">Ebben a példában is tárolja a letöltött hello fájlok tooan **kimenet.txt** fájl hello hello parancsfájlt futtató.</span><span class="sxs-lookup"><span data-stu-id="8b943-129">This example also stores hello downloaded files tooan **output.txt** file in hello directory that you run hello script from.</span></span>

### <a name="view-output"></a><span data-ttu-id="8b943-130">Nézet kimeneti</span><span class="sxs-lookup"><span data-stu-id="8b943-130">View output</span></span>

<span data-ttu-id="8b943-131">Nyissa meg hello **kimenet.txt** szavak és hello feladat által előállított adatokra is egy text editor toosee hello fájlban.</span><span class="sxs-lookup"><span data-stu-id="8b943-131">Open hello **output.txt** file in a text editor toosee hello words and counts produced by hello job.</span></span>

> [!NOTE]
> <span data-ttu-id="8b943-132">hello kimeneti fájlokat a MapReduce-feladatok nem módosíthatók.</span><span class="sxs-lookup"><span data-stu-id="8b943-132">hello output files of a MapReduce job are immutable.</span></span> <span data-ttu-id="8b943-133">Így ha ez a minta újrafuttatásához kell toochange hello hello kimeneti fájl nevét.</span><span class="sxs-lookup"><span data-stu-id="8b943-133">So if you rerun this sample, you need toochange hello name of hello output file.</span></span>

## <span data-ttu-id="8b943-134"><a id="troubleshooting"></a>Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="8b943-134"><a id="troubleshooting"></a>Troubleshooting</span></span>

<span data-ttu-id="8b943-135">Ha nem áll rendelkezésre információ ad vissza, ha hello feladat befejeződik, egy meghibásodott feldolgozása során.</span><span class="sxs-lookup"><span data-stu-id="8b943-135">If no information is returned when hello job completes, an error may have occurred during processing.</span></span> <span data-ttu-id="8b943-136">Ez a feladat információi tooview hiba hozzáadása a következő parancs toohello vége hello hello **mapreducejob.ps1** fájl, mentse, majd futtassa újból.</span><span class="sxs-lookup"><span data-stu-id="8b943-136">tooview error information for this job, add hello following command toohello end of hello **mapreducejob.ps1** file, save it, and then run it again.</span></span>

```powershell
# Print hello output of hello WordCount job.
Write-Host "Display hello standard output ..." -ForegroundColor Green
Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $wordCountJob.JobId `
        -HttpCredential $creds `
        -DisplayOutputType StandardError
```

<span data-ttu-id="8b943-137">Ez a parancsmag írt tooSTDERR hello kiszolgálón hello feladat futtatásakor hello-adatait adja vissza, és segíthet meghatározni, miért hello feladat sikertelen.</span><span class="sxs-lookup"><span data-stu-id="8b943-137">This cmdlet returns hello information that was written tooSTDERR on hello server when you ran hello job, and it may help determine why hello job is failing.</span></span>

## <span data-ttu-id="8b943-138"><a id="summary"></a>Summary (Összefoglalás)</span><span class="sxs-lookup"><span data-stu-id="8b943-138"><a id="summary"></a>Summary</span></span>

<span data-ttu-id="8b943-139">Ahogy látja, Azure PowerShell Ez egy egyszerű módot toorun MapReduce-feladatok egy HDInsight-fürthöz, a figyelő hello feladat állapotát, és a lekérése hello kimeneti.</span><span class="sxs-lookup"><span data-stu-id="8b943-139">As you can see, Azure PowerShell provides an easy way toorun MapReduce jobs on an HDInsight cluster, monitor hello job status, and retrieve hello output.</span></span>

## <span data-ttu-id="8b943-140"><a id="nextsteps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8b943-140"><a id="nextsteps"></a>Next steps</span></span>

<span data-ttu-id="8b943-141">Általános információk a hdinsight MapReduce-feladatok:</span><span class="sxs-lookup"><span data-stu-id="8b943-141">For general information about MapReduce jobs in HDInsight:</span></span>

* [<span data-ttu-id="8b943-142">A HDInsight Hadoop MapReduce használata</span><span class="sxs-lookup"><span data-stu-id="8b943-142">Use MapReduce on HDInsight Hadoop</span></span>](hdinsight-use-mapreduce.md)

<span data-ttu-id="8b943-143">Más módszerekkel kapcsolatos információk a HDInsight Hadoop dolgozhat:</span><span class="sxs-lookup"><span data-stu-id="8b943-143">For information about other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="8b943-144">A Hive használata a hdinsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="8b943-144">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="8b943-145">A Pig használata a HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="8b943-145">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)
