---
title: "aaaView és a StorSimple 8000 Series feladatok kezelése |} Microsoft Docs"
description: "Hello StorSimple Device Manager szolgáltatás feladatok panelen ismerteti, hogyan toouse azt tootrack újabb, aktuális és ütemezett biztonsági mentési feladatok."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/29/2017
ms.author: alkohli
ms.openlocfilehash: a76acd67e2568478dd43d0fb16c1ae2dfb72a23d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-tooview-and-manage-jobs-update-3-and-later"></a>Hello StorSimple Device Manager szolgáltatás tooview használatát és kezelését a feladatok (Update 3 és újabb verziók)

## <a name="overview"></a>Áttekintés
Hello **feladatok** panel egy egyetlen központi portált biztosít a megtekintése, és a feladatok az eszközökön az elindított kezelése csatlakoztatott tooyour StorSimple Device Manager szolgáltatás. Több eszköz ütemezett, futó, befejezett, törölt és sikertelen feladatok tekintheti meg. Eredmények táblázatos formában jelennek meg.

![Feladatok panelen](./media/storsimple-8000-manage-jobs-u2/jobs1.png)

Gyorsan megtalálhatja olyan mezőkön, például a szűréssel érdeklődik hello feladatok:

* **Állapot** – feladatok lehet folyamatban, sikeresen befejeződött, megszakított, sikertelen, zároló vagy sikeres, a hibák.
* **Időtartománynak** – feladatok alapján szűrt hello dátum és idő tartomány lehet. hello tartomány: 1 óra, az elmúlt 24 óra múlva elmúlt 7 nap elmúlt 30 nap, az elmúlt év, vagy egyéni dátuma múltbeli.
* **Típus** – hello feladattípus lehet ütemezett biztonsági mentést, a manuális biztonsági mentés, a biztonsági mentés visszaállítási, kötet klónozása, kötettárolók feladatátvételt, helyileg rögzített kötet létrehozása, módosítása kötet, telepítse a frissítéseket, támogatási gyűjtését és felhő készülék létrehozása.
* **Eszközök** – feladatok kezdeményezett egy bizonyos eszköz csatlakoztatásakor tooyour szolgáltatásra.
  
hello szűrt feladatok majd megjelennének a következő attribútumok hello hello alapján:
  
* **Név** – ütemezett biztonsági mentést, manuális biztonsági mentés, visszaállítás biztonsági mentés, klónozás köteten, a feladatátvétel kötettárolók, helyileg rögzített kötet létrehozása, módosítása a kötet, telepítse a frissítéseket, támogatási gyűjtését, vagy hozzon létre felhő készülék.
* **Állapot** – futó, befejeződött, megszakított, sikertelen, zároló vagy befejeződött, hibákkal.
* **Entitás** – hello feladatok is társítható egy köteten, hogy a biztonsági mentési házirend, vagy eszköz. Például a Klónozás feladat társítva egy köteten, mivel a biztonsági mentési házirend ütemezett biztonsági mentési feladat tartozik. Eszköz feladat jön létre a vészhelyreállítás (DR) vagy a visszaállítási művelet miatt.
* **Eszköz** – hello mely hello feladat el lett indítva hello eszköz nevét.
* **Megkezdődött a** – hello idő, amikor hello feladat el lett indítva.
* **Időtartam** – hello idő szükséges toocomplete hello feladat.

feladatok hello listája 30 másodpercenként frissül.

Ezen a lapon a feladathoz kapcsolódó műveletek következő hello végezheti el:

* Feladatok részleteinek megjelenítése
* Feladatok megszakítása

## <a name="view-job-details"></a>Feladatok részleteinek megjelenítése
Hajtsa végre a következő lépéseket tooview hello adatok bármely feladat hello.

#### <a name="tooview-job-details"></a>tooview feladat részletei
1. Nyissa meg tooyour StorSimple Device Manager szolgáltatást, és kattintson a **feladatok**.

2. A hello **feladatok** panelen, a megfelelő szűrőket és egy lekérdezés futtatásával érdeklődik megjelenítési hello feladat. Kereshet befejezése után fut, és nem vonható vissza a feladatokat.

    ![Feladat panelen](./media/storsimple-8000-manage-jobs-u2/jobs1.png)

2. Válassza ki, majd kattintson egy feladatra.

    ![Feladat panelen](./media/storsimple-8000-manage-jobs-u2/jobs3.png)

3. Hello feladat részleteit megjelenítő panelen megtekintheti hello állapota, részleteit, idő statisztikák és adatok statisztika.
   
    ![Feladat részletei](./media/storsimple-8000-manage-jobs-u2/jobs4.png)

## <a name="cancel-a-job"></a>Feladatok megszakítása
Hajtsa végre a következő lépéseket toocancel egy futó feladat hello.

> [!NOTE]
> Nem lehet megszakítani a más feladatok, például egy kötet toochange hello kötettípus módosítása vagy a kötet kiterjesztését.


### <a name="toocancel-a-job"></a>egy feladat toocancel
1. A hello **feladatok** lapján hello futó feladat, amelyet az toocancel megfelelő szűrőket és egy lekérdezés futtatásával megjelenítéséhez. Jelölje ki a hello feladatot.

2. Hello kijelölt feladat tooinvoke hello helyi menüben kattintson a jobb gombbal, és kattintson a **Mégse**.

    ![Feladat részletei](./media/storsimple-8000-manage-jobs-u2/jobs2.png)

3. Ha a rendszer megerősítést kér, kattintson az **Igen** gombra. Ez a feladat most megszakítva.

## <a name="next-steps"></a>Következő lépések
* Ismerje meg, hogyan túl[a StorSimple biztonsági mentési házirendek kezelése](storsimple-8000-manage-backup-policies-u2.md).
* Ismerje meg, hogyan túl[használata hello StorSimple Device Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-8000-manager-service-administration.md).

