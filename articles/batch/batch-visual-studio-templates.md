---
title: "Visual Studio projektsablonjai - Azure Batch megoldások készítésével aaaStart |} Microsoft Docs"
description: "Ismerje meg, hogyan Visual Studio projektsablonjai segítségével valósítja meg, és futtassa a számítási-igényes munkaterhelések Azure Batch."
services: batch
documentationcenter: .net
author: fayora
manager: timlt
editor: 
ms.assetid: 5e041ae2-25af-4882-a79e-3aa63c4bfb20
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 02/27/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a61c480ddc4dffd66c01220a137a3e852e39c338
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-visual-studio-project-templates-toojump-start-batch-solutions"></a>Visual Studio projekt sablonok toojump indítási kötegelt megoldások

Hello **Feladatkezelő** és **feladat processzor Visual Studio sablonok** kötegelt adja meg a kódot toohelp tooimplement, és futtassa a kötegben a számítási-igényes munkaterhelések hello legalább erőfeszítést. Ez a dokumentum ismerteti ezeket a sablonokat, és nyújt útmutatást toouse őket.

> [!IMPORTANT]
> Ez a cikk ismerteti, amelyek csak információk alkalmazható toothese két sablonok, és feltételezi, hogy Ön ismeri a hello Batch szolgáltatás és a kapcsolódó tooit főbb fogalmait:-készletek, a számítási csomópontok, feladatok és feladatokat, feladat manager-feladatokkal, környezeti változók és más vonatkozó információ. További információt a [alapjai az Azure Batch](batch-technical-overview.md), [Batch funkcióinak áttekintése a fejlesztők](batch-api-basics.md), és [hello Azure Batch könyvtár az első lépései a .NET-keretrendszerhez készült](batch-dotnet-get-started.md).
> 
> 

## <a name="high-level-overview"></a>Áttekintés
hello Job Manager eszköz és a feladat processzor sablonok lehet használt toocreate két hasznos összetevői:

* Egy feladat manager feladat, amely egy, amely képes felosztása a feladat több feladat is önállóan, párhuzamosan futó feladat elválasztó.
* Egy feladat processzor használt tooperform előtti és utáni feldolgozást egy alkalmazás parancs sora környékén használható.

Például movie megjelenítési esetben hello feladat elválasztó volna ikonná egy egyetlen movie feladat több száz vagy ezer a különböző feladatokhoz, amelyek feldolgozzák egyes keretek külön-külön kellene. Ennek megfelelően hello feladat processzor lenne meghívása hello alkalmazás és minden függő folyamatok szükséges toorender minden keret megjelenítése, valamint minden további műveleteket (például másolása megjelenítve hello keret tooa tárolási helyét).

> [!NOTE]
> hello Job Manager eszköz és a feladat processzor sablonok függetlenek egymástól, ezért is toouse mindegyikét, vagy csak az egyik, a számítási feladatok és preferenciák hello követelményeitől függően.
> 
> 

Ahogy az az alábbi ábrán hello, a számítási feladat által használt ezeket a sablonokat végighaladnia három fázisból áll:

1. hello Ügyfélkód (pl. alkalmazások, webes szolgáltatás, stb.) küldi el a feladat toohello Batch szolgáltatás Azure, adja meg a feladat manager feladat hello-manager programot.
2. hello Batch szolgáltatás hello manager feladat futtatja egy számítási csomóponton, és hello feladat elválasztó elindítja hello meghatározott feladat processzor feladatot, a különböző számítási csomópontokkal igény szerint hello paraméterek és hello feladat elválasztó kód specifikációk alapján.
3. hello feladat processzor feladatok egymástól függetlenül, párhuzamosan futnak, tooprocess hello bemeneti adatokat, és hozhat létre hello kimeneti adatokat.

![Hogyan kommunikál az Ügyfélkód hello Batch szolgáltatás ábrája][diagram01]

## <a name="prerequisites"></a>Előfeltételek
toouse hello kötegelt sablonok, szüksége lesz a következő hello:

* Egy számítógép, amelyen telepítve a Visual Studio 2015. Kötegelt sablonok jelenleg csak a Visual Studio 2015 esetében támogatott.
* hello kötegelt sablonok, amelyek érhetők el a hello [Visual Studio galériában] [ vs_gallery] Visual Studio kiterjesztéseket. Két módon tooget hello sablonokat:
  
  * Hello sablonok használatával hello telepítése **bővítmények és frissítések** párbeszédpanel a Visual Studio (további információkért lásd: [keresés és a Visual Studio-bővítmények használatával][vs_find_use_ext]). A hello **bővítmények és frissítések** keresés és a letöltési hello két bővítmény a következő párbeszédpanelen:
    
    * A feladat elválasztó Azure Batch Feladatkezelő
    * Az Azure Batch feladat processzor
  * Hello sablonok letöltésére hello online katalógusból a Visual Studio: [Projektsablonjai a Microsoft Azure Batch][vs_gallery_templates]
* Ha azt tervezi, hogy toouse hello [alkalmazáscsomagok](batch-application-packages.md) szolgáltatás toodeploy hello job manager eszköz és a feladat processzor toohello Batch számítási csomópontjain, toolink a tárolási fiók tooyour Batch-fiók szükséges.

## <a name="preparation"></a>Előkészítése
Azt javasoljuk, hogy a Feladatkezelő, valamint a feladat processzor tartalmazhat megoldás létrehozásával, mert ez teheti, hogy könnyebben tooshare kód a Feladatkezelőt és a feladat feldolgozó programok között. toocreate ebben az esetben kövesse az alábbi lépéseket:

1. Nyissa meg a Visual Studio, és válassza ki **fájl** > **új** > **projekt**.
2. A **sablonok**, bontsa ki a **egyéb projekttípusok**, kattintson **Visual Studio megoldás**, majd válassza ki **üres megoldás**.
3. Adjon meg egy nevet, amely leírja az ebben a megoldásban (pl. "LitwareBatchTaskPrograms") alkalmazás és hello célját.
4. toocreate hello új megoldás, kattintson a **OK**.

## <a name="job-manager-template"></a>Feladatkezelő sablon
hello Feladatkezelő sablon segítségével egy hajthat végre a következő műveletek hello manager feladat tooimplement:

* A feladatok felosztása több feladat.
* Küldje el azokat a kötegelt feladatok toorun.

> [!NOTE]
> Feladat manager feladatokkal kapcsolatos további információkért lásd: [Batch funkcióinak áttekintése a fejlesztők](batch-api-basics.md#job-manager-task).
> 
> 

### <a name="create-a-job-manager-using-hello-template"></a>A feladat-kezelőt hello sablon létrehozása
egy feladat manager toohello megoldás, amely korábban létrehozott tooadd kövesse az alábbi lépéseket:

1. Nyissa meg a meglévő megoldást a Visual Studio.
2. A Megoldáskezelőben kattintson a jobb gombbal a hello megoldásban kattintson **Hozzáadás** > **új projekt**.
3. A **Visual C#**, kattintson a **felhő**, és kattintson a **Azure Batch Job Manager használatát a feladat elválasztó**.
4. Adjon meg egy nevet, amely ismerteti az alkalmazást, és úgy azonosítja a projekt, hello job manager használatát (pl. "LitwareJobManager").
5. toocreate hello projektben kattintson **OK**.
6. Végezetül build hello projekt tooforce Visual Studio tooload minden hivatkozott NuGet-csomagok és, hogy a projekt hello tooverify érvényes módosításához megkezdése előtt.

### <a name="job-manager-template-files-and-their-purpose"></a>Feladatkezelő sablonfájlokat és célját
A projekt hello Feladatkezelő sablon létrehozásakor három fájlcsoportok kódot állít elő:

* hello fő programfájl (Program.cs). Hello program a belépési pont és a legfelső szintű kivételkezelést tartalmazza. Nem szabad általában toomodify ez szükséges.
* hello keretrendszer könyvtára. Ez tartalmazza a hello fájlok felelős hello "bolierplate" dolgozott hello feladat manager program – kicsomagolása paraméterek hozzáadásával toohello kötegelt feladatok stb. Nem megfelelően szükséges toomodify ezeket a fájlokat.
* hello feladat elválasztó fájlt (JobSplitter.cs). Ez azért, ahol az alkalmazás-specifikus logika vágását meghatározó egy feladatot a feladatok helyezzük el.

Természetesen szerint is felvehet további fájlok szükséges toosupport a feladat elválasztó kódot, attól függően, hogy a felosztás logika hello feladat hello összetettsége.

hello sablon is létrehoz a szabványos .NET project fájlok például .csproj fájlhoz, app.config, packages.config, stb.

Ez a szakasz többi hello hello más fájlokat és a kód szerkezete ismerteti, és minden egyes osztály funkciója ismerteti.

![A Visual Studio Megoldáskezelőben hello Feladatkezelő sablon megoldás][solution_explorer01]

**Keretrendszer fájlok**

* `Configuration.cs`: A feladat konfigurációs adatok, például kötegelt fiókadatok, csatolt tárfiók hitelesítő adatainak, feladat- és információk és a feladat paramétereit hello betöltését magában foglalja. Is hozzáférést biztosít a tooBatch által definiált környezeti változók (lásd a környezeti beállítások feladatokhoz hello kötegelt dokumentációjának) keresztül hello Configuration.EnvironmentVariable osztály.
* `IConfiguration.cs`: Kivonatok hello hello Configuration osztály végrehajtására, a egység tesztelése a feladatok elválasztó egy hamis vagy utánzatait konfigurációs objektuma használja.
* `JobManager.cs`: Frissítés hello feladat manager program hello összetevői. Felelős hello hello feladat elválasztó meghívása hello feladat elválasztó, inicializálása és hello feladat elválasztó toohello feladat küldő által visszaadott hello feladatok terjesztéséhez.
* `JobManagerException.cs`: Hiba hello feladat manager tooterminate igénylő jelöli. JobManagerException használt toowrap "várt" hibák, ahol a diagnosztikai információk lezárást részeként megadható.
* `TaskSubmitter.cs`: Ez az osztály hello feladat elválasztó toohello kötegelt által visszaadott felelős tooadding feladatok. hello JobManager osztály hello tevékenységsorozat történő hatékony, de időben történő hozzáadását toohello feladat kötegek összesíti, majd meghívja a TaskSubmitter.SubmitTasks a háttérszálon az egyes kötegekben.

**Feladat elválasztó**

`JobSplitter.cs`: Ez az osztály tartalmazza az alkalmazás-specifikus logika vágását meghatározó hello feladatot a feladatok. hello keretrendszer hív hello JobSplitter.Split metódus tooobtain feladatokat, melyekhez hozzáadja sorozatát toohello feladat hello módszerként adja vissza őket. Ez az osztály hello hol fogja tölteni a feladat hello logikát. Hello vegyes metódus tooreturn CloudTask példányok képviselő szeretné toopartition hello feladat hello feladatok sorozata megvalósításához.

**Standard szintű .NET parancssori project fájlok**

* `App.config`: Standard .NET alkalmazás konfigurációs fájljában.
* `Packages.config`: Standard NuGet csomag függőségi fájlt.
* `Program.cs`: Hello program a belépési pont és a legfelső szintű kivételkezelést tartalmazza.

### <a name="implementing-hello-job-splitter"></a>Végrehajtási hello feladat elválasztó
Hello Feladatkezelő sablonprojekt megnyitásakor hello projekt lesz hello JobSplitter.cs fájl nyissa meg alapértelmezettként. Ossza fel a számítási feladatok hello feladatainak logika hello Split() módszer megjelenítése az alábbi használatával hello valósíthatja meg:

```csharp
/// <summary>
/// Gets hello tasks into which toosplit hello job. This is where you inject
/// your application-specific logic for decomposing hello job into tasks.
///
/// hello job manager framework invokes hello Split method for you; you need
/// only tooimplement it, not toocall it yourself. Typically, your
/// implementation should return tasks lazily, for example using a C#
/// iterator and hello "yield return" statement; this allows tasks toobe added
/// and toostart running while splitting is still in progress.
/// </summary>
/// <returns>hello tasks toobe added toohello job. Tasks are added automatically
/// by hello job manager framework as they are returned by this method.</returns>
public IEnumerable<CloudTask> Split()
{
    // Your code for hello split logic goes here.
    int startFrame = Convert.ToInt32(_parameters["StartFrame"]);
    int endFrame = Convert.ToInt32(_parameters["EndFrame"]);

    for (int i = startFrame; i <= endFrame; i++)
    {
        yield return new CloudTask("myTask" + i, "cmd /c dir");
    }
}
```

> [!NOTE]
> hello feliratozva hello szakasz `Split()` metódus hello hello Feladatkezelő sablon kódot, amelyet Ön toomodify hozzáadásával hello logika toosplit a feladatok a különböző feladatok csak szakasza. Ha azt szeretné, hogy egy másik szakasz hello sablon toomodify, győződjön meg arról, kötegelt működése, a rendszer familiarized és hello néhány kipróbálásához [Batch-Kódminták][github_samples].
> 
> 

A Split() megvalósítás hozzáféréssel rendelkezik:

* feladat paramétereit keresztül hello hello `_parameters` mező.
* hello CloudJob objektumot képviselő hello feladat, keresztül hello `_job` mező.
* hello CloudTask objektumot képviselő hello manager feladata, keresztül hello `_jobManagerTask` mező.

A `Split()` megvalósítása nem kell közvetlenül tooadd feladatok toohello feladat. Ehelyett a kód CloudTask objektumok sorozatát kell visszaadnia, és ezek felveszi toohello feladat automatikusan hello feladat elválasztó meghívása hello keretrendszer osztályok által. Közös toouse C# tartozó iterátor (`yield return`) tooimplement feladat elválasztókat a beállítást, mivel ez lehetővé teszi, hogy fut a lehető leghamarabb hello feladatok toostart, ahelyett hogy számított összes feladatok toobe vár.

**A feladat elválasztó sikertelen**

Ha a feladat elválasztó hibát észlel, azt kell vagy:

* Hello C# használatával hello feladatütemezési leáll `yield break` utasítást, amelyben eset hello Feladatkezelő kezeli a program sikeres; vagy
* Kivételt jelez, amelyben eset hello Feladatkezelő számítanak, nem sikerült, és előfordulhat, hogy ismételhető attól függően, hogy hogyan hello ügyfél van konfigurálva).

Mindkét esetben hello feladat elválasztó már vissza olyan feladatokat, és hozzáadott toohello kötegelt jogosult toorun lesz. Ha nem szeretné a toohappen, majd meg a következőket teheti:

* Állítsa le a hello feladat hello feladat elválasztó visszatérése előtt
* Állítson össze hello feladat teljes gyűjteményhez való visszaküldés előtt. (Ez azt jelenti, hogy vissza egy `ICollection<CloudTask>` vagy `IList<CloudTask>` helyett a feladat felosztó használatával egy C# iterátor megvalósítása)
* Feladat függőségek toomake hello Feladatkezelő hello sikeres befejezését függő feladatok használata

**Feladat manager újrapróbálkozások**

Hello Feladatkezelő sikertelen lesz, ha azt hello Batch szolgáltatás hello ügyfél újrapróbálkozási beállításoktól függően előfordulhat, hogy ismételhető. Ez általában biztonságos, mert ha hello keretrendszer hozzáadja a feladatok toohello feladat, figyelmen kívül hagyja minden feladatot, amely már létezik. Azonban ha feladatok kiszámítása költséges, előfordulhat, hogy nem kívánja tooincur hello költségét újraszámításakor már hozzáadott toohello feladat; feladatok Ezzel szemben ha hello futtassa újra a van nem garantált toogenerate hello tevékenység ugyanazokat az azonosítókat ezután hello "figyelmen kívül az ismétlődő" viselkedés nem lesz indítsa. Ezekben az esetekben akkor tervezzen a feladat elválasztó toodetect hello munkáját, hogy már megtörtént, és ismétlődik, például egy CloudJob.ListTasks elvégzésével tooyield feladatok megkezdése előtt.

### <a name="exit-codes-and-exceptions-in-hello-job-manager-template"></a>Lépjen ki a kódok és kivételek hello Feladatkezelő sablonban
Olyan kilépési kódot és kivételeket egy mechanizmus toodetermine hello eredményeit a program fut, és segíthetnek tooidentify hello program végrehajtását hello problémákat. hello Feladatkezelő sablon hello kilépési kódot és a jelen szakaszban ismertetett kivételek valósítja meg.

A kezelő feladata hello Feladatkezelő sablonnal megvalósított visszatérhet három lehetséges kilépési kód:

| Kód | Leírás |
| --- | --- |
| 0 |hello Feladatkezelő sikeresen befejeződött. A feladat elválasztó kódot toocompletion lefutott, és minden feladat lettek hozzáadva toohello feladat. |
| 1 |hello manager feladat hello program "várt" részében kivételhiba miatt sikertelen. hello kivételhiba lefordított tooa JobManagerException diagnosztikai információkat, és ha lehetséges, kapcsolatos javaslatok hello hiba. |
| 2 |hello manager feladata egy "nem várt" kivétel miatt sikertelen volt. hello kivételhiba kimeneti naplózott toostandard, de hello Feladatkezelő nem tooadd további diagnosztikai vagy javítási adatokat. |

A feladat manager feladat sikertelen a hello esetben egyes feladatok továbbra is hozzáadható toohello szolgáltatás előtt hello hiba történt. Ezek a feladatok szokásos módon indul el. "Feladat elválasztó sikertelen" fent látható kód elérési út leírását.

Kivételek által visszaadott összes hello adatok stdout.txt és stderr.txt fájlok íródtak. További információkért lásd: [hiba kezelése](batch-api-basics.md#error-handling).

### <a name="client-considerations"></a>Az ügyfelek szempontjai
Ez a szakasz ismerteti a végrehajtása néhány ügyfélkövetelmények, a Feladatkezelő, ez a sablon alapján való meghívásakor. Lásd: [hogyan toopass paraméterek és a környezeti változókat hello Ügyfélkód](#pass-environment-settings) paraméterek-környezet beállításait, és leírását.

**Kötelező hitelesítő adatok**

Rendelés tooadd feladatok toohello Azure kötegelt hello manager feladata az Azure Batch-fiók URL-címe és a kulcs használatához szükséges. Át kell adnia ezeket a környezeti változók neve YOUR_BATCH_URL és YOUR_BATCH_KEY. Állíthatja be ezeket az hello Feladatkezelő tevékenység környezeti beállításokat. Például a C# ügyfél:

```csharp
job.JobManagerTask.EnvironmentSettings = new [] {
    new EnvironmentSetting("YOUR_BATCH_URL", "https://account.region.batch.azure.com"),
    new EnvironmentSetting("YOUR_BATCH_KEY", "{your_base64_encoded_account_key}"),
};
```
**Tároló hitelesítő adatait**

Általában hello ügyfélnek nincs szüksége tooprovide hello csatolt tárolási fiók hitelesítő adatait toohello manager feladat, mert a legtöbb feladatot kezelők nem szükséges tooexplicitly hozzáférés hello kapcsolódó tárfiók, és (b) hello kapcsolt tárfiókra gyakran biztosítja hello feladat általános környezet beállítása tooall feladatokat. Ha nem ad hello kapcsolódó tárfiók keresztül hello közös környezeti beállításokat, és hello Feladatkezelő szükséges toolinked tároló elérése, akkor meg kell adnia az kapcsolódó hello tárolási hitelesítő adatokat az alábbiak szerint:

```csharp
job.JobManagerTask.EnvironmentSettings = new [] {
    /* other environment settings */
    new EnvironmentSetting("LINKED_STORAGE_ACCOUNT", "{storageAccountName}"),
    new EnvironmentSetting("LINKED_STORAGE_KEY", "{storageAccountKey}"),
};
```

**Feladat manager feladatbeállítások**

hello ügyfél kell beállítania hello Feladatkezelő *killJobOnCompletion* túl jelzőt**hamis**.

Hello ügyfél tooset esetében általában biztonságosan *runExclusive* túl**hamis**.

hello ügyfélnek használnia kell a hello *resourceFiles* vagy *applicationPackageReferences* gyűjtemény toohave hello Feladatkezelő végrehajtható (és a szükséges DLL-ek) központi telepítése toohello számítási csomópont.

Alapértelmezés szerint hello Feladatkezelő nem úja sikertelen. Attól függően, hogy a feladat manager logika hello ügyfél azt szeretné, hogy tooenable újrapróbálkozások keresztül *megkötések*/*maxTaskRetryCount*.

**Feladatbeállítások**

Hello feladat elválasztó függőségekkel rendelkező feladatok bocsát ki, ha hello ügyfél hello feladat usesTaskDependencies tootrue be kell állítani.

Hello feladat elválasztó modell, az ügyfelek toowish tooadd feladatok toojobs feletti milyen hello feladat elválasztó hoz létre a szokatlan is. hello ügyfél ezért általában kitűzni hello feladat *onAllTasksComplete* túl**terminatejob**.

## <a name="task-processor-template"></a>Processzor sablon
Egy feladat processzor sablon tooimplement egy feladat processzor által elvégezhető műveletek a következő hello segítségével:

* Állítson be minden kötegelt feladat toorun által kért hello információkat.
* Minden kötegelt feladat által igényelt összes műveletek futtatása.
* A feladat kimenetének toopersistent tárolási mentéséhez.

Egy feladat processzor, de nem szükséges toorun feladatokat a kötegelt hello kulcs egy feladat processzor előnye biztosítja a burkoló tooimplement összes feladatütemezés végrehajtási műveletek egyetlen helyen. Például ha toorun hello környezetben minden egyes feladat több alkalmazás van szüksége, vagy ha szüksége toopersistent adattárolás toocopy befejezése után minden feladat.

hello feladat processzor által végzett hello műveletek lehet egyszerű vagy összetett, és annyi, vagy kevés a munkaterhelés szerint szükség szerint. Emellett egy feladat processzor az összes feladat műveletek alkalmazásával könnyen frissítése vagy is műveletek módosítások tooapplications vagy munkaterhelés követelmények alapján. Azonban bizonyos esetekben egy feladat processzor nem feltétlenül hello optimális megoldás megvalósítását, azt is hozzáadhat a szükségtelen összetettségét, például amikor fut, amely egy egyszerű parancssorból gyorsan indítható feladatok.

### <a name="create-a-task-processor-using-hello-template"></a>Hozzon létre egy hello sablonnal feladat processzor
egy feladat processzor toohello megoldás, amely korábban létrehozott tooadd kövesse az alábbi lépéseket:

1. Nyissa meg a meglévő megoldást a Visual Studio.
2. A Megoldáskezelőben kattintson a jobb gombbal a hello megoldásban kattintson **Hozzáadás**, és kattintson a **új projekt**.
3. A **Visual C#**, kattintson a **felhő**, és kattintson a **Azure kötegelt feladat processzor**.
4. Adjon meg egy nevet, amely ismerteti az alkalmazást, és azonosítja a projekt, hello (pl. a tevékenység processzor "LitwareTaskProcessor").
5. toocreate hello projektben kattintson **OK**.
6. Végezetül build hello projekt tooforce Visual Studio tooload minden hivatkozott NuGet-csomagok és, hogy a projekt hello tooverify érvényes módosításához megkezdése előtt.

### <a name="task-processor-template-files-and-their-purpose"></a>Feladat processzor sablonfájlokat és célját
A projekt hello processzor sablon létrehozásakor három fájlcsoportok kódot állít elő:

* hello fő programfájl (Program.cs). Hello program a belépési pont és a legfelső szintű kivételkezelést tartalmazza. Nem szabad általában toomodify ez szükséges.
* hello keretrendszer könyvtára. Ez tartalmazza a hello fájlok felelős hello "bolierplate" dolgozott hello feladat manager program – kicsomagolása paraméterek hozzáadásával toohello kötegelt feladatok stb. Nem megfelelően szükséges toomodify ezeket a fájlokat.
* hello feladat processzor fájl (TaskProcessor.cs). Ez az meg, ahová a az alkalmazás-specifikus logika egy feladat végrehajtása (általában meghívásával tooan létező végrehajtható fájl ki) a rendszer. Előtti és utáni feldolgozási kódok, például a további adatok letöltése vagy eredmény fájlok feltöltése is ide kerül.

Természetesen is hozzáadhat további fájlok szükséges toosupport, a processzor feladatkód, attól függően, hogy a felosztás logika hello feladat hello összetettsége.

hello sablon is létrehoz a szabványos .NET project fájlok például .csproj fájlhoz, app.config, packages.config, stb.

Ez a szakasz többi hello hello más fájlokat és a kód szerkezete ismerteti, és minden egyes osztály funkciója ismerteti.

![A Visual Studio Megoldáskezelőben hello feladat processzor sablon megoldás][solution_explorer02]

**Keretrendszer fájlok**

* `Configuration.cs`: A feladat konfigurációs adatok, például kötegelt fiókadatok, csatolt tárfiók hitelesítő adatainak, feladat- és információk és a feladat paramétereit hello betöltését magában foglalja. Is hozzáférést biztosít a tooBatch által definiált környezeti változók (lásd a környezeti beállítások feladatokhoz hello kötegelt dokumentációjának) keresztül hello Configuration.EnvironmentVariable osztály.
* `IConfiguration.cs`: Kivonatok hello hello Configuration osztály végrehajtására, a egység tesztelése a feladatok elválasztó egy hamis vagy utánzatait konfigurációs objektuma használja.
* `TaskProcessorException.cs`: Hiba hello feladat manager tooterminate igénylő jelöli. TaskProcessorException használt toowrap "várt" hibák, ahol a diagnosztikai információk lezárást részeként megadható.

**A feladat processzor**

* `TaskProcessor.cs`: Hello feladat fut. hello keretrendszer hello TaskProcessor.Run metódus meghívja. Ez az osztály hello hol fogja tölteni a feladat hello az alkalmazás-specifikus logika. Valósítsa meg hello Futtatás metódust:
  * Elemzése és a tevékenység paramétereket ellenőrzése
  * Hello parancssori összeállítása bármely külső program tooinvoke keresi
  * A diagnosztikai adatok lehet szükség a hibakereséshez
  * A folyamat a parancssor használatával
  * Várjon, amíg hello folyamat tooexit
  * Hello kilépési kód hello folyamat toodetermine rögzítését, ha ez sikeres vagy sikertelen volt
  * Mentse a kimeneti fájlokat, kívánt tookeep toopersistent tároló

**Standard szintű .NET parancssori project fájlok**

* `App.config`: Standard .NET alkalmazás konfigurációs fájljában.
* `Packages.config`: Standard NuGet csomag függőségi fájlt.
* `Program.cs`: Hello program a belépési pont és a legfelső szintű kivételkezelést tartalmazza.

## <a name="implementing-hello-task-processor"></a>Végrehajtási hello feladat processzor
Hello feladat processzor sablonprojekt megnyitásakor hello projekt lesz hello TaskProcessor.cs fájl nyissa meg alapértelmezettként. A számítási feladatok a hello feladatok futtatása hello logikát valósíthatja meg metódussal hello Run() alább látható:

```csharp
/// <summary>
/// Runs hello task processing logic. This is where you inject
/// your application-specific logic for decomposing hello job into tasks.
///
/// hello task processor framework invokes hello Run method for you; you need
/// only tooimplement it, not toocall it yourself. Typically, your
/// implementation will execute an external program (from resource files or
/// an application package), check hello exit code of that program and
/// save output files toopersistent storage.
/// </summary>
public async Task<int> Run()

{
    try
    {
        //Your code for hello task processor goes here.
        var command = $"compare {_parameters["Frame1"]} {_parameters["Frame2"]} compare.gif";
        using (var process = Process.Start($"cmd /c {command}"))
        {
            process.WaitForExit();
            var taskOutputStorage = new TaskOutputStorage(
            _configuration.StorageAccount,
            _configuration.JobId,
            _configuration.TaskId
            );
            await taskOutputStorage.SaveAsync(
            TaskOutputKind.TaskOutput,
            @"..\stdout.txt",
            @"stdout.txt"
            );
            return process.ExitCode;
        }
    }
    catch (Exception ex)
    {
        throw new TaskProcessorException(
        $"{ex.GetType().Name} exception in run task processor: {ex.Message}",
        ex
        );
    }
}
```
> [!NOTE]
> hello hello Run() metódus megjegyzésekkel ellátott szakaszában hello csak szakasza hello feladat processzor sablon kódot, amelyet Ön toomodify a számítási feladatok futtatása hello logika hello tevékenységek hozzáadásával. Ha azt szeretné, hogy egy másik szakasz hello sablon toomodify, adjon először megismerje a kötegelt működése hello kötegelt dokumentációja, illetve próbálhatja ki hello kötegelt mintakódok néhány által.
> 
> 

hello Run() módszer hello parancssor megnyitása, egy vagy több folyamatok, várakozik az összes folyamat toocomplete, hello eredmények mentése, és végül a kilépési kóddal vissza. hello Run() módszer, ahol a tevékenységek logika feldolgozási hello valósítja meg. hello feladat processzor keretrendszer hello Run() metódus hív meg; nem kell toocall azt saját maga.

A Run() megvalósítás hozzáféréssel rendelkezik:

* tevékenység-paraméterek, keresztül hello hello `_parameters` mező.
* feladat- és azonosítók keresztül hello hello `_jobId` és `_taskId` mezőket.
* hello feladat konfigurációja keresztül hello `_configuration` mező.

**A feladat sikertelen**

Hiba, esetén hello Run() metódus által egy kivétel kiváltása kiléphet, de ez hagyja hello felső szintű kivételkezelő hello feladat kilépési kód irányítását. Ha toocontrol hello kilépési kóddal kell, hogy a különböző típusú hiba, például képes megkülönböztetni, diagnosztikai célokra, vagy mert az egyes meghibásodási módok kell állítanunk hello feladat, míg mások nem kell-e, majd vissza kell hello Run() metódus kilép egy nem nulla kilépési kódot. Erre akkor van hello feladat kilépési kódot.

### <a name="exit-codes-and-exceptions-in-hello-task-processor-template"></a>Lépjen ki a kódok és kivételek hello feladat processzor sablonban
Olyan kilépési kódot és kivételeket egy mechanizmus toodetermine hello eredményeit a program fut, és segíthetnek hello program végrehajtását hello problémákat azonosítása. hello feladat processzor sablon hello kilépési kódot és a jelen szakaszban ismertetett kivételek valósítja meg.

Egy feladat processzor feladat hello feladat processzor sablonnal megvalósított visszatérhessen három lehetséges kilépési kód:

| Kód | Leírás |
| --- | --- |
| [Process.ExitCode][process_exitcode] |hello feladat processzor toocompletion futott. Vegye figyelembe, hogy ez nem feltétlenül jelenti azt, a meghívott hello program sikeres volt, – csak hello feladat processzorral sikeresen elindítva, és végre semmilyen utófeldolgozás kivételek nélkül. hello kilépési kód hello szerinti meghívott hello programtól függ, – általában kilépési kód 0 azt jelenti, hogy hello program sikeres volt, és bármely más kilépési kód azt jelenti, hogy hello programot nem sikerült. |
| 1 |hello feladat processzor hello program "várt" részében kivételhiba miatt sikertelen. hello kivételhiba lefordított tooa `TaskProcessorException` diagnosztikai információkat, és ha lehetséges, kapcsolatos javaslatok hello hiba. |
| 2 |hello feladat processzor "Váratlan" kivétel miatt sikertelen. hello kivételhiba kimeneti naplózott toostandard, de hello feladat processzor nem tooadd további diagnosztikai vagy javítási adatokat. |

> [!NOTE]
> Ha indításakor hello program kilépési kódokat 1 és 2 tooindicate hiba módot használ, a feladat processzor hibákat 1 és 2 kilépési kód használata nem egyértelmű. A feladat processzor kódok toodistinctive kilépési hibakódok hello kivétel esetekben hello Program.cs fájl szerkesztésével módosíthatja.
> 
> 

Kivételek által visszaadott összes hello adatok stdout.txt és stderr.txt fájlok íródtak. További információkért lásd: hiba kezelése, a hello dokumentációt.

### <a name="client-considerations"></a>Az ügyfelek szempontjai
**Tároló hitelesítő adatait**

Ha a feladat processzor használja az Azure blob storage toopersist kimenetek, például használatával hello fájl egyezmények segédkódtárba helyezni, akkor azt hozzá kell férnie túl*vagy* felhő tárfiók hitelesítő adatainak hello *vagy* egy blob tároló URL-címet, amely tartalmazza a közös hozzáférésű jogosultságkód (SAS). hello sablon közös környezeti változók hitelesítő adatokat biztosító támogatását is magában foglalja. Az ügyfél a következőképpen teljen hello tároló hitelesítő adatait:

```csharp
job.CommonEnvironmentSettings = new [] {
    new EnvironmentSetting("LINKED_STORAGE_ACCOUNT", "{storageAccountName}"),
    new EnvironmentSetting("LINKED_STORAGE_KEY", "{storageAccountKey}"),
};
```

hello tárfiók majd érhető el hello TaskProcessor osztály keresztül hello `_configuration.StorageAccount` tulajdonság.

A tároló SAS URL-cím toouse tetszés szerint is átadhatja ezt keresztül feladat közös környezeti beállításokat, de hello processzor sablon jelenleg nem tartalmaz ez beépített támogatása.

**Tárolás beállítása**

E hello ügyfél vagy a feladatot manager feladat létrehozása tárolókkal szükséges feladatok hello feladatok toohello feladat hozzáadása előtt ajánlott. Ez a kötelező SAS tároló URL-címet használja, ha ilyen URL-cím nem tartalmaz engedély toocreate hello tároló. Minden tevékenység toocall CloudBlobContainer.CreateIfNotExistsAsync gyakorló hello tároló menti, ajánlott akkor is, ha a tárfiók hitelesítő adatait, adja meg.

## <a name="pass-parameters-and-environment-variables"></a>Paraméterek és a környezeti változók
### <a name="pass-environment-settings"></a>Pass-környezet beállításaiban
Egy ügyfél továbbíthatja az információt toohello manager feladata a tesztkörnyezet beállításainak hello formájában. Ez az információ majd használni hello manager feladat által előállítása hello feladat processzor feladatok hello részeként futó számítási feladatok. Hello információk, amelyek átadhatók környezeti beállításokat, például a következők:

* Név és fiókkulcs tárfiókkulcsok
* Batch-fiók URL-címe
* Kötegelt fiókkulcs

Batch-szolgáltatás hello rendelkezik egy egyszerű módszer toopass környezet beállítások tooa manager feladat hello segítségével `EnvironmentSettings` tulajdonság [Microsoft.Azure.Batch.JobManagerTask][net_jobmanagertask].

Például tooget hello `BatchClient` példány környezeti változók hello ügyfélről code hello URL-címet, és megosztott kulcs hitelesítő adatai hello Batch-fiókhoz is át a Batch-fiókhoz. Hasonlóképpen tooaccess hello tárfiókja, amely toohello Batch-fiókhoz kapcsolódó, átadhatók hello a tárfiók neve vagy hello tárfiók hívóbetűjét, a környezeti változók.

### <a name="pass-parameters-toohello-job-manager-template"></a>Paraméterek toohello Feladatkezelő sablon fázis
Sok esetben hasznos toopass Feladatonkénti paraméterek toohello feladat manager feladat, vagy toocontrol hello folyamat felosztásával, vagy tooconfigure hello feladatok hello feladat. Ehhez a parameters.json nevű hello manager feladata az erőforrás-fájlként JSON-fájl feltöltésével. hello paraméterek tudja majd válnak elérhetővé hello `JobSplitter._parameters` hello Feladatkezelő sablon mezőbe.

> [!NOTE]
> hello beépített paraméter kezelő csak karakterlánc-karakterlánc szótárak támogatja. Ha azt szeretné, hogy toopass összetett JSON értékek paraméter értékeként, akkor lesz toopass ezek karakterláncként kell és hello feladat elválasztó elemzett őket, vagy módosítsa hello keretrendszer `Configuration.GetJobParameters` metódust.
> 
> 

### <a name="pass-parameters-toohello-task-processor-template"></a>Paraméterek toohello feladat processzor sablon fázis
Is átadhatja paraméterek tooindividual feladatok hello feladat processzor sablonnal megvalósítva. Ahogy hello feladat manager sablon, hello processzor sablon keresi nevű erőforrásfájl

Parameters.JSON, és ha találhatók, mint hello paraméterek szótár betölti azt. Többféle hogyan toopass paraméterek toohello feladat processzor feladatok beállításait:

* Hello feladatparaméter JSON felhasználhatja. Ez a módszer jól, ha hello csak paraméterei feladat kiterjedő (például a leképezési magassága és szélessége) is. toodo, egy CloudTask hello feladat elválasztó, létrehozásakor hozzáadása egy hivatkozás toohello parameters.json fájl objektum hello feladat manager feladat ResourceFiles (`JobSplitter._jobManagerTask.ResourceFiles`) toohello CloudTask ResourceFiles gyűjtemény.
* Készítése és feladatspecifikus parameters.json dokumentum feltöltése feladatok elválasztó végrehajtásának részeként, és hivatkozni, hogy a blob hello feladat erőforrás fájlok gyűjteményben. Ez azért szükséges, ha a különböző feladatok más paraméterekkel rendelkeznek. Például lehet egy 3D megjelenítés forgatókönyv, ahol hello keret index átadása toohello feladat paraméterként.

> [!NOTE]
> hello beépített paraméter kezelő csak karakterlánc-karakterlánc szótárak támogatja. Ha azt szeretné, hogy toopass összetett JSON értékek paraméter értékeként, akkor lesz toopass ezek karakterláncként kell és hello feladat processzor elemzett őket, vagy módosítsa hello keretrendszer `Configuration.GetTaskParameters` metódust.
> 
> 

## <a name="next-steps"></a>Következő lépések
### <a name="persist-job-and-task-output-tooazure-storage"></a>Feladat megmaradnak, és a kimeneti tooAzure tárolási feladat
Egy másik hasznos eszköz a kötegelt megoldás fejlesztési [Azure Batch fájl egyezmények][nuget_package]. A .NET osztálykönyvtár (jelenleg előzetes verzió) található Batch .NET-alkalmazások tooeasily tárolt, és a feladat kimenetének tooand lekérdezni az Azure Storage. [Azure Batch feladat- és kimeneti megőrzéséhez](batch-task-output.md) hello szalagtár és a használati ismertetését tartalmazza.

### <a name="batch-forum"></a>Batch fórum
Hello [Azure Batch fórum] [ forum] MSDN nagyszerű toodiscuss kötegelt helyezze, és hello szolgáltatás kérdése. Központi a keresztül a hasznos "kapcsolódó" bejegyzések, és a kötegelt megoldások létrehozása során felmerülő kérdéseit.

[forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[net_jobmanagertask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobmanagertask.aspx
[github_samples]: https://github.com/Azure/azure-batch-samples
[nuget_package]: https://www.nuget.org/packages/Microsoft.Azure.Batch.Conventions.Files
[process_exitcode]: https://msdn.microsoft.com/library/system.diagnostics.process.exitcode.aspx
[vs_gallery]: https://visualstudiogallery.msdn.microsoft.com/
[vs_gallery_templates]: https://go.microsoft.com/fwlink/?linkid=820714
[vs_find_use_ext]: https://msdn.microsoft.com/library/dd293638.aspx

[diagram01]: ./media/batch-visual-studio-templates/diagram01.png
[solution_explorer01]: ./media/batch-visual-studio-templates/solution_explorer01.png
[solution_explorer02]: ./media/batch-visual-studio-templates/solution_explorer02.png
