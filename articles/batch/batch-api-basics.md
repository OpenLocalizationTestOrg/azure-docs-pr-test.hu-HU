---
title: "a fejlesztők számára az Azure Batch aaaOverview |} Microsoft Docs"
description: "Ismerje meg, hogy hello Batch szolgáltatás és az API-k fejlesztési szempontból hello előnyeit."
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
ms.assetid: 416b95f8-2d7b-4111-8012-679b0f60d204
ms.service: batch
ms.devlang: multiple
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-compute
ms.date: 06/28/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3da9d82572b28d05d1923166bd08c26725ca3316
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="develop-large-scale-parallel-compute-solutions-with-batch"></a>Nagy léptékű párhuzamos számítási megoldások fejlesztése a Batch segítségével

Az Azure Batch szolgáltatás hello hello alapvető összetevői a áttekintés hello elsődleges szolgáltatása arról lesz szó, és erőforrásokhoz, hogy a fejlesztők kötegelt toobuild nagyméretű párhuzamos a számítási megoldások.

E kidolgozása számítási elosztott alkalmazás vagy szolgáltatás, amely közvetlen problémák [REST API] [ batch_rest_api] hívások, illetve használ egy hello [kötegelt SDK-k](batch-apis-tools.md#azure-accounts-for-batch-development), fogja használni hello erőforrások és a cikkben említett szolgáltatások számos.

> [!TIP]
> Tekintse meg a magasabb szintű bemutatása toohello Batch szolgáltatás [alapjai az Azure Batch](batch-technical-overview.md).
>
>

## <a name="batch-service-workflow"></a>A Batch szolgáltatás munkafolyamata
hello következő magas szintű munkafolyamat része jellemzően szinte minden alkalmazások és munkaterhelések párhuzamos feldolgozásra hello Batch szolgáltatás használó szolgáltatások:

1. Töltse fel a hello **adatfájlok** , amelyet az tooprocess tooan [Azure Storage] [ azure_storage] fiók. Kötegelt elérése az Azure Blob Storage tárolóban beépített támogatását is magában foglalja, és a feladatok túl letölthesse a fájlokat[számítási csomópontok](#compute-node) hello feladatok futása közben.
2. Töltse fel a hello **alkalmazásfájlok** , amely a feladatok fogja futtatni. Ezeket a fájlokat bináris fájlokat vagy parancsprogramokat és függőségi viszonyaikat, és a feladatok hello feladatokat hajt végre. A feladatok a fájlok letölthetők a tárfiók, vagy használhatja a hello [alkalmazáscsomagok](#application-packages) köteg szolgáltatás telepítéséhez és alkalmazások kezelése.
3. Hozza létre a számítási csomópontok [készletét](#pool). Készletet hoz létre, amikor Ön adja meg a számítási csomópontok hello készlet, méretét, és hello operációs rendszer hello számát. A feladat minden egyes feladat futtatásakor van hozzárendelve tooexecute hello csomópontok a készlet egyik.
4. Hozzon létre egy [feladatot](#job). A feladatok tevékenységek gyűjteményeit kezelik. Minden feladat tooa adott készlet ahol az adott feladat tevékenységeit futni fog társítani.
5. Adja hozzá [feladatok](#task) toohello feladat. Minden feladat hello alkalmazás vagy a korábbi tárfiókban lévő tölt le fájlokat tooprocess hello feltöltött parancsprogram fut. Minden feladat befejeződött, mert azt a kimeneti tooAzure tárolási feltöltheti.
6. Feladatok előrehaladásának figyeléséhez, és az Azure Storage hello feladat kimenetének beolvasása.

hello alábbiakban vitassa meg ezeket és más erőforrások, amelyek lehetővé teszik az elosztott számítási forgatókönyv köteg hello.

> [!NOTE]
> Kell egy [a Batch-fiók](#account) toouse hello Batch-szolgáltatás. A legtöbb Batch-megoldás esetében szükség van [Azure Storage][azure_storage]-fiókra is a fájlok tárolásához és lekéréséhez. Kötegelt jelenleg csak hello **általános célú** tárfióktípus, 5. lépésében leírtak [hozzon létre egy tárfiókot](../storage/common/storage-create-storage-account.md#create-a-storage-account) a [tudnivalók az Azure storage-fiókok](../storage/common/storage-create-storage-account.md).
>
>

## <a name="batch-service-resources"></a>A Batch szolgáltatáshoz szükséges erőforrások
Az egyes erőforrások – fiókok, a következő hello számítási csomópontok, készletek, feladatok és feladatokhoz – hello Batch szolgáltatás összes megoldásokkal igényel. Mások, például a feladatütemezések vagy az alkalmazáscsomagok ugyan hasznosak, de használatuk nem kötelező.

* [Fiók](#account)
* [Számítási csomópont](#compute-node)
* [Készlet](#pool)
* [Feladat](#job)

  * [Feladatütemezések](#scheduled-jobs)
* [Tevékenység](#task)

  * [Indítási tevékenység](#start-task)
  * [Feladatkezelő-tevékenység](#job-manager-task)
  * [Feladat-előkészítési és -kiadási tevékenységek](#job-preparation-and-release-tasks)
  * [Többpéldányos tevékenység (MPI)](#multi-instance-tasks)
  * [Tevékenységfüggőségek](#task-dependencies)
* [Alkalmazáscsomagok](#application-packages)

## <a name="account"></a>Fiók
Batch-fiók egy egyedi módon azonosított entitás hello Batch szolgáltatás belül. Minden feldolgozás Batch-fiókkal van társítva.

Azure Batch-fiók hello segítségével hozhat létre [Azure-portálon](batch-account-create-portal.md) vagy programozott módon, mint például a hello [Batch Management .NET könyvtár](batch-management-dotnet.md). Hello fiók létrehozásakor egy Azure storage-fiókban is hozzárendelhető.

### <a name="pool-allocation-mode"></a>Készletlefoglalási mód

Batch-fiók létrehozásakor megadhatja, hogyan történjen a számítási csomópontok [készleteinek](#pool) lefoglalása. Azure Batch által kezelt előfizetést választhatja ki a számítási csomópontok tooallocate készletek, vagy a saját előfizetésének foglalhatja őket. Hello *tárolókészlet foglalási mód* hello fiók tulajdonság határozza meg, ahol készletek foglal le. 

tárolókészlet foglalási mód toouse, amely toodecide fontolja meg, amely legjobban megfelel a forgatókönyv:

* **A Batch szolgáltatás**: Batch szolgáltatás hello alapértelmezett alkalmazáskészlet foglalási mód, amelyben készletek foglal le az Azure által kezelt előfizetések hello színfalak mögött. Vegye figyelembe a legfontosabb hello Batch szolgáltatás tárolókészlet foglalási mód:

    - hello Batch szolgáltatás tárolókészlet foglalási mód mind a felhőalapú szolgáltatás, és a virtuális gép készletek támogat.
    - hello Batch szolgáltatás tárolókészlet foglalási mód támogatja mindkét megosztott kulcsos hitelesítéséhez vagy [Azure Active Directory hitelesítési](batch-aad-auth.md) (az Azure AD). 
    - Hello Batch szolgáltatás tárolókészlet foglalási üzemmóddal lefoglalt készletek vagy dedikált alacsony prioritású vagy a számítási csomópontok is használhatja.
    - Mód nem használható hello Batch szolgáltatás tárolókészlet foglalási Ha azt tervezi, hogy toocreate Azure virtuális gép készletek az egyéni Virtuálisgép-lemezképek, vagy ha azt tervezi, hogy a virtuális hálózati toouse. Inkább hozzon létre a fiókját, hello felhasználói előfizetési tárolókészlet foglalási mód.
    - Virtuális gép készletek kiépítve hello Batch szolgáltatás tárolókészlet foglalási üzemmóddal létrehozott fiók, létre kell hozni a [Azure virtuális gépek piactér] [ vm_marketplace] képek.

* **Felhasználói előfizetési**: hello felhasználói előfizetési készlet foglalási üzemmóddal kötegelt készletek kiosztása az hello Azure-előfizetés ahol hello-fiók létrehozása. Vegye figyelembe a legfontosabb hello felhasználói előfizetési tárolókészlet foglalási mód:
     
    - hello felhasználói előfizetési tárolókészlet foglalási mód csak a virtuális gép készletek támogat. A Cloud Services-készletek nem támogatottak.
    - toocreate virtuálisgép-készletek egyéni Virtuálisgép-rendszerképek vagy toouse egy virtuális hálózatot a virtuális gép készletek, hello felhasználói előfizetési tárolókészlet foglalási módot kell használnia.  
    - Kell használnia [Azure Active Directory hitelesítési](batch-aad-auth.md) az erőforráskészleteket, amelyekben hello felhasználói előfizetés foglal le. 
    - Be kell állítania egy az Azure key vault a Batch-fiókhoz Ha hello készlet foglalási mód tooUser előfizetés beállítása. 
    - Egy fiók hello felhasználói előfizetési tárolókészlet foglalási üzemmóddal létrehozott készletek csak dedikált számítási csomópontok is használhatja. Alacsony prioritású csomópontok nem támogatottak.
    - Egy fiók hello felhasználói előfizetési tárolókészlet foglalási üzemmóddal kiosztott virtuális gép készletek létre lehet hozni a [Azure virtuális gépek piactér] [ vm_marketplace] lemezképeket, vagy az egyéni lemezképek, hogy Adja meg.

a következő táblázat hello hello Batch szolgáltatás és felhasználói előfizetési tárolókészlet foglalási mód hasonlítja össze.

| **Készletlefoglalási mód**                 | **Batch szolgáltatás**                                                                                       | **Felhasználói előfizetés**                                                              |
|-------------------------------------------|---------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------|
| **A készletek lefoglalásának helye**               | Azure által felügyelt előfizetés                                                                           | hello felhasználói előfizetési a mely hello Batch-fiók létrehozása                        |
| **Támogatott konfigurációk**             | <ul><li>Felhőszolgáltatás konfigurációja</li><li>Virtuálisgép-konfiguráció (Linux és Windows)</li></ul> | <ul><li>Virtuálisgép-konfiguráció (Linux és Windows)</li></ul>                |
| **Támogatott virtuálisgép-rendszerképek**                  | <ul><li>Azure Marketplace-rendszerképek</li></ul>                                                              | <ul><li>Azure Marketplace-rendszerképek</li><li>Egyéni rendszerképek</li></ul>                   |
| **Támogatott számításicsomópont-típusok**         | <ul><li>Dedikált csomópontok</li><li>Alacsony prioritású csomópontok</li></ul>                                            | <ul><li>Dedikált csomópontok</li></ul>                                                  |
| **Támogatott hitelesítés**             | <ul><li>Megosztott kulcsos</li><li>Azure AD</li></ul>                                                           | <ul><li>Azure AD</li></ul>                                                         |
| **Azure Key Vault szükséges**             | Nem                                                                                                      | Igen                                                                                |
| **Magkvóta**                           | A Batch-magkvóta határozza meg                                                                          | Az előfizetés magkvótája határozza meg                                              |
| **Azure Virtual Network- (Vnet-) támogatás** | A készletek létre hello felhőalapú szolgáltatás konfigurációja                                                      | Virtuálisgép-konfiguráció hello létre készletek                               |
| **Vnet üzembehelyezési modell támogatása**      | Klasszikus üzemi modellel létrehozott Vnetek                                                             | A Vnetek hello klasszikus telepítési modell vagy a Azure Resource Manager |

## <a name="azure-storage-account"></a>Azure Storage-fiók

Az erőforrásfájlok és a kimeneti fájlok tárolására a legtöbb Batch-megoldás az Azure Storage-ot használja.  

Kötegelt jelenleg csak hello általános célú tárfiók típusa, 5. lépésében leírtak [hozzon létre egy tárfiókot](../storage/common/storage-create-storage-account.md#create-a-storage-account) a [tudnivalók az Azure storage-fiókok](../storage/common/storage-create-storage-account.md). A Batch-tevékenységeknek (beleértve a szabványos tevékenységeket, az indítási tevékenységeket, a feladat-előkészítési és a feladatkiadási tevékenységeket) olyan erőforrásfájlokat kell meghatározniuk, amelyek általános célú tárfiókokban találhatóak.


## <a name="compute-node"></a>Számítási csomópont
A számítási csomópont egy Azure virtuális gép (VM) vagy a virtuális gép, amely dedikált tooprocessing egy részét az alkalmazás terhelés a felhőalapú szolgáltatás. egy csomópont hello mérete CPU mag, a memória-kapacitás és a helyi rendszer fájlméret toohello csomópont lefoglalt hello számát határozza meg. Azure Cloud Services, a hello lemezképek használatával hozhat létre a Windows vagy Linux-csomópont készleteinek [Azure virtuális gépek piactér][vm_marketplace], vagy egyéni lemezképeket készíteni. Tekintse meg a következőt hello [készlet](#pool) további információt ezen beállítások szakaszban.

Csomópontok futtatható bármely végrehajtható fájl vagy parancsfájl hello operációsrendszer-környezet hello csomópont által támogatott. Ezek közé Windows esetén az \*.exe-, a \*.cmd-, a \*.bat-fájlok és a PowerShell-parancsfájlok tartoznak, Linux esetén pedig a bináris fájlok, valamint rendszerhéj- és Python-parancsfájlok.

A Batch szolgáltatásban működő számítási csomópontok emellett a következőket is tartalmazzák:

* Szabványos [mappastruktúra](#files-and-directories), valamint az ehhez tartozó [környezeti változók](#environment-settings-for-tasks), amelyekre a tevékenységek hivatkozni tudnak.
* **Tűzfal** beállításokat, amelyek toocontrol elérésének beállítását.
* [Távelérés](#connecting-to-compute-nodes) tooboth (Remote Desktop Protocol (RDP)) Windows és Linux (Secure Shell (SSH)) csomópontot.

## <a name="pool"></a>Készlet
A készletek olyan csomópontok gyűjteményei, amelyeken az alkalmazás fut. hello készlet hozhatók létre manuálisan, illetve automatikusan hello Batch szolgáltatás hello munkahelyi toobe végzett megadásakor. Hozzon létre, és a készlet, amely megfelel az alkalmazás erőforrás-követelmények hello kezelése. Egy készlet csak hello Batch-fiók, amelyben létrehozták használhatja. Egy Batch-fiók több készlettel is rendelkezhet.

Azure Batch-készleteket hozhat létre. hello core Azure számítási platform felett. Adja meg a nagy méretű foglalás, alkalmazástelepítés, adatok terjesztési, állapotfigyelést és rugalmas készlet számítási csomópontjain hello száma illesztését ([skálázás](#scaling-compute-resources)).

Minden csomópont tooa készlet hozzáadott egyedi nevét és IP-cím van hozzárendelve. Csomópont eltávolítása a készletből, toohello operációs rendszer vagy a fájlok végrehajtott módosítások elvesznek, és annak nevét, IP-cím kiadott későbbi használatra. Amikor egy csomópont kikerül egy készletből, vége van az élettartamának.

Készletet hoz létre, amikor a következő attribútumok hello is megadhat. Egyes beállítások eltérőek, attól függően, hogy hello tárolókészlet foglalási módjának hello kötegelt [fiók](#account):

- A számítási csomópont operációs rendszere és verziója
- A számítási csomópont típusa és a csomópontok kívánt száma
- Hello számítási csomópontok mérete
- Skálázási szabályzat
- Tevékenységütemezési szabályzat
- A számítási csomópontok kommunikációs állapota
- A számítási csomópontok indítási tevékenységei
- Alkalmazáscsomagok
- Hálózati konfiguráció

Ezek a beállítások leírását a következő részekben hello részletesebben.

> [!IMPORTANT]
> Batch fiókjainak hello Batch szolgáltatás tárolókészlet foglalási üzemmóddal rendelkezik, amely korlátozza a Batch-fiók magok száma hello alapértelmezett kvótát. magok száma hello felel meg a számítási csomópontok toohello száma. Hello alapértelmezett kvóták és találhat útmutatást hogyan túl[a kvóta növeléséhez](batch-quota-limit.md#increase-a-quota) a [kvótái és korlátai hello Azure Batch szolgáltatás](batch-quota-limit.md). A készlet nem van elérése a cél a csomópontok számát, ha a hello core kvóta hello ok lehet.
>
>Batch fiókjainak hello felhasználói előfizetési tárolókészlet foglalási üzemmóddal nincs vizsgálat hello Batch szolgáltatás kvótái. Ehelyett a hello közös hello magkvótája megadott előfizetéshez. További információkért lásd [az Azure-előfizetésekre és -szolgáltatásokra vonatkozó korlátozásokat, kvótákat és megkötéseket](../azure-subscription-service-limits.md) ismertető témakör [a virtuális gépek korlátaira](../azure-subscription-service-limits.md#virtual-machines-limits) vonatkozó részét.
>
>

### <a name="compute-node-operating-system-and-version"></a>A számítási csomópont operációs rendszere és verziója

A Batch-készlet létrehozásakor megadhatja a hello Azure virtuális gép konfigurációs és operációs rendszer minden számítási csomóponton hello készletben toorun kívánt hello típusú. rendelkezésre álló kötegben konfigurációk hello két típusa van:

- Hello **virtuálisgép-konfiguráció**, amely a készlethez tartozó hello adja meg az Azure virtuális gépek magában foglalja. Ezek a virtuális gépek Linux- vagy Windows-rendszerképből is létrehozhatók. 

    Amikor alapuló virtuálisgép-konfiguráció hello készletet hoz létre, meg kell adnia hello csomópontok nem csak hello méretétől és hello használt képek toocreate hello forrását őket, de is hello **virtuális gép Képhivatkozás** és hello Kötegelt **csomópont ügynök SKU** toobe hello csomópontjára telepítve. A készlet e tulajdonságainak megadásával kapcsolatos további információk: [Provision Linux compute nodes in Azure Batch pools](batch-linux-nodes.md) (Linuxos számítási csomópontok kiépítése Azure Batch-készletekben).

- Hello **Felhőszolgáltatások konfigurálása**, amely adja meg a készlethez tartozó hello Azure Felhőszolgáltatások csomópontok magában foglalja. A Cloud Services *kizárólag* windowsos számítási csomópontok létrehozására használható.

    A felhő konfigurálása készletek rendelkezésre álló operációs rendszeren hello szereplő [Azure vendég operációs rendszereinek kiadásait és SDK-kompatibilitási mátrixát](../cloud-services/cloud-services-guestos-update-matrix.md). Cloud Services csomópontok tartalmazó készletet hoz létre, ha szüksége van-e toospecify hello csomópont méretének és a *operációsrendszer-család*. Felhőszolgáltatások gyorsabban Windows rendszerű virtuális gépek olyan telepített tooAzure. Ha windowsos számítási csomópontok készleteire van szükség, előfordulhat, hogy a Cloud Services üzembe helyezési idő szempontjából teljesítményelőnyt nyújt.

    * Hello *operációsrendszer-család* is meghatározza, mely verzióit .NET hello OS lettek telepítve.
    * A feldolgozói szerepkörök Felhőszolgáltatások belül, meg egy *operációsrendszer-verzió* (feldolgozói szerepkörök további információkért lásd: hello [információ a felhőszolgáltatások](../cloud-services/cloud-services-choose-me.md#tell-me-about-cloud-services) hello című [Cloud Services áttekintés](../cloud-services/cloud-services-choose-me.md)).
    * Mivel feldolgozói szerepkörök, azt javasoljuk, hogy megadja `*` hello a *operációsrendszer-verzió* , hogy hello csomópontok automatikusan megtörténik, és toonewly kiadott verziók nem szükséges munka toocater nincs. hello elsődleges használati eset kijelölése egy adott operációs rendszer verziója Alkalmazáskompatibilitás tooensure, amely lehetővé teszi az előző verziókkal való kompatibilitás tesztelési toobe végre, mielőtt engedélyezné a hello verzió toobe frissítése. Ellenőrzés után hello *operációsrendszer-verzió* hello készlet frissíthető és telepíthető hello új operációsrendszer-lemezképek – összes futó feladat megszakadt, és helyezte.

Készletet hoz létre, amikor szüksége tooselect hello megfelelő **nodeAgentSkuId**hello az operációs rendszer alapjául szolgáló lemezképhez hello a VHD-fájl, attól függően, hogy. Kérhető le a rendelkezésre álló csomópont ügynök SKU eseményazonosítók tootheir leképezése az operációsrendszer-lemezképek hivatkozások hívó hello [lista támogatott csomópont ügynök SKU](https://docs.microsoft.com/rest/api/batchservice/list-supported-node-agent-skus) műveletet.

Lásd: hello [fiók](#account) szakasz hello tárolókészlet foglalási mód beállításáról a Batch-fiók létrehozásakor.

#### <a name="custom-images-for-virtual-machine-pools"></a>Egyéni rendszerképek virtuálisgép-készletekhez

toouse egy egyéni lemezkép tooprovision virtuálisgép-készletek, a Batch-fiók létrehozása a hello felhasználói előfizetési tárolókészlet foglalási üzemmóddal. Ebben a módban a kötegelt készletek hello fiókot tartalmazó hello előfizetéssé foglal le. Lásd: hello [fiók](#account) szakasz hello tárolókészlet foglalási mód beállításáról a Batch-fiók létrehozásakor.

toouse egy egyéni lemezképet, szüksége lesz tooprepare hello lemezkép által normalizálása azt. További információ a Linux egyéni lemezképek előkészítése az Azure virtuális gépek: [rögzítése sablonként egy Azure Linux virtuális gép toouse](../virtual-machines/linux/capture-image-nodejs.md). További tudnivalókat az Azure-beli virtuális gépekről származó egyéni Windows-rendszerképek előkészítéséről az [egyéni virtuálisgép-rendszerképek Azure PowerShell-lel való létrehozását ismertető](../virtual-machines/windows/tutorial-custom-images.md) cikkben találhat. 

> [!IMPORTANT]
> Az egyéni lemezképet előkészítésekor tartsa szem előtt tartva hello következő:
> - Győződjön meg arról, hogy hello alap operációsrendszer-lemezképek használhatja a kötegelt készletek does tooprovision nem rendelkezik olyan előre telepített Azure-bővítményeket, például az egyéni parancsprogramok futtatására szolgáló bővítmény hello. Ha hello kép egy előre telepített bővítményt tartalmazza, Azure hello VM telepítése problémák fordulhatnak elő.
> - Győződjön meg arról, hogy hello alap operációsrendszer-lemezképek, megadására, használ hello alapértelmezett ideiglenes meghajtón, mert hello kötegelt csomópont ügynök jelenleg vár hello alapértelmezett ideiglenes meghajtón.
>
>

a virtuális gép konfigurációs készlet egyéni lemezkép használatával toocreate, létre kell hoznia egyet, vagy több szabványos Azure Storage-fiókok toostore az egyéni VHD lemezképek. Az egyéni lemezképek blobként vannak tárolva. az egyéni lemezképek, a készlet létrehozásakor tooreference adja meg a hello hello egyéni kép VHD-blobok hello az URI-azonosítók [osDisk](https://docs.microsoft.com/rest/api/batchservice/add-a-pool-to-an-account#bk_osdisk) hello tulajdonságának [virtualMachineConfiguration](https://docs.microsoft.com/rest/api/batchservice/add-a-pool-to-an-account#bk_vmconf) tulajdonság.

Győződjön meg arról, hogy a storage-fiókok megfelelnek-e a következő feltételek hello:   

- hello storage-fiókok tartalmazó hello egyéni kép VHD-blobok kell a toobe ugyanahhoz az előfizetéshez hello, mint a Batch-fiók (hello felhasználói előfizetési) hello.
- hello megadott tárfiókok kell a hello toobe és hello Batch-fiók ugyanabban a régióban.
- Jelenleg csak a standard szintű, általános célú tárfiókok támogatottak. Prémium szintű storage jövőbeli hello támogatott.
- Megadhat több egyéni virtuálismerevlemez-blobbal rendelkező tárfiókot, vagy több tárfiókot egyetlen blobhoz. Javasoljuk, hogy toouse több tárfiókot tooget a jobb teljesítmény.
- Egy egyedi egyéni lemezkép Virtuálismerevlemez-blobot too40 Linux Virtuálisgép-példányok vagy 20 Windows Virtuálisgép-példány képes támogatni. Szüksége lesz toocreate hello VHD blob toocreate készletek további virtuális gépek példányait. Például egy címkészlet, amely 200 Windows virtuális gépek kell 10 egyedi VHD-blobok hello megadott **osDisk** tulajdonság.

egy készlet hello Azure-portál használatával egyéni lemezképéről toocreate:

1. Keresse meg a Batch-fiók tooyour hello Azure-portálon.
2. A hello **beállítások** panelen, jelölje be hello **készletek** menüpont.
3. A hello **készletek** panelen, jelölje be hello **Hozzáadás** hello; utasításhoz **készlet hozzáadása** panel fog megjelenni.
4. Válassza ki **egyéni lemezkép (Linux/Windows)** hello a **képtípust** legördülő menüből. hello portál megjeleníti hello **egyéni lemezkép** kiválasztása. Válasszon egy vagy több virtuális merevlemezek hello azonos konténert, és kattintson hello **kiválasztása** gombra. 
    Több virtuális merevlemezeket különböző storage-fiókok és a különböző tárolók támogatása a jövőbeli hello lesz hozzáadva.
5. SELECT hello megfelelő **Publisher/ajánlat/Sku** az egyéni VHD-k, válassza ki a kívánt hello **gyorsítótárazását** üzemmódban, majd töltse ki az összes hello más paramétereket az hello készlet.
6. toocheck készlet alapú egyéni lemezképet, tekintse meg a hello **operációs rendszer** hello erőforrás összefoglaló szakaszában hello tulajdonság **készlet** panelen. Ez a tulajdonság értékének hello meg kell **egyéni Virtuálisgép-lemezkép**.
7. A készlethez társított összes egyéni virtuális merevlemezek hello készlet megjelenő **tulajdonságok** panelen.

### <a name="compute-node-type-and-target-number-of-nodes"></a>A számítási csomópont típusa és a csomópontok kívánt száma

Készletet hoz létre, ha szeretné, hogy és hello cél számát az egyes csomópontok milyen típusú is megadhat. számítási csomópontok hello két típusa van:

- **Dedikált számítási csomópontok.** A dedikált számítási csomópontok az adott feladatra vannak fenntartva. Alacsony prioritású csomópont drágább, de azok garantált toonever foglalható.

- **Alacsony prioritású számítási csomópontok.** Alacsony prioritású csomópont előnyeit felesleges kapacitás Azure toorun a Batch számítási feladatait. Az alacsony prioritású csomópontok olcsóbbak óránként, mint a dedikáltak, és nagy számítási teljesítményt igénylő feladatok futtatását teszik lehetővé. További információ: [Alacsony prioritású virtuális gépek használata a Batch szolgáltatással](batch-low-pri-vms.md).

    Az alacsony prioritású számítási csomópontok háttérbe szorulhatnak, ha az Azure-ban nem áll rendelkezése elég többletkapacitás. Ha egy csomópont van részesítés feladatok futtatása során, hello feladatok helyezte, és futtassa újra a után a számítási csomópont újra lesz elérhető. Alacsony prioritású csomópont olyan munkaterhelések esetén, ahol hello feladat befejezési idő rugalmas, és hello munkahelyi van osztva sok csomópontjai között jó választás. Mielőtt a forgatókönyvnek toouse alacsony prioritású csomópontok mellett dönt, győződjön meg arról, hogy a munka miatt megszakadt toopreemption minimális és könnyen toorecreate lesz.

    Alacsony prioritású számítási csomópontok érhetők el csak Batch fiókjainak hello tárolókészlet foglalási módban a túl**Batch szolgáltatás**.

Mindkét alacsony és dedikált számítási csomópont lehet hello azonos erőforráskészletben. Minden típusú csomópont &mdash; alacsony és dedikált &mdash; van saját cél beállítás, amelynek megadhatja a szükséges hello csomópontok száma. 
    
hello számítási csomópontok száma érték a hivatkozott tooas egy *cél* , mivel bizonyos esetekben a készlet nem fogja szükséges hello csomópontok száma. Például egy alkalmazáskészlet lehetséges, hogy nem fog hello cél Ha eléri a hello [core kvóta](batch-quota-limit.md) a kötegelt fiókok először. Vagy hello készlet nem lehetséges, hogy fog hello cél, ha telepítette az automatikus skálázás képlet toohello készlet, amely hello csomópontok maximális száma korlátozza.

Az alacsony prioritású és dedikált számítási csomópontok díjszabását a [Batch díjszabása](https://azure.microsoft.com/pricing/details/batch/) írja le.

### <a name="size-of-hello-compute-nodes"></a>Hello számítási csomópontok mérete

A **Cloud Services-konfigurációt** használó számítási csomópontok méretét a [Sizes for Cloud Services](../cloud-services/cloud-services-sizes-specs.md) (A Cloud Servicesben érvényes méretek) című cikk tartalmazza. A Batch az `ExtraSmall`, `STANDARD_A1_V2` és `STANDARD_A2_V2` kivételével az összes Cloud Services-méretet támogatja.

A **virtuálisgép-konfigurációt** használó számítási csomópontok méretét a [Sizes for virtual machines in Azure](../virtual-machines/linux/sizes.md) (Virtuális gépek mérete az Azure-ban, Linux) és a [Sizes for virtual machines in Azure](../virtual-machines/windows/sizes.md) (Virtuális gépek mérete az Azure-ban, Windows) című cikk tartalmazza. A Batch a `STANDARD_A0`, illetve a Premium Storage típusú méretek (`STANDARD_GS`, `STANDARD_DS` és `STANDARD_DSV2` sorozat) kivételével az összes Azure virtuálisgép-méretet támogatja.

A számítási csomópont méretű kiválasztásakor vegye figyelembe a hello jellemzőit és hello csomópontján futtathatja hello alkalmazások követelményeinek. Szempontok többszálas-e hello alkalmazást, és segít a mekkora memóriát el például hello leginkább megfelelő és költséghatékony csomópont mérete határozza meg. Egy csomópont mérete, feltéve, hogy a feladat is egyszerre egy csomóponton fog futni tipikus tooselect. Lehetséges toohave azonban több feladat (és így több alkalmazáspéldányt) [párhuzamos futtatása](batch-parallel-node-tasks.md) a számítási csomópontok feladat végrehajtása során. Ebben az esetben is közös toochoose nagyobb csomópont mérete tooaccommodate nőtt hello igény szerinti párhuzamos feladat-végrehajtás. A további információkat a [Tevékenységütemezési szabályzat](#task-scheduling-policy) tartalmazza.

Hello csomópontok a készlet összes azonos hello méretét. Ha eltérő rendszerkövetelményeinek toorun alkalmazások tervezi, illetve szintek betölteni, ajánlott külön készletek.

### <a name="scaling-policy"></a>Skálázási szabályzat

A dinamikus munkaterhelésekhez, írási és alkalmazni egy [automatikus skálázás képlet](#scaling-compute-resources) tooa készlet. hello Batch szolgáltatás rendszeresen értékeli ki a képletet, és különböző, feladat, és hogy paramétereinek megadása alapján hello készlet belüli csomópontok számát hello módosíthatja.

### <a name="task-scheduling-policy"></a>Tevékenységütemezési szabályzat

Hello [tevékenységek maximális száma legfeljebb](batch-parallel-node-tasks.md) konfigurációs beállítás hello hello belül minden számítási csomópont párhuzamosan futtatható feladatok maximális számát határozza meg.

hello alapértelmezett konfigurációja határozza meg, hogy egyszerre csak egy feladat futtatása egy csomóponton, de a forgatókönyvekben, ahol azt előnyös toohave egyszerre egy csomóponton végre két vagy több feladat. Lásd: hello [példa](batch-parallel-node-tasks.md#example-scenario) a hello [egyidejű csomópont feladatok](batch-parallel-node-tasks.md) cikk toosee hogyan előnyeit úgy használhatja ki több tevékenységek maximális száma.

Azt is megadhatja a *kitöltési mód* amely megállapítja, hogy a kötegelt között osztja el hello feladatok egyenletes készlet az összes csomópont, vagy minden csomópont, amelynek a feladatok maximális száma hello csomagok feladatok tooanother csomópontra hozzárendelése előtt.

### <a name="communication-status-for-compute-nodes"></a>A számítási csomópontok kommunikációs állapota

A legtöbb környezetben feladatokat egymástól függetlenül működnek, és nem kell toocommunicate egymással. Vannak azonban olyan alkalmazások (például [MPI-megoldások](batch-mpi.md)), ahol a tevékenységeknek kommunikálniuk kell egymással.

Beállíthatja, hogy a készlet tooallow **fürtök csomóponton belüli kommunikációjához kommunikációs**, hogy a készlet csomópontjain képes kommunikálni a futási időben. Ha engedélyezte a csomópontok közötti kommunikációt, a Cloud Services-konfigurációt használó készletekben működő csomópontok az 1100-nál magasabb portszámú portokon, a virtuálisgép-konfigurációs készletek csomópontjai pedig bármely porton képesek lesznek kommunikálni egymással.

Vegye figyelembe, hogy engedélyezi a fürtök csomóponton belüli kommunikációjához kommunikációs is hatással van a fürtök hello csomópontja hello elhelyezését korlátozhatja a készletben csomópontok maximális száma hello telepítési korlátozásai miatt. Ha az alkalmazás nem igényli a csomópontok közötti kommunikáció, hello Batch szolgáltatás is foglaljon le csomópontok potenciálisan nagy számú toohello készlet számos különböző fürtök és adatközpontok tooenable nőtt párhuzamos feldolgozási kapacitás.

### <a name="start-tasks-for-compute-nodes"></a>A számítási csomópontok indítási tevékenységei

nem kötelező hello *feladat indítása* végrehajtása minden egyes csomóponton, hogy a csomópont csatlakozik hello készletet, és minden alkalommal, amikor a csomópont újraindul vagy lemezképet. hello kezdő tevékenység különösen fontos a számítási csomópontok előkészítése hello végrehajtási feladatok, például a feladatok hello számítási csomóponton futó hello alkalmazások telepítése.

### <a name="application-packages"></a>Alkalmazáscsomagok

Megadhat [alkalmazáscsomagok](#application-packages) toodeploy toohello számítási csomópontok hello készletben. Alkalmazáscsomagok egyszerűbb telepítés és a feladatok futó hello alkalmazások versioning adja meg. A készlethez beállított alkalmazáscsomagokat a rendszer a készlethez csatlakozó összes csomópontra telepíti, illetve minden alkalommal telepíti őket, amikor egy csomópontot újraindítanak vagy rendszerképét alaphelyzetbe állítják.

> [!NOTE]
> Az alkalmazáscsomagok az összes 2017. július 5. után létrehozott Batch-készleten támogatottak. 2016. március 10. és 5. július 2017 között létrehozott, csak akkor, ha hello készlet lett létrehozva egy felhőalapú szolgáltatás konfigurációja kötegelt készletek azok támogatottak. Kötegelt készletek létrehozott előzetes too10 2016. március nem támogatják az alkalmazáscsomagok. Alkalmazás használatával kapcsolatos további információk a toodeploy az alkalmazások tooyour kötegelt csomópontok csomagokat, a következő témakörben: [központi telepítése az alkalmazások toocompute csomópontokat a kötegelt alkalmazáscsomagok](batch-application-packages.md).
>
>

### <a name="network-configuration"></a>Hálózati konfiguráció

Megadhat egy Azure hello alhálózata [virtuális hálózathoz (VNet)](../virtual-network/virtual-networks-overview.md) mely hello készlet számítási csomópontok kell létrehozni. Lásd: hello [készlet hálózati konfiguráció](#pool-network-configuration) szakaszban olvashat.


## <a name="job"></a>Feladat
A feladatok tevékenységek gyűjteményei. Milyen számítási végzi el a készlet számítási csomópontjai hello feladatainak kezeli.

* hello feladat megadja hello **készlet** mely hello munkahelyi toobe futtatásához. Az egyes feladatokhoz saját készletet hozhat létre, de egyetlen készletet is használhat több feladathoz. A feladatütemezésbe tartozó egyes feladatokhoz külön-külön készletet hozhat létre, vagy létrehozhat egy készletet, amely a feladatütemezésbe tartozó összes feladatot tartalmazza.
* Ha szeretné, megadhatja a **feladatok prioritását** is. Amikor a feladat jelenleg folyamatban lévő feladatok mint a magasabb prioritású, előre hello alacsonyabb prioritású feladatok feladatok hello várólista illeszteni a hello magasabb prioritású feladatok hello feladatok. Ezek azonban nem előzik meg a már futó alacsonyabb prioritású feladat tevékenységeit.
* Feldolgozással **megkötések** toospecify a feladatok esetében bizonyos korlátozások:

    Megadhat egy **maximális wallclock idő**, így ha egy feladat futtatása hosszabb megadott hello maximális wallclock ideje, hello feladat és összes a feladat leállt.

    A Batch képes észlelni, ha egy tevékenység sikertelen volt, és megpróbálni újra elvégezni. Megadhatja a hello **feladat újbóli próbálkozások maximális számát** korlátozásként, beleértve, hogy a tevékenység *mindig* vagy *soha nem* rendszer megpróbálja újból végrehajtani. Újrapróbálkozás a feladat azt jelenti, hogy e hello feladat helyezte toobe futtassa újból.
* Az ügyfélalkalmazás feladatok tooa feladat adhat hozzá, vagy megadhat egy [manager feladat](#job-manager-task). A kezelő feladat szükséges toocreate hello szükséges feladatok feladat hello információkat tartalmaz, hello manager feladat hello egyikét futtatja a számítási csomópontok hello készletben. hello feladata manager kezeli kifejezetten által kötegelt--azt kell várakoznia, amint hello feladat jön létre, és újraindítják, ha nem sikerül. A feladat manager feladatok *szükséges* feladatok által létrehozott egy [feladat ütemezésének](#scheduled-jobs) mert hello csak úgy toodefine hello feladatok előtt hello feladat jön létre.
* Alapértelmezés szerint feladatok maradnak hello aktív állapotú hello feladat minden feladat be nem fejeződik. Ez a viselkedés módosíthatja, hogy hello feladat automatikusan leáll, amikor az összes feladat hello feladat be nem fejeződik. Set hello feladat **onAllTasksComplete** tulajdonság ([OnAllTasksComplete] [ net_onalltaskscomplete] a Batch .NET) túl*terminatejob* tooautomatically hello feladat leáll, ha az összes feladatainak befejeződött hello állapotban.

    Vegye figyelembe, hogy hello Batch szolgáltatás úgy ítéli meg, a feladat *nem* feladatok toohave összes a feladat befejeződött. Ezért ezt a funkciót általában egy [feladatkezelői tevékenységgel](#job-manager-task) használjuk. Ha azt szeretné, hogy a Feladatkezelő nélkül toouse automatikus feladat futása, célszerű egy új feladat kezdetben **onAllTasksComplete** tulajdonság túl*noaction*, majd állítsa be túl*terminatejob* csak hozzáadásának feladatok toohello feladat befejezése után.

### <a name="job-priority"></a>A feladatok prioritása
Egy kötegben létrehozott prioritás toojobs rendelhet hozzá. hello Batch szolgáltatás hello feladat toodetermine hello sorrendjét a partner feladatütemezés hello prioritási értéket használja (ez nem az toobe összetéveszthető egy [ütemezett feladat](#scheduled-jobs)). hello prioritási érték között-1000 too1000,-1000 a legalacsonyabb prioritású hello alatt, és 1000 legmagasabb hello. egy feladat, hívás hello tooupdate hello prioritását [egy feladat hello tulajdonságainak frissítése] [ rest_update_job] művelet (Batch REST), vagy módosítsa a hello [CloudJob.Priority] [ net_cloudjob_priority] tulajdonság (Batch .NET).

Hello belül fiókot használja, a magasabb prioritású feladatok rendelkezik sorrend keresztül az alacsony prioritású feladatok ütemezése. Egy fiók magasabb prioritási értékű feladatai nem élveznek elsőbbséget egy másik fiók alacsonyabb prioritási értékű másik feladatával szemben.

A készletek között a feladatok ütemezése egymástól független. Különböző készletek között nem garantált, hogy a rendszer egy magasabb prioritású feladatot előbbre ütemez, ha annak társított készletében nincsenek tétlen csomópontok. A hello egyazon tárolókészlethez feladatok hello az azonos prioritási szintet van beütemezve egyenlő esélyét.

### <a name="scheduled-jobs"></a>Ütemezett feladatok
[Sikertelen feladat ütemezésének] [ rest_job_schedules] lehetővé teszik a toocreate ismétlődő feladatok hello Batch szolgáltatás belül. A feladatütemezés határozza meg, amikor toorun feladatokat, és futtassa hello feladatok toobe hello specifikációk tartalmazza. Hello ütemezés – hello időtartama mennyi ideig adja meg ha hello ütemezés van –, és milyen gyakran feladatok hello során jönnek létre ütemezett időszak.

## <a name="task"></a>Tevékenység
A tevékenységek olyan számítási egységek, amelyek feladathoz vannak társítva, és egy csomóponton futnak. Feladatok vannak hozzárendelve a végrehajtás tooa csomópont, vagy állítja sorba, míg egy csomópont szabad. Egyszerűen csak PUT, fusson a feladat egy vagy több programokat vagy parancsfájlokat a számítási csomópont tooperform hello munkahelyi kell elvégezni.

Amikor létrehozza a tevékenységet, a következőket kell megadnia:

* Hello **parancssori** hello feladathoz. Ez az az alkalmazás vagy a parancsfájl hello számítási csomóponton futó hello parancssor.

    Fontos, hogy a parancssor hello toonote nem futtatja ténylegesen a rendszerhéj alatt. Ezért az nem natív módon rendszerhéj szolgáltatások előnyeinek például [környezeti változó](#environment-settings-for-tasks) bővítése (Ez magában foglalja a hello `PATH`). tootake előny ilyen a szolgáltatásokat, meg kell hívnia a hello rendszerhéj hello parancssorban – például indításával `cmd.exe` Windows csomópontján vagy `/bin/sh` Linux rendszeren:

    `cmd /c MyTaskApplication.exe %MY_ENV_VAR%`

    `/bin/sh -c MyTaskApplication $MY_ENV_VAR`

    Ha a feladatokat kell toorun egy alkalmazás vagy parancsprogram, amely nem része hello csomópont `PATH` vagy környezeti változók hivatkoznak, meghívása hello rendszerhéj explicit módon a hello feladat parancssorban.
* **Erőforrás-fájlok** hello adatok toobe feldolgozott tartalmaznak. Ezeket a fájlokat a Blob storage egy általános célú Azure Storage-fiókban automatikusan másolt toohello csomópont, parancssor hello feladat végrehajtása előtt. További információkért lásd: hello szakaszok [kezdő tevékenység](#start-task) és [fájlok és könyvtárak](#files-and-directories).
* Hello **környezeti változók** az alkalmazás által igényelt. További információkért lásd: hello [környezeti beállítások feladatokhoz](#environment-settings-for-tasks) szakasz.
* Hello **megkötések** alapján mely hello feladatot végre kell hajtani. Megkötések például hello maximális ideje, hogy hello feladat toorun, hello maximálisan megengedett számú sikertelen feladat újra meg kell próbálni, engedélyezett és hello maximális időt, amely hello feladatütemezési munkakönyvtár fájlok kerülnek.
* **Alkalmazáscsomagok** toodeploy toohello számítási csomópont, mely hello az ütemezett toorun feladat. [Alkalmazáscsomagok](#application-packages) egyszerűbb telepítés és a feladatok futó hello alkalmazások versioning adja meg. Tevékenység szintű alkalmazáscsomagok különösen hasznosak megosztott készlet környezetekben, ahol különböző feladat futtatása egy készletet, és hello készlet nem törlődik, ha egy feladat befejeződött. Ha a feladat csomópontok eltérő kevesebb feladatok hello készletben, feladat alkalmazáscsomagok minimálisra adatátvitel mivel az alkalmazás telepített csak toohello a csomópontokra, amelyeket a feladatok futtatásához.

Továbbá a tootasks tooperform számítási csomóponton határozza meg, a következő különleges feladatok hello is által biztosított hello Batch szolgáltatás:

* [Indítási tevékenység](#start-task)
* [Feladatkezelő-tevékenység](#job-manager-task)
* [Feladat-előkészítési és -kiadási tevékenységek](#job-preparation-and-release-tasks)
* [Többpéldányos tevékenységek (MPI)](#multi-instance-tasks)
* [Tevékenységfüggőségek](#task-dependencies)

### <a name="start-task"></a>Indítási tevékenység
Társításával egy **feladat indítása** címkészlettel, előkészítheti a hello operációsrendszer-környezet csomópontja. Például a feladatok futó hello alkalmazások telepítése és elindítása háttérfolyamatot hasonló műveletek végezhetők. hello start feladat fut. minden alkalommal, amikor egy csomópont elindul, a mindaddig, amíg azt hello készletben – beleértve a hello csomópont először fel lett véve toohello címkészletet, és ha újra, vagy lemezképet

Egy elsődleges hello kezdő tevékenység előnye, hogy a számítási csomópont tartalmazza az összes szükséges tooconfigure hello információkat, és a feladat végrehajtásához szükséges hello alkalmazásokat telepíteni. A készletben található csomópontok számának hello növelése ezért egyszerűen hello új cél csomópont számának megadása. hello kezdő tevékenység hello Batch szolgáltatás hello információkat tartalmaz, amelyek szükséges tooconfigure hello új csomópont, és készen áll a feladatok elfogadása beolvasása.

Bármely Azure Batch feladatokkal, adjon meg egy listája **erőforrásfájlok** a [Azure Storage][azure_storage], továbbá tooa **parancssori** toobe végre. hello Batch szolgáltatás hello erőforrás fájlok toohello csomópont először másolja az Azure Storage-ból, és majd a hello parancssorból futtatja. Egy készlet kezdő tevékenység hello fájl általában listában hello feladat alkalmazás és annak függőségeit.

Hello kezdő tevékenység azonban is lehetnek a referencia-adatok toobe hello számítási csomóponton futó összes feladat által használt. Például egy kezdő tevékenység parancssori végezhet egy `robocopy` művelet toocopy alkalmazásfájlok (amely Erőforrásfájlok voltak meghatározva, és le toohello csomópont) a hello indítása tevékenység [munkakönyvtár](#files-and-directories) toohello [megosztott mappa](#files-and-directories), majd futtassa egy olyan MSI Csomaghoz, vagy `setup.exe`.

Kívánatos általában hello Batch szolgáltatás toowait hello kezdő tevékenység toocomplete előtt annak eldöntéséhez, hogy a hello csomópont készen toobe rendelt feladatok, de konfigurálható.

Egy kezdő tevékenység számítási csomóponton nem sikerül, ha majd hello hello csomópont állapota frissített tooreflect hello hiba, és hello csomópont nem tartozik minden feladatot. A kezdő tevékenység sikertelen lehet, ha az erőforrás-fájlok másolását a tárolási probléma van, vagy ha a parancssorból végre hello folyamat egy nem nulla kilépési kódot adja vissza.

Ha hozzáadásakor vagy módosításakor meglévő készletek hello kezdő tevékenység, újra kell indítani a számítási csomópontok hello kezdő tevékenység alkalmazott toobe toohello csomópontok.

>[!NOTE]
> hello teljes mérete a kezdő tevékenységre kell kisebb vagy egyenlő, mint too32768 karaktert, köztük Erőforrásfájlok és környezeti változókat. tooensure, hogy a kezdő tevékenység megfelel-e ezt a követelményt, használja két módszer egyikét:
>
> 1. Alkalmazás csomagok toodistribute alkalmazások és adatok között a Batch-készlet minden egyes csomópontja használható. Alkalmazáscsomagok kapcsolatos további információkért lásd: [központi telepítése az alkalmazások toocompute csomópontokat a kötegelt alkalmazáscsomagok](batch-application-packages.md).
> 2. Manuálisan is létrehozhatja az alkalmazások fájljait tartalmazó tömörített archívumot. A ZIP-archívum tooAzure tárolási feltöltése a blob-ként. Adja meg a kezdő tevékenység erőforrásfájl hello ZIP-archívumban. A kezdő tevékenység hello parancssor futtatása, előtt csomagolja ki a hello archív hello parancssorból. 
>
>    toounzip hello archív, az eszköz az Ön által választott archiválás hello is használhatja. Szüksége lesz tooinclude hello eszköz, a hello kezdő tevékenységre erőforrásfájl toounzip hello archív használatát.
>
>

### <a name="job-manager-task"></a>Feladatkezelő tevékenység
Általában egy **manager feladat** toocontrol és/vagy a figyelő feladat-végrehajtási – például toocreate és elküldeni a feladat hello feladatokat, további feladatok toorun meghatározása és a munkahelyi befejeződésének megállapítása. Azonban egy kezelő feladat nem korlátozott toothese tevékenységeket. Hajthat végre semmilyen műveletet a hello feladat által igényelt teljes értékű feladat. Például egy kezelő feladat előfordulhat, hogy töltsön le egy paraméterként megadott fájlt, elemzése, hogy a fájl tartalmát hello és küldje el azokat a tartalom alapján további feladatokat.

A rendszer minden más feladat előtt indítja el a feladatkezelői tevékenységeket. Hello a következő szolgáltatásokat tartalmazza:

* Az automatikusan nyújtja az feladatok hello Batch szolgáltatás hello feladat létrehozásakor.
* Az ütemezett tooexecute előtt hello más feladatok egy feladat van.
* A társított csomópontjában hello utolsó toobe távolítja el a készletből, amikor hello alkalmazáskészlet van folyamatban downsized.
* A megszakítási hello feladat összes tevékenységében kapcsolt toohello megszűnése lehet.
* Egy feladat manager feladat hello legmagasabb prioritású virtuális gép megkapja a amikor toobe újraindítása szükséges. Egy üresjárati csomópont nem érhető el, ha hello Batch szolgáltatás előfordulhat, hogy leáll valamelyik hello hello készlet toomake helyiségben hello feladat manager feladat toorun többi futó feladat.
* Egy feladat manager feladata nem elsőbbséget élveznek a hello feladatok, más feladatok. A feladatok között csak a feladatszintű prioritások érvényesek.

### <a name="job-preparation-and-release-tasks"></a>Feladat-előkészítési és -kiadási tevékenységek
A Batch a feladatok előtt elvégzendő beállításokhoz feladat-előkészítési tevékenységeket biztosít. A feladatkiadási tevékenységek ezzel szemben a feladat elvégzése utáni karbantartási vagy takarítási műveletekhez használhatók.

* **Feladat előkészítése tevékenységet**: A feladat előkészítése tevékenység fut, amelyek toorun ütemezett feladatok minden számítási csomóponton, hello minden más feladatot feladatok végrehajtásának. A feladat előkészítése tevékenység toocopy adatok legyen elosztva a minden feladat, de egyedi toohello feladat, például is használhatja.
* **Feladat kiadása tevékenység**: a feladat befejezése után a feladat kiadása tevékenység hello-készlet, amely legalább egy feladat végrehajtása minden egyes csomópontján fut-e. Egy feladat kiadása tevékenység toodelete hello feladat előkészítése tevékenységet a másolt vagy toocompress és feltöltése diagnosztikai napló adatok, például is használja.

Előkészítő feladat is, és kiadási tevékenységek teszik lehetővé a parancssor toorun toospecify hello feladat meghívásakor. Ezek segítségével számos különböző funkciót érhet el, például fájlokat tölthet le, emelt jogosultsági szintű futtatást végezhet, egyéni környezeti változókat adhat meg, illetve beállíthatja a maximális végrehajtási időt, az újrapróbálások számát, illetve a fájlmegőrzési időt.

A feladatelőkészítési és -kiadási tevékenységekkel kapcsolatos további információért lásd: [Feladat-előkészítési és -befejezési műveletek futtatása Azure Batch számítási csomópontokon](batch-job-prep-release.md).

### <a name="multi-instance-task"></a>Többpéldányos tevékenység
A [többpéldányos feladat](batch-mpi.md) egy feladat, amely konfigurált toorun egynél több számítási csomóponton egyidejűleg. A többpéldányos feladatokkal, engedélyezheti a nagy teljesítményű számítástechnikai számítási csomópontokat, amelyek csoportja igénylő forgatókönyvek lefoglalt együtt tooprocess egy egyetlen számítási feladat (például a Message Passing Interface (MPI)).

A kötegben MPI-feladatok futtatása hello Batch .NET könyvtár részletes leírását, tekintse meg [használata többpéldányos feladatok toorun Message Passing Interface (MPI) alkalmazások az Azure Batch](batch-mpi.md).

### <a name="task-dependencies"></a>Tevékenységfüggőségek
[Tevékenység-függőségek](batch-task-dependencies.md), mint hello név azt jelenti, teszik lehetővé, amely egy feladat attól függ, más feladatok végrehajtása előtt hello megvalósításának toospecify. Ez a funkció támogatást nyújt a "alsóbb rétegbeli" feladat használ fel egy "fölérendelt" feladat--, vagy egy felsőbb szintű tevékenység hajt végre bizonyos inicializálása egy alsóbb rétegbeli feladat által igényelt hello kimeneti helyzetekben. toouse ezzel a funkcióval kell első engedélyezése feladat függőségek a kötegelt. Ezt követően az egyes feladatok, amelyek elengedhetetlenek az egy másik (vagy sok más), megadhatja a hello feladatokat, amelyek adott feladat függ.

Feladat függőségekkel rendelkező hasonló hello forgatókönyvek is konfigurálhatja:

* A *taskB* a *taskA* tevékenységtől függ (a *taskB* végrehajtása nem kezdődik meg a *taskA* befejeződéséig).
* A *taskC* a *taskA* és a *taskB* tevékenységtől is függ.
* A *taskD* egy tevékenységtartománytól függ, például az *1* – *10.* tevékenység befejeződéséig nem hajtja végre a rendszer.

Tekintse meg [feladatot az Azure Batch függőségek](batch-task-dependencies.md) és hello [TaskDependencies] [ github_sample_taskdeps] hello a kódminta [azure-köteg-minták] [ github_samples] GitHub-tárházban, ez a funkció részletes olvashat.

## <a name="environment-settings-for-tasks"></a>Környezeti beállítások tevékenységekhez
Egyes feladatokat hajtja végre a Batch szolgáltatás hello rendelkezik, amely a számítási csomópontok állítja a hozzáférés tooenvironment változók. Ez magában foglalja a Batch szolgáltatás hello által meghatározott környezeti változók ([szolgáltatás által definiált][msdn_env_vars]) és egyéni környezeti változókat, amelyek a feladatok adhat meg. hello alkalmazásokat és parancsfájlokat, a feladatok végrehajtása rendelkezik hozzáférési toothese környezeti változók végrehajtása során.

Is megadhat egyéni környezeti változók hello feladat vagy feladat szinten hello feltöltése *környezeti beállítások* ezeket az entitásokat tulajdonsága. Lásd például: hello [feladat tooa feladat vehető] [ rest_add_task] művelet (Batch REST API-t) vagy hello [CloudTask.EnvironmentSettings] [ net_cloudtask_env] és [ CloudJob.CommonEnvironmentSettings] [ net_job_env] Batch .NET-tulajdonságokat.

Az ügyfél alkalmazás vagy szolgáltatás úgy szerezheti be a tevékenység környezeti változókat is szolgáltatás határozza meg, és egyéni, hello segítségével [feladat adatainak beolvasása] [ rest_get_task_info] művelet (Batch REST) vagy való hozzáférés Hello [CloudTask.EnvironmentSettings] [ net_cloudtask_env] tulajdonság (Batch .NET). A számítási csomóponton végrehajtása folyamatok férhetnek hozzá ezeket és más környezeti változók hello csomóponton, például a hello ismeri `%VARIABLE_NAME%` (Windows) vagy `$VARIABLE_NAME` (Linux) szintaxisa.

Az összes szolgáltatás által definiált környezeti változót tartalmazó teljes listát megtalálja a [Számítási csomópont környezeti változói][msdn_env_vars] című részben.

## <a name="files-and-directories"></a>Fájlok és könyvtárak
Minden tevékenységhez tartozik egy *munkakönyvtár*, amelyben a tevékenység létrehozza a további fájlokat és alkönyvtárakat, ha ilyenekre szükség van. A munkakönyvtár hello feladat, feldolgozza, hello adatok által futtatott hello programnak tárolására is használható, és hello kimeneti hello feldolgozási hajtja végre. Hello feladat felhasználói fájlok és könyvtárak egy feladat tulajdonosa.

hello Batch szolgáltatás közzétesz egy csomópont hello hello fájlrendszerében része *gyökérkönyvtár*. Feladatok hello gyökérkönyvtár férhetnek hozzá hello hivatkozó `AZ_BATCH_NODE_ROOT_DIR` környezeti változó. A környezeti változók használatával kapcsolatos további információért lásd: [Környezeti beállítások tevékenységekhez](#environment-settings-for-tasks).

hello gyökérkönyvtár neve tartalmazza a következő könyvtárstruktúrát hello:

![Számítási csomópont könyvtárstruktúrája][1]

* **megosztott**: Ez a könyvtár túl biztosít az olvasási/írási hozzáférést*összes* csomóponton futó feladatok. Bármely hello csomóponton futó feladat létrehozása, olvasása, frissítése, és törölje a mappában található fájlokat. Feladatok férhetnek hozzá a könyvtárhoz hello Vezérlőpultjának `AZ_BATCH_NODE_SHARED_DIR` környezeti változó.
* **startup**: az indítási tevékenység ezt a könyvtárat használja munkakönyvtárként. Az összes hello kezdő tevékenység által letöltött toohello csomópont hello fájlok itt találhatók. hello kezdő tevékenység létrehozása, olvasása, frissítése, és törölje a fájlokat a könyvtárban. Feladatok férhetnek hozzá a könyvtárhoz hello Vezérlőpultjának `AZ_BATCH_NODE_STARTUP_DIR` környezeti változó.
* **Feladatok**: hello csomóponton futó minden feladatot hoz létre egy könyvtárat. Hello Vezérlőpultjának hozzáférés `AZ_BATCH_TASK_DIR` környezeti változó.

    Minden tevékenység könyvtárban lévő hello Batch szolgáltatás létrehoz egy könyvtárat (`wd`) egyedi elérési úton megadott hello `AZ_BATCH_TASK_WORKING_DIR` környezeti változó. Ez a könyvtár biztosít az olvasási/írási hozzáférést toohello feladat. hello feladat létrehozása, olvasása, frissítése, és törölje a fájlokat a könyvtárban. Ez a könyvtár megmarad hello alapján *RetentionTime* hello feladathoz megadott korlátozást.

    `stdout.txt`és `stderr.txt`: ezek a fájlok toohello feladat mappa írt hello hello feladat végrehajtása során.

> [!IMPORTANT]
> A csomópont hello készletből való eltávolításakor *összes* a hello hello csomóponton tárolt fájlok törlődnek.
>
>

## <a name="application-packages"></a>Alkalmazáscsomagok
Hello [alkalmazáscsomagok](batch-application-packages.md) funkciójával egyszerűen kezelhető, és az alkalmazások telepítését a készletek toohello számítási csomópontok. Töltse fel és alkalmazások futnak, hello különböző verzióinak kezeli a tevékenységek, beleértve a bináris fájlok, és fájlokat támogatja. Majd automatikusan telepítheti az egy vagy több ezen alkalmazások toohello számítási csomópontok a készlet.

Alkalmazáscsomagok hello készlet és a feladat szinten is megadhat. Amikor készletbe alkalmazáscsomagok ad meg, a hello alkalmazás telepített tooevery csomópont hello készletben. Feladat alkalmazáscsomagok megadásakor hello alkalmazás telepített csak toonodes, amelyek ütemezett toorun legalább egy hello feladat feladatok, hello előtt tevékenység parancssor futtatása.

Kötegelt leírók hello használata Azure Storage toostore az alkalmazáscsomagok részleteit, és azok toocompute csomópontok, így a kód és a felügyeleti terhelést egyszerűsíthető.

toofind további hello alkalmazás csomag funkcióval kapcsolatban, tekintse meg [központi telepítése az alkalmazások toocompute csomópontokat a kötegelt alkalmazáscsomagok](batch-application-packages.md).

> [!NOTE]
> Készlet alkalmazás csomagok tooan hozzáadásakor *meglévő* készlet, az alkalmazás hello csomagok telepített toobe toohello csomópontok újra kell indítani a számítási csomópontok.
>
>

## <a name="pool-and-compute-node-lifetime"></a>Készlet és számítási csomópont élettartama
Az Azure Batch-megoldás tervezésekor rendelkezik toomake a tervezési döntés és tárolókészletek létrehozása, és mennyi ideig számítási csomópontok azokat a készleten belül mindig elérhető.

Hello pontszámot egyik végén az egyes feladatokhoz, amelyek akkor küldenek készlet létrehozása, és hello-készlet törlése, amint a feladatok végrehajtási befejezéséhez. Ez kihasználtságát a lehető legnagyobbra mert hello csomópontok csak foglal le, ha szükséges, és állítsa le, amint betöltik a tétlen. Amíg ez azt jelenti, hogy hello feladat hello csomópontok toobe lefoglalt kell várnia, a feladatok vannak ütemezve, amint csomópontok érhetők el külön-külön, lefoglalt, és hello fontos toonote kezdő tevékenység befejeződött. Kötegelt does *nem* Várjon, amíg a készlet összes csomópontja elérhető feladatok toohello csomópontok hozzárendelése előtt. Így garantálható az összes elérhető csomópont maximális kihasználtsága.

Hello: hello pontszámot másik végén feladatok azonnal elindítani, akkor az hello legmagasabb prioritású virtuális gép, ha hozhat létre egy alkalmazáskészlet időben és elérhetővé tenni a csomópontjainak a feladat elküldése előtt. Ebben a forgatókönyvben feladatok azonnal elindíthatja, de előfordulhat, hogy csomópontok azokat várakozás közben üresjárati elhelyezkedik toobe rendelve.

A változó természetű, ám folyamatos terhelések kezeléséhez általában a fenti két megoldás kombinációját használjuk. Akkor is rendelkezik, és több feladat elküldi, de felfelé vagy lefelé toohello feladat betöltése megfelelően méretezheti hello csomópontok száma (lásd: [méretezés számítási erőforrások](#scaling-compute-resources) a következő szakasz hello). Ez reaktív módon, az aktuális terhelés alapján is elvégezhető, de proaktív módszert is használhat, ha a terhelés előrejelezhető.

## <a name="virtual-network-vnet-and-firewall-configuration"></a>A virtuális hálózat (VNet) és a tűzfal konfigurálása 

Ha az Azure Batch számítási csomópontok készletét, társíthatja hello készlet egy Azure alhálózatának [virtuális hálózathoz (VNet)](../virtual-network/virtual-networks-overview.md). toolearn létre virtuális hálózatok és alhálózatok, bővebben lásd: [hozzon létre egy Azure virtuális hálózatra alhálózatok](../virtual-network/virtual-networks-create-vnet-arm-pportal.md). 

 * hello a készlethez társított virtuális hálózaton kell lennie:

   * Az azonos Azure hello **régió** , hello Azure Batch-fiókhoz.
   * Az azonos hello **előfizetés** , hello Azure Batch-fiókhoz.

* virtuális hálózat támogatott hello típusa attól függ, hogyan készletek lefoglalt hello Batch-fiók esetében:

    - Ha hello készlet foglalási mód a Batch-fiókhoz tooBatch szolgáltatás beállítása, majd egy virtuális hálózat csak toopools hello létre rendelhet **Felhőszolgáltatások konfigurálása**. Emellett hello megadott virtuális hálózat hello klasszikus üzembe helyezési modellel kell létrehozni. Hello Azure Resource Manager üzembe helyezési modellben létre Vnetek nem támogatottak.
 
    - Ha hello tárolókészlet foglalási mód a Batch-fiókhoz tooUser előfizetés van beállítva, akkor hozzárendelheti egy virtuális hálózat csak toopools hello létre **virtuálisgép-konfiguráció**. Hello létre készletek **felhőalapú szolgáltatás konfigurációja** nem támogatottak. hello hello Azure Resource Manager üzembe helyezési modellben vagy a klasszikus üzembe helyezési modellel hello kapcsolódó virtuális hálózatot lehet létrehozni.

    Virtuális hálózat támogatási összefoglalójához táblázat szerint toopool foglalási mód, lásd: hello [tárolókészlet foglalási mód](#pool-allocation-mode) szakasz.

* Ha hello tárolókészlet foglalási mód a Batch-fiókhoz tooBatch szolgáltatást, majd meg kell adnia hello Batch szolgáltatás egyszerű tooaccess engedélyeinek hello virtuális hálózat. hello Vnetet hozzá kell rendelnie hello [Classic Virtual Machine Contributor Role-Based hozzáférés-vezérlés (RBAC)](https://azure.microsoft.com/documentation/articles/role-based-access-built-in-roles/#classic-virtual-machine-contributor) szerepkör toohello Batch szolgáltatás egyszerű. Hello megadott RBAC-szerepkör nem áll rendelkezésre, ha a Batch szolgáltatás hello 400 (hibás kérés) adja vissza. hello szerepköre tooadd hello Azure-portálon:

    1. Jelölje be hello **VNet**, majd **hozzáférés-vezérlés (IAM)** > **szerepkörök** > **virtuális gép közreműködő**  >  **Hozzáadása**.
    2. A hello **engedélyek hozzáadása** panelen, jelölje be hello **virtuális gép közreműködő** szerepkör.
    3. A hello **engedélyek hozzáadása** panelen, keressen a hello kötegelt API. Keresse meg ezek a karakterláncok mindegyikének pedig hello API keresse:
        1. **MicrosoftAzureBatch**.
        2. **Microsoft Azure Batch**. Újabb Azure AD-bérlők ezt a nevet használhatják.
        3. **ddbf3205-c6bd-46ae-8127-60eb93363864** hello azonosítója hello kötegelt API. 
    3. Válassza ki a kötegelt API egyszerű szolgáltatásnév hello. 
    4. Kattintson a **Save** (Mentés) gombra.

        ![Rendelje hozzá a virtuális gép közreműködő szerepkört tooBatch egyszerű szolgáltatásnév](./media/batch-api-basics/iam-add-role.png)


* hello megadott alhálózati kell van-e elegendő szabad **IP-címek** tooaccommodate hello célcsomópontokat száma; Ez azt jelenti, hogy a hello összege hello `targetDedicatedNodes` és `targetLowPriorityNodes` hello címkészlet tulajdonságainak. Hello alhálózat nem rendelkezik elég szabad IP-cím, hello Batch szolgáltatás részben hello készlet számítási csomópontjai hello foglal le, és egy átméretezési hibát ad vissza.

* a megadott hello alhálózati kell engedélyezi a kommunikációt a hello Batch szolgáltatás toobe képes tooschedule feladatok hello számítási csomóponton. Ha kommunikációs toohello számítási csomópontok megtagadta a **hálózati biztonsági csoport (NSG)** társított hello VNet, akkor a Batch szolgáltatás hello túl hello számítási csomópontok hello állapotának beállítása**használhatatlanná**.

* Ha a megadott hello hálózatok vannak társítva **hálózati biztonsági csoportokkal (NSG-k)** és/vagy egy **tűzfal**, akkor néhány fenntartott portokat engedélyezni kell a bejövő kommunikáció:

- A virtuálisgép-konfigurációval létrehozott készletek esetén engedélyezze a 29876-os és a 29877-es portokat, valamint a 22-es portot Linux, illetve a 3389-es portot Windows rendszer esetén. 
- A felhőszolgáltatás-konfigurációval létrehozott készletek esetén engedélyezze a 10100-as, 20100-as és 30100-as portokat. 
- Engedélyezze a kimenő kapcsolatok tooAzure tárolót a 443-as portot. Arról is győződjön meg, hogy az Azure Storage-végpont feloldható bármely, a VNET-et kiszolgáló egyéni DNS-kiszolgáló által. Pontosabban, URL-cím hello `<account>.table.core.windows.net` feloldhatónak kell lennie.

    hello következő táblázatban található hello bejövő tooenable az erőforráskészleteket, amelyekben a virtuálisgép-konfiguráció hello segítségével létrehozott szükséges portok:

    |    Célport(ok)    |    Forrás IP-címe      |    Hozzáad a Batch NSG-ket?    |    A virtuális gép toobe használható szükséges?    |    Felhasználói művelet   |
    |---------------------------|---------------------------|----------------------------|-------------------------------------|-----------------------|
    |    <ul><li>A készletek létre hello virtuálisgép-konfiguráció: 29876, 29877</li><li>A készletek hello felhőalapú szolgáltatás konfigurációja létrehozott: 10100, 20100, 30100</li></ul>         |    Csak a Batch szolgáltatási szerepkör IP-címei |    Igen. Kötegelt NSG-k hozzáadása a hálózati adapterek (NIC) kapcsolódó tooVMs hello szintjén. Ezek az NSG-k csak a Batch szolgáltatási szerepkör IP-címeiről érkező forgalmat engedélyezik. Akkor is, ha ezeket a portokat, a teljes weben hello megnyitásához hello forgalom lesz blokkolnánk a hozzáférését: hello hálózati adaptert. |    Igen  |  Toospecify egy NSG-t, mert a kötegelt lehetővé teszi, hogy csak a kötegelt IP-címek nem kell. <br /><br /> Ha azonban NSG-t ad meg, győződjön meg arról, hogy ezek a portok nyitva vannak a bejövő forgalom számára. <br /><br /> Ha megad *, a forrás IP-cím a NSG hello, kötegelt továbbra is hozzáadja NSG-ket a csatlakoztatott hálózati tooVMs hello szintjén. |
    |    3389, 22               |    Felhasználó, használt gépek hibakeresési célra, úgy, hogy van-e távoli hozzáférési hello virtuális gép.    |    Nem                                    |    Nem                     |    Adja hozzá az NSG-k, ha azt szeretné, hogy toopermit távelérési (RDP/SSH-) toohello virtuális gép.   |                 

    hello a következő táblázat ismerteti, hogy kell-e tooenable toopermit hozzáférés tooAzure tárolási hello kimenő port:

    |    Kimenő port(ok)    |    Cél    |    Hozzáad a Batch NSG-ket?    |    A virtuális gép toobe használható szükséges?    |    Felhasználói művelet    |
    |------------------------|-------------------|----------------------------|-------------------------------------|------------------------|
    |    443    |    Azure Storage    |    Nem    |    Igen    |    Ha minden NSG-ket, majd ellenőrizze, hogy ezt a portot nyitott toooutbound forgalom.    |


## <a name="scaling-compute-resources"></a>A számítási erőforrások méretezése
A [automatikus skálázás](batch-automatic-scaling.md), hello Batch szolgáltatás dinamikusan beállítása hello toohello aktuális munkaterhelés és Erőforrás-kihasználtsága a számítási forgatókönyv szerint készlet számítási csomópontjainak számát is. Ez lehetővé teszi, hogy toolower hello teljes költség szempontjából, az alkalmazás futtatásának csak hello erőforrások használatával van szüksége, és azokat felszabadítása nincs szükség.

Az automatikus skálázáshoz írnia kell egy [automatikus skálázási képletet](batch-automatic-scaling.md#automatic-scaling-formulas), amelyet aztán társítania kell a készlethez. hello Batch szolgáltatás hello képlet toodetermine hello cél csomópontok száma hello készletben hello következő méretezési időköz (az Ön által konfigurált időköz) használja. Megadhat hello automatikus méretezési beállításainak készlet létrehozásakor azt, vagy később készlet méretezésének engedélyezése. Hello skálázás skálázásnak engedélyezve készlet beállításait is frissítheti.

Tegyük fel lehet, hogy egy feladat megköveteli, hogy végre feladatok toobe nagyon nagy számú elküldését. A méretezési képlet toohello készlet, amely beállítja a várólistán lévő feladatok aktuális száma hello és alapuló hello befejezési hello feladatok hello feladat hello készletben csomópontok száma hello rendelhet hozzá. hello Batch szolgáltatás rendszeres időközönként kiértékeli hello képlet és átméretezi hello készletbe, a munkaterhelés és a más képlet beállítások alapján. hello szolgáltatás csomópontok hozzáadása az aszinkron feladatok nagy számú esetén szükség szerint, és nincsenek aszinkron vagy nem futnak feladatok csomópontok eltávolítja.

Méretezési képlet alapulhat a következő metrikák hello:

* **Metrikák idő** megadott hello 5 percenként összegyűjtött statisztikai alapuló órák száma.
* Az **erőforrás-mérőszámok** a CPU-használat, a sávszélesség-használat, a memóriahasználat és a csomópontok száma alapján számíthatók ki.
* A **tevékenységmetrikák** alapját a tevékenységállapotok, például *Aktív* (sorban áll), *Fut* vagy *Befejezve* képezik.

Amikor hello számát a készlet számítási csomópontjainak automatikus skálázás csökkenti, át kell gondolnia, hogyan hello hello idején futó feladatok toohandle csökkentése műveletet. tooaccommodate ennek, biztosít egy *csomópont-felszabadítás beállítás* , amelyeket megadhat a képletekben. Például megadhatja, hogy futó feladatok vannak azonnal leállt, azonnal leállította és majd egy másik csomópontjára végrehajtásra helyezte, vagy engedélyezett toofinish hello készletből hello csomópont eltávolítása előtt.

Az alkalmazások automatikus méretezésével kapcsolatos további információért lásd: [Számítási csomópontok automatikus méretezése egy Azure Batch-készletben](batch-automatic-scaling.md).

> [!TIP]
> toomaximize számítási erőforrás-használat, beállítása hello cél csomópontok toozero hello végén egy feladatot, de a futó feladatok toofinish.
>
>

## <a name="security-with-certificates"></a>Biztonság tanúsítványokkal
Amikor titkosítása vagy visszafejtése tevékenységek, például hello kulcsa bizalmas adatait általában szükség tanúsítványokra toouse egy [Azure Storage-fiók][azure_storage]. toosupport, tanúsítványok csomópontok telepíthető. Titkosított titkos kulcsok tootasks parancssori paraméterek átadása vagy beágyazott hello feladat erőforrások egyike, és hello telepített tanúsítványok csak használt toodecrypt őket.

Hello használata [Hozzáadás tanúsítvány] [ rest_add_cert] művelet (Batch REST) vagy [CertificateOperations.CreateCertificate] [ net_create_cert] metódus (Batch .NET) tooadd egy tanúsítvány tooa Batch-fiókhoz. Majd hello tanúsítványt társíthat egy új vagy meglévő készletbe. Ha egy tanúsítvány társítva egy készletet, Batch szolgáltatás hello telepíti hello tanúsítványt hello készlet minden egyes csomóponton. hello Batch szolgáltatás telepíti a megfelelő tanúsítványok hello indításakor hello csomópont, a feladatok (beleértve a hello kezdő tevékenység és a kezelő feladat) elindítása előtt.

Tanúsítványok tooan hozzáadásakor *meglévő* készletbe tartozó hello alkalmazott toobe toohello csomópontok újra kell indítani a számítási csomópontok.

## <a name="error-handling"></a>Hibakezelés
Előfordulhat, hogy szükséges toohandle feladat, mind az alkalmazás sikertelen a kötegelt megoldás belül.

### <a name="task-failure-handling"></a>Tevékenységhibák kezelése
A tevékenységhibák a következő kategóriákba esnek:

* **Előfeldolgozási hibák**

    Ha egy feladat sikertelen toostart, hello feladat egy előre feldolgozási hiba van beállítva.  

    Előre feldolgozási hibák akkor történhet, ha hello feladat Erőforrásfájlok került, hello tárfiók már nem érhető el, vagy egy másik probléma történt, hogy megakadályozta abban hello sikeres másolását fájlok toohello csomópont.

* **Sikertelen fájlfeltöltés**

    Ha egy feladat a megadott fájlok feltöltése bármilyen okból nem sikerül, fájlfeltöltési hiba hello feladat van beállítva.

    A fájl feltöltése a hiba akkor fordulhat elő, ha hello elérése az Azure Storage érvénytelen, vagy nem rendelkezik írási engedéllyel, ha hello tárfiók már nem érhető el, vagy ha egy másik probléma volt a megadott SAS talált, amely miatt nem sikerült a fájlok másolása sikeres hello hello csomópont.    

* **Alkalmazáshibák**

    hello feladat parancssorból megadott hello folyamat is sikertelen lehet. hello folyamat akkor számít elértnek toohave egy nem nulla kilépési kód érkezésekor hello folyamat, amely hello feladat végrehajtása sikertelen volt (lásd: *feladat kilépési kódot* hello a következő szakaszban).

    Az alkalmazáshibák, konfigurálhatja a kötegelt tooautomatically újrapróbálkozási hello feladat tooa megadott számú alkalommal.

* **Korlátozáshibák**

    Megadhat egy korlátozás, amely meghatározza a feladatokat vagy tevékenységeket, hello maximális végrehajtásának időtartama hello *maxWallClockTime*. Ez lehet hasznos, ha leáll tooprogress sikertelen feladatokhoz.

    Amikor a rendszer túllépte a maximális időt hello, hello feladat be van-e megjelölve *befejeződött*, hello kilépési kód túl van-e állítva, de`0xC000013A` és hello *schedulingError* mező jelölése `{ category:"ServerError", code="TaskEnded"}`.

### <a name="debugging-application-failures"></a>Alkalmazáshibák keresése
* `stderr` és `stdout`

    Végrehajtásakor előfordulhat, hogy a alkalmazás használható tootroubleshoot problémák diagnosztikai kimenetet eredményez. Hello a korábbi szakaszban [fájlok és könyvtárak](#files-and-directories), hello Batch szolgáltatás írja a standard kimenet és a standard hiba túl kimeneti`stdout.txt` és `stderr.txt` hello feladat könyvtárban található fájlok hello a számítási csomópont. Használható hello Azure-portálon vagy az egyik hello kötegelt SDK-k toodownload ezeket a fájlokat. Például le ezeket és más fájlok hibaelhárítási célból használatával [ComputeNode.GetNodeFile] [ net_getfile_node] és [CloudTask.GetNodeFile] [ net_getfile_task] hello Batch .NET könyvtárban.

* **Tevékenységek kilépési kódjai**

    Amint azt korábban említettük, a feladatok hibás jelölést hello Batch szolgáltatás Ha hello feladat által végrehajtott folyamat hello egy nem nulla kilépési kódot ad vissza. A folyamat végrehajtásakor a feladat kötegelt tölti fel a hello feladat kilépési kód tulajdonság hello *visszatérési kód hello folyamat*. Fontos, hogy a tevékenység kilépési kód toonote **nem** hello Batch szolgáltatás határozza meg. Egy feladat kilépési kód hello folyamat saját magát vagy végre hello folyamatok hello operációs rendszere határozza meg.

### <a name="accounting-for-task-failures-or-interruptions"></a>Tevékenységhibák vagy -megszakítások kezelése
A tevékenységek időnként meghiúsulhatnak vagy megszakadhatnak. hello feladat alkalmazás maga sikertelen lehet, hello csomópont, mely hello feladat fut. Előfordulhat, hogy újra kell indítani, vagy hello csomópont esetleg elhagyják hello készlet során egy átméretezés esetén hello készlet felszabadítás házirend beállítása tooremove csomópontok azonnal várakozás nélkül feladatok toofinish. Minden esetben hello feladat is lehet automatikusan helyezte kötegelt végrehajtási egy másik csomópontjára.

Egy időszakos probléma toocause egy feladat toohang lehetőség is, vagy túl hosszú tooexecute igénybe vehet. Hello maximális végrehajtási jelenít meg olyan feladatra, állíthatja be. Hello maximális végrehajtási időköz túllépésekor hello Batch szolgáltatás megzavarja a hello feladat alkalmazás.

### <a name="connecting-toocompute-nodes"></a>Csatlakozás toocompute csomópontok
További hibakeresését vagy hibaelhárítását történő bejelentkezéssel tooa számítási csomópont távolról végezheti el. Hello Azure portál toodownload Remote Desktop Protocol (RDP) fájl használata a Windows-csomópontok számára, és szerezze be a Linux-csomópontok Secure Shell (SSH)-kapcsolódási információt. Is ehhez használatával hello kötegelt API-k – például [Batch .NET] [ net_rdpfile] vagy [kötegelt Python](batch-linux-nodes.md#connect-to-linux-nodes-using-ssh).

> [!IMPORTANT]
> tooconnect tooa csomópont RDP és az SSH segítségével, akkor először létre kell hoznia egy felhasználó hello csomóponton. toodo, használhatja az Azure-portálon hello [tooa csomópont felhasználói fiók hozzáadása] [ rest_create_user] hello Batch REST API használatával hívható meg hello [ComputeNode.CreateComputeNodeUser] [ net_create_user] metódus a Batch .NET vagy a hívás hello [add_user] [ py_add_user] hello kötegelt Python modul metódust.
>
>

### <a name="troubleshooting-problematic-compute-nodes"></a>Problémás számítási csomópontok hibaelhárítása
Olyan esetekben, ahol a műveletek egy része sikertelen a kötegelt ügyfél alkalmazás vagy szolgáltatás ellenőrizheti hello sikertelen feladatok tooidentify átirányítóban csomópont hello metaadatait. A készletbe minden csomópont kap egy egyedi Azonosítót, és hello csomópont, amelyen a feladat futtatása hello feladat metaadatokat tartalmazza. Ha sikerült azonosítani a problematikus csomópontot, számos különböző műveletet elvégezhet vele:

* **Hello csomópont újraindítása** ([REST][rest_reboot] | [.NET][net_reboot])

    Újraindítás hello csomópont néha rejtett problémák, például rögzített vagy lefagyott folyamatok is törlődnek. Vegye figyelembe, hogy a készlet egy kezdő tevékenység vagy a feladatot a feladat előkészítése tevékenység használja, ha végrehajtás hello csomópont újraindításakor.
* **Újból lemezképet létrehozni hello csomópont** ([REST][rest_reimage] | [.NET][net_reimage])

    Ez a Configuration Manager telepíti hello operációs rendszer hello csomóponton. Csakúgy, mint a csomópont újraindul, indítsa el a feladatok és a feladat előkészítése feladatok hello csomópont lemezképet után futtassa újra a rendszer.
* **Távolítsa el a hello csomópont hello készletből** ([REST][rest_remove] | [.NET][net_remove])

    Egyes esetekben szükséges toocompletely eltávolítása hello csomópont hello készletből.
* **Hello csomóponton tevékenységütemezés letiltása** ([REST][rest_offline] | [.NET][net_offline])

    Ez gyakorlatilag bontja hello csomópont kapcsolatot, hogy egy további feladat tooit vannak hozzárendelve, de lehetővé teszi, hogy a hello csomópont tooremain fut, és hello készletben. Ez lehetővé teszi tooperform további vizsgálata hello OK hello hibák hello meghiúsult feladat adatvesztés nélkül, és további feladat hibáihoz okozó hello csomópont nélkül. Például letilthatja tevékenységütemezés hello csomópontjára, majd [jelentkezzen be távolról](#connecting-to-compute-nodes) tooexamine csomópont eseménynaplók hello, vagy más hibaelhárítás. Miután befejezte a vizsgálatot, majd helyezheti hello csomópont újra online tevékenységütemezés engedélyezése ([REST][rest_online] | [.NET] [ net_online]), vagy a korábban tárgyalt egyéb műveleteket hajthat végre hello egyikét.

> [!IMPORTANT]
> Minden – ebben a szakaszban ismertetett művelettel újraindítás, a lemezkép alaphelyzetbe, távolítsa el, és tiltsa le a feladat ütemezés--, képes toospecify hello csomóponton futó feladatok kezelésének módja hello művelet végrehajtásakor. Például, ha letilt tevékenységütemezés a csomópont használatával hello Batch .NET ügyféloldali kódtár, megadhat egy [DisableComputeNodeSchedulingOption] [ net_offline_option] felsorolási érték toospecify, hogy túl**Megszakítás miatt** futó feladatok, **újra a várólistába helyezi** a többi csomóponton ütemezés őket, vagy engedélyezheti a futó feladatok toocomplete hello művelet végrehajtása előtt (**TaskCompletion**).
>
>

## <a name="next-steps"></a>Következő lépések
* További tudnivalók: hello [kötegelt API-k és eszközök](batch-apis-tools.md) kötegelt megoldások érhetők el.
* A kötegelt mintaalkalmazást a részletes útmutatót [.NET Ismerkedés az Azure Batch könyvtár hello](batch-dotnet-get-started.md). Is egy [Python verzió](batch-python-tutorial.md) hello oktatóanyag, a munkaterhelés Linux rendszeren futó számítási csomópontjain.
* Töltse le és build hello [kötegelt Explorer] [ github_batchexplorer] mintaprojektet használható, amíg a kötegelt megoldásokat fejleszt. Hello kötegelt Explorer használja, végezhet hello alábbi, és több:

  * Készletek, feladatok és tevékenységek megfigyelése és módosítása a Batch-fiókban
  * A `stdout.txt`, az `stderr.txt` és más fájlok letöltése csomópontokról
  * Felhasználók létrehozása csomópontokon és RDP-fájlok letöltése távoli bejelentkezéshez
* Ismerje meg, hogyan túl[létrehozása Linux számítási csomópontok készleteinek](batch-linux-nodes.md).
* A Microsoft hello [Azure Batch fórum] [ batch_forum] az MSDN Webhelyén. hello fórum jelenti jó tooask kérdéseket, hogy most ismerkedik, vagy a kötegelt használatával szakértő.

[1]: ./media/batch-api-basics/node-folder-structure.png

[azure_storage]: https://azure.microsoft.com/services/storage/
[batch_forum]: https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurebatch
[cloud_service_sizes]: ../cloud-services/cloud-services-sizes-specs.md
[msmpi]: https://msdn.microsoft.com/library/bb524831.aspx
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_sample_taskdeps]:  https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/TaskDependencies
[github_batchexplorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[batch_net_api]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[msdn_env_vars]: https://msdn.microsoft.com/library/azure/mt743623.aspx
[net_cloudjob_jobmanagertask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobmanagertask.aspx
[net_cloudjob_priority]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.priority.aspx
[net_cloudpool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx
[net_cloudtask_env]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.environmentsettings.aspx
[net_create_cert]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificateoperations.createcertificate.aspx
[net_create_user]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.createcomputenodeuser.aspx
[net_getfile_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.getnodefile.aspx
[net_getfile_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.getnodefile.aspx
[net_job_env]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.commonenvironmentsettings.aspx
[net_multiinstancesettings]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.aspx
[net_onalltaskscomplete]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.onalltaskscomplete.aspx
[net_rdp]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.getrdpfile.aspx
[net_reboot]: https://msdn.microsoft.com/library/azure/mt631495.aspx
[net_reimage]: https://msdn.microsoft.com/library/azure/mt631496.aspx
[net_remove]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.removefrompoolasync.aspx
[net_offline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.disableschedulingasync.aspx
[net_online]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.enableschedulingasync.aspx
[net_offline_option]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.disablecomputenodeschedulingoption.aspx
[net_rdpfile]: https://msdn.microsoft.com/library/azure/Mt272127.aspx
[vnet]: https://msdn.microsoft.com/library/azure/dn820174.aspx#bk_netconf

[py_add_user]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.ComputeNodeOperations.add_user

[batch_rest_api]: https://msdn.microsoft.com/library/azure/Dn820158.aspx
[rest_add_job]: https://msdn.microsoft.com/library/azure/mt282178.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[rest_add_cert]: https://msdn.microsoft.com/library/azure/dn820169.aspx
[rest_add_task]: https://msdn.microsoft.com/library/azure/dn820105.aspx
[rest_create_user]: https://msdn.microsoft.com/library/azure/dn820137.aspx
[rest_get_task_info]: https://msdn.microsoft.com/library/azure/dn820133.aspx
[rest_job_schedules]: https://msdn.microsoft.com/library/azure/mt282179.aspx
[rest_multiinstance]: https://msdn.microsoft.com/library/azure/mt637905.aspx
[rest_multiinstancesettings]: https://msdn.microsoft.com/library/azure/dn820105.aspx#multiInstanceSettings
[rest_update_job]: https://msdn.microsoft.com/library/azure/dn820162.aspx
[rest_rdp]: https://msdn.microsoft.com/library/azure/dn820120.aspx
[rest_reboot]: https://msdn.microsoft.com/library/azure/dn820171.aspx
[rest_reimage]: https://msdn.microsoft.com/library/azure/dn820157.aspx
[rest_remove]: https://msdn.microsoft.com/library/azure/dn820194.aspx
[rest_offline]: https://msdn.microsoft.com/library/azure/mt637904.aspx
[rest_online]: https://msdn.microsoft.com/library/azure/mt637907.aspx

[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/
