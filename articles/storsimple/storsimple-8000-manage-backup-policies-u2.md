---
title: "aaaManage a StorSimple 8000 series biztonsági mentési házirendek |} Microsoft Docs"
description: "Azt ismerteti, hogyan használhatja a hello StorSimple Device Manager szolgáltatás toocreate, és manuális biztonsági mentések, biztonsági mentési ütemezés és a StorSimple 8000 series eszközön biztonsági másolatok megőrzésének kezelése."
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
ms.date: 07/05/2017
ms.author: alkohli
ms.openlocfilehash: 7c56365abb6ba69d02008829ca6ae703d4632705
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-in-azure-portal-toomanage-backup-policies"></a>Hello StorSimple Device Manager szolgáltatás használata az Azure portál toomanage biztonsági mentési házirendek


## <a name="overview"></a>Áttekintés

Ez az oktatóanyag azt ismerteti, hogyan toouse hello StorSimple Device Manager szolgáltatás **biztonsági mentési házirend** panel toocontrol biztonsági mentési folyamatokat és a StorSimple-köteteket a biztonsági másolatok megőrzésének. Azt is ismerteti, hogyan toocomplete manuális biztonsági mentés.

Biztonsági mentését egy köteten, dönthet úgy toocreate egy helyi pillanatfelvétel vagy egy felhőalapú pillanatfelvétel. Ha biztonsági mentést egy helyileg rögzített kötet, javasoljuk, meg kell adnia egy felhőalapú pillanatfelvétel. Helyi pillanatképek készítése, amely rendelkezik a nagy mennyiségű forgalom adatkészlet alapján kialakulhat egy helyileg rögzített kötet nagy mennyiségű véve azt eredményezi, hogy olyan helyzet, amelyben gyorsan futtathatja helyi elfogyott. Ha tootake helyi pillanatképeket, azt javasoljuk, hogy meg kevesebb napi pillanatképek tooback eltarthat, mire hello legutóbbi állapota, tartsa meg őket egy napon, és törölje őket.

Amikor a pillanatképet egy felhőalapú, egy helyileg rögzített kötetet, csak megváltozott hello adatok toohello felhő, amennyiben deduplikált és tömörített másolja át.

## <a name="hello-backup-policy-blade"></a>hello biztonsági mentési házirend panel

Hello **biztonsági mentési házirend** a StorSimple eszköz paneljén lehetővé teszi a biztonsági mentési házirendek toomanage és ütemezés helyi és felhőbeli pillanatképeket. Biztonsági mentési házirendek nincsenek használt tooconfigure biztonsági mentési ütemezés és a biztonsági másolatok megőrzésének kötetek gyűjteményét. Biztonsági mentési házirendek lehetővé teszik több kötet pillanatképe tootake egyidejűleg. Ez azt jelenti, hogy hello hozta létre a biztonsági mentési házirend lesznek összeomlás-konzisztens másolja.

hello biztonsági mentési házirendek táblázatos felsorolása azt is lehetővé teszi, hogy az akkor toofilter hello meglévő biztonsági mentési házirendek egy vagy több mezőt a következő hello:

* **Házirend neve** – hello hello házirendhez társított név. hello különböző típusú házirendek a következők:

  * Ütemezett házirendek, amelyeket a rendszer explicit módon hello felhasználó által létrehozott.
  * A StorSimple Snapshot Manager hello eredetileg létrehozott házirendek importálása. Ezek rendelkezik hello StorSimple Snapshot Manager állomás hello házirendek az importált címkéjét.

  > [!NOTE]
  > Automatikus vagy alapértelmezett biztonsági mentési házirendek már nincs engedélyezve a kötetek létrehozását hello időpontjában.

* **Utolsó sikeres biztonsági mentés** – hello dátum meghatározott időpontjakor a hello utolsó sikeres készült biztonsági másolat az ehhez a szabályzathoz.

* **Következő biztonsági mentés** – hello dátum meghatározott időpontjakor hello következő ütemezett biztonsági mentés, hogy ez a házirend indul.

* **Kötetek** – hello hello házirendhez társított kötetek. A biztonsági mentési házirend társított összes hello kötet egy csoportba kerülnek, biztonsági mentések létrehozásakor.

* **Ütemezések** – hello hello biztonsági mentési házirend kapcsolódó száma.

a biztonsági mentési házirendek hajthat végre a gyakran használt hello műveletek a következők:

* Biztonsági mentési szabályzat hozzáadása
* Adja hozzá vagy ütemezésének módosítása
* Hozzáadni vagy eltávolítani egy kötet
* A biztonsági mentési házirend törlése
* Manuális biztonsági mentés készítése

## <a name="add-a-backup-policy"></a>Biztonsági mentési szabályzat hozzáadása

Adja hozzá a biztonsági mentési házirend tooautomatically ütemezés a biztonsági másolatokat. Először hozzon létre egy kötetet, ha nincs a kötethez társított alapértelmezett biztonsági mentési házirend. Tooadd kell, és rendelje hozzá a biztonsági mentési házirend tooprotect kötet adatait.

Hajtsa végre a következő lépéseket az Azure portál tooadd a biztonsági mentési házirend beállítása a StorSimple eszközhöz hello hello. Hello házirend hozzáadása után megadhat egy ütemezést (lásd: [hozzáadása vagy ütemezésének módosítása](#add-or-modify-a-schedule)).

[!INCLUDE [storsimple-8000-add-backup-policy-u2](../../includes/storsimple-8000-add-backup-policy-u2.md)]

## <a name="add-or-modify-a-schedule"></a>Adja hozzá vagy ütemezésének módosítása

Adja hozzá, vagy a StorSimple eszköz biztonsági mentési házirend meglévő csatolt tooan ütemezésének módosítása. Hajtsa végre a következő lépéseket az Azure portál tooadd hello hello, vagy módosítsa a ütemezését.

[!INCLUDE [storsimple-8000-add-modify-backup-schedule](../../includes/storsimple-8000-add-modify-backup-schedule-u2.md)]


## <a name="add-or-remove-a-volume"></a>Hozzáadni vagy eltávolítani egy kötet

Adja hozzá, vagy távolítsa el a hozzárendelt biztonsági mentési házirend tooa a StorSimple eszköz kötetet. Hajtsa végre a következő lépéseket az Azure portál tooadd hello hello, vagy távolítsa el a kötetet.

[!INCLUDE [storsimple-8000-add-volume-backup-policy-u2](../../includes/storsimple-8000-add-remove-volume-backup-policy-u2.md)]


## <a name="delete-a-backup-policy"></a>A biztonsági mentési házirend törlése

Hajtsa végre a StorSimple eszköz hello Azure portál toodelete a biztonsági mentési házirend lépések hello.

[!INCLUDE [storsimple-8000-delete-backup-policy](../../includes/storsimple-8000-delete-backup-policy.md)]

## <a name="take-a-manual-backup"></a>Manuális biztonsági mentés készítése

Hajtsa végre a hello lépései (hello Azure portál toocreate igény szerinti manuális) biztonsági mentés egyetlen kötetén.

[!INCLUDE [storsimple-8000-create-manual-backup](../../includes/storsimple-8000-create-manual-backup.md)]

## <a name="next-steps"></a>Következő lépések

További információ [használatával hello StorSimple Device Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-8000-manager-service-administration.md).

