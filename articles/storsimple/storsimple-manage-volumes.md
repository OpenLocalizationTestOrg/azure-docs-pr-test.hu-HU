---
title: "aaaManage a StorSimple-köteteket |} Microsoft Docs"
description: "Azt ismerteti, hogyan tooadd, módosítás, figyelése, és törölje a StorSimple-köteteket, és hogyan tootake őket, offline állapotba, ha a szükséges."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: ccabd859-590c-4568-a81d-6ff38f125be3
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/11/2016
ms.author: v-sharos
ms.openlocfilehash: 8dc646261ee2ad8f2b80291518716f4de55b63f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-toomanage-volumes"></a>Hello StorSimple Manager szolgáltatás toomanage köteteket
[!INCLUDE [storsimple-version-selector-manage-volumes](../../includes/storsimple-version-selector-manage-volumes.md)]

## <a name="overview"></a>Áttekintés
Ez az oktatóanyag azt ismerteti, hogyan toouse StorSimple Manager szolgáltatás toocreate hello és hello StorSimple eszköz és a StorSimple virtuális eszköz a kötetek kezelése.

hello StorSimple Manager szolgáltatás a klasszikus Azure-portál, amely lehetővé teszi a StorSimple megoldásban egy egyetlen webes felhasználói felületen keresztüli kezelését hello bővítményeként. Továbbá toomanaging kötetek, a is hello StorSimple Manager szolgáltatás toocreate használja és StorSimple szolgáltatások kezelése, megtekintése és eszközök kezeléséhez, megtekintheti a riasztásokat, és megtekinthető és kezelhető biztonsági mentési házirendek és hello biztonságimásolat-katalógus.

> [!NOTE]
> Az Azure StorSimple csak dinamikusan kiosztott köteteket hozhat létre. Nem hozható létre teljesen vagy részben kiosztott köteteket az Azure StorSimple rendszeren.
> 
> Dinamikus kiosztás egy olyan virtualizációs technológia, rendelkezésre álló tár jelenik meg tooexceed fizikai erőforrásokat. Ahelyett, hogy elegendő tárhely előre foglalással, Azure StorSimple használ a vékony létesítési tooallocate éppen elegendő toomeet aktuális lemezterület. felhőalapú tárolás rugalmas jellege hello elősegíti a ezt a módszert használja, mert Azure StorSimple növelheti vagy csökkentheti a felhőalapú tárolás toomeet tartalomkérései változó igényeinek.
> 
> 

## <a name="hello-volumes-page"></a>hello kötetek lap
Hello **kötetek** lap lehetővé teszi a toomanage hello tárolókötetek hello Microsoft Azure StorSimple eszközön a kezdeményezőktől (kiszolgálóktól) törlődnek. Hello kötetek listáját jeleníti meg a StorSimple eszközt.

 ![Kötetek lap](./media/storsimple-manage-volumes/HCS_VolumesPage.png)

A kötet attribútumainak állnak:

* **Név** – egy leíró nevet, amely egyedinek kell lennie, és azonosíthatja a hello kötet. Ez a név monitoring jelentések, amikor egy adott köteten szűrheti is szerepel.
* **Állapot** – lehet online vagy offline állapotú. Ha egy köteten, ha a kapcsolat nélküli, nincs látható tooinitiators (kiszolgálók), amelyek számára engedélyezett a hozzáférés toouse hello kötet.
* **Kapacitás** – Megadja, hogyan hello sok esetén, mert a gép hello kezdeményező (kiszolgáló) által érzékelt. Kapacitás hello hello kezdeményező (kiszolgáló) tárolt adatok teljes mennyisége határozza meg. Kötetek kiosztása, és a deduplikált adatok. Ez azt jelenti, hogy az eszköz nem előre lefoglalni fizikai tárolási kapacitás, belsőleg vagy hello felhő tooconfigured kötet kapacitása alapján történik. hello kötet kapacitása lefoglalt és felhasznált igény szerint.
* **Típus** – hello kötet típus rétegzett vagy archiválási lehet (a altípusa Szintezett)
* **Hozzáférés** – Itt adhatja meg, amelyek számára engedélyezett a hozzáférés toothis kötet hello kezdeményezőktől (kiszolgálóktól). Kezdeményezők, amelyek nem tagjai a hozzáférés-vezérlési rekordot (ACR) társított hello kötet nem jelenik meg a hello kötet.
* **Figyelési** – adja meg a kötet figyel-e. Egy kötet lesz figyelési létrehozásakor alapértelmezés szerint engedélyezett. Lesz, azonban figyelés, a kötet másolat le kell tiltani. egy kötet, figyelés tooenable kövesse a figyelő egy kötet hello utasításait.

a kötet társított hello leggyakoribb feladatokat a következők:

* Kötet hozzáadása
* A kötet módosítása
* Kötet törlése
* A kötet offline állapotba helyezése
* A kötet figyelése

## <a name="add-a-volume"></a>Kötet hozzáadása
Ön [kötet létrehozott](storsimple-deployment-walkthrough-u1.md#step-6-create-a-volume) a StorSimple megoldás üzembe helyezése során. Hasonló eljárással hozzáadna egy kötetet.

### <a name="tooadd-a-volume"></a>a kötet tooadd
1. A hello **eszközök** lapon, válassza ki azt hello eszközt, kattintson rá duplán, és kattintson a hello **Kötettárolók** fülre.
2. Válassza ki a kötettároló, és kattintson a hello megfelelő sor tooaccess hello kötetek hello tárolóhoz társított hello nyílra.
3. Kattintson a **Hozzáadás** hello lap hello alján. Elindítja a hello kötet hozzáadása varázslóban.
   
     ![Vegye fel a kötet varázsló alapbeállítások](./media/storsimple-manage-volumes/AddVolume1.png)
4. Hello hozzá egy kötet varázslóban a **alapbeállítások**, a következő hello:
   
   1. Adjon **nevet** a kötetnek.
   2. Adja meg a hello **kiosztott kapacitást** a kötet GB-ban vagy TB. hello kapacitás egy fizikai eszköz 1 GB és 64 TB-os között kell lennie. hello maximális kapacitás, amely egy olyan kötetre a StorSimple virtuális eszköz kiépítése 30 TB.
   3. Jelölje be hello **használat típusa** a köteten. Használata rétegzett kötetet archív adatokhoz, hello kiválasztásával hello **kötet használata ritkábban használt archív adatokhoz** jelölőnégyzetet módosítja hello deduplikációs adattömbméret a kötethez tartozó too512 KB. Ha nem ezt a beállítást, a hello megfelelő rétegzett kötet 64 KB adattömbméretet fogja használni. Egy nagyobb deduplikációs adattömbméret lehetővé teszi, hogy a nagy mennyiségű archiválási adatok toohello felhő hello eszköz tooexpedite hello átvitelét. (Rétegzett kötetek volt neve korábban elsődleges kötet.)
   4. Kattintson a nyíl ikonra hello ![nyíl ikonra](./media/storsimple-manage-volumes/HCS_ArrowIcon.png)toogo toohello **további beállításokat** lap.
      
        ![Vegye fel a kötet varázsló további beállításai](./media/storsimple-manage-volumes/AddVolume2.png)
5. A **további beállításokat**, adja hozzá egy új hozzáférés-vezérlési rekordot (ACR):
   
   1. Válasszon egy hozzáférés-vezérlési rekordot (ACR) hello legördülő listából. Másik megoldásként egy új ACR is hozzáadhat. ACRs határozza meg, hogy mely gazdagépek férhetnek hozzá a kötetek egyező hello hello rekordban szereplő gazdagép IQN.
   2. Ajánlott engedélyezni a alapértelmezett biztonsági mentés hello kiválasztásával **engedélyezése ehhez a kötethez alapértelmezett biztonsági mentés** jelölőnégyzetet.
   3. Kattintson a pipa ikonra hello ![Pipa ikon](./media/storsimple-manage-volumes/HCS_CheckIcon.png) hello toocreate hello kötet megadott beállításokat.

Az új kötet már készen áll a toouse.

## <a name="modify-a-volume"></a>A kötet módosítása
A kötet módosításával, vagy hello kötet elérő állomások hello módosítása tooexpand szükséges.

> [!IMPORTANT]
> * Hello kötet mérete hello eszközön módosításakor hello kötetméretet kell toobe hello gazdagépen is módosítani.
> * hello gazdagép-oldali lépéseket az itt ismertetett kell a Windows Server 2012 (2012R2). Eljárások a Linux vagy más állomás operációs rendszerek eltérőek lesznek. Tekintse meg tooyour állomás operációs rendszer utasításokat, ha módosítja egy másik operációs rendszert futtató kiszolgálóra hello köteten.
> 
> 

### <a name="toomodify-a-volume"></a>a kötet toomodify
1. A hello **eszközök** lapon, válassza ki azt hello eszközt, kattintson rá duplán, és kattintson a hello **Kötettároló** fülre. Ezen a lapon táblázatos formában sorolja fel az összes hello kötettárolók, hello eszköz társított.
2. Válassza ki a kötettároló, és kattintson rá a toodisplay hello összes hello kötetek listáját hello tárolóban.
3. A hello **kötetek** lapon jelöljön ki egy kötetet, majd kattintson **módosítás**.
4. Hello módosítsa kötet varázslóban a **alapbeállítások**, megteheti hello következő:
   
   * Hello szerkesztése **neve** és **típus** Ha kívánja toomodify tooan rétegzett kötetet archív kötet hello kiválasztásával **kötet használata ritkábban használt archív adatokhoz** jelölőnégyzet toochange hello deduplikációs adattömbméret a kötethez tartozó too512 KB.
   * Növelje a hello **kiosztott kapacitást**. Hello **kiosztott kapacitást** pedig csak növelni. A kötet nem zsugorítható, létrehozás után.
     
     > [!NOTE]
     > Hello kötettároló nem módosítható, miután tooa kötet van hozzárendelve.
     > 
     > 
5. A **további beállításokat**, megteheti hello következő:
   
   * Módosítsa a hello ACRs, feltéve, hogy a hello kötet offline állapotban. Ha hello kötet online állapotban, szüksége lesz a tootake azt kapcsolat nélküli első. Tekintse meg a toohello lépéseit [kötet offline állapotba](#take-a-volume-offline) előzetes toomodifying hello ACR.
   * Módosíthatja a ACRs hello listáját, miután hello kötet offline állapotban.
     
     > [!NOTE]
     > Nem módosíthatja a hello **engedélyezése ehhez a kötethez alapértelmezett biztonsági mentés** hello kötet lehetőséget.
     > 
     > 
6. A módosítások mentéséhez hello pipa ikonra kattintva ![pipa ikon](./media/storsimple-manage-volumes/HCS_CheckIcon.png). a klasszikus Azure portálon hello egy frissítési kötet üzenetet jelenít meg. Egy üzenetet, jelenít meg, ha hello kötet sikeresen frissítve.
7. Ha egy kötet bővíti, hajtsa végre a következő lépéseket a Windows-állomás számítógépen hello:
   
   1. Nyissa meg túl**számítógép-kezelés** ->**Lemezkezelés**.
   2. Kattintson a jobb gombbal **Lemezkezelés** válassza **lemezek újraellenőrzése**.
   3. Válassza a lemezek hello lista hello kötet, amelyeket azért frissített, kattintson a jobb gombbal, és válassza **kötet kiterjesztése**. hello kötet kiterjesztése varázsló elindul. Kattintson a **Tovább** gombra.
   4. Hello alapértelmezett értékek elfogadásával hello varázsló befejezése. Hello varázsló befejezése után hello kötet mérete nagyobb hello kell megjelennie.

![Videó elérhető](./media/storsimple-manage-volumes/Video_icon.png)**Videó elérhető**

videó toowatch hogyan tooexpand egy köteten, kattintson a [Itt](https://azure.microsoft.com/documentation/videos/expand-a-storsimple-volume/).

## <a name="take-a-volume-offline"></a>A kötet offline állapotba helyezése
Szükség lehet egy kötet offline tootake tervezésekor toomodify vagy törölje azt. Ha egy kötet offline állapotban, nincs olvasási és írási hozzáférése érhető el. Szüksége lesz tootake hello kötet offline hello állomáson, valamint hello eszközön. Hajtsa végre a következő lépéseket tootake egy kötet offline hello.

### <a name="tootake-a-volume-offline"></a>a kötet offline tootake
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

### <a name="toodelete-a-volume"></a>a kötet toodelete
1. A hello **eszközök** lapon, válassza ki azt hello eszközt, kattintson rá duplán, és kattintson a hello **Kötettárolók** fülre.
2. Válassza ki a kívánt toodelete hello kötet rendelkező hello kötettároló. Kattintson a hello kötet tároló tooaccess hello **kötetek** lap.
3. Ez a tároló társított összes hello kötetek táblázatos formában jelennek meg. Hello állapotának hello kötet toodelete szeretné. Ha azt szeretné, hogy toodelete hello kötet nem offline állapotban, végezze el az offline első, a következő hello lépéseket szereplő [kötet offline állapotba](#take-a-volume-offline).
4. Miután hello kötet offline állapotban, kattintson **törlése** hello lap hello alján.
5. Ha a rendszer megerősítést kér, kattintson az **Igen** gombra. a program törli hello kötet, és hello **kötetek** lapon megjelenik a kötetek hello tárolóban hello frissített listáját.

## <a name="monitor-a-volume"></a>A kötet figyelése
Mennyiségi figyelési lehetővé teszi egy kötet toocollect I/O-hez kapcsolódó statisztikája. Figyelési hello alapértelmezés szerint engedélyezve van az Ön által létrehozott első 32 kötetek. A további kötetek figyelés alapértelmezés szerint le van tiltva. A klónozott kötetek figyelését is alapértelmezés szerint nincs engedélyezve.

Hajtsa végre a következő lépéseket tooenable hello, vagy tiltsa le a kötet megfigyelését.

### <a name="tooenable-or-disable-volume-monitoring"></a>tooenable vagy a kötet figyelés letiltásakor
1. A hello **eszközök** lapon, válassza ki azt hello eszközt, kattintson rá duplán, és kattintson a hello **Kötettárolók** fülre.
2. Válassza ki, melyik hello köteten található hello kötettároló, és kattintson a hello kötet tároló tooaccess hello **kötetek** lap.
3. Ez a tároló társított összes hello kötetek hello táblázatos megjelenítési láthatók. Kattintson, és válassza a hello kötet és a kötet Klónozás.
4. Hello a hello lap alján, kattintson **módosítás**.
5. Hello kötet módosítása varázslóban a **alapbeállítások**, jelölje be **engedélyezése** vagy **tiltsa le a** a hello **figyelés** legördülő listából választhatja ki.
   
    ![A kötet alapvető beállítások módosítása](./media/storsimple-manage-volumes/HCS_MonitorVolumeM.png)

## <a name="next-steps"></a>Következő lépések
* Ismerje meg, hogyan túl[klónozza a StorSimple-kötet](storsimple-clone-volume.md).
* Ismerje meg, hogyan túl[használata hello StorSimple Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-manager-service-administration.md).

