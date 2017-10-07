---
title: "AAA \"Azure Batch feladat sikertelen esemény |} Microsoft dokumentumok\""
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
ms.openlocfilehash: e92604671650900072ba27f807501b704329e865
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="task-fail-event"></a>A feladat sikertelen esemény

 Ez az esemény kibocsátott hibával feladat befejezését. Jelenleg minden nem nulla kilépési kód minősülnek hibák. Ezt az eseményt a rendszer kibocsátott *kívül* feladat esemény befejeződik, és a használt toodetect lehet, ha a feladat végrehajtása sikertelen.


 hello következő példa bemutatja hello törzsét egy feladat sikertelen esemény.

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
|A JobId értékének|Karakterlánc|hello tevékenységet tartalmazó hello feladat hello azonosítóját.|
|id|Karakterlánc|hello feladat hello azonosítója.|
|taskType|Karakterlánc|hello hello feladat típusát. Ez is, miszerint manager feladata JobManager kell vagy a "User" jelezve, hogy ez nem egy kezelő feladat. Ez az esemény nem kibocsátott feladat előkészítő feladatok, a feladat kiadása feladatok vagy a start feladatokhoz.|
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
|numberOfInstances|Int32|számítási csomópontok hello feladat által igényelt hello száma.|

###  <a name="constraints"></a>megkötések

|Elem neve|Típus|Megjegyzések|
|------------------|----------|-----------|
|maxTaskRetryCount|Int32|hello maximálisan megengedett számú hello próbálhassa. hello Batch szolgáltatás feladat újrapróbálkozik, ha annak kilépési kódja nem nulla.<br /><br /> Vegye figyelembe, hogy ez az érték kifejezetten vezérlők hello az újrapróbálkozások számát. hello Batch szolgáltatás egyszer megpróbál hello feladat, és előfordulhat, hogy próbálja mentése toothis korlátot. Például ha hello újrapróbálkozások maximális száma: 3, a kötegelt próbálkozik too4 időpontokban (egy kezdeti próbálja és 3 újrapróbálás) mentése feladat.<br /><br /> Ha hello újrapróbálkozások maximális száma 0, hello Batch szolgáltatás nem próbálja meg újra feladatok.<br /><br /> Ha hello újrapróbálkozások maximális száma -1, a hello Batch szolgáltatás korlátozás nélkül feladatok újrapróbálkozik.<br /><br /> hello alapértelmezett értéke 0 (nincs újrapróbálás).|


###  <a name="executionInfo"></a>executionInfo

|Elem neve|Típus|Megjegyzések|
|------------------|----------|-----------|
|startTime|Dátum és idő|hello időpontot, mely indulása hello feladat. "Fut" megfelel a toohello **futtató** állapotba kerül, így hello feladat Erőforrásfájlok vagy alkalmazáscsomagok határozza meg, ha, hello kezdési ideje tükrözi hello időpontot, mely hello feladat letöltése vagy való telepítésének indítása.  Hello feladat újraindult, vagy a rendszer megpróbálja újból végrehajtani, ha ez az utolsó időpontot, mely hello feladat indulása hello.|
|endTime|Dátum és idő|hello időpontot, mely hello feladat befejeződött.|
|exitCode|Int32|hello kilépési kód hello feladat.|
|a retryCount|Int32|hello hello feladat próbálkozásainak már elérte a hello Batch szolgáltatás hányszor. a rendszer megpróbálja újból végrehajtani hello feladatot, ha kilép egy nem nulla kilépési kód mentése toohello megadott MaxTaskRetryCount.|
|requeueCount|Int32|hello hányszor hello feladat hello eredményként egy felhasználói kérelem rendelkezik hello Batch szolgáltatás által lett helyezte.<br /><br /> Amikor hello felhasználó eltávolítja az (az átméretezés vagy zsugorítását hello alkalmazáskészlet) egy készletet, vagy ha hello feladat letiltódik, hello felhasználói csomópontokat adhat meg, hogy a futó feladatok hello csomóponton kell helyezte a végrehajtáshoz. Ez a szám követi nyomon, hogy hányszor hello tevékenység rendelkezik lett helyezte ezen okok miatt.|
