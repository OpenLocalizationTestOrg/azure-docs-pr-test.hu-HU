---
title: "Hadoop a Pig használata a HDInsight - Azure PowerShell |} Microsoft Docs"
description: "Útmutató az Azure PowerShell hdinsight Hadoop-fürthöz Pig feladatok elküldéséhez."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 737089c1-b494-4387-9def-7b4dac3be532
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/16/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: 28904b07609ffb40a8195278fd1afd3957896733
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-azure-powershell-to-run-pig-jobs-with-hdinsight"></a><span data-ttu-id="4d08c-103">A Pig-feladatok futtatása a HDInsight az Azure PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="4d08c-103">Use Azure PowerShell to run Pig jobs with HDInsight</span></span>

[!INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

<span data-ttu-id="4d08c-104">Ez a dokumentum egy példát az Azure PowerShell elküldeni a Pig-feladatokhoz a Hadoop on HDInsight-fürt számára.</span><span class="sxs-lookup"><span data-stu-id="4d08c-104">This document provides an example of using Azure PowerShell to submit Pig jobs to a Hadoop on HDInsight cluster.</span></span> <span data-ttu-id="4d08c-105">A Pig MapReduce-feladatok írni egy nyelv (a Pig latin betűs) használatával adott modellek adatátalakítást helyett hozzárendelését és funkciók teszi lehetővé.</span><span class="sxs-lookup"><span data-stu-id="4d08c-105">Pig allows you to write MapReduce jobs by using a language (Pig Latin) that models data transformations, rather than map and reduce functions.</span></span>

> [!NOTE]
> <span data-ttu-id="4d08c-106">Ez a dokumentum nem biztosít a Pig Latin utasításokat a példákban szereplő mire részletes leírását.</span><span class="sxs-lookup"><span data-stu-id="4d08c-106">This document does not provide a detailed description of what the Pig Latin statements used in the examples do.</span></span> <span data-ttu-id="4d08c-107">A Pig Latin ebben a példában használt kapcsolatos információkért lásd: [a Pig használata a hdinsight Hadoop](hdinsight-use-pig.md).</span><span class="sxs-lookup"><span data-stu-id="4d08c-107">For information about the Pig Latin used in this example, see [Use Pig with Hadoop on HDInsight](hdinsight-use-pig.md).</span></span>

## <span data-ttu-id="4d08c-108"><a id="prereq"></a>Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="4d08c-108"><a id="prereq"></a>Prerequisites</span></span>

* <span data-ttu-id="4d08c-109">**Egy Azure HDInsight-fürt**</span><span class="sxs-lookup"><span data-stu-id="4d08c-109">**An Azure HDInsight cluster**</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="4d08c-110">A Linux az egyetlen operációs rendszer, amely a HDInsight 3.4-es vagy újabb verziói esetében használható.</span><span class="sxs-lookup"><span data-stu-id="4d08c-110">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="4d08c-111">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="4d08c-111">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="4d08c-112">**Munkaállomás Azure PowerShell-lel**.</span><span class="sxs-lookup"><span data-stu-id="4d08c-112">**A workstation with Azure PowerShell**.</span></span>

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

## <span data-ttu-id="4d08c-113"><a id="powershell"></a>Futtassa a PowerShell használatával Pig-feladatokhoz</span><span class="sxs-lookup"><span data-stu-id="4d08c-113"><a id="powershell"></a>Run Pig jobs using PowerShell</span></span>

<span data-ttu-id="4d08c-114">Az Azure PowerShell biztosít *parancsmagok* , amelyek lehetővé teszik, hogy távolról ugyanúgy futtathatják a HDInsight a Pig-feladatokhoz.</span><span class="sxs-lookup"><span data-stu-id="4d08c-114">Azure PowerShell provides *cmdlets* that allow you to remotely run Pig jobs on HDInsight.</span></span> <span data-ttu-id="4d08c-115">Belsőleg, PowerShell használja a többi hívások [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) futó a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="4d08c-115">Internally, PowerShell uses REST calls to [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) running on the HDInsight cluster.</span></span>

<span data-ttu-id="4d08c-116">A következő parancsmagok használhatók a Pig-feladatokhoz egy távoli HDInsight-fürt futtatásakor:</span><span class="sxs-lookup"><span data-stu-id="4d08c-116">The following cmdlets are used when running Pig jobs on a remote HDInsight cluster:</span></span>

* <span data-ttu-id="4d08c-117">**Login-AzureRmAccount**: hitelesíti az Azure-előfizetések az Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="4d08c-117">**Login-AzureRmAccount**: Authenticates Azure PowerShell to your Azure Subscription</span></span>
* <span data-ttu-id="4d08c-118">**Új AzureRmHDInsightPigJobDefinition**: létrehoz egy *definition feladat* a Pig Latin megadott utasítások használatával</span><span class="sxs-lookup"><span data-stu-id="4d08c-118">**New-AzureRmHDInsightPigJobDefinition**: Creates a *job definition* by using the specified Pig Latin statements</span></span>
* <span data-ttu-id="4d08c-119">**Start-AzureRmHDInsightJob**: a feladat definíciójához küld HDInsight, elindítja a feladatot, és adja vissza egy *feladat* objektum, amely segítségével a feladat állapotának ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="4d08c-119">**Start-AzureRmHDInsightJob**: Sends the job definition to HDInsight, starts the job, and returns a *job* object that can be used to check the status of the job</span></span>
* <span data-ttu-id="4d08c-120">**Várjon, amíg-AzureRmHDInsightJob**: a feladat állapotának ellenőrzése a feladatobjektum használja.</span><span class="sxs-lookup"><span data-stu-id="4d08c-120">**Wait-AzureRmHDInsightJob**: Uses the job object to check the status of the job.</span></span> <span data-ttu-id="4d08c-121">Arra vár, amíg a feladat befejeződött, vagy a várakozási idő túl lett lépve.</span><span class="sxs-lookup"><span data-stu-id="4d08c-121">It waits until the job has completed, or the wait time has been exceeded.</span></span>
* <span data-ttu-id="4d08c-122">**Get-AzureRmHDInsightJobOutput**: a feladat kimenetének beolvasása</span><span class="sxs-lookup"><span data-stu-id="4d08c-122">**Get-AzureRmHDInsightJobOutput**: Used to retrieve the output of the job</span></span>

<span data-ttu-id="4d08c-123">A következő lépések bemutatják, hogyan lehet ezeket a parancsmagokat használja a HDInsight-fürt a feladat futtatásához.</span><span class="sxs-lookup"><span data-stu-id="4d08c-123">The following steps demonstrate how to use these cmdlets to run a job on your HDInsight cluster.</span></span>

1. <span data-ttu-id="4d08c-124">Egy szerkesztővel, az alábbi kód, Mentés **pigjob.ps1**.</span><span class="sxs-lookup"><span data-stu-id="4d08c-124">Using an editor, save the following code as **pigjob.ps1**.</span></span>

    <span data-ttu-id="4d08c-125">[!code-powershell[fő](../../powershell_scripts/hdinsight/use-pig/use-pig.ps1?range=5-51)]</span><span class="sxs-lookup"><span data-stu-id="4d08c-125">[!code-powershell[main](../../powershell_scripts/hdinsight/use-pig/use-pig.ps1?range=5-51)]</span></span>

1. <span data-ttu-id="4d08c-126">Nyisson meg egy új Windows PowerShell parancssort.</span><span class="sxs-lookup"><span data-stu-id="4d08c-126">Open a new Windows PowerShell command prompt.</span></span> <span data-ttu-id="4d08c-127">Módosítsa a könyvtárat, hol található a **pigjob.ps1** fájlt, majd futtassa a parancsfájlt a következő paranccsal:</span><span class="sxs-lookup"><span data-stu-id="4d08c-127">Change directories to the location of the **pigjob.ps1** file, then use the following command to run the script:</span></span>

        .\pigjob.ps1

    <span data-ttu-id="4d08c-128">Jelentkezzen be az Azure-előfizetéshez kéri.</span><span class="sxs-lookup"><span data-stu-id="4d08c-128">You are prompted to log in to your Azure subscription.</span></span> <span data-ttu-id="4d08c-129">Ezt követően meg kell adnia azokat a a HTTPs/rendszergazda fiók nevét és jelszavát a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="4d08c-129">Then, you are asked for the HTTPs/Admin account name and password for the HDInsight cluster.</span></span>

2. <span data-ttu-id="4d08c-130">A feladat befejezése után, az kell visszaadnia információ az alábbihoz hasonló:</span><span class="sxs-lookup"><span data-stu-id="4d08c-130">When the job completes, it should return information similar to the following text:</span></span>

        Start the Pig job ...
        Wait for the Pig job to complete ...
        Display the standard output ...
        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

## <span data-ttu-id="4d08c-131"><a id="troubleshooting"></a>Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="4d08c-131"><a id="troubleshooting"></a>Troubleshooting</span></span>

<span data-ttu-id="4d08c-132">Ha nem áll rendelkezésre információ ad vissza, ha a feladat befejeződik, egy meghibásodott feldolgozása során.</span><span class="sxs-lookup"><span data-stu-id="4d08c-132">If no information is returned when the job completes, an error may have occurred during processing.</span></span> <span data-ttu-id="4d08c-133">Hiba történt a feladat információinak megtekintése, vegye fel a következő parancsot végén a **pigjob.ps1** fájl, mentse, majd futtassa újból.</span><span class="sxs-lookup"><span data-stu-id="4d08c-133">To view error information for this job, add the following command to the end of the **pigjob.ps1** file, save it, and then run it again.</span></span>

    # Print the output of the Pig job.
    Write-Host "Display the standard error output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $pigJob.JobId `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

<span data-ttu-id="4d08c-134">Ez írt ezzel a kiszolgálón a feladat futtatásakor olyan információkat ad vissza, és segíthet meghatározni, miért nem sikerült a feladat.</span><span class="sxs-lookup"><span data-stu-id="4d08c-134">This returns the information that was written to STDERR on the server when you ran the job, and it may help determine why the job is failing.</span></span>

## <span data-ttu-id="4d08c-135"><a id="summary"></a>Summary (Összefoglalás)</span><span class="sxs-lookup"><span data-stu-id="4d08c-135"><a id="summary"></a>Summary</span></span>
<span data-ttu-id="4d08c-136">Ahogy látja, Azure PowerShell itt egyszerűen futtassa a Pig-feladatokhoz a HDInsight-fürtöt, figyelheti a feladat állapotát és a kimeneti beolvasása.</span><span class="sxs-lookup"><span data-stu-id="4d08c-136">As you can see, Azure PowerShell provides an easy way to run Pig jobs on an HDInsight cluster, monitor the job status, and retrieve the output.</span></span>

## <span data-ttu-id="4d08c-137"><a id="nextsteps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4d08c-137"><a id="nextsteps"></a>Next steps</span></span>
<span data-ttu-id="4d08c-138">Általános információk a hdinsight Pig:</span><span class="sxs-lookup"><span data-stu-id="4d08c-138">For general information about Pig in HDInsight:</span></span>

* [<span data-ttu-id="4d08c-139">A Pig használata a HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="4d08c-139">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)

<span data-ttu-id="4d08c-140">Más módszerekkel kapcsolatos információk a HDInsight Hadoop dolgozhat:</span><span class="sxs-lookup"><span data-stu-id="4d08c-140">For information about other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="4d08c-141">A Hive használata a hdinsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="4d08c-141">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="4d08c-142">A HDInsight Hadoop MapReduce használata</span><span class="sxs-lookup"><span data-stu-id="4d08c-142">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)
