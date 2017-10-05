---
title: "Azure Data Lake Analytics az Azure PowerShell kezelése |} Microsoft Docs"
description: "Megtudhatja, hogyan kezelheti a Data Lake Analytics-fiókok, adatforrások, feladatok és a szolgáltatáskatalógusban található elemek. "
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
ms.openlocfilehash: 862e9551f1e129b7bba06651fbae94e337c92dcb
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="manage-azure-data-lake-analytics-using-azure-powershell"></a><span data-ttu-id="0d8c1-103">Az Azure Data Lake Analytics kezelése az Azure PowerShell-lel</span><span class="sxs-lookup"><span data-stu-id="0d8c1-103">Manage Azure Data Lake Analytics using Azure PowerShell</span></span>
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

<span data-ttu-id="0d8c1-104">Útmutató: Azure Data Lake Analytics-fiókok, adatforrások, feladatok és Azure PowerShell használatával a szolgáltatáskatalógusban található elemek kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-104">Learn how to manage Azure Data Lake Analytics accounts, data sources, jobs, and catalog items using Azure PowerShell.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="0d8c1-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="0d8c1-105">Prerequisites</span></span>

<span data-ttu-id="0d8c1-106">Data Lake Analytics-fiók létrehozásakor meg kell tudnia:</span><span class="sxs-lookup"><span data-stu-id="0d8c1-106">When creating a Data Lake Analytics account, you need to know:</span></span>

* <span data-ttu-id="0d8c1-107">**Előfizetés-azonosító**: az Azure előfizetés-azonosító alapján, amely a Data Lake Analytics-fiók található.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-107">**Subscription ID**: The Azure subscription ID under which your Data Lake Analytics account resides.</span></span>
* <span data-ttu-id="0d8c1-108">**Erőforráscsoport**: az Azure erőforráscsoport, amely tartalmazza a Data Lake Analytics-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-108">**Resource group**: The name of the Azure resource group that contains your Data Lake Analytics account.</span></span>
* <span data-ttu-id="0d8c1-109">**A Data Lake Analytics-fiók neve**: A fiók neve csak kisbetűket és számokat tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-109">**Data Lake Analytics account name**: The account name must only contain lowercase letters and numbers.</span></span>
* <span data-ttu-id="0d8c1-110">**Alapértelmezett Data Lake Store-fiók:** minden Data Lake Analytics-fiókhoz tartozik egy alapértelmezett Data Lake Store-fiók.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-110">**Default Data Lake Store account**: Each Data Lake Analytics account has a default Data Lake Store account.</span></span> <span data-ttu-id="0d8c1-111">A fiókoknak azonos helyen kell lenniük.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-111">These accounts must be in the same location.</span></span>
* <span data-ttu-id="0d8c1-112">**Hely**: Data Lake Analytics-fiókja, például az "USA keleti régiója 2", vagy más helyre támogatott helyeket.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-112">**Location**: The location of your Data Lake Analytics account, such as "East US 2" or other supported locations.</span></span> <span data-ttu-id="0d8c1-113">Támogatott helyek láthatóak a [árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/data-lake-analytics/).</span><span class="sxs-lookup"><span data-stu-id="0d8c1-113">Supported locations can be seen on our [pricing page](https://azure.microsoft.com/pricing/details/data-lake-analytics/).</span></span>

<span data-ttu-id="0d8c1-114">A jelen oktatóanyagban szereplő PowerShell-kódrészletek ezeket a változókat használják adattárolásra</span><span class="sxs-lookup"><span data-stu-id="0d8c1-114">The PowerShell snippets in this tutorial use these variables to store this information</span></span>

```powershell
$subId = "<SubscriptionId>"
$rg = "<ResourceGroupName>"
$adla = "<DataLakeAnalyticsAccountName>"
$adls = "<DataLakeStoreAccountName>"
$location = "<Location>"
```

## <a name="log-in"></a><span data-ttu-id="0d8c1-115">Bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="0d8c1-115">Log in</span></span>

<span data-ttu-id="0d8c1-116">Jelentkezzen be egy előfizetési azonosító használatával.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-116">Log in using a subscription id.</span></span>

```powershell
Login-AzureRmAccount -SubscriptionId $subId
```

<span data-ttu-id="0d8c1-117">Jelentkezzen be egy előfizetés nevét.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-117">Log in using a subscription name.</span></span>

```
Login-AzureRmAccount -SubscriptionName $subname 
```

<span data-ttu-id="0d8c1-118">A `Login-AzureRmAccount` parancsmag mindig elkéri a hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-118">The `Login-AzureRmAccount` cmdlet  always prompts for credentials.</span></span> <span data-ttu-id="0d8c1-119">Meg kell adni a következő parancsmagok használatával elkerülheti a:</span><span class="sxs-lookup"><span data-stu-id="0d8c1-119">You can avoid being prompted by using the following cmdlets:</span></span>

```powershell
# Save login session information
Save-AzureRmProfile -Path D:\profile.json  

# Load login session information
Select-AzureRmProfile -Path D:\profile.json 
```

## <a name="managing-accounts"></a><span data-ttu-id="0d8c1-120">Fiókok kezelése</span><span class="sxs-lookup"><span data-stu-id="0d8c1-120">Managing accounts</span></span>

### <a name="create-a-data-lake-analytics-account"></a><span data-ttu-id="0d8c1-121">Data Lake Analytics-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="0d8c1-121">Create a Data Lake Analytics account</span></span>

<span data-ttu-id="0d8c1-122">Ha még nem rendelkezik egy [erőforráscsoport](../azure-resource-manager/resource-group-overview.md#resource-groups) használatához hozzon létre egyet.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-122">If you don't already have a [resource group](../azure-resource-manager/resource-group-overview.md#resource-groups) to use, create one.</span></span> 

```powershell
New-AzureRmResourceGroup -Name  $rg -Location $location
```

<span data-ttu-id="0d8c1-123">A naplók tárolásához minden Data Lake Analytics-fiók esetében szükség van egy alapértelmezett Data Lake Store-fiókra.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-123">Every Data Lake Analytics account requires a default Data Lake Store account that it uses for storing logs.</span></span> <span data-ttu-id="0d8c1-124">Egy meglévő fiókot újra, vagy hozzon létre egy fiókot.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-124">You can reuse an existing account or create an account.</span></span> 

```powershell
New-AdlStore -ResourceGroupName $rg -Name $adls -Location $location
```

<span data-ttu-id="0d8c1-125">Ha az erőforráscsoport és a Data Lake Store-fiók már elérhető, hozzon létre egy Data Lake Analytics-fiókot.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-125">Once a Resource Group and Data Lake Store account is available, create a Data Lake Analytics account.</span></span>

```powershell
New-AdlAnalyticsAccount -ResourceGroupName $rg -Name $adla -Location $location -DefaultDataLake $adls
```

### <a name="get-information-about-an-account"></a><span data-ttu-id="0d8c1-126">Egy fiók adatainak beolvasása</span><span class="sxs-lookup"><span data-stu-id="0d8c1-126">Get information about an account</span></span>

<span data-ttu-id="0d8c1-127">Ezzel a fiókkal kapcsolatos adatokat.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-127">Get details about an account.</span></span>

```powershell
Get-AdlAnalyticsAccount -Name $adla
```

<span data-ttu-id="0d8c1-128">Ellenőrizze, létezik-e egy adott Data Lake Analytics-fiókot.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-128">Check the existence of a specific Data Lake Analytics account.</span></span> <span data-ttu-id="0d8c1-129">A parancsmag ad vissza, vagy `True` vagy `False`.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-129">The cmdlet returns either `True` or `False`.</span></span>

```powershell
Test-AdlAnalyticsAccount -Name $adla
```

<span data-ttu-id="0d8c1-130">Ellenőrizze, létezik-e egy adott Data Lake Store-fiókot.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-130">Check the existence of a specific Data Lake Store account.</span></span> <span data-ttu-id="0d8c1-131">A parancsmag ad vissza, vagy `True` vagy `False`.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-131">The cmdlet returns either `True` or `False`.</span></span>

```powershell
Test-AdlStoreAccount -Name $adls
```

### <a name="listing-accounts"></a><span data-ttu-id="0d8c1-132">Fiókok listázása</span><span class="sxs-lookup"><span data-stu-id="0d8c1-132">Listing accounts</span></span>

<span data-ttu-id="0d8c1-133">Lista Data Lake Analytics-fiókok az aktuális előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-133">List Data Lake Analytics accounts within the current subscription.</span></span>

```powershell
Get-AdlAnalyticsAccount
```

<span data-ttu-id="0d8c1-134">Lista Data Lake Analytics-fiókok egy adott erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-134">List Data Lake Analytics accounts within a specific resource group.</span></span>

```powershell
Get-AdlAnalyticsAccount -ResourceGroupName $rg
```

## <a name="managing-firewall-rules"></a><span data-ttu-id="0d8c1-135">Tűzfal-szabályok kezelése</span><span class="sxs-lookup"><span data-stu-id="0d8c1-135">Managing firewall rules</span></span>

<span data-ttu-id="0d8c1-136">Tűzfalszabályok listája.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-136">List firewall rules.</span></span>

```powershell
Get-AdlAnalyticsFirewallRule -Account $adla
```

<span data-ttu-id="0d8c1-137">Fel egy tűzfalszabályt.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-137">Add a firewall rule.</span></span>

```powershell
$ruleName = "Allow access from on-prem server"
$startIpAddress = "<start IP address>"
$endIpAddress = "<end IP address>"

Add-AdlAnalyticsFirewallRule -Account $adla -Name $ruleName -StartIpAddress $startIpAddress -EndIpAddress $endIpAddress
```

<span data-ttu-id="0d8c1-138">Egy tűzfalszabály módosítása.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-138">Change a firewall rule.</span></span>

```powershell
Set-AdlAnalyticsFirewallRule -Account $adla -Name $ruleName -StartIpAddress $startIpAddress -EndIpAddress $endIpAddress
```

<span data-ttu-id="0d8c1-139">Egy tűzfalszabály eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-139">Remove a firewall rule.</span></span>

```powershell
Remove-AdlAnalyticsFirewallRule -Account $adla -Name $ruleName
```



<span data-ttu-id="0d8c1-140">Azure IP-címek engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-140">Allow Azure IP addresses.</span></span>

```powershell
Set-AdlAnalyticsAccount -Name $adla -AllowAzureIpState Enabled
```

```powershell
Set-AdlAnalyticsAccount -Name $adla -FirewallState Enabled
Set-AdlAnalyticsAccount -Name $adla -FirewallState Disabled
```

## <a name="managing-data-sources"></a><span data-ttu-id="0d8c1-141">Az adatforrások kezelése</span><span class="sxs-lookup"><span data-stu-id="0d8c1-141">Managing data sources</span></span>
<span data-ttu-id="0d8c1-142">Az Azure Data Lake Analytics jelenleg a következő adatforrásokat támogatja:</span><span class="sxs-lookup"><span data-stu-id="0d8c1-142">Azure Data Lake Analytics currently supports the following data sources:</span></span>

* [<span data-ttu-id="0d8c1-143">Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="0d8c1-143">Azure Data Lake Store</span></span>](../data-lake-store/data-lake-store-overview.md)
* [<span data-ttu-id="0d8c1-144">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="0d8c1-144">Azure Storage</span></span>](../storage/common/storage-introduction.md)

<span data-ttu-id="0d8c1-145">Az Analytics-fiók létrehozásakor ki kell jelölnie egy Data Lake Store-fiókot az alapértelmezett adatforrás lehet.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-145">When you create an Analytics account, you must designate a Data Lake Store account to be the default data source.</span></span> <span data-ttu-id="0d8c1-146">Az alapértelmezett Data Lake Store-fiók feladat metaadatok és a feladat a vizsgálati naplók tárolására szolgál.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-146">The default Data Lake Store account is used to store job metadata and job audit logs.</span></span> <span data-ttu-id="0d8c1-147">Data Lake Analytics-fiók létrehozása után hozzáadhat további Data Lake Store-fiókok és/vagy a Storage-fiókok.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-147">After you have created a Data Lake Analytics account, you can add additional Data Lake Store accounts and/or Storage accounts.</span></span> 

### <a name="find-the-default-data-lake-store-account"></a><span data-ttu-id="0d8c1-148">Az alapértelmezett Data Lake Store-fiók található</span><span class="sxs-lookup"><span data-stu-id="0d8c1-148">Find the default Data Lake Store account</span></span>

```powershell
$adla_acct = Get-AdlAnalyticsAccount -Name $adla
$dataLakeStoreName = $adla_acct.DefaultDataLakeAccount
```

<span data-ttu-id="0d8c1-149">Az alapértelmezett Data Lake Store-fiók található adatforrások által listájának szűrése révén a `IsDefault` tulajdonság:</span><span class="sxs-lookup"><span data-stu-id="0d8c1-149">You can find the default Data Lake Store account by filtering the list of datasources by the `IsDefault` property:</span></span>

```powershell
Get-AdlAnalyticsDataSource -Account $adla  | ? { $_.IsDefault } 
```

### <a name="add-a-data-source"></a><span data-ttu-id="0d8c1-150">Egy adatforrás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="0d8c1-150">Add a data source</span></span>

```powershell

# Add an additional Storage (Blob) account.
$AzureStorageAccountName = "<AzureStorageAccountName>"
$AzureStorageAccountKey = "<AzureStorageAccountKey>"
Add-AdlAnalyticsDataSource -Account $adla -Blob $AzureStorageAccountName -AccessKey $AzureStorageAccountKey

# Add an additional Data Lake Store account.
$AzureDataLakeStoreName = "<AzureDataLakeStoreAccountName"
Add-AdlAnalyticsDataSource -Account $adla -DataLakeStore $AzureDataLakeStoreName 
```

### <a name="list-data-sources"></a><span data-ttu-id="0d8c1-151">Az adatforrások listája</span><span class="sxs-lookup"><span data-stu-id="0d8c1-151">List data sources</span></span>

```powershell
# List all the data sources
Get-AdlAnalyticsDataSource -Name $adla

# List attached Data Lake Store accounts
Get-AdlAnalyticsDataSource -Name $adla | where -Property Type -EQ "DataLakeStore"

# List attached Storage accounts
Get-AdlAnalyticsDataSource -Name $adla | where -Property Type -EQ "Blob"
```

## <a name="submit-u-sql-jobs"></a><span data-ttu-id="0d8c1-152">U-SQL-feladatok küldése</span><span class="sxs-lookup"><span data-stu-id="0d8c1-152">Submit U-SQL jobs</span></span>

### <a name="submit-a-string-as-a-u-sql-script"></a><span data-ttu-id="0d8c1-153">Küldje el egy karakterlánc, egy U-SQL-parancsfájl</span><span class="sxs-lookup"><span data-stu-id="0d8c1-153">Submit a string as a U-SQL script</span></span>

```powershell
$script = @"
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS D( customer, amount );
OUTPUT @a
    TO "/data.csv"
    USING Outputters.Csv();
"@

$scriptpath = "d:\test.usql"
$script | Out-File $scriptpath 

Submit-AdlJob -AccountName $adla -Script $script -Name "Demo"
```


### <a name="submit-a-file-as-a-u-sql-script"></a><span data-ttu-id="0d8c1-154">Küldje el a fájlt egy U-SQL-parancsfájl</span><span class="sxs-lookup"><span data-stu-id="0d8c1-154">Submit a file as a U-SQL script</span></span>

```powershell
$scriptpath = "d:\test.usql"
$script | Out-File $scriptpath 
Submit-AdlJob -AccountName $adla –ScriptPath $scriptpath -Name "Demo"
```

## <a name="list-jobs-in-an-account"></a><span data-ttu-id="0d8c1-155">Egy fiók feladatok listája</span><span class="sxs-lookup"><span data-stu-id="0d8c1-155">List jobs in an account</span></span>

### <a name="list-all-the-jobs-in-the-account"></a><span data-ttu-id="0d8c1-156">Sorolja fel a fiókban lévő összes feladatot.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-156">List all the jobs in the account.</span></span> 

<span data-ttu-id="0d8c1-157">A kimenet tartalmazza az aktuálisan futó és a nemrégiben befejezett feladatokat is.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-157">The output includes the currently running jobs and those jobs that have recently completed.</span></span>

```powershell
Get-AdlJob -Account $adla
```


### <a name="list-a-specific-number-of-jobs"></a><span data-ttu-id="0d8c1-158">Egy bizonyos számú feladatok listázása</span><span class="sxs-lookup"><span data-stu-id="0d8c1-158">List a specific number of jobs</span></span>

<span data-ttu-id="0d8c1-159">A feladatok van rendezve, az alapértelmezés szerint idő nyújt.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-159">By default the list of jobs is sorted on submit time.</span></span> <span data-ttu-id="0d8c1-160">Ezért a legutóbb elküldött feladatok elsőként jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-160">So the most recently submitted jobs appear first.</span></span> <span data-ttu-id="0d8c1-161">Alapértelmezés szerint a ADLA fiók feladatok emlékszik 180 napig tart, de a Ge-AdlJob parancsmag alapértelmezés szerint csak az első 500 adja vissza.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-161">By default, The ADLA account remembers jobs for 180 days, but the Ge-AdlJob  cmdlet by default returns only the first 500.</span></span> <span data-ttu-id="0d8c1-162">Használja az - felső paraméter egy bizonyos számú feladatok listázásához.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-162">Use -Top parameter to list a specific number of jobs.</span></span>

```powershell
$jobs = Get-AdlJob -Account $adla -Top 10
```


### <a name="list-jobs-based-on-the-value-of-job-property"></a><span data-ttu-id="0d8c1-163">Lista feladatok feladat tulajdonság értéke alapján</span><span class="sxs-lookup"><span data-stu-id="0d8c1-163">List jobs based on the value of job property</span></span>

<span data-ttu-id="0d8c1-164">Használja a `-State` paraméter.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-164">Using the `-State` parameter.</span></span> <span data-ttu-id="0d8c1-165">Ezek bármelyike kombinálhatja:</span><span class="sxs-lookup"><span data-stu-id="0d8c1-165">You can combine any of these values:</span></span>

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
# List the running jobs
Get-AdlJob -Account $adla -State Running

# List the jobs that have completed
Get-AdlJob -Account $adla -State Ended

# List the jobs that have not started yet
Get-AdlJob -Account $adla -State Accepted,Compiling,New,Paused,Scheduling,Start
```

<span data-ttu-id="0d8c1-166">Használja a `-Result` paraméter észleli, hogy befejeződött a feladat sikeresen befejeződött-e.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-166">Use the `-Result` parameter to detect whether ended jobs completed successfully.</span></span> <span data-ttu-id="0d8c1-167">Ezekkel az értékekkel rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="0d8c1-167">It has these values:</span></span>

* <span data-ttu-id="0d8c1-168">Megszakítva</span><span class="sxs-lookup"><span data-stu-id="0d8c1-168">Cancelled</span></span>
* <span data-ttu-id="0d8c1-169">Nem sikerült</span><span class="sxs-lookup"><span data-stu-id="0d8c1-169">Failed</span></span>
* <span data-ttu-id="0d8c1-170">None</span><span class="sxs-lookup"><span data-stu-id="0d8c1-170">None</span></span>
* <span data-ttu-id="0d8c1-171">Sikeres</span><span class="sxs-lookup"><span data-stu-id="0d8c1-171">Succeeded</span></span>

``` powershell
# List Successful jobs.
Get-AdlJob -Account $adla -State Ended -Result Succeeded

# List Failed jobs.
Get-AdlJob -Account $adla -State Ended -Result Failed
```


<span data-ttu-id="0d8c1-172">A `-Submitter` paraméter segít azonosítani, ki egy feladat elküldése megtörtént.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-172">The `-Submitter` parameter helps you identify who submitted a job.</span></span>

```powershell
Get-AdlJob -Account $adla -Submitter "joe@contoso.com"
```

<span data-ttu-id="0d8c1-173">A `-SubmittedAfter` hasznos egy időtartományt a szűrés.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-173">The `-SubmittedAfter` is useful in filtering to a time range.</span></span>


```powershell
# List  jobs submitted in the last day.
$d = [DateTime]::Now.AddDays(-1)
Get-AdlJob -Account $adla -SubmittedAfter $d

# List  jobs submitted in the last seven day.
$d = [DateTime]::Now.AddDays(-7)
Get-AdlJob -Account $adla -SubmittedAfter $d
```

### <a name="common-scenarios-for-listing-jobs"></a><span data-ttu-id="0d8c1-174">Általános példák a feladatok listázása</span><span class="sxs-lookup"><span data-stu-id="0d8c1-174">Common scenarios for listing jobs</span></span>


```
# List jobs submitted in the last five days and that successfully completed.
$d = (Get-Date).AddDays(-5)
Get-AdlJob -Account $adla -SubmittedAfter $d -State Ended -Result Succeeded

# List all failed jobs submitted by "joe@contoso.com" within the past seven days.
Get-AdlJob -Account $adla `
    -Submitter "joe@contoso.com" `
    -SubmittedAfter (Get-Date).AddDays(-7) `
    -Result Failed
```

## <a name="filtering-a-list-of-jobs"></a><span data-ttu-id="0d8c1-175">A feladatok listájának szűrése</span><span class="sxs-lookup"><span data-stu-id="0d8c1-175">Filtering a list of jobs</span></span>

<span data-ttu-id="0d8c1-176">Követően a feladatok listáját a jelenlegi PowerShell-munkamenetben.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-176">Once you have a list of jobs in your current PowerShell session.</span></span> <span data-ttu-id="0d8c1-177">Normál PowerShell-parancsmagok segítségével a tulajdonságlista szűréséhez.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-177">You can use normal PowerShell cmdlets to filter the list.</span></span>

<span data-ttu-id="0d8c1-178">A feladatok az elmúlt 24 órában küldött feladatok listájának szűrése</span><span class="sxs-lookup"><span data-stu-id="0d8c1-178">Filter a list of jobs to the jobs submitted in the last 24 hours</span></span>

```
$upperdate = Get-Date
$lowerdate = $upperdate.AddHours(-24)
$jobs | Where-Object { $_.EndTime -ge $lowerdate }
```

<span data-ttu-id="0d8c1-179">Feladatok az elmúlt 24 órában befejeződött feladatok listájának szűrése</span><span class="sxs-lookup"><span data-stu-id="0d8c1-179">Filter a list of jobs to the jobs that ended in the last 24 hours</span></span>

```
$upperdate = Get-Date
$lowerdate = $upperdate.AddHours(-24)
$jobs | Where-Object { $_.SubmitTime -ge $lowerdate }
```

<span data-ttu-id="0d8c1-180">Szűrheti a feladatok elindításának feladatok listáját.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-180">Filter a list of jobs to the jobs that started running.</span></span> <span data-ttu-id="0d8c1-181">Egy feladat sikertelen lehet a fordítás során –, és ezért soha nem kezdődik.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-181">A job might fail at compile time - and so it never starts.</span></span> <span data-ttu-id="0d8c1-182">Vizsgáljuk meg a sikertelen feladatok ténylegesen elindításának, majd nem sikerült.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-182">Let's look at the failed jobs that actually started running and then failed.</span></span>

```powershell
$jobs | Where-Object { $_.StartTime -ne $null }
```

### <a name="analyzing-a-list-of-jobs"></a><span data-ttu-id="0d8c1-183">Feladatokra elemzése</span><span class="sxs-lookup"><span data-stu-id="0d8c1-183">Analyzing a list of jobs</span></span>

<span data-ttu-id="0d8c1-184">Használja a `Group-Object` parancsmag feladatokra elemzéséhez.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-184">Use the `Group-Object` cmdlet to analyze a list of jobs.</span></span>

```
# Count the number of jobs by Submitter
$jobs | Group-Object Submitter | Select -Property Count,Name

# Count the number of jobs by Result
$jobs | Group-Object Result | Select -Property Count,Name

# Count the number of jobs by State
$jobs | Group-Object State | Select -Property Count,Name

#  Count the number of jobs by DegreeOfParallelism
$jobs | Group-Object DegreeOfParallelism | Select -Property Count,Name
```
<span data-ttu-id="0d8c1-185">Ha az elemzések végrehajtását, tulajdonságok hozzáadása a feladat objektumok egyszerűbbé teszik a szűrést és a csoportosítási hasznos lehet.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-185">When performing an analysis, it can be useful to add properties to the Job objects to make filtering and grouping simpler.</span></span> <span data-ttu-id="0d8c1-186">Az alábbi kódrészletben láthatja ellátása megjegyzésekkel a Feladatinformáció számított tulajdonságokkal.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-186">The following  snippet shows how to annotate a JobInfo with calculated properties.</span></span>

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

## <a name="get-information-about-pipelines-and-recurrences"></a><span data-ttu-id="0d8c1-187">Adatcsatornák és ismétlődések adatainak beolvasása</span><span class="sxs-lookup"><span data-stu-id="0d8c1-187">Get information about pipelines and recurrences</span></span>

<span data-ttu-id="0d8c1-188">Használja a `Get-AdlJobPipeline` csővezeték-információt parancsmag korábban küldött feladatok.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-188">Use the `Get-AdlJobPipeline` cmdlet to see the pipeline information previously submitted jobs.</span></span>

```powershell
$pipelines = Get-AdlJobPipeline -Account $adla

$pipeline = Get-AdlJobPipeline -Account $adla -PipelineId "<pipeline ID>"
```

<span data-ttu-id="0d8c1-189">Használja a `Get-AdlJobRecurrence` parancsmaggal olvassa ismétlődési korábban küldött feladatok.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-189">Use the `Get-AdlJobRecurrence` cmdlet to see the recurrence information for previously submitted jobs.</span></span>

```powershell
$recurrences = Get-AdlJobRecurrence -Account $adla

$recurrence = Get-AdlJobRecurrence -Account $adla -RecurrenceId "<recurrence ID>"
```

## <a name="get-information-about-a-job"></a><span data-ttu-id="0d8c1-190">A feladat adatainak beolvasása</span><span class="sxs-lookup"><span data-stu-id="0d8c1-190">Get information about a job</span></span>

### <a name="get-job-status"></a><span data-ttu-id="0d8c1-191">Feladat állapotának beolvasása</span><span class="sxs-lookup"><span data-stu-id="0d8c1-191">Get job status</span></span>

<span data-ttu-id="0d8c1-192">Kérje le egy adott feladat állapotát.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-192">Get the status of a specific job.</span></span>

```powershell
Get-AdlJob -AccountName $adla -JobId $job.JobId
```

### <a name="examine-the-job-outputs"></a><span data-ttu-id="0d8c1-193">Vizsgálja meg a feladat kimenetének létrehozása</span><span class="sxs-lookup"><span data-stu-id="0d8c1-193">Examine the job outputs</span></span>

<span data-ttu-id="0d8c1-194">Miután a feladat befejeződött, ellenőrizze, hogy léteznek-e a kimeneti fájl a mappában lévő fájlok listáját.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-194">After the job has ended, check if the output file exists by listing the files in a folder.</span></span>

```powershell
Get-AdlStoreChildItem -Account $adls -Path "/"
```

## <a name="manage-running-jobs"></a><span data-ttu-id="0d8c1-195">Futó feladatok kezelése</span><span class="sxs-lookup"><span data-stu-id="0d8c1-195">Manage running jobs</span></span>

### <a name="cancel-a-job"></a><span data-ttu-id="0d8c1-196">Feladatok megszakítása</span><span class="sxs-lookup"><span data-stu-id="0d8c1-196">Cancel a job</span></span>

```powershell
Stop-AdlJob -Account $adls -JobID $jobID
```

### <a name="wait-for-a-job-to-finish"></a><span data-ttu-id="0d8c1-197">Várjon, amíg a feladat befejeződésére</span><span class="sxs-lookup"><span data-stu-id="0d8c1-197">Wait for a job to finish</span></span>

<span data-ttu-id="0d8c1-198">Ismétlődő helyett `Get-AdlAnalyticsJob` amíg a feladat befejeződik, használhatja a `Wait-AdlJob` parancsmag használatával, vagy várja meg a feladat befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-198">Instead of repeating `Get-AdlAnalyticsJob` until a job finishes, you can use the `Wait-AdlJob` cmdlet to wait for the job to end.</span></span>

```powershell
Wait-AdlJob -Account $adla -JobId $job.JobId
```

## <a name="manage-compute-policies"></a><span data-ttu-id="0d8c1-199">Számítási házirendjeinek kezelése</span><span class="sxs-lookup"><span data-stu-id="0d8c1-199">Manage compute policies</span></span>

### <a name="list-existing-compute-policies"></a><span data-ttu-id="0d8c1-200">A meglévő számítási házirendek felsorolása</span><span class="sxs-lookup"><span data-stu-id="0d8c1-200">List existing compute policies</span></span>

<span data-ttu-id="0d8c1-201">A `Get-AdlAnalyticsComputePolicy` parancsmag beolvassa a Data Lake Analytics-fiók számítási házirendekkel kapcsolatos információ.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-201">The `Get-AdlAnalyticsComputePolicy` cmdlet retrieves info about compute policies for a Data Lake Analytics account.</span></span>

```powershell
$policies = Get-AdlAnalyticsComputePolicy -Account $adla
```

### <a name="create-a-compute-policy"></a><span data-ttu-id="0d8c1-202">Számítási házirend létrehozása</span><span class="sxs-lookup"><span data-stu-id="0d8c1-202">Create a compute policy</span></span>

<span data-ttu-id="0d8c1-203">A `New-AdlAnalyticsComputePolicy` parancsmag létrehoz egy új számítási házirendet a Data Lake Analytics-fiók.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-203">The `New-AdlAnalyticsComputePolicy` cmdlet creates a new compute policy for a Data Lake Analytics account.</span></span> <span data-ttu-id="0d8c1-204">Ebben a példában a rendelkezésre álló maximális ausztráliai állítja be a megadott felhasználó 50 és 250 minimális feladat prioritása.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-204">This example sets  the maximum AUs available to the specified user to 50, and the minimum job priority to 250.</span></span>

```powershell
$userObjectId = (Get-AzureRmAdUser -SearchString "garymcdaniel@contoso.com").Id

New-AdlAnalyticsComputePolicy -Account $adla -Name "GaryMcDaniel" -ObjectId $objectId -ObjectType User -MaxDegreeOfParallelismPerJob 50 -MinPriorityPerJob 250
```

## <a name="check-for-the-existence-of-a-file"></a><span data-ttu-id="0d8c1-205">Ellenőrizze, hogy létezik-e a fájl.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-205">Check for the existence of a file.</span></span>

```powershell
Test-AdlStoreItem -Account $adls -Path "/data.csv"
```

## <a name="uploading-and-downloading"></a><span data-ttu-id="0d8c1-206">Feltöltés és letöltés</span><span class="sxs-lookup"><span data-stu-id="0d8c1-206">Uploading and downloading</span></span>

<span data-ttu-id="0d8c1-207">Fájl feltöltése.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-207">Upload a file.</span></span>

```powershell
Import-AdlStoreItem -AccountName $adls -Path "c:\data.tsv" -Destination "/data_copy.csv" 
```

<span data-ttu-id="0d8c1-208">Töltsön fel egy teljes mappát rekurzív módon.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-208">Upload an entire folder recursively.</span></span>

```powershell
Import-AdlStoreItem -AccountName $adls -Path "c:\myData\" -Destination "/myData/" -Recurse
```

<span data-ttu-id="0d8c1-209">Töltse le a fájlt.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-209">Download a file.</span></span>

```powershell
Export-AdlStoreItem -AccountName $adls -Path "/data.csv" -Destination "c:\data.csv"
```

<span data-ttu-id="0d8c1-210">Töltse le egy teljes mappát rekurzív módon.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-210">Download an entire folder recursively.</span></span>

```powershell
Export-AdlStoreItem -AccountName $adls -Path "/" -Destination "c:\myData\" -Recurse
```

> [!NOTE]
> <span data-ttu-id="0d8c1-211">Ha a feltöltés és letöltés választható folyamat megszakad, esetleg az folytatásához újra a parancsmagot a ``-Resume`` jelzőt.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-211">If the upload or download process is interrupted, you can attempt to resume the process by running the cmdlet again with the ``-Resume`` flag.</span></span>

## <a name="manage-catalog-items"></a><span data-ttu-id="0d8c1-212">A szolgáltatáskatalógusban található elemek kezelése</span><span class="sxs-lookup"><span data-stu-id="0d8c1-212">Manage catalog items</span></span>

<span data-ttu-id="0d8c1-213">A U-SQL-katalógusról data és kód felépítését, így azok megoszthatók U-SQL-parancsfájlok szolgál.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-213">The U-SQL catalog is used to structure data and code so they can be shared by U-SQL scripts.</span></span> <span data-ttu-id="0d8c1-214">A katalógus lehetővé teszi, hogy a lehető legjobb teljesítmény szolgálja Azure Data Lake-adatokkal.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-214">The catalog enables the highest performance possible with data in Azure Data Lake.</span></span> <span data-ttu-id="0d8c1-215">További információk: [Use U-SQL catalog](data-lake-analytics-use-u-sql-catalog.md) (U-SQL-katalógus használata).</span><span class="sxs-lookup"><span data-stu-id="0d8c1-215">For more information, see [Use U-SQL catalog](data-lake-analytics-use-u-sql-catalog.md).</span></span>

### <a name="list-items-in-the-u-sql-catalog"></a><span data-ttu-id="0d8c1-216">A U-SQL catalog listaelemeit.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-216">List items in the U-SQL catalog</span></span>

```powershell
# List U-SQL databases
Get-AdlCatalogItem -Account $adla -ItemType Database 

# List tables within a database
Get-AdlCatalogItem -Account $adla -ItemType Table -Path "database"

# List tables within a schema.
Get-AdlCatalogItem -Account $adla -ItemType Table -Path "database.schema"
```

<span data-ttu-id="0d8c1-217">A ADLA fiók összes adatbázisát a szerelvények listázása.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-217">List all the assemblies in all the databases in an ADLA Account.</span></span>

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

### <a name="get-details-about-a-catalog-item"></a><span data-ttu-id="0d8c1-218">Részletes információkat szolgáltatva katalóguselemek</span><span class="sxs-lookup"><span data-stu-id="0d8c1-218">Get details about a catalog item</span></span>

```powershell
# Get details of a table
Get-AdlCatalogItem  -Account $adla -ItemType Table -Path "master.dbo.mytable"

# Test existence of a U-SQL database.
Test-AdlCatalogItem  -Account $adla -ItemType Database -Path "master"
```

### <a name="create-credentials-in-a-catalog"></a><span data-ttu-id="0d8c1-219">Hitelesítő adatok létrehozása a katalógusban</span><span class="sxs-lookup"><span data-stu-id="0d8c1-219">Create credentials in a catalog</span></span>

<span data-ttu-id="0d8c1-220">U-SQL-adatbázis hozzon létre egy Azure-ban üzemeltetett adatbázis hitelesítő objektumot.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-220">Within a U-SQL database, create a credential object for a database hosted in Azure.</span></span> <span data-ttu-id="0d8c1-221">U-SQL hitelesítő adatok jelenleg csak az ilyen típusú elemet, amely a PowerShell segítségével is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-221">Currently, U-SQL credentials are the only type of catalog item that you can create through PowerShell.</span></span>

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

### <a name="get-basic-information-about-an-adla-account"></a><span data-ttu-id="0d8c1-222">ADLA fiók alapszintű adatainak beolvasása</span><span class="sxs-lookup"><span data-stu-id="0d8c1-222">Get basic information about an ADLA account</span></span>

<span data-ttu-id="0d8c1-223">Megadott fióknév, az alábbi kód megkeresi a fiókkal kapcsolatos alapvető információk</span><span class="sxs-lookup"><span data-stu-id="0d8c1-223">Given an account name, the following code looks up basic information about the account</span></span>

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

## <a name="working-with-azure"></a><span data-ttu-id="0d8c1-224">Azure használata</span><span class="sxs-lookup"><span data-stu-id="0d8c1-224">Working with Azure</span></span>

### <a name="get-details-of-azurerm-errors"></a><span data-ttu-id="0d8c1-225">Részletes AzureRm hibák</span><span class="sxs-lookup"><span data-stu-id="0d8c1-225">Get details of AzureRm errors</span></span>

```powershell
Resolve-AzureRmError -Last
```

### <a name="verify-if-you-are-running-as-an-administrator"></a><span data-ttu-id="0d8c1-226">Győződjön meg arról, ha rendszergazdaként futtatja</span><span class="sxs-lookup"><span data-stu-id="0d8c1-226">Verify if you are running as an administrator</span></span>

```powershell
function Test-Administrator  
{  
    $user = [Security.Principal.WindowsIdentity]::GetCurrent();
    $p = New-Object Security.Principal.WindowsPrincipal $user
    $p.IsInRole([Security.Principal.WindowsBuiltinRole]::Administrator)  
}
```

### <a name="find-a-tenantid"></a><span data-ttu-id="0d8c1-227">A TenantID keresése</span><span class="sxs-lookup"><span data-stu-id="0d8c1-227">Find a TenantID</span></span>

<span data-ttu-id="0d8c1-228">Előfizetés neve:</span><span class="sxs-lookup"><span data-stu-id="0d8c1-228">From a subscription name:</span></span>

```powershell
function Get-TenantIdFromSubcriptionName( [string] $subname )
{
    $sub = (Get-AzureRmSubscription -SubscriptionName $subname)
    $sub.TenantId
}

Get-TenantIdFromSubcriptionName "ADLTrainingMS"
```

<span data-ttu-id="0d8c1-229">Az előfizetés-azonosító:</span><span class="sxs-lookup"><span data-stu-id="0d8c1-229">From a subscription id:</span></span>

```powershell
function Get-TenantIdFromSubcriptionId( [string] $subid )
{
    $sub = (Get-AzureRmSubscription -SubscriptionId $subid)
    $sub.TenantId
}

$subid = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
Get-TenantIdFromSubcriptionId $subid
```

<span data-ttu-id="0d8c1-230">Például a "contoso.com" tartomány címről</span><span class="sxs-lookup"><span data-stu-id="0d8c1-230">From a domain address such as "contoso.com"</span></span>


```powershell
function Get-TenantIdFromDomain( $domain )
{
    $url = "https://login.windows.net/" + $domain + "/.well-known/openid-configuration"
    return (Invoke-WebRequest $url|ConvertFrom-Json).token_endpoint.Split('/')[3]
}

$domain = "contoso.com"
Get-TenantIdFromDomain $domain
```

### <a name="list-all-your-subscriptions-and-tenant-ids"></a><span data-ttu-id="0d8c1-231">Az előfizetések listán, és a bérlői azonosító</span><span class="sxs-lookup"><span data-stu-id="0d8c1-231">List all your subscriptions and tenant ids</span></span>

```powershell
$subs = Get-AzureRmSubscription
foreach ($sub in $subs)
{
    Write-Host $sub.Name "("  $sub.Id ")"
    Write-Host "`tTenant Id" $sub.TenantId
}
```

## <a name="create-a-data-lake-analytics-account-using-a-template"></a><span data-ttu-id="0d8c1-232">Egy sablon használatával a Data Lake Analytics-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="0d8c1-232">Create a Data Lake Analytics account using a template</span></span>

<span data-ttu-id="0d8c1-233">Egy Azure erőforráscsoport-sablon a következő PowerShell-parancsfájl használatával is használhatja:</span><span class="sxs-lookup"><span data-stu-id="0d8c1-233">You can also use an Azure Resource Group template using the following  PowerShell script:</span></span>

```powershell
$subId = "<Your Azure Subscription ID>"

$rg = "<New Azure Resource Group Name>"
$location = "<Location (such as East US 2)>"
$adls = "<New Data Lake Store Account Name>"
$adla = "<New Data Lake Analytics Account Name>"

$deploymentName = "MyDataLakeAnalyticsDeployment"
$armTemplateFile = "<LocalFolderPath>\azuredeploy.json"  # update the JSON template path 

# Log in to Azure
Login-AzureRmAccount -SubscriptionId $subId

# Create the resource group
New-AzureRmResourceGroup -Name $rg -Location $location

# Create the Data Lake Analytics account with the default Data Lake Store account.
$parameters = @{"adlAnalyticsName"=$adla; "adlStoreName"=$adls}
New-AzureRmResourceGroupDeployment -Name $deploymentName -ResourceGroupName $rg -TemplateFile $armTemplateFile -TemplateParameterObject $parameters 
```

<span data-ttu-id="0d8c1-234">További információkért lásd: [Azure Resource Manager-sablon az alkalmazás központi telepítését](../azure-resource-manager/resource-group-template-deploy.md) és [Azure Resource Manager-sablonok készítése](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="0d8c1-234">For more information, see [Deploy an application with Azure Resource Manager template](../azure-resource-manager/resource-group-template-deploy.md) and [Authoring Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

<span data-ttu-id="0d8c1-235">**Példa sablon**</span><span class="sxs-lookup"><span data-stu-id="0d8c1-235">**Example template**</span></span>

<span data-ttu-id="0d8c1-236">Mentse a következő szöveget a `.json` fájlt, és az előző PowerShell-parancsfájl segítségével használhatja a sablont.</span><span class="sxs-lookup"><span data-stu-id="0d8c1-236">Save the following text as a `.json` file, and then use the preceding PowerShell script to use the template.</span></span> 

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "adlAnalyticsName": {
      "type": "string",
      "metadata": {
        "description": "The name of the Data Lake Analytics account to create."
      }
    },
    "adlStoreName": {
      "type": "string",
      "metadata": {
        "description": "The name of the Data Lake Store account to create."
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

## <a name="next-steps"></a><span data-ttu-id="0d8c1-237">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0d8c1-237">Next steps</span></span>
* [<span data-ttu-id="0d8c1-238">A Microsoft Azure Data Lake Analytics áttekintése</span><span class="sxs-lookup"><span data-stu-id="0d8c1-238">Overview of Microsoft Azure Data Lake Analytics</span></span>](data-lake-analytics-overview.md)
* <span data-ttu-id="0d8c1-239">A Data Lake Analytics használatának első lépései [Azure-portálon](data-lake-analytics-get-started-portal.md) | [Azure PowerShell](data-lake-analytics-get-started-powershell.md) | [CLI 2.0](data-lake-analytics-get-started-cli2.md)</span><span class="sxs-lookup"><span data-stu-id="0d8c1-239">Get started with Data Lake Analytics using [Azure portal](data-lake-analytics-get-started-portal.md) | [Azure PowerShell](data-lake-analytics-get-started-powershell.md) | [CLI 2.0](data-lake-analytics-get-started-cli2.md)</span></span>
* <span data-ttu-id="0d8c1-240">Azure Data Lake Analytics használatának kezelése [Azure-portálon](data-lake-analytics-manage-use-portal.md) | [Azure PowerShell](data-lake-analytics-manage-use-powershell.md) | [parancssori felület](data-lake-analytics-manage-use-cli.md)</span><span class="sxs-lookup"><span data-stu-id="0d8c1-240">Manage Azure Data Lake Analytics using [Azure portal](data-lake-analytics-manage-use-portal.md) | [Azure PowerShell](data-lake-analytics-manage-use-powershell.md) | [CLI](data-lake-analytics-manage-use-cli.md)</span></span> 
