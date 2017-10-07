---
title: "a feladatátvétel aaaStorSimple 8000 sorozat eszközeire vész-helyreállítási |} Microsoft Docs"
description: "Ismerje meg, hogy a StorSimple eszköz toohello keresztül toofail ugyanarra az eszközre."
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
ms.date: 06/23/2017
ms.author: alkohli
ms.openlocfilehash: b0b4216c7af6745ff68b85ca3d655691b43b4334
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="fail-over-your-storsimple-physical-device-toosame-device"></a>Feladatok átadása a StorSimple fizikai eszköz toosame eszköz

## <a name="overview"></a>Áttekintés

Ez az oktatóanyag leírja a StorSimple 8000 series fizikai eszköz tooitself keresztül hello lépéseket szükséges toofail, ha egy olyan vészhelyzet esetén. StorSimple hello eszköz feladatátvételi szolgáltatás toomigrate adatait használja a forrás fizikai eszköz hello datacenter tooanother fizikai eszköz. Ebben az oktatóanyagban hello útmutatást tooStorSimple 8000 sorozat fizikai eszközök 3 és újabb verzióit futtató a szoftver verziója frissítés vonatkozik.

További információ az eszköz feladatátvételi, és hogyan egy olyan vészhelyzet esetén, a használt toorecover toolearn lépjen túl[feladatátvétel és a katasztrófa utáni helyreállítás a StorSimple 8000 sorozat eszközeire](storsimple-8000-device-failover-disaster-recovery.md).

egy fizikai eszköz tooanother fizikai eszközön keresztül toofail lépjen túl[feladatátvételt toohello azonos StorSimple fizikai eszköz](storsimple-8000-device-failover-physical-device.md). túl nyissa meg a StorSimple fizikai eszköz tooa StorSimple felhő készülék keresztül toofail[tooa StorSimple felhő készülék feladatátvételt](storsimple-8000-device-failover-cloud-appliance.md).


## <a name="prerequisites"></a>Előfeltételek

- Győződjön meg arról, hogy átolvasta eszköz feladatátvételi hello szempontjai. További információ: túl[eszköz feladatátvételi a gyakori szempontok](storsimple-8000-device-failover-disaster-recovery.md).


## <a name="steps-toofail-over-toohello-same-device"></a>Lépéseket toofail keresztül toohello ugyanarra az eszközre

Hajtsa végre a következő lépéseket, ha toofail toohello keresztül kell hello ugyanarra az eszközre.

1. Az összes hello kötet felhőalapú pillanatfelvételek az eszközt a hálózatról. További információ: túl[StorSimple az Eszközkezelő szolgáltatás toocreate biztonsági mentések](storsimple-8000-manage-backup-policies-u2.md).
2. Az eszköz toofactory Alapértelmezések visszaállítása. Hajtsa végre a hello részletes utasításait [hogyan tooreset a StorSimple eszköz toofactory alapértelmezett beállítások](storsimple-8000-manage-device-controller.md#reset-the-device-to-factory-default-settings).
3. Nyissa meg toohello StorSimple Device Manager szolgáltatást, és válassza ki **eszközök**. A hello **eszközök** panelen jelennek meg hello régi eszköz **Offline**.

    ![Forrás-eszköz kapcsolat nélküli módban](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev2.png)

4. Állítsa be az eszközt, és regisztrálja újra a StorSimple eszköz Manager szolgáltatásban. hello újonnan regisztrált eszközre kell megjeleníteni **tooset kész mentése**. hello eszköznevet hello új eszköz hello ugyanaz, mint a régi eszköz hello azonban kiegészül a számok tooindicate hello eszköz alaphelyzetbe állítása toofactory alapértelmezett volt, és újra regisztrálva.

    ![Újonnan regisztrált eszközre készen tooset mentése](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev3.png)
5. Hello új eszközhöz hello eszköz beállításának befejezése. További információ: túl[4. lépés: minimális eszközbeállítások végrehajtása](storsimple-8000-deployment-walkthrough-u2.md#step-4-complete-minimum-device-setup). A hello **eszközök** panelen hello hello eszköz állapota túl**Online**.

   > [!IMPORTANT]
   > **Hello minimális konfiguráció befejezéséhez először, vagy a vész-Helyreállítási sikertelenek lehetnek.**

    ![Online az újonnan regisztrált eszközre](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev7.png)

6. Válassza ki a régi eszköz hello (kapcsolat nélküli állapota) és hello parancssávon kattintson **feladatátvételt**. A hello **feladatátvételt** panelen válassza ki azt a régi eszközt hello forrásként, és adja meg a hello céleszközt, hello újonnan regisztrált eszköz.

    ![Feladatátvételi összegzése](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev11.png)

    Részletes útmutatásért lásd túl[tooanother fizikai eszköz feladatátvételt](#fail-over-to-another-physical-device).

7. Egy eszköz visszaállítási feladat jön létre, hogy hello a figyelheti **feladatok** panelen.

8. Miután hello feladat sikeresen befejeződött, hello új eszközhöz való hozzáféréshez, és keresse meg a toohello **kötettárolók** panelen. Győződjön meg arról, hogy minden hello kötettárolók hello régi eszközről áttelepített toohello új eszköz.

   ![Kötettárolók áttelepítése](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev13.png)

9. Hello feladatátvételi befejeződése után inaktiválása, és törli a régi eszköz hello hello portálról. Hello régi eszköz (offline), kattintson a jobb gombbal, majd válassza ki és **Deactivate**. Hello eszközt az Inaktiválás után hello eszköz hello állapota frissül.

     ![Forrás eszköz inaktiválása](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev14.png)

10. Jelölje be hello inaktiválása eszközt, kattintson a jobb gombbal, és válassza **törlése**. Hello eszköz törlése hello eszközök listáját.

    ![Forrás eszköz törlése](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev15.png)



## <a name="next-steps"></a>Következő lépések

* Miután elvégezte a feladatátvételt, esetleg túl[inaktiválja vagy törölje a StorSimple eszköz](storsimple-8000-deactivate-and-delete-device.md).
* Hogyan toouse hello StorSimple Device Manager információ szolgáltatás, nyissa meg túl[használata hello StorSimple Device Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-8000-manager-service-administration.md).

