---
title: "aaaGet Azure SDK for Node.js használatával az Azure Data Lake Store használatába |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Node.js toowork a Data Lake Store-fiókok és hello fájlrendszer."
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
ms.openlocfilehash: ce36a2e0de4e091a4e85ed784a3381415ef6f9e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-data-lake-store-using-azure-sdk-for-nodejs"></a><span data-ttu-id="ad30b-103">Ismerkedés az Azure Data Lake Store Azure SDK for Node.js használatával</span><span class="sxs-lookup"><span data-stu-id="ad30b-103">Get started with Azure Data Lake Store using Azure SDK for Node.js</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="ad30b-104">Portál</span><span class="sxs-lookup"><span data-stu-id="ad30b-104">Portal</span></span>](data-lake-store-get-started-portal.md)
> * [<span data-ttu-id="ad30b-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="ad30b-105">PowerShell</span></span>](data-lake-store-get-started-powershell.md)
> * [<span data-ttu-id="ad30b-106">.NET SDK</span><span class="sxs-lookup"><span data-stu-id="ad30b-106">.NET SDK</span></span>](data-lake-store-get-started-net-sdk.md)
> * [<span data-ttu-id="ad30b-107">Java SDK</span><span class="sxs-lookup"><span data-stu-id="ad30b-107">Java SDK</span></span>](data-lake-store-get-started-java-sdk.md)
> * [<span data-ttu-id="ad30b-108">REST API</span><span class="sxs-lookup"><span data-stu-id="ad30b-108">REST API</span></span>](data-lake-store-get-started-rest-api.md)
> * [<span data-ttu-id="ad30b-109">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="ad30b-109">Azure CLI 2.0</span></span>](data-lake-store-get-started-cli-2.0.md)
> * [<span data-ttu-id="ad30b-110">Node.js</span><span class="sxs-lookup"><span data-stu-id="ad30b-110">Node.js</span></span>](data-lake-store-manage-use-nodejs.md)
> * [<span data-ttu-id="ad30b-111">Python</span><span class="sxs-lookup"><span data-stu-id="ad30b-111">Python</span></span>](data-lake-store-get-started-python.md)
>
> 

> [!NOTE]
> <span data-ttu-id="ad30b-112">Fel-és letöltése nagy mennyiségű adatot (nagy fájlok, számos fájlok vagy mindkettőt), azt javasoljuk, hogy használja-e hello [Python SDK](data-lake-store-get-started-python.md), hello [.NET SDK](data-lake-store-get-started-net-sdk.md), [Azure CLI 2.0](data-lake-store-get-started-cli-2.0.md), vagy [Azure PowerShell](data-lake-store-get-started-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="ad30b-112">For uploading and downloading large amount of data (large files, a large number of files, or both), we recommend that you use hello [Python SDK](data-lake-store-get-started-python.md), hello [.NET SDK](data-lake-store-get-started-net-sdk.md), [Azure CLI 2.0](data-lake-store-get-started-cli-2.0.md), or [Azure PowerShell](data-lake-store-get-started-powershell.md).</span></span> <span data-ttu-id="ad30b-113">Ezek a beállítások jobb teljesítményt, mivel ezek több szál tooparallelize hello adatmozgás rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="ad30b-113">These options have better performance as they use multiple threads tooparallelize hello data movement.</span></span>
> 
> 

<span data-ttu-id="ad30b-114">Ismerje meg, hogyan toouse hello Azure SDK Node.js toocreate egy Azure Data Lake Store-fiókot és alapvető műveleteket, mint például mappák létrehozása, és feltöltése adatfájlok le, a fiók törlése stb. További információk a Data Lake Store-ról: [Overview of Data Lake Store](data-lake-store-overview.md) (A Data Lake Store áttekintése).</span><span class="sxs-lookup"><span data-stu-id="ad30b-114">Learn how toouse hello Azure SDK for Node.js toocreate an Azure Data Lake Store account and perform basic operations such as create folders, upload and download data files, delete your account, etc. For more information about Data Lake Store, see [Overview of Data Lake Store](data-lake-store-overview.md).</span></span> <span data-ttu-id="ad30b-115">Jelenleg a hello SDK által támogatott</span><span class="sxs-lookup"><span data-stu-id="ad30b-115">Currently, hello SDK supports</span></span>

* <span data-ttu-id="ad30b-116">**Node.js-verzió: 0.10.0-s vagy újabb**</span><span class="sxs-lookup"><span data-stu-id="ad30b-116">**Node.js version: 0.10.0 or higher**</span></span>
* <span data-ttu-id="ad30b-117">**Fiókhoz tartozó REST API-verzió: 2015. 10. 01. előzetes verzió**</span><span class="sxs-lookup"><span data-stu-id="ad30b-117">**REST API version for Account: 2015-10-01-preview**</span></span>
* <span data-ttu-id="ad30b-118">**Fájlrendszer REST API-verzió: 2015-10-01. dátumú előnézeti**</span><span class="sxs-lookup"><span data-stu-id="ad30b-118">**REST API version for FileSystem: 2015-10-01-preview**</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ad30b-119">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ad30b-119">Prerequisites</span></span>
<span data-ttu-id="ad30b-120">Ez a cikk elkezdéséhez hello következő kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="ad30b-120">Before you begin this article, you must have hello following:</span></span>

* <span data-ttu-id="ad30b-121">**Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="ad30b-121">**An Azure subscription**.</span></span> <span data-ttu-id="ad30b-122">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ad30b-122">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="ad30b-123">**Egy Azure Active Directory-alkalmazás létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="ad30b-123">**Create an Azure Active Directory Application**.</span></span> <span data-ttu-id="ad30b-124">Hello Azure AD alkalmazás tooauthenticate hello Data Lake Store-alkalmazás használhatja az Azure ad-val.</span><span class="sxs-lookup"><span data-stu-id="ad30b-124">You use hello Azure AD application tooauthenticate hello Data Lake Store application with Azure AD.</span></span> <span data-ttu-id="ad30b-125">Nincsenek különböző szempontok tooauthenticate az Azure ad-vel, amelyek **végfelhasználói hitelesítési** vagy **szolgáltatások közötti hitelesítési**.</span><span class="sxs-lookup"><span data-stu-id="ad30b-125">There are different approaches tooauthenticate with Azure AD, which are **end-user authentication** or **service-to-service authentication**.</span></span> <span data-ttu-id="ad30b-126">További információt és útmutatást tooauthenticate, lásd: [végfelhasználói hitelesítési](data-lake-store-end-user-authenticate-using-active-directory.md) vagy [szolgáltatások közötti hitelesítési](data-lake-store-authenticate-using-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="ad30b-126">For instructions and more information on how tooauthenticate, see [End-user authentication](data-lake-store-end-user-authenticate-using-active-directory.md) or [Service-to-service authentication](data-lake-store-authenticate-using-active-directory.md).</span></span>

## <a name="how-tooinstall"></a><span data-ttu-id="ad30b-127">Hogyan tooInstall</span><span class="sxs-lookup"><span data-stu-id="ad30b-127">How tooInstall</span></span>
```bash
npm install azure-arm-datalake-store
```

## <a name="authenticate-using-azure-active-directory"></a><span data-ttu-id="ad30b-128">Hitelesítés az Azure Active Directory használatával</span><span class="sxs-lookup"><span data-stu-id="ad30b-128">Authenticate using Azure Active Directory</span></span>
<span data-ttu-id="ad30b-129">hello alábbi kódtöredékek megjelenítése a Data Lake Store hitelesítő külön kétféleképpen az Azure AD használatával.</span><span class="sxs-lookup"><span data-stu-id="ad30b-129">hello snippets below show two separate ways of authenticating with Data Lake Store using Azure AD.</span></span> <span data-ttu-id="ad30b-130">A hitelesítéshez a Data Lake Store különböző módszerek toouse részletes tárgyalását lásd: [hitelesítés a Data Lake Store az Azure Active Directoryval](data-lake-store-authenticate-using-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="ad30b-130">For a detailed discussion on various methods toouse for authentication with Data Lake Store, see [Authenticate with Data Lake Store using Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).</span></span>

<span data-ttu-id="ad30b-131">az alábbi hello részlet is hasonlóan az Azure AD-tartomány nevét, ügyfél-azonosító az Azure AD-alkalmazáshoz, más néven stb bemeneti értéket igényel. Ezek az adatok lekérhetők az Azure AD alkalmazás kell létrehozni, amelyek hello részletei is szerepelnek a fenti hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="ad30b-131">hello snippet below also requires inputs like Azure AD domain name, client ID for an Azure AD app, etc. All these details can be retrieved from an Azure AD application that you must created, hello details of which are also included in link above.</span></span>

 ```javascript
 var msrestAzure = require('ms-rest-azure');
 //user authentication
 var credentials = new msRestAzure.UserTokenCredentials('your-client-id', 'your-domain', 'your-username', 'your-password', 'your-redirect-uri');
 //service principal authentication
 var credentials = new msRestAzure.ApplicationTokenCredentials('your-client-id', 'your-domain', 'your-secret');
 ```

## <a name="create-hello-data-lake-store-clients"></a><span data-ttu-id="ad30b-132">Hello Data Lake Store ügyfelek létrehozása</span><span class="sxs-lookup"><span data-stu-id="ad30b-132">Create hello Data Lake Store Clients</span></span>
```javascript
var adlsManagement = require("azure-arm-datalake-store");
var acccountClient = new adlsManagement.DataLakeStoreAccountClient(credentials, "your-subscription-id");
var filesystemClient = new adlsManagement.DataLakeStoreFileSystemClient(credentials);
```

## <a name="create-a-data-lake-store-account"></a><span data-ttu-id="ad30b-133">Data Lake Store-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="ad30b-133">Create a Data Lake Store Account</span></span>
```javascript
var util = require('util');
var resourceGroupName = 'testrg';
var accountName = 'testadlsacct';
var location = 'eastus2';

// account object toocreate
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
    /*err has reference toohello actual request and response, so you can see what was sent and received on hello wire.
      hello structure of err looks like this:
      err: {
        code: 'Error Code',
        message: 'Error Message',
        body: 'hello response body if any',
        request: reference tooa stripped version of http request
        response: reference tooa stripped version of hello response
      }
    */
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## <a name="create-a-file-with-content"></a><span data-ttu-id="ad30b-134">Hozzon létre egy fájlt, amelyeknek a tartalma</span><span class="sxs-lookup"><span data-stu-id="ad30b-134">Create a file with content</span></span>
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

## <a name="get-a-list-of-files-and-folders"></a><span data-ttu-id="ad30b-135">Fájlok és mappák listájának lekérdezése</span><span class="sxs-lookup"><span data-stu-id="ad30b-135">Get a list of files and folders</span></span>
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

## <a name="see-also"></a><span data-ttu-id="ad30b-136">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="ad30b-136">See also</span></span>
* [<span data-ttu-id="ad30b-137">Microsoft Azure SDK for Node.js</span><span class="sxs-lookup"><span data-stu-id="ad30b-137">Microsoft Azure SDK for Node.js</span></span>](https://github.com/azure/azure-sdk-for-node)
* [<span data-ttu-id="ad30b-138">A Microsoft Azure SDK for Node.js – Data Lake Analytics kezelése</span><span class="sxs-lookup"><span data-stu-id="ad30b-138">Microsoft Azure SDK for Node.js - Data Lake Analytics Management</span></span>](https://www.npmjs.com/package/azure-arm-datalake-analytics)

