---
title: "aaaAzure esemény rács előfizetés séma"
description: "Előfizető tooan esemény Azure esemény rácshoz hello tulajdonságait ismerteti."
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 08/17/2017
ms.author: babanisa
ms.openlocfilehash: 6a96d67975a5a733c5ea3c56ea54501f94ea4cd2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="event-grid-subscription-schema"></a><span data-ttu-id="26ee2-103">Esemény rács előfizetés séma</span><span class="sxs-lookup"><span data-stu-id="26ee2-103">Event Grid subscription schema</span></span>

<span data-ttu-id="26ee2-104">egy esemény rács előfizetés toocreate, küldése egy kérelem toohello esemény létrehozása előfizetés művelet.</span><span class="sxs-lookup"><span data-stu-id="26ee2-104">toocreate an Event Grid subscription, you send a request toohello Create Event subscription operation.</span></span> <span data-ttu-id="26ee2-105">A következő formátumban hello használata:</span><span class="sxs-lookup"><span data-stu-id="26ee2-105">Use hello following format:</span></span>

```
PUT /subscriptions/{subscription-id}/resourceGroups/{group-name}/providers/{resource-provider}/{resource-type}/{resource-name}/Microsoft.EventGrid/eventSubscriptions/{event-type-definitions}?api-version=2017-06-15-preview
``` 

<span data-ttu-id="26ee2-106">Például egy esemény-előfizetést a storage-fiók toocreate `examplestorage` erőforráscsoportban nevű `examplegroup`, használjon hello formátuma:</span><span class="sxs-lookup"><span data-stu-id="26ee2-106">For example, toocreate an event subscription for a storage account named `examplestorage` in a resource group named `examplegroup`, use hello following format:</span></span>

```
PUT /subscriptions/{subscription-id}/resourceGroups/examplegroup/providers/Microsoft.Storage/storageaccounts/examplestorage/Microsoft.EventGrid/eventSubscriptions/{event-type-definitions}?api-version=2017-06-15-preview
``` 

<span data-ttu-id="26ee2-107">hello cikkben hello tulajdonságok és -sémát a hello hello kérelem törzse.</span><span class="sxs-lookup"><span data-stu-id="26ee2-107">hello article describes hello properties and schema for hello body of hello request.</span></span>
 
## <a name="event-subscription-properties"></a><span data-ttu-id="26ee2-108">Az esemény-előfizetés tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="26ee2-108">Event subscription properties</span></span>

| <span data-ttu-id="26ee2-109">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="26ee2-109">Property</span></span> | <span data-ttu-id="26ee2-110">Típus</span><span class="sxs-lookup"><span data-stu-id="26ee2-110">Type</span></span> | <span data-ttu-id="26ee2-111">Leírás</span><span class="sxs-lookup"><span data-stu-id="26ee2-111">Description</span></span> |
| -------- | ---- | ----------- |
| <span data-ttu-id="26ee2-112">Cél</span><span class="sxs-lookup"><span data-stu-id="26ee2-112">destination</span></span> | <span data-ttu-id="26ee2-113">Objektum</span><span class="sxs-lookup"><span data-stu-id="26ee2-113">object</span></span> | <span data-ttu-id="26ee2-114">hello objektum, amely meghatározza a hello végpont.</span><span class="sxs-lookup"><span data-stu-id="26ee2-114">hello object that defines hello endpoint.</span></span> |
| <span data-ttu-id="26ee2-115">Szűrő</span><span class="sxs-lookup"><span data-stu-id="26ee2-115">filter</span></span> | <span data-ttu-id="26ee2-116">Objektum</span><span class="sxs-lookup"><span data-stu-id="26ee2-116">object</span></span> | <span data-ttu-id="26ee2-117">Hello típusú események szűréséhez választható mező.</span><span class="sxs-lookup"><span data-stu-id="26ee2-117">An optional field for filtering hello types of events.</span></span> |

### <a name="destination-object"></a><span data-ttu-id="26ee2-118">Célobjektum</span><span class="sxs-lookup"><span data-stu-id="26ee2-118">destination object</span></span>

| <span data-ttu-id="26ee2-119">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="26ee2-119">Property</span></span> | <span data-ttu-id="26ee2-120">Típus</span><span class="sxs-lookup"><span data-stu-id="26ee2-120">Type</span></span> | <span data-ttu-id="26ee2-121">Leírás</span><span class="sxs-lookup"><span data-stu-id="26ee2-121">Description</span></span> |
| -------- | ---- | ----------- |
| <span data-ttu-id="26ee2-122">endpointType</span><span class="sxs-lookup"><span data-stu-id="26ee2-122">endpointType</span></span> | <span data-ttu-id="26ee2-123">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="26ee2-123">string</span></span> | <span data-ttu-id="26ee2-124">hello típusú végpont hello az előfizetéshez (webhook vagy HTTP, az Event Hubs vagy várólista).</span><span class="sxs-lookup"><span data-stu-id="26ee2-124">hello type of endpoint for hello subscription (webhook/HTTP, Event Hub, or queue).</span></span> | 
| <span data-ttu-id="26ee2-125">végponti URL-cím</span><span class="sxs-lookup"><span data-stu-id="26ee2-125">endpointUrl</span></span> | <span data-ttu-id="26ee2-126">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="26ee2-126">string</span></span> |  | 

### <a name="filter-object"></a><span data-ttu-id="26ee2-127">szűrő objektum</span><span class="sxs-lookup"><span data-stu-id="26ee2-127">filter object</span></span>

| <span data-ttu-id="26ee2-128">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="26ee2-128">Property</span></span> | <span data-ttu-id="26ee2-129">Típus</span><span class="sxs-lookup"><span data-stu-id="26ee2-129">Type</span></span> | <span data-ttu-id="26ee2-130">Leírás</span><span class="sxs-lookup"><span data-stu-id="26ee2-130">Description</span></span> |
| -------- | ---- | ----------- |
| <span data-ttu-id="26ee2-131">includedEventTypes</span><span class="sxs-lookup"><span data-stu-id="26ee2-131">includedEventTypes</span></span> | <span data-ttu-id="26ee2-132">A tömb</span><span class="sxs-lookup"><span data-stu-id="26ee2-132">array</span></span> | <span data-ttu-id="26ee2-133">Ha hello esemény típusa a hello eseményüzenet egy pontos egyezés tooone esemény típusa neveket felel meg.</span><span class="sxs-lookup"><span data-stu-id="26ee2-133">Match when hello event type in hello event message is an exact match tooone of these event type names.</span></span> <span data-ttu-id="26ee2-134">Riasztást a hiba, ha az esemény neve nem egyezik meg a regisztrált hello esemény típusneve hello esemény forrását.</span><span class="sxs-lookup"><span data-stu-id="26ee2-134">Raises an error when event name does not match hello registered event type names for hello event source.</span></span> <span data-ttu-id="26ee2-135">Alapértelmezett megfelel az összes eseménytípust.</span><span class="sxs-lookup"><span data-stu-id="26ee2-135">Default matches all event types.</span></span> |
| <span data-ttu-id="26ee2-136">subjectBeginsWith</span><span class="sxs-lookup"><span data-stu-id="26ee2-136">subjectBeginsWith</span></span> | <span data-ttu-id="26ee2-137">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="26ee2-137">string</span></span> | <span data-ttu-id="26ee2-138">Előtag-match szűrő toohello tulajdonos mező üdvözlőüzenetére esemény.</span><span class="sxs-lookup"><span data-stu-id="26ee2-138">A prefix-match filter toohello subject field in hello event message.</span></span> <span data-ttu-id="26ee2-139">hello alapértelmezett vagy üres karakterlánc megfelel.</span><span class="sxs-lookup"><span data-stu-id="26ee2-139">hello default or empty string matches all.</span></span> | 
| <span data-ttu-id="26ee2-140">subjectEndsWith</span><span class="sxs-lookup"><span data-stu-id="26ee2-140">subjectEndsWith</span></span> | <span data-ttu-id="26ee2-141">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="26ee2-141">string</span></span> | <span data-ttu-id="26ee2-142">Utótag-match szűrő toohello tulajdonos mező üdvözlőüzenetére esemény.</span><span class="sxs-lookup"><span data-stu-id="26ee2-142">A suffix-match filter toohello subject field in hello event message.</span></span> <span data-ttu-id="26ee2-143">hello alapértelmezett vagy üres karakterlánc megfelel.</span><span class="sxs-lookup"><span data-stu-id="26ee2-143">hello default or empty string matches all.</span></span> |
| <span data-ttu-id="26ee2-144">subjectIsCaseSensitive</span><span class="sxs-lookup"><span data-stu-id="26ee2-144">subjectIsCaseSensitive</span></span> | <span data-ttu-id="26ee2-145">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="26ee2-145">string</span></span> | <span data-ttu-id="26ee2-146">Kis-és nagybetűket szűrőknek megfelelő vezérlőket.</span><span class="sxs-lookup"><span data-stu-id="26ee2-146">Controls case-sensitive matching for filters.</span></span> |


## <a name="example-subscription-schema"></a><span data-ttu-id="26ee2-147">Példa előfizetés séma</span><span class="sxs-lookup"><span data-stu-id="26ee2-147">Example subscription schema</span></span>

```json
{
  "properties": {
    "destination": {
      "endpointType": "webhook",
      "properties": {
          "endpointUrl": "https://example.azurewebsites.net/api/HttpTriggerCSharp1?code=VXbGWce53l48Mt8wuotr0GPmyJ/nDT4hgdFj9DpBiRt38qqnnm5OFg=="
      }
    },
    "filter": {
      "includedEventTypes": [ "blobCreated", "blobDeleted" ],
      "subjectBeginsWith": "blobServices/default/containers/mycontainer/log",
      "subjectEndsWith": ".jpg",
      "subjectIsCaseSensitive": "true"
    }
  }
}
```

## <a name="next-steps"></a><span data-ttu-id="26ee2-148">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="26ee2-148">Next steps</span></span>

* <span data-ttu-id="26ee2-149">Egy bevezető tooEvent rács, lásd: [Mi az az esemény rács?](overview.md)</span><span class="sxs-lookup"><span data-stu-id="26ee2-149">For an introduction tooEvent Grid, see [What is Event Grid?](overview.md)</span></span>
