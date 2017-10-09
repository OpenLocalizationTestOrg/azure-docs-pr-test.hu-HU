---
title: aaaChange a StorSimple-jelszavak |} Microsoft Docs
description: "Ismerteti, hogyan toouse hello StorSimple Device Manager szolgáltatás toochange a StorSimple Snapshot Manager és az eszköz rendszergazdai jelszót."
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
ms.date: 07/03/2017
ms.author: alkohli
ms.openlocfilehash: cf884be31b4bbf9e372c0aa11b9da2eadcda35dd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-toochange-your-storsimple-passwords"></a>Hello StorSimple Device Manager szolgáltatás toochange a StorSimple-jelszavak használata

## <a name="overview"></a>Áttekintés
Azure-portálon hello **eszközbeállítások** beállítást tartalmaz olyan módon konfigurálhatja újra a StorSimple eszközön a StorSimple eszköz Manager szolgáltatás által kezelt összes hello eszköz paraméter. Ez az oktatóanyag azt ismerteti, hogyan használhatja a hello **biztonsági** lehetőség alatt **eszközbeállítások** toochange az eszköz-rendszergazdai vagy a StorSimple Snapshot Manager jelszavát.

## <a name="change-hello-device-administrator-password"></a>Változás hello eszköz rendszergazdai jelszava
Windows PowerShell felületet tooaccess hello StorSimple-eszköz használata esetén szükség tooenter egy eszköz rendszergazdai jelszava. Amikor hello első StorSimple eszköz regisztrálva van a szolgáltatásban, hello alapértelmezett Ez az interfész jelszava *jelszó1*. Hello biztonsági adatait, áll szükséges toochange ezt a jelszót hello regisztrációs folyamat hello végét. Ez a jelszó módosítása nélkül kilép nem hello regisztrációs folyamat során. További információkért lásd: [3. lépés: eszköz konfigurálása és regisztrálása hello a Windows PowerShell segítségével a StorSimple](storsimple-8000-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

hello jelszó, amelyet először hello Windows PowerShell felületen regisztrálás során később módosítható hello Azure-portálon keresztül. Hajtsa végre a következő lépéseket toochange hello eszköz rendszergazdai jelszava hello.

#### <a name="toochange-hello-device-administrator-password"></a>toochange hello eszköz rendszergazdai jelszava
1. Nyissa meg tooyour StorSimple Device Manager szolgáltatás és **eszközök**.

2. Eszközök listája táblázatos hello, válassza ki, és kattintson hello eszközre, amelynek a jelszavát szeretné toochange.

    ![](./media/storsimple-8000-change-passwords/changepwd1.png)

3. A hello **beállítások** paneljén lépjen túl**eszközbeállítások > biztonsági**.

    ![](./media/storsimple-8000-change-passwords/changepwd2.png)

4. A hello **biztonsági beállítások** panelen kattintson a **jelszó** toochange hello eszköz rendszergazdai jelszava.

    ![](./media/storsimple-8000-change-passwords/changepwd3.png)

5. A hello **jelszó** panelen adjon meg egy rendszergazdai jelszót, amely a 8 too15 karaktereket tartalmaz. hello jelszó 3 vagy több nagybetű, nagybetűk, numerikus és speciális karakterek kombinációjából kell lennie.

6. Hello jelszót.

    ![](./media/storsimple-8000-change-passwords/changepwd4.png)

7. Kattintson a **mentése** és megerősítést kér, amikor kattintson **Igen**.

    ![](./media/storsimple-8000-change-passwords/changepwd6.png)

hello eszköz rendszergazdai jelszava most frissíteni kell. A módosított jelszó tooaccess hello Windows PowerShell-felületet is használhatja.

## <a name="set-hello-storsimple-snapshot-manager-password"></a>Hello StorSimple Snapshot Manager jelszavának beállítása
StorSimple Snapshot Manager szoftver a Windows-gazdagépen helyezkedik el, és lehetővé teszi, hogy a rendszergazdák a StorSimple eszköz hello formában a helyi és felhőbeli pillanatképeket toomanage biztonsági másolatait.

Amikor konfigurál egy eszközt a StorSimple Snapshot Managerben, kérni fogja tooprovide hello eszköz IP cím és jelszó tooauthenticate a tárolóeszköz.

Állítsa be, vagy módosítsa a hello jelszavát a StorSimple Snapshot Manager hello Azure-portálon keresztül. Hajtsa végre a következő lépéseket tooset hello, vagy módosítsa a hello StorSimple Snapshot Manager jelszavát.

#### <a name="tooset-hello-storsimple-snapshot-manager-password"></a>tooset hello StorSimple Snapshot Manager jelszava
1. Nyissa meg tooyour StorSimple Device Manager szolgáltatás és **eszközök**.

2. Eszközök listája táblázatos hello, válassza ki, majd kattintson a hello eszköz tooset tervezi, vagy módosítsa, amelynek StorSimple Snapshot Manager jelszavát.

     ![](./media/storsimple-8000-change-passwords/changepwd1.png)

3. A hello **beállítások** paneljén lépjen túl**eszközbeállítások > biztonsági**.

     ![](./media/storsimple-8000-change-passwords/changepwd2.png)

4. A hello **biztonsági beállítások** panelen kattintson a **jelszó** tooset, vagy módosítsa a hello StorSimple Snapshot Manager jelszavát.

     ![](./media/storsimple-8000-change-passwords/changepwd3.png) 

5. A hello **jelszó** panelen adjon meg egy 14 vagy 15 karakter hosszú jelszót. Győződjön meg arról, hogy hello jelszó 3 vagy több nagybetű, nagybetűk, numerikus és speciális karaktert tartalmaz.

6. Hello jelszót.

     ![](./media/storsimple-8000-change-passwords/changepwd5.png)

7. Kattintson a **mentése** és megerősítést kér, amikor kattintson **Igen**.

     ![](./media/storsimple-8000-change-passwords/changepwd6.png)

hello StorSimple Snapshot Manager jelszava most frissíteni kell.

## <a name="next-steps"></a>Következő lépések
* További információ [StorSimple biztonsági](storsimple-8000-security.md).
* További információ [az eszköz konfigurációjának módosítása](storsimple-8000-modify-device-config.md).
* További információ [használatával hello StorSimple Device Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-8000-manager-service-administration.md).

