---
title: "AAA \"Azure kötegelt feladat kezdési esemény |} Microsoft dokumentumok\""
description: "Útmutató a kötegelt feladat esemény indítása."
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
ms.openlocfilehash: 2cb066be1578741125e9081a84a2b7c74dc8356a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="task-start-event"></a>A feladat kezdési esemény

 Ez az esemény is ki lesz adva a feladatok ütemezett toostart adott számítási csomóponton követően hello az ütemező által. Vegye figyelembe, hogy ha hello feladat újrapróbált vagy helyezte az esemény lesz kell kibocsátott újra hello ugyanezt a feladatot, de hello újrapróbálkozások számlálójának és a rendszer a feladat verziója ennek megfelelően frissül.


 hello következő példa bemutatja a feladat kezdési esemény hello törzsét.

```
{
    "jobId": "job-0000000001",
    "id": "task-5",
    "taskType": "User",
    "systemTaskVersion": 0,
    "nodeInfo": {
        "poolId": "pool-001",
        "nodeId": "tvm-257509324_1-20160908t162728z"
    },
    "multiInstanceSettings": {
        "numberOfInstances": 1
    },
    "constraints": {
        "maxTaskRetryCount": 2
    },
    "executionInfo": {
        "retryCount": 0
    }
}
```

|Elem neve|Típus|Megjegyzések|
|------------------|----------|-----------|
|A JobId értékének|Karakterlánc|hello tevékenységet tartalmazó hello feladat hello azonosítóját.|
|id|Karakterlánc|hello feladat hello azonosítója.|
|taskType|Karakterlánc|hello hello feladat típusát. Ez is, miszerint manager feladata JobManager kell vagy a "User" jelezve, hogy ez nem egy kezelő feladat.|
|systemTaskVersion|Int32|Ez a hello belső újrapróbálkozási számláló meg olyan feladatra. Belső hello Batch szolgáltatás újra egy feladat tooaccount az átmeneti problémákat. A hibák például belső ütemezési hibák vagy kísérletek toorecover a számítási csomópontok hibás állapotban.|
|[nodeInfo](#nodeInfo)|Összetett típus|Mely hello a feladat futott hello számítási csomópont adatait tartalmazza.|
|[multiInstanceSettings](#multiInstanceSettings)|Összetett típus|Határozza meg, hogy hello feladat több számítási csomópont igénylő többpéldányos feladat.  Lásd: [multiInstanceSettings](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-task) részleteiről.|
|[megkötések](#constraints)|Összetett típus|hello végrehajtási megkötések érvényes toothis feladat.|
|[executionInfo](#executionInfo)|Összetett típus|Hello feladat végrehajtásának hello adatait tartalmazza.|

###  <a name="nodeInfo"></a>nodeInfo

|Elem neve|Típus|Megjegyzések|
|------------------|----------|-----------|
|poolId|Karakterlánc|mely hello a feladat futott hello készlet hello azonosítója.|
|nodeId|Karakterlánc|hello csomópont, mely hello a feladat futott hello azonosítója.|

###  <a name="multiInstanceSettings"></a>multiInstanceSettings

|Elem neve|Típus|Megjegyzések|
|------------------|----------|-----------|
|numberOfInstances|int|számítási csomópontok hello feladat által igényelt hello száma.|

###  <a name="constraints"></a>megkötések

|Elem neve|Típus|Megjegyzések|
|------------------|----------|-----------|
|maxTaskRetryCount|Int32|hello maximálisan megengedett számú hello próbálhassa. hello Batch szolgáltatás feladat újrapróbálkozik, ha annak kilépési kódja nem nulla.<br /><br /> Vegye figyelembe, hogy ez az érték kifejezetten vezérlők hello az újrapróbálkozások számát. hello Batch szolgáltatás egyszer megpróbál hello feladat, és előfordulhat, hogy próbálja mentése toothis korlátot. Például ha hello újrapróbálkozások maximális száma: 3, a kötegelt próbálkozik too4 időpontokban (egy kezdeti próbálja és 3 újrapróbálás) mentése feladat.<br /><br /> Ha hello újrapróbálkozások maximális száma 0, hello Batch szolgáltatás nem próbálja meg újra feladatok.<br /><br /> Ha hello újrapróbálkozások maximális száma -1, a hello Batch szolgáltatás korlátozás nélkül feladatok újrapróbálkozik.<br /><br /> hello alapértelmezett értéke 0 (nincs újrapróbálás).|

###  <a name="executionInfo"></a>executionInfo

|Elem neve|Típus|Megjegyzések|
|------------------|----------|-----------|
|a retryCount|Int32|hello hello feladat próbálkozásainak már elérte a hello Batch szolgáltatás hányszor. Ha kilép egy nem nulla kilépési kód mentése toohello megadott MaxTaskRetryCount hello feladat megismétlése|
