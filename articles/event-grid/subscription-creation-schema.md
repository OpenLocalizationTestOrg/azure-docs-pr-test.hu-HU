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
# <a name="event-grid-subscription-schema"></a>Esemény rács előfizetés séma

egy esemény rács előfizetés toocreate, küldése egy kérelem toohello esemény létrehozása előfizetés művelet. A következő formátumban hello használata:

```
PUT /subscriptions/{subscription-id}/resourceGroups/{group-name}/providers/{resource-provider}/{resource-type}/{resource-name}/Microsoft.EventGrid/eventSubscriptions/{event-type-definitions}?api-version=2017-06-15-preview
``` 

Például egy esemény-előfizetést a storage-fiók toocreate `examplestorage` erőforráscsoportban nevű `examplegroup`, használjon hello formátuma:

```
PUT /subscriptions/{subscription-id}/resourceGroups/examplegroup/providers/Microsoft.Storage/storageaccounts/examplestorage/Microsoft.EventGrid/eventSubscriptions/{event-type-definitions}?api-version=2017-06-15-preview
``` 

hello cikkben hello tulajdonságok és -sémát a hello hello kérelem törzse.
 
## <a name="event-subscription-properties"></a>Az esemény-előfizetés tulajdonságai

| Tulajdonság | Típus | Leírás |
| -------- | ---- | ----------- |
| Cél | Objektum | hello objektum, amely meghatározza a hello végpont. |
| Szűrő | Objektum | Hello típusú események szűréséhez választható mező. |

### <a name="destination-object"></a>Célobjektum

| Tulajdonság | Típus | Leírás |
| -------- | ---- | ----------- |
| endpointType | Karakterlánc | hello típusú végpont hello az előfizetéshez (webhook vagy HTTP, az Event Hubs vagy várólista). | 
| végponti URL-cím | Karakterlánc |  | 

### <a name="filter-object"></a>szűrő objektum

| Tulajdonság | Típus | Leírás |
| -------- | ---- | ----------- |
| includedEventTypes | A tömb | Ha hello esemény típusa a hello eseményüzenet egy pontos egyezés tooone esemény típusa neveket felel meg. Riasztást a hiba, ha az esemény neve nem egyezik meg a regisztrált hello esemény típusneve hello esemény forrását. Alapértelmezett megfelel az összes eseménytípust. |
| subjectBeginsWith | Karakterlánc | Előtag-match szűrő toohello tulajdonos mező üdvözlőüzenetére esemény. hello alapértelmezett vagy üres karakterlánc megfelel. | 
| subjectEndsWith | Karakterlánc | Utótag-match szűrő toohello tulajdonos mező üdvözlőüzenetére esemény. hello alapértelmezett vagy üres karakterlánc megfelel. |
| subjectIsCaseSensitive | Karakterlánc | Kis-és nagybetűket szűrőknek megfelelő vezérlőket. |


## <a name="example-subscription-schema"></a>Példa előfizetés séma

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

## <a name="next-steps"></a>Következő lépések

* Egy bevezető tooEvent rács, lásd: [Mi az az esemény rács?](overview.md)
