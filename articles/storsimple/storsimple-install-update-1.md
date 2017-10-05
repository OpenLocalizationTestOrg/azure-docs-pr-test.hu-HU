---
title: "1.2-es frissítés telepítése a StorSimple eszköz |} Microsoft Docs"
description: "Ismerteti a StorSimple 8000 Series 1.2-es frissítés telepítése a StorSimple 8000 series eszközön."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 7a513923-eb77-4078-b0ab-f8e90183796a
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 80ff35cc47dfc38089f4c392ef4c90baf9ccc03e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="install-update-12-on-your-storsimple-8000-series-device"></a>A StorSimple 8000 series eszközön 1.2-es frissítés telepítése
## <a name="overview"></a>Áttekintés
Ez az oktatóanyag azt ismerteti, hogyan 1.2-es frissítés telepítése, amely az 1. frissítés előtt szoftver verziója fut. a StorSimple eszköz. Az oktatóanyag egy átjárót a hálózati adapteren, mint a DATA 0 a StorSimple eszköz konfigurálásakor frissítéséhez szükséges további lépéseket is ismerteti.

Frissítés 1.2-es eszköz szoftverfrissítések, a LSI illesztőprogram-frissítést és a belső vezérlőprogram-frissítésekre foglalja magában. A szoftver- és illesztőprogram-frissítést LSI nem zavaró frissítések érhetők el, és a klasszikus Azure portálon keresztül felügyelhetők. A lemez belső vezérlőprogram-frissítésekre zavaró frissítések érhetők el, és csak akkor érvényesíthetők, az eszköz a Windows PowerShell felületén keresztül.

Attól függően, melyik verzióját az eszköz fut, a segítségével meghatározhatja, hogy ha 1.2-es frissítés alkalmazza a rendszer. Nyissa meg az eszköz szoftverének verziójával ellenőrizheti a **gyors áttekintő** az eszköz szakasza **irányítópult**.

</br>

| Ha a szoftver verziója... | Mi történik, a portálon? |
| --- | --- |
| Kiadás – GA |Ha Release verzióban (GA) futtatja, nem vonatkoznak a frissítés. Adjon [forduljon a Microsoft Support](storsimple-contact-microsoft-support.md) az eszköz frissítéséhez. |
| 0.1-es frissítés |Portál 1.2-es frissítés vonatkozik. |
| 0.2-es frissítés |Portál 1.2-es frissítés vonatkozik. |
| 0.3-as frissítés |Portál 1.2-es frissítés vonatkozik. |
| 1. frissítés |A frissítés nem lesz elérhető. |
| 1.1-es frissítés |A frissítés nem lesz elérhető. |

</br>

> [!IMPORTANT]
> * 1.2-es frissítés nem láthatók azonnal, mert a frissítések fázisokra bontva történő bevezetéséhez végezzük. A néhány nap múlva újra a frissítések keresését, a frissítés hamarosan elérhető lesz.
> * A frissítés olyan manuális és automatikus előtti ellenőrzi, hogy az eszköz állapotának tekintetében a hardver állapotát és a hálózati kapcsolatot tartalmaz. Az előzetes ellenőrzések csak akkor, ha a frissítések telepítését a klasszikus Azure-portálon történik.
> * Azt javasoljuk, hogy telepítse a szoftver- és illesztőprogram frissítéseket, a klasszikus Azure portálon keresztül. Csak abban az csak lépjen a Windows PowerShell felületét (a frissítések telepítése) az eszköz, ha a frissítés előtti átjáró ellenőrzés sikertelen, a portálon. A frissítések telepítése (beleértve a Windows-frissítések) 5-10 óráig is eltarthat. A karbantartási mód frissítéseket telepíteni kell az eszköz a Windows PowerShell felületén keresztül. Karbantartási mód frissítések zavaró frissítések érhetők el, mert ezek jár le egyszerre az eszközhöz.
> 
> 

[!INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-12-via-the-azure-classic-portal"></a>1.2-es frissítés telepítése a klasszikus Azure portálon keresztül
Az eszköz frissítése a következő lépésekkel [frissítése 1.2](storsimple-update1-release-notes.md). Ezzel az eljárással csak akkor, ha más átjáró is konfigurálva a DATA 0 hálózati adapterén az eszközön.

[!INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

1. Győződjön meg arról, hogy az eszköz fut. **a StorSimple 8000 Series Update 1.2 (6.3.9600.17584)**. A **utolsó frissítés dátuma** is módosítani kell. Azt is megtudhatja, hogy a karbantartási mód frissítések érhetők el (Ez az üzenet előfordulhat, hogy továbbra is megjelennek a frissítések telepítése után legfeljebb 24 órában).
   
   Karbantartási mód frissítések zavaró frissítések eszköz állásidőt eredményezhettek, és csak akkor érvényesíthetők, az eszköz a Windows PowerShell felületén keresztül érhetők el.
   
   ![Karbantartási lap](./media/storsimple-install-update-1/InstallUpdate12_10M.png "karbantartási lap")
2. A karbantartási mód frissítések letöltése a felsorolt lépéseket követve [gyorsjavítások letöltése](#to-download-hotfixes) keresését, és töltse le a KB3063416, mely telepíti lemez belső vezérlőprogram-frissítésekre (a más frissítések már telepítve kell lennie mostanra).
3. Kövesse a felsorolt lépéseket [telepítse, és ellenőrizze a karbantartási mód gyorsjavítások](#to-install-and-verify-maintenance-mode-hotfixes) a karbantartási mód telepítendő frissítések.
4. A klasszikus Azure portálon keresse meg a **karbantartási** lapon, majd kattintson a lap alján **frissítések keresése** Windows-frissítések kereséséhez, majd **frissítésektelepítése**. Befejezte a frissítések sikeres telepítése után.

## <a name="install-update-12-on-a-device-that-has-a-gateway-configured-for-a-non-data-0-network-interface"></a>1.2-es frissítés telepítése egy eszközön, amely rendelkezik egy nem DATA 0 hálózati adapterén konfigurált átjáró
Ezt az eljárást kell használni, csak akkor, ha az átjáró ellenőrzése sikertelen, a klasszikus Azure portálon keresztül frissítések telepítése közben. Az ellenőrzés sikertelen, egy nem DATA 0 hálózati adapterén rendelt átjáró és az eszköz 1. frissítés előtti szoftvert verziót futtat. Ha az eszköz nem rendelkezik egy átjáró egy nem DATA 0 hálózati adapterén, frissítheti az eszköz közvetlenül a klasszikus Azure portálon. Lásd: [1.2-es frissítés telepítése a klasszikus Azure portálon keresztül](#install-update-1.2-via-the-azure-classic-portal).

A szoftver verziójával, amely ezzel a módszerrel frissítheti a következők: frissítés 0,1, a frissítés 0,2 és a frissítés 0,3.

> [!IMPORTANT]
> * Ha az eszköz kiadásban (GA) verziója fut, lépjen kapcsolatba [Microsoft Support](storsimple-contact-microsoft-support.md) segítik a frissítést.
> * Ezt az eljárást kell alkalmazni a frissítés 1.2 csak egyszer hajtható végre. A klasszikus Azure portál segítségével soron következő frissítések alkalmazásához.
> 
> 

Ha az eszköz 1 frissítés előtti szoftvert futtat, és állítsa be a hálózati illesztő DATA 0 kivételével átjáró, 1.2-es frissítést is alkalmazhat az alábbi két módon:

* **1. lehetőség**: Töltse le a frissítést, és alkalmazza azt használatával a `Start-HcsHotfix` parancsmag az eszköz a Windows PowerShell felületén. Ez az az ajánlott eljárás. **Ezt a módszert 1.2-es frissítés alkalmazásához, ha az eszköz fut frissítés 1.0 és 1.1-es frissítés.**
* **2. lehetőség**: távolítsa el az átjáró konfigurációját, és a frissítés telepítése közvetlenül a klasszikus Azure portálon.

Minden egyes részletes utasításokat a következő szakaszokban találhatók.

## <a name="option-1-use-windows-powershell-for-storsimple-to-apply-update-12-as-a-hotfix"></a>1. lehetőség: Használja a Windows PowerShell-lel alkalmazni a frissítés 1.2-es gyorsjavítást
Ezt az eljárást kell használni, csak akkor, ha futtatja a frissítési 0,1, 0,2, 0,3 és, hogy az átjáró ellenőrzése nem sikerült a klasszikus Azure portálon elérhető frissítések telepítése közben. Ha kiadásban (GA) szoftvert futtat, akkor segítségért [Microsoft Support](storsimple-contact-microsoft-support.md) az eszköz frissítéséhez.

1.2-es frissítés telepítése egy gyorsjavítás, töltse le, és a következő gyorsjavításokat telepíti:

| Sorrendje | KB | Leírás | Frissítés típusa |
| --- | --- | --- | --- |
| 1 |KB3063418 |Szoftverfrissítés |Rendszeres |
| 2 |KB3043005 |LSI SAS vezérlő frissítés |Rendszeres |
| 3 |KB3063416 |Lemez belső vezérlőprogram |Karbantartás |

Ezzel az eljárással-a frissítés telepítése előtt győződjön meg arról, hogy:

* Mindkét eszközvezérlők online állapotban.

A következő lépésekkel 1.2-es frissítés alkalmazásához. **A frissítések (körülbelül 30 percet szoftverek, 30 perc illesztőprogram, a lemez belső vezérlőprogram 45 percig) befejezéséhez körülbelül 2 órába is telhet.**

[!INCLUDE [storsimple-install-update-option1](../../includes/storsimple-install-update-option1.md)]

## <a name="option-2-use-the-azure-classic-portal-to-apply-update-12-after-removing-the-gateway-configuration"></a>2. lehetőség: A klasszikus Azure portál használatával 1.2-es frissítés alkalmazása után az átjáró beállításainak eltávolítása
Ez az eljárás csak az 1. frissítés előtti szoftvert verzióját futtatja, és állítsa be, mint a DATA 0 hálózati adapteren átjáró StorSimple eszközök vonatkozik. Szüksége lesz, törölje a beállítást a frissítés telepítése előtt.

A frissítés befejezéséhez néhány órát is igénybe vehet. Ha a gazdagépek külön alhálózatokon vannak, az átjáró konfigurációját az iSCSI-felület eltávolítása állásidőt eredményezhettek. Azt javasoljuk, hogy konfigurálja a DATA 0 az iSCSI-adatforgalomhoz a leállás csökkentése érdekében.

Hajtsa végre az alábbi lépések végrehajtásával tiltsa le az átjáró hálózati kapcsolaton, és követően telepítse a frissítést.

[!INCLUDE [storsimple-install-update-option2](../../includes/storsimple-install-update-option2.md)]

[!INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]

## <a name="next-steps"></a>Következő lépések
További információ a [frissítés 1.2-es kiadás](storsimple-update1-release-notes.md).

