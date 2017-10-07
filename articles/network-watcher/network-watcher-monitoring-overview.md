---
title: "Hálózati figyelőt aaaIntroduction tooAzure |} Microsoft Docs"
description: "Ezen a lapon hello hálózati figyelőt szolgáltatás Figyelés áttekintése és hálózati megjelenítése kapcsolódó erőforrások az Azure-ban"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 14bc2266-99e3-42a2-8d19-bd7257fec35e
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/11/2017
ms.author: gwallace
ms.openlocfilehash: 283b3fa6add05d9bad6d5dbdae1524344d1bfc7a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-network-monitoring-overview"></a>Azure-hálózat – áttekintés

Az ügyfelek egy Azure-végpontok közötti hálózati koordinálása, és különböző egyes hálózati erőforrások, például virtuális hálózaton, ExpressRoute, Alkalmazásátjáró, terheléselosztók, és több összeállítása hozhat létre. Figyelés a hello hálózati erőforrások mindegyikének érhető el. Irányítjuk toothis "figyelés" erőforrás-szintű figyelése.

hello end tooend hálózati összetett konfigurációk és erőforrások létrehozása összetett forgatókönyvek esetében, forgatókönyv-alapú hálózati figyelőt figyeléshez igénylő közötti interakció rendelkezhet.

Ez a cikk ismerteti a forgatókönyv és erőforrás-szintű figyelése. Hálózatfigyelés az Azure-ban átfogó, és ismerteti a két kategóriába sorolhatók:

* [**Hálózati figyelő** ](#network-watcher) -forgatókönyv-alapú figyelési által biztosított hello szolgáltatások a hálózati figyelőt. A szolgáltatás része a csomagrögzítéssel, a következő ugrás, az IP-adatfolyam győződjön meg arról, biztonsági csoport megtekintése, NSG folyamata naplókat. Forgatókönyv szintű figyelési jeleníti meg egy záró tooend ezzel szemben tooindividual hálózati erőforrás figyelési hálózati erőforrásokhoz.
* [**Erőforrás-figyelés** ](#network-resource-level-monitoring) -erőforrás-szintű figyelés négy szolgáltatások diagnosztikai naplók, metrikák, hibaelhárítási és erőforrás állapota áll. Ezek a szolgáltatások beépített hello hálózati erőforrás szinten.

## <a name="network-watcher"></a>Network Watcher

Hálózati figyelőt regionális szolgáltatás, amely lehetővé teszi, hogy Ön toomonitor és diagnosztizálásához szintjén feltételek egy hálózati forgatókönyv, hogy, és az Azure-ból. Hálózati diagnosztikai és a képi megjelenítés eszközök is elérhetők a hálózati figyelőt segítenek megérteni, diagnosztizálása és szerezhet insights tooyour hálózati az Azure-ban.

Hálózati figyelőt a következő képességeket hello van:

* **[Topológia](network-watcher-topology-overview.md)**  -biztosít a hálózati szintű áttekintés ábrázoló hello különböző csatlakozás és egy erőforráscsoportban található hálózati erőforrások egymáshoz rendelését.
* **[Változó csomagrögzítéssel](network-watcher-packet-capture-overview.md)**  -csomagadatok mindkét virtuális gép rögzíti. A speciális szűrési lehetőségek és finomhangolható vezérlők, például képes tooset idő alatt, és méretkorlátai indít. hello csomagok adatok tárolhatók a blob-tárolóban, vagy a helyi lemezen hello .cap formátumban.
* **[IP-adatfolyam ellenőrizze](network-watcher-ip-flow-verify-overview.md)**  -ellenőrzést, ha egy csomag engedélyezett vagy megtagadott folyamata adatokat 5 rekordos csomag paraméterek (cél IP-címe, forrás IP-címe, Célport, Forrásport és protokoll) alapján. Ha egy biztonsági csoportot megtagadta a hello csomagot, hello szabály és a csoportot, amely hello csomagok megtagadva ad vissza.
* **[Következő Ugrás](network-watcher-next-hop-overview.md)**  -hello a következő ugrás a csomagok irányítása a hello Azure hálózati háló, amely lehetővé teszi, toodiagnose minden helytelenül konfigurált felhasználó által definiált útvonalak határozza meg.
* **[Biztonsági csoport megtekintése](network-watcher-security-group-view-overview.md)**  -hello hatékony és alkalmazott szabályokat, amelyek érvényesek a virtuális gép lekérdezi.
* **[NSG Flow naplózási](network-watcher-nsg-flow-logging-overview.md)**  -folyamat a hálózati biztonsági csoportok naplóiban toocapture naplók kapcsolódó tootraffic, amelyek számára engedélyezett vagy megtagadott hello szabályok hello csoportban. hello folyamata 5 rekordos információkat – forrás IP-cím, a cél IP-cím, a forrásport, a célport és a protokoll határozzák meg.
* **[Virtuális hálózati átjáró és a kapcsolat hibaelhárítási](network-watcher-troubleshoot-manage-rest.md)**  -hello képességét tootroubleshoot biztosít virtuális hálózati átjárók és kapcsolatok.
* **[Előfizetési korlátozásait a hálózati](#network-subscription-limits)**  -lehetővé teszi tooview hálózati erőforrás-használati korlátozások.
* **[Diagnosztikai naplófájl konfigurálása](#diagnostic-logs)**  – lehetővé teszi egy egytáblás tooenable, vagy tiltsa le a diagnosztikai naplókat a hálózati erőforrások az erőforráscsoportban.
* **[A kapcsolat (előzetes verzió)](network-watcher-connectivity-overview.md)**  -hello létrehozásának lehetősége a virtuális gép tooa megadott végpont a közvetlen TCP-kapcsolatot ellenőrzi.

### <a name="role-based-access-control-rbac-in-network-watcher"></a>Szerepköralapú hozzáférés-vezérlés (RBAC) hálózati figyelőt

Hálózati figyelőt használ hello [átruházásához hozzáférés-vezérlés (RBAC) modell](../active-directory/role-based-access-control-what-is.md). hello hálózati figyelőt hello alábbi engedélyek szükségesek. Fontos, hogy a hálózati figyelő API-k kezdeményezése vagy hálózati figyelőt használatával hello portálról használt hello szerepkörhöz szükséges hello hozzáfér toomake.

|Erőforrás| Engedély|
|---|---| 
|Microsoft.Storage/ |Olvasás|
|Microsoft.Authorization/| Olvasás| 
|Microsoft.Resources/subscriptions/resourceGroups/| Olvasás|
|Microsoft.Storage/storageAccounts/listServiceSas/ | Műveletek|
|Microsoft.Storage/storageAccounts/listAccountSas/ |Műveletek|
|Microsoft.Storage/storageAccounts/listKeys/ | Műveletek|
|Microsoft.Compute/virtualMachines/ |Olvasás|
|Microsoft.Compute/virtualMachines/ |Írás|
|Microsoft.Compute/virtualMachineScaleSets/ |Olvasás|
|Microsoft.Compute/virtualMachineScaleSets/ |Írás|
|Microsoft.Network/networkWatchers/packetCaptures/ |Olvasás|
|Microsoft.Network/networkWatchers/packetCaptures/| Írás|
|Microsoft.Network/networkWatchers/packetCaptures/| Törlés|
|Microsoft.Network/networkWatchers/ |Írás |
|Microsoft.Network/networkWatchers/| Olvasás |
|Microsoft.Insights/alertRules/ |*|
|Microsoft.Support/ | *|

### <a name="network-subscription-limits"></a>Hálózati előfizetési korlátozásait

Hálózati előfizetési korlátozásait biztosít hello használata hello hálózati erőforrás hello maximális számát a rendelkezésre álló erőforrások elleni régióban előfizetés részleteit.

![hálózati előfizetési korlátját][nsl]

## <a name="network-resource-level-monitoring"></a>Hálózati erőforrás szolgáltatásiszint-figyelés

a következő funkciók hello erőforrás szintű figyelés érhetők el:

### <a name="audit-log"></a>Napló

Hálózatok hello konfiguráció részeként műveleteket a rendszer naplózza. Ezek a naplók megtekinthetők az Azure-portálon hello, vagy visszavonni a Microsoft eszközök, például a Power BI és a külső eszközök használatával. Naplók hello portálon, a PowerShell, a CLI és a Rest API-n keresztül érhetők el. A naplókat további információkért lásd: [naplózási műveletek a Resource Manager](../resource-group-audit.md)

Naplók az összes hálózati erőforrás végzett műveletek érhetők el.

### <a name="metrics"></a>Mérőszámok

Adatok gyűjtése le TELJESÍTMÉNYMÉRÉSEK és egy meghatározott időtartamra vonatkozóan gyűjtött adatait. Adatok gyűjtése le jelenleg elérhető az Alkalmazásátjáró. Metrikák használt tootrigger riasztások küszöbérték alapján lehet. Lásd: [átjáró Alkalmazásdiagnosztika](../application-gateway/application-gateway-diagnostics.md) tooview hogyan metrikák használt toocreate riasztásokat is lehet.

![metrikák megtekintése][metrics]

### <a name="diagnostic-logs"></a>Diagnosztikai naplók

Rendszeres és önkéntes események hálózati erőforrások által létrehozott, és a storage-fiókok, az Event Hubs vagy Naplóelemzési tooan küldött bejelentkezve. Ezek a naplók erőforrás állapotának hello betekintést. Ezek a naplók eszközöket, például a Power BI és a Naplóelemzési tekintheti meg. Hogyan tooview diagnosztikai naplók, látogasson el toolearn [Naplóelemzési](../log-analytics/log-analytics-azure-networking-analytics.md).

Diagnosztikai naplók érhetők el [terheléselosztó](../load-balancer/load-balancer-monitor-log.md), [hálózati biztonsági csoportok](../virtual-network/virtual-network-nsg-manage-log.md), útvonalakat, és [Alkalmazásátjáró](../application-gateway/application-gateway-diagnostics.md).

Hálózati figyelőt biztosít a diagnosztikai naplók megtekintése. Ez a nézet az összes hálózati erőforrások, amelyek támogatják a diagnosztikai naplózás tartalmazza. Ebben a nézetben engedélyezése, és tiltsa le a hálózati erőforrások egyszerűen és gyorsan.

![naplók][logs]

### <a name="troubleshooting"></a>Hibaelhárítás

hibaelhárítás panelen hello portálon élményt hello valósul meg a hálózati erőforrásokat ma toodiagnose egyedi erőforrás társított gyakori problémákat. A forgatókönyvben a következő hálózati erőforrások - ExpressRoute, VPN-átjáró, Alkalmazásátjáró, hálózati biztonsági naplókat, útvonalakat, DNS, Load Balancer és Traffic Manager hello érhető el. További információ az erőforrás szintű hibaelhárítása, toolearn látogasson el [Azure hibaelhárítási diagnosztizálása és megoldása problémái](https://azure.microsoft.com/blog/azure-troubleshoot-diagonse-resolve-issues/)

![hibaelhárítási információ][TS]

### <a name="resource-health"></a>Erőforrás állapota

a hálózati erőforrás állapotának hello rendszeres időközönként valósul meg. Ilyen erőforrások többek között a VPN-átjáró és a VPN-alagúton. Erőforrás állapota az Azure-portálon hello érhető el. További információ az erőforrás állapota toolearn látogasson el [erőforrás állapotának áttekintése](../resource-health/resource-health-overview.md)

## <a name="next-steps"></a>Következő lépések

Hálózati figyelőt megismerését követően megtanulhatja:

Látogasson el a virtuális gépen a csomagrögzítéssel tegye [hello Azure-portál a változó csomagrögzítéssel](network-watcher-packet-capture-manage-portal.md)

Hajtsa végre a proaktív figyeléshez és diagnosztika használatával [figyelmeztetés csomagrögzítéssel](network-watcher-alert-triggered-packet-capture.md).

A biztonsági rések észlelése [elemzése a Wireshark csomagrögzítéssel](network-watcher-deep-packet-inspection.md), nyílt forráskódú eszközökkel.

Megismerhet néhány hello más kulcs [hálózati lehetőségeket](../networking/networking-overview.md) Azure.

<!--Image references-->
[TS]: ./media/network-watcher-monitoring-overview/troubleshooting.png
[logs]: ./media/network-watcher-monitoring-overview/logs.png
[metrics]: ./media/network-watcher-monitoring-overview/metrics.png
[nsl]: ./media/network-watcher-monitoring-overview/nsl.png











