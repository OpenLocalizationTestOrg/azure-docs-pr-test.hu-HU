---
title: "Az Azure Event rács blob storage esemény séma"
description: "A blob storage-események Azure esemény rácshoz tartozó tulajdonságait ismerteti"
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 11/08/2017
ms.author: tomfitz
ms.openlocfilehash: bdc64733b75fd809cf0245986aa96370343c1a34
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 11/11/2017
---
# <a name="azure-event-grid-event-schema-for-blob-storage"></a>A Blob storage Azure esemény rács esemény séma

Ez a cikk a tulajdonságok és a séma biztosít a blob storage-események. Egy esemény sémák bemutatása, lásd: [Azure esemény rács esemény séma](event-schema.md).

## <a name="available-event-types"></a>Rendelkezésre álló eseményoszlopok típusok

A BLOB storage bocsát ki a következő esemény típusa:

| Esemény típusa | Leírás |
| ---------- | ----------- |
| Microsoft.Storage.BlobCreated | Egy blob létrehozásakor következik be. |
| Microsoft.Storage.BlobDeleted | Egy blob törlésekor következik be. |

## <a name="example-event"></a>Példa esemény

A következő példa bemutatja a séma, esemény létrehozott BLOB: 

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/xstoretestaccount",
  "subject": "/blobServices/default/containers/testcontainer/blobs/testfile.txt",
  "eventType": "Microsoft.Storage.BlobCreated",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "api": "PutBlockList",
    "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
    "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
    "eTag": "0x8D4BCC2E4835CD0",
    "contentType": "text/plain",
    "contentLength": 524288,
    "blobType": "BlockBlob",
    "url": "https://example.blob.core.windows.net/testcontainer/testfile.txt",
    "sequencer": "00000000000004420000000000028963",
    "storageDiagnostics": {
      "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  }
}]
```

A séma blob törlése esemény hasonlít: 

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/xstoretestaccount",
  "subject": "/blobServices/default/containers/testcontainer/blobs/testfile.txt",
  "eventType": "Microsoft.Storage.BlobDeleted",
  "eventTime": "2017-11-07T20:09:22.5674003Z",
  "id": "4c2359fe-001e-00ba-0e04-58586806d298",
  "data": {
    "api": "DeleteBlob",
    "requestId": "4c2359fe-001e-00ba-0e04-585868000000",
    "contentType": "text/plain",
    "blobType": "BlockBlob",
    "url": "https://example.blob.core.windows.net/testcontainer/testfile.txt",
    "sequencer": "0000000000000281000000000002F5CA",
    "storageDiagnostics": {
      "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  }
}]
```
 
## <a name="event-properties"></a>Esemény tulajdonságai

Az esemény a következő legfelső szintű adatokat tartalmaz:

| Tulajdonság | Típus | Leírás |
| -------- | ---- | ----------- |
| A témakör | Karakterlánc | A forrás teljes erőforrás elérési útja. Ez a mező nincs írható. |
| Tulajdonos | Karakterlánc | Az esemény tulajdonos közzétevő által megadott elérési útja. |
| Esemény típusa | Karakterlánc | Az esemény adatforrás regisztrált esemény típusok egyike. |
| eventTime | Karakterlánc | Az esemény jön létre az idő alapján a szolgáltató UTC idő szerint. |
| id | Karakterlánc | Az esemény egyedi azonosítója. |
| Adatok | Objektum | A BLOB storage eseményadatok. |

Az objektum tulajdonságai a következők:

| Tulajdonság | Típus | Leírás |
| -------- | ---- | ----------- |
| api-t | Karakterlánc | A művelet az eseményt kiváltó. |
| clientRequestId | Karakterlánc | Egy ügyfél által létrehozott, nem átlátszó értéket egy 1 KB-os karakteres korlátot. Ha engedélyezte a naplózás tárolási analitika, az elemzési naplókat van rögzítve. |
| Kérelemazonosító | Karakterlánc | A kérelem egyedi azonosítója. A hibaelhárítás a kérelem használni. |
| ETag | Karakterlánc | Az érték, amely feltételesen műveletek végrehajtásához használhatja. |
| A ContentType | Karakterlánc | A BLOB megadott tartalomtípus. |
| contentLength | egész szám | A blob bájtban kifejezett mérete. |
| BlobType | Karakterlánc | A blob típusú. |
| URL-címe | Karakterlánc | A blob elérési útja. |
| sorrendvezérlő | Karakterlánc | Egy felhasználó által felügyelt érték, amely segítségével nyomon követheti a kéréseket. |
| storageDiagnostics | Objektum | A tárolási diagnosztikai információt. |
 
## <a name="next-steps"></a>Következő lépések

* Megismerkedhet az Azure Event rács, lásd: [Mi az az esemény rács?](overview.md)
* Egy esemény rács Azure-előfizetés létrehozásával kapcsolatos további információkért lásd: [esemény rács előfizetés séma](subscription-creation-schema.md).
* A blob storage-események működő bevezető, lásd: [útvonal Blob storage-események - Azure CLI](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json). 
