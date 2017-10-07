---
title: "aaaManage a StorSimple kötettárolók hello StorSimple 8000 series eszközön |} Microsoft Docs"
description: "Ismerteti, hogyan használhatja a hello StorSimple Device Manager szolgáltatás kötettárolók tooadd lapon, módosítana vagy törölne egy kötettárolót."
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
ms.date: 07/19/2017
ms.author: alkohli
ms.openlocfilehash: 7374d4ab9aecd6280ae1d93a29f17d12d28c9362
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-toomanage-storsimple-volume-containers"></a>Hello StorSimple Device Manager szolgáltatás toomanage StorSimple kötet tárolók használata

## <a name="overview"></a>Áttekintés
Ez az oktatóanyag azt ismerteti, hogyan toouse StorSimple Device Manager szolgáltatás toocreate hello és a StorSimple-kötet tárolók kezelése.

A Microsoft Azure StorSimple eszköz a kötettároló tárfiók, a titkosítás és a sávszélesség-használat beállításokat használó egy vagy több kötetet tartalmazza. Egy eszköz több kötet tároló az összes kötet lehet. 

A kötettároló hello a következő attribútumokat:

* **Kötetek** – hello rétegzett, vagy helyileg rögzített belüli hello kötettároló StorSimple-köteteket. 
* **Titkosítási** – titkosítási kulcsot, amelyek az egyes mennyiségi tároló. Ezt a kulcsot a StorSimple eszköz toohello felhőből küldött hello adatok titkosításához használt. Egy katonai szintű AES-256 bit kulccsal hello a felhasználó által megadott kulccsal. toosecure adatait, javasoljuk, hogy mindig engedélyezze felhőalapú tárolás titkosításának.
* **A tárfiók** – hello használt toostore hello adatok Azure storage-fiók. A kötettároló szereplő összes hello kötetek ossza meg ezt a tárfiókot. A storage-fiók közül választhat egy meglévő listájához, vagy hozzon létre egy új fiókot, ha hello kötettároló létrehozása, és adja meg a fiókhoz tartozó hello hozzáférési hitelesítő adatok.
* **A felhő sávszélesség** – hello amikor hello adatokat hello eszközről toohello felhő küldi hello eszköz által felhasznált sávszélesség. Ez a tároló létrehozásakor 1 és 1000 közötti érték megadásával kényszerítheti a sávszélesség-vezérlést. Hello eszköz tooconsume a teljes elérhető sávszélesség, állítsa ezt a mezőt túl**korlátlan**. Hozhat létre, és a sávszélesség sablon tooallocate sávszélesség ütemezés alapján alkalmazza.

hello alábbi eljárások azt ismertetik, hogyan toouse hello StorSimple **kötettárolók** panel toocomplete hello közös műveletek a következő:

* A kötettároló hozzáadása
* A kötettároló módosítása
* A kötettároló törlése

## <a name="add-a-volume-container"></a>A kötettároló hozzáadása
Hajtsa végre a következő lépéseket tooadd kötettároló hello.

[!INCLUDE [storsimple-8000-add-volume-container](../../includes/storsimple-8000-create-volume-container.md)]

## <a name="modify-a-volume-container"></a>A kötettároló módosítása
Hajtsa végre a következő lépéseket toomodify kötettároló hello.

[!INCLUDE [storsimple-8000-modify-volume-container](../../includes/storsimple-8000-modify-volume-container.md)]

## <a name="delete-a-volume-container"></a>A kötettároló törlése
A kötettároló kötetek belül. Csak akkor, ha először törlődnek a benne tárolt összes hello kötetek törölhetők. Hajtsa végre a következő lépéseket toodelete kötettároló hello.

[!INCLUDE [storsimple-8000-delete-volume-container](../../includes/storsimple-8000-delete-volume-container.md)]

## <a name="next-steps"></a>Következő lépések
* További információ [felügyelete a StorSimple-köteteket](storsimple-8000-manage-volumes-u2.md). 
* További információ [használatával hello StorSimple Device Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-8000-manager-service-administration.md).

