---
title: "aaaChange jelszavak keresztül StorSimple Device Manager |} Microsoft Docs"
description: "Ismerteti, hogyan toouse hello StorSimple Manager szolgáltatás toochange a StorSimple Snapshot Manager és az eszköz rendszergazdai jelszót."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: f178509c-f4e1-48a8-90b2-d4ad050eeb30
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: b2836eb4d3a05e1d2a5eeeeefe66c75f63ba38ed
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-toochange-your-storsimple-passwords"></a>Hello StorSimple Manager szolgáltatás toochange a StorSimple-jelszavak használata
## <a name="overview"></a>Áttekintés
a klasszikus Azure portálon hello **konfigurálása** lap tartalmaz olyan módon konfigurálhatja újra a StorSimple eszközön a StorSimple Manager szolgáltatás által kezelt összes hello eszköz paramétert. Ez az oktatóanyag azt ismerteti, hogyan használhatja a hello **konfigurálása** toochange lapon, az eszköz-rendszergazdai vagy a StorSimple Snapshot Manager jelszavát.

## <a name="change-hello-device-administrator-password"></a>Változás hello eszköz rendszergazdai jelszava
Windows PowerShell felületet tooaccess hello StorSimple-eszköz használata esetén szükség tooenter egy eszköz rendszergazdai jelszava. Amikor hello első StorSimple eszköz regisztrálva van a szolgáltatásban, hello alapértelmezett Ez az interfész jelszava *jelszó1*. Hello biztonsági adatait, áll szükséges toochange ezt a jelszót hello regisztrációs folyamat hello végét. Ez a jelszó módosítása nélkül kilép nem hello regisztrációs folyamat során. További információkért lásd: [3. lépés: eszköz konfigurálása és regisztrálása hello a Windows PowerShell segítségével a StorSimple](storsimple-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

hello jelszó, amelyet először hello Windows PowerShell felületen regisztrálás során majd hello a klasszikus Azure portálon keresztül lehet módosítani. Hajtsa végre a következő lépéseket toochange hello eszköz rendszergazdai jelszava hello.

#### <a name="toochange-hello-device-administrator-password"></a>toochange hello eszköz rendszergazdai jelszava
1. Hello klasszikus portálon kattintson **eszközök** > **konfigurálása** az eszközhöz.
2. Görgessen lefelé toohello **eszköz rendszergazdai jelszavát** szakasz. Adjon meg egy rendszergazdai jelszót, amely a 8 too15 karaktereket tartalmaz. hello jelszó 3 vagy több nagybetű, nagybetűk, numerikus és speciális karakterek kombinációjából kell lennie.
3. Hello jelszót.
4. Kattintson a **mentése** hello lap hello alján.

hello eszköz rendszergazdai jelszava most frissíteni kell. A módosított jelszó tooaccess hello Windows PowerShell-felületet is használhatja.

## <a name="change-hello-storsimple-snapshot-manager-password"></a>Hello StorSimple Snapshot Manager jelszavának módosítása
StorSimple Snapshot Manager szoftver a Windows-gazdagépen helyezkedik el, és lehetővé teszi, hogy a rendszergazdák a StorSimple eszköz hello formában a helyi és felhőbeli pillanatképeket toomanage biztonsági másolatait.

Amikor konfigurál egy eszközt a StorSimple Snapshot Managerben, kérni fogja tooprovide hello eszköz IP cím és jelszó tooauthenticate a tárolóeszköz. Ez a jelszó először konfigurálja a hello Windows PowerShell felületen keresztül. További információkért lásd: [3. lépés: eszköz konfigurálása és regisztrálása hello a Windows PowerShell segítségével a StorSimple](storsimple-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

hello jelszó, amelyet először hello Windows PowerShell felületen regisztrálás során majd hello klasszikus portálon keresztül lehet módosítani. Hajtsa végre a következő lépéseket toochange hello StorSimple Snapshot Manager jelszava hello.

#### <a name="toochange-hello-storsimple-snapshot-manager-password"></a>toochange hello StorSimple Snapshot Manager jelszava
1. Hello klasszikus portálon kattintson **eszközök** > **konfigurálása** az eszközhöz.
2. Görgessen lefelé toohello **StorSimple Snapshot Manager** szakasz. Adjon meg egy 14 vagy 15 karakter hosszú jelszót. Győződjön meg arról, hogy hello jelszó 3 vagy több nagybetű, nagybetűk, numerikus és speciális karaktert tartalmaz.
3. Hello jelszót.
4. Kattintson a **mentése** hello lap hello alján.

hello StorSimple Snapshot Manager jelszava most frissíteni kell.

## <a name="next-steps"></a>Következő lépések
* További információ [StorSimple biztonsági](storsimple-security.md).
* További információ [az eszköz konfigurációjának módosítása](storsimple-modify-device-config.md).
* További információ [használatával hello StorSimple Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-manager-service-administration.md).

