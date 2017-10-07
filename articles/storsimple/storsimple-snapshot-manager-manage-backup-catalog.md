---
title: "aaaStorSimple Snapshot Manager biztonságimásolat-katalógus |} Microsoft Docs"
description: "Útmutatás a toouse StorSimple Snapshot Manager MMC beépülő modul tooview hello és hello biztonságimásolat-katalógus kezelése."
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: 
ms.assetid: 6abdbfd2-22ce-45a5-aa15-38fae4c8f4ec
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: v-sharos
ms.openlocfilehash: 173410095bcec7948d780d7fc258d2e3670bde8c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-storsimple-snapshot-manager-toomanage-hello-backup-catalog"></a>Használja a StorSimple Snapshot Manager toomanage hello biztonságimásolat-katalógus

## <a name="overview"></a>Áttekintés
hello elsődleges funkciója a StorSimple Snapshot Manager tooallow meg toocreate alkalmazáskonzisztens biztonsági mentését másolja át a StorSimple-köteteket a pillanatképek hello formátumban. A pillanatképek majd szereplő nevű XML-fájl egy *biztonságimásolat-katalógus*. hello biztonságimásolat-katalógus rendezi a pillanatképek a kötet csoportot, majd a helyi pillanatfelvétel és felhőbeli pillanatfelvétel.

Ez az oktatóanyag leírja, hogyan használhatja a hello **biztonságimásolat-katalógus** csomópont toocomplete hello a következő feladatokat:

* A kötet visszaállítása
* Egy kötet vagy kötet klónozása
* Biztonsági
* A fájl helyreállításához
* Hello Storsimple Snapshot Manager adatbázisának visszaállítása

Hello kibontásával megtekintheti az hello biztonságimásolat-katalógus **biztonságimásolat-katalógus** hello csomópontja **hatókör** ablaktáblán, és majd a hello kötet csoport kiterjesztése.

* Ha hello kötet csoportnév gombra kattint, hello **eredmények** ablaktábla megjeleníti azokat a helyi pillanatképeket és felhőbeli pillanatképeket hello kötet csoportra elérhető hello száma. 
* Ha **helyi pillanatfelvétel** vagy **felhőalapú pillanatfelvétel**, hello **eredmények** ablaktáblán látható minden egyes biztonsági mentési pillanatképet kapcsolatos információkat a következő hello (attól függően, hogy a **Nézet** beállítások):
  
  * **Név** – hello idő hello pillanatkép készítése.
  * **Típus** – hogy ez-e a helyi pillanatfelvétel vagy egy felhőalapú pillanatfelvétel.
  * **Tulajdonos** – hello tartalom tulajdonosa. 
  * **Rendelkezésre álló** – hello pillanatkép jelenleg elérhető-e. **Igaz** azt jelzi, hogy hello pillanatkép érhető el, és visszaállítása végezhető el; **Hamis** azt jelzi, hogy hello pillanatkép már nem érhető el. 
  * **Importált** – hogy hello backup importálták. **Igaz** jelzi a biztonsági mentését végző hello importált hello StorSimple Device Manager hello idő hello eszközön szolgáltatás konfigurálása a StorSimple Snapshot Manager; **Hamis** azt jelzi, hogy nem lett importálva, de a StorSimple Snapshot Manager által lett létrehozva. (Egyszerűbb azonosítani egy importált kötet csoport egy utótagot ad hozzá, amely azonosítja a hello eszköz, amelyből hello kötet csoport lett importálva, mert.)
    
    ![Biztonságimásolat-katalógus](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Backup_catalog.png)
* Ha kibontja **helyi pillanatfelvétel** vagy **felhőalapú pillanatfelvétel**, majd kattintson az egyes pillanatfelvétel neve hello **eredmények** ablaktábla megjeleníti azokat a következő információ hello hello kiválasztott pillanatkép:
  
  * **Név** – hello meghajtóbetűjellel azonosított kötetet. 
  * **Helyi** – hello helyi név hello meghajtó (ha elérhető). 
  * **Eszköz** – hello mely hello köteten található hello eszköz nevét. 
  * **Rendelkezésre álló** – hello pillanatkép jelenleg elérhető-e. **Igaz** azt jelzi, hogy hello pillanatkép érhető el, és visszaállítása végezhető el; **Hamis** azt jelzi, hogy hello pillanatkép már nem érhető el. 

## <a name="restore-a-volume"></a>A kötet visszaállítása
Használja a következő eljárás toorestore egy kötetet a biztonsági másolatból hello.

#### <a name="prerequisites"></a>Előfeltételek
Ha még nem tette meg, hozzon létre egy kötet és a kötet csoport, és törölje a hello kötet. Alapértelmezés szerint a StorSimple Snapshot Manager biztonsági mentést készít egy kötet lehetővé tevő toobe törlése előtt. Ez okokból előfordulhat, hogy adatvesztés Ha hello kötetet véletlenül, vagy ha hello adatok helyreállítása bármilyen okból toobe. 

StorSimple Snapshot Manager hello amikor hello elővigyázatos biztonsági mentés létrehozza a következő üzenetet jeleníti meg.

![Automatikus pillanatkép üzenet](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Automatic_snap.png) 

> [!IMPORTANT]
> A kötet olyan kötet csoport részét képező nem törölhető. hello törlési beállítás nem érhető el. <br>
> 
> 

#### <a name="toorestore-a-volume"></a>a kötet toorestore
1. Kattintson a hello asztali ikon toostart StorSimple Snapshot Manager. 
2. A hello **hatókör** ablaktáblában bontsa ki a hello **biztonságimásolat-katalógus** csomópontot, bontsa ki a kötet csoportot, és kattintson a **helyi pillanatképeket** vagy **felhőalapú pillanatfelvételek**. A biztonsági mentési pillanatképek listája jelenik meg hello **eredmények** ablaktáblán.
3. Megjeleníteni kívánt toorestore, kattintson a jobb gombbal, és kattintson a keresés hello biztonsági mentés **visszaállítása**.
   
    ![Állítsa vissza a biztonságimásolat-katalógus](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Restore_BU_catalog.png) 
4. Írja be a hello megerősítése lapon tekintse át hello részleteit, **megerősítése**, és kattintson a **OK**. StorSimple Snapshot Manager hello biztonsági mentési toorestore hello kötetet használ.
   
    ![Állítsa vissza a jóváhagyást kérő üzenet](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Restore_volume_msg.png) 
5. Figyelheti hello visszaállítási művelet futtatása közben. A hello **hatókör** ablaktáblában bontsa ki a hello **feladatok** csomópontra, majd **futtató**. hello feladat részletei megjelennek a hello **eredmények** ablaktáblán. Ha hello visszaállítási feladat befejeződött, hello feladat részletei átvitt toohello **utolsó 24 óra** listája.

## <a name="clone-a-volume-or-volume-group"></a>Egy kötet vagy kötet klónozása
A következő eljárás toocreate duplikált (Klónozás) egy kötet vagy a kötet csoport hello használata.

#### <a name="tooclone-a-volume-or-volume-group"></a>a kötet tooclone vagy kötet csoport
1. Kattintson a hello asztali ikon toostart StorSimple Snapshot Manager.
2. A hello **hatókör** ablaktáblában bontsa ki a hello **biztonságimásolat-katalógus** csomópontot, bontsa ki a kötet csoportot, és kattintson a **felhőalapú pillanatfelvételek**. Biztonsági mentések listája jelenik meg a hello **eredmények** ablaktáblán.
3. Hello kötet vagy a kötet csoport kívánja tooclone, kattintson a jobb gombbal hello kötet vagy kötet csoport nevét, és kattintson a található **Klónozás**. Hello **klón felhő pillanatkép** párbeszédpanel jelenik meg.
   
    ![Egy felhőalapú pillanatfelvétel klónozása](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Clone.png) 
4. Teljes hello **klón felhő pillanatkép** párbeszédpanel az alábbiak szerint: 
   
   1. A hello **neve** szövegmezőbe írja be egy nevet a hello kötet klónozása. Ez a név fog megjelenni hello **kötetek** csomópont. 
   2. (Nem kötelező) jelölje ki **meghajtó**, majd válassza ki a meghajtóbetűjel hello legördülő listából.
   3. (Nem kötelező) jelölje ki **mappa (NTFS)**, és írja be a mappa elérési útját, vagy kattintson a Tallózás gombra és hello mappa helyének kiválasztására. 
   4. Kattintson a **Create** (Létrehozás) gombra.
5. Hello a Klónozási folyamat befejezésekor inicializálnia kell a klónozott hello kötet. Indítsa el a Kiszolgálókezelőt, és indítsa el a Lemezkezelés eszközben. Részletes útmutatásért lásd: [csatlakoztassa köteteket](storsimple-snapshot-manager-manage-volumes.md#mount-volumes). Inicializált, miután hello kötet alatt jelenik meg hello **kötetek** hello csomópontja **hatókör** ablaktáblán. Ha nem látja a felsorolt hello kötetet, kötetek hello listájának frissítése (kattintson a jobb gombbal hello **kötetek** csomópontra, majd **frissítése**).

## <a name="delete-a-backup"></a>Biztonsági
Használja a következő eljárás toodelete pillanatkép hello biztonsági mentési katalógusból hello. 

> [!NOTE]
> A pillanatképek törlésével törli a biztonsági másolat hello pillanatkép társított hello. Azonban a hello folyamata hello felhőből adatok törlése eltarthat egy ideig.<br>


#### <a name="toodelete-a-backup"></a>a biztonsági mentés toodelete
1. Kattintson a hello asztali ikon toostart StorSimple Snapshot Manager.
2. A hello **hatókör** ablaktáblában bontsa ki a hello **biztonságimásolat-katalógus** csomópontot, bontsa ki a kötet csoportot, és kattintson a **helyi pillanatképeket** vagy **felhőalapú pillanatfelvételek**. Pillanatképek listája jelenik meg a hello **eredmények** ablaktáblán.
3. Kattintson a jobb gombbal hello pillanatkép toodelete szeretne, és kattintson a **törlése**.
4. Hello jóváhagyást kérő üzenet megjelenésekor kattintson **OK**.

## <a name="recover-a-file"></a>A fájl helyreállításához
Ha egy kötet véletlenül töröl egy fájlt, helyreállíthatja hello fájl lekérésével olyan pillanatképet, amely előre dátumok hello törlés használatával hello pillanatkép toocreate hello kötet másolat, és majd a hello fájl másolása hello klónozott kötet toohello eredeti kötet.

#### <a name="prerequisites"></a>Előfeltételek
Mielőtt elkezdené, győződjön meg arról, hogy rendelkezik-e hello kötet csoport aktuális biztonsági másolata. Ezt követően hello köteteket a kötet csoport egyik tárolt fájlok törléséhez. Végezetül a következő lépéseket toorestore hello használata hello fájl törölve a biztonsági mentésből. 

#### <a name="toorecover-a-deleted-file"></a>a törölt fájl toorecover
1. Kattintson a hello StorSimple Snapshot Manager ikonra az asztalon. hello StorSimple Snapshot Manager console ablakban jelenik meg. 
2. A hello **hatókör** ablaktáblában bontsa ki a hello **biztonságimásolat-katalógus** csomópontot és tallózási tooa pillanatkép törlése hello fájlt tartalmazó. Ki kell jelölni általában hello törlése előtt készült pillanatképet.
3. Megjeleníteni kívánt tooclone, kattintson a jobb gombbal, majd kattintson a keresés hello kötet **Klónozás**. Hello **klón felhő pillanatkép** párbeszédpanel jelenik meg.
   
    ![Egy felhőalapú pillanatfelvétel klónozása](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Clone.png) 
4. Teljes hello **klón felhő pillanatkép** párbeszédpanel az alábbiak szerint: 
   
   1. A hello **neve** szövegmezőbe írja be egy nevet a hello kötet klónozása. Ez a név fog megjelenni hello **kötetek** csomópont. 
   2. (Választható) Válassza ki **meghajtó**, majd válassza ki a meghajtóbetűjel hello legördülő listából. 
   3. (Választható) Válassza ki **mappa (NTFS)**, és írja be a mappa elérési útját, vagy kattintson a **Tallózás** és hello mappa helyének kiválasztására. 
   4. Kattintson a **Create** (Létrehozás) gombra. 
5. Hello a Klónozási folyamat befejezésekor inicializálnia kell a klónozott hello kötet. Indítsa el a Kiszolgálókezelőt, és indítsa el a Lemezkezelés eszközben. Részletes útmutatásért lásd: [csatlakoztassa köteteket](storsimple-snapshot-manager-manage-volumes.md#mount-volumes). Inicializált, miután hello kötet alatt jelenik meg hello **kötetek** hello csomópontja **hatókör** ablaktáblán. 
   
    Ha nem látja a felsorolt hello kötetet, kötetek hello listájának frissítése (kattintson a jobb gombbal hello **kötetek** csomópontra, majd **frissítése**).
6. Klónozott hello kötetet tartalmazó megnyitott hello NTFS-mappába bontsa ki a hello **kötetek** csomópontját, és klónozták, majd nyissa meg hello kötet. Szeretné, hogy toorecover, és másolja toohello elsődleges kötet hello fájl található.
7. Hello fájl visszaállítása után törölheti a klónozott hello kötetet tartalmazó hello NTFS-mappába.

## <a name="restore-hello-storsimple-snapshot-manager-database"></a>Hello StorSimple Snapshot Manager adatbázisának visszaállítása
Rendszeresen készítsen biztonsági másolatot hello StorSimple Snapshot Manager adatbázis hello állomáson. Ha katasztrófa történik, vagy hello gazdaszámítógépen bármilyen okból nem sikerül, majd visszaállíthatja hello biztonsági másolatból. Létrehozás hello az adatbázis biztonsági mentése egy kézi művelet.

#### <a name="tooback-up-and-restore-hello-database"></a>tooback mentése és visszaállítása hello adatbázis
1. Állítsa le a Microsoft a StorSimple szolgáltatás hello:
   
   1. Indítsa el a Kiszolgálókezelőt.
   2. Hello Kiszolgálókezelő irányítópulton hello **eszközök** menü **szolgáltatások**.
   3. A hello **szolgáltatások** ablakban, a select hello **Microsoft StorSimple szolgáltatás**.
   4. Hello a jobb oldali ablaktáblán, a **Microsoft StorSimple szolgáltatás**, kattintson a **hello szolgáltatás leállítása**.
2. Hello állomáson tooC:\ProgramData\Microsoft\StorSimple\BACatalog tallózása 
   
   > [!NOTE]
   > ProgramData mappa rejtett.
   > 
   > 
3. Egy biztonságos helyre, vagy hello felhőben hello katalógus XML-fájl, hello fájl másolása és tároló hello példány keresése. Ha hello gazdagép meghibásodik, a biztonságimásolat-fájl toohelp helyreállítás hello biztonsági mentési házirendek létrehozott StorSimple Snapshot Manager is használhatja.
   
    ![Az Azure StorSimple biztonsági mentési katalógusfájl](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_bacatalog.png)
4. Indítsa újra a Microsoft a StorSimple szolgáltatás hello: 
   
   1. Hello Kiszolgálókezelő irányítópulton hello **eszközök** menü **szolgáltatások**.
   2. A hello **szolgáltatások** ablakban, a select hello **Microsoft StorSimple szolgáltatás**.
   3. Hello a jobb oldali ablaktáblán, a **Microsoft StorSimple szolgáltatás**, kattintson a **hello szolgáltatás újraindítása**.
5. Hello állomáson tooC:\ProgramData\Microsoft\StorSimple\BACatalog tallózása 
6. Törölje a hello katalógus XML-fájlt, és cserélje le azt a létrehozott hello biztonsági másolatot. 
7. Kattintson a hello asztali StorSimple Snapshot Manager ikon toostart StorSimple Snapshot Manager. 

## <a name="next-steps"></a>Következő lépések
* További információ [használatával StorSimple Snapshot Manager tooadminister a StorSimple megoldásban](storsimple-snapshot-manager-admin.md).
* További információ [StorSimple Snapshot Manager feladatok és a munkafolyamatok](storsimple-snapshot-manager-admin.md#storsimple-snapshot-manager-tasks-and-workflows).

