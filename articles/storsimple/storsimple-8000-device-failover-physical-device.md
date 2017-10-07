---
title: "a feladatátvétel aaaStorSimple vész helyreállítási tooa a StorSimple 8000 series fizikai eszköz |} Microsoft Docs"
description: "Megtudhatja, hogyan toofail keresztül a StorSimple 8000 series fizikai eszköz tooanother fizikai eszköz."
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
ms.date: 05/03/2017
ms.author: alkohli
ms.openlocfilehash: 29d2576a96c446ff5ffcd98dcd0f5a07f1ab08ca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="fail-over-tooa-storsimple-8000-series-physical-device"></a>Feladatok átadása tooa a StorSimple 8000 series fizikai eszköz

## <a name="overview"></a>Áttekintés

Ez az oktatóanyag leírja hello lépéseket szükséges toofail keresztül a StorSimple 8000 series fizikai eszköz tooanother StorSimple fizikai eszköz, ha egy olyan vészhelyzet esetén. StorSimple hello eszköz feladatátvételi szolgáltatás toomigrate adatait használja a forrás fizikai eszköz hello datacenter tooanother fizikai eszköz. Ebben az oktatóanyagban hello útmutatást tooStorSimple 8000 sorozat fizikai eszközök 3 és újabb verzióit futtató a szoftver verziója frissítés vonatkozik.

További információ az eszköz feladatátvételi, és hogyan egy olyan vészhelyzet esetén, a használt toorecover toolearn lépjen túl[feladatátvétel és a katasztrófa utáni helyreállítás a StorSimple 8000 sorozat eszközeire](storsimple-8000-device-failover-disaster-recovery.md).

túl nyissa meg a StorSimple fizikai eszköz tooa StorSimple felhő készülék keresztül toofail[tooa StorSimple felhő készülék feladatátvételt](storsimple-8000-device-failover-cloud-appliance.md). túl nyissa meg a fizikai eszköz tooitself keresztül toofail[feladatátvételt toohello azonos StorSimple fizikai eszköz](storsimple-8000-device-failover-same-device.md).


## <a name="prerequisites"></a>Előfeltételek

- Győződjön meg arról, hogy átolvasta eszköz feladatátvételi hello szempontjai. További információ: túl[eszköz feladatátvételi a gyakori szempontok](storsimple-8000-device-failover-disaster-recovery.md).

- A StorSimple 8000 series fizikai eszköz hello adatközpontban telepített kell rendelkeznie. hello eszköz Update 3-as vagy újabb szoftvert kell futtatnia. További információ: túl[a helyszíni StorSimple eszköz üzembe helyezése](storsimple-8000-deployment-walkthrough-u2.md).


## <a name="steps-toofail-over-tooa-physical-device"></a>Lépéseket toofail keresztül tooa fizikai eszköz

Hajtsa végre a következő lépéseket toorestore hello az eszköz tooa cél fizikai eszköz.

1. Győződjön meg arról, hogy hello kötettároló toofail kívánt felhőalapú pillanatfelvételek vannak társítva. További információ: túl[StorSimple az Eszközkezelő szolgáltatás toocreate biztonsági mentések](storsimple-8000-manage-backup-policies-u2.md).
2. Nyissa meg a StorSimple Device Manager tooyour, és kattintson a **eszközök**. A hello **eszközök** panelen, a szolgáltatáshoz kapcsolódó eszközök lépjen toohello listája.
    ![Válassza ki az eszköz](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev1.png)
3. Válassza ki, és kattintson a forrás-eszközre. hello Forráseszköz hello kötettárolók kívánt toofail keresztül. Nyissa meg túl**beállítások > Kötettárolók**.
4. Válassza ki, hogy milyen toofail tooanother eszközön keresztül kötettároló. Kattintson a hello kötet tároló toodisplay hello kötetek listáját a tárolóban. Válassza ki a kötetet, kattintson a jobb gombbal, majd kattintson **Offline állapotba** tootake hello kötet offline állapotba. Ismételje meg ezt a folyamatot a hello kötettároló összes hello kötetet.
5. Ismétlődő hello előző lépést az összes hello kötettárolók milyen toofail tooanother eszközön keresztül.
6. Lépjen vissza toohello **eszközök** panelen. Hello parancssávon kattintson **feladatátvételt**.
    ![Kattintson a sikertelen keresztül](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev2.png)
    
7. A hello **feladatátvételt** panelen, hajtsa végre az alábbi lépésekkel hello:
   
   1. Kattintson a **forrás**. hello kötettárolók felhőalapú pillanatfelvételek társított kötetek jelennek meg. Csak a megjelenített hello tárolók jogosultak a feladatátvételre. Hello kötettárolók, jelölje ki hello kötettárolók milyen toofail keresztül. **Csak a hozzárendelt felhő pillanatképekkel kötettárolók hello és a kötetek offline állapotban jelennek meg.**

       ![Forrás kiválasztása](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev5.png)
   2. Kattintson a **cél**. Hello kötettárolók hello előző lépésben kiválasztott az elérhető eszközök hello legördülő listából válassza a céleszközön. Csak olyan hello eszközök, amelyeken elegendő kapacitása tooaccommodate forrás kötettárolók hello listában jelennek meg.

        ![Válassza ki a cél](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev6.png)

   3. Végül ellenőrizze az összes hello feladatátvételi beállításokat alatt **összegzés**. Hello beállítások áttekintése után válassza ki a hello jelölőnégyzet jelzi, hogy a kijelölt kötettárolók hello kötetek offline állapotban-e. Kattintson az **OK** gombra.

       ![Tekintse át a feladatátvételi beállításait](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev8.png)
  
8. StorSimple létrehoz egy feladatátvételi feladatot. Kattintson a hello feladat értesítési toomonitor hello feladatátvételi feladatban keresztül hello **feladatok** panelen.

    Ha hello keresztül megbukott kötettároló helyi köteteken, majd látható egyedi visszaállítási feladat minden helyi kötet (nem a rétegzett kötetek) hello tárolóban. Ezek a feladatok visszaállítási egy idő toocomplete is igénybe vehet. Valószínű, előfordulhat, hogy korábban végezze el a hello feladatátvételi feladatot. Ezek a kötetek lesz a helyi garanciákat, csak hello visszaállítási feladatok végrehajtását követően.

    ![A figyelő feladatátvételi feladatban](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev13.png)

9. Hello feladatátvétel elvégzése után nyissa meg toohello **eszközök** panelen.
   
   1. Válassza ki a feladatátvételi folyamat hello hello cél eszközként használt hello eszközt.

       ![Válassza ki az eszköz](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev14.png)

   2. Nyissa meg toohello **Kötettárolók** panelen. Minden hello kötettárolók, hello kötetek hello régi eszközről, és szerepelnie kell.

       ![Nézet target kötettárolók](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev16.png)


## <a name="next-steps"></a>Következő lépések

* Miután elvégezte a feladatátvételt, esetleg túl[inaktiválja vagy törölje a StorSimple eszköz](storsimple-8000-deactivate-and-delete-device.md).
* Hogyan toouse hello StorSimple Device Manager információ szolgáltatás, nyissa meg túl[használata hello StorSimple Device Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-8000-manager-service-administration.md).

