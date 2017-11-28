---
title: "Azure Batch feladat kezdési esemény |} Microsoft Docs"
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
ms.openlocfilehash: c47ab36c99dddd46a14c15018a2a46bf7f873ffa
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="task-start-event"></a><span data-ttu-id="fb9e9-103">A feladat kezdési esemény</span><span class="sxs-lookup"><span data-stu-id="fb9e9-103">Task start event</span></span>

 <span data-ttu-id="fb9e9-104">Ez az esemény után a feladat ütemezés szerint indítsa el a számítási csomóponton az ütemező által kibocsátott.</span><span class="sxs-lookup"><span data-stu-id="fb9e9-104">This event is emitted once a task has been scheduled to start on a compute node by the scheduler.</span></span> <span data-ttu-id="fb9e9-105">Vegye figyelembe, hogy ha a feladatot a rendszer újra, vagy helyezte ezt az eseményt fog kell kibocsátott újra ugyanezt a feladatot, de az újrapróbálkozások maximális számát és a rendszer a feladat verziója ennek megfelelően frissül.</span><span class="sxs-lookup"><span data-stu-id="fb9e9-105">Note that if the task is retried or requeued this event will be emitted again for the same task, but the retry count and system task version will be updated accordingly.</span></span>


 <span data-ttu-id="fb9e9-106">A következő példa bemutatja, hogy a feladat törzse esemény indítása.</span><span class="sxs-lookup"><span data-stu-id="fb9e9-106">The following example shows the body of a task start event.</span></span>

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

|<span data-ttu-id="fb9e9-107">Elem neve</span><span class="sxs-lookup"><span data-stu-id="fb9e9-107">Element name</span></span>|<span data-ttu-id="fb9e9-108">Típus</span><span class="sxs-lookup"><span data-stu-id="fb9e9-108">Type</span></span>|<span data-ttu-id="fb9e9-109">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="fb9e9-109">Notes</span></span>|
|------------------|----------|-----------|
|<span data-ttu-id="fb9e9-110">A JobId értékének</span><span class="sxs-lookup"><span data-stu-id="fb9e9-110">jobId</span></span>|<span data-ttu-id="fb9e9-111">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="fb9e9-111">String</span></span>|<span data-ttu-id="fb9e9-112">A feladat a tevékenységet tartalmazó azonosítója.</span><span class="sxs-lookup"><span data-stu-id="fb9e9-112">The id of the job containing the task.</span></span>|
|<span data-ttu-id="fb9e9-113">id</span><span class="sxs-lookup"><span data-stu-id="fb9e9-113">id</span></span>|<span data-ttu-id="fb9e9-114">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="fb9e9-114">String</span></span>|<span data-ttu-id="fb9e9-115">A feladat azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="fb9e9-115">The id of the task.</span></span>|
|<span data-ttu-id="fb9e9-116">taskType</span><span class="sxs-lookup"><span data-stu-id="fb9e9-116">taskType</span></span>|<span data-ttu-id="fb9e9-117">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="fb9e9-117">String</span></span>|<span data-ttu-id="fb9e9-118">A tevékenység típusa.</span><span class="sxs-lookup"><span data-stu-id="fb9e9-118">The type of the task.</span></span> <span data-ttu-id="fb9e9-119">Ez is, miszerint manager feladata JobManager kell vagy a "User" jelezve, hogy ez nem egy kezelő feladat.</span><span class="sxs-lookup"><span data-stu-id="fb9e9-119">This can either be 'JobManager' indicating it is a job manager task or 'User' indicating it is not a job manager task.</span></span>|
|<span data-ttu-id="fb9e9-120">systemTaskVersion</span><span class="sxs-lookup"><span data-stu-id="fb9e9-120">systemTaskVersion</span></span>|<span data-ttu-id="fb9e9-121">Int32</span><span class="sxs-lookup"><span data-stu-id="fb9e9-121">Int32</span></span>|<span data-ttu-id="fb9e9-122">Ez egy, a belső újrapróbálkozási számláló meg olyan feladatra.</span><span class="sxs-lookup"><span data-stu-id="fb9e9-122">This is the internal retry counter on a task.</span></span> <span data-ttu-id="fb9e9-123">A Batch szolgáltatás belső újra egy feladatot, hogy figyelembe vegye az átmeneti problémákat.</span><span class="sxs-lookup"><span data-stu-id="fb9e9-123">Internally the Batch service can retry a task to account for transient issues.</span></span> <span data-ttu-id="fb9e9-124">A hibák például belső ütemezési hibák, vagy megpróbálja helyreállítani a számítási csomópontok hibás állapotban.</span><span class="sxs-lookup"><span data-stu-id="fb9e9-124">These issues can include internal scheduling errors or attempts to recover from compute nodes in a bad state.</span></span>|
|[<span data-ttu-id="fb9e9-125">nodeInfo</span><span class="sxs-lookup"><span data-stu-id="fb9e9-125">nodeInfo</span></span>](#nodeInfo)|<span data-ttu-id="fb9e9-126">Összetett típus</span><span class="sxs-lookup"><span data-stu-id="fb9e9-126">Complex Type</span></span>|<span data-ttu-id="fb9e9-127">A számítási csomópont, amelyen a feladat futott adatait tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="fb9e9-127">Contains information about the compute node on which the task ran.</span></span>|
|[<span data-ttu-id="fb9e9-128">multiInstanceSettings</span><span class="sxs-lookup"><span data-stu-id="fb9e9-128">multiInstanceSettings</span></span>](#multiInstanceSettings)|<span data-ttu-id="fb9e9-129">Összetett típus</span><span class="sxs-lookup"><span data-stu-id="fb9e9-129">Complex Type</span></span>|<span data-ttu-id="fb9e9-130">Megadja, hogy a feladat több számítási csomópont igénylő többpéldányos feladat.</span><span class="sxs-lookup"><span data-stu-id="fb9e9-130">Specifies that the task  is Multi-Instance Task requiring multiple compute nodes.</span></span>  <span data-ttu-id="fb9e9-131">Lásd: [multiInstanceSettings](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-task) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="fb9e9-131">See [multiInstanceSettings](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-task) for details.</span></span>|
|[<span data-ttu-id="fb9e9-132">megkötések</span><span class="sxs-lookup"><span data-stu-id="fb9e9-132">constraints</span></span>](#constraints)|<span data-ttu-id="fb9e9-133">Összetett típus</span><span class="sxs-lookup"><span data-stu-id="fb9e9-133">Complex Type</span></span>|<span data-ttu-id="fb9e9-134">Ez a feladat vonatkozó végrehajtási korlátozások.</span><span class="sxs-lookup"><span data-stu-id="fb9e9-134">The execution constraints that apply to this task.</span></span>|
|[<span data-ttu-id="fb9e9-135">executionInfo</span><span class="sxs-lookup"><span data-stu-id="fb9e9-135">executionInfo</span></span>](#executionInfo)|<span data-ttu-id="fb9e9-136">Összetett típus</span><span class="sxs-lookup"><span data-stu-id="fb9e9-136">Complex Type</span></span>|<span data-ttu-id="fb9e9-137">A feladat végrehajtásának adatait tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="fb9e9-137">Contains information about the execution of the task.</span></span>|

###  <span data-ttu-id="fb9e9-138"><a name="nodeInfo"></a>nodeInfo</span><span class="sxs-lookup"><span data-stu-id="fb9e9-138"><a name="nodeInfo"></a> nodeInfo</span></span>

|<span data-ttu-id="fb9e9-139">Elem neve</span><span class="sxs-lookup"><span data-stu-id="fb9e9-139">Element name</span></span>|<span data-ttu-id="fb9e9-140">Típus</span><span class="sxs-lookup"><span data-stu-id="fb9e9-140">Type</span></span>|<span data-ttu-id="fb9e9-141">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="fb9e9-141">Notes</span></span>|
|------------------|----------|-----------|
|<span data-ttu-id="fb9e9-142">poolId</span><span class="sxs-lookup"><span data-stu-id="fb9e9-142">poolId</span></span>|<span data-ttu-id="fb9e9-143">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="fb9e9-143">String</span></span>|<span data-ttu-id="fb9e9-144">A készlet, amelyen a feladat futott azonosítója.</span><span class="sxs-lookup"><span data-stu-id="fb9e9-144">The id of the pool on which the task ran.</span></span>|
|<span data-ttu-id="fb9e9-145">nodeId</span><span class="sxs-lookup"><span data-stu-id="fb9e9-145">nodeId</span></span>|<span data-ttu-id="fb9e9-146">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="fb9e9-146">String</span></span>|<span data-ttu-id="fb9e9-147">A csomópont, amelyen a feladat futott azonosítója.</span><span class="sxs-lookup"><span data-stu-id="fb9e9-147">The id of the node on which the task ran.</span></span>|

###  <span data-ttu-id="fb9e9-148"><a name="multiInstanceSettings"></a>multiInstanceSettings</span><span class="sxs-lookup"><span data-stu-id="fb9e9-148"><a name="multiInstanceSettings"></a> multiInstanceSettings</span></span>

|<span data-ttu-id="fb9e9-149">Elem neve</span><span class="sxs-lookup"><span data-stu-id="fb9e9-149">Element name</span></span>|<span data-ttu-id="fb9e9-150">Típus</span><span class="sxs-lookup"><span data-stu-id="fb9e9-150">Type</span></span>|<span data-ttu-id="fb9e9-151">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="fb9e9-151">Notes</span></span>|
|------------------|----------|-----------|
|<span data-ttu-id="fb9e9-152">numberOfInstances</span><span class="sxs-lookup"><span data-stu-id="fb9e9-152">numberOfInstances</span></span>|<span data-ttu-id="fb9e9-153">int</span><span class="sxs-lookup"><span data-stu-id="fb9e9-153">Int</span></span>|<span data-ttu-id="fb9e9-154">A tevékenységhez szükséges számítási csomópontok száma.</span><span class="sxs-lookup"><span data-stu-id="fb9e9-154">The number of compute nodes required by the task.</span></span>|

###  <span data-ttu-id="fb9e9-155"><a name="constraints"></a>megkötések</span><span class="sxs-lookup"><span data-stu-id="fb9e9-155"><a name="constraints"></a> constraints</span></span>

|<span data-ttu-id="fb9e9-156">Elem neve</span><span class="sxs-lookup"><span data-stu-id="fb9e9-156">Element name</span></span>|<span data-ttu-id="fb9e9-157">Típus</span><span class="sxs-lookup"><span data-stu-id="fb9e9-157">Type</span></span>|<span data-ttu-id="fb9e9-158">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="fb9e9-158">Notes</span></span>|
|------------------|----------|-----------|
|<span data-ttu-id="fb9e9-159">maxTaskRetryCount</span><span class="sxs-lookup"><span data-stu-id="fb9e9-159">maxTaskRetryCount</span></span>|<span data-ttu-id="fb9e9-160">Int32</span><span class="sxs-lookup"><span data-stu-id="fb9e9-160">Int32</span></span>|<span data-ttu-id="fb9e9-161">A maximális száma a próbálhassa.</span><span class="sxs-lookup"><span data-stu-id="fb9e9-161">The maximum number of times the task may be retried.</span></span> <span data-ttu-id="fb9e9-162">A Batch szolgáltatás feladat újrapróbálkozik, ha annak kilépési kódja nem nulla.</span><span class="sxs-lookup"><span data-stu-id="fb9e9-162">The Batch service retries a task if its exit code is nonzero.</span></span><br /><br /> <span data-ttu-id="fb9e9-163">Vegye figyelembe, hogy ez az érték kifejezetten határozza meg az újbóli próbálkozások számát.</span><span class="sxs-lookup"><span data-stu-id="fb9e9-163">Note that this value specifically controls the number of retries.</span></span> <span data-ttu-id="fb9e9-164">A Batch szolgáltatás egyszer megpróbál-e a feladatot, és előfordulhat, hogy ismételje meg ezt a határt legfeljebb.</span><span class="sxs-lookup"><span data-stu-id="fb9e9-164">The Batch service will try the task once, and may then retry up to this limit.</span></span> <span data-ttu-id="fb9e9-165">Például ha az újrapróbálkozások maximális száma 3, a feladat kötegelt záma legfeljebb 4 alkalommal (egy kezdeti próbálja és 3 újrapróbálás).</span><span class="sxs-lookup"><span data-stu-id="fb9e9-165">For example, if the maximum retry count is 3, Batch tries a task up to 4 times (one initial try and 3 retries).</span></span><br /><br /> <span data-ttu-id="fb9e9-166">Ha az újrapróbálkozások maximális száma 0, a Batch szolgáltatás nem próbálja meg újra feladatok.</span><span class="sxs-lookup"><span data-stu-id="fb9e9-166">If the maximum retry count is 0, the Batch service does not retry tasks.</span></span><br /><br /> <span data-ttu-id="fb9e9-167">Ha az újrapróbálkozások maximális száma -1, a Batch szolgáltatás korlátozás nélkül feladatok után ismét.</span><span class="sxs-lookup"><span data-stu-id="fb9e9-167">If the maximum retry count is -1, the Batch service retries tasks without limit.</span></span><br /><br /> <span data-ttu-id="fb9e9-168">Az alapértelmezett érték: 0 (nincs újrapróbálás).</span><span class="sxs-lookup"><span data-stu-id="fb9e9-168">The default value is 0 (no retries).</span></span>|

###  <span data-ttu-id="fb9e9-169"><a name="executionInfo"></a>executionInfo</span><span class="sxs-lookup"><span data-stu-id="fb9e9-169"><a name="executionInfo"></a> executionInfo</span></span>

|<span data-ttu-id="fb9e9-170">Elem neve</span><span class="sxs-lookup"><span data-stu-id="fb9e9-170">Element name</span></span>|<span data-ttu-id="fb9e9-171">Típus</span><span class="sxs-lookup"><span data-stu-id="fb9e9-171">Type</span></span>|<span data-ttu-id="fb9e9-172">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="fb9e9-172">Notes</span></span>|
|------------------|----------|-----------|
|<span data-ttu-id="fb9e9-173">a retryCount</span><span class="sxs-lookup"><span data-stu-id="fb9e9-173">retryCount</span></span>|<span data-ttu-id="fb9e9-174">Int32</span><span class="sxs-lookup"><span data-stu-id="fb9e9-174">Int32</span></span>|<span data-ttu-id="fb9e9-175">A száma a feladat próbálkozásainak már elérte a Batch szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="fb9e9-175">The number of times the task has been retried by the Batch service.</span></span> <span data-ttu-id="fb9e9-176">Rendszer megpróbálja újból végrehajtani a feladatot, ha egy nem nulla kilépési kód, mely legfeljebb a megadott MaxTaskRetryCount kilép</span><span class="sxs-lookup"><span data-stu-id="fb9e9-176">The task is retried if it exits with a nonzero exit code, up to the specified MaxTaskRetryCount</span></span>|
