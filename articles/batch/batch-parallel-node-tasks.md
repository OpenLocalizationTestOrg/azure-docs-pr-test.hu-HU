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
# <a name="run-tasks-concurrently-toomaximize-usage-of-batch-compute-nodes"></a><span data-ttu-id="395a8-103">Egyidejűleg toomaximize használati Batch számítási csomópontok feladatok futtatása</span><span class="sxs-lookup"><span data-stu-id="395a8-103">Run tasks concurrently toomaximize usage of Batch compute nodes</span></span> 

<span data-ttu-id="395a8-104">Az Azure Batch-készlet egyes számítási csomópontjain egyidejűleg futtatja a egynél több feladat, erőforrás-használat a csomópontok hello készletben kevesebb maximalizálása érdekében.</span><span class="sxs-lookup"><span data-stu-id="395a8-104">By running more than one task simultaneously on each compute node in your Azure Batch pool, you can maximize resource usage on a smaller number of nodes in hello pool.</span></span> <span data-ttu-id="395a8-105">Bizonyos munkaterhelések esetén ez eredményezhet rövidebb feladat idővel és az alacsonyabb költségek.</span><span class="sxs-lookup"><span data-stu-id="395a8-105">For some workloads, this can result in shorter job times and lower cost.</span></span>

<span data-ttu-id="395a8-106">Bizonyos esetekben az, hogy az összes csomópont erőforrások tooa egyetlen feladat előnyeit, miközben Számos esetben így több feladatok tooshare ezeket az erőforrásokat előnyei:</span><span class="sxs-lookup"><span data-stu-id="395a8-106">While some scenarios benefit from dedicating all of a node's resources tooa single task, several situations benefit from allowing multiple tasks tooshare those resources:</span></span>

* <span data-ttu-id="395a8-107">**Minimalizálja a adatátvitel** amikor feladatokat is képes tooshare adatokat.</span><span class="sxs-lookup"><span data-stu-id="395a8-107">**Minimizing data transfer** when tasks are able tooshare data.</span></span> <span data-ttu-id="395a8-108">Ebben a forgatókönyvben jelentősen csökkentheti adatok adatátviteli díjakkal másolja a megosztott adatok tooa kisebb csomópontok száma és a feladatok végrehajtása minden egyes csomóponton párhuzamosan.</span><span class="sxs-lookup"><span data-stu-id="395a8-108">In this scenario, you can dramatically reduce data transfer charges by copying shared data tooa smaller number of nodes and executing tasks in parallel on each node.</span></span> <span data-ttu-id="395a8-109">Ez különösen igaz, ha a másolt tooeach toobe hello adatcsomóponton át kell vinni a földrajzi régiók között.</span><span class="sxs-lookup"><span data-stu-id="395a8-109">This especially applies if hello data toobe copied tooeach node must be transferred between geographic regions.</span></span>
* <span data-ttu-id="395a8-110">**Memóriahasználat maximalizálva** olvasható feladatokhoz szükséges, amikor nagy mennyiségű memóriát, de csak időszakokban rövid idő, valamint a változó időpontokban végrehajtása során.</span><span class="sxs-lookup"><span data-stu-id="395a8-110">**Maximizing memory usage** when tasks require a large amount of memory, but only during short periods of time, and at variable times during execution.</span></span> <span data-ttu-id="395a8-111">Kevesebb, de nagyobb alkalmaz, a számítási csomópontokat a további memória tooefficiently kezelni ilyen teljesítményt.</span><span class="sxs-lookup"><span data-stu-id="395a8-111">You can employ fewer, but larger, compute nodes with more memory tooefficiently handle such spikes.</span></span> <span data-ttu-id="395a8-112">Ezek a csomópontok kellene több feladat minden egyes csomóponton párhuzamosan futó, de minden feladat időt vesz igénybe hello csomópontok címtér memória különböző időpontokban.</span><span class="sxs-lookup"><span data-stu-id="395a8-112">These nodes would have multiple tasks running in parallel on each node, but each task would take advantage of hello nodes' plentiful memory at different times.</span></span>
* <span data-ttu-id="395a8-113">**Csomópont számú korlátok kiküszöböléséhez** csomópontok közötti kommunikáció esetén kötelező a készlet belül.</span><span class="sxs-lookup"><span data-stu-id="395a8-113">**Mitigating node number limits** when inter-node communication is required within a pool.</span></span> <span data-ttu-id="395a8-114">Csomópontok közötti kommunikációra konfigurálva készletek jelenleg korlátozott too50 számítási csomópontok.</span><span class="sxs-lookup"><span data-stu-id="395a8-114">Currently, pools configured for inter-node communication are limited too50 compute nodes.</span></span> <span data-ttu-id="395a8-115">Ha ilyen készlet minden egyes csomópontja képes tooexecute feladatok párhuzamosan, nagyobb számos feladatot egyszerre hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="395a8-115">If each node in such a pool is able tooexecute tasks in parallel, a greater number of tasks can be executed simultaneously.</span></span>
* <span data-ttu-id="395a8-116">**Egy a helyszíni számítási fürt replikálása**, például amikor először helyezi át a számítási környezet tooAzure.</span><span class="sxs-lookup"><span data-stu-id="395a8-116">**Replicating an on-premises compute cluster**, such as when you first move a compute environment tooAzure.</span></span> <span data-ttu-id="395a8-117">Ha a jelenlegi helyszíni megoldás egyes számítási csomópontjain több feladat végrehajtása során, megnövelheti hello maximális számát a csomópont feladatok toomore szorosan tükrözik, hogy a konfigurálás.</span><span class="sxs-lookup"><span data-stu-id="395a8-117">If your current on-premises solution executes multiple tasks per compute node, you can increase hello maximum number of node tasks toomore closely mirror that configuration.</span></span>

## <a name="example-scenario"></a><span data-ttu-id="395a8-118">Példa</span><span class="sxs-lookup"><span data-stu-id="395a8-118">Example scenario</span></span>
<span data-ttu-id="395a8-119">Egy példa tooillustrate, hello párhuzamos feladat a végrehajtás előnyeit, tegyük fel, hogy a feladat alkalmazás rendelkezik-e a CPU és memória úgy, hogy [szabványos\_D1](../cloud-services/cloud-services-sizes-specs.md) elegendőek csomópontjai.</span><span class="sxs-lookup"><span data-stu-id="395a8-119">As an example tooillustrate hello benefits of parallel task execution, let's say that your task application has CPU and memory requirements such that [Standard\_D1](../cloud-services/cloud-services-sizes-specs.md) nodes are sufficient.</span></span> <span data-ttu-id="395a8-120">De rendelés toofinish hello feladat hello szükséges idő, a csomópontok 1000 van szükség.</span><span class="sxs-lookup"><span data-stu-id="395a8-120">But, in order toofinish hello job in hello required time, 1,000 of these nodes are needed.</span></span>

<span data-ttu-id="395a8-121">A szabványos helyett\_D1 csomópontok 1 Processzormagok, használhat [szabványos\_D14](../cloud-services/cloud-services-sizes-specs.md) csomópontokat, amelyek 16 mag, és engedélyezze a párhuzamos feladat a végrehajtás.</span><span class="sxs-lookup"><span data-stu-id="395a8-121">Instead of using Standard\_D1 nodes that have 1 CPU core, you could use [Standard\_D14](../cloud-services/cloud-services-sizes-specs.md) nodes that have 16 cores each, and enable parallel task execution.</span></span> <span data-ttu-id="395a8-122">Ezért *16-szer kevesebb csomópontok* használható--1000 csomópont helyett csak 63 lenne szükséges.</span><span class="sxs-lookup"><span data-stu-id="395a8-122">Therefore, *16 times fewer nodes* could be used--instead of 1,000 nodes, only 63 would be required.</span></span> <span data-ttu-id="395a8-123">Továbbá, ha nagy alkalmazásfájlok vagy referenciaadatok szükség minden csomóponton, feladat időtartama és hatékonyságát újra növelése mivel hello adatok másolt tooonly 16 csomóponttal.</span><span class="sxs-lookup"><span data-stu-id="395a8-123">Additionally, if large application files or reference data are required for each node, job duration and efficiency are again improved since hello data is copied tooonly 16 nodes.</span></span>

## <a name="enable-parallel-task-execution"></a><span data-ttu-id="395a8-124">A párhuzamos feladat a végrehajtás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="395a8-124">Enable parallel task execution</span></span>
<span data-ttu-id="395a8-125">A párhuzamos végrehajtásához a számítási csomópontok hello készlet szinten konfigurálnia.</span><span class="sxs-lookup"><span data-stu-id="395a8-125">You configure compute nodes for parallel task execution at hello pool level.</span></span> <span data-ttu-id="395a8-126">A hello Batch .NET library, állítsa be a hello [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] tulajdonságot, ha a készletet hoz létre.</span><span class="sxs-lookup"><span data-stu-id="395a8-126">With hello Batch .NET library, set hello [CloudPool.MaxTasksPerComputeNode][maxtasks_net] property when you create a pool.</span></span> <span data-ttu-id="395a8-127">Ha hello Batch REST API-t használ, állítsa be a hello [maxTasksPerNode] [ rest_addpool] elem hello kérés törzsében készlet létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="395a8-127">If you are using hello Batch REST API, set hello [maxTasksPerNode][rest_addpool] element in hello request body during pool creation.</span></span>

<span data-ttu-id="395a8-128">Az Azure Batch lehetővé teszi tooset maximális tevékenységek maximális száma mentése toofour alkalommal (4 x) hello csomópont magok száma.</span><span class="sxs-lookup"><span data-stu-id="395a8-128">Azure Batch allows you tooset maximum tasks per node up toofour times (4x) hello number of node cores.</span></span> <span data-ttu-id="395a8-129">Például ha hello címkészlet konfigurálva a csomópont méretű "Nagy" (négy magok), majd `maxTasksPerNode` too16 lehet beállítani.</span><span class="sxs-lookup"><span data-stu-id="395a8-129">For example, if hello pool is configured with nodes of size "Large" (four cores), then `maxTasksPerNode` may be set too16.</span></span> <span data-ttu-id="395a8-130">Az egyes hello csomópont méretű magok száma hello a részletekért lásd: [Felhőszolgáltatások mérete](../cloud-services/cloud-services-sizes-specs.md).</span><span class="sxs-lookup"><span data-stu-id="395a8-130">For details on hello number of cores for each of hello node sizes, see [Sizes for Cloud Services](../cloud-services/cloud-services-sizes-specs.md).</span></span> <span data-ttu-id="395a8-131">További információ a szolgáltatásra vonatkozó korlátozások: [kvótái és korlátai hello Azure Batch szolgáltatás](batch-quota-limit.md).</span><span class="sxs-lookup"><span data-stu-id="395a8-131">For more information on service limits, see [Quotas and limits for hello Azure Batch service](batch-quota-limit.md).</span></span>

> [!TIP]
> <span data-ttu-id="395a8-132">A fiók hello meg arról, hogy tootake kell `maxTasksPerNode` érték-hoz egy [automatikus skálázás képlet] [ enable_autoscaling] a készlethez.</span><span class="sxs-lookup"><span data-stu-id="395a8-132">Be sure tootake into account hello `maxTasksPerNode` value when you construct an [autoscale formula][enable_autoscaling] for your pool.</span></span> <span data-ttu-id="395a8-133">Például megadó képlet `$RunningTasks` jelentős mértékben befolyásolhatja, megnövelheti a tevékenységek maximális száma.</span><span class="sxs-lookup"><span data-stu-id="395a8-133">For example, a formula that evaluates `$RunningTasks` could be dramatically affected by an increase in tasks per node.</span></span> <span data-ttu-id="395a8-134">Lásd: [automatikus méretezési számítási csomópontok az Azure Batch-készlet](batch-automatic-scaling.md) további információt.</span><span class="sxs-lookup"><span data-stu-id="395a8-134">See [Automatically scale compute nodes in an Azure Batch pool](batch-automatic-scaling.md) for more information.</span></span>
>
>

## <a name="distribution-of-tasks"></a><span data-ttu-id="395a8-135">Tevékenységek eloszlása</span><span class="sxs-lookup"><span data-stu-id="395a8-135">Distribution of tasks</span></span>
<span data-ttu-id="395a8-136">A készlet számítási csomópontjai hello egyidejűleg végrehajtható feladatok, esetén fontos toospecify hello feladatok toobe hello készletben hello csomópontjai között elosztott módját.</span><span class="sxs-lookup"><span data-stu-id="395a8-136">When hello compute nodes in a pool can execute tasks concurrently, it's important toospecify how you want hello tasks toobe distributed across hello nodes in hello pool.</span></span>

<span data-ttu-id="395a8-137">Hello segítségével [CloudPool.TaskSchedulingPolicy] [ task_schedule] tulajdonság, megadhatja, hogy feladatok egyenletes legyen hozzárendelve hello készlet ("terjednek") az összes csomópont.</span><span class="sxs-lookup"><span data-stu-id="395a8-137">By using hello [CloudPool.TaskSchedulingPolicy][task_schedule] property, you can specify that tasks should be assigned evenly across all nodes in hello pool ("spreading").</span></span> <span data-ttu-id="395a8-138">Vagy megadhatja, hogy a lehető legtöbb feladatok hozzá kell rendelni tooeach csomópont előtt feladatok rendelt tooanother csomópont hello készlet ("csomagolási").</span><span class="sxs-lookup"><span data-stu-id="395a8-138">Or you can specify that as many tasks as possible should be assigned tooeach node before tasks are assigned tooanother node in hello pool ("packing").</span></span>

<span data-ttu-id="395a8-139">Például hogyan Ez a szolgáltatás akkor hasznos, fontolja meg a hello készlete [szabványos\_D14](../cloud-services/cloud-services-sizes-specs.md) csomópontok (a fenti hello példában), amelynek része a [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] 16-os értéket.</span><span class="sxs-lookup"><span data-stu-id="395a8-139">As an example of how this feature is valuable, consider hello pool of [Standard\_D14](../cloud-services/cloud-services-sizes-specs.md) nodes (in hello example above) that is configured with a [CloudPool.MaxTasksPerComputeNode][maxtasks_net] value of 16.</span></span> <span data-ttu-id="395a8-140">Ha hello [CloudPool.TaskSchedulingPolicy] [ task_schedule] van konfigurálva egy [ComputeNodeFillType] [ fill_type] a *csomag*, azt ehhez maximalizálhatja használatát az egyes csomópontok összes 16 maggal és engedélyezése egy [automatikus skálázás készlet](batch-automatic-scaling.md) tooprune nem használt csomópontok hello készletből (csomópontok nélkül bármely feladatok hozzárendelve).</span><span class="sxs-lookup"><span data-stu-id="395a8-140">If hello [CloudPool.TaskSchedulingPolicy][task_schedule] is configured with a [ComputeNodeFillType][fill_type] of *Pack*, it would maximize usage of all 16 cores of each node and allow an [autoscaling pool](batch-automatic-scaling.md) tooprune unused nodes from hello pool (nodes without any tasks assigned).</span></span> <span data-ttu-id="395a8-141">Ez minimalizálja az erőforrás-használatát, és menti a pénz.</span><span class="sxs-lookup"><span data-stu-id="395a8-141">This minimizes resource usage and saves money.</span></span>

## <a name="batch-net-example"></a><span data-ttu-id="395a8-142">Batch .NET – példa</span><span class="sxs-lookup"><span data-stu-id="395a8-142">Batch .NET example</span></span>
<span data-ttu-id="395a8-143">Ez [Batch .NET] [ api_net] API kódrészletet jeleníti meg a kérelem toocreate négy tevékenységek maximális száma legfeljebb négy csomópont nagy tartalmazó készlet.</span><span class="sxs-lookup"><span data-stu-id="395a8-143">This [Batch .NET][api_net] API code snippet shows a request toocreate a pool that contains four large nodes with a maximum of four tasks per node.</span></span> <span data-ttu-id="395a8-144">A Feladatütemező házirendet, amely minden csomópont, amelynek a feladatok előzetes tooassigning feladatok tooanother csomópontra hello készletben betelik határoz meg.</span><span class="sxs-lookup"><span data-stu-id="395a8-144">It specifies a task scheduling policy that will fill each node with tasks prior tooassigning tasks tooanother node in hello pool.</span></span> <span data-ttu-id="395a8-145">A készletek hozzáadása hello Batch .NET API használatával további információkért lásd: [BatchClient.PoolOperations.CreatePool][poolcreate_net].</span><span class="sxs-lookup"><span data-stu-id="395a8-145">For more information on adding pools by using hello Batch .NET API, see [BatchClient.PoolOperations.CreatePool][poolcreate_net].</span></span>

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

## <a name="batch-rest-example"></a><span data-ttu-id="395a8-146">Batch REST – példa</span><span class="sxs-lookup"><span data-stu-id="395a8-146">Batch REST example</span></span>
<span data-ttu-id="395a8-147">Ez [Batch REST] [ api_rest] API kódrészletben láthatja a kérelem toocreate négy tevékenységek maximális száma legfeljebb két nagy csomópontot tartalmazó készlet.</span><span class="sxs-lookup"><span data-stu-id="395a8-147">This [Batch REST][api_rest] API snippet shows a request toocreate a pool that contains two large nodes with a maximum of four tasks per node.</span></span> <span data-ttu-id="395a8-148">A készletek hozzáadása hello REST API használatával további információkért lásd: [tooan alkalmazáskészlet-fiók hozzáadása][rest_addpool].</span><span class="sxs-lookup"><span data-stu-id="395a8-148">For more information on adding pools by using hello REST API, see [Add a pool tooan account][rest_addpool].</span></span>

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
> <span data-ttu-id="395a8-149">Beállíthatja a hello `maxTasksPerNode` elem és [MaxTasksPerComputeNode] [ maxtasks_net] tulajdonság csak a készlet létrehozás időpontjában.</span><span class="sxs-lookup"><span data-stu-id="395a8-149">You can set hello `maxTasksPerNode` element and [MaxTasksPerComputeNode][maxtasks_net] property only at pool creation time.</span></span> <span data-ttu-id="395a8-150">Ezeket nem lehet módosítani a készlet létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="395a8-150">They cannot be modified after a pool has already been created.</span></span>
>
>

## <a name="code-sample"></a><span data-ttu-id="395a8-151">Kódminta</span><span class="sxs-lookup"><span data-stu-id="395a8-151">Code sample</span></span>
<span data-ttu-id="395a8-152">Hello [ParallelNodeTasks] [ parallel_tasks_sample] a Githubon projekt hello hello használatát mutatja be [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="395a8-152">hello [ParallelNodeTasks][parallel_tasks_sample] project on GitHub illustrates hello use of hello [CloudPool.MaxTasksPerComputeNode][maxtasks_net] property.</span></span>

<span data-ttu-id="395a8-153">A C# Konzolalkalmazás használ hello [Batch .NET] [ api_net] könyvtár toocreate egy vagy több készlet számítási csomópontjain.</span><span class="sxs-lookup"><span data-stu-id="395a8-153">This C# console application uses hello [Batch .NET][api_net] library toocreate a pool with one or more compute nodes.</span></span> <span data-ttu-id="395a8-154">Konfigurálható számos feladatot e csomópontok toosimulate változó betöltéskor hajtja végre.</span><span class="sxs-lookup"><span data-stu-id="395a8-154">It executes a configurable number of tasks on those nodes toosimulate variable load.</span></span> <span data-ttu-id="395a8-155">Hello alkalmazás határozza meg, mely csomópontok végrehajtott minden tevékenység.</span><span class="sxs-lookup"><span data-stu-id="395a8-155">Output from hello application specifies which nodes executed each task.</span></span> <span data-ttu-id="395a8-156">hello alkalmazás is hello feladat paramétereit és időtartama összegzését tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="395a8-156">hello application also provides a summary of hello job parameters and duration.</span></span> <span data-ttu-id="395a8-157">két különböző frissítési kísérletei során hello mintaalkalmazás hello kimenete összefoglaló része hello alatt jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="395a8-157">hello summary portion of hello output from two different runs of hello sample application appears below.</span></span>

```
Nodes: 1
Node size: large
Max tasks per node: 1
Tasks: 32
Duration: 00:30:01.4638023
```

<span data-ttu-id="395a8-158">hello mintaalkalmazás hello első végrehajtása tartalmazza, amely egyetlen csomópontjába hello készlet és hello alapértelmezett beállítását egy feladat, csomópontonként hello feladat időtartama van több mint 30 perc.</span><span class="sxs-lookup"><span data-stu-id="395a8-158">hello first execution of hello sample application shows that with a single node in hello pool and hello default setting of one task per node, hello job duration is over 30 minutes.</span></span>

```
Nodes: 1
Node size: large
Max tasks per node: 4
Tasks: 32
Duration: 00:08:48.2423500
```

<span data-ttu-id="395a8-159">hello jelentősen csökkentheti a feladat időtartama hello minta azt mutatja be a második futtatásához.</span><span class="sxs-lookup"><span data-stu-id="395a8-159">hello second run of hello sample shows a significant decrease in job duration.</span></span> <span data-ttu-id="395a8-160">Ennek az az oka hello készlet négy tevékenységek maximális száma, ami lehetővé teszi a párhuzamos tevékenység végrehajtási toocomplete hello feladat majdnem hello idő negyedévében lett konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="395a8-160">This is because hello pool was configured with four tasks per node, which allows for parallel task execution toocomplete hello job in nearly a quarter of hello time.</span></span>

> [!NOTE]
> <span data-ttu-id="395a8-161">a fenti hello összesítések hello feladat időtartamok nem tartalmaznak címkészlet létrehozásának ideje.</span><span class="sxs-lookup"><span data-stu-id="395a8-161">hello job durations in hello summaries above do not include pool creation time.</span></span> <span data-ttu-id="395a8-162">Egyes hello feladatokat a fenti létrehozott elküldött toopreviously készletek számítási csomópontjainak voltak hello lett *üresjáratban* állapot küldése során.</span><span class="sxs-lookup"><span data-stu-id="395a8-162">Each of hello jobs above was submitted toopreviously created pools whose compute nodes were in hello *Idle* state at submission time.</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="395a8-163">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="395a8-163">Next steps</span></span>
### <a name="batch-explorer-heat-map"></a><span data-ttu-id="395a8-164">Kötegelt Explorer Hőtérkép</span><span class="sxs-lookup"><span data-stu-id="395a8-164">Batch Explorer Heat Map</span></span>
<span data-ttu-id="395a8-165">Hello [Azure Batch Explorer][batch_explorer], hello Azure Batch egyik [mintaalkalmazást][github_samples], tartalmazza a *Hőtérkép* képi megjelenítés feladat végrehajtása a szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="395a8-165">hello [Azure Batch Explorer][batch_explorer], one of hello Azure Batch [sample applications][github_samples], contains a *Heat Map* feature that provides visualization of task execution.</span></span> <span data-ttu-id="395a8-166">Ha most végrehajtása hello [ParallelTasks] [ parallel_tasks_sample] mintaalkalmazást, használhatja hello Hőtérkép szolgáltatás tooeasily hello végrehajtása minden egyes csomóponton párhuzamos feladatok megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="395a8-166">When you're executing hello [ParallelTasks][parallel_tasks_sample] sample application, you can use hello Heat Map feature tooeasily visualize hello execution of parallel tasks on each node.</span></span>

![Kötegelt Explorer Hőtérkép][1]

<span data-ttu-id="395a8-168">*Kötegelt Explorer Hőtérkép megjelenítő négy csomópont révén az egyes csomópontok, jelenleg feldolgozás alatt álló négy feladatok készlete*</span><span class="sxs-lookup"><span data-stu-id="395a8-168">*Batch Explorer Heat Map showing a pool of four nodes, with each node currently executing four tasks*</span></span>

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
