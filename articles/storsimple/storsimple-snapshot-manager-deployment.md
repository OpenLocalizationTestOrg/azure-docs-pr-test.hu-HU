---
title: StorSimple Snapshot Manager aaaDeploy |} Microsoft Docs
description: "Ismerje meg, hogyan toodownload és a telepítés hello StorSimple Snapshot Manager MMC beépülő modulként StorSimple adatok védelmét és biztonsági mentését szolgáltatások kezeléséhez."
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: 
ms.assetid: f0128f57-519e-49ec-9187-23575809cdbe
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: v-sharos
ms.openlocfilehash: dd90cca2bbb3410bb8cd459fb1a3ff3fb5f2997b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-hello-storsimple-snapshot-manager-mmc-snap-in"></a>Hello StorSimple Snapshot Manager MMC beépülő modul telepítése

## <a name="overview"></a>Áttekintés
StorSimple Snapshot Manager hello egy Microsoft Management Console (MMC) beépülő modulja, amely leegyszerűsíti az adatvédelem és biztonsági mentési kezelése a Microsoft Azure StorSimple környezetben. A StorSimple Snapshot Manager kezelheti a Microsoft Azure StorSimple a helyszíni és felhőbeli tárhelyén, mintha egy teljesen integrált tárolórendszer, így biztonsági mentési és helyreállítási folyamatok egyszerűsítése, és csökkenti a költségeket. 

Ez az oktatóanyag konfigurációs követelményeinek, valamint telepítése, eltávolítása és frissítése a StorSimple Snapshot Manager eljárásait ismerteti.

> [!NOTE]
> * StorSimple Snapshot Manager toomanage Microsoft Azure StorSimple virtuális tömbök (más néven StorSimple a helyszíni virtuális eszköz) nem használható.
> * Ha a StorSimple eszköz tooinstall StorSimple 2. frissítést, meg arról, hogy toodownload hello legújabb verziójú, a StorSimple Snapshot Manager, a telepítés **StorSimple 2. frissítés telepítése előtt**. hello a StorSimple Snapshot Manager legújabb verziójára visszamenőlegesen kompatibilis, és együttműködik a Microsoft Azure StorSimple kiadott verziói. Ha használ hello a StorSimple Snapshot Manager korábbi verziója, szüksége lesz a tooupdate informatikai (nem szükséges toouninstall hello előző verzió hello új verziójának telepítése előtt).


## <a name="storsimple-snapshot-manager-installation"></a>StorSimple Snapshot Manager telepítése
StorSimple Snapshot Manager hello Windows Server 2008 R2 SP1, Windows Server 2012 vagy Windows Server 2012 R2 operációs rendszert futtató számítógépeken telepíthető. Windows 2008 R2 rendszert futtató kiszolgálókon is telepítenie kell a Windows Server 2008 SP1 és a Windows Management Framework 3.0.

Ahhoz, hogy telepíteni vagy frissíteni hello StorSimple Snapshot Manager beépülő modul a Microsoft Management Console (MMC) hello, győződjön meg arról, hogy hello Microsoft Azure StorSimple eszközt, és gazdagép-kiszolgálón megfelelően van-e konfigurálva.

## <a name="configure-prerequisites"></a>Előfeltételek konfigurálása
hello lépések biztosítva a magas szintű áttekintését konfigurációs feladatot, hogy meg kell adnia a StorSimple Snapshot Manager hello telepítése előtt. A Microsoft Azure StorSimple konfigurációs és telepítési információk, beleértve a telepítési rendszerkövetelményeket és a részletes útmutatást lásd [a helyszíni StorSimple eszköz üzembe helyezése](storsimple-8000-deployment-walkthrough-u2.md).

> [!IMPORTANT]
> Mielőtt elkezdené, tekintse át a hello [üzembehelyezési konfigurációs ellenőrzőlista](storsimple-8000-deployment-walkthrough-u2.md#deployment-configuration-checklist) és és [telepítésének előfeltételei](storsimple-8000-deployment-walkthrough-u2.md#deployment-prerequisites) a [a helyszíni StorSimple eszköz üzembe helyezése](storsimple-8000-deployment-walkthrough-u2.md).
> <br>
> 
> 

### <a name="before-you-install-storsimple-snapshot-manager"></a>StorSimple Snapshot Manager telepítése előtt
1. Csomagolja ki, csatlakoztathatják és hello Microsoft Azure StorSimple eszköz csatlakoztatása a [a StorSimple 8100 eszköz telepítése](storsimple-8100-hardware-installation.md) vagy [a StorSimple 8600 eszköz telepítése](storsimple-8600-hardware-installation.md).
2. Győződjön meg arról, hogy a gazdaszámítógép hello a következő operációs rendszerek egyikét futtatja:
   
   * Windows Server 2008 R2 (a kiszolgálók Windows 2008 R2 fut, telepítenie kell a Windows Server 2008 SP1 és a Windows Management Framework 3.0)
   * Windows Server 2012
   * Windows Server 2012 R2
     
     A StorSimple virtuális eszköz hello állomás egy Microsoft Azure virtuális gépen kell lennie.
3. Győződjön meg arról, hogy teljesülnek-e hello Microsoft Azure StorSimple konfigurációs követelményeinek. További információkért lépjen túl[telepítésének előfeltételei](storsimple-8000-deployment-walkthrough-u2.md#deployment-prerequisites).
4. Hello eszköz toohello gazdagép csatlakoztatása és hello kezdeti konfigurálása. További információkért lépjen túl[az üzembe helyezés lépései](storsimple-8000-deployment-walkthrough-u2.md#deployment-steps).
5. Hozzon létre köteteket hello eszközön, és rendeljen hozzájuk toohello állomás hello üzemeltető csatlakoztathatja és használatuk ellenőrzése. StorSimple Snapshot Manager támogatja a következő kötetek típusú hello:
   
   * Alapszintű kötetek
   * Egyszerű kötetek
   * A dinamikus kötetek
   * Tükrözött dinamikus kötetek (RAID 1)
   * Fürt megosztott kötetei
     
     A kötetek hello StorSimple eszközön vagy a StorSimple virtuális eszköz létrehozásával kapcsolatos információkért lásd az túl[6. lépés: kötet létrehozása](storsimple-8000-deployment-walkthrough-u2.md#step-6-create-a-volume), a [a helyszíni StorSimple eszköz üzembe helyezése](storsimple-8000-deployment-walkthrough-u2.md).

## <a name="install-a-new-storsimple-snapshot-manager"></a>Egy új StorSimple Snapshot Manager telepítése
StorSimple Snapshot Manager telepítése előtt győződjön meg arról, hogy hello StorSimple eszközön vagy a StorSimple virtuális eszköz létrehozott hello kötetek csatlakoztatva, inicializálva, és a formázott [Előfeltételek konfigurálása](#configure-prerequisites).

> [!IMPORTANT]
> * A StorSimple virtuális eszköz hello állomás egy Microsoft Azure virtuális gépen kell lennie. 
> * hello állomás Windows 2008 R2, Windows Server 2012 vagy Windows Server 2012 R2 futnia kell. Ha a Windows Server 2008 R2 rendszer fut, telepítenie kell a Windows Server 2008 SP1 és a Windows Management Framework 3.0.
> * Hello eszköz tooStorSimple Snapshot Manager csatlakozás előtt be kell állítania egy iSCSI-kapcsolat hello állomás toohello StorSimple eszközön.

Kövesse ezeket a lépéseket toocomplete a StorSimple Snapshot Manager friss telepítését. Ha telepít egy frissítést, nyissa meg túl[frissítéséhez, vagy telepítse újra a StorSimple Snapshot Manager](#upgrade-or-reinstall-storsimple-snapshot-manager).

* 1. lépés: A StorSimple Snapshot Manager telepítése 
* 2. lépés: Csatlakozás a StorSimple Snapshot Manager tooa eszköz 
* 3. lépés: Hello kapcsolat toohello eszköz ellenőrzése 

### <a name="step-1-install-storsimple-snapshot-manager"></a>1. lépés: A StorSimple Snapshot Manager telepítése
A következő lépéseket tooinstall StorSimple Snapshot Manager hello használata.

#### <a name="tooinstall-storsimple-snapshot-manager"></a>StorSimple Snapshot Manager tooinstall
1. Hello StorSimple Snapshot Manager szoftver letöltése (Ugrás túl[StorSimple Snapshot Manager](https://www.microsoft.com/download/details.aspx?id=44220) a Microsoft Download Center hello), és mentse helyileg a hello gazdagépen.
2. A Fájlkezelőben kattintson a jobb gombbal a hello tömörített mappába, és kattintson **az összes kibontása**.
3. A hello **bontsa ki a tömörített (Zipped) mappák** ablak hello **válassza ki a célhelyet, és bontsa ki a fájlt** mezőbe írja be vagy keresse meg a kibontott toofile toobe helyének toohello elérési útja.
   
    > [!IMPORTANT]
    > Telepítenie kell a StorSimple Snapshot Manager hello C: meghajtó.
    
4. Jelölje be hello **megjelenítése kibontott fájlokat, amikor befejeződött** jelölőnégyzetet, majd kattintson a **kibontása**.
   
    ![Bontsa ki a fájlok párbeszédpanel](./media/storsimple-snapshot-manager-deployment/HCS_SSM_extract_files.png) 
5. Hello kibontási befejezésekor hello célmappa nyílik meg. Kattintson duplán hello alkalmazást telepítő megjelenő ikonra hello célmappában.
6. Ha hello **beállítása sikeres** üzenet jelenik meg, kattintson a **Bezárás**. Meg kell jelennie hello StorSimple Snapshot Manager ikonra az asztalon.
   
    ![asztali ikon](./media/storsimple-snapshot-manager-deployment/HCS_SSM_desktop_icon.png) 

### <a name="step-2-connect-storsimple-snapshot-manager-tooa-device"></a>2. lépés: Csatlakozás a StorSimple Snapshot Manager tooa eszköz
A következő lépéseket tooconnect StorSimple Snapshot Manager tooa StorSimple eszköz hello használata.

#### <a name="tooconnect-storsimple-snapshot-manager-tooa-device"></a>StorSimple Snapshot Manager tooa eszköz tooconnect
1. Kattintson a hello StorSimple Snapshot Manager ikonra az asztalon. hello StorSimple Snapshot Manager ablak. hello ablak tartalmaz egy **hatókör** ablaktáblán egy **eredmények** ablaktáblán, és egy **műveletek** ablaktáblán. 
   
    ![StorSimple Snapshot Manager felhasználói felülete](./media/storsimple-snapshot-manager-deployment/HCS_SSM_gui_panes.png)
   
   * Hello **hatókör** ablaktáblán (bal oldali ablaktábla hello) vannak rendszerezve fastruktúrában csomópontok listáját tartalmazza. Egyes csomópontok tooselect nézet vagy adott adatok kapcsolódó toothat csomópont kibontásával. Kattintson a hello nyíl ikon tooexpand, vagy a csomópont összecsukása. Kattintson a jobb gombbal a hello elemének **hatókör** ablaktábla toosee, a cikk elérhető műveletek listáját.
   * Hello **eredmények** (hello középső ablaktábla) panelen tartalmaz részletes állapotadatokat hello csomópont, a nézet vagy a kiválasztott hello adatok **hatókör** ablaktáblán.
   * Hello **műveletek** ablaktábla listázza a hello műveleteket hajthat végre hello csomópontra, a nézet vagy a hello kiválasztott adatok **hatókör** ablaktáblán.
     
     Hello StorSimple Snapshot Manager felhasználói felület teljes leírását lásd: [StorSimple Snapshot Manager felhasználói felület](storsimple-use-snapshot-manager.md).
2. A hello **hatókör** ablaktáblában kattintson a jobb gombbal hello **eszközök** csomópontra, majd **eszköz konfigurálása**. Hello **eszköz konfigurálása** párbeszédpanel jelenik meg.
   
    ![Egy eszköz konfigurálása](./media/storsimple-snapshot-manager-deployment/HCS_SSM_config_device.png) 
3. A hello **eszköz** jelölését, jelölje be hello IP-címét vagy hello Microsoft Azure StorSimple virtuális eszköz listában. A hello **jelszó** szövegmezőben, hello eszköz hello Azure-portálon létrehozott hello StorSimple Snapshot Manager jelszavát. Kattintson az **OK** gombra.
4. StorSimple Snapshot Manager hello eszköz azonosított keres. Hello eszköz érhető el, ha a StorSimple Snapshot Manager hozzáadja a kapcsolat. Is [ellenőrizze hello kapcsolat toohello eszközt](#to-verify-the-connection) tooconfirm, amely hello kapcsolat felvétele sikeresen megtörtént.
   
    Ha bármilyen okból hello eszköz nem érhető el, a StorSimple Snapshot Manager hibaüzenetet ad vissza. Kattintson a **OK** tooclose hello hibaüzenet, és kattintson a **Mégse** tooclose hello **eszköz konfigurálása** párbeszédpanel megnyitásához.
5. Tooa eszköz kapcsolódik, a StorSimple Snapshot Manager importálja minden kötet csoport van konfigurálva. az eszköznek, feltéve, hogy hello kötet csoporthoz társított biztonsági másolatok. Kötet csoportok, amelyek nem rendelkeznek társított biztonsági másolatok nincsenek importálva. Emellett a kötet csoport létrehozott biztonsági mentési házirendek nincsenek importálva. toosee hello importált csoportokat, kattintson a jobb gombbal a hello childwindow- **kötet csoportok** hello csomópontja **hatókör** ablaktáblán, majd kattintson **importált csoportok átváltása**.

### <a name="step-3-verify-hello-connection-toohello-device"></a>3. lépés: Hello kapcsolat toohello eszköz ellenőrzése
A következő lépéseket tooverify, hogy a StorSimple Snapshot Manager csatlakoztatott toohello StorSimple-eszköz hello használata.

#### <a name="tooverify-hello-connection"></a>tooverify hello kapcsolat
1. A hello **hatókör** ablaktáblában kattintson hello **eszközök** csomópont.
   
    ![StorSimple Snapshot Manager eszköz állapota](./media/storsimple-snapshot-manager-deployment/HCS_SSM_Device_status.png) 
2. Ellenőrizze a hello **eredmények** panelen: 
   
   * Ha hello eszköz ikonja megjelenik egy zöld jelző és **elérhető** hello megjelenik **állapot** , majd hello eszköz csatlakoztatva van. 
   * Ha egy piros kijelző látható a hello eszköz ikonra, és nem érhető el megjelenik hello **állapot** oszlopban, majd hello eszköz nincs csatlakoztatva. 
   * Ha **frissítés** hello megjelenik **állapot** oszlopban, majd a StorSimple Snapshot Manager beolvassa a kötet csoportok és a csatlakoztatott eszközhöz társított biztonsági másolatok.

## <a name="upgrade-or-reinstall-storsimple-snapshot-manager"></a>Frissítés, vagy telepítse újra a StorSimple Snapshot Manager
Távolítsa el a StorSimple Snapshot Manager teljesen frissíteni vagy újratelepíteni a szoftvert hello előtt. 

StorSimple Snapshot Manager újratelepítése, előtt adatbázis biztonsági mentése hello meglévő StorSimple Snapshot Manager hello állomáson. Ez menti a hello biztonsági mentési házirendek és a konfigurációs adatokat, így a visszaállíthatja az adatokat a biztonsági másolatból.

Ha frissíteni vagy újratelepíteni a StorSimple Snapshot Manager kövesse az alábbi lépéseket:

* 1. lépés: Távolítsa el a StorSimple Snapshot Manager 
* 2. lépés: Hello StorSimple Snapshot Manager-adatbázis biztonsági mentése 
* 3. lépés: Telepítse újra a StorSimple Snapshot Manager, és hello adatbázis visszaállítása 

### <a name="step-1-uninstall-storsimple-snapshot-manager"></a>1. lépés: Távolítsa el a StorSimple Snapshot Manager
A következő lépéseket toouninstall StorSimple Snapshot Manager hello használata.

#### <a name="toouninstall-storsimple-snapshot-manager"></a>StorSimple Snapshot Manager toouninstall
1. Hello állomás számítógépen nyissa meg a hello **Vezérlőpult**, kattintson a **programok**, és kattintson a **programok és szolgáltatások**.
2. Hello bal oldali ablaktáblában kattintson **eltávolítása vagy módosítása egy program**.
3. Kattintson a jobb gombbal **StorSimple Snapshot Manager**, és kattintson a **Eltávolítás**.
4. Ekkor elindul a hello StorSimple Snapshot Manager telepítő programját. Kattintson a **módosítása telepítő**, és kattintson a **Eltávolítás**.
   
   > [!NOTE]
   > Ha nincsenek hello háttérben futó MMC folyamatok, például a StorSimple Snapshot Manager vagy a Lemezkezelés hello eltávolításhoz és kap, egy üzenet tooclose MMC előtt az összes példányát toouninstall hello program történt kísérlet. Válassza ki **automatikusan alkalmazások bezárása, és próbálja meg azokat a telepítés után végezze el toorestart**, és kattintson a **OK**.
   > 
   > 
5. Hello eltávolításakor folyamat befejeződött, egy **beállítása sikeres** üzenet jelenik meg. Kattintson a **Bezárás** gombra.

### <a name="step-2-back-up-hello-storsimple-snapshot-manager-database"></a>2. lépés: Hello StorSimple Snapshot Manager-adatbázis biztonsági mentése
A következő lépéseket toocreate hello használja, és hello StorSimple Snapshot Manager adatbázis másolatának mentése.

#### <a name="tooback-up-hello-database"></a>hello adatbázis tooback
1. Állítsa le a Microsoft a StorSimple szolgáltatás hello:
   
   1. Indítsa el a Kiszolgálókezelőt.
   2. A Kiszolgálókezelő irányítópulton hello a hello **eszközök** menü **szolgáltatások**.
   3. A hello **szolgáltatások** lapon jelölje be **Microsoft StorSimple szolgáltatás**.
   4. Hello a jobb oldali ablaktáblán, a **Microsoft StorSimple szolgáltatás**, kattintson a **hello szolgáltatás leállítása**.
      
        ![Hello StorSimple Device Manager szolgáltatás leállítása](./media/storsimple-snapshot-manager-deployment/HCS_SSM_stop_service.png)
2. Keresse meg a tooC:\ProgramData\Microsoft\StorSimple\BACatalog. 
   
   > [!NOTE]
   > ProgramData mappa rejtett.
  
3. Egy biztonságos helyre, vagy hello felhőben hello katalógus XML-fájl, hello fájl másolása és tároló hello példány keresése.
   
    ![A StorSimple biztonsági mentési katalógusfájl](./media/storsimple-snapshot-manager-deployment/HCS_SSM_bacatalog.png)
4. Indítsa újra a Microsoft a StorSimple szolgáltatás hello: 
   
   1. A Kiszolgálókezelő irányítópulton hello a hello **eszközök** menü **szolgáltatások**.
   2. A hello **szolgáltatások** lapra, jelölje be hello **Microsoft StorSimple felügyeleti szolgáltatás**e.
   3. Hello a jobb oldali ablaktáblán, a **Microsoft StorSimple szolgáltatás**, kattintson a **hello szolgáltatás újraindítása**. 

### <a name="step-3-reinstall-storsimple-snapshot-manager-and-restore-hello-database"></a>3. lépés: Telepítse újra a StorSimple Snapshot Manager, és hello adatbázis visszaállítása
StorSimple Snapshot Manager tooreinstall kövesse hello [telepítése egy új StorSimple Snapshot Manager](#install-a-new-storsimple-snapshot-manager). Ezután használja a következő eljárás toorestore hello StorSimple Snapshot Manager adatbázis hello.

#### <a name="toorestore-hello-database"></a>toorestore hello adatbázis
1. Állítsa le a Microsoft a StorSimple szolgáltatás hello:
   
   1. Indítsa el a Kiszolgálókezelőt.
   2. A Kiszolgálókezelő irányítópulton hello a hello **eszközök** menü **szolgáltatások**.
   3. A hello **szolgáltatások** lapon jelölje be **Microsoft StorSimple szolgáltatás**.
   4. Hello a jobb oldali ablaktáblán, a **Microsoft StorSimple szolgáltatás**, kattintson a **hello szolgáltatás leállítása**.
2. Keresse meg a tooC:\ProgramData\Microsoft\StorSimple\BACatalog.
   
   > [!NOTE]
   > ProgramData mappa rejtett.
   > 
   > 
3. Törölje a hello katalógus XML-fájlt, és cserélje le a korábban mentett hello verziója.
4. Indítsa újra a Microsoft a StorSimple szolgáltatás hello: 
   
   1. A Kiszolgálókezelő irányítópulton hello a hello **eszközök** menü **szolgáltatások**.
   2. A hello **szolgáltatások** lapon jelölje be **Microsoft StorSimple szolgáltatás**.
   3. Hello a jobb oldali ablaktáblán, a **Microsoft StorSimple szolgáltatás**, kattintson a **hello szolgáltatás újraindítása**.

## <a name="next-steps"></a>Következő lépések
* További információ a StorSimple Snapshot Manager, toolearn lépjen túl[Mi az StorSimple Snapshot Manager?](storsimple-what-is-snapshot-manager.md).
* További információ a StorSimple Snapshot Manager felhasználói felületén hello toolearn lépjen túl[StorSimple Snapshot Manager felhasználói felület](storsimple-use-snapshot-manager.md).
* StorSimple Snapshot Manager használatáról további toolearn lépjen túl[használja a StorSimple Snapshot Manager tooadminister a StorSimple megoldásban](storsimple-snapshot-manager-admin.md).

