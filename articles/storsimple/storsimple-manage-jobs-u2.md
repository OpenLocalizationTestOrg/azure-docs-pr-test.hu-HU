---
title: "aaaView és a StorSimple-feladatok kezelése |} Microsoft Docs"
description: "Hello StorSimple Manager szolgáltatás feladatok lapján ismerteti, hogyan toouse azt tootrack újabb, aktuális és ütemezett biztonsági mentési feladatok."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: cdd3e9e8-7ccd-40a3-8c07-0ab1338c0e59
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: d6ecdcbc3d8a4757c2328303f268e51c8ce26b65
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-tooview-and-manage-storsimple-jobs-update-2"></a>Hello StorSimple Manager szolgáltatás tooview használatát és kezelését a StorSimple feladatok (2. frissítés)
[!INCLUDE [storsimple-version-selector-manage-jobs](../../includes/storsimple-version-selector-manage-jobs.md)]

## <a name="overview"></a>Áttekintés
Hello **feladatok** lap egy egyetlen központi portált biztosít a megtekintése, és a feladatok az eszközökön az elindított kezelése csatlakoztatott tooyour StorSimple Manager szolgáltatás. Több eszköz ütemezett, futó, befejezett, törölt és sikertelen feladatok tekintheti meg. Eredmények táblázatos formában jelennek meg. 

![Feladatok lap](./media/storsimple-manage-jobs-u2/jobs.png)

Gyorsan megtalálhatja olyan mezőkön, például a szűréssel érdeklődik hello feladatok:

* **Állapot** – futtathat feladatokat, befejeződött, megszakított, sikertelen, zároló vagy befejeződött, hibákkal.
* **A kezdő és a** – feladatok alapján szűrt hello dátum és idő tartomány lehet.
* **Típus** – hello feladattípus lehet biztonsági másolat, manuális biztonsági mentés, a visszaállítási, a klónozás, a eszköz feladatátvevő helyileg rögzített kötet létrehozásához, módosításához a kötetet, frissítés, támogatja a csomagot, vagy a virtuális eszköz kiépítése.
* **Eszközök** – feladatok kezdeményezett egy bizonyos eszköz csatlakoztatásakor tooyour szolgáltatásra.
  hello szűrt feladatok majd megjelennének a következő attribútumok hello hello alapján:
  
  * **Típus** – biztonsági másolat, manuális biztonsági mentés, a visszaállítási, a klónozás, a eszköz feladatátvevő helyileg rögzített kötet létrehozásához, módosításához a kötetet, frissítés, támogatja a csomagot, vagy a virtuális eszköz kiépítése.
  * **Állapot** – futó, befejeződött, megszakított, sikertelen, zároló vagy befejeződött, hibákkal.
  * **Entitás** – hello feladatok is társítható egy köteten, hogy a biztonsági mentési házirend, vagy eszköz. Például a Klónozás feladat társítva egy köteten, mivel a biztonsági mentési házirend ütemezett biztonsági mentési feladat tartozik. Eszköz feladat jön létre a vészhelyreállítás (DR) vagy a visszaállítási művelet miatt.
  * **Eszköz** – hello mely hello feladat el lett indítva hello eszköz nevét.
  * **Megkezdődött a** – hello idő, amikor hello feladat el lett indítva.
  * **Folyamatban lévő** – hello százalékos egy futó feladat befejezésére. A projekt mindig legyen 100 %.

feladatok hello listája 30 másodpercenként frissül.

Ezen a lapon a feladathoz kapcsolódó műveletek következő hello végezheti el:

* Feladatok részleteinek megjelenítése
* Feladatok megszakítása

## <a name="view-job-details"></a>Feladatok részleteinek megjelenítése
Hajtsa végre a következő lépéseket tooview hello adatok bármely feladat hello.

#### <a name="tooview-job-details"></a>tooview feladat részletei
1. A hello **feladatok** lapon, a megfelelő szűrőket és egy lekérdezés futtatásával érdeklődik hello feladat megjelenítése. Kereshet befejezése után fut, és nem vonható vissza a feladatokat.
2. Jelölje ki a feladatot.
3. Hello a hello lap alján, kattintson **részletek**.
4. A hello **biztonsági mentési feladat részleteit** párbeszédpanelen hello állapota, részleteit, idő statisztikák és adatok statisztika megtekintheti.
   
    ![Feladat részletei lap](./media/storsimple-manage-jobs-u2/JobDetails.png)

## <a name="cancel-a-job"></a>Feladatok megszakítása
Hajtsa végre a következő lépéseket toocancel egy futó feladat hello.

> [!NOTE]
> Nem lehet megszakítani a más feladatok, például egy kötet toochange hello kötettípus módosítása vagy a kötet kiterjesztését.
> 
> 

### <a name="toocancel-a-job"></a>egy feladat toocancel
1. A hello **feladatok** lapján hello futó feladat, amelyet az toocancel megfelelő szűrőket és egy lekérdezés futtatásával megjelenítéséhez.
2. Jelölje ki a hello feladatot.
3. Hello a hello lap alján, kattintson **Mégse**.
4. Ha a rendszer megerősítést kér, kattintson az **Igen** gombra.

Ez a feladat most megszakítva.

## <a name="next-steps"></a>Következő lépések
* Ismerje meg, hogyan túl[a StorSimple biztonsági mentési házirendek kezelése](storsimple-manage-backup-policies.md).
* Ismerje meg, hogyan túl[használata hello StorSimple Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-manager-service-administration.md).

