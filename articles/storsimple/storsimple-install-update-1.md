---
title: "a StorSimple eszköz frissítése 1.2 aaaInstall |} Microsoft Docs"
description: "Azt ismerteti, hogyan tooinstall a StorSimple 8000 Series Update 1.2 a StorSimple 8000 series eszközön."
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
ms.openlocfilehash: 0a7601dc0b1ce60eb854227243ecb02d6fb2c678
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-update-12-on-your-storsimple-8000-series-device"></a>A StorSimple 8000 series eszközön 1.2-es frissítés telepítése
## <a name="overview"></a>Áttekintés
Ez az oktatóanyag azt ismerteti, hogyan tooinstall frissítése a StorSimple eszközön a szoftver verziója előzetes tooUpdate 1 futtató 1.2. hello oktatóanyag is magában foglalja a hello egy átjárót a hálózati adapteren, mint a DATA 0 hello StorSimple eszköz konfigurálásakor hello frissítéséhez szükséges további lépéseket.

Frissítés 1.2-es eszköz szoftverfrissítések, a LSI illesztőprogram-frissítést és a belső vezérlőprogram-frissítésekre foglalja magában. hello szoftver- és illesztőprogram-frissítést LSI nem zavaró frissítések érhetők el, és alkalmazhatók hello a klasszikus Azure portálon keresztül. hello lemez belső vezérlőprogram-frissítésekre zavaró frissítések érhetők el, és csak akkor érvényesíthetők, hello eszköz hello Windows PowerShell felületén keresztül.

Attól függően, melyik verzióját az eszköz fut, a segítségével meghatározhatja, hogy ha 1.2-es frissítés alkalmazza a rendszer. Az eszköz hello szoftverének verziójával ellenőrizheti a toohello útvonalon **gyors áttekintő** az eszköz szakasza **irányítópult**.

</br>

| Ha a szoftver verziója... | Mi történik, hello portálon? |
| --- | --- |
| Kiadás – GA |Ha Release verzióban (GA) futtatja, nem vonatkoznak a frissítés. Adjon [forduljon a Microsoft Support](storsimple-contact-microsoft-support.md) tooupdate az eszközt. |
| 0.1-es frissítés |Portál 1.2-es frissítés vonatkozik. |
| 0.2-es frissítés |Portál 1.2-es frissítés vonatkozik. |
| 0.3-as frissítés |Portál 1.2-es frissítés vonatkozik. |
| 1. frissítés |A frissítés nem lesz elérhető. |
| 1.1-es frissítés |A frissítés nem lesz elérhető. |

</br>

> [!IMPORTANT]
> * 1.2-es frissítés nem láthatók azonnal, mert a hello frissítések fázisokra bontva történő bevezetéséhez végezzük. A néhány nap múlva újra a frissítések keresését, a frissítés hamarosan elérhető lesz.
> * A frissítés olyan manuális és automatikus előzetes ellenőrzése toodetermine hello Eszközállapot tekintetében a hardver állapotát és a hálózati kapcsolatot tartalmaz. Az előzetes ellenőrzések csak akkor, ha a klasszikus Azure portálon hello hello frissítések telepítését végzik.
> * Azt javasoljuk, hogy hello szoftver telepítése és illesztőprogram-frissítést keresztül hello a klasszikus Azure portálon. Ha hello frissítés előtti átjáró ellenőrzés sikertelen hello portálon toohello Windows PowerShell-felületet hello eszköz (tooinstall frissítések) csak kell lépjen. hello frissítések 5-10 óra tooinstall (beleértve a Windows-frissítések hello) is igénybe vehet. hello eszköz hello Windows PowerShell felületén keresztül hello karbantartási mód frissítéseket kell telepíteni. Karbantartási mód frissítések zavaró frissítések érhetők el, mert ezek jár le egyszerre az eszközhöz.
> 
> 

[!INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-12-via-hello-azure-classic-portal"></a>1.2-es frissítés telepítése hello a klasszikus Azure portálon keresztül
Túl hajtsa végre a következő lépéseket tooupdate hello-az eszköz[1.2-es frissítés](storsimple-update1-release-notes.md). Ezzel az eljárással csak akkor, ha más átjáró is konfigurálva a DATA 0 hálózati adapterén az eszközön.

[!INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

1. Győződjön meg arról, hogy az eszköz fut. **a StorSimple 8000 Series Update 1.2 (6.3.9600.17584)**. Hello **utolsó frissítés dátuma** is módosítani kell. Azt is megtudhatja, hogy a karbantartási mód frissítések érhetők el (Ez az üzenet továbbra is fel too24 óra telepítése után hello frissítések megjelenített toobe).
   
   Karbantartási mód frissítések zavaró frissítések eszköz állásidőt eredményezhettek, és csak akkor érvényesíthetők, az eszköz hello Windows PowerShell felületén keresztül érhetők el.
   
   ![Karbantartási lap](./media/storsimple-install-update-1/InstallUpdate12_10M.png "karbantartási lap")
2. Hello karbantartási mód frissítések letöltése felsorolt hello lépések segítségével [toodownload gyorsjavítások](#to-download-hotfixes) toosearch számára, és töltse le a KB3063416, amely telepíti a belső vezérlőprogram-frissítésekre (hello más frissítések már telepítve kell lennie mostanra).
3. Kövesse a felsorolt hello lépéseket [telepítse, és ellenőrizze a karbantartási mód gyorsjavítások](#to-install-and-verify-maintenance-mode-hotfixes) tooinstall hello karbantartási mód frissítések.
4. A klasszikus Azure portálon hello, keresse meg a toohello **karbantartási** lapon, majd kattintson a lap alján hello hello lap, **frissítések keresése** bármely Windows-frissítések, és kattintson a toocheck **frissítések telepítése** . Befejezte az összes hello a frissítések telepítése sikeresen megtörtént.

## <a name="install-update-12-on-a-device-that-has-a-gateway-configured-for-a-non-data-0-network-interface"></a>1.2-es frissítés telepítése egy eszközön, amely rendelkezik egy nem DATA 0 hálózati adapterén konfigurált átjáró
Ezt az eljárást kell használni, csak akkor, ha hello átjáró ellenőrizze akkor sikertelenek, amikor tooinstall hello frissítések hello a klasszikus Azure portálon keresztül. hello az ellenőrzés sikertelen, a hozzárendelt tooa nem DATA 0 hálózati adapterén átjáró és az eszköz fut-e a szoftver verziója előzetes tooUpdate 1. Ha az eszköz nem rendelkezik egy átjáró egy nem DATA 0 hálózati adapterén, közvetlenül a klasszikus Azure portálon hello az eszköz segítségével frissítheti. Lásd: [1.2-es frissítés telepítése hello a klasszikus Azure portálon keresztül](#install-update-1.2-via-the-azure-classic-portal).

Ezzel a módszerrel frissíthető hello szoftver verziója a következők: frissítés 0,1, frissítés 0,2 és frissítés 0,3.

> [!IMPORTANT]
> * Ha az eszköz kiadásban (GA) verziója fut, lépjen kapcsolatba [Microsoft Support](storsimple-contact-microsoft-support.md) tooassist hello való frissítéséhez.
> * Ez az eljárás igények toobe csak egyszer végre tooapply 1.2-es frissítés. Hello Azure klasszikus portál tooapply soron következő frissítések is használhatja.
> 
> 

Ha az eszköz 1 frissítés előtti szoftvert futtat, és állítsa be a hálózati illesztő DATA 0 kivételével átjáró, a következő két módszer hello 1.2-es frissítést is alkalmazhat:

* **1. lehetőség**: hello frissítés letöltése, és alkalmazza azt hello segítségével `Start-HcsHotfix` parancsmag hello Windows PowerShell felületén hello eszköz. Ez az ajánlott módszer hello. **Ne használjon a metódus tooapply 1.2-es frissítés, ha az eszköz fut frissítés 1.0 és 1.1-es frissítés.**
* **2. lehetőség**: eltávolítása hello átjáró konfigurálása és telepítése hello frissítéshez közvetlenül hello a klasszikus Azure portálon.

Minden egyes részletes utasításokat végrehajtva a következő részekben hello.

## <a name="option-1-use-windows-powershell-for-storsimple-tooapply-update-12-as-a-hotfix"></a>1. lehetőség: A Windows Powershellt használja a StorSimple tooapply frissítés 1.2-es gyorsjavítást
Ezt az eljárást kell használni, csak akkor, ha futtatja a frissítési 0,1, 0,2, 0,3 és, hogy az átjáró ellenőrzése nem sikerült a klasszikus Azure portálon hello tooinstall frissítések tett kísérlet során. Ha kiadásban (GA) szoftvert futtat, akkor segítségért [Microsoft Support](storsimple-contact-microsoft-support.md) tooupdate az eszközt.

tooinstall frissítés 1.2-es gyorsjavítást, meg kell töltse le és telepítse a következő gyorsjavításokat hello:

| Sorrendje | KB | Leírás | Frissítés típusa |
| --- | --- | --- | --- |
| 1 |KB3063418 |Szoftverfrissítés |Rendszeres |
| 2 |KB3043005 |LSI SAS vezérlő frissítés |Rendszeres |
| 3 |KB3063416 |Lemez belső vezérlőprogram |Karbantartás |

Ez az eljárás tooapply hello használata előtt frissíteni, győződjön meg arról, hogy:

* Mindkét eszközvezérlők online állapotban.

Hajtsa végre a következő lépéseket tooapply 1.2-es frissítés hello. **hello frissítések körülbelül 2 óra toocomplete (körülbelül 30 percet szoftverek, 30 perc illesztőprogram, a lemez belső vezérlőprogram 45 percig) is beletelhet.**

[!INCLUDE [storsimple-install-update-option1](../../includes/storsimple-install-update-option1.md)]

## <a name="option-2-use-hello-azure-classic-portal-tooapply-update-12-after-removing-hello-gateway-configuration"></a>2. lehetőség: Hello Azure klasszikus portál tooapply 1.2-es frissítés használata hello átjáró konfigurációs eltávolítása után
Ez az eljárás csak, amely a szoftver verziója előzetes tooUpdate 1 futnak, és állítsa be, mint a DATA 0 hálózati adapteren átjáró tooStorSimple eszközökre vonatkozik. Szüksége lesz tooclear hello átjáró előzetes tooapplying hello frissítését.

hello frissítés is igénybe vehet néhány óra toocomplete. Ha külön alhálózatokon vannak a gazdagépek, hello átjáró beállításainak hello iSCSI felületek eltávolítása állásidőt eredményezhettek. Azt javasoljuk, hogy az iSCSI forgalom tooreduce hello állásidő konfigurálhat a DATA 0.

Hajtsa végre a következő lépéseket toodisable hello hálózati illesztő – hello átjáró hello, és ezután alkalmazza az hello frissítést.

[!INCLUDE [storsimple-install-update-option2](../../includes/storsimple-install-update-option2.md)]

[!INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]

## <a name="next-steps"></a>Következő lépések
További tudnivalók hello [frissítés 1.2-es kiadás](storsimple-update1-release-notes.md).

