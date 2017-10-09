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
# <a name="pool-resize-start-event"></a><span data-ttu-id="d42b0-103">Készlet átméretezési start eseményét.</span><span class="sxs-lookup"><span data-stu-id="d42b0-103">Pool resize start event</span></span>

 <span data-ttu-id="d42b0-104">Ez az esemény kibocsátott, amikor egy alkalmazáskészlet átméretezési megkezdődött.</span><span class="sxs-lookup"><span data-stu-id="d42b0-104">This event is emitted when a pool resize has started.</span></span> <span data-ttu-id="d42b0-105">Mivel hello készlet átméretezési aszinkron esemény, számíthat egy készlet átméretezési befejeződésének eseményét toobe kibocsátott után hello átméretezése művelet befejeződik.</span><span class="sxs-lookup"><span data-stu-id="d42b0-105">Since hello pool resize is an asynchronous event, you can expect a pool resize complete event toobe emitted once hello resize operation completes.</span></span>

 <span data-ttu-id="d42b0-106">a következő példa azt mutatja be egy alkalmazáskészlet átméretezéséhez 0 too2 csomópontjairól a kézi készlet átméretezési indító esemény törzsét hello hello átméretezése.</span><span class="sxs-lookup"><span data-stu-id="d42b0-106">hello following example shows hello body of a pool resize start event for a pool resizing from 0 too2 nodes with a manual resize.</span></span>

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

|<span data-ttu-id="d42b0-107">Elem</span><span class="sxs-lookup"><span data-stu-id="d42b0-107">Element</span></span>|<span data-ttu-id="d42b0-108">Típus</span><span class="sxs-lookup"><span data-stu-id="d42b0-108">Type</span></span>|<span data-ttu-id="d42b0-109">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="d42b0-109">Notes</span></span>|
|-------------|----------|-----------|
|<span data-ttu-id="d42b0-110">poolId</span><span class="sxs-lookup"><span data-stu-id="d42b0-110">poolId</span></span>|<span data-ttu-id="d42b0-111">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="d42b0-111">String</span></span>|<span data-ttu-id="d42b0-112">hello készlet hello azonosítója.</span><span class="sxs-lookup"><span data-stu-id="d42b0-112">hello id of hello pool.</span></span>|
|<span data-ttu-id="d42b0-113">nodeDeallocationOption</span><span class="sxs-lookup"><span data-stu-id="d42b0-113">nodeDeallocationOption</span></span>|<span data-ttu-id="d42b0-114">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="d42b0-114">String</span></span>|<span data-ttu-id="d42b0-115">Itt adhatja meg, ha lehetséges, hogy lehet csomópontokat eltávolítani hello készletből, ha hello készlet méretének csökkentése.</span><span class="sxs-lookup"><span data-stu-id="d42b0-115">Specifies when nodes may be removed from hello pool, if hello pool size is decreasing.</span></span><br /><br /> <span data-ttu-id="d42b0-116">Lehetséges értékek:</span><span class="sxs-lookup"><span data-stu-id="d42b0-116">Possible values are:</span></span><br /><br /> <span data-ttu-id="d42b0-117">**requeue** – leállítja a futó tevékenységeket, és újra a várólistába helyezi őket.</span><span class="sxs-lookup"><span data-stu-id="d42b0-117">**requeue** – Terminate running tasks and requeue them.</span></span> <span data-ttu-id="d42b0-118">hello feladatok hello feladat engedélyezésekor fognak újra futni.</span><span class="sxs-lookup"><span data-stu-id="d42b0-118">hello tasks will run again when hello job is enabled.</span></span> <span data-ttu-id="d42b0-119">Tevékenységek leállítása után rögtön csomópontjának eltávolítására.</span><span class="sxs-lookup"><span data-stu-id="d42b0-119">Remove nodes as soon as tasks have been terminated.</span></span><br /><br /> <span data-ttu-id="d42b0-120">**Állítsa le** – az éppen futó feladatok megszakítását.</span><span class="sxs-lookup"><span data-stu-id="d42b0-120">**terminate** – Terminate running tasks.</span></span> <span data-ttu-id="d42b0-121">hello tevékenységeket nem futtatja újra.</span><span class="sxs-lookup"><span data-stu-id="d42b0-121">hello tasks will not run again.</span></span> <span data-ttu-id="d42b0-122">Tevékenységek leállítása után rögtön csomópontjának eltávolítására.</span><span class="sxs-lookup"><span data-stu-id="d42b0-122">Remove nodes as soon as tasks have been terminated.</span></span><br /><br /> <span data-ttu-id="d42b0-123">**taskcompletion** – jelenleg futó feladatok toocomplete engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="d42b0-123">**taskcompletion** – Allow currently running tasks toocomplete.</span></span> <span data-ttu-id="d42b0-124">Nem ütemez újabb tevékenységeket való várakozás során.</span><span class="sxs-lookup"><span data-stu-id="d42b0-124">Schedule no new tasks while waiting.</span></span> <span data-ttu-id="d42b0-125">Távolítsa el a csomópontok, ha minden feladat befejeződött.</span><span class="sxs-lookup"><span data-stu-id="d42b0-125">Remove nodes when all tasks have completed.</span></span><br /><br /> <span data-ttu-id="d42b0-126">**Retaineddata** – lehetővé teszi a futó feladatok toocomplete, majd várja meg, hogy minden tevékenység adatok megőrzési időszak tooexpire.</span><span class="sxs-lookup"><span data-stu-id="d42b0-126">**Retaineddata** - Allow currently running tasks toocomplete, then wait for all task data retention periods tooexpire.</span></span> <span data-ttu-id="d42b0-127">Nem ütemez újabb tevékenységeket való várakozás során.</span><span class="sxs-lookup"><span data-stu-id="d42b0-127">Schedule no new tasks while waiting.</span></span> <span data-ttu-id="d42b0-128">Csomópontjának eltávolítására, ha az összes feladat megőrzési időszak lejárt.</span><span class="sxs-lookup"><span data-stu-id="d42b0-128">Remove nodes when all task retention periods have expired.</span></span><br /><br /> <span data-ttu-id="d42b0-129">hello alapértelmezett értéke requeue.</span><span class="sxs-lookup"><span data-stu-id="d42b0-129">hello default value is requeue.</span></span><br /><br /> <span data-ttu-id="d42b0-130">Ha hello mérete növekszik, akkor hello értéke túl**érvénytelen**.</span><span class="sxs-lookup"><span data-stu-id="d42b0-130">If hello pool size is increasing then hello value is set too**invalid**.</span></span>|
|<span data-ttu-id="d42b0-131">currentDedicated</span><span class="sxs-lookup"><span data-stu-id="d42b0-131">currentDedicated</span></span>|<span data-ttu-id="d42b0-132">Int32</span><span class="sxs-lookup"><span data-stu-id="d42b0-132">Int32</span></span>|<span data-ttu-id="d42b0-133">számítási csomópontok száma hello rendelve toohello készlet.</span><span class="sxs-lookup"><span data-stu-id="d42b0-133">hello number of compute nodes currently assigned toohello pool.</span></span>|
|<span data-ttu-id="d42b0-134">targetDedicated</span><span class="sxs-lookup"><span data-stu-id="d42b0-134">targetDedicated</span></span>|<span data-ttu-id="d42b0-135">Int32</span><span class="sxs-lookup"><span data-stu-id="d42b0-135">Int32</span></span>|<span data-ttu-id="d42b0-136">számítási csomópontok hello készlet igényelt hello száma.</span><span class="sxs-lookup"><span data-stu-id="d42b0-136">hello number of compute nodes that are requested for hello pool.</span></span>|
|<span data-ttu-id="d42b0-137">enableAutoScale</span><span class="sxs-lookup"><span data-stu-id="d42b0-137">enableAutoScale</span></span>|<span data-ttu-id="d42b0-138">logikai érték</span><span class="sxs-lookup"><span data-stu-id="d42b0-138">Bool</span></span>|<span data-ttu-id="d42b0-139">Meghatározza, hogy hello mérete automatikusan igazodni adott idő alatt.</span><span class="sxs-lookup"><span data-stu-id="d42b0-139">Specifies whether hello pool size automatically adjusts over time.</span></span>|
|<span data-ttu-id="d42b0-140">isAutoPool</span><span class="sxs-lookup"><span data-stu-id="d42b0-140">isAutoPool</span></span>|<span data-ttu-id="d42b0-141">logikai érték</span><span class="sxs-lookup"><span data-stu-id="d42b0-141">Bool</span></span>|<span data-ttu-id="d42b0-142">Speficies e hello készlet használatával egy feladat AutoPool mechanizmus jött létre.</span><span class="sxs-lookup"><span data-stu-id="d42b0-142">Speficies whether hello pool was created via a job's AutoPool mechanism.</span></span>|
