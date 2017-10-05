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
# <a name="event-grid-event-schema"></a>Esemény rács esemény séma

A cikkben a tulajdonságok és a séma események. Események áll öt szükséges kapcsolatikarakterlánc-tulajdonságokat és a szükséges **adatok** objektum. A Tulajdonságok megegyeznek az összes esemény bármely közzétevőtől. A **adatok** objektum tartalmaz minden publisher jellemző tulajdonságok. A rendszer témaköröket ezeket a tulajdonságokat vonatkoznak, mint például a tároló- és az Event Hubs az erőforrás-szolgáltató.

Az események küldhetők Azure esemény rács tömbben, amely több esemény-objektumot is tartalmazhat. Ha csak egyetlen olyan esemény, a tömb hossza 1. 
 
## <a name="event-properties"></a>Esemény tulajdonságai

Összes esemény azonos következő felső szintű adatokat tartalmazzák.

| Tulajdonság | Típus | Leírás |
| -------- | ---- | ----------- |
| A témakör | Karakterlánc | A forrás teljes erőforrás elérési útja. Ez a mező nincs írható. |
| Tulajdonos | Karakterlánc | Közzétevő meghatározott elérési útját a esemény tulajdonos. |
| Esemény típusa | Karakterlánc | Az esemény adatforrás regisztrált esemény típusok egyike. |
| eventTime | Karakterlánc | Az esemény jön létre az idő alapján a szolgáltató UTC idő szerint. |
| id | Karakterlánc | Az esemény egyedi azonosítója. |
| Adatok | Objektum | Eseményadatok jellemző az erőforrás-szolgáltató. |

## <a name="available-event-sources"></a>Rendelkezésre álló Eseményforrások

A következő eseményforrások közzé az eseményeket a felhasználásához esemény rács keresztül:

* Erőforráscsoportok (műveletek)
* Azure-előfizetések (műveletek)
* Event Hubs
* Egyéni kapcsolatos témakörök

## <a name="azure-subscriptions"></a>Azure-előfizetések

Azure-előfizetések most hozható létre az Azure erőforrás-kezelő felügyeleti esemény, például a virtuális gépek létrehozásakor, vagy a tárfiók törlődik.

### <a name="available-event-types"></a>Rendelkezésre álló eseményoszlopok típusok

- **Microsoft.Resources.ResourceWriteSuccess**: jelenik meg, ha egy erőforrás létrehozása vagy frissítése a művelet sikeres lesz.  
- **Microsoft.Resources.ResourceWriteFailure**: következik be, amikor az erőforrás létrehozása vagy frissítési művelet sikertelen lesz.  
- **Microsoft.Resources.ResourceWriteCancel**: jelenik meg, ha egy erőforrás létrehozása vagy frissítése a művelet meg lett szakítva.  
- **Microsoft.Resources.ResourceDeleteSuccess**: jelenik meg, ha egy erőforrás-törlési művelet sikeres lesz.  
- **Microsoft.Resources.ResourceDeleteFailure**: jelenik meg, ha egy erőforrás-törlési művelet sikertelen lesz.  
- **Microsoft.Resources.ResourceDeleteCancel**: "jelenik meg, ha egy erőforrás törlése meg lett szakítva. Ez akkor fordul elő, ha a sablon-üzembehelyezés meg lett szakítva.

### <a name="example-event-schema"></a>Példa esemény séma

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



## <a name="resource-groups"></a>Erőforráscsoportok

Erőforráscsoportok most hozható létre az Azure erőforrás-kezelő felügyeleti esemény, például a virtuális gépek létrehozásakor, vagy a tárfiók törlődik.

### <a name="available-event-types"></a>Rendelkezésre álló eseményoszlopok típusok

- **Microsoft.Resources.ResourceWriteSuccess**: jelenik meg, ha egy erőforrás létrehozása vagy frissítése a művelet sikeres lesz.  
- **Microsoft.Resources.ResourceWriteFailure**: következik be, amikor az erőforrás létrehozása vagy frissítési művelet sikertelen lesz.  
- **Microsoft.Resources.ResourceWriteCancel**: jelenik meg, ha egy erőforrás létrehozása vagy frissítése a művelet meg lett szakítva.  
- **Microsoft.Resources.ResourceDeleteSuccess**: jelenik meg, ha egy erőforrás-törlési művelet sikeres lesz.  
- **Microsoft.Resources.ResourceDeleteFailure**: jelenik meg, ha egy erőforrás-törlési művelet sikertelen lesz.  
- **Microsoft.Resources.ResourceDeleteCancel**: "jelenik meg, ha egy erőforrás törlése meg lett szakítva. Ez akkor fordul elő, ha a sablon-üzembehelyezés meg lett szakítva.

### <a name="example-event"></a>Példa esemény

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



## <a name="event-hubs"></a>Event Hubs

Event Hubs események jelenleg csak kibocsátott, amikor egy fájl automatikusan elküld a rögzítése funkció segítségével.

### <a name="available-event-types"></a>Rendelkezésre álló eseményoszlopok típusok

- **Microsoft.EventHub.CaptureFileCreated**: következik be, amikor egy rögzítési fájl jön létre.

### <a name="example-event"></a>Példa esemény

Ez a minta az esemény az Event Hubs esemény jelenik meg, ha a rögzítési tárolja a fájlt a séma jeleníti meg. 

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



## <a name="azure-blob-storage"></a>Azure Blob Storage

Az Azure Blob Storage a magán előnézetben előfizetési esemény rács való integrációra.

### <a name="available-event-types"></a>Rendelkezésre álló eseményoszlopok típusok

- **Microsoft.Storage.BlobCreated**: blob létrehozásakor következik be.
- **Microsoft.Storage.BlobDeleted**: blob törlésekor következik be.

### <a name="example-event"></a>Példa esemény

Ez a minta az esemény következik be, amikor létrejön egy blob storage esemény séma jeleníti meg. 

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




## <a name="custom-topics"></a>Egyéni kapcsolatos témakörök

Az egyéni események adatainak hasznos az Ön által megadott, és bármely formátumának helyességét JSON. A legfelső szintű adatok szabványos erőforrás meghatározás eseményként is ugyanazokat a mezőket kell tartalmaznia. Egyéni témakörök események közzétételekor gondolja át az események tárgya útválasztási és szűrés modellezési.

### <a name="example-event"></a>Példa esemény

A következő példa bemutatja egy egyéni témakör esemény:
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

## <a name="next-steps"></a>Következő lépések

* Esemény rácshoz ismertetőért lásd: [Mi az az esemény rács?](overview.md)
* Egy esemény rács előfizetés létrehozásával kapcsolatos további tudnivalókért lásd: [esemény rács előfizetés séma](subscription-creation-schema.md).