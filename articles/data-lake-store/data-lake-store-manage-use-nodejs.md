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
# <a name="get-started-with-azure-data-lake-store-using-azure-sdk-for-nodejs"></a>Ismerkedés az Azure Data Lake Store Azure SDK for Node.js használatával
> [!div class="op_single_selector"]
> * [Portal](data-lake-store-get-started-portal.md)
> * [PowerShell](data-lake-store-get-started-powershell.md)
> * [.NET SDK](data-lake-store-get-started-net-sdk.md)
> * [Java SDK](data-lake-store-get-started-java-sdk.md)
> * [REST API](data-lake-store-get-started-rest-api.md)
> * [Azure CLI 2.0](data-lake-store-get-started-cli-2.0.md)
> * [Node.js](data-lake-store-manage-use-nodejs.md)
> * [Python](data-lake-store-get-started-python.md)
>
> 

> [!NOTE]
> A feltöltését és letöltését nagy mennyiségű adatot (nagy fájlok, számos fájlok vagy mindkettőt), azt javasoljuk, hogy használja a [Python SDK](data-lake-store-get-started-python.md), a [.NET SDK](data-lake-store-get-started-net-sdk.md), [Azure CLI 2.0](data-lake-store-get-started-cli-2.0.md), vagy [Azure PowerShell](data-lake-store-get-started-powershell.md). Ezek a lehetőségek jobb teljesítményt biztosítanak, mivel több szálat használnak, így az adattovábbítás párhuzamosan folyhat.
> 
> 

Útmutató: Azure Data Lake Store-fiók létrehozása és alapszintű műveleteket, mint például mappák létrehozása, és feltöltése adatfájlok le az Azure SDK for Node.js használatával, a fiók törlése stb. További információk a Data Lake Store-ról: [Overview of Data Lake Store](data-lake-store-overview.md) (A Data Lake Store áttekintése). Jelenleg az SDK támogatja

* **Node.js-verzió: 0.10.0-s vagy újabb**
* **Fiókhoz tartozó REST API-verzió: 2015. 10. 01. előzetes verzió**
* **Fájlrendszer REST API-verzió: 2015-10-01. dátumú előnézeti**

## <a name="prerequisites"></a>Előfeltételek
A cikk elkezdéséhez az alábbiakkal kell rendelkeznie:

* **Azure-előfizetés**. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).
* **Egy Azure Active Directory-alkalmazás létrehozása**. A Data Lake Store alkalmazás Azure AD-val történő hitelesítéséhez az Azure AD alkalmazást kell használni. Az Azure AD-val többféle módon is lehet hitelesíteni. Ezek a következők: **végfelhasználói hitelesítés** vagy **szolgáltatások közötti hitelesítés**. A hitelesítéssel kapcsolatban a [Végfelhasználói hitelesítés](data-lake-store-end-user-authenticate-using-active-directory.md) vagy a [Szolgáltatások közötti hitelesítés](data-lake-store-authenticate-using-active-directory.md) című témakörben talál útmutatást és további tudnivalókat.

## <a name="how-to-install"></a>A telepítés módja
```bash
npm install azure-arm-datalake-store
```

## <a name="authenticate-using-azure-active-directory"></a>Hitelesítés az Azure Active Directory használatával
Az alábbi részletek megjelenítése a Data Lake Store hitelesítő külön kétféleképpen az Azure AD. A hitelesítéshez a Data Lake Store különböző módszerek részletes leírását lásd: [hitelesítés a Data Lake Store az Azure Active Directoryval](data-lake-store-authenticate-using-active-directory.md).

Az alábbi részlet is szükséges bemeneti adatok, például az Azure AD-tartomány nevét, ügyfél-azonosító az Azure AD-alkalmazáshoz, stb. Ezek az adatok lekérhetők az Azure AD alkalmazás kell létrehozni, a részleteit is szerepelnek a fenti hivatkozásra.

 ```javascript
 var msrestAzure = require('ms-rest-azure');
 //user authentication
 var credentials = new msRestAzure.UserTokenCredentials('your-client-id', 'your-domain', 'your-username', 'your-password', 'your-redirect-uri');
 //service principal authentication
 var credentials = new msRestAzure.ApplicationTokenCredentials('your-client-id', 'your-domain', 'your-secret');
 ```

## <a name="create-the-data-lake-store-clients"></a>A Data Lake Store ügyfelek létrehozása
```javascript
var adlsManagement = require("azure-arm-datalake-store");
var acccountClient = new adlsManagement.DataLakeStoreAccountClient(credentials, "your-subscription-id");
var filesystemClient = new adlsManagement.DataLakeStoreFileSystemClient(credentials);
```

## <a name="create-a-data-lake-store-account"></a>Data Lake Store-fiók létrehozása
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

## <a name="create-a-file-with-content"></a>Hozzon létre egy fájlt, amelyeknek a tartalma
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

## <a name="get-a-list-of-files-and-folders"></a>Fájlok és mappák listájának lekérdezése
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

## <a name="see-also"></a>Lásd még:
* [Microsoft Azure SDK for Node.js](https://github.com/azure/azure-sdk-for-node)
* [A Microsoft Azure SDK for Node.js – Data Lake Analytics kezelése](https://www.npmjs.com/package/azure-arm-datalake-analytics)

