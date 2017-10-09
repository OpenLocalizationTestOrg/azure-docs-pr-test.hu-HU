---
title: "a StorSimple 8000 Series biztonsági másolatból kötet aaaRestore |} Microsoft Docs"
description: "Ismerteti, hogyan toouse hello StorSimple Device Manager szolgáltatás biztonságimásolat-katalógus toorestore a StorSimple-kötet a biztonságimásolat-készletből."
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
ms.date: 05/23/2017
ms.author: alkohli
ms.openlocfilehash: 0fe2e4c23a23c75ce4058a8531356c94c973c6f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="restore-a-storsimple-volume-from-a-backup-set"></a>A StorSimple-kötet visszaállítása egy biztonságimásolat-készlet

## <a name="overview"></a>Áttekintés

Ez az oktatóanyag leírja a StorSimple 8000 series eszközt egy meglévő biztonságimásolat-készletből végre hello visszaállítási műveletet. Használjon hello **biztonságimásolat-katalógus** panel toorestore helyi kötet vagy a felhőbeli biztonsági mentését. Hello **biztonságimásolat-katalógus** panelt jeleníti meg, amely jönnek létre, ha manuális vagy automatikus biztonsági mentés készül minden hello biztonsági mentés. biztonságimásolat-készletből hello visszaállítási folyamat során hello kötetet az azonnal, amíg az adatok hello háttér töltődik le.

Egy alternatív módszert toostart visszaállítási túl toogo**eszközök > [az eszköz] > kötetek**. A hello **kötetek** panelen válassza ki a kötetet, kattintson a jobb gombbal tooinvoke hello helyi menüt, és válassza ki **visszaállítása**.

## <a name="before-you-restore"></a>Mielőtt vissza tudná állítani

A helyreállítás megkezdése előtt tekintse át a következő kikötésekkel hello:

* **Végre kell hajtania az hello kötet offline** – hello kötet offline igénybe mindkét hello gazdagépen, és eszköz hello hello visszaállítási művelet indítása előtt. Bár a hello visszaállítási művelet automatikusan kiválasztja a hello kötetet hello eszközön, manuálisan kell hoznia hello eszköz online hello gazdagépen. Helyezheti hello kötetet hello állomáson, amint hello kötet hello eszköz online állapotban. (Nem szükséges toowait hello visszaállítási művelet befejezéséig.) Az eljárások, nyissa meg túl[kötet offline állapotba](storsimple-8000-manage-volumes-u2.md#take-a-volume-offline).

* **Visszaállítás után kötettípus** – törölt kötetek visszaállítása hello pillanatkép-hello típusú; Ez azt jelenti, hogy volt helyileg rögzített kötetek visszaállítását végzi el a helyileg rögzített kötetekhez, és volt rétegzett kötetek visszaállítását végzi el a rétegzett kötetek.

    Meglévő kötetek hello aktuális használat típusa hello kötet hello típus hello pillanatkép tárolt felülírja. Például ha (miatt tooa átalakítási műveletet végeznie), hogy kötettípus jelenleg helyileg rögzített kötet amikor hello kötet típusa történt rétegzett készült pillanatkép visszaállítása, majd hello kötet visszaállítja egy helyileg rögzített kötet. Hasonlóképpen ha meglévő helyileg rögzített kötet volt kibontva, és ezt követően a hajt végre, amikor hello kötet kisebb volt egy korábbi pillanatképből állítottak vissza hello visszaállított kötet megőrzik hello aktuális kiterjesztett méretét.

    A kötet nem alakítható át egy rétegzett kötet tooa helyileg rögzített kötet vagy egy helyileg rögzített kötet tooa a rétegzett kötet közben hello kötet visszaállítása folyamatban van. Várjon, amíg hello visszaállítási művelet befejeződik, majd átalakíthatja hello kötet tooanother típusa. A kötet konvertálása kapcsolatos információkért nyissa meg túl[hello kötet típusának módosítása](storsimple-8000-manage-volumes-u2.md#change-the-volume-type). 

* **hello kötetméretet tükrözi vissza hello kötet** – Ez akkor fontos szempont, ha egy helyileg rögzített kötet (mert helyileg rögzített kötetekhez teljesen kiépített) törölt visszaállítását végzi. Győződjön meg arról, hogy van elegendő hely, egy helyileg rögzített kötet korábban törölt toorestore megkísérlése előtt.

* **A kötet nem terjeszthető ki, miközben visszaállítása folyamatban van** – hello visszaállítási művelet befejezése előtt tooexpand hello kötet várja. A kötet kiterjesztését kapcsolatos információkért nyissa meg túl[kötet módosítása](storsimple-8000-manage-volumes-u2.md#modify-a-volume).

* **Végezheti el a biztonsági mentés során egy kötet visszaállítása** – az eljárások lépjen túl[hello StorSimple Device Manager szolgáltatás toomanage biztonsági mentési házirendek használata a](storsimple-8000-manage-backup-policies-u2.md).

* **A visszaállítási művelet megszakíthatja** – Ha megszakítja hello visszaállítási feladat, majd hello kötet vissza lesz állítva hello visszaállítási műveletet kezdeményezett előtti toohello-állapotát. Az eljárások, nyissa meg túl[feladatok megszakítása](storsimple-8000-manage-jobs-u2.md#cancel-a-job).

## <a name="how-does-restore-work"></a>Hogyan állítsa vissza a munkahelyi

4 vagy újabb frissítés rendszerű eszközöket a heatmap-alapú visszaállítási van megvalósítva. Hello állomás kérelmek tooaccess adatok hello eszköz éri el, mivel ezek a kérelmek nyomon követi, és egy heatmap jön létre. Magas kérelmek aránya magasabb tűz az adattömbök eredményez, mivel alacsonyabb kérelmek aránya az alsó tűz toochunks fordítja le. Hello adatok legalább kétszer toobe jelöléssel kell elérnie _működés közbeni_. A fájl módosított is van megjelölve _működés közbeni_. Miután elindította hello visszaállítás, az adatok proaktív hidratálásával történik hello heatmap alapján. Verziók Update 4-nél korábbi hello adatok letöltődik alapuló hozzáférés csak visszaállítás során.

a következő kikötésekkel hello tooheatmap alapú visszaállítások vonatkoznak:

* Csak a rétegzett kötetek engedélyezve van a változáskövetés Heatmap, és helyileg rögzített kötetek nem támogatottak.

* Heatmap-alapú visszaállítási nem támogatott, ha egy kötet tooanother eszköz Klónozás. 

* Ha egy helyi visszaállítási és hello eszközön létezik hello kötet toobe vissza egy helyi pillanatképet, majd azt nem rehydrate (mivel az adatok már helyileg nem érhető el). 

* Alapértelmezés szerint történő visszaállításához, a rendszer kezdeményezett hello rehidratálása feladatok proaktív rehydrate hello heatmap az adatokat. 

Update 4, a Windows PowerShell-parancsmagok is futó feladatok rehidratálása használt tooquery kell rehidratálása feladatok megszakítása és hello hello rehidratálása feladat állapotának beolvasása.

* `Get-HcsRehydrationJob`– Ez a parancsmag hello hello rehidratálása feladat állapotának beolvasása. Egy kötet egy egyetlen rehidratálása feladat elindul.

* `Set-HcsRehydrationJob`– Ez a parancsmag lehetővé teszi a toopause, folytatása hello rehidratálása feladat leállítása, ha hello rehidratálása van folyamatban.

A rehidratálása parancsmagok további információkért lépjen túl[Windows PowerShell parancsmag-referencia a StorSimple](https://technet.microsoft.com/library/dn688168.aspx).

Az automatikus rehdyration általában magasabb átmeneti olvasási teljesítmény várható. hello tényleges magniutde fejlesztései számos tényező befolyásolja, például a hozzáférési mintázatát, adatmennyiség és adattípus függ. 

egy rehidratálása feladat toocancel, hello PowerShell-parancsmagot is használhatja. Ha az összes jövőbeni visszaállítások hello toopermanently letiltása rehidratálása feladatok kívánja [forduljon a Microsoft Support](storsimple-8000-contact-microsoft-support.md).

## <a name="how-toouse-hello-backup-catalog"></a>Hogyan toouse hello biztonságimásolat-katalógus

Hello **biztonságimásolat-katalógus** panel biztosít egy lekérdezést, amely segít toonarrow a biztonságimásolat-készlet kiválasztása. Hello biztonságimásolat-készletek, amelyek a rendszer beolvassa a következő paraméterek hello alapján szűrheti:

* **Időtartománynak** – dátum és idő tartomány hello hello biztonságimásolat-készlet létrehozásakor.
* **Eszköz** – hello eszköz mely hello a biztonságimásolat-készlet létrehozása.
* **Biztonsági mentési házirend** vagy **kötet** – hello biztonsági mentési házirend, vagy a kötet a biztonságimásolat-készlet társítva.

hello szűrt biztonságimásolat-készletek majd megjelennének hello a következő attribútumok alapján:

* **Név** – hello hello biztonsági mentési házirend vagy a kötet társított hello biztonságimásolat-készlet neve.
* **Típus** – biztonsági másolat beállítása helyi pillanatképeket és felhőalapú pillanatfelvételek. Egy helyi pillanatfelvétel a köteten tárolt összes adat helyileg hello eszköz, biztonsági másolatot, mivel egy felhőalapú pillanatfelvétel toohello az adatok biztonsági mentése kötet hello felhőben található. Helyi pillanatképeket gyorsabb hozzáférést biztosít, mivel felhőalapú pillanatfelvételek az adatrugalmasság közül választ.
* **Méret** – hello hello biztonságimásolat-készlet tényleges mérete.
* **A létrehozott** – hello dátum és idő, amikor hello létrehozásuk. 
* **Kötetek** -hello hello biztonságimásolat-készlet társított kötetek számát.
* **Kezdeményezett** – hello biztonsági mentések kezdeményezhetők, automatikusan a tooa ütemezés szerint vagy manuálisan felhasználója. (A biztonsági mentési házirend tooschedule biztonsági mentéseket is használható. Másik lehetőségként használhatja a hello **biztonsági másolatok készítéséhez** beállítás tootake egy interaktív vagy igény szerinti biztonsági mentést.)

## <a name="how-toorestore-your-storsimple-volume-from-a-backup"></a>Hogyan toorestore a StorSimple-kötet egy biztonsági másolatból

Használhatja a hello **biztonságimásolat-katalógus** panel toorestore a StorSimple-kötet egy adott biztonsági másolatból. Ne feledje, azonban, hogy a kötet visszaállítja visszaállítása hello kötet toohello állapot, mint amikor hello a biztonsági mentés készült. Bármely hello biztonsági mentési művelet után felvett adatok elvesznek.

> [!WARNING]
> Egy biztonsági másolatból történő visszaállítását lecseréli a meglévő kötetek hello hello biztonsági másolatból. Ezt követően hello a biztonsági mentés készült írt adatok hello adatvesztést okozhat.


### <a name="toorestore-your-volume"></a>toorestore a kötet
1. Nyissa meg tooyour StorSimple Device Manager szolgáltatást, és kattintson a **biztonságimásolat-katalógus**.

2. Válassza ki a biztonságimásolat-készletet a következőképpen:
   
   1. Adjon meg hello időtartományt.
   2. Válassza ki azt a hello megfelelő eszközt.
   3. Hello legördülő listában válassza ki hello kötet vagy a biztonsági mentési házirendet hello biztonsági mentés, hogy kívánja-e tooselect.
   4. Kattintson a **alkalmaz** tooexecute ezt a lekérdezést.

    hello hello kijelölt kötet vagy a biztonsági mentési házirenddel társított biztonsági másolatok meg kell jelennie hello listájában biztonsági mentés.
   
    ![Biztonságimásolat-készlet listája](./media/storsimple-8000-restore-from-backup-set-u2/bucatalog.png)     
     
3. Bontsa ki a hello biztonságimásolat-készlet tooview hello társított kötetek. Ezek a kötetek offline állapotba kell helyezni a hello állomás és az eszköz visszaállítani őket. Hello hello köteteket hozzáférési **kötetek** az eszközt, és hajtsa végre hello panelen lévő lépéseket [kötet offline állapotba](storsimple-8000-manage-volumes-u2.md#take-a-volume-offline) tootake offline állapotba.
   
   > [!IMPORTANT]
   > Győződjön meg arról, hogy elvégezte hello kötetek offline hello gazdagépen először hello eszközön hello kötetek offline végrehajtása előtt. Ha nem hajtja végre hello kötetek offline hello állomáson, veremtúlcsordulást okozhat toodata sérülése.
   
4. Keresse meg a visszafelé toohello **biztonságimásolat-katalógus** lapot, és válasszon egy biztonságimásolat-készlet. Kattintson a jobb gombbal, és hello helyi menüből válassza **visszaállítása**.

    ![Biztonságimásolat-készlet listája](./media/storsimple-8000-restore-from-backup-set-u2/restorebu1.png)

5. Megerősítést kér bekéri. Felülvizsgálati hello adatok visszaállítására, és válassza a hello megerősítő jelölőnégyzetet.
   
    ![Jóváhagyás lap](./media/storsimple-8000-restore-from-backup-set-u2/restorebu2.png)

7.  Kattintson a **visszaállítása**. Ez elindít egy visszaállítási feladat, amelyet látni lehet hello elérésével **feladatok** lap.

    ![Jóváhagyás lap](./media/storsimple-8000-restore-from-backup-set-u2/restorebu5.png)

8. Hello visszaállítás befejezése után ellenőrizze, hogy a kötetek hello tartalmát helyébe kötetek hello biztonsági másolatból.


## <a name="if-hello-restore-fails"></a>Ha hello visszaállítása sikertelen

Hello restore művelet sikertelen lesz a bármilyen okból riasztást kap. Ha ez történik, a frissítési hello biztonságimásolat-lista tooverify hello biztonsági mentési még mindig érvényes. Ha hello biztonsági mentés érvényes és hello felhőből állítja vissza, majd kapcsolódási problémák okozta hello probléma.

toocomplete hello visszaállítási művelet, hello kötet offline érvénybe hello állomáson, majd próbálja megismételni hello visszaállítási műveletet. Vegye figyelembe, hogy a módosítások toohello kötetadatokról hello során végrehajtott visszaállítási folyamat el fog veszni.

## <a name="next-steps"></a>Következő lépések
* Ismerje meg, hogyan túl[kezelése StorSimple-köteteket](storsimple-8000-manage-volumes-u2.md).
* Ismerje meg, hogyan túl[használata hello StorSimple Device Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-8000-manager-service-administration.md).

