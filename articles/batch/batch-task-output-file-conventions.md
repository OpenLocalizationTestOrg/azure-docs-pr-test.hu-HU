---
title: "Feladat- és kimeneti, a fájl egyezmények könyvtárhoz Azure Storage .NET - Azure Batch fennállnak |} Microsoft Docs"
description: "Útmutató Azure Batch fájl egyezmények .NET-keretrendszerhez készült Azure Storage feladat és a feladat kimenete kötegelt, és a megőrzött eredményének megtekintéséhez az Azure portálon."
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
ms.openlocfilehash: a9de327c20463469bc91d9720aa17333a36f919e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="persist-job-and-task-data-to-azure-storage-with-the-batch-file-conventions-library-for-net-to-persist"></a><span data-ttu-id="0a615-103">A kötegelt fájl egyezmények könyvtárhoz megőrizni a .NET-keretrendszerhez készült Azure Storage feladat- és adatok megőrzése</span><span class="sxs-lookup"><span data-stu-id="0a615-103">Persist job and task data to Azure Storage with the Batch File Conventions library for .NET to persist</span></span> 

[!INCLUDE [batch-task-output-include](../../includes/batch-task-output-include.md)]

<span data-ttu-id="0a615-104">Egy módja feladat adatok megőrzéséhez a [Azure Batch fájl egyezmények .NET-keretrendszerhez készült][nuget_package].</span><span class="sxs-lookup"><span data-stu-id="0a615-104">One way to persist task data is to use the [Azure Batch File Conventions library for .NET][nuget_package].</span></span> <span data-ttu-id="0a615-105">A fájl egyezmények könyvtár egyszerűbbé teszi a tevékenység kimeneti adatok Azure Storage tárolására és beolvasására azt.</span><span class="sxs-lookup"><span data-stu-id="0a615-105">The File Conventions library simplifies the process of storing task output data to Azure Storage and retrieving it.</span></span> <span data-ttu-id="0a615-106">Használhatja a fájl egyezmények könyvtárra, a feladat- és ügyfél-kód &mdash; tárolásakor fájlok feladat kódját, és a Ügyfélkód listában, és kérheti le azokat.</span><span class="sxs-lookup"><span data-stu-id="0a615-106">You can use the File Conventions library in both task and client code &mdash; in task code for persisting files, and in client code to list and retrieve them.</span></span> <span data-ttu-id="0a615-107">A feladat kódot is használatával a szalagtár fölérendelt feladatot, kimeneti többek között a egy [függőségek feladat](batch-task-dependencies.md) forgatókönyv.</span><span class="sxs-lookup"><span data-stu-id="0a615-107">Your task code can also use the library to retrieve the output of upstream tasks, such as in a [task dependencies](batch-task-dependencies.md) scenario.</span></span> 

<span data-ttu-id="0a615-108">A fájl egyezmények könyvtárhoz kimenete fájlok lekéréséhez, keresse meg a fájlok egy adott feladat vagy tevékenység ehhez listázza azokat azonosítója és a cél.</span><span class="sxs-lookup"><span data-stu-id="0a615-108">To retrieve output files with the File Conventions library, you can locate the files for a given job or task by listing them by ID and purpose.</span></span> <span data-ttu-id="0a615-109">Nem kell tudja, hogy a nevek vagy a fájlok helyét.</span><span class="sxs-lookup"><span data-stu-id="0a615-109">You don't need to know the names or locations of the files.</span></span> <span data-ttu-id="0a615-110">Például a fájl egyezmények könyvtár használata egy adott tevékenység köztes fájlok listáját, vagy egy adott feladat preview fájl.</span><span class="sxs-lookup"><span data-stu-id="0a615-110">For example, you can use the File Conventions library to list all intermediate files for a given task, or get a preview file for a given job.</span></span>

> [!TIP]
> <span data-ttu-id="0a615-111">2017-05-01 verziójától kezdve, a Batch szolgáltatás API támogatja tárolásakor kimeneti adatok Azure Storage és a virtuálisgép-konfiguráció létre készletek futó feladat manager feladatok.</span><span class="sxs-lookup"><span data-stu-id="0a615-111">Starting with version 2017-05-01, the Batch service API supports persisting output data to Azure Storage for tasks and job manager tasks that run on pools created with the virtual machine configuration.</span></span> <span data-ttu-id="0a615-112">A Batch szolgáltatás API megőrizni a kimenet a kód, amely feladatot hoz létre, és a fájl egyezmények könyvtár alternatívájaként szolgál egyszerű módszert kínál.</span><span class="sxs-lookup"><span data-stu-id="0a615-112">The Batch service API provides a simple way to persist output from within the code that creates a task and serves as an alternative to the File Conventions library.</span></span> <span data-ttu-id="0a615-113">A kötegelt ügyfélalkalmazások megőrizni a kimenet anélkül, hogy frissítse az alkalmazást, amely a feladat futásának módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="0a615-113">You can modify your Batch client applications to persist output without needing to update the application that your task is running.</span></span> <span data-ttu-id="0a615-114">További információkért lásd: [megőrző feladat adatok Azure Storage a kötegelt API szolgáltatás](batch-task-output-files.md).</span><span class="sxs-lookup"><span data-stu-id="0a615-114">For more information, see [Persist task data to Azure Storage with the Batch service API](batch-task-output-files.md).</span></span>
> 
> 

## <a name="when-do-i-use-the-file-conventions-library-to-persist-task-output"></a><span data-ttu-id="0a615-115">Ha használja a fájl egyezmények könyvtár megőrizni a feladat kimenete?</span><span class="sxs-lookup"><span data-stu-id="0a615-115">When do I use the File Conventions library to persist task output?</span></span>

<span data-ttu-id="0a615-116">Az Azure Batch megőrizni a feladat kimenete egynél több módot nyújt.</span><span class="sxs-lookup"><span data-stu-id="0a615-116">Azure Batch provides more than one way to persist task output.</span></span> <span data-ttu-id="0a615-117">A fájl egyezmények ezekkel az esetekkel a legmegfelelőbb:</span><span class="sxs-lookup"><span data-stu-id="0a615-117">The File Conventions is best suited to these scenarios:</span></span>

- <span data-ttu-id="0a615-118">A feladat futásának megőrizni a fájlok a fájl egyezmények szalagtárat használó alkalmazás számára a kód könnyen módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="0a615-118">You can easily modify the code for the application that your task is running to persist files using the File Conventions library.</span></span>
- <span data-ttu-id="0a615-119">A feladat futása közben szeretné adatfolyam adatok Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="0a615-119">You want to stream data to Azure Storage while the task is still running.</span></span>
- <span data-ttu-id="0a615-120">Szeretné megőrizni a készletek a felhőalapú szolgáltatás konfigurációja vagy a virtuálisgép-konfiguráció adatait.</span><span class="sxs-lookup"><span data-stu-id="0a615-120">You want to persist data from pools created with either the cloud service configuration or the virtual machine configuration.</span></span>
- <span data-ttu-id="0a615-121">Keresse meg és töltse le a tevékenység kimeneti fájlok azonosító vagy célú kell az ügyfélalkalmazás vagy egyéb feladatok, a feladat.</span><span class="sxs-lookup"><span data-stu-id="0a615-121">Your client application or other tasks in the job needs to locate and download task output files by ID or by purpose.</span></span> 
- <span data-ttu-id="0a615-122">A feladat kimenetének megtekintése az Azure portálon szeretné.</span><span class="sxs-lookup"><span data-stu-id="0a615-122">You want to view task output in the Azure portal.</span></span>

<span data-ttu-id="0a615-123">Ha adott esetben eltér a fent felsorolt, esetleg érdemes lehet egy másik módszert.</span><span class="sxs-lookup"><span data-stu-id="0a615-123">If your scenario differs from those listed above, you may need to consider a different approach.</span></span> <span data-ttu-id="0a615-124">Más beállításokat a tárolásakor feladat kimenetének további információkért lásd: [megőrizni a feladat- és kimeneti Azure Storage](batch-task-output.md).</span><span class="sxs-lookup"><span data-stu-id="0a615-124">For more information on other options for persisting task output, see [Persist job and task output to Azure Storage](batch-task-output.md).</span></span> 

## <a name="what-is-the-batch-file-conventions-standard"></a><span data-ttu-id="0a615-125">Mi az a kötegelt fájl egyezmények szabványos?</span><span class="sxs-lookup"><span data-stu-id="0a615-125">What is the Batch File Conventions standard?</span></span>

<span data-ttu-id="0a615-126">A [kötegelt fájl egyezmények standard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions) egy elnevezési sémát biztosít a cél-tárolók és a blob elérési utak, amelyhez a kimeneti fájlok készültek.</span><span class="sxs-lookup"><span data-stu-id="0a615-126">The [Batch File Conventions standard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions) provides a naming scheme for the destination containers and blob paths to which your output files are written.</span></span> <span data-ttu-id="0a615-127">Azure Storage, amely igazodik fájl egyezmények normál megőrzött fájlok automatikusan elérhető megtekinthető az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="0a615-127">Files persisted to Azure Storage that adhere to the File Conventions standard are automatically available for viewing in the Azure portal.</span></span> <span data-ttu-id="0a615-128">A portál tudomást az elnevezési konvenciót, és így megjeleníthetik a fájlokat, csatlakozhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="0a615-128">The portal is aware of the naming convention and so can display files that adhere to it.</span></span>

<span data-ttu-id="0a615-129">A fájl egyezmények .NET-keretrendszerhez készült automatikusan nevet, a tároló- és tevékenység kimeneti fájlokat a szabványos fájl konvenciók szerint.</span><span class="sxs-lookup"><span data-stu-id="0a615-129">The File Conventions library for .NET automatically names your storage containers and task output files according to the File Conventions standard.</span></span> <span data-ttu-id="0a615-130">A fájl egyezmények könyvtár is lekérdezése az Azure Storage megfelelően Feladatazonosító, Tevékenységazonosító vagy célja a kimenő fájlok metódusokat biztosít.</span><span class="sxs-lookup"><span data-stu-id="0a615-130">The File Conventions library also provides methods to query output files in Azure Storage according to job ID, task ID, or purpose.</span></span>   

<span data-ttu-id="0a615-131">Ha .NET eltérő nyelvű fejleszt, is létrehozható fájl egyezmények normál saját magának az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="0a615-131">If you are developing with a language other than .NET, you can implement the File Conventions standard yourself in your application.</span></span> <span data-ttu-id="0a615-132">További információkért lásd: [kapcsolatos a kötegelt fájl egyezmények standard](batch-task-output.md#about-the-batch-file-conventions-standard).</span><span class="sxs-lookup"><span data-stu-id="0a615-132">For more information, see [About the Batch File Conventions standard](batch-task-output.md#about-the-batch-file-conventions-standard).</span></span>

## <a name="link-an-azure-storage-account-to-your-batch-account"></a><span data-ttu-id="0a615-133">Egy Azure Storage-fiók összekapcsolása a Batch-fiók</span><span class="sxs-lookup"><span data-stu-id="0a615-133">Link an Azure Storage account to your Batch account</span></span>

<span data-ttu-id="0a615-134">Megőrizni a kimeneti adatok Azure Storage a fájl egyezmények kódtár használatával, először a Batch-fiókhoz kell kapcsolni egy Azure Storage-fiókot.</span><span class="sxs-lookup"><span data-stu-id="0a615-134">To persist output data to Azure Storage using the File Conventions library, you must first link an Azure Storage account to your Batch account.</span></span> <span data-ttu-id="0a615-135">Még nem tette meg, ha egy Storage-fiók összekötése a Batch-fiók használatával a [Azure-portálon](https://portal.azure.com):</span><span class="sxs-lookup"><span data-stu-id="0a615-135">If you haven't done so already, link a Storage account to your Batch account by using the [Azure portal](https://portal.azure.com):</span></span>

1. <span data-ttu-id="0a615-136">Az Azure portálon lépjen Batch-fiókjára.</span><span class="sxs-lookup"><span data-stu-id="0a615-136">Navigate to your Batch account in the Azure portal.</span></span> 
2. <span data-ttu-id="0a615-137">A **beállítások**, jelölje be **Tárfiók**.</span><span class="sxs-lookup"><span data-stu-id="0a615-137">Under **Settings**, select **Storage Account**.</span></span>
3. <span data-ttu-id="0a615-138">Ha még nem rendelkezik egy tárfiókot, a Batch-fiókhoz társított, kattintson a **(nincs) Storage-fiók**.</span><span class="sxs-lookup"><span data-stu-id="0a615-138">If you do not already have a Storage account associated with your Batch account, click **Storage Account (None)**.</span></span>
4. <span data-ttu-id="0a615-139">Válasszon egy tárfiókot a listából az előfizetéséhez.</span><span class="sxs-lookup"><span data-stu-id="0a615-139">Select a Storage account from the list for your subscription.</span></span> <span data-ttu-id="0a615-140">A legjobb teljesítmény érdekében használjon egy Azure Storage-fiókot, amely ugyanabban a régióban a Batch-fiók, amelyen fut a feladat.</span><span class="sxs-lookup"><span data-stu-id="0a615-140">For best performance, use an Azure Storage account that is in the same region as the Batch account where your tasks are running.</span></span>

## <a name="persist-output-data"></a><span data-ttu-id="0a615-141">Kimeneti adatok megőrzése</span><span class="sxs-lookup"><span data-stu-id="0a615-141">Persist output data</span></span>

<span data-ttu-id="0a615-142">Megőrizni a feladat- és kimeneti adatokat a fájl egyezmények könyvtárhoz, az Azure Storage tárolókat hozhat létre, majd kimenetét mentse a tárolóhoz.</span><span class="sxs-lookup"><span data-stu-id="0a615-142">To persist job and task output data with the File Conventions library, create a container in Azure Storage, then save the output to the container.</span></span> <span data-ttu-id="0a615-143">Használja a [Azure Storage ügyféloldali kódtára a .NET](https://www.nuget.org/packages/WindowsAzure.Storage) a feladat kódban a feladat kimenete feltölteni a tárolóba.</span><span class="sxs-lookup"><span data-stu-id="0a615-143">Use the [Azure Storage client library for .NET](https://www.nuget.org/packages/WindowsAzure.Storage) in your task code to upload the task output to the container.</span></span> 

<span data-ttu-id="0a615-144">A tárolók és blobok az Azure Storage használatáról további információk: [az Azure Blob storage .NET használatának első lépései](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span><span class="sxs-lookup"><span data-stu-id="0a615-144">For more information about working with containers and blobs in Azure Storage, see [Get started with Azure Blob storage using .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span></span>

> [!WARNING]
> <span data-ttu-id="0a615-145">Feladat- és mindegyikhez állandó, a fájl egyezmények könyvtárban tárolják ugyanabban a tárolóban.</span><span class="sxs-lookup"><span data-stu-id="0a615-145">All job and task outputs persisted with the File Conventions library are stored in the same container.</span></span> <span data-ttu-id="0a615-146">Ha nagyszámú feladatok megőrizni a fájlok egy időben, próbálja [szabályozás korlátok tárolási](../storage/common/storage-performance-checklist.md#blobs) is kényszeríthető.</span><span class="sxs-lookup"><span data-stu-id="0a615-146">If a large number of tasks try to persist files at the same time, [storage throttling limits](../storage/common/storage-performance-checklist.md#blobs) may be enforced.</span></span>
> 
> 

### <a name="create-storage-container"></a><span data-ttu-id="0a615-147">A tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="0a615-147">Create storage container</span></span>

<span data-ttu-id="0a615-148">A feladat kimenetének Azure Storage megőrizni, először létre kell hoznia egy tárolót meghívásával [CloudJob][net_cloudjob].[ PrepareOutputStorageAsync][net_prepareoutputasync].</span><span class="sxs-lookup"><span data-stu-id="0a615-148">To persist task output to Azure Storage, first create a container by calling [CloudJob][net_cloudjob].[PrepareOutputStorageAsync][net_prepareoutputasync].</span></span> <span data-ttu-id="0a615-149">A bővítmény metódus egy [CloudStorageAccount] [ net_cloudstorageaccount] objektum paraméterként.</span><span class="sxs-lookup"><span data-stu-id="0a615-149">This extension method takes a [CloudStorageAccount][net_cloudstorageaccount] object as a parameter.</span></span> <span data-ttu-id="0a615-150">Hoz, hogy a tartalma az Azure portál által felderíthető fájl egyezmények szabvány szerinti nevű tárolót, és az a cikk későbbi részében ismertetett lekérdezési módszerek.</span><span class="sxs-lookup"><span data-stu-id="0a615-150">It creates a container named according to the File Conventions standard,so that its contents are discoverable by the Azure portal and the retrieval methods discussed later in the article.</span></span>

<span data-ttu-id="0a615-151">Általában helyezi-e a kód egy tároló létrehozásához az ügyfélalkalmazás az &mdash; az a készletek, a feladatok és a feladatok létrehozó alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="0a615-151">You typically place the code to create a container in your client application &mdash; the application that creates your pools, jobs, and tasks.</span></span>

```csharp
CloudJob job = batchClient.JobOperations.CreateJob(
    "myJob",
    new PoolInformation { PoolId = "myPool" });

// Create reference to the linked Azure Storage account
CloudStorageAccount linkedStorageAccount =
    new CloudStorageAccount(myCredentials, true);

// Create the blob storage container for the outputs
await job.PrepareOutputStorageAsync(linkedStorageAccount);
```

### <a name="store-task-outputs"></a><span data-ttu-id="0a615-152">Tárolási feladatok kimenete</span><span class="sxs-lookup"><span data-stu-id="0a615-152">Store task outputs</span></span>

<span data-ttu-id="0a615-153">Most, hogy az Azure Storage tárolója előkészítése után, feladatokat is mentése kimeneti a tároló segítségével a [TaskOutputStorage] [ net_taskoutputstorage] osztály a fájl egyezmények könyvtárban található.</span><span class="sxs-lookup"><span data-stu-id="0a615-153">Now that you've prepared a container in Azure Storage, tasks can save output to the container by using the [TaskOutputStorage][net_taskoutputstorage] class found in the File Conventions library.</span></span>

<span data-ttu-id="0a615-154">A feladat kódban, először létre kell hoznia egy [TaskOutputStorage] [ net_taskoutputstorage] objektumot, majd a tevékenységeket a feladat befejezése után hívja a [TaskOutputStorage][net_taskoutputstorage].[ SaveAsync] [ net_saveasync] mentése az Azure Storage metódust.</span><span class="sxs-lookup"><span data-stu-id="0a615-154">In your task code, first create a [TaskOutputStorage][net_taskoutputstorage] object, then when the task has completed its work, call the [TaskOutputStorage][net_taskoutputstorage].[SaveAsync][net_saveasync] method to save its output to Azure Storage.</span></span>

```csharp
CloudStorageAccount linkedStorageAccount = new CloudStorageAccount(myCredentials);
string jobId = Environment.GetEnvironmentVariable("AZ_BATCH_JOB_ID");
string taskId = Environment.GetEnvironmentVariable("AZ_BATCH_TASK_ID");

TaskOutputStorage taskOutputStorage = new TaskOutputStorage(
    linkedStorageAccount, jobId, taskId);

/* Code to process data and produce output file(s) */

await taskOutputStorage.SaveAsync(TaskOutputKind.TaskOutput, "frame_full_res.jpg");
await taskOutputStorage.SaveAsync(TaskOutputKind.TaskPreview, "frame_low_res.jpg");
```

<span data-ttu-id="0a615-155">A `kind` paramétere a [TaskOutputStorage](https://msdn.microsoft.com/library/microsoft.azure.batch.conventions.files.taskoutputstorage.aspx).[ SaveAsync](https://msdn.microsoft.com/library/microsoft.azure.batch.conventions.files.taskoutputstorage.saveasync.aspx) metódus kategorizálja a megőrzött fájlok.</span><span class="sxs-lookup"><span data-stu-id="0a615-155">The `kind` parameter of the [TaskOutputStorage](https://msdn.microsoft.com/library/microsoft.azure.batch.conventions.files.taskoutputstorage.aspx).[SaveAsync](https://msdn.microsoft.com/library/microsoft.azure.batch.conventions.files.taskoutputstorage.saveasync.aspx) method categorizes the persisted files.</span></span> <span data-ttu-id="0a615-156">Nincsenek négy előre definiált [TaskOutputKind] [ net_taskoutputkind] típusok: `TaskOutput`, `TaskPreview`, `TaskLog`, és `TaskIntermediate.` kimeneti egyéni kategóriáinak is meghatározhat.</span><span class="sxs-lookup"><span data-stu-id="0a615-156">There are four predefined [TaskOutputKind][net_taskoutputkind] types: `TaskOutput`, `TaskPreview`, `TaskLog`, and `TaskIntermediate.` You can also define custom categories of output.</span></span>

<span data-ttu-id="0a615-157">Kimeneti típusaival engedélyezi, hogy adja meg, milyen típusú kimenetek előfordulhat, hogy később kötegelt a megőrzött egy adott feladat kimenetének a listát.</span><span class="sxs-lookup"><span data-stu-id="0a615-157">These output types allow you to specify which type of outputs to list when you later query Batch for the persisted outputs of a given task.</span></span> <span data-ttu-id="0a615-158">Ez azt jelenti Ha egy feladat kimenetének felsorolja, végezhet listáról a kimeneti típusú.</span><span class="sxs-lookup"><span data-stu-id="0a615-158">In other words, when you list the outputs for a task, you can filter the list on one of the output types.</span></span> <span data-ttu-id="0a615-159">Például ", adja meg a *előzetes* tevékenység kimeneti *109*."</span><span class="sxs-lookup"><span data-stu-id="0a615-159">For example, "Give me the *preview* output for task *109*."</span></span> <span data-ttu-id="0a615-160">Több listázása és kimenetének beolvasása megjelenik [lekérnie a kimenetet](#retrieve-output) a cikk későbbi.</span><span class="sxs-lookup"><span data-stu-id="0a615-160">More on listing and retrieving outputs appears in [Retrieve output](#retrieve-output) later in the article.</span></span>

> [!TIP]
> <span data-ttu-id="0a615-161">A kimeneti típust is meghatározza, hogy az Azure portálon egy adott fájl helyére: *TaskOutput*-kategorizált fájlok alatt szerepelhet **tevékenység kimeneti fájlok**, és *TaskLog* fájlok jelennek meg a **naplók feladat**.</span><span class="sxs-lookup"><span data-stu-id="0a615-161">The output kind also determines where in the Azure portal a particular file appears: *TaskOutput*-categorized files appear under **Task output files**, and *TaskLog* files appear under **Task logs**.</span></span>
> 
> 

### <a name="store-job-outputs"></a><span data-ttu-id="0a615-162">Feladat kimenetének tárolásához</span><span class="sxs-lookup"><span data-stu-id="0a615-162">Store job outputs</span></span>

<span data-ttu-id="0a615-163">Tárolás, a feladat kimenetének létrehozása mellett egy teljes feladattal társított kimenetek is tárolhatja.</span><span class="sxs-lookup"><span data-stu-id="0a615-163">In addition to storing task outputs, you can store the outputs associated with an entire job.</span></span> <span data-ttu-id="0a615-164">Például az egyesítés a feladatban movie megjelenítési feladat, sikerült megőrizni a teljes megjelenített movie a feladat kimenete.</span><span class="sxs-lookup"><span data-stu-id="0a615-164">For example, in the merge task of a movie rendering job, you could persist the fully rendered movie as a job output.</span></span> <span data-ttu-id="0a615-165">Ha a feladat befejeződött, az ügyfélalkalmazás listában, és a feladat kimenetének beolvasása, és nem kell az egyes feladatok lekérdezni.</span><span class="sxs-lookup"><span data-stu-id="0a615-165">When your job is completed, your client application can list and retrieve the outputs for the job, and does not need to query the individual tasks.</span></span>

<span data-ttu-id="0a615-166">Tárolja a feladat kimenetére meghívásával a [JobOutputStorage][net_joboutputstorage].[ SaveAsync] [ net_joboutputstorage_saveasync] metódust, és adja meg a [JobOutputKind] [ net_joboutputkind] és fájlnév:</span><span class="sxs-lookup"><span data-stu-id="0a615-166">Store job output by calling the [JobOutputStorage][net_joboutputstorage].[SaveAsync][net_joboutputstorage_saveasync] method, and specify the [JobOutputKind][net_joboutputkind] and filename:</span></span>

```csharp
CloudJob job = new JobOutputStorage(acct, jobId);
JobOutputStorage jobOutputStorage = job.OutputStorage(linkedStorageAccount);

await jobOutputStorage.SaveAsync(JobOutputKind.JobOutput, "mymovie.mp4");
await jobOutputStorage.SaveAsync(JobOutputKind.JobPreview, "mymovie_preview.mp4");
```

<span data-ttu-id="0a615-167">Például a **TaskOutputKind** típus feladatot kimenetek, használja a [JobOutputKind] [ net_joboutputkind] kategorizálásának egy feladat típusa a megőrzött fájlok.</span><span class="sxs-lookup"><span data-stu-id="0a615-167">As with the **TaskOutputKind** type for task outputs, you use the [JobOutputKind][net_joboutputkind] type to categorize a job's persisted files.</span></span> <span data-ttu-id="0a615-168">Ez a paraméter lehetővé teszi később (lista) egy adott típusú kimeneti.</span><span class="sxs-lookup"><span data-stu-id="0a615-168">This parameter allows you to later query for (list) a specific type of output.</span></span> <span data-ttu-id="0a615-169">A **JobOutputKind** típusú kimeneti és a kép tartalmazza, és támogatja az egyéni kategóriák.</span><span class="sxs-lookup"><span data-stu-id="0a615-169">The **JobOutputKind** type includes both output and preview categories, and supports creating custom categories.</span></span>

### <a name="store-task-logs"></a><span data-ttu-id="0a615-170">A feladat naplók tárolására</span><span class="sxs-lookup"><span data-stu-id="0a615-170">Store task logs</span></span>

<span data-ttu-id="0a615-171">Mellett megőrzése a tartós tárolási egy fájlt egy feladat vagy a feladat befejezését, szükség lehet megőrizni a feladat végrehajtása közben fájlok &mdash; naplófájlok vagy `stdout.txt` és `stderr.txt`, például.</span><span class="sxs-lookup"><span data-stu-id="0a615-171">In addition to persisting a file to durable storage when a task or job completes, you may need to persist files that are updated during the execution of a task &mdash; log files or `stdout.txt` and `stderr.txt`, for example.</span></span> <span data-ttu-id="0a615-172">Erre a célra az Azure Batch fájl egyezmények kódtár biztosít a [TaskOutputStorage][net_taskoutputstorage].[ SaveTrackedAsync] [ net_savetrackedasync] metódust.</span><span class="sxs-lookup"><span data-stu-id="0a615-172">For this purpose, the Azure Batch File Conventions library provides the [TaskOutputStorage][net_taskoutputstorage].[SaveTrackedAsync][net_savetrackedasync] method.</span></span> <span data-ttu-id="0a615-173">A [SaveTrackedAsync][net_savetrackedasync], nyomon követheti a frissítések a csomópont (a megadott időközönként) fájlba, és ezek a frissítések Azure Storage megőrzéséhez.</span><span class="sxs-lookup"><span data-stu-id="0a615-173">With [SaveTrackedAsync][net_savetrackedasync], you can track updates to a file on the node (at an interval that you specify) and persist those updates to Azure Storage.</span></span>

<span data-ttu-id="0a615-174">A következő kódrészletet használjuk [SaveTrackedAsync] [ net_savetrackedasync] frissítése `stdout.txt` az Azure Storage 15 másodpercenként a feladat végrehajtása során:</span><span class="sxs-lookup"><span data-stu-id="0a615-174">In the following code snippet, we use [SaveTrackedAsync][net_savetrackedasync] to update `stdout.txt` in Azure Storage every 15 seconds during the execution of the task:</span></span>

```csharp
TimeSpan stdoutFlushDelay = TimeSpan.FromSeconds(3);
string logFilePath = Path.Combine(
    Environment.GetEnvironmentVariable("AZ_BATCH_TASK_DIR"), "stdout.txt");

// The primary task logic is wrapped in a using statement that sends updates to
// the stdout.txt blob in Storage every 15 seconds while the task code runs.
using (ITrackedSaveOperation stdout =
        await taskStorage.SaveTrackedAsync(
        TaskOutputKind.TaskLog,
        logFilePath,
        "stdout.txt",
        TimeSpan.FromSeconds(15)))
{
    /* Code to process data and produce output file(s) */

    // We are tracking the disk file to save our standard output, but the
    // node agent may take up to 3 seconds to flush the stdout stream to
    // disk. So give the file a moment to catch up.
     await Task.Delay(stdoutFlushDelay);
}
```

<span data-ttu-id="0a615-175">A megjegyzésként szakasz `Code to process data and produce output file(s)` helyőrzőjeként szolgál a kódot, amely a feladat általában hajtaná végre.</span><span class="sxs-lookup"><span data-stu-id="0a615-175">The commented section `Code to process data and produce output file(s)` is a placeholder for the code that your task would normally perform.</span></span> <span data-ttu-id="0a615-176">Például lehetséges, hogy kódot, amely letölti az adatokat az Azure Storage és átalakítás vagy számítási végez azt.</span><span class="sxs-lookup"><span data-stu-id="0a615-176">For example, you might have code that downloads data from Azure Storage and performs transformation or calculation on it.</span></span> <span data-ttu-id="0a615-177">A fontos része a részlet van bemutatásához, hogyan akkor futtathatja a kódok egy `using` blokk rendszeresen frissíteni a fájl [SaveTrackedAsync][net_savetrackedasync].</span><span class="sxs-lookup"><span data-stu-id="0a615-177">The important part of this snippet is demonstrating how you can wrap such code in a `using` block to periodically update a file with [SaveTrackedAsync][net_savetrackedasync].</span></span>

<span data-ttu-id="0a615-178">A csomópont-ügynök egy olyan program, a készlet minden egyes csomópontján fut, és a parancs-és-ellenőrzés felületet, a csomópont és a Batch szolgáltatás között.</span><span class="sxs-lookup"><span data-stu-id="0a615-178">The node agent is a program that runs on each node in the pool and provides the command-and-control interface between the node and the Batch service.</span></span> <span data-ttu-id="0a615-179">A `Task.Delay` tekintendő, amely szükséges a végén `using` blokk ellenőrizze, hogy a csomópont-ügynök van-e az idő a standard out tartalmát kiüríteni a stdout.txt fájlba a csomóponton.</span><span class="sxs-lookup"><span data-stu-id="0a615-179">The `Task.Delay` call is required at the end of this `using` block to ensure that the node agent has time to flush the contents of standard out to the stdout.txt file on the node.</span></span> <span data-ttu-id="0a615-180">Ez a késleltetés nélkül is lehet, amelyeken a kimenet utolsó néhány másodpercig.</span><span class="sxs-lookup"><span data-stu-id="0a615-180">Without this delay, it is possible to miss the last few seconds of output.</span></span> <span data-ttu-id="0a615-181">Ez a késés nem lehet szükség az összes fájl.</span><span class="sxs-lookup"><span data-stu-id="0a615-181">This delay may not be required for all files.</span></span>

> [!NOTE]
> <span data-ttu-id="0a615-182">Ha engedélyezi a nyomkövetési fájl **SaveTrackedAsync**, csak *hozzáfűzi* a nyomon követett fájl megmaradnak az Azure-tárhelyre.</span><span class="sxs-lookup"><span data-stu-id="0a615-182">When you enable file tracking with **SaveTrackedAsync**, only *appends* to the tracked file are persisted to Azure Storage.</span></span> <span data-ttu-id="0a615-183">Ezt a módszert csak a nyomkövetési naplófájlok nem elforgatása vagy egyéb fájlok, a műveletek hozzáfűzésére a fájl végére írni.</span><span class="sxs-lookup"><span data-stu-id="0a615-183">Use this method only for tracking non-rotating log files or other files that are written to with append operations to the end of the file.</span></span>
> 
> 

## <a name="retrieve-output-data"></a><span data-ttu-id="0a615-184">Kimeneti adatainak beolvasása</span><span class="sxs-lookup"><span data-stu-id="0a615-184">Retrieve output data</span></span>

<span data-ttu-id="0a615-185">Beolvasni a megőrzött kimeneti az Azure Batch fájl egyezmények kódtár használatával, ha ezt teszi feladat - és a feladat-központú módon.</span><span class="sxs-lookup"><span data-stu-id="0a615-185">When you retrieve your persisted output using the Azure Batch File Conventions library, you do so in a task- and job-centric manner.</span></span> <span data-ttu-id="0a615-186">A kimeneti kérhet a megadott feladat vagy a feladatot anélkül, hogy az elérési Azure Storage, vagy még a fájl nevét.</span><span class="sxs-lookup"><span data-stu-id="0a615-186">You can request the output for given task or job without needing to know its path in Azure Storage, or even its file name.</span></span> <span data-ttu-id="0a615-187">Ehelyett kérheti a kimeneti fájlok feladat, vagy sikertelen feladat-azonosítót.</span><span class="sxs-lookup"><span data-stu-id="0a615-187">Instead, you can request output files by task or job ID.</span></span>

<span data-ttu-id="0a615-188">A következő kódrészletet telepítéseket egy feladat tevékenységeit, kiírja a kimeneti fájlok a következő feladathoz kapcsolatos információkat és tölti le a fájlok a tároló.</span><span class="sxs-lookup"><span data-stu-id="0a615-188">The following code snippet iterates through a job's tasks, prints some information about the output files for the task, and then downloads its files from Storage.</span></span>

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

## <a name="view-output-files-in-the-azure-portal"></a><span data-ttu-id="0a615-189">Az Azure portálon kimeneti fájlok megtekintése</span><span class="sxs-lookup"><span data-stu-id="0a615-189">View output files in the Azure portal</span></span>

<span data-ttu-id="0a615-190">Az Azure-portálon jeleníti meg a tevékenység kimeneti fájlok és a naplófájlokat, amelyek megmaradnak a kapcsolt Azure Storage használatával a [kötegelt fájl egyezmények standard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions).</span><span class="sxs-lookup"><span data-stu-id="0a615-190">The Azure portal displays task output files and logs that are persisted to a linked Azure Storage account using the [Batch File Conventions standard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions).</span></span> <span data-ttu-id="0a615-191">Megvalósíthat szabályoknak saját kezűleg a a egy nyelvet a választott, vagy a fájl egyezmények könyvtár a .NET-alkalmazásokban használható.</span><span class="sxs-lookup"><span data-stu-id="0a615-191">You can implement these conventions yourself in the a language of your choice, or you can use the File Conventions library in your .NET applications.</span></span>

<span data-ttu-id="0a615-192">Ahhoz, hogy a kimeneti fájlok, a portál megjelenését, az alábbi követelményeknek kell megfelelnie:</span><span class="sxs-lookup"><span data-stu-id="0a615-192">To enable the display of your output files in the portal, you must satisfy the following requirements:</span></span>

1. <span data-ttu-id="0a615-193">[Egy Azure Storage-fiók csatolása](#requirement-linked-storage-account) a Batch-fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="0a615-193">[Link an Azure Storage account](#requirement-linked-storage-account) to your Batch account.</span></span>
2. <span data-ttu-id="0a615-194">Előre definiált tároló- és a fájlok elnevezési szabályai csatlakozhat, ha kimenetek megőrzése.</span><span class="sxs-lookup"><span data-stu-id="0a615-194">Adhere to the predefined naming conventions for Storage containers and files when persisting outputs.</span></span> <span data-ttu-id="0a615-195">Ezek egyezmények meghatározása a fájl egyezmények könyvtárban található [információs][github_file_conventions_readme].</span><span class="sxs-lookup"><span data-stu-id="0a615-195">You can find the definition of these conventions in the File Conventions library [README][github_file_conventions_readme].</span></span> <span data-ttu-id="0a615-196">Ha használja a [Azure Batch fájl egyezmények] [ nuget_package] megőrizni a kimeneti könyvtár fájl egyezmények szabvány szerinti őrződnek meg a fájlok.</span><span class="sxs-lookup"><span data-stu-id="0a615-196">If you use the [Azure Batch File Conventions][nuget_package] library to persist your output, your files are persisted according to the File Conventions standard.</span></span>

<span data-ttu-id="0a615-197">Az Azure portálon tevékenység kimeneti fájlok és a naplók megtekintéséhez nyissa meg a tevékenység, amelynek kimenete érdekli, majd kattintson **mentett kimeneti fájlok** vagy **mentett naplókat**.</span><span class="sxs-lookup"><span data-stu-id="0a615-197">To view task output files and logs in the Azure portal, navigate to the task whose output you are interested in, then click either **Saved output files** or **Saved logs**.</span></span> <span data-ttu-id="0a615-198">A képen látható a **mentett kimeneti fájlok** "007" azonosítójú feladat számára:</span><span class="sxs-lookup"><span data-stu-id="0a615-198">This image shows the **Saved output files** for the task with ID "007":</span></span>

<span data-ttu-id="0a615-199">![A feladat kimenetének panel az Azure-portálon][2]</span><span class="sxs-lookup"><span data-stu-id="0a615-199">![Task outputs blade in the Azure portal][2]</span></span>

## <a name="code-sample"></a><span data-ttu-id="0a615-200">Kódminta</span><span class="sxs-lookup"><span data-stu-id="0a615-200">Code sample</span></span>

<span data-ttu-id="0a615-201">A [PersistOutputs] [ github_persistoutputs] mintaprojektet egyike a [Azure Batch-Kódminták] [ github_samples] a Githubon.</span><span class="sxs-lookup"><span data-stu-id="0a615-201">The [PersistOutputs][github_persistoutputs] sample project is one of the [Azure Batch code samples][github_samples] on GitHub.</span></span> <span data-ttu-id="0a615-202">A Visual Studio megoldás bemutatja, hogyan használható az Azure Batch fájl egyezmények könyvtár továbbra is fennáll feladatkimenet tartós tárhelyre.</span><span class="sxs-lookup"><span data-stu-id="0a615-202">This Visual Studio solution demonstrates how to use the Azure Batch File Conventions library to persist task output to durable storage.</span></span> <span data-ttu-id="0a615-203">A minta futtatásához kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="0a615-203">To run the sample, follow these steps:</span></span>

1. <span data-ttu-id="0a615-204">Nyissa meg a projektet a **Visual Studio 2015-ös vagy újabb**.</span><span class="sxs-lookup"><span data-stu-id="0a615-204">Open the project in **Visual Studio 2015 or newer**.</span></span>
2. <span data-ttu-id="0a615-205">A kötegelt és a tárolási **fiók hitelesítő adatait** való **AccountSettings.settings** a Microsoft.Azure.Batch.Samples.Common projektben.</span><span class="sxs-lookup"><span data-stu-id="0a615-205">Add your Batch and Storage **account credentials** to **AccountSettings.settings** in the Microsoft.Azure.Batch.Samples.Common project.</span></span>
3. <span data-ttu-id="0a615-206">**Build** (de ne futtassa) a megoldás.</span><span class="sxs-lookup"><span data-stu-id="0a615-206">**Build** (but do not run) the solution.</span></span> <span data-ttu-id="0a615-207">Ha a rendszer kéri, állítsa vissza a NuGet-csomagok.</span><span class="sxs-lookup"><span data-stu-id="0a615-207">Restore any NuGet packages if prompted.</span></span>
4. <span data-ttu-id="0a615-208">Töltse fel az Azure-portál használatával egy [alkalmazáscsomag](batch-application-packages.md) a **PersistOutputsTask**.</span><span class="sxs-lookup"><span data-stu-id="0a615-208">Use the Azure portal to upload an [application package](batch-application-packages.md) for **PersistOutputsTask**.</span></span> <span data-ttu-id="0a615-209">Tartalmazza a `PersistOutputsTask.exe` és a függő szerelvényeket a .zip-csomagja "PersistOutputsTask" az alkalmazás-Azonosítót, és az alkalmazáscsomag verzióját "1.0".</span><span class="sxs-lookup"><span data-stu-id="0a615-209">Include the `PersistOutputsTask.exe` and its dependent assemblies in the .zip package, set the application ID to "PersistOutputsTask", and the application package version to "1.0".</span></span>
5. <span data-ttu-id="0a615-210">**Start** (Futtatás) a **PersistOutputs** projekt.</span><span class="sxs-lookup"><span data-stu-id="0a615-210">**Start** (run) the **PersistOutputs** project.</span></span>
6. <span data-ttu-id="0a615-211">Amikor a rendszer kéri a minta futtatásához használni, adja meg a adatmegőrzési technológiát válasszon **1** megőrizni a feladat kimenete a fájl egyezmények könyvtár használata a minta futtatásához.</span><span class="sxs-lookup"><span data-stu-id="0a615-211">When prompted to choose the persistence technology to use for running the sample, enter **1** to run the sample using the File Conventions library to persist task output.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="0a615-212">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0a615-212">Next steps</span></span>

### <a name="get-the-batch-file-conventions-library-for-net"></a><span data-ttu-id="0a615-213">A kötegelt fájl egyezmények könyvtár beolvasása a .NET-hez</span><span class="sxs-lookup"><span data-stu-id="0a615-213">Get the Batch File Conventions library for .NET</span></span>

<span data-ttu-id="0a615-214">A kötegelt fájl egyezmények .NET-keretrendszerhez készült érhető el a [NuGet][nuget_package].</span><span class="sxs-lookup"><span data-stu-id="0a615-214">The Batch File Conventions library for .NET is available on [NuGet][nuget_package].</span></span> <span data-ttu-id="0a615-215">A könyvtár bővíti a [CloudJob] [ net_cloudjob] és [CloudTask] [ net_cloudtask] osztályok új metódusával.</span><span class="sxs-lookup"><span data-stu-id="0a615-215">The library extends the [CloudJob][net_cloudjob] and [CloudTask][net_cloudtask] classes with new methods.</span></span> <span data-ttu-id="0a615-216">Lásd még: a [dokumentáció](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.conventions.files) a fájl egyezmények könyvtára.</span><span class="sxs-lookup"><span data-stu-id="0a615-216">Also see the [reference documentation](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.conventions.files) for the File Conventions library.</span></span>

<span data-ttu-id="0a615-217">A [forráskód] [ github_file_conventions] a fájl egyezmények könyvtár érhető el a Microsoft Azure SDK for .NET tárház a githubon.</span><span class="sxs-lookup"><span data-stu-id="0a615-217">The [source code][github_file_conventions] for the File Conventions library is available on GitHub in the Microsoft Azure SDK for .NET repository.</span></span> 

### <a name="explore-other-approaches-for-persisting-output-data"></a><span data-ttu-id="0a615-218">Más megoldások tárolásakor kimeneti adatok felfedezése</span><span class="sxs-lookup"><span data-stu-id="0a615-218">Explore other approaches for persisting output data</span></span>

- <span data-ttu-id="0a615-219">Lásd: [megőrizni a feladat- és kimeneti Azure Storage](batch-task-output.md) áttekintését a feladat és a feladat adatait.</span><span class="sxs-lookup"><span data-stu-id="0a615-219">See [Persist job and task output to Azure Storage](batch-task-output.md) for an overview of persisting task and job data.</span></span>
- <span data-ttu-id="0a615-220">Lásd: [megőrző feladat adatok Azure Storage a kötegelt API szolgáltatás](batch-task-output-files.md) a kimeneti adatok megőrzéséhez a Batch szolgáltatás API használata.</span><span class="sxs-lookup"><span data-stu-id="0a615-220">See [Persist task data to Azure Storage with the Batch service API](batch-task-output-files.md) to learn how to use the Batch service API to persist output data.</span></span>

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
[2]: ./media/batch-task-output/task-output-02.png "A feladat kimenetének panel az Azure-portálon"
