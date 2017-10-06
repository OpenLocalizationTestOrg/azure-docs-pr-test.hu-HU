---
title: "egyéb feladatok – Azure Batch hello megvalósításának alapján aaaUse feladat függőségek toorun feladatok |} Microsoft Docs"
description: "Egyéb feladatok MapReduce stílus és hasonló big Data típusú adatok feldolgozása hello megvalósításának függő feladatok létrehozása az Azure Batch munkaterhelések."
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
ms.assetid: b8d12db5-ca30-4c7d-993a-a05af9257210
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 05/22/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: faf08ec38cb30b1f66acd51e256c31aea6215c62
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-task-dependencies-toorun-tasks-that-depend-on-other-tasks"></a><span data-ttu-id="8150a-103">A feladat függőségek függenek más feladatok toorun végrehajtó tevékenységek létrehozása</span><span class="sxs-lookup"><span data-stu-id="8150a-103">Create task dependencies toorun tasks that depend on other tasks</span></span>

<span data-ttu-id="8150a-104">Adhat meg a feladat függőségek toorun feladat vagy feladatokhoz csak egy szülő feladat befejeződése után.</span><span class="sxs-lookup"><span data-stu-id="8150a-104">You can define task dependencies toorun a task or set of tasks only after a parent task has completed.</span></span> <span data-ttu-id="8150a-105">Bizonyos esetekben, ahol feladat függőségek hasznosak a következők:</span><span class="sxs-lookup"><span data-stu-id="8150a-105">Some scenarios where task dependencies are useful include:</span></span>

* <span data-ttu-id="8150a-106">MapReduce-stílusú munkaterhelések hello felhőben.</span><span class="sxs-lookup"><span data-stu-id="8150a-106">MapReduce-style workloads in hello cloud.</span></span>
* <span data-ttu-id="8150a-107">Feladatok, amelyek adatfeldolgozási feladatok irányított aciklikus diagramhoz (DAG) jelöl.</span><span class="sxs-lookup"><span data-stu-id="8150a-107">Jobs whose data processing tasks can be expressed as a directed acyclic graph (DAG).</span></span>
* <span data-ttu-id="8150a-108">Megjelenítés előtti és utáni megjelenítési folyamatok, ahol minden feladatot kell végeznie hello következő feladata megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="8150a-108">Pre-rendering and post-rendering processes, where each task must complete before hello next task can begin.</span></span>
* <span data-ttu-id="8150a-109">Ahol alárendelt feladatok függ hello kimeneti a felsőbb rétegbeli feladatok másik feladat.</span><span class="sxs-lookup"><span data-stu-id="8150a-109">Any other job in which downstream tasks depend on hello output of upstream tasks.</span></span>

<span data-ttu-id="8150a-110">Kötegelt feladat függőségek egy vagy több szülő feladatok hello befejezése után a számítási csomópontok végrehajtásra ütemezett feladatok hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="8150a-110">With Batch task dependencies, you can create tasks that are scheduled for execution on compute nodes after hello completion of one or more parent tasks.</span></span> <span data-ttu-id="8150a-111">Például létrehozhat egy feladatot jeleníti meg az egyes különálló, párhuzamos feladatok 3D film keretében.</span><span class="sxs-lookup"><span data-stu-id="8150a-111">For example, you can create a job that renders each frame of a 3D movie with separate, parallel tasks.</span></span> <span data-ttu-id="8150a-112">hello végső feladat – hello "merge feladat"--keretek megjelenítése a teljes movie hello hello összes keretek összevonása lett sikeresen tette.</span><span class="sxs-lookup"><span data-stu-id="8150a-112">hello final task--hello "merge task"--merges hello rendered frames into hello complete movie only after all frames have been successfully rendered.</span></span>

<span data-ttu-id="8150a-113">Alapértelmezés szerint a függő feladatok ütemezése a végrehajtás csak hello szülő feladat sikeres befejeződése után.</span><span class="sxs-lookup"><span data-stu-id="8150a-113">By default, dependent tasks are scheduled for execution only after hello parent task has completed successfully.</span></span> <span data-ttu-id="8150a-114">Adjon meg egy függőségi toooverride hello alapértelmezett viselkedését, és hello szülő feladat sikertelensége feladatok futtatásához.</span><span class="sxs-lookup"><span data-stu-id="8150a-114">You can specify a dependency action toooverride hello default behavior and run tasks when hello parent task fails.</span></span> <span data-ttu-id="8150a-115">Lásd: hello [függőségi műveletek](#dependency-actions) című szakaszban talál információt.</span><span class="sxs-lookup"><span data-stu-id="8150a-115">See hello [Dependency actions](#dependency-actions) section for details.</span></span>  

<span data-ttu-id="8150a-116">Létrehozhat egy az egyhez típusú vagy egy-a-többhöz kapcsolat más feladatok függő feladatok.</span><span class="sxs-lookup"><span data-stu-id="8150a-116">You can create tasks that depend on other tasks in a one-to-one or one-to-many relationship.</span></span> <span data-ttu-id="8150a-117">Ahol feladat függ-e a tevékenységazonosítók adott tartományon belüli feladatok csoport hello megvalósításának tartomány függőséget is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="8150a-117">You can also create a range dependency where a task depends on hello completion of a group of tasks within a specified range of task IDs.</span></span> <span data-ttu-id="8150a-118">Ilyen három alapvető forgatókönyv toocreate több-a-többhöz kapcsolatok kombinálhatja.</span><span class="sxs-lookup"><span data-stu-id="8150a-118">You can combine these three basic scenarios toocreate many-to-many relationships.</span></span>

## <a name="task-dependencies-with-batch-net"></a><span data-ttu-id="8150a-119">A Batch .NET tevékenység függőségei</span><span class="sxs-lookup"><span data-stu-id="8150a-119">Task dependencies with Batch .NET</span></span>
<span data-ttu-id="8150a-120">Ez a cikk arról lesz szó hogyan tooconfigure feladat függőségek használatával hello [Batch .NET] [ net_msdn] könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="8150a-120">In this article, we discuss how tooconfigure task dependencies by using hello [Batch .NET][net_msdn] library.</span></span> <span data-ttu-id="8150a-121">Először megmutatjuk, hogyan túl[feladatütemezés-függőség engedélyezéséhez](#enable-task-dependencies) a feladatokra és majd bemutatják, hogyan túl[feladat konfigurálása függőségekkel rendelkező](#create-dependent-tasks).</span><span class="sxs-lookup"><span data-stu-id="8150a-121">We first show you how too[enable task dependency](#enable-task-dependencies) on your jobs, and then demonstrate how too[configure a task with dependencies](#create-dependent-tasks).</span></span> <span data-ttu-id="8150a-122">Azt is leírják, hogyan toospecify egy függőségi művelet toorun függő feladatok hello szülő meghibásodásakor.</span><span class="sxs-lookup"><span data-stu-id="8150a-122">We also describe how toospecify a dependency action toorun dependent tasks if hello parent fails.</span></span> <span data-ttu-id="8150a-123">Végezetül arról lesz szó hello [függőségi forgatókönyvek](#dependency-scenarios) , amely támogatja a kötegelt.</span><span class="sxs-lookup"><span data-stu-id="8150a-123">Finally, we discuss hello [dependency scenarios](#dependency-scenarios) that Batch supports.</span></span>

## <a name="enable-task-dependencies"></a><span data-ttu-id="8150a-124">A feladat függőségek engedélyezése</span><span class="sxs-lookup"><span data-stu-id="8150a-124">Enable task dependencies</span></span>
<span data-ttu-id="8150a-125">a kötegelt kérelem toouse feladat függőségeit, először konfigurálnia kell hello feladat toouse feladat függőségek.</span><span class="sxs-lookup"><span data-stu-id="8150a-125">toouse task dependencies in your Batch application, you must first configure hello job toouse task dependencies.</span></span> <span data-ttu-id="8150a-126">A Batch .NET engedélyezni a [CloudJob] [ net_cloudjob] úgy, hogy a [UsesTaskDependencies] [ net_usestaskdependencies] tulajdonság túl`true`:</span><span class="sxs-lookup"><span data-stu-id="8150a-126">In Batch .NET, enable it on your [CloudJob][net_cloudjob] by setting its [UsesTaskDependencies][net_usestaskdependencies] property too`true`:</span></span>

```csharp
CloudJob unboundJob = batchClient.JobOperations.CreateJob( "job001",
    new PoolInformation { PoolId = "pool001" });

// IMPORTANT: This is REQUIRED for using task dependencies.
unboundJob.UsesTaskDependencies = true;
```

<span data-ttu-id="8150a-127">Hello megelőző kódrészletet, a "batchClient" példánya: hello [BatchClient] [ net_batchclient] osztály.</span><span class="sxs-lookup"><span data-stu-id="8150a-127">In hello preceding code snippet, "batchClient" is an instance of hello [BatchClient][net_batchclient] class.</span></span>

## <a name="create-dependent-tasks"></a><span data-ttu-id="8150a-128">Függő tevékenységek létrehozása</span><span class="sxs-lookup"><span data-stu-id="8150a-128">Create dependent tasks</span></span>
<span data-ttu-id="8150a-129">toocreate egy feladatot, amely egy vagy több szülő feladatok hello megvalósításának függ, is megadhat, amely hello feladat "függ a" hello más feladatok.</span><span class="sxs-lookup"><span data-stu-id="8150a-129">toocreate a task that depends on hello completion of one or more parent tasks, you can specify that hello task "depends on" hello other tasks.</span></span> <span data-ttu-id="8150a-130">A Batch .NET, konfigurálja a hello [CloudTask][net_cloudtask].[ DependsOn] [ net_dependson] hello példányának tulajdonság [TaskDependencies] [ net_taskdependencies] osztály:</span><span class="sxs-lookup"><span data-stu-id="8150a-130">In Batch .NET, configure hello [CloudTask][net_cloudtask].[DependsOn][net_dependson] property with an instance of hello [TaskDependencies][net_taskdependencies] class:</span></span>

```csharp
// Task 'Flowers' depends on completion of both 'Rain' and 'Sun'
// before it is run.
new CloudTask("Flowers", "cmd.exe /c echo Flowers")
{
    DependsOn = TaskDependencies.OnIds("Rain", "Sun")
},
```

<span data-ttu-id="8150a-131">A kódrészlet egy függő feladat tevékenység azonosítója "Virág" hoz létre.</span><span class="sxs-lookup"><span data-stu-id="8150a-131">This code snippet creates a dependent task with task ID "Flowers".</span></span> <span data-ttu-id="8150a-132">hello "Virág" feladat függ feladatok "Eső" és "Sun".</span><span class="sxs-lookup"><span data-stu-id="8150a-132">hello "Flowers" task depends on tasks "Rain" and "Sun".</span></span> <span data-ttu-id="8150a-133">"Virág" tevékenység csak a "Eső" és "Sun" sikeresen befejeződött feladatok után lesznek ütemezett toorun adott számítási csomóponton.</span><span class="sxs-lookup"><span data-stu-id="8150a-133">Task "Flowers" will be scheduled toorun on a compute node only after tasks "Rain" and "Sun" have completed successfully.</span></span>

> [!NOTE]
> <span data-ttu-id="8150a-134">Egy feladat tekinthető toobe sikeresen befejeződött, a hello van **befejeződött** állapot és a **kilépési kód** van `0`.</span><span class="sxs-lookup"><span data-stu-id="8150a-134">A task is considered toobe completed successfully when it is in hello **completed** state and its **exit code** is `0`.</span></span> <span data-ttu-id="8150a-135">A Batch .NET, ez azt jelenti, hogy egy [CloudTask][net_cloudtask].[ Állapot] [ net_taskstate] tulajdonság értékének `Completed` és CloudTask tartozó hello [TaskExecutionInformation][net_taskexecutioninformation].[ ExitCode] [ net_exitcode] tulajdonság értéke `0`.</span><span class="sxs-lookup"><span data-stu-id="8150a-135">In Batch .NET, this means a [CloudTask][net_cloudtask].[State][net_taskstate] property value of `Completed` and hello CloudTask's [TaskExecutionInformation][net_taskexecutioninformation].[ExitCode][net_exitcode] property value is `0`.</span></span>
> 
> 

## <a name="dependency-scenarios"></a><span data-ttu-id="8150a-136">A függőségi forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="8150a-136">Dependency scenarios</span></span>
<span data-ttu-id="8150a-137">Alapszintű feladat függőségi három forgatókönyv esetén használhatja az Azure Batch:-az-egyhez, egy-a-többhöz és Feladatazonosítót között függőség.</span><span class="sxs-lookup"><span data-stu-id="8150a-137">There are three basic task dependency scenarios that you can use in Azure Batch: one-to-one, one-to-many, and task ID range dependency.</span></span> <span data-ttu-id="8150a-138">Kombinált tooprovide egy negyedik forgatókönyv, több-a-többhöz is lehetnek.</span><span class="sxs-lookup"><span data-stu-id="8150a-138">These can be combined tooprovide a fourth scenario, many-to-many.</span></span>

| <span data-ttu-id="8150a-139">A forgatókönyv&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="sxs-lookup"><span data-stu-id="8150a-139">Scenario&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span></span> | <span data-ttu-id="8150a-140">Példa</span><span class="sxs-lookup"><span data-stu-id="8150a-140">Example</span></span> |  |
|:---:| --- | --- |
|  [<span data-ttu-id="8150a-141">-Az-egyhez</span><span class="sxs-lookup"><span data-stu-id="8150a-141">One-to-one</span></span>](#one-to-one) |<span data-ttu-id="8150a-142">*taskB* függ *taskA*</span><span class="sxs-lookup"><span data-stu-id="8150a-142">*taskB* depends on *taskA*</span></span> <p/> <span data-ttu-id="8150a-143">*taskB* végrehajtásra, amíg nem ütemezi *taskA* sikeresen befejeződött</span><span class="sxs-lookup"><span data-stu-id="8150a-143">*taskB* will not be scheduled for execution until *taskA* has completed successfully</span></span> |<span data-ttu-id="8150a-144">![Ábra: a feladat egy az egyhez típusú függőség][1]</span><span class="sxs-lookup"><span data-stu-id="8150a-144">![Diagram: one-to-one task dependency][1]</span></span> |
|  [<span data-ttu-id="8150a-145">Egy-a-többhöz</span><span class="sxs-lookup"><span data-stu-id="8150a-145">One-to-many</span></span>](#one-to-many) |<span data-ttu-id="8150a-146">A *taskC* a *taskA* és a *taskB* tevékenységtől is függ</span><span class="sxs-lookup"><span data-stu-id="8150a-146">*taskC* depends on both *taskA* and *taskB*</span></span> <p/> <span data-ttu-id="8150a-147">*taskC* végrehajtásra, amíg nem ütemezi *taskA* és *taskB* sikeresen befejeződött</span><span class="sxs-lookup"><span data-stu-id="8150a-147">*taskC* will not be scheduled for execution until both *taskA* and *taskB* have completed successfully</span></span> |<span data-ttu-id="8150a-148">![Ábra: egy-a-többhöz feladat függőség][2]</span><span class="sxs-lookup"><span data-stu-id="8150a-148">![Diagram: one-to-many task dependency][2]</span></span> |
|  [<span data-ttu-id="8150a-149">A feladat Azonosítótartományának kezdete</span><span class="sxs-lookup"><span data-stu-id="8150a-149">Task ID range</span></span>](#task-id-range) |<span data-ttu-id="8150a-150">*taskD* feladatok számos függ</span><span class="sxs-lookup"><span data-stu-id="8150a-150">*taskD* depends on a range of tasks</span></span> <p/> <span data-ttu-id="8150a-151">*taskD* végrehajtásra, amíg hello feladatok-azonosítók nem ütemezi *1* keresztül *10* sikeresen befejeződött</span><span class="sxs-lookup"><span data-stu-id="8150a-151">*taskD* will not be scheduled for execution until hello tasks with IDs *1* through *10* have completed successfully</span></span> |<span data-ttu-id="8150a-152">![Ábra: Feladat azonosítója tartomány függőség][3]</span><span class="sxs-lookup"><span data-stu-id="8150a-152">![Diagram: Task id range dependency][3]</span></span> |

> [!TIP]
> <span data-ttu-id="8150a-153">Létrehozhat **több-a-többhöz** kapcsolatokat, például ha C, D, E, és F egyes feladatok függ-e A és b feladatok Ez akkor hasznos, ahol az alsóbb rétegbeli feladatok függ-e több felsőbb szintű tevékenység kimenete hello párhuzamos működésű előfeldolgozási forgatókönyvek például a.</span><span class="sxs-lookup"><span data-stu-id="8150a-153">You can create **many-to-many** relationships, such as where tasks C, D, E, and F each depend on tasks A and B. This is useful, for example, in parallelized preprocessing scenarios where your downstream tasks depend on hello output of multiple upstream tasks.</span></span>
> 
> <span data-ttu-id="8150a-154">Ebben a szakaszban szereplő példák hello, a függő fusson a feladat csak azután hello szülő feladat sikeresen befejeződik.</span><span class="sxs-lookup"><span data-stu-id="8150a-154">In hello examples in this section, a dependent task runs only after hello parent tasks complete successfully.</span></span> <span data-ttu-id="8150a-155">Ez a viselkedés hello alapértelmezett viselkedését egy függő feladat.</span><span class="sxs-lookup"><span data-stu-id="8150a-155">This behavior is hello default behavior for a dependent task.</span></span> <span data-ttu-id="8150a-156">Egy függő feladat futtatása után egy szülő feladatot nem sikerül, egy függőségi toooverride hello alapértelmezett viselkedését megadásával lehetséges.</span><span class="sxs-lookup"><span data-stu-id="8150a-156">You can run a dependent task after a parent task fails by specifying a dependency action toooverride hello default behavior.</span></span> <span data-ttu-id="8150a-157">Lásd: hello [függőségi műveletek](#dependency-actions) című szakaszban talál információt.</span><span class="sxs-lookup"><span data-stu-id="8150a-157">See hello [Dependency actions](#dependency-actions) section for details.</span></span>

### <a name="one-to-one"></a><span data-ttu-id="8150a-158">-Az-egyhez</span><span class="sxs-lookup"><span data-stu-id="8150a-158">One-to-one</span></span>
<span data-ttu-id="8150a-159">Egy az egyhez kapcsolat, a feladat egy szülő tevékenység sikeres befejezésének hello függ.</span><span class="sxs-lookup"><span data-stu-id="8150a-159">In a one-to-one relationship, a task depends on hello successful completion of one parent task.</span></span> <span data-ttu-id="8150a-160">toocreate hello függőségi, adjon meg egy adott feladat azonosító toohello [TaskDependencies][net_taskdependencies].[ OnId] [ net_onid] statikus metódus amikor hello feltölti [DependsOn] [ net_dependson] tulajdonsága [CloudTask] [ net_cloudtask].</span><span class="sxs-lookup"><span data-stu-id="8150a-160">toocreate hello dependency, provide a single task ID toohello [TaskDependencies][net_taskdependencies].[OnId][net_onid] static method when you populate hello [DependsOn][net_dependson] property of [CloudTask][net_cloudtask].</span></span>

```csharp
// Task 'taskA' doesn't depend on any other tasks
new CloudTask("taskA", "cmd.exe /c echo taskA"),

// Task 'taskB' depends on completion of task 'taskA'
new CloudTask("taskB", "cmd.exe /c echo taskB")
{
    DependsOn = TaskDependencies.OnId("taskA")
},
```

### <a name="one-to-many"></a><span data-ttu-id="8150a-161">Egy-a-többhöz</span><span class="sxs-lookup"><span data-stu-id="8150a-161">One-to-many</span></span>
<span data-ttu-id="8150a-162">Egy-a-többhöz kapcsolat, a feladat hello befejezésekor, több szülő feladat függ.</span><span class="sxs-lookup"><span data-stu-id="8150a-162">In a one-to-many relationship, a task depends on hello completion of multiple parent tasks.</span></span> <span data-ttu-id="8150a-163">toocreate hello függőségi, adja meg a feladat azonosítók toohello gyűjteménye [TaskDependencies][net_taskdependencies].[ OnIds] [ net_onids] statikus metódus amikor hello feltölti [DependsOn] [ net_dependson] tulajdonsága [CloudTask] [ net_cloudtask].</span><span class="sxs-lookup"><span data-stu-id="8150a-163">toocreate hello dependency, provide a collection of task IDs toohello [TaskDependencies][net_taskdependencies].[OnIds][net_onids] static method when you populate hello [DependsOn][net_dependson] property of [CloudTask][net_cloudtask].</span></span>

```csharp
// 'Rain' and 'Sun' don't depend on any other tasks
new CloudTask("Rain", "cmd.exe /c echo Rain"),
new CloudTask("Sun", "cmd.exe /c echo Sun"),

// Task 'Flowers' depends on completion of both 'Rain' and 'Sun'
// before it is run.
new CloudTask("Flowers", "cmd.exe /c echo Flowers")
{
    DependsOn = TaskDependencies.OnIds("Rain", "Sun")
},
``` 

### <a name="task-id-range"></a><span data-ttu-id="8150a-164">A feladat Azonosítótartományának kezdete</span><span class="sxs-lookup"><span data-stu-id="8150a-164">Task ID range</span></span>
<span data-ttu-id="8150a-165">A szülő feladatok széles függ, a feladat hello hello megvalósításának feladatok széles találhatók, amelynek azonosítók függ.</span><span class="sxs-lookup"><span data-stu-id="8150a-165">In a dependency on a range of parent tasks, a task depends on hello hello completion of tasks whose IDs lie within a range.</span></span>
<span data-ttu-id="8150a-166">toocreate hello függőség hello először adja meg, és a legutóbbi feladat a hello tartomány toohello azonosítók [TaskDependencies][net_taskdependencies].[ OnIdRange] [ net_onidrange] statikus metódus amikor hello feltölti [DependsOn] [ net_dependson] tulajdonsága [CloudTask] [net_cloudtask].</span><span class="sxs-lookup"><span data-stu-id="8150a-166">toocreate hello dependency, provide hello first and last task IDs in hello range toohello [TaskDependencies][net_taskdependencies].[OnIdRange][net_onidrange] static method when you populate hello [DependsOn][net_dependson] property of [CloudTask][net_cloudtask].</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8150a-167">Tevékenység azonosítója címtartományok használatakor a függőségek hello hello közé azonosítókat *kell* egész értékek ábrázolásai karakterlánc lehet.</span><span class="sxs-lookup"><span data-stu-id="8150a-167">When you use task ID ranges for your dependencies, hello task IDs in hello range *must* be string representations of integer values.</span></span>
> 
> <span data-ttu-id="8150a-168">Minden tevékenység hello közé hello függőségi, meg kell felelnie a folyamat sikeres végrehajtása vagy a csatlakoztatott tooa függőségi művelet túl beállítása hibával befejezése**Satisfy**.</span><span class="sxs-lookup"><span data-stu-id="8150a-168">Every task in hello range must satisfy hello dependency, either by completing successfully or by completing with a failure that’s mapped tooa dependency action set too**Satisfy**.</span></span> <span data-ttu-id="8150a-169">Lásd: hello [függőségi műveletek](#dependency-actions) című szakaszban talál információt.</span><span class="sxs-lookup"><span data-stu-id="8150a-169">See hello [Dependency actions](#dependency-actions) section for details.</span></span>
>
>

```csharp
// Tasks 1, 2, and 3 don't depend on any other tasks. Because
// we will be using them for a task range dependency, we must
// specify string representations of integers as their ids.
new CloudTask("1", "cmd.exe /c echo 1"),
new CloudTask("2", "cmd.exe /c echo 2"),
new CloudTask("3", "cmd.exe /c echo 3"),

// Task 4 depends on a range of tasks, 1 through 3
new CloudTask("4", "cmd.exe /c echo 4")
{
    // toouse a range of tasks, their ids must be integer values.
    // Note that we pass integers as parameters tooTaskIdRange,
    // but their ids (above) are string representations of hello ids.
    DependsOn = TaskDependencies.OnIdRange(1, 3)
},
```

## <a name="dependency-actions"></a><span data-ttu-id="8150a-170">A függőségi műveletek</span><span class="sxs-lookup"><span data-stu-id="8150a-170">Dependency actions</span></span>

<span data-ttu-id="8150a-171">Alapértelmezés szerint egy függő feladat vagy feladatokhoz fut csak egy szülő feladat sikeres befejeződése után.</span><span class="sxs-lookup"><span data-stu-id="8150a-171">By default, a dependent task or set of tasks runs only after a parent task has completed successfully.</span></span> <span data-ttu-id="8150a-172">Bizonyos esetekben érdemes lehet toorun függő tevékenységek akkor is, ha hello szülő feladat sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="8150a-172">In some scenarios, you may want toorun dependent tasks even if hello parent task fails.</span></span> <span data-ttu-id="8150a-173">Hello alapértelmezett viselkedést felülírhatja egy függőségi művelet megadását.</span><span class="sxs-lookup"><span data-stu-id="8150a-173">You can override hello default behavior by specifying a dependency action.</span></span> <span data-ttu-id="8150a-174">A függőség művelet határozza meg, hogy a függő feladatok jogosult toorun hello sikerességét vagy sikertelenségét hello Szülőtevékenység alapján.</span><span class="sxs-lookup"><span data-stu-id="8150a-174">A dependency action specifies whether a dependent task is eligible toorun, based on hello success or failure of hello parent task.</span></span> 

<span data-ttu-id="8150a-175">Tegyük fel, hogy a függő feladatok vár hello fölérendelt feladatok végrehajtása hello adatait.</span><span class="sxs-lookup"><span data-stu-id="8150a-175">For example, suppose that a dependent task is awaiting data from hello completion of hello upstream task.</span></span> <span data-ttu-id="8150a-176">Hello felsőbb szintű tevékenység sikertelen lesz, ha hello függő feladat még lehet képes toorun régebbi adatok felhasználásával.</span><span class="sxs-lookup"><span data-stu-id="8150a-176">If hello upstream task fails, hello dependent task may still be able toorun using older data.</span></span> <span data-ttu-id="8150a-177">Ebben az esetben egy függőségi művelet megadható hello függő tevékenység jogosult toorun annak ellenére, hogy hello Szülőtevékenység hello sikertelen.</span><span class="sxs-lookup"><span data-stu-id="8150a-177">In this case, a dependency action can specify that hello dependent task is eligible toorun despite hello failure of hello parent task.</span></span>

<span data-ttu-id="8150a-178">A függőség művelet egy kilépési feltétel hello szülő feladat alapul.</span><span class="sxs-lookup"><span data-stu-id="8150a-178">A dependency action is based on an exit condition for hello parent task.</span></span> <span data-ttu-id="8150a-179">Megadhat egy függőségi művelet bármely hello a következő kilépési feltételeit; a .NET-hez, lásd: hello [ExitConditions] [ net_exitconditions] osztály további részletek:</span><span class="sxs-lookup"><span data-stu-id="8150a-179">You can specify a dependency action for any of hello following exit conditions; for .NET, see hello [ExitConditions][net_exitconditions] class for details:</span></span>

- <span data-ttu-id="8150a-180">Ha egy előre feldolgozási hiba történik.</span><span class="sxs-lookup"><span data-stu-id="8150a-180">When a pre-processing error occurs.</span></span>
- <span data-ttu-id="8150a-181">Hiba akkor fordul elő, amikor egy fájl feltöltése.</span><span class="sxs-lookup"><span data-stu-id="8150a-181">When a file upload error occurs.</span></span> <span data-ttu-id="8150a-182">Ha hello feladat keresztül megadott kilépési kóddal kilép **exitCodes** vagy **exitCodeRanges**, majd észlel, a fájl feltöltése hiba, hello kilépési kód elsőbbséget élvez által meghatározott hello műveletet.</span><span class="sxs-lookup"><span data-stu-id="8150a-182">If hello task exits with an exit code that was specified via **exitCodes** or **exitCodeRanges**, and then encounters a file upload error, hello action specified by hello exit code takes precedence.</span></span>
- <span data-ttu-id="8150a-183">Ha kilép, hello feladat hello által megadott kilépési kóddal **ExitCodes** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="8150a-183">When hello task exits with an exit code defined by hello **ExitCodes** property.</span></span>
- <span data-ttu-id="8150a-184">Ha kilép, hello feladat hello megadott tartományba esik kilépési kóddal **ExitCodeRanges** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="8150a-184">When hello task exits with an exit code that falls within a range specified by hello **ExitCodeRanges** property.</span></span>
- <span data-ttu-id="8150a-185">hello alapértelmezett esetben, ha hello feladat kilép, nem definiált kilépési kóddal **ExitCodes** vagy **ExitCodeRanges**, vagy ha hello feladat egy előre feldolgozási hiba és hello kilép **PreProcessingError**  tulajdonsága nincs beállítva, vagy ha hello feladat meghiúsul a fájl feltöltése hiba és hello **FileUploadError** tulajdonsága nincs beállítva.</span><span class="sxs-lookup"><span data-stu-id="8150a-185">hello default case, if hello task exits with an exit code not defined by **ExitCodes** or **ExitCodeRanges**, or if hello task exits with a pre-processing error and hello **PreProcessingError** property is not set, or if hello task fails with a file upload error and hello **FileUploadError** property is not set.</span></span> 

<span data-ttu-id="8150a-186">a .NET, a set hello egy függőségi művelet toospecify [ExitOptions][net_exitoptions].[ DependencyAction] [ net_dependencyaction] hello kilépési feltétel tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="8150a-186">toospecify a dependency action in .NET, set hello [ExitOptions][net_exitoptions].[DependencyAction][net_dependencyaction] property for hello exit condition.</span></span> <span data-ttu-id="8150a-187">Hello **DependencyAction** tulajdonság szükséges egy vagy két értéket:</span><span class="sxs-lookup"><span data-stu-id="8150a-187">hello **DependencyAction** property takes one of two values:</span></span>

- <span data-ttu-id="8150a-188">A beállítás hello **DependencyAction** tulajdonság túl**Satisfy** azt jelzi, hogy a függő feladatok jogosult toorun Ha hello Szülőtevékenység megadott hiba miatt kilép.</span><span class="sxs-lookup"><span data-stu-id="8150a-188">Setting hello **DependencyAction** property too**Satisfy** indicates that dependent tasks are eligible toorun if hello parent task exits with a specified error.</span></span>
- <span data-ttu-id="8150a-189">A beállítás hello **DependencyAction** tulajdonság túl**blokk** azt jelzi, hogy a függő feladatok nem jogosult toorun.</span><span class="sxs-lookup"><span data-stu-id="8150a-189">Setting hello **DependencyAction** property too**Block** indicates that dependent tasks are not eligible toorun.</span></span>

<span data-ttu-id="8150a-190">Alapértelmezés szerint hello hello **DependencyAction** tulajdonság **Satisfy** a kilépési kód: 0, és **blokk** minden más kilépési feltételeket.</span><span class="sxs-lookup"><span data-stu-id="8150a-190">hello default setting for hello **DependencyAction** property is **Satisfy** for exit code 0, and **Block** for all other exit conditions.</span></span>

<span data-ttu-id="8150a-191">hello következő kódrészletet beállítja hello **DependencyAction** tulajdonság a szülő feladathoz.</span><span class="sxs-lookup"><span data-stu-id="8150a-191">hello following code snippet sets hello **DependencyAction** property for a parent task.</span></span> <span data-ttu-id="8150a-192">Ha hello Szülőtevékenység előre feldolgozási hiba miatt kilép, vagy a hello megadott hibakódok, függő hello feladat le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="8150a-192">If hello parent task exits with a pre-processing error, or with hello specified error codes, hello dependent task is blocked.</span></span> <span data-ttu-id="8150a-193">Ha hello Szülőtevékenység más nem nulla hiba miatt kilép, a hello függő tevékenység nem jogosult toorun.</span><span class="sxs-lookup"><span data-stu-id="8150a-193">If hello parent task exits with any other non-zero error, hello dependent task is eligible toorun.</span></span>

```csharp
// Task A is hello parent task.
new CloudTask("A", "cmd.exe /c echo A")
{
    // Specify exit conditions for task A and their dependency actions.
    ExitConditions = new ExitConditions
    {
        // If task A exits with a pre-processing error, block any downstream tasks (in this example, task B).
        PreProcessingError = new ExitOptions
        {
            DependencyAction = DependencyAction.Block
        },
        // If task A exits with hello specified error codes, block any downstream tasks (in this example, task B).
        ExitCodes = new List<ExitCodeMapping>
        {
            new ExitCodeMapping(10, new ExitOptions() { DependencyAction = DependencyAction.Block }),
            new ExitCodeMapping(20, new ExitOptions() { DependencyAction = DependencyAction.Block })
        },
        // If task A succeeds or fails with any other error, any downstream tasks become eligible toorun 
        // (in this example, task B).
        Default = new ExitOptions
        {
            DependencyAction = DependencyAction.Satisfy
        }
    }
},
// Task B depends on task A. Whether it becomes eligible toorun depends on how task A exits.
new CloudTask("B", "cmd.exe /c echo B")
{
    DependsOn = TaskDependencies.OnId("A")
},
```

## <a name="code-sample"></a><span data-ttu-id="8150a-194">Kódminta</span><span class="sxs-lookup"><span data-stu-id="8150a-194">Code sample</span></span>
<span data-ttu-id="8150a-195">Hello [TaskDependencies] [ github_taskdependencies] mintaprojektet egyike hello [Azure Batch-Kódminták] [ github_samples] a Githubon.</span><span class="sxs-lookup"><span data-stu-id="8150a-195">hello [TaskDependencies][github_taskdependencies] sample project is one of hello [Azure Batch code samples][github_samples] on GitHub.</span></span> <span data-ttu-id="8150a-196">A Visual Studio megoldás mutatja be:</span><span class="sxs-lookup"><span data-stu-id="8150a-196">This Visual Studio solution demonstrates:</span></span>

- <span data-ttu-id="8150a-197">Hogyan tooenable feladat olyan feladaton függőségi</span><span class="sxs-lookup"><span data-stu-id="8150a-197">How tooenable task dependency on a job</span></span>
- <span data-ttu-id="8150a-198">Hogyan toocreate feladatok, más feladatok függ</span><span class="sxs-lookup"><span data-stu-id="8150a-198">How toocreate tasks that depend on other tasks</span></span>
- <span data-ttu-id="8150a-199">Hogyan tooexecute a feladatokat a számítási csomópontok készlete.</span><span class="sxs-lookup"><span data-stu-id="8150a-199">How tooexecute those tasks on a pool of compute nodes.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8150a-200">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8150a-200">Next steps</span></span>
### <a name="application-deployment"></a><span data-ttu-id="8150a-201">Alkalmazás központi telepítése</span><span class="sxs-lookup"><span data-stu-id="8150a-201">Application deployment</span></span>
<span data-ttu-id="8150a-202">Hello [alkalmazáscsomagok](batch-application-packages.md) kötegelt funkciójával egy tooboth telepítéséhez egyszerűen és hello alkalmazások, amelyek a feladatok végrehajtása a számítási csomópontok verziója.</span><span class="sxs-lookup"><span data-stu-id="8150a-202">hello [application packages](batch-application-packages.md) feature of Batch provides an easy way tooboth deploy and version hello applications that your tasks execute on compute nodes.</span></span>

### <a name="installing-applications-and-staging-data"></a><span data-ttu-id="8150a-203">Alkalmazások telepítése és átmeneti adatokat</span><span class="sxs-lookup"><span data-stu-id="8150a-203">Installing applications and staging data</span></span>
<span data-ttu-id="8150a-204">Lásd: [alkalmazás telepítését, és átmeneti adatokat a Batch számítási csomópontjain] [ forum_post] hello Azure Batch fórum való felkészülés a csomópontok toorun feladatok módszerek áttekintését.</span><span class="sxs-lookup"><span data-stu-id="8150a-204">See [Installing applications and staging data on Batch compute nodes][forum_post] in hello Azure Batch forum for an overview of methods for preparing your nodes toorun tasks.</span></span> <span data-ttu-id="8150a-205">Írt egy hello Azure Batch csoport tagjai, vagy a feladás egy vagy több egy jó ismertetése a hello különböző módokon toocopy alkalmazásokkal, a feladat bemeneti adatok és egyéb fájlok tooyour számítási csomópontjain.</span><span class="sxs-lookup"><span data-stu-id="8150a-205">Written by one of hello Azure Batch team members, this post is a good primer on hello different ways toocopy applications, task input data, and other files tooyour compute nodes.</span></span>

[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[github_taskdependencies]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/TaskDependencies
[github_samples]: https://github.com/Azure/azure-batch-samples
[net_batchclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_dependson]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.dependson.aspx
[net_exitcode]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskexecutioninformation.exitcode.aspx
[net_exitconditions]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.exitconditions
[net_exitoptions]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.exitoptions
[net_dependencyaction]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.exitoptions#Microsoft_Azure_Batch_ExitOptions_DependencyAction
[net_msdn]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[net_onid]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onid.aspx
[net_onids]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onids.aspx
[net_onidrange]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onidrange.aspx
[net_taskexecutioninformation]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskexecutioninformation.aspx
[net_taskstate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.taskstate.aspx
[net_usestaskdependencies]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.usestaskdependencies.aspx
[net_taskdependencies]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskdependencies.aspx

[1]: ./media/batch-task-dependency/01_one_to_one.png "Diagramja: egy az egyhez típusú függőség"
[2]: ./media/batch-task-dependency/02_one_to_many.png "Ábra: egy-a-többhöz függőség"
[3]: ./media/batch-task-dependency/03_task_id_range.png "Ábra: feladat azonosítója tartomány függőség"
