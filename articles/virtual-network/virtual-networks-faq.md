---
title: "aaaAzure virtuális hálózat – gyakori kérdések |} Microsoft Docs"
description: "Válaszok toohello legtöbb kapcsolatos gyakori kérdések a Microsoft Azure virtuális hálózatot."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
ms.assetid: 54bee086-a8a5-4312-9866-19a1fba913d0
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/18/2017
ms.author: jdial
ms.openlocfilehash: c2f9faee41b9c45e8bb196c58282d597ae732e54
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-virtual-network-frequently-asked-questions-faq"></a>Azure-beli virtuális hálózat gyakori kérdések (GYIK)

## <a name="virtual-network-basics"></a>Virtuális hálózati alapjai

### <a name="what-is-an-azure-virtual-network-vnet"></a>Mi az az Azure Virtual Network (VNet)?
Az Azure Virtual Network (VNet) a saját hálózati hello felhőben megjelenítése. Hello Azure felhőben dedikált tooyour előfizetés logikai elkülönítése is. Használhatja a Vnetek tooprovision és virtuális magánhálózatok (VPN) kezelése az Azure-ban és, ha szükséges, hivatkozás hello Vnetek más Vnetekről az Azure-ban, vagy a helyszíni informatikai infrastruktúra toocreate hibrid vagy létesítmények közötti megoldások. Minden egyes virtuális hálózat létrehozása a saját CIDR-blokkja rendelkezik, és a csatolt tooother virtuális hálózatokat és a helyi hálózatok lehetnek, amíg hello CIDR-blokkok nem lehetnek átfedésben. Akkor is, és a DNS-kiszolgáló beállításainak Vnetek ellenőrzése, hello hálózatok alhálózatokra szegmentálását.

A Vnetek használja:

* Hozzon létre egy dedikált titkos csak felhőalapú VNet néha nem feltétlenül szükséges a létesítmények közötti konfigurációs megoldást. Amikor létrehoz egy Vnetet, a szolgáltatások és a virtuális hálózaton belüli virtuális gépek kommunikálhatnak közvetlenül és biztonságosan egymással hello felhőben. Tartja a forgalom biztonságosan hello virtuális hálózaton belül, de továbbra is lehetővé teszi a tooconfigure végpont kapcsolatok hello virtuális gépek és szolgáltatások internetes kommunikáció a megoldás részeként igénylő.
* Biztonságos az Adatközpont a Vnetek meghosszabbításához hozhat létre hagyományos-webhelyek (közötti S2S) VPN toosecurely méretezési a datacenter kapacitás. S2S VPN IPSEC tooprovide a vállalati VPN-átjáró és az Azure közötti biztonságos kapcsolatot használjon.
* Enable hibrid felhős rendszerekben Vnetek Önnek hello rugalmasságot toosupport széles hibrid felhős rendszerekben. Felhőalapú alkalmazások tooany típusú Nagyszámítógépek például a helyszíni rendszer és a Unix rendszerek biztonságosan kapcsolódhatnak.

### <a name="how-do-i-know-if-i-need-a-vnet"></a>Hogyan állapítható meg, hogy ha egy virtuális hálózatot kell-e?
Hello [virtuális hálózat áttekintése](virtual-networks-overview.md) a cikk ismerteti, amely segít eldönteni hello ajánlott hálózati tervezési beállítás, döntési tábla.

### <a name="how-do-i-get-started"></a>Hogyan kezdhetek hozzá?
Látogasson el a hello [virtuális hálózati dokumentáció](https://docs.microsoft.com/azure/virtual-network/) tooget elindult. Ez a tartalom összes hello virtuális hálózat szolgáltatás áttekintése és a központi telepítési információkat nyújt.

### <a name="can-i-use-vnets-without-cross-premises-connectivity"></a>Használhatok Vnetek közötti kapcsolatot nyújthassanak nélkül?
Igen. Egy VNet hibrid kapcsolat használata nélkül is használhatja. Ez különösen fontos, ha azt szeretné, hogy toorun Microsoft Windows Server Active Directory-tartományvezérlőket és a SharePoint-farmok az Azure-ban.

### <a name="can-i-perform-wan-optimization-between-vnets-or-a-vnet-and-my-on-premises-data-center"></a>Végezheti a WAN-optimalizálást Vnetekhez vagy egy VNet és a helyszíni adatközpont között?

Igen. Telepíthet egy [WAN-optimalizálást hálózati virtuális készülék](https://azure.microsoft.com/marketplace/?term=wan+optimization) több szállítók hello Azure piactéren keresztül.

## <a name="configuration"></a>Konfiguráció

### <a name="what-tools-do-i-use-toocreate-a-vnet"></a>Eszközök mire toocreate VNet használhatom?
Használja a következő eszközök toocreate hello, vagy adja meg a virtuális hálózat:

* Azure-portálra (a klasszikus és Resource Manager Vnetek).
* A hálózati konfigurációs fájl (netcfg - csak a hagyományos Vneteket). Lásd: hello [egy Vnetet, a hálózati konfigurációs fájl segítségével konfigurálja](virtual-networks-using-network-configuration-file.md) cikk.
* PowerShell (a klasszikus és Resource Manager Vnetekről).
* Az Azure parancssori felület (a klasszikus és Resource Manager Vnetekről).

### <a name="what-address-ranges-can-i-use-in-my-vnets"></a>Milyen címtartományai lehet használni a Vnetekhez?
Meghatározott összes IP-címtartomány [RFC 1918](http://tools.ietf.org/html/rfc1918). Például 10.0.0.0/16.

### <a name="can-i-have-public-ip-addresses-in-my-vnets"></a>Is nyilvános IP-címeket is rendelkeznek a saját Vnetekhez?
Igen. Nyilvános IP-címtartományok kapcsolatos további információkért lásd: hello [nyilvános IP-címtér része virtuális hálózatnak](virtual-networks-public-ip-within-vnet.md) cikk. A nyilvános IP-címek csak akkor érhető el közvetlenül az internethez hello.

### <a name="is-there-a-limit-toohello-number-of-subnets-in-my-vnet"></a>A VNet alhálózatainak számának korlátozása toohello van?
Igen. Olvasási hello [Azure korlátozza](../azure-subscription-service-limits.md#networking-limits) cikkben alább. Alhálózati címterek nem átfedik egymást.

### <a name="are-there-any-restrictions-on-using-ip-addresses-within-these-subnets"></a>Vannak-e bármilyen korlátozás belül ezek alhálózatok IP-címeket használnak?
Igen. Azure fenntartja az egyes IP-címek minden alhálózaton belül. hello hello alhálózatok első és utolsó IP-címek számára fenntartott protokoll megfelelési, Azure-szolgáltatásokhoz használt 3 további címmel együtt.

### <a name="how-small-and-how-large-can-vnets-and-subnets-be"></a>Hogyan kis és nagy hogyan lehet virtuális hálózatokat és alhálózatokat kell?
hello támogatott legkisebb alhálózat egy /29 és legnagyobb hello egy /8 (CIDR alhálózati definíciók használatával).

### <a name="can-i-bring-my-vlans-tooazure-using-vnets"></a>Hozzáadhatom a VLAN-OK tooAzure használatával Vnetekhez?
Nem. Vnetek 3. rétegbeli átfedések. Azure nem támogatja a 2. rétegbeli szemantikáját.

### <a name="can-i-specify-custom-routing-policies-on-my-vnets-and-subnets"></a>Egyéni útválasztási házirend megadhatok a Vnetek és alhálózatok?
Igen. Felhasználó definiált útválasztás (UDR) is használhatja. További információt a UDR [felhasználó által megadott útvonalak és IP-továbbítás](virtual-networks-udr-overview.md).

### <a name="do-vnets-support-multicast-or-broadcast"></a>Csoportos küldéses vagy szórt támogatják Vnetekhez?
Nem. Csoportos küldéses vagy szórt nem támogatott.

### <a name="what-protocols-can-i-use-within-vnets"></a>Milyen protokollok Vnetek belül használható?
TCP, UDP és ICMP TCP/IP protokollt Vnetek belül is használhatja. Csoportos küldés, szórás, IP-be bújtatott IP encapsulated, és Generic Routing Encapsulation (GRE) csomagot a rendszer letiltotta Vnetek belül. 

### <a name="can-i-ping-my-default-routers-within-a-vnet"></a>Megpingelheti a saját alapértelmezett útválasztók egy Vneten belül?
Nem.

### <a name="can-i-use-tracert-toodiagnose-connectivity"></a>Használható tracert toodiagnose kapcsolatot?
Nem.

### <a name="can-i-add-subnets-after-hello-vnet-is-created"></a>Bővíthető alhálózatok hello virtuális hálózat létrejötte után?
Igen. Alhálózatok felveheti tooVNets bármikor, amíg hello alhálózati cím nem része egy másik alhálózatnak a virtuális hálózat hello.

### <a name="can-i-modify-hello-size-of-my-subnet-after-i-create-it"></a>Az létrehozása után is módosítani a alhálózati hello mérete?
Igen. Hozzáadása, eltávolítása, bővítése vagy zsugorítása alhálózatot, ha nincsenek virtuális gépek és szolgáltatások telepítése környezetben.

### <a name="can-i-modify-subnets-after-i-created-them"></a>Miután létrehozott azokat is módosítani alhálózatok?
Igen. Hozzáadhat, távolítsa el, és módosítsa a virtuális hálózat által használt hello CIDR-blokkok.

### <a name="can-i-connect-toohello-internet-if-i-am-running-my-services-in-a-vnet"></a>Csatlakoztathatok toohello internet fut, de a szolgáltatások a Vneten belül?
Igen. Egy Vneten belül telepített összes szolgáltatás kapcsolódhatnak toohello internet. Minden Azure szolgáltatásba telepített felhőszolgáltatás rendelkezik hozzárendelt tooit nyilvánosan címmel rendelkező virtuális IP-címhez. Hogy toodefine bemeneti végpontja PaaS szerepkörök és a virtuális gépek tooenable végpontjainak hello ezen szolgáltatások tooaccept kapcsolatokat interneten.

### <a name="do-vnets-support-ipv6"></a>Támogatják a Vnetek IPv6?
Nem. A Vnetek IPv6 jelenleg nem használható.

### <a name="can-a-vnet-span-regions"></a>Egy VNet régiókban is kiterjedhet?
Nem. Egy VNet korlátozott tooa egyetlen régióban.

### <a name="can-i-connect-a-vnet-tooanother-vnet-in-azure"></a>Csatlakozhatnak egy virtuális hálózat tooanother Vnetet az Azure-ban?
Igen. Egy VNet tooanother virtuális hálózat használatával kapcsolódhatnak:
- Az Azure VPN-átjáró. Olvasási hello [VNet – VNet-kapcsolatot konfiguráló](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md) cikkben alább. 
- Vnetben társviszony-létesítés. Olvasási hello [Vnetben társviszony-létesítési áttekintése](virtual-network-peering-overview.md) cikkben alább.

## <a name="name-resolution-dns"></a>Névfeloldás (DNS)

### <a name="what-are-my-dns-options-for-vnets"></a>Mik azok a DNS-beállítások tartozó Vnetek esetében?
Használjon hello döntési táblát a hello [névfeloldás virtuális gépek és a Szerepkörpéldányok](virtual-networks-name-resolution-for-vms-and-role-instances.md) le a összes hello DNS lap tooguide lehetőségekről.

### <a name="can-i-specify-dns-servers-for-a-vnet"></a>Képes-e meg DNS-kiszolgálók egy virtuális hálózatot?
Igen. DNS-kiszolgáló IP-címeket adhat meg hello VNet beállításait. Ez hello virtuális hálózat virtuális gépeinek hello alapértelmezett DNS-kiszolgálói lesz alkalmazva.

### <a name="how-many-dns-servers-can-i-specify"></a>DNS-kiszolgálók számának adhat meg?
Hivatkozás hello [Azure korlátozza](../azure-subscription-service-limits.md#networking-limits) cikkben alább.

### <a name="can-i-modify-my-dns-servers-after-i-have-created-hello-network"></a>Miután létrehozott hello hálózati is módosítani a DNS-kiszolgálók?
Igen. Hello DNS-kiszolgálók listája bármikor módosíthatja a virtuális hálózat. Ha módosítja a DNS-kiszolgálók listája, szüksége lesz toorestart minden hello virtuális gépek ahhoz a vnetben toopick hello új DNS-kiszolgáló fel.

### <a name="what-is-azure-provided-dns-and-does-it-work-with-vnets"></a>Mi az Azure által biztosított DNS- és működik a Vnetekhez?
Azure által biztosított DNS-a Microsoft által kínált több-bérlős DNS-szolgáltatás. Azure regisztrál az összes virtuális gépek és felhő szerepkör szolgáltatáspéldány ezt a szolgáltatást. Ezt a szolgáltatást biztosít a névfeloldás állomásnév szerint virtuális gépek és a szerepkörpéldányok belüli hello ugyanaz a felhőalapú szolgáltatás, és teljes tartománynevét a virtuális gépek és a szerepkörpéldányok hello azonos virtuális hálózaton. Olvasási hello [névfeloldás virtuális gépek és a Szerepkörpéldányok](virtual-networks-name-resolution-for-vms-and-role-instances.md) cikk toolearn DNS szolgáltatással kapcsolatos további.

> [!NOTE]
> Nincs: az idő toohello korlátozása először 100 felhőszolgáltatások egy Vnetet az Azure által biztosított DNS-sel, több-bérlős névfeloldás. Ha a saját DNS-kiszolgálót használ, ez a korlátozás nem vonatkozik.
> 
> 

### <a name="can-i-override-my-dns-settings-on-a-per-vm--service-basis"></a>Bírálja felül a DNS-beállítások a /-virtuális gép / service alapján?
Igen. DNS-kiszolgálók adhatók meg a felhő szolgáltatás alapján toooverride hello alapértelmezett hálózati beállításait. Azt javasoljuk azonban, hogy a lehető legnagyobb hálózati kiterjedő DNS használja.

### <a name="can-i-bring-my-own-dns-suffix"></a>Hozzáadhatom a saját DNS-utótag?
Nem. A Vnetek esetében nem adható meg egy egyéni DNS-utótagot.

## <a name="connecting-virtual-machines"></a>Csatlakozás a virtuális gépek

### <a name="can-i-deploy-vms-tooa-vnet"></a>Telepítheti a virtuális gépek tooa virtuális hálózatot?
Igen. Hálózati adapterek (NIC) kapcsolódó összes tooa hello Resource Manager üzembe helyezési modellben telepített virtuális gép csatlakoztatott tooa virtuális hálózaton kell lennie. Hello klasszikus telepítési modell használatával telepített virtuális gépek is lehet csatlakoztatott tooa virtuális hálózat.

### <a name="what-are-hello-different-types-of-ip-addresses-i-can-assign-toovms"></a>Mik azok a hello különböző IP-címek hozzárendelek is tooVMs?
* **Saját:** hozzárendelt tooeach NIC minden egyes virtuális Gépen belül. hello cím hozzá van rendelve, vagy hello statikus vagy dinamikus metódussal. Magán IP-címek a virtuális hálózat hello alhálózati beállítások megadott hello tartományt rendeli. Hello klasszikus üzembe helyezési modellben telepített erőforrásokhoz rendelt magánhálózati IP-címek, még akkor is, ha még nincs csatlakoztatva virtuális hálózat tooa. A magánhálózati IP-cím hozzárendelve hello dinamikus módszer továbbra is hozzárendelt tooa erőforrás hello erőforrás törléséig (virtuális gépek vagy a Felhőszolgáltatás üzembe helyezési). A magánhálózati IP-címet, dinamikus metódus hello rendelve módosíthatja, ha egy virtuális gép újraindítása után a hello lett leállítva (felszabadított) állapotát. A magánhálózati IP-cím hello statikus metódus rendelve marad tooa erőforrás rendelve, amíg nem hello erőforrás. Ha tooensure van szüksége, hogy hello magánhálózati IP-cím erőforrás soha nem változik, amíg nem hello erőforrás, egy magánhálózati IP-cím hozzárendelése hello statikus metódussal.
* **Nyilvános:** opcionálisan hozzárendelt tooNICs csatolt hello Azure Resource Manager üzembe helyezési modellben telepített tooVMs. hello cím hello statikus vagy dinamikus kiosztási módszerrel rendelhetők hozzá. Minden virtuális gépek és Felhőszolgáltatások szerepkörpéldányokat hello klasszikus üzembe helyezési modellben telepített létezik belül egy felhőalapú szolgáltatás, amely hozzá van rendelve egy *dinamikus*, nyilvános virtuális IP-címe. Nyilvános *statikus* IP-cím, az úgynevezett egy [lefoglalt IP-cím](virtual-networks-reserved-public-ip.md), opcionálisan rendelhető virtuális IP-címhez. IP-címek nyilvános tooindividual hello klasszikus üzembe helyezési modellben telepített virtuális gépek vagy Felhőszolgáltatások szerepkörpéldányokat rendelhet hozzá. Ezek az úgynevezett [példány szintű nyilvános IP-cím (ILPIP](virtual-networks-instance-level-public-ip.md) megoldást, és dinamikusan is hozzárendelhető.

### <a name="can-i-reserve-a-private-ip-address-for-a-vm-that-i-will-create-at-a-later-time"></a>Lehet-e foglalni a magánhálózati IP-címet, amely I létrehoz egy későbbi időpontban?
Nem. A magánhálózati IP-címet nem sikerült lefoglalni. Ha a magánhálózati IP-cím áll rendelkezésre a rendszer hozzárendel tooa virtuális gép vagy szerepkör példány hello DHCP-kiszolgáló. Hogy a virtuális gép is, vagy nem lehet egy hello privát IP cím toobe hozzárendelni kívánt hello. Hello magánhálózati IP-cím már létrehozott virtuális gép tooany elérhető privát IP-cím azonban módosíthatja.

### <a name="do-private-ip-addresses-change-for-vms-in-a-vnet"></a>Hajtsa végre a magánhálózati IP-címek módosítása virtuális gépekhez a Vneten belül?
Ez a konkrét licenctől függ. Magánhálózati IP-címek dinamikus marad a virtuális gép a Leállítva (felszabadítva), vagy törölték. Statikus magánhálózati IP-címek nem ki a virtuális gép, amíg nem törli.

### <a name="can-i-manually-assign-ip-addresses-toonics-within-hello-vm-operating-system"></a>Szeretnék kézzel rendelhetik hozzá IP-címek tooNICs hello virtuális gép operációs rendszerben?
Igen, de nem ajánlott. Manuálisan a virtuális gép operációs rendszerében a hálózati adapter a hello IP-cím megváltoztatása eredményezhet, toolosing kapcsolat toohello VM toochange mintha hello IP-cím tooa NIC hello Azure virtuális Gépen belül.

### <a name="what-happens-toomy-ip-addresses-if-i-stop-a-cloud-service-deployment-slot-or-shutdown-a-vm-from-within-hello-operating-system"></a>Mi történik a toomy IP-címeket, ha egy felhőalapú szolgáltatás üzembe helyezési pont vagy -leállítás hello operációs rendszerben a virtuális gép le?
Semmi sem. hello IP-címek (nyilvános VIP, nyilvános és titkos) hozzárendelt toohello cloud service üzembe helyezési pont vagy a virtuális gép maradnak. Dinamikus címek csak egy virtuális gép leállítása (felszabadított) kiadott vagy törölték, vagy egy felhőalapú szolgáltatás üzembe helyezési pont törlődik. Gombra kattintva hello **leállítása** számára a virtuális gépek hello Azure-portálon belül beállítja az állapot tooStopped (felszabadítva) gombra. Ebben az esetben a hello VM az IP-címek elvesznek.

### <a name="can-i-move-vms-from-one-subnet-tooanother-subnet-in-a-vnet-without-re-deploying"></a>Helyezhető át virtuális gépeket egy alhálózat tooanother alhálózatból a Vneten belül újbóli telepítése nélkül?
Igen. További információt talál a hello [hogyan toomove egy virtuális gép vagy szerepkör példány tooa másik alhálózat](virtual-networks-move-vm-role-to-subnet.md) cikk.

### <a name="can-i-configure-a-static-mac-address-for-my-vm"></a>Beállítható egy statikus MAC-címet a virtuális géphez?
Nem. A MAC-cím statikusan nem állítható be.

### <a name="will-hello-mac-address-remain-hello-same-for-my-vm-once-it-has-been-created"></a>MAC-cím hello marad hello ugyanaz a virtuális gép létrehozása után?
Igen, a MAC-cím hello hello hello Resource Manager és klasszikus üzembe helyezési modellre is ugyanaz a virtuális gépek telepítve, amíg nem törli marad. Hello MAC-címet jelent korábban, ha hello virtuális gép le lett állítva (felszabadított), de most a hello MAC-cím addig megmarad, akkor is, amikor hello VM a hello állapot felszabadítása.

### <a name="can-i-connect-toohello-internet-from-a-vm-in-a-vnet"></a>Csatlakoztathatok toohello a Vneten belül VM internet?
Igen. Egy Vneten belül telepített összes virtuális gépek és Felhőszolgáltatások szerepkörpéldányokat toohello Internet is elérheti.

## <a name="azure-services-that-connect-toovnets"></a>Azure-szolgáltatásokhoz kapcsolódó tooVNets

### <a name="can-i-use-azure-app-service-web-apps-with-a-vnet"></a>Használható egy Vnetet az Azure App Service Web Apps?
Igen. Webes alkalmazásokat belül egy Vnetet egy ASE (App Service Environment-környezet) segítségével telepíthet. Minden Web Apps biztonságosan csatlakozzon, és az Azure virtuális hálózat a erőforrások elérését, ha a virtuális hálózat beállítása pont – hely kapcsolat. További információkért tekintse meg a következő cikkek hello:

* [Webalkalmazások létrehozása az App Service-környezet](../app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md)
* [Az alkalmazás integrálása az Azure virtuális hálózat](../app-service-web/web-sites-integrate-with-vnet.md)
* [A webalkalmazások virtuális integráció és a hibrid kapcsolatok használja](../app-service-web/web-sites-integrate-with-vnet.md#hybrid-connections-and-app-service-environments)

### <a name="can-i-deploy-cloud-services-with-web-and-worker-roles-paas-in-a-vnet"></a>Központi telepítését Felhőszolgáltatások webes és feldolgozói szerepkörök (PaaS) egy virtuális hálózatot?
Igen. (Opcionális) telepítése Felhőszolgáltatások szerepkörpéldányokat Vnetek belül. toodo úgy, adja meg hello virtuális hálózat nevét és hello szerepkör/alhálózati hozzárendelések hello hálózati konfigurációs szakasz az szolgáltatás konfigurációját. Nem kell tooupdate a bináris fájlok bármelyike.

### <a name="can-i-connect-a-virtual-machine-scale-set-vmss-tooa-vnet"></a>Csatlakozás a virtuális gép méretezési beállítása (VMSS) tooa VNet is?
Igen. Egy VNet VMSS tooa kell csatlakoztatni.

### <a name="can-i-move-my-services-in-and-out-of-vnets"></a>Áthelyezheti a szolgáltatások mindkét Vnetekhez?
Nem. Mindkét Vnetek szolgáltatások nem helyezhető át. Rendszer toodelete rendelkezik, és helyezze újra üzembe a hello szolgáltatás toomove azt tooanother virtuális hálózat.

## <a name="security"></a>Biztonság

### <a name="what-is-hello-security-model-for-vnets"></a>Mi az hello biztonsági modell Vnetekhez?
Vnetek teljesen elkülönülnek egymástól, és más szolgáltatásokban tárolt hello Azure-infrastruktúra. A virtuális hálózat egy megbízhatósági kapcsolat határán.

### <a name="can-i-restrict-inbound-or-outbound-traffic-flow-toovnet-connected-resources"></a>Korlátozhatja a bejövő vagy kimenő forgalom folyamat tooVNet kapcsolódó erőforrások?
Igen. Alkalmazhat [hálózati biztonsági csoportok](virtual-networks-nsg.md) tooindividual alhálózatok történik egy Vneten belül a hálózati adapterek csatolt tooa virtuális hálózatot, vagy mindkettőt.

### <a name="can-i-implement-a-firewall-between-vnet-connected-resources"></a>Virtuális hálózat kapcsolódó erőforrások között tűzfal is létrehozható?
Igen. Telepíthet egy [tűzfal hálózati virtuális készülék](https://azure.microsoft.com/en-us/marketplace/?term=firewall) több szállítók hello Azure piactéren keresztül.

### <a name="is-there-information-available-about-securing-vnets"></a>Van információk védelme Vnetek elérhető?
Igen. Lásd: hello [Azure biztonsági áttekintése](../security/security-network-overview.md) cikkben alább.

## <a name="apis-schemas-and-tools"></a>API-k, sémákkal és eszközök

### <a name="can-i-manage-vnets-from-code"></a>Kezelhetem Vnetek kódból?
Igen. Használható a Vnetek REST API-k hello [Azure Resource Manager](https://msdn.microsoft.com/library/mt163658.aspx) és [klasszikus (kezelő)](http://go.microsoft.com/fwlink/?LinkId=296833)) üzembe helyezési modellek.

### <a name="is-there-tooling-support-for-vnets"></a>Van tooling támogatása Vnetekhez?
Igen. További információ:
- az Azure portál toodeploy Vnetek keresztül hello hello [Azure Resource Manager](virtual-networks-create-vnet-arm-pportal.md) és [klasszikus](virtual-networks-create-vnet-classic-pportal.md) üzembe helyezési modellek.
- PowerShell toomanage Vnetek ügyfélgépekre hello [erőforrás-kezelő](/powershell/resourcemanager/azurerm.network/v3.1.0/azurerm.network.md) és [klasszikus](/powershell/module/azure/?view=azuresmps-3.7.0) üzembe helyezési modellek.
- Hello [Azure parancssori felület (CLI)](../virtual-machines/azure-cli-arm-commands.md#azure-network-commands-to-manage-network-resources) toomanage Vnetek két üzembe helyezési modell ügyfélgépekre.  
