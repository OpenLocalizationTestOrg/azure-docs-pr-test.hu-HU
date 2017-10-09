---
title: "aaaUse Hadoop Hive hdinsight - Azure PowerShell használatával |} Microsoft Docs"
description: "A hdinsight Hadoop PowerShell toorun Hive-lekérdezéseket használhat."
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
ms.openlocfilehash: 9e0b72a25c5b12431f837b1a34a63ecc06223528
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="run-hive-queries-using-powershell"></a><span data-ttu-id="d7cfd-103">PowerShell-lel Hive-lekérdezések futtatása</span><span class="sxs-lookup"><span data-stu-id="d7cfd-103">Run Hive queries using PowerShell</span></span>
[!INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

<span data-ttu-id="d7cfd-104">Ez a dokumentum egy példát az hello Azure erőforráscsoport mód toorun Hive-lekérdezéseket a Hadoop on HDInsight-fürt az Azure PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="d7cfd-104">This document provides an example of using Azure PowerShell in hello Azure Resource Group mode toorun Hive queries in a Hadoop on HDInsight cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="d7cfd-105">Ez a dokumentum nem biztosít részletes leírása a hello HiveQL utasítások hello példákban használt tegye.</span><span class="sxs-lookup"><span data-stu-id="d7cfd-105">This document does not provide a detailed description of what hello HiveQL statements that are used in hello examples do.</span></span> <span data-ttu-id="d7cfd-106">Ebben a példában használt HiveQL hello információkért lásd: [használata a hdinsight Hadoop Hive](hdinsight-use-hive.md).</span><span class="sxs-lookup"><span data-stu-id="d7cfd-106">For information on hello HiveQL that is used in this example, see [Use Hive with Hadoop on HDInsight](hdinsight-use-hive.md).</span></span>

<span data-ttu-id="d7cfd-107">**Előfeltételek**</span><span class="sxs-lookup"><span data-stu-id="d7cfd-107">**Prerequisites**</span></span>

* <span data-ttu-id="d7cfd-108">**Egy Azure HDInsight fürt**: nem számít, hogy az hello fürt Windows vagy Linux-alapú.</span><span class="sxs-lookup"><span data-stu-id="d7cfd-108">**An Azure HDInsight cluster**: It does not matter whether hello cluster is Windows or Linux-based.</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="d7cfd-109">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="d7cfd-109">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="d7cfd-110">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="d7cfd-110">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="d7cfd-111">**Munkaállomás Azure PowerShell-lel**.</span><span class="sxs-lookup"><span data-stu-id="d7cfd-111">**A workstation with Azure PowerShell**.</span></span>

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

## <a name="run-hive-queries-using-azure-powershell"></a><span data-ttu-id="d7cfd-112">Az Azure PowerShell Hive-lekérdezések futtatása</span><span class="sxs-lookup"><span data-stu-id="d7cfd-112">Run Hive queries using Azure PowerShell</span></span>

<span data-ttu-id="d7cfd-113">Az Azure PowerShell biztosít *parancsmagok* , amelyek lehetővé teszik Hive-lekérdezések futtatása tooremotely a hdinsight platformon.</span><span class="sxs-lookup"><span data-stu-id="d7cfd-113">Azure PowerShell provides *cmdlets* that allow you tooremotely run Hive queries on HDInsight.</span></span> <span data-ttu-id="d7cfd-114">Belsőleg, hello parancsmagok hívások REST túl[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) hello HDInsight-fürtre.</span><span class="sxs-lookup"><span data-stu-id="d7cfd-114">Internally, hello cmdlets make REST calls too[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) on hello HDInsight cluster.</span></span>

<span data-ttu-id="d7cfd-115">hello alábbi parancsmagok használata, amikor egy távoli HDInsight-fürt Hive-lekérdezések futtatása:</span><span class="sxs-lookup"><span data-stu-id="d7cfd-115">hello following cmdlets are used when running Hive queries in a remote HDInsight cluster:</span></span>

* <span data-ttu-id="d7cfd-116">**Adja hozzá-AzureRmAccount**: hitelesíti az Azure PowerShell tooyour Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="d7cfd-116">**Add-AzureRmAccount**: Authenticates Azure PowerShell tooyour Azure subscription</span></span>
* <span data-ttu-id="d7cfd-117">**Új AzureRmHDInsightHiveJobDefinition**: létrehoz egy *definition feladat* hello segítségével megadott HiveQL utasítások</span><span class="sxs-lookup"><span data-stu-id="d7cfd-117">**New-AzureRmHDInsightHiveJobDefinition**: Creates a *job definition* by using hello specified HiveQL statements</span></span>
* <span data-ttu-id="d7cfd-118">**Start-AzureRmHDInsightJob**: hello feladat definition tooHDInsight küld, hello feladat elindul, és adja vissza egy *feladat* objektum, amely lehet használt toocheck hello hello feladat állapota</span><span class="sxs-lookup"><span data-stu-id="d7cfd-118">**Start-AzureRmHDInsightJob**: Sends hello job definition tooHDInsight, starts hello job, and returns a *job* object that can be used toocheck hello status of hello job</span></span>
* <span data-ttu-id="d7cfd-119">**Várjon, amíg-AzureRmHDInsightJob**: hello objektum toocheck hello feladatállapot hello feladat használja.</span><span class="sxs-lookup"><span data-stu-id="d7cfd-119">**Wait-AzureRmHDInsightJob**: Uses hello job object toocheck hello status of hello job.</span></span> <span data-ttu-id="d7cfd-120">Arra vár, amíg hello feladat befejeződik, vagy hello várakozási ideje lejár.</span><span class="sxs-lookup"><span data-stu-id="d7cfd-120">It waits until hello job completes or hello wait time is exceeded.</span></span>
* <span data-ttu-id="d7cfd-121">**Get-AzureRmHDInsightJobOutput**: hello feladat eredményének tooretrieve hello használt</span><span class="sxs-lookup"><span data-stu-id="d7cfd-121">**Get-AzureRmHDInsightJobOutput**: Used tooretrieve hello output of hello job</span></span>
* <span data-ttu-id="d7cfd-122">**Invoke-AzureRmHDInsightHiveJob**: toorun HiveQL utasítás használható.</span><span class="sxs-lookup"><span data-stu-id="d7cfd-122">**Invoke-AzureRmHDInsightHiveJob**: Used toorun HiveQL statements.</span></span> <span data-ttu-id="d7cfd-123">Ez a parancsmag blokkok hello lekérdezés befejeződött, majd hello eredményt ad vissza.</span><span class="sxs-lookup"><span data-stu-id="d7cfd-123">This cmdlet blocks hello query completes, then returns hello results</span></span>
* <span data-ttu-id="d7cfd-124">**Használjon-AzureRmHDInsightCluster**: készletek hello hello az aktuális fürt toouse **Invoke-AzureRmHDInsightHiveJob** parancs</span><span class="sxs-lookup"><span data-stu-id="d7cfd-124">**Use-AzureRmHDInsightCluster**: Sets hello current cluster toouse for hello **Invoke-AzureRmHDInsightHiveJob** command</span></span>

<span data-ttu-id="d7cfd-125">hello következő lépések bemutatják, hogyan toouse ezen parancsmagok toorun egy feladat a HDInsight-fürt:</span><span class="sxs-lookup"><span data-stu-id="d7cfd-125">hello following steps demonstrate how toouse these cmdlets toorun a job in your HDInsight cluster:</span></span>

1. <span data-ttu-id="d7cfd-126">Egy szerkesztővel, mentse a következő kódot hello **hivejob.ps1**.</span><span class="sxs-lookup"><span data-stu-id="d7cfd-126">Using an editor, save hello following code as **hivejob.ps1**.</span></span>

    [!code-powershell[main](../../powershell_scripts/hdinsight/use-hive/use-hive.ps1?range=5-42)]

2. <span data-ttu-id="d7cfd-127">Nyisson meg egy új **Azure PowerShell** parancssort.</span><span class="sxs-lookup"><span data-stu-id="d7cfd-127">Open a new **Azure PowerShell** command prompt.</span></span> <span data-ttu-id="d7cfd-128">Hello könyvtárak toohello módosítani **hivejob.ps1** fájlt, majd a következő parancsfájl toorun hello hello használata:</span><span class="sxs-lookup"><span data-stu-id="d7cfd-128">Change directories toohello location of hello **hivejob.ps1** file, then use hello following command toorun hello script:</span></span>

        .\hivejob.ps1

    <span data-ttu-id="d7cfd-129">Hello parancsprogram futtatásakor felszólító tooenter hello fürt nevét és hello HTTPS/rendszergazdai fiók hitelesítő adatait hello fürt áll.</span><span class="sxs-lookup"><span data-stu-id="d7cfd-129">When hello script runs, you are prompted tooenter hello cluster name and hello HTTPS/Admin account credentials for hello cluster.</span></span> <span data-ttu-id="d7cfd-130">Az Azure-előfizetés tooyour rákérdezéses toolog is lehet.</span><span class="sxs-lookup"><span data-stu-id="d7cfd-130">You may also be prompted toolog in tooyour Azure subscription.</span></span>

3. <span data-ttu-id="d7cfd-131">Hello feladat befejezése után információkat a következő thext hasonló toohello adja vissza:</span><span class="sxs-lookup"><span data-stu-id="d7cfd-131">When hello job completes, it returns information similar toohello following thext:</span></span>

        Display hello standard output...
        2012-02-03      18:35:34        SampleClass0    [ERROR] incorrect       id
        2012-02-03      18:55:54        SampleClass1    [ERROR] incorrect       id
        2012-02-03      19:25:27        SampleClass4    [ERROR] incorrect       id

4. <span data-ttu-id="d7cfd-132">A korábbiak **Invoke-struktúra** is használt toorun lekérdezés lehet és hello válaszra.</span><span class="sxs-lookup"><span data-stu-id="d7cfd-132">As mentioned earlier, **Invoke-Hive** can be used toorun a query and wait for hello response.</span></span> <span data-ttu-id="d7cfd-133">A következő parancsfájl toosee Invoke-struktúra működése hello használata:</span><span class="sxs-lookup"><span data-stu-id="d7cfd-133">Use hello following script toosee how Invoke-Hive works:</span></span>

    [!code-powershell[main](../../powershell_scripts/hdinsight/use-hive/use-hive.ps1?range=50-71)]

    <span data-ttu-id="d7cfd-134">hello kimenete a következő szöveg hello néz ki:</span><span class="sxs-lookup"><span data-stu-id="d7cfd-134">hello output looks like hello following text:</span></span>

        2012-02-03    18:35:34    SampleClass0    [ERROR]    incorrect    id
        2012-02-03    18:55:54    SampleClass1    [ERROR]    incorrect    id
        2012-02-03    19:25:27    SampleClass4    [ERROR]    incorrect    id

   > [!NOTE]
   > <span data-ttu-id="d7cfd-135">Hosszabb HiveQL lekérdezések esetén használhatja hello Azure PowerShell **ide-karakterláncok** parancsmag vagy a HiveQL parancsfájlok.</span><span class="sxs-lookup"><span data-stu-id="d7cfd-135">For longer HiveQL queries, you can use hello Azure PowerShell **Here-Strings** cmdlet or HiveQL script files.</span></span> <span data-ttu-id="d7cfd-136">a következő kódrészletet mutat be hogyan hello toouse hello **Invoke-struktúra** parancsmag toorun HiveQL-parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="d7cfd-136">hello following snippet shows how toouse hello **Invoke-Hive** cmdlet toorun a HiveQL script file.</span></span> <span data-ttu-id="d7cfd-137">hello HiveQL-parancsfájlt kell feltöltött toowasb: / /.</span><span class="sxs-lookup"><span data-stu-id="d7cfd-137">hello HiveQL script file must be uploaded toowasb://.</span></span>
   >
   > `Invoke-AzureRmHDInsightHiveJob -File "wasb://<ContainerName>@<StorageAccountName>/<Path>/query.hql"`
   >
   > <span data-ttu-id="d7cfd-138">További információ **ide-karakterláncok**, lásd: <a href="http://technet.microsoft.com/library/ee692792.aspx" target="_blank">használata a Windows PowerShell ide-karakterláncok</a>.</span><span class="sxs-lookup"><span data-stu-id="d7cfd-138">For more information about **Here-Strings**, see <a href="http://technet.microsoft.com/library/ee692792.aspx" target="_blank">Using Windows PowerShell Here-Strings</a>.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="d7cfd-139">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="d7cfd-139">Troubleshooting</span></span>

<span data-ttu-id="d7cfd-140">Ha nem áll rendelkezésre információ ad vissza, ha hello feladat befejeződik, egy meghibásodott feldolgozása során.</span><span class="sxs-lookup"><span data-stu-id="d7cfd-140">If no information is returned when hello job completes, an error may have occurred during processing.</span></span> <span data-ttu-id="d7cfd-141">Ez a feladat információi tooview hiba hozzáadása hello hello toohello végét a következő **hivejob.ps1** fájl, mentse, majd futtassa újból.</span><span class="sxs-lookup"><span data-stu-id="d7cfd-141">tooview error information for this job, add hello following toohello end of hello **hivejob.ps1** file, save it, and then run it again.</span></span>

```powershell
# Print hello output of hello Hive job.
Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $job.JobId `
        -HttpCredential $creds `
        -DisplayOutputType StandardError
```

<span data-ttu-id="d7cfd-142">Ez a parancsmag hello írt információ tooSTDERR hello kiszolgálón hello feladat futtatásakor adja vissza.</span><span class="sxs-lookup"><span data-stu-id="d7cfd-142">This cmdlet returns hello information that is written tooSTDERR on hello server when you ran hello job.</span></span>

## <a name="summary"></a><span data-ttu-id="d7cfd-143">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="d7cfd-143">Summary</span></span>

<span data-ttu-id="d7cfd-144">Ahogy látja, Azure PowerShell biztosít egy egyszerűen toorun Hive-lekérdezéseket a HDInsight-fürtöt, a figyelő hello feladat állapota, hello kimeneti beolvasása.</span><span class="sxs-lookup"><span data-stu-id="d7cfd-144">As you can see, Azure PowerShell provides an easy way toorun Hive queries in an HDInsight cluster, monitor hello job status, and retrieve hello output.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d7cfd-145">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d7cfd-145">Next steps</span></span>

<span data-ttu-id="d7cfd-146">Általános információk a hdinsight Hive:</span><span class="sxs-lookup"><span data-stu-id="d7cfd-146">For general information about Hive in HDInsight:</span></span>

* [<span data-ttu-id="d7cfd-147">A Hive használata a hdinsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="d7cfd-147">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)

<span data-ttu-id="d7cfd-148">Más módszerekkel kapcsolatos információk a HDInsight Hadoop dolgozhat:</span><span class="sxs-lookup"><span data-stu-id="d7cfd-148">For information about other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="d7cfd-149">A Pig használata a HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="d7cfd-149">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="d7cfd-150">A HDInsight Hadoop MapReduce használata</span><span class="sxs-lookup"><span data-stu-id="d7cfd-150">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)
