---
title: "aaaUse hello Azure Batch megjelenítése szolgáltatás toorender hello felhőben |} Microsoft Docs"
description: "Renderelési feladatok végezhetők Azure-alapú virtuális gépeken, közvetlenül a Maya szoftverből és használatalapú fizetéssel."
services: batch
author: tamram
manager: timlt
ms.service: batch
ms.topic: hero-article
ms.date: 07/31/2017
ms.author: tamram
ms.openlocfilehash: 3fb78d883311bbc3ab62743b7d1b111ffad177cd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-batch-rendering-service"></a>Ismerkedés a hello szolgáltatás kötegelt megjelenítése

hello Azure kötegben megjelenítést szolgáltatás használati fizetési alapon felhőméretű képességeit kínál. hello szolgáltatás kötegelt megjelenítése kezelje a feladatütemezés és várólistázást, kezelése hibák és az újrapróbálkozások és automatikus méretezése a leképezési feladat. hello szolgáltatás kötegelt megjelenítése Autodesk Maya, 3ds támogatja a Max és Arnold támogatása hamarosan elérhető más alkalmazások. hello Maya 2017 beépülő modul kötegelt teszi, hogy könnyen toostart egy megjelenítési feladat Azure jobb az asztalról. 

## <a name="supported-applications"></a>Támogatott alkalmazások köre

hello szolgáltatás kötegelt megjelenítése jelenleg a következő alkalmazások hello támogatja:

- Autodesk Maya
- Autodesk 3ds Max
- Autodesk Arnold

## <a name="prerequisites"></a>Előfeltételek

toouse hello kötegelt megjelenítése szolgáltatás, lesz szüksége:

- Egy [Azure-fiók](https://azure.microsoft.com/free/). 
- **Egy Azure Batch-fiók.** A Batch-fiók létrehozása az Azure-portálon hello útmutatóért lásd: [Batch-fiók létrehozása az Azure-portálon hello](batch-account-create-portal.md).
- **Egy Azure Storage-fiók.** a Megjelenítés feladathoz használt hello eszközeinek tárolására az Azure Storage. A Batch-fiók beállításakor automatikusan létrehozható egy tárfiók. Vagy használhat egy létező tárfiókot is. További információ a tárfiókok, toolearn lásd: [hogyan toocreate, kezelése vagy törlése az Azure-portálon hello](https://docs.microsoft.com/azure/storage/storage-create-storage-account).

Kötegelt toouse hello Maya beépülő modult, van szüksége:

- **Maya 2017**
- **Arnold for Maya**

Is használhatja a hello [Azure-portálon](https://portal.azure.com) toocreate készletek, amelyek a Maya, 3ds előre konfigurált virtuális gépek maximális és Arnold. Hello portál toomonitor feladatok, és sikertelen feladatokhoz diagnosztizálása alkalmazásnaplók letöltésével és távolról csatlakozzon a tooindividual virtuális gépek RDP és az SSH használatával.

## <a name="basic-batch-concepts"></a>Alapszintű Batch-fogalmak

Hello kötegelt megjelenítése szolgáltatás használata előtt hasznos toobe ismeri a kötegelt néhány fogalommal, beleértve a számítási csomópontokat, a készletek és a feladatok is. További információk az Azure Batch általában, lásd: toolearn [belsőleg párhuzamos alkalmazásokat és szolgáltatásokat futtathatnak kötegelt](batch-technical-overview.md).

### <a name="pools"></a>Készletek

A Batch egy platformszolgáltatás nagy számítási igényű munkák, pl. renderelés egy **számítási csomópontokból** álló **készleten** történő futtatásához. Egy készleten belül minden számítási csomópont egy Azure-beli virtuális gép (VM), amely Linux vagy Windows rendszert futtat. 

Kötegelt készletek és a számítási csomópontok kapcsolatos további információkért lásd: hello [készlet](batch-api-basics.md#pool) és [számítási csomópont](batch-api-basics.md#compute-node) szakaszában [Develop nagyméretű párhuzamos számítási solutions a kötegelt](batch-api-basics.md).

### <a name="jobs"></a>Feladatok

A kötegelt **feladat** hello futó feladatok egy gyűjteményének számítási csomópontjain a készletben. Megjelenítés feladat elküldése, amikor a kötegelt hello feladat osztja a feladatokat, és hello feladatok toohello számítási csomópontjai hello készlet toorun terjeszti.

Kötegelt feladatok kapcsolatos további információkért lásd: hello [feladat](batch-api-basics.md#job) szakasz [Develop nagyméretű párhuzamos számítási solutions a kötegelt](batch-api-basics.md).

## <a name="use-hello-batch-plug-in-for-maya-toosubmit-a-render-job"></a>Hello kötegelt beépülő modul használata Maya toosubmit leképezési feladat

Hello kötegelt Maya beépülő modul egy feladat toohello kötegelt megjelenítése szolgáltatás közvetlenül a Maya elküldheti. hello a következő szakaszok ismertetik, hogyan tooconfigure hello beépülő modul hello feladatot, és küldje el. 

### <a name="load-hello-batch-plug-in-in-maya"></a>Hello Maya beépülő modulja kötegelt betöltése

hello beépülő modul kötegelt érhető el a [GitHub](https://github.com/Azure/azure-batch-maya/releases). Bontsa ki a hello archív tooa könyvtár az Ön által választott. Közvetlenül a hello hello beépülő modul betöltése *azure_batch_maya* könyvtár.

tooload hello Maya beépülő modulja:

1. Indítsa el a Maya alkalmazást.
2. Nyissa meg a **Window** > **Settings/Preferences** > **Plug-in Manager** (Ablak/Beállítások/Beépülő modulok kezelése) elemet.
3. Kattintson a **Browse** (Tallózás) gombra.
4. Keresse meg a tooand válassza *azure_batch_maya/plug-in/AzureBatch.py*.

### <a name="authenticate-access-tooyour-batch-and-storage-accounts"></a>Hozzáférés tooyour kötegelt és a Storage-fiókok hitelesítéséhez

toouse hello beépülő modul, az Azure Batch és Azure tárfiókkulcsok tooauthenticate kell. tooretrieve a kulcsait:

1. Megjelenítési hello Maya beépülő modulja, és jelölje be hello **Config** fülre.
2. Keresse meg a toohello [Azure-portálon](https://portal.azure.com).
3. Válassza ki **Batch-fiókok** hello bal oldali menüből. Ha szükséges, kattintson a **További szolgáltatások** elemre, majd állítsa be a _Batch_ szűrést.
4. Keresse meg a Batch-fiók szükséges hello hello listában.
5. Jelölje be hello **kulcsok** menüelem-toodisplay a fiók nevét, a fiók URL-címe és a hívóbetűk:
    - Hello Batch-fiók URL-címe beillesztése hello **szolgáltatás** hello beépülő modul kötegelt mezőbe.
    - Beillesztés hello fiók nevét hello **Batch-fiók** mező.
    - Beillesztés hello elsődleges kulcsa a hello **kötegelt kulcs** mező.
7. Válassza ki a Storage-fiókok hello bal oldali menüből. Ha szükséges, kattintson a **További szolgáltatások** elemre, majd állítsa be a _Storage_ szűrést.
8. Keresse meg a szükséges hello tárfiók hello listában.
9. Jelölje be hello **hívóbetűk** menüelem toodisplay hello tárolási fiók neve és a kulcsok.
    - Beillesztés hello Tárfiók neve a hello **Tárfiók** hello beépülő modul kötegelt mezőbe.
    - Beillesztés hello elsődleges kulcsa a hello **Biztonságitár-kulcs** mező.
10. Kattintson a **hitelesítés** , hogy a beépülő modul hello tooensure fiókot is hozzáférhet.

Miután sikeresen hitelesítette, hello beépülő modul készlet túl hello-e állapotmező**hitelesített**: 

![A Batch- és Storage-fiókok hitelesítése](./media/batch-rendering-service/authentication.png)

### <a name="configure-a-pool-for-a-render-job"></a>Egy készlet konfigurálása egy renderelési feladathoz

A Batch- és Storage-fiókok hitelesítését követően állítson be egy készletet a renderelési feladathoz. beépülő modul hello menti a beállításokat a munkamenetek között. Miután beállította a javasolt konfiguráció, akkor nem kell toomodify, kivéve, ha módosítja.

hello alábbiakban végigvezetik Önt hello elérhető lehetőségekről, a hello **Submit** lapon:

#### <a name="specify-a-new-or-existing-pool"></a>Új vagy létező készlet megadása

a készlet mely toorun hello leképezési feladathoz, jelölje be hello toospecify **Submit** fülre. Ezen a lapon lehetősége van létrehozni egy új készletet, vagy kiválasztani egy meglévő készletet:

- Is **automatikus kiépítése a feladat készlet** (hello az alapértelmezett beállítás). Ha ezt a lehetőséget választja, a kötegelt kizárólag a tárolókészletben hello hello jelenlegi feladatot hoz létre, és automatikusan törli hello-készlet hello leképezési feladat befejeződött. Ez a beállítás akkor ajánlott, ha egy egyetlen leképezési feladat toocomplete rendelkezik.
- **Újrahasznosíthat egy meglévő állandó készletet.** Ha egy meglévő készlet által üresjáratban, megadhatja, hogy a tárolókészletben futó hello leképezési feladat hello legördülő menüből való kiválasztása. Hello szükséges idő tooprovision hello készlet újból felhasználja a meglévő állandó készlet menti.  
- **Létrehozhat egy új állandó készletet.** Ez a beállítás hoz létre egy új készletet hello feladat futtatásához. Azt nem hello-készlet törlése, ha hello feladat befejeződött, így a későbbi feladatokhoz is felhasználhatja. Válassza ezt a beállítást, ha a feladatok megjelenítési folyamatosan szükség toorun. A további feladat választhat **felhasználhatja egy létező állandó címkészlet** toouse hello állandó hello első feladathoz létrehozott alkalmazáskészletet.

![Készlet, operációsrendszer-kép, virtuálisgép-méret és licenc megadása](./media/batch-rendering-service/submit.png)

Hogyan díjak keletkeznek Azure virtuális gépek további információkért lásd: hello [Linux árképzési GYIK](https://azure.microsoft.com/pricing/details/virtual-machines/linux/#faq) és [Windows árképzési GYIK](https://azure.microsoft.com/pricing/details/virtual-machines/windows/#faq).

#### <a name="specify-hello-os-image-tooprovision"></a>Adja meg a hello az operációs rendszer lemezképének tooprovision

Hello hello készletet adhat meg az operációs rendszer lemezképének toouse tooprovision számítási csomópontok hello típusú **Env** (környezet) lapon. Kötegelt jelenleg a kép beállítások megjelenítése a feladat a következő hello támogatja:

|Operációs rendszer  |Kép  |
|---------|---------|
|Linux     |Batch CentOS Preview |
|Windows     |Batch Windows Preview |

#### <a name="choose-a-vm-size"></a>Virtuális gép méretének kiválasztása

Megadhat hello Virtuálisgép-méretet hello **Env** fülre. Az elérhető virtuálisgép-méretekkel kapcsolatos bővebb információkért olvassa el az [Azure-beli linuxos virtuális gépek méreteit](https://docs.microsoft.com/azure/virtual-machines/linux/sizes), illetve az [Azure-beli windowsos virtuális gépek méreteit](https://docs.microsoft.com/azure/virtual-machines/windows/sizes) ismertető cikket. 

![Adja meg a virtuális gép operációsrendszer-lemezképek hello és mérete hello Env lapon](./media/batch-rendering-service/environment.png)

#### <a name="specify-licensing-options"></a>Licencelési beállítások megadása

Megadhatja a hello toouse kívánja hello licencek **Env** fülre. A lehetőségek a következők:

- **Maya** – ez a beállítás alapértelmezés szerint engedélyezve van.
- **Arnold**, amelyen engedélyezve van, ha Arnold hello aktív leképezési motor Maya észleli.

 Ha a saját licenc használata toorender, konfigurálhatja a licenc végpont hello megfelelő környezeti változók toohello tábla hozzáadásával. Lehet, hogy toodeselect hello alapértelmezett licencelési lehetőségek erre.

> [!IMPORTANT]
> Díját is felszámítjuk hello licencek használatra közben a virtuális gépek futnak a hello készletben, akkor is, ha a virtuális gépek hello nem éppen használatban vannak a megjelenítés. tooavoid felesleges költségek, keresse meg a toohello **készletek** lapra, és méretezze át hello készlet too0 csomópontok toorun készen áll egy másik leképezési feladat. 
>
>

#### <a name="manage-persistent-pools"></a>Állandó készletek kezelése

Kezelheti egy meglévő állandó készletet hello **készletek** fülre. Ha kijelöl egy alkalmazáskészlet hello listáról állapota hello aktuális hello készlet.

A hello **készletek** lapon is hello-készlet törlése és méretezze át a virtuális gépek hello készletben hello száma. Egy készlet too0 csomópontok tooavoid ezzel járó költségek Between munkaterhelések is átméretezhetők.

![Készletek megtekintése, átméretezése és törlése](./media/batch-rendering-service/pools.png)

### <a name="configure-a-render-job-for-submission"></a>Egy renderelési feladat konfigurálása küldéshez

Többször megadott hello paramétereinek hello készletet, majd hello leképezési feladat futtatása hello feladat magát. 

#### <a name="specify-scene-parameters"></a>Jelenetparaméterek megadása

hello beépülő modul kötegelt melyik jelenleg a képmegjelenítő motor megkeresi Maya, és megjeleníti hello megfelelő megjelenítési beállítások hello **Submit** fülre. Ezek a beállítások tartalmazzák a hello start keretet, a záró keret, a kimeneti előtag és a keret lépés. Hello helyszín fájl megjelenítési beállítások hello beépülő modult ad meg a különböző beállításokat lehet felülbírálni. Változtatások toohello beépülő modul beállításai csak nem megőrzött hátsó toohello helyszín leképezési beállításai, így a módosításokat végezheti el a feladat-feladat alapon tooreupload hello helyszín fájl anélkül.

beépülő modul hello figyelmezteti, ha hello Maya kiválasztott motor leképezése nem támogatott.

Ha egy új helyszín betölti beépülő modul hello meg van nyitva, kattintson a hello **frissítése** gomb toomake meg arról, hogy hello-beállítások frissítését.

#### <a name="resolve-asset-paths"></a>Adategységek elérési útjainak feloldása

Amikor hello beépülő modul betöltéséhez hello helyszín fájl bármely külső fájl hivatkozásainak vizsgálatokat végez. Ezek a hivatkozások jelennek meg hello **eszközök** fülre. A hivatkozott elérési útja nem oldható fel, ha a beépülő modul hello próbálkozások toolocate hello fájl néhány alapértelmezett helyen, beleértve:

- hello hello helyszín fájl helye 
- hello jelenlegi projekt _sourceimages_ könyvtár
- hello aktuális munkakönyvtárban. 

Ha hello eszköz még nem található, akkor egy figyelmeztető ikon szerepel:

![A hiányzó adategységek mellett egy figyelmeztető ikon jelenik meg](./media/batch-rendering-service/missing_assets.png)

Ha ismeri az feloldatlan fájlhivatkozás hello helyét, kattintson a figyelmeztető ikon toobe kéri tooadd a keresési elérési út hello. beépülő modul majd hello használja a keresési elérési út tooattempt tooresolve a hiányzó eszközöket. Tetszőleges számú további keresési útvonalat lehet megadni.

Ha egy hivatkozást a rendszer feloldott, akkor egy zöld jelzőlámpa ikon jelenik meg mellette:

![A feloldott adategységek mellett egy zöld jelzőlámpa ikon látható](./media/batch-rendering-service/found_assets.png)

Ha a leképezni kívánt jelenetben szükséges egyéb fájlokat, hogy hello beépülő modul nem talált, hozzáadhat további fájlokat vagy könyvtárakat. Használjon hello **fájlok hozzáadása** és **könyvtár hozzáadása** gombokat. Ha egy új helyszín betölti beépülő modul hello meg van nyitva, lehet, hogy tooclick **frissítése** tooupdate hello helyszín hivatkozik.

#### <a name="upload-assets-tooan-asset-project"></a>Eszközök tooan eszköz projekt feltöltése

Ha Ön leképezési feladat elküldése, hello hivatkozott hello megjelenő fájlok **eszközök** lapon automatikusan feltöltött tooAzure eszköz projektként tárolási. Is feltöltheti hello adategység-fájloknak a leképezési feladat függetlenül hello segítségével **feltöltése** hello gombjára **eszközök** külön-külön hello eszköz projekt neve van megadva az hello **projekt**mezőben, és alapértelmezés szerint hello aktuális Maya projekt neve. Ha az eszköz fájlok feltöltése után hello helyi fájlstruktúra megőrződik. 

A feltöltött adategység-fájlokra bármennyi renderelési feladat hivatkozhat. Olyan rendelkezésre álló tooany feladat hello eszköz projekt, hivatkozó összes feltöltött eszközök hello helyszín szerepelnek-e. toochange hello eszköz projekt hivatkozik a következő feladat hello nevének módosítása a hello **projekt** hello mezőjét **eszközök** fülre. Ha nincsenek hivatkozott fájlokat töltsenek tooexclude kívánja, törölje azokat hello listaelem mellett zöld hello gomb használatával.

#### <a name="submit-and-monitor-hello-render-job"></a>Küldje el, és a figyelő hello leképezési feladat

Beépülő modul hello hello leképezési feladat konfigurálása után kattintson a hello **feladat elküldése** hello gombjára **Submit** toosubmit hello feladat tooBatch lapon:

![Hello leképezési feladat elküldése](./media/batch-rendering-service/submit_job.png)

Egy feladatot, amely a hello folyamatban van a figyelheti **feladatok** beépülő modul hello lapján. Jelölje ki a feladatot a hello lista toodisplay hello aktuális hello feladat állapota. Is ezen a lapon toocancel használja, és törölje a feladatok esetében, valamint a toodownload hello kimenetek és a naplók megjelenítése. 

toodownload kimenetek módosítása hello **kimenete** mező tooset hello kívánt célkönyvtáron. Kattintson a hello fogaskerék ikonra toostart háttérfolyamatként, amely figyeli a hello feladat és kimenetek letölti a végrehajtás során: 

![Feladat állapotának megtekintése és kimenetek letöltése](./media/batch-rendering-service/jobs.png)

Bezárhatja a Maya hello letöltési folyamat megszakítása nélkül.

## <a name="next-steps"></a>Következő lépések

toolearn kötegelt, kapcsolatos további információkért lásd: [belsőleg párhuzamos alkalmazásokat és szolgáltatásokat futtathatnak kötegelt](batch-technical-overview.md).
