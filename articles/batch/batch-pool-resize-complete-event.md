---
title: "AAA \"Azure Batch-készlet átméretezése befejeződésének eseményét. |} Microsoft dokumentumok\""
description: "Útmutató a Batch-készlet átméretezése befejeződésének eseményét."
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
ms.openlocfilehash: dc64711a01aa4cf6192edba1a2c4cad56f953766
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="pool-resize-complete-event"></a>Készlet átméretezési befejeződésének eseményét.

 Ez az esemény is ki lesz adva a készlet átméretezési rendelkezik befejezése után, illetve nem sikerült.

 hello következő példa bemutatja hello törzsét egy alkalmazáskészlet átméretezési befejeződésének eseményét. egy készlet mérete megnövekedett, és sikeresen befejeződött.

```
{
    "id": "p_1_0_01503750-252d-4e57-bd96-d6aa05601ad8",
    "nodeDeallocationOption": "invalid",
    "currentDedicated": 4,
    "targetDedicated": 4,
    "enableAutoScale": false,
    "isAutoPool": false,
    "startTime": "2016-09-09T22:13:06.573Z",
    "endTime": "2016-09-09T22:14:01.727Z",
    "result": "Success",
    "resizeError": "hello operation succeeded"
}
```

|Elem|Típus|Megjegyzések|
|-------------|----------|-----------|
|id|Karakterlánc|hello készlet hello azonosítója.|
|nodeDeallocationOption|Karakterlánc|Itt adhatja meg, ha lehetséges, hogy lehet csomópontokat eltávolítani hello készletből, ha hello készlet méretének csökkentése.<br /><br /> Lehetséges értékek:<br /><br /> **requeue** – leállítja a futó tevékenységeket, és újra a várólistába helyezi őket. hello feladatok hello feladat engedélyezésekor fognak újra futni. Tevékenységek leállítása után rögtön csomópontjának eltávolítására.<br /><br /> **Állítsa le** – az éppen futó feladatok megszakítását. hello tevékenységeket nem futtatja újra. Tevékenységek leállítása után rögtön csomópontjának eltávolítására.<br /><br /> **taskcompletion** – jelenleg futó feladatok toocomplete engedélyezése. Nem ütemez újabb tevékenységeket való várakozás során. Távolítsa el a csomópontok, ha minden feladat befejeződött.<br /><br /> **Retaineddata** – lehetővé teszi a futó feladatok toocomplete, majd várja meg, hogy minden tevékenység adatok megőrzési időszak tooexpire. Nem ütemez újabb tevékenységeket való várakozás során. Csomópontjának eltávolítására, ha az összes feladat megőrzési időszak lejárt.<br /><br /> hello alapértelmezett értéke requeue.<br /><br /> Ha hello mérete növekszik, akkor hello értéke túl**érvénytelen**.|
|currentDedicated|Int32|számítási csomópontok száma hello rendelve toohello készlet.|
|targetDedicated|Int32|számítási csomópontok hello készlet igényelt hello száma.|
|enableAutoScale|logikai érték|Meghatározza, hogy hello mérete automatikusan igazodni adott idő alatt.|
|isAutoPool|logikai érték|Meghatározza, hogy hello készlet hozott-e a feladat AutoPool mechanizmus révén.|
|startTime|Dátum és idő|hello hello készlet átméretezése indulásakor.|
|endTime|Dátum és idő|hello hello készlet átméretezése idő befejeződött.|
|ResultCode|Karakterlánc|Automatikus oszlopszélesség hello hello eredményét.|
|resultMessage|Karakterlánc|hello átméretezési hiba tartalmazza a hello hello eredménye.<br /><br /> Ha hello átméretezése sikeresen befejeződött, a művelet sikerült hello állapotok.|
