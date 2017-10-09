---
title: "aaaManage a StorSimple-köteteket (U2) |} Microsoft Docs"
description: "Azt ismerteti, hogyan tooadd, módosítás, figyelése, és törölje a StorSimple-köteteket, és hogyan tootake őket, offline állapotba, ha a szükséges."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 57896932-0aa5-4805-970c-d13403ae7551
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/28/2016
ms.author: alkohli
ms.openlocfilehash: 8b64f1d92023646cdf881882854d9bc5d7334a10
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-toomanage-volumes-update-2"></a>Hello StorSimple Manager szolgáltatás toomanage köteteket (2. frissítés)
[!INCLUDE [storsimple-version-selector-manage-volumes](../../includes/storsimple-version-selector-manage-volumes.md)]

## <a name="overview"></a>Áttekintés
Ez az oktatóanyag azt ismerteti, hogy a toouse hogyan StorSimple Manager szolgáltatás toocreate hello és kötetek hello StorSimple eszközön, és a StorSimple virtuális eszköz kezelése frissítés 2-es telepítve.

hello StorSimple Manager szolgáltatás a klasszikus Azure-portál, amely lehetővé teszi a StorSimple megoldásban egy egyetlen webes felhasználói felületen keresztüli kezelését hello bővítmény. Továbbá toomanaging kötetek, a is hello StorSimple Manager szolgáltatás toocreate használja és StorSimple szolgáltatások kezelése, megtekintése és eszközök kezeléséhez, megtekintheti a riasztásokat, és megtekinthető és kezelhető biztonsági mentési házirendek és hello biztonságimásolat-katalógus.

## <a name="volume-types"></a>Kötet típusok
StorSimple-köteteket lehet:

* **Helyileg rögzített kötetek**: hello helyi StorSimple eszközön mindig továbbra is ezeken a köteteken lévő adatok.
* **Rétegzett kötet**: ezek a kötetek adatainak toohello felhő is kerülnek.

Az archiválási köteten a rétegzett kötet típusú. hello nagyobb deduplikációs adattömbméret archiválási kötetek használt lehetővé teszi, hogy a hello eszköz tootransfer nagyobb szegmensek adatok toohello felhő. 

Ha szükséges, a hello kötettípus helyi tootiered vagy a rétegzett toolocal módosíthatja. További információ: túl[hello kötet típusának módosítása](#change-the-volume-type).

### <a name="locally-pinned-volumes"></a>Helyileg rögzített kötetek
Helyileg rögzített kötetek, amely nem réteg adatok toohello felhő teljesen kiosztott köteteket, ezzel biztosítva a helyi biztosítja, hogy az elsődleges adatok, a felhő kapcsolat független. A helyileg rögzített kötetekhez adatok nem deduplikált, és tömörített; azonban a pillanatképeket a helyileg rögzített kötetek deduplikálásának. 

Helyileg rögzített kötetekhez teljesen kiépített; ezért rendelkeznie kell elegendő lemezterület az eszközön a létrehozott. Megadhat tooa maximális méretét hello StorSimple 8100 eszközön 8 TB és 20 TB hello 8600 eszközön helyileg rögzített kötetekhez. StorSimple hello helyi fennmaradó helyből a pillanatképek, a metaadatok és az adatok feldolgozása hello eszköz foglalja le. Hello mérete egy helyileg rögzített kötet toohello maximális rendelkezésre álló lemezterület növelhető, de egy kötet létrehozását követően hello mérete nem csökkenthető.

Ha helyileg rögzített kötetet hoz létre, hello a rétegzett kötetek létrehozásához rendelkezésre álló terület csökken. fordított hello akkor is igaz: Ha a meglévő rétegzett kötetek, előfordulhat, hogy létrehozása helyileg rögzített kötetek hello terület alacsonyabb, mint a fent megadott maximális határok hello lesz-e. A helyi köteteken további információkért tekintse meg a toohello [gyakran ismételt kérdések a helyileg rögzített kötetekhez](storsimple-local-volume-faq.md).   

### <a name="tiered-volumes"></a>Rétegzett kötetek
Rétegzett kötetek mely hello gyakran elért adatok helyben hello eszközön, és kevesebb, mint a gyakran használt adatokat a rendszer automatikusan a rétegzett toohello felhő dinamikusan kiosztott köteteket. Dinamikus kiosztás egy olyan virtualizációs technológia, rendelkezésre álló tár jelenik meg tooexceed fizikai erőforrásokat. Ahelyett, hogy elegendő tárhely előre foglalással, a StorSimple éppen elegendő toomeet aktuális lemezterület használ vékony létesítési tooallocate. felhőalapú tárolás rugalmas jellege hello elősegíti a ezt a módszert használja, mert StorSimple növelheti vagy csökkentheti a felhőalapú tárolás toomeet tartalomkérései változó igényeinek.

Használata rétegzett kötetet archív adatokhoz, hello kiválasztásával hello **kötet használata ritkábban használt archív adatokhoz** jelölőnégyzetet módosítja hello deduplikációs adattömbméret a kötethez tartozó too512 KB. Ha nem ezt a beállítást, a hello megfelelő rétegzett kötet 64 KB adattömbméretet fogja használni. Egy nagyobb deduplikációs adattömbméret lehetővé teszi, hogy a nagy mennyiségű archiválási adatok toohello felhő hello eszköz tooexpedite hello átvitelét.

> [!NOTE]
> A frissítés előtti StorSimple 2 verziójával létrehozott archiválási kötetek fog importálható, mert rétegzett hello archiválási jelölőnégyzet bejelölésével.
> 
> 

### <a name="provisioned-capacity"></a>Kiosztott kapacitást
Tekintse meg a következő táblázat az egyes eszköz és a kötet maximális kiosztott kapacitást toohello. (Vegye figyelembe, hogy helyileg rögzített kötetekhez ne legyenek elérhetők a virtuális eszköz.)

|  | Maximális rétegzett kötet mérete | Maximális helyileg rögzített kötet mérete |
| --- | --- | --- |
| **Fizikai eszköz** | | |
| 8100 |64 TB |8 TB |
| 8600 |64 TB |20 TB |
| **Virtuális eszközök** | | |
| 8010 |30 TB |N/A |
| 8020 |64 TB |N/A |

## <a name="hello-volumes-page"></a>hello kötetek lap
Hello **kötetek** lap lehetővé teszi a toomanage hello tárolókötetek hello Microsoft Azure StorSimple eszközön a kezdeményezőktől (kiszolgálóktól) törlődnek. Hello kötetek listáját jeleníti meg a StorSimple eszközt.

 ![Kötetek lap](./media/storsimple-manage-volumes-u2/VolumePage.png)

A kötet attribútumainak állnak:

* **Kötet neve** – egy leíró nevet, amely egyedinek kell lennie, és azonosíthatja a hello kötet. Ez a név monitoring jelentések, amikor egy adott köteten szűrheti is szerepel.
* **Állapot** – lehet online vagy offline állapotú. Ha egy kötet offline állapotban, nincs látható tooinitiators (kiszolgálók), amelyek számára engedélyezett a hozzáférés toouse hello kötet.
* **Kapacitás** – hello hello kezdeményező (kiszolgáló) tárolt adatok teljes mennyisége határozza meg. Helyileg rögzített kötetek teljesen kiépített és hello StorSimple eszközön találhatók. Rétegzett kötetek kiosztása és hello adatok deduplikált. A dinamikusan kiosztott köteteket az eszköz nem előre lefoglalni fizikai tárolási kapacitás belsőleg vagy hello felhő tooconfigured kötet kapacitása szerint. hello kötet kapacitása lefoglalt és felhasznált igény szerint.
* **Típus** – azt jelzi, hogy hello kötet **rétegzett** (alapértelmezett hello) vagy **helyileg rögzített**.
* **Biztonsági mentési** – azt jelzi, hogy létezik-e alapértelmezett biztonsági mentési házirend hello kötet.
* **Hozzáférés** – Itt adhatja meg, amelyek számára engedélyezett a hozzáférés toothis kötet hello kezdeményezőktől (kiszolgálóktól). Kezdeményezők, amelyek nem tagjai a hozzáférés-vezérlési rekordot (ACR) társított hello kötet nem jelenik meg a hello kötet.
* **Figyelési** – adja meg a kötet figyel-e. Egy kötet lesz figyelési létrehozásakor alapértelmezés szerint engedélyezett. Lesz, azonban figyelés, a kötet másolat le kell tiltani. hello utasításokat követve tooenable egy kötet, figyelés [kötet figyelése](#monitor-a-volume). 

Ez a következő feladatok oktatóanyag tooperform hello használata hello utasításait:

* Kötet hozzáadása 
* A kötet módosítása 
* Hello kötet típusának módosítása
* Kötet törlése 
* A kötet offline állapotba helyezése 
* A kötet figyelése 

## <a name="add-a-volume"></a>Kötet hozzáadása
Ön [kötet létrehozott](storsimple-deployment-walkthrough-u2.md#step-6-create-a-volume) a StorSimple megoldás üzembe helyezése során. Hasonló eljárással hozzáadna egy kötetet.

#### <a name="tooadd-a-volume"></a>a kötet tooadd
1. A hello **eszközök** lapon, válassza ki azt hello eszközt, kattintson rá duplán, és kattintson a hello **Kötettárolók** fülre.
2. Válasszon ki egy kötettárolót hello listából, és kattintson rá duplán tooaccess hello hello tárolóhoz társított kötetek.
3. Kattintson a **Hozzáadás** hello lap hello alján. Elindítja a hello kötet hozzáadása varázslóban.
   
     ![Vegye fel a kötet varázsló alapbeállítások](./media/storsimple-manage-volumes-u2/TieredVolEx.png)
4. Hello hozzá egy kötet varázslóban a **alapbeállítások**, a következő hello:
   
   1. Adjon **nevet** a kötetnek.
   2. Válassza ki a **használat típusa** hello legördülő listából. Mindig helyileg hello eszközön elérhető adatok toobe igénylő munkaterhelésekhez, válassza ki a **helyileg rögzített**. Minden más típusú adatok, válassza a **rétegzett**. (**Rétegzett** hello alapértelmezett.)
   3. Ha a kiválasztott **rétegzett** a 2. lépésben, kiválaszthatja a hello **kötet használata ritkábban használt archív adatokhoz** jelölőnégyzetet tooconfigure az archiválási köteten.
   4. Adja meg a hello **kiosztott kapacitást** a kötet GB-ban vagy TB. Lásd: [kapacitás kiosztása](#provisioned-capacity) az eszköz és a kötet típusonkénti maximális méret. Nézze meg hello **rendelkezésre álló kapacitásból** toodetermine mennyi tárhelyre ténylegesen érhető el az eszközön.
5. Hello nyíl ikonra![Nyíl ikon](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png). Ha konfigurál egy helyileg rögzített kötetet, hello a következő üzenet jelenik meg.
   
    ![Módosítsa a kötet típusa üzenet](./media/storsimple-manage-volumes-u2/LocalVolEx.png)
6. Hello nyíl ikonra ![nyíl ikonra](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png)újra toogo toohello **további beállításokat** lap.
   
    ![Vegye fel a kötet varázsló további beállításai](./media/storsimple-manage-volumes-u2/AddVolume2.png)<br>
7. A **további beállításokat**, adja hozzá egy új hozzáférés-vezérlési rekordot (ACR):
   
   1. Válasszon egy hozzáférés-vezérlési rekordot (ACR) hello legördülő listából. Másik megoldásként egy új ACR is hozzáadhat. ACRs határozza meg, hogy mely gazdagépek férhetnek hozzá a kötetek egyező hello hello rekordban szereplő gazdagép IQN. Ha nem adja meg az ACR, hello a következő üzenet jelenik meg.
      
        ![Adja meg az ACR](./media/storsimple-manage-volumes-u2/SpecifyACR.png)
   2. Azt javasoljuk, hogy kiválassza hello **engedélyezése ehhez a kötethez alapértelmezett biztonsági mentés** jelölőnégyzetet.
   3. Kattintson a pipa ikonra hello ![Pipa ikon](./media/storsimple-manage-volumes-u2/HCS_CheckIcon.png) hello toocreate hello kötet megadott beállításokat.

Az új kötet már készen áll a toouse.

> [!NOTE]
> Ha hozzon létre egy helyileg rögzített kötetet, majd létrehoz egy másik helyileg rögzített kötet azonnal ezután hello kötet létrehozási feladatok egymás után futnak. hello első kötet létrehozási feladata Befejezés hello tovább kötet létrehozására irányuló feladat megkezdése előtt.
> 
> 

## <a name="modify-a-volume"></a>A kötet módosítása
A kötet módosításával, vagy hello kötet elérő állomások hello módosítása tooexpand szükséges.

> [!IMPORTANT]
> * Hello kötet mérete hello eszközön módosításakor hello kötetméretet kell toobe hello gazdagépen is módosítani. 
> * hello gazdagép-oldali lépéseket az itt ismertetett kell a Windows Server 2012 (2012R2). Eljárások a Linux vagy más állomás operációs rendszerek eltérőek lesznek. Tekintse meg tooyour állomás operációs rendszer utasításokat, ha módosítja egy másik operációs rendszert futtató kiszolgálóra hello köteten. 
> 
> 

#### <a name="toomodify-a-volume"></a>a kötet toomodify
1. A hello **eszközök** lapon, válassza ki azt hello eszközt, kattintson rá duplán, és kattintson a hello **Kötettárolók** fülre.
2. Válasszon ki egy kötettárolót hello listából, és kattintson rá duplán tooview hello kötetek hello tárolóhoz társított.
3. Jelöljön ki egy kötetet, és a lap alján hello hello lap, kattintson **módosítás**. hello módosítás kötet varázsló elindul.
4. Hello módosítsa kötet varázslóban a **alapbeállítások**, megteheti hello következő:
   
   * Hello szerkesztése **neve**.
   * Hello átalakítása **használat típusa** helyileg rögzített tootiered vagy rögzített rétegzett toolocally (lásd: [hello kötet típusának módosítása](#change-the-volume-type) olvashat).
   * Növelje a hello **kiosztott kapacitást**. Hello **kiosztott kapacitást** pedig csak növelni. A kötet nem zsugorítható, létrehozás után.
5. A **további beállításokat**, módosíthatja a hello ACR, feltéve, hogy hello kötet offline állapotban. Ha hello kötet online állapotban, szüksége lesz a tootake azt kapcsolat nélküli első. Tekintse meg a toohello lépéseit [kötet offline állapotba](#take-a-volume-offline) előzetes toomodifying hello ACR.
   
   > [!NOTE]
   > Nem módosítható hello **alapértelmezett biztonsági mentés engedélyezése** hello kötet lehetőséget.
   > 
   > 
6. A módosítások mentéséhez hello pipa ikonra kattintva ![pipa ikon](./media/storsimple-manage-volumes-u2/HCS_CheckIcon.png). a klasszikus Azure portálon hello egy frissítési kötet üzenetet jelenít meg. Egy üzenetet, jelenít meg, ha hello kötet sikeresen frissítve.
7. Ha egy kötet bővíti, hajtsa végre a következő lépéseket a Windows-állomás számítógépen hello:
   
   1. Nyissa meg túl**számítógép-kezelés** ->**Lemezkezelés**.
   2. Kattintson a jobb gombbal **Lemezkezelés** válassza **lemezek újraellenőrzése**.
   3. Válassza a lemezek hello lista hello kötet, amelyeket azért frissített, kattintson a jobb gombbal, és válassza **kötet kiterjesztése**. hello kötet kiterjesztése varázsló elindul. Kattintson a **Tovább** gombra.
   4. Hello alapértelmezett értékek elfogadásával hello varázsló befejezése. Hello varázsló befejezése után hello kötet mérete nagyobb hello kell megjelennie.
      
      > [!NOTE]
      > Ha bontsa ki a helyileg rögzített kötet, és bontsa ki egy másik helyileg rögzített kötet azonnal ezután hello kötet bővítése feladatok egymás után futnak. hello első kötet kiterjesztési feladat Befejezés hello következő kötet bővítése feladat megkezdése előtt.
      > 
      > 

![Videó elérhető](./media/storsimple-manage-volumes-u2/Video_icon.png)**Videó elérhető**

videó toowatch hogyan tooexpand egy köteten, kattintson a [Itt](https://azure.microsoft.com/documentation/videos/expand-a-storsimple-volume/).

## <a name="change-hello-volume-type"></a>Hello kötet típusának módosítása
Hello kötettípus rögzítve rétegzett toolocally vagy a helyileg rögzített tootiered módosíthatja. Ez a konverzió azonban nem lehet egy gyakori előfordulásainak kívánt számát. Kötet konvertálása rögzítve rétegzett toolocally okai a következők:

* Helyi garanciákat adatok rendelkezésre állását és teljesítményét
* Felhő késéseket és a felhő kapcsolódási problémák megszüntetését.

Ezek általában kis tooaccess gyakran kívánt meglévő köteteket. Egy helyileg rögzített kötet teljesen kiépítve, létrehozásakor. Ha egy rétegzett kötet tooa helyileg rögzített kötet, StorSimple ellenőrzi, hogy Ön elegendő lemezterület az eszközön hello átalakítás megkezdése előtt. Ha nincs elég hely van, hibaüzenetet kap, és hello művelet megszakad. 

> [!NOTE]
> A rétegzett toolocally rögzítve konvertálást megkezdése előtt győződjön meg arról, hogy érdemes hello lemezterület-igénye a többi munkaterhelését. 
> 
> 

Érdemes lehet egy helyileg rögzített kötet tooa toochange rétegzett kötet Ha további tárhelyet tooprovision kell más kötet. Hello helyileg rögzített kötet tootiered alakításakor hello elérhető kapacitás hello eszközön, amely a hello kapacitás hello mérete nő. Ha problémák megelőzése hello átalakítás kötet hello helyi típus toohello a rétegzett típusa, hello helyi kötet virágot fog hozni egy rétegzett kötet tulajdonságainak hello átalakítás befejeződéséig. Ennek oka az lehet, hogy néhány adat kiömlött toohello felhő. Ezek az adatok kiömlött továbbra is toooccupy helyi terület hello eszközön, amely nem szabadítható, amíg hello művelet újraindul, és befejeződött.

> [!NOTE]
> Egy kötet konvertálása eltarthat egy ideig, és a konvertálás nem vethető el, miután elindult. hello kötet online állapotban marad hello konvertálás során, és a biztonsági másolatok készítése, de nem bontsa vagy hello kötet visszaállítása hello átalakítás közben.  
> 
> 

Egy rétegzett tooa helyileg rögzített kötet konverzió kedvezőtlen hatással lehet a Teljesítmény eszköz. Emellett a következő tényezőket hello növelheti toocomplete hello átalakításhoz szükséges hello idő:

* Nincs elegendő sávszélesség.
* Nincs aktuális biztonsági másolat.

Ezek a tényezők lehetnek hello hatások toominimize:

* Tekintse át a sávszélesség-szabályozási házirendeket, és győződjön meg arról, hogy rendelkezésre áll-e a dedikált 40 MB/s sávszélességet.
* Hello átalakítást végrehajtani kevesen ütemezni.
* A felhő pillanatképet hello átalakítás megkezdése előtt.

Ha több kötet (támogatja a különböző terhelésekhez), majd kell helyezi előtérbe hello kötet átalakítás, hogy a magasabb prioritású kötetek először konvertálja. Például kell átalakítása üzemeltető virtuális gépek (VM) és az SQL-munkaterhelések kötetek fájl megosztási munkaterhelések kötetek átalakítása előtt.

#### <a name="toochange-hello-volume-type"></a>toochange hello kötet típusa
1. A hello **eszközök** lapon, válassza ki azt hello eszközt, kattintson rá duplán, és kattintson a hello **Kötettárolók** fülre.
2. Válasszon ki egy kötettárolót hello listából, és kattintson rá duplán tooview hello kötetek hello tárolóhoz társított.
3. Jelöljön ki egy kötetet, és a lap alján hello hello lap, kattintson **módosítás**. hello módosítás kötet varázsló elindul.
4. A hello **alapbeállítások** lapon, majd jelölje ki hello új hello a hello használat típusa **használat típusa** legördülő listából.
   
   * Hello típusa túl módosítani**helyileg rögzített**, StorSimple toosee ellenőrzi, hogy van elegendő kapacitással.
   * Hello típusa túl módosítani**rétegzett** ezt a kötetet archív adatokhoz, jelölje be hello használandó **kötet használata ritkábban használt archív adatokhoz** jelölőnégyzetet.
     
       ![Archív jelölőnégyzet](./media/storsimple-manage-volumes-u2/ModifyTieredVolEx.png)
5. Kattintson a nyíl ikonra hello ![nyíl ikonra](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png) toogo toohello **további beállításokat** lap. Ha konfigurál egy helyileg rögzített kötetet, hello következő üzenet jelenik meg.
   
    ![Módosítsa a kötet típusa üzenet](./media/storsimple-manage-volumes-u2/ModifyLocalVolEx.png)
6. Hello nyíl ikonra ![nyíl ikon](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png) újra toocontinue.
7. Kattintson a pipa ikonra hello ![Pipa ikon](./media/storsimple-manage-volumes-u2/HCS_CheckIcon.png) toostart hello átalakítási folyamat. hello Azure portál egy frissítési kötet üzenetet jelenít meg. Egy üzenetet, jelenít meg, ha hello kötet sikeresen frissítve.

## <a name="take-a-volume-offline"></a>A kötet offline állapotba helyezése
Szükség lehet egy kötet offline tootake tervezésekor toomodify vagy törölje azt. Ha egy kötet offline állapotban, nincs olvasási és írási hozzáférése érhető el. Szüksége lesz tootake hello kötet offline hello állomáson, valamint hello eszközön. 

#### <a name="tootake-a-volume-offline"></a>a kötet offline tootake
1. Győződjön meg arról, hogy a szóban forgó hello kötet nem offline állapotba helyezése előtt használatban van.
2. Igénybe hello kötet offline hello állomás első. Ezzel a megoldással adatsérülés hello köteten lehetséges kockázatát. Lépéseit tekintse meg a gazda operációs rendszer toohello utasításokat.
3. Miután hello állomás elérhetetlenné válik, igénybe hello kötet offline hello eszközön hello lépések végrehajtásával:
   
   1. A hello **eszközök** lapon, válassza ki azt hello eszközt, kattintson rá duplán, és kattintson a hello **Kötettárolók** külön-külön hello **Kötettárolók** összes táblázatos formában listák lap hello kötettárolók, amelyek hello eszközhöz kapcsolódnak.
   2. Válassza ki a kötettároló, és kattintson rá a toodisplay hello összes hello kötetek listáját hello tárolóban.
   3. Jelöljön ki egy kötetet, majd **offline állapotba**.
   4. Ha a rendszer megerősítést kér, kattintson az **Igen** gombra. hello kötet offline állapotban kell.
      
      Miután egy kötet offline állapotban, hello **online állapotba hozás** összetevőn.

> [!NOTE]
> Hello **Offline állapotba** parancs küld egy kérelem toohello eszköz tootake hello kötet offline állapotba. Gazdagépek hello kötet használják, ha az eredmény megszakadt kapcsolat, de hello kötet offline állapotba helyezése nem sikertelen lesz. 
> 
> 

## <a name="delete-a-volume"></a>Kötet törlése
> [!IMPORTANT]
> Törölheti a kötet csak akkor, ha offline állapotban.
> 
> 

Hajtsa végre a következő lépéseket toodelete kötet hello.

#### <a name="toodelete-a-volume"></a>a kötet toodelete
1. A hello **eszközök** lapon, válassza ki azt hello eszközt, kattintson rá duplán, és kattintson a hello **Kötettárolók** fülre.
2. Válassza ki a kívánt toodelete hello kötet rendelkező hello kötettároló. Kattintson a hello kötet tároló tooaccess hello **kötetek** lap.
3. Ez a tároló társított összes hello kötetek táblázatos formában jelennek meg. Hello állapotának hello kötet toodelete szeretné. Ha azt szeretné, hogy toodelete hello kötet nem offline állapotban, végezze el az offline első, a következő hello lépéseket szereplő [kötet offline állapotba](#take-a-volume-offline).
4. Miután hello kötet offline állapotban, kattintson **törlése** hello lap hello alján.
5. Ha a rendszer megerősítést kér, kattintson az **Igen** gombra. a program törli hello kötet, és hello **kötetek** lapon megjelenik a kötetek hello tárolóban hello frissített listáját.
   
   > [!NOTE]
   > Ha töröl egy helyileg rögzített kötetet, hello szabad lemezterület az új kötetekre nem frissülhet azonnal. hello StorSimple Manager szolgáltatás hello helyi hely rendszeres időközönként frissíti. Javasoljuk, hogy néhány percig, mielőtt újból toocreate hello új kötet várja.<br> Emellett ha töröl egy helyileg rögzített kötet, és törölje a másikra helyileg rögzített kötet azonnal ezután hello kötet törlési feladatok egymás után futnak. hello első kötet törlési feladat Befejezés hello tovább kötet törlési feladat megkezdése előtt.
   > 
   > 

## <a name="monitor-a-volume"></a>A kötet figyelése
Mennyiségi figyelési lehetővé teszi egy kötet toocollect I/O-hez kapcsolódó statisztikája. Figyelési hello alapértelmezés szerint engedélyezve van az Ön által létrehozott első 32 kötetek. A további kötetek figyelés alapértelmezés szerint le van tiltva. A klónozott kötetek figyelését is alapértelmezés szerint nincs engedélyezve.

Hajtsa végre a következő lépéseket tooenable hello, vagy tiltsa le a kötet megfigyelését.

#### <a name="tooenable-or-disable-volume-monitoring"></a>tooenable vagy a kötet figyelés letiltásakor
1. A hello **eszközök** lapon, válassza ki azt hello eszközt, kattintson rá duplán, és kattintson a hello **Kötettárolók** fülre.
2. Válassza ki, melyik hello köteten található hello kötettároló, és kattintson a hello kötet tároló tooaccess hello **kötetek** lap.
3. Ez a tároló társított összes hello kötetek hello táblázatos megjelenítési láthatók. Kattintson, és válassza a hello kötet és a kötet Klónozás.
4. Hello a hello lap alján, kattintson **módosítás**.
5. Hello kötet módosítása varázslóban a **alapbeállítások**, jelölje be **engedélyezése** vagy **tiltsa le a** a hello **figyelés** legördülő listából választhatja ki.

## <a name="next-steps"></a>Következő lépések
* Ismerje meg, hogyan túl[klónozza a StorSimple-kötet](storsimple-clone-volume.md).
* Ismerje meg, hogyan túl[használata hello StorSimple Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-manager-service-administration.md).

