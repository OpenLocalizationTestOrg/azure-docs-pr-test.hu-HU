---
title: "a StorSimple-kötet biztonsági másolatból aaaRestore |} Microsoft Docs"
description: "Ismerteti, hogyan toouse hello StorSimple Manager szolgáltatás biztonságimásolat-katalógus lap toorestore a StorSimple-kötet a biztonságimásolat-készletből."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 6f289c39-96c7-4d57-b68a-4bc2e99aef9d
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 03/22/2017
ms.author: alkohli
ms.openlocfilehash: c2e38765e750749f5764b5cbf2167d3cd5edfe5d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="restore-a-storsimple-volume-from-a-backup-set-update-2"></a>A StorSimple-kötet visszaállítása egy biztonságimásolat-készlet (2. frissítés)
[!INCLUDE [storsimple-version-selector-restore-from-backup](../../includes/storsimple-version-selector-restore-from-backup.md)]

## <a name="overview"></a>Áttekintés
Hello **biztonságimásolat-katalógus** oldal megjelenik, amely jönnek létre, ha manuális vagy automatikus biztonsági mentés készül minden hello biztonsági mentés. Használja az oldal toolist és biztonsági mentéseit, állítsa vissza biztonságimásolat-készletből, vagy klónozza a kötet.

 ![Biztonságimásolat-katalógus lap](./media/storsimple-restore-from-backup-set-u2/restore.png)

Ez az oktatóanyag azt ismerteti, hogyan toouse hello **biztonságimásolat-katalógus** lapon toorestore biztonságimásolat-készletből az eszköz.

Visszaállíthatja egy helyi kötet vagy a felhőbeli biztonsági mentését. Mindkét esetben hello visszaállítási művelet azonnal, amíg az adatok letöltődik hello háttérben megteremti hello kötetet. 

## <a name="before-you-restore"></a>Mielőtt vissza tudná állítani
Még a visszaállítás elindítása előtt a következő kikötésekkel hello tisztában kell lennie:

* **Hello kötet offline állapotba** – hello kötet offline igénybe mindkét hello gazdagépen, és eszköz hello hello visszaállítási művelet indítása előtt. Bár a hello visszaállítási művelet automatikusan kiválasztja a hello kötetet hello eszközön, manuálisan kell hoznia hello eszköz online hello gazdagépen. Helyezheti hello kötetet hello állomáson, ha hello kötet hello eszköz online állapotban. (Nem szükséges toowait hello visszaállítási művelet befejezéséig.) Az eljárások, nyissa meg túl[kötet offline állapotba](storsimple-manage-volumes-u2.md#take-a-volume-offline).
* **Visszaállítás után kötettípus** – törölt kötetek visszaállítása hello pillanatkép-hello típusú. Volt helyileg rögzített kötetek visszaállítását végzi el a helyileg rögzített kötetekhez, és volt rétegzett kötetek visszaállítását végzi el a rétegzett kötetek.
  
    Meglévő kötetek hello aktuális használat típusa hello kötet hello típus hello pillanatkép tárolt felülírja. Például ha egy pillanatképből, amikor hello kötet típusa történt rétegzett készült állítsa vissza egy kötet, és, hogy kötettípus jelenleg helyileg rögzített (miatt tooa átalakítási műveletet), majd hello kötet visszaállítja egy helyileg rögzített kötet. Hasonlóképpen, meglévő helyileg rögzített kötet kibontva, és ezt követően a hajt végre, amikor hello kötet kisebb volt egy korábbi pillanatképből állítottak vissza, ha hello visszaállított kötet megtartja hello aktuális kiterjesztett méretét.
  
    A kötetek nem konvertálhatók egy rétegzett kötet tooa helyileg rögzített kötet vagy _viszont_ közben hello kötet visszaállítása folyamatban van. Várjon, amíg hello visszaállítási művelet befejeződik, majd átalakíthatja hello kötet tooanother típusa. A kötet konvertálása kapcsolatos információkért nyissa meg túl[hello kötet típusának módosítása](storsimple-manage-volumes-u2.md#change-the-volume-type). 
* **hello kötetméretet tükrözi vissza hello kötet** – Ez akkor fontos szempont, ha egy helyileg rögzített kötet (mert helyileg rögzített kötetekhez teljesen kiépített) törölt visszaállítását végzi. Győződjön meg arról, hogy van elegendő hely, egy helyileg rögzített kötet korábban törölt toorestore megkísérlése előtt. 
* **A kötet nem terjeszthető ki, miközben visszaállítása folyamatban van** – hello visszaállítási művelet befejezése előtt tooexpand hello kötet várja. A kötet kiterjesztését kapcsolatos információkért nyissa meg túl[kötet módosítása](storsimple-manage-volumes-u2.md#modify-a-volume).
* **Végezheti el a biztonsági mentés közben állítja vissza egy kötet** – az eljárások lépjen túl[hello StorSimple Manager szolgáltatás toomanage biztonsági mentési házirendek használata a](storsimple-manage-backup-policies.md).
* **A visszaállítási művelet megszakíthatja** – Ha megszakítja hello visszaállítási feladat, majd hello kötet vissza lesz állítva által kezdeményezett hello visszaállítás előtti toohello állapotot. Az eljárások, nyissa meg túl[feladatok megszakítása](storsimple-manage-jobs-u2.md#cancel-a-job).

## <a name="how-does-restore-work"></a>Hogyan állítsa vissza a munkahelyi
4 vagy újabb frissítés rendszerű eszközöket a heatmap-alapú visszaállítási van megvalósítva. Hello állomás kérelmek tooaccess adatok hello eszköz éri el, mivel ezek a kérelmek nyomon követi, és egy heatmap jön létre. Magas kérelmek aránya magasabb tűz az adattömbök eredményez, mivel alacsonyabb kérelmek aránya az alsó tűz toochunks fordítja le. Hello adatok legalább kétszer toobe jelöléssel kell elérnie _működés közbeni_. A fájl módosított is van megjelölve _működés közbeni_. Miután elindította hello visszaállítás, az adatok proaktív hidratálásával történik hello heatmap alapján. Verziók Update 4-nél korábbi hello adatok letöltött alapuló hozzáférés csak visszaállítás során. 

Heatmap-alapú engedélyezve van a változáskövetés csak rétegzett kötet, és helyileg rögzített kötetek nem támogatottak. Heatmap-alapú visszaállítási nem is támogatott, ha egy kötet tooanother eszköz Klónozás. Ha egy helyi visszaállítási és hello eszközön létezik hello kötet toobe vissza egy helyi pillanatképet, majd azt nem rehydrate (mivel az adatok már helyileg nem érhető el). Alapértelmezés szerint történő visszaállításához, a rendszer kezdeményezett hello rehidratálása feladatok proaktív rehydrate hello heatmap az adatokat. Update 4, a Windows PowerShell-parancsmagok is futó feladatok rehidratálása használt tooquery kell rehidratálása feladatok megszakítása és hello hello rehidratálása feladat állapotának beolvasása.

* `Get-HcsRehydrationJob`– Ez a parancsmag hello hello rehidratálása feladat állapotának beolvasása. Egy kötet egy egyetlen rehidratálása feladat elindul.
* `Set-HcsRehydrationJob`– Ez a parancsmag lehetővé teszi a toopause, folytatása hello rehidratálása feladat leállítása, ha hello rehidratálása van folyamatban. 

A rehidratálása parancsmagok további információkért lépjen túl[Windows PowerShell parancsmag-referencia a StorSimple](https://technet.microsoft.com/library/dn688168.aspx).

Az automatikus rehdyration általában magasabb átmeneti olvasási teljesítmény várható. hello tényleges magniutde fejlesztései számos tényező befolyásolja, például a hozzáférési mintázatát, adatmennyiség és adattípus függ. egy rehidratálása feladat toocancel, hello PowerShell-parancsmagot is használhatja. Ha a toopermanently letiltása rehidratálása feladataihoz összes hello jövőbeli visszaállítások, forduljon a Microsoft Support.

## <a name="how-toouse-hello-backup-catalog"></a>Hogyan toouse hello biztonságimásolat-katalógus
Hello **biztonságimásolat-katalógus** lap biztosít egy lekérdezést, amely segít toonarrow a biztonságimásolat-készlet kiválasztása. Hello biztonságimásolat-készletek, amelyek a rendszer beolvassa a következő paraméterek hello alapján szűrheti:

* **Eszköz** – hello eszköz mely hello a biztonságimásolat-készlet létrehozása.
* **Biztonsági mentési házirend** vagy **kötet** – hello biztonsági mentési házirend, vagy a kötet a biztonságimásolat-készlet társítva.
* **A** és **való** – dátum és idő tartomány hello hello biztonságimásolat-készlet létrehozásakor.

hello szűrt biztonságimásolat-készletek majd megjelennének hello a következő attribútumok alapján:

* **Név** – hello hello biztonsági mentési házirend vagy a kötet társított hello biztonságimásolat-készlet neve.
* **Méret** – hello hello biztonságimásolat-készlet tényleges mérete.
* **A létrehozott** – hello dátum és idő, amikor hello létrehozásuk. 
* **Típus** – biztonsági másolat beállítása helyi pillanatképeket és felhőalapú pillanatfelvételek. Egy helyi pillanatfelvétel az összes kötet adatait hello eszközön helyileg tárolt biztonsági másolatot. Egy felhőalapú pillanatfelvétel toohello az adatok biztonsági mentése kötet szereplő hello felhő hivatkozik. Helyi pillanatképeket gyorsabb hozzáférést biztosít, mivel felhőalapú pillanatfelvételek az adatrugalmasság közül választ.
* **Által kezdeményezett** – hello biztonsági mentések kezdeményezhetők, automatikusan a tooa ütemezés szerint vagy kézzel. (A biztonsági mentési házirend tooschedule biztonsági mentéseket is használható. Másik lehetőségként használhatja a hello **biztonsági másolatok készítéséhez** beállítás tootake egy interaktív biztonsági mentést.)

## <a name="how-toorestore-your-storsimple-volume-from-a-backup"></a>Hogyan toorestore a StorSimple-kötet egy biztonsági másolatból
Használhatja a hello **biztonságimásolat-katalógus** lapon toorestore a StorSimple-kötet egy adott biztonsági másolatból. Tartsa szem előtt, azonban, a kötet visszaállítása helyreállítja hello kötet toohello állapot, mint amikor hello a biztonsági mentés készült. Hello biztonsági mentési művelet után hozzáadott összes adat elvész.

> [!WARNING]
> Egy biztonsági másolatból történő visszaállítását lecseréli a meglévő kötetek hello hello biztonsági másolatból. Ezt követően hello a biztonsági mentés készült írt adatok hello adatvesztést okozhat.
> 
> 

### <a name="toorestore-your-volume"></a>toorestore a kötet
1. A hello StorSimple Manager szolgáltatás lapján, kattintson a hello **biztonságimásolat-katalógus** fülre.
   
    ![Biztonságimásolat-katalógus](./media/storsimple-restore-from-backup-set-u2/restore.png)
2. Válassza ki a biztonságimásolat-készletet a következőképpen:
   
   1. Válassza ki azt a hello megfelelő eszközt.
   2. Hello legördülő listában válassza ki hello kötet vagy a biztonsági mentési házirendet hello biztonsági mentés, hogy kívánja-e tooselect.
   3. Adjon meg hello időtartományt.
   4. Kattintson a pipa ikonra hello ![pipa ikon](./media/storsimple-restore-from-backup-set-u2/HCS_CheckIcon.png) tooexecute ezt a lekérdezést.
      
      hello hello kijelölt kötet vagy a biztonsági mentési házirenddel társított biztonsági másolatok meg kell jelennie hello listájában biztonsági mentés.
3. Bontsa ki a hello biztonságimásolat-készlet tooview hello társított kötetek. Ezek a kötetek offline állapotba kell helyezni a hello állomás és az eszköz visszaállítani őket. Hello hello köteteket hozzáférési **Kötettárolók** lapon, és kövesse a lépéseket hello [kötet offline állapotba](storsimple-manage-volumes-u2.md#take-a-volume-offline) tootake offline állapotba.
   
   > [!IMPORTANT]
   > Győződjön meg arról, hogy elvégezte hello kötetek offline hello gazdagépen először hello eszközön hello kötetek offline végrehajtása előtt. Ha nem hajtja végre hello kötetek offline hello állomáson, veremtúlcsordulást okozhat toodata sérülése.
   > 
   > 
4. Keresse meg a visszafelé toohello **biztonságimásolat-katalógus** lapot, és válasszon egy biztonságimásolat-készlet.
5. Kattintson a **visszaállítása** hello lap hello alján.
6. Kell megerősítést kérni. Felülvizsgálati hello adatok visszaállítására, és válassza a hello megerősítő jelölőnégyzetet.
   
    ![Jóváhagyás lap](./media/storsimple-restore-from-backup-set-u2/ConfirmRestore.png)
7. Kattintson a pipa ikonra hello ![pipa ikon](./media/storsimple-restore-from-backup-set-u2/HCS_CheckIcon.png). A visszaállítási feladat kezdődik. Megtekintheti a hello feladat hello elérésével **feladatok** lap. 
8. Hello visszaállítás befejezése után ellenőrizheti, hogy a kötetek hello tartalmát helyébe kötetek hello biztonsági másolatból.

![Videó elérhető](./media/storsimple-restore-from-backup-set-u2/Video_icon.png)**Videó elérhető**

a videó bemutatja, hogyan használja a hello klónozást, és állítsa vissza a StorSimple toorecover törölt fájlok, a szolgáltatások toowatch kattintson [Itt](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).

## <a name="if-hello-restore-fails"></a>Ha hello visszaállítása sikertelen
Riasztást kapjon, ha hello restore művelet sikertelen lesz a bármilyen okból. Ha ez történik, a frissítési hello biztonságimásolat-lista tooverify hello biztonsági mentési még mindig érvényes. Ha hello biztonsági mentés érvényes és hello felhőből állítja vissza, majd kapcsolódási problémák okozta hello probléma. 

toocomplete hello visszaállítási művelet, hello kötet offline érvénybe hello állomáson, majd próbálja megismételni hello visszaállítási műveletet. A módosítások toohello kötetadatokról hello visszaállítási folyamat során végrehajtott elvesznek.

## <a name="next-steps"></a>Következő lépések
* Ismerje meg, hogyan túl[kezelése StorSimple-köteteket](storsimple-manage-volumes-u2.md).
* Ismerje meg, hogyan túl[használata hello StorSimple Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-manager-service-administration.md).

