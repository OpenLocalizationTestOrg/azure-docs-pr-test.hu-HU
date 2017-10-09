---
title: "a párhuzamos toouse aaaRun feladatok számítási erőforrások hatékony - Azure Batch |} Microsoft Docs"
description: "Növelje a hatékonyság és az alacsonyabb költségek Azure Batch-készlet minden egyes csomópontján kevesebb számítási csomópontok és futó egyidejű feladatok használatával"
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
ms.assetid: 538a067c-1f6e-44eb-a92b-8d51c33d3e1a
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 05/22/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 05df4b7d8e0bc595168a97faa231b7c90fe81980
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="run-tasks-concurrently-toomaximize-usage-of-batch-compute-nodes"></a>Egyidejűleg toomaximize használati Batch számítási csomópontok feladatok futtatása 

Az Azure Batch-készlet egyes számítási csomópontjain egyidejűleg futtatja a egynél több feladat, erőforrás-használat a csomópontok hello készletben kevesebb maximalizálása érdekében. Bizonyos munkaterhelések esetén ez eredményezhet rövidebb feladat idővel és az alacsonyabb költségek.

Bizonyos esetekben az, hogy az összes csomópont erőforrások tooa egyetlen feladat előnyeit, miközben Számos esetben így több feladatok tooshare ezeket az erőforrásokat előnyei:

* **Minimalizálja a adatátvitel** amikor feladatokat is képes tooshare adatokat. Ebben a forgatókönyvben jelentősen csökkentheti adatok adatátviteli díjakkal másolja a megosztott adatok tooa kisebb csomópontok száma és a feladatok végrehajtása minden egyes csomóponton párhuzamosan. Ez különösen igaz, ha a másolt tooeach toobe hello adatcsomóponton át kell vinni a földrajzi régiók között.
* **Memóriahasználat maximalizálva** olvasható feladatokhoz szükséges, amikor nagy mennyiségű memóriát, de csak időszakokban rövid idő, valamint a változó időpontokban végrehajtása során. Kevesebb, de nagyobb alkalmaz, a számítási csomópontokat a további memória tooefficiently kezelni ilyen teljesítményt. Ezek a csomópontok kellene több feladat minden egyes csomóponton párhuzamosan futó, de minden feladat időt vesz igénybe hello csomópontok címtér memória különböző időpontokban.
* **Csomópont számú korlátok kiküszöböléséhez** csomópontok közötti kommunikáció esetén kötelező a készlet belül. Csomópontok közötti kommunikációra konfigurálva készletek jelenleg korlátozott too50 számítási csomópontok. Ha ilyen készlet minden egyes csomópontja képes tooexecute feladatok párhuzamosan, nagyobb számos feladatot egyszerre hajtható végre.
* **Egy a helyszíni számítási fürt replikálása**, például amikor először helyezi át a számítási környezet tooAzure. Ha a jelenlegi helyszíni megoldás egyes számítási csomópontjain több feladat végrehajtása során, megnövelheti hello maximális számát a csomópont feladatok toomore szorosan tükrözik, hogy a konfigurálás.

## <a name="example-scenario"></a>Példa
Egy példa tooillustrate, hello párhuzamos feladat a végrehajtás előnyeit, tegyük fel, hogy a feladat alkalmazás rendelkezik-e a CPU és memória úgy, hogy [szabványos\_D1](../cloud-services/cloud-services-sizes-specs.md) elegendőek csomópontjai. De rendelés toofinish hello feladat hello szükséges idő, a csomópontok 1000 van szükség.

A szabványos helyett\_D1 csomópontok 1 Processzormagok, használhat [szabványos\_D14](../cloud-services/cloud-services-sizes-specs.md) csomópontokat, amelyek 16 mag, és engedélyezze a párhuzamos feladat a végrehajtás. Ezért *16-szer kevesebb csomópontok* használható--1000 csomópont helyett csak 63 lenne szükséges. Továbbá, ha nagy alkalmazásfájlok vagy referenciaadatok szükség minden csomóponton, feladat időtartama és hatékonyságát újra növelése mivel hello adatok másolt tooonly 16 csomóponttal.

## <a name="enable-parallel-task-execution"></a>A párhuzamos feladat a végrehajtás engedélyezése
A párhuzamos végrehajtásához a számítási csomópontok hello készlet szinten konfigurálnia. A hello Batch .NET library, állítsa be a hello [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] tulajdonságot, ha a készletet hoz létre. Ha hello Batch REST API-t használ, állítsa be a hello [maxTasksPerNode] [ rest_addpool] elem hello kérés törzsében készlet létrehozása során.

Az Azure Batch lehetővé teszi tooset maximális tevékenységek maximális száma mentése toofour alkalommal (4 x) hello csomópont magok száma. Például ha hello címkészlet konfigurálva a csomópont méretű "Nagy" (négy magok), majd `maxTasksPerNode` too16 lehet beállítani. Az egyes hello csomópont méretű magok száma hello a részletekért lásd: [Felhőszolgáltatások mérete](../cloud-services/cloud-services-sizes-specs.md). További információ a szolgáltatásra vonatkozó korlátozások: [kvótái és korlátai hello Azure Batch szolgáltatás](batch-quota-limit.md).

> [!TIP]
> A fiók hello meg arról, hogy tootake kell `maxTasksPerNode` érték-hoz egy [automatikus skálázás képlet] [ enable_autoscaling] a készlethez. Például megadó képlet `$RunningTasks` jelentős mértékben befolyásolhatja, megnövelheti a tevékenységek maximális száma. Lásd: [automatikus méretezési számítási csomópontok az Azure Batch-készlet](batch-automatic-scaling.md) további információt.
>
>

## <a name="distribution-of-tasks"></a>Tevékenységek eloszlása
A készlet számítási csomópontjai hello egyidejűleg végrehajtható feladatok, esetén fontos toospecify hello feladatok toobe hello készletben hello csomópontjai között elosztott módját.

Hello segítségével [CloudPool.TaskSchedulingPolicy] [ task_schedule] tulajdonság, megadhatja, hogy feladatok egyenletes legyen hozzárendelve hello készlet ("terjednek") az összes csomópont. Vagy megadhatja, hogy a lehető legtöbb feladatok hozzá kell rendelni tooeach csomópont előtt feladatok rendelt tooanother csomópont hello készlet ("csomagolási").

Például hogyan Ez a szolgáltatás akkor hasznos, fontolja meg a hello készlete [szabványos\_D14](../cloud-services/cloud-services-sizes-specs.md) csomópontok (a fenti hello példában), amelynek része a [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] 16-os értéket. Ha hello [CloudPool.TaskSchedulingPolicy] [ task_schedule] van konfigurálva egy [ComputeNodeFillType] [ fill_type] a *csomag*, azt ehhez maximalizálhatja használatát az egyes csomópontok összes 16 maggal és engedélyezése egy [automatikus skálázás készlet](batch-automatic-scaling.md) tooprune nem használt csomópontok hello készletből (csomópontok nélkül bármely feladatok hozzárendelve). Ez minimalizálja az erőforrás-használatát, és menti a pénz.

## <a name="batch-net-example"></a>Batch .NET – példa
Ez [Batch .NET] [ api_net] API kódrészletet jeleníti meg a kérelem toocreate négy tevékenységek maximális száma legfeljebb négy csomópont nagy tartalmazó készlet. A Feladatütemező házirendet, amely minden csomópont, amelynek a feladatok előzetes tooassigning feladatok tooanother csomópontra hello készletben betelik határoz meg. A készletek hozzáadása hello Batch .NET API használatával további információkért lásd: [BatchClient.PoolOperations.CreatePool][poolcreate_net].

```csharp
CloudPool pool =
    batchClient.PoolOperations.CreatePool(
        poolId: "mypool",
        targetDedicatedComputeNodes: 4
        virtualMachineSize: "large",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));

pool.MaxTasksPerComputeNode = 4;
pool.TaskSchedulingPolicy = new TaskSchedulingPolicy(ComputeNodeFillType.Pack);
pool.Commit();
```

## <a name="batch-rest-example"></a>Batch REST – példa
Ez [Batch REST] [ api_rest] API kódrészletben láthatja a kérelem toocreate négy tevékenységek maximális száma legfeljebb két nagy csomópontot tartalmazó készlet. A készletek hozzáadása hello REST API használatával további információkért lásd: [tooan alkalmazáskészlet-fiók hozzáadása][rest_addpool].

```json
{
  "odata.metadata":"https://myaccount.myregion.batch.azure.com/$metadata#pools/@Element",
  "id":"mypool",
  "vmSize":"large",
  "cloudServiceConfiguration": {
    "osFamily":"4",
    "targetOSVersion":"*",
  }
  "targetDedicatedComputeNodes":2,
  "maxTasksPerNode":4,
  "enableInterNodeCommunication":true,
}
```

> [!NOTE]
> Beállíthatja a hello `maxTasksPerNode` elem és [MaxTasksPerComputeNode] [ maxtasks_net] tulajdonság csak a készlet létrehozás időpontjában. Ezeket nem lehet módosítani a készlet létrehozása után.
>
>

## <a name="code-sample"></a>Kódminta
Hello [ParallelNodeTasks] [ parallel_tasks_sample] a Githubon projekt hello hello használatát mutatja be [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] tulajdonság.

A C# Konzolalkalmazás használ hello [Batch .NET] [ api_net] könyvtár toocreate egy vagy több készlet számítási csomópontjain. Konfigurálható számos feladatot e csomópontok toosimulate változó betöltéskor hajtja végre. Hello alkalmazás határozza meg, mely csomópontok végrehajtott minden tevékenység. hello alkalmazás is hello feladat paramétereit és időtartama összegzését tartalmazza. két különböző frissítési kísérletei során hello mintaalkalmazás hello kimenete összefoglaló része hello alatt jelenik meg.

```
Nodes: 1
Node size: large
Max tasks per node: 1
Tasks: 32
Duration: 00:30:01.4638023
```

hello mintaalkalmazás hello első végrehajtása tartalmazza, amely egyetlen csomópontjába hello készlet és hello alapértelmezett beállítását egy feladat, csomópontonként hello feladat időtartama van több mint 30 perc.

```
Nodes: 1
Node size: large
Max tasks per node: 4
Tasks: 32
Duration: 00:08:48.2423500
```

hello jelentősen csökkentheti a feladat időtartama hello minta azt mutatja be a második futtatásához. Ennek az az oka hello készlet négy tevékenységek maximális száma, ami lehetővé teszi a párhuzamos tevékenység végrehajtási toocomplete hello feladat majdnem hello idő negyedévében lett konfigurálva.

> [!NOTE]
> a fenti hello összesítések hello feladat időtartamok nem tartalmaznak címkészlet létrehozásának ideje. Egyes hello feladatokat a fenti létrehozott elküldött toopreviously készletek számítási csomópontjainak voltak hello lett *üresjáratban* állapot küldése során.
>
>

## <a name="next-steps"></a>Következő lépések
### <a name="batch-explorer-heat-map"></a>Kötegelt Explorer Hőtérkép
Hello [Azure Batch Explorer][batch_explorer], hello Azure Batch egyik [mintaalkalmazást][github_samples], tartalmazza a *Hőtérkép* képi megjelenítés feladat végrehajtása a szolgáltatás. Ha most végrehajtása hello [ParallelTasks] [ parallel_tasks_sample] mintaalkalmazást, használhatja hello Hőtérkép szolgáltatás tooeasily hello végrehajtása minden egyes csomóponton párhuzamos feladatok megjelenítése.

![Kötegelt Explorer Hőtérkép][1]

*Kötegelt Explorer Hőtérkép megjelenítő négy csomópont révén az egyes csomópontok, jelenleg feldolgozás alatt álló négy feladatok készlete*

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[enable_autoscaling]: https://msdn.microsoft.com/library/azure/dn820173.aspx
[fill_type]: https://msdn.microsoft.com/library/microsoft.azure.batch.common.computenodefilltype.aspx
[github_samples]: https://github.com/Azure/azure-batch-samples
[maxtasks_net]: http://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.maxtaskspercomputenode.aspx
[rest_addpool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[parallel_tasks_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/ParallelTasks
[poolcreate_net]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx
[task_schedule]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudpool.taskschedulingpolicy.aspx

[1]: ./media/batch-parallel-node-tasks\heat_map.png
