---
title: "Az Azure Event rács esemény séma"
description: "Azure Event rácshoz események tartozó tulajdonságait ismerteti."
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 08/15/2017
ms.author: babanisa
ms.openlocfilehash: 9e3c7b31ef23b29827d7184dc033227685ed92f8
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="event-grid-event-schema"></a><span data-ttu-id="c61fa-103">Esemény rács esemény séma</span><span class="sxs-lookup"><span data-stu-id="c61fa-103">Event Grid event schema</span></span>

<span data-ttu-id="c61fa-104">A cikkben a tulajdonságok és a séma események.</span><span class="sxs-lookup"><span data-stu-id="c61fa-104">This article provides the properties and schema for events.</span></span> <span data-ttu-id="c61fa-105">Események áll öt szükséges kapcsolatikarakterlánc-tulajdonságokat és a szükséges **adatok** objektum.</span><span class="sxs-lookup"><span data-stu-id="c61fa-105">Events consist of a set of five required string properties and a required **data** object.</span></span> <span data-ttu-id="c61fa-106">A Tulajdonságok megegyeznek az összes esemény bármely közzétevőtől.</span><span class="sxs-lookup"><span data-stu-id="c61fa-106">The properties are common to all events from any publisher.</span></span> <span data-ttu-id="c61fa-107">A **adatok** objektum tartalmaz minden publisher jellemző tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="c61fa-107">The **data** object contains properties that are specific to each publisher.</span></span> <span data-ttu-id="c61fa-108">A rendszer témaköröket ezeket a tulajdonságokat vonatkoznak, mint például a tároló- és az Event Hubs az erőforrás-szolgáltató.</span><span class="sxs-lookup"><span data-stu-id="c61fa-108">For system topics, these properties are specific to the resource provider, such as Storage or Event Hubs.</span></span>

<span data-ttu-id="c61fa-109">Az események küldhetők Azure esemény rács tömbben, amely több esemény-objektumot is tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="c61fa-109">Events are sent to Azure Event Grid in an array, which can contain multiple event objects.</span></span> <span data-ttu-id="c61fa-110">Ha csak egyetlen olyan esemény, a tömb hossza 1.</span><span class="sxs-lookup"><span data-stu-id="c61fa-110">If there is only a single event, the array has a length of 1.</span></span> 
 
## <a name="event-properties"></a><span data-ttu-id="c61fa-111">Esemény tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="c61fa-111">Event properties</span></span>

<span data-ttu-id="c61fa-112">Összes esemény azonos következő felső szintű adatokat tartalmazzák.</span><span class="sxs-lookup"><span data-stu-id="c61fa-112">All events will contain the same following top level data.</span></span>

| <span data-ttu-id="c61fa-113">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="c61fa-113">Property</span></span> | <span data-ttu-id="c61fa-114">Típus</span><span class="sxs-lookup"><span data-stu-id="c61fa-114">Type</span></span> | <span data-ttu-id="c61fa-115">Leírás</span><span class="sxs-lookup"><span data-stu-id="c61fa-115">Description</span></span> |
| -------- | ---- | ----------- |
| <span data-ttu-id="c61fa-116">A témakör</span><span class="sxs-lookup"><span data-stu-id="c61fa-116">topic</span></span> | <span data-ttu-id="c61fa-117">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="c61fa-117">string</span></span> | <span data-ttu-id="c61fa-118">A forrás teljes erőforrás elérési útja.</span><span class="sxs-lookup"><span data-stu-id="c61fa-118">Full resource path to the event source.</span></span> <span data-ttu-id="c61fa-119">Ez a mező nincs írható.</span><span class="sxs-lookup"><span data-stu-id="c61fa-119">This field is not writeable.</span></span> |
| <span data-ttu-id="c61fa-120">Tulajdonos</span><span class="sxs-lookup"><span data-stu-id="c61fa-120">subject</span></span> | <span data-ttu-id="c61fa-121">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="c61fa-121">string</span></span> | <span data-ttu-id="c61fa-122">Közzétevő meghatározott elérési útját a esemény tulajdonos.</span><span class="sxs-lookup"><span data-stu-id="c61fa-122">Publisher defined path to the event subject.</span></span> |
| <span data-ttu-id="c61fa-123">Esemény típusa</span><span class="sxs-lookup"><span data-stu-id="c61fa-123">eventType</span></span> | <span data-ttu-id="c61fa-124">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="c61fa-124">string</span></span> | <span data-ttu-id="c61fa-125">Az esemény adatforrás regisztrált esemény típusok egyike.</span><span class="sxs-lookup"><span data-stu-id="c61fa-125">One of the registered event types for this event source.</span></span> |
| <span data-ttu-id="c61fa-126">eventTime</span><span class="sxs-lookup"><span data-stu-id="c61fa-126">eventTime</span></span> | <span data-ttu-id="c61fa-127">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="c61fa-127">string</span></span> | <span data-ttu-id="c61fa-128">Az esemény jön létre az idő alapján a szolgáltató UTC idő szerint.</span><span class="sxs-lookup"><span data-stu-id="c61fa-128">The time the event is generated based on the provider's UTC time.</span></span> |
| <span data-ttu-id="c61fa-129">id</span><span class="sxs-lookup"><span data-stu-id="c61fa-129">id</span></span> | <span data-ttu-id="c61fa-130">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="c61fa-130">string</span></span> | <span data-ttu-id="c61fa-131">Az esemény egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="c61fa-131">Unique identifier for the event.</span></span> |
| <span data-ttu-id="c61fa-132">Adatok</span><span class="sxs-lookup"><span data-stu-id="c61fa-132">data</span></span> | <span data-ttu-id="c61fa-133">Objektum</span><span class="sxs-lookup"><span data-stu-id="c61fa-133">object</span></span> | <span data-ttu-id="c61fa-134">Eseményadatok jellemző az erőforrás-szolgáltató.</span><span class="sxs-lookup"><span data-stu-id="c61fa-134">Event data specific to the resource provider.</span></span> |

## <a name="available-event-sources"></a><span data-ttu-id="c61fa-135">Rendelkezésre álló Eseményforrások</span><span class="sxs-lookup"><span data-stu-id="c61fa-135">Available event sources</span></span>

<span data-ttu-id="c61fa-136">A következő eseményforrások közzé az eseményeket a felhasználásához esemény rács keresztül:</span><span class="sxs-lookup"><span data-stu-id="c61fa-136">The following event sources publish events for consumption via Event Grid:</span></span>

* <span data-ttu-id="c61fa-137">Erőforráscsoportok (műveletek)</span><span class="sxs-lookup"><span data-stu-id="c61fa-137">Resource Groups (management operations)</span></span>
* <span data-ttu-id="c61fa-138">Azure-előfizetések (műveletek)</span><span class="sxs-lookup"><span data-stu-id="c61fa-138">Azure Subscriptions (management operations)</span></span>
* <span data-ttu-id="c61fa-139">Event Hubs</span><span class="sxs-lookup"><span data-stu-id="c61fa-139">Event Hubs</span></span>
* <span data-ttu-id="c61fa-140">Egyéni kapcsolatos témakörök</span><span class="sxs-lookup"><span data-stu-id="c61fa-140">Custom Topics</span></span>

## <a name="azure-subscriptions"></a><span data-ttu-id="c61fa-141">Azure-előfizetések</span><span class="sxs-lookup"><span data-stu-id="c61fa-141">Azure Subscriptions</span></span>

<span data-ttu-id="c61fa-142">Azure-előfizetések most hozható létre az Azure erőforrás-kezelő felügyeleti esemény, például a virtuális gépek létrehozásakor, vagy a tárfiók törlődik.</span><span class="sxs-lookup"><span data-stu-id="c61fa-142">Azure subscriptions can now emit management events from Azure Resource Manager such as when a VM is created or a storage account is deleted.</span></span>

### <a name="available-event-types"></a><span data-ttu-id="c61fa-143">Rendelkezésre álló eseményoszlopok típusok</span><span class="sxs-lookup"><span data-stu-id="c61fa-143">Available event types</span></span>

- <span data-ttu-id="c61fa-144">**Microsoft.Resources.ResourceWriteSuccess**: jelenik meg, ha egy erőforrás létrehozása vagy frissítése a művelet sikeres lesz.</span><span class="sxs-lookup"><span data-stu-id="c61fa-144">**Microsoft.Resources.ResourceWriteSuccess**: Raised when a resource create or update operation succeeds.</span></span>  
- <span data-ttu-id="c61fa-145">**Microsoft.Resources.ResourceWriteFailure**: következik be, amikor az erőforrás létrehozása vagy frissítési művelet sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="c61fa-145">**Microsoft.Resources.ResourceWriteFailure**: Raised when a resource create or update operation fails.</span></span>  
- <span data-ttu-id="c61fa-146">**Microsoft.Resources.ResourceWriteCancel**: jelenik meg, ha egy erőforrás létrehozása vagy frissítése a művelet meg lett szakítva.</span><span class="sxs-lookup"><span data-stu-id="c61fa-146">**Microsoft.Resources.ResourceWriteCancel**: Raised when a resource create or update operation is cancelled.</span></span>  
- <span data-ttu-id="c61fa-147">**Microsoft.Resources.ResourceDeleteSuccess**: jelenik meg, ha egy erőforrás-törlési művelet sikeres lesz.</span><span class="sxs-lookup"><span data-stu-id="c61fa-147">**Microsoft.Resources.ResourceDeleteSuccess**: Raised when a resource deletion operation succeeds.</span></span>  
- <span data-ttu-id="c61fa-148">**Microsoft.Resources.ResourceDeleteFailure**: jelenik meg, ha egy erőforrás-törlési művelet sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="c61fa-148">**Microsoft.Resources.ResourceDeleteFailure**: Raised when a resource delete operation fails.</span></span>  
- <span data-ttu-id="c61fa-149">**Microsoft.Resources.ResourceDeleteCancel**: "jelenik meg, ha egy erőforrás törlése meg lett szakítva.</span><span class="sxs-lookup"><span data-stu-id="c61fa-149">**Microsoft.Resources.ResourceDeleteCancel**: "Raised when a resource delete is cancelled.</span></span> <span data-ttu-id="c61fa-150">Ez akkor fordul elő, ha a sablon-üzembehelyezés meg lett szakítva.</span><span class="sxs-lookup"><span data-stu-id="c61fa-150">This happens when template deployment is cancelled.</span></span>

### <a name="example-event-schema"></a><span data-ttu-id="c61fa-151">Példa esemény séma</span><span class="sxs-lookup"><span data-stu-id="c61fa-151">Example event schema</span></span>

```json
[
    {
    "topic":"/subscriptions/{subscription-id}",
    "subject":"/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventGrid/eventSubscriptions/LogicAppdd584bdf-8347-49c9-b9a9-d1f980783501",
    "eventType":"Microsoft.Resources.ResourceWriteSuccess",
    "eventTime":"2017-08-16T03:54:38.2696833Z",
    "id":"25b3b0d0-d79b-44d5-9963-440d4e6a9bba",
    "data": {
        "authorization":"{azure_resource_manager_authorizations}",
        "claims":"{azure_resource_manager_claims}",
        "correlationId":"54ef1e39-6a82-44b3-abc1-bdeb6ce4d3c6",
        "httpRequest":"",
        "resourceProvider":"Microsoft.EventGrid",
        "resourceUri":"/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventGrid/eventSubscriptions/LogicAppdd584bdf-8347-49c9-b9a9-d1f980783501",
        "operationName":"Microsoft.EventGrid/eventSubscriptions/write",
        "status":"Succeeded",
        "subscriptionId":"{subscription-id}",
        "tenantId":"72f988bf-86f1-41af-91ab-2d7cd011db47"
        },
    }
]
```



## <a name="resource-groups"></a><span data-ttu-id="c61fa-152">Erőforráscsoportok</span><span class="sxs-lookup"><span data-stu-id="c61fa-152">Resource Groups</span></span>

<span data-ttu-id="c61fa-153">Erőforráscsoportok most hozható létre az Azure erőforrás-kezelő felügyeleti esemény, például a virtuális gépek létrehozásakor, vagy a tárfiók törlődik.</span><span class="sxs-lookup"><span data-stu-id="c61fa-153">Resource Groups can now emit management events from Azure Resource Manager such as when a VM is created or a storage account is deleted.</span></span>

### <a name="available-event-types"></a><span data-ttu-id="c61fa-154">Rendelkezésre álló eseményoszlopok típusok</span><span class="sxs-lookup"><span data-stu-id="c61fa-154">Available event types</span></span>

- <span data-ttu-id="c61fa-155">**Microsoft.Resources.ResourceWriteSuccess**: jelenik meg, ha egy erőforrás létrehozása vagy frissítése a művelet sikeres lesz.</span><span class="sxs-lookup"><span data-stu-id="c61fa-155">**Microsoft.Resources.ResourceWriteSuccess**: Raised when a resource create or update operation succeeds.</span></span>  
- <span data-ttu-id="c61fa-156">**Microsoft.Resources.ResourceWriteFailure**: következik be, amikor az erőforrás létrehozása vagy frissítési művelet sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="c61fa-156">**Microsoft.Resources.ResourceWriteFailure**: Raised when a resource create or update operation fails.</span></span>  
- <span data-ttu-id="c61fa-157">**Microsoft.Resources.ResourceWriteCancel**: jelenik meg, ha egy erőforrás létrehozása vagy frissítése a művelet meg lett szakítva.</span><span class="sxs-lookup"><span data-stu-id="c61fa-157">**Microsoft.Resources.ResourceWriteCancel**: Raised when a resource create or update operation is cancelled.</span></span>  
- <span data-ttu-id="c61fa-158">**Microsoft.Resources.ResourceDeleteSuccess**: jelenik meg, ha egy erőforrás-törlési művelet sikeres lesz.</span><span class="sxs-lookup"><span data-stu-id="c61fa-158">**Microsoft.Resources.ResourceDeleteSuccess**: Raised when a resource deletion operation succeeds.</span></span>  
- <span data-ttu-id="c61fa-159">**Microsoft.Resources.ResourceDeleteFailure**: jelenik meg, ha egy erőforrás-törlési művelet sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="c61fa-159">**Microsoft.Resources.ResourceDeleteFailure**: Raised when a resource delete operation fails.</span></span>  
- <span data-ttu-id="c61fa-160">**Microsoft.Resources.ResourceDeleteCancel**: "jelenik meg, ha egy erőforrás törlése meg lett szakítva.</span><span class="sxs-lookup"><span data-stu-id="c61fa-160">**Microsoft.Resources.ResourceDeleteCancel**: "Raised when a resource delete is cancelled.</span></span> <span data-ttu-id="c61fa-161">Ez akkor fordul elő, ha a sablon-üzembehelyezés meg lett szakítva.</span><span class="sxs-lookup"><span data-stu-id="c61fa-161">This happens when template deployment is cancelled.</span></span>

### <a name="example-event"></a><span data-ttu-id="c61fa-162">Példa esemény</span><span class="sxs-lookup"><span data-stu-id="c61fa-162">Example event</span></span>

```json
[
    {
    "topic":"/subscriptions/{subscription-id}/resourceGroups/{resource-group}",
    "subject":"/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventGrid/eventSubscriptions/LogicAppdd584bdf-8347-49c9-b9a9-d1f980783501",
    "eventType":"Microsoft.Resources.ResourceWriteSuccess",
    "eventTime":"2017-08-16T03:54:38.2696833Z",
    "id":"25b3b0d0-d79b-44d5-9963-440d4e6a9bba",
    "data": {
        "authorization":"{azure_resource_manager_authorizations}",
        "claims":"{azure_resource_manager_claims}",
        "correlationId":"54ef1e39-6a82-44b3-abc1-bdeb6ce4d3c6",
        "httpRequest":"",
        "resourceProvider":"Microsoft.EventGrid",
        "resourceUri":"/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventGrid/eventSubscriptions/LogicAppdd584bdf-8347-49c9-b9a9-d1f980783501",
        "operationName":"Microsoft.EventGrid/eventSubscriptions/write",
        "status":"Succeeded",
        "subscriptionId":"{subscription-id}",
        "tenantId":"72f988bf-86f1-41af-91ab-2d7cd011db47"
        },
    }
]
```



## <a name="event-hubs"></a><span data-ttu-id="c61fa-163">Event Hubs</span><span class="sxs-lookup"><span data-stu-id="c61fa-163">Event Hubs</span></span>

<span data-ttu-id="c61fa-164">Event Hubs események jelenleg csak kibocsátott, amikor egy fájl automatikusan elküld a rögzítése funkció segítségével.</span><span class="sxs-lookup"><span data-stu-id="c61fa-164">Event Hubs events are currently only emitted when a file is automatically sent to storage using the Capture feature.</span></span>

### <a name="available-event-types"></a><span data-ttu-id="c61fa-165">Rendelkezésre álló eseményoszlopok típusok</span><span class="sxs-lookup"><span data-stu-id="c61fa-165">Available event types</span></span>

- <span data-ttu-id="c61fa-166">**Microsoft.EventHub.CaptureFileCreated**: következik be, amikor egy rögzítési fájl jön létre.</span><span class="sxs-lookup"><span data-stu-id="c61fa-166">**Microsoft.EventHub.CaptureFileCreated**: Raised when a capture file is created.</span></span>

### <a name="example-event"></a><span data-ttu-id="c61fa-167">Példa esemény</span><span class="sxs-lookup"><span data-stu-id="c61fa-167">Example event</span></span>

<span data-ttu-id="c61fa-168">Ez a minta az esemény az Event Hubs esemény jelenik meg, ha a rögzítési tárolja a fájlt a séma jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="c61fa-168">This sample event shows the schema of an Event Hubs event raised when Capture stores a file.</span></span> 

```json
[
    {
        "topic": "/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.EventHub/namespaces/{event-hubs-ns}",
        "subject": "eventhubs/eh1",
        "eventType": "Microsoft.EventHub.CaptureFileCreated",
        "eventTime": "2017-07-11T00:55:55.0120485Z",
        "id": "bd440490-a65e-4c97-8298-ef1eb325673c",
        "data": {
            "fileUrl": "https://gridtest1.blob.core.windows.net/acontainer/eventgridtest1/eh1/1/2017/07/11/00/54/54.avro",
            "fileType": "AzureBlockBlob",
            "partitionId": "1",
            "sizeInBytes": 0,
            "eventCount": 0,
            "firstSequenceNumber": -1,
            "lastSequenceNumber": -1,
            "firstEnqueueTime": "0001-01-01T00:00:00",
            "lastEnqueueTime": "0001-01-01T00:00:00"
        },
    }
]

```



## <a name="azure-blob-storage"></a><span data-ttu-id="c61fa-169">Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="c61fa-169">Azure Blob Storage</span></span>

<span data-ttu-id="c61fa-170">Az Azure Blob Storage a magán előnézetben előfizetési esemény rács való integrációra.</span><span class="sxs-lookup"><span data-stu-id="c61fa-170">Azure Blob Storage in private preview with sign-up for integration with Event Grid.</span></span>

### <a name="available-event-types"></a><span data-ttu-id="c61fa-171">Rendelkezésre álló eseményoszlopok típusok</span><span class="sxs-lookup"><span data-stu-id="c61fa-171">Available event types</span></span>

- <span data-ttu-id="c61fa-172">**Microsoft.Storage.BlobCreated**: blob létrehozásakor következik be.</span><span class="sxs-lookup"><span data-stu-id="c61fa-172">**Microsoft.Storage.BlobCreated**: Raised when a blob is created.</span></span>
- <span data-ttu-id="c61fa-173">**Microsoft.Storage.BlobDeleted**: blob törlésekor következik be.</span><span class="sxs-lookup"><span data-stu-id="c61fa-173">**Microsoft.Storage.BlobDeleted**: Raised when a blob is deleted.</span></span>

### <a name="example-event"></a><span data-ttu-id="c61fa-174">Példa esemény</span><span class="sxs-lookup"><span data-stu-id="c61fa-174">Example event</span></span>

<span data-ttu-id="c61fa-175">Ez a minta az esemény következik be, amikor létrejön egy blob storage esemény séma jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="c61fa-175">This sample event shows the schema of a storage event raised when a blob is created.</span></span> 

```json
[
  {
    "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/xstoretestaccount",
    "subject": "/blobServices/default/containers/oc2d2817345i200097container/blobs/oc2d2817345i20002296blob",
    "eventType": "Microsoft.Storage.BlobCreated",
    "eventTime": "2017-06-26T18:41:00.9584103Z",
    "id": "831e1650-001e-001b-66ab-eeb76e069631",
    "data": {
      "api": "PutBlockList",
      "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
      "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
      "eTag": "0x8D4BCC2E4835CD0",
      "contentType": "application/octet-stream",
      "contentLength": 524288,
      "blobType": "BlockBlob",
      "url": "https://oc2d2817345i60006.blob.core.windows.net/oc2d2817345i200097container/oc2d2817345i20002296blob",
      "sequencer": "00000000000004420000000000028963",
      "storageDiagnostics": {
        "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
      }
    }
  }
]
```




## <a name="custom-topics"></a><span data-ttu-id="c61fa-176">Egyéni kapcsolatos témakörök</span><span class="sxs-lookup"><span data-stu-id="c61fa-176">Custom Topics</span></span>

<span data-ttu-id="c61fa-177">Az egyéni események adatainak hasznos az Ön által megadott, és bármely formátumának helyességét JSON.</span><span class="sxs-lookup"><span data-stu-id="c61fa-177">The data payload of your custom events is defined by you and can be any well formated JSON.</span></span> <span data-ttu-id="c61fa-178">A legfelső szintű adatok szabványos erőforrás meghatározás eseményként is ugyanazokat a mezőket kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="c61fa-178">The top level data should contain the same fields as standard resource defined events.</span></span> <span data-ttu-id="c61fa-179">Egyéni témakörök események közzétételekor gondolja át az események tárgya útválasztási és szűrés modellezési.</span><span class="sxs-lookup"><span data-stu-id="c61fa-179">When publishing events to custom topics you should consider modeling the subject of your events to aid in routing and filtering.</span></span>

### <a name="example-event"></a><span data-ttu-id="c61fa-180">Példa esemény</span><span class="sxs-lookup"><span data-stu-id="c61fa-180">Example event</span></span>

<span data-ttu-id="c61fa-181">A következő példa bemutatja egy egyéni témakör esemény:</span><span class="sxs-lookup"><span data-stu-id="c61fa-181">The following example shows an event for a custom topic:</span></span>
````json
[
  {
    "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.EventGrid/topics/myeventgridtopic",
    "subject": "/myapp/vehicles/motorcycles",    
    "id": "b68529f3-68cd-4744-baa4-3c0498ec19e2",
    "eventType": "recordInserted",
    "eventTime": "2017-06-26T18:41:00.9584103Z",
    "data":{
      "make": "Ducati",
      "model": "Monster"
    }
  }
]

````

## <a name="next-steps"></a><span data-ttu-id="c61fa-182">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c61fa-182">Next steps</span></span>

* <span data-ttu-id="c61fa-183">Esemény rácshoz ismertetőért lásd: [Mi az az esemény rács?](overview.md)</span><span class="sxs-lookup"><span data-stu-id="c61fa-183">For an introduction to Event Grid, see [What is Event Grid?](overview.md)</span></span>
* <span data-ttu-id="c61fa-184">Egy esemény rács előfizetés létrehozásával kapcsolatos további tudnivalókért lásd: [esemény rács előfizetés séma](subscription-creation-schema.md).</span><span class="sxs-lookup"><span data-stu-id="c61fa-184">To learn about creating an Event Grid subscription, see [Event Grid subscription schema](subscription-creation-schema.md).</span></span>
