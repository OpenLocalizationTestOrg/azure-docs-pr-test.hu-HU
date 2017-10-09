---
title: "aaaCreate feladatok tooprepare feladatok és a számítási csomópontok - Azure Batch egy teljes feladat |} Microsoft Docs"
description: "Feladat szintű előkészítő feladatok toominimize adatokat használjon átviteli tooAzure Batch számítási csomópontokat, és kiadással csomópont karbantartási feladat befejezésére, a feladatok."
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
ms.assetid: 63d9d4f1-8521-4bbb-b95a-c4cad73692d3
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 02/27/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: fd5fb47ae6700281e63048c49a1241f4e935baba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="run-job-preparation-and-job-release-tasks-on-batch-compute-nodes"></a><span data-ttu-id="d162b-103">Futtatási feladat előkészítése és a feladat kiadási tevékenységek a Batch számítási csomópontjain</span><span class="sxs-lookup"><span data-stu-id="d162b-103">Run job preparation and job release tasks on Batch compute nodes</span></span>

 <span data-ttu-id="d162b-104">Egy Azure kötegelt gyakran néhány adatot kér a telepítő a feladatokat hajtja végre a rendszer, és karbantartási utáni feladat, a feladatok befejezése előtt.</span><span class="sxs-lookup"><span data-stu-id="d162b-104">An Azure Batch job often requires some form of setup before its tasks are executed, and post-job maintenance when its tasks are completed.</span></span> <span data-ttu-id="d162b-105">Lehet, hogy a kell toodownload közös tevékenység bemeneti adatok tooyour számítási csomópontokat, vagy töltse fel a tevékenység kimeneti adatok tooAzure tárolási hello feladat befejezése után.</span><span class="sxs-lookup"><span data-stu-id="d162b-105">You might need toodownload common task input data tooyour compute nodes, or upload task output data tooAzure Storage after hello job completes.</span></span> <span data-ttu-id="d162b-106">Használhat **előkészítő feladat** és **kiadás feladat** feladatok tooperform ezeket a műveleteket.</span><span class="sxs-lookup"><span data-stu-id="d162b-106">You can use **job preparation** and **job release** tasks tooperform these operations.</span></span>

## <a name="what-are-job-preparation-and-release-tasks"></a><span data-ttu-id="d162b-107">Mi feladat előkészítése és a feladatok kiadási?</span><span class="sxs-lookup"><span data-stu-id="d162b-107">What are job preparation and release tasks?</span></span>
<span data-ttu-id="d162b-108">A feladat tevékenységeit futtatja, mielőtt hello feladat előkészítése tevékenységet a futó minden számítási csomópont ütemezett toorun legalább egy feladat.</span><span class="sxs-lookup"><span data-stu-id="d162b-108">Before a job's tasks run, hello job preparation task runs on all compute nodes scheduled toorun at least one task.</span></span> <span data-ttu-id="d162b-109">Ha hello feladat befejeződött, hello feladat kiadása tevékenység hello-készlet, amely legalább egy feladat végrehajtása minden egyes csomópontján fut.</span><span class="sxs-lookup"><span data-stu-id="d162b-109">Once hello job is completed, hello job release task runs on each node in hello pool that executed at least one task.</span></span> <span data-ttu-id="d162b-110">Normál kötegelt feladatok, adjon meg egy parancssori toobe meghívni, amikor a feladat előkészítése, illetve kiadás feladat futtatása.</span><span class="sxs-lookup"><span data-stu-id="d162b-110">As with normal Batch tasks, you can specify a command line toobe invoked when a job preparation or release task is run.</span></span>

<span data-ttu-id="d162b-111">Feladat előkészítése és a kiadási tevékenységek megszokott kötegelt feladat szolgáltatásokat kínálnak például fájl letöltése ([erőforrásfájlok][net_job_prep_resourcefiles]), emelt szintű végrehajtási, egyéni környezeti változókat, végrehajtási maximális időtartam, újrapróbálkozások maximális számát és fájl adatmegőrzési időtartam.</span><span class="sxs-lookup"><span data-stu-id="d162b-111">Job preparation and release tasks offer familiar Batch task features such as file download ([resource files][net_job_prep_resourcefiles]), elevated execution, custom environment variables, maximum execution duration, retry count, and file retention time.</span></span>

<span data-ttu-id="d162b-112">A következő szakaszok hello, megtudhatja, hogyan toouse hello [JobPreparationTask] [ net_job_prep] és [JobReleaseTask] [ net_job_release] hello található osztályok [Batch .NET] [ api_net] könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="d162b-112">In hello following sections, you'll learn how toouse hello [JobPreparationTask][net_job_prep] and [JobReleaseTask][net_job_release] classes found in hello [Batch .NET][api_net] library.</span></span>

> [!TIP]
> <span data-ttu-id="d162b-113">Feladat előkészítése és a kiadási tevékenységek hasznosak lehetnek "megosztás készlet" környezetben, amelyben számítási csomópontok készlete közötti feladat futtatása továbbra is fennáll, és sok feladatok által használt.</span><span class="sxs-lookup"><span data-stu-id="d162b-113">Job preparation and release tasks are especially helpful in "shared pool" environments, in which a pool of compute nodes persists between job runs and is used by many jobs.</span></span>
> 
> 

## <a name="when-toouse-job-preparation-and-release-tasks"></a><span data-ttu-id="d162b-114">Ha a toouse előkészítő feladat, és felszabadíthatja a feladatok</span><span class="sxs-lookup"><span data-stu-id="d162b-114">When toouse job preparation and release tasks</span></span>
<span data-ttu-id="d162b-115">Feladat előkészítése és a feladat kiadása feladatok esetében a remekül beválik, ha a következő helyzetekben hello:</span><span class="sxs-lookup"><span data-stu-id="d162b-115">Job preparation and job release tasks are a good fit for hello following situations:</span></span>

<span data-ttu-id="d162b-116">**Általános feladat adatok letöltése**</span><span class="sxs-lookup"><span data-stu-id="d162b-116">**Download common task data**</span></span>

<span data-ttu-id="d162b-117">Kötegelt feladatok gyakran megkövetelik a közös adatkészletet hello feladat tevékenységeit bemenetként.</span><span class="sxs-lookup"><span data-stu-id="d162b-117">Batch jobs often require a common set of data as input for hello job's tasks.</span></span> <span data-ttu-id="d162b-118">Napi kockázati elemzés számítások, például piaci adatok feladat-specifikus, de közös tooall feladatok hello feladat.</span><span class="sxs-lookup"><span data-stu-id="d162b-118">For example, in daily risk analysis calculations, market data is job-specific, yet common tooall tasks in hello job.</span></span> <span data-ttu-id="d162b-119">A piacon adatok gyakran számos gigabájtig terjedhetnek, így hello csomóponton futó minden feladatot használhatja azt csak egyszer kell letöltött tooeach számítási csomópont.</span><span class="sxs-lookup"><span data-stu-id="d162b-119">This market data, often several gigabytes in size, should be downloaded tooeach compute node only once so that any task that runs on hello node can use it.</span></span> <span data-ttu-id="d162b-120">Használja a **feladat előkészítése tevékenységet** toodownload ezen adatok tooeach csomópont más feladatok hello hello feladat végrehajtása előtt.</span><span class="sxs-lookup"><span data-stu-id="d162b-120">Use a **job preparation task** toodownload this data tooeach node before hello execution of hello job's other tasks.</span></span>

<span data-ttu-id="d162b-121">**Feladat- és kimeneti törlése**</span><span class="sxs-lookup"><span data-stu-id="d162b-121">**Delete job and task output**</span></span>

<span data-ttu-id="d162b-122">"Megosztás készlet" környezetben, ahol nincsenek leszerelt feladatok között a készlet számítási csomópontokat, szükség lehet a feladatadatok toodelete fusson.</span><span class="sxs-lookup"><span data-stu-id="d162b-122">In a "shared pool" environment, where a pool's compute nodes are not decommissioned between jobs, you may need toodelete job data between runs.</span></span> <span data-ttu-id="d162b-123">Előfordulhat, hogy tooconserve hello csomópontok hely a lemezen, vagy megfelelnek a szervezet biztonsági házirendjeivel.</span><span class="sxs-lookup"><span data-stu-id="d162b-123">You might need tooconserve disk space on hello nodes, or satisfy your organization's security policies.</span></span> <span data-ttu-id="d162b-124">Használja a **feladat kiadása tevékenység** tölti le a feladat előkészítése tevékenység, vagy során toodelete adatok feladat végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="d162b-124">Use a **job release task** toodelete data that was downloaded by a job preparation task, or generated during task execution.</span></span>

<span data-ttu-id="d162b-125">**Napló megőrzési**</span><span class="sxs-lookup"><span data-stu-id="d162b-125">**Log retention**</span></span>

<span data-ttu-id="d162b-126">Érdemes lehet tookeep naplófájlokat, amelyek a feladatokat, vagy esetleg összeomlási memóriaképek sikertelen alkalmazások által létrehozott egy példányát.</span><span class="sxs-lookup"><span data-stu-id="d162b-126">You might want tookeep a copy of log files that your tasks generate, or perhaps crash dump files that can be generated by failed applications.</span></span> <span data-ttu-id="d162b-127">Használja a **feladat kiadása tevékenység** az ilyen esetekben toocompress, és töltse fel az adatok tooan [Azure Storage] [ azure_storage] fiók.</span><span class="sxs-lookup"><span data-stu-id="d162b-127">Use a **job release task** in such cases toocompress and upload this data tooan [Azure Storage][azure_storage] account.</span></span>

> [!TIP]
> <span data-ttu-id="d162b-128">Egy másik módja toopersist naplók és egyéb feladat- és kimeneti adatok toouse hello [Azure Batch fájl egyezmények](batch-task-output.md) könyvtár.</span><span class="sxs-lookup"><span data-stu-id="d162b-128">Another way toopersist logs and other job and task output data is toouse hello [Azure Batch File Conventions](batch-task-output.md) library.</span></span>
> 
> 

## <a name="job-preparation-task"></a><span data-ttu-id="d162b-129">Feladat előkészítése tevékenységet</span><span class="sxs-lookup"><span data-stu-id="d162b-129">Job preparation task</span></span>
<span data-ttu-id="d162b-130">Egy feladat feladatok végrehajtása, mielőtt kötegelt hello feladat előkészítése tevékenységet minden számítási csomóponton, amely a feladatok ütemezett toorun hajtja végre.</span><span class="sxs-lookup"><span data-stu-id="d162b-130">Before execution of a job's tasks, Batch executes hello job preparation task on each compute node that is scheduled toorun a task.</span></span> <span data-ttu-id="d162b-131">Alapértelmezés szerint hello Batch szolgáltatás megvárja-e a hello feladat előkészítése tevékenység toobe hello feladatok ütemezett tooexecute hello csomóponton futó előtt végzi el.</span><span class="sxs-lookup"><span data-stu-id="d162b-131">By default, hello Batch service waits for hello job preparation task toobe completed before running hello tasks scheduled tooexecute on hello node.</span></span> <span data-ttu-id="d162b-132">Azonban nem toowait hello szolgáltatást konfigurálhatja.</span><span class="sxs-lookup"><span data-stu-id="d162b-132">However, you can configure hello service not toowait.</span></span> <span data-ttu-id="d162b-133">Ha hello csomópont újraindul, hello feladat előkészítése tevékenységet újra fut, de le is tilthatja ezt a viselkedést.</span><span class="sxs-lookup"><span data-stu-id="d162b-133">If hello node restarts, hello job preparation task runs again, but you can also disable this behavior.</span></span>

<span data-ttu-id="d162b-134">hello feladat előkészítése tevékenységet csak olyan csomópontot, amely ütemezett toorun feladat végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="d162b-134">hello job preparation task is executed only on nodes that are scheduled toorun a task.</span></span> <span data-ttu-id="d162b-135">Ez megakadályozza, hogy hello szükségtelen előkészítő feladat végrehajtásának abban az esetben, ha egy csomópont nem hozzá van rendelve egy feladatot.</span><span class="sxs-lookup"><span data-stu-id="d162b-135">This prevents hello unnecessary execution of a preparation task in case a node is not assigned a task.</span></span> <span data-ttu-id="d162b-136">Ez akkor fordulhat elő, ha a feladat feladatok hello száma nem éri el a készletben található csomópontok számának hello.</span><span class="sxs-lookup"><span data-stu-id="d162b-136">This can occur when hello number of tasks for a job is less than hello number of nodes in a pool.</span></span> <span data-ttu-id="d162b-137">Akkor is érvényes, ha [egyidejű feladat a végrehajtás](batch-parallel-node-tasks.md) engedélyezve van, amely hagyja egyes csomópontok üresjárati Ha hello feladatok száma nem éri el hello teljes lehetséges egyidejű feladatok.</span><span class="sxs-lookup"><span data-stu-id="d162b-137">It also applies when [concurrent task execution](batch-parallel-node-tasks.md) is enabled, which leaves some nodes idle if hello task count is lower than hello total possible concurrent tasks.</span></span> <span data-ttu-id="d162b-138">Nem hello feladat előkészítése tevékenységet a kiszolgálón való futtatásával üresjárati csomópontok, adatok adatátviteli költségek kevesebb pénz képes költeni.</span><span class="sxs-lookup"><span data-stu-id="d162b-138">By not running hello job preparation task on idle nodes, you can spend less money on data transfer charges.</span></span>

> [!NOTE]
> <span data-ttu-id="d162b-139">[JobPreparationTask] [ net_job_prep_cloudjob] eltér az [CloudPool.StartTask] [ pool_starttask] abban a JobPreparationTask hello elején minden feladatot hajt végre, mivel StartTask végrehajtja, csak ha a számítási csomópont először csatlakozik egy készletet vagy újraindul.</span><span class="sxs-lookup"><span data-stu-id="d162b-139">[JobPreparationTask][net_job_prep_cloudjob] differs from [CloudPool.StartTask][pool_starttask] in that JobPreparationTask executes at hello start of each job, whereas StartTask executes only when a compute node first joins a pool or restarts.</span></span>
> 
> 

## <a name="job-release-task"></a><span data-ttu-id="d162b-140">Feladat kiadása tevékenység</span><span class="sxs-lookup"><span data-stu-id="d162b-140">Job release task</span></span>
<span data-ttu-id="d162b-141">Ha egy feladat be van jelölve befejezettként, hello feladat kiadása tevékenység végrehajtása hello készlet, amely legalább egy feladat végrehajtása minden egyes csomóponton.</span><span class="sxs-lookup"><span data-stu-id="d162b-141">Once a job is marked as completed, hello job release task is executed on each node in hello pool that executed at least one task.</span></span> <span data-ttu-id="d162b-142">Megszakítási kérelmet befejezte megjelölése az egy feladatot.</span><span class="sxs-lookup"><span data-stu-id="d162b-142">You mark a job as completed by issuing a terminate request.</span></span> <span data-ttu-id="d162b-143">hello a Batch szolgáltatás, majd a készlet túl hello feladatállapotot*leáll*hello feladattal társított minden aktív vagy futó feladatot megszakítja és hello feladat kiadása tevékenységet futtatja.</span><span class="sxs-lookup"><span data-stu-id="d162b-143">hello Batch service then sets hello job state too*terminating*, terminates any active or running tasks associated with hello job, and runs hello job release task.</span></span> <span data-ttu-id="d162b-144">hello feladat helyezi toohello *befejeződött* állapotát.</span><span class="sxs-lookup"><span data-stu-id="d162b-144">hello job then moves toohello *completed* state.</span></span>

> [!NOTE]
> <span data-ttu-id="d162b-145">Feladat törlése hello feladat kiadása tevékenységet is végez.</span><span class="sxs-lookup"><span data-stu-id="d162b-145">Job deletion also executes hello job release task.</span></span> <span data-ttu-id="d162b-146">Azonban ha egy feladat már meg lett szakítva, hello kiadás feladat nem fut még egyszer hello feladat később törlésekor.</span><span class="sxs-lookup"><span data-stu-id="d162b-146">However, if a job has already been terminated, hello release task is not run a second time if hello job is later deleted.</span></span>
> 
> 

## <a name="job-prep-and-release-tasks-with-batch-net"></a><span data-ttu-id="d162b-147">Előkészítő feladat, és felszabadíthatja a feladatok a Batch .NET kódtárral</span><span class="sxs-lookup"><span data-stu-id="d162b-147">Job prep and release tasks with Batch .NET</span></span>
<span data-ttu-id="d162b-148">a feladat előkészítése tevékenység toouse hozzárendelése egy [JobPreparationTask] [ net_job_prep] objektum tooyour feladat [CloudJob.JobPreparationTask] [ net_job_prep_cloudjob] tulajdonság .</span><span class="sxs-lookup"><span data-stu-id="d162b-148">toouse a job preparation task, assign a [JobPreparationTask][net_job_prep] object tooyour job's [CloudJob.JobPreparationTask][net_job_prep_cloudjob] property.</span></span> <span data-ttu-id="d162b-149">Hasonlóképpen, inicializálni egy [JobReleaseTask] [ net_job_release] , és rendelje hozzá tooyour feladat [CloudJob.JobReleaseTask] [ net_job_prep_cloudjob] tulajdonság tooset hello feladat kiadása tevékenységet.</span><span class="sxs-lookup"><span data-stu-id="d162b-149">Similarly, initialize a [JobReleaseTask][net_job_release] and assign it tooyour job's [CloudJob.JobReleaseTask][net_job_prep_cloudjob] property tooset hello job's release task.</span></span>

<span data-ttu-id="d162b-150">A következő kódrészletet `myBatchClient` példánya [BatchClient][net_batch_client], és `myPool` egy meglévő készlet hello Batch-fiók belül van.</span><span class="sxs-lookup"><span data-stu-id="d162b-150">In this code snippet, `myBatchClient` is an instance of [BatchClient][net_batch_client], and `myPool` is an existing pool within hello Batch account.</span></span>

```csharp
// Create hello CloudJob for CloudPool "myPool"
CloudJob myJob =
    myBatchClient.JobOperations.CreateJob(
        "JobPrepReleaseSampleJob",
        new PoolInformation() { PoolId = "myPool" });

// Specify hello command lines for hello job preparation and release tasks
string jobPrepCmdLine =
    "cmd /c echo %AZ_BATCH_NODE_ID% > %AZ_BATCH_NODE_SHARED_DIR%\\shared_file.txt";
string jobReleaseCmdLine =
    "cmd /c del %AZ_BATCH_NODE_SHARED_DIR%\\shared_file.txt";

// Assign hello job preparation task toohello job
myJob.JobPreparationTask =
    new JobPreparationTask { CommandLine = jobPrepCmdLine };

// Assign hello job release task toohello job
myJob.JobReleaseTask =
    new JobPreparationTask { CommandLine = jobReleaseCmdLine };

await myJob.CommitAsync();
```

<span data-ttu-id="d162b-151">A korábban említett hello kiadási tevékenység végrehajtása, ha egy feladat megszakadt, vagy törölték.</span><span class="sxs-lookup"><span data-stu-id="d162b-151">As mentioned earlier, hello release task is executed when a job is terminated or deleted.</span></span> <span data-ttu-id="d162b-152">Egy feladat leáll [JobOperations.TerminateJobAsync][net_job_terminate].</span><span class="sxs-lookup"><span data-stu-id="d162b-152">Terminate a job with [JobOperations.TerminateJobAsync][net_job_terminate].</span></span> <span data-ttu-id="d162b-153">A feladat törlése [JobOperations.DeleteJobAsync][net_job_delete].</span><span class="sxs-lookup"><span data-stu-id="d162b-153">Delete a job with [JobOperations.DeleteJobAsync][net_job_delete].</span></span> <span data-ttu-id="d162b-154">Általában leáll, vagy törli a feladatot, feladatainak befejezésekor, vagy amikor eléri a beállított időkorlát.</span><span class="sxs-lookup"><span data-stu-id="d162b-154">You typically terminate or delete a job when its tasks are completed, or when a timeout that you've defined has been reached.</span></span>

```csharp
// Terminate hello job toomark it as Completed; this will initiate the
// Job Release Task on any node that executed job tasks. Note that the
// Job Release Task is also executed when a job is deleted, thus you
// need not call Terminate if you typically delete jobs after task completion.
await myBatchClient.JobOperations.TerminateJobAsy("JobPrepReleaseSampleJob");
```

## <a name="code-sample-on-github"></a><span data-ttu-id="d162b-155">A Githubon kódminta</span><span class="sxs-lookup"><span data-stu-id="d162b-155">Code sample on GitHub</span></span>
<span data-ttu-id="d162b-156">toosee feladat előkészítése és a kiadási feladatokat művelet, tekintse meg a hello [JobPrepRelease] [ job_prep_release_sample] mintaprojektet a Githubon.</span><span class="sxs-lookup"><span data-stu-id="d162b-156">toosee job preparation and release tasks in action, check out hello [JobPrepRelease][job_prep_release_sample] sample project on GitHub.</span></span> <span data-ttu-id="d162b-157">Ezt a konzolalkalmazást hello a következő:</span><span class="sxs-lookup"><span data-stu-id="d162b-157">This console application does hello following:</span></span>

1. <span data-ttu-id="d162b-158">Egy készletet hoz létre két "kicsi" csomópont.</span><span class="sxs-lookup"><span data-stu-id="d162b-158">Creates a pool with two "small" nodes.</span></span>
2. <span data-ttu-id="d162b-159">Létrehoz egy feladatot a feladat előkészítése, a kiadás és a szabványos feladatok.</span><span class="sxs-lookup"><span data-stu-id="d162b-159">Creates a job with job preparation, release, and standard tasks.</span></span>
3. <span data-ttu-id="d162b-160">Futtatása hello előkészítő feladat, amely először írja a csomópont "megosztott" könyvtárban hello csomópont azonosítója tooa szövegfájl.</span><span class="sxs-lookup"><span data-stu-id="d162b-160">Runs hello job preparation task, which first writes hello node ID tooa text file in a node's "shared" directory.</span></span>
4. <span data-ttu-id="d162b-161">Minden egyes csomóponton a feladat Azonosítóját toohello írja futtat egy feladatot azonos szövegfájl.</span><span class="sxs-lookup"><span data-stu-id="d162b-161">Runs a task on each node that writes its task ID toohello same text file.</span></span>
5. <span data-ttu-id="d162b-162">Miután minden feladat befejeződött (vagy hello időtúllépését), jelenít meg minden egyes csomópont szöveges fájl toohello konzol hello tartalmát.</span><span class="sxs-lookup"><span data-stu-id="d162b-162">Once all tasks are completed (or hello timeout is reached), prints hello contents of each node's text file toohello console.</span></span>
6. <span data-ttu-id="d162b-163">Hello feladat befejezése után futtatja hello feladat kiadása tevékenység toodelete hello fájl hello csomópont.</span><span class="sxs-lookup"><span data-stu-id="d162b-163">When hello job is completed, runs hello job release task toodelete hello file from hello node.</span></span>
7. <span data-ttu-id="d162b-164">Megrendelése hello kilépési kódokat a hello feladat előkészítése, és felszabadíthatja a feladatokat az egyes csomópontok, amelyeken végre.</span><span class="sxs-lookup"><span data-stu-id="d162b-164">Prints hello exit codes of hello job preparation and release tasks for each node on which they executed.</span></span>
8. <span data-ttu-id="d162b-165">Szünetel végrehajtási tooallow megerősítése feladat és/vagy a készlet törlése.</span><span class="sxs-lookup"><span data-stu-id="d162b-165">Pauses execution tooallow confirmation of job and/or pool deletion.</span></span>

<span data-ttu-id="d162b-166">A mintaalkalmazás hello hasonló toohello következő kimenete:</span><span class="sxs-lookup"><span data-stu-id="d162b-166">Output from hello sample application is similar toohello following:</span></span>

```
Attempting toocreate pool: JobPrepReleaseSamplePool
Created pool JobPrepReleaseSamplePool with 2 small nodes
Checking for existing job JobPrepReleaseSampleJob...
Job JobPrepReleaseSampleJob not found, creating...
Submitting tasks and awaiting completion...
All tasks completed.

Contents of shared\job_prep_and_release.txt on tvm-2434664350_1-20160623t173951z:
-------------------------------------------
tvm-2434664350_1-20160623t173951z tasks:
  task001
  task004
  task005
  task006

Contents of shared\job_prep_and_release.txt on tvm-2434664350_2-20160623t173951z:
-------------------------------------------
tvm-2434664350_2-20160623t173951z tasks:
  task008
  task002
  task003
  task007

Waiting for job JobPrepReleaseSampleJob tooreach state Completed
...

tvm-2434664350_1-20160623t173951z:
  Prep task exit code:    0
  Release task exit code: 0

tvm-2434664350_2-20160623t173951z:
  Prep task exit code:    0
  Release task exit code: 0

Delete job? [yes] no
yes
Delete pool? [yes] no
yes

Sample complete, hit ENTER tooexit...
```

> [!NOTE]
> <span data-ttu-id="d162b-167">Lejáró toohello változó létrehozása és a kezdési idő a csomópontok egy új készletet (az egyes csomópontok állnak készen a többi előtt feladatok) a különböző kimeneti jelenhet meg.</span><span class="sxs-lookup"><span data-stu-id="d162b-167">Due toohello variable creation and start time of nodes in a new pool (some nodes are ready for tasks before others), you may see different output.</span></span> <span data-ttu-id="d162b-168">Pontosabban hello feladatok gyorsan végezze el, mert egy hello készlet csomópontok előfordulhat, hogy execute hello feladat feladatokat.</span><span class="sxs-lookup"><span data-stu-id="d162b-168">Specifically, because hello tasks complete quickly, one of hello pool's nodes may execute all of hello job's tasks.</span></span> <span data-ttu-id="d162b-169">Ha ez történik, megfigyelheti, hogy hello feladat-előkészítő feladatát, és felszabadíthatja a feladatok által végrehajtott feladatok nem hello csomópontja számára nem léteznek.</span><span class="sxs-lookup"><span data-stu-id="d162b-169">If this occurs, you will notice that hello job prep and release tasks do not exist for hello node that executed no tasks.</span></span>
> 
> 

### <a name="inspect-job-preparation-and-release-tasks-in-hello-azure-portal"></a><span data-ttu-id="d162b-170">Vizsgálja meg a feladat előkészítése és a kiadási feladatokat hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="d162b-170">Inspect job preparation and release tasks in hello Azure portal</span></span>
<span data-ttu-id="d162b-171">Hello mintaalkalmazás futtatásakor hello használhatja [Azure-portálon] [ portal] tooview hello feladat és a feladatok tulajdonságainak hello, vagy akár hello megosztott szöveges fájl hello feladat tevékenységeit által módosított.</span><span class="sxs-lookup"><span data-stu-id="d162b-171">When you run hello sample application, you can use hello [Azure portal][portal] tooview hello properties of hello job and its tasks, or even download hello shared text file that is modified by hello job's tasks.</span></span>

<span data-ttu-id="d162b-172">az alábbi hello képernyőfelvételen látható hello **előkészítő feladatok panel** a hello Azure-portálon után hello mintaalkalmazás futását.</span><span class="sxs-lookup"><span data-stu-id="d162b-172">hello screenshot below shows hello **Preparation tasks blade** in hello Azure portal after a run of hello sample application.</span></span> <span data-ttu-id="d162b-173">Keresse meg a toohello *JobPrepReleaseSampleJob* tulajdonságok a feladatok befejezése után (de a feladat és a készlet törlése előtt), és kattintson a **előkészítő feladatok** vagy **kiadási tevékenységek** tooview tulajdonságaikat.</span><span class="sxs-lookup"><span data-stu-id="d162b-173">Navigate toohello *JobPrepReleaseSampleJob* properties after your tasks have completed (but before deleting your job and pool) and click **Preparation tasks** or **Release tasks** tooview their properties.</span></span>

![Azure-portálon feladattulajdonság előkészítése][1]

## <a name="next-steps"></a><span data-ttu-id="d162b-175">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d162b-175">Next steps</span></span>
### <a name="application-packages"></a><span data-ttu-id="d162b-176">Alkalmazáscsomagok</span><span class="sxs-lookup"><span data-stu-id="d162b-176">Application packages</span></span>
<span data-ttu-id="d162b-177">Továbbá toohello feladat előkészítése tevékenység esetében is használhatja hello [alkalmazáscsomagok](batch-application-packages.md) kötegelt tooprepare szolgáltatása számítási csomópontjain a végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="d162b-177">In addition toohello job preparation task, you can also use hello [application packages](batch-application-packages.md) feature of Batch tooprepare compute nodes for task execution.</span></span> <span data-ttu-id="d162b-178">Ez a szolgáltatás különösen fontos telepítőhöz, alkalmazásokat, amelyek számos fájljait (100 +) vagy szigorú verziókezelést igénylő alkalmazások nem igénylő alkalmazások központi telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="d162b-178">This feature is especially useful for deploying applications that do not require running an installer, applications that contain many (100+) files, or applications that require strict version control.</span></span>

### <a name="installing-applications-and-staging-data"></a><span data-ttu-id="d162b-179">Alkalmazások telepítése és átmeneti adatokat</span><span class="sxs-lookup"><span data-stu-id="d162b-179">Installing applications and staging data</span></span>
<span data-ttu-id="d162b-180">Az MSDN fórum bejegyzése a csomópont előkészítése az éppen futó feladatok többféle módszer áttekintése:</span><span class="sxs-lookup"><span data-stu-id="d162b-180">This MSDN forum post provides an overview of several methods of preparing your nodes for running tasks:</span></span>

<span data-ttu-id="d162b-181">[Alkalmazások telepítése és átmeneti adatokat a Batch számítási csomópontjain][forum_post]</span><span class="sxs-lookup"><span data-stu-id="d162b-181">[Installing applications and staging data on Batch compute nodes][forum_post]</span></span>

<span data-ttu-id="d162b-182">Írták hello Azure Batch csoporttagok közül, hogy miként használható toodeploy alkalmazások és adatok toocompute csomópontok számos módszert.</span><span class="sxs-lookup"><span data-stu-id="d162b-182">Written by one of hello Azure Batch team members, it discusses several techniques that you can use toodeploy applications and data toocompute nodes.</span></span>

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_listjobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[azure_storage]: https://azure.microsoft.com/services/storage/
[portal]: https://portal.azure.com
[job_prep_release_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/JobPrepRelease
[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[net_batch_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]:https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_job_prep]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.aspx
[net_job_prep_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobpreparationtask.aspx
[net_job_prep_resourcefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.resourcefiles.aspx
[net_job_delete]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.deletejobasync.aspx
[net_job_terminate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.terminatejobasync.aspx
[net_job_release]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobreleasetask.aspx
[net_job_release_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobreleasetask.aspx
[pool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx

[net_list_certs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificateoperations.listcertificates.aspx
[net_list_compute_nodes]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listcomputenodes.aspx
[net_list_job_schedules]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobschedules.aspx
[net_list_jobprep_status]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobpreparationandreleasetaskstatus.aspx
[net_list_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[net_list_nodefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listnodefiles.aspx
[net_list_pools]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listpools.aspx
[net_list_schedule_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobs.aspx
[net_list_task_files]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listnodefiles.aspx
[net_list_tasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listtasks.aspx

[1]: ./media/batch-job-prep-release/portal-jobprep-01.png
