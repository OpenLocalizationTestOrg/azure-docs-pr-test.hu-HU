---
title: "Azure Batch-készlet átméretezési esemény indítása |} Microsoft Docs"
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
ms.openlocfilehash: 826cd984d26b923ba38562e05a2e75c399be9121
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="pool-resize-start-event"></a><span data-ttu-id="c1e82-103">Készlet átméretezési start eseményét.</span><span class="sxs-lookup"><span data-stu-id="c1e82-103">Pool resize start event</span></span>

 <span data-ttu-id="c1e82-104">Ez az esemény kibocsátott, amikor egy alkalmazáskészlet átméretezési megkezdődött.</span><span class="sxs-lookup"><span data-stu-id="c1e82-104">This event is emitted when a pool resize has started.</span></span> <span data-ttu-id="c1e82-105">Mivel a készlet átméretezési aszinkron esemény, számíthat egy alkalmazáskészlet átméretezési teljes esemény kell kibocsátott az átméretezés befejeződése után.</span><span class="sxs-lookup"><span data-stu-id="c1e82-105">Since the pool resize is an asynchronous event, you can expect a pool resize complete event to be emitted once the resize operation completes.</span></span>

 <span data-ttu-id="c1e82-106">A következő példa bemutatja a készlet átméretezéséhez 0 2 csomópontokhoz egy manuális méretezze át a készlet átméretezési indító esemény törzsét.</span><span class="sxs-lookup"><span data-stu-id="c1e82-106">The following example shows the body of a pool resize start event for a pool resizing from 0 to 2 nodes with a manual resize.</span></span>

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

|<span data-ttu-id="c1e82-107">Elem</span><span class="sxs-lookup"><span data-stu-id="c1e82-107">Element</span></span>|<span data-ttu-id="c1e82-108">Típus</span><span class="sxs-lookup"><span data-stu-id="c1e82-108">Type</span></span>|<span data-ttu-id="c1e82-109">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="c1e82-109">Notes</span></span>|
|-------------|----------|-----------|
|<span data-ttu-id="c1e82-110">poolId</span><span class="sxs-lookup"><span data-stu-id="c1e82-110">poolId</span></span>|<span data-ttu-id="c1e82-111">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="c1e82-111">String</span></span>|<span data-ttu-id="c1e82-112">A készlet azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="c1e82-112">The id of the pool.</span></span>|
|<span data-ttu-id="c1e82-113">nodeDeallocationOption</span><span class="sxs-lookup"><span data-stu-id="c1e82-113">nodeDeallocationOption</span></span>|<span data-ttu-id="c1e82-114">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="c1e82-114">String</span></span>|<span data-ttu-id="c1e82-115">Itt adhatja meg, ha lehetséges, hogy lehet csomópontokat eltávolítani a készletből, ha a készlet méretének csökkentése.</span><span class="sxs-lookup"><span data-stu-id="c1e82-115">Specifies when nodes may be removed from the pool, if the pool size is decreasing.</span></span><br /><br /> <span data-ttu-id="c1e82-116">Lehetséges értékek:</span><span class="sxs-lookup"><span data-stu-id="c1e82-116">Possible values are:</span></span><br /><br /> <span data-ttu-id="c1e82-117">**requeue** – leállítja a futó tevékenységeket, és újra a várólistába helyezi őket.</span><span class="sxs-lookup"><span data-stu-id="c1e82-117">**requeue** – Terminate running tasks and requeue them.</span></span> <span data-ttu-id="c1e82-118">A feladatok a feladat engedélyezésekor fognak újra futni.</span><span class="sxs-lookup"><span data-stu-id="c1e82-118">The tasks will run again when the job is enabled.</span></span> <span data-ttu-id="c1e82-119">Tevékenységek leállítása után rögtön csomópontjának eltávolítására.</span><span class="sxs-lookup"><span data-stu-id="c1e82-119">Remove nodes as soon as tasks have been terminated.</span></span><br /><br /> <span data-ttu-id="c1e82-120">**Állítsa le** – az éppen futó feladatok megszakítását.</span><span class="sxs-lookup"><span data-stu-id="c1e82-120">**terminate** – Terminate running tasks.</span></span> <span data-ttu-id="c1e82-121">A tevékenységeket nem futtatja újra.</span><span class="sxs-lookup"><span data-stu-id="c1e82-121">The tasks will not run again.</span></span> <span data-ttu-id="c1e82-122">Tevékenységek leállítása után rögtön csomópontjának eltávolítására.</span><span class="sxs-lookup"><span data-stu-id="c1e82-122">Remove nodes as soon as tasks have been terminated.</span></span><br /><br /> <span data-ttu-id="c1e82-123">**taskcompletion** – engedélyezése jelenleg futó feladatok elvégzéséhez.</span><span class="sxs-lookup"><span data-stu-id="c1e82-123">**taskcompletion** – Allow currently running tasks to complete.</span></span> <span data-ttu-id="c1e82-124">Nem ütemez újabb tevékenységeket való várakozás során.</span><span class="sxs-lookup"><span data-stu-id="c1e82-124">Schedule no new tasks while waiting.</span></span> <span data-ttu-id="c1e82-125">Távolítsa el a csomópontok, ha minden feladat befejeződött.</span><span class="sxs-lookup"><span data-stu-id="c1e82-125">Remove nodes when all tasks have completed.</span></span><br /><br /> <span data-ttu-id="c1e82-126">**Retaineddata** -lehetővé teszi a futó feladatok befejeződését, majd várja meg, hogy minden tevékenység adatmegőrzés időszakait lejár.</span><span class="sxs-lookup"><span data-stu-id="c1e82-126">**Retaineddata** - Allow currently running tasks to complete, then wait for all task data retention periods to expire.</span></span> <span data-ttu-id="c1e82-127">Nem ütemez újabb tevékenységeket való várakozás során.</span><span class="sxs-lookup"><span data-stu-id="c1e82-127">Schedule no new tasks while waiting.</span></span> <span data-ttu-id="c1e82-128">Csomópontjának eltávolítására, ha az összes feladat megőrzési időszak lejárt.</span><span class="sxs-lookup"><span data-stu-id="c1e82-128">Remove nodes when all task retention periods have expired.</span></span><br /><br /> <span data-ttu-id="c1e82-129">Az alapértelmezett érték: requeue.</span><span class="sxs-lookup"><span data-stu-id="c1e82-129">The default value is requeue.</span></span><br /><br /> <span data-ttu-id="c1e82-130">Ha a készlet mérete növekszik, akkor a változó értéke **érvénytelen**.</span><span class="sxs-lookup"><span data-stu-id="c1e82-130">If the pool size is increasing then the value is set to **invalid**.</span></span>|
|<span data-ttu-id="c1e82-131">currentDedicated</span><span class="sxs-lookup"><span data-stu-id="c1e82-131">currentDedicated</span></span>|<span data-ttu-id="c1e82-132">Int32</span><span class="sxs-lookup"><span data-stu-id="c1e82-132">Int32</span></span>|<span data-ttu-id="c1e82-133">A számítási csomópontok a készlethez rendelt száma.</span><span class="sxs-lookup"><span data-stu-id="c1e82-133">The number of compute nodes currently assigned to the pool.</span></span>|
|<span data-ttu-id="c1e82-134">targetDedicated</span><span class="sxs-lookup"><span data-stu-id="c1e82-134">targetDedicated</span></span>|<span data-ttu-id="c1e82-135">Int32</span><span class="sxs-lookup"><span data-stu-id="c1e82-135">Int32</span></span>|<span data-ttu-id="c1e82-136">A kért a készlet számítási csomópontok száma.</span><span class="sxs-lookup"><span data-stu-id="c1e82-136">The number of compute nodes that are requested for the pool.</span></span>|
|<span data-ttu-id="c1e82-137">enableAutoScale</span><span class="sxs-lookup"><span data-stu-id="c1e82-137">enableAutoScale</span></span>|<span data-ttu-id="c1e82-138">logikai érték</span><span class="sxs-lookup"><span data-stu-id="c1e82-138">Bool</span></span>|<span data-ttu-id="c1e82-139">Meghatározza, hogy a készlet mérete automatikusan igazodni adott idő alatt.</span><span class="sxs-lookup"><span data-stu-id="c1e82-139">Specifies whether the pool size automatically adjusts over time.</span></span>|
|<span data-ttu-id="c1e82-140">isAutoPool</span><span class="sxs-lookup"><span data-stu-id="c1e82-140">isAutoPool</span></span>|<span data-ttu-id="c1e82-141">logikai érték</span><span class="sxs-lookup"><span data-stu-id="c1e82-141">Bool</span></span>|<span data-ttu-id="c1e82-142">Speficies hogy a készlet egy feladat AutoPool mechanizmus révén készült.</span><span class="sxs-lookup"><span data-stu-id="c1e82-142">Speficies whether the pool was created via a job's AutoPool mechanism.</span></span>|
