---
title: Azure Data Lake Analytics az Azure PowerShell aaaManage |} Microsoft Docs
description: "Ismerje meg, hogyan toomanage Data Lake Analytics-fiókok, adatforrások, feladatok, és a katalóguselemek. "
services: data-lake-analytics
documentationcenter: 
author: matt1883
manager: jhubbard
editor: cgronlun
ms.assetid: ad14d53c-fed4-478d-ab4b-6d2e14ff2097
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/23/2017
ms.author: mahi
ms.openlocfilehash: 5954f0efb7d5a9778727edfccae83aec046343bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-data-lake-analytics-using-azure-powershell"></a><span data-ttu-id="265c9-103">Az Azure Data Lake Analytics kezelése az Azure PowerShell-lel</span><span class="sxs-lookup"><span data-stu-id="265c9-103">Manage Azure Data Lake Analytics using Azure PowerShell</span></span>
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

<span data-ttu-id="265c9-104">Ismerje meg, hogyan toomanage Azure Data Lake Analytics-fiókok, adatforrások, feladatok, és az Azure PowerShell használatával a szolgáltatáskatalógusban található elemek.</span><span class="sxs-lookup"><span data-stu-id="265c9-104">Learn how toomanage Azure Data Lake Analytics accounts, data sources, jobs, and catalog items using Azure PowerShell.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="265c9-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="265c9-105">Prerequisites</span></span>

<span data-ttu-id="265c9-106">Data Lake Analytics-fiók létrehozásakor tooknow szüksége:</span><span class="sxs-lookup"><span data-stu-id="265c9-106">When creating a Data Lake Analytics account, you need tooknow:</span></span>

* <span data-ttu-id="265c9-107">**Előfizetés-azonosító**: hello Azure előfizetés-azonosító, amely alatt a Data Lake Analytics-fiók található.</span><span class="sxs-lookup"><span data-stu-id="265c9-107">**Subscription ID**: hello Azure subscription ID under which your Data Lake Analytics account resides.</span></span>
* <span data-ttu-id="265c9-108">**Erőforráscsoport**: hello a Data Lake Analytics-fiókot tartalmazó hello Azure erőforráscsoport nevét.</span><span class="sxs-lookup"><span data-stu-id="265c9-108">**Resource group**: hello name of hello Azure resource group that contains your Data Lake Analytics account.</span></span>
* <span data-ttu-id="265c9-109">**A Data Lake Analytics-fiók neve**: hello fiók neve csak kisbetűket és számokat tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="265c9-109">**Data Lake Analytics account name**: hello account name must only contain lowercase letters and numbers.</span></span>
* <span data-ttu-id="265c9-110">**Alapértelmezett Data Lake Store-fiók:** minden Data Lake Analytics-fiókhoz tartozik egy alapértelmezett Data Lake Store-fiók.</span><span class="sxs-lookup"><span data-stu-id="265c9-110">**Default Data Lake Store account**: Each Data Lake Analytics account has a default Data Lake Store account.</span></span> <span data-ttu-id="265c9-111">Ezek a fiókok hello kell lennie ugyanazon a helyen.</span><span class="sxs-lookup"><span data-stu-id="265c9-111">These accounts must be in hello same location.</span></span>
* <span data-ttu-id="265c9-112">**Hely**: hello hely Data Lake Analytics-fiókja, például az "USA keleti régiója 2", vagy más támogatott helyeket.</span><span class="sxs-lookup"><span data-stu-id="265c9-112">**Location**: hello location of your Data Lake Analytics account, such as "East US 2" or other supported locations.</span></span> <span data-ttu-id="265c9-113">Támogatott helyek láthatóak a [árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/data-lake-analytics/).</span><span class="sxs-lookup"><span data-stu-id="265c9-113">Supported locations can be seen on our [pricing page](https://azure.microsoft.com/pricing/details/data-lake-analytics/).</span></span>

<span data-ttu-id="265c9-114">Ebben az oktatóanyagban hello PowerShell kódtöredékek ezeket az információkat használja a változók toostore</span><span class="sxs-lookup"><span data-stu-id="265c9-114">hello PowerShell snippets in this tutorial use these variables toostore this information</span></span>

```powershell
$subId = "<SubscriptionId>"
$rg = "<ResourceGroupName>"
$adla = "<DataLakeAnalyticsAccountName>"
$adls = "<DataLakeStoreAccountName>"
$location = "<Location>"
```

## <a name="log-in"></a><span data-ttu-id="265c9-115">Bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="265c9-115">Log in</span></span>

<span data-ttu-id="265c9-116">Jelentkezzen be egy előfizetési azonosító használatával.</span><span class="sxs-lookup"><span data-stu-id="265c9-116">Log in using a subscription id.</span></span>

```powershell
Login-AzureRmAccount -SubscriptionId $subId
```

<span data-ttu-id="265c9-117">Jelentkezzen be egy előfizetés nevét.</span><span class="sxs-lookup"><span data-stu-id="265c9-117">Log in using a subscription name.</span></span>

```
Login-AzureRmAccount -SubscriptionName $subname 
```

<span data-ttu-id="265c9-118">Hello `Login-AzureRmAccount` parancsmag mindig elkéri a hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="265c9-118">hello `Login-AzureRmAccount` cmdlet  always prompts for credentials.</span></span> <span data-ttu-id="265c9-119">Meg kell adni a következő parancsmagok hello használatával elkerülheti a:</span><span class="sxs-lookup"><span data-stu-id="265c9-119">You can avoid being prompted by using hello following cmdlets:</span></span>

```powershell
# Save login session information
Save-AzureRmProfile -Path D:\profile.json  

# Load login session information
Select-AzureRmProfile -Path D:\profile.json 
```

## <a name="managing-accounts"></a><span data-ttu-id="265c9-120">Fiókok kezelése</span><span class="sxs-lookup"><span data-stu-id="265c9-120">Managing accounts</span></span>

### <a name="create-a-data-lake-analytics-account"></a><span data-ttu-id="265c9-121">Data Lake Analytics-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="265c9-121">Create a Data Lake Analytics account</span></span>

<span data-ttu-id="265c9-122">Ha még nem rendelkezik egy [erőforráscsoport](../azure-resource-manager/resource-group-overview.md#resource-groups) toouse, hozzon létre egyet.</span><span class="sxs-lookup"><span data-stu-id="265c9-122">If you don't already have a [resource group](../azure-resource-manager/resource-group-overview.md#resource-groups) toouse, create one.</span></span> 

```powershell
New-AzureRmResourceGroup -Name  $rg -Location $location
```

<span data-ttu-id="265c9-123">A naplók tárolásához minden Data Lake Analytics-fiók esetében szükség van egy alapértelmezett Data Lake Store-fiókra.</span><span class="sxs-lookup"><span data-stu-id="265c9-123">Every Data Lake Analytics account requires a default Data Lake Store account that it uses for storing logs.</span></span> <span data-ttu-id="265c9-124">Egy meglévő fiókot újra, vagy hozzon létre egy fiókot.</span><span class="sxs-lookup"><span data-stu-id="265c9-124">You can reuse an existing account or create an account.</span></span> 

```powershell
New-AdlStore -ResourceGroupName $rg -Name $adls -Location $location
```

<span data-ttu-id="265c9-125">Ha az erőforráscsoport és a Data Lake Store-fiók már elérhető, hozzon létre egy Data Lake Analytics-fiókot.</span><span class="sxs-lookup"><span data-stu-id="265c9-125">Once a Resource Group and Data Lake Store account is available, create a Data Lake Analytics account.</span></span>

```powershell
New-AdlAnalyticsAccount -ResourceGroupName $rg -Name $adla -Location $location -DefaultDataLake $adls
```

### <a name="get-information-about-an-account"></a><span data-ttu-id="265c9-126">Egy fiók adatainak beolvasása</span><span class="sxs-lookup"><span data-stu-id="265c9-126">Get information about an account</span></span>

<span data-ttu-id="265c9-127">Ezzel a fiókkal kapcsolatos adatokat.</span><span class="sxs-lookup"><span data-stu-id="265c9-127">Get details about an account.</span></span>

```powershell
Get-AdlAnalyticsAccount -Name $adla
```

<span data-ttu-id="265c9-128">Meghatározott Data Lake Analytics-fiók hello meglétének ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="265c9-128">Check hello existence of a specific Data Lake Analytics account.</span></span> <span data-ttu-id="265c9-129">hello parancsmag ad vissza, vagy `True` vagy `False`.</span><span class="sxs-lookup"><span data-stu-id="265c9-129">hello cmdlet returns either `True` or `False`.</span></span>

```powershell
Test-AdlAnalyticsAccount -Name $adla
```

<span data-ttu-id="265c9-130">Egy adott Data Lake Store-fiók hello meglétének ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="265c9-130">Check hello existence of a specific Data Lake Store account.</span></span> <span data-ttu-id="265c9-131">hello parancsmag ad vissza, vagy `True` vagy `False`.</span><span class="sxs-lookup"><span data-stu-id="265c9-131">hello cmdlet returns either `True` or `False`.</span></span>

```powershell
Test-AdlStoreAccount -Name $adls
```

### <a name="listing-accounts"></a><span data-ttu-id="265c9-132">Fiókok listázása</span><span class="sxs-lookup"><span data-stu-id="265c9-132">Listing accounts</span></span>

<span data-ttu-id="265c9-133">Lista Data Lake Analytics-fiókok hello aktuális előfizetésen belül.</span><span class="sxs-lookup"><span data-stu-id="265c9-133">List Data Lake Analytics accounts within hello current subscription.</span></span>

```powershell
Get-AdlAnalyticsAccount
```

<span data-ttu-id="265c9-134">Lista Data Lake Analytics-fiókok egy adott erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="265c9-134">List Data Lake Analytics accounts within a specific resource group.</span></span>

```powershell
Get-AdlAnalyticsAccount -ResourceGroupName $rg
```

## <a name="managing-firewall-rules"></a><span data-ttu-id="265c9-135">Tűzfal-szabályok kezelése</span><span class="sxs-lookup"><span data-stu-id="265c9-135">Managing firewall rules</span></span>

<span data-ttu-id="265c9-136">Tűzfalszabályok listája.</span><span class="sxs-lookup"><span data-stu-id="265c9-136">List firewall rules.</span></span>

```powershell
Get-AdlAnalyticsFirewallRule -Account $adla
```

<span data-ttu-id="265c9-137">Fel egy tűzfalszabályt.</span><span class="sxs-lookup"><span data-stu-id="265c9-137">Add a firewall rule.</span></span>

```powershell
$ruleName = "Allow access from on-prem server"
$startIpAddress = "<start IP address>"
$endIpAddress = "<end IP address>"

Add-AdlAnalyticsFirewallRule -Account $adla -Name $ruleName -StartIpAddress $startIpAddress -EndIpAddress $endIpAddress
```

<span data-ttu-id="265c9-138">Egy tűzfalszabály módosítása.</span><span class="sxs-lookup"><span data-stu-id="265c9-138">Change a firewall rule.</span></span>

```powershell
Set-AdlAnalyticsFirewallRule -Account $adla -Name $ruleName -StartIpAddress $startIpAddress -EndIpAddress $endIpAddress
```

<span data-ttu-id="265c9-139">Egy tűzfalszabály eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="265c9-139">Remove a firewall rule.</span></span>

```powershell
Remove-AdlAnalyticsFirewallRule -Account $adla -Name $ruleName
```



<span data-ttu-id="265c9-140">Azure IP-címek engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="265c9-140">Allow Azure IP addresses.</span></span>

```powershell
Set-AdlAnalyticsAccount -Name $adla -AllowAzureIpState Enabled
```

```powershell
Set-AdlAnalyticsAccount -Name $adla -FirewallState Enabled
Set-AdlAnalyticsAccount -Name $adla -FirewallState Disabled
```

## <a name="managing-data-sources"></a><span data-ttu-id="265c9-141">Az adatforrások kezelése</span><span class="sxs-lookup"><span data-stu-id="265c9-141">Managing data sources</span></span>
<span data-ttu-id="265c9-142">Az Azure Data Lake Analytics jelenleg a következő adatforrások hello támogatja:</span><span class="sxs-lookup"><span data-stu-id="265c9-142">Azure Data Lake Analytics currently supports hello following data sources:</span></span>

* [<span data-ttu-id="265c9-143">Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="265c9-143">Azure Data Lake Store</span></span>](../data-lake-store/data-lake-store-overview.md)
* [<span data-ttu-id="265c9-144">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="265c9-144">Azure Storage</span></span>](../storage/common/storage-introduction.md)

<span data-ttu-id="265c9-145">Az Analytics-fiók létrehozásakor ki kell jelölnie egy Data Lake Store fiók toobe hello alapértelmezett adatforrás.</span><span class="sxs-lookup"><span data-stu-id="265c9-145">When you create an Analytics account, you must designate a Data Lake Store account toobe hello default data source.</span></span> <span data-ttu-id="265c9-146">hello alapértelmezett Data Lake Store-fiók használatos toostore feladat metaadatok és a feladat naplók.</span><span class="sxs-lookup"><span data-stu-id="265c9-146">hello default Data Lake Store account is used toostore job metadata and job audit logs.</span></span> <span data-ttu-id="265c9-147">Data Lake Analytics-fiók létrehozása után hozzáadhat további Data Lake Store-fiókok és/vagy a Storage-fiókok.</span><span class="sxs-lookup"><span data-stu-id="265c9-147">After you have created a Data Lake Analytics account, you can add additional Data Lake Store accounts and/or Storage accounts.</span></span> 

### <a name="find-hello-default-data-lake-store-account"></a><span data-ttu-id="265c9-148">Hello alapértelmezett Data Lake Store-fiók található</span><span class="sxs-lookup"><span data-stu-id="265c9-148">Find hello default Data Lake Store account</span></span>

```powershell
$adla_acct = Get-AdlAnalyticsAccount -Name $adla
$dataLakeStoreName = $adla_acct.DefaultDataLakeAccount
```

<span data-ttu-id="265c9-149">Hello alapértelmezett Data Lake Store-fiók találhatja meg adatforrások listája hello hello szerinti szűrés `IsDefault` tulajdonság:</span><span class="sxs-lookup"><span data-stu-id="265c9-149">You can find hello default Data Lake Store account by filtering hello list of datasources by hello `IsDefault` property:</span></span>

```powershell
Get-AdlAnalyticsDataSource -Account $adla  | ? { $_.IsDefault } 
```

### <a name="add-a-data-source"></a><span data-ttu-id="265c9-150">Egy adatforrás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="265c9-150">Add a data source</span></span>

```powershell

# Add an additional Storage (Blob) account.
$AzureStorageAccountName = "<AzureStorageAccountName>"
$AzureStorageAccountKey = "<AzureStorageAccountKey>"
Add-AdlAnalyticsDataSource -Account $adla -Blob $AzureStorageAccountName -AccessKey $AzureStorageAccountKey

# Add an additional Data Lake Store account.
$AzureDataLakeStoreName = "<AzureDataLakeStoreAccountName"
Add-AdlAnalyticsDataSource -Account $adla -DataLakeStore $AzureDataLakeStoreName 
```

### <a name="list-data-sources"></a><span data-ttu-id="265c9-151">Az adatforrások listája</span><span class="sxs-lookup"><span data-stu-id="265c9-151">List data sources</span></span>

```powershell
# List all hello data sources
Get-AdlAnalyticsDataSource -Name $adla

# List attached Data Lake Store accounts
Get-AdlAnalyticsDataSource -Name $adla | where -Property Type -EQ "DataLakeStore"

# List attached Storage accounts
Get-AdlAnalyticsDataSource -Name $adla | where -Property Type -EQ "Blob"
```

## <a name="submit-u-sql-jobs"></a><span data-ttu-id="265c9-152">U-SQL-feladatok küldése</span><span class="sxs-lookup"><span data-stu-id="265c9-152">Submit U-SQL jobs</span></span>

### <a name="submit-a-string-as-a-u-sql-script"></a><span data-ttu-id="265c9-153">Küldje el egy karakterlánc, egy U-SQL-parancsfájl</span><span class="sxs-lookup"><span data-stu-id="265c9-153">Submit a string as a U-SQL script</span></span>

```powershell
$script = @"
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS D( customer, amount );
OUTPUT @a
    too"/data.csv"
    USING Outputters.Csv();
"@

$scriptpath = "d:\test.usql"
$script | Out-File $scriptpath 

Submit-AdlJob -AccountName $adla -Script $script -Name "Demo"
```


### <a name="submit-a-file-as-a-u-sql-script"></a><span data-ttu-id="265c9-154">Küldje el a fájlt egy U-SQL-parancsfájl</span><span class="sxs-lookup"><span data-stu-id="265c9-154">Submit a file as a U-SQL script</span></span>

```powershell
$scriptpath = "d:\test.usql"
$script | Out-File $scriptpath 
Submit-AdlJob -AccountName $adla –ScriptPath $scriptpath -Name "Demo"
```

## <a name="list-jobs-in-an-account"></a><span data-ttu-id="265c9-155">Egy fiók feladatok listája</span><span class="sxs-lookup"><span data-stu-id="265c9-155">List jobs in an account</span></span>

### <a name="list-all-hello-jobs-in-hello-account"></a><span data-ttu-id="265c9-156">Az összes hello feladat hello fiók felsorolása.</span><span class="sxs-lookup"><span data-stu-id="265c9-156">List all hello jobs in hello account.</span></span> 

<span data-ttu-id="265c9-157">hello kimenete hello jelenleg futó feladatok, és ezeket a feladatokat, amelyek nemrégiben fejeződtek be.</span><span class="sxs-lookup"><span data-stu-id="265c9-157">hello output includes hello currently running jobs and those jobs that have recently completed.</span></span>

```powershell
Get-AdlJob -Account $adla
```


### <a name="list-a-specific-number-of-jobs"></a><span data-ttu-id="265c9-158">Egy bizonyos számú feladatok listázása</span><span class="sxs-lookup"><span data-stu-id="265c9-158">List a specific number of jobs</span></span>

<span data-ttu-id="265c9-159">Alapértelmezés szerint a feladatok hello lista rendezett küldésre idő.</span><span class="sxs-lookup"><span data-stu-id="265c9-159">By default hello list of jobs is sorted on submit time.</span></span> <span data-ttu-id="265c9-160">Így a legutóbb elküldött hello feladatok első jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="265c9-160">So hello most recently submitted jobs appear first.</span></span> <span data-ttu-id="265c9-161">Alapértelmezés szerint hello ADLA fiók feladatok emlékszik 180 napig tart, de csak a hello első 500 hello Ge-AdlJob parancsmag által alapértelmezett értéket ad vissza.</span><span class="sxs-lookup"><span data-stu-id="265c9-161">By default, hello ADLA account remembers jobs for 180 days, but hello Ge-AdlJob  cmdlet by default returns only hello first 500.</span></span> <span data-ttu-id="265c9-162">Használja az - felső paraméter toolist adott számú feladatot.</span><span class="sxs-lookup"><span data-stu-id="265c9-162">Use -Top parameter toolist a specific number of jobs.</span></span>

```powershell
$jobs = Get-AdlJob -Account $adla -Top 10
```


### <a name="list-jobs-based-on-hello-value-of-job-property"></a><span data-ttu-id="265c9-163">Lista feladatok hello feladat tulajdonság értéke alapján</span><span class="sxs-lookup"><span data-stu-id="265c9-163">List jobs based on hello value of job property</span></span>

<span data-ttu-id="265c9-164">Hello segítségével `-State` paraméter.</span><span class="sxs-lookup"><span data-stu-id="265c9-164">Using hello `-State` parameter.</span></span> <span data-ttu-id="265c9-165">Ezek bármelyike kombinálhatja:</span><span class="sxs-lookup"><span data-stu-id="265c9-165">You can combine any of these values:</span></span>

* `Accepted`
* `Compiling`
* `Ended`
* `New`
* `Paused`
* `Queued`
* `Running`
* `Scheduling`
* `Start`

```powershell
# List hello running jobs
Get-AdlJob -Account $adla -State Running

# List hello jobs that have completed
Get-AdlJob -Account $adla -State Ended

# List hello jobs that have not started yet
Get-AdlJob -Account $adla -State Accepted,Compiling,New,Paused,Scheduling,Start
```

<span data-ttu-id="265c9-166">Használjon hello `-Result` paraméter toodetect hogy befejeződött a feladat sikeresen befejeződött-e.</span><span class="sxs-lookup"><span data-stu-id="265c9-166">Use hello `-Result` parameter toodetect whether ended jobs completed successfully.</span></span> <span data-ttu-id="265c9-167">Ezekkel az értékekkel rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="265c9-167">It has these values:</span></span>

* <span data-ttu-id="265c9-168">Megszakítva</span><span class="sxs-lookup"><span data-stu-id="265c9-168">Cancelled</span></span>
* <span data-ttu-id="265c9-169">Nem sikerült</span><span class="sxs-lookup"><span data-stu-id="265c9-169">Failed</span></span>
* <span data-ttu-id="265c9-170">None</span><span class="sxs-lookup"><span data-stu-id="265c9-170">None</span></span>
* <span data-ttu-id="265c9-171">Sikeres</span><span class="sxs-lookup"><span data-stu-id="265c9-171">Succeeded</span></span>

``` powershell
# List Successful jobs.
Get-AdlJob -Account $adla -State Ended -Result Succeeded

# List Failed jobs.
Get-AdlJob -Account $adla -State Ended -Result Failed
```


<span data-ttu-id="265c9-172">Hello `-Submitter` paraméter segít azonosítani, ki egy feladat elküldése megtörtént.</span><span class="sxs-lookup"><span data-stu-id="265c9-172">hello `-Submitter` parameter helps you identify who submitted a job.</span></span>

```powershell
Get-AdlJob -Account $adla -Submitter "joe@contoso.com"
```

<span data-ttu-id="265c9-173">Hello `-SubmittedAfter` hasznos tooa időtartomány szűrést.</span><span class="sxs-lookup"><span data-stu-id="265c9-173">hello `-SubmittedAfter` is useful in filtering tooa time range.</span></span>


```powershell
# List  jobs submitted in hello last day.
$d = [DateTime]::Now.AddDays(-1)
Get-AdlJob -Account $adla -SubmittedAfter $d

# List  jobs submitted in hello last seven day.
$d = [DateTime]::Now.AddDays(-7)
Get-AdlJob -Account $adla -SubmittedAfter $d
```

### <a name="common-scenarios-for-listing-jobs"></a><span data-ttu-id="265c9-174">Általános példák a feladatok listázása</span><span class="sxs-lookup"><span data-stu-id="265c9-174">Common scenarios for listing jobs</span></span>


```
# List jobs submitted in hello last five days and that successfully completed.
$d = (Get-Date).AddDays(-5)
Get-AdlJob -Account $adla -SubmittedAfter $d -State Ended -Result Succeeded

# List all failed jobs submitted by "joe@contoso.com" within hello past seven days.
Get-AdlJob -Account $adla `
    -Submitter "joe@contoso.com" `
    -SubmittedAfter (Get-Date).AddDays(-7) `
    -Result Failed
```

## <a name="filtering-a-list-of-jobs"></a><span data-ttu-id="265c9-175">A feladatok listájának szűrése</span><span class="sxs-lookup"><span data-stu-id="265c9-175">Filtering a list of jobs</span></span>

<span data-ttu-id="265c9-176">Követően a feladatok listáját a jelenlegi PowerShell-munkamenetben.</span><span class="sxs-lookup"><span data-stu-id="265c9-176">Once you have a list of jobs in your current PowerShell session.</span></span> <span data-ttu-id="265c9-177">Használhatja a szokásos PowerShell parancsmagok toofilter hello listája.</span><span class="sxs-lookup"><span data-stu-id="265c9-177">You can use normal PowerShell cmdlets toofilter hello list.</span></span>

<span data-ttu-id="265c9-178">Szűrő feladatok toohello feladatokra hello utolsó 24 órában küldött</span><span class="sxs-lookup"><span data-stu-id="265c9-178">Filter a list of jobs toohello jobs submitted in hello last 24 hours</span></span>

```
$upperdate = Get-Date
$lowerdate = $upperdate.AddHours(-24)
$jobs | Where-Object { $_.EndTime -ge $lowerdate }
```

<span data-ttu-id="265c9-179">Véget ért hello az elmúlt 24 órában feladatok toohello feladatok listájának szűrése</span><span class="sxs-lookup"><span data-stu-id="265c9-179">Filter a list of jobs toohello jobs that ended in hello last 24 hours</span></span>

```
$upperdate = Get-Date
$lowerdate = $upperdate.AddHours(-24)
$jobs | Where-Object { $_.SubmitTime -ge $lowerdate }
```

<span data-ttu-id="265c9-180">Szűrheti a feladatok elindításának toohello feladatok listáját.</span><span class="sxs-lookup"><span data-stu-id="265c9-180">Filter a list of jobs toohello jobs that started running.</span></span> <span data-ttu-id="265c9-181">Egy feladat sikertelen lehet a fordítás során –, és ezért soha nem kezdődik.</span><span class="sxs-lookup"><span data-stu-id="265c9-181">A job might fail at compile time - and so it never starts.</span></span> <span data-ttu-id="265c9-182">Nézzük hello sikertelen feladatok ténylegesen elindításának, majd nem sikerült.</span><span class="sxs-lookup"><span data-stu-id="265c9-182">Let's look at hello failed jobs that actually started running and then failed.</span></span>

```powershell
$jobs | Where-Object { $_.StartTime -ne $null }
```

### <a name="analyzing-a-list-of-jobs"></a><span data-ttu-id="265c9-183">Feladatokra elemzése</span><span class="sxs-lookup"><span data-stu-id="265c9-183">Analyzing a list of jobs</span></span>

<span data-ttu-id="265c9-184">Használjon hello `Group-Object` parancsmag tooanalyze feladatokra.</span><span class="sxs-lookup"><span data-stu-id="265c9-184">Use hello `Group-Object` cmdlet tooanalyze a list of jobs.</span></span>

```
# Count hello number of jobs by Submitter
$jobs | Group-Object Submitter | Select -Property Count,Name

# Count hello number of jobs by Result
$jobs | Group-Object Result | Select -Property Count,Name

# Count hello number of jobs by State
$jobs | Group-Object State | Select -Property Count,Name

#  Count hello number of jobs by DegreeOfParallelism
$jobs | Group-Object DegreeOfParallelism | Select -Property Count,Name
```
<span data-ttu-id="265c9-185">Ha az elemzések végrehajtását, hasznos tooadd tulajdonságok toohello feladat objektumok toomake szűrést és a csoportosítási egyszerűbb lehet.</span><span class="sxs-lookup"><span data-stu-id="265c9-185">When performing an analysis, it can be useful tooadd properties toohello Job objects toomake filtering and grouping simpler.</span></span> <span data-ttu-id="265c9-186">a következő kódrészletet hello jeleníti meg, hogyan tooannotate a Feladatinformáció a számított tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="265c9-186">hello following  snippet shows how tooannotate a JobInfo with calculated properties.</span></span>

```
function annotate_job( $j )
{
    $dic1 = @{
        Label='AUHours';
        Expression={ ($_.DegreeOfParallelism * ($_.EndTime-$_.StartTime).TotalHours)}}
    $dic2 = @{
        Label='DurationSeconds';
        Expression={ ($_.EndTime-$_.StartTime).TotalSeconds}}
    $dic3 = @{
        Label='DidRun';
        Expression={ ($_.StartTime -ne $null)}}

    $j2 = $j | select *, $dic1, $dic2, $dic3
    $j2
}

$jobs = Get-AdlJob -Account $adla -Top 10
$jobs = $jobs | %{ annotate_job( $_ ) }
```

## <a name="get-information-about-pipelines-and-recurrences"></a><span data-ttu-id="265c9-187">Adatcsatornák és ismétlődések adatainak beolvasása</span><span class="sxs-lookup"><span data-stu-id="265c9-187">Get information about pipelines and recurrences</span></span>

<span data-ttu-id="265c9-188">Használjon hello `Get-AdlJobPipeline` parancsmag toosee hello csővezeték korábban információkat feladatok.</span><span class="sxs-lookup"><span data-stu-id="265c9-188">Use hello `Get-AdlJobPipeline` cmdlet toosee hello pipeline information previously submitted jobs.</span></span>

```powershell
$pipelines = Get-AdlJobPipeline -Account $adla

$pipeline = Get-AdlJobPipeline -Account $adla -PipelineId "<pipeline ID>"
```

<span data-ttu-id="265c9-189">Használjon hello `Get-AdlJobRecurrence` parancsmag toosee hello ismétlődési információkat az előzőleg elküldött feladatok.</span><span class="sxs-lookup"><span data-stu-id="265c9-189">Use hello `Get-AdlJobRecurrence` cmdlet toosee hello recurrence information for previously submitted jobs.</span></span>

```powershell
$recurrences = Get-AdlJobRecurrence -Account $adla

$recurrence = Get-AdlJobRecurrence -Account $adla -RecurrenceId "<recurrence ID>"
```

## <a name="get-information-about-a-job"></a><span data-ttu-id="265c9-190">A feladat adatainak beolvasása</span><span class="sxs-lookup"><span data-stu-id="265c9-190">Get information about a job</span></span>

### <a name="get-job-status"></a><span data-ttu-id="265c9-191">Feladat állapotának beolvasása</span><span class="sxs-lookup"><span data-stu-id="265c9-191">Get job status</span></span>

<span data-ttu-id="265c9-192">Egy adott feladat hello állapotának beolvasása.</span><span class="sxs-lookup"><span data-stu-id="265c9-192">Get hello status of a specific job.</span></span>

```powershell
Get-AdlJob -AccountName $adla -JobId $job.JobId
```

### <a name="examine-hello-job-outputs"></a><span data-ttu-id="265c9-193">Vizsgálja meg a hello feladat kimenetének létrehozása</span><span class="sxs-lookup"><span data-stu-id="265c9-193">Examine hello job outputs</span></span>

<span data-ttu-id="265c9-194">Miután hello feladat befejeződött, ellenőrizze, hogy léteznek-e hello kimeneti fájl hello mappában lévő fájlok listáját.</span><span class="sxs-lookup"><span data-stu-id="265c9-194">After hello job has ended, check if hello output file exists by listing hello files in a folder.</span></span>

```powershell
Get-AdlStoreChildItem -Account $adls -Path "/"
```

## <a name="manage-running-jobs"></a><span data-ttu-id="265c9-195">Futó feladatok kezelése</span><span class="sxs-lookup"><span data-stu-id="265c9-195">Manage running jobs</span></span>

### <a name="cancel-a-job"></a><span data-ttu-id="265c9-196">Feladatok megszakítása</span><span class="sxs-lookup"><span data-stu-id="265c9-196">Cancel a job</span></span>

```powershell
Stop-AdlJob -Account $adls -JobID $jobID
```

### <a name="wait-for-a-job-toofinish"></a><span data-ttu-id="265c9-197">Várjon, amíg a feladat toofinish</span><span class="sxs-lookup"><span data-stu-id="265c9-197">Wait for a job toofinish</span></span>

<span data-ttu-id="265c9-198">Ismétlődő helyett `Get-AdlAnalyticsJob` egy feladat befejezéséig hello használhatja `Wait-AdlJob` parancsmag toowait hello feladat tooend számára.</span><span class="sxs-lookup"><span data-stu-id="265c9-198">Instead of repeating `Get-AdlAnalyticsJob` until a job finishes, you can use hello `Wait-AdlJob` cmdlet toowait for hello job tooend.</span></span>

```powershell
Wait-AdlJob -Account $adla -JobId $job.JobId
```

## <a name="manage-compute-policies"></a><span data-ttu-id="265c9-199">Számítási házirendjeinek kezelése</span><span class="sxs-lookup"><span data-stu-id="265c9-199">Manage compute policies</span></span>

### <a name="list-existing-compute-policies"></a><span data-ttu-id="265c9-200">A meglévő számítási házirendek felsorolása</span><span class="sxs-lookup"><span data-stu-id="265c9-200">List existing compute policies</span></span>

<span data-ttu-id="265c9-201">Hello `Get-AdlAnalyticsComputePolicy` parancsmag beolvassa a Data Lake Analytics-fiók számítási házirendekkel kapcsolatos információ.</span><span class="sxs-lookup"><span data-stu-id="265c9-201">hello `Get-AdlAnalyticsComputePolicy` cmdlet retrieves info about compute policies for a Data Lake Analytics account.</span></span>

```powershell
$policies = Get-AdlAnalyticsComputePolicy -Account $adla
```

### <a name="create-a-compute-policy"></a><span data-ttu-id="265c9-202">Számítási házirend létrehozása</span><span class="sxs-lookup"><span data-stu-id="265c9-202">Create a compute policy</span></span>

<span data-ttu-id="265c9-203">Hello `New-AdlAnalyticsComputePolicy` parancsmag létrehoz egy új számítási házirendet a Data Lake Analytics-fiók.</span><span class="sxs-lookup"><span data-stu-id="265c9-203">hello `New-AdlAnalyticsComputePolicy` cmdlet creates a new compute policy for a Data Lake Analytics account.</span></span> <span data-ttu-id="265c9-204">Ez a példa beállítása hello maximális ausztráliai elérhető toohello megadott felhasználói too50 és hello minimális feladat prioritása too250.</span><span class="sxs-lookup"><span data-stu-id="265c9-204">This example sets  hello maximum AUs available toohello specified user too50, and hello minimum job priority too250.</span></span>

```powershell
$userObjectId = (Get-AzureRmAdUser -SearchString "garymcdaniel@contoso.com").Id

New-AdlAnalyticsComputePolicy -Account $adla -Name "GaryMcDaniel" -ObjectId $objectId -ObjectType User -MaxDegreeOfParallelismPerJob 50 -MinPriorityPerJob 250
```

## <a name="check-for-hello-existence-of-a-file"></a><span data-ttu-id="265c9-205">Ellenőrizze, hogy egy fájl hello megléte.</span><span class="sxs-lookup"><span data-stu-id="265c9-205">Check for hello existence of a file.</span></span>

```powershell
Test-AdlStoreItem -Account $adls -Path "/data.csv"
```

## <a name="uploading-and-downloading"></a><span data-ttu-id="265c9-206">Feltöltés és letöltés</span><span class="sxs-lookup"><span data-stu-id="265c9-206">Uploading and downloading</span></span>

<span data-ttu-id="265c9-207">Fájl feltöltése.</span><span class="sxs-lookup"><span data-stu-id="265c9-207">Upload a file.</span></span>

```powershell
Import-AdlStoreItem -AccountName $adls -Path "c:\data.tsv" -Destination "/data_copy.csv" 
```

<span data-ttu-id="265c9-208">Töltsön fel egy teljes mappát rekurzív módon.</span><span class="sxs-lookup"><span data-stu-id="265c9-208">Upload an entire folder recursively.</span></span>

```powershell
Import-AdlStoreItem -AccountName $adls -Path "c:\myData\" -Destination "/myData/" -Recurse
```

<span data-ttu-id="265c9-209">Töltse le a fájlt.</span><span class="sxs-lookup"><span data-stu-id="265c9-209">Download a file.</span></span>

```powershell
Export-AdlStoreItem -AccountName $adls -Path "/data.csv" -Destination "c:\data.csv"
```

<span data-ttu-id="265c9-210">Töltse le egy teljes mappát rekurzív módon.</span><span class="sxs-lookup"><span data-stu-id="265c9-210">Download an entire folder recursively.</span></span>

```powershell
Export-AdlStoreItem -AccountName $adls -Path "/" -Destination "c:\myData\" -Recurse
```

> [!NOTE]
> <span data-ttu-id="265c9-211">Ha hello feltöltése vagy letöltési folyamat megszakad, megkísérelheti tooresume hello folyamat futó hello parancsmag történő újra hello ``-Resume`` jelzőt.</span><span class="sxs-lookup"><span data-stu-id="265c9-211">If hello upload or download process is interrupted, you can attempt tooresume hello process by running hello cmdlet again with hello ``-Resume`` flag.</span></span>

## <a name="manage-catalog-items"></a><span data-ttu-id="265c9-212">A szolgáltatáskatalógusban található elemek kezelése</span><span class="sxs-lookup"><span data-stu-id="265c9-212">Manage catalog items</span></span>

<span data-ttu-id="265c9-213">hello U-SQL catalog használt toostructure adatok és a kód, így azok megoszthatók U-SQL-parancsfájlok.</span><span class="sxs-lookup"><span data-stu-id="265c9-213">hello U-SQL catalog is used toostructure data and code so they can be shared by U-SQL scripts.</span></span> <span data-ttu-id="265c9-214">hello katalógus lehetővé teszi, hogy a lehető legjobb teljesítmény hello Azure Data Lake-adatokkal szolgálja.</span><span class="sxs-lookup"><span data-stu-id="265c9-214">hello catalog enables hello highest performance possible with data in Azure Data Lake.</span></span> <span data-ttu-id="265c9-215">További információk: [Use U-SQL catalog](data-lake-analytics-use-u-sql-catalog.md) (U-SQL-katalógus használata).</span><span class="sxs-lookup"><span data-stu-id="265c9-215">For more information, see [Use U-SQL catalog](data-lake-analytics-use-u-sql-catalog.md).</span></span>

### <a name="list-items-in-hello-u-sql-catalog"></a><span data-ttu-id="265c9-216">Hello U-SQL catalog listaelemeit.</span><span class="sxs-lookup"><span data-stu-id="265c9-216">List items in hello U-SQL catalog</span></span>

```powershell
# List U-SQL databases
Get-AdlCatalogItem -Account $adla -ItemType Database 

# List tables within a database
Get-AdlCatalogItem -Account $adla -ItemType Table -Path "database"

# List tables within a schema.
Get-AdlCatalogItem -Account $adla -ItemType Table -Path "database.schema"
```

<span data-ttu-id="265c9-217">Listázza az összes hello adatbázisának ADLA fiók összes hello szerelvényeket.</span><span class="sxs-lookup"><span data-stu-id="265c9-217">List all hello assemblies in all hello databases in an ADLA Account.</span></span>

```powershell
$dbs = Get-AdlCatalogItem -Account $adla -ItemType Database

foreach ($db in $dbs)
{
    $asms = Get-AdlCatalogItem -Account $adla -ItemType Assembly -Path $db.Name

    foreach ($asm in $asms)
    {
        $asmname = "[" + $db.Name + "].[" + $asm.Name + "]"
        Write-Host $asmname
    }
}
```

### <a name="get-details-about-a-catalog-item"></a><span data-ttu-id="265c9-218">Részletes információkat szolgáltatva katalóguselemek</span><span class="sxs-lookup"><span data-stu-id="265c9-218">Get details about a catalog item</span></span>

```powershell
# Get details of a table
Get-AdlCatalogItem  -Account $adla -ItemType Table -Path "master.dbo.mytable"

# Test existence of a U-SQL database.
Test-AdlCatalogItem  -Account $adla -ItemType Database -Path "master"
```

### <a name="create-credentials-in-a-catalog"></a><span data-ttu-id="265c9-219">Hitelesítő adatok létrehozása a katalógusban</span><span class="sxs-lookup"><span data-stu-id="265c9-219">Create credentials in a catalog</span></span>

<span data-ttu-id="265c9-220">U-SQL-adatbázis hozzon létre egy Azure-ban üzemeltetett adatbázis hitelesítő objektumot.</span><span class="sxs-lookup"><span data-stu-id="265c9-220">Within a U-SQL database, create a credential object for a database hosted in Azure.</span></span> <span data-ttu-id="265c9-221">U-SQL hitelesítő adatok jelenleg hello csak ilyen típusú elemet, amely a PowerShell segítségével is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="265c9-221">Currently, U-SQL credentials are hello only type of catalog item that you can create through PowerShell.</span></span>

```powershell
$dbName = "master"
$credentialName = "ContosoDbCreds"
$dbUri = "https://contoso.database.windows.net:8080"

New-AdlCatalogCredential -AccountName $adla `
          -DatabaseName $db `
          -CredentialName $credentialName `
          -Credential (Get-Credential) `
          -Uri $dbUri
```

### <a name="get-basic-information-about-an-adla-account"></a><span data-ttu-id="265c9-222">ADLA fiók alapszintű adatainak beolvasása</span><span class="sxs-lookup"><span data-stu-id="265c9-222">Get basic information about an ADLA account</span></span>

<span data-ttu-id="265c9-223">A megadott fióknév a következő kód hello keresi a hello fiókkal kapcsolatos alapvető információk</span><span class="sxs-lookup"><span data-stu-id="265c9-223">Given an account name, hello following code looks up basic information about hello account</span></span>

```
$adla_acct = Get-AdlAnalyticsAccount -Name "saveenrdemoadla"
$adla_name = $adla_acct.Name
$adla_subid = $adla_acct.Id.Split("/")[2]
$adla_sub = Get-AzureRmSubscription -SubscriptionId $adla_subid
$adla_subname = $adla_sub.Name
$adla_defadls_datasource = Get-AdlAnalyticsDataSource -Account $adla_name  | ? { $_.IsDefault } 
$adla_defadlsname = $adla_defadls_datasource.Name

Write-Host "ADLA Account Name" $adla_name
Write-Host "Subscription Id" $adla_subid
Write-Host "Subscription Name" $adla_subname
Write-Host "Defautl ADLS Store" $adla_defadlsname
Write-Host 

Write-Host '$subname' " = ""$adla_subname"" "
Write-Host '$subid' " = ""$adla_subid"" "
Write-Host '$adla' " = ""$adla_name"" "
Write-Host '$adls' " = ""$adla_defadlsname"" "
```

## <a name="working-with-azure"></a><span data-ttu-id="265c9-224">Azure használata</span><span class="sxs-lookup"><span data-stu-id="265c9-224">Working with Azure</span></span>

### <a name="get-details-of-azurerm-errors"></a><span data-ttu-id="265c9-225">Részletes AzureRm hibák</span><span class="sxs-lookup"><span data-stu-id="265c9-225">Get details of AzureRm errors</span></span>

```powershell
Resolve-AzureRmError -Last
```

### <a name="verify-if-you-are-running-as-an-administrator"></a><span data-ttu-id="265c9-226">Győződjön meg arról, ha rendszergazdaként futtatja</span><span class="sxs-lookup"><span data-stu-id="265c9-226">Verify if you are running as an administrator</span></span>

```powershell
function Test-Administrator  
{  
    $user = [Security.Principal.WindowsIdentity]::GetCurrent();
    $p = New-Object Security.Principal.WindowsPrincipal $user
    $p.IsInRole([Security.Principal.WindowsBuiltinRole]::Administrator)  
}
```

### <a name="find-a-tenantid"></a><span data-ttu-id="265c9-227">A TenantID keresése</span><span class="sxs-lookup"><span data-stu-id="265c9-227">Find a TenantID</span></span>

<span data-ttu-id="265c9-228">Előfizetés neve:</span><span class="sxs-lookup"><span data-stu-id="265c9-228">From a subscription name:</span></span>

```powershell
function Get-TenantIdFromSubcriptionName( [string] $subname )
{
    $sub = (Get-AzureRmSubscription -SubscriptionName $subname)
    $sub.TenantId
}

Get-TenantIdFromSubcriptionName "ADLTrainingMS"
```

<span data-ttu-id="265c9-229">Az előfizetés-azonosító:</span><span class="sxs-lookup"><span data-stu-id="265c9-229">From a subscription id:</span></span>

```powershell
function Get-TenantIdFromSubcriptionId( [string] $subid )
{
    $sub = (Get-AzureRmSubscription -SubscriptionId $subid)
    $sub.TenantId
}

$subid = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
Get-TenantIdFromSubcriptionId $subid
```

<span data-ttu-id="265c9-230">Például a "contoso.com" tartomány címről</span><span class="sxs-lookup"><span data-stu-id="265c9-230">From a domain address such as "contoso.com"</span></span>


```powershell
function Get-TenantIdFromDomain( $domain )
{
    $url = "https://login.windows.net/" + $domain + "/.well-known/openid-configuration"
    return (Invoke-WebRequest $url|ConvertFrom-Json).token_endpoint.Split('/')[3]
}

$domain = "contoso.com"
Get-TenantIdFromDomain $domain
```

### <a name="list-all-your-subscriptions-and-tenant-ids"></a><span data-ttu-id="265c9-231">Az előfizetések listán, és a bérlői azonosító</span><span class="sxs-lookup"><span data-stu-id="265c9-231">List all your subscriptions and tenant ids</span></span>

```powershell
$subs = Get-AzureRmSubscription
foreach ($sub in $subs)
{
    Write-Host $sub.Name "("  $sub.Id ")"
    Write-Host "`tTenant Id" $sub.TenantId
}
```

## <a name="create-a-data-lake-analytics-account-using-a-template"></a><span data-ttu-id="265c9-232">Egy sablon használatával a Data Lake Analytics-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="265c9-232">Create a Data Lake Analytics account using a template</span></span>

<span data-ttu-id="265c9-233">Egy Azure erőforráscsoport-sablon használatával a következő PowerShell-parancsfájl hello is használhatja:</span><span class="sxs-lookup"><span data-stu-id="265c9-233">You can also use an Azure Resource Group template using hello following  PowerShell script:</span></span>

```powershell
$subId = "<Your Azure Subscription ID>"

$rg = "<New Azure Resource Group Name>"
$location = "<Location (such as East US 2)>"
$adls = "<New Data Lake Store Account Name>"
$adla = "<New Data Lake Analytics Account Name>"

$deploymentName = "MyDataLakeAnalyticsDeployment"
$armTemplateFile = "<LocalFolderPath>\azuredeploy.json"  # update hello JSON template path 

# Log in tooAzure
Login-AzureRmAccount -SubscriptionId $subId

# Create hello resource group
New-AzureRmResourceGroup -Name $rg -Location $location

# Create hello Data Lake Analytics account with hello default Data Lake Store account.
$parameters = @{"adlAnalyticsName"=$adla; "adlStoreName"=$adls}
New-AzureRmResourceGroupDeployment -Name $deploymentName -ResourceGroupName $rg -TemplateFile $armTemplateFile -TemplateParameterObject $parameters 
```

<span data-ttu-id="265c9-234">További információkért lásd: [Azure Resource Manager-sablon az alkalmazás központi telepítését](../azure-resource-manager/resource-group-template-deploy.md) és [Azure Resource Manager-sablonok készítése](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="265c9-234">For more information, see [Deploy an application with Azure Resource Manager template](../azure-resource-manager/resource-group-template-deploy.md) and [Authoring Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

<span data-ttu-id="265c9-235">**Példa sablon**</span><span class="sxs-lookup"><span data-stu-id="265c9-235">**Example template**</span></span>

<span data-ttu-id="265c9-236">A következő szöveg hello menteni egy `.json` fájlt, és a PowerShell parancsfájl toouse hello sablon megelőző hello használja.</span><span class="sxs-lookup"><span data-stu-id="265c9-236">Save hello following text as a `.json` file, and then use hello preceding PowerShell script toouse hello template.</span></span> 

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "adlAnalyticsName": {
      "type": "string",
      "metadata": {
        "description": "hello name of hello Data Lake Analytics account toocreate."
      }
    },
    "adlStoreName": {
      "type": "string",
      "metadata": {
        "description": "hello name of hello Data Lake Store account toocreate."
      }
    }
  },
  "resources": [
    {
      "name": "[parameters('adlStoreName')]",
      "type": "Microsoft.DataLakeStore/accounts",
      "location": "East US 2",
      "apiVersion": "2015-10-01-preview",
      "dependsOn": [ ],
      "tags": { }
    },
    {
      "name": "[parameters('adlAnalyticsName')]",
      "type": "Microsoft.DataLakeAnalytics/accounts",
      "location": "East US 2",
      "apiVersion": "2015-10-01-preview",
      "dependsOn": [ "[concat('Microsoft.DataLakeStore/accounts/',parameters('adlStoreName'))]" ],
      "tags": { },
      "properties": {
        "defaultDataLakeStoreAccount": "[parameters('adlStoreName')]",
        "dataLakeStoreAccounts": [
          { "name": "[parameters('adlStoreName')]" }
        ]
      }
    }
  ],
  "outputs": {
    "adlAnalyticsAccount": {
      "type": "object",
      "value": "[reference(resourceId('Microsoft.DataLakeAnalytics/accounts',parameters('adlAnalyticsName')))]"
    },
    "adlStoreAccount": {
      "type": "object",
      "value": "[reference(resourceId('Microsoft.DataLakeStore/accounts',parameters('adlStoreName')))]"
    }
  }
}
```

## <a name="next-steps"></a><span data-ttu-id="265c9-237">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="265c9-237">Next steps</span></span>
* [<span data-ttu-id="265c9-238">A Microsoft Azure Data Lake Analytics áttekintése</span><span class="sxs-lookup"><span data-stu-id="265c9-238">Overview of Microsoft Azure Data Lake Analytics</span></span>](data-lake-analytics-overview.md)
* <span data-ttu-id="265c9-239">A Data Lake Analytics használatának első lépései [Azure-portálon](data-lake-analytics-get-started-portal.md) | [Azure PowerShell](data-lake-analytics-get-started-powershell.md) | [CLI 2.0](data-lake-analytics-get-started-cli2.md)</span><span class="sxs-lookup"><span data-stu-id="265c9-239">Get started with Data Lake Analytics using [Azure portal](data-lake-analytics-get-started-portal.md) | [Azure PowerShell](data-lake-analytics-get-started-powershell.md) | [CLI 2.0](data-lake-analytics-get-started-cli2.md)</span></span>
* <span data-ttu-id="265c9-240">Azure Data Lake Analytics használatának kezelése [Azure-portálon](data-lake-analytics-manage-use-portal.md) | [Azure PowerShell](data-lake-analytics-manage-use-powershell.md) | [parancssori felület](data-lake-analytics-manage-use-cli.md)</span><span class="sxs-lookup"><span data-stu-id="265c9-240">Manage Azure Data Lake Analytics using [Azure portal](data-lake-analytics-manage-use-portal.md) | [Azure PowerShell](data-lake-analytics-manage-use-powershell.md) | [CLI](data-lake-analytics-manage-use-cli.md)</span></span> 
