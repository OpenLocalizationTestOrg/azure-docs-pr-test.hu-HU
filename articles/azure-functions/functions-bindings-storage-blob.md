---
title: "aaaAzure funkciók Blob Storage kötések |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Azure Storage eseményindítók és kötések az Azure Functions."
services: functions
documentationcenter: na
author: lindydonna
manager: erikre
editor: 
tags: 
keywords: "Azure functions, Funkciók, Eseményfeldolgozási, dinamikus számítási kiszolgáló nélküli architektúrája"
ms.assetid: aba8976c-6568-4ec7-86f5-410efd6b0fb9
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/25/2017
ms.author: glenga
ms.openlocfilehash: cef44bd2154d0b97cca9220b6c5024a5b620c80d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-blob-storage-bindings"></a><span data-ttu-id="42d7f-104">Az Azure Functions Blob tároló kötések</span><span class="sxs-lookup"><span data-stu-id="42d7f-104">Azure Functions Blob storage bindings</span></span>
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

<span data-ttu-id="42d7f-105">Ez a cikk azt ismerteti, hogyan tooconfigure és dolgozhat Azure Blob storage kötések Azure Functions a.</span><span class="sxs-lookup"><span data-stu-id="42d7f-105">This article explains how tooconfigure and work with Azure Blob storage bindings in Azure Functions.</span></span> <span data-ttu-id="42d7f-106">Az Azure Functions támogatja eseményindító, adjon meg, és kimeneti Azure Blob Storage tárolóban kötéseit.</span><span class="sxs-lookup"><span data-stu-id="42d7f-106">Azure Functions supports trigger, input, and output bindings for Azure Blob storage.</span></span> <span data-ttu-id="42d7f-107">Elérhető összes kötését lásd [Azure Functions eseményindítók és kötések fogalmak](functions-triggers-bindings.md).</span><span class="sxs-lookup"><span data-stu-id="42d7f-107">For features that are available in all bindings, see [Azure Functions triggers and bindings concepts](functions-triggers-bindings.md).</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

> [!NOTE]
> <span data-ttu-id="42d7f-108">A [blob csak a storage-fiók](../storage/common/storage-create-storage-account.md#blob-storage-accounts) nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="42d7f-108">A [blob only storage account](../storage/common/storage-create-storage-account.md#blob-storage-accounts) is not supported.</span></span> <span data-ttu-id="42d7f-109">A BLOB storage eseményindítók és kötések általános célú tárfiók szükséges.</span><span class="sxs-lookup"><span data-stu-id="42d7f-109">Blob storage triggers and bindings require a general-purpose storage account.</span></span> 
> 

<a name="trigger"></a>
<a name="storage-blob-trigger"></a>
## <a name="blob-storage-triggers-and-bindings"></a><span data-ttu-id="42d7f-110">A BLOB storage eseményindítók és kötések</span><span class="sxs-lookup"><span data-stu-id="42d7f-110">Blob storage triggers and bindings</span></span>

<span data-ttu-id="42d7f-111">Hello Azure Blob storage eseményindítót használ, a függvény kódot nevezik egy új vagy frissített blob észlelése esetén.</span><span class="sxs-lookup"><span data-stu-id="42d7f-111">Using hello Azure Blob storage trigger, your function code is called when a new or updated blob is detected.</span></span> <span data-ttu-id="42d7f-112">hello blob tartalmát vannak megadva, a bemeneti toohello függvény.</span><span class="sxs-lookup"><span data-stu-id="42d7f-112">hello blob contents are provided as input toohello function.</span></span>

<span data-ttu-id="42d7f-113">Adja meg a blob storage eseményindító hello segítségével **integráció** hello Functions portálra a lap.</span><span class="sxs-lookup"><span data-stu-id="42d7f-113">Define a blob storage trigger using hello **Integrate** tab in hello Functions portal.</span></span> <span data-ttu-id="42d7f-114">hello portál létrehoz egyet a következő hello definíciójában hello **kötések** szakasza *function.json*:</span><span class="sxs-lookup"><span data-stu-id="42d7f-114">hello portal creates hello following definition in hello  **bindings** section of *function.json*:</span></span>

```json
{
    "name": "<hello name used tooidentify hello trigger data in your code>",
    "type": "blobTrigger",
    "direction": "in",
    "path": "<container toomonitor, and optionally a blob name pattern - see below>",
    "connection": "<Name of app setting - see below>"
}
```

<span data-ttu-id="42d7f-115">A BLOB bemeneti és kimeneti kötések meghatározása `blob` hello kötés típusa:</span><span class="sxs-lookup"><span data-stu-id="42d7f-115">Blob input and output bindings are defined using `blob` as hello binding type:</span></span>

```json
{
  "name": "<hello name used tooidentify hello blob input in your code>",
  "type": "blob",
  "direction": "in", // other supported directions are "inout" and "out"
  "path": "<Path of input blob - see below>",
  "connection":"<Name of app setting - see below>"
},
```

* <span data-ttu-id="42d7f-116">Hello `path` tulajdonság által támogatott kötelező kifejezések és a szűrő paramétereit.</span><span class="sxs-lookup"><span data-stu-id="42d7f-116">hello `path` property supports binding expressions and filter parameters.</span></span> <span data-ttu-id="42d7f-117">Lásd: [minták neve](#pattern).</span><span class="sxs-lookup"><span data-stu-id="42d7f-117">See [Name patterns](#pattern).</span></span>
* <span data-ttu-id="42d7f-118">Hello `connection` tulajdonságának tartalmaznia kell egy tárolási kapcsolati karakterlánc tartalmazó Alkalmazásbeállítás hello nevét.</span><span class="sxs-lookup"><span data-stu-id="42d7f-118">hello `connection` property must contain hello name of an app setting that contains a storage connection string.</span></span> <span data-ttu-id="42d7f-119">Hello Azure-portálon, a hello hello a szokásos szerkesztő **integráció** lapon konfigurálja az Alkalmazásbeállítás meg, amikor kiválaszt egy tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="42d7f-119">In hello Azure portal, hello standard editor in hello **Integrate** tab configures this app setting for you when you select a storage account.</span></span>

> [!NOTE]
> <span data-ttu-id="42d7f-120">Egy blob eseményindító használatakor a fogyasztás terven lehet tooa 10 perces késleltetést mentése új blobok feldolgozása után egy függvény app inaktív állapotba került.</span><span class="sxs-lookup"><span data-stu-id="42d7f-120">When you're using a blob trigger on a Consumption plan, there can be up tooa 10-minute delay in processing new blobs after a function app has gone idle.</span></span> <span data-ttu-id="42d7f-121">Hello függvény alkalmazás futtatása után blobok feldolgozása azonnal megtörténik.</span><span class="sxs-lookup"><span data-stu-id="42d7f-121">After hello function app is running, blobs are processed immediately.</span></span> <span data-ttu-id="42d7f-122">a kezdeti tooavoid késleltetés, javasoljuk, hogy hello alábbi beállítások egyikét:</span><span class="sxs-lookup"><span data-stu-id="42d7f-122">tooavoid this initial delay, consider one of hello following options:</span></span>
> - <span data-ttu-id="42d7f-123">Használja az App Service-csomag a mindig engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="42d7f-123">Use an App Service plan with Always On enabled.</span></span>
> - <span data-ttu-id="42d7f-124">Használjon egy másik mechanizmust tootrigger hello blob feldolgozására, például egy üzenetsor-üzenetet, amely hello blob nevét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="42d7f-124">Use another mechanism tootrigger hello blob processing, such as a queue message that contains hello blob name.</span></span> <span data-ttu-id="42d7f-125">Egy vonatkozó példáért lásd: [várólista eseményindító blob bemeneti kötése](#input-sample).</span><span class="sxs-lookup"><span data-stu-id="42d7f-125">For an example, see [Queue trigger with blob input binding](#input-sample).</span></span>

<a name="pattern"></a>

### <a name="name-patterns"></a><span data-ttu-id="42d7f-126">Név minták</span><span class="sxs-lookup"><span data-stu-id="42d7f-126">Name patterns</span></span>
<span data-ttu-id="42d7f-127">Megadhat egy blob mintát hello `path` tulajdonság, amely lehet egy szűrőt, vagy kötési kifejezés.</span><span class="sxs-lookup"><span data-stu-id="42d7f-127">You can specify a blob name pattern in hello `path` property, which can be a filter or binding expression.</span></span> <span data-ttu-id="42d7f-128">Lásd: [kötelező kifejezések és minták](functions-triggers-bindings.md#binding-expressions-and-patterns).</span><span class="sxs-lookup"><span data-stu-id="42d7f-128">See [Binding expressions and patterns](functions-triggers-bindings.md#binding-expressions-and-patterns).</span></span>

<span data-ttu-id="42d7f-129">Például a toofilter tooblobs "eredeti" hello karakterlánccal kezdődő definícióját a következő hello használja.</span><span class="sxs-lookup"><span data-stu-id="42d7f-129">For example, toofilter tooblobs that start with hello string "original," use hello following definition.</span></span> <span data-ttu-id="42d7f-130">Az elérési út keresése nevű blob *eredeti-Blob1.txt* a hello *bemeneti* tároló és a hello hello értékének `name` függvény a kódban a változó `Blob1`.</span><span class="sxs-lookup"><span data-stu-id="42d7f-130">This path finds a blob named *original-Blob1.txt* in hello *input* container, and hello value of hello `name` variable in function code is `Blob1`.</span></span>

```json
"path": "input/original-{name}",
```

<span data-ttu-id="42d7f-131">toobind toohello blob nevét és kiterjesztését külön-külön, használja a két mintát.</span><span class="sxs-lookup"><span data-stu-id="42d7f-131">toobind toohello blob file name and extension separately, use two patterns.</span></span> <span data-ttu-id="42d7f-132">Ezt az elérési utat is talál nevű blob *eredeti-Blob1.txt*, és hello hello értékének `blobname` és `blobextension` változók a funkciókódot *eredeti-Blob1* és *txt*.</span><span class="sxs-lookup"><span data-stu-id="42d7f-132">This path also finds a blob named *original-Blob1.txt*, and hello value of hello `blobname` and `blobextension` variables in function code are *original-Blob1* and *txt*.</span></span>

```json
"path": "input/{blobname}.{blobextension}",
```

<span data-ttu-id="42d7f-133">Korlátozhatja, hogy a BLOB típusú hello fájl hello fájlnévkiterjesztéshez rögzített érték használatával.</span><span class="sxs-lookup"><span data-stu-id="42d7f-133">You can restrict hello file type of blobs by using a fixed value for hello file extension.</span></span> <span data-ttu-id="42d7f-134">Például tootrigger csak a .png fájlok, a következő mintát használja hello:</span><span class="sxs-lookup"><span data-stu-id="42d7f-134">For instance, tootrigger only on .png files, use hello following pattern:</span></span>

```json
"path": "samples/{name}.png",
```

<span data-ttu-id="42d7f-135">Kapcsos zárójelek speciális karakterek a mintában.</span><span class="sxs-lookup"><span data-stu-id="42d7f-135">Curly braces are special characters in name patterns.</span></span> <span data-ttu-id="42d7f-136">toospecify blob neve kapcsos zárójelek hello nevében, is escape hello kapcsos zárójeleket két kell használni.</span><span class="sxs-lookup"><span data-stu-id="42d7f-136">toospecify blob names that have curly braces in hello name, you can escape hello braces using two braces.</span></span> <span data-ttu-id="42d7f-137">hello következő példát talál nevű blob *{20140101}-soundfile.mp3* a hello *képek* tároló és a hello `name` változó hello funkciókódot érték  *soundfile.mp3*.</span><span class="sxs-lookup"><span data-stu-id="42d7f-137">hello following example finds a blob named *{20140101}-soundfile.mp3* in hello *images* container, and hello `name` variable value in hello function code is *soundfile.mp3*.</span></span> 

```json
"path": "images/{{20140101}}-{name}",
```

### <a name="trigger-metadata"></a><span data-ttu-id="42d7f-138">Eseményindító metaadatok</span><span class="sxs-lookup"><span data-stu-id="42d7f-138">Trigger metadata</span></span>

<span data-ttu-id="42d7f-139">hello blob eseményindító biztosít több metaadat-tulajdonságot.</span><span class="sxs-lookup"><span data-stu-id="42d7f-139">hello blob trigger provides several metadata properties.</span></span> <span data-ttu-id="42d7f-140">Ezeket a tulajdonságokat meg más kötésekben kötések kifejezések részeként vagy a kód paramétereiben használható.</span><span class="sxs-lookup"><span data-stu-id="42d7f-140">These properties can be used as part of bindings expressions in other bindings or as parameters in your code.</span></span> <span data-ttu-id="42d7f-141">Ezekkel az értékekkel rendelkezik hello azonos szemantikákkal, [CloudBlob](https://docs.microsoft.com/en-us/dotnet/api/microsoft.windowsazure.storage.blob.cloudblob?view=azure-dotnet).</span><span class="sxs-lookup"><span data-stu-id="42d7f-141">These values have hello same semantics as [CloudBlob](https://docs.microsoft.com/en-us/dotnet/api/microsoft.windowsazure.storage.blob.cloudblob?view=azure-dotnet).</span></span>

- <span data-ttu-id="42d7f-142">**BlobTrigger**.</span><span class="sxs-lookup"><span data-stu-id="42d7f-142">**BlobTrigger**.</span></span> <span data-ttu-id="42d7f-143">Gépelje be: `string`.</span><span class="sxs-lookup"><span data-stu-id="42d7f-143">Type `string`.</span></span> <span data-ttu-id="42d7f-144">hello eseményindító blob elérési út</span><span class="sxs-lookup"><span data-stu-id="42d7f-144">hello triggering blob path</span></span>
- <span data-ttu-id="42d7f-145">**URI**.</span><span class="sxs-lookup"><span data-stu-id="42d7f-145">**Uri**.</span></span> <span data-ttu-id="42d7f-146">Gépelje be: `System.Uri`.</span><span class="sxs-lookup"><span data-stu-id="42d7f-146">Type `System.Uri`.</span></span> <span data-ttu-id="42d7f-147">elsődleges hely hello hello blob URI-Azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="42d7f-147">hello blob's URI for hello primary location.</span></span>
- <span data-ttu-id="42d7f-148">**Tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="42d7f-148">**Properties**.</span></span> <span data-ttu-id="42d7f-149">Gépelje be: `Microsoft.WindowsAzure.Storage.Blob.BlobProperties`.</span><span class="sxs-lookup"><span data-stu-id="42d7f-149">Type `Microsoft.WindowsAzure.Storage.Blob.BlobProperties`.</span></span> <span data-ttu-id="42d7f-150">hello blob tulajdonságai.</span><span class="sxs-lookup"><span data-stu-id="42d7f-150">hello blob's system properties.</span></span>
- <span data-ttu-id="42d7f-151">**Metaadatok**.</span><span class="sxs-lookup"><span data-stu-id="42d7f-151">**Metadata**.</span></span> <span data-ttu-id="42d7f-152">Gépelje be: `IDictionary<string,string>`.</span><span class="sxs-lookup"><span data-stu-id="42d7f-152">Type `IDictionary<string,string>`.</span></span> <span data-ttu-id="42d7f-153">hello felhasználó által definiált metaadatok hello BLOB.</span><span class="sxs-lookup"><span data-stu-id="42d7f-153">hello user-defined metadata for hello blob.</span></span>

<a name="receipts"></a>

### <a name="blob-receipts"></a><span data-ttu-id="42d7f-154">A BLOB visszaigazolások</span><span class="sxs-lookup"><span data-stu-id="42d7f-154">Blob receipts</span></span>
<span data-ttu-id="42d7f-155">hello Azure Functions futásidejű biztosítja, hogy nincs blob funkció egynél többször meghívása megtörténik a hello azonos új vagy frissített blob.</span><span class="sxs-lookup"><span data-stu-id="42d7f-155">hello Azure Functions runtime ensures that no blob trigger function gets called more than once for hello same new or updated blob.</span></span> <span data-ttu-id="42d7f-156">Ha egy adott blobverzió feldolgozása toodetermine, tart fenn *blob-visszaigazolások*.</span><span class="sxs-lookup"><span data-stu-id="42d7f-156">toodetermine if a given blob version has been processed, it maintains *blob receipts*.</span></span>

<span data-ttu-id="42d7f-157">Az Azure Functions tárolók blob-visszaigazolások nevű tároló *azure-webjobs-állomások* az Azure storage-fiók alkalmazás függvény hello (hello Alkalmazásbeállítás által meghatározott `AzureWebJobsStorage`).</span><span class="sxs-lookup"><span data-stu-id="42d7f-157">Azure Functions stores blob receipts in a container named *azure-webjobs-hosts* in hello Azure storage account for your function app (defined by hello app setting `AzureWebJobsStorage`).</span></span> <span data-ttu-id="42d7f-158">Egy blob fogadását a következő információk hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="42d7f-158">A blob receipt has hello following information:</span></span>

* <span data-ttu-id="42d7f-159">hello indított függvény ("*&lt;függvény alkalmazás neve >*. Működik.  *&lt;függvény neve >*", például:"MyFunctionApp.Functions.CopyBlob")</span><span class="sxs-lookup"><span data-stu-id="42d7f-159">hello triggered function ("*&lt;function app name>*.Functions.*&lt;function name>*", for example: "MyFunctionApp.Functions.CopyBlob")</span></span>
* <span data-ttu-id="42d7f-160">hello tároló neve</span><span class="sxs-lookup"><span data-stu-id="42d7f-160">hello container name</span></span>
* <span data-ttu-id="42d7f-161">hello blob típushoz ("BlockBlob" vagy "PageBlob")</span><span class="sxs-lookup"><span data-stu-id="42d7f-161">hello blob type ("BlockBlob" or "PageBlob")</span></span>
* <span data-ttu-id="42d7f-162">hello blob neve</span><span class="sxs-lookup"><span data-stu-id="42d7f-162">hello blob name</span></span>
* <span data-ttu-id="42d7f-163">hello ETag (például egy blob verziójának azonosítója: "0x8D1DC6E70A277EF")</span><span class="sxs-lookup"><span data-stu-id="42d7f-163">hello ETag (a blob version identifier, for example: "0x8D1DC6E70A277EF")</span></span>

<span data-ttu-id="42d7f-164">egy blob újrafeldolgozása tooforce hello blob fogadását, hogy a BLOB törlése hello *azure-webjobs-állomások* tároló manuálisan.</span><span class="sxs-lookup"><span data-stu-id="42d7f-164">tooforce reprocessing of a blob, delete hello blob receipt for that blob from hello *azure-webjobs-hosts* container manually.</span></span>

<a name="poison"></a>

### <a name="handling-poison-blobs"></a><span data-ttu-id="42d7f-165">Az elhalt blobok kezelése</span><span class="sxs-lookup"><span data-stu-id="42d7f-165">Handling poison blobs</span></span>
<span data-ttu-id="42d7f-166">Ha egy adott BLOB egy blob funkció nem sikerül, az Azure Functions újrapróbálkozik, amelyek összesen 5 alkalommal alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="42d7f-166">When a blob trigger function fails for a given blob, Azure Functions retries that function a total of 5 times by default.</span></span> 

<span data-ttu-id="42d7f-167">Minden 5 próbálkozás sikertelen lesz, ha az Azure Functions hozzáadja egy tooa tárolási üzenetsor nevű *webjobs-blobtrigger-poison*.</span><span class="sxs-lookup"><span data-stu-id="42d7f-167">If all 5 tries fail, Azure Functions adds a message tooa Storage queue named *webjobs-blobtrigger-poison*.</span></span> <span data-ttu-id="42d7f-168">az elhalt blobok várólista üdvözlőüzenetére egy JSON-objektum, amely tartalmazza a következő tulajdonságai hello:</span><span class="sxs-lookup"><span data-stu-id="42d7f-168">hello queue message for poison blobs is a JSON object that contains hello following properties:</span></span>

* <span data-ttu-id="42d7f-169">FunctionId (hello formátumban  *&lt;függvény alkalmazás neve >*. Működik.  *&lt;függvény neve >*)</span><span class="sxs-lookup"><span data-stu-id="42d7f-169">FunctionId (in hello format *&lt;function app name>*.Functions.*&lt;function name>*)</span></span>
* <span data-ttu-id="42d7f-170">BlobType ("BlockBlob" vagy "PageBlob")</span><span class="sxs-lookup"><span data-stu-id="42d7f-170">BlobType ("BlockBlob" or "PageBlob")</span></span>
* <span data-ttu-id="42d7f-171">ContainerName</span><span class="sxs-lookup"><span data-stu-id="42d7f-171">ContainerName</span></span>
* <span data-ttu-id="42d7f-172">Blobnév</span><span class="sxs-lookup"><span data-stu-id="42d7f-172">BlobName</span></span>
* <span data-ttu-id="42d7f-173">ETag (például egy blob verziójának azonosítója: "0x8D1DC6E70A277EF")</span><span class="sxs-lookup"><span data-stu-id="42d7f-173">ETag (a blob version identifier, for example: "0x8D1DC6E70A277EF")</span></span>

### <a name="blob-polling-for-large-containers"></a><span data-ttu-id="42d7f-174">Ciklikus lekérdezés a nagyméretű tárolók BLOB</span><span class="sxs-lookup"><span data-stu-id="42d7f-174">Blob polling for large containers</span></span>
<span data-ttu-id="42d7f-175">Ha a figyelt hello blobtárolóban több mint 10 000 blobot tartalmaz, hello funkciók futásidejű vizsgálatok napló fájlok toowatch új vagy módosított blobot.</span><span class="sxs-lookup"><span data-stu-id="42d7f-175">If hello blob container being monitored contains more than 10,000 blobs, hello Functions runtime scans log files toowatch for new or changed blobs.</span></span> <span data-ttu-id="42d7f-176">Ez a folyamat nincs valós időben.</span><span class="sxs-lookup"><span data-stu-id="42d7f-176">This process is not real time.</span></span> <span data-ttu-id="42d7f-177">Egy olyan függvényt előfordulhat, hogy nem beolvasása indulnak el, amíg több percig vagy már hello blob létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="42d7f-177">A function might not get triggered until several minutes or longer after hello blob is created.</span></span> <span data-ttu-id="42d7f-178">Emellett [tárolási naplófájlokat hoz létre a lehető legjobb rendezését,"a](/rest/api/storageservices/About-Storage-Analytics-Logging) alapján.</span><span class="sxs-lookup"><span data-stu-id="42d7f-178">In addition, [storage logs are created on a "best effort"](/rest/api/storageservices/About-Storage-Analytics-Logging) basis.</span></span> <span data-ttu-id="42d7f-179">Nincs nem garantálja, hogy a rendszer rögzíti-e az összes esemény.</span><span class="sxs-lookup"><span data-stu-id="42d7f-179">There is no guarantee that all events are captured.</span></span> <span data-ttu-id="42d7f-180">Bizonyos körülmények között a naplók kimaradhatnak.</span><span class="sxs-lookup"><span data-stu-id="42d7f-180">Under some conditions, logs may be missed.</span></span> <span data-ttu-id="42d7f-181">Ha a gyorsabb és megbízhatóbb blob feldolgozási van szüksége, érdemes létrehozni egy [üzenetsor](../storage/queues/storage-dotnet-how-to-use-queues.md) hello blob létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="42d7f-181">If you require faster or more reliable blob processing, consider creating a [queue message](../storage/queues/storage-dotnet-how-to-use-queues.md) when you create hello blob.</span></span> <span data-ttu-id="42d7f-182">Ezt követően egy [várólista eseményindító](functions-bindings-storage-queue.md) blob eseményindító tooprocess hello blob helyett.</span><span class="sxs-lookup"><span data-stu-id="42d7f-182">Then, use a [queue trigger](functions-bindings-storage-queue.md) instead of a blob trigger tooprocess hello blob.</span></span>

<a name="triggerusage"></a>

## <a name="using-a-blob-trigger-and-input-binding"></a><span data-ttu-id="42d7f-183">Egy blob eseményindítót használ, és a bemeneti kötése</span><span class="sxs-lookup"><span data-stu-id="42d7f-183">Using a blob trigger and input binding</span></span>
<span data-ttu-id="42d7f-184">.NET-es függvényeket, férnek hozzá, mint egy metódus paraméter használatával hello Blobadatok `Stream paramName`.</span><span class="sxs-lookup"><span data-stu-id="42d7f-184">In .NET functions, access hello blob data using a method parameter such as `Stream paramName`.</span></span> <span data-ttu-id="42d7f-185">Itt `paramName` hello megadott hello érték [eseményindító konfigurációs](#trigger).</span><span class="sxs-lookup"><span data-stu-id="42d7f-185">Here, `paramName` is hello value you specified in hello [trigger configuration](#trigger).</span></span> <span data-ttu-id="42d7f-186">A Node.js funkciók hozzáférés hello bemeneti blob használatával végzett `context.bindings.<name>`.</span><span class="sxs-lookup"><span data-stu-id="42d7f-186">In Node.js functions, access hello input blob data using `context.bindings.<name>`.</span></span>

<span data-ttu-id="42d7f-187">A .NET az alábbi listában hello kötni az tooany hello típusú.</span><span class="sxs-lookup"><span data-stu-id="42d7f-187">In .NET, you can bind tooany of hello types in hello list below.</span></span> <span data-ttu-id="42d7f-188">Ha egy bemeneti kötése, néhány, a következő típusú szükséges egy `inout` irányban kötés *function.json*.</span><span class="sxs-lookup"><span data-stu-id="42d7f-188">If used as an input binding, some of these types require an `inout` binding direction in *function.json*.</span></span> <span data-ttu-id="42d7f-189">Ebben az irányban nem támogatja hello szokásos szerkesztő, így speciális szerkesztő hello kell használni.</span><span class="sxs-lookup"><span data-stu-id="42d7f-189">This direction is not supported by hello standard editor, so you must use hello advanced editor.</span></span>

* `TextReader`
* `Stream`
* <span data-ttu-id="42d7f-190">`ICloudBlob`(a "inout" kötés irányát igényel)</span><span class="sxs-lookup"><span data-stu-id="42d7f-190">`ICloudBlob` (requires "inout" binding direction)</span></span>
* <span data-ttu-id="42d7f-191">`CloudBlockBlob`(a "inout" kötés irányát igényel)</span><span class="sxs-lookup"><span data-stu-id="42d7f-191">`CloudBlockBlob` (requires "inout" binding direction)</span></span>
* <span data-ttu-id="42d7f-192">`CloudPageBlob`(a "inout" kötés irányát igényel)</span><span class="sxs-lookup"><span data-stu-id="42d7f-192">`CloudPageBlob` (requires "inout" binding direction)</span></span>
* <span data-ttu-id="42d7f-193">`CloudAppendBlob`(a "inout" kötés irányát igényel)</span><span class="sxs-lookup"><span data-stu-id="42d7f-193">`CloudAppendBlob` (requires "inout" binding direction)</span></span>

<span data-ttu-id="42d7f-194">Szöveges BLOB várható, ha is köthető tooa .NET `string` típusa.</span><span class="sxs-lookup"><span data-stu-id="42d7f-194">If text blobs are expected, you can also bind tooa .NET `string` type.</span></span> <span data-ttu-id="42d7f-195">Ez csak akkor javasolt, ha hello blob mérete kisebb, mint hello teljes blob tartalmát a memóriába betöltött.</span><span class="sxs-lookup"><span data-stu-id="42d7f-195">This is only recommended if hello blob size is small, as hello entire blob contents are loaded into memory.</span></span> <span data-ttu-id="42d7f-196">Általában akkor érdemes toouse egy `Stream` vagy `CloudBlockBlob` típusa.</span><span class="sxs-lookup"><span data-stu-id="42d7f-196">Generally, it is preferable toouse a `Stream` or `CloudBlockBlob` type.</span></span>

## <a name="trigger-sample"></a><span data-ttu-id="42d7f-197">Eseményindító minta</span><span class="sxs-lookup"><span data-stu-id="42d7f-197">Trigger sample</span></span>
<span data-ttu-id="42d7f-198">Tegyük fel, amely meghatározza egy blob storage eseményindító function.json következő hello:</span><span class="sxs-lookup"><span data-stu-id="42d7f-198">Suppose you have hello following function.json that defines a blob storage trigger:</span></span>

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myBlob",
            "type": "blobTrigger",
            "direction": "in",
            "path": "samples-workitems",
            "connection":"MyStorageAccount"
        }
    ]
}
```

<span data-ttu-id="42d7f-199">Lásd: hello nyelvspecifikus mintát, amely minden egyes blob toohello figyelt tároló hozzáadott hello tartalmát naplózza.</span><span class="sxs-lookup"><span data-stu-id="42d7f-199">See hello language-specific sample that logs hello contents of each blob that is added toohello monitored container.</span></span>

* [<span data-ttu-id="42d7f-200">C#</span><span class="sxs-lookup"><span data-stu-id="42d7f-200">C#</span></span>](#triggercsharp)
* [<span data-ttu-id="42d7f-201">Node.js</span><span class="sxs-lookup"><span data-stu-id="42d7f-201">Node.js</span></span>](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="blob-trigger-examples-in-c"></a><span data-ttu-id="42d7f-202">A BLOB eseményindító példákban a C#</span><span class="sxs-lookup"><span data-stu-id="42d7f-202">Blob trigger examples in C#</span></span> #

```cs
// Blob trigger sample using a Stream binding
public static void Run(Stream myBlob, TraceWriter log)
{
   log.Info($"C# Blob trigger function Processed blob\n Name:{name} \n Size: {myBlob.Length} Bytes");
}
```

```cs
// Blob trigger binding tooa CloudBlockBlob
#r "Microsoft.WindowsAzure.Storage"

using Microsoft.WindowsAzure.Storage.Blob;

public static void Run(CloudBlockBlob myBlob, string name, TraceWriter log)
{
    log.Info($"C# Blob trigger function Processed blob\n Name:{name}\nURI:{myBlob.StorageUri}");
}
```

<a name="triggernodejs"></a>

### <a name="trigger-example-in-nodejs"></a><span data-ttu-id="42d7f-203">A node.js eseményindító – példa</span><span class="sxs-lookup"><span data-stu-id="42d7f-203">Trigger example in Node.js</span></span>

```javascript
module.exports = function(context) {
    context.log('Node.js Blob trigger function processed', context.bindings.myBlob);
    context.done();
};
```
<a name="outputusage"></a>
<a name="storage-blob-output-binding"></a>

## <a name="using-a-blob-output-binding"></a><span data-ttu-id="42d7f-204">Egy blob használatával kimeneti kötése</span><span class="sxs-lookup"><span data-stu-id="42d7f-204">Using a blob output binding</span></span>

<span data-ttu-id="42d7f-205">A .NET-es függvényeket, akkor vagy használja a `out string` a függvényaláíráshoz vagy használja a következő lista hello hello típusú paraméter.</span><span class="sxs-lookup"><span data-stu-id="42d7f-205">In .NET functions, you should either use a `out string` parameter in your function signature or use one of hello types in hello following list.</span></span> <span data-ttu-id="42d7f-206">A Node.js funkciókat érheti el hello kimeneti blob használatával `context.bindings.<name>`.</span><span class="sxs-lookup"><span data-stu-id="42d7f-206">In Node.js functions, you access hello output blob using `context.bindings.<name>`.</span></span>

<span data-ttu-id="42d7f-207">A .NET-es függvényeket a kimenetre küldheti a következő típusok hello tooany:</span><span class="sxs-lookup"><span data-stu-id="42d7f-207">In .NET functions you can output tooany of hello following types:</span></span>

* `out string`
* `TextWriter`
* `Stream`
* `CloudBlobStream`
* `ICloudBlob`
* `CloudBlockBlob` 
* `CloudPageBlob` 

<a name="input-sample"></a>

## <a name="queue-trigger-with-blob-input-and-output-sample"></a><span data-ttu-id="42d7f-208">Várólista eseményindító blob bemeneti és kimeneti minta</span><span class="sxs-lookup"><span data-stu-id="42d7f-208">Queue trigger with blob input and output sample</span></span>
<span data-ttu-id="42d7f-209">Tegyük fel, hogy a következő function.json hello rendelkezik, amely meghatározza egy [Queue Storage eseményindító](functions-bindings-storage-queue.md), a blob-tároló bemeneti és kimeneti blob-tároló.</span><span class="sxs-lookup"><span data-stu-id="42d7f-209">Suppose you have hello following function.json, that defines a [Queue Storage trigger](functions-bindings-storage-queue.md), a blob storage input, and a blob storage output.</span></span> <span data-ttu-id="42d7f-210">Értesítés hello használata hello `queueTrigger` metaadat-tulajdonságnak.</span><span class="sxs-lookup"><span data-stu-id="42d7f-210">Notice hello use of hello `queueTrigger` metadata property.</span></span> <span data-ttu-id="42d7f-211">a bemeneti és kimeneti hello blob `path` tulajdonságok:</span><span class="sxs-lookup"><span data-stu-id="42d7f-211">in hello blob input and output `path` properties:</span></span>

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myInputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "MyStorageConnection",
      "direction": "in"
    },
    {
      "name": "myOutputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "MyStorageConnection",
      "direction": "out"
    }
  ],
  "disabled": false
}
``` 

<span data-ttu-id="42d7f-212">Lásd: hello nyelvspecifikus mintát, amely hello bemeneti blob toohello kimeneti blob másolása.</span><span class="sxs-lookup"><span data-stu-id="42d7f-212">See hello language-specific sample that copies hello input blob toohello output blob.</span></span>

* [<span data-ttu-id="42d7f-213">C#</span><span class="sxs-lookup"><span data-stu-id="42d7f-213">C#</span></span>](#incsharp)
* [<span data-ttu-id="42d7f-214">Node.js</span><span class="sxs-lookup"><span data-stu-id="42d7f-214">Node.js</span></span>](#innodejs)

<a name="incsharp"></a>

### <a name="blob-binding-example-in-c"></a><span data-ttu-id="42d7f-215">A BLOB kötése például a C#</span><span class="sxs-lookup"><span data-stu-id="42d7f-215">Blob binding example in C#</span></span> #

```cs
// Copy blob from input toooutput, based on a queue trigger
public static void Run(string myQueueItem, Stream myInputBlob, out string myOutputBlob, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    myOutputBlob = myInputBlob;
}
```

<a name="innodejs"></a>

### <a name="blob-binding-example-in-nodejs"></a><span data-ttu-id="42d7f-216">A node.js BLOB kötés – példa</span><span class="sxs-lookup"><span data-stu-id="42d7f-216">Blob binding example in Node.js</span></span>

```javascript
// Copy blob from input toooutput, based on a queue trigger
module.exports = function(context) {
    context.log('Node.js Queue trigger function processed', context.bindings.myQueueItem);
    context.bindings.myOutputBlob = context.bindings.myInputBlob;
    context.done();
};
```

## <a name="next-steps"></a><span data-ttu-id="42d7f-217">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="42d7f-217">Next steps</span></span>
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

