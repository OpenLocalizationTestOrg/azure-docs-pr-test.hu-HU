---
title: "aaaManage a StorSimple biztonsági mentési házirendek |} Microsoft Docs"
description: "Azt ismerteti, hogyan használhatja a hello StorSimple Manager szolgáltatás toocreate, és manuális biztonsági mentések, a biztonsági mentési ütemezés és a biztonsági másolatok megőrzésének kezelése."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: d1834bc8-d520-4463-82ae-3b32fabd021c
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/10/2016
ms.author: v-sharos
ms.openlocfilehash: 710cbe54d14031b4de43e9da292ed169085d5af9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-toomanage-backup-policies"></a>Hello StorSimple Manager szolgáltatás toomanage biztonsági mentési házirendek használata
[!INCLUDE [storsimple-version-selector-manage-backup-policies](../../includes/storsimple-version-selector-manage-backup-policies.md)]

## <a name="overview"></a>Áttekintés
Ez az oktatóanyag azt ismerteti, hogyan toouse hello StorSimple Manager szolgáltatás **biztonsági mentési házirendek** toocontrol biztonsági mentési folyamatokat és a StorSimple-köteteket a biztonsági másolatok megőrzésének lapon. Azt is ismerteti, hogyan toocomplete manuális biztonsági mentés.

Hello **biztonsági mentési házirendek** lap lehetővé teszi a biztonsági mentési házirendek toomanage és ütemezés helyi és felhőbeli pillanatképeket. (A biztonsági mentési házirendek nincsenek használt tooconfigure biztonsági mentési ütemezés és a biztonsági másolatok megőrzésének kötetek gyűjteményét.) Biztonsági mentési házirendek lehetővé teszik több kötet pillanatképe tootake egyidejűleg. Ez azt jelenti, hogy hello hozta létre a biztonsági mentési házirend lesznek összeomlás-konzisztens másolja. Ezen a lapon hello biztonsági mentési házirendek, típusok, hello társított kötetek, megőrzött biztonsági mentések hello számát sorolja fel, és hello beállítás tooenable ezeket a házirendeket.

Hello **biztonsági mentési házirendek** lap lehetővé teszi toofilter hello meglévő biztonsági mentési házirendek közül egy vagy több mezőt a következő hello:

* **Házirend neve** – hello hello házirendhez társított név. hello különböző típusú házirendek a következők:
  
  * Ütemezett házirendek, amelyeket a rendszer explicit módon hello felhasználó által létrehozott.
  * Az automatikus házirendek, amelyek jönnek létre, ha a kötet beállítás hello alapértelmezett biztonsági mentés a kötetek létrehozását hello időpontjában volt engedélyezve. Ezek a házirendek, ahol kötetneve hivatkozik toohello neve a klasszikus Azure portálon hello hello felhasználó által beállított StorSimple-kötet hello VolumeName_Default neve. hello automatikus házirendek 22:30 eszköz időpontban verziótól napi felhőalapú pillanatfelvételek eredményez.
  * A StorSimple Snapshot Manager hello eredetileg létrehozott házirendek importálása. Ezek rendelkezik hello StorSimple Snapshot Manager állomás hello házirendek az importált címkéjét.
* **Kötetek** – hello hello házirendhez társított kötetek. A biztonsági mentési házirend társított összes hello kötet egy csoportba kerülnek, biztonsági mentések létrehozásakor.
* **Utolsó sikeres biztonsági mentés** – hello dátum meghatározott időpontjakor a hello utolsó sikeres készült biztonsági másolat az ehhez a szabályzathoz.
* **Következő biztonsági mentés** – hello dátum meghatározott időpontjakor hello következő ütemezett biztonsági mentés, hogy ez a házirend indul.
* **Ütemezések** – hello hello biztonsági mentési házirend kapcsolódó száma.

Ezen a lapon elvégezhető a gyakran használt hello műveletek a következők:

* Biztonsági mentési szabályzat hozzáadása 
* Adja hozzá vagy ütemezésének módosítása 
* A biztonsági mentési házirend törlése 
* Manuális biztonsági mentés készítése 
* Hozzon létre egy egyéni biztonsági mentési házirend több kötetek és ütemezése 

## <a name="add-a-backup-policy"></a>Biztonsági mentési szabályzat hozzáadása
Adja hozzá a biztonsági mentési házirend tooautomatically ütemezés a biztonsági másolatokat. Hajtsa végre a következő lépéseket az Azure klasszikus portál tooadd a biztonsági mentési házirend beállítása a StorSimple eszközhöz hello hello. Hello házirend hozzáadása után megadhat egy ütemezést (lásd: [hozzáadása vagy ütemezésének módosítása](#add-or-modify-a-schedule)).

[!INCLUDE [storsimple-add-backup-policy](../../includes/storsimple-add-backup-policy.md)]

![Videó elérhető](./media/storsimple-manage-backup-policies/Video_icon.png)**Videó elérhető**

a videó bemutatja, hogyan toocreate egy helyi vagy felhőalapú biztonsági másolatot készíteni a házirend, toowatch kattintson [Itt](https://azure.microsoft.com/documentation/videos/create-storsimple-backup-policies/).

## <a name="add-or-modify-a-schedule"></a>Adja hozzá vagy ütemezésének módosítása
Adja hozzá, vagy a StorSimple eszköz biztonsági mentési házirend meglévő csatolt tooan ütemezésének módosítása. Hajtsa végre a következő lépéseket az Azure klasszikus portál tooadd hello hello, vagy módosítsa a ütemezését.

[!INCLUDE [storsimple-add-modify-backup-schedule](../../includes/storsimple-add-modify-backup-schedule.md)]

## <a name="delete-a-backup-policy"></a>A biztonsági mentési házirend törlése
Hajtsa végre a StorSimple eszköz hello Azure klasszikus portál toodelete a biztonsági mentési házirend lépések hello.

[!INCLUDE [storsimple-delete-backup-policy](../../includes/storsimple-delete-backup-policy.md)]

## <a name="take-a-manual-backup"></a>Manuális biztonsági mentés készítése
Hajtsa végre a hello lépései (hello Azure klasszikus portál toocreate igény szerinti manuális) biztonsági mentés egyetlen kötetén.

[!INCLUDE [storsimple-create-manual-backup](../../includes/storsimple-create-manual-backup.md)]

## <a name="create-a-custom-backup-policy-with-multiple-volumes-and-schedules"></a>Hozzon létre egy egyéni biztonsági mentési házirend több kötetek és ütemezése
Hajtsa végre a következő lépéseket az Azure klasszikus portál toocreate egy egyéni biztonsági mentési házirend, amelyen több kötetek és ütemezések hello hello.

[!INCLUDE [storsimple-create-custom-backup-policy](../../includes/storsimple-create-custom-backup-policy.md)]

## <a name="next-steps"></a>Következő lépések
További információ [használatával hello StorSimple Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-manager-service-administration.md).

