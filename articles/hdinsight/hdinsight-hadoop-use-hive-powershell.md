---
title: "Hadoop Hive használata a hdinsight - Azure PowerShell |} Microsoft Docs"
description: "HDInsight Hadoop Hive-lekérdezéseket futtatni a PowerShell segítségével."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: cb795b7c-bcd0-497a-a7f0-8ed18ef49195
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/16/2017
ms.author: larryfr
ms.openlocfilehash: e1cb2e4a1fc82fb43082e79a5feba71b81b3eaa8
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="run-hive-queries-using-powershell"></a><span data-ttu-id="03c5d-103">PowerShell-lel Hive-lekérdezések futtatása</span><span class="sxs-lookup"><span data-stu-id="03c5d-103">Run Hive queries using PowerShell</span></span>
[!INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

<span data-ttu-id="03c5d-104">Ez a dokumentum az Azure PowerShell használatával az Azure-erőforráscsoport módban a Hive-lekérdezések futtatásához egy Hadoop on HDInsight-fürt példaként szolgál.</span><span class="sxs-lookup"><span data-stu-id="03c5d-104">This document provides an example of using Azure PowerShell in the Azure Resource Group mode to run Hive queries in a Hadoop on HDInsight cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="03c5d-105">Ez a dokumentum nem biztosít a HiveQL utasításokat a példákban használt mire részletes leírását.</span><span class="sxs-lookup"><span data-stu-id="03c5d-105">This document does not provide a detailed description of what the HiveQL statements that are used in the examples do.</span></span> <span data-ttu-id="03c5d-106">Az ebben a példában használt HiveQL információkért lásd: [használata a hdinsight Hadoop Hive](hdinsight-use-hive.md).</span><span class="sxs-lookup"><span data-stu-id="03c5d-106">For information on the HiveQL that is used in this example, see [Use Hive with Hadoop on HDInsight](hdinsight-use-hive.md).</span></span>

<span data-ttu-id="03c5d-107">**Előfeltételek**</span><span class="sxs-lookup"><span data-stu-id="03c5d-107">**Prerequisites**</span></span>

* <span data-ttu-id="03c5d-108">**Egy Azure HDInsight fürt**: nem számít, hogy a fürt Windows vagy Linux-alapú.</span><span class="sxs-lookup"><span data-stu-id="03c5d-108">**An Azure HDInsight cluster**: It does not matter whether the cluster is Windows or Linux-based.</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="03c5d-109">A Linux az egyetlen operációs rendszer, amely a HDInsight 3.4-es vagy újabb verziói esetében használható.</span><span class="sxs-lookup"><span data-stu-id="03c5d-109">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="03c5d-110">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="03c5d-110">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="03c5d-111">**Munkaállomás Azure PowerShell-lel**.</span><span class="sxs-lookup"><span data-stu-id="03c5d-111">**A workstation with Azure PowerShell**.</span></span>

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

## <a name="run-hive-queries-using-azure-powershell"></a><span data-ttu-id="03c5d-112">Az Azure PowerShell Hive-lekérdezések futtatása</span><span class="sxs-lookup"><span data-stu-id="03c5d-112">Run Hive queries using Azure PowerShell</span></span>

<span data-ttu-id="03c5d-113">Az Azure PowerShell biztosít *parancsmagok* , amelyek lehetővé teszik, hogy távolról ugyanúgy futtathatják a HDInsight Hive-lekérdezéseket.</span><span class="sxs-lookup"><span data-stu-id="03c5d-113">Azure PowerShell provides *cmdlets* that allow you to remotely run Hive queries on HDInsight.</span></span> <span data-ttu-id="03c5d-114">Belső, a parancsmagok hívások REST való [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) a HDInsight-fürtre.</span><span class="sxs-lookup"><span data-stu-id="03c5d-114">Internally, the cmdlets make REST calls to [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) on the HDInsight cluster.</span></span>

<span data-ttu-id="03c5d-115">A következő parancsmagok használhatók egy távoli HDInsight-fürtöt a Hive-lekérdezések futtatásakor:</span><span class="sxs-lookup"><span data-stu-id="03c5d-115">The following cmdlets are used when running Hive queries in a remote HDInsight cluster:</span></span>

* <span data-ttu-id="03c5d-116">**Adja hozzá-AzureRmAccount**: Azure PowerShell hitelesíti az Azure-előfizetéshez</span><span class="sxs-lookup"><span data-stu-id="03c5d-116">**Add-AzureRmAccount**: Authenticates Azure PowerShell to your Azure subscription</span></span>
* <span data-ttu-id="03c5d-117">**Új AzureRmHDInsightHiveJobDefinition**: létrehoz egy *definition feladat* a megadott HiveQL utasítások használatával</span><span class="sxs-lookup"><span data-stu-id="03c5d-117">**New-AzureRmHDInsightHiveJobDefinition**: Creates a *job definition* by using the specified HiveQL statements</span></span>
* <span data-ttu-id="03c5d-118">**Start-AzureRmHDInsightJob**: a feladat definíciójához küld HDInsight, elindítja a feladatot, és adja vissza egy *feladat* objektum, amely segítségével a feladat állapotának ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="03c5d-118">**Start-AzureRmHDInsightJob**: Sends the job definition to HDInsight, starts the job, and returns a *job* object that can be used to check the status of the job</span></span>
* <span data-ttu-id="03c5d-119">**Várjon, amíg-AzureRmHDInsightJob**: a feladat állapotának ellenőrzése a feladatobjektum használja.</span><span class="sxs-lookup"><span data-stu-id="03c5d-119">**Wait-AzureRmHDInsightJob**: Uses the job object to check the status of the job.</span></span> <span data-ttu-id="03c5d-120">Arra vár, amíg a feladat befejeződik, vagy a várakozási ideje lejár.</span><span class="sxs-lookup"><span data-stu-id="03c5d-120">It waits until the job completes or the wait time is exceeded.</span></span>
* <span data-ttu-id="03c5d-121">**Get-AzureRmHDInsightJobOutput**: a feladat kimenetének beolvasása</span><span class="sxs-lookup"><span data-stu-id="03c5d-121">**Get-AzureRmHDInsightJobOutput**: Used to retrieve the output of the job</span></span>
* <span data-ttu-id="03c5d-122">**Invoke-AzureRmHDInsightHiveJob**: HiveQL utasítás futtatásához használt.</span><span class="sxs-lookup"><span data-stu-id="03c5d-122">**Invoke-AzureRmHDInsightHiveJob**: Used to run HiveQL statements.</span></span> <span data-ttu-id="03c5d-123">Ez a parancsmag blokkolja a lekérdezés befejeződött, majd az eredményeket ad vissza</span><span class="sxs-lookup"><span data-stu-id="03c5d-123">This cmdlet blocks the query completes, then returns the results</span></span>
* <span data-ttu-id="03c5d-124">**Használjon-AzureRmHDInsightCluster**: a jelenlegi fürthöz való használatra beállítja a **Invoke-AzureRmHDInsightHiveJob** parancs</span><span class="sxs-lookup"><span data-stu-id="03c5d-124">**Use-AzureRmHDInsightCluster**: Sets the current cluster to use for the **Invoke-AzureRmHDInsightHiveJob** command</span></span>

<span data-ttu-id="03c5d-125">A következő lépések bemutatják, hogyan lehet ezeket a parancsmagokat használja a feladat futtatásához a HDInsight fürt:</span><span class="sxs-lookup"><span data-stu-id="03c5d-125">The following steps demonstrate how to use these cmdlets to run a job in your HDInsight cluster:</span></span>

1. <span data-ttu-id="03c5d-126">Egy szerkesztővel, az alábbi kód, Mentés **hivejob.ps1**.</span><span class="sxs-lookup"><span data-stu-id="03c5d-126">Using an editor, save the following code as **hivejob.ps1**.</span></span>

    <span data-ttu-id="03c5d-127">[!code-powershell[fő](../../powershell_scripts/hdinsight/use-hive/use-hive.ps1?range=5-42)]</span><span class="sxs-lookup"><span data-stu-id="03c5d-127">[!code-powershell[main](../../powershell_scripts/hdinsight/use-hive/use-hive.ps1?range=5-42)]</span></span>

2. <span data-ttu-id="03c5d-128">Nyisson meg egy új **Azure PowerShell** parancssort.</span><span class="sxs-lookup"><span data-stu-id="03c5d-128">Open a new **Azure PowerShell** command prompt.</span></span> <span data-ttu-id="03c5d-129">Módosítsa a könyvtárat, hol található a **hivejob.ps1** fájlt, majd futtassa a parancsfájlt a következő paranccsal:</span><span class="sxs-lookup"><span data-stu-id="03c5d-129">Change directories to the location of the **hivejob.ps1** file, then use the following command to run the script:</span></span>

        .\hivejob.ps1

    <span data-ttu-id="03c5d-130">A parancsprogram futtatásakor kéri a fürt a fürt neve és a HTTPS/rendszergazdai fiók hitelesítő adatait adja meg.</span><span class="sxs-lookup"><span data-stu-id="03c5d-130">When the script runs, you are prompted to enter the cluster name and the HTTPS/Admin account credentials for the cluster.</span></span> <span data-ttu-id="03c5d-131">Előfordulhat, hogy is kérni fogja-e jelentkezni az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="03c5d-131">You may also be prompted to log in to your Azure subscription.</span></span>

3. <span data-ttu-id="03c5d-132">A feladat befejeződik, ha olyan információkat ad vissza a következő thext hasonlít:</span><span class="sxs-lookup"><span data-stu-id="03c5d-132">When the job completes, it returns information similar to the following thext:</span></span>

        Display the standard output...
        2012-02-03      18:35:34        SampleClass0    [ERROR] incorrect       id
        2012-02-03      18:55:54        SampleClass1    [ERROR] incorrect       id
        2012-02-03      19:25:27        SampleClass4    [ERROR] incorrect       id

4. <span data-ttu-id="03c5d-133">A korábbiak **Invoke-struktúra** segítségével futtassa a lekérdezést, és a válaszra.</span><span class="sxs-lookup"><span data-stu-id="03c5d-133">As mentioned earlier, **Invoke-Hive** can be used to run a query and wait for the response.</span></span> <span data-ttu-id="03c5d-134">A következő parancsfájl segítségével tekintse meg az Invoke-struktúra működése:</span><span class="sxs-lookup"><span data-stu-id="03c5d-134">Use the following script to see how Invoke-Hive works:</span></span>

    <span data-ttu-id="03c5d-135">[!code-powershell[fő](../../powershell_scripts/hdinsight/use-hive/use-hive.ps1?range=50-71)]</span><span class="sxs-lookup"><span data-stu-id="03c5d-135">[!code-powershell[main](../../powershell_scripts/hdinsight/use-hive/use-hive.ps1?range=50-71)]</span></span>

    <span data-ttu-id="03c5d-136">A kimeneti néz ki a következő szöveget:</span><span class="sxs-lookup"><span data-stu-id="03c5d-136">The output looks like the following text:</span></span>

        2012-02-03    18:35:34    SampleClass0    [ERROR]    incorrect    id
        2012-02-03    18:55:54    SampleClass1    [ERROR]    incorrect    id
        2012-02-03    19:25:27    SampleClass4    [ERROR]    incorrect    id

   > [!NOTE]
   > <span data-ttu-id="03c5d-137">Hosszabb HiveQL lekérdezések esetén használhatja az Azure PowerShell **ide-karakterláncok** parancsmag vagy a HiveQL parancsfájlok.</span><span class="sxs-lookup"><span data-stu-id="03c5d-137">For longer HiveQL queries, you can use the Azure PowerShell **Here-Strings** cmdlet or HiveQL script files.</span></span> <span data-ttu-id="03c5d-138">Az alábbi kódrészletben láthatja, hogyan használható a **Invoke-struktúra** parancsmag HiveQL parancsfájl futtatásához.</span><span class="sxs-lookup"><span data-stu-id="03c5d-138">The following snippet shows how to use the **Invoke-Hive** cmdlet to run a HiveQL script file.</span></span> <span data-ttu-id="03c5d-139">Fel kell tölteni a HiveQL-parancsfájlt, a wasb: / /.</span><span class="sxs-lookup"><span data-stu-id="03c5d-139">The HiveQL script file must be uploaded to wasb://.</span></span>
   >
   > `Invoke-AzureRmHDInsightHiveJob -File "wasb://<ContainerName>@<StorageAccountName>/<Path>/query.hql"`
   >
   > <span data-ttu-id="03c5d-140">További információ **ide-karakterláncok**, lásd: <a href="http://technet.microsoft.com/library/ee692792.aspx" target="_blank">használata a Windows PowerShell ide-karakterláncok</a>.</span><span class="sxs-lookup"><span data-stu-id="03c5d-140">For more information about **Here-Strings**, see <a href="http://technet.microsoft.com/library/ee692792.aspx" target="_blank">Using Windows PowerShell Here-Strings</a>.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="03c5d-141">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="03c5d-141">Troubleshooting</span></span>

<span data-ttu-id="03c5d-142">Ha nem áll rendelkezésre információ ad vissza, ha a feladat befejeződik, egy meghibásodott feldolgozása során.</span><span class="sxs-lookup"><span data-stu-id="03c5d-142">If no information is returned when the job completes, an error may have occurred during processing.</span></span> <span data-ttu-id="03c5d-143">Hiba történt a feladat információinak megtekintése, adja hozzá a következő végének a **hivejob.ps1** fájl, mentse, majd futtassa újból.</span><span class="sxs-lookup"><span data-stu-id="03c5d-143">To view error information for this job, add the following to the end of the **hivejob.ps1** file, save it, and then run it again.</span></span>

```powershell
# Print the output of the Hive job.
Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $job.JobId `
        -HttpCredential $creds `
        -DisplayOutputType StandardError
```

<span data-ttu-id="03c5d-144">Ez a parancsmag írt STDERR a kiszolgálón a feladat futtatásakor olyan információkat ad vissza.</span><span class="sxs-lookup"><span data-stu-id="03c5d-144">This cmdlet returns the information that is written to STDERR on the server when you ran the job.</span></span>

## <a name="summary"></a><span data-ttu-id="03c5d-145">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="03c5d-145">Summary</span></span>

<span data-ttu-id="03c5d-146">Ahogy látja, Azure PowerShell könnyedén futtathat Hive-lekérdezéseket a HDInsight-fürtöt, figyelheti a feladat állapotát és a kimeneti beolvasása.</span><span class="sxs-lookup"><span data-stu-id="03c5d-146">As you can see, Azure PowerShell provides an easy way to run Hive queries in an HDInsight cluster, monitor the job status, and retrieve the output.</span></span>

## <a name="next-steps"></a><span data-ttu-id="03c5d-147">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="03c5d-147">Next steps</span></span>

<span data-ttu-id="03c5d-148">Általános információk a hdinsight Hive:</span><span class="sxs-lookup"><span data-stu-id="03c5d-148">For general information about Hive in HDInsight:</span></span>

* [<span data-ttu-id="03c5d-149">A Hive használata a hdinsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="03c5d-149">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)

<span data-ttu-id="03c5d-150">Más módszerekkel kapcsolatos információk a HDInsight Hadoop dolgozhat:</span><span class="sxs-lookup"><span data-stu-id="03c5d-150">For information about other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="03c5d-151">A Pig használata a HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="03c5d-151">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="03c5d-152">A HDInsight Hadoop MapReduce használata</span><span class="sxs-lookup"><span data-stu-id="03c5d-152">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)
