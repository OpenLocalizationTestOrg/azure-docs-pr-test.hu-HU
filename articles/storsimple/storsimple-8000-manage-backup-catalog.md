---
title: "aaaManage a StorSimple biztonsági mentés katalógusa |} Microsoft Docs"
description: "Azt ismerteti, hogyan toouse hello StorSimple Device Manager szolgáltatás biztonságimásolat-katalógus lap toolist, jelölje ki, és törölje a biztonsági mentés."
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
ms.openlocfilehash: e464609e74409a06a198790719abd82ed03c03d9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-toomanage-your-backup-catalog"></a>A biztonságimásolat-katalógus használatáról az hello StorSimple Device Manager szolgáltatás toomanage
## <a name="overview"></a>Áttekintés
StorSimple Device Manager szolgáltatás hello **biztonságimásolat-katalógus** panelt jeleníti meg, amely jönnek létre, ha manuális vagy ütemezett biztonsági mentést készít minden hello biztonsági mentés. A lap toolist összes hello biztonsági mentés használja a biztonsági mentési házirend, vagy egy kötetet, jelölje be vagy törölje a biztonsági mentések, vagy használja a biztonsági mentési toorestore vagy kötet klónozása.

Ez az oktatóanyag azt ismerteti, hogyan toolist, válassza ki és törölje a biztonságimásolat-készlet. toolearn hogyan toorestore az eszköz biztonsági másolatból, nyissa meg túl[szeretné visszaállítani az eszközt egy biztonságimásolat-készlet](storsimple-8000-restore-from-backup-set-u2.md). Hogyan tooclone egy köteten, nyissa meg túl toolearn[klónozza a StorSimple-kötet](storsimple-8000-clone-volume-u2.md).

![Biztonságimásolat-katalógus](./media/storsimple-8000-manage-backup-catalog/bucatalog.png) 

Hello **biztonságimásolat-katalógus** panel biztosít egy lekérdezés toonarrow a biztonságimásolat-készlet kiválasztása. Hello biztonságimásolat-készletek, amelyek lekérése, a következő paraméterek hello alapján szűrheti:

* **Eszköz** – hello eszköz mely hello a biztonságimásolat-készlet létrehozása.
* **A biztonsági mentési házirend, vagy a kötet** – hello biztonsági mentési házirend, vagy a kötet a biztonságimásolat-készlet társítva.
* **A kezdő és a** – dátum és idő tartomány hello hello biztonságimásolat-készlet létrehozásakor.

hello szűrt biztonságimásolat-készletek majd megjelennének hello a következő attribútumok alapján:

* **Név** – hello hello biztonsági mentési házirend vagy a kötet társított hello biztonságimásolat-készlet neve.
* **Méret** – hello hello biztonságimásolat-készlet tényleges mérete.
* **Létrehozás** – hello dátum és idő, amikor hello létrehozásuk. 
* **Típus** – biztonsági másolat beállítása helyi pillanatképeket és felhőalapú pillanatfelvételek. Egy helyi pillanatfelvétel a köteten tárolt összes adat helyileg hello eszköz, biztonsági másolatot, mivel egy felhőalapú pillanatfelvétel toohello az adatok biztonsági mentése kötet hello felhőben található. Helyi pillanatképeket gyorsabb hozzáférést biztosít, mivel felhőalapú pillanatfelvételek az adatrugalmasság közül választ.
* **Elindító** – hello biztonsági mentések kezdeményezheti automatikusan egy ütemezés szerint vagy manuálisan egy felhasználó. A biztonsági mentési házirend tooschedule biztonsági mentéseket is használhatja. Másik lehetőségként használhatja a hello **biztonsági másolatok készítéséhez** beállítás tootake manuális biztonsági mentés.

## <a name="list-backup-sets-for-a-backup-policy"></a>Lista biztonságimásolat-készletet a biztonsági mentési házirend
Hajtsa végre a következő lépéseket toolist hello összes hello biztonsági mentés a biztonsági mentési házirend.

#### <a name="toolist-backup-sets"></a>toolist biztonságimásolat-készletek
1. Nyissa meg tooyour StorSimple Device Manager szolgáltatás és **biztonságimásolat-katalógus**.

2. Az alábbiak szerint szűrheti hello beállításokat:
   
   1. Adjon meg hello időtartományt.
   2. Válassza ki azt a hello megfelelő eszközt.
   3. Szűrés **biztonsági mentési házirend** tooview hello megfelelő hello biztonsági mentéseket.
   3. Hello biztonsági mentési házirend legördülő listából, válassza ki a **összes** összes hello hello kiválasztva a biztonsági mentések tooview eszköz.
   4. Kattintson a **alkalmaz** tooexecute ezt a lekérdezést.
      
      hello biztonsági mentések hello kiválasztva a biztonsági mentési házirend társított meg kell jelennie a biztonságimásolat-készletek hello listájában.

      ![Nyissa meg toobackup katalógus](./media/storsimple-8000-manage-backup-catalog/bucatalog1.png)

## <a name="select-a-backup-set"></a>Válassza ki a biztonságimásolat-készletből
A következő lépéseket tooselect teljes hello a biztonságimásolat-készlet egy kötet vagy a biztonsági mentési házirend.

#### <a name="tooselect-a-backup-set"></a>tooselect biztonságimásolat-készletből
1. Nyissa meg tooyour StorSimple Device Manager szolgáltatás és **biztonságimásolat-katalógus**.
2. Az alábbiak szerint szűrheti hello beállításokat:
   
   1. Adjon meg hello időtartományt. 
   2. Válassza ki azt a hello megfelelő eszközt. 
   3. Szűrheti a kötet vagy a biztonsági mentési házirend, hogy kívánja-e tooselect hello biztonsági mentés.
   4. Kattintson a **alkalmaz** tooexecute ezt a lekérdezést.
      
      hello hello kijelölt kötet vagy a biztonsági mentési házirenddel társított biztonsági másolatok meg kell jelennie hello listájában biztonsági mentés.

      ![Nyissa meg toobackup katalógus](./media/storsimple-8000-manage-backup-catalog/bucatalog1.png)

3. Válassza ki, és bontsa ki a biztonságimásolat-készletből. Most már megtekintheti hello biztonságimásolat-készletek bontásban hello köteteket tartalmaz. Hello **visszaállítása** és **törlése** beállítások érhetők el hello helyi menüjére (kattintson a jobb gombbal) hello biztonságimásolat-készlet. Ezek a műveletek egyikét a hello biztonságimásolat-készlet, amely a kijelölt hajthatja végre.

    ![Nyissa meg toobackup katalógus](./media/storsimple-8000-manage-backup-catalog/bucatalog2.png)

## <a name="delete-a-backup-set"></a>A biztonságimásolat-készlet törlése
A biztonsági mentés tooretain-hello adatok már nem kívánja törölni. Hajtsa végre a következő lépéseket toodelete biztonságimásolat-készletből hello.

#### <a name="toodelete-a-backup-set"></a>toodelete biztonságimásolat-készletből
 Nyissa meg tooyour StorSimple Device Manager szolgáltatás és **biztonságimásolat-katalógus**.
2. Az alábbiak szerint szűrheti hello beállításokat:
   
   1. Adjon meg hello időtartományt. 
   2. Válassza ki azt a hello megfelelő eszközt. 
   3. Szűrheti a kötet vagy a biztonsági mentési házirend, hogy kívánja-e tooselect hello biztonsági mentés.
   4. Kattintson a **alkalmaz** tooexecute ezt a lekérdezést.
      
      hello hello kijelölt kötet vagy a biztonsági mentési házirenddel társított biztonsági másolatok meg kell jelennie hello listájában biztonsági mentés.

      ![Nyissa meg toobackup katalógus](./media/storsimple-8000-manage-backup-catalog/bucatalog1.png)

3. Válassza ki, és bontsa ki a biztonságimásolat-készletből. Most már megtekintheti hello biztonságimásolat-készletek bontásban hello köteteket tartalmaz. Hello **visszaállítása** és **törlése** beállítások érhetők el hello helyi menüjére (kattintson a jobb gombbal) hello biztonságimásolat-készlet. Kattintson a jobb gombbal a kiválasztott hello biztonságimásolat-készlet és hello helyi menüben válassza a **törlése**.

    ![Nyissa meg toobackup katalógus](./media/storsimple-8000-manage-backup-catalog/bucatalog3.png)

4. Amikor felszólítja a megerősítésre, tekintse át a hello megjelenő információkat, majd kattintson **törlése**. hello kijelölt biztonsági véglegesen törlődik.

    ![Nyissa meg toobackup katalógus](./media/storsimple-8000-manage-backup-catalog/bucatalog4.png)  

5. Értesítést fog kapni hello törlése folyamatban van, valamint hogy mikor sikeresen befejeződött. Hello törlése után frissítse a hello lekérdezés ezen a lapon. törölt hello biztonságimásolat-készlet nem fog megjelenni a biztonsági mentés hello listában.

    ![Nyissa meg toobackup katalógus](./media/storsimple-8000-manage-backup-catalog/bucatalog7.png)

## <a name="next-steps"></a>Következő lépések
* Ismerje meg, hogyan túl[használata hello biztonságimásolat-katalógus toorestore biztonságimásolat-készletből az eszköz](storsimple-8000-restore-from-backup-set-u2.md).
* Ismerje meg, hogyan túl[használata hello StorSimple Device Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-8000-manager-service-administration.md).

