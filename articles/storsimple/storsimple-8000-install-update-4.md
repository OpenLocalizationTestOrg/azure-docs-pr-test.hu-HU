---
title: "a StorSimple 8000 series eszköz Update 4 aaaInstall |} Microsoft Docs"
description: "Azt ismerteti, hogyan tooinstall a StorSimple 8000 Series Update 4 a StorSimple 8000 series eszközön."
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
ms.date: 08/02/2017
ms.author: alkohli
ms.openlocfilehash: 3507edbde5e6e43b6c450bfea19494d47b5a5ae7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-update-4-on-your-storsimple-device"></a>A StorSimple eszköz Update 4 telepítése

## <a name="overview"></a>Áttekintés

Ez az oktatóanyag azt ismerteti, hogyan tooinstall Update 4 a StorSimple eszköz egy korábbi verzióját szoftver keresztül hello Azure-portál és hello gyorsjavítások a módszerrel. hello gyorsjavítás módszer használható egy átjáró konfigurálva van a hálózati adaptert, mint a DATA 0 hello StorSimple eszköz és a frissítés előtti 1 szoftver verziójából származó tooupdate próbált.

Update 4 eszköz szoftver, a legpontosabb Beállításhoz belső vezérlőprogram, LSI illesztőprogram és a belső vezérlőprogram, Storport és Spaceport, az operációs rendszer biztonsági frissítések és az állomást, más operációs rendszer frissítéseit tartalmazza.  hello eszköz szoftver, a legpontosabb Beállításhoz belső vezérlőprogram, Spaceport, Storport és más operációs rendszer frissítése nem zavaró frissítések érhetők el. hello rendszeres vagy nem zavaró frissítések hello Azure-portálon keresztül vagy hello gyorsjavítások a módszerrel lehet alkalmazni. hello lemez belső vezérlőprogram-frissítésekre zavaró frissítések érhetők el, és csak akkor érvényesíthetők, hello gyorsjavítások a módszerrel hello eszköz hello Windows PowerShell felületén.

> [!IMPORTANT]
> * Manuális és automatikus előtti ellenőrzés végzett előzetes toohello telepítés toodetermine hello eszközök állapotának tekintetében a hardver állapotát és a hálózati kapcsolatot. Az előzetes ellenőrzések csak akkor, ha az Azure-portálon hello hello frissítések telepítését végzik.
> * Azt javasoljuk, hogy hello szoftver- és egyéb hello Azure-portálon keresztül rendszeres frissítéseket telepíti. Ha hello frissítés előtti átjáró ellenőrzés sikertelen hello portálon toohello Windows PowerShell-felületet hello eszköz (tooinstall frissítések) csak kell lépjen. Attól függően, hello verzióra frissít, a, hello frissítések órát is igénybe vehet 4 (vagy újabb) tooinstall. hello eszköz hello Windows PowerShell felületén keresztül hello karbantartási mód frissítéseket is telepíteni kell. Karbantartási mód frissítések zavaró frissítések érhetők el, mert ezek jár le egyszerre az eszközhöz.
> * Ha fut a választható StorSimple Snapshot Manager hello, győződjön meg arról, hogy frissítette a Snapshot Manager verzió tooUpdate 4 előzetes tooupdating hello eszköz.


[!INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-4-via-hello-azure-portal"></a>Telepítse az Update 4 hello Azure-portálon keresztül
Túl hajtsa végre a következő lépéseket tooupdate hello-az eszköz[Update 4](storsimple-update4-release-notes.md).

> [!NOTE]
> A Microsoft hello eszközről további diagnosztikai adatokat kéri le. Ennek eredményeképpen a műveletek csapat azonosítja az eszközöket, amelyek problémákat tapasztal, amikor azt jobb felszerelt toocollect információkkal hello eszközről és problémák elemzéséhez. 

[!INCLUDE [storsimple-8000-install-update4-via-portal](../../includes/storsimple-8000-install-update4-via-portal.md)]

Győződjön meg arról, hogy az eszköz fut. **a StorSimple 8000 Series Update 4 (6.3.9600.17820)**. Hello **utolsó frissítés dátuma** is módosítani kell.

* Most láthatja, hogy hello karbantartási mód frissítések érhetők el (Ez az üzenet továbbra is fel too24 óra telepítése után hello frissítések megjelenített toobe). Karbantartási mód frissítések zavaró frissítések eszköz állásidőt eredményezhettek, és csak akkor érvényesíthetők, az eszköz hello Windows PowerShell felületén keresztül érhetők el.

* Hello karbantartási mód frissítések letöltése felsorolt hello lépések segítségével [toodownload gyorsjavítások](#to-download-hotfixes) toosearch számára, és töltse le a KB4011837, amely telepíti a belső vezérlőprogram-frissítésekre (hello más frissítések már telepítve kell lennie mostanra). Kövesse a felsorolt hello lépéseket [telepítse, és ellenőrizze a karbantartási mód gyorsjavítások](#to-install-and-verify-maintenance-mode-hotfixes) tooinstall hello karbantartási mód frissítések.

## <a name="install-update-4-as-a-hotfix"></a>Update 4 telepíti egy gyorsjavítás
hello ajánlott módszer tooinstall Update 4 van hello Azure-portálon keresztül.

Ez az eljárás használható, ha tooinstall hello frissítések hello Azure-portálon keresztül közben nem sikerül hello átjáró ellenőrzése. hello az ellenőrzés sikertelen, a hozzárendelt tooa nem DATA 0 hálózati adapterén átjáró és az eszköz fut-e a szoftver verziója előzetes tooUpdate 1.

hello szoftververziók, amelyekre hello gyorsjavítások a módszerrel frissítheti a következők:

* Frissítéskezelés 0,1 vagy 0,2 0,3
* 1., 1.1-es, 1.2 frissítése
* 2, 2.1, 2.2 frissítése
* Frissítés 3 3.1.


hello gyorsjavítás módszer a következő három lépésből hello foglalja magában:

1. A Microsoft Update katalógus hello hello gyorsjavításokat töltheti le.
2. Telepítse, és ellenőrizze a hello rendszeres mód gyorsjavítások.
3. Telepítse a, és ellenőrizze a hello karbantartási mód gyorsjavítás.

#### <a name="download-updates-for-your-device"></a>Az eszköz frissítéseinek letöltése

Kell töltse le és telepítse a következő hello gyorsjavítások hello előírt sorrendben, és javasolt mappák hello:

| Sorrendje | KB | Leírás | Frissítés típusa | Telepítés időpontja |A mappa telepítéséhez|
| --- | --- | --- | --- | --- | --- |
| 1. |KB4011839 |Szoftverfrissítés |Rendszeres <br></br>A nem zavaró |~ 25 perc |FirstOrderUpdate|
| 2A. |KB4011841 <br> KB4011842 |LSI illesztőprogram és a belső vezérlőprogram-frissítésekre <br> A legpontosabb Beállításhoz vezérlőprogram-frissítés (3.38 verzió) |Rendszeres <br></br>A nem zavaró |~ 3 óra <br> (2/a. tartalmazza. + 2B. + 2 C.)|SecondOrderUpdate|
| 2B. |KB3139398, KB3108381 <br> KB3205400, KB3142030 <br> KB3197873, KB3197873 <br> KB3192392, KB3153704 <br> KB3174644, KB3139914  |Az operációs rendszer biztonsági frissítések csomag <br> Töltse le a Windows Server 2012 R2 rendszerben |Rendszeres <br></br>A nem zavaró |- |SecondOrderUpdate|
| 2C. |KB3210083, KB3103616 <br> KB3146621, KB3121261 <br> KB3123538 |Az operációs rendszer frissítéseinek csomag <br> Töltse le a Windows Server 2012 R2 rendszerben |Rendszeres <br></br>A nem zavaró |- |SecondOrderUpdate|

Tooinstall lemez belső vezérlőprogram-frissítésekre felett megjelenő hello előző táblák összes hello frissítések is szükség lehet. Ellenőrizheti, hogy kell-e belső vezérlőprogram-frissítésekre hello hello futtatásával `Get-HcsFirmwareVersion` parancsmag. Ha ezek a belső vezérlőprogram verzióinak futtatja: `XMGJ`, `XGEG`, `KZ50`, `F6C2`, `VR08`, `N002`, `0106`, akkor nem szükséges tooinstall ezeket a frissítéseket.

| Sorrendje | KB | Leírás | Frissítés típusa | Telepítés időpontja | A mappa telepítéséhez|
| --- | --- | --- | --- | --- | --- |
| 3. |KB3121899 |Lemez belső vezérlőprogram |Karbantartás <br></br>Zavaró |~ 30 perc | ThirdOrderUpdate |

<br></br>

> [!IMPORTANT]
> * Ez az eljárás igények toobe csak egyszer végre tooapply Update 4. Hello Azure portál tooapply soron következő frissítések is használhatja.
> * Ha frissíti a 3. frissítés vagy 3.1, hello teljes telepítés ideje Bezárás too4 óra.
> * Ez az eljárás tooapply hello használata előtt frissíteni, győződjön meg arról, hogy mindkét hello eszközvezérlők online állapotban, és minden hello hardverösszetevőinek kifogástalan.

Hajtsa végre a következő lépéseket toodownload hello és hello gyorsjavításainak telepítéséhez.

[!INCLUDE [storsimple-install-update4-hotfix](../../includes/storsimple-install-update4-hotfix.md)]

[!INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]

## <a name="next-steps"></a>Következő lépések
További tudnivalók hello [Update 4 kiadás](storsimple-update4-release-notes.md).

