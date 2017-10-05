---
title: "MapReduce és a PowerShell használata a Hadoop - az Azure HDInsight |} Microsoft Docs"
description: "Tudnivalók a PowerShell használatával távolról ugyanúgy futtathatják a HDInsight Hadoop a MapReduce-feladatok."
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
ms.openlocfilehash: c3801573808709f29cb1e563ac803f225a28cafc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="run-mapreduce-jobs-with-hadoop-on-hdinsight-using-powershell"></a><span data-ttu-id="2e238-103">A PowerShell használatával HDInsight Hadoop MapReduce-feladatok futtassa</span><span class="sxs-lookup"><span data-stu-id="2e238-103">Run MapReduce jobs with Hadoop on HDInsight using PowerShell</span></span>

[!INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

<span data-ttu-id="2e238-104">Ez a dokumentum egy példát a MapReduce-feladatok futtatásához egy Hadoop on HDInsight-fürt Azure PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="2e238-104">This document provides an example of using Azure PowerShell to run a MapReduce job in a Hadoop on HDInsight cluster.</span></span>

## <span data-ttu-id="2e238-105"><a id="prereq"></a>Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="2e238-105"><a id="prereq"></a>Prerequisites</span></span>

* <span data-ttu-id="2e238-106">**(A HDInsight Hadoop) Azure HDInsight-fürtök**</span><span class="sxs-lookup"><span data-stu-id="2e238-106">**An Azure HDInsight (Hadoop on HDInsight) cluster**</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="2e238-107">A Linux az egyetlen operációs rendszer, amely a HDInsight 3.4-es vagy újabb verziói esetében használható.</span><span class="sxs-lookup"><span data-stu-id="2e238-107">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="2e238-108">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="2e238-108">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="2e238-109">**Munkaállomás Azure PowerShell-lel**.</span><span class="sxs-lookup"><span data-stu-id="2e238-109">**A workstation with Azure PowerShell**.</span></span>

## <span data-ttu-id="2e238-110"><a id="powershell"></a>A MapReduce feladatot az Azure PowerShell használatával futtassa</span><span class="sxs-lookup"><span data-stu-id="2e238-110"><a id="powershell"></a>Run a MapReduce job using Azure PowerShell</span></span>

<span data-ttu-id="2e238-111">Az Azure PowerShell biztosít *parancsmagok* , amelyek lehetővé teszik, hogy távolról a HDInsight a MapReduce-feladatok futtatását.</span><span class="sxs-lookup"><span data-stu-id="2e238-111">Azure PowerShell provides *cmdlets* that allow you to remotely run MapReduce jobs on HDInsight.</span></span> <span data-ttu-id="2e238-112">Belsőleg, mindez REST-hívások segítségével [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (korábbi nevén lépni a Templeton) fut a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="2e238-112">Internally, this is accomplished by using REST calls to [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (formerly called Templeton) running on the HDInsight cluster.</span></span>

<span data-ttu-id="2e238-113">A következő parancsmagokat használja a MapReduce-feladatok futtatása egy távoli HDInsight-fürt.</span><span class="sxs-lookup"><span data-stu-id="2e238-113">The following cmdlets are used when running MapReduce jobs in a remote HDInsight cluster.</span></span>

* <span data-ttu-id="2e238-114">**Login-AzureRmAccount**: Azure PowerShell hitelesíti az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="2e238-114">**Login-AzureRmAccount**: Authenticates Azure PowerShell to your Azure subscription.</span></span>

* <span data-ttu-id="2e238-115">**Új AzureRmHDInsightMapReduceJobDefinition**: létrehoz egy új *definition feladat* MapReduce megadott információk segítségével.</span><span class="sxs-lookup"><span data-stu-id="2e238-115">**New-AzureRmHDInsightMapReduceJobDefinition**: Creates a new *job definition* by using the specified MapReduce information.</span></span>

* <span data-ttu-id="2e238-116">**Start-AzureRmHDInsightJob**: a feladat definíciójához küld HDInsight, elindítja a feladatot, és adja vissza egy *feladat* objektum, amely segítségével a feladat állapotának ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="2e238-116">**Start-AzureRmHDInsightJob**: Sends the job definition to HDInsight, starts the job, and returns a *job* object that can be used to check the status of the job.</span></span>

* <span data-ttu-id="2e238-117">**Várjon, amíg-AzureRmHDInsightJob**: a feladat állapotának ellenőrzése a feladatobjektum használja.</span><span class="sxs-lookup"><span data-stu-id="2e238-117">**Wait-AzureRmHDInsightJob**: Uses the job object to check the status of the job.</span></span> <span data-ttu-id="2e238-118">Arra vár, amíg a feladat befejeződik, vagy a várakozási ideje lejár.</span><span class="sxs-lookup"><span data-stu-id="2e238-118">It waits until the job completes or the wait time is exceeded.</span></span>

* <span data-ttu-id="2e238-119">**Get-AzureRmHDInsightJobOutput**: a feladat kimenetének beolvasása.</span><span class="sxs-lookup"><span data-stu-id="2e238-119">**Get-AzureRmHDInsightJobOutput**: Used to retrieve the output of the job.</span></span>

<span data-ttu-id="2e238-120">A következő lépések bemutatják, hogyan lehet ezeket a parancsmagokat használja a feladat futtatásához a HDInsight-fürtön.</span><span class="sxs-lookup"><span data-stu-id="2e238-120">The following steps demonstrate how to use these cmdlets to run a job in your HDInsight cluster.</span></span>

1. <span data-ttu-id="2e238-121">Egy szerkesztővel, az alábbi kód, Mentés **mapreducejob.ps1**.</span><span class="sxs-lookup"><span data-stu-id="2e238-121">Using an editor, save the following code as **mapreducejob.ps1**.</span></span>

    <span data-ttu-id="2e238-122">[!code-powershell[fő](../../powershell_scripts/hdinsight/use-mapreduce/use-mapreduce.ps1?range=5-69)]</span><span class="sxs-lookup"><span data-stu-id="2e238-122">[!code-powershell[main](../../powershell_scripts/hdinsight/use-mapreduce/use-mapreduce.ps1?range=5-69)]</span></span>

2. <span data-ttu-id="2e238-123">Nyisson meg egy új **Azure PowerShell** parancssort.</span><span class="sxs-lookup"><span data-stu-id="2e238-123">Open a new **Azure PowerShell** command prompt.</span></span> <span data-ttu-id="2e238-124">Módosítsa a könyvtárat, hol található a **mapreducejob.ps1** fájlt, majd futtassa a parancsfájlt a következő paranccsal:</span><span class="sxs-lookup"><span data-stu-id="2e238-124">Change directories to the location of the **mapreducejob.ps1** file, then use the following command to run the script:</span></span>

        .\mapreducejob.ps1

    <span data-ttu-id="2e238-125">A parancsprogram futtatásakor kéri a HDInsight-fürt nevét és a HTTPS/rendszergazda fiók nevét és a jelszót a fürthöz.</span><span class="sxs-lookup"><span data-stu-id="2e238-125">When you run the script, you are prompted for the name of the HDInsight cluster and the HTTPS/Admin account name and password for the cluster.</span></span> <span data-ttu-id="2e238-126">Is kérheti, hogy az Azure-előfizetéshez hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="2e238-126">You may also be prompted to authenticate to your Azure subscription.</span></span>

3. <span data-ttu-id="2e238-127">A feladat befejeződik, a kimenet az alábbihoz hasonló jelenhet meg:</span><span class="sxs-lookup"><span data-stu-id="2e238-127">When the job completes, you receive output similar to the following text:</span></span>

        Cluster         : CLUSTERNAME
        ExitCode        : 0
        Name            : wordcount
        PercentComplete : map 100% reduce 100%
        Query           :
        State           : Completed
        StatusDirectory : f1ed2028-afe8-402f-a24b-13cc17858097
        SubmissionTime  : 12/5/2014 8:34:09 PM
        JobId           : job_1415949758166_0071

    <span data-ttu-id="2e238-128">A kimeneti azt jelzi, hogy a feladat sikeresen befejeződött-e.</span><span class="sxs-lookup"><span data-stu-id="2e238-128">This output indicates that the job completed successfully.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2e238-129">Ha a **ExitCode** értéke csak 0, lásd: [hibaelhárítás](#troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="2e238-129">If the **ExitCode** is a value other than 0, see [Troubleshooting](#troubleshooting).</span></span>

    <span data-ttu-id="2e238-130">Ebben a példában a letöltött fájlokat tárolja egy **kimenet.txt** fájl a könyvtárban, amely futtatja a parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="2e238-130">This example also stores the downloaded files to an **output.txt** file in the directory that you run the script from.</span></span>

### <a name="view-output"></a><span data-ttu-id="2e238-131">Nézet kimeneti</span><span class="sxs-lookup"><span data-stu-id="2e238-131">View output</span></span>

<span data-ttu-id="2e238-132">Nyissa meg a **kimenet.txt** fájlt egy szövegszerkesztőben, a szavakat, és a feladat által létrehozott számát.</span><span class="sxs-lookup"><span data-stu-id="2e238-132">Open the **output.txt** file in a text editor to see the words and counts produced by the job.</span></span>

> [!NOTE]
> <span data-ttu-id="2e238-133">A MapReduce feladatot, kimeneti fájlok nem módosíthatók.</span><span class="sxs-lookup"><span data-stu-id="2e238-133">The output files of a MapReduce job are immutable.</span></span> <span data-ttu-id="2e238-134">Ezért ez a minta fut újra, ha módosítani szeretné a kimeneti fájl nevét.</span><span class="sxs-lookup"><span data-stu-id="2e238-134">So if you rerun this sample, you need to change the name of the output file.</span></span>

## <span data-ttu-id="2e238-135"><a id="troubleshooting"></a>Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="2e238-135"><a id="troubleshooting"></a>Troubleshooting</span></span>

<span data-ttu-id="2e238-136">Ha nem áll rendelkezésre információ ad vissza, ha a feladat befejeződik, egy meghibásodott feldolgozása során.</span><span class="sxs-lookup"><span data-stu-id="2e238-136">If no information is returned when the job completes, an error may have occurred during processing.</span></span> <span data-ttu-id="2e238-137">Hiba történt a feladat információinak megtekintése, vegye fel a következő parancsot végén a **mapreducejob.ps1** fájl, mentse, majd futtassa újból.</span><span class="sxs-lookup"><span data-stu-id="2e238-137">To view error information for this job, add the following command to the end of the **mapreducejob.ps1** file, save it, and then run it again.</span></span>

```powershell
# Print the output of the WordCount job.
Write-Host "Display the standard output ..." -ForegroundColor Green
Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $wordCountJob.JobId `
        -HttpCredential $creds `
        -DisplayOutputType StandardError
```

<span data-ttu-id="2e238-138">Ez a parancsmag írt ezzel a kiszolgálón a feladat futtatásakor olyan információkat ad vissza, és segíthet meghatározni, miért nem sikerült a feladat.</span><span class="sxs-lookup"><span data-stu-id="2e238-138">This cmdlet returns the information that was written to STDERR on the server when you ran the job, and it may help determine why the job is failing.</span></span>

## <span data-ttu-id="2e238-139"><a id="summary"></a>Summary (Összefoglalás)</span><span class="sxs-lookup"><span data-stu-id="2e238-139"><a id="summary"></a>Summary</span></span>

<span data-ttu-id="2e238-140">Ahogy látja, Azure PowerShell könnyedén MapReduce-feladatok futtatása a HDInsight-fürtöt, figyelheti a feladat állapotát és a kimeneti beolvasása.</span><span class="sxs-lookup"><span data-stu-id="2e238-140">As you can see, Azure PowerShell provides an easy way to run MapReduce jobs on an HDInsight cluster, monitor the job status, and retrieve the output.</span></span>

## <span data-ttu-id="2e238-141"><a id="nextsteps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2e238-141"><a id="nextsteps"></a>Next steps</span></span>

<span data-ttu-id="2e238-142">Általános információk a hdinsight MapReduce-feladatok:</span><span class="sxs-lookup"><span data-stu-id="2e238-142">For general information about MapReduce jobs in HDInsight:</span></span>

* [<span data-ttu-id="2e238-143">A HDInsight Hadoop MapReduce használata</span><span class="sxs-lookup"><span data-stu-id="2e238-143">Use MapReduce on HDInsight Hadoop</span></span>](hdinsight-use-mapreduce.md)

<span data-ttu-id="2e238-144">Más módszerekkel kapcsolatos információk a HDInsight Hadoop dolgozhat:</span><span class="sxs-lookup"><span data-stu-id="2e238-144">For information about other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="2e238-145">A Hive használata a hdinsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="2e238-145">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="2e238-146">A Pig használata a HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="2e238-146">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)
