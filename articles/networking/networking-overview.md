---
title: "aaaAzure hálózatkezelés |} Microsoft Docs"
description: "További tudnivalók az Azure hálózati szolgáltatásokat és képességeket."
services: networking
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/19/2017
ms.author: jdial
ms.openlocfilehash: 18945d139427f2e65138c0fd223e663fa46e9211
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-networking"></a>Az Azure hálózatkezelés

Azure biztosít a különböző hálózati képességekkel, amelyek együtt vagy külön-külön is használható. Kattintson a következő további velük kapcsolatos főbb funkciók toolearn hello bármelyikét:
- [Azure-erőforrások közötti kapcsolat](#connectivity): csatlakozás Azure-erőforrások együtt hello felhőben biztonságos, titkos virtuális hálózaton.
- [Internetkapcsolat](#internet-connectivity): hello Internet protokollt használó kommunikációra tooand az Azure-erőforrások.
- [A helyi kapcsolat](#on-premises-connectivity): egy a helyszíni hálózati tooAzure erőforrások csatlakozzon a virtuális magánhálózat (VPN) hello interneten keresztül, akár egy dedikált kapcsolat tooAzure módon.
- [Terheléselosztás és a forgalom iránya betöltése](#load-balancing): Load balance forgalom tooservers a hello ugyanarra a helyre, és közvetlen forgalom tooservers különböző helyeken.
- [Biztonsági](#security): alhálózatok vagy egyedi virtuális gépeket (VM) közötti hálózati forgalom szűrésére.
- [Útválasztás](#routing): használja az alapértelmezett útválasztási vagy teljes irányítása az Azure és a helyszíni erőforrások között.
- [Kezelhetőségi](#manageability): a figyelő és az Azure hálózati erőforrások kezeléséhez.
- [Üzembe helyezési és konfigurációs eszközök](#tools): egy webes portál vagy a platformfüggetlen parancssori eszközök toodeploy használja, és konfigurálja a hálózati erőforrásokhoz.

## <a name="Connectivity"></a>Azure-erőforrások közötti kapcsolat

Azure-erőforrások, például a virtuális gépek, a Cloud Services, a virtuális gépek méretezési csoportok és az Azure App Service Environment-környezetek is közvetlenül egymással kommunikálnak az Azure Virtual Network (VNet) keresztül. A virtuális hálózat hello Azure felhőben dedikált tooyour logikai elkülönítése [előfizetés](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fnetworking%2ftoc.json). Megvalósíthat több Vnetek belül minden Azure-előfizetés és Azure [régió](https://azure.microsoft.com/regions). Minden egyes virtuális hálózat el különítve a más Vnetekről. Minden egyes vnet a következő műveletek végezhetők el:

- Adjon meg egy egyéni privát IP-címtér nyilvános és titkos (az RFC 1918) címeket használnak. Azure rendeli hozzá az erőforrások csatlakoztatott toohello VNet egy magánhálózati IP-címet rendel hello címterület.
- Hello VNet be egy vagy több alhálózatból szegmentálni, és hello VNet terület tooeach címalhálózata része lefoglalni.
- Azure által biztosított névfeloldás használata, vagy adja meg az erőforrások használhatják a saját DNS-kiszolgáló csatlakoztatva tooa virtuális hálózat.

További információk hello Azure Virtual Network service, olvassa el a hello toolearn [virtuális hálózat áttekintése](../virtual-network/virtual-networks-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) cikk. Csatlakozhat a más Vnetekről tooeach, erőforrások engedélyezése tooeither VNet toocommunicate egymással keresztül kapcsolódó Vnetek. Valamelyike vagy mindegyike a következő beállítások tooconnect Vnetek tooeach más hello használhatja:

- **Társviszony-létesítés:** lehetővé teszi, hogy erőforrásokat csatlakoztatott toodifferent Azure Vnetekhez hello belül azonos Azure-régiót toocommunicate egymással. hello sávszélesség és a késleltetés között Vnetek van hello hello ugyanaz, mintha hello erőforrások csatlakoztatott toohello ugyanazt a virtuális hálózatot. toolearn társviszony-létesítést, kapcsolatos további információkért olvassa el a hello [virtuális hálózati társviszony-létesítési áttekintése](../virtual-network/virtual-network-peering-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) cikk.
- **VPN-átjáró:** lehetővé teszi, hogy erőforrásokat toodifferent Azure Vnetekhez különböző Azure-régiók toocommunicate belül kapcsolódnak egymáshoz. Vnetek közötti forgalmat egy Azure VPN-átjárón keresztül. Közötti Vnetek sávszélessége korlátozott toohello sávszélesség hello átjáró. További információk a Vneteket összekötő VPN-átjáró, olvassa el a hello toolearn [konfigurálja a VNet – VNet-kapcsolatot régiók](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md?toc=%2fazure%2fnetworking%2ftoc.json) cikk.

## <a name="internet-connectivity"></a>Internetkapcsolat

Tooa VNet összes Azure-erőforrások csatlakoztatott kimenő kapcsolat toohello Internet alapértelmezés szerint rendelkezik. hello erőforrás hello magánhálózati IP-címe a forrás hálózati címe (SNAT) tooa nyilvános IP-cím fordította hello Azure-infrastruktúra. További információk a kimenő internetkapcsolat működését, olvassa el a hello toolearn [ismertetése az Azure-ban kimenő kapcsolatok](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fnetworking%2ftoc.json) cikk.

toocommunicate bejövő hello Internet, illetve toocommunicate kimenő toohello Internet nélkül SNAT, egy erőforrást egy nyilvános IP-cím rendelendő tooAzure erőforrásokat. További információ a nyilvános IP-címek, olvassa el a hello toolearn [nyilvános IP-címek](../virtual-network/virtual-network-public-ip-address.md?toc=%2fazure%2fnetworking%2ftoc.json) cikk.

## <a name="on-premises-connectivity"></a>A helyi kapcsolat

Keresztül elérhető erőforrásokhoz a virtuális hálózat biztonságos VPN-kapcsolatot, és a közvetlen kapcsolatot. az Azure virtuális hálózat és a helyszíni hálózat között toosend a hálózati forgalom, létre kell hoznia a virtuális hálózati átjáró. Hello átjáró toocreate hello kapcsolat típusát, amelyet, VPN vagy ExpressRoute beállításainak konfigurálása.

Csatlakozhat a helyszíni hálózati tooa virtuális hálózatot az alábbi beállítások hello tetszőleges kombinációját használja:

**Pont-pont (keresztül az SSTP VPN)**

a következő kép hello külön pont toosite kapcsolat több számítógép és egy virtuális hálózat között mutatja:

![Pont–hely kapcsolat](./media/networking-overview/point-to-site.png)

Ez a kapcsolat létrejön egy egyedi számítógép és egy virtuális hálózat között. A kapcsolat típusa nem nagyszerű, ha Ön most csak az első lépések az Azure-ral, vagy a fejlesztők számára, mert azt igényli, hogy kevéssé vagy egyáltalán ne módosítások meglévő tooyour-hálózati. Célszerű is kényelmesen használható, ha csatlakozik egy távoli helyre, például egy konferenciaterem vagy otthoni. Pont – hely kapcsolatok gyakran alapján kialakulhat egy pont-pont kapcsolaton keresztül hello ugyanazon virtuális hálózati átjáró. hello kapcsolat hello Internet hello és hello VNet között hello SSTP protokollal tooprovide titkosított kommunikációt használ. pont-pont típusú VPN hello késés is előre nem látható, mivel hello a forgalom halad át hello Internet.

**Pont-pont (IPsec/IKE VPN-alagút)**

![Helyek közötti kapcsolat](./media/networking-overview/site-to-site.png)

Ezt a kapcsolatot a helyszíni VPN-eszköz és az Azure VPN-átjáró között. A kapcsolat típusa lehetővé teszi, hogy az összes helyszíni erőforrás, hogy engedélyezi-e, hogy az tooaccess hello virtuális hálózat. hello kapcsolat az IPSec/IKE VPN, amely titkosított kommunikáció keresztül hello Internet hello Azure VPN gateway a helyszíni eszközök között. Csatlakozhat a helyszíni helyek több toohello azonos VPN-átjáró. hello a helyszíni VPN-eszköz az egyes helyeken kívülről elérhető nyilvános IP-címnek, amely nincs a NAT mögött kell rendelkeznie. a pont-pont kapcsolat hello késés is előre nem látható, mivel hello a forgalom halad át hello Internet.

**Az ExpressRoute (dedikált magánhálózati kapcsolat)**

![ExpressRoute](./media/networking-overview/expressroute.png)

Ilyen típusú kapcsolat hoznak létre a hálózat és az Azure-ban egy ExpressRoute-partner keresztül. Ezt a kapcsolatot a sajátja. Forgalom nem járja hello Internet. az ExpressRoute-kapcsolat hello késés is előre jelezhető, mivel a forgalom nem bejárása hello Internet. ExpressRoute össze lehet kombinálni webhelyek kapcsolatot.

További információk az összes hello előző kapcsolati beállítások, olvassa el a hello toolearn [kapcsolat topológiai diagramot](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fnetworking%2ftoc.json) cikk.

## <a name="load-balancing"></a>Terheléselosztás és a forgalom iránya betöltése

A Microsoft Azure több kezelése a hálózati adatforgalom elosztása milyen szolgáltatásokat és elosztott terhelésű biztosít. Hello külön-külön vagy együtt a következő lehetőségek bármelyikét használhatja:

**DNS terheléselosztás**

hello Azure Traffic Manager szolgáltatás a globális DNS terheléselosztás biztosít. A TRAFFIC Manager válaszol tooclients hello IP-címével a kifogástalan állapotú végpontok, a következő útválasztási metódusait hello valamelyike alapján:
- **Földrajzi:** ügyfelek van irányítva toospecific végpontok (Azure, külső vagy beágyazott) alapú mely földrajzi helyen található, a DNS-lekérdezés származik. Ez a módszer lehetővé teszi, hogy a forgatókönyvek, ahol egy ügyfél a földrajzi régióban ismerete, és a velük alapján, fontos. Például megfelel az adatok közös joghatóság alá megbízás honosítása tartalom & felhasználói élményt, és a különböző régiókban érkező forgalmat méri.
- **Teljesítmény:** hello IP-cím visszaadott toohello ügyfél hello "legközelebbi" toohello ügyfél. hello "legközelebbi" végpont nincs feltétlenül legközelebbi földrajzi távolság mérve. Ehelyett ez a módszer hálózati késés mérésével hello legközelebbi végpont határozza meg. A TRAFFIC Manager egy Internet késés tábla tootrack hello körbejárási idő közötti IP-címtartományok és minden Azure-adatközpontban tart fenn.
- **Prioritás:** akkor irányított toohello elsődleges (legnagyobb prioritás) végpont. Hello elsődleges végpont nem érhető el, ha a Traffic Manager útvonalak hello forgalom toohello második végpontnak. Ha mindkét hello elsődleges és másodlagos végpont nem érhetők el, a hello forgalom halad toohello, harmadik, és így tovább. Hello végpont rendelkezésre állását konfigurált hello állapota (engedélyezett vagy tiltott) azon alapul, és folyamatban lévő végpontmonitoring kijelző hello.
- **Súlyozott ciklikus multiplexelés:** az egyes kérelmek Traffic Manager véletlenszerűen választ egy elérhető végpontot. hello valószínűség kiválasztása a végpont tooall számára elérhető végpontok hozzárendelt hello súlyok alapul. Azonos súlyozási hello segítségével is forgalomeloszlás eredményez az összes végpontok között. A meghatározott végpontokhoz magasabb vagy alacsonyabb súlyok használatával hatására az adott vissza ritkább vagy gyakoribb hello DNS-válaszok végpontok toobe.

a következő kép hello egy webes alkalmazás felé irányuló tooa webalkalmazás végpont kérelmet jeleníti meg. Végpontok más Azure-szolgáltatásokkal például a virtuális gépek és Felhőszolgáltatások is lehet.

![Traffic Manager](./media/networking-overview/traffic-manager.png)

hello ügyfél közvetlenül kapcsolódik toothat végpont. Az Azure Traffic Manager észleli, ha a végpont nem kifogástalan, és ezután átirányítja az ügyfelek tooa másik, a megfelelő végpont. a Traffic Manager hello olvasható további toolearn [Azure Traffic Manager – áttekintés](../traffic-manager/traffic-manager-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) cikk.

**Alkalmazásterhelés-elosztás**

hello Azure Application Gateway szolgáltatás alkalmazás kézbesítési vezérlő (LÉPETT) biztosít szolgáltatásként. Alkalmazásátjáró biztosít az alkalmazások, beleértve a webes alkalmazás különböző réteg 7 (HTTP/HTTPS) terheléselosztási képességeit tűzfal tooprotect a webalkalmazások biztonsági réseket és biztonsági réseket. Alkalmazásátjáró is lehetővé teszi toooptimize webkiszolgáló farm termelékenység kiszervezésével processzorigényes SSL-lezárást toohello Alkalmazásátjáró. 

Más réteg 7 útválasztási lehetőségek közé tartozik a ciklikus multiplexelés bejövő forgalmat, a munkamenet cookie-alapú kapcsolat, a URL-cím elérési út-alapú útválasztási és hello képességét toohost mögött egyetlen alkalmazás átjáró több webhelyek. Alkalmazásátjáró konfigurálhat egy internetes átjárót, egy csak belső-átjáró vagy mindkettőt. Alkalmazásátjáró teljesen Azure felügyelt, méretezhető és magas rendelkezésre állású. Diagnosztikai és naplózási képességek széles skáláját biztosítja a jobb kezelhetőség érdekében. További információk az Alkalmazásátjáró, olvassa el a hello toolearn [Alkalmazásátjáró áttekintése](../application-gateway/application-gateway-introduction.md?toc=%2fazure%2fnetworking%2ftoc.json) cikk.

hello alábbi képen látható URL-cím elérési út-alapú alkalmazás átjáróval útválasztás:

![Application Gateway](./media/networking-overview/application-gateway.png)

**Hálózati terheléselosztás**

hello Azure terheléselosztó biztosít a nagy teljesítményű, alacsony késésű réteg 4 terheléselosztás összes UDP- és TCP protokollt. Bejövő és kimenő kapcsolatok kezeli. Nyilvános és belső elosztott terhelésű végpont konfigurálhatja. Adhat meg szabályokat toomap kapcsolatok tooback-a befejezési készlet célok bejövő TCP- és HTTP állapot-ellenőrzés beállítások toomanage szolgáltatás rendelkezésre állása használatával. További információk a terheléselosztóhoz, olvassa el a hello toolearn [Load Balancer áttekintése](../load-balancer/load-balancer-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) cikk.

a következő kép hello mindkét külső és belső terheléselosztók használja az Internet felé néző többrétegű alkalmazást jeleníti meg:

![Terheléselosztó](./media/networking-overview/load-balancer.png)

## <a name="security"></a>Biztonsági

Az alábbi beállítások hello használata Azure-erőforrások forgalom tooand szűrheti:

- **Hálózati:** Megvalósíthat Azure hálózati biztonsági csoportokkal (NSG-k) toofilter bejövő és kimenő forgalom tooAzure erőforrásokat. Minden NSG tartalmaz egy vagy több bejövő és kimenő szabályokat. Minden egyes szabály azt határozza meg, hello forrás IP-címek, cél IP-címek, port és protokoll, amely a forgalom szűrve van. NSG-ket lehet alkalmazott tooindividual alhálózatokat és az egyes virtuális gépeken. További információk az NSG-k, olvassa el a hello toolearn [hálózati biztonsági csoportok – áttekintés](../virtual-network/virtual-networks-nsg.md?toc=%2fazure%2fnetworking%2ftoc.json) cikk.
- **Alkalmazás:** webalkalmazási tűzfal Alkalmazásátjáró segítségével megvédheti a webalkalmazások biztonsági réseket és biztonsági réseket. Közös többek között az SQL kihasználó közötti helyközi és hibás fejléceket. Alkalmazásátjáró kiszűri a forgalmat, és leállítja a webkiszolgálók elérése. Rendszer képes tooconfigure, milyen szabályok szeretne engedélyezni. hello képességét tooconfigure SSL egyeztetési házirend biztosított tooallow egyes házirendek toobe le van tiltva. További információk hello webalkalmazási tűzfal, olvassa el a hello toolearn [webalkalmazási tűzfal](../application-gateway/application-gateway-web-application-firewall-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) cikk.

Ha módosítania kell az Azure nem adja meg, vagy nem szeretné, hogy toouse hálózati alkalmazások használhatja a helyszíni hálózati képességet, hello termékek megvalósítását a virtuális gépek, és csatlakoztassa őket tooyour virtuális hálózat. Hello [Azure piactér](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?page=1&subcategories=appliances) előre konfigurálva van, előfordulhat, hogy a jelenleg használt hálózati alkalmazások számos különböző virtuális gépek tartalmazza. Ezek előre konfigurált virtuális gépek általában hivatkozott tooas hálózati virtuális készülékek (NVA). Az alkalmazások, például a tűzfal és a WAN-optimalizálást NVAs érhetők el.

## <a name="routing"></a>Útválasztás

Az Azure létrehoz alapértelmezett útvonaltábláit, amelyek lehetővé teszik az erőforrások csatlakoztatott tooany alhálózatának bármely virtuális hálózat toocommunicate egymással. Megvalósíthat vagy, vagy mindkét következő útvonalak toooverride típusú hello hello alapértelmezett útvonalak Azure-hoz:
- **Felhasználó által definiált:** hozhat létre egyéni útvonaltáblák útvonalat, ahol a forgalmat az irányított toofor szabályozó az egyes alhálózatokon. További információk a felhasználó által definiált útvonalak, olvassa el a hello toolearn [felhasználó által definiált útvonalak](../virtual-network/virtual-networks-udr-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) cikk.
- **A border gateway protocol (BGP):** Ha a virtuális hálózat tooyour a helyszíni hálózat az Azure VPN-átjáró vagy ExpressRoute kapcsolattal csatlakozik, a BGP-útvonalak tooyour Vnetek kerülhet. A BGP egy szabványos útválasztási protokoll hello általánosan használt az hello Internet tooexchange az Útválasztás és reachability information legalább két hálózat között. Azure virtuális hálózatok, a BGP lehetővé teszi, hogy hello Azure VPN Gatewayek és a helyszíni VPN-eszközök hello környezetben való használata esetén a hívott BGP állomásokhoz vagy szomszédok tooexchange "irányítja a", amely tájékoztatja mindkét átjárókat a hello rendelkezésre állását és az adott előtagok elérhetőség toogo hello átjárók és az érintett útválasztók keresztül. BGP is engedélyezhető tranzit útválasztást több hálózat között útvonalak BGP átjáró egy BGP-társ tooall való megtanulja propagálására más BGP-társak. toolearn BGP, kapcsolatos további információkért lásd: hello [BGP Azure VPN Gatewayek – áttekintés](../vpn-gateway/vpn-gateway-bgp-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) cikk.

## <a name="manageability"></a>Kezelhetőség

Azure hello következő toomonitor eszközökhöz, és kezelheti a hálózat biztosítja:
- **Tevékenységi naplóit:** összes Azure-erőforrások tevékenységi naplóit, amelyek információval szolgálnak végrehajtott műveletek kell elhelyezni, műveletek állapotának, és akik hello műveletet kezdeményezett. További információk a tevékenységi naplóit, olvassa el a hello toolearn [tevékenység naplózza áttekintése](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md?toc=%2fazure%2fnetworking%2ftoc.json) cikk.
- **Diagnosztikai naplók:** időszakos és önkéntes események hálózati erőforrások által létrehozott és bejelentkezett az Azure storage-fiókok, tooan Azure Event Hubs küldött, vagy tooAzure Naplóelemzési küldött. Diagnosztikai naplók erőforrás insight toohello állapotát adja meg. Diagnosztikai naplók terheléselosztó (internetről hozzáférhető), a hálózati biztonsági csoportok, a útvonalak és az Application Gateway-okat. További információk a diagnosztikai naplók, olvassa el a hello toolearn [diagnosztikai naplók áttekintése](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md?toc=%2fazure%2fnetworking%2ftoc.json) cikk.
- **Metrikák:** adatok gyűjtése le TELJESÍTMÉNYMÉRÉSEK és az erőforrás egy meghatározott időtartamra vonatkozóan gyűjtött adatait. Metrikák lehet használt tootrigger riasztások küszöbértékek alapján. Jelenleg metrikák érhetők el az alkalmazás-átjárón. További információk a metrikákat, olvassa el a hello toolearn [metrikák áttekintése](../monitoring-and-diagnostics/monitoring-overview-metrics.md?toc=%2fazure%2fnetworking%2ftoc.json) cikk.
- **Hibaelhárítás:** hibaelhárítási információkat érhető el közvetlenül a hello Azure-portálon. hello információk segítséget nyújtanak a ExpressRoute, VPN-átjáró, Alkalmazásátjáró, hálózati biztonsági naplókat, útvonalakat, DNS, Load Balancer és Traffic Manager kapcsolatos gyakori problémák diagnosztizálásához.
- **Szerepköralapú hozzáférés-vezérlést (RBAC):** személyek hozhat létre és kezelhet a szerepköralapú hozzáférés-vezérlést (RBAC) hálózati erőforrásokhoz. További tudnivalók az RBAC hello olvasásával [Ismerkedés az RBAC](../active-directory/role-based-access-control-what-is.md?toc=%2fazure%2fnetworking%2ftoc.json) cikk. 
- **Csomagrögzítéssel:** hello Azure hálózati figyelőt szolgáltatás hello képességét toorun egy csomagot a virtuális gép egy bővítményével belül hello virtuális gép rögzítése. Ez a funkció a Linux és a Windows virtuális gépek érhető el. További információk a csomagrögzítéssel, olvassa el a hello toolearn [csomag rögzítési áttekintése](../network-watcher/network-watcher-packet-capture-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) cikk.
- **Ellenőrizze az IP-adatfolyamok:** hálózati figyelőt lehetővé teszi a tooverify IP adatfolyamok között egy Azure virtuális gép és a távoli erőforrás toodetermine e csomagok számára engedélyezi vagy megtagadja. Ezt a képességet biztosít a rendszergazdák hello képességét tooquickly kapcsolódási problémák diagnosztizálásához. toolearn hogyan tooverify IP zajló kommunikációról, kapcsolatos további információkért olvassa el a hello [IP folyamata – áttekintés ellenőrzése](../network-watcher/network-watcher-ip-flow-verify-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) cikk.
- **VPN-kapcsolatok:** hello VPN hibaelhárító képességét hálózati figyelőt biztosít a kapcsolat vagy az átjáró képes tooquery hello és hello erőforrások hello állapotának ellenőrzése. VPN-kapcsolatok, olvassa el a hello hibaelhárítással kapcsolatos további toolearn [VPN-kapcsolat hibaelhárítása – áttekintés](../network-watcher/network-watcher-troubleshoot-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) cikk.
- **Hálózati topológia megtekintése:** hello hálózati erőforrások grafikus ábrázolása tekintse meg a virtuális hálózatot a hálózati figyelőt. hálózati topológia, olvassa el a hello megtekintéséről további toolearn [topológiájának áttekintése](../network-watcher/network-watcher-topology-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) cikk.

## <a name="tools"></a>Üzembe helyezési és konfigurációs eszközök

Telepítheti és konfigurálhatja az Azure-hálózati erőforrások hello a következő eszközök bármelyikével:

- **Azure-portálon:** egy grafikus felhasználói felület, amelyen a böngészőben. Nyissa meg hello [Azure-portálon](http://portal.azure.com).
- **Az Azure PowerShell:** Azure a Windows rendszerű számítógépek felügyeletére szolgáló parancssori eszközök. További tudnivalók az Azure PowerShell által olvasása hello [Azure PowerShell áttekintése](/powershell/azure/overview?view=azurermps-3.8.0?toc=%2fazure%2fnetworking%2ftoc.json) cikk.
- **Azure parancssori felület (CLI):** Azure Linux, macOS vagy Windows-számítógépek felügyeletére szolgáló parancssori eszközök. További információk hello Azure CLI által olvasása hello [Azure CLI áttekintése](/cli/azure/get-started-with-azure-cli?toc=%2fazure%2fnetworking%2ftoc.json) cikk.
- **Az Azure Resource Manager-sablonok:** (JSON formátumú fájlba), amely meghatározza a hello infrastruktúráját és konfigurációját, egy Azure megoldás. A sablonok segítségével a megoldás a teljes életciklusa során ismételten üzembe helyezhető, és az erőforrások üzembe helyezése biztosan konzisztens lesz. További információk a létrehozásról sablonok, olvassa el a hello toolearn [gyakorlati tanácsok sablonok létrehozásához](../azure-resource-manager/resource-manager-template-best-practices.md?toc=%2fazure%2fnetworking%2ftoc.json) cikk. Sablonok hello Azure-portálon parancssori felületen vagy a PowerShell üzembe helyezhetők. sablonok azonnal, használatába tooget telepítése hello egyik hello számos előre konfigurált sablonok [Azure gyors üzembe helyezési sablonokat](https://azure.microsoft.com/resources/templates/?term=network) könyvtár. 

## <a name="pricing"></a>Díjszabás

Néhány hello Azure hálózati szolgáltatásokat kell járnak, míg mások szabad. Nézet hello [virtuális hálózati](https://azure.microsoft.com/pricing/details/virtual-network), [VPN-átjáró](https://azure.microsoft.com/pricing/details/vpn-gateway), [Alkalmazásátjáró](https://azure.microsoft.com/en-us/pricing/details/application-gateway/), [terheléselosztó](https://azure.microsoft.com/pricing/details/load-balancer), [hálózatifigyelőt](https://azure.microsoft.com/pricing/details/network-watcher), [DNS](https://azure.microsoft.com/pricing/details/dns), [Traffic Manager](https://azure.microsoft.com/pricing/details/traffic-manager) és [ExpressRoute](https://azure.microsoft.com/pricing/details/expressroute) árképzési lapok további információt.

## <a name="next-steps"></a>Következő lépések

- Az első virtuális hálózat létrehozása, és néhány virtuális gépek tooit, csatlakozzon a hello hello lépések elvégzésével [az első virtuális hálózat létrehozása](../virtual-network/virtual-network-get-started-vnet-subnet.md?toc=%2fazure%2fnetworking%2ftoc.json) cikk.
- Csatlakozás a számítógép tooa VNet hello lépések végrehajtásával a hello [egy pont – hely kapcsolat beállítása](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fnetworking%2ftoc.json) cikk.
- A terhelést internetes forgalom toopublic kiszolgálókra hello lépések végrehajtásával a hello [hozzon létre egy internetre irányuló terheléselosztót](../load-balancer/load-balancer-get-started-internet-portal.md?toc=%2fazure%2fnetworking%2ftoc.json) cikk.
