---
title: "Azure Table storage-ának Node.js aaaHow toouse |} Microsoft Docs"
description: "Azure Table storage, egy NoSQL-adattár hello felhőbe strukturált adat tárolása."
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
ms.openlocfilehash: 21022491a9a21a5365628de93582ea3a325ed869
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-table-storage-from-nodejs"></a><span data-ttu-id="82df9-103">Hogyan toouse Azure Table storage Node.js-ből</span><span class="sxs-lookup"><span data-stu-id="82df9-103">How toouse Azure Table storage from Node.js</span></span>
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

## <a name="overview"></a><span data-ttu-id="82df9-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="82df9-104">Overview</span></span>
<span data-ttu-id="82df9-105">Ez a témakör bemutatja, hogyan tooperform szolgáltatást használó általános forgatókönyvhöz hello Azure Table szolgáltatás Node.js-alkalmazásokban.</span><span class="sxs-lookup"><span data-stu-id="82df9-105">This topic shows how tooperform common scenarios using hello Azure Table service in a Node.js application.</span></span>

<span data-ttu-id="82df9-106">hello ebben a témakörben szereplő példák azt feltételezik, hogy már rendelkezik egy Node.js-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="82df9-106">hello code examples in this topic assume you already have a Node.js application.</span></span> <span data-ttu-id="82df9-107">További információ egy Node.js-alkalmazás az Azure toocreate jelennek meg ezek a témakörök:</span><span class="sxs-lookup"><span data-stu-id="82df9-107">For information about how toocreate a Node.js application in Azure, see any of these topics:</span></span>

* [<span data-ttu-id="82df9-108">Node.js-webalkalmazás létrehozása az Azure App Service-ben</span><span class="sxs-lookup"><span data-stu-id="82df9-108">Create a Node.js web app in Azure App Service</span></span>](../app-service-web/app-service-web-get-started-nodejs.md)
* [<span data-ttu-id="82df9-109">Hozza létre és telepítheti a Node.js web app tooAzure WebMatrix használatával.</span><span class="sxs-lookup"><span data-stu-id="82df9-109">Build and deploy a Node.js web app tooAzure using WebMatrix</span></span>](../app-service-web/web-sites-nodejs-use-webmatrix.md)
* <span data-ttu-id="82df9-110">[És üzembe egy Node.js-alkalmazás tooan Azure Cloud Service](../cloud-services/cloud-services-nodejs-develop-deploy-app.md) (a Windows PowerShell használatával)</span><span class="sxs-lookup"><span data-stu-id="82df9-110">[Build and deploy a Node.js application tooan Azure Cloud Service](../cloud-services/cloud-services-nodejs-develop-deploy-app.md) (using Windows PowerShell)</span></span>

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="configure-your-application-tooaccess-azure-storage"></a><span data-ttu-id="82df9-111">Az alkalmazás tooaccess Azure Storage konfigurálása</span><span class="sxs-lookup"><span data-stu-id="82df9-111">Configure your application tooaccess Azure Storage</span></span>
<span data-ttu-id="82df9-112">Azure Storage toouse, kell hello Azure Storage szolgáltatás SDK a Node.js, amely kényelmi szalagtárak hello tárolószolgáltatások REST kommunikációt készletét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="82df9-112">toouse Azure Storage, you need hello Azure Storage SDK for Node.js, which includes a set of convenience libraries that communicate with hello storage REST services.</span></span>

### <a name="use-node-package-manager-npm-tooinstall-hello-package"></a><span data-ttu-id="82df9-113">Csomópont Package Manager (NPM) tooinstall hello csomag használata</span><span class="sxs-lookup"><span data-stu-id="82df9-113">Use Node Package Manager (NPM) tooinstall hello package</span></span>
1. <span data-ttu-id="82df9-114">Használjon például egy parancssori felületet **PowerShell** (Windows) **Terminálszolgáltatások** (Mac), vagy **Bash** (Unix), és keresse meg a toohello mappa, amelyben létrehozta az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="82df9-114">Use a command-line interface such as **PowerShell** (Windows), **Terminal** (Mac), or **Bash** (Unix), and navigate toohello folder where you created your application.</span></span>
2. <span data-ttu-id="82df9-115">Típus **npm telepítése azure-tároló** hello parancssori ablakban.</span><span class="sxs-lookup"><span data-stu-id="82df9-115">Type **npm install azure-storage** in hello command window.</span></span> <span data-ttu-id="82df9-116">Hello parancs kimenete a következő példa hasonló toohello.</span><span class="sxs-lookup"><span data-stu-id="82df9-116">Output from hello command is similar toohello following example.</span></span>

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
3. <span data-ttu-id="82df9-117">Manuális futtatásával hello **ls** parancs tooverify, amely egy **csomópont\_modulok** mappa hozták létre.</span><span class="sxs-lookup"><span data-stu-id="82df9-117">You can manually run hello **ls** command tooverify that a **node\_modules** folder was created.</span></span> <span data-ttu-id="82df9-118">Mappán belül található hello **azure-tároló** csomagot, amely tartalmaz hello szalagtárak tooaccess tárolási van szüksége.</span><span class="sxs-lookup"><span data-stu-id="82df9-118">Inside that folder you will find hello **azure-storage** package, which contains hello libraries you need tooaccess storage.</span></span>

### <a name="import-hello-package"></a><span data-ttu-id="82df9-119">Hello csomag importálása</span><span class="sxs-lookup"><span data-stu-id="82df9-119">Import hello package</span></span>
<span data-ttu-id="82df9-120">Adja hozzá a következő kód toohello felső részén hello hello **server.js** fájl az alkalmazásban:</span><span class="sxs-lookup"><span data-stu-id="82df9-120">Add hello following code toohello top of hello **server.js** file in your application:</span></span>

```nodejs
var azure = require('azure-storage');
```

## <a name="set-up-an-azure-storage-connection"></a><span data-ttu-id="82df9-121">Egy Azure Storage-kapcsolat beállítása</span><span class="sxs-lookup"><span data-stu-id="82df9-121">Set up an Azure Storage connection</span></span>
<span data-ttu-id="82df9-122">hello azure modul olvassák hello környezeti változók AZURE\_tárolási\_FIÓKOT és az AZURE\_tárolási\_hozzáférés\_kulcs, vagy AZURE\_tárolási\_kapcsolat \_Szükséges adatokat tooconnect tooyour Azure storage-fiók KARAKTERLÁNCOT.</span><span class="sxs-lookup"><span data-stu-id="82df9-122">hello azure module will read hello environment variables AZURE\_STORAGE\_ACCOUNT and AZURE\_STORAGE\_ACCESS\_KEY, or AZURE\_STORAGE\_CONNECTION\_STRING for information required tooconnect tooyour Azure storage account.</span></span> <span data-ttu-id="82df9-123">Ha ezek a környezeti változók nem, meg kell adnia hello fiók meghívásakor **TableService**.</span><span class="sxs-lookup"><span data-stu-id="82df9-123">If these environment variables are not set, you must specify hello account information when calling **TableService**.</span></span>

<span data-ttu-id="82df9-124">Példa a hello hello környezeti változók beállítása [Azure-portálon](https://portal.azure.com) egy Azure-webhelyre, lásd: [Node.js web app használatával hello Azure Table szolgáltatás](../app-service-web/storage-nodejs-use-table-storage-web-site.md).</span><span class="sxs-lookup"><span data-stu-id="82df9-124">For an example of setting hello environment variables in hello [Azure portal](https://portal.azure.com) for an Azure Website, see [Node.js web app using hello Azure Table Service](../app-service-web/storage-nodejs-use-table-storage-web-site.md).</span></span>

## <a name="create-a-table"></a><span data-ttu-id="82df9-125">Tábla létrehozása</span><span class="sxs-lookup"><span data-stu-id="82df9-125">Create a table</span></span>
<span data-ttu-id="82df9-126">hello alábbi kód létrehoz egy **TableService** objektumra, és használja ezt a toocreate új tábla.</span><span class="sxs-lookup"><span data-stu-id="82df9-126">hello following code creates a **TableService** object and uses it toocreate a new table.</span></span> <span data-ttu-id="82df9-127">Adja hozzá hello következő tetején hello **server.js**.</span><span class="sxs-lookup"><span data-stu-id="82df9-127">Add hello following near hello top of **server.js**.</span></span>

```nodejs
var tableSvc = azure.createTableService();
```

<span data-ttu-id="82df9-128">hívás túl hello**createTableIfNotExists** hoz létre egy új tábla hello megadott névvel, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="82df9-128">hello call too**createTableIfNotExists** will create a new table with hello specified name if it does not already exist.</span></span> <span data-ttu-id="82df9-129">hello alábbi példa táblát hoz létre egy új "mytable" nevű, ha még nem létezik:</span><span class="sxs-lookup"><span data-stu-id="82df9-129">hello following example creates a new table named 'mytable' if it does not already exist:</span></span>

```nodejs
tableSvc.createTableIfNotExists('mytable', function(error, result, response){
  if(!error){
    // Table exists or created
  }
});
```

<span data-ttu-id="82df9-130">Hello `result.created` lesz `true` Ha új tábla létrehozása, és `false` Ha hello tábla már létezik.</span><span class="sxs-lookup"><span data-stu-id="82df9-130">hello `result.created` will be `true` if a new table is created, and `false` if hello table already exists.</span></span> <span data-ttu-id="82df9-131">Hello `response` hello kérelem információkat tartalmaznak.</span><span class="sxs-lookup"><span data-stu-id="82df9-131">hello `response` will contain information about hello request.</span></span>

### <a name="filters"></a><span data-ttu-id="82df9-132">Szűrők</span><span class="sxs-lookup"><span data-stu-id="82df9-132">Filters</span></span>
<span data-ttu-id="82df9-133">Választható szűrési műveletek lehet segítségével alkalmazott toooperations **TableService**.</span><span class="sxs-lookup"><span data-stu-id="82df9-133">Optional filtering operations can be applied toooperations performed using **TableService**.</span></span> <span data-ttu-id="82df9-134">Műveletek szűrésének lehetnek naplózási, automatikus újrapróbálkozása, stb. Szűrők hello aláírással metódus megvalósító objektumok a következők:</span><span class="sxs-lookup"><span data-stu-id="82df9-134">Filtering operations can include logging, automatically retrying, etc. Filters are objects that implement a method with hello signature:</span></span>

```nodejs
function handle (requestOptions, next)
```

<span data-ttu-id="82df9-135">Ezután a hello lehetőségek előfeldolgozása, hello metódust kell toocall "Tovább" gombra, egy visszahívási átadja a aláírását hello:</span><span class="sxs-lookup"><span data-stu-id="82df9-135">After doing its preprocessing on hello request options, hello method needs toocall "next", passing a callback with hello following signature:</span></span>

```nodejs
function (returnObject, finalCallback, next)
```

<span data-ttu-id="82df9-136">A visszahívási és hello returnObject (hello hello kérelem toohello kiszolgálótól kapott válasz) feldolgozása után hello visszahívási kell tooeither következő meghívni, ha más szűrők feldolgozása toocontinue létezik, vagy egyszerűen a finalCallback meghívása egyéb tooend hello szolgáltatás hívása.</span><span class="sxs-lookup"><span data-stu-id="82df9-136">In this callback, and after processing hello returnObject (hello response from hello request toohello server), hello callback needs tooeither invoke next if it exists toocontinue processing other filters or simply invoke finalCallback otherwise tooend hello service invocation.</span></span>

<span data-ttu-id="82df9-137">Két szűrőket, amelyek megvalósítják az újrapróbálkozási logika érhetők el a hello Azure SDK for Node.js, **ExponentialRetryPolicyFilter** és **LinearRetryPolicyFilter**.</span><span class="sxs-lookup"><span data-stu-id="82df9-137">Two filters that implement retry logic are included with hello Azure SDK for Node.js, **ExponentialRetryPolicyFilter** and **LinearRetryPolicyFilter**.</span></span> <span data-ttu-id="82df9-138">hello következő létrehoz egy **TableService** hello használó objektum **ExponentialRetryPolicyFilter**:</span><span class="sxs-lookup"><span data-stu-id="82df9-138">hello following creates a **TableService** object that uses hello **ExponentialRetryPolicyFilter**:</span></span>

```nodejs
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var tableSvc = azure.createTableService().withFilter(retryOperations);
```

## <a name="add-an-entity-tooa-table"></a><span data-ttu-id="82df9-139">Egy entitás tooa táblázat hozzáadása</span><span class="sxs-lookup"><span data-stu-id="82df9-139">Add an entity tooa table</span></span>
<span data-ttu-id="82df9-140">egy entitás tooadd először létre kell hoznia egy objektum, amely meghatározza az entitás tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="82df9-140">tooadd an entity, first create an object that defines your entity properties.</span></span> <span data-ttu-id="82df9-141">Összes entitás tartalmaznia kell egy **PartitionKey** és **RowKey**, amelyeket hello entitás egyedi azonosítót.</span><span class="sxs-lookup"><span data-stu-id="82df9-141">All entities must contain a **PartitionKey** and **RowKey**, which are unique identifiers for hello entity.</span></span>

* <span data-ttu-id="82df9-142">**PartitionKey** -hello partíció hello entitás tárolt határozza meg</span><span class="sxs-lookup"><span data-stu-id="82df9-142">**PartitionKey** - determines hello partition that hello entity is stored in</span></span>
* <span data-ttu-id="82df9-143">**RowKey** - egyedileg azonosítja a hello entitás hello partíción belül</span><span class="sxs-lookup"><span data-stu-id="82df9-143">**RowKey** - uniquely identifies hello entity within hello partition</span></span>

<span data-ttu-id="82df9-144">Mindkét **PartitionKey** és **RowKey** karakterlánc-értékekkel kell lennie.</span><span class="sxs-lookup"><span data-stu-id="82df9-144">Both **PartitionKey** and **RowKey** must be string values.</span></span> <span data-ttu-id="82df9-145">További információkért lásd: [ismertetése hello tábla szolgáltatás adatmodell](http://msdn.microsoft.com/library/azure/dd179338.aspx).</span><span class="sxs-lookup"><span data-stu-id="82df9-145">For more information, see [Understanding hello Table Service Data Model](http://msdn.microsoft.com/library/azure/dd179338.aspx).</span></span>

<span data-ttu-id="82df9-146">hello az alábbiakban látható egy példa egy entitás meghatározása.</span><span class="sxs-lookup"><span data-stu-id="82df9-146">hello following is an example of defining an entity.</span></span> <span data-ttu-id="82df9-147">Vegye figyelembe, hogy **dueDate** van definiálva, egyfajta **Edm.DateTime**.</span><span class="sxs-lookup"><span data-stu-id="82df9-147">Note that **dueDate** is defined as a type of **Edm.DateTime**.</span></span> <span data-ttu-id="82df9-148">Megadását hello típusa nem kötelező, és típusok fog lehet következtetni rá, ha nincs megadva.</span><span class="sxs-lookup"><span data-stu-id="82df9-148">Specifying hello type is optional, and types will be inferred if not specified.</span></span>

```nodejs
var task = {
  PartitionKey: {'_':'hometasks'},
  RowKey: {'_': '1'},
  description: {'_':'take out hello trash'},
  dueDate: {'_':new Date(2015, 6, 20), '$':'Edm.DateTime'}
};
```

> [!NOTE]
> <span data-ttu-id="82df9-149">Szerepel továbbá egy **időbélyeg** mezőjét az egyes rekordokhoz, amely az Azure-ban van beállítva, ha entitás beszúrni vagy frissíteni.</span><span class="sxs-lookup"><span data-stu-id="82df9-149">There is also a **Timestamp** field for each record, which is set by Azure when an entity is inserted or updated.</span></span>
>
>

<span data-ttu-id="82df9-150">Is használhatja a hello **entityGenerator** toocreate entitásokat.</span><span class="sxs-lookup"><span data-stu-id="82df9-150">You can also use hello **entityGenerator** toocreate entities.</span></span> <span data-ttu-id="82df9-151">hello alábbi példakód létrehozza hello hello azonos feladat entitást **entityGenerator**.</span><span class="sxs-lookup"><span data-stu-id="82df9-151">hello following example creates hello same task entity using hello **entityGenerator**.</span></span>

```nodejs
var entGen = azure.TableUtilities.entityGenerator;
var task = {
  PartitionKey: entGen.String('hometasks'),
  RowKey: entGen.String('1'),
  description: entGen.String('take out hello trash'),
  dueDate: entGen.DateTime(new Date(Date.UTC(2015, 6, 20))),
};
```

<span data-ttu-id="82df9-152">egy entitás tooyour tábla tooadd átadni hello entitás objektum toohello **insertEntity** metódust.</span><span class="sxs-lookup"><span data-stu-id="82df9-152">tooadd an entity tooyour table, pass hello entity object toohello **insertEntity** method.</span></span>

```nodejs
tableSvc.insertEntity('mytable',task, function (error, result, response) {
  if(!error){
    // Entity inserted
  }
});
```

<span data-ttu-id="82df9-153">Ha hello művelet sikeres, `result` fogja tartalmazni hello [ETag](http://en.wikipedia.org/wiki/HTTP_ETag) a hello beszúrni a rekord és `response` hello művelet információkat tartalmaznak.</span><span class="sxs-lookup"><span data-stu-id="82df9-153">If hello operation is successful, `result` will contain hello [ETag](http://en.wikipedia.org/wiki/HTTP_ETag) of hello inserted record and `response` will contain information about hello operation.</span></span>

<span data-ttu-id="82df9-154">Példa egy válasz:</span><span class="sxs-lookup"><span data-stu-id="82df9-154">Example response:</span></span>

```nodejs
{ '.metadata': { etag: 'W/"datetime\'2015-02-25T01%3A22%3A22.5Z\'"' } }
```

> [!NOTE]
> <span data-ttu-id="82df9-155">Alapértelmezés szerint **insertEntity** nem ad vissza beszúrni hello entitás hello részeként `response` információkat.</span><span class="sxs-lookup"><span data-stu-id="82df9-155">By default, **insertEntity** does not return hello inserted entity as part of hello `response` information.</span></span> <span data-ttu-id="82df9-156">Ha más műveleteket végez ehhez az entitáshoz tervez, vagy toocache hello adatokat kívánja, azok által visszaadott hello részeként hasznos toohave `result`.</span><span class="sxs-lookup"><span data-stu-id="82df9-156">If you plan on performing other operations on this entity, or wish toocache hello information, it can be useful toohave it returned as part of hello `result`.</span></span> <span data-ttu-id="82df9-157">Ehhez engedélyezésével **echoContent** az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="82df9-157">You can do this by enabling **echoContent** as follows:</span></span>
>
> `tableSvc.insertEntity('mytable', task, {echoContent: true}, function (error, result, response) {...}`
>
>

## <a name="update-an-entity"></a><span data-ttu-id="82df9-158">Egy entitás frissítése</span><span class="sxs-lookup"><span data-stu-id="82df9-158">Update an entity</span></span>
<span data-ttu-id="82df9-159">Van több módszer érhető el tooupdate meglévő entitás:</span><span class="sxs-lookup"><span data-stu-id="82df9-159">There are multiple methods available tooupdate an existing entity:</span></span>

* <span data-ttu-id="82df9-160">**replaceEntity** -cserélje ki egy meglévő entitás frissítéséhez</span><span class="sxs-lookup"><span data-stu-id="82df9-160">**replaceEntity** - updates an existing entity by replacing it</span></span>
* <span data-ttu-id="82df9-161">**mergeEntity** -meglévő entitás frissíti új tulajdonságértékek egyesíti hello meglévő entitás</span><span class="sxs-lookup"><span data-stu-id="82df9-161">**mergeEntity** - updates an existing entity by merging new property values into hello existing entity</span></span>
* <span data-ttu-id="82df9-162">**insertOrReplaceEntity** -cserélje ki egy meglévő entitás frissíti.</span><span class="sxs-lookup"><span data-stu-id="82df9-162">**insertOrReplaceEntity** - updates an existing entity by replacing it.</span></span> <span data-ttu-id="82df9-163">Ha nem entitás létezik, egy új szúrja be a</span><span class="sxs-lookup"><span data-stu-id="82df9-163">If no entity exists, a new one will be inserted</span></span>
* <span data-ttu-id="82df9-164">**insertOrMergeEntity** -frissíti a meglévő entitás által hello meglévő új tulajdonságértékek egyesíti.</span><span class="sxs-lookup"><span data-stu-id="82df9-164">**insertOrMergeEntity** - updates an existing entity by merging new property values into hello existing.</span></span> <span data-ttu-id="82df9-165">Ha nem entitás létezik, egy új szúrja be a</span><span class="sxs-lookup"><span data-stu-id="82df9-165">If no entity exists, a new one will be inserted</span></span>

<span data-ttu-id="82df9-166">hello következő példa bemutatja egy entitás használatával frissítése **replaceEntity**:</span><span class="sxs-lookup"><span data-stu-id="82df9-166">hello following example demonstrates updating an entity using **replaceEntity**:</span></span>

```nodejs
tableSvc.replaceEntity('mytable', updatedTask, function(error, result, response){
  if(!error) {
    // Entity updated
  }
});
```

> [!NOTE]
> <span data-ttu-id="82df9-167">Alapértelmezés szerint egy entitás frissítése nem ellenőrzi toosee Ha hello adatok frissítése során korábban egy másik folyamat módosult.</span><span class="sxs-lookup"><span data-stu-id="82df9-167">By default, updating an entity does not check toosee if hello data being updated has previously been modified by another process.</span></span> <span data-ttu-id="82df9-168">toosupport egyidejű frissítések:</span><span class="sxs-lookup"><span data-stu-id="82df9-168">toosupport concurrent updates:</span></span>
>
> 1. <span data-ttu-id="82df9-169">Hello ETag frissítendő hello objektum beolvasása.</span><span class="sxs-lookup"><span data-stu-id="82df9-169">Get hello ETag of hello object being updated.</span></span> <span data-ttu-id="82df9-170">A visszaadott hello részeként `response` entitás kapcsolatos műveletek és kérhető `response['.metadata'].etag`.</span><span class="sxs-lookup"><span data-stu-id="82df9-170">This is returned as part of hello `response` for any entity-related operation and can be retrieved through `response['.metadata'].etag`.</span></span>
> 2. <span data-ttu-id="82df9-171">Entitás frissítési művelet végrehajtásakor adja hozzá a korábban beolvasni toohello új entitás hello ETag adatait.</span><span class="sxs-lookup"><span data-stu-id="82df9-171">When performing an update operation on an entity, add hello ETag information previously retrieved toohello new entity.</span></span> <span data-ttu-id="82df9-172">Példa:</span><span class="sxs-lookup"><span data-stu-id="82df9-172">For example:</span></span>
>
>       <span data-ttu-id="82df9-173">entity2 [.metadata] .etag = currentEtag;</span><span class="sxs-lookup"><span data-stu-id="82df9-173">entity2['.metadata'].etag = currentEtag;</span></span>
> 3. <span data-ttu-id="82df9-174">Hello frissítési műveletet végrehajtani.</span><span class="sxs-lookup"><span data-stu-id="82df9-174">Perform hello update operation.</span></span> <span data-ttu-id="82df9-175">Ha hello entitás hello ETag érték, például az alkalmazás egy másik példánya lekért óta módosították egy `error` az eredmény figyelmezteti a felhasználókat arra, hogy hello kérelemben megadott hello frissítés feltétel nem teljesült.</span><span class="sxs-lookup"><span data-stu-id="82df9-175">If hello entity has been modified since you retrieved hello ETag value, such as another instance of your application, an `error` will be returned stating that hello update condition specified in hello request was not satisfied.</span></span>
>
>

<span data-ttu-id="82df9-176">A **replaceEntity** és **mergeEntity**, ha a frissítés alatt álló hello entitás nem létezik, akkor hello frissítési művelet sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="82df9-176">With **replaceEntity** and **mergeEntity**, if hello entity that is being updated doesn't exist, then hello update operation will fail.</span></span> <span data-ttu-id="82df9-177">Ezért ha toostore egy entitás, függetlenül attól, hogy létezik-e, használjon **insertOrReplaceEntity** vagy **insertOrMergeEntity**.</span><span class="sxs-lookup"><span data-stu-id="82df9-177">Therefore if you wish toostore an entity regardless of whether it already exists, use **insertOrReplaceEntity** or **insertOrMergeEntity**.</span></span>

<span data-ttu-id="82df9-178">Hello `result` sikeres frissítés műveletek hello fogja tartalmazni **Etag** hello az entitás frissítése.</span><span class="sxs-lookup"><span data-stu-id="82df9-178">hello `result` for successful update operations will contain hello **Etag** of hello updated entity.</span></span>

## <a name="work-with-groups-of-entities"></a><span data-ttu-id="82df9-179">Az entitások csoportok használata</span><span class="sxs-lookup"><span data-stu-id="82df9-179">Work with groups of entities</span></span>
<span data-ttu-id="82df9-180">Néha azt teszi logika toosubmit több művelet együtt egy kötegelt tooensure atomi hello kiszolgáló általi feldolgozás alatt.</span><span class="sxs-lookup"><span data-stu-id="82df9-180">Sometimes it makes sense toosubmit multiple operations together in a batch tooensure atomic processing by hello server.</span></span> <span data-ttu-id="82df9-181">használó, hello tooaccomplish **TableBatch** toocreate egy kötegelt osztály, és aztán hello **executeBatch** metódusában **TableService** tooperform hello kötegelni műveletek.</span><span class="sxs-lookup"><span data-stu-id="82df9-181">tooaccomplish that, use hello **TableBatch** class toocreate a batch, and then use hello **executeBatch** method of **TableService** tooperform hello batched operations.</span></span>

 <span data-ttu-id="82df9-182">a következő példa hello mutatja be két entitás kötegben elküldése:</span><span class="sxs-lookup"><span data-stu-id="82df9-182">hello following example demonstrates submitting two entities in a batch:</span></span>

```nodejs
var task1 = {
  PartitionKey: {'_':'hometasks'},
  RowKey: {'_': '1'},
  description: {'_':'Take out hello trash'},
  dueDate: {'_':new Date(2015, 6, 20)}
};
var task2 = {
  PartitionKey: {'_':'hometasks'},
  RowKey: {'_': '2'},
  description: {'_':'Wash hello dishes'},
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

<span data-ttu-id="82df9-183">A sikeres kötegelt műveleteket `result` hello kötegben lévő egyes műveletek információt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="82df9-183">For successful batch operations, `result` will contain information for each operation in hello batch.</span></span>

### <a name="work-with-batched-operations"></a><span data-ttu-id="82df9-184">A kötegelt műveletek használata</span><span class="sxs-lookup"><span data-stu-id="82df9-184">Work with batched operations</span></span>
<span data-ttu-id="82df9-185">Műveletek hozzáadott tooa kötegelt ellenőrizni kell hello megtekintésével `operations` tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="82df9-185">Operations added tooa batch can be inspected by viewing hello `operations` property.</span></span> <span data-ttu-id="82df9-186">A következő módszerek toowork műveletekkel hello is használhatja:</span><span class="sxs-lookup"><span data-stu-id="82df9-186">You can also use hello following methods toowork with operations:</span></span>

* <span data-ttu-id="82df9-187">**Törölje a jelet** -egy köteg az összes művelet törlése</span><span class="sxs-lookup"><span data-stu-id="82df9-187">**clear** - clears all operations from a batch</span></span>
* <span data-ttu-id="82df9-188">**getOperations** -művelet lekérdezi a hello kötegelt</span><span class="sxs-lookup"><span data-stu-id="82df9-188">**getOperations** - gets an operation from hello batch</span></span>
* <span data-ttu-id="82df9-189">**hasOperations** -hello kötegelt műveleteket tartalmaz. Ha igaz értéket ad vissza</span><span class="sxs-lookup"><span data-stu-id="82df9-189">**hasOperations** - returns true if hello batch contains operations</span></span>
* <span data-ttu-id="82df9-190">**removeOperations** -eltávolítja egy műveletet</span><span class="sxs-lookup"><span data-stu-id="82df9-190">**removeOperations** - removes an operation</span></span>
* <span data-ttu-id="82df9-191">**méret** -értéket ad vissza hello hello kötegben műveletek száma</span><span class="sxs-lookup"><span data-stu-id="82df9-191">**size** - returns hello number of operations in hello batch</span></span>

## <a name="retrieve-an-entity-by-key"></a><span data-ttu-id="82df9-192">Egy entitás kulcs letöltése</span><span class="sxs-lookup"><span data-stu-id="82df9-192">Retrieve an entity by key</span></span>
<span data-ttu-id="82df9-193">egy adott entitás alapján hello tooreturn **PartitionKey** és **RowKey**, használja a hello **retrieveEntity** metódus.</span><span class="sxs-lookup"><span data-stu-id="82df9-193">tooreturn a specific entity based on hello **PartitionKey** and **RowKey**, use hello **retrieveEntity** method.</span></span>

```nodejs
tableSvc.retrieveEntity('mytable', 'hometasks', '1', function(error, result, response){
  if(!error){
    // result contains hello entity
  }
});
```

<span data-ttu-id="82df9-194">Ez a művelet befejeződése után `result` hello entitás fogja tartalmazni.</span><span class="sxs-lookup"><span data-stu-id="82df9-194">Once this operation is complete, `result` will contain hello entity.</span></span>

## <a name="query-a-set-of-entities"></a><span data-ttu-id="82df9-195">Az entitások készletének lekérdezése</span><span class="sxs-lookup"><span data-stu-id="82df9-195">Query a set of entities</span></span>
<span data-ttu-id="82df9-196">egy tábla tooquery hello használata **TableQuery** objektum toobuild fel egy lekérdezési kifejezésben hello záradék a következő használatával:</span><span class="sxs-lookup"><span data-stu-id="82df9-196">tooquery a table, use hello **TableQuery** object toobuild up a query expression using hello following clauses:</span></span>

* <span data-ttu-id="82df9-197">**Válassza ki** -hello mezők toobe hello lekérdezés által visszaadott</span><span class="sxs-lookup"><span data-stu-id="82df9-197">**select** - hello fields toobe returned from hello query</span></span>
* <span data-ttu-id="82df9-198">**Ha** – hello ahol záradék</span><span class="sxs-lookup"><span data-stu-id="82df9-198">**where** - hello where clause</span></span>

  * <span data-ttu-id="82df9-199">**és** - `and` where feltétel</span><span class="sxs-lookup"><span data-stu-id="82df9-199">**and** - an `and` where condition</span></span>
  * <span data-ttu-id="82df9-200">**vagy** - `or` where feltétel</span><span class="sxs-lookup"><span data-stu-id="82df9-200">**or** - an `or` where condition</span></span>
* <span data-ttu-id="82df9-201">**felső** -hello toofetch elemek száma</span><span class="sxs-lookup"><span data-stu-id="82df9-201">**top** - hello number of items toofetch</span></span>

<span data-ttu-id="82df9-202">hello alábbi példa összeállít egy lekérdezést, amely visszaadható hello felső öt cikkeket a PartitionKey "hometasks".</span><span class="sxs-lookup"><span data-stu-id="82df9-202">hello following example builds a query that will return hello top five items with a PartitionKey of 'hometasks'.</span></span>

```nodejs
var query = new azure.TableQuery()
  .top(5)
  .where('PartitionKey eq ?', 'hometasks');
```

<span data-ttu-id="82df9-203">Mivel a **válasszon** nem használ, akkor minden mező adja vissza.</span><span class="sxs-lookup"><span data-stu-id="82df9-203">Since **select** is not used, all fields will be returned.</span></span> <span data-ttu-id="82df9-204">egy tábla, használja a tooperform hello lekérdezése **queryEntities**.</span><span class="sxs-lookup"><span data-stu-id="82df9-204">tooperform hello query against a table, use **queryEntities**.</span></span> <span data-ttu-id="82df9-205">hello alábbi példában a "mytable" lekérdezés tooreturn entitásokat.</span><span class="sxs-lookup"><span data-stu-id="82df9-205">hello following example uses this query tooreturn entities from 'mytable'.</span></span>

```nodejs
tableSvc.queryEntities('mytable',query, null, function(error, result, response) {
  if(!error) {
    // query was successful
  }
});
```

<span data-ttu-id="82df9-206">Ha sikeres, `result.entries` hello lekérdezésnek megfelelő entitások tömbjét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="82df9-206">If successful, `result.entries` will contain an array of entities that match hello query.</span></span> <span data-ttu-id="82df9-207">Ha hello lekérdezés nem tooreturn összes entitás `result.continuationToken` lesz nem -*null* és harmadik paramétere hello használható **queryEntities** tooretrieve további eredmények.</span><span class="sxs-lookup"><span data-stu-id="82df9-207">If hello query was unable tooreturn all entities, `result.continuationToken` will be non-*null* and can be used as hello third parameter of **queryEntities** tooretrieve more results.</span></span> <span data-ttu-id="82df9-208">Hello kezdeti lekérdezést, használjon *null* hello harmadik paraméter.</span><span class="sxs-lookup"><span data-stu-id="82df9-208">For hello initial query, use *null* for hello third parameter.</span></span>

### <a name="query-a-subset-of-entity-properties"></a><span data-ttu-id="82df9-209">Az entitástulajdonságok egy részének lekérdezése</span><span class="sxs-lookup"><span data-stu-id="82df9-209">Query a subset of entity properties</span></span>
<span data-ttu-id="82df9-210">A lekérdezéstábla tooa pár mezők kérhetnek le egy entitás.</span><span class="sxs-lookup"><span data-stu-id="82df9-210">A query tooa table can retrieve just a few fields from an entity.</span></span>
<span data-ttu-id="82df9-211">Ez csökkenti a sávszélesség, és javíthatja a lekérdezési teljesítményt, különösen olyan nagyméretű entitásokat.</span><span class="sxs-lookup"><span data-stu-id="82df9-211">This reduces bandwidth and can improve query performance, especially for large entities.</span></span> <span data-ttu-id="82df9-212">Használjon hello **válasszon** hello mezők toobe záradék és pass hello nevét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="82df9-212">Use hello **select** clause and pass hello names of hello fields toobe returned.</span></span> <span data-ttu-id="82df9-213">Például hello következő lekérdezés által visszaadott csak hello **leírás** és **dueDate** mezőket.</span><span class="sxs-lookup"><span data-stu-id="82df9-213">For example, hello following query will return only hello **description** and **dueDate** fields.</span></span>

```nodejs
var query = new azure.TableQuery()
  .select(['description', 'dueDate'])
  .top(5)
  .where('PartitionKey eq ?', 'hometasks');
```

## <a name="delete-an-entity"></a><span data-ttu-id="82df9-214">Entitás törlése</span><span class="sxs-lookup"><span data-stu-id="82df9-214">Delete an entity</span></span>
<span data-ttu-id="82df9-215">A partíció- és sorfejlécek kulcsokkal entitás törölheti.</span><span class="sxs-lookup"><span data-stu-id="82df9-215">You can delete an entity using its partition and row keys.</span></span> <span data-ttu-id="82df9-216">Ebben a példában hello **task1** objektum tartalmaz hello **RowKey** és **PartitionKey** hello entitás toobe törölt értékeit.</span><span class="sxs-lookup"><span data-stu-id="82df9-216">In this example, hello **task1** object contains hello **RowKey** and **PartitionKey** values of hello entity toobe deleted.</span></span> <span data-ttu-id="82df9-217">Ezután hello objektum átadása toohello **deleteEntity** metódust.</span><span class="sxs-lookup"><span data-stu-id="82df9-217">Then hello object is passed toohello **deleteEntity** method.</span></span>

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
> <span data-ttu-id="82df9-218">ETag-EK esetekben érdemes elemek, tooensure, amely hello elem törlése nem módosították egy másik folyamat.</span><span class="sxs-lookup"><span data-stu-id="82df9-218">Consider using ETags when deleting items, tooensure that hello item hasn't been modified by another process.</span></span> <span data-ttu-id="82df9-219">Lásd: [frissíthető entitás](#update-an-entity) információ az ETag-EK használatával.</span><span class="sxs-lookup"><span data-stu-id="82df9-219">See [Update an entity](#update-an-entity) for information on using ETags.</span></span>
>
>

## <a name="delete-a-table"></a><span data-ttu-id="82df9-220">Tábla törlése</span><span class="sxs-lookup"><span data-stu-id="82df9-220">Delete a table</span></span>
<span data-ttu-id="82df9-221">a következő kód hello törölhető egy tábla a tárfiókból.</span><span class="sxs-lookup"><span data-stu-id="82df9-221">hello following code deletes a table from a storage account.</span></span>

```nodejs
tableSvc.deleteTable('mytable', function(error, response){
    if(!error){
        // Table deleted
    }
});
```

<span data-ttu-id="82df9-222">Ha biztos benne, hogy hello tábla létezik, **deleteTableIfExists**.</span><span class="sxs-lookup"><span data-stu-id="82df9-222">If you are uncertain whether hello table exists, use **deleteTableIfExists**.</span></span>

## <a name="use-continuation-tokens"></a><span data-ttu-id="82df9-223">Használjon tokeneket folytatása</span><span class="sxs-lookup"><span data-stu-id="82df9-223">Use continuation tokens</span></span>
<span data-ttu-id="82df9-224">Ha nagy mennyiségű eredmények táblák kérdez le, keresse meg folytatási jogkivonatokat.</span><span class="sxs-lookup"><span data-stu-id="82df9-224">When you are querying tables for large amounts of results, look for continuation tokens.</span></span> <span data-ttu-id="82df9-225">Nagy mennyiségű adat lehet a lekérdezést, hogy még lehet, hogy nem megvalósítható Ha toorecognize nem hoz létre, ha jelen a folytatási kód érhető el.</span><span class="sxs-lookup"><span data-stu-id="82df9-225">There may be large amounts of data available for your query that you might not realize if you do not build toorecognize when a continuation token is present.</span></span>

<span data-ttu-id="82df9-226">hello eredmények objektum lekérdezése entitások beállítása során a `continuationToken` tulajdonságot, ha ilyen tokent.</span><span class="sxs-lookup"><span data-stu-id="82df9-226">hello results object returned during querying entities sets a `continuationToken` property when such a token is present.</span></span> <span data-ttu-id="82df9-227">Majd ezzel a lekérdezés toocontinue toomove hello partíció és tábla entitások közötti végrehajtása során.</span><span class="sxs-lookup"><span data-stu-id="82df9-227">You can then use this when performing a query toocontinue toomove across hello partition and table entities.</span></span>

<span data-ttu-id="82df9-228">Kérdez le, ha egy continuationToken paraméter is megadható hello lekérdezés objektumpéldány és hello visszahívási függvény között:</span><span class="sxs-lookup"><span data-stu-id="82df9-228">When querying, a continuationToken parameter may be provided between hello query object instance and hello callback function:</span></span>

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

<span data-ttu-id="82df9-229">Ha hello nézze meg `continuationToken` objektum található tulajdonságok például `nextPartitionKey`, `nextRowKey` és `targetLocation`, amely minden hello eredmény keresztül használt tooiterate lehet.</span><span class="sxs-lookup"><span data-stu-id="82df9-229">If you inspect hello `continuationToken` object, you will find properties such as `nextPartitionKey`, `nextRowKey` and `targetLocation`, which can be used tooiterate through all hello results.</span></span>

<span data-ttu-id="82df9-230">Hello Azure Storage Node.js tárház a Githubon belül folytatási minta is van.</span><span class="sxs-lookup"><span data-stu-id="82df9-230">There is also a continuation sample within hello Azure Storage Node.js repo on GitHub.</span></span> <span data-ttu-id="82df9-231">Keressen `examples/samples/continuationsample.js`.</span><span class="sxs-lookup"><span data-stu-id="82df9-231">Look for `examples/samples/continuationsample.js`.</span></span>

## <a name="work-with-shared-access-signatures"></a><span data-ttu-id="82df9-232">Megosztott hozzáférési aláírásokkal működik</span><span class="sxs-lookup"><span data-stu-id="82df9-232">Work with shared access signatures</span></span>
<span data-ttu-id="82df9-233">Közös hozzáférésű jogosultságkód (SAS) anélkül, hogy a tárfiók neve vagy a kulcsok egy biztonságos módon tooprovide részletes hozzáférés tootables.</span><span class="sxs-lookup"><span data-stu-id="82df9-233">Shared access signatures (SAS) are a secure way tooprovide granular access tootables without providing your storage account name or keys.</span></span> <span data-ttu-id="82df9-234">SAS olyan gyakran használt tooprovide korlátozott hozzáférés tooyour adatok, például így a mobilalkalmazások tooquery rögzíti.</span><span class="sxs-lookup"><span data-stu-id="82df9-234">SAS are often used tooprovide limited access tooyour data, such as allowing a mobile app tooquery records.</span></span>

<span data-ttu-id="82df9-235">A megbízható alkalmazások, például egy felhőalapú szolgáltatás hoz létre egy SAS használatával hello **generateSharedAccessSignature** a hello **TableService**, tooan biztosítja azt, és nem megbízható vagy félig megbízható alkalmazás például egy mobilalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="82df9-235">A trusted application such as a cloud-based service generates a SAS using hello **generateSharedAccessSignature** of hello **TableService**, and provides it tooan untrusted or semi-trusted application such as a mobile app.</span></span> <span data-ttu-id="82df9-236">hello SAS egy házirendet, amely ismerteti a hello kezdő és záró dátumát, mely hello során SAS érvénytelen jön létre, valamint hello hozzáférési szint megadott toohello SAS tulajdonosával.</span><span class="sxs-lookup"><span data-stu-id="82df9-236">hello SAS is generated using a policy, which describes hello start and end dates during which hello SAS is valid, as well as hello access level granted toohello SAS holder.</span></span>

<span data-ttu-id="82df9-237">hello alábbi példa létrehoz egy új megosztott elérési házirendet, amely lehetővé teszi a hello SAS jogosult tooquery (r) hello tábla, és 100 perc létrehozták hello idő múlva lejár.</span><span class="sxs-lookup"><span data-stu-id="82df9-237">hello following example generates a new shared access policy that will allow hello SAS holder tooquery ('r') hello table, and expires 100 minutes after hello time it is created.</span></span>

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

<span data-ttu-id="82df9-238">Vegye figyelembe, hogy hello állomásadatai csak megadott is szükség rá, amikor hello SAS jogosult megpróbálja tooaccess hello tábla.</span><span class="sxs-lookup"><span data-stu-id="82df9-238">Note that hello host information must be provided also, as it is required when hello SAS holder attempts tooaccess hello table.</span></span>

<span data-ttu-id="82df9-239">hello ügyfélalkalmazást, akkor használja a biztonsági Társítások hello **TableServiceWithSAS** tooperform műveletek hello táblázaton.</span><span class="sxs-lookup"><span data-stu-id="82df9-239">hello client application then uses hello SAS with **TableServiceWithSAS** tooperform operations against hello table.</span></span> <span data-ttu-id="82df9-240">a következő példa hello toohello tábla kapcsolódik, és lekérdezést hajt végre.</span><span class="sxs-lookup"><span data-stu-id="82df9-240">hello following example connects toohello table and performs a query.</span></span>

```nodejs
var sharedTableService = azure.createTableServiceWithSas(host, tableSAS);
var query = azure.TableQuery()
  .where('PartitionKey eq ?', 'hometasks');

sharedTableService.queryEntities(query, null, function(error, result, response) {
  if(!error) {
    // result contains hello entities
  }
});
```

<span data-ttu-id="82df9-241">Hello SAS generálása óta lekérdezés hozzáférést, csak ha kísérlet történt tooinsert, frissítése vagy törlése entitások, akkor kell hibát jelez.</span><span class="sxs-lookup"><span data-stu-id="82df9-241">Since hello SAS was generated with only query access, if an attempt were made tooinsert, update, or delete entities, an error would be returned.</span></span>

### <a name="access-control-lists"></a><span data-ttu-id="82df9-242">Hozzáférés-vezérlési listák</span><span class="sxs-lookup"><span data-stu-id="82df9-242">Access Control Lists</span></span>
<span data-ttu-id="82df9-243">A hozzáférés-vezérlési lista (ACL) tooset hello hozzáférési házirendek tartozó SAS korlátozására is használhatja.</span><span class="sxs-lookup"><span data-stu-id="82df9-243">You can also use an Access Control List (ACL) tooset hello access policy for a SAS.</span></span> <span data-ttu-id="82df9-244">Ez akkor hasznos, ha akarja tooallow több ügyfelek tooaccess hello táblában, de adjon meg másik hozzáférési házirendeket az egyes ügyfelekhez.</span><span class="sxs-lookup"><span data-stu-id="82df9-244">This is useful if you wish tooallow multiple clients tooaccess hello table, but provide different access policies for each client.</span></span>

<span data-ttu-id="82df9-245">Hozzáférés-vezérlési Listában hozzáférési házirendeket, tömbje segítségével minden házirendhez társított azonosítójú van megvalósítva.</span><span class="sxs-lookup"><span data-stu-id="82df9-245">An ACL is implemented using an array of access policies, with an ID associated with each policy.</span></span> <span data-ttu-id="82df9-246">a következő példa hello két házirend, egy "felhasználó1", és egy "felhasználó2" határozza meg:</span><span class="sxs-lookup"><span data-stu-id="82df9-246">hello following example defines two policies, one for 'user1' and one for 'user2':</span></span>

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

<span data-ttu-id="82df9-247">a következő példa lekérdezi hello hello aktuális hozzáférés-vezérlési listája hello **hometasks** tábla, és hozzáadja az új házirendeket hello segítségével **setTableAcl**.</span><span class="sxs-lookup"><span data-stu-id="82df9-247">hello following example gets hello current ACL for hello **hometasks** table, and then adds hello new policies using **setTableAcl**.</span></span> <span data-ttu-id="82df9-248">Ez a megközelítés lehetővé teszi, hogy:</span><span class="sxs-lookup"><span data-stu-id="82df9-248">This approach allows:</span></span>

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

<span data-ttu-id="82df9-249">Egyszer hello hozzáférés-vezérlési lista van beállítva, majd létrehozhat egy SAS-házirendje hello azonosítója alapján.</span><span class="sxs-lookup"><span data-stu-id="82df9-249">Once hello ACL has been set, you can then create a SAS based on hello ID for a policy.</span></span> <span data-ttu-id="82df9-250">a következő példa hello hoz létre egy új SAS-kód "felhasználó2":</span><span class="sxs-lookup"><span data-stu-id="82df9-250">hello following example creates a new SAS for 'user2':</span></span>

```nodejs
tableSAS = tableSvc.generateSharedAccessSignature('hometasks', { Id: 'user2' });
```

## <a name="next-steps"></a><span data-ttu-id="82df9-251">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="82df9-251">Next steps</span></span>
<span data-ttu-id="82df9-252">További információkért tekintse meg a következő erőforrások hello.</span><span class="sxs-lookup"><span data-stu-id="82df9-252">For more information, see hello following resources.</span></span>

* <span data-ttu-id="82df9-253">[A Microsoft Azure Tártallózó](../vs-azure-tools-storage-manage-with-storage-explorer.md) egy ingyenes, különálló alkalmazás, amely lehetővé teszi toowork vizuálisan macOS, Linux és a Windows Azure Storage-adatokat a Microsoft.</span><span class="sxs-lookup"><span data-stu-id="82df9-253">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) is a free, standalone app from Microsoft that enables you toowork visually with Azure Storage data on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="82df9-254">[Az Azure Storage szolgáltatás SDK csomópont](https://github.com/Azure/azure-storage-node) GitHub tárházából.</span><span class="sxs-lookup"><span data-stu-id="82df9-254">[Azure Storage SDK for Node](https://github.com/Azure/azure-storage-node) repository on GitHub.</span></span>
* [<span data-ttu-id="82df9-255">Node.js fejlesztői központ</span><span class="sxs-lookup"><span data-stu-id="82df9-255">Node.js Developer Center</span></span>](/develop/nodejs/)
* [<span data-ttu-id="82df9-256">Létrehozhat és telepíthet egy Node.js-alkalmazás tooan Azure-webhelyen</span><span class="sxs-lookup"><span data-stu-id="82df9-256">Create and deploy a Node.js application tooan Azure website</span></span>](../app-service-web/app-service-web-get-started-nodejs.md)
