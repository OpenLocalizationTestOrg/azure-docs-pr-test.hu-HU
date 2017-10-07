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
# <a name="persist-job-and-task-data-tooazure-storage-with-hello-batch-file-conventions-library-for-net-toopersist"></a>Feladat megmaradnak, és az adatok tooAzure tárolási hello kötegelt fájl egyezmények kódtára a .NET toopersist a feladat 

[!INCLUDE [batch-task-output-include](../../includes/batch-task-output-include.md)]

Egyirányú toopersist feladat adata toouse hello [Azure Batch fájl egyezmények .NET-keretrendszerhez készült][nuget_package]. hello fájl egyezmények könyvtár egyszerűbben hello tárolása tevékenység kimeneti adatok tooAzure tárolási és azt lekérése során. Használhatja a feladat- és ügyfél-kód hello fájl egyezmények szalagtár &mdash; feladatkód tárolásakor fájlok, és az ügyfél code toolist, és kérheti le azokat. A feladat kód segítségével is hello könyvtár tooretrieve hello kimeneti fölérendelt feladatokat, többek között a [függőségek feladat](batch-task-dependencies.md) forgatókönyv. 

tooretrieve kimeneti fájlok hello fájl egyezmények könyvtárhoz, hello fájlok egy adott feladat vagy tevékenység azonosítója és a cél szerint felsorolva keresheti. Nincs szükség tooknow hello nevek vagy hello fájlok helyét. Például egy adott feladat hello fájl egyezmények könyvtár toolist összes köztes fájlt használja, vagy egy adott feladat preview fájl.

> [!TIP]
> Verziójával 2017-05-01-től kezdődően hello Batch szolgáltatás API támogatja tárolásakor kimeneti adatok tooAzure tároló és a virtuálisgép-konfiguráció hello létre készletek futó feladat manager feladatok. hello Batch szolgáltatás API biztosít egy egyszerű módon toopersist hello kód, amely létrehoz egy feladatot, és egy alternatív toohello fájl egyezmények könyvtár funkcionál kimenetét. Módosíthatja a kötegelt ügyfél alkalmazások toopersist kimeneti anélkül, hogy a feladat futásának tooupdate hello alkalmazást. További információkért lásd: [megőrizni a feladat adatainak tooAzure tárolási hello Batch szolgáltatás API](batch-task-output-files.md).
> 
> 

## <a name="when-do-i-use-hello-file-conventions-library-toopersist-task-output"></a>Ha használja a hello fájl egyezmények könyvtár toopersist feladat kimenetének?

Az Azure Batch szolgáltatás több mint egyirányú toopersist feladat kimenete. hello fájl egyezmények a legjobb olyan környezethez a legalkalmasabb toothese forgatókönyvek:

- Hello kódot, hogy a feladat futásának toopersist fájlokat a hello fájl egyezmények könyvtár hello alkalmazás könnyen módosíthatja.
- Érdemes toostream adatok tooAzure tárolási hello feladat futása közben.
- Azt szeretné, hogy a készletek hello felhőalapú szolgáltatás konfigurációja vagy a virtuálisgép-konfiguráció hello toopersist adatait.
- Az ügyfélalkalmazás vagy más feladatok hello igények toolocate feladat, és töltse le a tevékenység kimeneti fájlok azonosító vagy célja. 
- Azt szeretné, hogy a feladat kimenetének tooview hello Azure-portálon.

Ha a forgatókönyv eltér a fent felsorolt, szükség lehet tooconsider másik módszert. Más beállításokat a tárolásakor feladat kimenetének további információkért lásd: [Persist feladat- és kimeneti tooAzure tárolási](batch-task-output.md). 

## <a name="what-is-hello-batch-file-conventions-standard"></a>Mi az a hello kötegelt fájl egyezmények szabványos?

Hello [kötegelt fájl egyezmények standard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions) egy elnevezési sémát biztosít hello cél tárolók és a blob elérési utak toowhich a kimeneti fájlok kerülnek. Fájlok megőrzött tooAzure tároló, amely igazodik toohello fájl egyezmények szabványos automatikusan elérhetők megtekintését a hello Azure-portálon. hello portal ismeri a hello elnevezési konvenciót, és így megjeleníthetik a fájlokat, tooit igazodik.

hello fájl egyezmények .NET-keretrendszerhez készült automatikusan nevet, a tároló- és tevékenység kimeneti fájlok toohello szabványos fájl konvenciók szerint. hello fájl egyezmények kódtár is biztosít a módszerek tooquery kimeneti fájlok az Azure Storage toojob azonosító, a tevékenység azonosítója vagy a cél alapján történik.   

Ha .NET eltérő nyelvű fejleszt, valósíthatja meg hello fájl egyezmények standard saját magának az alkalmazásban. További információkért lásd: [kapcsolatos hello kötegelt fájl egyezmények standard](batch-task-output.md#about-the-batch-file-conventions-standard).

## <a name="link-an-azure-storage-account-tooyour-batch-account"></a>Egy Azure Storage-fiók tooyour Batch-fiók csatolása

toopersist a kimeneti adatok tooAzure hello fájl egyezmények szalagtárat használó tárolási, kell először kapcsolni az Azure Storage-fiók tooyour Batch-fiókhoz. Ha még nem tette meg, a tárolási fiók tooyour Batch-fiókhoz csatolja hello segítségével [Azure-portálon](https://portal.azure.com):

1. Keresse meg a Batch-fiók tooyour hello Azure-portálon. 
2. A **beállítások**, jelölje be **Tárfiók**.
3. Ha még nem rendelkezik egy tárfiókot, a Batch-fiókhoz társított, kattintson a **(nincs) Storage-fiók**.
4. Válasszon egy tárfiókot az előfizetéshez tartozó hello listából. A legjobb teljesítmény érdekében használjon, amely hello Azure Storage-fiók ugyanabban a régióban hello Batch-fiókhoz, amelyen fut a feladat.

## <a name="persist-output-data"></a>Kimeneti adatok megőrzése

toopersist feladat- és kimeneti adatai hello fájl egyezmények könyvtárhoz, a tároló létrehozása az Azure Storage, majd mentse a hello kimeneti toohello tároló. Használjon hello [Azure Storage ügyféloldali kódtára a .NET](https://www.nuget.org/packages/WindowsAzure.Storage) a feladat kód tooupload hello tevékenység kimeneti toohello tárolóban. 

A tárolók és blobok az Azure Storage használatáról további információk: [az Azure Blob storage .NET használatának első lépései](../storage/blobs/storage-dotnet-how-to-use-blobs.md).

> [!WARNING]
> Feladat- és mindegyikhez hello fájl egyezmények könyvtárban tárolják a megőrzött hello ugyanabban a tárolóban. Ha nagyszámú feladatok próbálja toopersist fájlok: hello azonos idő, [szabályozás korlátok tárolási](../storage/common/storage-performance-checklist.md#blobs) is kényszeríthető.
> 
> 

### <a name="create-storage-container"></a>A tároló létrehozása

toopersist tevékenység kimeneti tooAzure tárolási, először létre kell hoznia egy tárolót meghívásával [CloudJob][net_cloudjob].[ PrepareOutputStorageAsync][net_prepareoutputasync]. A bővítmény metódus egy [CloudStorageAccount] [ net_cloudstorageaccount] objektum paraméterként. Létrehoz egy tároló elnevezett függően toohello fájl egyezmények szabványos, így a tartalma által felderíthető hello Azure portál és hello lekérés ismertetett módszerek hello cikk későbbi részében.

Hello kód toocreate a tároló általában helyez az ügyfélalkalmazást &mdash; hello a készletek, a feladatok és a feladatok létrehozó alkalmazást.

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

### <a name="store-task-outputs"></a>Tárolási feladatok kimenete

Most, hogy az Azure Storage tárolója előkészítése után, a feladatok mentheti-e a kimeneti toohello tároló hello segítségével [TaskOutputStorage] [ net_taskoutputstorage] osztály hello fájl egyezmények könyvtárban található.

A feladat kódban, először létre kell hoznia egy [TaskOutputStorage] [ net_taskoutputstorage] objektumot, majd a tevékenységeket hello feladat befejezése után hívható hello [TaskOutputStorage] [ net_taskoutputstorage]. [SaveAsync] [ net_saveasync] metódus toosave a kimeneti tooAzure tároló.

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

Hello `kind` hello paramétere [TaskOutputStorage](https://msdn.microsoft.com/library/microsoft.azure.batch.conventions.files.taskoutputstorage.aspx).[ SaveAsync](https://msdn.microsoft.com/library/microsoft.azure.batch.conventions.files.taskoutputstorage.saveasync.aspx) metódus kategorizálja hello megőrzött fájlok. Nincsenek négy előre definiált [TaskOutputKind] [ net_taskoutputkind] típusok: `TaskOutput`, `TaskPreview`, `TaskLog`, és `TaskIntermediate.` kimeneti egyéni kategóriáinak is meghatározhat.

Kimeneti típusaival milyen típusú kimenetek toolist előfordulhat, hogy később kötegelt hello a megőrzött egy adott feladat kimenetének toospecify engedélyezése. Ez azt jelenti amikor hello kimenetek tevékenységek listájának szűrheti hello listát hello kimeneti típusok egyike. Például "hello engedi *előzetes* tevékenység kimeneti *109*." Több listázása és kimenetének beolvasása megjelenik [lekérnie a kimenetet](#retrieve-output) újabb hello cikkben.

> [!TIP]
> hello kimeneti jellegű is meghatározza, hogy a hello Azure portál egy adott fájl helyére: *TaskOutput*-kategorizált fájlok alatt szerepelhet **tevékenység kimeneti fájlok**, és *TaskLog*fájlok alatt szerepelhet **naplók feladat**.
> 
> 

### <a name="store-job-outputs"></a>Feladat kimenetének tárolásához

Továbbá toostoring feladatok kimenete, tárolható egy teljes feladattal társított hello kimenetek. Például hello egyesítési feladatban movie megjelenítési feladat, sikerült megőrizni a teljes megjelenítve hello movie a feladat kimenete. A feladat befejezése után az ügyfélalkalmazást listában, és a hello kimenetek hello feladat beolvasása, és nem nem kell tooquery hello egyedi feladatok.

Feladat kimenetére tárolására hívó hello [JobOutputStorage][net_joboutputstorage].[ SaveAsync] [ net_joboutputstorage_saveasync] metódust, és adja meg a hello [JobOutputKind] [ net_joboutputkind] és fájlnév:

```csharp
CloudJob job = new JobOutputStorage(acct, jobId);
JobOutputStorage jobOutputStorage = job.OutputStorage(linkedStorageAccount);

await jobOutputStorage.SaveAsync(JobOutputKind.JobOutput, "mymovie.mp4");
await jobOutputStorage.SaveAsync(JobOutputKind.JobPreview, "mymovie_preview.mp4");
```

A hello **TaskOutputKind** típust a feladat kimenetének hello használnia [JobOutputKind] [ net_joboutputkind] típus toocategorize egy feladatot a megőrzött fájlok. Ez a paraméter lehetővé teszi (lista) egy adott típusú kimeneti toolater lekérdezés. Hello **JobOutputKind** típusú kimeneti és a kép tartalmazza, és támogatja az egyéni kategóriák.

### <a name="store-task-logs"></a>A feladat naplók tárolására

Ezenkívül toopersisting egy fájl toodurable tárolására, ha egy feladat vagy a feladat befejeződik, szükséges lehet a toopersist fájlok hello egy feladat végrehajtása során &mdash; naplófájlok vagy `stdout.txt` és `stderr.txt`, például. Erre a célra hello Azure Batch fájl egyezmények kódtár biztosít hello [TaskOutputStorage][net_taskoutputstorage].[ SaveTrackedAsync] [ net_savetrackedasync] metódust. A [SaveTrackedAsync][net_savetrackedasync], nyomon követheti a frissítések tooa fájlt (a megadott időközönként) hello csomóponton, és ezen frissítések tooAzure tárolási megőrzéséhez.

A következő kódrészletet hello, használjuk [SaveTrackedAsync] [ net_savetrackedasync] tooupdate `stdout.txt` az Azure Storage 15 másodpercenként hello hello feladat végrehajtása során:

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

hello megjegyzésként szakasz `Code tooprocess data and produce output file(s)` hello kódot, amely a feladat általában hajtaná végre helyőrzője. Például lehetséges, hogy kódot, amely letölti az adatokat az Azure Storage és átalakítás vagy számítási végez azt. hello fontos része a részlet van bemutatásához, hogyan akkor futtathatja a kódok egy `using` blokk tooperiodically fájl frissítése [SaveTrackedAsync][net_savetrackedasync].

hello csomópont ügynök egy olyan program, hello készlet minden egyes csomópontján fut, és hello parancs vezérlő és felületet hello csomópont és hello Batch szolgáltatás között. Hello `Task.Delay` tekintendő, amely szükséges a végén hello ez `using` blokk tooensure, hogy a csomópont ügynök hello tartalma idő tooflush hello standard toohello stdout.txt fájlt hello csomóponton. Ez a késleltetés nélkül is lehetséges toomiss hello kimenet utolsó néhány másodpercig. Ez a késés nem lehet szükség az összes fájl.

> [!NOTE]
> Ha engedélyezi a nyomkövetési fájl **SaveTrackedAsync**, csak *hozzáfűzi* toohello nyomon követett fájl megőrzött tooAzure tárolási. Ezt a módszert csak a nyomkövetési naplófájlok nem elforgatása, vagy más fájlok toowith írt hozzáfűzése hello fájl műveletek toohello végét.
> 
> 

## <a name="retrieve-output-data"></a>Kimeneti adatainak beolvasása

Beolvasni a megőrzött kimeneti hello Azure Batch fájl egyezmények kódtár használatával, ha ezt teszi feladat - és a feladat-központú módon. Hello kimeneti a feladat vagy a feladatot a megadott tooknow anélkül Azure Storage, vagy még a fájl neve az elérési úttal is igényelhet. Ehelyett kérheti a kimeneti fájlok feladat, vagy sikertelen feladat-azonosítót.

hello következő kódrészletet a feladat tevékenységeit telepítéseket hello kimeneti fájlok hello feladat kapcsolatos információkat jelenít és tárolásból tölti le a fájlokat.

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

## <a name="view-output-files-in-hello-azure-portal"></a>A kimeneti fájlok nézet hello Azure-portálon

hello Azure-portál megjeleníti tevékenység kimeneti fájlok és a megőrzött tooa-naplók kapcsolódó hello Azure Storage-fiókra [kötegelt fájl egyezmények standard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions). Megvalósíthat szabályoknak saját kezűleg a hello egy tetszőleges nyelv, vagy hello fájl egyezmények könyvtár a .NET-alkalmazásokban használható.

a kimeneti fájlok hello portálon tooenable hello megjelenítését, meg kell felelniük a következő követelmények hello:

1. [Egy Azure Storage-fiók csatolása](#requirement-linked-storage-account) tooyour Batch-fiókhoz.
2. Az előre megadott toohello tároló- és a fájlok elnevezési szabályai igazodik, amikor kimenetek megőrzése. Ezek egyezmények hello definition hello fájl egyezmények könyvtárban található [információs][github_file_conventions_readme]. Ha hello [Azure Batch fájl egyezmények] [ nuget_package] könyvtár toopersist a kimeneti, a fájlok megmaradnak toohello szabványos fájl konvenciók szerint.

tooview tevékenység kimeneti fájlokat, és bejelentkezik hello Azure-portálon, keresse meg a toohello tevékenység, amelynek kimenete érdekli, majd kattintson **mentett kimeneti fájlok** vagy **mentett naplókat**. A képen látható hello **mentett kimeneti fájlok** hello feladat "007" azonosítójú:

![A feladat kimenete panel az Azure-portálon hello][2]

## <a name="code-sample"></a>Kódminta

Hello [PersistOutputs] [ github_persistoutputs] mintaprojektet egyike hello [Azure Batch-Kódminták] [ github_samples] a Githubon. A Visual Studio megoldás bemutatja, hogyan toouse hello Azure Batch fájl egyezmények könyvtár toopersist tevékenység kimeneti toodurable tároló. toorun hello mintát, kövesse az alábbi lépéseket:

1. Nyissa meg hello projektre a **Visual Studio 2015-ös vagy újabb**.
2. A kötegelt és a tárolási **fiók hitelesítő adatait** túl**AccountSettings.settings** hello Microsoft.Azure.Batch.Samples.Common projektben.
3. **Build** (de ne futtassa) hello megoldás. Ha a rendszer kéri, állítsa vissza a NuGet-csomagok.
4. Használjon hello Azure portál tooupload egy [alkalmazáscsomag](batch-application-packages.md) a **PersistOutputsTask**. Hello tartalmaznak `PersistOutputsTask.exe` és a függő szerelvényeket hello .zip-csomagja, hello alkalmazás azonosítója túl "PersistOutputsTask" és az alkalmazás hello Csomagverzió túl "1.0".
5. **Start** (Futtatás) hello **PersistOutputs** projekt.
6. Amikor a kért toochoose hello adatmegőrzési technológia toouse futó hello minta be **1** toorun hello mintákkal hello fájl egyezmények könyvtár toopersist feladat kimenete. 

## <a name="next-steps"></a>Következő lépések

### <a name="get-hello-batch-file-conventions-library-for-net"></a>Hello kötegelt fájl egyezmények könyvtár beolvasása a .NET-hez

hello kötegelt fájl egyezmények .NET-keretrendszerhez készült érhető el a [NuGet][nuget_package]. hello könyvtár bővíti hello [CloudJob] [ net_cloudjob] és [CloudTask] [ net_cloudtask] osztályok új metódusával. Lásd még: hello [dokumentáció](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.conventions.files) hello fájl egyezmények könyvtárhoz.

Hello [forráskód] [ github_file_conventions] hello fájl egyezmények könyvtár a Microsoft Azure SDK hello .NET tárház elérhető a Githubon. 

### <a name="explore-other-approaches-for-persisting-output-data"></a>Más megoldások tárolásakor kimeneti adatok felfedezése

- Lásd: [Persist feladat- és kimeneti tooAzure tárolási](batch-task-output.md) áttekintését a feladat és a feladat adatait.
- Lásd: [megőrizni a feladat adatainak tooAzure tárolási hello Batch szolgáltatás API](batch-task-output-files.md) toolearn hogyan toouse hello Batch szolgáltatás API toopersist kimeneti adatokat.

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
