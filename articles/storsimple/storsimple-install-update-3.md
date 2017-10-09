---
title: "a StorSimple eszköz frissítése 3 aaaInstall |} Microsoft Docs"
description: "Azt ismerteti, hogyan tooinstall a StorSimple 8000 Series Update 3 a StorSimple 8000 series eszközön."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: c6c4634d-4f3a-4bc4-b307-a22bf18664e1
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a156b8919639f1c7afb0fdef3d882d40d48f1c48
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-update-3-on-your-storsimple-8000-series-device"></a>A StorSimple 8000 series eszközön Update 3 telepítéséhez

## <a name="overview"></a>Áttekintés

Ez az oktatóanyag azt ismerteti, hogyan hello tooinstall 3 frissítés egy korábbi verzióját szoftver keresztül StorSimple eszközön, a klasszikus Azure portálon, és hello gyorsjavítások a módszerrel. hello gyorsjavítás módszer használható egy átjáró konfigurálva van a hálózati adaptert, mint a DATA 0 hello StorSimple eszköz és a frissítés előtti 1 szoftver verziójából származó tooupdate próbált.

Frissítés 3 eszköz szoftver, a LSI illesztőprogram és a belső vezérlőprogram, Storport és Spaceport frissíti. Ha frissíti a 2. frissítés vagy korábbi verziójú, akkor fog is szükséges tooapply iSCSI, WMI, és bizonyos esetekben lemez a belső vezérlőprogram-frissítésekre. hello eszközhöz, WMI, iSCSI, LSI illesztőprogram, Spaceport és Storport javításokat nem zavaró frissítések érhetők el, és hello a klasszikus Azure portálon keresztül is alkalmazható. hello lemez belső vezérlőprogram-frissítésekre zavaró frissítések érhetők el, és csak akkor érvényesíthetők, hello eszköz hello Windows PowerShell felületén keresztül. 

> [!IMPORTANT]
> * Manuális és automatikus előtti ellenőrzés végzett előzetes toohello telepítés toodetermine hello eszközök állapotának tekintetében a hardver állapotát és a hálózati kapcsolatot. Az előzetes ellenőrzések csak akkor, ha a klasszikus Azure portálon hello hello frissítések telepítését végzik.
> * Azt javasoljuk, hogy hello szoftver telepítése és illesztőprogram-frissítést keresztül hello a klasszikus Azure portálon. Ha hello frissítés előtti átjáró ellenőrzés sikertelen hello portálon toohello Windows PowerShell-felületet hello eszköz (tooinstall frissítések) csak kell lépjen. Attól függően, hello verzióra frissít, a, hello frissítések 1.5-2.5 óra tooinstall is eltarthat. hello eszköz hello Windows PowerShell felületén keresztül hello karbantartási mód frissítéseket kell telepíteni. Karbantartási mód frissítések zavaró frissítések érhetők el, mert ezek jár le egyszerre az eszközhöz.
> * Ha fut a választható StorSimple Snapshot Manager hello, győződjön meg arról, hogy frissítette a Snapshot Manager verzió tooUpdate 2 előzetes tooupdating hello eszköz.
> 
> 

[!INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-3-via-hello-azure-classic-portal"></a>Frissítés 3 telepítéséhez hello a klasszikus Azure portálon keresztül
Túl hajtsa végre a következő lépéseket tooupdate hello-az eszköz[Update 3](storsimple-update3-release-notes.md).

> [!NOTE]
> Ha Ön alkalmazzák a 2. frissítés vagy újabb verziók (beleértve a 2.1-es frissítés), a Microsoft lesz képes toopull további diagnosztikai adatokat hello eszközről. Ennek eredményeképpen a műveletek csapat azonosítja az eszközöket, amelyek problémákat tapasztal, amikor azt jobb felszerelt toocollect információkkal hello eszközről és problémák elemzéséhez. 2. frissítés vagy újabb elfogadásával, hogy lehetővé teszik a számunkra tooprovide a proaktív támogatás.
> 
> 

[!INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

Győződjön meg arról, hogy az eszköz fut. **a StorSimple 8000 Series Update 3 (6.3.9600.17759)**. Hello **utolsó frissítés dátuma** is módosítani kell. 
   - Ha egy verziója előzetes tooUpdate 2 frissít, akkor is látható, hogy hello karbantartási mód frissítések érhetők el (Ez az üzenet továbbra is fel too24 óra telepítése után hello frissítések megjelenített toobe).
     Karbantartási mód frissítések zavaró frissítések eszköz állásidőt eredményezhettek, és csak akkor érvényesíthetők, az eszköz hello Windows PowerShell felületén keresztül érhetők el. Bizonyos esetekben 1.2-es frissítés futtatásakor a lemez belső vezérlőprogram előfordulhat, hogy már naprakész, ebben az esetben nem kell tooinstall a karbantartási mód frissíti.
   - Ha frissíteni a 2. frissítés vagy újabb, az eszköz kell naprakész. Hello következő lépést kihagyhatja.

Hello karbantartási mód frissítések letöltése felsorolt hello lépések segítségével [toodownload gyorsjavítások](#to-download-hotfixes) toosearch számára, és töltse le a KB3121899, amely telepíti a belső vezérlőprogram-frissítésekre (hello más frissítések már telepítve kell lennie mostanra). Kövesse a felsorolt hello lépéseket [telepítse, és ellenőrizze a karbantartási mód gyorsjavítások](#to-install-and-verify-maintenance-mode-hotfixes) tooinstall hello karbantartási mód frissítések. 

## <a name="install-update-3-as-a-hotfix"></a>3. frissítés gyorsjavítás telepítése
Ez az eljárás használható, ha tooinstall hello frissítések hello a klasszikus Azure portálon keresztül közben nem sikerül hello átjáró ellenőrzése. hello az ellenőrzés sikertelen, a hozzárendelt tooa nem DATA 0 hálózati adapterén átjáró és az eszköz fut-e a szoftver verziója előzetes tooUpdate 1.

hello szoftververziók, amelyekre hello gyorsjavítások a módszerrel frissítheti a következők:

* Frissítéskezelés 0,1 vagy 0,2 0,3
* 1., 1.1-es, 1.2 frissítése
* 2, 2.1, 2.2 frissítése 

> [!IMPORTANT]
> * Ha az eszköz kiadásban (GA) verziója fut, lépjen kapcsolatba [Microsoft Support](storsimple-contact-microsoft-support.md) tooassist hello való frissítéséhez.
> 
> 

hello gyorsjavítás módszer a következő három lépésből hello foglalja magában:

1. A Microsoft Update katalógus hello hello gyorsjavításokat töltheti le.
2. Telepítse, és ellenőrizze a hello rendszeres mód gyorsjavítások.
3. Telepítse és hello karbantartási mód gyorsjavítás ellenőrzése (csak frissítésekor a frissítés előtti 2 szoftver).

#### <a name="download-updates-for-your-device"></a>Az eszköz frissítéseinek letöltése
**Ha az eszköz fut frissítés 2.1-es vagy 2.2**, kell töltse le és telepítse a következő sorrendben előírt hello gyorsjavítások hello:

| Sorrendje | KB | Leírás | Frissítés típusa | Telepítés időpontja |
| --- | --- | --- | --- | --- |
| 1. |KB3186843 |Szoftverfrissítési &#42; |Rendszeres <br></br>A nem zavaró |~ 45 perc |
| 2. |KB3186859 |LSI illesztőprogram és a belső vezérlőprogram |Rendszeres <br></br>A nem zavaró |~ 20 perc |
| 3. |KB3121261 |Storport és Spaceport javítás </br> Windows Server 2012 R2 |Rendszeres <br></br>A nem zavaró |~ 20 perc |

&#42;  *Vegye figyelembe, hogy hello szoftverfrissítés két bináris fájlok áll: eszköz szoftverfrissítés végrehajtásával kerüli meg a `all-hcsmdssoftwareupdate` és a hello Cis és Mds ügynök végrehajtásával kerüli meg a `all-cismdsagentupdatebundle`. hello Cis és Mds előtt telepíteni kell a hello eszköz szoftverfrissítés ügynök. Hello aktív vezérlő hello keresztül is újra kell indítania `Restart-HcsController` parancsmag hello Cis és Mds ügynök frissítés telepítését követően (és hello fennmaradó frissítések alkalmazása előtt).* 

**Ha az eszköz fut-e frissítés 0,1, 0,2, 0,3, 1.0-s, 1.1-es, 1.2-es vagy 2.0-s**, kell letölti és telepíti a következő hozzáadása toohello szoftver LSI illesztőprogram gyorsjavítások hello és a belső vezérlőprogram frissítése (látható hello megelőző táblázat), az előírt sorrendben hello:

| Sorrendje | KB | Leírás | Frissítés típusa | Telepítés időpontja |
| --- | --- | --- | --- | --- |
| 4. |KB3146621 |az iSCSI-csomag |Rendszeres <br></br>A nem zavaró |~ 20 perc |
| 5. |KB3103616 |WMI-csomag |Rendszeres <br></br>A nem zavaró |~ 12 perc |

<br></br>

**Ha az eszköz 0,2, 0,3, 1.0-s, 1.1-es és 1.2-es verzió fut**, szükség lehet a tooinstall lemez belső vezérlőprogram-frissítésekre felett megjelenő hello előző táblák összes hello frissítések. Ellenőrizheti, hogy kell-e belső vezérlőprogram-frissítésekre hello hello futtatásával `Get-HcsFirmwareVersion` parancsmag. Ha ezek a belső vezérlőprogram verzióinak futtatja: `XMGG`, `XGEG`, `KZ50`, `F6C2`, `VR08`, akkor nem szükséges tooinstall ezeket a frissítéseket.

| Sorrendje | KB | Leírás | Frissítés típusa | Telepítés időpontja |
| --- | --- | --- | --- | --- |
| 6. |KB3121899 |Lemez belső vezérlőprogram |Karbantartás <br></br>Zavaró |~ 30 perc |

<br></br>

> [!IMPORTANT]
> * Ez az eljárás igények toobe csak egyszer végre tooapply Update 3. Hello Azure klasszikus portál tooapply soron következő frissítések is használhatja.
> * Ha a frissítés 2.2 frissítését, hello teljes telepítés ideje Bezárás too1.1 óra.
> * Ez az eljárás tooapply hello használata előtt frissíteni, győződjön meg arról, hogy mindkét hello eszközvezérlők online állapotban, és minden hello hardverösszetevőinek kifogástalan.
> 
> 

Hajtsa végre a következő lépéseket toodownload hello és hello gyorsjavításainak telepítéséhez.

[!INCLUDE [storsimple-install-update3-hotfix](../../includes/storsimple-install-update3-hotfix.md)]

[!INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]

## <a name="next-steps"></a>Következő lépések
További tudnivalók hello [Update 3 kiadásra](storsimple-update3-release-notes.md).

