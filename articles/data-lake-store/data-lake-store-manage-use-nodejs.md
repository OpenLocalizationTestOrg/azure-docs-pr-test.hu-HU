---
title: "Ismerkedés az Azure Data Lake Store Azure SDK for Node.js használatával |} Microsoft Docs"
description: "Útmutató a Node.js használhatja a Data Lake Store-fiókok és a fájlrendszerhez."
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 2fee173c-69ae-4e1d-8773-48618cda9e16
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/06/2017
ms.author: nitinme
ms.openlocfilehash: 8c7a2e6ca061bbfa077592efb73d592906c3d070
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-azure-data-lake-store-using-azure-sdk-for-nodejs"></a><span data-ttu-id="f4f9e-103">Ismerkedés az Azure Data Lake Store Azure SDK for Node.js használatával</span><span class="sxs-lookup"><span data-stu-id="f4f9e-103">Get started with Azure Data Lake Store using Azure SDK for Node.js</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="f4f9e-104">Portal</span><span class="sxs-lookup"><span data-stu-id="f4f9e-104">Portal</span></span>](data-lake-store-get-started-portal.md)
> * [<span data-ttu-id="f4f9e-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="f4f9e-105">PowerShell</span></span>](data-lake-store-get-started-powershell.md)
> * [<span data-ttu-id="f4f9e-106">.NET SDK</span><span class="sxs-lookup"><span data-stu-id="f4f9e-106">.NET SDK</span></span>](data-lake-store-get-started-net-sdk.md)
> * [<span data-ttu-id="f4f9e-107">Java SDK</span><span class="sxs-lookup"><span data-stu-id="f4f9e-107">Java SDK</span></span>](data-lake-store-get-started-java-sdk.md)
> * [<span data-ttu-id="f4f9e-108">REST API</span><span class="sxs-lookup"><span data-stu-id="f4f9e-108">REST API</span></span>](data-lake-store-get-started-rest-api.md)
> * [<span data-ttu-id="f4f9e-109">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="f4f9e-109">Azure CLI 2.0</span></span>](data-lake-store-get-started-cli-2.0.md)
> * [<span data-ttu-id="f4f9e-110">Node.js</span><span class="sxs-lookup"><span data-stu-id="f4f9e-110">Node.js</span></span>](data-lake-store-manage-use-nodejs.md)
> * [<span data-ttu-id="f4f9e-111">Python</span><span class="sxs-lookup"><span data-stu-id="f4f9e-111">Python</span></span>](data-lake-store-get-started-python.md)
>
> 

> [!NOTE]
> <span data-ttu-id="f4f9e-112">A feltöltését és letöltését nagy mennyiségű adatot (nagy fájlok, számos fájlok vagy mindkettőt), azt javasoljuk, hogy használja a [Python SDK](data-lake-store-get-started-python.md), a [.NET SDK](data-lake-store-get-started-net-sdk.md), [Azure CLI 2.0](data-lake-store-get-started-cli-2.0.md), vagy [Azure PowerShell](data-lake-store-get-started-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="f4f9e-112">For uploading and downloading large amount of data (large files, a large number of files, or both), we recommend that you use the [Python SDK](data-lake-store-get-started-python.md), the [.NET SDK](data-lake-store-get-started-net-sdk.md), [Azure CLI 2.0](data-lake-store-get-started-cli-2.0.md), or [Azure PowerShell](data-lake-store-get-started-powershell.md).</span></span> <span data-ttu-id="f4f9e-113">Ezek a lehetőségek jobb teljesítményt biztosítanak, mivel több szálat használnak, így az adattovábbítás párhuzamosan folyhat.</span><span class="sxs-lookup"><span data-stu-id="f4f9e-113">These options have better performance as they use multiple threads to parallelize the data movement.</span></span>
> 
> 

<span data-ttu-id="f4f9e-114">Útmutató: Azure Data Lake Store-fiók létrehozása és alapszintű műveleteket, mint például mappák létrehozása, és feltöltése adatfájlok le az Azure SDK for Node.js használatával, a fiók törlése stb. További információk a Data Lake Store-ról: [Overview of Data Lake Store](data-lake-store-overview.md) (A Data Lake Store áttekintése).</span><span class="sxs-lookup"><span data-stu-id="f4f9e-114">Learn how to use the Azure SDK for Node.js to create an Azure Data Lake Store account and perform basic operations such as create folders, upload and download data files, delete your account, etc. For more information about Data Lake Store, see [Overview of Data Lake Store](data-lake-store-overview.md).</span></span> <span data-ttu-id="f4f9e-115">Jelenleg az SDK támogatja</span><span class="sxs-lookup"><span data-stu-id="f4f9e-115">Currently, the SDK supports</span></span>

* <span data-ttu-id="f4f9e-116">**Node.js-verzió: 0.10.0-s vagy újabb**</span><span class="sxs-lookup"><span data-stu-id="f4f9e-116">**Node.js version: 0.10.0 or higher**</span></span>
* <span data-ttu-id="f4f9e-117">**Fiókhoz tartozó REST API-verzió: 2015. 10. 01. előzetes verzió**</span><span class="sxs-lookup"><span data-stu-id="f4f9e-117">**REST API version for Account: 2015-10-01-preview**</span></span>
* <span data-ttu-id="f4f9e-118">**Fájlrendszer REST API-verzió: 2015-10-01. dátumú előnézeti**</span><span class="sxs-lookup"><span data-stu-id="f4f9e-118">**REST API version for FileSystem: 2015-10-01-preview**</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f4f9e-119">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f4f9e-119">Prerequisites</span></span>
<span data-ttu-id="f4f9e-120">A cikk elkezdéséhez az alábbiakkal kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="f4f9e-120">Before you begin this article, you must have the following:</span></span>

* <span data-ttu-id="f4f9e-121">**Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="f4f9e-121">**An Azure subscription**.</span></span> <span data-ttu-id="f4f9e-122">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f4f9e-122">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="f4f9e-123">**Egy Azure Active Directory-alkalmazás létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="f4f9e-123">**Create an Azure Active Directory Application**.</span></span> <span data-ttu-id="f4f9e-124">A Data Lake Store alkalmazás Azure AD-val történő hitelesítéséhez az Azure AD alkalmazást kell használni.</span><span class="sxs-lookup"><span data-stu-id="f4f9e-124">You use the Azure AD application to authenticate the Data Lake Store application with Azure AD.</span></span> <span data-ttu-id="f4f9e-125">Az Azure AD-val többféle módon is lehet hitelesíteni. Ezek a következők: **végfelhasználói hitelesítés** vagy **szolgáltatások közötti hitelesítés**.</span><span class="sxs-lookup"><span data-stu-id="f4f9e-125">There are different approaches to authenticate with Azure AD, which are **end-user authentication** or **service-to-service authentication**.</span></span> <span data-ttu-id="f4f9e-126">A hitelesítéssel kapcsolatban a [Végfelhasználói hitelesítés](data-lake-store-end-user-authenticate-using-active-directory.md) vagy a [Szolgáltatások közötti hitelesítés](data-lake-store-authenticate-using-active-directory.md) című témakörben talál útmutatást és további tudnivalókat.</span><span class="sxs-lookup"><span data-stu-id="f4f9e-126">For instructions and more information on how to authenticate, see [End-user authentication](data-lake-store-end-user-authenticate-using-active-directory.md) or [Service-to-service authentication](data-lake-store-authenticate-using-active-directory.md).</span></span>

## <a name="how-to-install"></a><span data-ttu-id="f4f9e-127">A telepítés módja</span><span class="sxs-lookup"><span data-stu-id="f4f9e-127">How to Install</span></span>
```bash
npm install azure-arm-datalake-store
```

## <a name="authenticate-using-azure-active-directory"></a><span data-ttu-id="f4f9e-128">Hitelesítés az Azure Active Directory használatával</span><span class="sxs-lookup"><span data-stu-id="f4f9e-128">Authenticate using Azure Active Directory</span></span>
<span data-ttu-id="f4f9e-129">Az alábbi részletek megjelenítése a Data Lake Store hitelesítő külön kétféleképpen az Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f4f9e-129">The snippets below show two separate ways of authenticating with Data Lake Store using Azure AD.</span></span> <span data-ttu-id="f4f9e-130">A hitelesítéshez a Data Lake Store különböző módszerek részletes leírását lásd: [hitelesítés a Data Lake Store az Azure Active Directoryval](data-lake-store-authenticate-using-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="f4f9e-130">For a detailed discussion on various methods to use for authentication with Data Lake Store, see [Authenticate with Data Lake Store using Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).</span></span>

<span data-ttu-id="f4f9e-131">Az alábbi részlet is szükséges bemeneti adatok, például az Azure AD-tartomány nevét, ügyfél-azonosító az Azure AD-alkalmazáshoz, stb. Ezek az adatok lekérhetők az Azure AD alkalmazás kell létrehozni, a részleteit is szerepelnek a fenti hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="f4f9e-131">The snippet below also requires inputs like Azure AD domain name, client ID for an Azure AD app, etc. All these details can be retrieved from an Azure AD application that you must created, the details of which are also included in link above.</span></span>

 ```javascript
 var msrestAzure = require('ms-rest-azure');
 //user authentication
 var credentials = new msRestAzure.UserTokenCredentials('your-client-id', 'your-domain', 'your-username', 'your-password', 'your-redirect-uri');
 //service principal authentication
 var credentials = new msRestAzure.ApplicationTokenCredentials('your-client-id', 'your-domain', 'your-secret');
 ```

## <a name="create-the-data-lake-store-clients"></a><span data-ttu-id="f4f9e-132">A Data Lake Store ügyfelek létrehozása</span><span class="sxs-lookup"><span data-stu-id="f4f9e-132">Create the Data Lake Store Clients</span></span>
```javascript
var adlsManagement = require("azure-arm-datalake-store");
var acccountClient = new adlsManagement.DataLakeStoreAccountClient(credentials, "your-subscription-id");
var filesystemClient = new adlsManagement.DataLakeStoreFileSystemClient(credentials);
```

## <a name="create-a-data-lake-store-account"></a><span data-ttu-id="f4f9e-133">Data Lake Store-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="f4f9e-133">Create a Data Lake Store Account</span></span>
```javascript
var util = require('util');
var resourceGroupName = 'testrg';
var accountName = 'testadlsacct';
var location = 'eastus2';

// account object to create
var accountToCreate = {
  tags: {
    testtag1: 'testvalue1',
    testtag2: 'testvalue2'
  },
  name: accountName,
  location: location
};

client.account.create(resourceGroupName, accountName, accountToCreate, function (err, result, request, response) {
  if (err) {
    console.log(err);
    /*err has reference to the actual request and response, so you can see what was sent and received on the wire.
      The structure of err looks like this:
      err: {
        code: 'Error Code',
        message: 'Error Message',
        body: 'The response body if any',
        request: reference to a stripped version of http request
        response: reference to a stripped version of the response
      }
    */
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## <a name="create-a-file-with-content"></a><span data-ttu-id="f4f9e-134">Hozzon létre egy fájlt, amelyeknek a tartalma</span><span class="sxs-lookup"><span data-stu-id="f4f9e-134">Create a file with content</span></span>
```javascript
var util = require('util');
var accountName = 'testadlsacct';
var fileToCreate = '/myfolder/myfile.txt';
var options = {
  streamContents: new Buffer('some string content')
}

filesystemClient.fileSystem.listFileStatus(accountName, fileToCreate, options, function (err, result, request, response) {
  if (err) {
    console.log(err);
  } else {
    // no result is returned, only a 201 response for success.
    console.log('response is: ' + util.inspect(response, {depth: null}));
  }
});
```

## <a name="get-a-list-of-files-and-folders"></a><span data-ttu-id="f4f9e-135">Fájlok és mappák listájának lekérdezése</span><span class="sxs-lookup"><span data-stu-id="f4f9e-135">Get a list of files and folders</span></span>
```javascript
var util = require('util');
var accountName = 'testadlsacct';
var pathToEnumerate = '/myfolder';
filesystemClient.fileSystem.listFileStatus(accountName, pathToEnumerate, function (err, result, request, response) {
  if (err) {
    console.log(err);
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## <a name="see-also"></a><span data-ttu-id="f4f9e-136">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="f4f9e-136">See also</span></span>
* [<span data-ttu-id="f4f9e-137">Microsoft Azure SDK for Node.js</span><span class="sxs-lookup"><span data-stu-id="f4f9e-137">Microsoft Azure SDK for Node.js</span></span>](https://github.com/azure/azure-sdk-for-node)
* [<span data-ttu-id="f4f9e-138">A Microsoft Azure SDK for Node.js – Data Lake Analytics kezelése</span><span class="sxs-lookup"><span data-stu-id="f4f9e-138">Microsoft Azure SDK for Node.js - Data Lake Analytics Management</span></span>](https://www.npmjs.com/package/azure-arm-datalake-analytics)

