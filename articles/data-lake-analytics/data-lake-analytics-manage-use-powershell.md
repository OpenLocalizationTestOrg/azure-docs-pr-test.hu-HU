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
# <a name="manage-azure-data-lake-analytics-using-azure-powershell"></a>Az Azure Data Lake Analytics kezelése az Azure PowerShell-lel
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Ismerje meg, hogyan toomanage Azure Data Lake Analytics-fiókok, adatforrások, feladatok, és az Azure PowerShell használatával a szolgáltatáskatalógusban található elemek. 

## <a name="prerequisites"></a>Előfeltételek

Data Lake Analytics-fiók létrehozásakor tooknow szüksége:

* **Előfizetés-azonosító**: hello Azure előfizetés-azonosító, amely alatt a Data Lake Analytics-fiók található.
* **Erőforráscsoport**: hello a Data Lake Analytics-fiókot tartalmazó hello Azure erőforráscsoport nevét.
* **A Data Lake Analytics-fiók neve**: hello fiók neve csak kisbetűket és számokat tartalmazhat.
* **Alapértelmezett Data Lake Store-fiók:** minden Data Lake Analytics-fiókhoz tartozik egy alapértelmezett Data Lake Store-fiók. Ezek a fiókok hello kell lennie ugyanazon a helyen.
* **Hely**: hello hely Data Lake Analytics-fiókja, például az "USA keleti régiója 2", vagy más támogatott helyeket. Támogatott helyek láthatóak a [árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/data-lake-analytics/).

Ebben az oktatóanyagban hello PowerShell kódtöredékek ezeket az információkat használja a változók toostore

```powershell
$subId = "<SubscriptionId>"
$rg = "<ResourceGroupName>"
$adla = "<DataLakeAnalyticsAccountName>"
$adls = "<DataLakeStoreAccountName>"
$location = "<Location>"
```

## <a name="log-in"></a>Bejelentkezés

Jelentkezzen be egy előfizetési azonosító használatával.

```powershell
Login-AzureRmAccount -SubscriptionId $subId
```

Jelentkezzen be egy előfizetés nevét.

```
Login-AzureRmAccount -SubscriptionName $subname 
```

Hello `Login-AzureRmAccount` parancsmag mindig elkéri a hitelesítő adatokat. Meg kell adni a következő parancsmagok hello használatával elkerülheti a:

```powershell
# Save login session information
Save-AzureRmProfile -Path D:\profile.json  

# Load login session information
Select-AzureRmProfile -Path D:\profile.json 
```

## <a name="managing-accounts"></a>Fiókok kezelése

### <a name="create-a-data-lake-analytics-account"></a>Data Lake Analytics-fiók létrehozása

Ha még nem rendelkezik egy [erőforráscsoport](../azure-resource-manager/resource-group-overview.md#resource-groups) toouse, hozzon létre egyet. 

```powershell
New-AzureRmResourceGroup -Name  $rg -Location $location
```

A naplók tárolásához minden Data Lake Analytics-fiók esetében szükség van egy alapértelmezett Data Lake Store-fiókra. Egy meglévő fiókot újra, vagy hozzon létre egy fiókot. 

```powershell
New-AdlStore -ResourceGroupName $rg -Name $adls -Location $location
```

Ha az erőforráscsoport és a Data Lake Store-fiók már elérhető, hozzon létre egy Data Lake Analytics-fiókot.

```powershell
New-AdlAnalyticsAccount -ResourceGroupName $rg -Name $adla -Location $location -DefaultDataLake $adls
```

### <a name="get-information-about-an-account"></a>Egy fiók adatainak beolvasása

Ezzel a fiókkal kapcsolatos adatokat.

```powershell
Get-AdlAnalyticsAccount -Name $adla
```

Meghatározott Data Lake Analytics-fiók hello meglétének ellenőrzése. hello parancsmag ad vissza, vagy `True` vagy `False`.

```powershell
Test-AdlAnalyticsAccount -Name $adla
```

Egy adott Data Lake Store-fiók hello meglétének ellenőrzése. hello parancsmag ad vissza, vagy `True` vagy `False`.

```powershell
Test-AdlStoreAccount -Name $adls
```

### <a name="listing-accounts"></a>Fiókok listázása

Lista Data Lake Analytics-fiókok hello aktuális előfizetésen belül.

```powershell
Get-AdlAnalyticsAccount
```

Lista Data Lake Analytics-fiókok egy adott erőforráscsoportban.

```powershell
Get-AdlAnalyticsAccount -ResourceGroupName $rg
```

## <a name="managing-firewall-rules"></a>Tűzfal-szabályok kezelése

Tűzfalszabályok listája.

```powershell
Get-AdlAnalyticsFirewallRule -Account $adla
```

Fel egy tűzfalszabályt.

```powershell
$ruleName = "Allow access from on-prem server"
$startIpAddress = "<start IP address>"
$endIpAddress = "<end IP address>"

Add-AdlAnalyticsFirewallRule -Account $adla -Name $ruleName -StartIpAddress $startIpAddress -EndIpAddress $endIpAddress
```

Egy tűzfalszabály módosítása.

```powershell
Set-AdlAnalyticsFirewallRule -Account $adla -Name $ruleName -StartIpAddress $startIpAddress -EndIpAddress $endIpAddress
```

Egy tűzfalszabály eltávolítása.

```powershell
Remove-AdlAnalyticsFirewallRule -Account $adla -Name $ruleName
```



Azure IP-címek engedélyezése.

```powershell
Set-AdlAnalyticsAccount -Name $adla -AllowAzureIpState Enabled
```

```powershell
Set-AdlAnalyticsAccount -Name $adla -FirewallState Enabled
Set-AdlAnalyticsAccount -Name $adla -FirewallState Disabled
```

## <a name="managing-data-sources"></a>Az adatforrások kezelése
Az Azure Data Lake Analytics jelenleg a következő adatforrások hello támogatja:

* [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md)
* [Azure Storage](../storage/common/storage-introduction.md)

Az Analytics-fiók létrehozásakor ki kell jelölnie egy Data Lake Store fiók toobe hello alapértelmezett adatforrás. hello alapértelmezett Data Lake Store-fiók használatos toostore feladat metaadatok és a feladat naplók. Data Lake Analytics-fiók létrehozása után hozzáadhat további Data Lake Store-fiókok és/vagy a Storage-fiókok. 

### <a name="find-hello-default-data-lake-store-account"></a>Hello alapértelmezett Data Lake Store-fiók található

```powershell
$adla_acct = Get-AdlAnalyticsAccount -Name $adla
$dataLakeStoreName = $adla_acct.DefaultDataLakeAccount
```

Hello alapértelmezett Data Lake Store-fiók találhatja meg adatforrások listája hello hello szerinti szűrés `IsDefault` tulajdonság:

```powershell
Get-AdlAnalyticsDataSource -Account $adla  | ? { $_.IsDefault } 
```

### <a name="add-a-data-source"></a>Egy adatforrás hozzáadása

```powershell

# Add an additional Storage (Blob) account.
$AzureStorageAccountName = "<AzureStorageAccountName>"
$AzureStorageAccountKey = "<AzureStorageAccountKey>"
Add-AdlAnalyticsDataSource -Account $adla -Blob $AzureStorageAccountName -AccessKey $AzureStorageAccountKey

# Add an additional Data Lake Store account.
$AzureDataLakeStoreName = "<AzureDataLakeStoreAccountName"
Add-AdlAnalyticsDataSource -Account $adla -DataLakeStore $AzureDataLakeStoreName 
```

### <a name="list-data-sources"></a>Az adatforrások listája

```powershell
# List all hello data sources
Get-AdlAnalyticsDataSource -Name $adla

# List attached Data Lake Store accounts
Get-AdlAnalyticsDataSource -Name $adla | where -Property Type -EQ "DataLakeStore"

# List attached Storage accounts
Get-AdlAnalyticsDataSource -Name $adla | where -Property Type -EQ "Blob"
```

## <a name="submit-u-sql-jobs"></a>U-SQL-feladatok küldése

### <a name="submit-a-string-as-a-u-sql-script"></a>Küldje el egy karakterlánc, egy U-SQL-parancsfájl

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


### <a name="submit-a-file-as-a-u-sql-script"></a>Küldje el a fájlt egy U-SQL-parancsfájl

```powershell
$scriptpath = "d:\test.usql"
$script | Out-File $scriptpath 
Submit-AdlJob -AccountName $adla –ScriptPath $scriptpath -Name "Demo"
```

## <a name="list-jobs-in-an-account"></a>Egy fiók feladatok listája

### <a name="list-all-hello-jobs-in-hello-account"></a>Az összes hello feladat hello fiók felsorolása. 

hello kimenete hello jelenleg futó feladatok, és ezeket a feladatokat, amelyek nemrégiben fejeződtek be.

```powershell
Get-AdlJob -Account $adla
```


### <a name="list-a-specific-number-of-jobs"></a>Egy bizonyos számú feladatok listázása

Alapértelmezés szerint a feladatok hello lista rendezett küldésre idő. Így a legutóbb elküldött hello feladatok első jelennek meg. Alapértelmezés szerint hello ADLA fiók feladatok emlékszik 180 napig tart, de csak a hello első 500 hello Ge-AdlJob parancsmag által alapértelmezett értéket ad vissza. Használja az - felső paraméter toolist adott számú feladatot.

```powershell
$jobs = Get-AdlJob -Account $adla -Top 10
```


### <a name="list-jobs-based-on-hello-value-of-job-property"></a>Lista feladatok hello feladat tulajdonság értéke alapján

Hello segítségével `-State` paraméter. Ezek bármelyike kombinálhatja:

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

Használjon hello `-Result` paraméter toodetect hogy befejeződött a feladat sikeresen befejeződött-e. Ezekkel az értékekkel rendelkezik:

* Megszakítva
* Nem sikerült
* None
* Sikeres

``` powershell
# List Successful jobs.
Get-AdlJob -Account $adla -State Ended -Result Succeeded

# List Failed jobs.
Get-AdlJob -Account $adla -State Ended -Result Failed
```


Hello `-Submitter` paraméter segít azonosítani, ki egy feladat elküldése megtörtént.

```powershell
Get-AdlJob -Account $adla -Submitter "joe@contoso.com"
```

Hello `-SubmittedAfter` hasznos tooa időtartomány szűrést.


```powershell
# List  jobs submitted in hello last day.
$d = [DateTime]::Now.AddDays(-1)
Get-AdlJob -Account $adla -SubmittedAfter $d

# List  jobs submitted in hello last seven day.
$d = [DateTime]::Now.AddDays(-7)
Get-AdlJob -Account $adla -SubmittedAfter $d
```

### <a name="common-scenarios-for-listing-jobs"></a>Általános példák a feladatok listázása


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

## <a name="filtering-a-list-of-jobs"></a>A feladatok listájának szűrése

Követően a feladatok listáját a jelenlegi PowerShell-munkamenetben. Használhatja a szokásos PowerShell parancsmagok toofilter hello listája.

Szűrő feladatok toohello feladatokra hello utolsó 24 órában küldött

```
$upperdate = Get-Date
$lowerdate = $upperdate.AddHours(-24)
$jobs | Where-Object { $_.EndTime -ge $lowerdate }
```

Véget ért hello az elmúlt 24 órában feladatok toohello feladatok listájának szűrése

```
$upperdate = Get-Date
$lowerdate = $upperdate.AddHours(-24)
$jobs | Where-Object { $_.SubmitTime -ge $lowerdate }
```

Szűrheti a feladatok elindításának toohello feladatok listáját. Egy feladat sikertelen lehet a fordítás során –, és ezért soha nem kezdődik. Nézzük hello sikertelen feladatok ténylegesen elindításának, majd nem sikerült.

```powershell
$jobs | Where-Object { $_.StartTime -ne $null }
```

### <a name="analyzing-a-list-of-jobs"></a>Feladatokra elemzése

Használjon hello `Group-Object` parancsmag tooanalyze feladatokra.

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
Ha az elemzések végrehajtását, hasznos tooadd tulajdonságok toohello feladat objektumok toomake szűrést és a csoportosítási egyszerűbb lehet. a következő kódrészletet hello jeleníti meg, hogyan tooannotate a Feladatinformáció a számított tulajdonságokat.

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

## <a name="get-information-about-pipelines-and-recurrences"></a>Adatcsatornák és ismétlődések adatainak beolvasása

Használjon hello `Get-AdlJobPipeline` parancsmag toosee hello csővezeték korábban információkat feladatok.

```powershell
$pipelines = Get-AdlJobPipeline -Account $adla

$pipeline = Get-AdlJobPipeline -Account $adla -PipelineId "<pipeline ID>"
```

Használjon hello `Get-AdlJobRecurrence` parancsmag toosee hello ismétlődési információkat az előzőleg elküldött feladatok.

```powershell
$recurrences = Get-AdlJobRecurrence -Account $adla

$recurrence = Get-AdlJobRecurrence -Account $adla -RecurrenceId "<recurrence ID>"
```

## <a name="get-information-about-a-job"></a>A feladat adatainak beolvasása

### <a name="get-job-status"></a>Feladat állapotának beolvasása

Egy adott feladat hello állapotának beolvasása.

```powershell
Get-AdlJob -AccountName $adla -JobId $job.JobId
```

### <a name="examine-hello-job-outputs"></a>Vizsgálja meg a hello feladat kimenetének létrehozása

Miután hello feladat befejeződött, ellenőrizze, hogy léteznek-e hello kimeneti fájl hello mappában lévő fájlok listáját.

```powershell
Get-AdlStoreChildItem -Account $adls -Path "/"
```

## <a name="manage-running-jobs"></a>Futó feladatok kezelése

### <a name="cancel-a-job"></a>Feladatok megszakítása

```powershell
Stop-AdlJob -Account $adls -JobID $jobID
```

### <a name="wait-for-a-job-toofinish"></a>Várjon, amíg a feladat toofinish

Ismétlődő helyett `Get-AdlAnalyticsJob` egy feladat befejezéséig hello használhatja `Wait-AdlJob` parancsmag toowait hello feladat tooend számára.

```powershell
Wait-AdlJob -Account $adla -JobId $job.JobId
```

## <a name="manage-compute-policies"></a>Számítási házirendjeinek kezelése

### <a name="list-existing-compute-policies"></a>A meglévő számítási házirendek felsorolása

Hello `Get-AdlAnalyticsComputePolicy` parancsmag beolvassa a Data Lake Analytics-fiók számítási házirendekkel kapcsolatos információ.

```powershell
$policies = Get-AdlAnalyticsComputePolicy -Account $adla
```

### <a name="create-a-compute-policy"></a>Számítási házirend létrehozása

Hello `New-AdlAnalyticsComputePolicy` parancsmag létrehoz egy új számítási házirendet a Data Lake Analytics-fiók. Ez a példa beállítása hello maximális ausztráliai elérhető toohello megadott felhasználói too50 és hello minimális feladat prioritása too250.

```powershell
$userObjectId = (Get-AzureRmAdUser -SearchString "garymcdaniel@contoso.com").Id

New-AdlAnalyticsComputePolicy -Account $adla -Name "GaryMcDaniel" -ObjectId $objectId -ObjectType User -MaxDegreeOfParallelismPerJob 50 -MinPriorityPerJob 250
```

## <a name="check-for-hello-existence-of-a-file"></a>Ellenőrizze, hogy egy fájl hello megléte.

```powershell
Test-AdlStoreItem -Account $adls -Path "/data.csv"
```

## <a name="uploading-and-downloading"></a>Feltöltés és letöltés

Fájl feltöltése.

```powershell
Import-AdlStoreItem -AccountName $adls -Path "c:\data.tsv" -Destination "/data_copy.csv" 
```

Töltsön fel egy teljes mappát rekurzív módon.

```powershell
Import-AdlStoreItem -AccountName $adls -Path "c:\myData\" -Destination "/myData/" -Recurse
```

Töltse le a fájlt.

```powershell
Export-AdlStoreItem -AccountName $adls -Path "/data.csv" -Destination "c:\data.csv"
```

Töltse le egy teljes mappát rekurzív módon.

```powershell
Export-AdlStoreItem -AccountName $adls -Path "/" -Destination "c:\myData\" -Recurse
```

> [!NOTE]
> Ha hello feltöltése vagy letöltési folyamat megszakad, megkísérelheti tooresume hello folyamat futó hello parancsmag történő újra hello ``-Resume`` jelzőt.

## <a name="manage-catalog-items"></a>A szolgáltatáskatalógusban található elemek kezelése

hello U-SQL catalog használt toostructure adatok és a kód, így azok megoszthatók U-SQL-parancsfájlok. hello katalógus lehetővé teszi, hogy a lehető legjobb teljesítmény hello Azure Data Lake-adatokkal szolgálja. További információk: [Use U-SQL catalog](data-lake-analytics-use-u-sql-catalog.md) (U-SQL-katalógus használata).

### <a name="list-items-in-hello-u-sql-catalog"></a>Hello U-SQL catalog listaelemeit.

```powershell
# List U-SQL databases
Get-AdlCatalogItem -Account $adla -ItemType Database 

# List tables within a database
Get-AdlCatalogItem -Account $adla -ItemType Table -Path "database"

# List tables within a schema.
Get-AdlCatalogItem -Account $adla -ItemType Table -Path "database.schema"
```

Listázza az összes hello adatbázisának ADLA fiók összes hello szerelvényeket.

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

### <a name="get-details-about-a-catalog-item"></a>Részletes információkat szolgáltatva katalóguselemek

```powershell
# Get details of a table
Get-AdlCatalogItem  -Account $adla -ItemType Table -Path "master.dbo.mytable"

# Test existence of a U-SQL database.
Test-AdlCatalogItem  -Account $adla -ItemType Database -Path "master"
```

### <a name="create-credentials-in-a-catalog"></a>Hitelesítő adatok létrehozása a katalógusban

U-SQL-adatbázis hozzon létre egy Azure-ban üzemeltetett adatbázis hitelesítő objektumot. U-SQL hitelesítő adatok jelenleg hello csak ilyen típusú elemet, amely a PowerShell segítségével is létrehozhat.

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

### <a name="get-basic-information-about-an-adla-account"></a>ADLA fiók alapszintű adatainak beolvasása

A megadott fióknév a következő kód hello keresi a hello fiókkal kapcsolatos alapvető információk

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

## <a name="working-with-azure"></a>Azure használata

### <a name="get-details-of-azurerm-errors"></a>Részletes AzureRm hibák

```powershell
Resolve-AzureRmError -Last
```

### <a name="verify-if-you-are-running-as-an-administrator"></a>Győződjön meg arról, ha rendszergazdaként futtatja

```powershell
function Test-Administrator  
{  
    $user = [Security.Principal.WindowsIdentity]::GetCurrent();
    $p = New-Object Security.Principal.WindowsPrincipal $user
    $p.IsInRole([Security.Principal.WindowsBuiltinRole]::Administrator)  
}
```

### <a name="find-a-tenantid"></a>A TenantID keresése

Előfizetés neve:

```powershell
function Get-TenantIdFromSubcriptionName( [string] $subname )
{
    $sub = (Get-AzureRmSubscription -SubscriptionName $subname)
    $sub.TenantId
}

Get-TenantIdFromSubcriptionName "ADLTrainingMS"
```

Az előfizetés-azonosító:

```powershell
function Get-TenantIdFromSubcriptionId( [string] $subid )
{
    $sub = (Get-AzureRmSubscription -SubscriptionId $subid)
    $sub.TenantId
}

$subid = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
Get-TenantIdFromSubcriptionId $subid
```

Például a "contoso.com" tartomány címről


```powershell
function Get-TenantIdFromDomain( $domain )
{
    $url = "https://login.windows.net/" + $domain + "/.well-known/openid-configuration"
    return (Invoke-WebRequest $url|ConvertFrom-Json).token_endpoint.Split('/')[3]
}

$domain = "contoso.com"
Get-TenantIdFromDomain $domain
```

### <a name="list-all-your-subscriptions-and-tenant-ids"></a>Az előfizetések listán, és a bérlői azonosító

```powershell
$subs = Get-AzureRmSubscription
foreach ($sub in $subs)
{
    Write-Host $sub.Name "("  $sub.Id ")"
    Write-Host "`tTenant Id" $sub.TenantId
}
```

## <a name="create-a-data-lake-analytics-account-using-a-template"></a>Egy sablon használatával a Data Lake Analytics-fiók létrehozása

Egy Azure erőforráscsoport-sablon használatával a következő PowerShell-parancsfájl hello is használhatja:

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

További információkért lásd: [Azure Resource Manager-sablon az alkalmazás központi telepítését](../azure-resource-manager/resource-group-template-deploy.md) és [Azure Resource Manager-sablonok készítése](../azure-resource-manager/resource-group-authoring-templates.md).

**Példa sablon**

A következő szöveg hello menteni egy `.json` fájlt, és a PowerShell parancsfájl toouse hello sablon megelőző hello használja. 

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

## <a name="next-steps"></a>Következő lépések
* [A Microsoft Azure Data Lake Analytics áttekintése](data-lake-analytics-overview.md)
* A Data Lake Analytics használatának első lépései [Azure-portálon](data-lake-analytics-get-started-portal.md) | [Azure PowerShell](data-lake-analytics-get-started-powershell.md) | [CLI 2.0](data-lake-analytics-get-started-cli2.md)
* Azure Data Lake Analytics használatának kezelése [Azure-portálon](data-lake-analytics-manage-use-portal.md) | [Azure PowerShell](data-lake-analytics-manage-use-powershell.md) | [parancssori felület](data-lake-analytics-manage-use-cli.md) 
