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
# <a name="manage-azure-data-lake-analytics-using-azure-net-sdk"></a><span data-ttu-id="677b8-103">Azure Data Lake Analytics Azure .NET SDK használatával kezelése</span><span class="sxs-lookup"><span data-stu-id="677b8-103">Manage Azure Data Lake Analytics using Azure .NET SDK</span></span>
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

<span data-ttu-id="677b8-104">Ismerje meg, hogyan toomanage Azure Data Lake Analytics-fiókok, adatforrások, felhasználók és használó feladatok hello Azure .NET SDK-t.</span><span class="sxs-lookup"><span data-stu-id="677b8-104">Learn how toomanage Azure Data Lake Analytics accounts, data sources, users, and jobs using hello Azure .NET SDK.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="677b8-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="677b8-105">Prerequisites</span></span>

* <span data-ttu-id="677b8-106">**Visual Studio 2015, Visual Studio 2013 4. frissítéssel vagy Visual Studio 2012 és telepített Visual C++**.</span><span class="sxs-lookup"><span data-stu-id="677b8-106">**Visual Studio 2015, Visual Studio 2013 update 4, or Visual Studio 2012 with Visual C++ Installed**.</span></span>
* <span data-ttu-id="677b8-107">**Microsoft Azure SDK for .NET 2.5-ös vagy újabb verzió**.</span><span class="sxs-lookup"><span data-stu-id="677b8-107">**Microsoft Azure SDK for .NET version 2.5 or above**.</span></span>  <span data-ttu-id="677b8-108">Telepítse hello [webplatform-telepítő](http://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="677b8-108">Install it using hello [Web platform installer](http://www.microsoft.com/web/downloads/platform.aspx).</span></span>
* <span data-ttu-id="677b8-109">**Szükséges NuGet-csomagok**</span><span class="sxs-lookup"><span data-stu-id="677b8-109">**Required NuGet Packages**</span></span>

### <a name="install-nuget-packages"></a><span data-ttu-id="677b8-110">NuGet-csomagok telepítése</span><span class="sxs-lookup"><span data-stu-id="677b8-110">Install NuGet packages</span></span>

|<span data-ttu-id="677b8-111">Csomag</span><span class="sxs-lookup"><span data-stu-id="677b8-111">Package</span></span>|<span data-ttu-id="677b8-112">Verzió</span><span class="sxs-lookup"><span data-stu-id="677b8-112">Version</span></span>|
|-------|-------|
|[<span data-ttu-id="677b8-113">Microsoft.Rest.ClientRuntime.Azure.Authentication</span><span class="sxs-lookup"><span data-stu-id="677b8-113">Microsoft.Rest.ClientRuntime.Azure.Authentication</span></span>](https://www.nuget.org/packages/Microsoft.Rest.ClientRuntime.Azure.Authentication)| <span data-ttu-id="677b8-114">2.3.1</span><span class="sxs-lookup"><span data-stu-id="677b8-114">2.3.1</span></span>|
|[<span data-ttu-id="677b8-115">Microsoft.Azure.Management.DataLake.Analytics</span><span class="sxs-lookup"><span data-stu-id="677b8-115">Microsoft.Azure.Management.DataLake.Analytics</span></span>](https://www.nuget.org/packages/Microsoft.Azure.Management.DataLake.Analytics)|<span data-ttu-id="677b8-116">3.0.0</span><span class="sxs-lookup"><span data-stu-id="677b8-116">3.0.0</span></span>|
|[<span data-ttu-id="677b8-117">Microsoft.Azure.Management.DataLake.Store</span><span class="sxs-lookup"><span data-stu-id="677b8-117">Microsoft.Azure.Management.DataLake.Store</span></span>](https://www.nuget.org/packages/Microsoft.Azure.Management.DataLake.Store)|<span data-ttu-id="677b8-118">2.2.0</span><span class="sxs-lookup"><span data-stu-id="677b8-118">2.2.0</span></span>|
|[<span data-ttu-id="677b8-119">Microsoft.Azure.Management.ResourceManager</span><span class="sxs-lookup"><span data-stu-id="677b8-119">Microsoft.Azure.Management.ResourceManager</span></span>](https://www.nuget.org/packages/Microsoft.Azure.Management.ResourceManager)|<span data-ttu-id="677b8-120">1.6.0-Preview</span><span class="sxs-lookup"><span data-stu-id="677b8-120">1.6.0-preview</span></span>|
|[<span data-ttu-id="677b8-121">Microsoft.Azure.Graph.RBAC</span><span class="sxs-lookup"><span data-stu-id="677b8-121">Microsoft.Azure.Graph.RBAC</span></span>](https://www.nuget.org/packages/Microsoft.Azure.Management.ResourceManager)|<span data-ttu-id="677b8-122">3.4.0-Preview</span><span class="sxs-lookup"><span data-stu-id="677b8-122">3.4.0-preview</span></span>|

<span data-ttu-id="677b8-123">A következő parancsok hello telepítheti ezeket a csomagokat hello NuGet parancssorból:</span><span class="sxs-lookup"><span data-stu-id="677b8-123">You can install these packages via hello NuGet command line with hello following commands:</span></span>

```
Install-Package -Id Microsoft.Rest.ClientRuntime.Azure.Authentication  -Version 2.3.1
Install-Package -Id Microsoft.Azure.Management.DataLake.Analytics  -Version 3.0.0
Install-Package -Id Microsoft.Azure.Management.DataLake.Store  -Version 2.2.0
Install-Package -Id Microsoft.Azure.Management.ResourceManager  -Version 1.6.0-preview
Install-Package -Id Microsoft.Azure.Graph.RBAC -Version 3.4.0-preview
```

## <a name="common-variables"></a><span data-ttu-id="677b8-124">Közös változók</span><span class="sxs-lookup"><span data-stu-id="677b8-124">Common variables</span></span>

``` csharp
string subid = "<Subscription ID>"; // Subscription ID (a GUID)
string tenantid = "<Tenant ID>"; // AAD tenant ID or domain. For example, "contoso.onmicrosoft.com"
string rg == "<value>"; // Resource  group name
string clientid = "1950a258-227b-4e31-a9cf-717495945fc2"; // Sample client ID (this will work, but you should pick your own)
```

## <a name="authentication"></a><span data-ttu-id="677b8-125">Authentication</span><span class="sxs-lookup"><span data-stu-id="677b8-125">Authentication</span></span>

<span data-ttu-id="677b8-126">A Data Lake Analytics tooAzure bejelentkezés több lehetősége van.</span><span class="sxs-lookup"><span data-stu-id="677b8-126">You have multiple options for logging on tooAzure Data Lake Analytics.</span></span> <span data-ttu-id="677b8-127">hello alábbi kódrészletet mutat példát hitelesítési egy előugró ablak az interaktív felhasználói hitelesítéssel.</span><span class="sxs-lookup"><span data-stu-id="677b8-127">hello following snippet shows an example of authentication with interactive user authentication with a pop-up.</span></span>

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

<span data-ttu-id="677b8-128">hello forráskódja **GetCreds_User_Popup** és hello kód számára más beállításokat a hitelesítési ismertetnek [Data Lake Analytics .NET-hitelesítés beállításai](https://github.com/Azure-Samples/data-lake-analytics-dotnet-auth-options)</span><span class="sxs-lookup"><span data-stu-id="677b8-128">hello source code for **GetCreds_User_Popup** and hello code for other options for authentication are covered in [Data Lake Analytics .NET authentication options](https://github.com/Azure-Samples/data-lake-analytics-dotnet-auth-options)</span></span>


## <a name="create-hello-client-management-objects"></a><span data-ttu-id="677b8-129">Hello ügyfél létrehozása kezelési objektumok</span><span class="sxs-lookup"><span data-stu-id="677b8-129">Create hello client management objects</span></span>

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

## <a name="manage-accounts"></a><span data-ttu-id="677b8-130">Fiókok kezelése</span><span class="sxs-lookup"><span data-stu-id="677b8-130">Manage accounts</span></span>

### <a name="create-an-azure-resource-group"></a><span data-ttu-id="677b8-131">Azure-erőforráscsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="677b8-131">Create an Azure Resource Group</span></span>

<span data-ttu-id="677b8-132">Ha még nem már létrehozott egy, a Data Lake Analytics-összetevők az Azure-erőforráscsoport toocreate kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="677b8-132">If you haven't already created one, you must have an Azure Resource Group toocreate your Data Lake Analytics components.</span></span> <span data-ttu-id="677b8-133">A hitelesítő adatok, előfizetés-azonosító és egy helyre van szükség.</span><span class="sxs-lookup"><span data-stu-id="677b8-133">You  need your authentication credentials, subscription ID, and a location.</span></span> <span data-ttu-id="677b8-134">a következő kód bemutatja hogyan hello toocreate erőforráscsoport:</span><span class="sxs-lookup"><span data-stu-id="677b8-134">hello following code shows how toocreate a resource group:</span></span>

``` csharp
var resourceGroup = new ResourceGroup { Location = location };
resourceManagementClient.ResourceGroups.CreateOrUpdate(groupName, rg);
```
<span data-ttu-id="677b8-135">További információkért lásd: [Azure erőforráscsoport-sablonok és a Data Lake Analytics](#Azure-Resource-Groups-and-Data-Lake-Analytics).</span><span class="sxs-lookup"><span data-stu-id="677b8-135">For more information, see [Azure Resource Groups and Data Lake Analytics](#Azure-Resource-Groups-and-Data-Lake-Analytics).</span></span>

### <a name="create-a-data-lake-store-account"></a><span data-ttu-id="677b8-136">Data Lake Store-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="677b8-136">Create a Data Lake Store account</span></span>

<span data-ttu-id="677b8-137">Legalább egyszer a ADLA fiók egy ADLS-fiók szükséges.</span><span class="sxs-lookup"><span data-stu-id="677b8-137">Ever ADLA account requires an ADLS account.</span></span> <span data-ttu-id="677b8-138">Ha még nem rendelkezik egy toouse, létrehozhat egyet a következő kód hello:</span><span class="sxs-lookup"><span data-stu-id="677b8-138">If you don't already have one toouse, you can create one with hello following code:</span></span>

``` csharp
var new_adls_params = new DataLakeStoreAccount(location: _location);
adlsAccountClient.Account.Create(rg, adls, new_adls_params);
```

### <a name="create-a-data-lake-analytics-account"></a><span data-ttu-id="677b8-139">Data Lake Analytics-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="677b8-139">Create a Data Lake Analytics account</span></span>

<span data-ttu-id="677b8-140">a következő kód hello létrehoz egy ADLS-fiók</span><span class="sxs-lookup"><span data-stu-id="677b8-140">hello following code creates an ADLS account</span></span>

``` csharp
var new_adla_params = new DataLakeAnalyticsAccount()
{
   DefaultDataLakeStoreAccount = adls,
   Location = location
};

adlaClient.Account.Create(rg, adla, new_adla_params);
```

### <a name="list-data-lake-store-accounts"></a><span data-ttu-id="677b8-141">Lista Data Lake Store-fiók</span><span class="sxs-lookup"><span data-stu-id="677b8-141">List Data Lake Store accounts</span></span>

``` csharp
var adlsAccounts = adlsAccountClient.Account.List().ToList();
foreach (var adls in adlsAccounts)
{
   Console.WriteLine($"ADLS: {0}", adls.Name);
}
```

### <a name="list-data-lake-analytics-accounts"></a><span data-ttu-id="677b8-142">Lista Data Lake Analytics-fiókok</span><span class="sxs-lookup"><span data-stu-id="677b8-142">List Data Lake Analytics accounts</span></span>

``` csharp
var adlaAccounts = adlaClient.Account.List().ToList();

for (var adla in AdlaAccounts)
{
   Console.WriteLine($"ADLA: {0}, adla.Name");
}
```

### <a name="checking-if-an-account-exists"></a><span data-ttu-id="677b8-143">Ha létezik-e a fiók ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="677b8-143">Checking if an account exists</span></span>

``` csharp
bool exists = adlaClient.Account.Exists(rg, adla));
```

### <a name="get-information-about-an-account"></a><span data-ttu-id="677b8-144">Egy fiók adatainak beolvasása</span><span class="sxs-lookup"><span data-stu-id="677b8-144">Get information about an account</span></span>

``` csharp
bool exists = adlaClient.Account.Exists(rg, adla));
if (exists)
{
   var adla_accnt = adlaClient.Account.Get(rg, adla);
}
```

### <a name="delete-an-account"></a><span data-ttu-id="677b8-145">-Fiók törlése</span><span class="sxs-lookup"><span data-stu-id="677b8-145">Delete an account</span></span>

``` csharp
if (adlaClient.Account.Exists(rg, adla))
{
   adlaClient.Account.Delete(rg, adla);
}
```

### <a name="get-hello-default-data-lake-store-account"></a><span data-ttu-id="677b8-146">Hello alapértelmezett Data Lake Store-fiók beszerzése</span><span class="sxs-lookup"><span data-stu-id="677b8-146">Get hello default Data Lake Store account</span></span>

<span data-ttu-id="677b8-147">Minden Data Lake Analytics-fiókja alapértelmezett Data Lake Store-fiók szükséges.</span><span class="sxs-lookup"><span data-stu-id="677b8-147">Every Data Lake Analytics account requires a default Data Lake Store account.</span></span> <span data-ttu-id="677b8-148">Használja a kód toodetermine hello alapértelmezett tároló fiókot az Analytics-fiók.</span><span class="sxs-lookup"><span data-stu-id="677b8-148">Use this code toodetermine hello default Store account for an Analytics account.</span></span>

``` csharp
if (adlaClient.Account.Exists(rg, adla))
{
  var adla_accnt = adlaClient.Account.Get(rg, adla);
  string def_adls_account = adla_accnt.DefaultDataLakeStoreAccount;
}
```

## <a name="manage-data-sources"></a><span data-ttu-id="677b8-149">Adatforrások kezelése</span><span class="sxs-lookup"><span data-stu-id="677b8-149">Manage data sources</span></span>

<span data-ttu-id="677b8-150">A Data Lake Analytics jelenleg a következő adatforrások hello támogatja:</span><span class="sxs-lookup"><span data-stu-id="677b8-150">Data Lake Analytics currently supports hello following data sources:</span></span>

* [<span data-ttu-id="677b8-151">Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="677b8-151">Azure Data Lake Store</span></span>](../data-lake-store/data-lake-store-overview.md)
* [<span data-ttu-id="677b8-152">Azure Storage-fiók</span><span class="sxs-lookup"><span data-stu-id="677b8-152">Azure Storage Account</span></span>](../storage/common/storage-introduction.md)

### <a name="link-tooan-azure-storage-account"></a><span data-ttu-id="677b8-153">Hivatkozás tooan Azure Storage-fiókban</span><span class="sxs-lookup"><span data-stu-id="677b8-153">Link tooan Azure Storage account</span></span>

<span data-ttu-id="677b8-154">Hivatkozások tooAzure Storage-fiókokat hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="677b8-154">You can create links tooAzure Storage accounts.</span></span>

``` csharp
string storage_key = "xxxxxxxxxxxxxxxxxxxx";
string storage_account = "mystorageaccount";
var addParams = new AddStorageAccountParameters(storage_key);            
adlaClient.StorageAccounts.Add(rg, adla, storage_account, addParams);
```

### <a name="list-azure-storage-data-sources"></a><span data-ttu-id="677b8-155">Azure Storage adatforrások listája</span><span class="sxs-lookup"><span data-stu-id="677b8-155">List Azure Storage data sources</span></span>

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

### <a name="list-data-lake-store-data-sources"></a><span data-ttu-id="677b8-156">Lista Data Lake Store adatforrások</span><span class="sxs-lookup"><span data-stu-id="677b8-156">List Data Lake Store data sources</span></span>

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

### <a name="upload-and-download-folders-and-files"></a><span data-ttu-id="677b8-157">Fájlok és mappák le- és feltöltése</span><span class="sxs-lookup"><span data-stu-id="677b8-157">Upload and download folders and files</span></span>
<span data-ttu-id="677b8-158">Hello Data Lake Store fájl rendszer ügyfél felügyeleti objektum tooupload használja, és töltse le a fájlokat vagy mappákat Azure tooyour helyi számítógépről, a következő módszerek hello használata:</span><span class="sxs-lookup"><span data-stu-id="677b8-158">You can use hello Data Lake Store file system client management object tooupload and download individual files or folders from Azure tooyour local computer, using hello following methods:</span></span>

- <span data-ttu-id="677b8-159">UploadFolder</span><span class="sxs-lookup"><span data-stu-id="677b8-159">UploadFolder</span></span>
- <span data-ttu-id="677b8-160">UploadFile</span><span class="sxs-lookup"><span data-stu-id="677b8-160">UploadFile</span></span>
- <span data-ttu-id="677b8-161">DownloadFolder</span><span class="sxs-lookup"><span data-stu-id="677b8-161">DownloadFolder</span></span>
- <span data-ttu-id="677b8-162">DownloadFile</span><span class="sxs-lookup"><span data-stu-id="677b8-162">DownloadFile</span></span>

<span data-ttu-id="677b8-163">Ezek a módszerek hello első paraméter értéke nem hello hello forrás elérési útja és hello célútvonalat paramétereinek követ hello Data Lake Store-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="677b8-163">hello first parameter for these methods is hello name of hello Data Lake Store Account, followed by parameters for hello source path and hello destination path.</span></span>

<span data-ttu-id="677b8-164">hello a következő példa bemutatja, hogyan toodownload egy mappát a hello Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="677b8-164">hello following example shows how toodownload a folder in hello Data Lake Store.</span></span>

``` csharp
adlsFileSystemClient.FileSystem.DownloadFolder(adls, sourcePath, destinationPath);
```

### <a name="create-a-file-in-a-data-lake-store-account"></a><span data-ttu-id="677b8-165">Hozzon létre egy fájlt egy Data Lake Store-fiókban</span><span class="sxs-lookup"><span data-stu-id="677b8-165">Create a file in a Data Lake Store account</span></span>

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

### <a name="verify-azure-storage-account-paths"></a><span data-ttu-id="677b8-166">Ellenőrizze az Azure Storage-fiók elérési utak</span><span class="sxs-lookup"><span data-stu-id="677b8-166">Verify Azure Storage account paths</span></span>
<span data-ttu-id="677b8-167">hello alábbira ellenőrzi, ha egy Azure Storage-fiók (storageAccntName) szerepel a Data Lake Analytics-fiók (analyticsAccountName), és a tároló (containerName) hello Azure Storage-fiók létezik.</span><span class="sxs-lookup"><span data-stu-id="677b8-167">hello following code checks if an Azure Storage account (storageAccntName) exists in a Data Lake Analytics account (analyticsAccountName), and if a container (containerName) exists in hello Azure Storage account.</span></span>

``` csharp
string storage_account = "mystorageaccount";
string storage_container = "mycontainer";
bool accountExists = adlaClient.Account.StorageAccountExists(rg, adla, storage_account));
bool containerExists = adlaClient.Account.StorageContainerExists(rg, adla, storage_account, storage_container));
```

## <a name="manage-catalog-and-jobs"></a><span data-ttu-id="677b8-168">Katalógus és feladatok kezelése</span><span class="sxs-lookup"><span data-stu-id="677b8-168">Manage catalog and jobs</span></span>
<span data-ttu-id="677b8-169">hello DataLakeAnalyticsCatalogManagementClient objektum hello minden Azure Data Lake Analytics-fiók a megadott SQL-adatbázis kezelésére szolgáló módszert biztosít.</span><span class="sxs-lookup"><span data-stu-id="677b8-169">hello DataLakeAnalyticsCatalogManagementClient object provides methods for managing hello SQL database provided for each Azure Data Lake Analytics account.</span></span> <span data-ttu-id="677b8-170">hello DataLakeAnalyticsJobManagementClient módszerek toosubmit biztosít, és futtassa a U-SQL-parancsfájlok hello adatbázis feladatok kezelése.</span><span class="sxs-lookup"><span data-stu-id="677b8-170">hello DataLakeAnalyticsJobManagementClient provides methods toosubmit and manage jobs run on hello database with U-SQL scripts.</span></span>

### <a name="list-databases-and-schemas"></a><span data-ttu-id="677b8-171">Lista adatbázisok és -sémákat</span><span class="sxs-lookup"><span data-stu-id="677b8-171">List databases and schemas</span></span>
<span data-ttu-id="677b8-172">Hello között több eredményoldalra is listázhatja, leggyakrabban előforduló hello minden olyan adatbázisok és a séma.</span><span class="sxs-lookup"><span data-stu-id="677b8-172">Among hello several things you can list, hello most common are databases and their schema.</span></span> <span data-ttu-id="677b8-173">hello alábbira jut hozzá a adatbázisok gyűjteménye, és az egyes adatbázisok hello séma majd enumerálása.</span><span class="sxs-lookup"><span data-stu-id="677b8-173">hello following code obtains a collection of databases, and then enumerates hello schema for each database.</span></span>

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

### <a name="list-table-columns"></a><span data-ttu-id="677b8-174">A táblázat oszlopaihoz listája</span><span class="sxs-lookup"><span data-stu-id="677b8-174">List table columns</span></span>
<span data-ttu-id="677b8-175">hello következő kód bemutatja, hogyan tooaccess hello egy Data Lake Analytics-katalógus felügyeleti ügyfél toolist hello a táblázatban levő oszlopkészleteket megadott adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="677b8-175">hello following code shows how tooaccess hello database with a Data Lake Analytics Catalog management client toolist hello columns in a specified table.</span></span>

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

### <a name="list-failed-jobs"></a><span data-ttu-id="677b8-176">A sikertelen feladatok listázása</span><span class="sxs-lookup"><span data-stu-id="677b8-176">List failed jobs</span></span>
<span data-ttu-id="677b8-177">hello alábbira sorolja fel a sikertelen feladatok kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="677b8-177">hello following code lists information about jobs that failed.</span></span>

``` csharp
var odq = new ODataQuery<JobInformation> { Filter = "result eq 'Failed'" };
var jobs = adlaJobClient.Job.List(adla, odq);
foreach (var j in jobs)
{
   Console.WriteLine($"{j.Name}\t{j.JobId}\t{j.Type}\t{j.StartTime}\t{j.EndTime}");
}
```

### <a name="list-pipelines"></a><span data-ttu-id="677b8-178">Lista folyamatok</span><span class="sxs-lookup"><span data-stu-id="677b8-178">List pipelines</span></span>
<span data-ttu-id="677b8-179">hello alábbira információ megjelenítése minden folyamatkezelési feladatok elküldött toohello fiók.</span><span class="sxs-lookup"><span data-stu-id="677b8-179">hello following code lists information about each pipeline of jobs submitted toohello account.</span></span>

``` csharp
var pipelines = adlaJobClient.Pipeline.List(adla);
foreach (var p in pipelines)
{
   Console.WriteLine($"Pipeline: {p.Name}\t{p.PipelineId}\t{p.LastSubmitTime}");
}
```

### <a name="list-recurrences"></a><span data-ttu-id="677b8-180">Lista ismétlődések</span><span class="sxs-lookup"><span data-stu-id="677b8-180">List recurrences</span></span>
<span data-ttu-id="677b8-181">hello alábbira információ megjelenítése minden ismétlődési feladatok elküldött toohello fiók.</span><span class="sxs-lookup"><span data-stu-id="677b8-181">hello following code lists information about each recurrence of jobs submitted toohello account.</span></span>

``` csharp
var recurrences = adlaJobClient.Recurrence.List(adla);
foreach (var r in recurrences)
{
   Console.WriteLine($"Recurrence: {r.Name}\t{r.RecurrenceId}\t{r.LastSubmitTime}");
}
```

## <a name="common-graph-scenarios"></a><span data-ttu-id="677b8-182">Általános graph-forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="677b8-182">Common graph scenarios</span></span>

### <a name="look-up-user-in-hello-aad-directory"></a><span data-ttu-id="677b8-183">Hello AAD-címtárában lévő felhasználó keresése</span><span class="sxs-lookup"><span data-stu-id="677b8-183">Look up user in hello AAD directory</span></span>

``` csharp
var userinfo = graphClient.Users.Get( "bill@contoso.com" );
```

### <a name="get-hello-objectid-of-a-user-in-hello-aad-directory"></a><span data-ttu-id="677b8-184">Egy felhasználó ObjectId hello beolvasása a hello AAD-címtárában</span><span class="sxs-lookup"><span data-stu-id="677b8-184">Get hello ObjectId of a user in hello AAD directory</span></span>

``` csharp
var userinfo = graphClient.Users.Get( "bill@contoso.com" );
Console.WriteLine( userinfo.ObjectId )
```

## <a name="manage-compute-policies"></a><span data-ttu-id="677b8-185">Számítási házirendjeinek kezelése</span><span class="sxs-lookup"><span data-stu-id="677b8-185">Manage compute policies</span></span>
<span data-ttu-id="677b8-186">hello DataLakeAnalyticsAccountManagementClient objektum biztosít hello kezelésére szolgáló módszerek számítási házirendek a Data Lake Analytics-fiók.</span><span class="sxs-lookup"><span data-stu-id="677b8-186">hello DataLakeAnalyticsAccountManagementClient object provides methods for managing hello compute policies for a Data Lake Analytics account.</span></span>

### <a name="list-compute-policies"></a><span data-ttu-id="677b8-187">Számítási házirendek felsorolása</span><span class="sxs-lookup"><span data-stu-id="677b8-187">List compute policies</span></span>
<span data-ttu-id="677b8-188">hello a következő kód lekéri a Data Lake Analytics-fiók számítási házirendek listáját.</span><span class="sxs-lookup"><span data-stu-id="677b8-188">hello following code retrieves a list of compute policies for a Data Lake Analytics account.</span></span>

``` csharp
var policies = adlaAccountClient.ComputePolicies.ListByAccount(rg, adla);
foreach (var p in policies)
{
   Console.WriteLine($"Name: {p.Name}\tType: {p.ObjectType}\tMax AUs / job: {p.MaxDegreeOfParallelismPerJob}\tMin priority / job: {p.MinPriorityPerJob}");
}
```

### <a name="create-a-new-compute-policy"></a><span data-ttu-id="677b8-189">Hozzon létre egy új számítási házirendet</span><span class="sxs-lookup"><span data-stu-id="677b8-189">Create a new compute policy</span></span>
<span data-ttu-id="677b8-190">a következő kód hello létrehoz egy új számítási házirendet a Data Lake Analytics-fiók, beállítás hello maximális ausztráliai elérhető toohello megadott felhasználói too50, és hello minimális feladat prioritása too250.</span><span class="sxs-lookup"><span data-stu-id="677b8-190">hello following code creates a new compute policy for a Data Lake Analytics account, setting hello maximum AUs available toohello specified user too50, and hello minimum job priority too250.</span></span>

``` csharp
var userAadObjectId = "3b097601-4912-4d41-b9d2-78672fc2acde";
var newPolicyParams = new ComputePolicyCreateOrUpdateParameters(userAadObjectId, "User", 50, 250);
adlaAccountClient.ComputePolicies.CreateOrUpdate(rg, adla, "GaryMcDaniel", newPolicyParams);
```

## <a name="next-steps"></a><span data-ttu-id="677b8-191">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="677b8-191">Next steps</span></span>
* [<span data-ttu-id="677b8-192">A Microsoft Azure Data Lake Analytics áttekintése</span><span class="sxs-lookup"><span data-stu-id="677b8-192">Overview of Microsoft Azure Data Lake Analytics</span></span>](data-lake-analytics-overview.md)
* [<span data-ttu-id="677b8-193">Az Azure Data Lake Analytics kezelése az Azure Portal használatával</span><span class="sxs-lookup"><span data-stu-id="677b8-193">Manage Azure Data Lake Analytics using Azure portal</span></span>](data-lake-analytics-manage-use-portal.md)
* [<span data-ttu-id="677b8-194">Az Azure Data Lake Analytics-feladatok figyelése és hibaelhárítása az Azure Portal használatával</span><span class="sxs-lookup"><span data-stu-id="677b8-194">Monitor and troubleshoot Azure Data Lake Analytics jobs using Azure portal</span></span>](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)
