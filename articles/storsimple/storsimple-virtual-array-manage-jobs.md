---
title: "aaaView és a StorSimple virtuális tömb feladatok kezelése |} Microsoft Docs"
description: "Hello StorSimple Device Manager szolgáltatás feladatok lapján ismerteti, hogyan toouse azt tootrack legutóbbi és a jelenlegi feladataihoz hello StorSimple virtuális tömb."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 31879821-b599-4609-a7f4-d4b0f658a933
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 11/11/2016
ms.author: alkohli
ms.openlocfilehash: cf3f3e7bcdfff0ff2328b7354db2482286800e93
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-tooview-jobs-for-hello-storsimple-virtual-array"></a>A StorSimple virtuális tömb hello hello StorSimple Device Manager szolgáltatás tooview feladatok használata
## <a name="overview"></a>Áttekintés
Hello **feladatok** panel megtekintéséhez és kezeléséhez, amelyek csatlakoztatott tooyour StorSimple Device Manager szolgáltatás a virtuális tömbök indított feladatok egyetlen központi portált biztosít. Több virtuális eszközön futó, befejezett, és a sikertelen feladatok tekintheti meg. Eredmények táblázatos formában jelennek meg.

![Feladatok panelen](./media/storsimple-virtual-array-manage-jobs/ova-jobs-blade.png)

Gyorsan megtalálhatja olyan mezőkön, például a szűréssel érdeklődik hello feladatok:

* **Időtartománynak** – feladatok alapján szűrt hello dátum és idő tartomány lehet.
* **Eszközök** – feladatok kezdeményezett egy csatlakoztatott eszközre tooyour szolgáltatásra. hello szűrt feladatok majd megjelennének hello a következő attribútumok alapján:
  
  * **Név** – hello lehet **összes**, **biztonsági mentés**, **Klónozás**, **feladatátvételt**, **frissítések letöltése** , vagy **frissítések telepítése**.
  * **Állapot** – feladatok lehetnek **összes**, **folyamatban**, **sikeres**, vagy **sikertelen**, vagy **visszavonva**.
  * **Entitás** – hello feladatok társíthatók a kötet, megosztás, vagy eszköz.
  * **Eszköz** – hello mely hello feladat el lett indítva hello eszköz nevét.
  * **Megkezdődött a** – hello idő, amikor hello feladat el lett indítva.
  * **Időtartam** – hello időtartama mely hello feladat volt futtatva.
* **Állapot** – minden, futó, hajtható végre, vagy a sikertelen feladatok is kereshet.
* **Típusú feladattal** – hello feladat típusa is all, biztonsági mentési, helyreállítási, feladatátvevő, töltse le a frissítéseket, vagy telepítse a frissítéseket.

feladatok hello listája 30 másodpercenként frissül.

## <a name="view-job-details"></a>Feladatok részleteinek megjelenítése
Hajtsa végre a következő lépéseket tooview hello adatok bármely feladat hello.

#### <a name="tooview-job-details"></a>tooview feladat részletei
1. A hello **feladatok** panelen, a megfelelő szűrőket és egy lekérdezés futtatásával érdeklődik megjelenítési hello feladat. Befejezett vagy nem futnak feladatok is kereshet.
2. Válassza ki a feladatot a feladatok hello táblázatos listáról.
   
    ![Feladat panelen](./media/storsimple-virtual-array-manage-jobs/ova-jobs-blade.png)
3. Hello a hello lap alján, kattintson **részletek**.
4. A hello **részletek** párbeszédpanelen megtekintheti állapotát, a részletek és a vonatkozó adatok. hello alábbi ábrán egy példa hello **biztonsági mentési feladat részleteit** párbeszédpanel megnyitásához.
   
    ![Feladat részletei](./media/storsimple-virtual-array-manage-jobs/ova-jobs-details.png)

#### <a name="job-failures-when-hello-virtual-machine-is-paused-in-hello-hypervisor"></a>Amikor hello virtuális gép fel van függesztve, a hello hipervizor a sikertelen feladatok
Ha egy feladat a folyamatban van a a StorSimple virtuális tömb és hello eszközök (a virtuális gép kiépítése a hipervizor) fel van függesztve, a nagyobb, mint 15 percig, hello feladat sikertelen lesz. Ez miatt tooyour StorSimple virtuális tömb idő alatt szinkronban hello Microsoft Azure idő. 

Látni fogja a következő hiba hello: "az eszköz ideje szinkronban hello Microsoft Azure idő több mint 15 perces. Győződjön meg arról, hogy hello hipervizor és hello eszköz idő szinkronizálva van egy NTP ügyfélkérelmet. Győződjön meg arról, hogy nincsenek-e kapcsolódási problémák. tootroubleshoot kapcsolódási problémák, futtassa a diagnosztikai tesztek hello helyi webes felhasználói Felületét a virtuális eszköz."

Ezek a hibák toobackup, a visszaállítás, a frissítés és a feladatátvételi feladatok alkalmazni. Ha a Hyper-v-ben a virtuális gép üzembe helyezése hello gép idejét végül szinkronizálja a hipervizor. Ebben az esetben, ha a feladat indíthatja el.

## <a name="next-steps"></a>Következő lépések
[Ismerje meg, hogyan toouse hello helyi webes felhasználói felület tooadminister a StorSimple virtuális tömb](storsimple-ova-web-ui-admin.md).

