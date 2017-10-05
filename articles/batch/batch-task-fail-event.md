---
title: "Azure Batch feladat sikertelen esemény |} Microsoft Docs"
description: "Útmutató a kötegelt feladat nem esemény."
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
ms.openlocfilehash: 08feb4ec34bb1635f8ea744b54a10b677b94ab3e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="task-fail-event"></a>A feladat sikertelen esemény

 Ez az esemény kibocsátott hibával feladat befejezését. Jelenleg minden nem nulla kilépési kód minősülnek hibák. Ezt az eseményt a rendszer kibocsátott *kívül* feladat esemény befejeződik, és segítségével észlelés, ha a feladat végrehajtása sikertelen.


 A következő példa bemutatja, hogy a szervezet egy feladat sikertelen esemény.

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
        "startTime": "2016-09-08T16:32:23.799Z",
        "endTime": "2016-09-08T16:34:00.666Z",
        "exitCode": 1,
        "retryCount": 2,
        "requeueCount": 0
    }
}
```

|Elem neve|Típus|Megjegyzések|
|------------------|----------|-----------|
|A JobId értékének|Karakterlánc|A feladat a tevékenységet tartalmazó azonosítója.|
|id|Karakterlánc|A feladat azonosítóját.|
|taskType|Karakterlánc|A tevékenység típusa. Ez is, miszerint manager feladata JobManager kell vagy a "User" jelezve, hogy ez nem egy kezelő feladat. Ez az esemény nem kibocsátott feladat előkészítő feladatok, a feladat kiadása feladatok vagy a start feladatokhoz.|
|systemTaskVersion|Int32|Ez egy, a belső újrapróbálkozási számláló meg olyan feladatra. A Batch szolgáltatás belső újra egy feladatot, hogy figyelembe vegye az átmeneti problémákat. A hibák például belső ütemezési hibák, vagy megpróbálja helyreállítani a számítási csomópontok hibás állapotban.|
|[nodeInfo](#nodeInfo)|Összetett típus|A számítási csomópont, amelyen a feladat futott adatait tartalmazza.|
|[multiInstanceSettings](#multiInstanceSettings)|Összetett típus|Megadja, hogy a feladat több számítási csomópont igénylő többpéldányos feladat.  Lásd: [multiInstanceSettings](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-task) részleteiről.|
|[megkötések](#constraints)|Összetett típus|Ez a feladat vonatkozó végrehajtási korlátozások.|
|[executionInfo](#executionInfo)|Összetett típus|A feladat végrehajtásának adatait tartalmazza.|

###  <a name="nodeInfo"></a>nodeInfo

|Elem neve|Típus|Megjegyzések|
|------------------|----------|-----------|
|poolId|Karakterlánc|A készlet, amelyen a feladat futott azonosítója.|
|nodeId|Karakterlánc|A csomópont, amelyen a feladat futott azonosítója.|

###  <a name="multiInstanceSettings"></a>multiInstanceSettings

|Elem neve|Típus|Megjegyzések|
|------------------|----------|-----------|
|numberOfInstances|Int32|A tevékenységhez szükséges számítási csomópontok száma.|

###  <a name="constraints"></a>megkötések

|Elem neve|Típus|Megjegyzések|
|------------------|----------|-----------|
|maxTaskRetryCount|Int32|A maximális száma a próbálhassa. A Batch szolgáltatás feladat újrapróbálkozik, ha annak kilépési kódja nem nulla.<br /><br /> Vegye figyelembe, hogy ez az érték kifejezetten határozza meg az újbóli próbálkozások számát. A Batch szolgáltatás egyszer megpróbál-e a feladatot, és előfordulhat, hogy ismételje meg ezt a határt legfeljebb. Például ha az újrapróbálkozások maximális száma 3, a feladat kötegelt záma legfeljebb 4 alkalommal (egy kezdeti próbálja és 3 újrapróbálás).<br /><br /> Ha az újrapróbálkozások maximális száma 0, a Batch szolgáltatás nem próbálja meg újra feladatok.<br /><br /> Ha az újrapróbálkozások maximális száma -1, a Batch szolgáltatás korlátozás nélkül feladatok után ismét.<br /><br /> Az alapértelmezett érték: 0 (nincs újrapróbálás).|


###  <a name="executionInfo"></a>executionInfo

|Elem neve|Típus|Megjegyzések|
|------------------|----------|-----------|
|startTime|Dátum és idő|A feladat kezdésének ideje futó. "Fut" megfelel-e a **futtató** állapotba kerül, így ha a feladat Erőforrásfájlok vagy alkalmazásokat ad meg, kezdési idejét tükrözi a indításakor, amelyen a feladat letöltése vagy telepítése ezeket.  Ha a feladat újraindítása, vagy a rendszer megpróbálja újból végrehajtani, akkor a legutóbbi feladat kezdésének ideje futó.|
|Befejezés időpontja|Dátum és idő|Az az idő, amelyen a feladat befejeződött.|
|exitCode|Int32|A feladat kilépési kódot.|
|a retryCount|Int32|A száma a feladat próbálkozásainak már elérte a Batch szolgáltatás. A rendszer megpróbálja újból végrehajtani a feladatot, ha egy nem nulla kilépési kód, mely legfeljebb a megadott MaxTaskRetryCount kilép.|
|requeueCount|Int32|A száma a feladat egy felhasználói kérelem miatt van a Batch szolgáltatás által lett helyezte.<br /><br /> Amikor a felhasználó eltávolítja csomópontok (az átméretezés, vagy a készlet méretének csökkentése) egy készletet vagy ha a feladat letiltódik, a felhasználó használatával adja meg, hogy fut a csomópontokon feladatokat kell helyezte a végrehajtáshoz. A számláló értéke ezen okok miatt a feladat rendelkezik lett helyezte hány alkalommal követi nyomon.|
