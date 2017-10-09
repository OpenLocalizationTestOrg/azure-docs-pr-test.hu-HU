---
title: "AAA \"Azure Batch-készlet törlése befejeződésének eseményét. |} Microsoft dokumentumok\""
description: "Útmutató a Batch-készlet törlése befejeződésének eseményét."
services: batch
author: tamram
manager: timlt
ms.assetid: 
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: tamram
ms.openlocfilehash: 494c371e48ebfb1bf3d2973a7401829a939ba141
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="pool-delete-complete-event"></a>Készlet törlés befejeződésének eseményét.

 Ez az esemény is ki lesz adva a készlet törlési művelet befejeződésekor.

 hello következő példa bemutatja a készletben törlés befejeződésének eseményét hello törzsét.

```
{
    "id": "myPool1",
    "startTime": "2016-09-09T22:13:48.579Z",
    "endTime": "2016-09-09T22:14:08.836Z"
}
```

|Elem|Típus|Megjegyzések|
|-------------|----------|-----------|
|id|Karakterlánc|hello készlet hello azonosítója.|
|startTime|Dátum és idő|hello hello készlet törlése indulásakor.|
|endTime|Dátum és idő|hello idő hello készlet törlése befejeződött.|

## <a name="remarks"></a>Megjegyzések
Állapotok és hibakódok készlet átméretezés kapcsolatos további információkért lásd: [címkészlet törlése fiókból](https://docs.microsoft.com/rest/api/batchservice/delete-a-pool-from-an-account).