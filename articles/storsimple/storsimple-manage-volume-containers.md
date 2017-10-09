---
title: "aaaManage a StorSimple kötettárolók |} Microsoft Docs"
description: "Ismerteti, hogyan használhatja a StorSimple Manager szolgáltatás kötettárolók tooadd lapon, módosítana vagy törölne egy kötettárolót hello."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: 1c64ce75-1fd3-4d3b-9304-d4dc0fc2b069
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/24/2016
ms.author: v-sharos
ms.openlocfilehash: 9b29536e0072306e53ac92bacca78a13d932c2b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-toomanage-storsimple-volume-containers"></a>Hello StorSimple Manager szolgáltatás toomanage StorSimple kötet tárolók használata
## <a name="overview"></a>Áttekintés
Ez az oktatóanyag azt ismerteti, hogyan toouse StorSimple Manager szolgáltatás toocreate hello és a StorSimple-kötet tárolók kezelése.

A Microsoft Azure StorSimple eszköz a kötettároló tárfiók, a titkosítás és a sávszélesség-használat beállításokat használó egy vagy több kötetet tartalmazza. Egy eszköz több kötet tároló az összes kötet lehet. 

A kötettároló hello a következő attribútumokat:

* **Kötetek** – hello rétegzett, vagy helyileg rögzített belüli hello kötettároló StorSimple-köteteket. A kötettároló tartalmazhat be too256 StorSimple-köteteket.
* **Titkosítási** – titkosítási kulcsot, amelyek az egyes mennyiségi tároló. Ezt a kulcsot a StorSimple eszköz toohello felhőből küldött hello adatok titkosításához használt. Egy katonai szintű AES-256 bit kulccsal hello a felhasználó által megadott kulccsal. toosecure adatait, javasoljuk, hogy mindig engedélyezze felhőalapú tárolás titkosításának.
* **A tárfiók** – hello tárfiókja, amely csatolt tooyour felhőalapú tárolási szolgáltató. A kötettároló szereplő összes hello kötetek ossza meg ezt a tárfiókot. A storage-fiók közül választhat egy meglévő listájához, vagy hozzon létre egy új fiókot, ha hello kötettároló létrehozása, és adja meg a fiókhoz tartozó hello hozzáférési hitelesítő adatok.
* **A felhő sávszélesség** – hello amikor hello adatokat hello eszközről toohello felhő küldi hello eszköz által felhasznált sávszélesség. 1 és 1000 MB/s ha ez a tároló közötti érték megadásával kényszerítheti a sávszélesség-vezérlést. Hello eszköz tooconsume a teljes elérhető sávszélesség, állítsa a mező tooUnlimited. Hozhat létre, és a sávszélesség sablon tooallocate sávszélesség ütemezés alapján alkalmazza.

![Kötet tárolók lap](./media/storsimple-manage-volume-containers/HCS_VolumeContainersPage.png)

A következő eljárások azt ismertetik, hogyan toouse hello StorSimple **kötettárolók** lap toocomplete hello közös műveletek a következő:

* A kötettároló hozzáadása 
* A kötettároló módosítása 
* A kötettároló törlése 

## <a name="add-a-volume-container"></a>A kötettároló hozzáadása
Hajtsa végre a következő lépéseket tooadd kötettároló hello.

[!INCLUDE [storsimple-add-volume-container](../../includes/storsimple-add-volume-container.md)]

## <a name="modify-a-volume-container"></a>A kötettároló módosítása
Hajtsa végre a következő lépéseket toomodify kötettároló hello.

[!INCLUDE [storsimple-modify-volume-container](../../includes/storsimple-modify-volume-container.md)]

## <a name="delete-a-volume-container"></a>A kötettároló törlése
A kötettároló kötetek belül. Csak akkor, ha először törlődnek a benne tárolt összes hello kötetek törölhetők. Hajtsa végre a következő lépéseket toodelete kötettároló hello.

[!INCLUDE [storsimple-delete-volume-container](../../includes/storsimple-delete-volume-container.md)]

## <a name="next-steps"></a>Következő lépések
* További információ [felügyelete a StorSimple-köteteket](storsimple-manage-volumes.md). 
* További információ [használatával hello StorSimple Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-manager-service-administration.md).

