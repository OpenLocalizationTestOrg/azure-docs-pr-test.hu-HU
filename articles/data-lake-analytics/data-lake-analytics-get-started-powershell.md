---
title: "az Azure Data Lake Analytics az Azure PowerShell lépései aaaGet |} Microsoft Docs"
description: "Azure PowerShell toocreate egy Data Lake Analytics-fiókot használja, hozzon létre egy U-SQL használatával a Data Lake Analytics-feladatot, valamint hello feladat elküldéséhez. "
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.assetid: 8a4e901e-9656-4a60-90d0-d78ff2f00656
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/04/2017
ms.author: edmaca
ms.openlocfilehash: cb9b35352d1cc9a78337448b1d6835875a212e08
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-data-lake-analytics-using-azure-powershell"></a><span data-ttu-id="134de-103">Az Azure Data Lake Analytics használatának első lépései az Azure PowerShell-lel</span><span class="sxs-lookup"><span data-stu-id="134de-103">Get started with Azure Data Lake Analytics using Azure PowerShell</span></span>
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

<span data-ttu-id="134de-104">Ismerje meg, hogyan toouse Azure PowerShell toocreate Azure Data Lake Analytics fiókok majd küldje el és U-SQL feladatok futtatásához.</span><span class="sxs-lookup"><span data-stu-id="134de-104">Learn how toouse Azure PowerShell toocreate Azure Data Lake Analytics accounts and then submit and run U-SQL jobs.</span></span> <span data-ttu-id="134de-105">További információk a Data Lake Analyticsről: [Azure Data Lake Analytics overview](data-lake-analytics-overview.md) (Az Azure Data Lake Analytics áttekintése).</span><span class="sxs-lookup"><span data-stu-id="134de-105">For more information about Data Lake Analytics, see [Azure Data Lake Analytics overview](data-lake-analytics-overview.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="134de-106">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="134de-106">Prerequisites</span></span>

<span data-ttu-id="134de-107">Ez az oktatóanyag elkezdéséhez hello a következő információkat kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="134de-107">Before you begin this tutorial, you must have hello following information:</span></span>

* <span data-ttu-id="134de-108">**Egy Azure Data Lake Analytics-fiók**.</span><span class="sxs-lookup"><span data-stu-id="134de-108">**An Azure Data Lake Analytics account**.</span></span> <span data-ttu-id="134de-109">Lásd: [Ismerkedés a Data Lake Analytics szolgáltatással](https://docs.microsoft.com/en-us/azure/data-lake-analytics/data-lake-analytics-get-started-portal).</span><span class="sxs-lookup"><span data-stu-id="134de-109">See [Get started with Data Lake Analytics](https://docs.microsoft.com/en-us/azure/data-lake-analytics/data-lake-analytics-get-started-portal).</span></span>
* <span data-ttu-id="134de-110">**Munkaállomás Azure PowerShell-lel**.</span><span class="sxs-lookup"><span data-stu-id="134de-110">**A workstation with Azure PowerShell**.</span></span> <span data-ttu-id="134de-111">Lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="134de-111">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

## <a name="log-in-tooazure"></a><span data-ttu-id="134de-112">Jelentkezzen be tooAzure</span><span class="sxs-lookup"><span data-stu-id="134de-112">Log in tooAzure</span></span>

<span data-ttu-id="134de-113">Ez az oktatóanyag feltételezi az Azure PowerShell használatának előzetes ismeretét.</span><span class="sxs-lookup"><span data-stu-id="134de-113">This tutorial assumes you are already familiar with using Azure PowerShell.</span></span> <span data-ttu-id="134de-114">Különösen tooknow szüksége, hogy a tooAzure toolog.</span><span class="sxs-lookup"><span data-stu-id="134de-114">In particular, you need tooknow how toolog in tooAzure.</span></span> <span data-ttu-id="134de-115">Lásd: hello [Ismerkedés az Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/get-started-azureps) Ha segítségre van szüksége.</span><span class="sxs-lookup"><span data-stu-id="134de-115">See hello [Get started with Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/get-started-azureps) if you need help.</span></span>

<span data-ttu-id="134de-116">toolog be egy előfizetés nevét:</span><span class="sxs-lookup"><span data-stu-id="134de-116">toolog in with a subscription name:</span></span>

```
Login-AzureRmAccount -SubscriptionName "ContosoSubscription"
```

<span data-ttu-id="134de-117">Hello előfizetés neve helyett egy előfizetési azonosító toolog a is használhatja:</span><span class="sxs-lookup"><span data-stu-id="134de-117">Instead of hello subscription name, you can also use a subscription id toolog in:</span></span>

```
Login-AzureRmAccount -SubscriptionId "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
```

<span data-ttu-id="134de-118">Ha sikeres, a parancs kimenetének hello néz hello a következő szöveget:</span><span class="sxs-lookup"><span data-stu-id="134de-118">If  successful, hello output of this command looks like hello following text:</span></span>

```
Environment           : AzureCloud
Account               : joe@contoso.com
TenantId              : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
SubscriptionId        : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
SubscriptionName      : ContosoSubscription
CurrentStorageAccount :
```

## <a name="preparing-for-hello-tutorial"></a><span data-ttu-id="134de-119">Hello oktatóanyag előkészítése</span><span class="sxs-lookup"><span data-stu-id="134de-119">Preparing for hello tutorial</span></span>

<span data-ttu-id="134de-120">Ebben az oktatóanyagban hello PowerShell kódtöredékek ezeket az információkat használja a változók toostore:</span><span class="sxs-lookup"><span data-stu-id="134de-120">hello PowerShell snippets in this tutorial use these variables toostore this information:</span></span>

```
$rg = "<ResourceGroupName>"
$adls = "<DataLakeStoreAccountName>"
$adla = "<DataLakeAnalyticsAccountName>"
$location = "East US 2"
```

## <a name="get-information-about-a-data-lake-analytics-account"></a><span data-ttu-id="134de-121">Data Lake Analytics-fiókkal kapcsolatos információk beszerzése</span><span class="sxs-lookup"><span data-stu-id="134de-121">Get information about a Data Lake Analytics account</span></span>

```
Get-AdlAnalyticsAccount -ResourceGroupName $rg -Name $adla  
```

## <a name="submit-a-u-sql-job"></a><span data-ttu-id="134de-122">U-SQL-feladat elküldése</span><span class="sxs-lookup"><span data-stu-id="134de-122">Submit a U-SQL job</span></span>

<span data-ttu-id="134de-123">Hozzon létre egy PowerShell-változó toohold hello U-SQL parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="134de-123">Create a PowerShell variable toohold hello U-SQL script.</span></span>

```
$script = @"
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS 
              D( customer, amount );
OUTPUT @a
    too"/data.csv"
    USING Outputters.Csv();

"@
```

<span data-ttu-id="134de-124">Hello parancsfájl nyújt.</span><span class="sxs-lookup"><span data-stu-id="134de-124">Submit hello script.</span></span>

```
$job = Submit-AdlJob -AccountName $adla –Script $script
```

<span data-ttu-id="134de-125">Azt is megteheti sikerült hello parancsfájl menteni a fájlt, és küldje el a hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="134de-125">Alternatively, you could save hello script as a file and submit with hello following command:</span></span>

```
$filename = "d:\test.usql"
$script | out-File $filename
$job = Submit-AdlJob -AccountName $adla –ScriptPath $filename
```


<span data-ttu-id="134de-126">Egy adott feladat hello állapotának beolvasása.</span><span class="sxs-lookup"><span data-stu-id="134de-126">Get hello status of a specific job.</span></span> <span data-ttu-id="134de-127">Ez a parancsmag továbbra is használatára, amíg megjelenik a hello történik.</span><span class="sxs-lookup"><span data-stu-id="134de-127">Keep using this cmdlet until you see hello job is done.</span></span>

```
$job = Get-AdlJob -AccountName $adla -JobId $job.JobId
```

<span data-ttu-id="134de-128">Helyett Get-AdlAnalyticsJob és újra amíg a feladat befejeződik, hello várakozási-AdlJob parancsmagot használhatja.</span><span class="sxs-lookup"><span data-stu-id="134de-128">Instead of calling Get-AdlAnalyticsJob over and over until a job finishes, you can use hello Wait-AdlJob cmdlet.</span></span>

```
Wait-AdlJob -Account $adla -JobId $job.JobId
```

<span data-ttu-id="134de-129">Hello kimeneti fájl letöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="134de-129">Download hello output file.</span></span>

```
Export-AdlStoreItem -AccountName $adls -Path "/data.csv" -Destination "C:\data.csv"
```

## <a name="see-also"></a><span data-ttu-id="134de-130">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="134de-130">See also</span></span>
* <span data-ttu-id="134de-131">toosee hello ugyanaz az oktatóanyagot más eszközök használatával hello szeretné a hello hello lap tetején kattintson.</span><span class="sxs-lookup"><span data-stu-id="134de-131">toosee hello same tutorial using other tools, click hello tab selectors on hello top of hello page.</span></span>
* <span data-ttu-id="134de-132">toolearn U-SQL, lásd: [Ismerkedés az Azure Data Lake Analytics U-SQL nyelv](data-lake-analytics-u-sql-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="134de-132">toolearn U-SQL, see [Get started with Azure Data Lake Analytics U-SQL language](data-lake-analytics-u-sql-get-started.md).</span></span>
* <span data-ttu-id="134de-133">Felügyeleti feladatok: [Manage Azure Data Lake Analytics using Azure Portal](data-lake-analytics-manage-use-portal.md) (Az Azure Data Lake Analytics kezelése az Azure Portallal).</span><span class="sxs-lookup"><span data-stu-id="134de-133">For management tasks, see [Manage Azure Data Lake Analytics using Azure portal](data-lake-analytics-manage-use-portal.md).</span></span>
