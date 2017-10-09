---
title: "Azure Data Lake Analytics Azure .NET SDK használatával aaaManage |} Microsoft Docs"
description: "Ismerje meg, hogyan toomanage Data Lake Analytics feladatok, adatforrások, felhasználók. "
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.assetid: 811d172d-9873-4ce9-a6d5-c1a26b374c79
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/18/2017
ms.author: saveenr
ms.openlocfilehash: 98630ba411823644a8bce1f1b0c1331f689cbb0c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-data-lake-analytics-using-azure-net-sdk"></a>Azure Data Lake Analytics Azure .NET SDK használatával kezelése
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Ismerje meg, hogyan toomanage Azure Data Lake Analytics-fiókok, adatforrások, felhasználók és használó feladatok hello Azure .NET SDK-t. 

## <a name="prerequisites"></a>Előfeltételek

* **Visual Studio 2015, Visual Studio 2013 4. frissítéssel vagy Visual Studio 2012 és telepített Visual C++**.
* **Microsoft Azure SDK for .NET 2.5-ös vagy újabb verzió**.  Telepítse hello [webplatform-telepítő](http://www.microsoft.com/web/downloads/platform.aspx).
* **Szükséges NuGet-csomagok**

### <a name="install-nuget-packages"></a>NuGet-csomagok telepítése

|Csomag|Verzió|
|-------|-------|
|[Microsoft.Rest.ClientRuntime.Azure.Authentication](https://www.nuget.org/packages/Microsoft.Rest.ClientRuntime.Azure.Authentication)| 2.3.1|
|[Microsoft.Azure.Management.DataLake.Analytics](https://www.nuget.org/packages/Microsoft.Azure.Management.DataLake.Analytics)|3.0.0|
|[Microsoft.Azure.Management.DataLake.Store](https://www.nuget.org/packages/Microsoft.Azure.Management.DataLake.Store)|2.2.0|
|[Microsoft.Azure.Management.ResourceManager](https://www.nuget.org/packages/Microsoft.Azure.Management.ResourceManager)|1.6.0-Preview|
|[Microsoft.Azure.Graph.RBAC](https://www.nuget.org/packages/Microsoft.Azure.Management.ResourceManager)|3.4.0-Preview|

A következő parancsok hello telepítheti ezeket a csomagokat hello NuGet parancssorból:

```
Install-Package -Id Microsoft.Rest.ClientRuntime.Azure.Authentication  -Version 2.3.1
Install-Package -Id Microsoft.Azure.Management.DataLake.Analytics  -Version 3.0.0
Install-Package -Id Microsoft.Azure.Management.DataLake.Store  -Version 2.2.0
Install-Package -Id Microsoft.Azure.Management.ResourceManager  -Version 1.6.0-preview
Install-Package -Id Microsoft.Azure.Graph.RBAC -Version 3.4.0-preview
```

## <a name="common-variables"></a>Közös változók

``` csharp
string subid = "<Subscription ID>"; // Subscription ID (a GUID)
string tenantid = "<Tenant ID>"; // AAD tenant ID or domain. For example, "contoso.onmicrosoft.com"
string rg == "<value>"; // Resource  group name
string clientid = "1950a258-227b-4e31-a9cf-717495945fc2"; // Sample client ID (this will work, but you should pick your own)
```

## <a name="authentication"></a>Authentication

A Data Lake Analytics tooAzure bejelentkezés több lehetősége van. hello alábbi kódrészletet mutat példát hitelesítési egy előugró ablak az interaktív felhasználói hitelesítéssel.

``` csharp
using System;
using System.IO;
using System.Threading;
using System.Security.Cryptography.X509Certificates;

using Microsoft.Rest;
using Microsoft.Rest.Azure.Authentication;
using Microsoft.Azure.Management.DataLake.Analytics;
using Microsoft.Azure.Management.DataLake.Analytics.Models;
using Microsoft.Azure.Management.DataLake.Store;
using Microsoft.Azure.Management.DataLake.Store.Models;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using Microsoft.Azure.Graph.RBAC;

public static Program
{
   public static string TENANT = "microsoft.onmicrosoft.com";
   public static string CLIENTID = "1950a258-227b-4e31-a9cf-717495945fc2";
   public static System.Uri ARM_TOKEN_AUDIENCE = new System.Uri( @"https://management.core.windows.net/");
   public static System.Uri ADL_TOKEN_AUDIENCE = new System.Uri( @"https://datalake.azure.net/" );
   public static System.Uri GRAPH_TOKEN_AUDIENCE = new System.Uri( @"https://graph.windows.net/" );

   static void Main(string[] args)
   {
      string MY_DOCUMENTS= System.Environment.GetFolderPath( System.Environment.SpecialFolder.MyDocuments);
      string TOKEN_CACHE_PATH = System.IO.Path.Combine(MY_DOCUMENTS, "my.tokencache");

      var tokenCache = GetTokenCache(TOKEN_CACHE_PATH);
      var armCreds = GetCreds_User_Popup(TENANT, ARM_TOKEN_AUDIENCE, CLIENTID, tokenCache);
      var adlCreds = GetCreds_User_Popup(TENANT, ADL_TOKEN_AUDIENCE, CLIENTID, tokenCache);
      var graphCreds = GetCreds_User_Popup(TENANT, GRAPH_TOKEN_AUDIENCE, CLIENTID, tokenCache);
   }
}
```

hello forráskódja **GetCreds_User_Popup** és hello kód számára más beállításokat a hitelesítési ismertetnek [Data Lake Analytics .NET-hitelesítés beállításai](https://github.com/Azure-Samples/data-lake-analytics-dotnet-auth-options)


## <a name="create-hello-client-management-objects"></a>Hello ügyfél létrehozása kezelési objektumok

``` csharp
var resourceManagementClient = new ResourceManagementClient(armCreds) { SubscriptionId = subid };

var adlaAccountClient = new DataLakeAnalyticsAccountManagementClient(armCreds);
adlaAccountClient.SubscriptionId = subid;

var adlsAccountClient = new DataLakeStoreAccountManagementClient(armCreds);
adlsAccountClient.SubscriptionId = subid;

var adlaCatalogClient = new DataLakeAnalyticsCatalogManagementClient(adlCreds);
var adlaJobClient = new DataLakeAnalyticsJobManagementClient(adlCreds);

var adlsFileSystemClient = new DataLakeStoreFileSystemManagementClient(adlCreds);

var  graphClient = new GraphRbacManagementClient(graphCreds);
graphClient.TenantID = domain;

```

## <a name="manage-accounts"></a>Fiókok kezelése

### <a name="create-an-azure-resource-group"></a>Azure-erőforráscsoport létrehozása

Ha még nem már létrehozott egy, a Data Lake Analytics-összetevők az Azure-erőforráscsoport toocreate kell rendelkeznie. A hitelesítő adatok, előfizetés-azonosító és egy helyre van szükség. a következő kód bemutatja hogyan hello toocreate erőforráscsoport:

``` csharp
var resourceGroup = new ResourceGroup { Location = location };
resourceManagementClient.ResourceGroups.CreateOrUpdate(groupName, rg);
```
További információkért lásd: [Azure erőforráscsoport-sablonok és a Data Lake Analytics](#Azure-Resource-Groups-and-Data-Lake-Analytics).

### <a name="create-a-data-lake-store-account"></a>Data Lake Store-fiók létrehozása

Legalább egyszer a ADLA fiók egy ADLS-fiók szükséges. Ha még nem rendelkezik egy toouse, létrehozhat egyet a következő kód hello:

``` csharp
var new_adls_params = new DataLakeStoreAccount(location: _location);
adlsAccountClient.Account.Create(rg, adls, new_adls_params);
```

### <a name="create-a-data-lake-analytics-account"></a>Data Lake Analytics-fiók létrehozása

a következő kód hello létrehoz egy ADLS-fiók

``` csharp
var new_adla_params = new DataLakeAnalyticsAccount()
{
   DefaultDataLakeStoreAccount = adls,
   Location = location
};

adlaClient.Account.Create(rg, adla, new_adla_params);
```

### <a name="list-data-lake-store-accounts"></a>Lista Data Lake Store-fiók

``` csharp
var adlsAccounts = adlsAccountClient.Account.List().ToList();
foreach (var adls in adlsAccounts)
{
   Console.WriteLine($"ADLS: {0}", adls.Name);
}
```

### <a name="list-data-lake-analytics-accounts"></a>Lista Data Lake Analytics-fiókok

``` csharp
var adlaAccounts = adlaClient.Account.List().ToList();

for (var adla in AdlaAccounts)
{
   Console.WriteLine($"ADLA: {0}, adla.Name");
}
```

### <a name="checking-if-an-account-exists"></a>Ha létezik-e a fiók ellenőrzése

``` csharp
bool exists = adlaClient.Account.Exists(rg, adla));
```

### <a name="get-information-about-an-account"></a>Egy fiók adatainak beolvasása

``` csharp
bool exists = adlaClient.Account.Exists(rg, adla));
if (exists)
{
   var adla_accnt = adlaClient.Account.Get(rg, adla);
}
```

### <a name="delete-an-account"></a>-Fiók törlése

``` csharp
if (adlaClient.Account.Exists(rg, adla))
{
   adlaClient.Account.Delete(rg, adla);
}
```

### <a name="get-hello-default-data-lake-store-account"></a>Hello alapértelmezett Data Lake Store-fiók beszerzése

Minden Data Lake Analytics-fiókja alapértelmezett Data Lake Store-fiók szükséges. Használja a kód toodetermine hello alapértelmezett tároló fiókot az Analytics-fiók.

``` csharp
if (adlaClient.Account.Exists(rg, adla))
{
  var adla_accnt = adlaClient.Account.Get(rg, adla);
  string def_adls_account = adla_accnt.DefaultDataLakeStoreAccount;
}
```

## <a name="manage-data-sources"></a>Adatforrások kezelése

A Data Lake Analytics jelenleg a következő adatforrások hello támogatja:

* [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md)
* [Azure Storage-fiók](../storage/common/storage-introduction.md)

### <a name="link-tooan-azure-storage-account"></a>Hivatkozás tooan Azure Storage-fiókban

Hivatkozások tooAzure Storage-fiókokat hozhat létre.

``` csharp
string storage_key = "xxxxxxxxxxxxxxxxxxxx";
string storage_account = "mystorageaccount";
var addParams = new AddStorageAccountParameters(storage_key);            
adlaClient.StorageAccounts.Add(rg, adla, storage_account, addParams);
```

### <a name="list-azure-storage-data-sources"></a>Azure Storage adatforrások listája

``` csharp
var stg_accounts = adlaAccountClient.StorageAccounts.ListByAccount(rg, adla);

if (stg_accounts != null)
{
  foreach (var stg_account in stg_accounts)
  {
      Console.WriteLine($"Storage account: {0}", stg_account.Name);
  }
}
```

### <a name="list-data-lake-store-data-sources"></a>Lista Data Lake Store adatforrások

``` csharp
var adls_accounts = adlsClient.Account.List();

if (adls_accounts != null)
{
  foreach (var adls_accnt in adls_accounts)
  {
      Console.WriteLine($"ADLS account: {0}", adls_accnt.Name);
  }
}
```

### <a name="upload-and-download-folders-and-files"></a>Fájlok és mappák le- és feltöltése
Hello Data Lake Store fájl rendszer ügyfél felügyeleti objektum tooupload használja, és töltse le a fájlokat vagy mappákat Azure tooyour helyi számítógépről, a következő módszerek hello használata:

- UploadFolder
- UploadFile
- DownloadFolder
- DownloadFile

Ezek a módszerek hello első paraméter értéke nem hello hello forrás elérési útja és hello célútvonalat paramétereinek követ hello Data Lake Store-fiók nevét.

hello a következő példa bemutatja, hogyan toodownload egy mappát a hello Data Lake Store.

``` csharp
adlsFileSystemClient.FileSystem.DownloadFolder(adls, sourcePath, destinationPath);
```

### <a name="create-a-file-in-a-data-lake-store-account"></a>Hozzon létre egy fájlt egy Data Lake Store-fiókban

``` csharp
using (var memstream = new MemoryStream())
{
   using (var sw = new StreamWriter(memstream, UTF8Encoding.UTF8))
   {
      sw.WriteLine("Hello World");
      sw.Flush();

      adlsFileSystemClient.FileSystem.Create(adls, "/Samples/Output/randombytes.csv", memstream);
   }
}
```

### <a name="verify-azure-storage-account-paths"></a>Ellenőrizze az Azure Storage-fiók elérési utak
hello alábbira ellenőrzi, ha egy Azure Storage-fiók (storageAccntName) szerepel a Data Lake Analytics-fiók (analyticsAccountName), és a tároló (containerName) hello Azure Storage-fiók létezik.

``` csharp
string storage_account = "mystorageaccount";
string storage_container = "mycontainer";
bool accountExists = adlaClient.Account.StorageAccountExists(rg, adla, storage_account));
bool containerExists = adlaClient.Account.StorageContainerExists(rg, adla, storage_account, storage_container));
```

## <a name="manage-catalog-and-jobs"></a>Katalógus és feladatok kezelése
hello DataLakeAnalyticsCatalogManagementClient objektum hello minden Azure Data Lake Analytics-fiók a megadott SQL-adatbázis kezelésére szolgáló módszert biztosít. hello DataLakeAnalyticsJobManagementClient módszerek toosubmit biztosít, és futtassa a U-SQL-parancsfájlok hello adatbázis feladatok kezelése.

### <a name="list-databases-and-schemas"></a>Lista adatbázisok és -sémákat
Hello között több eredményoldalra is listázhatja, leggyakrabban előforduló hello minden olyan adatbázisok és a séma. hello alábbira jut hozzá a adatbázisok gyűjteménye, és az egyes adatbázisok hello séma majd enumerálása.

``` csharp
var databases = adlaCatalogClient.Catalog.ListDatabases(adla);
foreach (var db in databases)
{
  Console.WriteLine($"Database: {db.Name}");
  Console.WriteLine(" - Schemas:");
  var schemas = adlaCatalogClient.Catalog.ListSchemas(adla, db.Name);
  foreach (var schm in schemas)
  {
      Console.WriteLine($"\t{schm.Name}");
  }
}
```

### <a name="list-table-columns"></a>A táblázat oszlopaihoz listája
hello következő kód bemutatja, hogyan tooaccess hello egy Data Lake Analytics-katalógus felügyeleti ügyfél toolist hello a táblázatban levő oszlopkészleteket megadott adatbázishoz.

``` csharp
var tbl = adlaCatalogClient.Catalog.GetTable(adla, "master", "dbo", "MyTableName");
IEnumerable<USqlTableColumn> columns = tbl.ColumnList;

foreach (USqlTableColumn utc in columns)
{
  string scriptPath = "/Samples/Scripts/SearchResults_Wikipedia_Script.txt";
  Stream scriptStrm = adlsFileSystemClient.FileSystem.Open(_adlsAccountName, scriptPath);
  string scriptTxt = string.Empty;
  using (StreamReader sr = new StreamReader(scriptStrm))
  {
      scriptTxt = sr.ReadToEnd();
  }

  var jobName = "SR_Wikipedia";
  var jobId = Guid.NewGuid();
  var properties = new USqlJobProperties(scriptTxt);
  var parameters = new JobInformation(jobName, JobType.USql, properties, priority: 1, degreeOfParallelism: 1, jobId: jobId);
  var jobInfo = adlaJobClient.Job.Create(adla, jobId, parameters);
  Console.WriteLine($"Job {jobName} submitted.");

}
```

### <a name="list-failed-jobs"></a>A sikertelen feladatok listázása
hello alábbira sorolja fel a sikertelen feladatok kapcsolatos információkat.

``` csharp
var odq = new ODataQuery<JobInformation> { Filter = "result eq 'Failed'" };
var jobs = adlaJobClient.Job.List(adla, odq);
foreach (var j in jobs)
{
   Console.WriteLine($"{j.Name}\t{j.JobId}\t{j.Type}\t{j.StartTime}\t{j.EndTime}");
}
```

### <a name="list-pipelines"></a>Lista folyamatok
hello alábbira információ megjelenítése minden folyamatkezelési feladatok elküldött toohello fiók.

``` csharp
var pipelines = adlaJobClient.Pipeline.List(adla);
foreach (var p in pipelines)
{
   Console.WriteLine($"Pipeline: {p.Name}\t{p.PipelineId}\t{p.LastSubmitTime}");
}
```

### <a name="list-recurrences"></a>Lista ismétlődések
hello alábbira információ megjelenítése minden ismétlődési feladatok elküldött toohello fiók.

``` csharp
var recurrences = adlaJobClient.Recurrence.List(adla);
foreach (var r in recurrences)
{
   Console.WriteLine($"Recurrence: {r.Name}\t{r.RecurrenceId}\t{r.LastSubmitTime}");
}
```

## <a name="common-graph-scenarios"></a>Általános graph-forgatókönyvek

### <a name="look-up-user-in-hello-aad-directory"></a>Hello AAD-címtárában lévő felhasználó keresése

``` csharp
var userinfo = graphClient.Users.Get( "bill@contoso.com" );
```

### <a name="get-hello-objectid-of-a-user-in-hello-aad-directory"></a>Egy felhasználó ObjectId hello beolvasása a hello AAD-címtárában

``` csharp
var userinfo = graphClient.Users.Get( "bill@contoso.com" );
Console.WriteLine( userinfo.ObjectId )
```

## <a name="manage-compute-policies"></a>Számítási házirendjeinek kezelése
hello DataLakeAnalyticsAccountManagementClient objektum biztosít hello kezelésére szolgáló módszerek számítási házirendek a Data Lake Analytics-fiók.

### <a name="list-compute-policies"></a>Számítási házirendek felsorolása
hello a következő kód lekéri a Data Lake Analytics-fiók számítási házirendek listáját.

``` csharp
var policies = adlaAccountClient.ComputePolicies.ListByAccount(rg, adla);
foreach (var p in policies)
{
   Console.WriteLine($"Name: {p.Name}\tType: {p.ObjectType}\tMax AUs / job: {p.MaxDegreeOfParallelismPerJob}\tMin priority / job: {p.MinPriorityPerJob}");
}
```

### <a name="create-a-new-compute-policy"></a>Hozzon létre egy új számítási házirendet
a következő kód hello létrehoz egy új számítási házirendet a Data Lake Analytics-fiók, beállítás hello maximális ausztráliai elérhető toohello megadott felhasználói too50, és hello minimális feladat prioritása too250.

``` csharp
var userAadObjectId = "3b097601-4912-4d41-b9d2-78672fc2acde";
var newPolicyParams = new ComputePolicyCreateOrUpdateParameters(userAadObjectId, "User", 50, 250);
adlaAccountClient.ComputePolicies.CreateOrUpdate(rg, adla, "GaryMcDaniel", newPolicyParams);
```

## <a name="next-steps"></a>Következő lépések
* [A Microsoft Azure Data Lake Analytics áttekintése](data-lake-analytics-overview.md)
* [Az Azure Data Lake Analytics kezelése az Azure Portal használatával](data-lake-analytics-manage-use-portal.md)
* [Az Azure Data Lake Analytics-feladatok figyelése és hibaelhárítása az Azure Portal használatával](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)
