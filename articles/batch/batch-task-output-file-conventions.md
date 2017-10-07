---
title: "aaaPersist feladat- és kimeneti tooAzure tárolási hello fájl egyezmények könyvtárhoz a .NET - Azure Batch |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Azure Batch fájl egyezmények kódtára a .NET toopersist kötegelt feladat és a feladat kimenete tooAzure tároló és a nézet hello megőrzött kimenetet a hello Azure-portálon."
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
ms.assetid: 16e12d0e-958c-46c2-a6b8-7843835d830e
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 06/16/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: cf2ac8632a13d32438c1bdcf11b4b9649de1e2b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="persist-job-and-task-data-tooazure-storage-with-hello-batch-file-conventions-library-for-net-toopersist"></a><span data-ttu-id="6dc98-103">Feladat megmaradnak, és az adatok tooAzure tárolási hello kötegelt fájl egyezmények kódtára a .NET toopersist a feladat</span><span class="sxs-lookup"><span data-stu-id="6dc98-103">Persist job and task data tooAzure Storage with hello Batch File Conventions library for .NET toopersist</span></span> 

[!INCLUDE [batch-task-output-include](../../includes/batch-task-output-include.md)]

<span data-ttu-id="6dc98-104">Egyirányú toopersist feladat adata toouse hello [Azure Batch fájl egyezmények .NET-keretrendszerhez készült][nuget_package].</span><span class="sxs-lookup"><span data-stu-id="6dc98-104">One way toopersist task data is toouse hello [Azure Batch File Conventions library for .NET][nuget_package].</span></span> <span data-ttu-id="6dc98-105">hello fájl egyezmények könyvtár egyszerűbben hello tárolása tevékenység kimeneti adatok tooAzure tárolási és azt lekérése során.</span><span class="sxs-lookup"><span data-stu-id="6dc98-105">hello File Conventions library simplifies hello process of storing task output data tooAzure Storage and retrieving it.</span></span> <span data-ttu-id="6dc98-106">Használhatja a feladat- és ügyfél-kód hello fájl egyezmények szalagtár &mdash; feladatkód tárolásakor fájlok, és az ügyfél code toolist, és kérheti le azokat.</span><span class="sxs-lookup"><span data-stu-id="6dc98-106">You can use hello File Conventions library in both task and client code &mdash; in task code for persisting files, and in client code toolist and retrieve them.</span></span> <span data-ttu-id="6dc98-107">A feladat kód segítségével is hello könyvtár tooretrieve hello kimeneti fölérendelt feladatokat, többek között a [függőségek feladat](batch-task-dependencies.md) forgatókönyv.</span><span class="sxs-lookup"><span data-stu-id="6dc98-107">Your task code can also use hello library tooretrieve hello output of upstream tasks, such as in a [task dependencies](batch-task-dependencies.md) scenario.</span></span> 

<span data-ttu-id="6dc98-108">tooretrieve kimeneti fájlok hello fájl egyezmények könyvtárhoz, hello fájlok egy adott feladat vagy tevékenység azonosítója és a cél szerint felsorolva keresheti.</span><span class="sxs-lookup"><span data-stu-id="6dc98-108">tooretrieve output files with hello File Conventions library, you can locate hello files for a given job or task by listing them by ID and purpose.</span></span> <span data-ttu-id="6dc98-109">Nincs szükség tooknow hello nevek vagy hello fájlok helyét.</span><span class="sxs-lookup"><span data-stu-id="6dc98-109">You don't need tooknow hello names or locations of hello files.</span></span> <span data-ttu-id="6dc98-110">Például egy adott feladat hello fájl egyezmények könyvtár toolist összes köztes fájlt használja, vagy egy adott feladat preview fájl.</span><span class="sxs-lookup"><span data-stu-id="6dc98-110">For example, you can use hello File Conventions library toolist all intermediate files for a given task, or get a preview file for a given job.</span></span>

> [!TIP]
> <span data-ttu-id="6dc98-111">Verziójával 2017-05-01-től kezdődően hello Batch szolgáltatás API támogatja tárolásakor kimeneti adatok tooAzure tároló és a virtuálisgép-konfiguráció hello létre készletek futó feladat manager feladatok.</span><span class="sxs-lookup"><span data-stu-id="6dc98-111">Starting with version 2017-05-01, hello Batch service API supports persisting output data tooAzure Storage for tasks and job manager tasks that run on pools created with hello virtual machine configuration.</span></span> <span data-ttu-id="6dc98-112">hello Batch szolgáltatás API biztosít egy egyszerű módon toopersist hello kód, amely létrehoz egy feladatot, és egy alternatív toohello fájl egyezmények könyvtár funkcionál kimenetét.</span><span class="sxs-lookup"><span data-stu-id="6dc98-112">hello Batch service API provides a simple way toopersist output from within hello code that creates a task and serves as an alternative toohello File Conventions library.</span></span> <span data-ttu-id="6dc98-113">Módosíthatja a kötegelt ügyfél alkalmazások toopersist kimeneti anélkül, hogy a feladat futásának tooupdate hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="6dc98-113">You can modify your Batch client applications toopersist output without needing tooupdate hello application that your task is running.</span></span> <span data-ttu-id="6dc98-114">További információkért lásd: [megőrizni a feladat adatainak tooAzure tárolási hello Batch szolgáltatás API](batch-task-output-files.md).</span><span class="sxs-lookup"><span data-stu-id="6dc98-114">For more information, see [Persist task data tooAzure Storage with hello Batch service API](batch-task-output-files.md).</span></span>
> 
> 

## <a name="when-do-i-use-hello-file-conventions-library-toopersist-task-output"></a><span data-ttu-id="6dc98-115">Ha használja a hello fájl egyezmények könyvtár toopersist feladat kimenetének?</span><span class="sxs-lookup"><span data-stu-id="6dc98-115">When do I use hello File Conventions library toopersist task output?</span></span>

<span data-ttu-id="6dc98-116">Az Azure Batch szolgáltatás több mint egyirányú toopersist feladat kimenete.</span><span class="sxs-lookup"><span data-stu-id="6dc98-116">Azure Batch provides more than one way toopersist task output.</span></span> <span data-ttu-id="6dc98-117">hello fájl egyezmények a legjobb olyan környezethez a legalkalmasabb toothese forgatókönyvek:</span><span class="sxs-lookup"><span data-stu-id="6dc98-117">hello File Conventions is best suited toothese scenarios:</span></span>

- <span data-ttu-id="6dc98-118">Hello kódot, hogy a feladat futásának toopersist fájlokat a hello fájl egyezmények könyvtár hello alkalmazás könnyen módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="6dc98-118">You can easily modify hello code for hello application that your task is running toopersist files using hello File Conventions library.</span></span>
- <span data-ttu-id="6dc98-119">Érdemes toostream adatok tooAzure tárolási hello feladat futása közben.</span><span class="sxs-lookup"><span data-stu-id="6dc98-119">You want toostream data tooAzure Storage while hello task is still running.</span></span>
- <span data-ttu-id="6dc98-120">Azt szeretné, hogy a készletek hello felhőalapú szolgáltatás konfigurációja vagy a virtuálisgép-konfiguráció hello toopersist adatait.</span><span class="sxs-lookup"><span data-stu-id="6dc98-120">You want toopersist data from pools created with either hello cloud service configuration or hello virtual machine configuration.</span></span>
- <span data-ttu-id="6dc98-121">Az ügyfélalkalmazás vagy más feladatok hello igények toolocate feladat, és töltse le a tevékenység kimeneti fájlok azonosító vagy célja.</span><span class="sxs-lookup"><span data-stu-id="6dc98-121">Your client application or other tasks in hello job needs toolocate and download task output files by ID or by purpose.</span></span> 
- <span data-ttu-id="6dc98-122">Azt szeretné, hogy a feladat kimenetének tooview hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="6dc98-122">You want tooview task output in hello Azure portal.</span></span>

<span data-ttu-id="6dc98-123">Ha a forgatókönyv eltér a fent felsorolt, szükség lehet tooconsider másik módszert.</span><span class="sxs-lookup"><span data-stu-id="6dc98-123">If your scenario differs from those listed above, you may need tooconsider a different approach.</span></span> <span data-ttu-id="6dc98-124">Más beállításokat a tárolásakor feladat kimenetének további információkért lásd: [Persist feladat- és kimeneti tooAzure tárolási](batch-task-output.md).</span><span class="sxs-lookup"><span data-stu-id="6dc98-124">For more information on other options for persisting task output, see [Persist job and task output tooAzure Storage](batch-task-output.md).</span></span> 

## <a name="what-is-hello-batch-file-conventions-standard"></a><span data-ttu-id="6dc98-125">Mi az a hello kötegelt fájl egyezmények szabványos?</span><span class="sxs-lookup"><span data-stu-id="6dc98-125">What is hello Batch File Conventions standard?</span></span>

<span data-ttu-id="6dc98-126">Hello [kötegelt fájl egyezmények standard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions) egy elnevezési sémát biztosít hello cél tárolók és a blob elérési utak toowhich a kimeneti fájlok kerülnek.</span><span class="sxs-lookup"><span data-stu-id="6dc98-126">hello [Batch File Conventions standard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions) provides a naming scheme for hello destination containers and blob paths toowhich your output files are written.</span></span> <span data-ttu-id="6dc98-127">Fájlok megőrzött tooAzure tároló, amely igazodik toohello fájl egyezmények szabványos automatikusan elérhetők megtekintését a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="6dc98-127">Files persisted tooAzure Storage that adhere toohello File Conventions standard are automatically available for viewing in hello Azure portal.</span></span> <span data-ttu-id="6dc98-128">hello portal ismeri a hello elnevezési konvenciót, és így megjeleníthetik a fájlokat, tooit igazodik.</span><span class="sxs-lookup"><span data-stu-id="6dc98-128">hello portal is aware of hello naming convention and so can display files that adhere tooit.</span></span>

<span data-ttu-id="6dc98-129">hello fájl egyezmények .NET-keretrendszerhez készült automatikusan nevet, a tároló- és tevékenység kimeneti fájlok toohello szabványos fájl konvenciók szerint.</span><span class="sxs-lookup"><span data-stu-id="6dc98-129">hello File Conventions library for .NET automatically names your storage containers and task output files according toohello File Conventions standard.</span></span> <span data-ttu-id="6dc98-130">hello fájl egyezmények kódtár is biztosít a módszerek tooquery kimeneti fájlok az Azure Storage toojob azonosító, a tevékenység azonosítója vagy a cél alapján történik.</span><span class="sxs-lookup"><span data-stu-id="6dc98-130">hello File Conventions library also provides methods tooquery output files in Azure Storage according toojob ID, task ID, or purpose.</span></span>   

<span data-ttu-id="6dc98-131">Ha .NET eltérő nyelvű fejleszt, valósíthatja meg hello fájl egyezmények standard saját magának az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="6dc98-131">If you are developing with a language other than .NET, you can implement hello File Conventions standard yourself in your application.</span></span> <span data-ttu-id="6dc98-132">További információkért lásd: [kapcsolatos hello kötegelt fájl egyezmények standard](batch-task-output.md#about-the-batch-file-conventions-standard).</span><span class="sxs-lookup"><span data-stu-id="6dc98-132">For more information, see [About hello Batch File Conventions standard](batch-task-output.md#about-the-batch-file-conventions-standard).</span></span>

## <a name="link-an-azure-storage-account-tooyour-batch-account"></a><span data-ttu-id="6dc98-133">Egy Azure Storage-fiók tooyour Batch-fiók csatolása</span><span class="sxs-lookup"><span data-stu-id="6dc98-133">Link an Azure Storage account tooyour Batch account</span></span>

<span data-ttu-id="6dc98-134">toopersist a kimeneti adatok tooAzure hello fájl egyezmények szalagtárat használó tárolási, kell először kapcsolni az Azure Storage-fiók tooyour Batch-fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="6dc98-134">toopersist output data tooAzure Storage using hello File Conventions library, you must first link an Azure Storage account tooyour Batch account.</span></span> <span data-ttu-id="6dc98-135">Ha még nem tette meg, a tárolási fiók tooyour Batch-fiókhoz csatolja hello segítségével [Azure-portálon](https://portal.azure.com):</span><span class="sxs-lookup"><span data-stu-id="6dc98-135">If you haven't done so already, link a Storage account tooyour Batch account by using hello [Azure portal](https://portal.azure.com):</span></span>

1. <span data-ttu-id="6dc98-136">Keresse meg a Batch-fiók tooyour hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="6dc98-136">Navigate tooyour Batch account in hello Azure portal.</span></span> 
2. <span data-ttu-id="6dc98-137">A **beállítások**, jelölje be **Tárfiók**.</span><span class="sxs-lookup"><span data-stu-id="6dc98-137">Under **Settings**, select **Storage Account**.</span></span>
3. <span data-ttu-id="6dc98-138">Ha még nem rendelkezik egy tárfiókot, a Batch-fiókhoz társított, kattintson a **(nincs) Storage-fiók**.</span><span class="sxs-lookup"><span data-stu-id="6dc98-138">If you do not already have a Storage account associated with your Batch account, click **Storage Account (None)**.</span></span>
4. <span data-ttu-id="6dc98-139">Válasszon egy tárfiókot az előfizetéshez tartozó hello listából.</span><span class="sxs-lookup"><span data-stu-id="6dc98-139">Select a Storage account from hello list for your subscription.</span></span> <span data-ttu-id="6dc98-140">A legjobb teljesítmény érdekében használjon, amely hello Azure Storage-fiók ugyanabban a régióban hello Batch-fiókhoz, amelyen fut a feladat.</span><span class="sxs-lookup"><span data-stu-id="6dc98-140">For best performance, use an Azure Storage account that is in hello same region as hello Batch account where your tasks are running.</span></span>

## <a name="persist-output-data"></a><span data-ttu-id="6dc98-141">Kimeneti adatok megőrzése</span><span class="sxs-lookup"><span data-stu-id="6dc98-141">Persist output data</span></span>

<span data-ttu-id="6dc98-142">toopersist feladat- és kimeneti adatai hello fájl egyezmények könyvtárhoz, a tároló létrehozása az Azure Storage, majd mentse a hello kimeneti toohello tároló.</span><span class="sxs-lookup"><span data-stu-id="6dc98-142">toopersist job and task output data with hello File Conventions library, create a container in Azure Storage, then save hello output toohello container.</span></span> <span data-ttu-id="6dc98-143">Használjon hello [Azure Storage ügyféloldali kódtára a .NET](https://www.nuget.org/packages/WindowsAzure.Storage) a feladat kód tooupload hello tevékenység kimeneti toohello tárolóban.</span><span class="sxs-lookup"><span data-stu-id="6dc98-143">Use hello [Azure Storage client library for .NET](https://www.nuget.org/packages/WindowsAzure.Storage) in your task code tooupload hello task output toohello container.</span></span> 

<span data-ttu-id="6dc98-144">A tárolók és blobok az Azure Storage használatáról további információk: [az Azure Blob storage .NET használatának első lépései](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span><span class="sxs-lookup"><span data-stu-id="6dc98-144">For more information about working with containers and blobs in Azure Storage, see [Get started with Azure Blob storage using .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span></span>

> [!WARNING]
> <span data-ttu-id="6dc98-145">Feladat- és mindegyikhez hello fájl egyezmények könyvtárban tárolják a megőrzött hello ugyanabban a tárolóban.</span><span class="sxs-lookup"><span data-stu-id="6dc98-145">All job and task outputs persisted with hello File Conventions library are stored in hello same container.</span></span> <span data-ttu-id="6dc98-146">Ha nagyszámú feladatok próbálja toopersist fájlok: hello azonos idő, [szabályozás korlátok tárolási](../storage/common/storage-performance-checklist.md#blobs) is kényszeríthető.</span><span class="sxs-lookup"><span data-stu-id="6dc98-146">If a large number of tasks try toopersist files at hello same time, [storage throttling limits](../storage/common/storage-performance-checklist.md#blobs) may be enforced.</span></span>
> 
> 

### <a name="create-storage-container"></a><span data-ttu-id="6dc98-147">A tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="6dc98-147">Create storage container</span></span>

<span data-ttu-id="6dc98-148">toopersist tevékenység kimeneti tooAzure tárolási, először létre kell hoznia egy tárolót meghívásával [CloudJob][net_cloudjob].[ PrepareOutputStorageAsync][net_prepareoutputasync].</span><span class="sxs-lookup"><span data-stu-id="6dc98-148">toopersist task output tooAzure Storage, first create a container by calling [CloudJob][net_cloudjob].[PrepareOutputStorageAsync][net_prepareoutputasync].</span></span> <span data-ttu-id="6dc98-149">A bővítmény metódus egy [CloudStorageAccount] [ net_cloudstorageaccount] objektum paraméterként.</span><span class="sxs-lookup"><span data-stu-id="6dc98-149">This extension method takes a [CloudStorageAccount][net_cloudstorageaccount] object as a parameter.</span></span> <span data-ttu-id="6dc98-150">Létrehoz egy tároló elnevezett függően toohello fájl egyezmények szabványos, így a tartalma által felderíthető hello Azure portál és hello lekérés ismertetett módszerek hello cikk későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="6dc98-150">It creates a container named according toohello File Conventions standard,so that its contents are discoverable by hello Azure portal and hello retrieval methods discussed later in hello article.</span></span>

<span data-ttu-id="6dc98-151">Hello kód toocreate a tároló általában helyez az ügyfélalkalmazást &mdash; hello a készletek, a feladatok és a feladatok létrehozó alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="6dc98-151">You typically place hello code toocreate a container in your client application &mdash; hello application that creates your pools, jobs, and tasks.</span></span>

```csharp
CloudJob job = batchClient.JobOperations.CreateJob(
    "myJob",
    new PoolInformation { PoolId = "myPool" });

// Create reference toohello linked Azure Storage account
CloudStorageAccount linkedStorageAccount =
    new CloudStorageAccount(myCredentials, true);

// Create hello blob storage container for hello outputs
await job.PrepareOutputStorageAsync(linkedStorageAccount);
```

### <a name="store-task-outputs"></a><span data-ttu-id="6dc98-152">Tárolási feladatok kimenete</span><span class="sxs-lookup"><span data-stu-id="6dc98-152">Store task outputs</span></span>

<span data-ttu-id="6dc98-153">Most, hogy az Azure Storage tárolója előkészítése után, a feladatok mentheti-e a kimeneti toohello tároló hello segítségével [TaskOutputStorage] [ net_taskoutputstorage] osztály hello fájl egyezmények könyvtárban található.</span><span class="sxs-lookup"><span data-stu-id="6dc98-153">Now that you've prepared a container in Azure Storage, tasks can save output toohello container by using hello [TaskOutputStorage][net_taskoutputstorage] class found in hello File Conventions library.</span></span>

<span data-ttu-id="6dc98-154">A feladat kódban, először létre kell hoznia egy [TaskOutputStorage] [ net_taskoutputstorage] objektumot, majd a tevékenységeket hello feladat befejezése után hívható hello [TaskOutputStorage] [ net_taskoutputstorage]. [SaveAsync] [ net_saveasync] metódus toosave a kimeneti tooAzure tároló.</span><span class="sxs-lookup"><span data-stu-id="6dc98-154">In your task code, first create a [TaskOutputStorage][net_taskoutputstorage] object, then when hello task has completed its work, call hello [TaskOutputStorage][net_taskoutputstorage].[SaveAsync][net_saveasync] method toosave its output tooAzure Storage.</span></span>

```csharp
CloudStorageAccount linkedStorageAccount = new CloudStorageAccount(myCredentials);
string jobId = Environment.GetEnvironmentVariable("AZ_BATCH_JOB_ID");
string taskId = Environment.GetEnvironmentVariable("AZ_BATCH_TASK_ID");

TaskOutputStorage taskOutputStorage = new TaskOutputStorage(
    linkedStorageAccount, jobId, taskId);

/* Code tooprocess data and produce output file(s) */

await taskOutputStorage.SaveAsync(TaskOutputKind.TaskOutput, "frame_full_res.jpg");
await taskOutputStorage.SaveAsync(TaskOutputKind.TaskPreview, "frame_low_res.jpg");
```

<span data-ttu-id="6dc98-155">Hello `kind` hello paramétere [TaskOutputStorage](https://msdn.microsoft.com/library/microsoft.azure.batch.conventions.files.taskoutputstorage.aspx).[ SaveAsync](https://msdn.microsoft.com/library/microsoft.azure.batch.conventions.files.taskoutputstorage.saveasync.aspx) metódus kategorizálja hello megőrzött fájlok.</span><span class="sxs-lookup"><span data-stu-id="6dc98-155">hello `kind` parameter of hello [TaskOutputStorage](https://msdn.microsoft.com/library/microsoft.azure.batch.conventions.files.taskoutputstorage.aspx).[SaveAsync](https://msdn.microsoft.com/library/microsoft.azure.batch.conventions.files.taskoutputstorage.saveasync.aspx) method categorizes hello persisted files.</span></span> <span data-ttu-id="6dc98-156">Nincsenek négy előre definiált [TaskOutputKind] [ net_taskoutputkind] típusok: `TaskOutput`, `TaskPreview`, `TaskLog`, és `TaskIntermediate.` kimeneti egyéni kategóriáinak is meghatározhat.</span><span class="sxs-lookup"><span data-stu-id="6dc98-156">There are four predefined [TaskOutputKind][net_taskoutputkind] types: `TaskOutput`, `TaskPreview`, `TaskLog`, and `TaskIntermediate.` You can also define custom categories of output.</span></span>

<span data-ttu-id="6dc98-157">Kimeneti típusaival milyen típusú kimenetek toolist előfordulhat, hogy később kötegelt hello a megőrzött egy adott feladat kimenetének toospecify engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="6dc98-157">These output types allow you toospecify which type of outputs toolist when you later query Batch for hello persisted outputs of a given task.</span></span> <span data-ttu-id="6dc98-158">Ez azt jelenti amikor hello kimenetek tevékenységek listájának szűrheti hello listát hello kimeneti típusok egyike.</span><span class="sxs-lookup"><span data-stu-id="6dc98-158">In other words, when you list hello outputs for a task, you can filter hello list on one of hello output types.</span></span> <span data-ttu-id="6dc98-159">Például "hello engedi *előzetes* tevékenység kimeneti *109*."</span><span class="sxs-lookup"><span data-stu-id="6dc98-159">For example, "Give me hello *preview* output for task *109*."</span></span> <span data-ttu-id="6dc98-160">Több listázása és kimenetének beolvasása megjelenik [lekérnie a kimenetet](#retrieve-output) újabb hello cikkben.</span><span class="sxs-lookup"><span data-stu-id="6dc98-160">More on listing and retrieving outputs appears in [Retrieve output](#retrieve-output) later in hello article.</span></span>

> [!TIP]
> <span data-ttu-id="6dc98-161">hello kimeneti jellegű is meghatározza, hogy a hello Azure portál egy adott fájl helyére: *TaskOutput*-kategorizált fájlok alatt szerepelhet **tevékenység kimeneti fájlok**, és *TaskLog*fájlok alatt szerepelhet **naplók feladat**.</span><span class="sxs-lookup"><span data-stu-id="6dc98-161">hello output kind also determines where in hello Azure portal a particular file appears: *TaskOutput*-categorized files appear under **Task output files**, and *TaskLog* files appear under **Task logs**.</span></span>
> 
> 

### <a name="store-job-outputs"></a><span data-ttu-id="6dc98-162">Feladat kimenetének tárolásához</span><span class="sxs-lookup"><span data-stu-id="6dc98-162">Store job outputs</span></span>

<span data-ttu-id="6dc98-163">Továbbá toostoring feladatok kimenete, tárolható egy teljes feladattal társított hello kimenetek.</span><span class="sxs-lookup"><span data-stu-id="6dc98-163">In addition toostoring task outputs, you can store hello outputs associated with an entire job.</span></span> <span data-ttu-id="6dc98-164">Például hello egyesítési feladatban movie megjelenítési feladat, sikerült megőrizni a teljes megjelenítve hello movie a feladat kimenete.</span><span class="sxs-lookup"><span data-stu-id="6dc98-164">For example, in hello merge task of a movie rendering job, you could persist hello fully rendered movie as a job output.</span></span> <span data-ttu-id="6dc98-165">A feladat befejezése után az ügyfélalkalmazást listában, és a hello kimenetek hello feladat beolvasása, és nem nem kell tooquery hello egyedi feladatok.</span><span class="sxs-lookup"><span data-stu-id="6dc98-165">When your job is completed, your client application can list and retrieve hello outputs for hello job, and does not need tooquery hello individual tasks.</span></span>

<span data-ttu-id="6dc98-166">Feladat kimenetére tárolására hívó hello [JobOutputStorage][net_joboutputstorage].[ SaveAsync] [ net_joboutputstorage_saveasync] metódust, és adja meg a hello [JobOutputKind] [ net_joboutputkind] és fájlnév:</span><span class="sxs-lookup"><span data-stu-id="6dc98-166">Store job output by calling hello [JobOutputStorage][net_joboutputstorage].[SaveAsync][net_joboutputstorage_saveasync] method, and specify hello [JobOutputKind][net_joboutputkind] and filename:</span></span>

```csharp
CloudJob job = new JobOutputStorage(acct, jobId);
JobOutputStorage jobOutputStorage = job.OutputStorage(linkedStorageAccount);

await jobOutputStorage.SaveAsync(JobOutputKind.JobOutput, "mymovie.mp4");
await jobOutputStorage.SaveAsync(JobOutputKind.JobPreview, "mymovie_preview.mp4");
```

<span data-ttu-id="6dc98-167">A hello **TaskOutputKind** típust a feladat kimenetének hello használnia [JobOutputKind] [ net_joboutputkind] típus toocategorize egy feladatot a megőrzött fájlok.</span><span class="sxs-lookup"><span data-stu-id="6dc98-167">As with hello **TaskOutputKind** type for task outputs, you use hello [JobOutputKind][net_joboutputkind] type toocategorize a job's persisted files.</span></span> <span data-ttu-id="6dc98-168">Ez a paraméter lehetővé teszi (lista) egy adott típusú kimeneti toolater lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="6dc98-168">This parameter allows you toolater query for (list) a specific type of output.</span></span> <span data-ttu-id="6dc98-169">Hello **JobOutputKind** típusú kimeneti és a kép tartalmazza, és támogatja az egyéni kategóriák.</span><span class="sxs-lookup"><span data-stu-id="6dc98-169">hello **JobOutputKind** type includes both output and preview categories, and supports creating custom categories.</span></span>

### <a name="store-task-logs"></a><span data-ttu-id="6dc98-170">A feladat naplók tárolására</span><span class="sxs-lookup"><span data-stu-id="6dc98-170">Store task logs</span></span>

<span data-ttu-id="6dc98-171">Ezenkívül toopersisting egy fájl toodurable tárolására, ha egy feladat vagy a feladat befejeződik, szükséges lehet a toopersist fájlok hello egy feladat végrehajtása során &mdash; naplófájlok vagy `stdout.txt` és `stderr.txt`, például.</span><span class="sxs-lookup"><span data-stu-id="6dc98-171">In addition toopersisting a file toodurable storage when a task or job completes, you may need toopersist files that are updated during hello execution of a task &mdash; log files or `stdout.txt` and `stderr.txt`, for example.</span></span> <span data-ttu-id="6dc98-172">Erre a célra hello Azure Batch fájl egyezmények kódtár biztosít hello [TaskOutputStorage][net_taskoutputstorage].[ SaveTrackedAsync] [ net_savetrackedasync] metódust.</span><span class="sxs-lookup"><span data-stu-id="6dc98-172">For this purpose, hello Azure Batch File Conventions library provides hello [TaskOutputStorage][net_taskoutputstorage].[SaveTrackedAsync][net_savetrackedasync] method.</span></span> <span data-ttu-id="6dc98-173">A [SaveTrackedAsync][net_savetrackedasync], nyomon követheti a frissítések tooa fájlt (a megadott időközönként) hello csomóponton, és ezen frissítések tooAzure tárolási megőrzéséhez.</span><span class="sxs-lookup"><span data-stu-id="6dc98-173">With [SaveTrackedAsync][net_savetrackedasync], you can track updates tooa file on hello node (at an interval that you specify) and persist those updates tooAzure Storage.</span></span>

<span data-ttu-id="6dc98-174">A következő kódrészletet hello, használjuk [SaveTrackedAsync] [ net_savetrackedasync] tooupdate `stdout.txt` az Azure Storage 15 másodpercenként hello hello feladat végrehajtása során:</span><span class="sxs-lookup"><span data-stu-id="6dc98-174">In hello following code snippet, we use [SaveTrackedAsync][net_savetrackedasync] tooupdate `stdout.txt` in Azure Storage every 15 seconds during hello execution of hello task:</span></span>

```csharp
TimeSpan stdoutFlushDelay = TimeSpan.FromSeconds(3);
string logFilePath = Path.Combine(
    Environment.GetEnvironmentVariable("AZ_BATCH_TASK_DIR"), "stdout.txt");

// hello primary task logic is wrapped in a using statement that sends updates to
// hello stdout.txt blob in Storage every 15 seconds while hello task code runs.
using (ITrackedSaveOperation stdout =
        await taskStorage.SaveTrackedAsync(
        TaskOutputKind.TaskLog,
        logFilePath,
        "stdout.txt",
        TimeSpan.FromSeconds(15)))
{
    /* Code tooprocess data and produce output file(s) */

    // We are tracking hello disk file toosave our standard output, but the
    // node agent may take up too3 seconds tooflush hello stdout stream to
    // disk. So give hello file a moment toocatch up.
     await Task.Delay(stdoutFlushDelay);
}
```

<span data-ttu-id="6dc98-175">hello megjegyzésként szakasz `Code tooprocess data and produce output file(s)` hello kódot, amely a feladat általában hajtaná végre helyőrzője.</span><span class="sxs-lookup"><span data-stu-id="6dc98-175">hello commented section `Code tooprocess data and produce output file(s)` is a placeholder for hello code that your task would normally perform.</span></span> <span data-ttu-id="6dc98-176">Például lehetséges, hogy kódot, amely letölti az adatokat az Azure Storage és átalakítás vagy számítási végez azt.</span><span class="sxs-lookup"><span data-stu-id="6dc98-176">For example, you might have code that downloads data from Azure Storage and performs transformation or calculation on it.</span></span> <span data-ttu-id="6dc98-177">hello fontos része a részlet van bemutatásához, hogyan akkor futtathatja a kódok egy `using` blokk tooperiodically fájl frissítése [SaveTrackedAsync][net_savetrackedasync].</span><span class="sxs-lookup"><span data-stu-id="6dc98-177">hello important part of this snippet is demonstrating how you can wrap such code in a `using` block tooperiodically update a file with [SaveTrackedAsync][net_savetrackedasync].</span></span>

<span data-ttu-id="6dc98-178">hello csomópont ügynök egy olyan program, hello készlet minden egyes csomópontján fut, és hello parancs vezérlő és felületet hello csomópont és hello Batch szolgáltatás között.</span><span class="sxs-lookup"><span data-stu-id="6dc98-178">hello node agent is a program that runs on each node in hello pool and provides hello command-and-control interface between hello node and hello Batch service.</span></span> <span data-ttu-id="6dc98-179">Hello `Task.Delay` tekintendő, amely szükséges a végén hello ez `using` blokk tooensure, hogy a csomópont ügynök hello tartalma idő tooflush hello standard toohello stdout.txt fájlt hello csomóponton.</span><span class="sxs-lookup"><span data-stu-id="6dc98-179">hello `Task.Delay` call is required at hello end of this `using` block tooensure that hello node agent has time tooflush hello contents of standard out toohello stdout.txt file on hello node.</span></span> <span data-ttu-id="6dc98-180">Ez a késleltetés nélkül is lehetséges toomiss hello kimenet utolsó néhány másodpercig.</span><span class="sxs-lookup"><span data-stu-id="6dc98-180">Without this delay, it is possible toomiss hello last few seconds of output.</span></span> <span data-ttu-id="6dc98-181">Ez a késés nem lehet szükség az összes fájl.</span><span class="sxs-lookup"><span data-stu-id="6dc98-181">This delay may not be required for all files.</span></span>

> [!NOTE]
> <span data-ttu-id="6dc98-182">Ha engedélyezi a nyomkövetési fájl **SaveTrackedAsync**, csak *hozzáfűzi* toohello nyomon követett fájl megőrzött tooAzure tárolási.</span><span class="sxs-lookup"><span data-stu-id="6dc98-182">When you enable file tracking with **SaveTrackedAsync**, only *appends* toohello tracked file are persisted tooAzure Storage.</span></span> <span data-ttu-id="6dc98-183">Ezt a módszert csak a nyomkövetési naplófájlok nem elforgatása, vagy más fájlok toowith írt hozzáfűzése hello fájl műveletek toohello végét.</span><span class="sxs-lookup"><span data-stu-id="6dc98-183">Use this method only for tracking non-rotating log files or other files that are written toowith append operations toohello end of hello file.</span></span>
> 
> 

## <a name="retrieve-output-data"></a><span data-ttu-id="6dc98-184">Kimeneti adatainak beolvasása</span><span class="sxs-lookup"><span data-stu-id="6dc98-184">Retrieve output data</span></span>

<span data-ttu-id="6dc98-185">Beolvasni a megőrzött kimeneti hello Azure Batch fájl egyezmények kódtár használatával, ha ezt teszi feladat - és a feladat-központú módon.</span><span class="sxs-lookup"><span data-stu-id="6dc98-185">When you retrieve your persisted output using hello Azure Batch File Conventions library, you do so in a task- and job-centric manner.</span></span> <span data-ttu-id="6dc98-186">Hello kimeneti a feladat vagy a feladatot a megadott tooknow anélkül Azure Storage, vagy még a fájl neve az elérési úttal is igényelhet.</span><span class="sxs-lookup"><span data-stu-id="6dc98-186">You can request hello output for given task or job without needing tooknow its path in Azure Storage, or even its file name.</span></span> <span data-ttu-id="6dc98-187">Ehelyett kérheti a kimeneti fájlok feladat, vagy sikertelen feladat-azonosítót.</span><span class="sxs-lookup"><span data-stu-id="6dc98-187">Instead, you can request output files by task or job ID.</span></span>

<span data-ttu-id="6dc98-188">hello következő kódrészletet a feladat tevékenységeit telepítéseket hello kimeneti fájlok hello feladat kapcsolatos információkat jelenít és tárolásból tölti le a fájlokat.</span><span class="sxs-lookup"><span data-stu-id="6dc98-188">hello following code snippet iterates through a job's tasks, prints some information about hello output files for hello task, and then downloads its files from Storage.</span></span>

```csharp
foreach (CloudTask task in myJob.ListTasks())
{
    foreach (OutputFileReference output in
        task.OutputStorage(storageAccount).ListOutputs(
            TaskOutputKind.TaskOutput))
    {
        Console.WriteLine($"output file: {output.FilePath}");

        output.DownloadToFileAsync(
            $"{jobId}-{output.FilePath}",
            System.IO.FileMode.Create).Wait();
    }
}
```

## <a name="view-output-files-in-hello-azure-portal"></a><span data-ttu-id="6dc98-189">A kimeneti fájlok nézet hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="6dc98-189">View output files in hello Azure portal</span></span>

<span data-ttu-id="6dc98-190">hello Azure-portál megjeleníti tevékenység kimeneti fájlok és a megőrzött tooa-naplók kapcsolódó hello Azure Storage-fiókra [kötegelt fájl egyezmények standard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions).</span><span class="sxs-lookup"><span data-stu-id="6dc98-190">hello Azure portal displays task output files and logs that are persisted tooa linked Azure Storage account using hello [Batch File Conventions standard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions).</span></span> <span data-ttu-id="6dc98-191">Megvalósíthat szabályoknak saját kezűleg a hello egy tetszőleges nyelv, vagy hello fájl egyezmények könyvtár a .NET-alkalmazásokban használható.</span><span class="sxs-lookup"><span data-stu-id="6dc98-191">You can implement these conventions yourself in hello a language of your choice, or you can use hello File Conventions library in your .NET applications.</span></span>

<span data-ttu-id="6dc98-192">a kimeneti fájlok hello portálon tooenable hello megjelenítését, meg kell felelniük a következő követelmények hello:</span><span class="sxs-lookup"><span data-stu-id="6dc98-192">tooenable hello display of your output files in hello portal, you must satisfy hello following requirements:</span></span>

1. <span data-ttu-id="6dc98-193">[Egy Azure Storage-fiók csatolása](#requirement-linked-storage-account) tooyour Batch-fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="6dc98-193">[Link an Azure Storage account](#requirement-linked-storage-account) tooyour Batch account.</span></span>
2. <span data-ttu-id="6dc98-194">Az előre megadott toohello tároló- és a fájlok elnevezési szabályai igazodik, amikor kimenetek megőrzése.</span><span class="sxs-lookup"><span data-stu-id="6dc98-194">Adhere toohello predefined naming conventions for Storage containers and files when persisting outputs.</span></span> <span data-ttu-id="6dc98-195">Ezek egyezmények hello definition hello fájl egyezmények könyvtárban található [információs][github_file_conventions_readme].</span><span class="sxs-lookup"><span data-stu-id="6dc98-195">You can find hello definition of these conventions in hello File Conventions library [README][github_file_conventions_readme].</span></span> <span data-ttu-id="6dc98-196">Ha hello [Azure Batch fájl egyezmények] [ nuget_package] könyvtár toopersist a kimeneti, a fájlok megmaradnak toohello szabványos fájl konvenciók szerint.</span><span class="sxs-lookup"><span data-stu-id="6dc98-196">If you use hello [Azure Batch File Conventions][nuget_package] library toopersist your output, your files are persisted according toohello File Conventions standard.</span></span>

<span data-ttu-id="6dc98-197">tooview tevékenység kimeneti fájlokat, és bejelentkezik hello Azure-portálon, keresse meg a toohello tevékenység, amelynek kimenete érdekli, majd kattintson **mentett kimeneti fájlok** vagy **mentett naplókat**.</span><span class="sxs-lookup"><span data-stu-id="6dc98-197">tooview task output files and logs in hello Azure portal, navigate toohello task whose output you are interested in, then click either **Saved output files** or **Saved logs**.</span></span> <span data-ttu-id="6dc98-198">A képen látható hello **mentett kimeneti fájlok** hello feladat "007" azonosítójú:</span><span class="sxs-lookup"><span data-stu-id="6dc98-198">This image shows hello **Saved output files** for hello task with ID "007":</span></span>

<span data-ttu-id="6dc98-199">![A feladat kimenete panel az Azure-portálon hello][2]</span><span class="sxs-lookup"><span data-stu-id="6dc98-199">![Task outputs blade in hello Azure portal][2]</span></span>

## <a name="code-sample"></a><span data-ttu-id="6dc98-200">Kódminta</span><span class="sxs-lookup"><span data-stu-id="6dc98-200">Code sample</span></span>

<span data-ttu-id="6dc98-201">Hello [PersistOutputs] [ github_persistoutputs] mintaprojektet egyike hello [Azure Batch-Kódminták] [ github_samples] a Githubon.</span><span class="sxs-lookup"><span data-stu-id="6dc98-201">hello [PersistOutputs][github_persistoutputs] sample project is one of hello [Azure Batch code samples][github_samples] on GitHub.</span></span> <span data-ttu-id="6dc98-202">A Visual Studio megoldás bemutatja, hogyan toouse hello Azure Batch fájl egyezmények könyvtár toopersist tevékenység kimeneti toodurable tároló.</span><span class="sxs-lookup"><span data-stu-id="6dc98-202">This Visual Studio solution demonstrates how toouse hello Azure Batch File Conventions library toopersist task output toodurable storage.</span></span> <span data-ttu-id="6dc98-203">toorun hello mintát, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="6dc98-203">toorun hello sample, follow these steps:</span></span>

1. <span data-ttu-id="6dc98-204">Nyissa meg hello projektre a **Visual Studio 2015-ös vagy újabb**.</span><span class="sxs-lookup"><span data-stu-id="6dc98-204">Open hello project in **Visual Studio 2015 or newer**.</span></span>
2. <span data-ttu-id="6dc98-205">A kötegelt és a tárolási **fiók hitelesítő adatait** túl**AccountSettings.settings** hello Microsoft.Azure.Batch.Samples.Common projektben.</span><span class="sxs-lookup"><span data-stu-id="6dc98-205">Add your Batch and Storage **account credentials** too**AccountSettings.settings** in hello Microsoft.Azure.Batch.Samples.Common project.</span></span>
3. <span data-ttu-id="6dc98-206">**Build** (de ne futtassa) hello megoldás.</span><span class="sxs-lookup"><span data-stu-id="6dc98-206">**Build** (but do not run) hello solution.</span></span> <span data-ttu-id="6dc98-207">Ha a rendszer kéri, állítsa vissza a NuGet-csomagok.</span><span class="sxs-lookup"><span data-stu-id="6dc98-207">Restore any NuGet packages if prompted.</span></span>
4. <span data-ttu-id="6dc98-208">Használjon hello Azure portál tooupload egy [alkalmazáscsomag](batch-application-packages.md) a **PersistOutputsTask**.</span><span class="sxs-lookup"><span data-stu-id="6dc98-208">Use hello Azure portal tooupload an [application package](batch-application-packages.md) for **PersistOutputsTask**.</span></span> <span data-ttu-id="6dc98-209">Hello tartalmaznak `PersistOutputsTask.exe` és a függő szerelvényeket hello .zip-csomagja, hello alkalmazás azonosítója túl "PersistOutputsTask" és az alkalmazás hello Csomagverzió túl "1.0".</span><span class="sxs-lookup"><span data-stu-id="6dc98-209">Include hello `PersistOutputsTask.exe` and its dependent assemblies in hello .zip package, set hello application ID too"PersistOutputsTask", and hello application package version too"1.0".</span></span>
5. <span data-ttu-id="6dc98-210">**Start** (Futtatás) hello **PersistOutputs** projekt.</span><span class="sxs-lookup"><span data-stu-id="6dc98-210">**Start** (run) hello **PersistOutputs** project.</span></span>
6. <span data-ttu-id="6dc98-211">Amikor a kért toochoose hello adatmegőrzési technológia toouse futó hello minta be **1** toorun hello mintákkal hello fájl egyezmények könyvtár toopersist feladat kimenete.</span><span class="sxs-lookup"><span data-stu-id="6dc98-211">When prompted toochoose hello persistence technology toouse for running hello sample, enter **1** toorun hello sample using hello File Conventions library toopersist task output.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="6dc98-212">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6dc98-212">Next steps</span></span>

### <a name="get-hello-batch-file-conventions-library-for-net"></a><span data-ttu-id="6dc98-213">Hello kötegelt fájl egyezmények könyvtár beolvasása a .NET-hez</span><span class="sxs-lookup"><span data-stu-id="6dc98-213">Get hello Batch File Conventions library for .NET</span></span>

<span data-ttu-id="6dc98-214">hello kötegelt fájl egyezmények .NET-keretrendszerhez készült érhető el a [NuGet][nuget_package].</span><span class="sxs-lookup"><span data-stu-id="6dc98-214">hello Batch File Conventions library for .NET is available on [NuGet][nuget_package].</span></span> <span data-ttu-id="6dc98-215">hello könyvtár bővíti hello [CloudJob] [ net_cloudjob] és [CloudTask] [ net_cloudtask] osztályok új metódusával.</span><span class="sxs-lookup"><span data-stu-id="6dc98-215">hello library extends hello [CloudJob][net_cloudjob] and [CloudTask][net_cloudtask] classes with new methods.</span></span> <span data-ttu-id="6dc98-216">Lásd még: hello [dokumentáció](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.conventions.files) hello fájl egyezmények könyvtárhoz.</span><span class="sxs-lookup"><span data-stu-id="6dc98-216">Also see hello [reference documentation](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.conventions.files) for hello File Conventions library.</span></span>

<span data-ttu-id="6dc98-217">Hello [forráskód] [ github_file_conventions] hello fájl egyezmények könyvtár a Microsoft Azure SDK hello .NET tárház elérhető a Githubon.</span><span class="sxs-lookup"><span data-stu-id="6dc98-217">hello [source code][github_file_conventions] for hello File Conventions library is available on GitHub in hello Microsoft Azure SDK for .NET repository.</span></span> 

### <a name="explore-other-approaches-for-persisting-output-data"></a><span data-ttu-id="6dc98-218">Más megoldások tárolásakor kimeneti adatok felfedezése</span><span class="sxs-lookup"><span data-stu-id="6dc98-218">Explore other approaches for persisting output data</span></span>

- <span data-ttu-id="6dc98-219">Lásd: [Persist feladat- és kimeneti tooAzure tárolási](batch-task-output.md) áttekintését a feladat és a feladat adatait.</span><span class="sxs-lookup"><span data-stu-id="6dc98-219">See [Persist job and task output tooAzure Storage](batch-task-output.md) for an overview of persisting task and job data.</span></span>
- <span data-ttu-id="6dc98-220">Lásd: [megőrizni a feladat adatainak tooAzure tárolási hello Batch szolgáltatás API](batch-task-output-files.md) toolearn hogyan toouse hello Batch szolgáltatás API toopersist kimeneti adatokat.</span><span class="sxs-lookup"><span data-stu-id="6dc98-220">See [Persist task data tooAzure Storage with hello Batch service API](batch-task-output-files.md) toolearn how toouse hello Batch service API toopersist output data.</span></span>

[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[github_file_conventions]: https://github.com/Azure/azure-sdk-for-net/tree/AutoRest/src/Batch/FileConventions
[github_file_conventions_readme]: https://github.com/Azure/azure-sdk-for-net/blob/AutoRest/src/Batch/FileConventions/README.md
[github_persistoutputs]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/PersistOutputs
[github_samples]: https://github.com/Azure/azure-batch-samples
[net_batchclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_cloudstorageaccount]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.cloudstorageaccount.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_fileconventions_readme]: https://github.com/Azure/azure-sdk-for-net/blob/AutoRest/src/Batch/FileConventions/README.md
[net_joboutputkind]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputkind.aspx
[net_joboutputstorage]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputstorage.aspx
[net_joboutputstorage_saveasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputstorage.saveasync.aspx
[net_msdn]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[net_prepareoutputasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.cloudjobextensions.prepareoutputstorageasync.aspx
[net_saveasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.saveasync.aspx
[net_savetrackedasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.savetrackedasync.aspx
[net_taskoutputkind]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputkind.aspx
[net_taskoutputstorage]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.aspx
[nuget_manager]: https://docs.nuget.org/consume/installing-nuget
[nuget_package]: https://www.nuget.org/packages/Microsoft.Azure.Batch.Conventions.Files
[portal]: https://portal.azure.com
[storage_explorer]: http://storageexplorer.com/

[1]: ./media/batch-task-output/task-output-01.png "Mentett kimeneti fájlokat és mentett naplókat választók portálon"
[2]: ./media/batch-task-output/task-output-02.png "A feladat kimenete panel az Azure-portálon hello"
