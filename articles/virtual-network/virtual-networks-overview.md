---
title: "Virtuális hálózati aaaAzure |} Microsoft Docs"
description: "További tudnivalók az Azure Virtual Network fogalmakat és szolgáltatásokat."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 9633de4b-a867-4ddf-be3c-a332edf02e24
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/23/2017
ms.author: jdial
ms.openlocfilehash: 55ae6a131d882ad893aeffcaa4127bc47beda552
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-virtual-network"></a>Azure Virtual Network

hello Azure Virtual Network szolgáltatás lehetővé teszi, hogy Ön toosecurely Csatlakozás más Azure-erőforrások tooeach virtuális hálózatokról (Vnetekről). A virtuális hálózat a saját hálózati hello felhőben megjelenítése. A virtuális hálózat hello Azure felhőben dedikált tooyour előfizetés logikai elkülönítése. Vnetek tooyour a helyszíni hálózathoz is csatlakoztathatja. a következő kép hello néhány hello Azure Virtual Network szolgáltatás hello képességeit mutatja be:

![Hálózati diagram](./media/virtual-networks-overview/virtual-network-overview.png)

További információ az Azure Virtual Network képességeit, a következő hello toolearn kattintson hello funkció:
- **[Elkülönítési:](#isolation)**  Vnetek el különítve egymástól. Fejlesztési, tesztelési és éles külön Vnetek adott használata hello is létrehozhat ugyanahhoz a CIDR címblokkokat. Ezzel szemben, amelyek különböző CIDR címblokkokat és összekapcsolhatja az hálózatok több Vnetek is létrehozhat. Egy VNet érdekében több alhálózatra is szegmentálhatja. Azure belső névfeloldást biztosít a virtuális gépek és Felhőszolgáltatások szerepkörpéldányokat csatlakoztatott tooa virtuális hálózat. Konfigurálhatja a virtuális hálózat toouse saját DNS-kiszolgálókat, az Azure belső névfeloldást használata helyett.
- **[Internetkapcsolat:](#internet)**  minden Azure virtuális gépek (VM) és a Felhőszolgáltatások szerepkörpéldányokat csatlakoztatott tooa VNet rendelkezik hozzáférési toohello Internet, alapértelmezés szerint. Bejövő hozzáférést toospecific erőforrások, igény szerint is engedélyezheti.
- **[Az Azure erőforrás-kapcsolat:](#within-vnet)**  például a Cloud Services és a virtuális gépek Azure-erőforrások lehetnek csatlakoztatott toohello ugyanazt a virtuális hálózatot. hello erőforrások képesek-e csatlakozni más tooeach privát IP-címek, még akkor is, ha külön alhálózatokon vannak. Azure biztosít alapértelmezett között alhálózatok, a Vnetek és a helyszíni hálózatokhoz, így nem rendelkezik tooconfigure és útvonalak kezelése.
- **[VNet-kapcsolatot:](#connect-vnets)**  Vnetek lehet a más csatlakoztatott tooeach, erőforrások engedélyezése tooany VNet toocommunicate kapcsolódnak bármilyen olyan erőforrás bármely más virtuális hálózaton.
- **[A helyi kapcsolat:](#connect-on-premises)**  Vnetek csatlakoztatott tooon helyi hálózatok magánhálózati kapcsolatok a hálózati és az Azure között, vagy egy pont-pont VPN-kapcsolaton keresztül lehet hello interneten keresztül.
- **[Forgalomszűrést végez:](#filtering)**  virtuális gép és a felhőalapú szolgáltatások szerepkör példányok hálózati forgalom bejövő és kimenő alapján szűrhetők forrás IP-cím és port, cél IP-cím és port és protokoll.
- **[Útválasztás:](#routing)**  opcionálisan felülbírálhatja Azure alapértelmezett útválasztási saját útvonalak konfigurálása, vagy használja a BGP-útvonalakat hálózati átjárón keresztül.

## <a name = "isolation"></a>A hálózati elkülönítés és Szegmentálás

Minden Azure belül több Vnetek Megvalósíthat [előfizetés](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) és az Azure [régió](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#region). Minden egyes virtuális hálózat el különítve a más Vnetekről. Minden egyes vnet a következő műveletek végezhetők el:
- Adjon meg egy egyéni privát IP-címtér nyilvános és titkos (az RFC 1918) címeket használnak. Azure rendeli hozzá az erőforrások csatlakoztatott toohello VNet egy magánhálózati IP-címet rendel hello címterület.
- Hello VNet be egy vagy több alhálózatból szegmentálni, és hello VNet terület tooeach címalhálózata része lefoglalni.
- Azure által biztosított névfeloldás használata, vagy adja meg az erőforrások használhatják a saját DNS-kiszolgáló csatlakoztatva tooa virtuális hálózat. További információk a névfeloldás a Vneteket, olvassa el a hello toolearn [névfeloldását virtuális gépek és Felhőszolgáltatások](virtual-networks-name-resolution-for-vms-and-role-instances.md) cikk.

## <a name = "internet"></a>Csatlakozás toohello Internet
Tooa VNet összes erőforrások csatlakoztatott kimenő kapcsolat toohello Internet alapértelmezés szerint rendelkezik. hello erőforrás hello magánhálózati IP-címe a forrás hálózati címe (SNAT) tooa nyilvános IP-cím fordította hello Azure-infrastruktúra. További információk a kimenő internetkapcsolat működését, olvassa el a hello toolearn [ismertetése az Azure-ban kimenő kapcsolatok](..\load-balancer\load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json#standalone-vm-with-no-instance-level-public-ip-address) cikk. Hello alapértelmezett kapcsolat egyéni Útválasztás és a forgalom szűrés alkalmazásával módosíthatja.

toocommunicate bejövő hello Internet, illetve toocommunicate kimenő toohello Internet nélkül SNAT, egy erőforrást egy nyilvános IP-cím rendelendő tooAzure erőforrásokat. További információ a nyilvános IP-címek, olvassa el a hello toolearn [nyilvános IP-címek](virtual-network-public-ip-address.md) cikk.

## <a name="within-vnet"></a>Csatlakozás Azure-erőforrások
Több Azure-erőforrások tooa virtuális hálózat, például a virtuális gépek (VM), a Cloud Services, az App Service Environment-környezetek és a virtuálisgép-méretezési csoportok is elérheti. Virtuális gépek tooa alhálózati történik egy Vneten belül egy adott hálózati csatoló (NIC) keresztül csatlakozni. További információk a hálózati adapterek, olvassa el a hello toolearn [hálózati illesztőt](virtual-network-network-interface.md) cikk.

## <a name="connect-vnets"></a>Virtuális hálózatok csatlakoztatása

Csatlakozhat a más Vnetekről tooeach, erőforrások engedélyezése tooeither VNet toocommunicate egymással keresztül kapcsolódó Vnetek. Valamelyike vagy mindegyike a következő beállítások tooconnect Vnetek tooeach más hello használhatja:
- **Társviszony-létesítés:** lehetővé teszi, hogy erőforrásokat csatlakoztatott toodifferent Azure Vnetekhez hello belül azonos Azure-beli hely toocommunicate egymással. hello sávszélesség és a késleltetés között Vnetek van hello hello ugyanaz, mintha hello erőforrások csatlakoztatott toohello ugyanazt a virtuális hálózatot. toolearn társviszony-létesítést, kapcsolatos további információkért olvassa el a hello [virtuális hálózati társviszony-létesítés](virtual-network-peering-overview.md) cikk.
- **VNet – VNet-kapcsolatot:** lehetővé teszi, hogy erőforrásokat csatlakoztatott toodifferent Azure virtuális hálózaton belül hello ugyanazon vagy másik Azure helyét. Ellentétben a társviszony-létesítést, sávszélesség oka Vnetek közötti forgalmat kell áramlása az Azure VPN Gateway. További információk a Vneteket összekötő egy VNet – VNet kapcsolattal, olvassa el a hello toolearn [VNet – VNet-kapcsolatot konfiguráló](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) cikk.

## <a name="connect-on-premises"></a>Tooan a helyszíni hálózat

Csatlakozhat a helyszíni hálózati tooa virtuális hálózatot az alábbi beállítások hello tetszőleges kombinációját használja:
- **Pont-pont virtuális magánhálózati (VPN):** egyetlen Számítógéphez csatlakoztatott tooyour hálózati és hello VNet között. A kapcsolat típusa nem nagyszerű, ha Ön most csak az első lépések az Azure-ral, vagy a fejlesztők számára, mert azt igényli, hogy kevéssé vagy egyáltalán ne módosítások meglévő tooyour-hálózati. hello kapcsolat hello Internet hello PC és hello VNet között hello SSTP protokollal tooprovide titkosított kommunikációt használ. pont-pont típusú VPN hello késés is előre nem látható, mivel hello a forgalom halad át hello Internet.
- **Telephelyek közötti VPN:** a VPN-eszköz és az Azure VPN-átjáró között. A kapcsolat típusa lehetővé teszi, hogy bármely tooaccess egy Vnetet engedélyezi, hogy a helyi erőforrás. hello kapcsolat az IPSec/IKE VPN, amely titkosított kommunikáció keresztül hello Internet hello Azure VPN gateway a helyszíni eszközök között. a pont-pont kapcsolat hello késés is előre nem látható, mivel hello a forgalom halad át hello Internet.
- **Az Azure ExpressRoute:** hoznak létre egy ExpressRoute-partner keresztül a hálózat és az Azure-ban. Ezt a kapcsolatot a sajátja. Forgalom nem járja hello Internet. az ExpressRoute-kapcsolat hello késés is előre jelezhető, mivel a forgalom nem bejárása hello Internet.

További információk az összes hello előző kapcsolati beállítások, olvassa el a hello toolearn [kapcsolat topológiai diagramot](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#diagrams) cikk.

## <a name="filtering"></a>Hálózati forgalom szűrésére
Egyik vagy mindkét alábbi beállítások hello alhálózatokat közötti hálózati forgalom szűrheti:
- **Hálózati biztonsági csoportok (NSG):** minden NSG több bejövő és kimenő szabályokat, amelyek lehetővé teszik a forrás és cél IP-cím, port és protokoll toofilter forgalmat is tartalmazhat. Az NSG tooeach egy virtuális hálózati adapter is alkalmazhatja. Az NSG-toohello alhálózatot a hálózati adapter is alkalmazhat, vagy más Azure-erőforrás, csatlakozik-e. További információk az NSG-k, olvassa el a hello toolearn [hálózati biztonsági csoportok](virtual-networks-nsg.md) cikk.
- **Virtuális készülékek (NVA) hálózati:** az NVA egy hálózati funkciót, például egy tűzfal végző szoftvert futtató virtuális gép. Rendelkezésre álló NVAs listájának megtekintése a hello [Azure piactér](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?page=1&subcategories=appliances). NVAs is elérhetők, hogy WAN-optimalizálást és az egyéb hálózati forgalom funkciókat biztosít. NVAs általában használt felhasználói vagy a BGP-útvonalakat. Használhatja az NVA toofilter forgalom Vnetek között is.

## <a name="routing"></a>Hálózati forgalom

Az útvonaltáblák, amelyek lehetővé teszik az erőforrások csatlakoztatott tooany alhálózatának bármely virtuális hálózat toocommunicate egymással, alapértelmezés szerint az Azure létrehoz. Megvalósíthat vagy, vagy mindkét a következő beállítások toooverride hello hello alapértelmezett útvonalak Azure-hoz:
- **Felhasználó által definiált útvonalak:** hozhat létre egyéni útvonaltáblák útvonalat, ahol a forgalmat az irányított toofor szabályozó az egyes alhálózatokon. További információk a felhasználó által definiált útvonalak, olvassa el a hello toolearn [felhasználó által definiált útvonalak](virtual-networks-udr-overview.md) cikk.
- **BGP-útvonalakat:** Ha a virtuális hálózat tooyour a helyszíni hálózat az Azure VPN-átjáró vagy ExpressRoute kapcsolattal csatlakozik, a BGP-útvonalak tooyour Vnetek kerülhet.

## <a name="pricing"></a>Díjszabás

Használata díjmentes virtuális hálózatok, alhálózatok, útvonaltábláit vagy hálózati biztonsági csoportok. Kimenő internetes sávszélesség, nyilvános IP-címek, virtuális hálózati társviszony-létesítést, VPN-átjárók, és ExpressRoute minden rendelkezik meg saját struktúrákat árképzési. Nézet hello [virtuális hálózati](https://azure.microsoft.com/pricing/details/virtual-network), [VPN-átjáró](https://azure.microsoft.com/pricing/details/vpn-gateway), és [ExpressRoute](https://azure.microsoft.com/pricing/details/expressroute) árképzési lapok további információt.

## <a name="faq"></a>GYIK

Virtuális hálózati kapcsolatos gyakori kérdések tooreview lásd: hello [virtuális hálózati gyakran ismételt kérdések](virtual-networks-faq.md) cikk.


## <a name="next-steps"></a>Következő lépések

- Az első virtuális hálózat létrehozása, és néhány virtuális gépek tooit, csatlakozzon a hello hello lépések elvégzésével [az első virtuális hálózat létrehozása](virtual-network-get-started-vnet-subnet.md) cikk.
- Hozzon létre egy pont – hely kapcsolat tooa VNet hello lépések végrehajtásával a hello [egy pont – hely kapcsolat beállítása](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) cikk.
- Megismerhet néhány hello más kulcs [hálózati képességek](../networking/networking-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) Azure.
