---
title: "aaaView és a StorSimple-feladatok kezelése |} Microsoft Docs"
description: "Hello StorSimple Manager szolgáltatás feladatok lapján ismerteti, hogyan toouse azt tootrack újabb, aktuális és ütemezett biztonsági mentési feladatok."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 55922cd0-d490-48eb-938a-012a67c1c09e
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: b7341270e37a9f2530a8da1fb7c54f6b699c699c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-tooview-and-manage-storsimple-jobs"></a>Hello StorSimple Manager szolgáltatás tooview használja, és a StorSimple-feladatok kezelése
[!INCLUDE [storsimple-version-selector-manage-jobs](../../includes/storsimple-version-selector-manage-jobs.md)]

## <a name="overview"></a>Áttekintés
Hello **feladatok** lap egy egyetlen központi portált biztosít a megtekintése, és a feladatok az eszközökön az elindított kezelése csatlakoztatott tooyour StorSimple Manager szolgáltatás. Több eszköz ütemezett, futó, befejezett, és a sikertelen feladatok tekintheti meg. Eredmények táblázatos formában jelennek meg. 

![Feladatok lap](./media/storsimple-manage-jobs/HCS_JobsPage.png)

Gyorsan megtalálhatja olyan mezőkön, például a szűréssel érdeklődik hello feladatok:

* **Állapot** – futtathat feladatokat, ütemezett, sikertelen, befejezett, zároló vagy visszavont.
* **Típus** – feladatok is létrehozható egy ütemezett vagy egy igény szerinti biztonsági mentést (**érvénybe biztonsági mentési**), a klónozás, eszköz-visszaállítási vagy frissítési művelet.
* **Eszközök** – feladatok kezdeményezett egy bizonyos eszköz csatlakoztatásakor tooyour szolgáltatásra.
* **A kezdő és a** – feladatok alapján szűrt hello dátum és idő tartomány lehet.

hello szűrt feladatok majd megjelennének a következő attribútumok hello hello alapján:

* **Típus** – biztonsági másolat, klónozás, visszaállítási, feladatátvétel vagy frissítés.
* **Állapot** – futó, ütemezett, sikertelen, befejeződött, megszakítása vagy megszakítva.
* **Entitás** – hello feladatok is társítható egy köteten, hogy a biztonsági mentési házirend, vagy eszköz. A Klónozás feladat tartozik egy köteten, mivel a biztonsági mentési házirend társítva egy ütemezett biztonsági mentési feladat. Eszköz feladat jön létre a vészhelyreállítás (DR) vagy a visszaállítási művelet miatt.
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

## <a name="cancel-a-job"></a>Feladatok megszakítása
Hajtsa végre a következő lépéseket toocancel egy futó feladat hello.

### <a name="toocancel-a-job"></a>egy feladat toocancel
1. A hello **feladatok** lapján hello futó feladat, amelyet az toocancel megfelelő szűrőket és egy lekérdezés futtatásával megjelenítéséhez.
2. Jelölje ki a hello feladatot.
3. Hello a hello lap alján, kattintson **Mégse**.
4. Ha a rendszer megerősítést kér, kattintson az **Igen** gombra.

Ez a feladat most megszakítva.

## <a name="next-steps"></a>Következő lépések
* Ismerje meg, hogyan túl[a StorSimple biztonsági mentési házirendek kezelése](storsimple-manage-backup-policies.md).
* Ismerje meg, hogyan túl[használata hello StorSimple Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-manager-service-administration.md).

