---
title: "AAA \"Azure kötegelt feladat kezdési esemény |} Microsoft dokumentumok\""
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
ms.openlocfilehash: 2cb066be1578741125e9081a84a2b7c74dc8356a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="task-start-event"></a><span data-ttu-id="c6d4b-103">A feladat kezdési esemény</span><span class="sxs-lookup"><span data-stu-id="c6d4b-103">Task start event</span></span>

 <span data-ttu-id="c6d4b-104">Ez az esemény is ki lesz adva a feladatok ütemezett toostart adott számítási csomóponton követően hello az ütemező által.</span><span class="sxs-lookup"><span data-stu-id="c6d4b-104">This event is emitted once a task has been scheduled toostart on a compute node by hello scheduler.</span></span> <span data-ttu-id="c6d4b-105">Vegye figyelembe, hogy ha hello feladat újrapróbált vagy helyezte az esemény lesz kell kibocsátott újra hello ugyanezt a feladatot, de hello újrapróbálkozások számlálójának és a rendszer a feladat verziója ennek megfelelően frissül.</span><span class="sxs-lookup"><span data-stu-id="c6d4b-105">Note that if hello task is retried or requeued this event will be emitted again for hello same task, but hello retry count and system task version will be updated accordingly.</span></span>


 <span data-ttu-id="c6d4b-106">hello következő példa bemutatja a feladat kezdési esemény hello törzsét.</span><span class="sxs-lookup"><span data-stu-id="c6d4b-106">hello following example shows hello body of a task start event.</span></span>

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

|<span data-ttu-id="c6d4b-107">Elem neve</span><span class="sxs-lookup"><span data-stu-id="c6d4b-107">Element name</span></span>|<span data-ttu-id="c6d4b-108">Típus</span><span class="sxs-lookup"><span data-stu-id="c6d4b-108">Type</span></span>|<span data-ttu-id="c6d4b-109">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="c6d4b-109">Notes</span></span>|
|------------------|----------|-----------|
|<span data-ttu-id="c6d4b-110">A JobId értékének</span><span class="sxs-lookup"><span data-stu-id="c6d4b-110">jobId</span></span>|<span data-ttu-id="c6d4b-111">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="c6d4b-111">String</span></span>|<span data-ttu-id="c6d4b-112">hello tevékenységet tartalmazó hello feladat hello azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="c6d4b-112">hello id of hello job containing hello task.</span></span>|
|<span data-ttu-id="c6d4b-113">id</span><span class="sxs-lookup"><span data-stu-id="c6d4b-113">id</span></span>|<span data-ttu-id="c6d4b-114">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="c6d4b-114">String</span></span>|<span data-ttu-id="c6d4b-115">hello feladat hello azonosítója.</span><span class="sxs-lookup"><span data-stu-id="c6d4b-115">hello id of hello task.</span></span>|
|<span data-ttu-id="c6d4b-116">taskType</span><span class="sxs-lookup"><span data-stu-id="c6d4b-116">taskType</span></span>|<span data-ttu-id="c6d4b-117">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="c6d4b-117">String</span></span>|<span data-ttu-id="c6d4b-118">hello hello feladat típusát.</span><span class="sxs-lookup"><span data-stu-id="c6d4b-118">hello type of hello task.</span></span> <span data-ttu-id="c6d4b-119">Ez is, miszerint manager feladata JobManager kell vagy a "User" jelezve, hogy ez nem egy kezelő feladat.</span><span class="sxs-lookup"><span data-stu-id="c6d4b-119">This can either be 'JobManager' indicating it is a job manager task or 'User' indicating it is not a job manager task.</span></span>|
|<span data-ttu-id="c6d4b-120">systemTaskVersion</span><span class="sxs-lookup"><span data-stu-id="c6d4b-120">systemTaskVersion</span></span>|<span data-ttu-id="c6d4b-121">Int32</span><span class="sxs-lookup"><span data-stu-id="c6d4b-121">Int32</span></span>|<span data-ttu-id="c6d4b-122">Ez a hello belső újrapróbálkozási számláló meg olyan feladatra.</span><span class="sxs-lookup"><span data-stu-id="c6d4b-122">This is hello internal retry counter on a task.</span></span> <span data-ttu-id="c6d4b-123">Belső hello Batch szolgáltatás újra egy feladat tooaccount az átmeneti problémákat.</span><span class="sxs-lookup"><span data-stu-id="c6d4b-123">Internally hello Batch service can retry a task tooaccount for transient issues.</span></span> <span data-ttu-id="c6d4b-124">A hibák például belső ütemezési hibák vagy kísérletek toorecover a számítási csomópontok hibás állapotban.</span><span class="sxs-lookup"><span data-stu-id="c6d4b-124">These issues can include internal scheduling errors or attempts toorecover from compute nodes in a bad state.</span></span>|
|[<span data-ttu-id="c6d4b-125">nodeInfo</span><span class="sxs-lookup"><span data-stu-id="c6d4b-125">nodeInfo</span></span>](#nodeInfo)|<span data-ttu-id="c6d4b-126">Összetett típus</span><span class="sxs-lookup"><span data-stu-id="c6d4b-126">Complex Type</span></span>|<span data-ttu-id="c6d4b-127">Mely hello a feladat futott hello számítási csomópont adatait tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="c6d4b-127">Contains information about hello compute node on which hello task ran.</span></span>|
|[<span data-ttu-id="c6d4b-128">multiInstanceSettings</span><span class="sxs-lookup"><span data-stu-id="c6d4b-128">multiInstanceSettings</span></span>](#multiInstanceSettings)|<span data-ttu-id="c6d4b-129">Összetett típus</span><span class="sxs-lookup"><span data-stu-id="c6d4b-129">Complex Type</span></span>|<span data-ttu-id="c6d4b-130">Határozza meg, hogy hello feladat több számítási csomópont igénylő többpéldányos feladat.</span><span class="sxs-lookup"><span data-stu-id="c6d4b-130">Specifies that hello task  is Multi-Instance Task requiring multiple compute nodes.</span></span>  <span data-ttu-id="c6d4b-131">Lásd: [multiInstanceSettings](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-task) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="c6d4b-131">See [multiInstanceSettings](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-task) for details.</span></span>|
|[<span data-ttu-id="c6d4b-132">megkötések</span><span class="sxs-lookup"><span data-stu-id="c6d4b-132">constraints</span></span>](#constraints)|<span data-ttu-id="c6d4b-133">Összetett típus</span><span class="sxs-lookup"><span data-stu-id="c6d4b-133">Complex Type</span></span>|<span data-ttu-id="c6d4b-134">hello végrehajtási megkötések érvényes toothis feladat.</span><span class="sxs-lookup"><span data-stu-id="c6d4b-134">hello execution constraints that apply toothis task.</span></span>|
|[<span data-ttu-id="c6d4b-135">executionInfo</span><span class="sxs-lookup"><span data-stu-id="c6d4b-135">executionInfo</span></span>](#executionInfo)|<span data-ttu-id="c6d4b-136">Összetett típus</span><span class="sxs-lookup"><span data-stu-id="c6d4b-136">Complex Type</span></span>|<span data-ttu-id="c6d4b-137">Hello feladat végrehajtásának hello adatait tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="c6d4b-137">Contains information about hello execution of hello task.</span></span>|

###  <span data-ttu-id="c6d4b-138"><a name="nodeInfo"></a>nodeInfo</span><span class="sxs-lookup"><span data-stu-id="c6d4b-138"><a name="nodeInfo"></a> nodeInfo</span></span>

|<span data-ttu-id="c6d4b-139">Elem neve</span><span class="sxs-lookup"><span data-stu-id="c6d4b-139">Element name</span></span>|<span data-ttu-id="c6d4b-140">Típus</span><span class="sxs-lookup"><span data-stu-id="c6d4b-140">Type</span></span>|<span data-ttu-id="c6d4b-141">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="c6d4b-141">Notes</span></span>|
|------------------|----------|-----------|
|<span data-ttu-id="c6d4b-142">poolId</span><span class="sxs-lookup"><span data-stu-id="c6d4b-142">poolId</span></span>|<span data-ttu-id="c6d4b-143">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="c6d4b-143">String</span></span>|<span data-ttu-id="c6d4b-144">mely hello a feladat futott hello készlet hello azonosítója.</span><span class="sxs-lookup"><span data-stu-id="c6d4b-144">hello id of hello pool on which hello task ran.</span></span>|
|<span data-ttu-id="c6d4b-145">nodeId</span><span class="sxs-lookup"><span data-stu-id="c6d4b-145">nodeId</span></span>|<span data-ttu-id="c6d4b-146">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="c6d4b-146">String</span></span>|<span data-ttu-id="c6d4b-147">hello csomópont, mely hello a feladat futott hello azonosítója.</span><span class="sxs-lookup"><span data-stu-id="c6d4b-147">hello id of hello node on which hello task ran.</span></span>|

###  <span data-ttu-id="c6d4b-148"><a name="multiInstanceSettings"></a>multiInstanceSettings</span><span class="sxs-lookup"><span data-stu-id="c6d4b-148"><a name="multiInstanceSettings"></a> multiInstanceSettings</span></span>

|<span data-ttu-id="c6d4b-149">Elem neve</span><span class="sxs-lookup"><span data-stu-id="c6d4b-149">Element name</span></span>|<span data-ttu-id="c6d4b-150">Típus</span><span class="sxs-lookup"><span data-stu-id="c6d4b-150">Type</span></span>|<span data-ttu-id="c6d4b-151">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="c6d4b-151">Notes</span></span>|
|------------------|----------|-----------|
|<span data-ttu-id="c6d4b-152">numberOfInstances</span><span class="sxs-lookup"><span data-stu-id="c6d4b-152">numberOfInstances</span></span>|<span data-ttu-id="c6d4b-153">int</span><span class="sxs-lookup"><span data-stu-id="c6d4b-153">Int</span></span>|<span data-ttu-id="c6d4b-154">számítási csomópontok hello feladat által igényelt hello száma.</span><span class="sxs-lookup"><span data-stu-id="c6d4b-154">hello number of compute nodes required by hello task.</span></span>|

###  <span data-ttu-id="c6d4b-155"><a name="constraints"></a>megkötések</span><span class="sxs-lookup"><span data-stu-id="c6d4b-155"><a name="constraints"></a> constraints</span></span>

|<span data-ttu-id="c6d4b-156">Elem neve</span><span class="sxs-lookup"><span data-stu-id="c6d4b-156">Element name</span></span>|<span data-ttu-id="c6d4b-157">Típus</span><span class="sxs-lookup"><span data-stu-id="c6d4b-157">Type</span></span>|<span data-ttu-id="c6d4b-158">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="c6d4b-158">Notes</span></span>|
|------------------|----------|-----------|
|<span data-ttu-id="c6d4b-159">maxTaskRetryCount</span><span class="sxs-lookup"><span data-stu-id="c6d4b-159">maxTaskRetryCount</span></span>|<span data-ttu-id="c6d4b-160">Int32</span><span class="sxs-lookup"><span data-stu-id="c6d4b-160">Int32</span></span>|<span data-ttu-id="c6d4b-161">hello maximálisan megengedett számú hello próbálhassa.</span><span class="sxs-lookup"><span data-stu-id="c6d4b-161">hello maximum number of times hello task may be retried.</span></span> <span data-ttu-id="c6d4b-162">hello Batch szolgáltatás feladat újrapróbálkozik, ha annak kilépési kódja nem nulla.</span><span class="sxs-lookup"><span data-stu-id="c6d4b-162">hello Batch service retries a task if its exit code is nonzero.</span></span><br /><br /> <span data-ttu-id="c6d4b-163">Vegye figyelembe, hogy ez az érték kifejezetten vezérlők hello az újrapróbálkozások számát.</span><span class="sxs-lookup"><span data-stu-id="c6d4b-163">Note that this value specifically controls hello number of retries.</span></span> <span data-ttu-id="c6d4b-164">hello Batch szolgáltatás egyszer megpróbál hello feladat, és előfordulhat, hogy próbálja mentése toothis korlátot.</span><span class="sxs-lookup"><span data-stu-id="c6d4b-164">hello Batch service will try hello task once, and may then retry up toothis limit.</span></span> <span data-ttu-id="c6d4b-165">Például ha hello újrapróbálkozások maximális száma: 3, a kötegelt próbálkozik too4 időpontokban (egy kezdeti próbálja és 3 újrapróbálás) mentése feladat.</span><span class="sxs-lookup"><span data-stu-id="c6d4b-165">For example, if hello maximum retry count is 3, Batch tries a task up too4 times (one initial try and 3 retries).</span></span><br /><br /> <span data-ttu-id="c6d4b-166">Ha hello újrapróbálkozások maximális száma 0, hello Batch szolgáltatás nem próbálja meg újra feladatok.</span><span class="sxs-lookup"><span data-stu-id="c6d4b-166">If hello maximum retry count is 0, hello Batch service does not retry tasks.</span></span><br /><br /> <span data-ttu-id="c6d4b-167">Ha hello újrapróbálkozások maximális száma -1, a hello Batch szolgáltatás korlátozás nélkül feladatok újrapróbálkozik.</span><span class="sxs-lookup"><span data-stu-id="c6d4b-167">If hello maximum retry count is -1, hello Batch service retries tasks without limit.</span></span><br /><br /> <span data-ttu-id="c6d4b-168">hello alapértelmezett értéke 0 (nincs újrapróbálás).</span><span class="sxs-lookup"><span data-stu-id="c6d4b-168">hello default value is 0 (no retries).</span></span>|

###  <span data-ttu-id="c6d4b-169"><a name="executionInfo"></a>executionInfo</span><span class="sxs-lookup"><span data-stu-id="c6d4b-169"><a name="executionInfo"></a> executionInfo</span></span>

|<span data-ttu-id="c6d4b-170">Elem neve</span><span class="sxs-lookup"><span data-stu-id="c6d4b-170">Element name</span></span>|<span data-ttu-id="c6d4b-171">Típus</span><span class="sxs-lookup"><span data-stu-id="c6d4b-171">Type</span></span>|<span data-ttu-id="c6d4b-172">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="c6d4b-172">Notes</span></span>|
|------------------|----------|-----------|
|<span data-ttu-id="c6d4b-173">a retryCount</span><span class="sxs-lookup"><span data-stu-id="c6d4b-173">retryCount</span></span>|<span data-ttu-id="c6d4b-174">Int32</span><span class="sxs-lookup"><span data-stu-id="c6d4b-174">Int32</span></span>|<span data-ttu-id="c6d4b-175">hello hello feladat próbálkozásainak már elérte a hello Batch szolgáltatás hányszor.</span><span class="sxs-lookup"><span data-stu-id="c6d4b-175">hello number of times hello task has been retried by hello Batch service.</span></span> <span data-ttu-id="c6d4b-176">Ha kilép egy nem nulla kilépési kód mentése toohello megadott MaxTaskRetryCount hello feladat megismétlése</span><span class="sxs-lookup"><span data-stu-id="c6d4b-176">hello task is retried if it exits with a nonzero exit code, up toohello specified MaxTaskRetryCount</span></span>|
