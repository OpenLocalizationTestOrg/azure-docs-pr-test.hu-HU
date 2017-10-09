---
title: "a StorSimple virtuális tömb aaaManage kötetek |} Microsoft Docs"
description: "StorSimple Device Manager hello ismerteti és bemutatja hogyan toouse azt a StorSimple virtuális tömb toomanage köteteket."
services: storsimple
documentationcenter: 
author: manuaery
manager: syadav
editor: 
ms.assetid: caa6a26b-b7ba-4a05-b092-1a79450225cf
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2016
ms.author: manuaery
ms.openlocfilehash: 46aa6d7508b3e62f75a3b78ed73302b88320a0f9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-storsimple-device-manager-service-toomanage-volumes-on-hello-storsimple-virtual-array"></a>StorSimple az Eszközkezelő szolgáltatás toomanage kötetet a StorSimple virtuális tömb hello

## <a name="overview"></a>Áttekintés

Ez az oktatóanyag azt ismerteti, hogyan toouse hello StorSimple Device Manager szolgáltatás toocreate, és a StorSimple virtuális tömb kötetek kezelése.

StorSimple Device Manager szolgáltatás hello egy bővítmény hello Azure-portál, amely lehetővé teszi a StorSimple megoldásban egy egyetlen webes felhasználói felületen keresztüli kezelését. Továbbá toomanaging megosztásokat és -kötetek, a is hello StorSimple Device Manager szolgáltatás tooview használja és eszközök kezeléséhez, megtekintheti a riasztásokat, és megtekinthető és kezelhető biztonsági mentési házirendek és hello biztonságimásolat-katalógus.

## <a name="volume-types"></a>Kötet típusok

StorSimple-köteteket lehet:

* **Helyileg rögzített**: ezek a kötetek adatainak mindig hello tömb marad, és nem kerülnek toohello felhő.
* **Rétegzett**: ezek a kötetek adatainak toohello felhő is kerülnek. Ha rétegzett kötetet hoz létre, körülbelül 10 %-a hello terület hello helyi rétegen van kiépítve és 90 % hello terület hello felhőben regisztráltak-e. Például ha 1 TB-os kötet létesített, 100 GB hello helyi terület kellene lennie, és 900 GB lesz felhasználva a következőben hello felhő amikor hello adat szintet. Ez viszont azt jelenti, hogy minden hello helyi tárhely fogyjon hello eszközön, ha nem használhatók a rétegzett kötetek (mert hello 10 % szükséges, a helyi hello réteg nem lesz elérhető).

### <a name="provisioned-capacity"></a>Kiosztott kapacitást
Tekintse meg a következő táblázat az egyes mennyiségi maximális kiosztott kapacitást toohello.

| **Korlát azonosítója**                                       | **Korlát**     |
|------------------------------------------------------------|---------------|
| A rétegzett kötetek minimális mérete                            | 500 GB        |
| A rétegzett kötetek maximális mérete                            | 5 TB          |
| Egy helyileg rögzített kötet minimális mérete                    | 50 GB         |
| Egy helyileg rögzített kötet maximális mérete                    | 500 GB        |

## <a name="hello-volumes-blade"></a>hello kötetek panel
Hello **kötetek** menü a StorSimple szolgáltatás összefoglaló panelen megjeleníti egy adott StorSimple tömb tárolási köteteket hello listát, és lehetővé teszi a toomanage őket.

![Kötetek panel](./media/storsimple-virtual-array-manage-volumes/volumes-blade.png)

A kötet attribútumainak állnak:

* **Kötet neve** – egy leíró nevet, amely egyedinek kell lennie, és azonosíthatja a hello kötet.
* **Állapot** – lehet online vagy offline állapotú. Ha egy kötet offline állapotban, nincs látható tooinitiators (kiszolgálók), amelyek számára engedélyezett a hozzáférés toouse hello kötet.
* **Típus** – azt jelzi, hogy hello kötet **rétegzett** (alapértelmezett hello) vagy **helyileg rögzített**.
* **Kapacitás** – hello kezdeményező (kiszolgáló) tárolt adatok teljes mennyiségének összehasonlított toohello használt hello adatmennyiség határozza meg.
* **Biztonsági mentési** – abban az esetben a StorSimple virtuális tömb hello, az összes kötegről automatikusan engedélyezve vannak a biztonsági mentéshez.
* **Kapcsolódó állomások** – Itt adhatja meg, amelyek számára engedélyezett a hozzáférés toothis kötet hello kezdeményezőktől (kiszolgálóktól).

![Kötetek részletei](./media/storsimple-virtual-array-manage-volumes/volume-details.png)

Ez a következő feladatok oktatóanyag tooperform hello használata hello utasításait:

* Kötet hozzáadása
* A kötet módosítása
* A kötet offline állapotba helyezése
* Kötet törlése

## <a name="add-a-volume"></a>Kötet hozzáadása

1. Hello StorSimple szolgáltatás összefoglaló panelen, kattintson a **+ kötet hozzáadása** hello parancs segítségével. Ezzel megnyílik hello **kötet hozzáadása** panelen.
   
    ![Kötet hozzáadása](./media/storsimple-virtual-array-manage-volumes/add-volume.png)
2. A hello **kötet hozzáadása** panelen a következő hello:
   
   * A hello **kötetneve** mezőben adjon meg egy egyedi nevet a kötethez. hello neve 3 too127 karaktert tartalmazó karakterláncnak kell lennie.
   * A hello **típus** legördülő listában, adja meg, hogy toocreate egy **rétegzett** vagy **helyileg rögzített** kötet. Helyi garanciákat, kis késleltetést és magasabb teljesítményt igénylő munkaterheléseknél válasszon **helyileg rögzített kötet**. Minden más adathoz válasszon **rétegzett** kötet.
   * A hello **kapacitás** mezőben adja meg a hello hello kötet méretét. A rétegzett kötetek 500 GB és 5 TB között kell lennie, és egy helyileg rögzített kötet 50 GB-os és 500 GB között kell lennie.
   * * Kattintson a **csatlakozó állomások**, válassza ki a hozzáférési vezérlési rekordot (ACR) megfelelő toohello iSCSI-kezdeményező szeretné, hogy tooconnect toothis kötet, és kattintson **válasszon**.
3. új csatlakoztatott gazdagépek esetén tooadd kattintson **új hozzáadása**, adjon meg egy nevet a hello állomás és az iSCSI minősített nevét (IQN), és kattintson **Hozzáadás**.
   
    ![Kötet hozzáadása](./media/storsimple-virtual-array-manage-volumes/volume-add-acr.png)
4. Amikor elkészült, a kötet konfigurálása, kattintson a **létrehozása**. A kötet jön létre a megadott hello beállításait, és megjelenik egy értesítés a hello sikeres létrehozása, hello azonos. Alapértelmezés szerint biztonsági mentés hello kötet engedélyezve lesz.
5. amely a kötet hello tooconfirm lett sikeresen létrehozva, lépjen toohello **kötetek** panelen. Meg kell jelenniük a felsorolt hello kötetet.
   
    ![Kötet létrehozása sikeres](./media/storsimple-virtual-array-manage-volumes/volume-success.png)

## <a name="modify-a-volume"></a>A kötet módosítása

A kötet módosításával toochange hello állomások hello kötet elérő van szüksége. hello egyéb attribútumai a kötet nem módosítható hello kötet létrehozása után.

#### <a name="toomodify-a-volume"></a>a kötet toomodify

1. A hello **kötetek** hello StorSimple szolgáltatás összefoglaló panelen beállításnál válassza hello virtuális tömb mely hello toomodify kívánja meg kötet található.
2. **Válassza ki** hello kötet, és kattintson a **csatlakozó állomások** tooview hello a jelenleg csatlakozó állomás, és módosítsa azt tooa másik kiszolgálóra.
   
    ![Kötet szerkesztése](./media/storsimple-virtual-array-manage-volumes/volume-edit-acr.png)
3. A módosítások mentéséhez kattintson a hello **mentése** parancssávon. A megadott beállítások lesznek alkalmazva, és megjelenik egy értesítés.

## <a name="take-a-volume-offline"></a>A kötet offline állapotba helyezése

Szükség lehet egy kötet offline tootake tervezésekor toomodify vagy törölje azt. Ha egy kötet offline állapotban, nincs olvasási és írási hozzáférése érhető el. Szüksége lesz tootake hello kötet offline hello állomáson, valamint hello eszközön.

#### <a name="tootake-a-volume-offline"></a>a kötet offline tootake

1. Győződjön meg arról, hogy a szóban forgó hello kötet nem offline állapotba helyezése előtt használatban van.
2. Igénybe hello kötet offline hello állomás első. Ezzel a megoldással adatsérülés hello köteten lehetséges kockázatát. Lépéseit tekintse meg a gazda operációs rendszer toohello utasításokat.
3. Miután hello gazdagépen hello kötet offline állapotban, igénybe hello kötet offline hello tömb hello lépések végrehajtásával:
   
   * A hello **kötetek** hello StorSimple szolgáltatás összefoglaló panelen beállításnál válassza hello virtuális tömb mely hello tootake kapcsolat nélkül kívánja azt kötet található.
   * **Válassza ki** hello kötet, és kattintson a **...**  (felváltva kattintson a jobb gombbal a sorhoz) hello helyi menüből válassza ki a **offline állapotba**.
     
        ![Kapcsolat nélküli kötet](./media/storsimple-virtual-array-manage-volumes/volume-offline.png)
   * Tekintse át a hello hello információkat **offline állapotba** panel és elfogadta hello műveletet. Kattintson a **offline állapotba** tootake hello kötet offline állapotba. Megjelenik egy értesítés, amely hello művelet folyamatban van.
   * tooconfirm hello kötet sikeresen készült offline, nyissa meg toohello **kötetek** panelen. Kapcsolat nélküli, meg kell jelennie hello hello kötetre vonatkozó állapotának.
     
       ![Kapcsolat nélküli kötet megerősítése](./media/storsimple-virtual-array-manage-volumes/volume-offline-confirm.png)

## <a name="delete-a-volume"></a>Kötet törlése

> [!IMPORTANT]
> Törölheti a kötet csak akkor, ha offline állapotban.
> 
> 

Hajtsa végre a következő lépéseket toodelete kötet hello.

#### <a name="toodelete-a-volume"></a>a kötet toodelete

1. A hello **kötetek** hello StorSimple szolgáltatás összefoglaló panelen beállításnál válassza hello virtuális tömb mely hello toodelete kívánja meg kötet található.
2. **Válassza ki** hello kötet, és kattintson a **...**  (felváltva kattintson a jobb gombbal a sorhoz) hello helyi menüből válassza ki a **törlése**.
   
    ![Kötet törlése](./media/storsimple-virtual-array-manage-volumes/volume-delete.png)
3. Hello állapotának hello kötet toodelete szeretné. Ha azt szeretné, hogy toodelete hello kötet nem offline állapotban, végezze el az offline első, a következő hello lépéseket szereplő [kötet offline állapotba](#take-a-volume-offline).
4. Ha a hello megerősítést kér **törlése** panelen fogadja el a hello megerősítő, majd kattintson **törlése**. a program törli hello kötet, és hello **kötetek** panelen megjelenik a kötetek hello virtuális tömbön belüli hello frissített listáját.

## <a name="next-steps"></a>Következő lépések

Ismerje meg, hogyan túl[klónozza a StorSimple-kötet](storsimple-virtual-array-clone.md).

