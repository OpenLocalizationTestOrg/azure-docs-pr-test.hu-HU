---
title: "a feladat előrehaladását leltár szerint feladatokat állapot - Azure Batch szerint aaaMonitor |} Microsoft Docs"
description: "Hello előrehaladását a feladatok figyelése meghívásával hello beolvasása feladat száma művelet toocount feladatok feladat. Egy aktív, futó, és befejezett feladatok, és rendelkezik a sikeres és sikertelen feladatok száma kérheti le."
services: batch
author: tamram
manager: timlt
ms.service: batch
ms.topic: article
ms.date: 08/02/2017
ms.author: tamram
ms.openlocfilehash: 03957d8a3d678bf44587f3bc7f988a76885c2af0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="count-tasks-by-state-toomonitor-a-jobs-progress-preview"></a>Feladatok száma állapot toomonitor által a feladat előrehaladását (előzetes verzió)

Az Azure Batch szolgáltatás egy hatékony módját toomonitor hello folyamatban van egy feladat, a feladatok futása. Hello hívása [beolvasása feladat száma] [ rest_get_task_counts] művelet toofind hány feladatok egy aktív, futó vagy befejezett állapotban vannak, és hány van sikeres vagy sikertelen volt. Az egyes feladatok hello száma alapján, több egyszerűen hello feladat folyamatban tooa felhasználói megjelenítéséhez, vagy nem várt késedelmeket vagy hibák, amelyek befolyásolhatják a hello feladat.

> [!IMPORTANT]
> hello beolvasása feladat száma művelet jelenleg előzetes verzióban érhetők, és még nem érhető el az Azure Government, Azure Kína és németországi Azure. 
>
>

## <a name="how-tasks-are-counted"></a>Hogyan feladatok számítanak.

hello beolvasása feladat száma művelet visszajelzését feladatok állam, az alábbiak szerint:

- A feladatütemezés akkor számít **aktív** amikor aszinkron és képes toorun, de jelenleg nincs hozzárendelve tooa számítási csomópont. Egy feladat is számít **aktív** Ha még nem fejeződött be szülő feladat függ. Feladat függőségekkel kapcsolatos további információkért lásd: [létrehozása feladat függőségek toorun feladatok, más feladatok függő](batch-task-dependencies.md). 
- A feladatütemezés akkor számít **futtató** amikor tooa számítási csomópont lett rendelve, de még nem fejeződött be. Egy feladat számít **futtató** állapotában esetén vagy `preparing` vagy `running`, ahogy azt hello [feladat adatainak beolvasása] [ rest_get_task] művelet.
- A feladatütemezés akkor számít **befejeződött** már nem jogosult toorun. Egy feladat számít **befejeződött** még általában fejeződött be sikeresen, vagy sikertelenül befejeződött, és a is kimerítette az Újrapróbálkozási korlát. 

hello beolvasása feladat száma műveletet is jelent, hány feladatok sikeres vagy sikertelen volt. Kötegelt határozza meg, hogy a feladat végrehajtása sikeres vagy hello ellenőrzése sikertelen **eredmény** tulajdonsága hello [executionInfo] [https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-task#executionInfo] tulajdonság:

    - A feladatütemezés akkor számít **sikeres** Ha hello feladat végrehajtásának eredménye `success`.
    - A feladatütemezés akkor számít **sikertelen** Ha hello feladat végrehajtásának eredménye `failure`.

További információ a feladat állapota: [feladat adatainak beolvasása][rest_get_task].

hello következő .NET mintakód bemutatja, hogyan tooretrieve feladat állapota száma: 

```csharp
var taskCounts = await batchClient.JobOperations.GetTaskCountsAsync("job-1");

Console.WriteLine("Task count in active state: {0}", taskCounts.Active);
Console.WriteLine("Task count in preparing or running state: {0}", taskCounts.Running);
Console.WriteLine("Task count in completed state: {0}", taskCounts.Completed);
Console.WriteLine("Succeeded task count: {0}", taskCounts.Succeeded);
Console.WriteLine("Failed task count: {0}", taskCounts.Failed);
Console.WriteLine("ValidationStatus: {0}", taskCounts.ValidationStatus);
```

> [!NOTE]
> A többi hasonló mintát és egyéb támogatott nyelvek tooget feladatot a feladatok száma is használhatja. 
> 
> 

## <a name="consistency-checking-for-task-counts"></a>Konzisztencia-ellenőrzés a feladat összesítése

hello Batch szolgáltatás feladat számát összesíti az adatokat gyűjt több részből aszinkron elosztott rendszer által. tooensure, hogy a feladat száma megfelelőek-e, Batch szolgáltatás további ellenőrzést, az állapot száma több összetevő hello rendszer ellen konzisztencia-ellenőrzést végez. Kötegelt ezek konzisztencia-ellenőrzéseket is végez, mindaddig, amíg nincsenek kevesebb, mint 200 000 feladatok hello feladat. Hello valószínű esemény hello konzisztencia-ellenőrzés hibákat talál, a kötegelt javítja hello feladatok száma Get művelet hello hello konzisztencia-ellenőrzés eredménye alapján hello eredményét. hello konzisztencia-ellenőrzés egy külön intézkedés tooensure, hogy az ügyfelek, akik hello beolvasása feladat száma művelet támaszkodnak hello a megoldáshoz szükséges megfelelő információt lekérni.

Hello **validationStatus** hello válaszként a tulajdonság azt jelzi, hogy kötegelt hello konzisztencia-ellenőrzés hajtott végre. Ha a köteg nem tud toocheck állapot számát hello tényleges állapotok hello rendszerben tárolt ellen, majd hello **validationStatus** tulajdonsága túl`unvalidated`. A megfelelő teljesítmény érdekében kötegelt nem hajt végre, ha hello feladatokkal hello konzisztencia-ellenőrzés több mint 200 000 feladatok, így hello **validationStatus** tulajdonság túl lehet beállítani`unvalidated` ebben az esetben. Hello feladatok száma azonban nem feltétlenül megfelelő ebben az esetben, még a nagyon kevés adatvesztés nagyon valószínű. 

A feladatok állapota, ha hello összesítési csővezeték dolgozza fel a hello módosítása néhány másodpercen belül. hello beolvasása feladat száma művelet tükrözi hello frissítése feladat számát adott időszakon belül. Azonban hello összesítési csővezeték gyorsítótárbeli sikertelen keresések egy feladat állapotának változását, ha majd, hogy módosítás nem regisztrálva van amíg hello következő ellenőrzési fázisban. Ebben az időszakban feladat számát toohello kimaradt esemény miatt némileg pontatlan lehet, de azok a hello tovább érvényesítési fázist kijavítására sor.

## <a name="best-practices-for-counting-a-jobs-tasks"></a>A feladat tevékenységeit számbavételi ajánlott eljárásai

Hello beolvasása feladat száma művelet hívása hello leghatékonyabb módon tooreturn egy feladat feladatokat állapot szerint alapvető számát. Kötegelt verziójú 2017-06-01.5.1 használatakor javasoljuk írása vagy frissítésekor. a kód toouse beolvasása feladat száma.

hello beolvasása feladat száma művelet nem érhető el a Batch szolgáltatás verziókban 2017-06-01.5.1 verziójánál. Ha hello szolgáltatást egy régebbi verzióját használja, majd használja egy lekérdezés toocount tevékenységei feladatokban. További információkért lásd: [lekérdezések létrehozása toolist kötegelt erőforrások hatékony](batch-efficient-list-queries.md).

## <a name="next-steps"></a>Következő lépések

* Lásd: hello [Batch funkcióinak áttekintése](batch-api-basics.md) toolearn további kötegelt szolgáltatással kapcsolatos fogalmak és szolgáltatásokkal kapcsolatban. hello cikk hello elsődleges kötegelt erőforrások, például a készletek, a számítási csomópontokat, a feladatok és a feladatokat ismerteti, és egy hello szolgáltatás funkcióinak áttekintése.
* A Batch-kompatibilis alkalmazások hello használata fejlődő hello alapvető [Batch .NET ügyféloldali kódtár](batch-dotnet-get-started.md) vagy [Python](batch-python-tutorial.md). A bevezető cikkeiben végigvezeti Önt egy működő alkalmazást, amely hello Batch szolgáltatás tooexecute használja a munkaterhelés több számítási csomóponton.


[rest_get_task_counts]: https://docs.microsoft.com/rest/api/batchservice/get-the-task-counts-for-a-job
[rest_get_task]: https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-task
[rest_list_tasks]: https://docs.microsoft.com/rest/api/batchservice/list-the-tasks-associated-with-a-job
