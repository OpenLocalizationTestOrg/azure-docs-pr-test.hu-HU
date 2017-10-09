---
title: "a StorSimple eszköz Update 2 aaaInstall |} Microsoft Docs"
description: "Azt ismerteti, hogyan tooinstall a StorSimple 8000 Series Update 2 a StorSimple 8000 series eszközön."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 8c8981df-75d9-4d19-b137-d6c6ba39dcfb
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 09/21/2016
ms.author: alkohli
ms.openlocfilehash: 33a0bea4358c944644563192f686af332d2ad7bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-update-2-on-your-storsimple-device"></a>2. frissítés telepítése a StorSimple eszköz
## <a name="overview"></a>Áttekintés
Ez az oktatóanyag azt ismerteti, hogyan tooinstall frissítése a StorSimple eszköz egy korábbi verzióját szoftver hello a klasszikus Azure portálon keresztül a 2. hello oktatóanyag is magában foglalja a hello lépéseket, amikor egy átjáró konfigurálva van a hálózati adaptert, mint a DATA 0 hello StorSimple eszköz és a frissítés előtti 1 szoftver verziójából származó tooupdate próbált hello frissítéséhez szükséges.

Frissítés 2 eszköz szoftverfrissítések, a LSI illesztőprogram-frissítést és a belső vezérlőprogram-frissítésekre foglalja magában. hello eszköz szoftverek és LSI frissítések nem zavaró frissítések érhetők el, és hello a klasszikus Azure portálon keresztül felügyelhetők. hello lemez belső vezérlőprogram-frissítésekre zavaró frissítések érhetők el, és csak akkor érvényesíthetők, hello eszköz hello Windows PowerShell felületén keresztül.

> [!IMPORTANT]
> * Nem láthatja a 2. frissítés azonnal, mert a hello frissítések fázisokra bontva történő bevezetéséhez végezzük. A néhány nap múlva újra a frissítések keresését, a frissítés hamarosan elérhető lesz.
> * Manuális és automatikus előtti ellenőrzés végzett előzetes toohello telepítés toodetermine hello eszközök állapotának tekintetében a hardver állapotát és a hálózati kapcsolatot. Az előzetes ellenőrzések csak akkor, ha a klasszikus Azure portálon hello hello frissítések telepítését végzik.
> * Azt javasoljuk, hogy hello szoftver telepítése és illesztőprogram-frissítést keresztül hello a klasszikus Azure portálon. Ha hello frissítés előtti átjáró ellenőrzés sikertelen hello portálon toohello Windows PowerShell-felületet hello eszköz (tooinstall frissítések) csak kell lépjen. hello frissítések 4 – 7 óra tooinstall (beleértve a Windows-frissítések hello) is igénybe vehet. hello eszköz hello Windows PowerShell felületén keresztül hello karbantartási mód frissítéseket kell telepíteni. Karbantartási mód frissítések zavaró frissítések érhetők el, mert ezek jár le egyszerre az eszközhöz.
> * Ha fut a választható StorSimple Snapshot Manager hello, győződjön meg arról, hogy frissítette a Snapshot Manager verzió tooUpdate 2 előzetes tooupdating hello eszköz.
> 
> 

[!INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-2-via-hello-azure-classic-portal"></a>2. frissítés telepítése hello a klasszikus Azure portálon keresztül
Túl hajtsa végre a következő lépéseket tooupdate hello-az eszköz[Update 2](storsimple-update2-release-notes.md).

> [!NOTE]
> 2. frissítés lehetővé teszi, hogy a Microsoft toopull további diagnosztikai adatokat hello eszközről. Ennek eredményeképpen a műveletek csapat azonosítja az eszközöket, amelyek problémákat tapasztal, amikor azt jobb felszerelt toocollect információkkal hello eszközről és problémák elemzéséhez. 2. frissítés elfogadásával, hogy lehetővé teszik a számunkra tooprovide a proaktív támogatás.
> 
> 

[!INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

1. Győződjön meg arról, hogy az eszköz fut. **a StorSimple 8000 Series Update 2 (6.3.9600.17673)**. Hello **utolsó frissítés dátuma** is módosítani kell. Azt is megtudhatja, hogy a karbantartási mód frissítések érhetők el (Ez az üzenet továbbra is fel too24 óra telepítése után hello frissítések megjelenített toobe).
   
   Karbantartási mód frissítések zavaró frissítések eszköz állásidőt eredményezhettek, és csak akkor érvényesíthetők, az eszköz hello Windows PowerShell felületén keresztül érhetők el. Bizonyos esetekben 1.2-es frissítés futtatásakor a lemez belső vezérlőprogram előfordulhat, hogy már naprakész, ebben az esetben nem kell tooinstall a karbantartási mód frissíti.
2. Hello karbantartási mód frissítések letöltése felsorolt hello lépések segítségével [toodownload gyorsjavítások](#to-download-hotfixes) toosearch számára, és töltse le a KB3121899, amely telepíti a belső vezérlőprogram-frissítésekre (hello más frissítések már telepítve kell lennie mostanra).
3. Kövesse a felsorolt hello lépéseket [telepítse, és ellenőrizze a karbantartási mód gyorsjavítások](#to-install-and-verify-maintenance-mode-hotfixes) tooinstall hello karbantartási mód frissítések.

## <a name="install-update-2-as-a-hotfix"></a>2. frissítés gyorsjavítás telepítése
Ez az eljárás használható, ha tooinstall hello frissítések hello a klasszikus Azure portálon keresztül közben nem sikerül hello átjáró ellenőrzése. hello az ellenőrzés sikertelen, a hozzárendelt tooa nem DATA 0 hálózati adapterén átjáró és az eszköz fut-e a szoftver verziója előzetes tooUpdate 1.

hello szoftververziók, amelyekre hello gyorsjavítások a módszerrel frissítheti a frissítés 0,1, 0,2, frissítés és frissítés 0,3, 1. frissítést, frissítés 1.1-es és 1.2-es frissítés. hello gyorsjavítás módszer a következő három lépésből hello foglalja magában:

* A Microsoft Update katalógus hello hello gyorsjavításokat töltheti le.
* Telepítse, és ellenőrizze a hello rendszeres mód gyorsjavítások.
* Telepítse a, és ellenőrizze a hello karbantartási mód gyorsjavítás.

tooinstall Update 2 egy gyorsjavítás, kell letölti és telepíti a következő gyorsjavításokat hello:

| Sorrendje | KB | Leírás | Frissítés típusa |
| --- | --- | --- | --- |
| 1 |KB3121901 |Szoftverfrissítés |Rendszeres |
| 2 |KB3121900 |LSI illesztőprogram |Rendszeres |
| 3 |KB3080728 |Storport-javítás </br> Windows Server 2012 R2 |Rendszeres |
| 4 |KB3090322 |Spaceport javítás </br> Windows Server 2012 R2 |Rendszeres |
| 5 |KB3121899 |Lemez belső vezérlőprogram |Karbantartás |

> [!IMPORTANT]
> * Ha az eszköz kiadásban (GA) verziója fut, lépjen kapcsolatba [Microsoft Support](storsimple-contact-microsoft-support.md) tooassist hello való frissítéséhez.
> * Ez az eljárás igények toobe csak egyszer végre tooapply Update 2. Hello Azure klasszikus portál tooapply soron következő frissítések is használhatja.
> * Minden egyes gyorsjavítás telepítési toocomplete körülbelül 20 percet is igénybe vehet. Teljes telepítés ideje Bezárás too2 óra.
> * Ez az eljárás tooapply hello használata előtt frissíteni, győződjön meg arról, hogy mindkét eszközvezérlők online állapotban, és minden hello hardverösszetevőinek kifogástalan.
> 
> 

A következő lépéseket tooapply hello a frissítés végrehajtásához egy gyorsjavítást.

[!INCLUDE [storsimple-install-update2-hotfix](../../includes/storsimple-install-update2-hotfix.md)]

[!INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]

## <a name="next-steps"></a>Következő lépések
További tudnivalók hello [Update 2 kiadásban](storsimple-update2-release-notes.md).

