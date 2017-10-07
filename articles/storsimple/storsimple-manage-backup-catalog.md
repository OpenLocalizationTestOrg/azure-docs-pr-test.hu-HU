---
title: "aaaManage a StorSimple biztonsági mentés katalógusa |} Microsoft Docs"
description: "Azt ismerteti, hogyan toouse hello StorSimple Manager szolgáltatás biztonságimásolat-katalógus lap toolist, válassza ki, és egy kötet a biztonságimásolat-készletek törlése."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: ad81bee9-fe43-40b3-a384-b15fb274ecd9
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 04/28/2016
ms.author: v-sharos
ms.openlocfilehash: 14f565c174a10da2c9e2f934a533a5e493f77226
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-toomanage-your-backup-catalog"></a>A biztonságimásolat-katalógus használatáról az hello StorSimple Manager szolgáltatás toomanage
## <a name="overview"></a>Áttekintés
a StorSimple Manager szolgáltatás hello **biztonságimásolat-katalógus** lapon megjelenik, ha manuális vagy ütemezett biztonsági mentést készít a létrehozott összes hello biztonsági mentés. A lap toolist összes hello biztonsági mentés használja a biztonsági mentési házirend, vagy egy kötetet, jelölje be vagy törölje a biztonsági mentések, vagy használja a biztonsági mentési toorestore vagy kötet klónozása.

Ez az oktatóanyag azt ismerteti, hogyan toolist, válassza ki és törölje a biztonságimásolat-készlet. toolearn hogyan toorestore az eszköz biztonsági másolatból, nyissa meg túl[szeretné visszaállítani az eszközt egy biztonságimásolat-készlet](storsimple-restore-from-backup-set.md). Hogyan tooclone egy köteten, nyissa meg túl toolearn[klónozza a StorSimple-kötet](storsimple-clone-volume.md).

![Biztonságimásolat-katalógus](./media/storsimple-manage-backup-catalog/backupcatalog.png) 

Hello **biztonságimásolat-katalógus** lap biztosít egy lekérdezés toonarrow a biztonságimásolat-készlet kiválasztása. Hello biztonságimásolat-készletek, amelyek lekérése, a következő paraméterek hello alapján szűrheti:

* **Eszköz** – hello eszköz mely hello a biztonságimásolat-készlet létrehozása.
* **Biztonsági mentési házirend, vagy a kötet** – hello biztonsági mentési házirend, vagy a kötet a biztonságimásolat-készlet társítva.
* **A kezdő és a** – dátum és idő tartomány hello hello biztonságimásolat-készlet létrehozásakor.

hello szűrt biztonságimásolat-készletek majd megjelennének hello a következő attribútumok alapján:

* **Név** – hello hello biztonsági mentési házirend vagy a kötet társított hello biztonságimásolat-készlet neve.
* **Méret** – hello hello biztonságimásolat-készlet tényleges mérete.
* **Létrehozás** – hello dátum és idő, amikor hello létrehozásuk. 
* **Típus** – biztonsági másolat beállítása helyi pillanatképeket és felhőalapú pillanatfelvételek. Egy helyi pillanatfelvétel a köteten tárolt összes adat helyileg hello eszköz, biztonsági másolatot, mivel egy felhőalapú pillanatfelvétel toohello az adatok biztonsági mentése kötet hello felhőben található. Helyi pillanatképeket gyorsabb hozzáférést biztosít, mivel felhőalapú pillanatfelvételek az adatrugalmasság közül választ.
* **Elindító** – hello biztonsági mentések kezdeményezheti automatikusan egy ütemezés szerint vagy manuálisan egy felhasználó. A biztonsági mentési házirend tooschedule biztonsági mentéseket is használhatja. Másik lehetőségként használhatja a hello **biztonsági másolatok készítéséhez** beállítás tootake manuális biztonsági mentés.

## <a name="list-backup-sets-for-a-volume"></a>Lista biztonságimásolat-készletet kötetre
Hajtsa végre a következő lépéseket toolist hello összes hello biztonsági mentés kötetre.

#### <a name="toolist-backup-sets"></a>toolist biztonságimásolat-készletek
1. A hello StorSimple Manager szolgáltatás lapján, kattintson a hello **biztonságimásolat-katalógus** fülre.
2. Az alábbiak szerint szűrheti hello beállításokat:
   
   1. Válassza ki azt a hello megfelelő eszközt.
   2. Hello legördülő listában válassza a kötet tooview hello megfelelő hello biztonsági mentések.
   3. Adjon meg hello időtartományt.
   4. Kattintson a pipa ikonra hello ![Pipa ikon](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) tooexecute ezt a lekérdezést.
      
      hello biztonsági mentések hello kijelölt kötet társított meg kell jelennie biztonsági mentés hello listájában.

## <a name="select-a-backup-set"></a>Válassza ki a biztonságimásolat-készletből
A következő lépéseket tooselect teljes hello a biztonságimásolat-készlet egy kötet vagy a biztonsági mentési házirend.

#### <a name="tooselect-a-backup-set"></a>tooselect biztonságimásolat-készletből
1. A hello StorSimple Manager szolgáltatás lapján, kattintson a hello **biztonságimásolat-katalógus** fülre.
2. Az alábbiak szerint szűrheti hello beállításokat:
   
   1. Válassza ki azt a hello megfelelő eszközt.
   2. Hello legördülő listában válassza ki hello kötet vagy a biztonsági mentési házirendet hello biztonsági mentés, hogy kívánja-e tooselect.
   3. Adjon meg hello időtartományt.
   4. Kattintson a pipa ikonra hello ![Pipa ikon](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) tooexecute ezt a lekérdezést.
      
      hello hello kijelölt kötet vagy a biztonsági mentési házirenddel társított biztonsági másolatok meg kell jelennie hello listájában biztonsági mentés.
3. Válassza ki, és bontsa ki a biztonságimásolat-készletből. Hello **visszaállítása** és **törlése** alján hello hello beállítások jelennek meg. Ezek a műveletek egyikét a hello biztonságimásolat-készlet, amely a kijelölt hajthatja végre.

## <a name="delete-a-backup-set"></a>A biztonságimásolat-készlet törlése
A biztonsági mentés tooretain-hello adatok már nem kívánja törölni. Hajtsa végre a következő lépéseket toodelete biztonságimásolat-készletből hello.

#### <a name="toodelete-a-backup-set"></a>toodelete biztonságimásolat-készletből
1. A hello StorSimple Manager szolgáltatás lapján, kattintson a hello **biztonságimásolat-katalógus lapon**.
2. Az alábbiak szerint szűrheti hello beállításokat:
   
   1. Válassza ki azt a hello megfelelő eszközt.
   2. Hello legördülő listában válassza ki hello kötet vagy a biztonsági mentési házirendet hello biztonsági mentés, hogy kívánja-e tooselect.
   3. Adjon meg hello időtartományt.
   4. Kattintson a pipa ikonra hello ![Pipa ikon](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) tooexecute ezt a lekérdezést.
      
      hello hello kijelölt kötet vagy a biztonsági mentési házirenddel társított biztonsági másolatok meg kell jelennie hello listájában biztonsági mentés.
3. Válassza ki, és bontsa ki a biztonságimásolat-készletből. Hello **visszaállítása** és **törlése** alján hello hello beállítások jelennek meg. Kattintson a **Törlés** gombra.
4. Értesítést fog kapni hello törlése folyamatban van, valamint hogy mikor sikeresen befejeződött. Hello törlése után frissítse a hello lekérdezés ezen a lapon. törölt hello biztonságimásolat-készlet nem fog megjelenni a biztonsági mentés hello listában.

## <a name="next-steps"></a>Következő lépések
* Ismerje meg, hogyan túl[használata hello biztonságimásolat-katalógus toorestore biztonságimásolat-készletből az eszköz](storsimple-restore-from-backup-set.md).
* Ismerje meg, hogyan túl[használata hello StorSimple Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-manager-service-administration.md).

