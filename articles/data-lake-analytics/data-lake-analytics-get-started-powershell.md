---
title: "Az Azure Data Lake Analytics használatának első lépései az Azure PowerShell-lel | Microsoft Docs"
description: "Az Azure PowerShell használata egy Data Lake Analytics-fiók létrehozásához, egy Data Lake Analytics-feladat létrehozására U-SQL használatával, valamint a feladat elküldésére. "
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
ms.openlocfilehash: 4f73e27c733edae658d1ea3bdabe48076328279b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-azure-data-lake-analytics-using-azure-powershell"></a><span data-ttu-id="f8abf-103">Az Azure Data Lake Analytics használatának első lépései az Azure PowerShell-lel</span><span class="sxs-lookup"><span data-stu-id="f8abf-103">Get started with Azure Data Lake Analytics using Azure PowerShell</span></span>
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

<span data-ttu-id="f8abf-104">Ebből a cikkből megtudhatja, hogyan használhatja az Azure PowerShellt Azure Data Lake Analytics-fiókok létrehozására, majd U-SQL-feladatok elküldéséhez és futtatásához.</span><span class="sxs-lookup"><span data-stu-id="f8abf-104">Learn how to use Azure PowerShell to create Azure Data Lake Analytics accounts and then submit and run U-SQL jobs.</span></span> <span data-ttu-id="f8abf-105">További információk a Data Lake Analyticsről: [Azure Data Lake Analytics overview](data-lake-analytics-overview.md) (Az Azure Data Lake Analytics áttekintése).</span><span class="sxs-lookup"><span data-stu-id="f8abf-105">For more information about Data Lake Analytics, see [Azure Data Lake Analytics overview](data-lake-analytics-overview.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f8abf-106">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f8abf-106">Prerequisites</span></span>

<span data-ttu-id="f8abf-107">Az oktatóanyag elkezdéséhez az alábbiakkal kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="f8abf-107">Before you begin this tutorial, you must have the following information:</span></span>

* <span data-ttu-id="f8abf-108">**Egy Azure Data Lake Analytics-fiók**.</span><span class="sxs-lookup"><span data-stu-id="f8abf-108">**An Azure Data Lake Analytics account**.</span></span> <span data-ttu-id="f8abf-109">Lásd: [Ismerkedés a Data Lake Analytics szolgáltatással](https://docs.microsoft.com/en-us/azure/data-lake-analytics/data-lake-analytics-get-started-portal).</span><span class="sxs-lookup"><span data-stu-id="f8abf-109">See [Get started with Data Lake Analytics](https://docs.microsoft.com/en-us/azure/data-lake-analytics/data-lake-analytics-get-started-portal).</span></span>
* <span data-ttu-id="f8abf-110">**Munkaállomás Azure PowerShell-lel**.</span><span class="sxs-lookup"><span data-stu-id="f8abf-110">**A workstation with Azure PowerShell**.</span></span> <span data-ttu-id="f8abf-111">Lásd: [How to install and configure Azure PowerShell](/powershell/azure/overview) (Az Azure PowerShell telepítése és konfigurálása).</span><span class="sxs-lookup"><span data-stu-id="f8abf-111">See [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

## <a name="log-in-to-azure"></a><span data-ttu-id="f8abf-112">Jelentkezzen be az Azure-ba</span><span class="sxs-lookup"><span data-stu-id="f8abf-112">Log in to Azure</span></span>

<span data-ttu-id="f8abf-113">Ez az oktatóanyag feltételezi az Azure PowerShell használatának előzetes ismeretét.</span><span class="sxs-lookup"><span data-stu-id="f8abf-113">This tutorial assumes you are already familiar with using Azure PowerShell.</span></span> <span data-ttu-id="f8abf-114">Az előzetes ismeretek fontos része az Azure-ba történő bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="f8abf-114">In particular, you need to know how to log in to Azure.</span></span> <span data-ttu-id="f8abf-115">Ha segítségre van szüksége, tekintse meg az [Ismerkedés az Azure PowerShell szolgáltatással](https://docs.microsoft.com/en-us/powershell/azure/get-started-azureps) című cikket.</span><span class="sxs-lookup"><span data-stu-id="f8abf-115">See the [Get started with Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/get-started-azureps) if you need help.</span></span>

<span data-ttu-id="f8abf-116">Előfizetés nevével történő bejelentkezéshez:</span><span class="sxs-lookup"><span data-stu-id="f8abf-116">To log in with a subscription name:</span></span>

```
Login-AzureRmAccount -SubscriptionName "ContosoSubscription"
```

<span data-ttu-id="f8abf-117">Az előfizetés neve helyett előfizetés-azonosítót is használhat a bejelentkezéshez:</span><span class="sxs-lookup"><span data-stu-id="f8abf-117">Instead of the subscription name, you can also use a subscription id to log in:</span></span>

```
Login-AzureRmAccount -SubscriptionId "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
```

<span data-ttu-id="f8abf-118">Sikeres művelet esetén a parancs kimenete az alábbi szöveghez hasonlít:</span><span class="sxs-lookup"><span data-stu-id="f8abf-118">If  successful, the output of this command looks like the following text:</span></span>

```
Environment           : AzureCloud
Account               : joe@contoso.com
TenantId              : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
SubscriptionId        : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
SubscriptionName      : ContosoSubscription
CurrentStorageAccount :
```

## <a name="preparing-for-the-tutorial"></a><span data-ttu-id="f8abf-119">Felkészülés az oktatóanyag elvégzésére</span><span class="sxs-lookup"><span data-stu-id="f8abf-119">Preparing for the tutorial</span></span>

<span data-ttu-id="f8abf-120">A jelen oktatóanyagban szereplő PowerShell-kódrészletek ezeket a változókat használják az adattárolásra:</span><span class="sxs-lookup"><span data-stu-id="f8abf-120">The PowerShell snippets in this tutorial use these variables to store this information:</span></span>

```
$rg = "<ResourceGroupName>"
$adls = "<DataLakeStoreAccountName>"
$adla = "<DataLakeAnalyticsAccountName>"
$location = "East US 2"
```

## <a name="get-information-about-a-data-lake-analytics-account"></a><span data-ttu-id="f8abf-121">Data Lake Analytics-fiókkal kapcsolatos információk beszerzése</span><span class="sxs-lookup"><span data-stu-id="f8abf-121">Get information about a Data Lake Analytics account</span></span>

```
Get-AdlAnalyticsAccount -ResourceGroupName $rg -Name $adla  
```

## <a name="submit-a-u-sql-job"></a><span data-ttu-id="f8abf-122">U-SQL-feladat elküldése</span><span class="sxs-lookup"><span data-stu-id="f8abf-122">Submit a U-SQL job</span></span>

<span data-ttu-id="f8abf-123">Hozzon létre egy PowerShell-változót a U-SQL-szkript tárolásához.</span><span class="sxs-lookup"><span data-stu-id="f8abf-123">Create a PowerShell variable to hold the U-SQL script.</span></span>

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
    TO "/data.csv"
    USING Outputters.Csv();

"@
```

<span data-ttu-id="f8abf-124">Küldje el a szkriptet.</span><span class="sxs-lookup"><span data-stu-id="f8abf-124">Submit the script.</span></span>

```
$job = Submit-AdlJob -AccountName $adla –Script $script
```

<span data-ttu-id="f8abf-125">Azt is megteheti, hogy fájlként menti a szkriptet, és a következő paranccsal küldi el:</span><span class="sxs-lookup"><span data-stu-id="f8abf-125">Alternatively, you could save the script as a file and submit with the following command:</span></span>

```
$filename = "d:\test.usql"
$script | out-File $filename
$job = Submit-AdlJob -AccountName $adla –ScriptPath $filename
```


<span data-ttu-id="f8abf-126">Kérje le egy adott feladat állapotát.</span><span class="sxs-lookup"><span data-stu-id="f8abf-126">Get the status of a specific job.</span></span> <span data-ttu-id="f8abf-127">Használja a parancsmagot addig, amíg a feladat be nem fejeződik.</span><span class="sxs-lookup"><span data-stu-id="f8abf-127">Keep using this cmdlet until you see the job is done.</span></span>

```
$job = Get-AdlJob -AccountName $adla -JobId $job.JobId
```

<span data-ttu-id="f8abf-128">Ahelyett, hogy újra és újra meghívná a Get-AdlAnalyticsJob parancsmagot a feladat befejeződéséig, használja a Wait-AdlJob parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="f8abf-128">Instead of calling Get-AdlAnalyticsJob over and over until a job finishes, you can use the Wait-AdlJob cmdlet.</span></span>

```
Wait-AdlJob -Account $adla -JobId $job.JobId
```

<span data-ttu-id="f8abf-129">Töltse le a kimeneti fájlt.</span><span class="sxs-lookup"><span data-stu-id="f8abf-129">Download the output file.</span></span>

```
Export-AdlStoreItem -AccountName $adls -Path "/data.csv" -Destination "C:\data.csv"
```

## <a name="see-also"></a><span data-ttu-id="f8abf-130">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="f8abf-130">See also</span></span>
* <span data-ttu-id="f8abf-131">Ha ugyanezt az oktatóanyagot más eszközök használatával szeretné megtekinteni, kattintson az oldal tetején található lapválasztókra.</span><span class="sxs-lookup"><span data-stu-id="f8abf-131">To see the same tutorial using other tools, click the tab selectors on the top of the page.</span></span>
* <span data-ttu-id="f8abf-132">A U-SQL nyelv megismerése: [Get started with Azure Data Lake Analytics U-SQL language](data-lake-analytics-u-sql-get-started.md) (Ismerkedés az Azure Data Lake Analytics U-SQL nyelvével).</span><span class="sxs-lookup"><span data-stu-id="f8abf-132">To learn U-SQL, see [Get started with Azure Data Lake Analytics U-SQL language](data-lake-analytics-u-sql-get-started.md).</span></span>
* <span data-ttu-id="f8abf-133">Felügyeleti feladatok: [Manage Azure Data Lake Analytics using Azure Portal](data-lake-analytics-manage-use-portal.md) (Az Azure Data Lake Analytics kezelése az Azure Portallal).</span><span class="sxs-lookup"><span data-stu-id="f8abf-133">For management tasks, see [Manage Azure Data Lake Analytics using Azure portal](data-lake-analytics-manage-use-portal.md).</span></span>
