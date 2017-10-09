---
title: "Virtuális tömb vész helyreállítási és eszköz feladatátvételi aaaStorSimple |} Microsoft Docs"
description: "További tudnivalók toofailover a StorSimple virtuális tömbjét."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 3c1f9c62-af57-4634-a0d8-435522d969aa
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5f125efd1ffb94489cdfa7cfaafae7d57cc10131
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="disaster-recovery-and-device-failover-for-your-storsimple-virtual-array-via-azure-portal"></a>Katasztrófa utáni helyreállítás és eszköz feladatátvevő a StorSimple virtuális tömb Azure-portálon

## <a name="overview"></a>Áttekintés
Ez a cikk ismerteti hello vész-helyreállítási a Microsoft Azure StorSimple virtuális tömb hello többek között részletes lépéseket toofail tooanother virtuális tömb keresztül. A feladatátvétel toomove lehetővé teszi az adatokat egy *forrás* hello datacenter tooa eszköz *cél* eszköz. hello céleszköz lehet hello található azonos vagy különböző földrajzi elhelyezkedését. hello eszköz feladatátvétel során hello teljes eszközhöz. A feladatátvételi hello felhőbeli adatát hello forráseszközt hello target eszköz tulajdonjogának toothat változik.

Ez a cikk virtuális tömbök alkalmazható tooStorSimple. túl nyissa meg egy 8000 sorozat-eszközön keresztül toofail[eszköz feladatátvételi és katasztrófa-helyreállítás a StorSimple eszköz](storsimple-device-failover-disaster-recovery.md).

## <a name="what-is-disaster-recovery-and-device-failover"></a>Mi az a vész-helyreállítási és eszköz feladatátvevő?

Egy vész-helyreállítási forgatókönyv szerint hello elsődleges eszköz nem működik. Ebben a forgatókönyvben áthelyezheti hello sikertelen eszköz tooanother eszközhöz társított hello felhőbeli adatát. Akkor használható elsődleges eszköz hello hello *forrás* , és adja meg egy másik eszköz hello *cél*. A folyamat be nem hivatkozott tooas hello *feladatátvételi*. A feladatátvételi összes hello kötetek vagy hello megosztások hello forrás eszközről tulajdonjogai módosulnak, és átvitt toohello céleszközön. Nincs hello adatok szűrés engedélyezve van.

DR használatával hello tűz térkép-alapú rétegezéséhez és nyomon követni a teljes visszaállítási van modellezve. Egy hőtérkép hozzárendelése egy tűz érték toohello adatok alapján írási és olvasási minták határozzák meg. A tűz képezze le, majd rétegek hello legalacsonyabb tűz adatok adattömbök toohello felhő először hello helyi réteg hello (használat gyakorisága) magas tűz adattömbök megtartásával. Egy Vészhelyreállítás során a StorSimple hello tűz térkép toorestore használ, és rehydrate hello felhő hello adatait. hello eszköz lekéri az összes hello kötetek vagy megosztások hello utolsó legutóbbi biztonsági mentés (a belső), és a visszaállítás végez, hogy biztonsági másolatból. hello virtuális tömb koordinálja a hello teljes vész-Helyreállítási folyamatot.

> [!IMPORTANT]
> hello forráseszközt hello végén lévő eszköz feladatátvételi törlődik, és ezért a feladat-visszavétel nem támogatott.
> 
> 

Vész-helyreállítási hello eszköz feladatátvételi szolgáltatás keresztül van összehangolva és hello kezdeményezi **eszközök** panelen. Ez a panel összes hello StorSimple eszközök csatlakoztatott tooyour StorSimple Device Manager szolgáltatás vannak. Az egyes eszközök láthatja hello rövid nevét, állapotát, kiépített és a maximális kapacitás, típusa és modell.

## <a name="prerequisites-for-device-failover"></a>Eszköz feladatátvételi előfeltételei

### <a name="prerequisites"></a>Előfeltételek

Egy eszköz feladatátvételi győződjön meg arról, hogy hello a következő előfeltételek teljesülését:

* hello forráseszközt kell a toobe egy **inaktív** állapotát.
* hello céleszköz kell tooshow mentése másként **tooset kész mentése** a hello Azure-portálon. A cél virtuális tömbje hello kiépítése ugyanolyan vagy nagyobb kapacitású. Hello helyi webes felhasználói felület tooconfigure használja, és sikeresen regisztrált hello virtuális céltömb.
  
  > [!IMPORTANT]
  > Ne kísérelje meg tooconfigure hello regisztrált virtuális eszköz hello szolgáltatáson keresztül. Nincs eszközkonfiguráció hello szolgáltatáson keresztül kell elvégezni.
  > 
  > 
* hello figyelt eszköz nem lehet azonos nevet hello forrás eszközként hello.
* hello forrása és célja az eszköz rendelkezik toobe hello ugyanarra a típusra. Csak egy virtuális fájlkiszolgálóként a fájl server tooanother konfigurált tömb feladatátvételt. hello is igaz az iSCSI-kiszolgáló.
* Egy fájlkiszolgáló vész-Helyreállítási, azt javasoljuk, hogy csatlakozik-e hello target eszköz toohello hello forrásaként ugyanabban a tartományban. Ez a konfiguráció biztosítja, hogy hello megosztási engedélyek automatikusan feloldani. Csak hello feladatátvételi tooa céleszköz hello az ugyanabban a tartományban.
* elérhető Céleszközök hello vész-Helyreállítási olyan eszközökre, amelyeken hello azonos vagy nagyobb kapacitás toohello forráseszközt képest. hello csatlakoztatott tooyour eszközök szolgáltatással, de nem felelnek meg a hello feltételek elegendő lemezterület cél eszközök nem érhetők el.

### <a name="other-considerations"></a>Egyéb szempontok

* A tervezett feladatátvételre 
  
  * Azt javasoljuk, hogy minden hello kötetek vagy megosztások érvénybe hello adatforrás-kapcsolat nélküli eszközt.
  * Azt javasoljuk, hogy készítsen biztonsági mentést hello eszköz, és ezután folytathatja a hello feladatátvételi toominimize adatvesztés. 
* Egy nem tervezett feladatátvétel hello eszköz hello legutóbbi biztonsági mentés toorestore hello adatokat használ.

### <a name="device-failover-prechecks"></a>Eszköz feladatátvételi prechecks

Hello vész-Helyreállítási kezdődik, mielőtt hello eszköz prechecks hajt végre. Az ellenőrzések biztosíthatja, hogy nincs hiba fordulhat elő, ha a vész-Helyreállítási megkezdése. hello prechecks a következők:

* Hello tárolási fiók ellenőrzése.
* Hello felhő kapcsolat tooAzure ellenőrzése.
* Hello céleszközön rendelkezésre álló lemezterület ellenőrzése.
* Ellenőrzi, hogy rendelkezik-e az iSCSI server eszköz forráskötet
  
  * érvényes ACR neve.
  * érvényes IQN (legfeljebb 220 karakter).
  * érvényes CHAP jelszavak (12-16 karakter).

Ha bármelyik prechecks megelőző hello meghibásodik, hello vész-Helyreállítási nem folytatódhat. Ezek a problémák megoldásához, majd próbálkozzon újra a vész-Helyreállítási.

Hello vész-Helyreállítási sikeres befejezése után hello tulajdonjogát hello felhőbeli adatát hello forrás eszközön átvitt toohello céleszközön. hello forráseszközt már majd nincs elérhető hello portálon. Hozzáférés tooall hello kötetek vagy megosztások hello forráseszközt le van tiltva, és hello céleszköz aktívvá válik.

> [!IMPORTANT]
> Bár a hello eszköz már nem érhető el, hello gazdarendszer kiépített hello virtuális gép továbbra is fogyassza az erőforrásokat. Ha hello vész-Helyreállítási sikeresen befejeződött, a gazdarendszer törölheti ezt a virtuális gépet.
> 
> 

## <a name="fail-over-tooa-virtual-array"></a>Feladatok átadása tooa virtuális tömb

Javasoljuk, hogy kiépíteni, konfigurálása, és egy másik, a StorSimple virtuális tömb regisztrálása a StorSimple Device Manager szolgáltatásban, mielőtt elkezdi a műveletet.

> [!IMPORTANT]
> 
> * A StorSimple 8000 series eszköz tooa 1200-as virtuális eszköz nem feladatátvételt.
> * Feladatokat átveheti Federal Information Processing Standard (FIPS) engedélyezett virtuális eszköz tooanother FIPS-kompatibilis eszközről vagy tooa a FIPS eszközön hello kormányzati portálon telepítik.


Hajtsa végre a következő lépéseket toorestore hello eszköz tooa cél StorSimple virtuális eszköz hello.

1. Felkészítse és konfigurálja a céleszközön, amely megfelel a hello [eszköz feladatátvételi Előfeltételek](#prerequisites). Végezze el a hello eszköz konfigurálását hello helyi webes felhasználói felületen keresztül, és regisztrálhatja azt tooyour StorSimple Device Manager szolgáltatást. Létrehozásakor egy fájlkiszolgálón, nyissa meg az 1 toostep [fájlkiszolgálóként beállítása](storsimple-virtual-array-deploy3-fs-setup.md#step-1-complete-the-local-web-ui-setup-and-register-your-device). Ha az iSCSI-kiszolgáló létrehozása, nyissa meg az 1 toostep [állítsa be az iSCSI-kiszolgálóként](storsimple-virtual-array-deploy3-iscsi-setup.md#step-1-complete-the-local-web-ui-setup-and-register-your-device).

2. Kötetek vagy megosztások offline végrehajtása a hello állomás. tootake hello kötetek vagy megosztások offline módban vannak, tekintse meg a toohello operációsrendszer-specifikus utasításokat hello állomás. Ha nem már offline állapotú, akkor tootake összes hello kötetek vagy megosztások offline hello eszközön hello következő tevékenységek végrehajtásával.
   
    1. Nyissa meg túl**eszközök** panelt, és válassza ki azt az eszközt.
   
    2. Nyissa meg túl**beállítások > kezelése > megosztások** (vagy **beállítások > kezelés > kötetek**). 
   
    3. Jelöljön ki egy megosztást/kötetet, kattintson a jobb gombbal, és válassza ki **offline állapotba**. 
   
    4. Megerősítést kér, hogy nem **tudomásul veszem, hogy a megosztás offline állapotba helyezése hello hatását.** 
   
    5. Kattintson a **offline állapotba**.

3. A StorSimple eszköz Manager szolgáltatást, lépjen túl**felügyeleti > eszközök**. A hello **eszközök** panelen jelölje ki, majd kattintson a forrás eszköz.

4. Az a **eszköz irányítópult** panelen kattintson a **Deactivate**.

5. A hello **Deactivate** panelen megerősítést kér. Eszköz inaktiválása egy *állandó* folyamat, amely nem vonható vissza. Hogy a megosztások vagy kötetek offline hello gazdagépen emlékezteti a tootake is vannak. Írja be a hello eszköz neve tooconfirm, és kattintson a **Deactivate**.
   
    ![](./media/storsimple-virtual-array-failover-dr/failover1.png)
6. hello deaktiválása kezdődik. Egy értesítést fog kapni, hello deaktiválása sikeres végrehajtása után.
   
    ![](./media/storsimple-virtual-array-failover-dr/failover2.png)
7. Hello eszközök lapján hello eszközállapotba most változik túl**inaktív**.
    ![](./media/storsimple-virtual-array-failover-dr/failover3.png)
8. A hello **eszközök** panel megnyitásához, válassza ki, és kattintson hello deaktivált forráseszközt feladatátvételhez. 
9. A hello **eszköz irányítópult** panelen kattintson a **feladatátvételt**. 
10. A hello **eszköz feladatátvételt** panelen a következő hello:
    
    1. hello forrás eszköz mező automatikusan feltöltődik értékkel. Megjegyzés: a teljes adatméret hello hello forráseszközt. hello adattároló mérete lehet kisebb, mint a rendelkezésre álló kapacitás hello hello céleszközön. Tekintse át a hello részletes adatait, például az eszköz neve, a teljes kapacitás és a feladatátvétel történt hello megosztások hello nevei hello forráseszközt társított.

    2. Az elérhető eszközök hello legördülő listájában válassza a **céleszközön**. Csak az olyan hello eszközökre, amelyeken elegendő kapacitással hello legördülő listában jelennek meg.

    3. Ellenőrizze, hogy **tudomásul veszem, hogy ez a művelet sikertelen lesz-e adatok toohello céleszközön keresztül**. 

    4. Kattintson a **feladatátvételt**.
    
        ![](./media/storsimple-virtual-array-failover-dr/failover4.png)
11. A feladatátvételi feladatban elindul, és értesíti. Nyissa meg túl**eszközök > feladatok** toomonitor hello feladatátvételi.
    
     ![](./media/storsimple-virtual-array-failover-dr/failover5.png)
12. A hello **feladatok** panelen megjelenik a feladatátvételi feladatban hello forrás rendszerhez létrehozott. Ez a feladat hello vész-Helyreállítási prechecks végzi.
    
    ![](./media/storsimple-virtual-array-failover-dr/failover6.png)
    
     Követően hello vész-Helyreállítási prechecks sikeres, a hello feladatátvételi feladatban fogja elindítanak visszaállítási feladat minden megosztás/kötet létezik-e az adatforrás-eszközön.
    
    ![](./media/storsimple-virtual-array-failover-dr/failover7.png)
13. Miután a hello feladatátvétel befejeződött, nyissa meg toohello **eszközök** panelen.
    
    1. Jelölje ki, majd kattintson a feladatátvételi folyamat hello hello cél eszközként használt hello StorSimple-eszköz.
    2. Nyissa meg túl**beállítások > felügyeleti > megosztások** (vagy **kötetek** Ha iSCSI-kiszolgálókkal). A hello **megosztások** panelen megtekintheti az összes hello megosztások (kötetek) hello régi eszközről.
        ![](./media/storsimple-virtual-array-failover-dr/failover9.png)
14. Szüksége lesz túl[DNS-alias létrehozása](https://support.microsoft.com/kb/168322) , hogy az összes létrehozott hello tooconnect átirányított toohello új eszköz kérheti le.

## <a name="errors-during-dr"></a>A Vészhelyreállítás során hibák

**Felhő kapcsolat kimaradás során vész-Helyreállítási**

Ha után hello felhő kapcsolat megszakad a vész-Helyreállítási elindult-e, és hello eszköz visszaállítás befejezése előtt a vész-Helyreállítási hello sikertelen lesz. Failore értesítést kaphat. van megjelölve, a vész-Helyreállítási hello céleszköz *használható.* Nem használhat hello azonos céleszköz jövőbeli DRs-hez.

**Nem kompatibilis a Céleszközök számára**

Ha hello az elérhető célkiszolgálók eszközök nem rendelkeznek elegendő lemezterülettel, láthatja, hogy vannak-e nem kompatibilis a Céleszközök hiba toohello hatása.

**Precheck hibák**

Ha egy hello prechecks nem teljesülnek, majd megjelenik precheck hibák.

## <a name="business-continuity-disaster-recovery-bcdr"></a>Üzleti folytonosság vészhelyreállítási (BCDR)

Üzleti folytonosság (BCDR) vészhelyreállítás hello teljes Azure-adatközpontban leáll a működése következik be. Ez befolyásolhatja a StorSimple Device Manager szolgáltatáshoz, és a hello kapcsolódó StorSimple eszközökhöz.

Ha nincs regisztrált előtt történt egy olyan vészhelyzet esetén, akkor a StorSimple eszköz előfordulhat, hogy törölni toobe StorSimple eszközökhöz. Hello katasztrófa utáni hozza létre újra, és konfigurálja az eszközöket.

## <a name="next-steps"></a>Következő lépések

További tudnivalók túl[felügyelete a StorSimple virtuális tömb hello helyi webes felhasználói felület használatával](storsimple-ova-web-ui-admin.md).

