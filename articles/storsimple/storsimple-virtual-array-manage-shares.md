---
title: "aaaManage StorSimple virtuális tömb felhőszolgáltatásaival |} Microsoft Docs"
description: "StorSimple Device Manager hello ismerteti és bemutatja hogyan toouse azt a StorSimple virtuális tömb toomanage megosztásokat."
services: storsimple
documentationcenter: 
author: manuaery
manager: syadav
editor: 
ms.assetid: 0a799c83-fde5-4f3f-af0e-67535d1882b6
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2016
ms.author: manuaery
ms.openlocfilehash: 9b57d7ec7c0b7de5a22e1b816daa8852d0f32a48
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-toomanage-shares-on-hello-storsimple-virtual-array"></a>A StorSimple virtuális tömb hello hello StorSimple Device Manager szolgáltatás toomanage megosztásokat használata

## <a name="overview"></a>Áttekintés

Ez az oktatóanyag azt ismerteti, hogyan toouse hello StorSimple Device Manager szolgáltatás toocreate, és a StorSimple virtuális tömb-megosztások kezelése.

StorSimple Device Manager szolgáltatás hello egy bővítmény hello Azure-portál, amely lehetővé teszi a StorSimple megoldásban egy egyetlen webes felhasználói felületen keresztüli kezelését. Toomanaging megosztások hozzáadását és a kötetek akkor is hello StorSimple Device Manager szolgáltatás tooview használja és eszközök kezeléséhez, megtekintheti a riasztásokat, biztonsági mentési házirendek, valamint kezelheti és hello biztonságimásolat-katalógus.

## <a name="share-types"></a>Megosztás típusok

StorSimple megosztások lehet:

* **Helyileg rögzített**: ezek az adatok mindig hello tömb marad, és nem kerülnek toohello felhő.
* **Rétegzett**: ezek az adatok toohello felhő is kerülnek. Egy rétegzett megosztás létrehozásakor körülbelül 10 %-a hello terület hello helyi rétegen ki van építve, és 90 % hello terület hello felhőben lett kiépítve. Például ha egy 1 TB-os megosztás létesített, 100 GB hello helyi terület kellene lennie, és 900 GB lesz felhasználva a következőben hello felhő amikor hello adat szintet. Ez viszont azt jelenti, hogy minden hello helyi tárhely fogyjon hello eszközön, ha nem használhatók a rétegzett megosztás (mert hello 10 % szükséges, a helyi hello réteg nem lesz elérhető).

### <a name="provisioned-capacity"></a>Kiosztott kapacitást

Tekintse meg a következő táblázat az egyes megosztás maximális kiosztott kapacitást toohello.

| **Korlát azonosítója** | **Korlát** |
| --- | --- |
| A rétegzett megosztás minimális mérete |500 GB |
| A rétegzett megosztás maximális mérete |20 TB |
| Egy helyileg rögzített megosztás minimális mérete |50 GB |
| Egy helyileg rögzített megosztás maximális mérete |2 TB |

## <a name="hello-shares-blade"></a>hello megosztások panel

Hello **megosztások** menü a StorSimple szolgáltatás összefoglaló panelen megjeleníti egy adott StorSimple tömb storage-megosztásokat hello listát, és lehetővé teszi a toomanage őket.

![Megosztások panel](./media/storsimple-virtual-array-manage-shares/shares-blade.png)

A megosztás sorozatából attribútumok:

* **Megosztási név** – egy leíró nevet, amely egyedinek kell lennie, és azonosíthatja a hello megosztást.
* **Állapot** – lehet online vagy offline állapotú. Ha egy megosztást offline állapotban, hello megosztás felhasználóira nem lesz képes tooaccess azt.
* **Típus** – azt jelzi, hogy hello megosztás **rétegzett** (alapértelmezett hello) vagy **helyileg rögzített**.
* **Kapacitás** – hello megosztáson tárolt adatok teljes mennyiségének összehasonlított toohello használt hello adatmennyiség határozza meg.
* **Leírás** – választható beállítás, amely segít a hello megosztás leírása.
* **Engedélyek** -hello NTFS engedélyek toohello megosztást, amelyet a Windows Explorer használatával kezelhetők.
* **Biztonsági mentési** – abban az esetben a StorSimple virtuális tömb hello, minden megosztás automatikusan engedélyezve vannak a biztonsági mentéshez.

![Megosztások részletei](./media/storsimple-virtual-array-manage-shares/share-details.png)

Ez a következő feladatok oktatóanyag tooperform hello használata hello utasításait:

* Megosztás hozzáadása
* Egy megosztás módosítása
* Egy kapcsolat nélküli üzemmódra állítása
* Megosztás törlése

## <a name="add-a-share"></a>Megosztás hozzáadása

1. Hello StorSimple szolgáltatás összefoglaló panelen, kattintson a **+ Hozzáadás megosztás** hello parancs segítségével. Ezzel megnyílik hello **Hozzáadás megosztás** panelen.

    ![Adja hozzá a megosztás](./media/storsimple-virtual-array-manage-shares/add-share.png)

2. A hello **Hozzáadás megosztás** panelen a következő hello:
   
    1. A hello **megosztásnevet** mezőben adjon meg egy egyedi nevet a megosztáshoz. hello neve 3 too127 karaktert tartalmazó karakterláncnak kell lennie.

    2. Egy nem kötelező **leírás** hello megosztás. hello leírás azonosításához hello fájlmegosztás-tulajdonosok.

    3. A hello **típus** legördülő listában, adja meg, hogy toocreate egy **rétegzett** vagy **helyileg rögzített** megosztani. Helyi garanciákat, kis késleltetést és magasabb teljesítményt igénylő munkaterheléseknél válasszon **helyileg rögzített megosztás**. Minden más adathoz válasszon **rétegzett** megosztani.

    4. A hello **kapacitás** mezőben adja meg a hello megosztás hello méretét. A rétegzett megosztás 500 GB és 20 TB között kell lennie, és egy helyileg rögzített megosztás 50 GB-os és a 2 TB közé kell esnie.

    5. A hello **értékre alapértelmezett teljes körű engedélyekkel** mezőbe hello engedélyek toohello felhasználói vagy fér hozzá a megosztás hello csoport hozzárendelése. Adja meg a hello felhasználó vagy felhasználói csoport hello hello nevét  _john@contoso.com_  formátumban. Javasoljuk, hogy használjon egy felhasználói csoport (helyett egy-egy felhasználóhoz) tooallow rendszergazdai jogosultságokkal tooaccess megosztást. Miután itt hello engedélyek hozzárendelt, használhatja a Fájlkezelőben toomodify ezeket az engedélyeket.
3. Amikor elkészült, a megosztás konfigurálására, kattintson a **létrehozása**. A megosztás jön létre a megadott hello beállításait, és megjelenik egy értesítés. Alapértelmezés szerint biztonsági mentés hello megosztás engedélyezve lesz.
4. tooconfirm, hogy a megosztás hello lett sikeresen létrehozva, lépjen toohello **megosztások** panelen. Megosztás felsorolt hello kell megjelennie.
   
    ![Fájlmegosztás létrehozása sikerült](./media/storsimple-virtual-array-manage-shares/share-success.png)

## <a name="modify-a-share"></a>Egy megosztás módosítása

A megosztás módosításával hello megosztás toochange hello leírása van szüksége. Nincs más tulajdonságok hello megosztás létrehozása után módosítható.

#### <a name="toomodify-a-share"></a>a megosztás toomodify

1. A hello **megosztások** válasszon hello beállítása hello StorSimple szolgáltatás összefoglaló panelre, mely hello toomodify kívánja meg megosztás található virtuális tömb.
2. **Válassza ki** hello megosztás tooview hello aktuális leírása, és módosítsa azt.
3. A módosítások mentéséhez kattintson a hello **mentése** parancssávon. A megadott beállítások lesznek alkalmazva, és megjelenik egy értesítés.
   
    ![ Megosztás módosítása](./media/storsimple-virtual-array-manage-shares/share-edit.png)

## <a name="take-a-share-offline"></a>Egy kapcsolat nélküli üzemmódra állítása

Szükség lehet a megosztás nélküli tootake tervezésekor toomodify vagy törölje azt. Ha egy megosztást offline állapotban, nincs olvasási és írási hozzáférése érhető el. Szüksége lesz tootake hello megosztás nélküli hello állomáson, valamint hello eszközön.

#### <a name="tootake-a-share-offline"></a>a megosztás nélküli tootake

1. Győződjön meg arról, hogy a szóban forgó hello megosztás nem offline állapotba helyezése előtt használatban van.
2. Hello megosztás végrehajtása a kapcsolat nélküli hello tömb hello lépések végrehajtásával:
   
    1. A hello **megosztások** válasszon hello beállítása hello StorSimple szolgáltatás összefoglaló panelre, mely hello tootake kapcsolat nélkül kívánja azt megosztás található virtuális tömb.

    2. **Válassza ki** hello megosztást, és kattintson **...**  (felváltva kattintson a jobb gombbal a sorhoz) hello helyi menüből válassza ki a **offline állapotba**.
     
        ![Kapcsolat nélküli megosztás](./media/storsimple-virtual-array-manage-shares/shares-offline.png)

    3. Tekintse át a hello hello információkat **offline állapotba** panel és elfogadta hello műveletet. Kattintson a **offline állapotba** tootake hello offline megosztást. Megjelenik egy értesítés, amely hello művelet folyamatban van.

    4. tooconfirm, hogy a megosztás hello sikeresen készült offline, lépjen toohello **megosztások** panelen. Hello állapotának hello megosztás offline állapotúként kell megjelennie.

## <a name="delete-a-share"></a>Megosztás törlése

> [!IMPORTANT]
> Törölheti a megosztás csak akkor, ha offline állapotban.


Hajtsa végre a következő lépéseket toodelete megosztás hello.

#### <a name="toodelete-a-share"></a>a megosztás toodelete

1. A hello **megosztások** válasszon hello beállítása hello StorSimple szolgáltatás összefoglaló panelre, mely hello megosztáson kívánja toodelete található virtuális tömb.
2. **Válassza ki** hello megosztást, és kattintson **...**  (felváltva kattintson a jobb gombbal a sorhoz) hello helyi menüből válassza ki a **törlése**.
   
    ![Megosztás törlése](./media/storsimple-virtual-array-manage-shares/share-delete.png)
3. Hello állapotának hello megosztás toodelete szeretné. Ha azt szeretné, hogy toodelete hello megosztás nem offline állapotban, offline állapotba először. Hello kövesse [offline állapotba a megosztás](#take-a-share-offline).
4. Ha a hello megerősítést kér **törlése** panelen fogadja el a hello megerősítő, majd kattintson **törlése**. a program törli hello megosztás- és hello **megosztások** panel hello virtuális tömbön belüli megosztások hello frissített listáját jeleníti meg.

## <a name="next-steps"></a>Következő lépések
Ismerje meg, hogyan túl[klónozza a StorSimple megosztás](storsimple-virtual-array-clone.md).

