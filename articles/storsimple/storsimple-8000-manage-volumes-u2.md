---
title: "aaaManage StorSimple-köteteket (Update 3) |} Microsoft Docs"
description: "Azt ismerteti, hogyan tooadd, módosítás, figyelése, és törölje a StorSimple-köteteket, és hogyan tootake őket, offline állapotba, ha a szükséges."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/19/2017
ms.author: alkohli
ms.openlocfilehash: 6228c4486dd5a7887df670c4c4584c4edcdfc509
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-toomanage-volumes-update-3-or-later"></a>Hello StorSimple Device Manager szolgáltatás toomanage köteteket (Update 3-as vagy újabb)

## <a name="overview"></a>Áttekintés

Ez az oktatóanyag azt ismerteti, hogyan toouse StorSimple Device Manager szolgáltatás toocreate hello és hello StorSimple 8000 sorozat eszközeire 3 és újabb verzióit futtató a frissítés a kötetek kezelése.

StorSimple Device Manager szolgáltatás hello egy bővítmény hello Azure-portál, amely lehetővé teszi a StorSimple megoldásban egy egyetlen webes felhasználói felületen keresztüli kezelését. Hello Azure portál toomanage kötetek használatát az eszközök. Akkor is létrehozása és a StorSimple-szolgáltatások kezelése, kezelheti az eszközöket, a biztonsági mentési házirendek és a biztonságimásolat-katalógus, és megtekintheti a riasztásokat.

## <a name="volume-types"></a>Kötet típusok

StorSimple-köteteket lehet:

* **Helyileg rögzített kötetek**: hello helyi StorSimple eszközön mindig továbbra is ezeken a köteteken lévő adatok.
* **Rétegzett kötet**: ezek a kötetek adatainak toohello felhő is kerülnek.

Az archiválási köteten a rétegzett kötet típusú. hello nagyobb deduplikációs adattömbméret archiválási kötetek használt lehetővé teszi, hogy a hello eszköz tootransfer nagyobb szegmensek adatok toohello felhő.

Ha szükséges, a hello kötettípus helyi tootiered vagy a rétegzett toolocal módosíthatja. További információ: túl[hello kötet típusának módosítása](#change-the-volume-type).

### <a name="locally-pinned-volumes"></a>Helyileg rögzített kötetek

Helyileg rögzített kötetek, amely nem réteg adatok toohello felhő teljesen kiosztott köteteket, ezzel biztosítva a helyi biztosítja, hogy az elsődleges adatok, a felhő kapcsolat független. A helyileg rögzített kötetekhez adatok nem deduplikált, és tömörített; azonban a pillanatképeket a helyileg rögzített kötetek deduplikálásának. 

Helyileg rögzített kötetekhez teljesen kiépített; ezért rendelkeznie kell elegendő lemezterület az eszközön a létrehozott. Megadhat tooa maximális méretét hello StorSimple 8100 eszközön 8 TB és 20 TB hello 8600 eszközön helyileg rögzített kötetekhez. StorSimple hello helyi fennmaradó helyből a pillanatképek, a metaadatok és az adatok feldolgozása hello eszköz foglalja le. Hello mérete egy helyileg rögzített kötet toohello maximális rendelkezésre álló lemezterület növelhető, de egy kötet létrehozását követően hello mérete nem csökkenthető.

Ha helyileg rögzített kötetet hoz létre, hello a rétegzett kötetek létrehozásához rendelkezésre álló terület csökken. fordított hello akkor is igaz: Ha a meglévő rétegzett kötetek, előfordulhat, hogy létrehozása helyileg rögzített kötetek hello terület alacsonyabb, mint a fent megadott maximális határok hello lesz-e. A helyi köteteken további információkért tekintse meg a toohello [gyakran ismételt kérdések a helyileg rögzített kötetekhez](storsimple-8000-local-volume-faq.md).

### <a name="tiered-volumes"></a>Rétegzett kötetek

Rétegzett kötetek mely hello gyakran elért adatok helyben hello eszközön, és kevesebb, mint a gyakran használt adatokat a rendszer automatikusan a rétegzett toohello felhő dinamikusan kiosztott köteteket. Dinamikus kiosztás egy olyan virtualizációs technológia, rendelkezésre álló tár jelenik meg tooexceed fizikai erőforrásokat. Ahelyett, hogy elegendő tárhely előre foglalással, a StorSimple éppen elegendő toomeet aktuális lemezterület használ vékony létesítési tooallocate. felhőalapú tárolás rugalmas jellege hello elősegíti a ezt a módszert használja, mert StorSimple növelheti vagy csökkentheti a felhőalapú tárolás toomeet tartalomkérései változó igényeinek.

Ha használ rétegzett kötetet archív adatokhoz, jelölje be hello hello **kötet használata ritkábban használt archív adatokhoz** jelölőnégyzetet toochange hello deduplikációs adattömbméret a kötethez tartozó too512 KB. Ha nem ezt a beállítást, a hello megfelelő rétegzett kötet 64 KB adattömbméretet fogja használni. Egy nagyobb deduplikációs adattömbméret lehetővé teszi, hogy a nagy mennyiségű archiválási adatok toohello felhő hello eszköz tooexpedite hello átvitelét.


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

## <a name="hello-volumes-blade"></a>hello kötetek panel

Hello **kötetek** panel lehetővé teszi a toomanage hello tárolókötetek hello Microsoft Azure StorSimple eszközön a kezdeményezőktől (kiszolgálóktól) törlődnek. Hello StorSimple eszközök csatlakoztatott tooyour szolgáltatás hello kötetek listáját megjeleníti.

 ![Kötetek lap](./media/storsimple-8000-manage-volumes-u2/volumeslist.png)

A kötet attribútumainak állnak:

* **Kötet neve** – egy leíró nevet, amely egyedinek kell lennie, és azonosíthatja a hello kötet. Ez a név monitoring jelentések, amikor egy adott köteten szűrheti is szerepel. Hello kötet létrehozása után nem lehet átnevezni.
* **Állapot** – lehet online vagy offline állapotú. Ha egy kötet offline állapotban, nincs látható tooinitiators (kiszolgálók), amelyek számára engedélyezett a hozzáférés toouse hello kötet.
* **Kapacitás** – hello hello kezdeményező (kiszolgáló) tárolt adatok teljes mennyisége határozza meg. Helyileg rögzített kötetek teljesen kiépített és hello StorSimple eszközön találhatók. Rétegzett kötetek kiosztása és hello adatok deduplikált. A dinamikusan kiosztott köteteket az eszköz nem előre lefoglalni fizikai tárolási kapacitás belsőleg vagy hello felhő tooconfigured kötet kapacitása szerint. hello kötet kapacitása lefoglalt és felhasznált igény szerint.
* **Típus** – azt jelzi, hogy hello kötet **rétegzett** (alapértelmezett hello) vagy **helyileg rögzített**.

Ez a következő feladatok oktatóanyag tooperform hello használata hello utasításait:

* Kötet hozzáadása 
* A kötet módosítása 
* Hello kötet típusának módosítása
* Kötet törlése 
* A kötet offline állapotba helyezése 
* A kötet figyelése 

## <a name="add-a-volume"></a>Kötet hozzáadása

Ön [kötet létrehozott](storsimple-8000-deployment-walkthrough-u2.md#step-6-create-a-volume) a StorSimple 8000 series eszköz telepítése során. Hasonló eljárással hozzáadna egy kötetet.

#### <a name="tooadd-a-volume"></a>a kötet tooadd

1. A hello táblázatos felsorolása hello hello eszközök **eszközök** panelen válassza ki azt az eszközt. Kattintson a **+ Kötet hozzáadása** gombra.

    ![Új kötet hozzáadása](./media/storsimple-8000-manage-volumes-u2/step5createvol1.png)

2. A hello **kötet hozzáadása** panel:
   
    1. Hello **kiválasztására szolgáló** mező automatikusan feltöltődik értékkel az aktuális eszközzel.

    2. Hello legördülő listájából válassza ki egy kötetet tooadd kell hello kötettároló.

    3.  Adja meg a kötet **Nevét**. Hello kötet létrehozása után hello kötet nem nevezhető át.

    4. Hello a legördülő listán válassza ki a hello **típus** a köteten. Helyi garanciákat, kis késleltetést és magasabb teljesítményt igénylő munkaterheléseknél válasszon **Helyileg rögzített** kötetet. Minden más adathoz válasszon **Rétegzett** kötetet. Ha archív adatokhoz használja ezt a kötetet, jelölje be a **Kötet használata ritkábban használt archív adatokhoz** beállítást.
      
       A rétegzett kötetek kiosztása dinamikus, ezért gyorsan létrehozhatók. Kiválasztása **kötet használata ritkábban használt archív adatokhoz** a rétegzett kötetet archív adatokhoz módosítások hello deduplikációs adattömbméret a kötethez tartozó szánt too512 KB. Ha ez a mező nincs bejelölve, a hello megfelelő rétegzett kötet 64 KB adattömbméretet használ. Egy nagyobb deduplikációs adattömbméret lehetővé teszi, hogy a nagy mennyiségű archiválási adatok toohello felhő hello eszköz tooexpedite hello átvitelét.
       
       Egy helyileg rögzített kötet kiosztása, és biztosítja, hogy hello hello kötet elsődleges adatai helyi toohello eszköz marad, és nem kerülnek toohello felhő.  Ha helyileg rögzített kötetet hoz létre, hello eszköz kérdezze le a hello helyi rétegeken elérhető tárhelyet tooprovision hello kötet hello a kért méret. hello működését egy helyileg rögzített kötet létrehozásához magukban foglalhatják kiömlést hello eszköz toohello felhő adatait, és előfordulhat, hogy hosszú idő hello toocreate hello kötet. hello teljes ideje hello kiosztott kötetre, rendelkezésre álló hálózati sávszélességet és az eszközökön hello adatok hello méretétől függ.

    5. Adja meg a hello **kiosztott kapacitást** a köteten. Jegyezze fel a rendelkezésre álló hello kapacitás a kiválasztott hello kötettípus alapján. a megadott hello kötet mérete nem haladhatja meg a hello rendelkezésre álló területet.
      
       Megadhat helyileg rögzített kötetekhez mentése too8.5 TB, rétegzett kötetekhez too200 TB hello 8100-as eszközön be. Hello nagyobb 8600 eszköz a helyileg rögzített kötetekhez too22.5 TB mentése vagy mentése too500 TB rétegzett kötetek oszthat. Mivel hello eszközön helyi terület szükséges toohost hello használata a rétegzett kötetek készlete, a helyileg rögzített kötetek létrehozása hatással van hello lemezterület rétegzett kötetek. Ha tehát helyileg rögzített kötetet hoz létre, a rétegzett kötetek létrehozásához rendelkezésre álló terület lecsökken. Hasonlóképpen ha rétegzett kötetet hoz létre, hello helyileg rögzített kötetek létrehozásához rendelkezésre álló terület csökken.
      
       Ha a 8100-as eszközön helyileg rögzített kötet 8.5 TB-os (megengedett maximális méretet), majd már kipróbálta összes hello helyi szabad lemezterület a hello eszköz. Nem hozható létre további rétegzett köteteket ettől kezdve, mivel nincs helyi terület hello eszköz toohost hello munkakészletének hello rétegzett kötet. A meglévő rétegzett kötetek is hatással vannak a hello szabad lemezterület. Ha például egy 8100-as eszközhöz már tartozik körülbelül 106 TB rétegzett kötet, akkor csak 4 TB érhető el a gyors helyi kötetekhez.

    6. A hello **csatlakozó állomások** , majd kattintson a hello nyílra. 

        ![Csatlakoztatott gazdagépek](./media/storsimple-8000-manage-volumes-u2/step5createvol2.png)

    7. A hello **csatlakozó állomások** panelen válassza ki egy meglévő ACR, vagy adja hozzá egy új ACR. Ha úgy dönt, hogy egy új ACR, majd adjon meg egy **neve** hello adja meg az ACR **iSCSI Qualified Name** (IQN) a Windows-állomás. Ha nem rendelkezik hello IQN, nyissa meg túl[Get hello egy Windows Server-állomás IQN-Nevének](#get-the-iqn-of-a-windows-server-host). Kattintson a **Create** (Létrehozás) gombra. A kötet jön létre a megadott hello beállításait.

        ![Kattintson a Létrehozás gombra](./media/storsimple-8000-manage-volumes-u2/step5createvol3.png)

Az új kötet már készen áll a toouse.

> [!NOTE]
> Ha hozzon létre egy helyileg rögzített kötetet, majd létrehoz egy másik helyileg rögzített kötet azonnal ezután hello kötet létrehozási feladatok egymás után futnak. hello első kötet létrehozási feladata Befejezés hello tovább kötet létrehozására irányuló feladat megkezdése előtt.

## <a name="modify-a-volume"></a>A kötet módosítása

A kötet módosításával, vagy hello kötet elérő állomások hello módosítása tooexpand szükséges.

> [!IMPORTANT]
> * Hello kötet mérete hello eszközön módosításakor hello kötetméretet kell toobe hello gazdagépen is módosítani.
> * hello gazdagép-oldali lépéseket az itt ismertetett kell a Windows Server 2012 (2012R2). Eljárások a Linux vagy más állomás operációs rendszerek eltérőek lesznek. Tekintse meg tooyour állomás operációs rendszer utasításokat, ha módosítja egy másik operációs rendszert futtató kiszolgálóra hello köteten.

#### <a name="toomodify-a-volume"></a>a kötet toomodify

1. Nyissa meg tooyour StorSimple Device Manager szolgáltatást, és kattintson a **eszközök**. Hello táblázatos felsorolása hello eszközökre, válassza ki, hogy szeretné-e toomodify hello kötet hello-eszköz. Kattintson a **beállítások > kötetek**.

    ![Nyissa meg tooVolumes panel](./media/storsimple-8000-manage-volumes-u2/modifyvol2.png)

2. A táblázatos kötetek listája hello ki hello kötetet, és kattintson a jobb gombbal a tooinvoke hello helyi menü. Válassza ki **offline állapotba** tootake hello kötet offline módosítja.

    ![Válassza ki, és helyezze offline állapotba a kötet](./media/storsimple-8000-manage-volumes-u2/modifyvol4.png)

3. A hello **offline állapotba** panelt, tekintse át a hello hatása hello kötet offline állapotba helyezése, és válassza ki a megfelelő jelölőnégyzet hello. Győződjön meg arról, hogy hello megfelelő mennyiségi hello gazdagépen offline először. Hogyan tootake offline egy kötetet a gazdakiszolgálón csatlakoztatva tooStorSimple kapcsolatos tudnivalókért tekintse meg a toooperating rendszer konkrét utasításokat. Kattintson a **offline állapotba**.

    ![Tekintse át a kötet offline állapotba helyezése hatása](./media/storsimple-8000-manage-volumes-u2/modifyvol5.png)

4. Miután hello kötet offline (ahogy hello kötet állapot szerint), válasszon hello kötetet, és kattintson a jobb gombbal a tooinvoke hello helyi menü. Válassza ki **kötet módosítása**.

    ![Válassza ki a kötet módosítása](./media/storsimple-8000-manage-volumes-u2/modifyvol9.png)


5. A hello **kötet módosítása** panelen, hogy a következő módosításokat hello:
   
   1. kötet hello **neve** nem szerkeszthető.
   2. Átalakítás hello **típus** helyileg rögzített tootiered vagy rögzített rétegzett toolocally (lásd: [hello kötet típusának módosítása](#change-the-volume-type) további információt).
   3. Növelje a hello **kiosztott kapacitást**. Hello **kiosztott kapacitást** pedig csak növelni. A kötet nem zsugorítható, létrehozás után.
   4. A **csatlakozó állomások**, hello ACR módosíthatja. egy ACR toomodify, hello kötet offline állapotban kell lennie.

       ![Tekintse át a kötet offline állapotba helyezése hatása](./media/storsimple-8000-manage-volumes-u2/modifyvol11.png)

5. Kattintson a **mentése** toosave a módosításokat. Ha a rendszer megerősítést kér, kattintson az **Igen** gombra. hello Azure portál egy frissítési kötet üzenetet jelenít meg. Egy üzenetet, jelenít meg, ha hello kötet sikeresen frissítve.

    ![Tekintse át a kötet offline állapotba helyezése hatása](./media/storsimple-8000-manage-volumes-u2/modifyvol5.png)

7. Ha egy kötet bővíti, hajtsa végre a következő lépéseket a Windows-állomás számítógépen hello:
   
   1. Nyissa meg túl**számítógép-kezelés** ->**Lemezkezelés**.
   2. Kattintson a jobb gombbal **Lemezkezelés** válassza **lemezek újraellenőrzése**.
   3. Válassza a lemezek hello lista hello kötet, amelyeket azért frissített, kattintson a jobb gombbal, és válassza **kötet kiterjesztése**. hello kötet kiterjesztése varázsló elindul. Kattintson a **Tovább** gombra.
   4. Hello alapértelmezett értékek elfogadásával hello varázsló befejezése. Hello varázsló befejezése után hello kötet mérete nagyobb hello kell megjelennie.
      
      > [!NOTE]
      > Ha bontsa ki a helyileg rögzített kötet, és bontsa ki egy másik helyileg rögzített kötet azonnal ezután hello kötet bővítése feladatok egymás után futnak. hello első kötet kiterjesztési feladat Befejezés hello következő kötet bővítése feladat megkezdése előtt.
      

## <a name="change-hello-volume-type"></a>Hello kötet típusának módosítása

Hello kötettípus rögzítve rétegzett toolocally vagy a helyileg rögzített tootiered módosíthatja. Ez a konverzió azonban nem lehet egy gyakori előfordulásainak kívánt számát.

### <a name="tiered-toolocal-volume-conversion-considerations"></a>Toolocal rétegzett kötet átalakítás kapcsolatos szempontok

Kötet konvertálása rögzítve rétegzett toolocally okai a következők:

* Helyi garanciákat adatok rendelkezésre állását és teljesítményét
* Felhő késéseket és a felhő kapcsolódási problémák megszüntetését.

Ezek általában kis tooaccess gyakran kívánt meglévő köteteket. Egy helyileg rögzített kötet teljesen kiépítve, létrehozásakor. 

Ha egy rétegzett kötet tooa helyileg rögzített kötet, StorSimple ellenőrzi, hogy Ön elegendő lemezterület az eszközön hello átalakítás megkezdése előtt. Ha nincs elég hely van, hibaüzenetet kap, és hello művelet megszakad. 

> [!NOTE]
> A rétegzett toolocally rögzítve konvertálást megkezdése előtt győződjön meg arról, hogy érdemes hello lemezterület-igénye a többi munkaterhelését. 

Egy rétegzett tooa helyileg rögzített kötet konverzió kedvezőtlen hatással lehet a Teljesítmény eszköz. Emellett a következő tényezőket hello növelheti toocomplete hello átalakításhoz szükséges hello idő:

* Nincs elegendő sávszélesség.
* Nincs aktuális biztonsági másolat.

Ezek a tényezők lehetnek hello hatások toominimize:

* Tekintse át a sávszélesség-szabályozási házirendeket, és győződjön meg arról, hogy rendelkezésre áll-e a dedikált 40 MB/s sávszélességet.
* Hello átalakítást végrehajtani kevesen ütemezni.
* A felhő pillanatképet hello átalakítás megkezdése előtt.

Ha több kötet (támogatja a különböző terhelésekhez), majd kell helyezi előtérbe hello kötet átalakítás, hogy a magasabb prioritású kötetek először konvertálja. Például kell átalakítása üzemeltető virtuális gépek (VM) és az SQL-munkaterhelések kötetek fájl megosztási munkaterhelések kötetek átalakítása előtt.

### <a name="local-tootiered-volume-conversion-considerations"></a>Helyi tootiered kötet átalakítás kapcsolatos szempontok

Érdemes lehet egy helyileg rögzített kötet tooa toochange rétegzett kötet Ha további tárhelyet tooprovision kell más kötet. Hello helyileg rögzített kötet tootiered alakításakor hello elérhető kapacitás hello eszközön, amely a hello kapacitás hello mérete nő. Ha problémák megelőzése hello átalakítás kötet hello helyi típus toohello a rétegzett típusa, hello helyi kötet virágot fog hozni egy rétegzett kötet tulajdonságainak csak hello átalakítás befejeződése után. Ennek oka az lehet, hogy néhány adat kiömlött toohello felhő. A kiömlött adatai továbbra is a helyi terület toooccupy hello eszközön, amely nem szabadítható, amíg hello művelet újraindul, és befejeződött.

> [!NOTE]
> Egy kötet konvertálása eltarthat egy ideig, és a konvertálás nem vethető el, miután elindult. hello kötet online állapotban marad hello konvertálás során, és a biztonsági másolatok készítése, de nem bontsa vagy hello kötet visszaállítása hello átalakítás közben.


#### <a name="toochange-hello-volume-type"></a>toochange hello kötet típusa

1. Nyissa meg tooyour StorSimple Device Manager szolgáltatást, és kattintson a **eszközök**. Hello táblázatos felsorolása hello eszközökre, válassza ki, hogy szeretné-e toomodify hello kötet hello-eszköz. Kattintson a **beállítások > kötetek**.

    ![Nyissa meg tooVolumes panel](./media/storsimple-8000-manage-volumes-u2/modifyvol2.png)

3. A táblázatos kötetek listája hello ki hello kötetet, és kattintson a jobb gombbal a tooinvoke hello helyi menü. Válassza ki **módosítása**.

    ![Válassza ki a helyi menüből módosítása](./media/storsimple-8000-manage-volumes-u2/changevoltype2.png)

4. A hello **kötet módosítása** panelen hello kötet típusának módosítása hello új típusú kijelölésével hello **típus** legördülő listából válassza ki.
   
   * Hello típusa túl módosítani**helyileg rögzített**, StorSimple toosee ellenőrzi, hogy van elegendő kapacitással.
   * Hello típusa túl módosítani**rétegzett** ezt a kötetet archív adatokhoz, jelölje be hello használandó **kötet használata ritkábban használt archív adatokhoz** jelölőnégyzetet.
   * Ha konfigurál egy helyileg rögzített kötetet, rétegzett vagy _viszont_, hello következő üzenet jelenik meg.
   
    ![Változás kötet típus üzenet](./media/storsimple-8000-manage-volumes-u2/changevoltype3.png)

7. Kattintson a **mentése** toosave hello módosításokat. Amikor felszólítja a megerősítésre, kattintson **Igen** toostart hello átalakítási folyamat. 

    ![Mentse és megerősítése](./media/storsimple-8000-manage-volumes-u2/modifyvol11.png)

8. hello Azure-portálon a hello feladat létrehozásához, amely hello kötet frissítené értesítést jelenít meg. Kattintson a hello értesítési toomonitor hello hello kötet átalakítási feladat állapota.

    ![Kötet átalakítási feladat](./media/storsimple-8000-manage-volumes-u2/changevoltype5.png)

## <a name="take-a-volume-offline"></a>A kötet offline állapotba helyezése

Szükség lehet tootake egy kötet offline toomodify tervezi vagy hello kötet törlésekor. Ha egy kötet offline állapotban, nincs olvasási és írási hozzáférése érhető el. Hello kötet offline hello állomás és hello eszköz szükséges.

#### <a name="tootake-a-volume-offline"></a>a kötet offline tootake

1. Győződjön meg arról, hogy a szóban forgó hello kötet nem offline állapotba helyezése előtt használatban van.
2. Igénybe hello kötet offline hello állomás első. Ezzel a megoldással adatsérülés hello köteten lehetséges kockázatát. Lépéseit tekintse meg a gazda operációs rendszer toohello utasításokat.
3. Miután hello állomás elérhetetlenné válik, igénybe hello kötet offline hello eszközön hello lépések végrehajtásával:
   
    1. Nyissa meg tooyour StorSimple Device Manager szolgáltatást, és kattintson a **eszközök**. Hello táblázatos felsorolása hello eszközökre, válassza ki, hogy szeretné-e toomodify hello kötet hello-eszköz. Kattintson a **beállítások > kötetek**.

        ![Nyissa meg tooVolumes panel](./media/storsimple-8000-manage-volumes-u2/modifyvol2.png)

    2. A táblázatos kötetek listája hello ki hello kötetet, és kattintson a jobb gombbal a tooinvoke hello helyi menü. Válassza ki **offline állapotba** tootake hello kötet offline módosítja.

        ![Válassza ki, és helyezze offline állapotba a kötet](./media/storsimple-8000-manage-volumes-u2/modifyvol4.png)

3. A hello **offline állapotba** panelt, tekintse át a hello hatása hello kötet offline állapotba helyezése, és válassza ki a megfelelő jelölőnégyzet hello. Kattintson a **offline állapotba**. 

    ![Tekintse át a kötet offline állapotba helyezése hatása](./media/storsimple-8000-manage-volumes-u2/modifyvol5.png)
      
      Értesítést kap hello kötet offline állapotban. hello kötet állapota tooOffline is frissíti.
      
4. Miután egy kötet nem érhető el, ha hello kötet, és kattintson a jobb gombbal, **online állapotba hozás** elem hello helyi menü elérhetővé válik.

> [!NOTE]
> Hello **Offline állapotba** parancs küld egy kérelem toohello eszköz tootake hello kötet offline állapotba. Gazdagépek hello kötet használják, ha az eredmény megszakadt kapcsolat, de hello kötet offline állapotba helyezése nem sikertelen lesz.

## <a name="delete-a-volume"></a>Kötet törlése

> [!IMPORTANT]
> Törölheti a kötet csak akkor, ha offline állapotban.

Hajtsa végre a következő lépéseket toodelete kötet hello.

#### <a name="toodelete-a-volume"></a>a kötet toodelete

1. Nyissa meg tooyour StorSimple Device Manager szolgáltatást, és kattintson a **eszközök**. Hello táblázatos felsorolása hello eszközökre, válassza ki, hogy szeretné-e toomodify hello kötet hello-eszköz. Kattintson a **beállítások > kötetek**.

    ![Nyissa meg tooVolumes panel](./media/storsimple-8000-manage-volumes-u2/modifyvol2.png)

3. Hello állapotának hello kötet toodelete szeretné. Ha azt szeretné, hogy toodelete hello kötet nem offline állapotban, offline állapotba először. Hello kövesse [kötet offline állapotba](#take-a-volume-offline).
4. Miután hello kötet offline állapotban, jelölje be a hello kötet, kattintson a jobb gombbal a tooinvoke hello helyi menüt, és válassza **törlése**.

    ![Helyi menüből válassza törlése](./media/storsimple-8000-manage-volumes-u2/deletevol1.png)

5. A hello **törlése** panelt, tekintse meg és válassza hello jelölőnégyzet hello hatása kötet törlése ellen. Ha töröl egy kötetet, hello hello köteten található összes adat elvész. 

    ![Mentse és módosításának megerősítése](./media/storsimple-8000-manage-volumes-u2/deletevol2.png)

6. Hello kötet törlését követően a hello táblázatos kötetek listáját frissíti tooindicate hello törlését.

    ![Frissített kötet listája](./media/storsimple-8000-manage-volumes-u2/deletevol3.png)
   
   > [!NOTE]
   > Ha töröl egy helyileg rögzített kötetet, hello szabad lemezterület az új kötetekre nem frissülhet azonnal. hello StorSimple Device Manager szolgáltatás hello helyi hely rendszeres időközönként frissíti. Javasoljuk, hogy néhány percig, mielőtt újból toocreate hello új kötet várja.
   >
   > Emellett ha töröl egy helyileg rögzített kötet, és törölje a másikra helyileg rögzített kötet azonnal ezután hello kötet törlési feladatok egymás után futnak. hello első kötet törlési feladat Befejezés hello tovább kötet törlési feladat megkezdése előtt.

## <a name="monitor-a-volume"></a>A kötet figyelése

Mennyiségi figyelési lehetővé teszi egy kötet toocollect I/O-hez kapcsolódó statisztikája. Figyelési hello alapértelmezés szerint engedélyezve van az Ön által létrehozott első 32 kötetek. A további kötetek figyelés alapértelmezés szerint le van tiltva. 

> [!NOTE]
> A klónozott kötetek figyelés alapértelmezés szerint le van tiltva.


Hajtsa végre a következő lépéseket tooenable hello, vagy tiltsa le a kötet megfigyelését.

#### <a name="tooenable-or-disable-volume-monitoring"></a>tooenable vagy a kötet figyelés letiltásakor

1. Nyissa meg tooyour StorSimple Device Manager szolgáltatást, és kattintson a **eszközök**. Hello táblázatos felsorolása hello eszközökre, válassza ki, hogy szeretné-e toomodify hello kötet hello-eszköz. Kattintson a **beállítások > kötetek**.
2. A táblázatos kötetek listája hello ki hello kötetet, és kattintson a jobb gombbal a tooinvoke hello helyi menü. Válassza ki **módosítása**.
3. A hello **kötet módosítása** panelen a **figyelés** kiválasztása **engedélyezése** vagy **tiltsa le a** tooenable vagy tiltsa le a figyelését.

    ![Tiltsa le a figyelése](./media/storsimple-8000-manage-volumes-u2/monitorvol1.png) 

4. Kattintson a **mentése** és megerősítést kér, amikor kattintson **Igen**. Azure-portálon hello frissítési hello kötetet és egy üzenetet, miután hello kötet frissítése sikeres volt értesítést jelenít meg.

## <a name="next-steps"></a>Következő lépések

* Ismerje meg, hogyan túl[klónozza a StorSimple-kötet](storsimple-8000-clone-volume-u2.md).
* Ismerje meg, hogyan túl[használata hello StorSimple Device Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-8000-manager-service-administration.md).

