---
title: "Azure hálózati figyelőt aaaPacket vizsgálatának |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan toouse hálózati figyelőt tooperform mély Csomagvizsgálat begyűjti a virtuális gép"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 7b907d00-9c35-40f5-a61e-beb7b782276f
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 4aeddcd482edc4df3d63e87b5c4b0788c540850b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="packet-inspection-with-azure-network-watcher"></a>Az Azure hálózati figyelőt Csomagvizsgálat

A szolgáltatással hello csomagok rögzítése a hálózati figyelőt, kezdeményezzen, és rögzíti munkameneteket kezelhessen a portálról hello PowerShell parancssori felület, és szoftveres hello SDK keresztül Azure virtuális gépeken és a REST API-t. Csomagrögzítéssel lehetővé teszi csomagadatok szintű hello információk könnyen használható formátumban igénylő tooaddress helyzeteket. Ingyenesen elérhető eszközök tooinspect hello adatok kihasználva, vizsgálja meg a virtuális gépek tooand küldött kommunikációra, és betekintést nyerhet a hálózati forgalmat. Néhány példa-használati csomag rögzítési adatok többek között: hálózati vagy alkalmazás problémákat vizsgálja, hálózati visszaélés és a behatolás kísérletek észlelése vagy előírásoknak való megfelelés karbantartása. Ebben a cikkben megmutatjuk, hogyan tooopen csomag rögzítési fájlt által biztosított hálózati figyelőt népszerű nyílt forráskódú eszköz használatával. Hogyan toocalculate egy kapcsolat késleltetése azonosítani a rendellenes forgalom, és vizsgálja meg a hálózati statisztika bemutató példák azt is nyújt.

## <a name="before-you-begin"></a>Előkészületek

Ez a cikk a korábban futtatott egy csomagrögzítéssel végighalad az néhány előre konfigurált forgatókönyvet. Ezek a forgatókönyvek a csomagrögzítéssel megtekintésével elérhető képességek mutatják be. Ez a forgatókönyv használ [WireShark](https://www.wireshark.org/) tooinspect hello csomagrögzítéssel.

Ez a forgatókönyv azt feltételezi, hogy a virtuális gépen már futott egy csomagrögzítéssel. hogyan keresse fel a csomagrögzítéssel toocreate toolearn [kezelése csomagrögzítéseket hello portal](network-watcher-packet-capture-manage-portal.md) vagy látogasson el a többi [kezelése Csomagrögzítéseket REST API](network-watcher-packet-capture-manage-rest.md).

## <a name="scenario"></a>Forgatókönyv

Ebben a forgatókönyvben azt:

* A csomagrögzítéssel áttekintése

## <a name="calculate-network-latency"></a>Kiszámítja a hálózati késés

Ebben a forgatókönyvben megmutatjuk, hogyan tooview hello kezdeti oda-vissza időt (RTT) a Transmission Control Protocol (TCP) témakör két végpontok közötti lépett fel.

Ha a TCP-kapcsolat létrejött, hello hello kapcsolatban küldött első három csomagok járjon el a mintát gyakran említett tooas hello háromutas kézfogás. A kézfogás, az eredeti kérést hello ügyfélről és hello kiszolgáló válaszára küldi hello első két csomagok megvizsgálásával azt is kiszámíthatja hello késleltetés, ha létrejött a kapcsolat. Ez a késés hivatkozott tooas hello oda-vissza időt (RTT). Tekintse meg a következő erőforrás toohello hello TCP protokollt és hello háromutas kézfogás további információt. https://support.microsoft.com/en-us/help/172983/Explanation-of-the-three-Way-Handshake-via-TCP-IP

### <a name="step-1"></a>1. lépés

Indítsa el a WireShark

### <a name="step-2"></a>2. lépés

Betöltési hello **.cap** a csomagrögzítéssel a fájlt. Ez a fájl megtalálható hello blob mentik el a helyileg a hello virtuális gép, attól függően, hogyan konfigurálta.

### <a name="step-3"></a>3. lépés

tooview hello kezdeti oda-vissza időt (RTT) TCP témák, azt fogja csak megtekintik hello első két csomagok hello TCP kézfogás részt. Azt fogja használni hello első két csomagok hello háromutas kézfogás, amelyek hello [SZIN], [SZIN, Nyugtázási] csomagok. A TCP-fejlécben hello beállítású néven szerepelnek. utolsó csomag hello hello kézfogás, hello [Nyugtázási] csomagot nem használható ebben a forgatókönyvben. [SZIN] hello csomagok hello ügyfél által küldött. Miután érkezik a hello a kiszolgáló elküldi a hello [Nyugtázási] csomag hello SZIN fogadása hello ügyfél jóváhagyva. Hello tényt, hogy a hello kiszolgáló válaszára csekély terhelés igényel, ami, azt kiszámításához hello [SZIN, Nyugtázási] csomag hello idő [SZIN] csomag fogadta hello ügyfél hello idő kivonásával RTT hello ügyfél által küldött hello.

WireShark használatával Ez az érték kiszámítása nekünk.

toomore könnyen hozzáférhető a hello első két csomagok hello TCP háromutas kézfogás, azt felhasznál hello WireShark által biztosított képesség szűrést.

tooapply hello szűrő WireShark, bontsa ki a hello "Transmission Control Protocol" szegmens egy [SZIN] csomag, a rögzítés, és vizsgálja meg a TCP-fejlécben hello hello beállítású.

Mivel azt keresett toofilter összes [SZIN] és [SZIN, Nyugtázási] csomagok jelzők a cofirm hello szin bitet too1, majd kattintson a jobb gombbal a hello szin bit, a szűrő -> kijelölt -> alkalmaz.

![7. ábra][7]

### <a name="step-4"></a>4. lépés

Most, hogy hello ablak tooonly lásd a csomagok szűrését hello [SZIN] bitje, egyszerűen kiválaszthatja tooview érdekli beszélgetések hello kezdeti RTT. Egy egyszerű módon tooview hello RTT a WireShark egyszerűen kattintson a "SEQ/Nyugtázási" elemzés megjelölve hello legördülő. Ekkor megjelenik a hello RTT jelenik meg. Ebben az esetben a hello RTT 0.0022114 másodperc, vagy 2.211 ms volt.

![8. ábra][8]

## <a name="unwanted-protocols"></a>Nem kívánt protokollok

Akkor is számos olyan alkalmazás, az Azure-ban telepített virtuálisgép-példányon futnak. Ezek az alkalmazások számos kommunikációhoz hello hálózaton, akár az Ön engedélye nélkül. Csomag rögzítési toostore hálózati kommunikációra használja, azt is vizsgálja ki hogyan alkalmazás hello hálózaton beszélünk, és keresse meg az esetleges problémákat.

Ebben a példában egy korábbi tanulmányozzák csomagrögzítéssel nemkívánatos protokollok, amelyek nem hitelesített kommunikáció a számítógépen futó valamelyik alkalmazás futott.

### <a name="step-1"></a>1. lépés

Azonos rögzítése hello előző esetben kattintson a használatával hello **statisztika** > **protokoll hierarchia**

![protokoll hierarchia menü][2]

hello protokoll hierarchia ablak. Ez a nézet az összes hello protokoll hello rögzítési munkamenet és hello csomagok számának, valamint küldött és fogadott hello protokollok használata során használt listáját tartalmazza. Ez a nézet a nem kívánt hálózati forgalmat a virtuális gépek vagy a hálózaton kereséshez hasznos lehet.

![protokoll hierarchia megnyitása][3]

Ahogy az alábbi képernyőfelvétel-készítés hello látja, történt hello BitTorrent protokoll, amely társ toopeer fájlmegosztás használó forgalom. Rendszergazdaként nem várható toosee BitTorrent forgalom az adott virtuális gépeken. Most már tisztában legyen a forgalmat, akkor eltávolíthatja hello társ toopeer szoftver van telepítve a virtuális gép vagy blokk hello forgalom hálózati biztonsági csoportok vagy egy tűzfal kezelését. Emellett előfordulhat, hogy választja toorun csomag rögzítésekre ütemezés szerint, áttekintheti hello protokoll használatát rendszeresen a virtuális gépeken. Hogyan tooautomate hálózati feladatainak azure látogasson el a példa [az azure Automation szolgáltatásban, a hálózati erőforrások figyelése](network-watcher-monitor-with-azure-automation.md)

## <a name="finding-top-destinations-and-ports"></a>Felső a célok és portok keresése

Hello típusú forgalom ismertetése, hello végpontok, és hello portokon keresztül érkezett egy fontos figyelés vagy hibaelhárítási alkalmazások és a hálózati erőforrásokhoz. A fenti csomag rögzítési fájlt használ, azt gyorsan tanul hello legfontosabb célok a virtuális gép kommunikál, és hello kihasználtságának portok.

### <a name="step-1"></a>1. lépés

Használatával hello azonos rögzítése hello előző esetben kattintson a **statisztika** > **IPv4 statisztika** > **a célok és portok**

![csomag rögzítési ablak][4]

### <a name="step-2"></a>2. lépés

Reméljük, hello eredmények keresztül egy vonal jelöli, történt több kapcsolatot 111 porton. hello leggyakrabban használt port lett 3389-es, vagyis a távoli asztal, és hello fennmaradó RPC dinamikus portok.

Ez az adatforgalom azt jelentheti, semmi sem, de napjainkban ismeretlen toohello rendszergazda, és sok kapcsolatokhoz használt portot.

![5. ábra][5]

### <a name="step-3"></a>3. lépés

Most, hogy a hely port kívüli azt észlelte azt a rögzítési hello port alapján szűrhetők.

hello szűrő ebben a forgatókönyvben a következő lesz:

```
tcp.port == 111
```

Azt adja meg a fenti hello szűrő szöveget, hello szűrő szövegmezőben, majd nyomja le adjon meg.

![6. ábra][6]

Hello találatokban láthatja az összes hello forgalom helyi virtuális gépről érkező hello ugyanazon az alhálózaton. Ha még nem tudjuk miért jelentkezik a forgalmat, azt további vizsgálhatja hello csomagok toodetermine miért azt, hogy így hívásokat a 111 porton. Ezt az információt a Microsoft hello megfelelő lépéseket is tarthat.

## <a name="next-steps"></a>Következő lépések

Ismerje meg, körülbelül hello látogasson el a hálózati figyelőt más diagnosztikai funkcióit [Azure hálózatfigyelési áttekintése](network-watcher-monitoring-overview.md)

[1]: ./media/network-watcher-deep-packet-inspection/figure1.png
[2]: ./media/network-watcher-deep-packet-inspection/figure2.png
[3]: ./media/network-watcher-deep-packet-inspection/figure3.png
[4]: ./media/network-watcher-deep-packet-inspection/figure4.png
[5]: ./media/network-watcher-deep-packet-inspection/figure5.png
[6]: ./media/network-watcher-deep-packet-inspection/figure6.png
[7]: ./media/network-watcher-deep-packet-inspection/figure7.png
[8]: ./media/network-watcher-deep-packet-inspection/figure8.png













