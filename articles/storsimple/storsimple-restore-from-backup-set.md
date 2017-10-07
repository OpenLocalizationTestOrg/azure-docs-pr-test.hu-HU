---
title: "a StorSimple-kötet biztonsági másolatból aaaRestore |} Microsoft Docs"
description: "Ismerteti, hogyan toouse hello StorSimple Manager szolgáltatás biztonságimásolat-katalógus lap toorestore a StorSimple-kötet a biztonságimásolat-készletből."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: b979782e-3184-4465-ad5f-e4516a5885d2
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: e0efa74b14603be41af0cfc5400de3c39ab8824e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="restore-a-storsimple-volume-from-a-backup-set"></a>A StorSimple-kötet visszaállítása egy biztonságimásolat-készlet
[!INCLUDE [storsimple-version-selector-restore-from-backup](../../includes/storsimple-version-selector-restore-from-backup.md)]

## <a name="overview"></a>Áttekintés
Hello **biztonságimásolat-katalógus** oldal megjelenik, amely jönnek létre, ha manuális vagy automatikus biztonsági mentés készül minden hello biztonsági mentés. A lap toolist összes hello biztonsági mentés használja a biztonsági mentési házirend, vagy egy kötetet, jelölje be vagy törölje a biztonsági mentések, vagy használja a biztonsági mentési toorestore vagy kötet klónozása.

 ![Biztonságimásolat-katalógus lap](./media/storsimple-restore-from-backup-set/HCS_BackupCatalog.png)

Ez az oktatóanyag azt ismerteti, hogyan toouse hello **biztonságimásolat-katalógus** lap toorestore egy kötet a biztonságimásolat-készletből az eszközön.

## <a name="how-toouse-hello-backup-catalog"></a>Hogyan toouse hello biztonságimásolat-katalógus
Hello **biztonságimásolat-katalógus** lap biztosít egy lekérdezést, amely segít toonarrow a biztonságimásolat-készlet kiválasztása. Hello biztonságimásolat-készletek, amelyek a rendszer beolvassa a következő paraméterek hello alapján szűrheti:

* **Eszköz** – hello eszköz mely hello a biztonságimásolat-készlet létrehozása.
* **Biztonsági mentési házirend** vagy **kötet** – hello biztonsági mentési házirend, vagy a kötet a biztonságimásolat-készlet társítva.
* **A** és **való** – dátum és idő tartomány hello hello biztonságimásolat-készlet létrehozásakor.

hello szűrt biztonságimásolat-készletek majd megjelennének hello a következő attribútumok alapján:

* **Név** – hello hello biztonsági mentési házirend vagy a kötet társított hello biztonságimásolat-készlet neve.
* **Méret** – hello hello biztonságimásolat-készlet tényleges mérete.
* **A létrehozott** – hello dátum és idő, amikor hello létrehozásuk. 
* **Típus** – biztonsági másolat beállítása helyi pillanatképeket és felhőalapú pillanatfelvételek. Egy helyi pillanatfelvétel a köteten tárolt összes adat helyileg hello eszköz, biztonsági másolatot, mivel egy felhőalapú pillanatfelvétel toohello az adatok biztonsági mentése kötet hello felhőben található. Helyi pillanatképeket gyorsabb hozzáférést biztosít, mivel felhőalapú pillanatfelvételek az adatrugalmasság közül választ.
* **Által kezdeményezett** – hello biztonsági mentések kezdeményezhetők, automatikusan a tooa ütemezés szerint vagy manuálisan felhasználója. (A biztonsági mentési házirend tooschedule biztonsági mentéseket is használható. Másik lehetőségként használhatja a hello **biztonsági másolatok készítéséhez** beállítás tootake egy interaktív biztonsági mentést.)

## <a name="how-toorestore-your-storsimple-volume-from-a-backup"></a>Hogyan toorestore a StorSimple-kötet egy biztonsági másolatból
Használhatja a hello **biztonságimásolat-katalógus** lapon toorestore a StorSimple-kötet egy adott biztonsági másolatból. 

> [!WARNING]
> Egy biztonsági másolatból történő visszaállítását lecseréli a meglévő kötetek hello hello biztonsági másolatból. Ezt követően hello a biztonsági mentés készült írt adatok hello adatvesztést okozhat.
> 
> 

Egy olyan köteten a visszaállítási elindítása előtt győződjön meg arról, hogy hello kötet offline állapotban. Először kell először tootake hello kötet offline hello állomáson, majd az eszköz hello. Hello kövesse [kötet offline állapotba](storsimple-manage-volumes.md#take-a-volume-offline). Hajtsa végre a következő lépéseket toorestore egy kötet a biztonságimásolat-készletből hello.

### <a name="toorestore-from-a-backup-set"></a>a biztonságimásolat-készletből toorestore
1. A hello StorSimple Manager szolgáltatás lapján, kattintson a hello **biztonságimásolat-katalógus** fülre.
   
    ![Biztonságimásolat-katalógus](./media/storsimple-restore-from-backup-set/HCS_Restore.png)
2. Válassza ki a biztonságimásolat-készletet a következőképpen:
   
   1. Válassza ki azt a hello megfelelő eszközt.
   2. Hello legördülő listában válassza ki hello kötet vagy a biztonsági mentési házirendet hello biztonsági mentés, hogy kívánja-e tooselect.
   3. Adjon meg hello időtartományt.
   4. Kattintson a pipa ikonra hello ![pipa ikon](./media/storsimple-restore-from-backup-set/HCS_CheckIcon.png) tooexecute ezt a lekérdezést.
      
      hello hello kijelölt kötet vagy a biztonsági mentési házirenddel társított biztonsági másolatok meg kell jelennie hello listájában biztonsági mentés.
3. Bontsa ki a hello biztonságimásolat-készlet tooview hello társított kötetek. Ezek a kötetek offline állapotba kell helyezni a hello állomás és az eszköz visszaállítani őket. Hello kövesse [kötet offline állapotba](storsimple-manage-volumes.md#take-a-volume-offline).
   
   > [!IMPORTANT]
   > Győződjön meg arról, hogy elvégezte hello kötetek offline hello gazdagépen először hello eszközön hello kötetek offline végrehajtása előtt. Ha nem hajtja végre hello kötetek offline hello állomáson, veremtúlcsordulást okozhat toodata sérülése.
   > 
   > 
4. Válassza ki a biztonságimásolat-készletből. Kattintson a **visszaállítása** hello lap hello alján.
5. Megerősítést kér bekéri. 
   
    ![Jóváhagyás lap](./media/storsimple-restore-from-backup-set/HCS_ConfirmRestore.png)
6. Hello visszaállítási információk áttekintéséhez, és kattintson a pipa ikonra hello ![pipa ikon](./media/storsimple-restore-from-backup-set/HCS_CheckIcon.png). A szolgáltatás kezdeményez egy visszaállítási feladat, amelyet látni lehet hello elérésével **feladatok** lap. 
7. Hello visszaállítás befejezése után ellenőrizheti, hogy a kötetek hello tartalmát helyébe kötetek hello biztonsági másolatból.

![Videó elérhető](./media/storsimple-restore-from-backup-set/Video_icon.png)**Videó elérhető**

a videó bemutatja, hogyan használja a hello klónozást, és állítsa vissza a StorSimple toorecover törölt fájlok, a szolgáltatások toowatch kattintson [Itt](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).

## <a name="next-steps"></a>Következő lépések
* Ismerje meg, hogyan túl[kezelése StorSimple-köteteket](storsimple-manage-volumes.md).
* Ismerje meg, hogyan túl[használata hello StorSimple Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-manager-service-administration.md).

