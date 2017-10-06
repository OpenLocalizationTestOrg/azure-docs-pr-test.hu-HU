---
title: "aaaUse többpéldányos toorun MPI alkalmazások – Azure kötegelt feladatok |} Microsoft Docs"
description: "Ismerje meg, hogyan tooexecute Message Passing Interface (MPI) alkalmazások hello többpéldányos feladat írja be az Azure Batch."
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
ms.assetid: 83e34bd7-a027-4b1b-8314-759384719327
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: 5/22/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b0e3295a6aeb76267c26d5504bcff59de3dc5e22
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-multi-instance-tasks-toorun-message-passing-interface-mpi-applications-in-batch"></a><span data-ttu-id="a47b2-103">A kötegelt többpéldányos feladatok toorun Message Passing Interface (MPI) alkalmazások használata</span><span class="sxs-lookup"><span data-stu-id="a47b2-103">Use multi-instance tasks toorun Message Passing Interface (MPI) applications in Batch</span></span>

<span data-ttu-id="a47b2-104">Többpéldányos feladatok lehetővé teszik az Azure Batch feladat toorun több számítási csomóponton egyidejűleg.</span><span class="sxs-lookup"><span data-stu-id="a47b2-104">Multi-instance tasks allow you toorun an Azure Batch task on multiple compute nodes simultaneously.</span></span> <span data-ttu-id="a47b2-105">Ezek a feladatok lehetővé teszik a nagy teljesítményű számítástechnikai forgatókönyvekhez hasonlóan a Message Passing Interface (MPI) alkalmazások kötegben.</span><span class="sxs-lookup"><span data-stu-id="a47b2-105">These tasks enable high performance computing scenarios like Message Passing Interface (MPI) applications in Batch.</span></span> <span data-ttu-id="a47b2-106">Ebből a cikkből megismerheti, hogyan tooexecute többpéldányos feladatok hello [Batch .NET] [ api_net] könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="a47b2-106">In this article, you learn how tooexecute multi-instance tasks using hello [Batch .NET][api_net] library.</span></span>

> [!NOTE]
> <span data-ttu-id="a47b2-107">A cikkben szereplő példák hello összpontosítani Batch .NET, MS-MPI, és a Windows számítási csomópontok, hello többpéldányos feladat itt tárgyalt a következők alkalmazható tooother platformok és technológiák (a Python és a Linux-csomópont, például Intel MPI).</span><span class="sxs-lookup"><span data-stu-id="a47b2-107">While hello examples in this article focus on Batch .NET, MS-MPI, and Windows compute nodes, hello multi-instance task concepts discussed here are applicable tooother platforms and technologies (Python and Intel MPI on Linux nodes, for example).</span></span>
>
>

## <a name="multi-instance-task-overview"></a><span data-ttu-id="a47b2-108">Többpéldányos feladat áttekintése</span><span class="sxs-lookup"><span data-stu-id="a47b2-108">Multi-instance task overview</span></span>
<span data-ttu-id="a47b2-109">A kötegelt, minden feladata a normál esetben – egyetlen számítási csomóponton végre több feladatok tooa feladat elküldését, és az hello Batch szolgáltatás ütemezések minden egyes csomóponton végrehajtandó feladatot.</span><span class="sxs-lookup"><span data-stu-id="a47b2-109">In Batch, each task is normally executed on a single compute node--you submit multiple tasks tooa job, and hello Batch service schedules each task for execution on a node.</span></span> <span data-ttu-id="a47b2-110">A tevékenység konfigurálásával azonban **többpéldányos beállítások**, pedig utasítani fogja a kötegelt tooinstead hozzon létre egy elsődleges feladat és több résztevékenység, amely több csomóponton végrehajthatók.</span><span class="sxs-lookup"><span data-stu-id="a47b2-110">However, by configuring a task's **multi-instance settings**, you tell Batch tooinstead create one primary task and several subtasks that are then executed on multiple nodes.</span></span>

<span data-ttu-id="a47b2-111">![Többpéldányos feladat áttekintése][1]</span><span class="sxs-lookup"><span data-stu-id="a47b2-111">![Multi-instance task overview][1]</span></span>

<span data-ttu-id="a47b2-112">Kötegazonosítójú feladat többpéldányos beállítások tooa feladat elküldésekor a kötegelt több lépéseket egyedi toomulti-példány feladatokat hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="a47b2-112">When you submit a task with multi-instance settings tooa job, Batch performs several steps unique toomulti-instance tasks:</span></span>

1. <span data-ttu-id="a47b2-113">hello Batch szolgáltatás létrehoz egy **elsődleges** és több **résztevékenység** hello többpéldányos beállításai alapján.</span><span class="sxs-lookup"><span data-stu-id="a47b2-113">hello Batch service creates one **primary** and several **subtasks** based on hello multi-instance settings.</span></span> <span data-ttu-id="a47b2-114">hello számának egyeznie kell (elsődleges és minden résztevékenység) feladatok teljes száma hello **példányok** (számítási csomópontok) hello többpéldányos beállításokat ad meg.</span><span class="sxs-lookup"><span data-stu-id="a47b2-114">hello total number of tasks (primary plus all subtasks) matches hello number of **instances** (compute nodes) you specify in hello multi-instance settings.</span></span>
2. <span data-ttu-id="a47b2-115">Kötegelt jelöli ki valamelyik hello számítási csomópontok hello **fő**, és az ütemezések hello elsődleges feladat tooexecute hello főkiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="a47b2-115">Batch designates one of hello compute nodes as hello **master**, and schedules hello primary task tooexecute on hello master.</span></span> <span data-ttu-id="a47b2-116">Hello résztevékenység tooexecute hello fennmaradó hello számítási csomópontok lefoglalt toohello többpéldányos feladat, csomópontonként egy részfeladatnál annak regisztrálása az ütemezés.</span><span class="sxs-lookup"><span data-stu-id="a47b2-116">It schedules hello subtasks tooexecute on hello remainder of hello compute nodes allocated toohello multi-instance task, one subtask per node.</span></span>
3. <span data-ttu-id="a47b2-117">elsődleges hello és minden résztevékenység letölteni egy **közös erőforrásfájlok** hello többpéldányos beállításokat ad meg.</span><span class="sxs-lookup"><span data-stu-id="a47b2-117">hello primary and all subtasks download any **common resource files** you specify in hello multi-instance settings.</span></span>
4. <span data-ttu-id="a47b2-118">Hello közös erőforrás fájlok letöltését követően elsődleges hello és részfeladatok végrehajtása hello **koordinációs parancs** hello többpéldányos beállításokat ad meg.</span><span class="sxs-lookup"><span data-stu-id="a47b2-118">After hello common resource files have been downloaded, hello primary and subtasks execute hello **coordination command** you specify in hello multi-instance settings.</span></span> <span data-ttu-id="a47b2-119">hello koordinációs parancs hello feladat végrehajtása általánosan használt tooprepare-csomópont.</span><span class="sxs-lookup"><span data-stu-id="a47b2-119">hello coordination command is typically used tooprepare nodes for executing hello task.</span></span> <span data-ttu-id="a47b2-120">Ilyen lehet például a háttér-szolgáltatás indítása (például [Microsoft MPI][msmpi_msdn]tartozó `smpd.exe`) és annak ellenőrzése, hogy hello csomópontok készen tooprocess csomópontok közötti üzenetek.</span><span class="sxs-lookup"><span data-stu-id="a47b2-120">This can include starting background services (such as [Microsoft MPI][msmpi_msdn]'s `smpd.exe`) and verifying that hello nodes are ready tooprocess inter-node messages.</span></span>
5. <span data-ttu-id="a47b2-121">hello elsődleges feladat végrehajtása hello **alkalmazás parancs** hello fő csomóponton *után* hello koordinációs parancs sikeresen befejeződött elsődleges hello és minden résztevékenység.</span><span class="sxs-lookup"><span data-stu-id="a47b2-121">hello primary task executes hello **application command** on hello master node *after* hello coordination command has been completed successfully by hello primary and all subtasks.</span></span> <span data-ttu-id="a47b2-122">hello alkalmazás parancs hello parancssori hello többpéldányos feladat magát, és csak hello elsődleges feladatot futtatja.</span><span class="sxs-lookup"><span data-stu-id="a47b2-122">hello application command is hello command line of hello multi-instance task itself, and is executed only by hello primary task.</span></span> <span data-ttu-id="a47b2-123">Az egy [MS-MPI][msmpi_msdn]-alapú megoldás, ahol végrehajtani a a MPI-kompatibilis alkalmazások használata azt `mpiexec.exe`.</span><span class="sxs-lookup"><span data-stu-id="a47b2-123">In an [MS-MPI][msmpi_msdn]-based solution, this is where you execute your MPI-enabled application using `mpiexec.exe`.</span></span>

> [!NOTE]
> <span data-ttu-id="a47b2-124">Habár funkcionálisan különálló, hello "többpéldányos feladat" egyedi feladat típus nincs például hello [StartTask] [ net_starttask] vagy [JobPreparationTask] [ net_jobprep].</span><span class="sxs-lookup"><span data-stu-id="a47b2-124">Though it is functionally distinct, hello "multi-instance task" is not a unique task type like hello [StartTask][net_starttask] or [JobPreparationTask][net_jobprep].</span></span> <span data-ttu-id="a47b2-125">hello többpéldányos feladata egyszerűen egy szabványos kötegelt feladat ([CloudTask] [ net_task] a Batch .NET) amelynek többpéldányos beállítások lettek konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="a47b2-125">hello multi-instance task is simply a standard Batch task ([CloudTask][net_task] in Batch .NET) whose multi-instance settings have been configured.</span></span> <span data-ttu-id="a47b2-126">Ebben a cikkben, hello toothis irányítjuk **többpéldányos feladat**.</span><span class="sxs-lookup"><span data-stu-id="a47b2-126">In this article, we refer toothis as hello **multi-instance task**.</span></span>
>
>

## <a name="requirements-for-multi-instance-tasks"></a><span data-ttu-id="a47b2-127">Többpéldányos feladatok követelményei</span><span class="sxs-lookup"><span data-stu-id="a47b2-127">Requirements for multi-instance tasks</span></span>
<span data-ttu-id="a47b2-128">Többpéldányos feladatok elvégzéséhez a készlet **engedélyezett csomópontok közötti kommunikáció**, és a **egyidejű feladat a végrehajtás letiltja**.</span><span class="sxs-lookup"><span data-stu-id="a47b2-128">Multi-instance tasks require a pool with **inter-node communication enabled**, and with **concurrent task execution disabled**.</span></span> <span data-ttu-id="a47b2-129">toodisable egyidejű feladat a végrehajtás, set hello [CloudPool.MaxTasksPerComputeNode](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool#Microsoft_Azure_Batch_CloudPool_MaxTasksPerComputeNode) tulajdonság too1.</span><span class="sxs-lookup"><span data-stu-id="a47b2-129">toodisable concurrent task execution, set hello [CloudPool.MaxTasksPerComputeNode](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool#Microsoft_Azure_Batch_CloudPool_MaxTasksPerComputeNode) property too1.</span></span>

<span data-ttu-id="a47b2-130">A kódrészletet bemutatja, hogyan toocreate egy tárolókészletben többpéldányos feladatok hello Batch .NET kódtár használatával.</span><span class="sxs-lookup"><span data-stu-id="a47b2-130">This code snippet shows how toocreate a pool for multi-instance tasks using hello Batch .NET library.</span></span>

```csharp
CloudPool myCloudPool =
    myBatchClient.PoolOperations.CreatePool(
        poolId: "MultiInstanceSamplePool",
        targetDedicatedComputeNodes: 3
        virtualMachineSize: "small",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));

// Multi-instance tasks require inter-node communication, and those nodes
// must run only one task at a time.
myCloudPool.InterComputeNodeCommunicationEnabled = true;
myCloudPool.MaxTasksPerComputeNode = 1;
```

> [!NOTE]
> <span data-ttu-id="a47b2-131">Ha a többpéldányos feladat a fürtök csomóponton belüli kommunikációjához közötti kommunikáció a készletben engedélyezett toorun, vagy az egy *maxTasksPerNode* 1-nél nagyobb értéket, hello feladat soha nem ütemezett--határozatlan ideig hello "aktív" állapotban marad.</span><span class="sxs-lookup"><span data-stu-id="a47b2-131">If you try toorun a multi-instance task in a pool with internode communication disabled, or with a *maxTasksPerNode* value greater than 1, hello task is never scheduled--it remains indefinitely in hello "active" state.</span></span> 
>
> <span data-ttu-id="a47b2-132">Többpéldányos feladatokat hajthat végre 2015. December 14. után létrehozott készletek csomópontján.</span><span class="sxs-lookup"><span data-stu-id="a47b2-132">Multi-instance tasks can execute only on nodes in pools created after 14 December 2015.</span></span>
>
>

### <a name="use-a-starttask-tooinstall-mpi"></a><span data-ttu-id="a47b2-133">Egy StartTask tooinstall MPI használata</span><span class="sxs-lookup"><span data-stu-id="a47b2-133">Use a StartTask tooinstall MPI</span></span>
<span data-ttu-id="a47b2-134">toorun MPI alkalmazások többpéldányos feladatokkal, először tooinstall MPI megvalósítása (MS-MPI vagy Intel MPI, például) hello készletben hello számítási csomóponton.</span><span class="sxs-lookup"><span data-stu-id="a47b2-134">toorun MPI applications with a multi-instance task, you first need tooinstall an MPI implementation (MS-MPI or Intel MPI, for example) on hello compute nodes in hello pool.</span></span> <span data-ttu-id="a47b2-135">Ez az egy időben toouse egy [StartTask][net_starttask], amely hajt végre, amikor a csomópont csatlakozik egy készletet, vagy újraindítják.</span><span class="sxs-lookup"><span data-stu-id="a47b2-135">This is a good time toouse a [StartTask][net_starttask], which executes whenever a node joins a pool, or is restarted.</span></span> <span data-ttu-id="a47b2-136">A kódrészletet, amely meghatározza az MS-MPI hello telepítőcsomagját, egy StartTask hoz létre egy [erőforrásfájl][net_resourcefile].</span><span class="sxs-lookup"><span data-stu-id="a47b2-136">This code snippet creates a StartTask that specifies hello MS-MPI setup package as a [resource file][net_resourcefile].</span></span> <span data-ttu-id="a47b2-137">hello kezdő tevékenység Parancssor végrehajtása után hello erőforrásfájl letöltött toohello csomópont.</span><span class="sxs-lookup"><span data-stu-id="a47b2-137">hello start task's command line is executed after hello resource file is downloaded toohello node.</span></span> <span data-ttu-id="a47b2-138">Ebben az esetben a hello parancssori MS-MPI felügyelet nélküli telepítés hajt végre.</span><span class="sxs-lookup"><span data-stu-id="a47b2-138">In this case, hello command line performs an unattended install of MS-MPI.</span></span>

```csharp
// Create a StartTask for hello pool which we use for installing MS-MPI on
// hello nodes as they join hello pool (or when they are restarted).
StartTask startTask = new StartTask
{
    CommandLine = "cmd /c MSMpiSetup.exe -unattend -force",
    ResourceFiles = new List<ResourceFile> { new ResourceFile("https://mystorageaccount.blob.core.windows.net/mycontainer/MSMpiSetup.exe", "MSMpiSetup.exe") },
    UserIdentity = new UserIdentity(new AutoUserSpecification(elevationLevel: ElevationLevel.Admin)),
    WaitForSuccess = true
};
myCloudPool.StartTask = startTask;

// Commit hello fully configured pool toohello Batch service tooactually create
// hello pool and its compute nodes.
await myCloudPool.CommitAsync();
```

### <a name="remote-direct-memory-access-rdma"></a><span data-ttu-id="a47b2-139">Távoli közvetlen memória-hozzáférés (RDMA)</span><span class="sxs-lookup"><span data-stu-id="a47b2-139">Remote direct memory access (RDMA)</span></span>
<span data-ttu-id="a47b2-140">Ha úgy dönt, egy [RDMA-kompatibilis mérete](../virtual-machines/windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) A9 hello a számítási csomópontok a Batch-készlet, mint például a MPI alkalmazás előnyeit az Azure nagy teljesítményű, alacsony késésű távoli közvetlen memória-hozzáférés (RDMA) hálózati.</span><span class="sxs-lookup"><span data-stu-id="a47b2-140">When you choose an [RDMA-capable size](../virtual-machines/windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) such as A9 for hello compute nodes in your Batch pool, your MPI application can take advantage of Azure's high-performance, low-latency remote direct memory access (RDMA) network.</span></span>

<span data-ttu-id="a47b2-141">Keresse meg az "RDMA-kompatibilis" szerepel a következő cikkek hello hello méretek:</span><span class="sxs-lookup"><span data-stu-id="a47b2-141">Look for hello sizes specified as "RDMA capable" in hello following articles:</span></span>

* <span data-ttu-id="a47b2-142">**CloudServiceConfiguration** készletek</span><span class="sxs-lookup"><span data-stu-id="a47b2-142">**CloudServiceConfiguration** pools</span></span>

  * <span data-ttu-id="a47b2-143">[A Felhőszolgáltatások mérete](../cloud-services/cloud-services-sizes-specs.md) (csak Windows)</span><span class="sxs-lookup"><span data-stu-id="a47b2-143">[Sizes for Cloud Services](../cloud-services/cloud-services-sizes-specs.md) (Windows only)</span></span>
* <span data-ttu-id="a47b2-144">**VirtualMachineConfiguration** készletek</span><span class="sxs-lookup"><span data-stu-id="a47b2-144">**VirtualMachineConfiguration** pools</span></span>

  * <span data-ttu-id="a47b2-145">[Az Azure virtuális gépek méretei](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Linux)</span><span class="sxs-lookup"><span data-stu-id="a47b2-145">[Sizes for virtual machines in Azure](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Linux)</span></span>
  * <span data-ttu-id="a47b2-146">[Az Azure virtuális gépek méretei](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Windows)</span><span class="sxs-lookup"><span data-stu-id="a47b2-146">[Sizes for virtual machines in Azure](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Windows)</span></span>

> [!NOTE]
> <span data-ttu-id="a47b2-147">az RDMA tootake előnyeit [Linux számítási csomópontok](batch-linux-nodes.md), kell használnia **Intel MPI** hello csomópontján.</span><span class="sxs-lookup"><span data-stu-id="a47b2-147">tootake advantage of RDMA on [Linux compute nodes](batch-linux-nodes.md), you must use **Intel MPI** on hello nodes.</span></span> <span data-ttu-id="a47b2-148">A CloudServiceConfiguration és VirtualMachineConfiguration készletek további információkért lásd: hello készlet szakasza hello [Batch funkcióinak áttekintése](batch-api-basics.md).</span><span class="sxs-lookup"><span data-stu-id="a47b2-148">For more information on CloudServiceConfiguration and VirtualMachineConfiguration pools, see hello Pool section of hello [Batch feature overview](batch-api-basics.md).</span></span>
>
>

## <a name="create-a-multi-instance-task-with-batch-net"></a><span data-ttu-id="a47b2-149">A Batch .NET többpéldányos feladat létrehozása</span><span class="sxs-lookup"><span data-stu-id="a47b2-149">Create a multi-instance task with Batch .NET</span></span>
<span data-ttu-id="a47b2-150">Most, hogy azt már szabályozott hello készlet követelmények és MPI csomag telepítése, hozzon létre hello többpéldányos feladat.</span><span class="sxs-lookup"><span data-stu-id="a47b2-150">Now that we've covered hello pool requirements and MPI package installation, let's create hello multi-instance task.</span></span> <span data-ttu-id="a47b2-151">Az ezt a kódrészletet standard létrehozhatunk [CloudTask][net_task], majd konfigurálja a [MultiInstanceSettings] [ net_multiinstance_prop] tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="a47b2-151">In this snippet, we create a standard [CloudTask][net_task], then configure its [MultiInstanceSettings][net_multiinstance_prop] property.</span></span> <span data-ttu-id="a47b2-152">A korábban említett hello többpéldányos feladat nincs a különböző feladattípust, de egy szabványos kötegelt feladat többpéldányos beállításokat.</span><span class="sxs-lookup"><span data-stu-id="a47b2-152">As mentioned earlier, hello multi-instance task is not a distinct task type, but a standard Batch task configured with multi-instance settings.</span></span>

```csharp
// Create hello multi-instance task. Its command line is hello "application command"
// and will be executed *only* by hello primary, and only after hello primary and
// subtasks execute hello CoordinationCommandLine.
CloudTask myMultiInstanceTask = new CloudTask(id: "mymultiinstancetask",
    commandline: "cmd /c mpiexec.exe -wdir %AZ_BATCH_TASK_SHARED_DIR% MyMPIApplication.exe");

// Configure hello task's MultiInstanceSettings. hello CoordinationCommandLine will be executed by
// hello primary and all subtasks.
myMultiInstanceTask.MultiInstanceSettings =
    new MultiInstanceSettings(numberOfNodes) {
    CoordinationCommandLine = @"cmd /c start cmd /c ""%MSMPI_BIN%\smpd.exe"" -d",
    CommonResourceFiles = new List<ResourceFile> {
    new ResourceFile("https://mystorageaccount.blob.core.windows.net/mycontainer/MyMPIApplication.exe",
                     "MyMPIApplication.exe")
    }
};

// Submit hello task toohello job. Batch will take care of splitting it into subtasks and
// scheduling them for execution on hello nodes.
await myBatchClient.JobOperations.AddTaskAsync("mybatchjob", myMultiInstanceTask);
```

## <a name="primary-task-and-subtasks"></a><span data-ttu-id="a47b2-153">Elsődleges feladatok és részfeladatok</span><span class="sxs-lookup"><span data-stu-id="a47b2-153">Primary task and subtasks</span></span>
<span data-ttu-id="a47b2-154">Hello létrehozásakor egy feladat többpéldányos beállításai hello számítási csomópontokat, amelyek tooexecute hello feladat számát adja meg.</span><span class="sxs-lookup"><span data-stu-id="a47b2-154">When you create hello multi-instance settings for a task, you specify hello number of compute nodes that are tooexecute hello task.</span></span> <span data-ttu-id="a47b2-155">Elküldött hello feladat tooa feladat, a Batch szolgáltatás hello létrehoz egy **elsődleges** feladatot, és elég **résztevékenység** együtt megfelelő a megadott csomópontok hello száma.</span><span class="sxs-lookup"><span data-stu-id="a47b2-155">When you submit hello task tooa job, hello Batch service creates one **primary** task and enough **subtasks** that together match hello number of nodes you specified.</span></span>

<span data-ttu-id="a47b2-156">Ezek a feladatok rendelt hello tartomány 0 egész azonosító túl*numberOfInstances* – 1.</span><span class="sxs-lookup"><span data-stu-id="a47b2-156">These tasks are assigned an integer id in hello range of 0 too*numberOfInstances* - 1.</span></span> <span data-ttu-id="a47b2-157">hello feladat 0 azonosítójú hello elsődleges feladat, és minden egyéb azonosítók résztevékenység.</span><span class="sxs-lookup"><span data-stu-id="a47b2-157">hello task with id 0 is hello primary task, and all other ids are subtasks.</span></span> <span data-ttu-id="a47b2-158">Például ha egy feladat többpéldányos beállításai a következő hello hoz létre, hello elsődleges feladat kellene 0 azonosítót, és hello résztevékenység rendelkeznie azonosítók 1 – 9.</span><span class="sxs-lookup"><span data-stu-id="a47b2-158">For example, if you create hello following multi-instance settings for a task, hello primary task would have an id of 0, and hello subtasks would have ids 1 through 9.</span></span>

```csharp
int numberOfNodes = 10;
myMultiInstanceTask.MultiInstanceSettings = new MultiInstanceSettings(numberOfNodes);
```

### <a name="master-node"></a><span data-ttu-id="a47b2-159">Fő csomópont</span><span class="sxs-lookup"><span data-stu-id="a47b2-159">Master node</span></span>
<span data-ttu-id="a47b2-160">A többpéldányos feladatok elküldésekor hello Batch szolgáltatás jelöli ki valamelyik hello számítási csomópontok hello "fő" csomópont, és ütemezések hello elsődleges feladat tooexecute hello fő csomóponton.</span><span class="sxs-lookup"><span data-stu-id="a47b2-160">When you submit a multi-instance task, hello Batch service designates one of hello compute nodes as hello "master" node, and schedules hello primary task tooexecute on hello master node.</span></span> <span data-ttu-id="a47b2-161">hello résztevékenység ütemezett tooexecute toohello többpéldányos feladat lefoglalt hello csomópontok hello további része a rendszer.</span><span class="sxs-lookup"><span data-stu-id="a47b2-161">hello subtasks are scheduled tooexecute on hello remainder of hello nodes allocated toohello multi-instance task.</span></span>

## <a name="coordination-command"></a><span data-ttu-id="a47b2-162">Koordinációs parancs</span><span class="sxs-lookup"><span data-stu-id="a47b2-162">Coordination command</span></span>
<span data-ttu-id="a47b2-163">Hello **koordinációs parancs** hello elsődleges, mind a résztevékenység futtatja.</span><span class="sxs-lookup"><span data-stu-id="a47b2-163">hello **coordination command** is executed by both hello primary and subtasks.</span></span>

<span data-ttu-id="a47b2-164">hello koordinációs parancs meghívása hello blokkolja--kötegelt nem hajtható végre: hello alkalmazás parancs amíg hello koordinációs parancs sikeresen visszatért az összes résztevékenység.</span><span class="sxs-lookup"><span data-stu-id="a47b2-164">hello invocation of hello coordination command is blocking--Batch does not execute hello application command until hello coordination command has returned successfully for all subtasks.</span></span> <span data-ttu-id="a47b2-165">hello koordinációs parancsot kell ezért szükséges háttér szolgáltatások indítása, győződjön meg arról, hogy azok készen áll a használatra, és zárja be.</span><span class="sxs-lookup"><span data-stu-id="a47b2-165">hello coordination command should therefore start any required background services, verify that they are ready for use, and then exit.</span></span> <span data-ttu-id="a47b2-166">A koordinációs parancs használatával a MS-MPI 7-es verzió megoldás például hello csomóponton hello SMPD szolgáltatás elindul, majd kilép:</span><span class="sxs-lookup"><span data-stu-id="a47b2-166">For example, this coordination command for a solution using MS-MPI version 7 starts hello SMPD service on hello node, then exits:</span></span>

```
cmd /c start cmd /c ""%MSMPI_BIN%\smpd.exe"" -d
```

<span data-ttu-id="a47b2-167">Vegye figyelembe a hello használata `start` koordinációs parancsban.</span><span class="sxs-lookup"><span data-stu-id="a47b2-167">Note hello use of `start` in this coordination command.</span></span> <span data-ttu-id="a47b2-168">Ez azért szükséges, mert hello `smpd.exe` végrehajtása után azonnal alkalmazás tér vissza.</span><span class="sxs-lookup"><span data-stu-id="a47b2-168">This is required because hello `smpd.exe` application does not return immediately after execution.</span></span> <span data-ttu-id="a47b2-169">Hello hello használata nélkül [start] [ cmd_start] parancs, a tranzakciókoordináció-parancs nem alakítanák vissza, és ezért megakadályozza a hello alkalmazás parancs futtatását.</span><span class="sxs-lookup"><span data-stu-id="a47b2-169">Without hello use of hello [start][cmd_start] command, this coordination command would not return, and would therefore block hello application command from running.</span></span>

## <a name="application-command"></a><span data-ttu-id="a47b2-170">Alkalmazás parancs</span><span class="sxs-lookup"><span data-stu-id="a47b2-170">Application command</span></span>
<span data-ttu-id="a47b2-171">Hello elsődleges feladat és összes résztevékenység végzett hello koordinációs parancs végrehajtása, amennyiben hello többpéldányos feladat parancssori hello elsődleges feladat végrehajtása *csak*.</span><span class="sxs-lookup"><span data-stu-id="a47b2-171">Once hello primary task and all subtasks have finished executing hello coordination command, hello multi-instance task's command line is executed by hello primary task *only*.</span></span> <span data-ttu-id="a47b2-172">Ezt nevezik a hello **alkalmazás parancs** toodistinguish hello koordinációs parancs le.</span><span class="sxs-lookup"><span data-stu-id="a47b2-172">We call this hello **application command** toodistinguish it from hello coordination command.</span></span>

<span data-ttu-id="a47b2-173">MS-MPI-alkalmazások használata hello alkalmazás parancs tooexecute a MPI-kompatibilis alkalmazás a `mpiexec.exe`.</span><span class="sxs-lookup"><span data-stu-id="a47b2-173">For MS-MPI applications, use hello application command tooexecute your MPI-enabled application with `mpiexec.exe`.</span></span> <span data-ttu-id="a47b2-174">Például ez az alkalmazás parancs használatával a MS-MPI 7-es verzió megoldás:</span><span class="sxs-lookup"><span data-stu-id="a47b2-174">For example, here is an application command for a solution using MS-MPI version 7:</span></span>

```
cmd /c ""%MSMPI_BIN%\mpiexec.exe"" -c 1 -wdir %AZ_BATCH_TASK_SHARED_DIR% MyMPIApplication.exe
```

> [!NOTE]
> <span data-ttu-id="a47b2-175">Mivel az MS-MPI `mpiexec.exe` használ hello `CCP_NODES` változó alapértelmezés szerint (lásd: [környezeti változók](#environment-variables)) hello példa a fenti alkalmazás parancssor nem tartalmazza azt.</span><span class="sxs-lookup"><span data-stu-id="a47b2-175">Because MS-MPI's `mpiexec.exe` uses hello `CCP_NODES` variable by default (see [Environment variables](#environment-variables)) hello example application command line above excludes it.</span></span>
>
>

## <a name="environment-variables"></a><span data-ttu-id="a47b2-176">Környezeti változók</span><span class="sxs-lookup"><span data-stu-id="a47b2-176">Environment variables</span></span>
<span data-ttu-id="a47b2-177">Kötegelt hoz létre több [környezeti változók] [ msdn_env_var] hello toomulti-példány feladatai számítási csomópontok lefoglalt tooa többpéldányos feladat.</span><span class="sxs-lookup"><span data-stu-id="a47b2-177">Batch creates several [environment variables][msdn_env_var] specific toomulti-instance tasks on hello compute nodes allocated tooa multi-instance task.</span></span> <span data-ttu-id="a47b2-178">A koordinációs és alkalmazás parancssorokat hivatkozhat ezen környezeti változókkal, parancsfájlok és az általuk programok is hello.</span><span class="sxs-lookup"><span data-stu-id="a47b2-178">Your coordination and application command lines can reference these environment variables, as can hello scripts and programs they execute.</span></span>

<span data-ttu-id="a47b2-179">hello következő környezeti változók vannak által hello Batch-szolgáltatás a feladatok által létrehozott többpéldányos:</span><span class="sxs-lookup"><span data-stu-id="a47b2-179">hello following environment variables are created by hello Batch service for use by multi-instance tasks:</span></span>

* `CCP_NODES`
* `AZ_BATCH_NODE_LIST`
* `AZ_BATCH_HOST_LIST`
* `AZ_BATCH_MASTER_NODE`
* `AZ_BATCH_TASK_SHARED_DIR`
* `AZ_BATCH_IS_CURRENT_NODE_MASTER`

<span data-ttu-id="a47b2-180">Ezek a részletes és hello más Batch számítási csomópont környezeti változókat, beleértve azok tartalmát és a Láthatóság: [számítási csomópont környezeti változók][msdn_env_var].</span><span class="sxs-lookup"><span data-stu-id="a47b2-180">For full details on these and hello other Batch compute node environment variables, including their contents and visibility, see [Compute node environment variables][msdn_env_var].</span></span>

> [!TIP]
> <span data-ttu-id="a47b2-181">hello kötegelt Linux MPI kódminta egy példával hogyan ezen környezeti változókkal számos használható-e.</span><span class="sxs-lookup"><span data-stu-id="a47b2-181">hello Batch Linux MPI code sample contains an example of how several of these environment variables can be used.</span></span> <span data-ttu-id="a47b2-182">Hello [tranzakciókoordináció-cmd] [ coord_cmd_example] Bash parancsfájl közös alkalmazás és a bemeneti fájlokat tölti le a Azure Storage-ból, lehetővé teszi, hogy a hálózati fájlrendszer (NFS) megosztott hello főcsomópont helyén, és konfigurálja a többi csomópont hello NFS-ügyfelek rendelkezésre bocsátott toohello többpéldányos feladat.</span><span class="sxs-lookup"><span data-stu-id="a47b2-182">hello [coordination-cmd][coord_cmd_example] Bash script downloads common application and input files from Azure Storage, enables a Network File System (NFS) share on hello master node, and configures hello other nodes allocated toohello multi-instance task as NFS clients.</span></span>
>
>

## <a name="resource-files"></a><span data-ttu-id="a47b2-183">Erőforrás-fájlok</span><span class="sxs-lookup"><span data-stu-id="a47b2-183">Resource files</span></span>
<span data-ttu-id="a47b2-184">Az erőforrás fájlok tooconsider többpéldányos feladatok két csoportját: **közös erőforrásfájlok** , amely *összes* feladatok letöltése (mind az elsődleges és részfeladatok), és hello **erőforrásfájlok** hello többpéldányos feladat, amely megadott *csak elsődleges hello* letöltések feladat.</span><span class="sxs-lookup"><span data-stu-id="a47b2-184">There are two sets of resource files tooconsider for multi-instance tasks: **common resource files** that *all* tasks download (both primary and subtasks), and hello **resource files** specified for hello multi-instance task itself, which *only hello primary* task downloads.</span></span>

<span data-ttu-id="a47b2-185">Megadhat egy vagy több **közös erőforrásfájlok** hello többpéldányos beállításaiban feladathoz.</span><span class="sxs-lookup"><span data-stu-id="a47b2-185">You can specify one or more **common resource files** in hello multi-instance settings for a task.</span></span> <span data-ttu-id="a47b2-186">A közös erőforrás fájlokat szeretne letölteni a [Azure Storage](../storage/common/storage-introduction.md) az egyes csomópontok **feladat megosztott könyvtár** elsődleges hello és minden résztevékenység.</span><span class="sxs-lookup"><span data-stu-id="a47b2-186">These common resource files are downloaded from [Azure Storage](../storage/common/storage-introduction.md) into each node's **task shared directory** by hello primary and all subtasks.</span></span> <span data-ttu-id="a47b2-187">Elérhető hello feladat megosztott könyvtár alkalmazás és koordinációs parancssorokat hello segítségével `AZ_BATCH_TASK_SHARED_DIR` környezeti változó.</span><span class="sxs-lookup"><span data-stu-id="a47b2-187">You can access hello task shared directory from application and coordination command lines by using hello `AZ_BATCH_TASK_SHARED_DIR` environment variable.</span></span> <span data-ttu-id="a47b2-188">Hello `AZ_BATCH_TASK_SHARED_DIR` elérési út minden csomópont lefoglalt toohello többpéldányos tevékenységen azonos, így megoszthatja a közötti elsődleges hello és minden résztevékenység egyetlen koordinációs parancsot.</span><span class="sxs-lookup"><span data-stu-id="a47b2-188">hello `AZ_BATCH_TASK_SHARED_DIR` path is identical on every node allocated toohello multi-instance task, thus you can share a single coordination command between hello primary and all subtasks.</span></span> <span data-ttu-id="a47b2-189">Kötegelt nem "könyvtárban" hello távelérési értelemben, de csatlakoztatási használni, vagy pont, ahogy azt korábban említettük, a környezeti változók hello tipp a korábbi megosztásához.</span><span class="sxs-lookup"><span data-stu-id="a47b2-189">Batch does not "share" hello directory in a remote access sense, but you can use it as a mount or share point as mentioned earlier in hello tip on environment variables.</span></span>

<span data-ttu-id="a47b2-190">Hello többpéldányos magát a rendszer letöltött toohello feladatütemezési munkakönyvtár, az erőforrásfájlokat `AZ_BATCH_TASK_WORKING_DIR`, alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="a47b2-190">Resource files that you specify for hello multi-instance task itself are downloaded toohello task's working directory, `AZ_BATCH_TASK_WORKING_DIR`, by default.</span></span> <span data-ttu-id="a47b2-191">Ahogy azt korábban említettük, ezzel szemben toocommon erőforrásfájlokat, csak hello elsődleges feladat fájlokat tölti le erőforrás megadott hello többpéldányos tevékenység saját magát.</span><span class="sxs-lookup"><span data-stu-id="a47b2-191">As mentioned, in contrast toocommon resource files, only hello primary task downloads resource files specified for hello  multi-instance task itself.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a47b2-192">Mindig használjon hello környezeti változók `AZ_BATCH_TASK_SHARED_DIR` és `AZ_BATCH_TASK_WORKING_DIR` a parancssorok toorefer toothese könyvtárakat.</span><span class="sxs-lookup"><span data-stu-id="a47b2-192">Always use hello environment variables `AZ_BATCH_TASK_SHARED_DIR` and `AZ_BATCH_TASK_WORKING_DIR` toorefer toothese directories in your command lines.</span></span> <span data-ttu-id="a47b2-193">Ne próbálja meg manuálisan tooconstruct hello elérési utakat.</span><span class="sxs-lookup"><span data-stu-id="a47b2-193">Do not attempt tooconstruct hello paths manually.</span></span>
>
>

## <a name="task-lifetime"></a><span data-ttu-id="a47b2-194">Feladatütemezés élettartama</span><span class="sxs-lookup"><span data-stu-id="a47b2-194">Task lifetime</span></span>
<span data-ttu-id="a47b2-195">hello hello elsődleges feladat vezérlők hello élettartama hello teljes többpéldányos feladatütemezés élettartama.</span><span class="sxs-lookup"><span data-stu-id="a47b2-195">hello lifetime of hello primary task controls hello lifetime of hello entire multi-instance task.</span></span> <span data-ttu-id="a47b2-196">Amikor elsődleges hello kilép, az összes hello résztevékenység leállítása van.</span><span class="sxs-lookup"><span data-stu-id="a47b2-196">When hello primary exits, all of hello subtasks are terminated.</span></span> <span data-ttu-id="a47b2-197">hello kilépési kód elsődleges hello hello kilépési kód hello feladat, és ezért használt toodetermine hello sikerességét vagy sikertelenségét hello feladat újrapróbálkozási célokra.</span><span class="sxs-lookup"><span data-stu-id="a47b2-197">hello exit code of hello primary is hello exit code of hello task, and is therefore used toodetermine hello success or failure of hello task for retry purposes.</span></span>

<span data-ttu-id="a47b2-198">Ha bármelyik hello résztevékenység sikertelen, kilép egy nem nulla visszatérési kóddal például hello teljes többpéldányos feladat sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="a47b2-198">If any of hello subtasks fail, exiting with a non-zero return code, for example, hello entire multi-instance task fails.</span></span> <span data-ttu-id="a47b2-199">hello többpéldányos feladat majd lezárva, és újra megpróbálja mentése tooits Újrapróbálkozási korlát.</span><span class="sxs-lookup"><span data-stu-id="a47b2-199">hello multi-instance task is then terminated and retried, up tooits retry limit.</span></span>

<span data-ttu-id="a47b2-200">A többpéldányos tevékenység törlésekor elsődleges hello és minden résztevékenység is törli hello Batch-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="a47b2-200">When you delete a multi-instance task, hello primary and all subtasks are also deleted by hello Batch service.</span></span> <span data-ttu-id="a47b2-201">Az összes végző részfeladat a könyvtárak, és a fájlok törlődnek hello számítási csomópontokat, ugyanúgy, mint a szabványos feladat.</span><span class="sxs-lookup"><span data-stu-id="a47b2-201">All subtask directories and their files are deleted from hello compute nodes, just as for a standard task.</span></span>

<span data-ttu-id="a47b2-202">[TaskConstraints] [ net_taskconstraints] többpéldányos tevékenység, például a hello [MaxTaskRetryCount][net_taskconstraint_maxretry], [MaxWallClockTime] [ net_taskconstraint_maxwallclock], és [RetentionTime] [ net_taskconstraint_retention] tulajdonságainak vannak figyelembe véve, egy szabványos tevékenység, és alkalmazza a toohello elsődleges és az összes résztevékenység.</span><span class="sxs-lookup"><span data-stu-id="a47b2-202">[TaskConstraints][net_taskconstraints] for a multi-instance task, such as hello [MaxTaskRetryCount][net_taskconstraint_maxretry], [MaxWallClockTime][net_taskconstraint_maxwallclock], and [RetentionTime][net_taskconstraint_retention] properties, are honored as they are for a standard task, and apply toohello primary and all subtasks.</span></span> <span data-ttu-id="a47b2-203">Azonban ha megváltoztatja hello [RetentionTime] [ net_taskconstraint_retention] alkalmazott csak toohello elsődleges feladat tulajdonság hozzáadása hello többpéldányos feladat toohello feladat, a módosítás után.</span><span class="sxs-lookup"><span data-stu-id="a47b2-203">However, if you change hello [RetentionTime][net_taskconstraint_retention] property after adding hello multi-instance task toohello job, this change is applied only toohello primary task.</span></span> <span data-ttu-id="a47b2-204">Az összes hello résztevékenység továbbra is toouse hello eredeti [RetentionTime][net_taskconstraint_retention].</span><span class="sxs-lookup"><span data-stu-id="a47b2-204">All of hello subtasks continue toouse hello original [RetentionTime][net_taskconstraint_retention].</span></span>

<span data-ttu-id="a47b2-205">A számítási csomópont legutóbbi tevékenységek listájának hello azonosítója egy részfeladatnál annak regisztrálása tükrözi, ha a legutóbbi feladat hello többpéldányos feladat része volt.</span><span class="sxs-lookup"><span data-stu-id="a47b2-205">A compute node's recent task list reflects hello id of a subtask if hello recent task was part of a multi-instance task.</span></span>

## <a name="obtain-information-about-subtasks"></a><span data-ttu-id="a47b2-206">Résztevékenység kapcsolatos információkhoz</span><span class="sxs-lookup"><span data-stu-id="a47b2-206">Obtain information about subtasks</span></span>
<span data-ttu-id="a47b2-207">hello Batch .NET könyvtár hívás hello használatával résztevékenység tooobtain információk [CloudTask.ListSubtasks] [ net_task_listsubtasks] metódust.</span><span class="sxs-lookup"><span data-stu-id="a47b2-207">tooobtain information on subtasks by using hello Batch .NET library, call hello [CloudTask.ListSubtasks][net_task_listsubtasks] method.</span></span> <span data-ttu-id="a47b2-208">Ez a módszer az összes résztevékenység információkat ad vissza, és hello információ csomópontjain hello feladatok végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="a47b2-208">This method returns information on all subtasks, and information about hello compute node that executed hello tasks.</span></span> <span data-ttu-id="a47b2-209">Ezekből az adatokból meghatározhatja, hogy minden részfeladatnál annak regisztrálása a gyökérkönyvtár, hello alkalmazáskészlet-azonosító, a jelenlegi állapotában, kilépési kód, és több.</span><span class="sxs-lookup"><span data-stu-id="a47b2-209">From this information, you can determine each subtask's root directory, hello pool id, its current state, exit code, and more.</span></span> <span data-ttu-id="a47b2-210">Ez az információ hello együtt használható [PoolOperations.GetNodeFile] [ poolops_getnodefile] metódus tooobtain hello részfeladatnál annak regisztrálása a fájlokat.</span><span class="sxs-lookup"><span data-stu-id="a47b2-210">You can use this information in combination with hello [PoolOperations.GetNodeFile][poolops_getnodefile] method tooobtain hello subtask's files.</span></span> <span data-ttu-id="a47b2-211">Vegye figyelembe, hogy ez a metódus nem ad vissza adatokat hello elsődleges feladat (azonosító: 0).</span><span class="sxs-lookup"><span data-stu-id="a47b2-211">Note that this method does not return information for hello primary task (id 0).</span></span>

> [!NOTE]
> <span data-ttu-id="a47b2-212">Hacsak másként nem jelezzük, a Batch .NET-metódusokat, amely működik többpéldányos hello [CloudTask] [ net_task] maga alkalmazása *csak* toohello elsődleges feladat.</span><span class="sxs-lookup"><span data-stu-id="a47b2-212">Unless otherwise stated, Batch .NET methods that operate on hello multi-instance [CloudTask][net_task] itself apply *only* toohello primary task.</span></span> <span data-ttu-id="a47b2-213">Például, ha meghívja a hello [CloudTask.ListNodeFiles] [ net_task_listnodefiles] metódus a többpéldányos tevékenységen, csak hello elsődleges feladat fájlok vannak adott vissza.</span><span class="sxs-lookup"><span data-stu-id="a47b2-213">For example, when you call hello [CloudTask.ListNodeFiles][net_task_listnodefiles] method on a multi-instance task, only hello primary task's files are returned.</span></span>
>
>

<span data-ttu-id="a47b2-214">hello következő kódrészletet bemutatja, hogyan tooobtain végző részfeladat információkat, valamint fájl tartalmának kérhet hello csomópontokat, amelyeken végre.</span><span class="sxs-lookup"><span data-stu-id="a47b2-214">hello following code snippet shows how tooobtain subtask information, as well as request file contents from hello nodes on which they executed.</span></span>

```csharp
// Obtain hello job and hello multi-instance task from hello Batch service
CloudJob boundJob = batchClient.JobOperations.GetJob("mybatchjob");
CloudTask myMultiInstanceTask = boundJob.GetTask("mymultiinstancetask");

// Now obtain hello list of subtasks for hello task
IPagedEnumerable<SubtaskInformation> subtasks = myMultiInstanceTask.ListSubtasks();

// Asynchronously iterate over hello subtasks and print their stdout and stderr
// output if hello subtask has completed
await subtasks.ForEachAsync(async (subtask) =>
{
    Console.WriteLine("subtask: {0}", subtask.Id);
    Console.WriteLine("exit code: {0}", subtask.ExitCode);

    if (subtask.State == SubtaskState.Completed)
    {
        ComputeNode node =
            await batchClient.PoolOperations.GetComputeNodeAsync(subtask.ComputeNodeInformation.PoolId,
                                                                 subtask.ComputeNodeInformation.ComputeNodeId);

        NodeFile stdOutFile = await node.GetNodeFileAsync(subtask.ComputeNodeInformation.TaskRootDirectory + "\\" + Constants.StandardOutFileName);
        NodeFile stdErrFile = await node.GetNodeFileAsync(subtask.ComputeNodeInformation.TaskRootDirectory + "\\" + Constants.StandardErrorFileName);
        stdOut = await stdOutFile.ReadAsStringAsync();
        stdErr = await stdErrFile.ReadAsStringAsync();

        Console.WriteLine("node: {0}:", node.Id);
        Console.WriteLine("stdout.txt: {0}", stdOut);
        Console.WriteLine("stderr.txt: {0}", stdErr);
    }
    else
    {
        Console.WriteLine("\tSubtask {0} is in state {1}", subtask.Id, subtask.State);
    }
});
```

## <a name="code-sample"></a><span data-ttu-id="a47b2-215">Kódminta</span><span class="sxs-lookup"><span data-stu-id="a47b2-215">Code sample</span></span>
<span data-ttu-id="a47b2-216">Hello [MultiInstanceTasks] [ github_mpi] a Githubon példakód azt mutatja be, hogyan toouse egy többpéldányos feladat toorun egy [MS-MPI] [ msmpi_msdn] alkalmazás Batch számítási csomóponton.</span><span class="sxs-lookup"><span data-stu-id="a47b2-216">hello [MultiInstanceTasks][github_mpi] code sample on GitHub demonstrates how toouse a multi-instance task toorun an [MS-MPI][msmpi_msdn] application on Batch compute nodes.</span></span> <span data-ttu-id="a47b2-217">Hello kövesse [előkészítése](#preparation) és [végrehajtási](#execution) toorun hello minta.</span><span class="sxs-lookup"><span data-stu-id="a47b2-217">Follow hello steps in [Preparation](#preparation) and [Execution](#execution) toorun hello sample.</span></span>

### <a name="preparation"></a><span data-ttu-id="a47b2-218">Előkészítése</span><span class="sxs-lookup"><span data-stu-id="a47b2-218">Preparation</span></span>
1. <span data-ttu-id="a47b2-219">Kövesse hello első két [hogyan toocompile és egyszerű MS-MPI program futtatása][msmpi_howto].</span><span class="sxs-lookup"><span data-stu-id="a47b2-219">Follow hello first two steps in [How toocompile and run a simple MS-MPI program][msmpi_howto].</span></span> <span data-ttu-id="a47b2-220">Ez megfelel a következő hello hello prerequesites lépést.</span><span class="sxs-lookup"><span data-stu-id="a47b2-220">This satisfies hello prerequesites for hello following step.</span></span>
2. <span data-ttu-id="a47b2-221">Build egy *kiadás* hello verziójának [MPIHelloWorld] [ helloworld_proj] minta MPI program.</span><span class="sxs-lookup"><span data-stu-id="a47b2-221">Build a *Release* version of hello [MPIHelloWorld][helloworld_proj] sample MPI program.</span></span> <span data-ttu-id="a47b2-222">Ez akkor fog futni a számítási csomópontok hello többpéldányos feladat hello programot.</span><span class="sxs-lookup"><span data-stu-id="a47b2-222">This is hello program that will be run on compute nodes by hello multi-instance task.</span></span>
3. <span data-ttu-id="a47b2-223">Hozzon létre egy zip fájlt tartalmazó `MPIHelloWorld.exe` (melyet beépített 2. lépés) és `MSMpiSetup.exe` (amely letöltött 1. lépés).</span><span class="sxs-lookup"><span data-stu-id="a47b2-223">Create a zip file containing `MPIHelloWorld.exe` (which you built step 2) and `MSMpiSetup.exe` (which you downloaded step 1).</span></span> <span data-ttu-id="a47b2-224">A zip-fájl hello következő lépésben egy alkalmazás csomag fogja feltölteni.</span><span class="sxs-lookup"><span data-stu-id="a47b2-224">You'll upload this zip file as an application package in hello next step.</span></span>
4. <span data-ttu-id="a47b2-225">Használjon hello [Azure-portálon] [ portal] toocreate egy kötegelt [alkalmazás](batch-application-packages.md) "MPIHelloWorld" nevezik, és adja meg az "1.0" verzióként hello előző lépésben létrehozott hello zip-fájl hello alkalmazáscsomagot.</span><span class="sxs-lookup"><span data-stu-id="a47b2-225">Use hello [Azure portal][portal] toocreate a Batch [application](batch-application-packages.md) called "MPIHelloWorld", and specify hello zip file you created in hello previous step as version "1.0" of hello application package.</span></span> <span data-ttu-id="a47b2-226">Lásd: [alkalmazások kezelését és feltöltését](batch-application-packages.md#upload-and-manage-applications) további információt.</span><span class="sxs-lookup"><span data-stu-id="a47b2-226">See [Upload and manage applications](batch-application-packages.md#upload-and-manage-applications) for more information.</span></span>

> [!TIP]
> <span data-ttu-id="a47b2-227">Build egy *kiadás* verziójának `MPIHelloWorld.exe` , hogy nem rendelkezik tooinclude további függőségek (például `msvcp140d.dll` vagy `vcruntime140d.dll`) a alkalmazáscsomagban.</span><span class="sxs-lookup"><span data-stu-id="a47b2-227">Build a *Release* version of `MPIHelloWorld.exe` so that you don't have tooinclude any additional dependencies (for example, `msvcp140d.dll` or `vcruntime140d.dll`) in your application package.</span></span>
>
>

### <a name="execution"></a><span data-ttu-id="a47b2-228">Végrehajtás</span><span class="sxs-lookup"><span data-stu-id="a47b2-228">Execution</span></span>
1. <span data-ttu-id="a47b2-229">Töltse le a hello [azure-köteg-minták] [ github_samples_zip] a Githubról.</span><span class="sxs-lookup"><span data-stu-id="a47b2-229">Download hello [azure-batch-samples][github_samples_zip] from GitHub.</span></span>
2. <span data-ttu-id="a47b2-230">Nyissa meg hello MultiInstanceTasks **megoldás** a Visual Studio 2015-öt vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="a47b2-230">Open hello MultiInstanceTasks **solution** in Visual Studio 2015 or newer.</span></span> <span data-ttu-id="a47b2-231">Hello `MultiInstanceTasks.sln` megoldásfájlt található:</span><span class="sxs-lookup"><span data-stu-id="a47b2-231">hello `MultiInstanceTasks.sln` solution file is located in:</span></span>

    `azure-batch-samples\CSharp\ArticleProjects\MultiInstanceTasks\`
3. <span data-ttu-id="a47b2-232">Adja meg a kötegelt és a tároló hitelesítő adatait a `AccountSettings.settings` a hello **Microsoft.Azure.Batch.Samples.Common** projekt.</span><span class="sxs-lookup"><span data-stu-id="a47b2-232">Enter your Batch and Storage account credentials in `AccountSettings.settings` in hello **Microsoft.Azure.Batch.Samples.Common** project.</span></span>
4. <span data-ttu-id="a47b2-233">**Létrehozása és futtatása** hello MultiInstanceTasks megoldás tooexecute hello MPI mintaalkalmazást a számítási csomópontok a Batch-készlet.</span><span class="sxs-lookup"><span data-stu-id="a47b2-233">**Build and run** hello MultiInstanceTasks solution tooexecute hello MPI sample application on compute nodes in a Batch pool.</span></span>
5. <span data-ttu-id="a47b2-234">*Nem kötelező*: használata hello [Azure-portálon] [ portal] vagy hello [kötegelt Explorer] [ batch_explorer] tooexamine hello minta készlet, feladat, és a feladat ("MultiInstanceSamplePool", "MultiInstanceSampleJob", "MultiInstanceSampleTask") előtt hello erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="a47b2-234">*Optional*: Use hello [Azure portal][portal] or hello [Batch Explorer][batch_explorer] tooexamine hello sample pool, job, and task ("MultiInstanceSamplePool", "MultiInstanceSampleJob", "MultiInstanceSampleTask") before you delete hello resources.</span></span>

> [!TIP]
> <span data-ttu-id="a47b2-235">Letöltheti a [Visual Studio Community] [ visual_studio] szabad, ha nem rendelkezik a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a47b2-235">You can download [Visual Studio Community][visual_studio] for free if you do not have Visual Studio.</span></span>
>
>

<span data-ttu-id="a47b2-236">A kimeneti `MultiInstanceTasks.exe` hasonló toohello következő:</span><span class="sxs-lookup"><span data-stu-id="a47b2-236">Output from `MultiInstanceTasks.exe` is similar toohello following:</span></span>

```
Creating pool [MultiInstanceSamplePool]...
Creating job [MultiInstanceSampleJob]...
Adding task [MultiInstanceSampleTask] toojob [MultiInstanceSampleJob]...
Awaiting task completion, timeout in 00:30:00...

Main task [MultiInstanceSampleTask] is in state [Completed] and ran on compute node [tvm-1219235766_1-20161017t162002z]:
---- stdout.txt ----
Rank 2 received string "Hello world" from Rank 0
Rank 1 received string "Hello world" from Rank 0

---- stderr.txt ----

Main task completed, waiting 00:00:10 for subtasks toocomplete...

---- Subtask information ----
subtask: 1
        exit code: 0
        node: tvm-1219235766_3-20161017t162002z
        stdout.txt:
        stderr.txt:
subtask: 2
        exit code: 0
        node: tvm-1219235766_2-20161017t162002z
        stdout.txt:
        stderr.txt:

Delete job? [yes] no: yes
Delete pool? [yes] no: yes

Sample complete, hit ENTER tooexit...
```

## <a name="next-steps"></a><span data-ttu-id="a47b2-237">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a47b2-237">Next steps</span></span>
* <span data-ttu-id="a47b2-238">ismerteti, amelyek hello Microsoft HPC & Azure Batch csapat blogja [MPI támogatja az Azure Batch Linux][blog_mpi_linux], és használatával kapcsolatos tartalmaz [OpenFOAM] [ openfoam] kötegelt.</span><span class="sxs-lookup"><span data-stu-id="a47b2-238">hello Microsoft HPC & Azure Batch Team blog discusses [MPI support for Linux on Azure Batch][blog_mpi_linux], and includes information on using [OpenFOAM][openfoam] with Batch.</span></span> <span data-ttu-id="a47b2-239">Python-Kódminták találhat meg hello [OpenFOAM példa a Githubon][github_mpi].</span><span class="sxs-lookup"><span data-stu-id="a47b2-239">You can find Python code samples for hello [OpenFOAM example on GitHub][github_mpi].</span></span>
* <span data-ttu-id="a47b2-240">Ismerje meg, hogyan túl[létrehozása Linux számítási csomópontok készleteinek](batch-linux-nodes.md) használható az Azure Batch MPI megoldások.</span><span class="sxs-lookup"><span data-stu-id="a47b2-240">Learn how too[create pools of Linux compute nodes](batch-linux-nodes.md) for use in your Azure Batch MPI solutions.</span></span>

[helloworld_proj]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/MultiInstanceTasks/MPIHelloWorld

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[blog_mpi_linux]: https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/
[cmd_start]: https://technet.microsoft.com/library/cc770297.aspx
[coord_cmd_example]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/article_samples/mpi/data/linux/openfoam/coordination-cmd
[github_mpi]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/MultiInstanceTasks
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_zip]: https://github.com/Azure/azure-batch-samples/archive/master.zip
[msdn_env_var]: https://msdn.microsoft.com/library/azure/mt743623.aspx
[msmpi_msdn]: https://msdn.microsoft.com/library/bb524831.aspx
[msmpi_sdk]: http://go.microsoft.com/FWLink/p/?LinkID=389556
[msmpi_howto]: http://blogs.technet.com/b/windowshpc/archive/2015/02/02/how-to-compile-and-run-a-simple-ms-mpi-program.aspx
[openfoam]: http://www.openfoam.com/
[visual_studio]: https://www.visualstudio.com/vs/community/

[net_jobprep]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.aspx
[net_multiinstance_class]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.aspx
[net_multiinstance_prop]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.multiinstancesettings.aspx
[net_multiinsance_commonresfiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.commonresourcefiles.aspx
[net_multiinstance_coordcmdline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.coordinationcommandline.aspx
[net_multiinstance_numinstances]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.numberofinstances.aspx
[net_pool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_pool_create]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx
[net_pool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx
[net_resourcefile]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.resourcefile.aspx
[net_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.aspx
[net_starttask_cmdline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.commandline.aspx
[net_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_taskconstraints]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.aspx
[net_taskconstraint_maxretry]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.maxtaskretrycount.aspx
[net_taskconstraint_maxwallclock]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.maxwallclocktime.aspx
[net_taskconstraint_retention]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.retentiontime.aspx
[net_task_listsubtasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listsubtasks.aspx
[net_task_listnodefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listnodefiles.aspx
[poolops_getnodefile]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.getnodefile.aspx

[portal]: https://portal.azure.com
[rest_multiinstance]: https://msdn.microsoft.com/library/azure/mt637905.aspx

[1]: ./media/batch-mpi/batch_mpi_01.png "Többpéldányos áttekintése"
