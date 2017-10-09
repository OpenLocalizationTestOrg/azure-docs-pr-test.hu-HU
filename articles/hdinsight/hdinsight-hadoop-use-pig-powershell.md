---
title: a hdinsight - Azure PowerShell Hadoop Pig aaaUse |} Microsoft Docs
description: "Ismerje meg, hogyan toosubmit Pig feladatok tooa Hadoop on HDInsight az Azure PowerShell fürt."
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
ms.openlocfilehash: 771617df203011eaec715a0dba6f5014a42877f3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-powershell-toorun-pig-jobs-with-hdinsight"></a><span data-ttu-id="9784d-103">Azure PowerShell toorun Pig-feladatokhoz és a HDInsight együttes használata</span><span class="sxs-lookup"><span data-stu-id="9784d-103">Use Azure PowerShell toorun Pig jobs with HDInsight</span></span>

[!INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

<span data-ttu-id="9784d-104">Ez a dokumentum egy példát az Azure PowerShell toosubmit Pig feladatok tooa Hadoop on HDInsight-fürt használatával.</span><span class="sxs-lookup"><span data-stu-id="9784d-104">This document provides an example of using Azure PowerShell toosubmit Pig jobs tooa Hadoop on HDInsight cluster.</span></span> <span data-ttu-id="9784d-105">A Pig lehetővé teszi egy nyelv (a Pig latin betűs) adatátalakítást, modellek segítségével toowrite MapReduce-feladatok helyett képezi le, és csökkentheti a funkciók.</span><span class="sxs-lookup"><span data-stu-id="9784d-105">Pig allows you toowrite MapReduce jobs by using a language (Pig Latin) that models data transformations, rather than map and reduce functions.</span></span>

> [!NOTE]
> <span data-ttu-id="9784d-106">Ez a dokumentum nem biztosít részletes leírása a hello Pig Latin utasítások hello a példákban szereplő tegye.</span><span class="sxs-lookup"><span data-stu-id="9784d-106">This document does not provide a detailed description of what hello Pig Latin statements used in hello examples do.</span></span> <span data-ttu-id="9784d-107">Ebben a példában használt Pig Latin hello kapcsolatos információkért lásd: [a Pig használata a hdinsight Hadoop](hdinsight-use-pig.md).</span><span class="sxs-lookup"><span data-stu-id="9784d-107">For information about hello Pig Latin used in this example, see [Use Pig with Hadoop on HDInsight](hdinsight-use-pig.md).</span></span>

## <span data-ttu-id="9784d-108"><a id="prereq"></a>Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="9784d-108"><a id="prereq"></a>Prerequisites</span></span>

* <span data-ttu-id="9784d-109">**Egy Azure HDInsight-fürt**</span><span class="sxs-lookup"><span data-stu-id="9784d-109">**An Azure HDInsight cluster**</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="9784d-110">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="9784d-110">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="9784d-111">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="9784d-111">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="9784d-112">**Munkaállomás Azure PowerShell-lel**.</span><span class="sxs-lookup"><span data-stu-id="9784d-112">**A workstation with Azure PowerShell**.</span></span>

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

## <span data-ttu-id="9784d-113"><a id="powershell"></a>Futtassa a PowerShell használatával Pig-feladatokhoz</span><span class="sxs-lookup"><span data-stu-id="9784d-113"><a id="powershell"></a>Run Pig jobs using PowerShell</span></span>

<span data-ttu-id="9784d-114">Az Azure PowerShell biztosít *parancsmagok* , amelyek lehetővé teszik futtatása tooremotely Pig-feladatokhoz a HDInsight-on.</span><span class="sxs-lookup"><span data-stu-id="9784d-114">Azure PowerShell provides *cmdlets* that allow you tooremotely run Pig jobs on HDInsight.</span></span> <span data-ttu-id="9784d-115">Belső, a PowerShell használ REST-hívások túl[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) hello HDInsight-fürtön futó.</span><span class="sxs-lookup"><span data-stu-id="9784d-115">Internally, PowerShell uses REST calls too[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) running on hello HDInsight cluster.</span></span>

<span data-ttu-id="9784d-116">hello következő parancsmagok használhatók egy távoli HDInsight-fürt Pig-feladatokhoz futtatásakor:</span><span class="sxs-lookup"><span data-stu-id="9784d-116">hello following cmdlets are used when running Pig jobs on a remote HDInsight cluster:</span></span>

* <span data-ttu-id="9784d-117">**Login-AzureRmAccount**: hitelesíti az Azure PowerShell tooyour Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="9784d-117">**Login-AzureRmAccount**: Authenticates Azure PowerShell tooyour Azure Subscription</span></span>
* <span data-ttu-id="9784d-118">**Új AzureRmHDInsightPigJobDefinition**: létrehoz egy *definition feladat* hello segítségével megadott Pig Latin utasítások</span><span class="sxs-lookup"><span data-stu-id="9784d-118">**New-AzureRmHDInsightPigJobDefinition**: Creates a *job definition* by using hello specified Pig Latin statements</span></span>
* <span data-ttu-id="9784d-119">**Start-AzureRmHDInsightJob**: hello feladat definition tooHDInsight küld, hello feladat elindul, és adja vissza egy *feladat* objektum, amely lehet használt toocheck hello hello feladat állapota</span><span class="sxs-lookup"><span data-stu-id="9784d-119">**Start-AzureRmHDInsightJob**: Sends hello job definition tooHDInsight, starts hello job, and returns a *job* object that can be used toocheck hello status of hello job</span></span>
* <span data-ttu-id="9784d-120">**Várjon, amíg-AzureRmHDInsightJob**: hello objektum toocheck hello feladatállapot hello feladat használja.</span><span class="sxs-lookup"><span data-stu-id="9784d-120">**Wait-AzureRmHDInsightJob**: Uses hello job object toocheck hello status of hello job.</span></span> <span data-ttu-id="9784d-121">Arra vár, amíg hello feladat befejeződött, vagy hello várakozási idő túl lett lépve.</span><span class="sxs-lookup"><span data-stu-id="9784d-121">It waits until hello job has completed, or hello wait time has been exceeded.</span></span>
* <span data-ttu-id="9784d-122">**Get-AzureRmHDInsightJobOutput**: hello feladat eredményének tooretrieve hello használt</span><span class="sxs-lookup"><span data-stu-id="9784d-122">**Get-AzureRmHDInsightJobOutput**: Used tooretrieve hello output of hello job</span></span>

<span data-ttu-id="9784d-123">hello következő lépések bemutatják, hogyan toouse ezen parancsmagok toorun egy feladatot a HDInsight-fürtre.</span><span class="sxs-lookup"><span data-stu-id="9784d-123">hello following steps demonstrate how toouse these cmdlets toorun a job on your HDInsight cluster.</span></span>

1. <span data-ttu-id="9784d-124">Egy szerkesztővel, mentse a következő kódot hello **pigjob.ps1**.</span><span class="sxs-lookup"><span data-stu-id="9784d-124">Using an editor, save hello following code as **pigjob.ps1**.</span></span>

    [!code-powershell[main](../../powershell_scripts/hdinsight/use-pig/use-pig.ps1?range=5-51)]

1. <span data-ttu-id="9784d-125">Nyisson meg egy új Windows PowerShell parancssort.</span><span class="sxs-lookup"><span data-stu-id="9784d-125">Open a new Windows PowerShell command prompt.</span></span> <span data-ttu-id="9784d-126">Hello könyvtárak toohello módosítani **pigjob.ps1** fájlt, majd a következő parancsfájl toorun hello hello használata:</span><span class="sxs-lookup"><span data-stu-id="9784d-126">Change directories toohello location of hello **pigjob.ps1** file, then use hello following command toorun hello script:</span></span>

        .\pigjob.ps1

    <span data-ttu-id="9784d-127">Az Azure-előfizetés tooyour rákérdezéses toolog áll.</span><span class="sxs-lookup"><span data-stu-id="9784d-127">You are prompted toolog in tooyour Azure subscription.</span></span> <span data-ttu-id="9784d-128">Ezt követően felkérést hello HTTPs/rendszergazdai fiók felhasználónevét és jelszavát hello HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="9784d-128">Then, you are asked for hello HTTPs/Admin account name and password for hello HDInsight cluster.</span></span>

2. <span data-ttu-id="9784d-129">Hello feladat befejezése után az információkat a következő szöveg hasonló toohello kell visszaadnia:</span><span class="sxs-lookup"><span data-stu-id="9784d-129">When hello job completes, it should return information similar toohello following text:</span></span>

        Start hello Pig job ...
        Wait for hello Pig job toocomplete ...
        Display hello standard output ...
        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

## <span data-ttu-id="9784d-130"><a id="troubleshooting"></a>Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="9784d-130"><a id="troubleshooting"></a>Troubleshooting</span></span>

<span data-ttu-id="9784d-131">Ha nem áll rendelkezésre információ ad vissza, ha hello feladat befejeződik, egy meghibásodott feldolgozása során.</span><span class="sxs-lookup"><span data-stu-id="9784d-131">If no information is returned when hello job completes, an error may have occurred during processing.</span></span> <span data-ttu-id="9784d-132">Ez a feladat információi tooview hiba hozzáadása a következő parancs toohello vége hello hello **pigjob.ps1** fájl, mentse, majd futtassa újból.</span><span class="sxs-lookup"><span data-stu-id="9784d-132">tooview error information for this job, add hello following command toohello end of hello **pigjob.ps1** file, save it, and then run it again.</span></span>

    # Print hello output of hello Pig job.
    Write-Host "Display hello standard error output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $pigJob.JobId `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

<span data-ttu-id="9784d-133">Írt tooSTDERR hello kiszolgálón hello feladat futtatásakor hello adatait adja vissza. Ez, és segíthet meghatározni, miért hello feladat sikertelen.</span><span class="sxs-lookup"><span data-stu-id="9784d-133">This returns hello information that was written tooSTDERR on hello server when you ran hello job, and it may help determine why hello job is failing.</span></span>

## <span data-ttu-id="9784d-134"><a id="summary"></a>Summary (Összefoglalás)</span><span class="sxs-lookup"><span data-stu-id="9784d-134"><a id="summary"></a>Summary</span></span>
<span data-ttu-id="9784d-135">Ahogy látja, Azure PowerShell Ez egy egyszerű módot toorun Pig-feladatokhoz egy HDInsight-fürthöz, a figyelő hello feladat állapotát, és a lekérése hello kimeneti.</span><span class="sxs-lookup"><span data-stu-id="9784d-135">As you can see, Azure PowerShell provides an easy way toorun Pig jobs on an HDInsight cluster, monitor hello job status, and retrieve hello output.</span></span>

## <span data-ttu-id="9784d-136"><a id="nextsteps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9784d-136"><a id="nextsteps"></a>Next steps</span></span>
<span data-ttu-id="9784d-137">Általános információk a hdinsight Pig:</span><span class="sxs-lookup"><span data-stu-id="9784d-137">For general information about Pig in HDInsight:</span></span>

* [<span data-ttu-id="9784d-138">A Pig használata a HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="9784d-138">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)

<span data-ttu-id="9784d-139">Más módszerekkel kapcsolatos információk a HDInsight Hadoop dolgozhat:</span><span class="sxs-lookup"><span data-stu-id="9784d-139">For information about other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="9784d-140">A Hive használata a hdinsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="9784d-140">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="9784d-141">A HDInsight Hadoop MapReduce használata</span><span class="sxs-lookup"><span data-stu-id="9784d-141">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)
