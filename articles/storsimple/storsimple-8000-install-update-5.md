---
title: "5. frissítésétől aaaInstall a StorSimple 8000 series eszközön |} Microsoft Docs"
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
ms.date: 08/22/2017
ms.author: alkohli
ms.openlocfilehash: a832f9953e8e39408efeeed375e3afe8eee8d0e5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-update-5-on-your-storsimple-device"></a>5. frissítésétől a StorSimple eszköz telepítése

## <a name="overview"></a>Áttekintés

Ez az oktatóanyag azt ismerteti, hogyan egy StorSimple eszközön keresztül szoftver korábbi verzióját futtató frissítési 5 tooinstall hello Azure-portál és hello gyorsjavítások a módszerrel. hello gyorsjavítás módszer használható egy átjáró konfigurálva van a hálózati adaptert, mint a DATA 0 hello StorSimple eszköz és a frissítés előtti 1 szoftver verziójából származó tooupdate próbált.

Frissítés 5 tartalmazza az eszköz szoftvere Storport és Spaceport, az operációs rendszer biztonsági frissítések és az operációs rendszer frissítéseit és a belső vezérlőprogram-frissítésekre.  hello eszközhöz, Spaceport, Storport, biztonsági és egyéb operációs rendszer frissítése nem zavaró frissítések érhetők el. hello rendszeres vagy nem zavaró frissítések hello Azure-portálon keresztül vagy hello gyorsjavítások a módszerrel lehet alkalmazni. hello lemez belső vezérlőprogram-frissítésekre zavaró frissítések érhetők el, és akkor hello eszköz hello gyorsjavítások a módszerrel hello Windows PowerShell felületén hello eszköz karbantartási módban van.

> [!IMPORTANT]
> * Manuális és automatikus előtti ellenőrzés végzett előzetes toohello telepítés toodetermine hello eszközök állapotának tekintetében a hardver állapotát és a hálózati kapcsolatot. Az előzetes ellenőrzések csak akkor, ha az Azure-portálon hello hello frissítések telepítését végzik.
> * Azt javasoljuk, hogy hello szoftver- és egyéb hello Azure-portálon keresztül rendszeres frissítéseket telepíti. Ha hello frissítés előtti átjáró ellenőrzés sikertelen hello portálon toohello Windows PowerShell-felületet hello eszköz (tooinstall frissítések) csak kell lépjen. Attól függően, hello verzióra frissít, a, hello frissítések órát is igénybe vehet 4 (vagy újabb) tooinstall. hello eszköz hello Windows PowerShell felületén keresztül hello karbantartási mód frissítéseket kell telepíteni. Karbantartási mód frissítések zavaró frissítések érhetők el, mert ezek eredményez le egyszerre az eszközhöz.
> * Ha fut a választható StorSimple Snapshot Manager hello, győződjön meg arról, hogy frissítette a Snapshot Manager verzió tooUpdate 5 előzetes tooupdating hello eszköz.


[!INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-5-via-hello-azure-portal"></a>5. frissítésétől telepítése hello Azure-portálon keresztül
Túl hajtsa végre a következő lépéseket tooupdate hello-az eszköz[frissítés 5](storsimple-update5-release-notes.md).

> [!NOTE]
> A Microsoft hello eszközről további diagnosztikai adatokat kéri le. Ennek eredményeképpen a műveletek csapat azonosítja az eszközöket, amelyek problémákat tapasztal, amikor azt jobb felszerelt toocollect információkkal hello eszközről és problémák elemzéséhez.

[!INCLUDE [storsimple-8000-install-update4-via-portal](../../includes/storsimple-8000-install-update5-via-portal.md)]

Győződjön meg arról, hogy az eszköz fut. **a StorSimple 8000 Series Update 5 (6.3.9600.17845)**. Hello **utolsó frissítés dátuma** kell módosítani.

* Most láthatja, hogy hello karbantartási mód frissítések érhetők el (Ez az üzenet továbbra is fel too24 óra telepítése után hello frissítések megjelenített toobe). Karbantartási mód frissítések zavaró frissítések eszköz állásidőt eredményezhettek, és csak akkor érvényesíthetők, az eszköz hello Windows PowerShell felületén keresztül érhetők el.

* Hello karbantartási mód frissítések letöltése felsorolt hello lépések segítségével [toodownload gyorsjavítások](#to-download-hotfixes) toosearch számára, és töltse le a KB4011837, amely telepíti a belső vezérlőprogram-frissítésekre (hello más frissítések már telepítve kell lennie mostanra). Kövesse a felsorolt hello lépéseket [telepítse, és ellenőrizze a karbantartási mód gyorsjavítások](#to-install-and-verify-maintenance-mode-hotfixes) tooinstall hello karbantartási mód frissítések.

## <a name="install-update-5-as-a-hotfix"></a>5. frissítésétől gyorsjavítás telepítése


hello szoftververziók, amelyekre hello gyorsjavítások a módszerrel frissítheti a következők:

* Frissítéskezelés 0,1 vagy 0,2 0,3
* 1., 1.1-es, 1.2 frissítése
* 2, 2.1, 2.2 frissítése
* Frissítés 3 3.1.
* 4. frissítés

> [!NOTE] 
> hello ajánlott módszer tooinstall frissítés 5-ös verziója hello Azure-portálon keresztül. Ez az eljárás használható, ha tooinstall hello frissítések hello Azure-portálon keresztül közben nem sikerül hello átjáró ellenőrzése. hello az ellenőrzés sikertelen, ha egy átjáró tooa nem DATA 0 hálózati adapterén rendelt és az eszköz frissítése 1-nél korábbi szoftver verziót futtat.

hello gyorsjavítás módszer a következő három lépésből hello foglalja magában:

1. A Microsoft Update katalógus hello hello gyorsjavításokat töltheti le.
2. Telepítse, és ellenőrizze a hello rendszeres mód gyorsjavítások.
3. Telepítse a, és ellenőrizze a hello karbantartási mód gyorsjavítás.

#### <a name="download-updates-for-your-device"></a>Az eszköz frissítéseinek letöltése

Kell töltse le és telepítse a következő hello gyorsjavítások hello előírt sorrendben, és javasolt mappák hello:

| Sorrendje | KB | Leírás | Frissítés típusa | Telepítés időpontja |A mappa telepítéséhez|
| --- | --- | --- | --- | --- | --- |
| 1. |KB4037264 |Szoftverfrissítés<br> Töltse le mindkét _HcsSfotwareUpdate.exe_ és _CisMSDAgent.exe_ |Rendszeres <br></br>A nem zavaró |~ 25 perc |FirstOrderUpdate|

Ha frissíti az Update 4 futtató eszközökről, csak akkor kell tooinstall hello operációs rendszer összegző frissítése második rendelés frissítésére.

| Sorrendje | KB | Leírás | Frissítés típusa | Telepítés időpontja |A mappa telepítéséhez|
| --- | --- | --- | --- | --- | --- |
| 2A. |KB4025336 |Az operációs rendszer összegző frissítések csomag <br> Töltse le a Windows Server 2012 R2 verzióra |Rendszeres <br></br>A nem zavaró |- |SecondOrderUpdate|

Ha 3 frissítést futtató eszközről telepítése vagy korábbi, telepítse a következő továbbá toohello összegző frissítések hello.

| Sorrendje | KB | Leírás | Frissítés típusa | Telepítés időpontja |A mappa telepítéséhez|
| --- | --- | --- | --- | --- | --- |
| 2B. |KB4011841 <br> KB4011842 |LSI illesztőprogram és a belső vezérlőprogram-frissítésekre <br> A legpontosabb Beállításhoz vezérlőprogram-frissítés (3.38 verzió) |Rendszeres <br></br>A nem zavaró |~ 3 óra <br> (2/a. tartalmazza. + 2B. + 2 C.)|SecondOrderUpdate|
| 2C. |KB3139398 <br> KB3142030 <br> KB3108381 <br> KB3153704 <br> KB3174644 <br> KB3139914   |Az operációs rendszer biztonsági frissítések csomag <br> Töltse le a Windows Server 2012 R2 verzióra |Rendszeres <br></br>A nem zavaró |- |SecondOrderUpdate|
| 2D. |KB3146621 <br> KB3103616 <br> KB3121261 <br> KB3123538 |Az operációs rendszer frissítéseinek csomag <br> Töltse le a Windows Server 2012 R2 verzióra |Rendszeres <br></br>A nem zavaró |- |SecondOrderUpdate|


Tooinstall lemez belső vezérlőprogram-frissítésekre felett megjelenő hello előző táblák összes hello frissítések is szükség lehet. Ellenőrizheti, hogy kell-e belső vezérlőprogram-frissítésekre hello hello futtatásával `Get-HcsFirmwareVersion` parancsmag. Ha ezek a belső vezérlőprogram verzióinak futtatja: `XMGJ`, `XGEG`, `KZ50`, `F6C2`, `VR08`, `N003`, `0107`, akkor nem szükséges tooinstall ezeket a frissítéseket.

| Sorrendje | KB | Leírás | Frissítés típusa | Telepítés időpontja | A mappa telepítéséhez|
| --- | --- | --- | --- | --- | --- |
| 3. |KB4037263 |Lemez belső vezérlőprogram |Karbantartás <br></br>Zavaró |~ 30 perc | ThirdOrderUpdate |

<br></br>

> [!IMPORTANT]
> * Ha frissíti az Update 4, hello teljes telepítés ideje Bezárás too4 óra.
> * Ez az eljárás tooapply hello használata előtt frissíteni, győződjön meg arról, hogy mindkét hello eszközvezérlők online állapotban, és minden hello hardverösszetevőinek kifogástalan.

Hajtsa végre a következő lépéseket toodownload hello és hello gyorsjavításainak telepítéséhez.

[!INCLUDE [storsimple-install-update5-hotfix](../../includes/storsimple-install-update5-hotfix.md)]

[!INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]

## <a name="next-steps"></a>Következő lépések
További tudnivalók hello [frissítés 5 kiadás](storsimple-update5-release-notes.md).

