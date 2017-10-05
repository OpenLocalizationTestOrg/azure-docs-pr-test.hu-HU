---
title: "Azure Data Lake Analytics Azure .NET SDK használatával kezelése |} Microsoft Docs"
description: "Megtudhatja, hogyan kezelheti a Data Lake Analytics-feladatok, az adatforrások, a felhasználók. "
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
ms.openlocfilehash: 0f8a95f96ce4c816dfb9132923faa9a9bf20c205
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="manage-azure-data-lake-analytics-using-azure-net-sdk"></a><span data-ttu-id="11d2b-103">Azure Data Lake Analytics Azure .NET SDK használatával kezelése</span><span class="sxs-lookup"><span data-stu-id="11d2b-103">Manage Azure Data Lake Analytics using Azure .NET SDK</span></span>
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

<span data-ttu-id="11d2b-104">Útmutató: Azure Data Lake Analytics-fiókok, adatforrások, felhasználók és az Azure .NET SDK-t használó feladatok kezelése.</span><span class="sxs-lookup"><span data-stu-id="11d2b-104">Learn how to manage Azure Data Lake Analytics accounts, data sources, users, and jobs using the Azure .NET SDK.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="11d2b-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="11d2b-105">Prerequisites</span></span>

* <span data-ttu-id="11d2b-106">**Visual Studio 2015, Visual Studio 2013 4. frissítéssel vagy Visual Studio 2012 és telepített Visual C++**.</span><span class="sxs-lookup"><span data-stu-id="11d2b-106">**Visual Studio 2015, Visual Studio 2013 update 4, or Visual Studio 2012 with Visual C++ Installed**.</span></span>
* <span data-ttu-id="11d2b-107">**Microsoft Azure SDK for .NET 2.5-ös vagy újabb verzió**.</span><span class="sxs-lookup"><span data-stu-id="11d2b-107">**Microsoft Azure SDK for .NET version 2.5 or above**.</span></span>  <span data-ttu-id="11d2b-108">Telepítse a [Webplatform-telepítővel](http://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="11d2b-108">Install it using the [Web platform installer](http://www.microsoft.com/web/downloads/platform.aspx).</span></span>
* <span data-ttu-id="11d2b-109">**Szükséges NuGet-csomagok**</span><span class="sxs-lookup"><span data-stu-id="11d2b-109">**Required NuGet Packages**</span></span>

### <a name="install-nuget-packages"></a><span data-ttu-id="11d2b-110">Telepítse a NuGet-csomagok</span><span class="sxs-lookup"><span data-stu-id="11d2b-110">Install NuGet packages</span></span>

|<span data-ttu-id="11d2b-111">Csomag</span><span class="sxs-lookup"><span data-stu-id="11d2b-111">Package</span></span>|<span data-ttu-id="11d2b-112">Verzió</span><span class="sxs-lookup"><span data-stu-id="11d2b-112">Version</span></span>|
|-------|-------|
|[<span data-ttu-id="11d2b-113">Microsoft.Rest.ClientRuntime.Azure.Authentication</span><span class="sxs-lookup"><span data-stu-id="11d2b-113">Microsoft.Rest.ClientRuntime.Azure.Authentication</span></span>](https://www.nuget.org/packages/Microsoft.Rest.ClientRuntime.Azure.Authentication)| <span data-ttu-id="11d2b-114">2.3.1</span><span class="sxs-lookup"><span data-stu-id="11d2b-114">2.3.1</span></span>|
|[<span data-ttu-id="11d2b-115">Microsoft.Azure.Management.DataLake.Analytics</span><span class="sxs-lookup"><span data-stu-id="11d2b-115">Microsoft.Azure.Management.DataLake.Analytics</span></span>](https://www.nuget.org/packages/Microsoft.Azure.Management.DataLake.Analytics)|<span data-ttu-id="11d2b-116">3.0.0</span><span class="sxs-lookup"><span data-stu-id="11d2b-116">3.0.0</span></span>|
|[<span data-ttu-id="11d2b-117">Microsoft.Azure.Management.DataLake.Store</span><span class="sxs-lookup"><span data-stu-id="11d2b-117">Microsoft.Azure.Management.DataLake.Store</span></span>](https://www.nuget.org/packages/Microsoft.Azure.Management.DataLake.Store)|<span data-ttu-id="11d2b-118">2.2.0</span><span class="sxs-lookup"><span data-stu-id="11d2b-118">2.2.0</span></span>|
|[<span data-ttu-id="11d2b-119">Microsoft.Azure.Management.ResourceManager</span><span class="sxs-lookup"><span data-stu-id="11d2b-119">Microsoft.Azure.Management.ResourceManager</span></span>](https://www.nuget.org/packages/Microsoft.Azure.Management.ResourceManager)|<span data-ttu-id="11d2b-120">1.6.0-Preview</span><span class="sxs-lookup"><span data-stu-id="11d2b-120">1.6.0-preview</span></span>|
|[<span data-ttu-id="11d2b-121">Microsoft.Azure.Graph.RBAC</span><span class="sxs-lookup"><span data-stu-id="11d2b-121">Microsoft.Azure.Graph.RBAC</span></span>](https://www.nuget.org/packages/Microsoft.Azure.Management.ResourceManager)|<span data-ttu-id="11d2b-122">3.4.0-Preview</span><span class="sxs-lookup"><span data-stu-id="11d2b-122">3.4.0-preview</span></span>|

<span data-ttu-id="11d2b-123">A következő parancsokkal telepítheti ezeket a csomagokat a NuGet parancssorból:</span><span class="sxs-lookup"><span data-stu-id="11d2b-123">You can install these packages via the NuGet command line with the following commands:</span></span>

```
Install-Package -Id Microsoft.Rest.ClientRuntime.Azure.Authentication  -Version 2.3.1
Install-Package -Id Microsoft.Azure.Management.DataLake.Analytics  -Version 3.0.0
Install-Package -Id Microsoft.Azure.Management.DataLake.Store  -Version 2.2.0
Install-Package -Id Microsoft.Azure.Management.ResourceManager  -Version 1.6.0-preview
Install-Package -Id Microsoft.Azure.Graph.RBAC -Version 3.4.0-preview
```

## <a name="common-variables"></a><span data-ttu-id="11d2b-124">Közös változók</span><span class="sxs-lookup"><span data-stu-id="11d2b-124">Common variables</span></span>

``` csharp
string subid = "<Subscription ID>"; // Subscription ID (a GUID)
string tenantid = "<Tenant ID>"; // AAD tenant ID or domain. For example, "contoso.onmicrosoft.com"
string rg == "<value>"; // Resource  group name
string clientid = "1950a258-227b-4e31-a9cf-717495945fc2"; // Sample client ID (this will work, but you should pick your own)
```

## <a name="authentication"></a><span data-ttu-id="11d2b-125">Authentication</span><span class="sxs-lookup"><span data-stu-id="11d2b-125">Authentication</span></span>

<span data-ttu-id="11d2b-126">Lehetősége van több Azure Data Lake Analytics történő bejelentkezéshez.</span><span class="sxs-lookup"><span data-stu-id="11d2b-126">You have multiple options for logging on to Azure Data Lake Analytics.</span></span> <span data-ttu-id="11d2b-127">Az alábbi részlet egy előugró ablak az interaktív felhasználói hitelesítéssel hitelesítési példáját mutatja be.</span><span class="sxs-lookup"><span data-stu-id="11d2b-127">The following snippet shows an example of authentication with interactive user authentication with a pop-up.</span></span>

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

<span data-ttu-id="11d2b-128">Forráskódja **GetCreds_User_Popup** és más beállításokat a hitelesítési kódját ismertetnek [Data Lake Analytics .NET-hitelesítés beállításai](https://github.com/Azure-Samples/data-lake-analytics-dotnet-auth-options)</span><span class="sxs-lookup"><span data-stu-id="11d2b-128">The source code for **GetCreds_User_Popup** and the code for other options for authentication are covered in [Data Lake Analytics .NET authentication options](https://github.com/Azure-Samples/data-lake-analytics-dotnet-auth-options)</span></span>


## <a name="create-the-client-management-objects"></a><span data-ttu-id="11d2b-129">Az ügyfél létrehozása kezelési objektumok</span><span class="sxs-lookup"><span data-stu-id="11d2b-129">Create the client management objects</span></span>

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

## <a name="manage-accounts"></a><span data-ttu-id="11d2b-130">Fiókok kezelése</span><span class="sxs-lookup"><span data-stu-id="11d2b-130">Manage accounts</span></span>

### <a name="create-an-azure-resource-group"></a><span data-ttu-id="11d2b-131">Azure-erőforráscsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="11d2b-131">Create an Azure Resource Group</span></span>

<span data-ttu-id="11d2b-132">Ha még nem már létrehozott egy, a Data Lake Analytics-összetevők létrehozásához Azure-erőforráscsoport kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="11d2b-132">If you haven't already created one, you must have an Azure Resource Group to create your Data Lake Analytics components.</span></span> <span data-ttu-id="11d2b-133">A hitelesítő adatok, előfizetés-azonosító és egy helyre van szükség.</span><span class="sxs-lookup"><span data-stu-id="11d2b-133">You  need your authentication credentials, subscription ID, and a location.</span></span> <span data-ttu-id="11d2b-134">A következő kód bemutatja, hogyan hozzon létre egy erőforráscsoportot:</span><span class="sxs-lookup"><span data-stu-id="11d2b-134">The following code shows how to create a resource group:</span></span>

``` csharp
var resourceGroup = new ResourceGroup { Location = location };
resourceManagementClient.ResourceGroups.CreateOrUpdate(groupName, rg);
```
<span data-ttu-id="11d2b-135">További információkért lásd: [Azure erőforráscsoport-sablonok és a Data Lake Analytics](#Azure-Resource-Groups-and-Data-Lake-Analytics).</span><span class="sxs-lookup"><span data-stu-id="11d2b-135">For more information, see [Azure Resource Groups and Data Lake Analytics](#Azure-Resource-Groups-and-Data-Lake-Analytics).</span></span>

### <a name="create-a-data-lake-store-account"></a><span data-ttu-id="11d2b-136">Data Lake Store-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="11d2b-136">Create a Data Lake Store account</span></span>

<span data-ttu-id="11d2b-137">Legalább egyszer a ADLA fiók egy ADLS-fiók szükséges.</span><span class="sxs-lookup"><span data-stu-id="11d2b-137">Ever ADLA account requires an ADLS account.</span></span> <span data-ttu-id="11d2b-138">Ha még nem rendelkezik egyike, létrehozhat egyet az alábbi kódra:</span><span class="sxs-lookup"><span data-stu-id="11d2b-138">If you don't already have one to use, you can create one with the following code:</span></span>

``` csharp
var new_adls_params = new DataLakeStoreAccount(location: _location);
adlsAccountClient.Account.Create(rg, adls, new_adls_params);
```

### <a name="create-a-data-lake-analytics-account"></a><span data-ttu-id="11d2b-139">Data Lake Analytics-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="11d2b-139">Create a Data Lake Analytics account</span></span>

<span data-ttu-id="11d2b-140">Az alábbi kód létrehoz egy ADLS-fiók</span><span class="sxs-lookup"><span data-stu-id="11d2b-140">The following code creates an ADLS account</span></span>

``` csharp
var new_adla_params = new DataLakeAnalyticsAccount()
{
   DefaultDataLakeStoreAccount = adls,
   Location = location
};

adlaClient.Account.Create(rg, adla, new_adla_params);
```

### <a name="list-data-lake-store-accounts"></a><span data-ttu-id="11d2b-141">Lista Data Lake Store-fiók</span><span class="sxs-lookup"><span data-stu-id="11d2b-141">List Data Lake Store accounts</span></span>

``` csharp
var adlsAccounts = adlsAccountClient.Account.List().ToList();
foreach (var adls in adlsAccounts)
{
   Console.WriteLine($"ADLS: {0}", adls.Name);
}
```

### <a name="list-data-lake-analytics-accounts"></a><span data-ttu-id="11d2b-142">Lista Data Lake Analytics-fiókok</span><span class="sxs-lookup"><span data-stu-id="11d2b-142">List Data Lake Analytics accounts</span></span>

``` csharp
var adlaAccounts = adlaClient.Account.List().ToList();

for (var adla in AdlaAccounts)
{
   Console.WriteLine($"ADLA: {0}, adla.Name");
}
```

### <a name="checking-if-an-account-exists"></a><span data-ttu-id="11d2b-143">Ha létezik-e a fiók ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="11d2b-143">Checking if an account exists</span></span>

``` csharp
bool exists = adlaClient.Account.Exists(rg, adla));
```

### <a name="get-information-about-an-account"></a><span data-ttu-id="11d2b-144">Egy fiók adatainak beolvasása</span><span class="sxs-lookup"><span data-stu-id="11d2b-144">Get information about an account</span></span>

``` csharp
bool exists = adlaClient.Account.Exists(rg, adla));
if (exists)
{
   var adla_accnt = adlaClient.Account.Get(rg, adla);
}
```

### <a name="delete-an-account"></a><span data-ttu-id="11d2b-145">-Fiók törlése</span><span class="sxs-lookup"><span data-stu-id="11d2b-145">Delete an account</span></span>

``` csharp
if (adlaClient.Account.Exists(rg, adla))
{
   adlaClient.Account.Delete(rg, adla);
}
```

### <a name="get-the-default-data-lake-store-account"></a><span data-ttu-id="11d2b-146">Az alapértelmezett Data Lake Store-fiók beszerzése</span><span class="sxs-lookup"><span data-stu-id="11d2b-146">Get the default Data Lake Store account</span></span>

<span data-ttu-id="11d2b-147">Minden Data Lake Analytics-fiókja alapértelmezett Data Lake Store-fiók szükséges.</span><span class="sxs-lookup"><span data-stu-id="11d2b-147">Every Data Lake Analytics account requires a default Data Lake Store account.</span></span> <span data-ttu-id="11d2b-148">Ez a kód segítségével határozhatja meg az alapértelmezett Store-fiókot az Analytics-fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="11d2b-148">Use this code to determine the default Store account for an Analytics account.</span></span>

``` csharp
if (adlaClient.Account.Exists(rg, adla))
{
  var adla_accnt = adlaClient.Account.Get(rg, adla);
  string def_adls_account = adla_accnt.DefaultDataLakeStoreAccount;
}
```

## <a name="manage-data-sources"></a><span data-ttu-id="11d2b-149">Adatforrások kezelése</span><span class="sxs-lookup"><span data-stu-id="11d2b-149">Manage data sources</span></span>

<span data-ttu-id="11d2b-150">A Data Lake Analytics jelenleg a következő adatforrásokat támogatja:</span><span class="sxs-lookup"><span data-stu-id="11d2b-150">Data Lake Analytics currently supports the following data sources:</span></span>

* [<span data-ttu-id="11d2b-151">Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="11d2b-151">Azure Data Lake Store</span></span>](../data-lake-store/data-lake-store-overview.md)
* [<span data-ttu-id="11d2b-152">Azure Storage-fiók</span><span class="sxs-lookup"><span data-stu-id="11d2b-152">Azure Storage Account</span></span>](../storage/common/storage-introduction.md)

### <a name="link-to-an-azure-storage-account"></a><span data-ttu-id="11d2b-153">Egy Azure Storage-fiók összekapcsolása</span><span class="sxs-lookup"><span data-stu-id="11d2b-153">Link to an Azure Storage account</span></span>

<span data-ttu-id="11d2b-154">Azure Storage-fiókok mutató hivatkozásokat is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="11d2b-154">You can create links to Azure Storage accounts.</span></span>

``` csharp
string storage_key = "xxxxxxxxxxxxxxxxxxxx";
string storage_account = "mystorageaccount";
var addParams = new AddStorageAccountParameters(storage_key);            
adlaClient.StorageAccounts.Add(rg, adla, storage_account, addParams);
```

### <a name="list-azure-storage-data-sources"></a><span data-ttu-id="11d2b-155">Azure Storage adatforrások listája</span><span class="sxs-lookup"><span data-stu-id="11d2b-155">List Azure Storage data sources</span></span>

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

### <a name="list-data-lake-store-data-sources"></a><span data-ttu-id="11d2b-156">Lista Data Lake Store adatforrások</span><span class="sxs-lookup"><span data-stu-id="11d2b-156">List Data Lake Store data sources</span></span>

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

### <a name="upload-and-download-folders-and-files"></a><span data-ttu-id="11d2b-157">Fájlok és mappák le- és feltöltése</span><span class="sxs-lookup"><span data-stu-id="11d2b-157">Upload and download folders and files</span></span>
<span data-ttu-id="11d2b-158">A Data Lake Store ügyfél felügyeleti Fájlrendszerobjektum segítségével töltse fel, és töltse le a fájlokat vagy mappákat az Azure-ból a helyi számítógépen, az alábbi módszerekkel:</span><span class="sxs-lookup"><span data-stu-id="11d2b-158">You can use the Data Lake Store file system client management object to upload and download individual files or folders from Azure to your local computer, using the following methods:</span></span>

- <span data-ttu-id="11d2b-159">UploadFolder</span><span class="sxs-lookup"><span data-stu-id="11d2b-159">UploadFolder</span></span>
- <span data-ttu-id="11d2b-160">UploadFile</span><span class="sxs-lookup"><span data-stu-id="11d2b-160">UploadFile</span></span>
- <span data-ttu-id="11d2b-161">DownloadFolder</span><span class="sxs-lookup"><span data-stu-id="11d2b-161">DownloadFolder</span></span>
- <span data-ttu-id="11d2b-162">DownloadFile</span><span class="sxs-lookup"><span data-stu-id="11d2b-162">DownloadFile</span></span>

<span data-ttu-id="11d2b-163">Az ezen módszerek első paramétere a Data Lake Store-fiók, kiegészítve a forrás elérési útja és a cél elérési útja paramétereinek a nevét.</span><span class="sxs-lookup"><span data-stu-id="11d2b-163">The first parameter for these methods is the name of the Data Lake Store Account, followed by parameters for the source path and the destination path.</span></span>

<span data-ttu-id="11d2b-164">A következő példa bemutatja, hogyan töltheti le a Data Lake Store egyik mappájába.</span><span class="sxs-lookup"><span data-stu-id="11d2b-164">The following example shows how to download a folder in the Data Lake Store.</span></span>

``` csharp
adlsFileSystemClient.FileSystem.DownloadFolder(adls, sourcePath, destinationPath);
```

### <a name="create-a-file-in-a-data-lake-store-account"></a><span data-ttu-id="11d2b-165">Hozzon létre egy fájlt egy Data Lake Store-fiókban</span><span class="sxs-lookup"><span data-stu-id="11d2b-165">Create a file in a Data Lake Store account</span></span>

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

### <a name="verify-azure-storage-account-paths"></a><span data-ttu-id="11d2b-166">Ellenőrizze az Azure Storage-fiók elérési utak</span><span class="sxs-lookup"><span data-stu-id="11d2b-166">Verify Azure Storage account paths</span></span>
<span data-ttu-id="11d2b-167">Az alábbi kód ellenőrzi, ha egy Azure Storage-fiók (storageAccntName) szerepel a Data Lake Analytics-fiók (analyticsAccountName), és a tároló (containerName) szerepel az Azure Storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="11d2b-167">The following code checks if an Azure Storage account (storageAccntName) exists in a Data Lake Analytics account (analyticsAccountName), and if a container (containerName) exists in the Azure Storage account.</span></span>

``` csharp
string storage_account = "mystorageaccount";
string storage_container = "mycontainer";
bool accountExists = adlaClient.Account.StorageAccountExists(rg, adla, storage_account));
bool containerExists = adlaClient.Account.StorageContainerExists(rg, adla, storage_account, storage_container));
```

## <a name="manage-catalog-and-jobs"></a><span data-ttu-id="11d2b-168">Katalógus és feladatok kezelése</span><span class="sxs-lookup"><span data-stu-id="11d2b-168">Manage catalog and jobs</span></span>
<span data-ttu-id="11d2b-169">A DataLakeAnalyticsCatalogManagementClient objektum minden Azure Data Lake Analytics-fiók a megadott SQL-adatbázis kezelésére szolgáló módszert biztosít.</span><span class="sxs-lookup"><span data-stu-id="11d2b-169">The DataLakeAnalyticsCatalogManagementClient object provides methods for managing the SQL database provided for each Azure Data Lake Analytics account.</span></span> <span data-ttu-id="11d2b-170">A DataLakeAnalyticsJobManagementClient biztosít a U-SQL-parancsfájlok adatbázis futtatásához módszereket küldje el, és a feladatok kezelése.</span><span class="sxs-lookup"><span data-stu-id="11d2b-170">The DataLakeAnalyticsJobManagementClient provides methods to submit and manage jobs run on the database with U-SQL scripts.</span></span>

### <a name="list-databases-and-schemas"></a><span data-ttu-id="11d2b-171">Lista adatbázisok és -sémákat</span><span class="sxs-lookup"><span data-stu-id="11d2b-171">List databases and schemas</span></span>
<span data-ttu-id="11d2b-172">Között is számos dolgot a legtöbb olyan adatbázisok és a séma.</span><span class="sxs-lookup"><span data-stu-id="11d2b-172">Among the several things you can list, the most common are databases and their schema.</span></span> <span data-ttu-id="11d2b-173">A következő kódot jut hozzá a adatbázisok gyűjteménye, és az egyes adatbázisok séma majd enumerálása.</span><span class="sxs-lookup"><span data-stu-id="11d2b-173">The following code obtains a collection of databases, and then enumerates the schema for each database.</span></span>

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

### <a name="list-table-columns"></a><span data-ttu-id="11d2b-174">A táblázat oszlopaihoz listája</span><span class="sxs-lookup"><span data-stu-id="11d2b-174">List table columns</span></span>
<span data-ttu-id="11d2b-175">A következő kód bemutatja, hogyan érik el az adatbázist a megadott tábla az oszlopok egy Data Lake Analytics-katalógus felügyeleti ügyféllel.</span><span class="sxs-lookup"><span data-stu-id="11d2b-175">The following code shows how to access the database with a Data Lake Analytics Catalog management client to list the columns in a specified table.</span></span>

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

### <a name="list-failed-jobs"></a><span data-ttu-id="11d2b-176">A sikertelen feladatok listázása</span><span class="sxs-lookup"><span data-stu-id="11d2b-176">List failed jobs</span></span>
<span data-ttu-id="11d2b-177">A következő kódot a sikertelen feladatok információkat sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="11d2b-177">The following code lists information about jobs that failed.</span></span>

``` csharp
var odq = new ODataQuery<JobInformation> { Filter = "result eq 'Failed'" };
var jobs = adlaJobClient.Job.List(adla, odq);
foreach (var j in jobs)
{
   Console.WriteLine($"{j.Name}\t{j.JobId}\t{j.Type}\t{j.StartTime}\t{j.EndTime}");
}
```

### <a name="list-pipelines"></a><span data-ttu-id="11d2b-178">Lista folyamatok</span><span class="sxs-lookup"><span data-stu-id="11d2b-178">List pipelines</span></span>
<span data-ttu-id="11d2b-179">A következő kódot az a fiók a feladat minden folyamatkezelési kapcsolatos információkat sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="11d2b-179">The following code lists information about each pipeline of jobs submitted to the account.</span></span>

``` csharp
var pipelines = adlaJobClient.Pipeline.List(adla);
foreach (var p in pipelines)
{
   Console.WriteLine($"Pipeline: {p.Name}\t{p.PipelineId}\t{p.LastSubmitTime}");
}
```

### <a name="list-recurrences"></a><span data-ttu-id="11d2b-180">Lista ismétlődések</span><span class="sxs-lookup"><span data-stu-id="11d2b-180">List recurrences</span></span>
<span data-ttu-id="11d2b-181">A következő kódot az ahhoz a fiókhoz feladat minden egyes ismétlődési kapcsolatos információkat sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="11d2b-181">The following code lists information about each recurrence of jobs submitted to the account.</span></span>

``` csharp
var recurrences = adlaJobClient.Recurrence.List(adla);
foreach (var r in recurrences)
{
   Console.WriteLine($"Recurrence: {r.Name}\t{r.RecurrenceId}\t{r.LastSubmitTime}");
}
```

## <a name="common-graph-scenarios"></a><span data-ttu-id="11d2b-182">Általános graph-forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="11d2b-182">Common graph scenarios</span></span>

### <a name="look-up-user-in-the-aad-directory"></a><span data-ttu-id="11d2b-183">Az AAD-címtárában lévő felhasználó keresése</span><span class="sxs-lookup"><span data-stu-id="11d2b-183">Look up user in the AAD directory</span></span>

``` csharp
var userinfo = graphClient.Users.Get( "bill@contoso.com" );
```

### <a name="get-the-objectid-of-a-user-in-the-aad-directory"></a><span data-ttu-id="11d2b-184">Egy felhasználó ObjectId beolvasni az AAD-címtárában</span><span class="sxs-lookup"><span data-stu-id="11d2b-184">Get the ObjectId of a user in the AAD directory</span></span>

``` csharp
var userinfo = graphClient.Users.Get( "bill@contoso.com" );
Console.WriteLine( userinfo.ObjectId )
```

## <a name="manage-compute-policies"></a><span data-ttu-id="11d2b-185">Számítási házirendjeinek kezelése</span><span class="sxs-lookup"><span data-stu-id="11d2b-185">Manage compute policies</span></span>
<span data-ttu-id="11d2b-186">A DataLakeAnalyticsAccountManagementClient objektum a számítási házirendek előnyeit a Data Lake Analytics-fiók kezelésére szolgáló módszert biztosít.</span><span class="sxs-lookup"><span data-stu-id="11d2b-186">The DataLakeAnalyticsAccountManagementClient object provides methods for managing the compute policies for a Data Lake Analytics account.</span></span>

### <a name="list-compute-policies"></a><span data-ttu-id="11d2b-187">Számítási házirendek felsorolása</span><span class="sxs-lookup"><span data-stu-id="11d2b-187">List compute policies</span></span>
<span data-ttu-id="11d2b-188">A következő kód lekéri a Data Lake Analytics-fiók számítási házirendek listáját.</span><span class="sxs-lookup"><span data-stu-id="11d2b-188">The following code retrieves a list of compute policies for a Data Lake Analytics account.</span></span>

``` csharp
var policies = adlaAccountClient.ComputePolicies.ListByAccount(rg, adla);
foreach (var p in policies)
{
   Console.WriteLine($"Name: {p.Name}\tType: {p.ObjectType}\tMax AUs / job: {p.MaxDegreeOfParallelismPerJob}\tMin priority / job: {p.MinPriorityPerJob}");
}
```

### <a name="create-a-new-compute-policy"></a><span data-ttu-id="11d2b-189">Hozzon létre egy új számítási házirendet</span><span class="sxs-lookup"><span data-stu-id="11d2b-189">Create a new compute policy</span></span>
<span data-ttu-id="11d2b-190">Az alábbi kód létrehoz egy új számítási házirendet Data Lake Analytics-fiók esetén a rendelkezésre álló maximális ausztráliai beállítása a megadott felhasználó 50 és 250 minimális feladat prioritása.</span><span class="sxs-lookup"><span data-stu-id="11d2b-190">The following code creates a new compute policy for a Data Lake Analytics account, setting the maximum AUs available to the specified user to 50, and the minimum job priority to 250.</span></span>

``` csharp
var userAadObjectId = "3b097601-4912-4d41-b9d2-78672fc2acde";
var newPolicyParams = new ComputePolicyCreateOrUpdateParameters(userAadObjectId, "User", 50, 250);
adlaAccountClient.ComputePolicies.CreateOrUpdate(rg, adla, "GaryMcDaniel", newPolicyParams);
```

## <a name="next-steps"></a><span data-ttu-id="11d2b-191">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="11d2b-191">Next steps</span></span>
* [<span data-ttu-id="11d2b-192">A Microsoft Azure Data Lake Analytics áttekintése</span><span class="sxs-lookup"><span data-stu-id="11d2b-192">Overview of Microsoft Azure Data Lake Analytics</span></span>](data-lake-analytics-overview.md)
* [<span data-ttu-id="11d2b-193">Az Azure Data Lake Analytics kezelése az Azure Portal használatával</span><span class="sxs-lookup"><span data-stu-id="11d2b-193">Manage Azure Data Lake Analytics using Azure portal</span></span>](data-lake-analytics-manage-use-portal.md)
* [<span data-ttu-id="11d2b-194">Az Azure Data Lake Analytics-feladatok figyelése és hibaelhárítása az Azure Portal használatával</span><span class="sxs-lookup"><span data-stu-id="11d2b-194">Monitor and troubleshoot Azure Data Lake Analytics jobs using Azure portal</span></span>](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)
