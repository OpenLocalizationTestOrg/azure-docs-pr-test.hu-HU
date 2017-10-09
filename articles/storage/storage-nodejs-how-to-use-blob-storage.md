---
title: "Blob storage-ának Node.js aaaHow toouse |} Microsoft Docs"
description: "Strukturálatlan adatok tárolása az Azure Blob storage (object storage) hello felhő."
services: storage
documentationcenter: nodejs
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 8b0df222-1ca8-4967-8248-6d6d720947b8
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 12/08/2016
ms.author: marsma
ms.openlocfilehash: e405eecdc60cd1eaa77510e7b29b41269372b65e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-blob-storage-from-nodejs"></a><span data-ttu-id="356af-103">Hogyan toouse Blob-tároló Node.js-ből</span><span class="sxs-lookup"><span data-stu-id="356af-103">How toouse Blob storage from Node.js</span></span>
[!INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-check-out-samples-all](../../includes/storage-check-out-samples-all.md)]

## <a name="overview"></a><span data-ttu-id="356af-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="356af-104">Overview</span></span>
<span data-ttu-id="356af-105">Ez a cikk bemutatja, hogyan tooperform szolgáltatást használó általános forgatókönyvhöz Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="356af-105">This article shows you how tooperform common scenarios using Blob storage.</span></span> <span data-ttu-id="356af-106">hello minták hello Node.js API használatával készültek.</span><span class="sxs-lookup"><span data-stu-id="356af-106">hello samples are written via hello Node.js API.</span></span> <span data-ttu-id="356af-107">hello jelzett esetek hogyan tooupload, listázása, letöltése és blobok törlése.</span><span class="sxs-lookup"><span data-stu-id="356af-107">hello scenarios covered include how tooupload, list, download, and delete blobs.</span></span>

[!INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-nodejs-application"></a><span data-ttu-id="356af-108">Node.js alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="356af-108">Create a Node.js application</span></span>
<span data-ttu-id="356af-109">Útmutatást toocreate egy Node.js-alkalmazást, lásd: [Node.js-webalkalmazás létrehozása az Azure App Service], [és üzembe egy Node.js-alkalmazás tooan Azure Cloud Service] – Windows PowerShell használatával , vagy [létrehozása és központi telepítése a Node.js web app tooAzure Web Matrix].</span><span class="sxs-lookup"><span data-stu-id="356af-109">For instructions on how toocreate a Node.js application, see [Create a Node.js web app in Azure App Service], [Build and deploy a Node.js application tooan Azure Cloud Service] -- using Windows PowerShell, or [Build and deploy a Node.js web app tooAzure using Web Matrix].</span></span>

## <a name="configure-your-application-tooaccess-storage"></a><span data-ttu-id="356af-110">Az alkalmazás tooaccess tároló konfigurálása</span><span class="sxs-lookup"><span data-stu-id="356af-110">Configure your application tooaccess storage</span></span>
<span data-ttu-id="356af-111">az Azure storage toouse, kell hello Azure Storage szolgáltatás SDK a Node.js, amely kényelmi szalagtárak hello tárolószolgáltatások REST kommunikációt készletét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="356af-111">toouse Azure storage, you need hello Azure Storage SDK for Node.js, which includes a set of convenience libraries that communicate with hello storage REST services.</span></span>

### <a name="use-node-package-manager-npm-tooobtain-hello-package"></a><span data-ttu-id="356af-112">Csomópont Package Manager (NPM) tooobtain hello csomag használata</span><span class="sxs-lookup"><span data-stu-id="356af-112">Use Node Package Manager (NPM) tooobtain hello package</span></span>
1. <span data-ttu-id="356af-113">Használjon például egy parancssori felületet **PowerShell** (Windows), **Terminálszolgáltatások** (Mac), vagy **Bash** (Unix) toonavigate toohello mappa, amelyben létrehozta a minta az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="356af-113">Use a command-line interface such as **PowerShell** (Windows), **Terminal** (Mac), or **Bash** (Unix), toonavigate toohello folder where you created your sample application.</span></span>
2. <span data-ttu-id="356af-114">Típus **npm telepítése azure-tároló** hello parancssori ablakban.</span><span class="sxs-lookup"><span data-stu-id="356af-114">Type **npm install azure-storage** in hello command window.</span></span> <span data-ttu-id="356af-115">Hello parancs az alábbi kódpéldát hasonló toohello.</span><span class="sxs-lookup"><span data-stu-id="356af-115">Output from hello command is similar toohello following code example.</span></span>

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
3. <span data-ttu-id="356af-116">Manuális futtatásával hello **ls** parancs tooverify, amely egy **csomópont\_modulok** mappa hozták létre.</span><span class="sxs-lookup"><span data-stu-id="356af-116">You can manually run hello **ls** command tooverify that a **node\_modules** folder was created.</span></span> <span data-ttu-id="356af-117">A mappában található hello **azure-tároló** csomagot, amely tartalmazza, hogy kell-e tooaccess tárolási hello szalagtárak.</span><span class="sxs-lookup"><span data-stu-id="356af-117">Inside that folder, find hello **azure-storage** package, which contains hello libraries that you need tooaccess storage.</span></span>

### <a name="import-hello-package"></a><span data-ttu-id="356af-118">Hello csomag importálása</span><span class="sxs-lookup"><span data-stu-id="356af-118">Import hello package</span></span>
<span data-ttu-id="356af-119">A Jegyzettömbben vagy más szövegszerkesztőben, adja hozzá a következő toohello felső részén hello hello **server.js** toouse tárolási célhelyeként hello alkalmazás fájlt:</span><span class="sxs-lookup"><span data-stu-id="356af-119">Using Notepad or another text editor, add hello following toohello top of hello **server.js** file of hello application where you intend toouse storage:</span></span>

```nodejs
var azure = require('azure-storage');
```

## <a name="set-up-an-azure-storage-connection"></a><span data-ttu-id="356af-120">Egy Azure Storage-kapcsolat beállítása</span><span class="sxs-lookup"><span data-stu-id="356af-120">Set up an Azure Storage connection</span></span>
<span data-ttu-id="356af-121">hello Azure modul olvassák hello környezeti változók `AZURE_STORAGE_ACCOUNT` és `AZURE_STORAGE_ACCESS_KEY`, vagy `AZURE_STORAGE_CONNECTION_STRING`, információ tooconnect tooyour Azure storage-fiók szükséges.</span><span class="sxs-lookup"><span data-stu-id="356af-121">hello Azure module will read hello environment variables `AZURE_STORAGE_ACCOUNT` and `AZURE_STORAGE_ACCESS_KEY`, or `AZURE_STORAGE_CONNECTION_STRING`, for information required tooconnect tooyour Azure storage account.</span></span> <span data-ttu-id="356af-122">Ha ezek a környezeti változók nem, meg kell adnia hello fiók meghívásakor **createBlobService**.</span><span class="sxs-lookup"><span data-stu-id="356af-122">If these environment variables are not set, you must specify hello account information when calling **createBlobService**.</span></span>

<span data-ttu-id="356af-123">Példa a hello hello környezeti változók beállítása [Azure-portálon](https://portal.azure.com) Azure-webalkalmazás, lásd: [Node.js web app használatával hello Azure Table szolgáltatás].</span><span class="sxs-lookup"><span data-stu-id="356af-123">For an example of setting hello environment variables in hello [Azure portal](https://portal.azure.com) for an Azure web app, see [Node.js web app using hello Azure Table Service].</span></span>

## <a name="create-a-container"></a><span data-ttu-id="356af-124">Tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="356af-124">Create a container</span></span>
<span data-ttu-id="356af-125">Hello **BlobService** objektum lehetővé teszi, hogy a tárolók és blobok.</span><span class="sxs-lookup"><span data-stu-id="356af-125">hello **BlobService** object lets you work with containers and blobs.</span></span> <span data-ttu-id="356af-126">hello alábbi kód létrehoz egy **BlobService** objektum.</span><span class="sxs-lookup"><span data-stu-id="356af-126">hello following code creates a **BlobService** object.</span></span> <span data-ttu-id="356af-127">Adja hozzá hello következő tetején hello **server.js**:</span><span class="sxs-lookup"><span data-stu-id="356af-127">Add hello following near hello top of **server.js**:</span></span>

```nodejs
var blobSvc = azure.createBlobService();
```

> [!NOTE]
> <span data-ttu-id="356af-128">Akkor érhető el blob névtelenül **createBlobServiceAnonymous** hello állomás cím megadásával.</span><span class="sxs-lookup"><span data-stu-id="356af-128">You can access a blob anonymously by using **createBlobServiceAnonymous** and providing hello host address.</span></span> <span data-ttu-id="356af-129">Tegyük fel például, `var blobSvc = azure.createBlobServiceAnonymous('https://myblob.blob.core.windows.net/');`.</span><span class="sxs-lookup"><span data-stu-id="356af-129">For example, use `var blobSvc = azure.createBlobServiceAnonymous('https://myblob.blob.core.windows.net/');`.</span></span>
>
>

[!INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

<span data-ttu-id="356af-130">egy új tároló toocreate használja **createContainerIfNotExists**.</span><span class="sxs-lookup"><span data-stu-id="356af-130">toocreate a new container, use **createContainerIfNotExists**.</span></span> <span data-ttu-id="356af-131">hello alábbi példakód létrehoz egy új tároló "mycontainer" nevű:</span><span class="sxs-lookup"><span data-stu-id="356af-131">hello following code example creates a new container named 'mycontainer':</span></span>

```nodejs
blobSvc.createContainerIfNotExists('mycontainer', function(error, result, response){
    if(!error){
      // Container exists and is private
    }
});
```

<span data-ttu-id="356af-132">Hello tároló újonnan jön létre, ha `result.created` értéke true.</span><span class="sxs-lookup"><span data-stu-id="356af-132">If hello container is newly created, `result.created` is true.</span></span> <span data-ttu-id="356af-133">Ha hello tároló már létezik, `result.created` értéke "false".</span><span class="sxs-lookup"><span data-stu-id="356af-133">If hello container already exists, `result.created` is false.</span></span> <span data-ttu-id="356af-134">`response`hello művelet hello ETag párbeszédeket hello a tároló adatait tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="356af-134">`response` contains information about hello operation, including hello ETag information for hello container.</span></span>

### <a name="container-security"></a><span data-ttu-id="356af-135">Tároló biztonsági</span><span class="sxs-lookup"><span data-stu-id="356af-135">Container security</span></span>
<span data-ttu-id="356af-136">Alapértelmezés szerint az új tárolók személyesek, és nem érhető el névtelen hozzáféréssel.</span><span class="sxs-lookup"><span data-stu-id="356af-136">By default, new containers are private and cannot be accessed anonymously.</span></span> <span data-ttu-id="356af-137">nyilvános, hogy hozzá tud férni névtelenül toomake hello tároló, állíthatja be hello tároló hozzáférési szint túl**blob** vagy **tároló**.</span><span class="sxs-lookup"><span data-stu-id="356af-137">toomake hello container public so that you can access it anonymously, you can set hello container's access level too**blob** or **container**.</span></span>

* <span data-ttu-id="356af-138">**a BLOB** -lehetővé teszi a névtelen olvasási hozzáférés tooblob tartalom és metaadatok található, de nem toocontainer metaadatainak, például az összes BLOB a tárolóban lévő listázása</span><span class="sxs-lookup"><span data-stu-id="356af-138">**blob** - allows anonymous read access tooblob content and metadata within this container, but not toocontainer metadata such as listing all blobs within a container</span></span>
* <span data-ttu-id="356af-139">**tároló** -lehetővé teszi a névtelen olvasási hozzáférés tooblob tartalom és a metaadatok, valamint a csomagtároló metaadatai</span><span class="sxs-lookup"><span data-stu-id="356af-139">**container** - allows anonymous read access tooblob content and metadata as well as container metadata</span></span>

<span data-ttu-id="356af-140">hello alábbi példakód mutatja be érvényes hello hozzáférési szint túl**blob**:</span><span class="sxs-lookup"><span data-stu-id="356af-140">hello following code example demonstrates setting hello access level too**blob**:</span></span>

```nodejs
blobSvc.createContainerIfNotExists('mycontainer', {publicAccessLevel : 'blob'}, function(error, result, response){
    if(!error){
      // Container exists and allows
      // anonymous read access tooblob
      // content and metadata within this container
    }
});
```

<span data-ttu-id="356af-141">Azt is megteheti, módosíthatja a tároló hello hozzáférési szint használatával **setContainerAcl** toospecify hello hozzáférési szintet.</span><span class="sxs-lookup"><span data-stu-id="356af-141">Alternatively, you can modify hello access level of a container by using **setContainerAcl** toospecify hello access level.</span></span> <span data-ttu-id="356af-142">hello következő példa módosítások hello hozzáférési szint toocontainer kódot:</span><span class="sxs-lookup"><span data-stu-id="356af-142">hello following code example changes hello access level toocontainer:</span></span>

```nodejs
blobSvc.setContainerAcl('mycontainer', null /* signedIdentifiers */, {publicAccessLevel : 'container'} /* publicAccessLevel*/, function(error, result, response){
  if(!error){
    // Container access level set too'container'
  }
});
```

<span data-ttu-id="356af-143">hello eredmény információt tartalmaz hello műveletek, például a hello aktuális **ETag** hello tároló.</span><span class="sxs-lookup"><span data-stu-id="356af-143">hello result contains information about hello operation, including hello current **ETag** for hello container.</span></span>

### <a name="filters"></a><span data-ttu-id="356af-144">Szűrők</span><span class="sxs-lookup"><span data-stu-id="356af-144">Filters</span></span>
<span data-ttu-id="356af-145">Nem kötelező szűrési műveletek végre toooperations használatával is alkalmazhat **BlobService**.</span><span class="sxs-lookup"><span data-stu-id="356af-145">You can apply optional filtering operations toooperations performed using **BlobService**.</span></span> <span data-ttu-id="356af-146">Műveletek szűrésének lehetnek naplózási, automatikus újrapróbálkozása, stb. Szűrők hello aláírással metódus megvalósító objektumok a következők:</span><span class="sxs-lookup"><span data-stu-id="356af-146">Filtering operations can include logging, automatically retrying, etc. Filters are objects that implement a method with hello signature:</span></span>

```nodejs
function handle (requestOptions, next)
```

<span data-ttu-id="356af-147">Ezután a hello lehetőségek előfeldolgozása, hello metódust kell toocall "Tovább" gombra, egy visszahívási átadja a aláírását hello:</span><span class="sxs-lookup"><span data-stu-id="356af-147">After doing its preprocessing on hello request options, hello method needs toocall "next", passing a callback with hello following signature:</span></span>

```nodejs
function (returnObject, finalCallback, next)
```

<span data-ttu-id="356af-148">A visszahívási és hello returnObject (hello hello kérelem toohello kiszolgálótól kapott válasz) feldolgozása után hello visszahívási kell tooeither következő meghívni, ha más szűrők feldolgozása toocontinue létezik, vagy egyszerűen a finalCallback tooend hello szolgáltatás meghívása hívása.</span><span class="sxs-lookup"><span data-stu-id="356af-148">In this callback, and after processing hello returnObject (hello response from hello request toohello server), hello callback needs tooeither invoke next if it exists toocontinue processing other filters or simply invoke finalCallback tooend hello service invocation.</span></span>

<span data-ttu-id="356af-149">Két szűrőket, amelyek megvalósítják az újrapróbálkozási logika érhetők el a hello Azure SDK for Node.js, **ExponentialRetryPolicyFilter** és **LinearRetryPolicyFilter**.</span><span class="sxs-lookup"><span data-stu-id="356af-149">Two filters that implement retry logic are included with hello Azure SDK for Node.js, **ExponentialRetryPolicyFilter** and **LinearRetryPolicyFilter**.</span></span> <span data-ttu-id="356af-150">hello következő létrehoz egy **BlobService** hello használó objektum **ExponentialRetryPolicyFilter**:</span><span class="sxs-lookup"><span data-stu-id="356af-150">hello following creates a **BlobService** object that uses hello **ExponentialRetryPolicyFilter**:</span></span>

```nodejs
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var blobSvc = azure.createBlobService().withFilter(retryOperations);
```

## <a name="upload-a-blob-into-a-container"></a><span data-ttu-id="356af-151">Blobok feltöltése a tárolóba</span><span class="sxs-lookup"><span data-stu-id="356af-151">Upload a blob into a container</span></span>
<span data-ttu-id="356af-152">Nincsenek a blobok három különböző típusa: blokkblobokat, lapblobokat és hozzáfűző blobokat.</span><span class="sxs-lookup"><span data-stu-id="356af-152">There are three types of blobs: block blobs, page blobs and append blobs.</span></span> <span data-ttu-id="356af-153">Blokkblobok lehetővé teszik toomore hatékonyan nagy fájlok feltöltése.</span><span class="sxs-lookup"><span data-stu-id="356af-153">Block blobs allow you toomore efficiently upload large data.</span></span> <span data-ttu-id="356af-154">Hozzáfűző blobok vannak optimalizálva, műveletek hozzáfűzésére.</span><span class="sxs-lookup"><span data-stu-id="356af-154">Append blobs are optimized for append operations.</span></span> <span data-ttu-id="356af-155">Optimalizált blobok, amelyek az olvasási/írási műveletek.</span><span class="sxs-lookup"><span data-stu-id="356af-155">Page blobs are optimized for read/write operations.</span></span> <span data-ttu-id="356af-156">További információkért lásd: [ismertetése Blokkblobokat, hozzáfűző blobokat és Lapblobokat](http://msdn.microsoft.com/library/azure/ee691964.aspx).</span><span class="sxs-lookup"><span data-stu-id="356af-156">For more information, see [Understanding Block Blobs, Append Blobs, and Page Blobs](http://msdn.microsoft.com/library/azure/ee691964.aspx).</span></span>

### <a name="block-blobs"></a><span data-ttu-id="356af-157">Blokkblobok</span><span class="sxs-lookup"><span data-stu-id="356af-157">Block blobs</span></span>
<span data-ttu-id="356af-158">tooupload adatok tooa blokkblob, a következő használatát hello:</span><span class="sxs-lookup"><span data-stu-id="356af-158">tooupload data tooa block blob, use hello following:</span></span>

* <span data-ttu-id="356af-159">**createBlockBlobFromLocalFile** - hoz létre egy új blokkblob, és feltölti a fájl tartalmának hello</span><span class="sxs-lookup"><span data-stu-id="356af-159">**createBlockBlobFromLocalFile** - creates a new block blob and uploads hello contents of a file</span></span>
* <span data-ttu-id="356af-160">**createBlockBlobFromStream** - hoz létre egy új blokkblob, és feltölti a hello tartalmát egy stream</span><span class="sxs-lookup"><span data-stu-id="356af-160">**createBlockBlobFromStream** - creates a new block blob and uploads hello contents of a stream</span></span>
* <span data-ttu-id="356af-161">**createBlockBlobFromText** - hoz létre egy új blokkblob és feltölt egy karakterlánc hello tartalmát</span><span class="sxs-lookup"><span data-stu-id="356af-161">**createBlockBlobFromText** - creates a new block blob and uploads hello contents of a string</span></span>
* <span data-ttu-id="356af-162">**createWriteStreamToBlockBlob** -biztosít egy írási adatfolyam tooa blokkblob</span><span class="sxs-lookup"><span data-stu-id="356af-162">**createWriteStreamToBlockBlob** - provides a write stream tooa block blob</span></span>

<span data-ttu-id="356af-163">hello alábbi példakód feltölt hello hello tartalmát **teszt.txt** fájlt **myblob**.</span><span class="sxs-lookup"><span data-stu-id="356af-163">hello following code example uploads hello contents of hello **test.txt** file into **myblob**.</span></span>

```nodejs
blobSvc.createBlockBlobFromLocalFile('mycontainer', 'myblob', 'test.txt', function(error, result, response){
  if(!error){
    // file uploaded
  }
});
```

<span data-ttu-id="356af-164">Hello `result` ezen módszerek által visszaadott hello művelet, például a hello információkat tartalmaz az **ETag** hello BLOB.</span><span class="sxs-lookup"><span data-stu-id="356af-164">hello `result` returned by these methods contains information on hello operation, such as hello **ETag** of hello blob.</span></span>

### <a name="append-blobs"></a><span data-ttu-id="356af-165">Hozzáfűző blobokat</span><span class="sxs-lookup"><span data-stu-id="356af-165">Append blobs</span></span>
<span data-ttu-id="356af-166">tooupload adatok tooa új hozzáfűző blob, használja a következő hello:</span><span class="sxs-lookup"><span data-stu-id="356af-166">tooupload data tooa new append blob, use hello following:</span></span>

* <span data-ttu-id="356af-167">**createAppendBlobFromLocalFile** - hoz létre egy új hozzáfűző blob, és feltölti a fájl tartalmának hello</span><span class="sxs-lookup"><span data-stu-id="356af-167">**createAppendBlobFromLocalFile** - creates a new append blob and uploads hello contents of a file</span></span>
* <span data-ttu-id="356af-168">**createAppendBlobFromStream** - hoz létre egy új hozzáfűző blob, és feltölti a hello tartalmát egy stream</span><span class="sxs-lookup"><span data-stu-id="356af-168">**createAppendBlobFromStream** - creates a new append blob and uploads hello contents of a stream</span></span>
* <span data-ttu-id="356af-169">**createAppendBlobFromText** - hoz létre egy új hozzáfűző blob és feltölt egy karakterlánc hello tartalmát</span><span class="sxs-lookup"><span data-stu-id="356af-169">**createAppendBlobFromText** - creates a new append blob and uploads hello contents of a string</span></span>
* <span data-ttu-id="356af-170">**createWriteStreamToNewAppendBlob** - hoz létre egy új hozzáfűző blob, majd egy adatfolyam toowrite tooit</span><span class="sxs-lookup"><span data-stu-id="356af-170">**createWriteStreamToNewAppendBlob** - creates a new append blob and then provides a stream toowrite tooit</span></span>

<span data-ttu-id="356af-171">hello alábbi példakód feltölt hello hello tartalmát **teszt.txt** fájlt **myappendblob**.</span><span class="sxs-lookup"><span data-stu-id="356af-171">hello following code example uploads hello contents of hello **test.txt** file into **myappendblob**.</span></span>

```nodejs
blobSvc.createAppendBlobFromLocalFile('mycontainer', 'myappendblob', 'test.txt', function(error, result, response){
  if(!error){
    // file uploaded
  }
});
```

<span data-ttu-id="356af-172">tooappend egy blokk tooan létező hozzáfűző blob, a következő használatát hello:</span><span class="sxs-lookup"><span data-stu-id="356af-172">tooappend a block tooan existing append blob, use hello following:</span></span>

* <span data-ttu-id="356af-173">**appendFromLocalFile** -hozzáfűzése hello tartalmát, a fájl tooan a hozzáfűző blob meglévő</span><span class="sxs-lookup"><span data-stu-id="356af-173">**appendFromLocalFile** - append hello contents of a file tooan existing append blob</span></span>
* <span data-ttu-id="356af-174">**appendFromStream** -hozzáfűzése hello tartalmát egy stream tooan a meglévő hozzáfűző blob</span><span class="sxs-lookup"><span data-stu-id="356af-174">**appendFromStream** - append hello contents of a stream tooan existing append blob</span></span>
* <span data-ttu-id="356af-175">**appendFromText** -hello tartalma hozzáfűzése egy karakterlánc tooan a meglévő hozzáfűző blob</span><span class="sxs-lookup"><span data-stu-id="356af-175">**appendFromText** - append hello contents of a string tooan existing append blob</span></span>
* <span data-ttu-id="356af-176">**appendBlockFromStream** -hozzáfűzése hello tartalmát egy stream tooan a meglévő hozzáfűző blob</span><span class="sxs-lookup"><span data-stu-id="356af-176">**appendBlockFromStream** - append hello contents of a stream tooan existing append blob</span></span>
* <span data-ttu-id="356af-177">**appendBlockFromText** -hello tartalma hozzáfűzése egy karakterlánc tooan a meglévő hozzáfűző blob</span><span class="sxs-lookup"><span data-stu-id="356af-177">**appendBlockFromText** - append hello contents of a string tooan existing append blob</span></span>

> [!NOTE]
> <span data-ttu-id="356af-178">API-k appendFromXXX néhány ügyféloldali ellenőrzés toofail gyors tooavoid szükségtelen server hívás fog tenni.</span><span class="sxs-lookup"><span data-stu-id="356af-178">appendFromXXX APIs will do some client-side validation toofail fast tooavoid unnecessary server calls.</span></span> <span data-ttu-id="356af-179">appendBlockFromXXX nem.</span><span class="sxs-lookup"><span data-stu-id="356af-179">appendBlockFromXXX won't.</span></span>
>
>

<span data-ttu-id="356af-180">hello alábbi példakód feltölt hello hello tartalmát **teszt.txt** fájlt **myappendblob**.</span><span class="sxs-lookup"><span data-stu-id="356af-180">hello following code example uploads hello contents of hello **test.txt** file into **myappendblob**.</span></span>

```nodejs
blobSvc.appendFromText('mycontainer', 'myappendblob', 'text toobe appended', function(error, result, response){
  if(!error){
    // text appended
  }
});
```

### <a name="page-blobs"></a><span data-ttu-id="356af-181">Lapblobok</span><span class="sxs-lookup"><span data-stu-id="356af-181">Page blobs</span></span>
<span data-ttu-id="356af-182">tooupload adatok tooa oldalakra vonatkozó blob, a következő használatát hello:</span><span class="sxs-lookup"><span data-stu-id="356af-182">tooupload data tooa page blob, use hello following:</span></span>

* <span data-ttu-id="356af-183">**createPageBlob** -hoz létre egy új oldalakra vonatkozó blob egy meghatározott hosszúságú</span><span class="sxs-lookup"><span data-stu-id="356af-183">**createPageBlob** - creates a new page blob of a specific length</span></span>
* <span data-ttu-id="356af-184">**createPageBlobFromLocalFile** - hoz létre egy új oldalakra vonatkozó blob, és feltölti a fájl tartalmának hello</span><span class="sxs-lookup"><span data-stu-id="356af-184">**createPageBlobFromLocalFile** - creates a new page blob and uploads hello contents of a file</span></span>
* <span data-ttu-id="356af-185">**createPageBlobFromStream** - hoz létre egy új oldalakra vonatkozó blob, és feltölti a hello tartalmát egy stream</span><span class="sxs-lookup"><span data-stu-id="356af-185">**createPageBlobFromStream** - creates a new page blob and uploads hello contents of a stream</span></span>
* <span data-ttu-id="356af-186">**createWriteStreamToExistingPageBlob** -írási adatfolyam tooan meglévő oldalakra vonatkozó blob biztosít</span><span class="sxs-lookup"><span data-stu-id="356af-186">**createWriteStreamToExistingPageBlob** - provides a write stream tooan existing page blob</span></span>
* <span data-ttu-id="356af-187">**createWriteStreamToNewPageBlob** - hoz létre egy új oldalakra vonatkozó blob, majd egy adatfolyam toowrite tooit</span><span class="sxs-lookup"><span data-stu-id="356af-187">**createWriteStreamToNewPageBlob** - creates a new page blob and then provides a stream toowrite tooit</span></span>

<span data-ttu-id="356af-188">hello alábbi példakód feltölt hello hello tartalmát **teszt.txt** fájlt **mypageblob**.</span><span class="sxs-lookup"><span data-stu-id="356af-188">hello following code example uploads hello contents of hello **test.txt** file into **mypageblob**.</span></span>

```nodejs
blobSvc.createPageBlobFromLocalFile('mycontainer', 'mypageblob', 'test.txt', function(error, result, response){
  if(!error){
    // file uploaded
  }
});
```

> [!NOTE]
> <span data-ttu-id="356af-189">Lapblobokat 512 bájtos "lap" alkotják.</span><span class="sxs-lookup"><span data-stu-id="356af-189">Page blobs consist of 512-byte 'pages'.</span></span> <span data-ttu-id="356af-190">Egy hibaüzenetet fog kapni, amely nincs 512 többszöröse méretű adatok feltöltésekor.</span><span class="sxs-lookup"><span data-stu-id="356af-190">You will receive an error when uploading data with a size that is not a multiple of 512.</span></span>
>
>

## <a name="list-hello-blobs-in-a-container"></a><span data-ttu-id="356af-191">Lista hello a tárolóban lévő blobok</span><span class="sxs-lookup"><span data-stu-id="356af-191">List hello blobs in a container</span></span>
<span data-ttu-id="356af-192">toolist hello BLOB a tárolóban lévő hello használata **listBlobsSegmented** metódust.</span><span class="sxs-lookup"><span data-stu-id="356af-192">toolist hello blobs in a container, use hello **listBlobsSegmented** method.</span></span> <span data-ttu-id="356af-193">Ha szeretné, hogy egy adott előtaggal rendelkező tooreturn blobok, **listBlobsSegmentedWithPrefix**.</span><span class="sxs-lookup"><span data-stu-id="356af-193">If you'd like tooreturn blobs with a specific prefix, use **listBlobsSegmentedWithPrefix**.</span></span>

```nodejs
blobSvc.listBlobsSegmented('mycontainer', null, function(error, result, response){
  if(!error){
      // result.entries contains hello entries
      // If not all blobs were returned, result.continuationToken has hello continuation token.
  }
});
```

<span data-ttu-id="356af-194">Hello `result` tartalmaz egy `entries` gyűjteményt, amely minden egyes blob leíró objektumok tömbje.</span><span class="sxs-lookup"><span data-stu-id="356af-194">hello `result` contains an `entries` collection, which is an array of objects that describe each blob.</span></span> <span data-ttu-id="356af-195">Összes BLOB nem adható vissza, ha hello `result` is biztosít a `continuationToken`, amely akár hello második paraméter tooretrieve további bejegyzéseknek is használhatja.</span><span class="sxs-lookup"><span data-stu-id="356af-195">If all blobs cannot be returned, hello `result` also provides a `continuationToken`, which you may use as hello second parameter tooretrieve additional entries.</span></span>

## <a name="download-blobs"></a><span data-ttu-id="356af-196">Blobok letöltése</span><span class="sxs-lookup"><span data-stu-id="356af-196">Download blobs</span></span>
<span data-ttu-id="356af-197">toodownload adatait egy blob hello következő használja:</span><span class="sxs-lookup"><span data-stu-id="356af-197">toodownload data from a blob, use hello following:</span></span>

* <span data-ttu-id="356af-198">**getBlobToLocalFile** -hello blob tartalma toofile írási műveletek</span><span class="sxs-lookup"><span data-stu-id="356af-198">**getBlobToLocalFile** - writes hello blob contents toofile</span></span>
* <span data-ttu-id="356af-199">**getBlobToStream** -ír hello blob tartalma tooa adatfolyam</span><span class="sxs-lookup"><span data-stu-id="356af-199">**getBlobToStream** - writes hello blob contents tooa stream</span></span>
* <span data-ttu-id="356af-200">**getBlobToText** -ír hello blob tartalmát tooa karakterlánc</span><span class="sxs-lookup"><span data-stu-id="356af-200">**getBlobToText** - writes hello blob contents tooa string</span></span>
* <span data-ttu-id="356af-201">**createReadStream** -biztosít egy adatfolyam tooread hello blobból</span><span class="sxs-lookup"><span data-stu-id="356af-201">**createReadStream** - provides a stream tooread from hello blob</span></span>

<span data-ttu-id="356af-202">hello alábbi példakód bemutatja használatával **getBlobToStream** toodownload hello tartalmát hello **myblob** blob-, és tárolja toohello **kimenet.txt** fájl használatával egy az adatfolyam:</span><span class="sxs-lookup"><span data-stu-id="356af-202">hello following code example demonstrates using **getBlobToStream** toodownload hello contents of hello **myblob** blob and store it toohello **output.txt** file by using a stream:</span></span>

```nodejs
var fs = require('fs');
blobSvc.getBlobToStream('mycontainer', 'myblob', fs.createWriteStream('output.txt'), function(error, result, response){
  if(!error){
    // blob retrieved
  }
});
```

<span data-ttu-id="356af-203">Hello `result` hello blob adatait tartalmazza többek között **ETag** információkat.</span><span class="sxs-lookup"><span data-stu-id="356af-203">hello `result` contains information about hello blob, including **ETag** information.</span></span>

## <a name="delete-a-blob"></a><span data-ttu-id="356af-204">Blob törlése</span><span class="sxs-lookup"><span data-stu-id="356af-204">Delete a blob</span></span>
<span data-ttu-id="356af-205">Végezetül toodelete blob, hívja **deleteBlob**.</span><span class="sxs-lookup"><span data-stu-id="356af-205">Finally, toodelete a blob, call **deleteBlob**.</span></span> <span data-ttu-id="356af-206">a következő példakód törlések hello nevű blob hello **myblob**.</span><span class="sxs-lookup"><span data-stu-id="356af-206">hello following code example deletes hello blob named **myblob**.</span></span>

```nodejs
blobSvc.deleteBlob(containerName, 'myblob', function(error, response){
  if(!error){
    // Blob has been deleted
  }
});
```

## <a name="concurrent-access"></a><span data-ttu-id="356af-207">Egyidejű hozzáférés</span><span class="sxs-lookup"><span data-stu-id="356af-207">Concurrent access</span></span>
<span data-ttu-id="356af-208">toosupport egyidejű hozzáférés tooa blob több ügyfél vagy a folyamat több példányt is használhat **ETag-EK** vagy **bérleteket**.</span><span class="sxs-lookup"><span data-stu-id="356af-208">toosupport concurrent access tooa blob from multiple clients or multiple process instances, you can use **ETags** or **leases**.</span></span>

* <span data-ttu-id="356af-209">**ETag** -tartalmaz egy módja toodetect, amely blob vagy tároló hello módosították egy másik folyamat</span><span class="sxs-lookup"><span data-stu-id="356af-209">**Etag** - provides a way toodetect that hello blob or container has been modified by another process</span></span>
* <span data-ttu-id="356af-210">**Címbérlet** - tartalmaz egy módja tooobtain kizárólagos, megújítható, írási vagy egy adott időn belül hozzáférés tooa blob törlése</span><span class="sxs-lookup"><span data-stu-id="356af-210">**Lease** - provides a way tooobtain exclusive, renewable, write or delete access tooa blob for a period of time</span></span>

### <a name="etag"></a><span data-ttu-id="356af-211">ETag</span><span class="sxs-lookup"><span data-stu-id="356af-211">ETag</span></span>
<span data-ttu-id="356af-212">ETag-EK használja, ha több ügyfelek vagy a példányok toowrite toohello blokk Blob vagy a lap Blob egyidejűleg tooallow van szüksége.</span><span class="sxs-lookup"><span data-stu-id="356af-212">Use ETags if you need tooallow multiple clients or instances toowrite toohello block Blob or page Blob simultaneously.</span></span> <span data-ttu-id="356af-213">hello ETag lehetővé teszi toodetermine hello tárolót vagy blobot óta módosították kezdetben olvasására vagy létrehozta, amely lehetővé teszi egy másik ügyfél vagy a folyamat által előjegyzett módosítások felülírását tooavoid.</span><span class="sxs-lookup"><span data-stu-id="356af-213">hello ETag allows you toodetermine if hello container or blob was modified since you initially read or created it, which allows you tooavoid overwriting changes committed by another client or process.</span></span>

<span data-ttu-id="356af-214">Választható hello segítségével beállíthatja az ETag feltételek `options.accessConditions` paraméter.</span><span class="sxs-lookup"><span data-stu-id="356af-214">You can set ETag conditions by using hello optional `options.accessConditions` parameter.</span></span> <span data-ttu-id="356af-215">hello következő Kódpélda csak feltölt hello **teszt.txt** által tartalmazott fájlok Ha hello blob már létezik, és hello ETag érték `etagToMatch`.</span><span class="sxs-lookup"><span data-stu-id="356af-215">hello following code example only uploads hello **test.txt** file if hello blob already exists and has hello ETag value contained by `etagToMatch`.</span></span>

```nodejs
blobSvc.createBlockBlobFromLocalFile('mycontainer', 'myblob', 'test.txt', { accessConditions: { EtagMatch: etagToMatch} }, function(error, result, response){
    if(!error){
    // file uploaded
  }
});
```

<span data-ttu-id="356af-216">ETag-EK használata esetén hello általános mintát esetén:</span><span class="sxs-lookup"><span data-stu-id="356af-216">When you're using ETags, hello general pattern is:</span></span>

1. <span data-ttu-id="356af-217">Szerezze be a hello ETag hello így létrehozása, a lista vagy a get műveletet.</span><span class="sxs-lookup"><span data-stu-id="356af-217">Obtain hello ETag as hello result of a create, list, or get operation.</span></span>
2. <span data-ttu-id="356af-218">Egy műveletet, hogy hello ETag érték ellenőrzése nem történt módosítás.</span><span class="sxs-lookup"><span data-stu-id="356af-218">Perform an action, checking that hello ETag value has not been modified.</span></span>

<span data-ttu-id="356af-219">Ha hello érték módosítva lett, az azt jelenti, hogy egy másik ügyfél vagy a példány hello blob vagy tároló óta módosított hello ETag értéket kapott.</span><span class="sxs-lookup"><span data-stu-id="356af-219">If hello value was modified, this indicates that another client or instance modified hello blob or container since you obtained hello ETag value.</span></span>

### <a name="lease"></a><span data-ttu-id="356af-220">Címbérlet</span><span class="sxs-lookup"><span data-stu-id="356af-220">Lease</span></span>
<span data-ttu-id="356af-221">Hello segítségével lekérheti egy új bérleti **acquireLease** metódus hello blob vagy tároló megadott kívánt tooobtain a címbérlet a megadásával.</span><span class="sxs-lookup"><span data-stu-id="356af-221">You can acquire a new lease by using hello **acquireLease** method, specifying hello blob or container that you wish tooobtain a lease on.</span></span> <span data-ttu-id="356af-222">Például a következő kód hello címbérletet a **myblob**.</span><span class="sxs-lookup"><span data-stu-id="356af-222">For example, hello following code acquires a lease on **myblob**.</span></span>

```nodejs
blobSvc.acquireLease('mycontainer', 'myblob', function(error, result, response){
  if(!error) {
    console.log('leaseId: ' + result.id);
  }
});
```

<span data-ttu-id="356af-223">A későbbi műveletek **myblob** biztosítania kell a hello `options.leaseId` paraméter.</span><span class="sxs-lookup"><span data-stu-id="356af-223">Subsequent operations on **myblob** must provide hello `options.leaseId` parameter.</span></span> <span data-ttu-id="356af-224">hello bérleti Azonosítóját adja vissza a rendszer `result.id` a **acquireLease**.</span><span class="sxs-lookup"><span data-stu-id="356af-224">hello lease ID is returned as `result.id` from **acquireLease**.</span></span>

> [!NOTE]
> <span data-ttu-id="356af-225">Alapértelmezés szerint hello címbérlet érték a végtelen.</span><span class="sxs-lookup"><span data-stu-id="356af-225">By default, hello lease duration is infinite.</span></span> <span data-ttu-id="356af-226">Megadhat egy nem végtelen időtartama (között 15 és 60 másodperc), adja meg a hello `options.leaseDuration` paraméter.</span><span class="sxs-lookup"><span data-stu-id="356af-226">You can specify a non-infinite duration (between 15 and 60 seconds) by providing hello `options.leaseDuration` parameter.</span></span>
>
>

<span data-ttu-id="356af-227">használja a címbérlet tooremove **releaseLease**.</span><span class="sxs-lookup"><span data-stu-id="356af-227">tooremove a lease, use **releaseLease**.</span></span> <span data-ttu-id="356af-228">toobreak a címbérlet, de megakadályozza mások beszerezni új bérletet hello eredeti időtartam lejártáig, használjon **breakLease**.</span><span class="sxs-lookup"><span data-stu-id="356af-228">toobreak a lease, but prevent others from obtaining a new lease until hello original duration has expired, use **breakLease**.</span></span>

## <a name="work-with-shared-access-signatures"></a><span data-ttu-id="356af-229">Megosztott hozzáférési aláírásokkal működik</span><span class="sxs-lookup"><span data-stu-id="356af-229">Work with shared access signatures</span></span>
<span data-ttu-id="356af-230">Közös hozzáférésű jogosultságkód (SAS) egy biztonságos módon tooprovide részletes hozzáférés tooblobs és a tárolók a tárfiók neve vagy a kulcsok megadása nélkül.</span><span class="sxs-lookup"><span data-stu-id="356af-230">Shared access signatures (SAS) are a secure way tooprovide granular access tooblobs and containers without providing your storage account name or keys.</span></span> <span data-ttu-id="356af-231">Közös hozzáférésű jogosultságkód gyakran használt tooprovide korlátozott hozzáférés tooyour adatok, például a mobilalkalmazások tooaccess blobok így.</span><span class="sxs-lookup"><span data-stu-id="356af-231">Shared access signatures are often used tooprovide limited access tooyour data, such as allowing a mobile app tooaccess blobs.</span></span>

> [!NOTE]
> <span data-ttu-id="356af-232">Közben is engedélyezheti a névtelen hozzáférés tooblobs, közös hozzáférésű jogosultságkód lehetővé teszik több ellenőrzött tooprovide hozzáférés, például hello SAS létre kell hoznia.</span><span class="sxs-lookup"><span data-stu-id="356af-232">While you can also allow anonymous access tooblobs, shared access signatures allow you tooprovide more controlled access, as you must generate hello SAS.</span></span>
>
>

<span data-ttu-id="356af-233">A megbízható alkalmazások, például egy felhőalapú szolgáltatás közös hozzáférésű jogosultságkód hello segítségével hoz létre **generateSharedAccessSignature** a hello **BlobService**, és a nem megbízható tooan vagy félig megbízható alkalmazásadatokat, például egy mobilalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="356af-233">A trusted application such as a cloud-based service generates shared access signatures using hello **generateSharedAccessSignature** of hello **BlobService**, and provides it tooan untrusted or semi-trusted application such as a mobile app.</span></span> <span data-ttu-id="356af-234">Közös hozzáférésű aláírások akkor jönnek létre a szabályzatot, amely ismerteti a hello start használatával és záró dátumát, mely hello során közös hozzáférésű jogosultságkód érvényesek, valamint hello hozzáférési toohello megosztott hozzáférési szint megadott aláírások tulajdonosával.</span><span class="sxs-lookup"><span data-stu-id="356af-234">Shared access signatures are generated using a policy, which describes hello start and end dates during which hello shared access signatures are valid, as well as hello access level granted toohello shared access signatures holder.</span></span>

<span data-ttu-id="356af-235">hello alábbi példakód létrehoz egy új megosztott elérési házirendet, amely lehetővé teszi, hogy hello megosztott hozzáférési aláírásokkal jogosult tooperform olvasási műveletek a hello **myblob** blob-, és 100 perc létrehozták hello idő múlva lejár.</span><span class="sxs-lookup"><span data-stu-id="356af-235">hello following code example generates a new shared access policy that allows hello shared access signatures holder tooperform read operations on hello **myblob** blob, and expires 100 minutes after hello time it is created.</span></span>

```nodejs
var startDate = new Date();
var expiryDate = new Date(startDate);
expiryDate.setMinutes(startDate.getMinutes() + 100);
startDate.setMinutes(startDate.getMinutes() - 100);

var sharedAccessPolicy = {
  AccessPolicy: {
    Permissions: azure.BlobUtilities.SharedAccessPermissions.READ,
    Start: startDate,
    Expiry: expiryDate
  },
};

var blobSAS = blobSvc.generateSharedAccessSignature('mycontainer', 'myblob', sharedAccessPolicy);
var host = blobSvc.host;
```

<span data-ttu-id="356af-236">Vegye figyelembe, hogy hello állomásadatai csak megadott is szükség rá, amikor hello megosztott hozzáférési aláírásokkal jogosult megpróbálja tooaccess hello tároló.</span><span class="sxs-lookup"><span data-stu-id="356af-236">Note that hello host information must be provided also, as it is required when hello shared access signatures holder attempts tooaccess hello container.</span></span>

<span data-ttu-id="356af-237">hello majd ügyfélalkalmazás megosztott hozzáférési aláírásokkal rendelkező **BlobServiceWithSAS** hello blob tooperform műveleteket.</span><span class="sxs-lookup"><span data-stu-id="356af-237">hello client application then uses shared access signatures with **BlobServiceWithSAS** tooperform operations against hello blob.</span></span> <span data-ttu-id="356af-238">hello következő lekérdezi, **myblob**.</span><span class="sxs-lookup"><span data-stu-id="356af-238">hello following gets information about **myblob**.</span></span>

```nodejs
var sharedBlobSvc = azure.createBlobServiceWithSas(host, blobSAS);
sharedBlobSvc.getBlobProperties('mycontainer', 'myblob', function (error, result, response) {
  if(!error) {
    // retrieved info
  }
});
```

<span data-ttu-id="356af-239">Hello megosztott hozzáférési aláírásokkal csak olvasási hozzáféréssel rendelkező jött létre, ha toomodify hello blob tett kísérlet, mert egy hibaüzenetet küld.</span><span class="sxs-lookup"><span data-stu-id="356af-239">Since hello shared access signatures were generated with read-only access, if an attempt is made toomodify hello blob, an error will be returned.</span></span>

### <a name="access-control-lists"></a><span data-ttu-id="356af-240">Hozzáférés-vezérlési listák</span><span class="sxs-lookup"><span data-stu-id="356af-240">Access control lists</span></span>
<span data-ttu-id="356af-241">Hozzáférés-vezérlési lista (ACL) tooset hello hozzáférési szabályzatok az SAS is használhatja.</span><span class="sxs-lookup"><span data-stu-id="356af-241">You can also use an access control list (ACL) tooset hello access policy for SAS.</span></span> <span data-ttu-id="356af-242">Ez akkor hasznos, ha akarja tooallow több ügyfelek tooaccess tárolója, de adjon meg másik hozzáférési házirendeket az egyes ügyfelekhez.</span><span class="sxs-lookup"><span data-stu-id="356af-242">This is useful if you wish tooallow multiple clients tooaccess a container but provide different access policies for each client.</span></span>

<span data-ttu-id="356af-243">Hozzáférés-vezérlési Listában hozzáférési házirendeket, tömbje segítségével minden házirendhez társított azonosítójú van megvalósítva.</span><span class="sxs-lookup"><span data-stu-id="356af-243">An ACL is implemented using an array of access policies, with an ID associated with each policy.</span></span> <span data-ttu-id="356af-244">a következő példakód hello meghatározza, hogy két házirend, egy "felhasználó1", és egy "felhasználó2":</span><span class="sxs-lookup"><span data-stu-id="356af-244">hello following code example defines two policies, one for 'user1' and one for 'user2':</span></span>

```nodejs
var sharedAccessPolicy = {
  user1: {
    Permissions: azure.BlobUtilities.SharedAccessPermissions.READ,
    Start: startDate,
    Expiry: expiryDate
  },
  user2: {
    Permissions: azure.BlobUtilities.SharedAccessPermissions.WRITE,
    Start: startDate,
    Expiry: expiryDate
  }
};
```

<span data-ttu-id="356af-245">a következő kód példa lekérdezi hello hello aktuális hozzáférés-Vezérlési **mycontainer**, és hozzáadja az új házirendeket hello segítségével **setBlobAcl**.</span><span class="sxs-lookup"><span data-stu-id="356af-245">hello following code example gets hello current ACL for **mycontainer**, and then adds hello new policies using **setBlobAcl**.</span></span> <span data-ttu-id="356af-246">Ez a megközelítés lehetővé teszi, hogy:</span><span class="sxs-lookup"><span data-stu-id="356af-246">This approach allows:</span></span>

```nodejs
var extend = require('extend');
blobSvc.getBlobAcl('mycontainer', function(error, result, response) {
  if(!error){
    var newSignedIdentifiers = extend(true, result.signedIdentifiers, sharedAccessPolicy);
    blobSvc.setBlobAcl('mycontainer', newSignedIdentifiers, function(error, result, response){
      if(!error){
        // ACL set
      }
    });
  }
});
```

<span data-ttu-id="356af-247">Egyszer hello hozzáférés-vezérlési lista van beállítva, majd létrehozhat megosztott hozzáférési aláírásokkal hello azonosító egy házirend alapján.</span><span class="sxs-lookup"><span data-stu-id="356af-247">Once hello ACL is set, you can then create shared access signatures based on hello ID for a policy.</span></span> <span data-ttu-id="356af-248">a következő példakód hello "felhasználó2" az új közös hozzáférésű jogosultságkód hoz létre:</span><span class="sxs-lookup"><span data-stu-id="356af-248">hello following code example creates new shared access signatures for 'user2':</span></span>

```nodejs
blobSAS = blobSvc.generateSharedAccessSignature('mycontainer', { Id: 'user2' });
```

## <a name="next-steps"></a><span data-ttu-id="356af-249">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="356af-249">Next steps</span></span>
<span data-ttu-id="356af-250">További információkért tekintse meg a következő erőforrások hello.</span><span class="sxs-lookup"><span data-stu-id="356af-250">For more information, see hello following resources.</span></span>

* <span data-ttu-id="356af-251">[Az Azure Storage SDK csomópont API-referencia][Azure Storage SDK for Node API Reference]</span><span class="sxs-lookup"><span data-stu-id="356af-251">[Azure Storage SDK for Node API Reference][Azure Storage SDK for Node API Reference]</span></span>
* <span data-ttu-id="356af-252">[Az Azure Storage csapat blogja][Azure Storage Team Blog]</span><span class="sxs-lookup"><span data-stu-id="356af-252">[Azure Storage Team Blog][Azure Storage Team Blog]</span></span>
* <span data-ttu-id="356af-253">[Az Azure Storage szolgáltatás SDK csomópont] [ Azure Storage SDK for Node] GitHub tárházából</span><span class="sxs-lookup"><span data-stu-id="356af-253">[Azure Storage SDK for Node][Azure Storage SDK for Node] repository on GitHub</span></span>
* [<span data-ttu-id="356af-254">Node.js fejlesztői központ</span><span class="sxs-lookup"><span data-stu-id="356af-254">Node.js Developer Center</span></span>](https://azure.microsoft.com/develop/nodejs/)
* [<span data-ttu-id="356af-255">Adatátvitel az AzCopy parancssori segédprogram hello</span><span class="sxs-lookup"><span data-stu-id="356af-255">Transfer data with hello AzCopy command-line utility</span></span>](storage-use-azcopy.md)

[Azure Storage SDK for Node]: https://github.com/Azure/azure-storage-node

[Node.js-webalkalmazás létrehozása az Azure App Service]: ../app-service-web/app-service-web-get-started-nodejs.md
[Node.js web app használatával hello Azure Table szolgáltatás]: ../app-service-web/storage-nodejs-use-table-storage-web-site.md
[létrehozása és központi telepítése a Node.js web app tooAzure Web Matrix]: ../app-service-web/web-sites-nodejs-use-webmatrix.md
[Using hello REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx
[Azure portal]: https://portal.azure.com
[és üzembe egy Node.js-alkalmazás tooan Azure Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
[Azure Storage SDK for Node API Reference]: http://dl.windowsazure.com/nodestoragedocs/index.html
