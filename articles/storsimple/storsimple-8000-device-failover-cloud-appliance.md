---
title: "aaaStorSimple feladatátvétel, vagy vész-helyreállítási tooa StorSimple felhő készülék |} Microsoft Docs"
description: "Ismerje meg, hogyan toofail keresztül a StorSimple 8000 series fizikai eszköz tooa felhőalapú készüléket."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/03/2017
ms.author: alkohli
ms.openlocfilehash: e8a0bca057024358e3a557fe85a42ddefea36cff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="fail-over-tooyour-storsimple-cloud-appliance"></a>Feladatok átadása tooyour StorSimple felhő készülék

## <a name="overview"></a>Áttekintés

Ez az oktatóanyag leírja a StorSimple 8000 series fizikai eszköz tooa StorSimple felhő készülék keresztül hello lépéseket szükséges toofail, ha van egy olyan vészhelyzet esetén. StorSimple hello datacenter tooa felhő készülék Azure-beli hello eszköz feladatátvételi szolgáltatás toomigrate adatait a forrás fizikai eszközről használja. Ebben az oktatóanyagban hello útmutatást tooStorSimple 8000 sorozat fizikai eszközöket és a felhő készülékek 3 és újabb verzióit futtató a szoftver verziója frissítés vonatkozik.

További információ az eszköz feladatátvételi, és hogyan egy olyan vészhelyzet esetén, a használt toorecover toolearn lépjen túl[feladatátvétel és a katasztrófa utáni helyreállítás a StorSimple 8000 sorozat eszközeire](storsimple-8000-device-failover-disaster-recovery.md).

túl nyissa meg a StorSimple fizikai eszköz tooanother fizikai eszközön keresztül toofail[tooa StorSimple fizikai eszköz feladatátvételt](storsimple-8000-device-failover-physical-device.md). egy eszköz tooitself keresztül toofail lépjen túl[feladatátvételt toohello azonos StorSimple fizikai eszköz](storsimple-8000-device-failover-same-device.md).

## <a name="prerequisites"></a>Előfeltételek

- Győződjön meg arról, hogy átolvasta eszköz feladatátvételi hello szempontjai. További információ: túl[eszköz feladatátvételi a gyakori szempontok](storsimple-8000-device-failover-disaster-recovery.md).

- A StorSimple felhő készülék létrehozása és konfigurálása a művelet futtatása előtt kell rendelkeznie. Ha a futó 3 ügyfélszoftverének verziójára frissítése, vagy később, érdemes lehet a 8020-as modellt felhő készülék hello vész-Helyreállítási. hello 8020 modell 64 TB-os rendelkezik, és a prémium szintű Storage használja. További információ: túl[központi telepítése és kezelése a StorSimple felhő készülék](storsimple-8000-cloud-appliance-u2.md).

## <a name="steps-toofail-over-tooa-cloud-appliance"></a>Lépéseket toofail tooa felhő készülék keresztül

Hajtsa végre a következő lépéseket toorestore hello eszköz tooa cél StorSimple felhő készülék hello.

1.  Győződjön meg arról, hogy hello kötettároló toofail kívánt felhőalapú pillanatfelvételek vannak társítva. További információ: túl[StorSimple az Eszközkezelő szolgáltatás toocreate biztonsági mentések](storsimple-8000-manage-backup-policies-u2.md).
2. Nyissa meg tooyour StorSimple Device Manager szolgáltatás és **eszközök**. A hello **eszközök** panelen, a szolgáltatáshoz kapcsolódó eszközök lépjen toohello listája.
    ![Válassza ki az eszköz](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev1.png)
3. Válassza ki, és kattintson a forrás-eszközre. hello Forráseszköz hello kötettárolók kívánt toofail keresztül. Nyissa meg túl**beállítások > Kötettárolók**.

    ![Válassza ki az eszköz](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev2.png)
    
4. Válassza ki, hogy milyen toofail tooanother eszközön keresztül kötettároló. Kattintson a hello kötet tároló toodisplay hello kötetek listáját a tárolóban. Válassza ki a kötetet, kattintson a jobb gombbal, majd kattintson **Offline állapotba** tootake hello kötet offline állapotba.

    ![Válassza ki az eszköz](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev5.png)

5. Ismételje meg ezt a folyamatot a hello kötettároló összes hello kötetet.

     ![Válassza ki az eszköz](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev7.png)

6. Ismétlődő hello előző lépést az összes hello kötettárolók milyen toofail tooanother eszközön keresztül.

7. Lépjen vissza toohello **eszközök** panelen. Hello parancssávon kattintson **feladatátvételt**.

    ![Kattintson a sikertelen keresztül](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev8.png)
8. A hello **feladatátvételt** panelen, hajtsa végre az alábbi lépésekkel hello:
   
    1. Kattintson a **forrás**. Válassza ki a hello kötet tárolók toofail keresztül. **Csak a hozzárendelt felhő pillanatképekkel kötettárolók hello és a kötetek offline állapotban jelennek meg.**
        ![Forrás kiválasztása](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev11.png)
    2. Kattintson a **cél**. Válassza ki a cél felhő készülék az elérhető eszközök hello legördülő listából. **Csak olyan hello eszközök, amelyeken elegendő kapacitása tooaccommodate forrás kötettárolók hello listában jelennek meg.**

        ![Válassza ki a cél](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev12.png)

    3. Tekintse át a hello feladatátvételi beállításokat **összegzés** válassza ki a hello jelölőnégyzet jelzi, hogy a kijelölt kötettárolók hello kötetek offline állapotban-e. 

        ![Tekintse át a feladatátvételi beállításait](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev13.png)

9. A feladatátvételi feladatban jön létre. toomonitor hello feladatátvételi feladat, kattintson a hello feladat értesítés.

    ![A figyelő feladatátvételi feladatban](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev13.png)

10. Hello feladatátvétel elvégzése után térjen vissza toohello **eszközök** panelen.

    1. Válassza ki a hello feladatátvételi hello célként használt hello eszközt.

       ![Válassza ki az eszköz](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev14.png)

    2. Kattintson a **Kötettárolók**. Minden hello kötettárolók, hello kötetek hello régi eszközről, és szerepelnie kell.

       Ha rendelkezik a feladatokat átvenni hello kötettároló helyileg rögzített kötetek, a köteteket feladatátvétel történt rétegzett kötetek. Helyileg rögzített kötetek nem támogatottak a StorSimple-felhő készüléken.

       ![Nézet target kötettárolók](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev17.png)


## <a name="next-steps"></a>Következő lépések

* Miután elvégezte a feladatátvételt, esetleg túl[inaktiválja vagy törölje a StorSimple eszköz](storsimple-8000-deactivate-and-delete-device.md).

* Hogyan toouse hello StorSimple Device Manager információ szolgáltatás, nyissa meg túl[használata hello StorSimple Device Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-8000-manager-service-administration.md).

