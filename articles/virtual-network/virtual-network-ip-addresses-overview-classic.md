---
title: "aaaIP cím típusok (klasszikus) Azure-ban |} Microsoft Docs"
description: "További információk a nyilvános és magánhálózati IP-címek (klasszikus) Azure-ban."
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
tags: azure-service-management
ms.assetid: 2f8664ab-2daf-43fa-bbeb-be9773efc978
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/11/2016
ms.author: jdial
ms.openlocfilehash: 7e09a5ad5b5f2d55329152b5d843de3c4455d1b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="ip-address-types-and-allocation-methods-classic-in-azure"></a>IP-cím típusok és elosztási módszert (klasszikus) Azure-ban
Rendelhet IP címek tooAzure erőforrások toocommunicate más Azure-erőforrások, a helyszíni hálózat és hello Internet. Az IP-címek is használhatja az Azure-ban két típusa van: nyilvános és titkos.

Nyilvános IP-címek hello Internet, beleértve a nyilvánosan elérhető Azure services szolgáltatással való kommunikációhoz használt.

Magán IP-címek vannak használja egy Azure virtuális hálózatot (VNet), egy felhőalapú szolgáltatás és a helyszíni hálózaton belüli kommunikációhoz, ha a VPN-átjáró vagy ExpressRoute-kapcsolatcsoport tooextend a hálózati tooAzure.

> [!IMPORTANT]
> Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../resource-manager-deployment-model.md).  Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén használja a Resource Manager. További tudnivalók az IP-címeket az erőforrás-kezelőben olvasása hello [IP-címek](virtual-network-ip-addresses-overview-arm.md) cikk.

## <a name="public-ip-addresses"></a>Nyilvános IP-címek
Nyilvános IP-címek engedélyezése Azure-erőforrások toocommunicate az internetes és az Azure nyilvános szolgáltatásaival, például [Azure Redis Cache](https://azure.microsoft.com/services/cache/), [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/), [SQL-adatbázisok](../sql-database/sql-database-technical-overview.md), és [az Azure storage](../storage/common/storage-introduction.md).

A következő típusú erőforrások hello egy nyilvános IP-cím hozzárendelve:

* Felhőszolgáltatások
* Infrastruktúra-szolgáltatási virtuális gépeket (VM)
* PaaS szerepkörpéldányok
* VPN-átjárók
* Alkalmazásátjárók

### <a name="allocation-method"></a>Lefoglalási módszer
Ha egy nyilvános IP-cím szükség van az Azure-erőforrás tooan hozzárendelt toobe, *dinamikusan* lefoglalt elérhető nyilvános IP-készletből belül hello hely hello erőforrás cím úgy jön létre. Az IP-cím van kiadva, ha hello erőforrás le van állítva. Abban az esetben, ha egy felhőszolgáltatás, ez történik, ha minden hello szerepkörpéldányokat le van állítva, amelyek segítségével elkerülhetők a *statikus* (fenntartott) IP-cím (lásd: [Felhőszolgáltatások](#Cloud-services)).

> [!NOTE]
> hello IP-címtartománylista, amelyből nyilvános IP-címek tooAzure erőforrásokat foglal le a közzétett [Azure Datacenter IP-címtartományok](https://www.microsoft.com/download/details.aspx?id=41653).
> 
> 

### <a name="dns-hostname-resolution"></a>DNS-állomásnév feloldása
Egy felhőalapú szolgáltatás, vagy az infrastruktúra-szolgáltatási virtuális gép létrehozásakor kell tooprovide egy felhőalapú szolgáltatás DNS-neve, ami esetében egyedi összes erőforrást az Azure-ban. Ez létrehoz leképezéseket hello Azure által kezelt DNS-kiszolgálóinak *dnsname*. cloudapp.net toohello nyilvános IP-cím hello erőforrás. Például ha hoz létre egy felhőszolgáltatás egy felhőalapú szolgáltatás DNS-neve **contoso**, hello teljesen minősített tartománynevét (FQDN) **contoso.cloudapp.net** tooa nyilvános IP-címét (VIP) fel hello felhőalapú szolgáltatás. Használhatja a teljes tartománynév toocreate toohello nyilvános IP-címre mutat az Azure-ban egyéni tartományhoz külön CNAME rekordot.

### <a name="cloud-services"></a>Felhőszolgáltatások
Egy felhőalapú szolgáltatás mindig van egy nyilvános IP-cím hivatkozott tooas egy virtuális IP-cím (VIP). Létrehozhat egy felhőalapú szolgáltatás tooassociate többféle porton lévő virtuális gépek és a szerepkörpéldányok belül hello felhőszolgáltatás hello VIP toointernal portok végpontok. 

A felhőszolgáltatás tartalmazhat több IaaS virtuális gépeket, vagy PaaS szerepkörpéldányok, az összes elérhetővé keresztül hello azonos cloud service VIP. Is rendelhet [több virtuális IP-címek tooa felhőszolgáltatás](../load-balancer/load-balancer-multivip.md), amely lehetővé teszi több virtuális IP-CÍMES helyzetekben, például a webhely SSL-alapú több-bérlős környezet.

Hello nyilvános IP-cím egy felhőalapú szolgáltatás marad hello azonos, akkor is, ha az összes hello szerepkörpéldányokat le van állítva, használatával biztosítható a *statikus* nyilvános IP-cím, a hivatkozott tooas [foglalt IP-cím](virtual-networks-reserved-public-ip.md). A statikus (fenntartott) IP-erőforrás létrehozása egy adott helyen, és rendelje hozzá az adott helyre tooany felhőalapú szolgáltatás. Hello szolgáltatás számára fenntartott IP-címhez hello tényleges IP-cím nem adható meg, a készlet létrehozása hello helyen elérhető IP-címek hozzárendelése. Az IP-cím kiadása nem történik, amíg explicit módon törlése.

Statikus (fenntartott) nyilvános IP-címek gyakran fordulnak elő hello forgatókönyvek, ahol egy felhőalapú szolgáltatás:

* tűzfal-szabályok toobe telepítő igényel a végfelhasználók által.
* külső DNS-névfeloldás, attól függ, és egy dinamikus IP-cím A rekordok frissítése szükséges.
* IP-alapú biztonsági modellt használó külső webes szolgáltatások igényel.
* SSL tanúsítványok csatolt tooan IP-címet használja.

> [!NOTE]
> Amikor létrehoz egy klasszikus virtuális Gépet, a tároló *felhőalapú szolgáltatás* jön létre az Azure-ban, a virtuális IP-cím (VIP). Ha egy alapértelmezett RDP és az SSH-portálon keresztül hello másolat elkészült *végpont* hello portál van beállítva, ezért a virtuális gép toohello csatlakozhatnak hello cloud service VIP. A felhőalapú szolgáltatás VIP foglalható le, amely hatékonyan biztosítja a fenntartott IP cím tooconnect toohello virtuális gép. További portokat úgy is megnyithatja, hogy további végpontok konfigurálásakor.
> 
> 

### <a name="iaas-vms-and-paas-role-instances"></a>Infrastruktúra-szolgáltatási virtuális gépeknél és PaaS szerepkörpéldányok
Hozzárendelhet egy nyilvános IP-címek közvetlenül tooan IaaS [VM](../virtual-machines/virtual-machines-linux-about.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) vagy PaaS szerepkörpéldányt felhőszolgáltatásban. Ez a hivatkozott tooas példányszintű nyilvános IP-címet ([ILPIP](virtual-networks-instance-level-public-ip.md)). A nyilvános IP-cím csak dinamikus lehet.

> [!NOTE]
> Ez nem azonos a hello VIP hello felhőalapú szolgáltatás, amely IaaS virtuális gépeket vagy PaaS szerepkörpéldányok tárolója, mivel egy felhőalapú szolgáltatás is tartalmazhat, több IaaS virtuális gépeket, vagy PaaS szerepkörpéldányt, minden kitett hello keresztül azonos cloud service VIP.
> 
> 

### <a name="vpn-gateways"></a>VPN-átjárók
A [VPN-átjáró](../vpn-gateway/vpn-gateway-about-vpngateways.md) lehet egy Azure virtuális hálózat tooother használt tooconnect Azure Vnetekhez vagy a helyszíni hálózat. VPN-átjáró hozzá van rendelve egy nyilvános IP-cím *dinamikusan*, amely lehetővé teszi, hogy a távoli hálózati hello kommunikál.

### <a name="application-gateways"></a>Alkalmazásátjárók
Egy Azure [Alkalmazásátjáró](../application-gateway/application-gateway-introduction.md) Layer7 HTTP-alapú terheléselosztás tooroute hálózati forgalmat is használható. Alkalmazásátjáró hozzá van rendelve egy nyilvános IP-cím *dinamikusan*, ami azzal működik elosztott terhelésű VIP hello.

### <a name="at-a-glance"></a>Áttekintés
hello az alábbi táblázatban minden erőforrástípus hello lehetséges, hogy foglalási módszerek (dinamikus vagy statikus), és képes tooassign több nyilvános IP-címet.

| Erőforrás | Dinamikus | Statikus | Több IP-cím |
| --- | --- | --- | --- |
| A felhőalapú szolgáltatás |Igen |Igen |Igen |
| Infrastruktúra-szolgáltatási virtuális gép vagy PaaS szerepkörpéldányt |Igen |Nem |Nem |
| VPN-átjáró |Igen |Nem |Nem |
| Alkalmazásátjáró |Igen |Nem |Nem |

## <a name="private-ip-addresses"></a>Magánhálózati IP-címek
Magán IP-címek az Azure-erőforrások toocommunicate egyéb erőforrásokkal egy felhőalapú szolgáltatás használatának engedélyezése vagy egy [virtuális hálózati](virtual-networks-overview.md)(VNet), vagy tooon helyi hálózathoz (keresztül egy VPN-átjáró vagy ExpressRoute-kapcsolatcsoportot) használata nélkül egy Internet-elérhető IP-címre.

Az Azure klasszikus üzembe helyezési modellel egy magánhálózati IP-cím rendelhető toohello Azure-erőforrások a következő:

* Infrastruktúra-szolgáltatási virtuális gépeknél és PaaS szerepkörpéldányok
* Belső terheléselosztó
* Alkalmazásátjáró

### <a name="iaas-vms-and-paas-role-instances"></a>Infrastruktúra-szolgáltatási virtuális gépeknél és PaaS szerepkörpéldányok
Egy felhőalapú szolgáltatás, hasonló tooPaaS szerepkörpéldányokat mindig kerülnek hello klasszikus telepítési modellel létrehozott virtuális gépeket (VM). hello viselkedés magánhálózati IP-címek így hasonlóak az ezekhez az erőforrásokhoz.

Fontos toonote, amely egy felhőalapú szolgáltatás telepítve két módon:

* Mint egy *önálló* felhőalapú szolgáltatás, amennyiben nincs a virtuális hálózaton belül.
* A virtuális hálózat része.

#### <a name="allocation-method"></a>Lefoglalási módszer
Esetében a *önálló* felhőalapú szolgáltatás, egy magánhálózati IP-cím lefoglalt erőforrások get *dinamikusan* a hello Azure datacenter privát IP-címtartományt. Használhatja a többi virtuális gépen belül hello kommunikációjához ugyanaz a felhőalapú szolgáltatás. Az IP-címet módosíthatja, amikor hello erőforrás leáll, majd elindítani.

Abban az esetben, ha egy felhőalapú szolgáltatás, a virtuális hálózaton belül telepített, erőforrások lekérése titkos hello címtartományán hello vannak lefoglalva az IP-címek társított alhálózat (a megadott hálózati konfigurációja). A privát IP-címek hello virtuális hálózaton belül az összes virtuális gépek közötti kommunikációra használható.

Emellett esetén felhőszolgáltatások történik egy Vneten belül, egy magánhálózati IP-cím van lefoglalva *dinamikusan* (a DHCP használatával) alapértelmezés szerint. Ha az hello erőforrás leáll, majd elindítani módosíthatja azt. tooensure hello IP-cím marad hello azonos, akkor az tooset hello elosztási módszert túl kell*statikus*, és adjon meg egy érvényes IP-cím hello megfelelő címtartományán belül.

Statikus magánhálózati IP-címeket általában a következő esetekben szoktak használni:

* Tartományvezérlőként vagy DNS-kiszolgálóként működő virtuális gépek esetén.
* Virtuális gépek IP-címeket használnak tűzfalszabályokat igénylő.
* Futó szolgáltatások IP-címet egyéb alkalmazások által elért virtuális gépeket.

#### <a name="internal-dns-hostname-resolution"></a>Belső DNS-állomásnév feloldása
Minden Azure virtuális gépek és PaaS szerepkörpéldányok konfigurált [Azure által kezelt DNS-kiszolgálók](virtual-networks-name-resolution-for-vms-and-role-instances.md#azure-provided-name-resolution) alapértelmezés szerint csak explicit módon egyéni DNS-kiszolgálók. A DNS-kiszolgálók belső névfeloldást biztosít a virtuális gépek és a szerepkörpéldányok belüli hello ugyanazt a virtuális hálózat vagy a felhőalapú szolgáltatás.

A virtuális gépek létrehozásakor a hello állomásnév tooits magánhálózati IP-cím társítás toohello Azure által kezelt DNS-kiszolgálók jelenik meg. A több hálózati Adapterrel virtuális gépek esetén hello állomásnév le van képezve toohello magánhálózati IP-címe hello elsődleges hálózati adaptert. A hozzárendelési információ azonban korlátozott tooresources hello belül ugyanaz a felhőalapú szolgáltatás, vagy a virtuális hálózat.

Esetében a *önálló* felhőalapú szolgáltatás, képes tooresolve állomásnevek fogja az összes virtuális gépeket/szerepkörpéldányokat hello belül ugyanaz a felhőalapú szolgáltatás csak. Esetén a Vneten belül egy felhőalapú szolgáltatás fogja képes tooresolve állomásnevek az összes hello virtuális gépeket/szerepkörpéldányokat hello virtuális hálózaton belül.

### <a name="internal-load-balancers-ilb--application-gateways"></a>Belső terheléselosztók (ILB) és alkalmazásátjárók
Hozzárendelhet egy privát IP-cím toohello **előtér** konfigurációja egy [Azure belső terheléselosztó](../load-balancer/load-balancer-internal-overview.md) (ILB) vagy egy [Azure Application Gateway](../application-gateway/application-gateway-introduction.md). A magánhálózati IP-cím belső végpont funkcionál, elérhető csak toohello erőforrások hello távoli hálózatok és virtuális hálózathoz (VNet) kapcsolódó toohello virtuális hálózat. Vagy a dinamikus vagy statikus magánhálózati IP-címének toohello előtér konfigurációja rendelhet hozzá. Több privát IP címek tooenable multi-vip forgatókönyvek is hozzárendelhetők.

### <a name="at-a-glance"></a>Áttekintés
hello az alábbi táblázatban minden erőforrástípus hello lehetséges, hogy foglalási módszerek (dinamikus vagy statikus), és képes tooassign több magánhálózati IP-címét.

| Erőforrás | Dinamikus | Statikus | Több IP-cím |
| --- | --- | --- | --- |
| Virtuális gép (a egy *önálló* felhőalapú szolgáltatás) |Igen |Igen |Igen |
| A PaaS szerepkörpéldányt (az egy *önálló* felhőalapú szolgáltatás) |Igen |Nem |Igen |
| A virtuális gép vagy PaaS szerepkörpéldányt (a VNet) |Igen |Igen |Igen |
| Belső terheléselosztó előtér |Igen |Igen |Igen |
| Átjáró előtér-alkalmazás |Igen |Igen |Igen |

## <a name="limits"></a>Korlátok
hello az alábbi táblázat hello határoz meg küszöbértéket IP-címzés az Azure-ban előfizetésenként. Is [forduljon a támogatási szolgálathoz](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooincrease hello alapértelmezett korlátokat mentése toohello határok a üzleti igények alapján.

|  | Alapértelmezett korlát | Felső korlát |
| --- | --- | --- |
| Nyilvános IP-címek (dinamikus) |5 |kapcsolatfelvétel az ügyfélszolgálattal |
| Fenntartott nyilvános IP-címek |20 |kapcsolatfelvétel az ügyfélszolgálattal |
| Nyilvános VIP (felhőszolgáltatás) üzemelő példányonként |5 |kapcsolatfelvétel az ügyfélszolgálattal |
| Magán a VIP (ILB) a (felhőszolgáltatás) üzemelő példányonként |1 |1 |

Győződjön meg arról, hogy olvassa el a teljes készletének hello [korlátozza a hálózati](../azure-subscription-service-limits.md#networking-limits) az Azure-ban.

## <a name="pricing"></a>Díjszabás
A legtöbb esetben a nyilvános IP-címek szabadon. Nincs névleges díj toouse további és/vagy a statikus nyilvános IP-címeket. Győződjön meg arról, hogy tudomásul veszi, hogy hello [struktúra árképzési nyilvános IP-címek](https://azure.microsoft.com/pricing/details/ip-addresses/).

## <a name="differences-between-resource-manager-and-classic-deployments"></a>Erőforrás-kezelő és a klasszikus üzembe helyezés közötti különbségek
Alább az IP-címzési funkciók erőforrás-kezelő és a hello klasszikus telepítési modellből-összehasonlítást láthat.

|  | Erőforrás | Klasszikus | Resource Manager |
| --- | --- | --- | --- |
| **Nyilvános IP-cím** |***VIRTUÁLIS GÉP*** |A hivatkozott tooas egy ILPIP (csak dinamikus) |Említett tooas egy nyilvános IP-cím (dinamikus vagy statikus) |
|  ||Hozzárendelt tooan infrastruktúra-szolgáltatási virtuális gép vagy egy PaaS szerepkörpéldányt |Kapcsolódó toohello VM hálózati adapter | |
|  |***Az Internet felé néző terheléselosztó*** |Említett tooas VIP (dinamikus) vagy fenntartott IP (statikus) |Említett tooas egy nyilvános IP-cím (dinamikus vagy statikus) | |
|  ||Hozzárendelt tooa felhőalapú szolgáltatás |Kapcsolódó toohello betölteni a terheléselosztó előtér-config | |
|  | | | |
| **Magánhálózati IP-cím** |***VIRTUÁLIS GÉP*** |A hivatkozott tooas, a DIP-ről |Említett tooas egy magánhálózati IP-cím |
|  ||Hozzárendelt tooan infrastruktúra-szolgáltatási virtuális gép vagy egy PaaS szerepkörpéldányt |Hozzárendelt toohello VM hálózati adapter | |
|  |***Belső terheléselosztón (ILB)*** |Hozzárendelt toohello ILB (dinamikus vagy statikus) |Hozzárendelt toohello ILB előtér konfigurációs (dinamikus vagy statikus) | |

## <a name="next-steps"></a>Következő lépések
* [Telepítse a virtuális Gépet egy statikus magánhálózati IP-cím](virtual-networks-static-private-ip-classic-pportal.md) használatával hello Azure-portálon.

