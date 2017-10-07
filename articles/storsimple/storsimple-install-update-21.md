---
title: "a StorSimple eszköz frissítése 2.2 aaaInstall |} Microsoft Docs"
description: "Azt ismerteti, hogyan tooinstall a StorSimple 8000 Series Update 2.2 a StorSimple 8000 series eszközön."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 047c7a4b-73d0-45ea-8d51-c54d71871392
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/02/2016
ms.author: alkohli
ms.openlocfilehash: cedb83ce42bc6bb81a4e43345da3f25b71036d1a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-update-22-on-your-storsimple-device"></a>2.2-es frissítés telepítése a StorSimple eszköz
## <a name="overview"></a>Áttekintés
Ez az oktatóanyag azt ismerteti, hogyan egy StorSimple eszközön keresztül szoftver korábbi verzióját futtató frissítési 2.2 tooinstall hello a klasszikus Azure portálon, és hello gyorsjavítások a módszerrel. hello gyorsjavítás módszer használható egy átjáró konfigurálva van a hálózati adaptert, mint a DATA 0 hello StorSimple eszköz és a frissítés előtti 1 szoftver verziójából származó tooupdate próbált.

2.2 frissítés tartalmazza a eszköz szoftver, a WMI és az iSCSI-frissítéseket is. Ha frissíti a 2.1-es verziója, csak hello eszköz szoftverfrissítést kell toobe alkalmazza. Ha frissíti a frissítés előtti 2 verziójából származó, akkor is szükséges tooapply LSI illesztőprogram, a Spaceport, a Storport és a belső vezérlőprogram-frissítésekre. hello eszközhöz, WMI, iSCSI, LSI illesztőprogram, Spaceport és Storport javításokat nem zavaró frissítések érhetők el, és hello a klasszikus Azure portálon keresztül is alkalmazható. hello lemez belső vezérlőprogram-frissítésekre zavaró frissítések érhetők el, és csak akkor érvényesíthetők, hello eszköz hello Windows PowerShell felületén keresztül. 

> [!IMPORTANT]
> * Manuális és automatikus előtti ellenőrzés végzett előzetes toohello telepítés toodetermine hello eszközök állapotának tekintetében a hardver állapotát és a hálózati kapcsolatot. Az előzetes ellenőrzések csak akkor, ha a klasszikus Azure portálon hello hello frissítések telepítését végzik.
> * Azt javasoljuk, hogy hello szoftver telepítése és illesztőprogram-frissítést keresztül hello a klasszikus Azure portálon. Ha hello frissítés előtti átjáró ellenőrzés sikertelen hello portálon toohello Windows PowerShell-felületet hello eszköz (tooinstall frissítések) csak kell lépjen. Attól függően, hello verzióra frissít, a, hello frissítések 1.5-2.5 óra tooinstall is eltarthat. hello eszköz hello Windows PowerShell felületén keresztül hello karbantartási mód frissítéseket kell telepíteni. Karbantartási mód frissítések zavaró frissítések érhetők el, mert ezek jár le egyszerre az eszközhöz.
> * Ha fut a választható StorSimple Snapshot Manager hello, győződjön meg arról, hogy frissítette a Snapshot Manager verzió tooUpdate 2.2 előzetes tooupdating hello eszköz.
> 
> 

[!INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-22-via-hello-azure-classic-portal"></a>2.2-es frissítés telepítése hello a klasszikus Azure portálon keresztül
Túl hajtsa végre a következő lépéseket tooupdate hello-az eszköz[frissítés 2.2](storsimple-update21-release-notes.md).

> [!NOTE]
> Ha Ön alkalmazzák a 2. frissítés vagy újabb verziók (beleértve a 2.1-es frissítés), a Microsoft lesz képes toopull további diagnosztikai adatokat hello eszközről. Ennek eredményeképpen a műveletek csapat azonosítja az eszközöket, amelyek problémákat tapasztal, amikor azt jobb felszerelt toocollect információkkal hello eszközről és problémák elemzéséhez. 2. frissítés vagy újabb elfogadásával, hogy lehetővé teszik a számunkra tooprovide a proaktív támogatás.
> 
> 

[!INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

1. Győződjön meg arról, hogy az eszköz fut. **a StorSimple 8000 Series Update 2.2 (6.3.9600.17708)**. Hello **utolsó frissítés dátuma** is módosítani kell. 
   
   Ha egy verziója előzetes tooUpdate 2 frissít, akkor is látható, hogy hello karbantartási mód frissítések érhetők el (Ez az üzenet továbbra is fel too24 óra telepítése után hello frissítések megjelenített toobe).
   
   Karbantartási mód frissítések zavaró frissítések eszköz állásidőt eredményezhettek, és csak akkor érvényesíthetők, az eszköz hello Windows PowerShell felületén keresztül érhetők el. Bizonyos esetekben 1.2-es frissítés futtatásakor a lemez belső vezérlőprogram előfordulhat, hogy már naprakész, ebben az esetben nem kell tooinstall a karbantartási mód frissíti.
   
   Ha frissíti a 2. frissítést, az eszköz kell naprakész. Hello fennmaradó lépéseit kihagyhatja.
2. Hello karbantartási mód frissítések letöltése felsorolt hello lépések segítségével [toodownload gyorsjavítások](#to-download-hotfixes) toosearch számára, és töltse le a KB3121899, amely telepíti a belső vezérlőprogram-frissítésekre (hello más frissítések már telepítve kell lennie mostanra).
3. Kövesse a felsorolt hello lépéseket [telepítse, és ellenőrizze a karbantartási mód gyorsjavítások](#to-install-and-verify-maintenance-mode-hotfixes) tooinstall hello karbantartási mód frissítések. 

## <a name="install-update-22-as-a-hotfix"></a>Frissítés 2.2 gyorsjavítás telepítése
Ez az eljárás használható, ha tooinstall hello frissítések hello a klasszikus Azure portálon keresztül közben nem sikerül hello átjáró ellenőrzése. hello az ellenőrzés sikertelen, a hozzárendelt tooa nem DATA 0 hálózati adapterén átjáró és az eszköz fut-e a szoftver verziója előzetes tooUpdate 1.

hello szoftververziók, amelyekre hello gyorsjavítások a módszerrel frissítheti a következők:

* Frissítéskezelés 0,1 vagy 0,2 0,3
* 1., 1.1-es, 1.2 frissítése
* Frissítés 2, 2.1-es verziója 

> [!IMPORTANT]
> * Ha az eszköz kiadásban (GA) verziója fut, lépjen kapcsolatba [Microsoft Support](storsimple-contact-microsoft-support.md) tooassist hello való frissítéséhez.
> 
> 

hello gyorsjavítás módszer a következő három lépésből hello foglalja magában:

* A Microsoft Update katalógus hello hello gyorsjavításokat töltheti le.
* Telepítse, és ellenőrizze a hello rendszeres mód gyorsjavítások.
* Telepítse és hello karbantartási mód gyorsjavítás ellenőrzése (csak frissítésekor a frissítés előtti 2 szoftver).

#### <a name="download-updates-for-a-device-running-update-21-software"></a>Töltse le a frissítéseket egy frissítési 2.1 szoftvert futtató eszköz
**Ha az eszköz fut-e frissíteni 2.1**, le kell töltenie csak hello eszköz szoftverfrissítés KB3179904. Hello bináris fájl végrehajtásával kerüli meg a "minden-hcsmdssoftwareudpate" csak telepítéséhez. Ne telepítse a hello Cis és hello MDS ügynök frissítése végrehajtásával kerüli meg a `all-cismdsagentupdatebundle`. Hiba toodo így eredményez hiba. Ez a frissítés nem zavaró, IO nem fog működni, és hello eszköz nem tudja az állásidő.

#### <a name="download-updates-for-a-device-running-update-2-software"></a>2. frissítés szoftvert futtató eszköz frissítések letöltése
**Ha az eszköz fut-e 2. frissítés**, kell töltse le és telepítse a következő sorrendben előírt hello gyorsjavítások hello:

| Sorrendje | KB | Leírás | Frissítés típusa | Telepítés időpontja |
| --- | --- | --- | --- | --- |
| 1. |KB3179904 |Szoftverfrissítési &#42; |Rendszeres <br></br>A nem zavaró |~ 45 perc |
| 2. |KB3146621 |az iSCSI-csomag |Rendszeres <br></br>A nem zavaró |~ 20 perc |
| 3. |KB3103616 |WMI-csomag |Rendszeres <br></br>A nem zavaró |~ 12 perc |

 &#42;  *Vegye figyelembe, szoftverfrissítés két bináris fájlt tartalmaz: eszköz szoftverfrissítés végrehajtásával kerüli meg a `all-hcsmdssoftwareupdate` és a hello Cis és Mds ügynök végrehajtásával kerüli meg a `all-cismdsagentupdatebundle`. hello eszköz szoftverfrissítés hello Cis és Mds ügynök előtt telepíteni kell. Hello aktív vezérlő hello keresztül is újra kell indítania `Restart-HcsController` parancsmag hello Cis és MDS ügynök frissítés telepítését követően (és hello fennmaradó frissítések alkalmazása előtt).* 

#### <a name="download-updates-for-a-device-running-pre-update-2-software"></a>Töltse le a frissítés előtti 2 szoftvert futtató eszközök
**Ha az eszköz fut 0,2, 0,3, 1.0 és 1.1**, akkor le kell töltenie és hello LSI illesztőprogramjának telepítése és a belső vezérlőprogram frissítése továbbá toohello szoftvert, az iSCSI és a WMI-frissítések. A frissítés már telepítve van, ha futtatja a frissítés 1.2-es vagy 2. 

| Sorrendje | KB | Leírás | Frissítés típusa | Telepítés időpontja |
| --- | --- | --- | --- | --- |
| 4. |KB3121900 |LSI illesztőprogram és a belső vezérlőprogram |Rendszeres <br></br>A nem zavaró |~ 20 perc |

<br></br>
**Ha az eszköz 0,2, 0,3, 1.0-s, 1.1-es és 1.2-es verzió fut**, meg kell és letöltése és telepítése hello Spaceport hello Storport javítás. Ha futtatja a 2. frissítés már van telepítve.

| Sorrendje | KB | Leírás | Frissítés típusa | Telepítés időpontja |
| --- | --- | --- | --- | --- |
| 5. |KB3090322 |Spaceport javítás </br> Windows Server 2012 R2 |Rendszeres <br></br>A nem zavaró |~ 20 perc |
| 6. |KB3080728 |Storport-javítás </br> Windows Server 2012 R2 |Rendszeres <br></br>A nem zavaró |~ 20 perc |

<br></br>
Szükség lehet a tooinstall lemez belső vezérlőprogram-frissítésekre. Ellenőrizheti, hogy kell-e belső vezérlőprogram-frissítésekre hello hello futtatásával `Get-HcsFirmwareVersion` parancsmag. Ha ezek a belső vezérlőprogram verzióinak futtatja: `XMGG`, `XGEG`, `KZ50`, `F6C2`, `VR08`, akkor nem szükséges tooinstall ezeket a frissítéseket.

| Sorrendje | KB | Leírás | Frissítés típusa | Telepítés időpontja |
| --- | --- | --- | --- | --- |
| 7. |KB3121899 |Lemez belső vezérlőprogram |Karbantartás <br></br>Zavaró |~ 30 perc |

<br></br>

> [!IMPORTANT]
> * Ez az eljárás igények toobe csak egyszer végre tooapply frissítés 2.2. Hello Azure klasszikus portál tooapply soron következő frissítések is használhatja.
> * Ha frissíti a 2. frissítést, hello teljes telepítés ideje Bezárás too1.5 óra.
> * Ez az eljárás tooapply hello használata előtt frissíteni, győződjön meg arról, hogy mindkét hello eszközvezérlők online állapotban, és minden hello hardverösszetevőinek kifogástalan.
> 
> 

Hajtsa végre a következő lépéseket toodownload hello és hello gyorsjavításainak telepítéséhez.

[!INCLUDE [storsimple-install-update21-hotfix](../../includes/storsimple-install-update21-hotfix.md)]

[!INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]

## <a name="next-steps"></a>Következő lépések
További tudnivalók hello [frissítés 2.1-es kiadás](storsimple-update21-release-notes.md).

