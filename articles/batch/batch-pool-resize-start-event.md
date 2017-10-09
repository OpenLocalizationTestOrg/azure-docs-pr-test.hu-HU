---
title: "AAA \"Azure Batch-készlet átméretezési esemény indítása |} Microsoft dokumentumok\""
description: "Útmutató a Batch-készlet átméretezési esemény indítása."
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
ms.openlocfilehash: 2ca2a4f1195c3f785ae5b051b63340f70eecbc22
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="pool-resize-start-event"></a>Készlet átméretezési start eseményét.

 Ez az esemény kibocsátott, amikor egy alkalmazáskészlet átméretezési megkezdődött. Mivel hello készlet átméretezési aszinkron esemény, számíthat egy készlet átméretezési befejeződésének eseményét toobe kibocsátott után hello átméretezése művelet befejeződik.

 a következő példa azt mutatja be egy alkalmazáskészlet átméretezéséhez 0 too2 csomópontjairól a kézi készlet átméretezési indító esemény törzsét hello hello átméretezése.

```
{
    "poolId": "myPool1",
    "nodeDeallocationOption": "invalid",
    "currentDedicated": 0,
    "targetDedicated": 2,
    "enableAutoScale": false,
    "isAutoPool": false
}
```

|Elem|Típus|Megjegyzések|
|-------------|----------|-----------|
|poolId|Karakterlánc|hello készlet hello azonosítója.|
|nodeDeallocationOption|Karakterlánc|Itt adhatja meg, ha lehetséges, hogy lehet csomópontokat eltávolítani hello készletből, ha hello készlet méretének csökkentése.<br /><br /> Lehetséges értékek:<br /><br /> **requeue** – leállítja a futó tevékenységeket, és újra a várólistába helyezi őket. hello feladatok hello feladat engedélyezésekor fognak újra futni. Tevékenységek leállítása után rögtön csomópontjának eltávolítására.<br /><br /> **Állítsa le** – az éppen futó feladatok megszakítását. hello tevékenységeket nem futtatja újra. Tevékenységek leállítása után rögtön csomópontjának eltávolítására.<br /><br /> **taskcompletion** – jelenleg futó feladatok toocomplete engedélyezése. Nem ütemez újabb tevékenységeket való várakozás során. Távolítsa el a csomópontok, ha minden feladat befejeződött.<br /><br /> **Retaineddata** – lehetővé teszi a futó feladatok toocomplete, majd várja meg, hogy minden tevékenység adatok megőrzési időszak tooexpire. Nem ütemez újabb tevékenységeket való várakozás során. Csomópontjának eltávolítására, ha az összes feladat megőrzési időszak lejárt.<br /><br /> hello alapértelmezett értéke requeue.<br /><br /> Ha hello mérete növekszik, akkor hello értéke túl**érvénytelen**.|
|currentDedicated|Int32|számítási csomópontok száma hello rendelve toohello készlet.|
|targetDedicated|Int32|számítási csomópontok hello készlet igényelt hello száma.|
|enableAutoScale|logikai érték|Meghatározza, hogy hello mérete automatikusan igazodni adott idő alatt.|
|isAutoPool|logikai érték|Speficies e hello készlet használatával egy feladat AutoPool mechanizmus jött létre.|
