---
title: "az Azure-ban aaaIP cím típusok |} Microsoft Docs"
description: "Információk a nyilvános és privát IP-címekről az Azure-ban."
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
tags: azure-resource-manager
ms.assetid: 610b911c-f358-4cfe-ad82-8b61b87c3b7e
ms.service: virtual-network
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/27/2016
ms.author: jdial
ms.openlocfilehash: 402d3707c00f0b3bf3ef1febd5ade66223da74bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="ip-address-types-and-allocation-methods-in-azure"></a>IP-cím-típusok és lefoglalási módszerek az Azure-ban
Rendelhet IP címek tooAzure erőforrások toocommunicate más Azure-erőforrások, a helyszíni hálózat és hello Internet. Az Azure-ban két típusú IP-címet használhat:

* **Nyilvános IP-címek**: hello Internet, beleértve a nyilvánosan elérhető Azure services szolgáltatással való kommunikációhoz használt
* **Privát IP-címek**: egy Azure virtuális hálózatot (VNet) belüli kommunikációhoz használt, és a helyszíni hálózati használatakor a VPN-átjáró vagy ExpressRoute-kapcsolatcsoport tooextend a hálózati tooAzure.

> [!NOTE]
> Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../resource-manager-deployment-model.md).  Ez a cikk ismerteti a használatával a Microsoft azt javasolja, a legtöbb új központi telepítés helyett hello hello Resource Manager üzembe helyezési modellben [klasszikus üzembe helyezési modellel](virtual-network-ip-addresses-overview-classic.md).
> 

Ha ismeri a hello klasszikus üzembe helyezési modellel, ellenőrizze a hello [IP-címzési klasszikus közötti különbségek és erőforrás-kezelő](virtual-network-ip-addresses-overview-classic.md#differences-between-resource-manager-and-classic-deployments).

## <a name="public-ip-addresses"></a>Nyilvános IP-címek
Nyilvános IP-címek engedélyezése Azure-erőforrások toocommunicate az internetes és az Azure nyilvános szolgáltatásaival, például [Azure Redis Cache](https://azure.microsoft.com/services/cache/), [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/), [SQL-adatbázisok](../sql-database/sql-database-technical-overview.md), és [az Azure storage](../storage/common/storage-introduction.md).

Az Azure Resource Manager szolgáltatásban a [nyilvános IP-cím](resource-groups-networking.md#public-ip-address) olyan erőforrás, amely saját tulajdonságokkal rendelkezik. A nyilvános IP-cím erőforrás társíthatja a következő erőforrások hello bármelyikét:

* Virtuális gépek (VM)
* Internetkapcsolattal rendelkező terheléselosztók
* VPN-átjárók
* Alkalmazásátjárók

### <a name="allocation-method"></a>Lefoglalási módszer
Két módszer, amelyben egy IP-cím van lefoglalva tooa *nyilvános* IP-erőforrás - *dinamikus* vagy *statikus*. hello alapértelmezett elosztási módszer *dinamikus*, ahol az IP-cím **nem** lefoglalt hello időpontban, ahol létrehozták. Ehelyett a hello nyilvános IP-cím (vagy hozzon létre) kapcsolódó hello erőforrás (például egy virtuális gép vagy terheléselosztó) történik. hello IP-cím leállítása (vagy törlése) hello erőforrás megjelenik. Ennek hatására hello IP-cím toochange, állítsa le és indítsa el az erőforrás.

tooensure hello IP-címet a hello kapcsolódó erőforrás marad hello azonos, állíthatja be hello elosztási módszert explicit módon túl*statikus*. Ebben az esetben a rendszer azonnal hozzárendel egy IP-címet. Csak akkor, ha hello erőforrás törlése vagy a foglalási mód megváltoztatása túl megjelent*dinamikus*.

> [!NOTE]
> Akkor is beállításakor hello elosztási módszert túl*statikus*, nem adható meg hello tényleges IP-cím toohello *nyilvános IP-erőforrás*. Ehelyett lekérdezi a lefoglalt hello Azure-beli hely szabad IP-cím egyes hello erőforrás jön létre.
>

Statikus nyilvános IP-címekhez gyakran fordulnak elő a következő forgatókönyvek hello:

* Végfelhasználók kell tooupdate tűzfal szabályok toocommunicate együtt az Azure-erőforrások.
* DNS-névfeloldás, ahol az IP-cím változása miatt frissíteni kellene az A-rekordokat.
* Az Ön Azure-erőforrásai más alkalmazásokkal és szolgáltatásokkal is kommunikálnak, amelyek IP-címalapú biztonsági modellt használnak.
* SSL tanúsítványok csatolt tooan IP-címet használja.

> [!NOTE]
> hello IP-címtartománylista, amelyből nyilvános IP-címek (dinamikus vagy statikus) tooAzure erőforrásokat foglal le a közzétett [Azure Datacenter IP-címtartományok](https://www.microsoft.com/download/details.aspx?id=41653).
>

### <a name="dns-hostname-resolution"></a>DNS-állomásnév feloldása
Megadhatja, hogy a DNS tartománynév-címke a nyilvános IP-erőforráshoz, létrehoz egy leképezést *domainnamelabel*. *hely*. cloudapp.azure.com toohello nyilvános IP-címet hello Azure által kezelt DNS-kiszolgálók. Például ha létrehoz egy nyilvános IP-erőforrás a **contoso** , egy *domainnamelabel* hello a **USA nyugati régiója** Azure *hely*, hello teljesen minősített tartománynevét (FQDN) **contoso.westus.cloudapp.azure.com** megoldja hello erőforrás toohello nyilvános IP-címét. Használhatja a teljes tartománynév toocreate toohello nyilvános IP-címre mutat az Azure-ban egyéni tartományhoz külön CNAME rekordot.

> [!IMPORTANT]
> Minden egyes létrehozott tartománynév-címkének egyedinek kell lennie az Azure-helyen.  
>

### <a name="virtual-machines"></a>Virtual machines (Virtuális gépek)
Egy nyilvános IP-cím is társíthat egy [Windows](../virtual-machines/windows/overview.md) vagy [Linux](../virtual-machines/virtual-machines-linux-about.md) tooits hozzárendelésével VM **hálózati illesztő**. Több hálózati adapterrel rendelkező virtuális hello esetben hozzá lehet rendelni toohello *elsődleges* csak a hálózati illesztőt. Dinamikus vagy egy statikus nyilvános IP cím tooa VM is rendelhet hozzá.

### <a name="internet-facing-load-balancers"></a>Internetkapcsolattal rendelkező terheléselosztók
Egy nyilvános IP-cím is társíthat egy [Azure terheléselosztó](../load-balancer/load-balancer-overview.md), toohello hozzárendelésével terheléselosztó **előtér** konfigurációs. Ez a nyilvános IP-cím terheléselosztásos virtuális IP-címként (VIP) szolgál majd. Dinamikus vagy statikus nyilvános IP cím tooa terheléselosztó előtér-is rendelhet hozzá. Több nyilvános IP címek tooa terheléselosztó előtér-, amely lehetővé teszi, hogy is hozzárendelhetők [multi-VIP](../load-balancer/load-balancer-multivip.md) forgatókönyvek, például az SSL-alapú webhelyek egy több-bérlős környezet.

### <a name="vpn-gateways"></a>VPN-átjárók
[Az Azure VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md) használt tooconnect egy Azure virtuális hálózathoz (VNet) tooother Azure Vnetekhez vagy tooan a helyi hálózaton van. A nyilvános IP-cím tooits tooassign kell **IP-konfiguráció** tooenable azt toocommunicate hello távoli hálózattal. Jelenleg csak rendelhet egy *dinamikus* nyilvános IP-cím tooa VPN átjáró.

### <a name="application-gateways"></a>Alkalmazásátjárók
A nyilvános IP-cím társíthatja egy Azure [Alkalmazásátjáró](../application-gateway/application-gateway-introduction.md), toohello átjáró hozzárendelésével **előtér** konfigurációs. Ez a nyilvános IP-cím terheléselosztásos virtuális IP-címként szolgál majd. Jelenleg csak rendelhet egy *dinamikus* nyilvános IP-címek tooan alkalmazás átjáró előtérbeli konfigurálásához.

### <a name="at-a-glance"></a>Egy pillantásra
hello az alábbi táblázat hello adott tulajdonságra, amelyen keresztül a nyilvános IP-cím lehet társított tooa legfelső szintű erőforrás és a hello lehetséges, hogy foglalási (dinamikus vagy statikus) használható.

| Legfelső szintű erőforrás | IP-cím társítása | Dinamikus | Statikus |
| --- | --- | --- | --- |
| Virtuális gép |Hálózati illesztő |Igen |Igen |
| Terheléselosztó |Előtér-konfiguráció |Igen |Igen |
| VPN-átjáró |Átjáró IP-konfigurációja |Igen |Nem |
| Alkalmazásátjáró |Előtér-konfiguráció |Igen |Nem |

## <a name="private-ip-addresses"></a>Magánhálózati IP-címek
Magán IP-címek engedélyezése az Azure-erőforrások toocommunicate más erőforrásokat egy [virtuális hálózati](virtual-networks-overview.md) vagy a helyszíni hálózatokhoz egy VPN-átjáró vagy ExpressRoute-kapcsolatcsoportot, Internet-elérhető IP-cím használata nélkül.

Hello Azure Resource Manager üzembe helyezési modellel egy magánhálózati IP-cím a következő Azure-erőforrások típusú társított toohello:

* Virtuális gépek
* Belső terheléselosztók (ILB-k)
* Alkalmazásátjárók

### <a name="allocation-method"></a>Lefoglalási módszer
A magánhálózati IP-cím van lefoglalva hello címről hello alhálózati toowhich hello erőforrás tartománya csatlakoztatva van. hello címtartományán hello alhálózatról hello virtuális hálózat címtartománya része.

Két módszer van, amellyel a magánhálózati IP-címek lefoglalhatók: *dinamikus* vagy *statikus*. hello alapértelmezett elosztási módszer *dinamikus*, ahol hello IP-cím automatikusan elvégzi a hello erőforrás alhálózatból (a DHCP használatával). Az IP-címet módosíthatja, állítsa le és indítsa el a hello erőforrás.

Beállíthatja azt hello elosztási módszert túl*statikus* tooensure hello IP-cím marad hello azonos. Ebben az esetben is kell tooprovide egy érvényes IP-címet, amely hello erőforrás alhálózat egy része.

Statikus magánhálózati IP-címeket általában a következő esetekben szoktak használni:

* Tartományvezérlőként vagy DNS-kiszolgálóként működő virtuális gépek esetén.
* IP-címeket használó tűzfalszabályokat igénylő erőforrások esetén.
* Más alkalmazások/erőforrások által IP-címen keresztül elért erőforrások esetén.

### <a name="virtual-machines"></a>Virtual machines (Virtuális gépek)
A magánhálózati IP-cím hozzá van rendelve a toohello **hálózati illesztő** , egy [Windows](../virtual-machines/windows/overview.md) vagy [Linux](../virtual-machines/virtual-machines-linux-about.md) virtuális gép. Több hálózati adapterrel rendelkező virtuális gép esetében mindegyik adapterhez külön lesz egy magánhálózati IP-cím rendelve. A hálózati illesztő statikus vagy dinamikus hello elosztási módszert is megadhat.

#### <a name="internal-dns-hostname-resolution-for-vms"></a>Belső DNS-állomásnév feloldása (virtuális gép esetén)
Minden Azure VM alapértelmezés szerint az [Azure által felügyelt DNS-kiszolgálókkal](virtual-networks-name-resolution-for-vms-and-role-instances.md#azure-provided-name-resolution) van konfigurálva, ha nem konfigurál kifejezetten egyéni DNS-kiszolgálókat. A DNS-kiszolgálók belső névfeloldást biztosít található hello azonos virtuális gépek virtuális hálózat.

A virtuális gépek létrehozásakor a hello állomásnév tooits magánhálózati IP-cím társítás toohello Azure által kezelt DNS-kiszolgálók jelenik meg. A több hálózati adaptert virtuális gép esetén hello állomásnév le van képezve toohello magánhálózati IP-cím hello elsődleges hálózati adapter.

Virtuális gépek Azure által kezelt DNS-kiszolgálók konfigurálva lesz képes tooresolve hello állomásnevek azok a virtuális gépek a virtuális hálózat tootheir privát IP-címek.

### <a name="internal-load-balancers-ilb--application-gateways"></a>Belső terheléselosztók (ILB) és alkalmazásátjárók
Hozzárendelhet egy privát IP-cím toohello **előtér** konfigurációja egy [Azure belső terheléselosztó](../load-balancer/load-balancer-internal-overview.md) (ILB) vagy egy [Azure Application Gateway](../application-gateway/application-gateway-introduction.md). A magánhálózati IP-cím belső végpont funkcionál, elérhető csak toohello erőforrások hello távoli hálózatok és virtuális hálózathoz (VNet) kapcsolódó toohello virtuális hálózat. Vagy a dinamikus vagy statikus magánhálózati IP-címének toohello előtér konfigurációja rendelhet hozzá.

### <a name="at-a-glance"></a>Egy pillantásra
hello az alábbi táblázat hello adott tulajdonságra, amelyen keresztül a magánhálózati IP-cím lehet társított tooa legfelső szintű erőforrás és a hello lehetséges, hogy foglalási (dinamikus vagy statikus) használható.

| Legfelső szintű erőforrás | IP-cím társítása | Dinamikus | Statikus |
| --- | --- | --- | --- |
| Virtuális gép |Hálózati illesztő |Igen |Igen |
| Terheléselosztó |Előtér-konfiguráció |Igen |Igen |
| Alkalmazásátjáró |Előtér-konfiguráció |Igen |Igen |

## <a name="limits"></a>Korlátok
hello határoz meg küszöbértéket az IP-címzés jelzi az hello teljes készletét [korlátozza a hálózati](../azure-subscription-service-limits.md#networking-limits) az Azure-ban. A korlátok régiónként és előfizetésenként értendők. Is [forduljon a támogatási szolgálathoz](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooincrease hello alapértelmezett korlátokat mentése toohello határok a üzleti igények alapján.

## <a name="pricing"></a>Díjszabás
A nyilvános IP-címek kapcsán névleges díjak merülhetnek fel. toolearn további információ az IP-cím felülvizsgálati hello Azure díjszabása [IP-cím árképzési](https://azure.microsoft.com/pricing/details/ip-addresses) lap.

## <a name="next-steps"></a>Következő lépések
* [Virtuális gép és egy statikus nyilvános IP-cím használatával hello Azure-portál telepítése](virtual-network-deploy-static-pip-arm-portal.md)
* [Statikus nyilvános IP-címmel rendelkező virtuális gép telepítése sablon használatával](virtual-network-deploy-static-pip-arm-template.md)
* [A statikus magánhálózati IP-cím hello Azure-portál használatával egy virtuális gép üzembe helyezése](virtual-networks-static-private-ip-arm-pportal.md)
