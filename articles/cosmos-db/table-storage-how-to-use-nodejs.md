---
title: "Node.js-ből a Azure Table storage használata |} Microsoft Docs"
description: "Az Azure Table Storage, amely egy NoSQL-adattár, a strukturált adatok felhőben való tárolásához használható."
services: cosmos-db
documentationcenter: nodejs
author: mimig1
manager: jhubbard
editor: tysonn
ms.assetid: fc2e33d2-c5da-4861-8503-53fdc25750de
ms.service: cosmos-db
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 12/08/2016
ms.author: mimig
ms.openlocfilehash: 539212c6abe7738c022d67245f8992516f0899ff
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-azure-table-storage-from-nodejs"></a><span data-ttu-id="faad1-103">Node.js-ből a Azure Table storage használata</span><span class="sxs-lookup"><span data-stu-id="faad1-103">How to use Azure Table storage from Node.js</span></span>
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

## <a name="overview"></a><span data-ttu-id="faad1-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="faad1-104">Overview</span></span>
<span data-ttu-id="faad1-105">Ez a témakör bemutatja, hogyan hajthat végre a gyakori forgatókönyvek az Azure Table szolgáltatás használata Node.js-alkalmazásokban.</span><span class="sxs-lookup"><span data-stu-id="faad1-105">This topic shows how to perform common scenarios using the Azure Table service in a Node.js application.</span></span>

<span data-ttu-id="faad1-106">Ebben a témakörben szereplő példák azt feltételezik, hogy már rendelkezik egy Node.js-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="faad1-106">The code examples in this topic assume you already have a Node.js application.</span></span> <span data-ttu-id="faad1-107">További információ a Node.js-alkalmazás létrehozása az Azure-ban jelennek meg ezek a témakörök:</span><span class="sxs-lookup"><span data-stu-id="faad1-107">For information about how to create a Node.js application in Azure, see any of these topics:</span></span>

* [<span data-ttu-id="faad1-108">Node.js-webalkalmazás létrehozása az Azure App Service-ben</span><span class="sxs-lookup"><span data-stu-id="faad1-108">Create a Node.js web app in Azure App Service</span></span>](../app-service-web/app-service-web-get-started-nodejs.md)
* [<span data-ttu-id="faad1-109">Hozza létre, és a Node.js webalkalmazás telepítése az Azure WebMatrix használatával.</span><span class="sxs-lookup"><span data-stu-id="faad1-109">Build and deploy a Node.js web app to Azure using WebMatrix</span></span>](../app-service-web/web-sites-nodejs-use-webmatrix.md)
* <span data-ttu-id="faad1-110">[Hozza létre és telepítheti az Azure Cloud Service a Node.js-alkalmazás](../cloud-services/cloud-services-nodejs-develop-deploy-app.md) (a Windows PowerShell használatával)</span><span class="sxs-lookup"><span data-stu-id="faad1-110">[Build and deploy a Node.js application to an Azure Cloud Service](../cloud-services/cloud-services-nodejs-develop-deploy-app.md) (using Windows PowerShell)</span></span>

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="configure-your-application-to-access-azure-storage"></a><span data-ttu-id="faad1-111">Állítsa be az alkalmazását Azure Storage eléréséhez</span><span class="sxs-lookup"><span data-stu-id="faad1-111">Configure your application to access Azure Storage</span></span>
<span data-ttu-id="faad1-112">Azure Storage használatához szüksége az Azure Storage szolgáltatás SDK for Node.js, amely tartalmaz egy kényelmi szalagtár szerepel, amely a többi tárolási szolgáltatásokkal kommunikálni.</span><span class="sxs-lookup"><span data-stu-id="faad1-112">To use Azure Storage, you need the Azure Storage SDK for Node.js, which includes a set of convenience libraries that communicate with the storage REST services.</span></span>

### <a name="use-node-package-manager-npm-to-install-the-package"></a><span data-ttu-id="faad1-113">A csomag telepítése a csomópont Package Manager (NPM) használatával</span><span class="sxs-lookup"><span data-stu-id="faad1-113">Use Node Package Manager (NPM) to install the package</span></span>
1. <span data-ttu-id="faad1-114">Használjon például egy parancssori felületet **PowerShell** (Windows), **Terminálszolgáltatások** (Mac), vagy **Bash** (Unix) eszközt, és lépjen abba a mappába, amelyben létrehozta az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="faad1-114">Use a command-line interface such as **PowerShell** (Windows), **Terminal** (Mac), or **Bash** (Unix), and navigate to the folder where you created your application.</span></span>
2. <span data-ttu-id="faad1-115">Típus **npm telepítése azure-tároló** a parancsablakban.</span><span class="sxs-lookup"><span data-stu-id="faad1-115">Type **npm install azure-storage** in the command window.</span></span> <span data-ttu-id="faad1-116">A parancs kimenetében a következőhöz hasonló.</span><span class="sxs-lookup"><span data-stu-id="faad1-116">Output from the command is similar to the following example.</span></span>

       azure-storage@0.5.0 node_modules\azure-storage
       +-- extend@1.2.1
       +-- xmlbuilder@0.4.3
       +-- mime@1.2.11
       +-- node-uuid@1.4.3
       +-- validator@3.22.2
       +-- underscore@1.4.4
       +-- readable-stream@1.0.33 (string_decoder@0.10.31, isarray@0.0.1, inherits@2.0.1, core-util-is@1.0.1)
       +-- xml2js@0.2.7 (sax@0.5.2)
       +-- request@2.57.0 (caseless@0.10.0, aws-sign2@0.5.0, forever-agent@0.6.1, stringstream@0.0.4, oauth-sign@0.8.0, tunnel-agent@0.4.1, isstream@0.1.2, json-stringify-safe@5.0.1, bl@0.9.4, combined-stream@1.0.5, qs@3.1.0, mime-types@2.0.14, form-data@0.2.0, http-signature@0.11.0, tough-cookie@2.0.0, hawk@2.3.1, har-validator@1.8.0)
3. <span data-ttu-id="faad1-117">Manuálisan futtatható a **ls** parancs futtatásával ellenőrizze, hogy egy **csomópont\_modulok** mappa hozták létre.</span><span class="sxs-lookup"><span data-stu-id="faad1-117">You can manually run the **ls** command to verify that a **node\_modules** folder was created.</span></span> <span data-ttu-id="faad1-118">A mappában megtalálja az **azure-tároló** csomagot, amely tartalmazza a könyvtárak kell érniük a tárhelyet.</span><span class="sxs-lookup"><span data-stu-id="faad1-118">Inside that folder you will find the **azure-storage** package, which contains the libraries you need to access storage.</span></span>

### <a name="import-the-package"></a><span data-ttu-id="faad1-119">A csomag importálása</span><span class="sxs-lookup"><span data-stu-id="faad1-119">Import the package</span></span>
<span data-ttu-id="faad1-120">Adja hozzá a következő kódot a felső részén a **server.js** fájl az alkalmazásban:</span><span class="sxs-lookup"><span data-stu-id="faad1-120">Add the following code to the top of the **server.js** file in your application:</span></span>

```nodejs
var azure = require('azure-storage');
```

## <a name="set-up-an-azure-storage-connection"></a><span data-ttu-id="faad1-121">Egy Azure Storage-kapcsolat beállítása</span><span class="sxs-lookup"><span data-stu-id="faad1-121">Set up an Azure Storage connection</span></span>
<span data-ttu-id="faad1-122">Az azure-moduljának olvassák a környezeti változók AZURE\_tárolási\_FIÓKOT és az AZURE\_tárolási\_hozzáférés\_kulcs, vagy AZURE\_tárolási\_kapcsolat\_KARAKTERLÁNCOT az Azure storage-fiókhoz való kapcsolódáshoz szükséges adatokat.</span><span class="sxs-lookup"><span data-stu-id="faad1-122">The azure module will read the environment variables AZURE\_STORAGE\_ACCOUNT and AZURE\_STORAGE\_ACCESS\_KEY, or AZURE\_STORAGE\_CONNECTION\_STRING for information required to connect to your Azure storage account.</span></span> <span data-ttu-id="faad1-123">Ha ezek a környezeti változók nem, meg kell adnia a fiókadatokat, meghívásakor **TableService**.</span><span class="sxs-lookup"><span data-stu-id="faad1-123">If these environment variables are not set, you must specify the account information when calling **TableService**.</span></span>

<span data-ttu-id="faad1-124">A környezeti változók beállítása példát a [Azure-portálon](https://portal.azure.com) egy Azure-webhelyre, lásd: [az Azure Table szolgáltatás használata Node.js-webalkalmazás](../app-service-web/storage-nodejs-use-table-storage-web-site.md).</span><span class="sxs-lookup"><span data-stu-id="faad1-124">For an example of setting the environment variables in the [Azure portal](https://portal.azure.com) for an Azure Website, see [Node.js web app using the Azure Table Service](../app-service-web/storage-nodejs-use-table-storage-web-site.md).</span></span>

## <a name="create-a-table"></a><span data-ttu-id="faad1-125">Tábla létrehozása</span><span class="sxs-lookup"><span data-stu-id="faad1-125">Create a table</span></span>
<span data-ttu-id="faad1-126">Az alábbi kód létrehoz egy **TableService** objektumot, és hozzon létre egy új táblát használja.</span><span class="sxs-lookup"><span data-stu-id="faad1-126">The following code creates a **TableService** object and uses it to create a new table.</span></span> <span data-ttu-id="faad1-127">Adja hozzá a következő tetejénél található **server.js**.</span><span class="sxs-lookup"><span data-stu-id="faad1-127">Add the following near the top of **server.js**.</span></span>

```nodejs
var tableSvc = azure.createTableService();
```

<span data-ttu-id="faad1-128">A hívás **createTableIfNotExists** új táblát hoz létre a megadott névvel, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="faad1-128">The call to **createTableIfNotExists** will create a new table with the specified name if it does not already exist.</span></span> <span data-ttu-id="faad1-129">Az alábbi példában a "mytable" nevű, ha még nem létezik új táblázat létrehozása:</span><span class="sxs-lookup"><span data-stu-id="faad1-129">The following example creates a new table named 'mytable' if it does not already exist:</span></span>

```nodejs
tableSvc.createTableIfNotExists('mytable', function(error, result, response){
  if(!error){
    // Table exists or created
  }
});
```

<span data-ttu-id="faad1-130">A `result.created` lesz `true` Ha új tábla létrehozása, és `false` Ha a tábla már létezik.</span><span class="sxs-lookup"><span data-stu-id="faad1-130">The `result.created` will be `true` if a new table is created, and `false` if the table already exists.</span></span> <span data-ttu-id="faad1-131">A `response` a kérelem információkat tartalmaznak.</span><span class="sxs-lookup"><span data-stu-id="faad1-131">The `response` will contain information about the request.</span></span>

### <a name="filters"></a><span data-ttu-id="faad1-132">Szűrők</span><span class="sxs-lookup"><span data-stu-id="faad1-132">Filters</span></span>
<span data-ttu-id="faad1-133">Választható szűrési műveletek használatával végrehajtott műveletek alkalmazhatók **TableService**.</span><span class="sxs-lookup"><span data-stu-id="faad1-133">Optional filtering operations can be applied to operations performed using **TableService**.</span></span> <span data-ttu-id="faad1-134">Műveletek szűrésének lehetnek naplózási, automatikus újrapróbálkozása, stb. A metódus aláírása megvalósító objektumok szűrők a következők:</span><span class="sxs-lookup"><span data-stu-id="faad1-134">Filtering operations can include logging, automatically retrying, etc. Filters are objects that implement a method with the signature:</span></span>

```nodejs
function handle (requestOptions, next)
```

<span data-ttu-id="faad1-135">Ezután a előfeldolgozása kérelmet a beállítások, a metódus meg kell hívni a "Tovább" gombra, átadja a következő aláírással rendelkező visszahívás:</span><span class="sxs-lookup"><span data-stu-id="faad1-135">After doing its preprocessing on the request options, the method needs to call "next", passing a callback with the following signature:</span></span>

```nodejs
function (returnObject, finalCallback, next)
```

<span data-ttu-id="faad1-136">A visszahívási, és a returnObject (válasza a kérés a kiszolgáló) feldolgozása után a visszahívás kell mellett meghívni, ha más szűrők feldolgozásának folytatásához létezik, vagy egyszerűen meghívása finalCallback ellenkező esetben a szolgáltatás hívás befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="faad1-136">In this callback, and after processing the returnObject (the response from the request to the server), the callback needs to either invoke next if it exists to continue processing other filters or simply invoke finalCallback otherwise to end the service invocation.</span></span>

<span data-ttu-id="faad1-137">Két szűrőket, amelyek megvalósítják az újrapróbálkozási logika érhetők el az Azure SDK for Node.js, a **ExponentialRetryPolicyFilter** és **LinearRetryPolicyFilter**.</span><span class="sxs-lookup"><span data-stu-id="faad1-137">Two filters that implement retry logic are included with the Azure SDK for Node.js, **ExponentialRetryPolicyFilter** and **LinearRetryPolicyFilter**.</span></span> <span data-ttu-id="faad1-138">A következő létrehoz egy **TableService** objektum, amely használja a **ExponentialRetryPolicyFilter**:</span><span class="sxs-lookup"><span data-stu-id="faad1-138">The following creates a **TableService** object that uses the **ExponentialRetryPolicyFilter**:</span></span>

```nodejs
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var tableSvc = azure.createTableService().withFilter(retryOperations);
```

## <a name="add-an-entity-to-a-table"></a><span data-ttu-id="faad1-139">Entitás hozzáadása a táblához</span><span class="sxs-lookup"><span data-stu-id="faad1-139">Add an entity to a table</span></span>
<span data-ttu-id="faad1-140">Egy entitás hozzáadásához először létre kell hoznia egy objektum, amely meghatározza az entitás tulajdonságai.</span><span class="sxs-lookup"><span data-stu-id="faad1-140">To add an entity, first create an object that defines your entity properties.</span></span> <span data-ttu-id="faad1-141">Összes entitás tartalmaznia kell egy **PartitionKey** és **RowKey**, amelyeket az entitáshoz tartozó egyedi azonosítóként.</span><span class="sxs-lookup"><span data-stu-id="faad1-141">All entities must contain a **PartitionKey** and **RowKey**, which are unique identifiers for the entity.</span></span>

* <span data-ttu-id="faad1-142">**PartitionKey** -meghatározza a partíció az entitás tárolt</span><span class="sxs-lookup"><span data-stu-id="faad1-142">**PartitionKey** - determines the partition that the entity is stored in</span></span>
* <span data-ttu-id="faad1-143">**RowKey** – egyedi módon azonosítja az entitást a partíción belül</span><span class="sxs-lookup"><span data-stu-id="faad1-143">**RowKey** - uniquely identifies the entity within the partition</span></span>

<span data-ttu-id="faad1-144">Mindkét **PartitionKey** és **RowKey** karakterlánc-értékekkel kell lennie.</span><span class="sxs-lookup"><span data-stu-id="faad1-144">Both **PartitionKey** and **RowKey** must be string values.</span></span> <span data-ttu-id="faad1-145">További információkért lásd: [ismertetése a Table szolgáltatás adatmodell](http://msdn.microsoft.com/library/azure/dd179338.aspx).</span><span class="sxs-lookup"><span data-stu-id="faad1-145">For more information, see [Understanding the Table Service Data Model](http://msdn.microsoft.com/library/azure/dd179338.aspx).</span></span>

<span data-ttu-id="faad1-146">A következő példában látható egy entitás meghatározásának.</span><span class="sxs-lookup"><span data-stu-id="faad1-146">The following is an example of defining an entity.</span></span> <span data-ttu-id="faad1-147">Vegye figyelembe, hogy **dueDate** van definiálva, egyfajta **Edm.DateTime**.</span><span class="sxs-lookup"><span data-stu-id="faad1-147">Note that **dueDate** is defined as a type of **Edm.DateTime**.</span></span> <span data-ttu-id="faad1-148">A típus megadása nem kötelező, és típusok fog lehet következtetni rá, ha nincs megadva.</span><span class="sxs-lookup"><span data-stu-id="faad1-148">Specifying the type is optional, and types will be inferred if not specified.</span></span>

```nodejs
var task = {
  PartitionKey: {'_':'hometasks'},
  RowKey: {'_': '1'},
  description: {'_':'take out the trash'},
  dueDate: {'_':new Date(2015, 6, 20), '$':'Edm.DateTime'}
};
```

> [!NOTE]
> <span data-ttu-id="faad1-149">Szerepel továbbá egy **időbélyeg** mezőjét az egyes rekordokhoz, amely az Azure-ban van beállítva, ha entitás beszúrni vagy frissíteni.</span><span class="sxs-lookup"><span data-stu-id="faad1-149">There is also a **Timestamp** field for each record, which is set by Azure when an entity is inserted or updated.</span></span>
>
>

<span data-ttu-id="faad1-150">Használhatja a **entityGenerator** entitások létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="faad1-150">You can also use the **entityGenerator** to create entities.</span></span> <span data-ttu-id="faad1-151">Az alábbi példakód létrehozza a azonos feladat entitás használatával a **entityGenerator**.</span><span class="sxs-lookup"><span data-stu-id="faad1-151">The following example creates the same task entity using the **entityGenerator**.</span></span>

```nodejs
var entGen = azure.TableUtilities.entityGenerator;
var task = {
  PartitionKey: entGen.String('hometasks'),
  RowKey: entGen.String('1'),
  description: entGen.String('take out the trash'),
  dueDate: entGen.DateTime(new Date(Date.UTC(2015, 6, 20))),
};
```

<span data-ttu-id="faad1-152">Egy entitás hozzáadása a táblához, adja át a forrásentitás-objektum a **insertEntity** metódust.</span><span class="sxs-lookup"><span data-stu-id="faad1-152">To add an entity to your table, pass the entity object to the **insertEntity** method.</span></span>

```nodejs
tableSvc.insertEntity('mytable',task, function (error, result, response) {
  if(!error){
    // Entity inserted
  }
});
```

<span data-ttu-id="faad1-153">Ha a művelet sikeres, `result` tartalmazni fogja a [ETag](http://en.wikipedia.org/wiki/HTTP_ETag) a beszúrt rekordok és `response` működésével kapcsolatos adatokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="faad1-153">If the operation is successful, `result` will contain the [ETag](http://en.wikipedia.org/wiki/HTTP_ETag) of the inserted record and `response` will contain information about the operation.</span></span>

<span data-ttu-id="faad1-154">Példa egy válasz:</span><span class="sxs-lookup"><span data-stu-id="faad1-154">Example response:</span></span>

```nodejs
{ '.metadata': { etag: 'W/"datetime\'2015-02-25T01%3A22%3A22.5Z\'"' } }
```

> [!NOTE]
> <span data-ttu-id="faad1-155">Alapértelmezés szerint **insertEntity** nem ad vissza a beszúrt entitás részeként a `response` információkat.</span><span class="sxs-lookup"><span data-stu-id="faad1-155">By default, **insertEntity** does not return the inserted entity as part of the `response` information.</span></span> <span data-ttu-id="faad1-156">Ha más műveleteket végez ehhez az entitáshoz tervez, vagy szeretné gyorsítótárazza az információkat, azok hasznos lehet akkor adja vissza részeként a `result`.</span><span class="sxs-lookup"><span data-stu-id="faad1-156">If you plan on performing other operations on this entity, or wish to cache the information, it can be useful to have it returned as part of the `result`.</span></span> <span data-ttu-id="faad1-157">Ehhez engedélyezésével **echoContent** az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="faad1-157">You can do this by enabling **echoContent** as follows:</span></span>
>
> `tableSvc.insertEntity('mytable', task, {echoContent: true}, function (error, result, response) {...}`
>
>

## <a name="update-an-entity"></a><span data-ttu-id="faad1-158">Egy entitás frissítése</span><span class="sxs-lookup"><span data-stu-id="faad1-158">Update an entity</span></span>
<span data-ttu-id="faad1-159">Több módszer érhető el frissíteni a meglévő entitás áll rendelkezésre:</span><span class="sxs-lookup"><span data-stu-id="faad1-159">There are multiple methods available to update an existing entity:</span></span>

* <span data-ttu-id="faad1-160">**replaceEntity** -cserélje ki egy meglévő entitás frissítéséhez</span><span class="sxs-lookup"><span data-stu-id="faad1-160">**replaceEntity** - updates an existing entity by replacing it</span></span>
* <span data-ttu-id="faad1-161">**mergeEntity** -meglévő entitás frissíti új tulajdonságértékek egyesíti a meglévő entitás</span><span class="sxs-lookup"><span data-stu-id="faad1-161">**mergeEntity** - updates an existing entity by merging new property values into the existing entity</span></span>
* <span data-ttu-id="faad1-162">**insertOrReplaceEntity** -cserélje ki egy meglévő entitás frissíti.</span><span class="sxs-lookup"><span data-stu-id="faad1-162">**insertOrReplaceEntity** - updates an existing entity by replacing it.</span></span> <span data-ttu-id="faad1-163">Ha nem entitás létezik, egy új szúrja be a</span><span class="sxs-lookup"><span data-stu-id="faad1-163">If no entity exists, a new one will be inserted</span></span>
* <span data-ttu-id="faad1-164">**insertOrMergeEntity** -meglévő entitás frissíti a meglévő tulajdonságértékek új egyesíti.</span><span class="sxs-lookup"><span data-stu-id="faad1-164">**insertOrMergeEntity** - updates an existing entity by merging new property values into the existing.</span></span> <span data-ttu-id="faad1-165">Ha nem entitás létezik, egy új szúrja be a</span><span class="sxs-lookup"><span data-stu-id="faad1-165">If no entity exists, a new one will be inserted</span></span>

<span data-ttu-id="faad1-166">A következő példa bemutatja, hogy egy entitás használatával frissítése **replaceEntity**:</span><span class="sxs-lookup"><span data-stu-id="faad1-166">The following example demonstrates updating an entity using **replaceEntity**:</span></span>

```nodejs
tableSvc.replaceEntity('mytable', updatedTask, function(error, result, response){
  if(!error) {
    // Entity updated
  }
});
```

> [!NOTE]
> <span data-ttu-id="faad1-167">Alapértelmezés szerint egy entitás frissítése nem ellenőrizze, hogy ha az adatok frissítése során korábban módosították egy másik folyamat.</span><span class="sxs-lookup"><span data-stu-id="faad1-167">By default, updating an entity does not check to see if the data being updated has previously been modified by another process.</span></span> <span data-ttu-id="faad1-168">Támogatja a párhuzamos frissítések:</span><span class="sxs-lookup"><span data-stu-id="faad1-168">To support concurrent updates:</span></span>
>
> 1. <span data-ttu-id="faad1-169">Az ETag a frissítendő objektum beolvasása.</span><span class="sxs-lookup"><span data-stu-id="faad1-169">Get the ETag of the object being updated.</span></span> <span data-ttu-id="faad1-170">Ennek részeként adja vissza a `response` entitás kapcsolatos műveletek és kérhető `response['.metadata'].etag`.</span><span class="sxs-lookup"><span data-stu-id="faad1-170">This is returned as part of the `response` for any entity-related operation and can be retrieved through `response['.metadata'].etag`.</span></span>
> 2. <span data-ttu-id="faad1-171">Entitás frissítési művelet végrehajtásakor adja hozzá az új entitást korábban lekért ETag adatok.</span><span class="sxs-lookup"><span data-stu-id="faad1-171">When performing an update operation on an entity, add the ETag information previously retrieved to the new entity.</span></span> <span data-ttu-id="faad1-172">Példa:</span><span class="sxs-lookup"><span data-stu-id="faad1-172">For example:</span></span>
>
>       <span data-ttu-id="faad1-173">entity2 [.metadata] .etag = currentEtag;</span><span class="sxs-lookup"><span data-stu-id="faad1-173">entity2['.metadata'].etag = currentEtag;</span></span>
> 3. <span data-ttu-id="faad1-174">A frissítési műveletet végrehajtani.</span><span class="sxs-lookup"><span data-stu-id="faad1-174">Perform the update operation.</span></span> <span data-ttu-id="faad1-175">Ha az entitást az ETag érték, például az alkalmazás egy másik példánya lekért óta módosították egy `error` az eredmény figyelmezteti a felhasználókat arra, hogy a kérelemben megadott frissítés feltétel nem teljesült.</span><span class="sxs-lookup"><span data-stu-id="faad1-175">If the entity has been modified since you retrieved the ETag value, such as another instance of your application, an `error` will be returned stating that the update condition specified in the request was not satisfied.</span></span>
>
>

<span data-ttu-id="faad1-176">A **replaceEntity** és **mergeEntity**, ha a frissítendő entitás nem létezik, akkor a frissítési művelet sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="faad1-176">With **replaceEntity** and **mergeEntity**, if the entity that is being updated doesn't exist, then the update operation will fail.</span></span> <span data-ttu-id="faad1-177">Ezért ha egy entitás tárolni kívánt függetlenül attól, hogy létezik-e, használja **insertOrReplaceEntity** vagy **insertOrMergeEntity**.</span><span class="sxs-lookup"><span data-stu-id="faad1-177">Therefore if you wish to store an entity regardless of whether it already exists, use **insertOrReplaceEntity** or **insertOrMergeEntity**.</span></span>

<span data-ttu-id="faad1-178">A `result` sikeres frissítés műveletek fogja tartalmazni az **Etag** a frissített entitás.</span><span class="sxs-lookup"><span data-stu-id="faad1-178">The `result` for successful update operations will contain the **Etag** of the updated entity.</span></span>

## <a name="work-with-groups-of-entities"></a><span data-ttu-id="faad1-179">Az entitások csoportok használata</span><span class="sxs-lookup"><span data-stu-id="faad1-179">Work with groups of entities</span></span>
<span data-ttu-id="faad1-180">Egyes esetekben érdemes elküldeni a több műveletei együtt egy kötegelt atomi feldolgozása a kiszolgáló biztosításához.</span><span class="sxs-lookup"><span data-stu-id="faad1-180">Sometimes it makes sense to submit multiple operations together in a batch to ensure atomic processing by the server.</span></span> <span data-ttu-id="faad1-181">Hajthatja végre, hogy, használja a **TableBatch** osztály létrehozza, és használja a **executeBatch** metódusában **TableService** a kötegelt műveletek végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="faad1-181">To accomplish that, use the **TableBatch** class to create a batch, and then use the **executeBatch** method of **TableService** to perform the batched operations.</span></span>

 <span data-ttu-id="faad1-182">A következő példa bemutatja, hogy egy kötegelt két entitása elküldése:</span><span class="sxs-lookup"><span data-stu-id="faad1-182">The following example demonstrates submitting two entities in a batch:</span></span>

```nodejs
var task1 = {
  PartitionKey: {'_':'hometasks'},
  RowKey: {'_': '1'},
  description: {'_':'Take out the trash'},
  dueDate: {'_':new Date(2015, 6, 20)}
};
var task2 = {
  PartitionKey: {'_':'hometasks'},
  RowKey: {'_': '2'},
  description: {'_':'Wash the dishes'},
  dueDate: {'_':new Date(2015, 6, 20)}
};

var batch = new azure.TableBatch();

batch.insertEntity(task1, {echoContent: true});
batch.insertEntity(task2, {echoContent: true});

tableSvc.executeBatch('mytable', batch, function (error, result, response) {
  if(!error) {
    // Batch completed
  }
});
```

<span data-ttu-id="faad1-183">A sikeres kötegelt műveleteket `result` a kötegben lévő egyes műveletek információt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="faad1-183">For successful batch operations, `result` will contain information for each operation in the batch.</span></span>

### <a name="work-with-batched-operations"></a><span data-ttu-id="faad1-184">A kötegelt műveletek használata</span><span class="sxs-lookup"><span data-stu-id="faad1-184">Work with batched operations</span></span>
<span data-ttu-id="faad1-185">Egy kötegelt hozzá műveletek megtekintésével meg lehessen vizsgálni a `operations` tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="faad1-185">Operations added to a batch can be inspected by viewing the `operations` property.</span></span> <span data-ttu-id="faad1-186">Az alábbi eljárások segítségével műveletek használata:</span><span class="sxs-lookup"><span data-stu-id="faad1-186">You can also use the following methods to work with operations:</span></span>

* <span data-ttu-id="faad1-187">**Törölje a jelet** -egy köteg az összes művelet törlése</span><span class="sxs-lookup"><span data-stu-id="faad1-187">**clear** - clears all operations from a batch</span></span>
* <span data-ttu-id="faad1-188">**getOperations** -művelet lekérdezi a kötegből</span><span class="sxs-lookup"><span data-stu-id="faad1-188">**getOperations** - gets an operation from the batch</span></span>
* <span data-ttu-id="faad1-189">**hasOperations** -ad vissza IGAZ, ha a köteg műveleteket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="faad1-189">**hasOperations** - returns true if the batch contains operations</span></span>
* <span data-ttu-id="faad1-190">**removeOperations** -eltávolítja egy műveletet</span><span class="sxs-lookup"><span data-stu-id="faad1-190">**removeOperations** - removes an operation</span></span>
* <span data-ttu-id="faad1-191">**méret** -műveletek számát adja vissza a kötegben</span><span class="sxs-lookup"><span data-stu-id="faad1-191">**size** - returns the number of operations in the batch</span></span>

## <a name="retrieve-an-entity-by-key"></a><span data-ttu-id="faad1-192">Egy entitás kulcs letöltése</span><span class="sxs-lookup"><span data-stu-id="faad1-192">Retrieve an entity by key</span></span>
<span data-ttu-id="faad1-193">Egy adott entitás alapján vissza a **PartitionKey** és **RowKey**, használja a **retrieveEntity** metódust.</span><span class="sxs-lookup"><span data-stu-id="faad1-193">To return a specific entity based on the **PartitionKey** and **RowKey**, use the **retrieveEntity** method.</span></span>

```nodejs
tableSvc.retrieveEntity('mytable', 'hometasks', '1', function(error, result, response){
  if(!error){
    // result contains the entity
  }
});
```

<span data-ttu-id="faad1-194">Ez a művelet befejeződése után `result` fogja tartalmazni az entitás.</span><span class="sxs-lookup"><span data-stu-id="faad1-194">Once this operation is complete, `result` will contain the entity.</span></span>

## <a name="query-a-set-of-entities"></a><span data-ttu-id="faad1-195">Az entitások készletének lekérdezése</span><span class="sxs-lookup"><span data-stu-id="faad1-195">Query a set of entities</span></span>
<span data-ttu-id="faad1-196">Ha egy táblából, használja a **TableQuery** objektumot egy lekérdezési kifejezésben, a következő záradék használatával kialakításához:</span><span class="sxs-lookup"><span data-stu-id="faad1-196">To query a table, use the **TableQuery** object to build up a query expression using the following clauses:</span></span>

* <span data-ttu-id="faad1-197">**Válassza ki** – a lekérdezés által visszaadott mezők</span><span class="sxs-lookup"><span data-stu-id="faad1-197">**select** - the fields to be returned from the query</span></span>
* <span data-ttu-id="faad1-198">**Ha** -a where záradék</span><span class="sxs-lookup"><span data-stu-id="faad1-198">**where** - the where clause</span></span>

  * <span data-ttu-id="faad1-199">**és** - `and` where feltétel</span><span class="sxs-lookup"><span data-stu-id="faad1-199">**and** - an `and` where condition</span></span>
  * <span data-ttu-id="faad1-200">**vagy** - `or` where feltétel</span><span class="sxs-lookup"><span data-stu-id="faad1-200">**or** - an `or` where condition</span></span>
* <span data-ttu-id="faad1-201">**felső** -beolvasási elemek száma.</span><span class="sxs-lookup"><span data-stu-id="faad1-201">**top** - the number of items to fetch</span></span>

<span data-ttu-id="faad1-202">Az alábbi példában egy lekérdezést, amely visszaállítja a felső öt cikkeket a PartitionKey "hometasks" hoz létre.</span><span class="sxs-lookup"><span data-stu-id="faad1-202">The following example builds a query that will return the top five items with a PartitionKey of 'hometasks'.</span></span>

```nodejs
var query = new azure.TableQuery()
  .top(5)
  .where('PartitionKey eq ?', 'hometasks');
```

<span data-ttu-id="faad1-203">Mivel a **válasszon** nem használ, akkor minden mező adja vissza.</span><span class="sxs-lookup"><span data-stu-id="faad1-203">Since **select** is not used, all fields will be returned.</span></span> <span data-ttu-id="faad1-204">A lekérdezés egy táblázaton végrehajtásához használja **queryEntities**.</span><span class="sxs-lookup"><span data-stu-id="faad1-204">To perform the query against a table, use **queryEntities**.</span></span> <span data-ttu-id="faad1-205">Az alábbi példában a lekérdezés a "mytable" ad vissza entitásokat.</span><span class="sxs-lookup"><span data-stu-id="faad1-205">The following example uses this query to return entities from 'mytable'.</span></span>

```nodejs
tableSvc.queryEntities('mytable',query, null, function(error, result, response) {
  if(!error) {
    // query was successful
  }
});
```

<span data-ttu-id="faad1-206">Ha sikeres, `result.entries` fogja tartalmazni, amelyek megfelelnek a lekérdezés entitások tömbjét.</span><span class="sxs-lookup"><span data-stu-id="faad1-206">If successful, `result.entries` will contain an array of entities that match the query.</span></span> <span data-ttu-id="faad1-207">Ha a lekérdezés nem tudta ad vissza az összes entitásokat `result.continuationToken` lesz nem -*null* és a harmadik paramétere használható **queryEntities** további eredmények beolvasásához.</span><span class="sxs-lookup"><span data-stu-id="faad1-207">If the query was unable to return all entities, `result.continuationToken` will be non-*null* and can be used as the third parameter of **queryEntities** to retrieve more results.</span></span> <span data-ttu-id="faad1-208">A kezdeti lekérdezés használata *null* a harmadik paraméter.</span><span class="sxs-lookup"><span data-stu-id="faad1-208">For the initial query, use *null* for the third parameter.</span></span>

### <a name="query-a-subset-of-entity-properties"></a><span data-ttu-id="faad1-209">Az entitástulajdonságok egy részének lekérdezése</span><span class="sxs-lookup"><span data-stu-id="faad1-209">Query a subset of entity properties</span></span>
<span data-ttu-id="faad1-210">A lekérdezés egy táblához pár mezők kérhetnek le egy entitás.</span><span class="sxs-lookup"><span data-stu-id="faad1-210">A query to a table can retrieve just a few fields from an entity.</span></span>
<span data-ttu-id="faad1-211">Ez csökkenti a sávszélesség, és javíthatja a lekérdezési teljesítményt, különösen olyan nagyméretű entitásokat.</span><span class="sxs-lookup"><span data-stu-id="faad1-211">This reduces bandwidth and can improve query performance, especially for large entities.</span></span> <span data-ttu-id="faad1-212">Használja a **válasszon** záradék, és adja át kell állítani a mezők nevét.</span><span class="sxs-lookup"><span data-stu-id="faad1-212">Use the **select** clause and pass the names of the fields to be returned.</span></span> <span data-ttu-id="faad1-213">Például a következő lekérdezés által visszaadott csak a **leírás** és **dueDate** mezőket.</span><span class="sxs-lookup"><span data-stu-id="faad1-213">For example, the following query will return only the **description** and **dueDate** fields.</span></span>

```nodejs
var query = new azure.TableQuery()
  .select(['description', 'dueDate'])
  .top(5)
  .where('PartitionKey eq ?', 'hometasks');
```

## <a name="delete-an-entity"></a><span data-ttu-id="faad1-214">Entitás törlése</span><span class="sxs-lookup"><span data-stu-id="faad1-214">Delete an entity</span></span>
<span data-ttu-id="faad1-215">A partíció- és sorfejlécek kulcsokkal entitás törölheti.</span><span class="sxs-lookup"><span data-stu-id="faad1-215">You can delete an entity using its partition and row keys.</span></span> <span data-ttu-id="faad1-216">Ebben a példában a **task1** objektum tartalmazza a **RowKey** és **PartitionKey** értékek a törölni kívánt entitás.</span><span class="sxs-lookup"><span data-stu-id="faad1-216">In this example, the **task1** object contains the **RowKey** and **PartitionKey** values of the entity to be deleted.</span></span> <span data-ttu-id="faad1-217">Az objektum átadott, majd a **deleteEntity** metódust.</span><span class="sxs-lookup"><span data-stu-id="faad1-217">Then the object is passed to the **deleteEntity** method.</span></span>

```nodejs
var task = {
  PartitionKey: {'_':'hometasks'},
  RowKey: {'_': '1'}
};

tableSvc.deleteEntity('mytable', task, function(error, response){
  if(!error) {
    // Entity deleted
  }
});
```

> [!NOTE]
> <span data-ttu-id="faad1-218">Érdemes lehet ETag-EK elemek, törlésekor annak érdekében, hogy az elem nem módosították-e egy másik folyamat.</span><span class="sxs-lookup"><span data-stu-id="faad1-218">Consider using ETags when deleting items, to ensure that the item hasn't been modified by another process.</span></span> <span data-ttu-id="faad1-219">Lásd: [frissíthető entitás](#update-an-entity) információ az ETag-EK használatával.</span><span class="sxs-lookup"><span data-stu-id="faad1-219">See [Update an entity](#update-an-entity) for information on using ETags.</span></span>
>
>

## <a name="delete-a-table"></a><span data-ttu-id="faad1-220">Tábla törlése</span><span class="sxs-lookup"><span data-stu-id="faad1-220">Delete a table</span></span>
<span data-ttu-id="faad1-221">Az alábbi kód törölhető egy tábla a tárfiókból.</span><span class="sxs-lookup"><span data-stu-id="faad1-221">The following code deletes a table from a storage account.</span></span>

```nodejs
tableSvc.deleteTable('mytable', function(error, response){
    if(!error){
        // Table deleted
    }
});
```

<span data-ttu-id="faad1-222">Ha biztos benne, hogy a tábla létezik, **deleteTableIfExists**.</span><span class="sxs-lookup"><span data-stu-id="faad1-222">If you are uncertain whether the table exists, use **deleteTableIfExists**.</span></span>

## <a name="use-continuation-tokens"></a><span data-ttu-id="faad1-223">Használjon tokeneket folytatása</span><span class="sxs-lookup"><span data-stu-id="faad1-223">Use continuation tokens</span></span>
<span data-ttu-id="faad1-224">Ha nagy mennyiségű eredmények táblák kérdez le, keresse meg folytatási jogkivonatokat.</span><span class="sxs-lookup"><span data-stu-id="faad1-224">When you are querying tables for large amounts of results, look for continuation tokens.</span></span> <span data-ttu-id="faad1-225">Nagy mennyiségű adat lehet használható a lekérdezésben, akkor előfordulhat, hogy nem ismeri fel, ha ismeri fel, ha jelen a folytatási kód nem hoz létre.</span><span class="sxs-lookup"><span data-stu-id="faad1-225">There may be large amounts of data available for your query that you might not realize if you do not build to recognize when a continuation token is present.</span></span>

<span data-ttu-id="faad1-226">Az eredmények objektum lekérdezése entitások beállítása során a `continuationToken` tulajdonságot, ha ilyen tokent.</span><span class="sxs-lookup"><span data-stu-id="faad1-226">The results object returned during querying entities sets a `continuationToken` property when such a token is present.</span></span> <span data-ttu-id="faad1-227">Majd ezzel a lekérdezés végrehajtása során továbbra is a partícióazonosító és a tábla entitás közötti áthelyezése közben.</span><span class="sxs-lookup"><span data-stu-id="faad1-227">You can then use this when performing a query to continue to move across the partition and table entities.</span></span>

<span data-ttu-id="faad1-228">Kérdez le, ha egy continuationToken paraméter is megadható a lekérdezés-példányt és a visszahívási függvény között:</span><span class="sxs-lookup"><span data-stu-id="faad1-228">When querying, a continuationToken parameter may be provided between the query object instance and the callback function:</span></span>

```nodejs
var nextContinuationToken = null;
dc.table.queryEntities(tableName,
    query,
    nextContinuationToken,
    function (error, results) {
        if (error) throw error;

        // iterate through results.entries with results

        if (results.continuationToken) {
            nextContinuationToken = results.continuationToken;
        }

    });
```

<span data-ttu-id="faad1-229">Ha nézze meg a `continuationToken` objektum található tulajdonságok például `nextPartitionKey`, `nextRowKey` és `targetLocation`, amelyek segítségével az eredményeket iterációt.</span><span class="sxs-lookup"><span data-stu-id="faad1-229">If you inspect the `continuationToken` object, you will find properties such as `nextPartitionKey`, `nextRowKey` and `targetLocation`, which can be used to iterate through all the results.</span></span>

<span data-ttu-id="faad1-230">Egy folytatási mintát a Githubon az Azure Storage Node.js tárház belül is van.</span><span class="sxs-lookup"><span data-stu-id="faad1-230">There is also a continuation sample within the Azure Storage Node.js repo on GitHub.</span></span> <span data-ttu-id="faad1-231">Keressen `examples/samples/continuationsample.js`.</span><span class="sxs-lookup"><span data-stu-id="faad1-231">Look for `examples/samples/continuationsample.js`.</span></span>

## <a name="work-with-shared-access-signatures"></a><span data-ttu-id="faad1-232">Megosztott hozzáférési aláírásokkal működik</span><span class="sxs-lookup"><span data-stu-id="faad1-232">Work with shared access signatures</span></span>
<span data-ttu-id="faad1-233">Közös hozzáférésű jogosultságkód (SAS), amelyek egy biztonságos táblák részletes hozzáférést biztosítanak a tárfiók neve vagy a kulcsok megadása nélkül.</span><span class="sxs-lookup"><span data-stu-id="faad1-233">Shared access signatures (SAS) are a secure way to provide granular access to tables without providing your storage account name or keys.</span></span> <span data-ttu-id="faad1-234">SAS gyakran használják korlátozott hozzáférést biztosít az adatok, például egy mobil alkalmazás lekérdezést rögzíti.</span><span class="sxs-lookup"><span data-stu-id="faad1-234">SAS are often used to provide limited access to your data, such as allowing a mobile app to query records.</span></span>

<span data-ttu-id="faad1-235">A megbízható alkalmazások, például egy felhőalapú szolgáltatás létrehoz egy SAS használatával a **generateSharedAccessSignature** , a **TableService**, és átadja egy nem megbízható vagy félig megbízható alkalmazás, például egy mobilalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="faad1-235">A trusted application such as a cloud-based service generates a SAS using the **generateSharedAccessSignature** of the **TableService**, and provides it to an untrusted or semi-trusted application such as a mobile app.</span></span> <span data-ttu-id="faad1-236">A biztonsági Társítások létrejön egy házirendet, amely a kezdő és záró dátumát, amely alatt a SAS érvénytelen ismerteti, valamint a hozzáférési szintet a biztonsági Társítások jogosult kap.</span><span class="sxs-lookup"><span data-stu-id="faad1-236">The SAS is generated using a policy, which describes the start and end dates during which the SAS is valid, as well as the access level granted to the SAS holder.</span></span>

<span data-ttu-id="faad1-237">Az alábbi példa létrehoz egy új megosztott elérési házirendet, amely lehetővé teszi a lekérdezés ("r") a tábla SAS jogosult, és 100 perc létrehozása után lejár.</span><span class="sxs-lookup"><span data-stu-id="faad1-237">The following example generates a new shared access policy that will allow the SAS holder to query ('r') the table, and expires 100 minutes after the time it is created.</span></span>

```nodejs
var startDate = new Date();
var expiryDate = new Date(startDate);
expiryDate.setMinutes(startDate.getMinutes() + 100);
startDate.setMinutes(startDate.getMinutes() - 100);

var sharedAccessPolicy = {
  AccessPolicy: {
    Permissions: azure.TableUtilities.SharedAccessPermissions.QUERY,
    Start: startDate,
    Expiry: expiryDate
  },
};

var tableSAS = tableSvc.generateSharedAccessSignature('mytable', sharedAccessPolicy);
var host = tableSvc.host;
```

<span data-ttu-id="faad1-238">Vegye figyelembe, hogy a gazdagép adatokat meg kell adni is, akkor szükség, ha a biztonsági Társítások jogosult megpróbál hozzáférni a táblában.</span><span class="sxs-lookup"><span data-stu-id="faad1-238">Note that the host information must be provided also, as it is required when the SAS holder attempts to access the table.</span></span>

<span data-ttu-id="faad1-239">Az ügyfélalkalmazás majd használja a biztonsági Társításait **TableServiceWithSAS** a táblázaton műveletek végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="faad1-239">The client application then uses the SAS with **TableServiceWithSAS** to perform operations against the table.</span></span> <span data-ttu-id="faad1-240">A következő példa a tábla kapcsolódik, és lekérdezést hajt végre.</span><span class="sxs-lookup"><span data-stu-id="faad1-240">The following example connects to the table and performs a query.</span></span>

```nodejs
var sharedTableService = azure.createTableServiceWithSas(host, tableSAS);
var query = azure.TableQuery()
  .where('PartitionKey eq ?', 'hometasks');

sharedTableService.queryEntities(query, null, function(error, result, response) {
  if(!error) {
    // result contains the entities
  }
});
```

<span data-ttu-id="faad1-241">A SAS létre lett hozva lekérdezés hozzáférést, csak ha insert, update vagy entitások törlésére tett kísérlet volt, mert akkor kell hibát jelez.</span><span class="sxs-lookup"><span data-stu-id="faad1-241">Since the SAS was generated with only query access, if an attempt were made to insert, update, or delete entities, an error would be returned.</span></span>

### <a name="access-control-lists"></a><span data-ttu-id="faad1-242">Hozzáférés-vezérlési listák</span><span class="sxs-lookup"><span data-stu-id="faad1-242">Access Control Lists</span></span>
<span data-ttu-id="faad1-243">Egy hozzáférés-vezérlési lista (ACL) segítségével állítsa be a hozzáférési házirend tartozó SAS korlátozására.</span><span class="sxs-lookup"><span data-stu-id="faad1-243">You can also use an Access Control List (ACL) to set the access policy for a SAS.</span></span> <span data-ttu-id="faad1-244">Ez akkor hasznos, ha a táblának az elérésére, de különböző hozzáférési házirendek biztosít az egyes ügyfelek több ügyféllel engedélyez.</span><span class="sxs-lookup"><span data-stu-id="faad1-244">This is useful if you wish to allow multiple clients to access the table, but provide different access policies for each client.</span></span>

<span data-ttu-id="faad1-245">Hozzáférés-vezérlési Listában hozzáférési házirendeket, tömbje segítségével minden házirendhez társított azonosítójú van megvalósítva.</span><span class="sxs-lookup"><span data-stu-id="faad1-245">An ACL is implemented using an array of access policies, with an ID associated with each policy.</span></span> <span data-ttu-id="faad1-246">Az alábbi példa meghatározza, hogy két házirend, egy "felhasználó1", és egy "felhasználó2":</span><span class="sxs-lookup"><span data-stu-id="faad1-246">The following example defines two policies, one for 'user1' and one for 'user2':</span></span>

```nodejs
var sharedAccessPolicy = {
  user1: {
    Permissions: azure.TableUtilities.SharedAccessPermissions.QUERY,
    Start: startDate,
    Expiry: expiryDate
  },
  user2: {
    Permissions: azure.TableUtilities.SharedAccessPermissions.ADD,
    Start: startDate,
    Expiry: expiryDate
  }
};
```

<span data-ttu-id="faad1-247">Az alábbi példa lekérdezi az aktuális hozzáférés-vezérlési listája a **hometasks** tábla, és hozzáadja az új házirendek **setTableAcl**.</span><span class="sxs-lookup"><span data-stu-id="faad1-247">The following example gets the current ACL for the **hometasks** table, and then adds the new policies using **setTableAcl**.</span></span> <span data-ttu-id="faad1-248">Ez a megközelítés lehetővé teszi, hogy:</span><span class="sxs-lookup"><span data-stu-id="faad1-248">This approach allows:</span></span>

```nodejs
var extend = require('extend');
tableSvc.getTableAcl('hometasks', function(error, result, response) {
if(!error){
    var newSignedIdentifiers = extend(true, result.signedIdentifiers, sharedAccessPolicy);
    tableSvc.setTableAcl('hometasks', newSignedIdentifiers, function(error, result, response){
      if(!error){
        // ACL set
      }
    });
  }
});
```

<span data-ttu-id="faad1-249">Miután a hozzáférés-vezérlési lista van beállítva, majd a házirend-azonosító alapján SAS hozhatók létre.</span><span class="sxs-lookup"><span data-stu-id="faad1-249">Once the ACL has been set, you can then create a SAS based on the ID for a policy.</span></span> <span data-ttu-id="faad1-250">Az alábbi példa létrehoz egy új SAS-kód "felhasználó2":</span><span class="sxs-lookup"><span data-stu-id="faad1-250">The following example creates a new SAS for 'user2':</span></span>

```nodejs
tableSAS = tableSvc.generateSharedAccessSignature('hometasks', { Id: 'user2' });
```

## <a name="next-steps"></a><span data-ttu-id="faad1-251">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="faad1-251">Next steps</span></span>
<span data-ttu-id="faad1-252">További információkért lásd a következőket.</span><span class="sxs-lookup"><span data-stu-id="faad1-252">For more information, see the following resources.</span></span>

* <span data-ttu-id="faad1-253">A [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) egy ingyenes, önálló alkalmazás, amelynek segítségével vizuálisan dolgozhat Azure Storage-adatokkal Windows, macOS és Linux rendszereken.</span><span class="sxs-lookup"><span data-stu-id="faad1-253">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) is a free, standalone app from Microsoft that enables you to work visually with Azure Storage data on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="faad1-254">[Az Azure Storage szolgáltatás SDK csomópont](https://github.com/Azure/azure-storage-node) GitHub tárházából.</span><span class="sxs-lookup"><span data-stu-id="faad1-254">[Azure Storage SDK for Node](https://github.com/Azure/azure-storage-node) repository on GitHub.</span></span>
* [<span data-ttu-id="faad1-255">Node.js fejlesztői központ</span><span class="sxs-lookup"><span data-stu-id="faad1-255">Node.js Developer Center</span></span>](/develop/nodejs/)
* [<span data-ttu-id="faad1-256">Létrehozhat és telepíthet egy Node.js-alkalmazás egy Azure-webhelyen</span><span class="sxs-lookup"><span data-stu-id="faad1-256">Create and deploy a Node.js application to an Azure website</span></span>](../app-service-web/app-service-web-get-started-nodejs.md)