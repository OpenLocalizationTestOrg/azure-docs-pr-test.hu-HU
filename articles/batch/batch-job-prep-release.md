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
# <a name="run-job-preparation-and-job-release-tasks-on-batch-compute-nodes"></a>Futtatási feladat előkészítése és a feladat kiadási tevékenységek a Batch számítási csomópontjain

 Egy Azure kötegelt gyakran néhány adatot kér a telepítő a feladatokat hajtja végre a rendszer, és karbantartási utáni feladat, a feladatok befejezése előtt. Lehet, hogy a kell toodownload közös tevékenység bemeneti adatok tooyour számítási csomópontokat, vagy töltse fel a tevékenység kimeneti adatok tooAzure tárolási hello feladat befejezése után. Használhat **előkészítő feladat** és **kiadás feladat** feladatok tooperform ezeket a műveleteket.

## <a name="what-are-job-preparation-and-release-tasks"></a>Mi feladat előkészítése és a feladatok kiadási?
A feladat tevékenységeit futtatja, mielőtt hello feladat előkészítése tevékenységet a futó minden számítási csomópont ütemezett toorun legalább egy feladat. Ha hello feladat befejeződött, hello feladat kiadása tevékenység hello-készlet, amely legalább egy feladat végrehajtása minden egyes csomópontján fut. Normál kötegelt feladatok, adjon meg egy parancssori toobe meghívni, amikor a feladat előkészítése, illetve kiadás feladat futtatása.

Feladat előkészítése és a kiadási tevékenységek megszokott kötegelt feladat szolgáltatásokat kínálnak például fájl letöltése ([erőforrásfájlok][net_job_prep_resourcefiles]), emelt szintű végrehajtási, egyéni környezeti változókat, végrehajtási maximális időtartam, újrapróbálkozások maximális számát és fájl adatmegőrzési időtartam.

A következő szakaszok hello, megtudhatja, hogyan toouse hello [JobPreparationTask] [ net_job_prep] és [JobReleaseTask] [ net_job_release] hello található osztályok [Batch .NET] [ api_net] könyvtárban.

> [!TIP]
> Feladat előkészítése és a kiadási tevékenységek hasznosak lehetnek "megosztás készlet" környezetben, amelyben számítási csomópontok készlete közötti feladat futtatása továbbra is fennáll, és sok feladatok által használt.
> 
> 

## <a name="when-toouse-job-preparation-and-release-tasks"></a>Ha a toouse előkészítő feladat, és felszabadíthatja a feladatok
Feladat előkészítése és a feladat kiadása feladatok esetében a remekül beválik, ha a következő helyzetekben hello:

**Általános feladat adatok letöltése**

Kötegelt feladatok gyakran megkövetelik a közös adatkészletet hello feladat tevékenységeit bemenetként. Napi kockázati elemzés számítások, például piaci adatok feladat-specifikus, de közös tooall feladatok hello feladat. A piacon adatok gyakran számos gigabájtig terjedhetnek, így hello csomóponton futó minden feladatot használhatja azt csak egyszer kell letöltött tooeach számítási csomópont. Használja a **feladat előkészítése tevékenységet** toodownload ezen adatok tooeach csomópont más feladatok hello hello feladat végrehajtása előtt.

**Feladat- és kimeneti törlése**

"Megosztás készlet" környezetben, ahol nincsenek leszerelt feladatok között a készlet számítási csomópontokat, szükség lehet a feladatadatok toodelete fusson. Előfordulhat, hogy tooconserve hello csomópontok hely a lemezen, vagy megfelelnek a szervezet biztonsági házirendjeivel. Használja a **feladat kiadása tevékenység** tölti le a feladat előkészítése tevékenység, vagy során toodelete adatok feladat végrehajtása.

**Napló megőrzési**

Érdemes lehet tookeep naplófájlokat, amelyek a feladatokat, vagy esetleg összeomlási memóriaképek sikertelen alkalmazások által létrehozott egy példányát. Használja a **feladat kiadása tevékenység** az ilyen esetekben toocompress, és töltse fel az adatok tooan [Azure Storage] [ azure_storage] fiók.

> [!TIP]
> Egy másik módja toopersist naplók és egyéb feladat- és kimeneti adatok toouse hello [Azure Batch fájl egyezmények](batch-task-output.md) könyvtár.
> 
> 

## <a name="job-preparation-task"></a>Feladat előkészítése tevékenységet
Egy feladat feladatok végrehajtása, mielőtt kötegelt hello feladat előkészítése tevékenységet minden számítási csomóponton, amely a feladatok ütemezett toorun hajtja végre. Alapértelmezés szerint hello Batch szolgáltatás megvárja-e a hello feladat előkészítése tevékenység toobe hello feladatok ütemezett tooexecute hello csomóponton futó előtt végzi el. Azonban nem toowait hello szolgáltatást konfigurálhatja. Ha hello csomópont újraindul, hello feladat előkészítése tevékenységet újra fut, de le is tilthatja ezt a viselkedést.

hello feladat előkészítése tevékenységet csak olyan csomópontot, amely ütemezett toorun feladat végrehajtása. Ez megakadályozza, hogy hello szükségtelen előkészítő feladat végrehajtásának abban az esetben, ha egy csomópont nem hozzá van rendelve egy feladatot. Ez akkor fordulhat elő, ha a feladat feladatok hello száma nem éri el a készletben található csomópontok számának hello. Akkor is érvényes, ha [egyidejű feladat a végrehajtás](batch-parallel-node-tasks.md) engedélyezve van, amely hagyja egyes csomópontok üresjárati Ha hello feladatok száma nem éri el hello teljes lehetséges egyidejű feladatok. Nem hello feladat előkészítése tevékenységet a kiszolgálón való futtatásával üresjárati csomópontok, adatok adatátviteli költségek kevesebb pénz képes költeni.

> [!NOTE]
> [JobPreparationTask] [ net_job_prep_cloudjob] eltér az [CloudPool.StartTask] [ pool_starttask] abban a JobPreparationTask hello elején minden feladatot hajt végre, mivel StartTask végrehajtja, csak ha a számítási csomópont először csatlakozik egy készletet vagy újraindul.
> 
> 

## <a name="job-release-task"></a>Feladat kiadása tevékenység
Ha egy feladat be van jelölve befejezettként, hello feladat kiadása tevékenység végrehajtása hello készlet, amely legalább egy feladat végrehajtása minden egyes csomóponton. Megszakítási kérelmet befejezte megjelölése az egy feladatot. hello a Batch szolgáltatás, majd a készlet túl hello feladatállapotot*leáll*hello feladattal társított minden aktív vagy futó feladatot megszakítja és hello feladat kiadása tevékenységet futtatja. hello feladat helyezi toohello *befejeződött* állapotát.

> [!NOTE]
> Feladat törlése hello feladat kiadása tevékenységet is végez. Azonban ha egy feladat már meg lett szakítva, hello kiadás feladat nem fut még egyszer hello feladat később törlésekor.
> 
> 

## <a name="job-prep-and-release-tasks-with-batch-net"></a>Előkészítő feladat, és felszabadíthatja a feladatok a Batch .NET kódtárral
a feladat előkészítése tevékenység toouse hozzárendelése egy [JobPreparationTask] [ net_job_prep] objektum tooyour feladat [CloudJob.JobPreparationTask] [ net_job_prep_cloudjob] tulajdonság . Hasonlóképpen, inicializálni egy [JobReleaseTask] [ net_job_release] , és rendelje hozzá tooyour feladat [CloudJob.JobReleaseTask] [ net_job_prep_cloudjob] tulajdonság tooset hello feladat kiadása tevékenységet.

A következő kódrészletet `myBatchClient` példánya [BatchClient][net_batch_client], és `myPool` egy meglévő készlet hello Batch-fiók belül van.

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

A korábban említett hello kiadási tevékenység végrehajtása, ha egy feladat megszakadt, vagy törölték. Egy feladat leáll [JobOperations.TerminateJobAsync][net_job_terminate]. A feladat törlése [JobOperations.DeleteJobAsync][net_job_delete]. Általában leáll, vagy törli a feladatot, feladatainak befejezésekor, vagy amikor eléri a beállított időkorlát.

```csharp
// Terminate hello job toomark it as Completed; this will initiate the
// Job Release Task on any node that executed job tasks. Note that the
// Job Release Task is also executed when a job is deleted, thus you
// need not call Terminate if you typically delete jobs after task completion.
await myBatchClient.JobOperations.TerminateJobAsy("JobPrepReleaseSampleJob");
```

## <a name="code-sample-on-github"></a>A Githubon kódminta
toosee feladat előkészítése és a kiadási feladatokat művelet, tekintse meg a hello [JobPrepRelease] [ job_prep_release_sample] mintaprojektet a Githubon. Ezt a konzolalkalmazást hello a következő:

1. Egy készletet hoz létre két "kicsi" csomópont.
2. Létrehoz egy feladatot a feladat előkészítése, a kiadás és a szabványos feladatok.
3. Futtatása hello előkészítő feladat, amely először írja a csomópont "megosztott" könyvtárban hello csomópont azonosítója tooa szövegfájl.
4. Minden egyes csomóponton a feladat Azonosítóját toohello írja futtat egy feladatot azonos szövegfájl.
5. Miután minden feladat befejeződött (vagy hello időtúllépését), jelenít meg minden egyes csomópont szöveges fájl toohello konzol hello tartalmát.
6. Hello feladat befejezése után futtatja hello feladat kiadása tevékenység toodelete hello fájl hello csomópont.
7. Megrendelése hello kilépési kódokat a hello feladat előkészítése, és felszabadíthatja a feladatokat az egyes csomópontok, amelyeken végre.
8. Szünetel végrehajtási tooallow megerősítése feladat és/vagy a készlet törlése.

A mintaalkalmazás hello hasonló toohello következő kimenete:

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
> Lejáró toohello változó létrehozása és a kezdési idő a csomópontok egy új készletet (az egyes csomópontok állnak készen a többi előtt feladatok) a különböző kimeneti jelenhet meg. Pontosabban hello feladatok gyorsan végezze el, mert egy hello készlet csomópontok előfordulhat, hogy execute hello feladat feladatokat. Ha ez történik, megfigyelheti, hogy hello feladat-előkészítő feladatát, és felszabadíthatja a feladatok által végrehajtott feladatok nem hello csomópontja számára nem léteznek.
> 
> 

### <a name="inspect-job-preparation-and-release-tasks-in-hello-azure-portal"></a>Vizsgálja meg a feladat előkészítése és a kiadási feladatokat hello Azure-portálon
Hello mintaalkalmazás futtatásakor hello használhatja [Azure-portálon] [ portal] tooview hello feladat és a feladatok tulajdonságainak hello, vagy akár hello megosztott szöveges fájl hello feladat tevékenységeit által módosított.

az alábbi hello képernyőfelvételen látható hello **előkészítő feladatok panel** a hello Azure-portálon után hello mintaalkalmazás futását. Keresse meg a toohello *JobPrepReleaseSampleJob* tulajdonságok a feladatok befejezése után (de a feladat és a készlet törlése előtt), és kattintson a **előkészítő feladatok** vagy **kiadási tevékenységek** tooview tulajdonságaikat.

![Azure-portálon feladattulajdonság előkészítése][1]

## <a name="next-steps"></a>Következő lépések
### <a name="application-packages"></a>Alkalmazáscsomagok
Továbbá toohello feladat előkészítése tevékenység esetében is használhatja hello [alkalmazáscsomagok](batch-application-packages.md) kötegelt tooprepare szolgáltatása számítási csomópontjain a végrehajtásához. Ez a szolgáltatás különösen fontos telepítőhöz, alkalmazásokat, amelyek számos fájljait (100 +) vagy szigorú verziókezelést igénylő alkalmazások nem igénylő alkalmazások központi telepítéséhez.

### <a name="installing-applications-and-staging-data"></a>Alkalmazások telepítése és átmeneti adatokat
Az MSDN fórum bejegyzése a csomópont előkészítése az éppen futó feladatok többféle módszer áttekintése:

[Alkalmazások telepítése és átmeneti adatokat a Batch számítási csomópontjain][forum_post]

Írták hello Azure Batch csoporttagok közül, hogy miként használható toodeploy alkalmazások és adatok toocompute csomópontok számos módszert.

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
