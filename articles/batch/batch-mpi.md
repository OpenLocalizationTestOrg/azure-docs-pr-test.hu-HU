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
# <a name="use-multi-instance-tasks-toorun-message-passing-interface-mpi-applications-in-batch"></a>A kötegelt többpéldányos feladatok toorun Message Passing Interface (MPI) alkalmazások használata

Többpéldányos feladatok lehetővé teszik az Azure Batch feladat toorun több számítási csomóponton egyidejűleg. Ezek a feladatok lehetővé teszik a nagy teljesítményű számítástechnikai forgatókönyvekhez hasonlóan a Message Passing Interface (MPI) alkalmazások kötegben. Ebből a cikkből megismerheti, hogyan tooexecute többpéldányos feladatok hello [Batch .NET] [ api_net] könyvtárban.

> [!NOTE]
> A cikkben szereplő példák hello összpontosítani Batch .NET, MS-MPI, és a Windows számítási csomópontok, hello többpéldányos feladat itt tárgyalt a következők alkalmazható tooother platformok és technológiák (a Python és a Linux-csomópont, például Intel MPI).
>
>

## <a name="multi-instance-task-overview"></a>Többpéldányos feladat áttekintése
A kötegelt, minden feladata a normál esetben – egyetlen számítási csomóponton végre több feladatok tooa feladat elküldését, és az hello Batch szolgáltatás ütemezések minden egyes csomóponton végrehajtandó feladatot. A tevékenység konfigurálásával azonban **többpéldányos beállítások**, pedig utasítani fogja a kötegelt tooinstead hozzon létre egy elsődleges feladat és több résztevékenység, amely több csomóponton végrehajthatók.

![Többpéldányos feladat áttekintése][1]

Kötegazonosítójú feladat többpéldányos beállítások tooa feladat elküldésekor a kötegelt több lépéseket egyedi toomulti-példány feladatokat hajtja végre:

1. hello Batch szolgáltatás létrehoz egy **elsődleges** és több **résztevékenység** hello többpéldányos beállításai alapján. hello számának egyeznie kell (elsődleges és minden résztevékenység) feladatok teljes száma hello **példányok** (számítási csomópontok) hello többpéldányos beállításokat ad meg.
2. Kötegelt jelöli ki valamelyik hello számítási csomópontok hello **fő**, és az ütemezések hello elsődleges feladat tooexecute hello főkiszolgálón. Hello résztevékenység tooexecute hello fennmaradó hello számítási csomópontok lefoglalt toohello többpéldányos feladat, csomópontonként egy részfeladatnál annak regisztrálása az ütemezés.
3. elsődleges hello és minden résztevékenység letölteni egy **közös erőforrásfájlok** hello többpéldányos beállításokat ad meg.
4. Hello közös erőforrás fájlok letöltését követően elsődleges hello és részfeladatok végrehajtása hello **koordinációs parancs** hello többpéldányos beállításokat ad meg. hello koordinációs parancs hello feladat végrehajtása általánosan használt tooprepare-csomópont. Ilyen lehet például a háttér-szolgáltatás indítása (például [Microsoft MPI][msmpi_msdn]tartozó `smpd.exe`) és annak ellenőrzése, hogy hello csomópontok készen tooprocess csomópontok közötti üzenetek.
5. hello elsődleges feladat végrehajtása hello **alkalmazás parancs** hello fő csomóponton *után* hello koordinációs parancs sikeresen befejeződött elsődleges hello és minden résztevékenység. hello alkalmazás parancs hello parancssori hello többpéldányos feladat magát, és csak hello elsődleges feladatot futtatja. Az egy [MS-MPI][msmpi_msdn]-alapú megoldás, ahol végrehajtani a a MPI-kompatibilis alkalmazások használata azt `mpiexec.exe`.

> [!NOTE]
> Habár funkcionálisan különálló, hello "többpéldányos feladat" egyedi feladat típus nincs például hello [StartTask] [ net_starttask] vagy [JobPreparationTask] [ net_jobprep]. hello többpéldányos feladata egyszerűen egy szabványos kötegelt feladat ([CloudTask] [ net_task] a Batch .NET) amelynek többpéldányos beállítások lettek konfigurálva. Ebben a cikkben, hello toothis irányítjuk **többpéldányos feladat**.
>
>

## <a name="requirements-for-multi-instance-tasks"></a>Többpéldányos feladatok követelményei
Többpéldányos feladatok elvégzéséhez a készlet **engedélyezett csomópontok közötti kommunikáció**, és a **egyidejű feladat a végrehajtás letiltja**. toodisable egyidejű feladat a végrehajtás, set hello [CloudPool.MaxTasksPerComputeNode](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool#Microsoft_Azure_Batch_CloudPool_MaxTasksPerComputeNode) tulajdonság too1.

A kódrészletet bemutatja, hogyan toocreate egy tárolókészletben többpéldányos feladatok hello Batch .NET kódtár használatával.

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
> Ha a többpéldányos feladat a fürtök csomóponton belüli kommunikációjához közötti kommunikáció a készletben engedélyezett toorun, vagy az egy *maxTasksPerNode* 1-nél nagyobb értéket, hello feladat soha nem ütemezett--határozatlan ideig hello "aktív" állapotban marad. 
>
> Többpéldányos feladatokat hajthat végre 2015. December 14. után létrehozott készletek csomópontján.
>
>

### <a name="use-a-starttask-tooinstall-mpi"></a>Egy StartTask tooinstall MPI használata
toorun MPI alkalmazások többpéldányos feladatokkal, először tooinstall MPI megvalósítása (MS-MPI vagy Intel MPI, például) hello készletben hello számítási csomóponton. Ez az egy időben toouse egy [StartTask][net_starttask], amely hajt végre, amikor a csomópont csatlakozik egy készletet, vagy újraindítják. A kódrészletet, amely meghatározza az MS-MPI hello telepítőcsomagját, egy StartTask hoz létre egy [erőforrásfájl][net_resourcefile]. hello kezdő tevékenység Parancssor végrehajtása után hello erőforrásfájl letöltött toohello csomópont. Ebben az esetben a hello parancssori MS-MPI felügyelet nélküli telepítés hajt végre.

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

### <a name="remote-direct-memory-access-rdma"></a>Távoli közvetlen memória-hozzáférés (RDMA)
Ha úgy dönt, egy [RDMA-kompatibilis mérete](../virtual-machines/windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) A9 hello a számítási csomópontok a Batch-készlet, mint például a MPI alkalmazás előnyeit az Azure nagy teljesítményű, alacsony késésű távoli közvetlen memória-hozzáférés (RDMA) hálózati.

Keresse meg az "RDMA-kompatibilis" szerepel a következő cikkek hello hello méretek:

* **CloudServiceConfiguration** készletek

  * [A Felhőszolgáltatások mérete](../cloud-services/cloud-services-sizes-specs.md) (csak Windows)
* **VirtualMachineConfiguration** készletek

  * [Az Azure virtuális gépek méretei](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Linux)
  * [Az Azure virtuális gépek méretei](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Windows)

> [!NOTE]
> az RDMA tootake előnyeit [Linux számítási csomópontok](batch-linux-nodes.md), kell használnia **Intel MPI** hello csomópontján. A CloudServiceConfiguration és VirtualMachineConfiguration készletek további információkért lásd: hello készlet szakasza hello [Batch funkcióinak áttekintése](batch-api-basics.md).
>
>

## <a name="create-a-multi-instance-task-with-batch-net"></a>A Batch .NET többpéldányos feladat létrehozása
Most, hogy azt már szabályozott hello készlet követelmények és MPI csomag telepítése, hozzon létre hello többpéldányos feladat. Az ezt a kódrészletet standard létrehozhatunk [CloudTask][net_task], majd konfigurálja a [MultiInstanceSettings] [ net_multiinstance_prop] tulajdonság. A korábban említett hello többpéldányos feladat nincs a különböző feladattípust, de egy szabványos kötegelt feladat többpéldányos beállításokat.

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

## <a name="primary-task-and-subtasks"></a>Elsődleges feladatok és részfeladatok
Hello létrehozásakor egy feladat többpéldányos beállításai hello számítási csomópontokat, amelyek tooexecute hello feladat számát adja meg. Elküldött hello feladat tooa feladat, a Batch szolgáltatás hello létrehoz egy **elsődleges** feladatot, és elég **résztevékenység** együtt megfelelő a megadott csomópontok hello száma.

Ezek a feladatok rendelt hello tartomány 0 egész azonosító túl*numberOfInstances* – 1. hello feladat 0 azonosítójú hello elsődleges feladat, és minden egyéb azonosítók résztevékenység. Például ha egy feladat többpéldányos beállításai a következő hello hoz létre, hello elsődleges feladat kellene 0 azonosítót, és hello résztevékenység rendelkeznie azonosítók 1 – 9.

```csharp
int numberOfNodes = 10;
myMultiInstanceTask.MultiInstanceSettings = new MultiInstanceSettings(numberOfNodes);
```

### <a name="master-node"></a>Fő csomópont
A többpéldányos feladatok elküldésekor hello Batch szolgáltatás jelöli ki valamelyik hello számítási csomópontok hello "fő" csomópont, és ütemezések hello elsődleges feladat tooexecute hello fő csomóponton. hello résztevékenység ütemezett tooexecute toohello többpéldányos feladat lefoglalt hello csomópontok hello további része a rendszer.

## <a name="coordination-command"></a>Koordinációs parancs
Hello **koordinációs parancs** hello elsődleges, mind a résztevékenység futtatja.

hello koordinációs parancs meghívása hello blokkolja--kötegelt nem hajtható végre: hello alkalmazás parancs amíg hello koordinációs parancs sikeresen visszatért az összes résztevékenység. hello koordinációs parancsot kell ezért szükséges háttér szolgáltatások indítása, győződjön meg arról, hogy azok készen áll a használatra, és zárja be. A koordinációs parancs használatával a MS-MPI 7-es verzió megoldás például hello csomóponton hello SMPD szolgáltatás elindul, majd kilép:

```
cmd /c start cmd /c ""%MSMPI_BIN%\smpd.exe"" -d
```

Vegye figyelembe a hello használata `start` koordinációs parancsban. Ez azért szükséges, mert hello `smpd.exe` végrehajtása után azonnal alkalmazás tér vissza. Hello hello használata nélkül [start] [ cmd_start] parancs, a tranzakciókoordináció-parancs nem alakítanák vissza, és ezért megakadályozza a hello alkalmazás parancs futtatását.

## <a name="application-command"></a>Alkalmazás parancs
Hello elsődleges feladat és összes résztevékenység végzett hello koordinációs parancs végrehajtása, amennyiben hello többpéldányos feladat parancssori hello elsődleges feladat végrehajtása *csak*. Ezt nevezik a hello **alkalmazás parancs** toodistinguish hello koordinációs parancs le.

MS-MPI-alkalmazások használata hello alkalmazás parancs tooexecute a MPI-kompatibilis alkalmazás a `mpiexec.exe`. Például ez az alkalmazás parancs használatával a MS-MPI 7-es verzió megoldás:

```
cmd /c ""%MSMPI_BIN%\mpiexec.exe"" -c 1 -wdir %AZ_BATCH_TASK_SHARED_DIR% MyMPIApplication.exe
```

> [!NOTE]
> Mivel az MS-MPI `mpiexec.exe` használ hello `CCP_NODES` változó alapértelmezés szerint (lásd: [környezeti változók](#environment-variables)) hello példa a fenti alkalmazás parancssor nem tartalmazza azt.
>
>

## <a name="environment-variables"></a>Környezeti változók
Kötegelt hoz létre több [környezeti változók] [ msdn_env_var] hello toomulti-példány feladatai számítási csomópontok lefoglalt tooa többpéldányos feladat. A koordinációs és alkalmazás parancssorokat hivatkozhat ezen környezeti változókkal, parancsfájlok és az általuk programok is hello.

hello következő környezeti változók vannak által hello Batch-szolgáltatás a feladatok által létrehozott többpéldányos:

* `CCP_NODES`
* `AZ_BATCH_NODE_LIST`
* `AZ_BATCH_HOST_LIST`
* `AZ_BATCH_MASTER_NODE`
* `AZ_BATCH_TASK_SHARED_DIR`
* `AZ_BATCH_IS_CURRENT_NODE_MASTER`

Ezek a részletes és hello más Batch számítási csomópont környezeti változókat, beleértve azok tartalmát és a Láthatóság: [számítási csomópont környezeti változók][msdn_env_var].

> [!TIP]
> hello kötegelt Linux MPI kódminta egy példával hogyan ezen környezeti változókkal számos használható-e. Hello [tranzakciókoordináció-cmd] [ coord_cmd_example] Bash parancsfájl közös alkalmazás és a bemeneti fájlokat tölti le a Azure Storage-ból, lehetővé teszi, hogy a hálózati fájlrendszer (NFS) megosztott hello főcsomópont helyén, és konfigurálja a többi csomópont hello NFS-ügyfelek rendelkezésre bocsátott toohello többpéldányos feladat.
>
>

## <a name="resource-files"></a>Erőforrás-fájlok
Az erőforrás fájlok tooconsider többpéldányos feladatok két csoportját: **közös erőforrásfájlok** , amely *összes* feladatok letöltése (mind az elsődleges és részfeladatok), és hello **erőforrásfájlok** hello többpéldányos feladat, amely megadott *csak elsődleges hello* letöltések feladat.

Megadhat egy vagy több **közös erőforrásfájlok** hello többpéldányos beállításaiban feladathoz. A közös erőforrás fájlokat szeretne letölteni a [Azure Storage](../storage/common/storage-introduction.md) az egyes csomópontok **feladat megosztott könyvtár** elsődleges hello és minden résztevékenység. Elérhető hello feladat megosztott könyvtár alkalmazás és koordinációs parancssorokat hello segítségével `AZ_BATCH_TASK_SHARED_DIR` környezeti változó. Hello `AZ_BATCH_TASK_SHARED_DIR` elérési út minden csomópont lefoglalt toohello többpéldányos tevékenységen azonos, így megoszthatja a közötti elsődleges hello és minden résztevékenység egyetlen koordinációs parancsot. Kötegelt nem "könyvtárban" hello távelérési értelemben, de csatlakoztatási használni, vagy pont, ahogy azt korábban említettük, a környezeti változók hello tipp a korábbi megosztásához.

Hello többpéldányos magát a rendszer letöltött toohello feladatütemezési munkakönyvtár, az erőforrásfájlokat `AZ_BATCH_TASK_WORKING_DIR`, alapértelmezés szerint. Ahogy azt korábban említettük, ezzel szemben toocommon erőforrásfájlokat, csak hello elsődleges feladat fájlokat tölti le erőforrás megadott hello többpéldányos tevékenység saját magát.

> [!IMPORTANT]
> Mindig használjon hello környezeti változók `AZ_BATCH_TASK_SHARED_DIR` és `AZ_BATCH_TASK_WORKING_DIR` a parancssorok toorefer toothese könyvtárakat. Ne próbálja meg manuálisan tooconstruct hello elérési utakat.
>
>

## <a name="task-lifetime"></a>Feladatütemezés élettartama
hello hello elsődleges feladat vezérlők hello élettartama hello teljes többpéldányos feladatütemezés élettartama. Amikor elsődleges hello kilép, az összes hello résztevékenység leállítása van. hello kilépési kód elsődleges hello hello kilépési kód hello feladat, és ezért használt toodetermine hello sikerességét vagy sikertelenségét hello feladat újrapróbálkozási célokra.

Ha bármelyik hello résztevékenység sikertelen, kilép egy nem nulla visszatérési kóddal például hello teljes többpéldányos feladat sikertelen lesz. hello többpéldányos feladat majd lezárva, és újra megpróbálja mentése tooits Újrapróbálkozási korlát.

A többpéldányos tevékenység törlésekor elsődleges hello és minden résztevékenység is törli hello Batch-szolgáltatás. Az összes végző részfeladat a könyvtárak, és a fájlok törlődnek hello számítási csomópontokat, ugyanúgy, mint a szabványos feladat.

[TaskConstraints] [ net_taskconstraints] többpéldányos tevékenység, például a hello [MaxTaskRetryCount][net_taskconstraint_maxretry], [MaxWallClockTime] [ net_taskconstraint_maxwallclock], és [RetentionTime] [ net_taskconstraint_retention] tulajdonságainak vannak figyelembe véve, egy szabványos tevékenység, és alkalmazza a toohello elsődleges és az összes résztevékenység. Azonban ha megváltoztatja hello [RetentionTime] [ net_taskconstraint_retention] alkalmazott csak toohello elsődleges feladat tulajdonság hozzáadása hello többpéldányos feladat toohello feladat, a módosítás után. Az összes hello résztevékenység továbbra is toouse hello eredeti [RetentionTime][net_taskconstraint_retention].

A számítási csomópont legutóbbi tevékenységek listájának hello azonosítója egy részfeladatnál annak regisztrálása tükrözi, ha a legutóbbi feladat hello többpéldányos feladat része volt.

## <a name="obtain-information-about-subtasks"></a>Résztevékenység kapcsolatos információkhoz
hello Batch .NET könyvtár hívás hello használatával résztevékenység tooobtain információk [CloudTask.ListSubtasks] [ net_task_listsubtasks] metódust. Ez a módszer az összes résztevékenység információkat ad vissza, és hello információ csomópontjain hello feladatok végrehajtása. Ezekből az adatokból meghatározhatja, hogy minden részfeladatnál annak regisztrálása a gyökérkönyvtár, hello alkalmazáskészlet-azonosító, a jelenlegi állapotában, kilépési kód, és több. Ez az információ hello együtt használható [PoolOperations.GetNodeFile] [ poolops_getnodefile] metódus tooobtain hello részfeladatnál annak regisztrálása a fájlokat. Vegye figyelembe, hogy ez a metódus nem ad vissza adatokat hello elsődleges feladat (azonosító: 0).

> [!NOTE]
> Hacsak másként nem jelezzük, a Batch .NET-metódusokat, amely működik többpéldányos hello [CloudTask] [ net_task] maga alkalmazása *csak* toohello elsődleges feladat. Például, ha meghívja a hello [CloudTask.ListNodeFiles] [ net_task_listnodefiles] metódus a többpéldányos tevékenységen, csak hello elsődleges feladat fájlok vannak adott vissza.
>
>

hello következő kódrészletet bemutatja, hogyan tooobtain végző részfeladat információkat, valamint fájl tartalmának kérhet hello csomópontokat, amelyeken végre.

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

## <a name="code-sample"></a>Kódminta
Hello [MultiInstanceTasks] [ github_mpi] a Githubon példakód azt mutatja be, hogyan toouse egy többpéldányos feladat toorun egy [MS-MPI] [ msmpi_msdn] alkalmazás Batch számítási csomóponton. Hello kövesse [előkészítése](#preparation) és [végrehajtási](#execution) toorun hello minta.

### <a name="preparation"></a>Előkészítése
1. Kövesse hello első két [hogyan toocompile és egyszerű MS-MPI program futtatása][msmpi_howto]. Ez megfelel a következő hello hello prerequesites lépést.
2. Build egy *kiadás* hello verziójának [MPIHelloWorld] [ helloworld_proj] minta MPI program. Ez akkor fog futni a számítási csomópontok hello többpéldányos feladat hello programot.
3. Hozzon létre egy zip fájlt tartalmazó `MPIHelloWorld.exe` (melyet beépített 2. lépés) és `MSMpiSetup.exe` (amely letöltött 1. lépés). A zip-fájl hello következő lépésben egy alkalmazás csomag fogja feltölteni.
4. Használjon hello [Azure-portálon] [ portal] toocreate egy kötegelt [alkalmazás](batch-application-packages.md) "MPIHelloWorld" nevezik, és adja meg az "1.0" verzióként hello előző lépésben létrehozott hello zip-fájl hello alkalmazáscsomagot. Lásd: [alkalmazások kezelését és feltöltését](batch-application-packages.md#upload-and-manage-applications) további információt.

> [!TIP]
> Build egy *kiadás* verziójának `MPIHelloWorld.exe` , hogy nem rendelkezik tooinclude további függőségek (például `msvcp140d.dll` vagy `vcruntime140d.dll`) a alkalmazáscsomagban.
>
>

### <a name="execution"></a>Végrehajtás
1. Töltse le a hello [azure-köteg-minták] [ github_samples_zip] a Githubról.
2. Nyissa meg hello MultiInstanceTasks **megoldás** a Visual Studio 2015-öt vagy újabb. Hello `MultiInstanceTasks.sln` megoldásfájlt található:

    `azure-batch-samples\CSharp\ArticleProjects\MultiInstanceTasks\`
3. Adja meg a kötegelt és a tároló hitelesítő adatait a `AccountSettings.settings` a hello **Microsoft.Azure.Batch.Samples.Common** projekt.
4. **Létrehozása és futtatása** hello MultiInstanceTasks megoldás tooexecute hello MPI mintaalkalmazást a számítási csomópontok a Batch-készlet.
5. *Nem kötelező*: használata hello [Azure-portálon] [ portal] vagy hello [kötegelt Explorer] [ batch_explorer] tooexamine hello minta készlet, feladat, és a feladat ("MultiInstanceSamplePool", "MultiInstanceSampleJob", "MultiInstanceSampleTask") előtt hello erőforrások törlése.

> [!TIP]
> Letöltheti a [Visual Studio Community] [ visual_studio] szabad, ha nem rendelkezik a Visual Studio.
>
>

A kimeneti `MultiInstanceTasks.exe` hasonló toohello következő:

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

## <a name="next-steps"></a>Következő lépések
* ismerteti, amelyek hello Microsoft HPC & Azure Batch csapat blogja [MPI támogatja az Azure Batch Linux][blog_mpi_linux], és használatával kapcsolatos tartalmaz [OpenFOAM] [ openfoam] kötegelt. Python-Kódminták találhat meg hello [OpenFOAM példa a Githubon][github_mpi].
* Ismerje meg, hogyan túl[létrehozása Linux számítási csomópontok készleteinek](batch-linux-nodes.md) használható az Azure Batch MPI megoldások.

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
