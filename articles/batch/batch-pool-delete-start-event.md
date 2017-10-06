---
title: "AAA \"Azure Batch készlet törlése start esemény |} Microsoft dokumentumok\""
description: "Útmutató a Batch-készlet törlése esemény indítása."
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
ms.openlocfilehash: 79bb28bffc760a49cc0a95062f5086dc96c6a795
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="pool-delete-start-event"></a>Készlet törlése start esemény

 Ez az esemény kibocsátott, amikor egy alkalmazáskészlet törlési művelete megkezdődött. Mivel hello készlet törlése egy aszinkron esemény, számíthat egy alkalmazáskészlet törlés befejeződésének eseményét. toobe kibocsátott után hello törlésének művelete befejeződött.

 hello következő példa bemutatja egy készlet törlése start esemény hello törzsét.

```
{
    "id": "myPool1"
}
```

|Elem|Típus|Megjegyzések|
|-------------|----------|-----------|
|id|Karakterlánc|hello készlet hello azonosítója.|
