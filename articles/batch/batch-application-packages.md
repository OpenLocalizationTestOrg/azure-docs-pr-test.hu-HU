---
title: "a számítási csomópontok - Azure Batch aaaInstall alkalmazáscsomagok |} Microsoft Docs"
description: "Az Azure Batch tooeasily használata hello alkalmazás csomagok szolgáltatása több alkalmazás kezelése és a kötegelt telepítés verziók számítási csomópontjain."
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
ms.assetid: 3b6044b7-5f65-4a27-9d43-71e1863d16cf
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 07/20/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 683be7b7f1bd5db7835332016f6dccb72f45c3b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-applications-toocompute-nodes-with-batch-application-packages"></a>Alkalmazások toocompute csomópontokat a kötegelt kérelem csomagok központi telepítése

hello alkalmazás csomagok szolgáltatás Azure-köteg feladat alkalmazások könnyen kezelhető, és a központi telepítés toohello számítási csomópontok a készlet. Az alkalmazáscsomagok töltse fel, és a feladatok futnak, beleértve a segédfájlok hello alkalmazások több verziójának kezelése. Ezután automatikusan telepítheti egy vagy több ezen alkalmazások toohello számítási csomópontok a készlethez.

Ebből a cikkből megtudhatja, hogyan tooupload és hello Azure-portálon az alkalmazáscsomagok kezelése. Ezután megismerkedhet a hogyan tooinstall őket a készlet számítási csomópontokat a hello [Batch .NET] [ api_net] könyvtárban.

> [!NOTE]
> 
> Az alkalmazáscsomagok az összes 2017. július 5. után létrehozott Batch-készleten támogatottak. 2016. március 10. és 5. július 2017 között létrehozott, csak akkor, ha hello készlet lett létrehozva egy felhőalapú szolgáltatás konfigurációja kötegelt készletek azok támogatottak. Kötegelt készletek létrehozott előzetes too10 2016. március nem támogatják az alkalmazáscsomagok.
>
> hello API-k létrehozására és kezelésére alkalmazáscsomagok részét képező hello [Batch Management .NET kódtárral] [[api_net_mgmt]] könyvtár. hello API-k adott számítási csomóponton alkalmazáscsomagok telepítésének részét képezik hello [Batch .NET] [ api_net] könyvtárban.  
>
> itt leírt hello alkalmazás csomagok szolgáltatás felülír hello Batch-alkalmazások szolgáltatás hello szolgáltatás korábbi verzióiban érhető el.
> 
> 

## <a name="application-package-requirements"></a>Csomag alkalmazáskövetelmények
toouse, meg kell túl[egy Azure Storage-fiók csatolása](#link-a-storage-account) tooyour Batch-fiókhoz.

Ez a szolgáltatás bemutatott [Batch REST API] [ api_rest] 2015-12-01.2.2 verziója, a megfelelő hello [Batch .NET] [ api_net] könyvtárverzió 3.1.0. Javasoljuk, hogy mindig használjon hello legújabb API-verzió az kötegelt használatakor.

> [!NOTE]
> Az alkalmazáscsomagok az összes 2017. július 5. után létrehozott Batch-készleten támogatottak. 2016. március 10. és 5. július 2017 között létrehozott, csak akkor, ha hello készlet lett létrehozva egy felhőalapú szolgáltatás konfigurációja kötegelt készletek azok támogatottak. Kötegelt készletek létrehozott előzetes too10 2016. március nem támogatják az alkalmazáscsomagok.
>
>

## <a name="about-applications-and-application-packages"></a>Alkalmazások és csomagok
Azure Batch belül egy *alkalmazás* tooa készlete, amelyek a készlet számítási csomópontjai automatikusan letöltött toohello lehetnek rendszerverzióval ellátott bináris hivatkozik. Egy *alkalmazáscsomag* tooa hivatkozik *meghatározott* e bináris fájljait és jelöli egy adott *verzió* hello alkalmazás.

![Magas szintű diagramját, alkalmazások és csomagok][1]

### <a name="applications"></a>Alkalmazások
Egy alkalmazás kötegben tartalmaz egy vagy több alkalmazás csomagokat, és adja meg a konfigurációs beállítások hello alkalmazás. Például egy alkalmazást adhat meg hello alapértelmezett alkalmazás csomag verziója tooinstall számítási csomópontokat, és hogy a csomagokat is frissíthető és nem törölhető.

### <a name="application-packages"></a>Alkalmazáscsomagok
Alkalmazáscsomag hello bináris alkalmazásfájlokat tartalmazó .zip fájl és a fájlokat, amelyek szükségesek a feladatok toorun hello alkalmazás. Minden alkalmazáscsomag hello alkalmazás adott verzióját jeleníti meg.

Alkalmazáscsomagok hello készlet és a feladat szintjén is megadhat. Egy készletet vagy feladat létrehozásakor megadhatja egy vagy több ezeket a csomagokat, és (opcionálisan) verzióval.

* **Tárolókészlet alkalmazáscsomagok** túl telepített*minden* csomópont hello készletben. Alkalmazások vannak telepítve, amikor a csomópont csatlakozik egy készletet, és amikor újraindították vagy a lemezképet.
  
    Készlet alkalmazáscsomagok megfelelőek, amikor egy alkalmazáskészlet összes csomópontjának hajtható végre egy feladat tevékenységeit. Megadhat egy vagy több alkalmazáscsomagok készletet hoz létre, és adja hozzá, vagy egy meglévő készlet-csomagok frissítése. Ha egy meglévő készlet alkalmazáscsomagok frissítéséhez újra kell indítani a csomópontok tooinstall hello új csomag.
* **Tevékenység-alkalmazáscsomagok** telepített csak tooa számítási csomópont ütemezett toorun egy feladatot, csak a hello feladat parancssor futtatása előtt. Ha meg van adva hello alkalmazáscsomag és verzió már hello csomóponton, nem újra telepíteni és hello meglévő csomag használata.
  
    A feladat alkalmazáscsomagok olyan hasznos megosztott készlet környezetekben, ahol különböző feladat futtatása egy készletet, és hello készlet nem törlődik, amikor a feladat befejezését. Ha a feladat csomópontok eltérő kevesebb feladatok hello készletben, feladat alkalmazáscsomagok minimálisra adatátvitel mivel az alkalmazás telepített csak toohello a csomópontokra, amelyeket a feladatok futtatásához.
  
    Más tevékenység alkalmazáscsomagok is előnyeit kihasználó forgatókönyvek, amelyek a nagyméretű alkalmazások futnak feladatok, de csak néhány feladatot. Például egy előre feldolgozásra szakaszban, vagy egy merge feladatra, ahol hello előzetes feldolgozás vagy egyesítési alkalmazás nehéz, előfordulhat, hogy előnyt az alkalmazáscsomagok feladat.

> [!IMPORTANT]
> Nincsenek korlátozások hello számára az alkalmazások és az alkalmazáscsomagok belül Batch-fiók és a hello maximális csomag mérete. Lásd: [kvótái és korlátai hello Azure Batch szolgáltatás](batch-quota-limit.md) kapcsolatos részletekért.
> 
> 

### <a name="benefits-of-application-packages"></a>Az alkalmazáscsomagok előnyei
Alkalmazáscsomagok egyszerűbbé teheti az hello kód a kötegelt megoldás és alacsonyabb hello általános szükséges toomanage hello alkalmazások, amelyek a feladatok futnak.

Az alkalmazáscsomagok a készlet kezdő tevékenység nem rendelkezik toospecify egyéni erőforrás fájlok tooinstall listája túl hosszú hello csomópontján. Az alkalmazásfájlokat az Azure Storage vagy a csomóponton több verziójának kezelése toomanually nincs. És nem kell talál az tooworry [SAS URL-címek](../storage/common/storage-dotnet-shared-access-signature-part-1.md) tooprovide toohello található fájlokat a tárfiók. A Batch-működik az Azure Storage toostore alkalmazáscsomagok hello háttérben, és azok toocompute csomópontok.

> [!NOTE] 
> hello teljes mérete a kezdő tevékenységre kell kisebb vagy egyenlő, mint too32768 karaktert, köztük Erőforrásfájlok és környezeti változókat. A kezdő tevékenység meghaladja ezt a korlátot, majd az alkalmazás-csomagok használata esetén egy másik lehetőséget. Is létrehozhat az erőforrás-fájlokat tartalmazó tömörített archívum létrehozása, töltse fel a blob tooAzure Storage, és ezután bontsa ki a parancssorból hello a kezdő tevékenység. 
>
>

## <a name="upload-and-manage-applications"></a>Alkalmazások kezelését és feltöltését
Használhatja a hello [Azure-portálon] [ portal] vagy hello [Batch Management .NET kódtárral](batch-management-dotnet.md) könyvtár toomanage hello alkalmazáscsomagok a Batch-fiók. A hello mellett néhány szakaszok, először megmutatjuk, hogyan toolink egy tárfiókot, majd a további alkalmazásokat ismertetik, és a csomagok és kezelnie azokat a hello portálon.

### <a name="link-a-storage-account"></a>A tárfiók csatolása
toouse alkalmazáscsomagok, először rendelnie egy Azure Storage-fiók tooyour Batch-fiókhoz. Ha még nincs konfigurálva egy tárfiókot, hello Azure-portálon jeleníti meg a figyelmeztető hello először hello kattint **alkalmazások** hello csempére **a Batch-fiók** panelen.

> [!IMPORTANT]
> Jelenleg a Batch-támogatja *csak* hello **általános célú** tárfióktípus 5, lépésben leírt [hozzon létre egy tárfiókot](../storage/common/storage-create-storage-account.md#create-a-storage-account), a [kapcsolatos Azure Storage-fiókok](../storage/common/storage-create-storage-account.md). Egy Azure Storage-fiók tooyour Batch-fiók csatolásához kapcsolja *csak* egy **általános célú** storage-fiók.
> 
> 

!["Nincs beállítva tárfiók" figyelmeztetés Azure-portálon][9]

hello Batch szolgáltatás által használt hello társított tárolási fiók toostore az alkalmazáscsomagok. Után a hozzá csatolt hello két fiókot, akkor kötegelt automatikusan kapcsolódó hello tárolási fiók tooyour számítási csomópontok tárolt hello csomagok központi telepítése. toolink a tárolási fiók tooyour Batch-fiókhoz, kattintson a **tárolási Fiókbeállítások** a hello **figyelmeztetés** panelt, és kattintson **Tárfiók** hello a**Tárfiók** panelen.

![Storage-fiók panelen válassza az Azure-portálon][10]

Azt javasoljuk, hogy hozzon létre egy tárfiókot *kifejezetten* a Batch-fiókhoz való használatra, és jelölje ki itt. Részletes információt toocreate egy tárfiókot, lásd: "Create a Storage-fiók" az [Azure Storage-fiókok](../storage/common/storage-create-storage-account.md). Miután létrehozott egy tárfiókot, majd társíthatja azt tooyour Batch-fiók hello segítségével **Tárfiók** panelen.

> [!WARNING]
> hello Batch szolgáltatás által használt Azure Storage toostore az alkalmazáscsomagok blokkblobként. Ön [szokásos módon felszámított] [ storage_pricing] hello blokk blob adatok. Lehet, hogy tooconsider hello méretét és az alkalmazáscsomagok számát, és bizonyos időközönként eltávolítja az elavult csomagok toominimize költségeket.
> 
> 

### <a name="view-current-applications"></a>Aktuális alkalmazások megtekintése
a Batch-fiók tooview hello alkalmazások kattintson hello **alkalmazások** menüpont hello bal oldali menüben hello megtekintésekor **a Batch-fiók** panelen.

![Alkalmazások csempe][2]

Ezzel a beállítással menü megnyitása hello **alkalmazások** panel:

![Alkalmazások listája][3]

Hello **alkalmazások** panelt jeleníti meg a fiók minden alkalmazás hello és hello következő tulajdonságai:

* **Csomagok**: hello jelen alkalmazáshoz rendelt verzióinak száma.
* **Alapértelmezett verzió**: hello alkalmazás verziója a Ha hello alkalmazás készlet megadásakor nem jelzi azt egy verzió telepítve. Ez a beállítás nem kötelező.
* **Frissítések engedélyezése**: hello érték, amely meghatározza, hogy frissíti a csomagot, törléseket és kiegészítéseit engedélyezettek. Ha a beállított érték túl**nem**, csomag frissítések és törlések hello alkalmazás le vannak tiltva. Csak új alkalmazáscsomag-verziók is hozzáadhatók. hello alapértelmezett érték a **Igen**.

### <a name="view-application-details"></a>Az alkalmazás részleteinek megtekintése
tooopen hello panel, amely tartalmazza az alkalmazás, jelölje be hello alkalmazás hello részletek hello **alkalmazások** panelen.

![Az alkalmazás részletei][4]

Hello alkalmazás részleteit megjelenítő panelen konfigurálhatja az alkalmazás beállításainak a következő hello.

* **Frissítések engedélyezése**: Adja meg, hogy az alkalmazáscsomagok is frissíthető és nem törölhető. Tekintse meg a "Frissíteni vagy törölni egy alkalmazáscsomagot" a cikk későbbi részében.
* **Alapértelmezett verzió**: Adjon meg egy alapértelmezett alkalmazás csomag toodeploy toocompute csomópontok.
* **Megjelenített név**: Adjon meg egy rövid nevet, amely a kötegelt megoldás használható tartalmáról hello alkalmazás adatait, például hello felhasználói felületén keresztül kötegelt tooyour ügyfelek biztosító szolgáltatás.

### <a name="add-a-new-application"></a>Új alkalmazás felvétele
toocreate egy új alkalmazást, vegye fel egy alkalmazáscsomagot, és adjon meg egy új, egyedi azonosítót. hello első alkalmazáscsomag felvételekor hello új application ID hello új alkalmazást hoz létre.

Kattintson a **Hozzáadás** a hello **alkalmazások** panel tooopen hello **új alkalmazás** panelen.

![Új alkalmazás panel az Azure-portálon][5]

Hello **új alkalmazás** panel biztosít hello következő mezői toospecify hello beállításait az új alkalmazás- és alkalmazáscsomagot.

**Alkalmazásazonosító**

Ez a mező az új alkalmazást, amely tulajdonos toohello szabványos Azure Kötegazonosító ellenőrzési szabályok hello Azonosítóját adja meg. biztosítani az Alkalmazásazonosító hello szabályok a következők:

* Windows-csomópont a hello azonosító az alfanumerikus karaktereket, kötőjeleket és aláhúzásjeleket tartalmazhat tetszőleges kombinációját tartalmazhatja. Linux-csomópont csak alfanumerikus karaktereket és aláhúzás karaktereket tartalmazhatnak engedélyezettek.
* Nem lehet hosszabb 64 karakternél.
* A Batch-fiókhoz hello belül egyedinek kell lennie.
* Egy nagybetűket és a nagybetűk között.

**Verzió**

Ez a mező határozza meg a feltölteni kívánt alkalmazáscsomag hello hello verzióját. Verzió értékek a következők: tulajdonos toohello ellenőrzési szabályok a következő:

* Windows csomópont a hello verzió-karakterláncnak a alfanumerikus karaktereket, kötőjeleket, aláhúzásjeleket és időszakok tetszőleges kombinációját tartalmazhatja. Linux csomópont hello verzió-karakterlánca csak alfanumerikus karaktereket és aláhúzásjeleket tartalmazhat.
* Nem lehet hosszabb 64 karakternél.
* Hello alkalmazáson belül egyedinek kell lennie.
* A rendszer megőrzi és a nagybetűk között.

**Alkalmazáscsomag**

Ez a mező hello bináris alkalmazásfájlokat tartalmazó hello .zip fájl és a fájlokat, amelyek a szükséges tooexecute hello alkalmazás adja meg. Kattintson a hello **válasszon ki egy fájlt** mező vagy hello mappa ikon toobrowse tooand, válassza ki az alkalmazás fájlokat tartalmazó .zip-fájlt.

Miután kijelölt egy fájlt, kattintson **OK** toobegin hello feltöltés tooAzure tároló. Hello feltöltési művelet befejeződése után hello portál egy értesítést jelenít meg, és hello panel bezárása után. Attól függően, hogy-e a hálózati kapcsolat sebességétől feltöltését és hello hello fájl hello méretét Ez a művelet eltarthat egy ideig.

> [!WARNING]
> Ne zárja be az hello **új alkalmazás** panel hello feltöltési művelet befejezése előtt. Ezzel megszűnik a hello feltöltési folyamat.
> 
> 

### <a name="add-a-new-application-package"></a>Új alkalmazás csomag hozzáadása
tooadd alkalmazás új csomagverziójának egy meglévő alkalmazáshoz, válasszon ki egy alkalmazást hello **alkalmazások** panelen kattintson **csomagok**, majd kattintson a **Hozzáadás** tooopen Hello **Hozzáadás csomag** panelen.

![Adja hozzá az alkalmazás csomag panel Azure-portálon][8]

Ahogy látja, hello mezők egyeznek hello **új alkalmazás** panelen, de hello **alkalmazásazonosító** mezőben le van tiltva. Új alkalmazás hello hasonló módon adja meg a hello **verzió** az új csomag tallózással tooyour **alkalmazáscsomag** .zip fájlt, majd kattintson az **OK** tooupload hello a csomag.

### <a name="update-or-delete-an-application-package"></a>Frissítés vagy törlés alkalmazáscsomag
tooupdate vagy törölje a meglévő alkalmazáscsomag, nyissa meg hello részleteit megjelenítő panelen hello alkalmazáshoz, kattintson **csomagok** tooopen hello **csomagok** panelen kattintson a hello **három pont**hello alkalmazáscsomagot, hogy szeretné, hogy toomodify, és válassza ki a megjeleníteni kívánt tooperform hello művelet hello sorában.

![Frissítés vagy törlés csomag Azure-portálon][7]

**Update**

Amikor rákattint **frissítés**, hello *csomag* panel jelenik meg. Ezen a panelen hasonló toohello *új alkalmazáscsomag* panelen, azonban csak hello csomag kiválasztási mezőben engedélyezve van, lehetővé teszi egy új ZIP-fájl tooupload toospecify.

![Frissítési csomag panel az Azure-portálon][11]

**Törlés**

Amikor rákattint **törlése**, a rendszer felkéri tooconfirm hello törlésének hello Csomagverzió és kötegelt hello csomag törli az Azure Storage-ból. Ha törli egy alkalmazás alapértelmezett verzióját hello, hello **alapértelmezett verzió** beállítás hello alkalmazás eltávolítása.

![Alkalmazás törlése][12]

## <a name="install-applications-on-compute-nodes"></a>Alkalmazások telepítése a számítási csomópontok
Most, hogy megismerte a hogyan toomanage alkalmazás Azure-portálon hello csomagok, tárgyaljuk is hogyan toodeploy őket toocompute csomópontok és kötegelt feladatok futtathatók.

### <a name="install-pool-application-packages"></a>Készlet alkalmazáscsomagok telepítése
egy alkalmazáscsomag összes tooinstall számítási csomópontjain a készletben, adjon meg egy vagy több alkalmazáscsomagot *hivatkozások* hello készlet. Ha a csomópont csatlakozik hello alkalmazáskészlet, illetve ha hello csomópont újraindítása után, vagy lemezképet hello csomagok számára egy készlet megadott minden számítási csomópont telepítése.

A Batch .NET, adjon meg egy vagy több [CloudPool][net_cloudpool].[ ApplicationPackageReferences] [ net_cloudpool_pkgref] amikor létrehoz egy új készletet, vagy egy meglévő készlet. Hello [ApplicationPackageReference] [ net_pkgref] osztály megadja egy alkalmazás-Azonosítót és tooinstall a készlet számítási csomópontjain verziót.

```csharp
// Create hello unbound CloudPool
CloudPool myCloudPool =
    batchClient.PoolOperations.CreatePool(
        poolId: "myPool",
        targetDedicatedComputeNodes: 1,
        virtualMachineSize: "small",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));

// Specify hello application and version tooinstall on hello compute nodes
myCloudPool.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference {
        ApplicationId = "litware",
        Version = "1.1001.2b" }
};

// Commit hello pool so that it's created in hello Batch service. As hello nodes join
// hello pool, hello specified application package is installed on each.
await myCloudPool.CommitAsync();
```

> [!IMPORTANT]
> Az alkalmazás csomag központi telepítése a bármilyen okból nem sikerül, ha hello Batch szolgáltatás jelek hello csomópont [használhatatlanná][net_nodestate], és nincsenek feladatok vannak ütemezve ezen a csomóponton. Ebben az esetben kell **indítsa újra a** hello csomópont tooreinitiate hello package deployment mappában. Újraindítás hello csomópont is lehetővé teszi, hogy a feladatütemezés ismét hello csomóponton.
> 
> 

### <a name="install-task-application-packages"></a>A feladat alkalmazáscsomagok telepítése
Hasonló tooa készlet meg alkalmazáscsomagot *hivatkozások* feladathoz. Ha a feladat ütemezett toorun egy csomóponton, hello csomag letöltése és kicsomagolása, csak a parancssor hello feladat végrehajtása előtt. Ha a megadott csomag és verzió hello csomóponton már van telepítve, hello csomag letöltése nem történik meg, és hello meglévő csomag használata.

a feladat alkalmazáscsomagok tooinstall konfigurálása hello feladat [CloudTask][net_cloudtask].[ ApplicationPackageReferences] [ net_cloudtask_pkgref] tulajdonság:

```csharp
CloudTask task =
    new CloudTask(
        "litwaretask001",
        "cmd /c %AZ_BATCH_APP_PACKAGE_LITWARE%\\litware.exe -args -here");

task.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference
    {
        ApplicationId = "litware",
        Version = "1.1001.2b"
    }
};
```

## <a name="execute-hello-installed-applications"></a>Hello telepített alkalmazások végrehajtása
hello csomagok egy készletet vagy feladathoz megadott vannak letöltött és kibontott nevű könyvtár belüli hello tooa `AZ_BATCH_ROOT_DIR` hello csomópont. Kötegelt is létrehoz egy környezeti változó, amely tartalmazza a hello elérési toohello nevű könyvtár. A feladat parancssorokat e környezeti változó használatával való hivatkozáskor hello alkalmazás hello csomóponton. 

Windows csomópontján hello változó hello formátuma a következő szerepel:

```
Windows:
AZ_BATCH_APP_PACKAGE_APPLICATIONID#version
```

Linux-csomópont hello formátuma némileg eltérő. A pontok (.), kötőjelet (-) és a kettős kereszttel (#) is egybesimított toounderscores hello környezeti változóban. Példa:

```
Linux:
AZ_BATCH_APP_PACKAGE_APPLICATIONID_version
```

`APPLICATIONID`és `version` értékek, amelyek megfelelnek a megadott központi telepítés toohello alkalmazás és csomag verziója. Például, ha a megadott alkalmazás 2.7-es verzió *keverőgép* telepítendő Windows csomópont, a feladat parancssorokat szeretné használni a környezeti változó tooaccess fájlokat:

```
Windows:
AZ_BATCH_APP_PACKAGE_BLENDER#2.7
```

Linux-csomópont adja meg a hello környezeti változó a következő formátumban:

```
Linux:
AZ_BATCH_APP_PACKAGE_BLENDER_2_7
``` 

Ha feltölt egy alkalmazáscsomagot, megadhat egy alapértelmezett verzió toodeploy tooyour számítási csomópontjain. Ha egy alkalmazás alapértelmezett verziót adott meg, hello verzió utótag kihagyhatja, ha hello alkalmazás hivatkozik. Megadhat hello alapértelmezett Alkalmazásverzió hello Azure-portálon, a hello alkalmazások panelen látható módon [alkalmazások kezelését és feltöltését](#upload-and-manage-applications).

Például, ha állítja be "2.7" hello alapértelmezett verzió az alkalmazáshoz *keverőgép*, és a feladatok a következő környezeti változó hello hivatkozzon, majd a Windows-csomópontok végrehajtja a 2.7-es verzió:

`AZ_BATCH_APP_PACKAGE_BLENDER`

hello következő kódrészletet látható egy példa a feladat parancssori hello alapértelmezett verziója hello indító *keverőgép* alkalmazás:

```csharp
string taskId = "blendertask01";
string commandLine =
    @"cmd /c %AZ_BATCH_APP_PACKAGE_BLENDER%\blender.exe -args -here";
CloudTask blenderTask = new CloudTask(taskId, commandLine);
```

> [!TIP]
> Lásd: [környezeti beállítások feladatok](batch-api-basics.md#environment-settings-for-tasks) a hello [Batch funkcióinak áttekintése](batch-api-basics.md) számítási csomópont környezet beállításaival kapcsolatos további információt.
> 
> 

## <a name="update-a-pools-application-packages"></a>Készlet alkalmazáscsomagjainak frissítése
Ha egy meglévő készlet már be van állítva egy alkalmazási csomaggal rendelkező, hello készlet új csomagot is megadhat. Ha megadja az új csomag leírását, a készletbe, a következő apply hello:

* hello Batch szolgáltatás telepítheti hello újonnan meghatározott csomag, az összes új csomópont, amelyhez csatlakozni hello alkalmazáskészlet, illetve bármely létező csomópontján újraindították vagy lemezképet.
* A számítási csomópontokat, amelyek már hello készletben hello csomaghivatkozásokhoz frissítésekor nem telepíti automatikusan hello új alkalmazáscsomagot. Ezek a számítási csomópontok újra kell indítani, vagy azon tooreceive hello új csomag.
* Új csomag telepítésekor a környezeti változók létrehozott hello hello új alkalmazás csomaghivatkozásokhoz tükrözik.

Ebben a példában a meglévő készlet hello rendelkezik hello 2.7-es verziójának *keverőgép* egyik konfigurált alkalmazás a [CloudPool][net_cloudpool].[ ApplicationPackageReferences][net_cloudpool_pkgref]. tooupdate hello készlet csomópontok verziójával 2.76b, adjon meg egy új [ApplicationPackageReference] [ net_pkgref] hello új verziója, és a véglegesítési hello módosítása.

```csharp
string newVersion = "2.76b";
CloudPool boundPool = await batchClient.PoolOperations.GetPoolAsync("myPool");
boundPool.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference {
        ApplicationId = "blender",
        Version = newVersion }
};
await boundPool.CommitAsync();
```

Most, hogy hello új verzióra van beállítva, hello Batch-szolgáltatás telepítése verzió 2.76b tooany *új* csomópont, amelyhez csatlakozik hello készlet. tooinstall 2.76b csomópontokon hello *már* hello készletben, indítsa újra, vagy újból lemezképet létrehozni őket. Vegye figyelembe, hogy újraindított csomópontok megőrzi-e a csomag korábbi telepítések hello fájlokat.

## <a name="list-hello-applications-in-a-batch-account"></a>Hello alkalmazások listáján a Batch-fiók
Hello segítségével is listázhatja hello alkalmazások és a Batch-fiók csomagok [ApplicationOperations][net_appops].[ ListApplicationSummaries] [ net_appops_listappsummaries] metódust.

```csharp
// List hello applications and their application packages in hello Batch account.
List<ApplicationSummary> applications = await batchClient.ApplicationOperations.ListApplicationSummaries().ToListAsync();
foreach (ApplicationSummary app in applications)
{
    Console.WriteLine("ID: {0} | Display Name: {1}", app.Id, app.DisplayName);

    foreach (string version in app.Versions)
    {
        Console.WriteLine("  {0}", version);
    }
}
```

## <a name="wrap-up"></a>Burkolja
Az alkalmazáscsomagok esetén az ügyfelek arra, hogy hello alkalmazások kiválasztása, és adja meg a hello pontos verziót toouse, ha engedélyezve van a szolgáltatással a feladatok feldolgozásában, segítségével. Előfordulhat, hogy hello lehetőséget biztosít az ügyfelek tooupload, és nyomon követheti a saját alkalmazásaikat a szolgáltatásban.

## <a name="next-steps"></a>Következő lépések
* Hello [Batch REST API] [ api_rest] is biztosít támogatást toowork alkalmazáscsomagok. Lásd például: hello [applicationPackageReferences] [ rest_add_pool_with_packages] elemében [tooan alkalmazáskészlet-fiók hozzáadása] [ rest_add_pool] további információ toospecify csomagok tooinstall hello REST API használatával. Lásd: [alkalmazások] [ rest_applications] hogyan tooobtain alkalmazással kapcsolatos információk segítségével hello Batch REST API vonatkozó további információért.
* Megtudhatja, hogyan tooprogrammatically [kezelése az Azure Batch fiókjainak és kvótáinak a Batch Management .NET kódtárral](batch-management-dotnet.md). Hello [Batch Management .NET kódtárral][api_net_mgmt] könyvtár engedélyezheti a fiók létrehozását és törlését funkciói a kötegelt alkalmazást vagy szolgáltatást.

[api_net]: https://docs.microsoft.com/dotnet/api/overview/azure/batch/client?view=azure-dotnet
[api_net_mgmt]: https://docs.microsoft.com/dotnet/api/overview/azure/batch/management?view=azure-dotnet
[api_rest]: https://docs.microsoft.com/en-us/rest/api/batchservice/
[batch_mgmt_nuget]: https://www.nuget.org/packages/Microsoft.Azure.Management.Batch/
[github_samples]: https://github.com/Azure/azure-batch-samples
[storage_pricing]: https://azure.microsoft.com/pricing/details/storage/
[net_appops]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationoperations.aspx
[net_appops_listappsummaries]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationoperations.listapplicationsummaries.aspx
[net_cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_cloudpool_pkgref]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.applicationpackagereferences.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudtask.aspx
[net_cloudtask_pkgref]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudtask.applicationpackagereferences.aspx
[net_nodestate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.state.aspx
[net_pkgref]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationpackagereference.aspx
[portal]: https://portal.azure.com
[rest_applications]: https://msdn.microsoft.com/library/azure/mt643945.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[rest_add_pool_with_packages]: https://msdn.microsoft.com/library/azure/dn820174.aspx#bk_apkgreference

[1]: ./media/batch-application-packages/app_pkg_01.png "Csomagok magas szintű diagramja"
[2]: ./media/batch-application-packages/app_pkg_02.png "Alkalmazások csempe az Azure portálon"
[3]: ./media/batch-application-packages/app_pkg_03.png "Alkalmazások panel az Azure-portálon"
[4]: ./media/batch-application-packages/app_pkg_04.png "Alkalmazás részletei panel az Azure-portálon"
[5]: ./media/batch-application-packages/app_pkg_05.png "Új alkalmazás panel az Azure-portálon"
[7]: ./media/batch-application-packages/app_pkg_07.png "Frissítés vagy törlés csomagok legördülő Azure-portálon"
[8]: ./media/batch-application-packages/app_pkg_08.png "Új alkalmazás csomag panel az Azure-portálon"
[9]: ./media/batch-application-packages/app_pkg_09.png "Csatolt tárolási fiók riasztás"
[10]: ./media/batch-application-packages/app_pkg_10.png "Storage-fiók panelen válassza az Azure-portálon"
[11]: ./media/batch-application-packages/app_pkg_11.png "Frissítési csomag panel az Azure-portálon"
[12]: ./media/batch-application-packages/app_pkg_12.png "Törlési megerősítés csomag Azure-portálon"
