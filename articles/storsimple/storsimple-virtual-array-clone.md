---
title: "a StorSimple virtuális tömb biztonsági aaaClone |} Microsoft Docs"
description: "Megtudhatja, hogyan tooclone a biztonsági mentés és a fájlok visszaállítására a StorSimple virtuális tömbből."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: af6e979c-55e3-477c-b53e-a76a697f80c9
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/21/2016
ms.author: alkohli
ms.openlocfilehash: 21bfcae48ee07762179cf00ce842b6094abe18ee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="clone-from-a-backup-of-your-storsimple-virtual-array"></a>A StorSimple virtuális tömb biztonsági másolatából klónozása

## <a name="overview"></a>Áttekintés

Ez a cikk ismerteti részletesen hogyan tooclone a biztonságimásolat-készlet, a megosztások vagy kötetek; a Microsoft Azure StorSimple virtuális tömbben. hello klónozott biztonsági mentés használt toorecover a törölt vagy elveszett fájlt. hello cikket is részletes lépéseket tooperform az elemszintű helyreállításhoz a StorSimple virtuális tömb fájlkiszolgálóként konfigurálni.

## <a name="clone-shares-from-a-backup-set"></a>Klónozás megosztások biztonságimásolat-készletből

**Mielőtt újból tooclone megosztások, ellenőrizze, hogy elegendő hely a hello eszköz toocomplete ezt a műveletet.** egy biztonsági másolatból, a hello tooclone [Azure-portálon](https://portal.azure.com/), hajtsa végre az alábbi lépésekkel hello.

#### <a name="tooclone-a-share"></a>a megosztás tooclone

1. Keresse meg a túl**eszközök** panelen. Válassza ki, és kattintson az eszköz majd **megosztások**. Válassza ki a megjeleníteni kívánt tooclone, kattintson a jobb gombbal hello megosztás tooinvoke hello helyi menü hello megosztás. Válassza ki **Klónozás**.
   
   ![A biztonsági mentés klónozása](./media/storsimple-virtual-array-clone/cloneshare1.png)
2. A hello **Klónozás** panelen kattintson a **biztonsági mentés > Válassza ki** és majd hello a következő: 
   
   a.    Az eszköz hello időtartomány alapján biztonsági szűréséhez. Választhat **elmúlt 7 napban**, **az elmúlt 30 napban**, és **múltbeli évre**.
   
   b.    Hello szűrt biztonsági mentések jelenik meg, jelölje ki a biztonsági mentési tooclone a.
   
   c.    Kattintson az **OK** gombra.
   
   ![A biztonsági mentés klónozása](./media/storsimple-virtual-array-clone/cloneshare3.png)
3. Hello a **Klónozás** panelen kattintson **céloz beállítások** és majd hello a következő:
   
   a.    Adja meg a fájlmegosztás nevét. hello megosztás nevének hossza csak 3-127 karakternél.
   
   b.    Megadhat hello klónozott megosztás leírását.
   
   c.    Hello megosztás vissza hello típusa nem módosítható. A rétegzett megosztás egy rétegzett és egy helyileg rögzített megosztást, a helyileg rögzített klónozták.
   
   d.    hello kapacitás be van állítva a rendszer a klónozott hello megosztás egyenlő toohello méretét.
   
   e.    Ez a megosztás hello Rendszergazdák hozzárendelése. Képes toomodify hello megosztás tulajdonságainak keresztül Fájlkezelőben hello Klónozás befejezése után lesz.
   
   f.    Kattintson az **OK** gombra.
   
   ![A biztonsági mentés klónozása](./media/storsimple-virtual-array-clone/cloneshare6.png)

4. Kattintson a **Klónozás** toostart egy klónozott feladat. Hello feladat befejezése után hello Klónozás elindul, és értesítést kap. a klón, nyissa meg toohello toomonitor hello folyamatban **feladatok** panel megnyitásához, és kattintson hello feladat tooview feladat részleteit.
5. Hello Klónozás sikeres létrehozását követően nyissa meg a visszafelé toohello **megosztások** panel az eszközön.
6. Az eszközön lévő megosztások listájának hello most már megtekintheti hello új klónozott megosztás. A rétegzett megosztás klónozták, rétegzett, valamint a helyileg rögzített helyileg rögzített megosztásként.
   
   ![A biztonsági mentés klónozása](./media/storsimple-virtual-array-clone/cloneshare10.png)

## <a name="clone-volumes-from-a-backup-set"></a>A biztonságimásolat-készletből kötetek klónozása

egy biztonsági másolatból, az Azure-portálon hello tooclone rendelkezik tooperform lépéseket hasonló toohello néhányat a meglévők közül a megosztás klónozásakor. hello Klónozás klónokat hello biztonsági mentési tooa új kötet hello az azonos virtuális eszköz; másik eszköz tooa nem klónozható.

#### <a name="tooclone-a-volume"></a>a kötet tooclone

1. Keresse meg a túl**eszközök** panelen. Válassza ki, és kattintson az eszköz majd **kötetek**. Kiválasztása hello kötet, amelyet az tooclone, kattintson a jobb gombbal hello kötet tooinvoke hello helyi menü. Válassza ki **Klónozás**.
   
   ![Kötet klónozása](./media/storsimple-virtual-array-clone/clonevolume1.png)
2. A hello **Klónozás** panelen kattintson a **biztonsági mentés** és majd hello a következő: 
   
   a.    Az eszköz hello időtartomány alapján biztonsági szűréséhez. Választhat **elmúlt 7 napban**, **az elmúlt 30 napban**, és **múltbeli évre**. 
   
   b.    Hello szűrt biztonsági mentések jelenik meg, jelölje ki a biztonsági mentési tooclone a.
   
   c.    Kattintson az **OK** gombra.
   
   ![A biztonsági mentés klónozása](./media/storsimple-virtual-array-clone/clonevolume3.png)
3. A hello **Klónozás** panelen kattintson **céloz kötet beállítások** és majd hello a következő::
   
   a. hello eszköznév automatikusan feltöltődik értékkel.
   
   b. Adjon meg egy kötet nevet hello **kötet klónozása**. hello kötetneve too127 3 karaktert kell tartalmaznia.
   
   c. hello kötettípus értéke toohello eredeti kötet automatikusan. A rétegzett kötetek klónozták, rétegzett, és egy helyileg rögzített kötetet, helyileg van rögzítve.
   
   d. A hello **csatlakozó állomások**, kattintson a **válasszon**.
   
   ![A biztonsági mentés klónozása](./media/storsimple-virtual-array-clone/clonevolume4.png)
4. A hello **csatlakozó állomások** panelen válassza ki a egy meglévő ACR, vagy adja hozzá egy új ACR. egy új ACR tooadd, szüksége lesz egy ACR neve tooprovide és hello állomás IQN-Nevének. Kattintson a **Kiválasztás** gombra.
   
   ![A biztonsági mentés klónozása](./media/storsimple-virtual-array-clone/clonevolume5.png)
5. Kattintson a **Klónozás** toolaunch egy klónozott feladat.
   
   ![A biztonsági mentés klónozása](./media/storsimple-virtual-array-clone/clonevolume6.png)  
6. Hello Klónozás feladat létrehozása után a Klónozás elindul. Hello Klónozás létrehozása után megjelenik a hello kötetek panel az eszközön. Vegye figyelembe, hogy a rétegzett kötetek klónozhatók-e, mivel a rétegzett, és egy helyileg rögzített kötet klónozták egy helyileg rögzített kötet.
   
   ![A biztonsági mentés klónozása](./media/storsimple-virtual-array-clone/clonevolume8.png)
7. Hello kötet hello listán a kötetek online jelenik meg, miután hello lemez használható. Hello iSCSI-kezdeményező gazdagép, az iSCSI-kezdeményező tulajdonságai ablakban célok hello listájának frissítése. Egy új cél hello klónozott kötet nevét tartalmazó, "inactive" hello állapot oszlopában jelenik meg.
8. Jelöljön ki hello célként, majd **Connect**. Miután hello kezdeményező csatlakoztatott toohello cél, hello kell állapotmódosítási túl**csatlakoztatva**.
9. A hello **Lemezkezelés** ablakban hello csatlakoztatott kötetek jelennek meg a hello a következő ábrán látható módon. Kattintson a jobb gombbal a hello felderített kötetre (kattintson a hello lemez nevére), és kattintson a **Online**.

> [!IMPORTANT]
> Ha próbált tooclone egy kötet vagy egy megosztást a biztonságimásolat-készletből, ha hello Klónozás feladat sikertelen lesz, egy tároló kötetet vagy megosztást továbbra is létrehozható hello portálon. Fontos, hogy törli a célkötet, vagy megosztás hello portál toominimize ehhez az elemhez jövőbeli kérdéseket.
> 
> 

## <a name="item-level-recovery-ilr"></a>Elemszintű helyreállítás (ILR)

Ebben a kiadásban hello elemszintű helyreállítást (ILR) fájlkiszolgálóként konfigurált egy StorSimple virtuális tömb vezet be. hello funkcióval toodo részletes fájlok és mappák helyreállítása minden hello megosztás felhőalapú biztonsági másolatából hello StorSimple eszközön. A törölt fájlok kérhetnek le legutóbbi biztonsági mentés egy önkiszolgáló modell használatával.

Minden fájlmegosztás tartozik egy *.backups* hello legfrissebb biztonsági másolatokat tartalmazó mappát. Keresse meg a toohello kívánt biztonsági mentés, másolja át a fontos fájlok és mappák hello biztonsági másolatból, és állítsa vissza őket. Ez a szolgáltatás megszünteti a hívások tooadministrators a fájlok visszaállítása biztonsági másolatból.

1. Hello ILR végrehajtásakor hello biztonsági mentések Fájlkezelőben keresztül tekintheti meg. Kattintson az adott megosztás hello toolook hello biztonsági mentés a kívánt. Látni fogja a *.backups* hello megosztást tároló összes hello biztonsági mentés alatt létrehozott mappára. Bontsa ki a hello *.backups* mappa tooview hello biztonsági mentéseket. hello mappa hello teljes biztonsági mentés hierarchia hello robbantott nézetét jeleníti meg. Ez a nézet igény jön létre, és csak néhány másodperc toocreate rendszerint tart.
   
   hello utolsó öt biztonsági mentés ily módon jelennek meg, és használt tooperform az elemszintű helyreállítás. hello öt legutóbbi biztonsági mentés tartalmaznia a hello alapértelmezett ütemezett és manuális biztonsági mentések hello.
   
   * **Ütemezett biztonsági mentések** megnevezett &lt;eszköznév&gt;DailySchedule-ÉÉÉÉHHNN-ÓÓPPMM-UTC.
   * **Manuális biztonsági mentések** nevű Ad-hoc-ÉÉÉÉHHNN-ÓÓPPMM-UTC formátumban.
     
     ![](./media/storsimple-virtual-array-clone/image14.png)

2. Hello biztonsági mentést tartalmazó hello törölt hello fájl legújabb verziójának azonosításához. Bár hello mappanevet tartalmaz az egyes esetekben megelőző hello UTC időbélyeggel, hello létrehozásakor, mely hello mappa az hello tényleges eszköz idő, amikor hello biztonságimásolat-készítése elindult. Hello mappa időbélyeg toolocate használja, és hello biztonsági másolatok meghatározása.

3. Keresse meg a hello mappát vagy a kívánt hello biztonsági mentése azonosított toorestore hello előző lépésben hello fájlt. Vegye figyelembe, hogy csak megtekinteni tudja hello fájlokat vagy mappákat, a jogosultsággal kell rendelkeznie. Ha egyes fájlok és mappák nem férhet hozzá, kérjen meg egy megosztást rendszergazdát. hello rendszergazdák Fájlkezelőben tooedit hello megosztási engedélyeket használ, és ad hozzáférési toohello adott fájlt vagy mappát. Javasoljuk, hogy a megosztás rendszergazda hello az a felhasználói csoport helyett egy-egy felhasználóhoz.

4. Hello fájl vagy hello mappa toohello megfelelő fájlkiszolgáló megosztott helyén a StorSimple másolja.

## <a name="next-steps"></a>Következő lépések

További tudnivalók túl[felügyelete a StorSimple virtuális tömb hello helyi webes felhasználói felület használatával](storsimple-ova-web-ui-admin.md).

