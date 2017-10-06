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
# <a name="create-task-dependencies-toorun-tasks-that-depend-on-other-tasks"></a>A feladat függőségek függenek más feladatok toorun végrehajtó tevékenységek létrehozása

Adhat meg a feladat függőségek toorun feladat vagy feladatokhoz csak egy szülő feladat befejeződése után. Bizonyos esetekben, ahol feladat függőségek hasznosak a következők:

* MapReduce-stílusú munkaterhelések hello felhőben.
* Feladatok, amelyek adatfeldolgozási feladatok irányított aciklikus diagramhoz (DAG) jelöl.
* Megjelenítés előtti és utáni megjelenítési folyamatok, ahol minden feladatot kell végeznie hello következő feladata megkezdése előtt.
* Ahol alárendelt feladatok függ hello kimeneti a felsőbb rétegbeli feladatok másik feladat.

Kötegelt feladat függőségek egy vagy több szülő feladatok hello befejezése után a számítási csomópontok végrehajtásra ütemezett feladatok hozhat létre. Például létrehozhat egy feladatot jeleníti meg az egyes különálló, párhuzamos feladatok 3D film keretében. hello végső feladat – hello "merge feladat"--keretek megjelenítése a teljes movie hello hello összes keretek összevonása lett sikeresen tette.

Alapértelmezés szerint a függő feladatok ütemezése a végrehajtás csak hello szülő feladat sikeres befejeződése után. Adjon meg egy függőségi toooverride hello alapértelmezett viselkedését, és hello szülő feladat sikertelensége feladatok futtatásához. Lásd: hello [függőségi műveletek](#dependency-actions) című szakaszban talál információt.  

Létrehozhat egy az egyhez típusú vagy egy-a-többhöz kapcsolat más feladatok függő feladatok. Ahol feladat függ-e a tevékenységazonosítók adott tartományon belüli feladatok csoport hello megvalósításának tartomány függőséget is létrehozhat. Ilyen három alapvető forgatókönyv toocreate több-a-többhöz kapcsolatok kombinálhatja.

## <a name="task-dependencies-with-batch-net"></a>A Batch .NET tevékenység függőségei
Ez a cikk arról lesz szó hogyan tooconfigure feladat függőségek használatával hello [Batch .NET] [ net_msdn] könyvtárban. Először megmutatjuk, hogyan túl[feladatütemezés-függőség engedélyezéséhez](#enable-task-dependencies) a feladatokra és majd bemutatják, hogyan túl[feladat konfigurálása függőségekkel rendelkező](#create-dependent-tasks). Azt is leírják, hogyan toospecify egy függőségi művelet toorun függő feladatok hello szülő meghibásodásakor. Végezetül arról lesz szó hello [függőségi forgatókönyvek](#dependency-scenarios) , amely támogatja a kötegelt.

## <a name="enable-task-dependencies"></a>A feladat függőségek engedélyezése
a kötegelt kérelem toouse feladat függőségeit, először konfigurálnia kell hello feladat toouse feladat függőségek. A Batch .NET engedélyezni a [CloudJob] [ net_cloudjob] úgy, hogy a [UsesTaskDependencies] [ net_usestaskdependencies] tulajdonság túl`true`:

```csharp
CloudJob unboundJob = batchClient.JobOperations.CreateJob( "job001",
    new PoolInformation { PoolId = "pool001" });

// IMPORTANT: This is REQUIRED for using task dependencies.
unboundJob.UsesTaskDependencies = true;
```

Hello megelőző kódrészletet, a "batchClient" példánya: hello [BatchClient] [ net_batchclient] osztály.

## <a name="create-dependent-tasks"></a>Függő tevékenységek létrehozása
toocreate egy feladatot, amely egy vagy több szülő feladatok hello megvalósításának függ, is megadhat, amely hello feladat "függ a" hello más feladatok. A Batch .NET, konfigurálja a hello [CloudTask][net_cloudtask].[ DependsOn] [ net_dependson] hello példányának tulajdonság [TaskDependencies] [ net_taskdependencies] osztály:

```csharp
// Task 'Flowers' depends on completion of both 'Rain' and 'Sun'
// before it is run.
new CloudTask("Flowers", "cmd.exe /c echo Flowers")
{
    DependsOn = TaskDependencies.OnIds("Rain", "Sun")
},
```

A kódrészlet egy függő feladat tevékenység azonosítója "Virág" hoz létre. hello "Virág" feladat függ feladatok "Eső" és "Sun". "Virág" tevékenység csak a "Eső" és "Sun" sikeresen befejeződött feladatok után lesznek ütemezett toorun adott számítási csomóponton.

> [!NOTE]
> Egy feladat tekinthető toobe sikeresen befejeződött, a hello van **befejeződött** állapot és a **kilépési kód** van `0`. A Batch .NET, ez azt jelenti, hogy egy [CloudTask][net_cloudtask].[ Állapot] [ net_taskstate] tulajdonság értékének `Completed` és CloudTask tartozó hello [TaskExecutionInformation][net_taskexecutioninformation].[ ExitCode] [ net_exitcode] tulajdonság értéke `0`.
> 
> 

## <a name="dependency-scenarios"></a>A függőségi forgatókönyvek
Alapszintű feladat függőségi három forgatókönyv esetén használhatja az Azure Batch:-az-egyhez, egy-a-többhöz és Feladatazonosítót között függőség. Kombinált tooprovide egy negyedik forgatókönyv, több-a-többhöz is lehetnek.

| A forgatókönyv&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | Példa |  |
|:---:| --- | --- |
|  [-Az-egyhez](#one-to-one) |*taskB* függ *taskA* <p/> *taskB* végrehajtásra, amíg nem ütemezi *taskA* sikeresen befejeződött |![Ábra: a feladat egy az egyhez típusú függőség][1] |
|  [Egy-a-többhöz](#one-to-many) |A *taskC* a *taskA* és a *taskB* tevékenységtől is függ <p/> *taskC* végrehajtásra, amíg nem ütemezi *taskA* és *taskB* sikeresen befejeződött |![Ábra: egy-a-többhöz feladat függőség][2] |
|  [A feladat Azonosítótartományának kezdete](#task-id-range) |*taskD* feladatok számos függ <p/> *taskD* végrehajtásra, amíg hello feladatok-azonosítók nem ütemezi *1* keresztül *10* sikeresen befejeződött |![Ábra: Feladat azonosítója tartomány függőség][3] |

> [!TIP]
> Létrehozhat **több-a-többhöz** kapcsolatokat, például ha C, D, E, és F egyes feladatok függ-e A és b feladatok Ez akkor hasznos, ahol az alsóbb rétegbeli feladatok függ-e több felsőbb szintű tevékenység kimenete hello párhuzamos működésű előfeldolgozási forgatókönyvek például a.
> 
> Ebben a szakaszban szereplő példák hello, a függő fusson a feladat csak azután hello szülő feladat sikeresen befejeződik. Ez a viselkedés hello alapértelmezett viselkedését egy függő feladat. Egy függő feladat futtatása után egy szülő feladatot nem sikerül, egy függőségi toooverride hello alapértelmezett viselkedését megadásával lehetséges. Lásd: hello [függőségi műveletek](#dependency-actions) című szakaszban talál információt.

### <a name="one-to-one"></a>-Az-egyhez
Egy az egyhez kapcsolat, a feladat egy szülő tevékenység sikeres befejezésének hello függ. toocreate hello függőségi, adjon meg egy adott feladat azonosító toohello [TaskDependencies][net_taskdependencies].[ OnId] [ net_onid] statikus metódus amikor hello feltölti [DependsOn] [ net_dependson] tulajdonsága [CloudTask] [ net_cloudtask].

```csharp
// Task 'taskA' doesn't depend on any other tasks
new CloudTask("taskA", "cmd.exe /c echo taskA"),

// Task 'taskB' depends on completion of task 'taskA'
new CloudTask("taskB", "cmd.exe /c echo taskB")
{
    DependsOn = TaskDependencies.OnId("taskA")
},
```

### <a name="one-to-many"></a>Egy-a-többhöz
Egy-a-többhöz kapcsolat, a feladat hello befejezésekor, több szülő feladat függ. toocreate hello függőségi, adja meg a feladat azonosítók toohello gyűjteménye [TaskDependencies][net_taskdependencies].[ OnIds] [ net_onids] statikus metódus amikor hello feltölti [DependsOn] [ net_dependson] tulajdonsága [CloudTask] [ net_cloudtask].

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

### <a name="task-id-range"></a>A feladat Azonosítótartományának kezdete
A szülő feladatok széles függ, a feladat hello hello megvalósításának feladatok széles találhatók, amelynek azonosítók függ.
toocreate hello függőség hello először adja meg, és a legutóbbi feladat a hello tartomány toohello azonosítók [TaskDependencies][net_taskdependencies].[ OnIdRange] [ net_onidrange] statikus metódus amikor hello feltölti [DependsOn] [ net_dependson] tulajdonsága [CloudTask] [net_cloudtask].

> [!IMPORTANT]
> Tevékenység azonosítója címtartományok használatakor a függőségek hello hello közé azonosítókat *kell* egész értékek ábrázolásai karakterlánc lehet.
> 
> Minden tevékenység hello közé hello függőségi, meg kell felelnie a folyamat sikeres végrehajtása vagy a csatlakoztatott tooa függőségi művelet túl beállítása hibával befejezése**Satisfy**. Lásd: hello [függőségi műveletek](#dependency-actions) című szakaszban talál információt.
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

## <a name="dependency-actions"></a>A függőségi műveletek

Alapértelmezés szerint egy függő feladat vagy feladatokhoz fut csak egy szülő feladat sikeres befejeződése után. Bizonyos esetekben érdemes lehet toorun függő tevékenységek akkor is, ha hello szülő feladat sikertelen lesz. Hello alapértelmezett viselkedést felülírhatja egy függőségi művelet megadását. A függőség művelet határozza meg, hogy a függő feladatok jogosult toorun hello sikerességét vagy sikertelenségét hello Szülőtevékenység alapján. 

Tegyük fel, hogy a függő feladatok vár hello fölérendelt feladatok végrehajtása hello adatait. Hello felsőbb szintű tevékenység sikertelen lesz, ha hello függő feladat még lehet képes toorun régebbi adatok felhasználásával. Ebben az esetben egy függőségi művelet megadható hello függő tevékenység jogosult toorun annak ellenére, hogy hello Szülőtevékenység hello sikertelen.

A függőség művelet egy kilépési feltétel hello szülő feladat alapul. Megadhat egy függőségi művelet bármely hello a következő kilépési feltételeit; a .NET-hez, lásd: hello [ExitConditions] [ net_exitconditions] osztály további részletek:

- Ha egy előre feldolgozási hiba történik.
- Hiba akkor fordul elő, amikor egy fájl feltöltése. Ha hello feladat keresztül megadott kilépési kóddal kilép **exitCodes** vagy **exitCodeRanges**, majd észlel, a fájl feltöltése hiba, hello kilépési kód elsőbbséget élvez által meghatározott hello műveletet.
- Ha kilép, hello feladat hello által megadott kilépési kóddal **ExitCodes** tulajdonság.
- Ha kilép, hello feladat hello megadott tartományba esik kilépési kóddal **ExitCodeRanges** tulajdonság.
- hello alapértelmezett esetben, ha hello feladat kilép, nem definiált kilépési kóddal **ExitCodes** vagy **ExitCodeRanges**, vagy ha hello feladat egy előre feldolgozási hiba és hello kilép **PreProcessingError**  tulajdonsága nincs beállítva, vagy ha hello feladat meghiúsul a fájl feltöltése hiba és hello **FileUploadError** tulajdonsága nincs beállítva. 

a .NET, a set hello egy függőségi művelet toospecify [ExitOptions][net_exitoptions].[ DependencyAction] [ net_dependencyaction] hello kilépési feltétel tulajdonság. Hello **DependencyAction** tulajdonság szükséges egy vagy két értéket:

- A beállítás hello **DependencyAction** tulajdonság túl**Satisfy** azt jelzi, hogy a függő feladatok jogosult toorun Ha hello Szülőtevékenység megadott hiba miatt kilép.
- A beállítás hello **DependencyAction** tulajdonság túl**blokk** azt jelzi, hogy a függő feladatok nem jogosult toorun.

Alapértelmezés szerint hello hello **DependencyAction** tulajdonság **Satisfy** a kilépési kód: 0, és **blokk** minden más kilépési feltételeket.

hello következő kódrészletet beállítja hello **DependencyAction** tulajdonság a szülő feladathoz. Ha hello Szülőtevékenység előre feldolgozási hiba miatt kilép, vagy a hello megadott hibakódok, függő hello feladat le van tiltva. Ha hello Szülőtevékenység más nem nulla hiba miatt kilép, a hello függő tevékenység nem jogosult toorun.

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

## <a name="code-sample"></a>Kódminta
Hello [TaskDependencies] [ github_taskdependencies] mintaprojektet egyike hello [Azure Batch-Kódminták] [ github_samples] a Githubon. A Visual Studio megoldás mutatja be:

- Hogyan tooenable feladat olyan feladaton függőségi
- Hogyan toocreate feladatok, más feladatok függ
- Hogyan tooexecute a feladatokat a számítási csomópontok készlete.

## <a name="next-steps"></a>Következő lépések
### <a name="application-deployment"></a>Alkalmazás központi telepítése
Hello [alkalmazáscsomagok](batch-application-packages.md) kötegelt funkciójával egy tooboth telepítéséhez egyszerűen és hello alkalmazások, amelyek a feladatok végrehajtása a számítási csomópontok verziója.

### <a name="installing-applications-and-staging-data"></a>Alkalmazások telepítése és átmeneti adatokat
Lásd: [alkalmazás telepítését, és átmeneti adatokat a Batch számítási csomópontjain] [ forum_post] hello Azure Batch fórum való felkészülés a csomópontok toorun feladatok módszerek áttekintését. Írt egy hello Azure Batch csoport tagjai, vagy a feladás egy vagy több egy jó ismertetése a hello különböző módokon toocopy alkalmazásokkal, a feladat bemeneti adatok és egyéb fájlok tooyour számítási csomópontjain.

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
