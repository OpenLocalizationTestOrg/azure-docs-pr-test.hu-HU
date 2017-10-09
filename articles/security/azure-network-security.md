---
title: "Hálózati biztonsági aaaAzure |} Microsoft Docs"
description: "További információk a felhőalapú számítási szolgáltatás, amely tartalmazza a számítási példányokért széles kijelölt & szolgáltatások automatikusan toomeet hello igényeinek az alkalmazás vagy a vállalati fel és le is méretezhető."
services: security
documentationcenter: na
author: UnifyCloud
manager: swadhwa
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/24/2017
ms.author: TomSh
ms.openlocfilehash: 7dae83bbe338a2727575447583a7fb773527dd59
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-network-security"></a>Az Azure hálózati biztonság

Tudjuk, hogy biztonsági-e a feladat hello felhőben, és hogy mennyire fontos, hogy található-e az Azure biztonsági pontos és időben információt. Hello legjobb okok toouse Azure az alkalmazások és szolgáltatások egyik tootake Azure széles köréről a biztonsági eszközök és képességek előnyeit. Ezekkel az eszközökkel és képességek segítségével lehetséges toocreate biztonságos megoldások elérhetővé hello Azure platformon.

A Microsoft Azure a titkosítás, integritás és rendelkezésre állás ügyféladatok, biztosítása átlátszó elszámolási kötelezettségéről szóló biztosít. toohelp jobban megismerte hálózati biztonsági vezérlők hello az ügyfél szempontjából a Microsoft Azure-ban végrehajtott hello gyűjteménye, ez, "Azure hálózati biztonság", a cikk tooprovide egy átfogó pillantást hello hálózati biztonság a vezérlő érhető el a Microsoft Azure-ban.

A dokumentum célja hello azokat a hálózati kapcsolatban szabályozza, hogy tooinform tooenhance hello biztonsági telepít hello megoldások konfigurálhatja az Azure-ban. Ha érdekli milyen Microsoft does toosecure hello hálózati háló a hello Azure platformon, magát, a hello hello Azure biztonsági című [Microsoft Trust Center](https://www.microsoft.com/trustcenter/security/azure-security).

## <a name="azure-platform"></a>Azure-platform

Azure egy nyilvános felhőszolgáltatási platform, amely támogatja az operációs rendszerek, programozási nyelvek, keretrendszerek, eszközök, adatbázisok és eszközök széles körű kijelölés.  Linux-tárolók futtatható a Docker integrációja; alkalmazások, JavaScript, Python, .NET, PHP, Java és Node.js; build vissza-végpontok iOS, Android és Windows eszközökhöz. Azure felhőszolgáltatások hello azonos technológiák több millió fejlesztők és informatikai szakemberek számára már alapulnak, és megbízható támogatja.

Amikor létrehozása vagy áttelepítése informatikai eszközök, egy nyilvános felhőszolgáltatóhoz, akkor vannak hagyatkoznia adott szervezet képességek tooprotect az alkalmazások és adatok hello szolgáltatások és hello vezérlők toomanage hello biztonságát, a felhő alapú biztosítanak eszközök.

Azure infrastruktúrája a hello létesítmény tooapplications felhasználók millióit egyidejűleg üzemeltetéséhez készült, és egy megbízható foundation, amelyre vállalatok megfelel a biztonsági követelményeknek biztosít. Emellett Azure biztosít konfigurálható biztonsági beállítások és hello képességét toocontrol rengeteg őket, hogy a biztonsági toomeet hello egyéni követelményeinek a szervezet központi telepítések szabhatja testre.

## <a name="abstract"></a>Absztrakt

Nyilvános felhőalapú Microsoft-szolgáltatás biztosításához kapacitású szolgáltatások és az infrastruktúra, a vállalati szintű képességet és a számos választhat való hibrid kapcsolathoz. Választhat tooaccess hello interneten keresztül vagy a magánhálózati kapcsolatot biztosít, az Azure ExpressRoute ezeket a szolgáltatásokat. hello Microsoft Azure platform lehetővé teszi a tooseamlessly hello felhőbe bővítheti a infrastruktúrát, és hozhat létre többrétegű architektúrák. Emellett harmadik felek engedélyezéséhez bővített szolgáltatásokat nyújtó biztonsági szolgáltatásait és a virtuális készülékek.

Azure hálózati szolgáltatások maximalizálása érdekében a rugalmasságot, a rendelkezésre állási, a rugalmasság, a biztonságának és integritásának úgy lett kialakítva. Ez a dokumentum biztosít hello Azure hálózati funkcióit a részletek és a felhasználók használatát Azure natív biztonsági szolgáltatások toohelp információk védelme információs.

hello készült e tanulmány célközönség tartalmazza:

- Műszaki kezelők, a hálózati rendszergazdák és fejlesztők számára elérhető és támogatott az Azure biztonsági megoldásokat keres.

-   Kis-és középvállalkozások vagy üzleti folyamat vezetők számára, akik tooget magas szintű áttekintését hello Azure hálózati technológiákat és szolgáltatásokat beszélgetéseket körül hello Azure nyilvános felhőjében a hálózati biztonság szempontjából.

## <a name="azure-networking-big-picture"></a>Azure-hálózat nagy vonalakban tekinti
Microsoft Azure tartalmaz egy robusztus hálózati infrastruktúra toosupport, az alkalmazás és szolgáltatás hálózati kapcsolati követelményeinek. Hálózati kapcsolat lehetséges helyszíni között, az Azure-ban lévő erőforrások között, és Azure megtalálható erőforrásokhoz, és az hello Internet tooand és az Azure.

![Azure-hálózat nagy vonalakban tekinti](media/azure-network-security/azure-network-security-fig-1.png)

Hello [Azure hálózati infrastruktúra](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-networking-guidelines) lehetővé teszi, hogy toosecurely Csatlakozás más Azure-erőforrások tooeach virtuális hálózatokról (Vnetekről). A virtuális hálózat a saját hálózati hello felhőben megjelenítése. Egy virtuális hálózat a logikai elkülönítés hello Azure felhőben dedikált hálózati tooyour előfizetés. Vnetek tooyour helyszíni hálózatokhoz is elérheti.

Az Azure által támogatott dedikált WAN-kapcsolaton kapcsolat tooyour a helyszíni hálózat és egy Azure virtuális hálózat [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction). hello hivatkozás Azure és a webhely között, amelyek nem halad át hello dedikált kapcsolatot használ nyilvános internethez. Ha több adatközpontot az Azure alkalmazás fut, akkor használhatja [Azure Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview) tooroute kérelmeinek intelligens módon különböző hello alkalmazás példányát. Nem fut az Azure-ban, ha azok elérhetők a hello internetes forgalom tooservices is irányíthatja.

## <a name="enterprise-view-of-azure-networking-components"></a>Vállalati nézet Azure hálózati összetevők
Azure számos hálózati összetevők, amelyek megfelelő toonetwork biztonsági beszélgetéseket rendelkezik. azt ismerteti, hogy a hálózati összetevők, és helyezi a hangsúlyt hello biztonsági problémák kapcsolódó toothem.

> [!Note]
> Nem minden szempontját Azure hálózatkezelés ismerteti – arról lesz szó csak azok tér toobe döntő tervezése és kialakítása során a biztonságos hálózati infrastruktúra a szolgáltatások és alkalmazások központi telepítése az Azure-ban.

A dokumentum fog kell borítóján hello a következő Azure hálózati vállalati lehetőségeket:

-   Alapvető hálózati kapcsolat

-   A hibrid kapcsolat

-   Biztonsági vezérlőket

-   Hálózat érvényességének ellenőrzése

### <a name="basic-network-connectivity"></a>Alapvető hálózati kapcsolat

Hello [Azure Virtual Network](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) szolgáltatás lehetővé teszi, hogy Ön toosecurely más Azure-erőforrások tooeach csatlakozzon a virtuális hálózatokkal (VNet). A virtuális hálózat a saját hálózati hello felhőben megjelenítése. Egy virtuális hálózat a logikai elkülönítés hello Azure hálózati infrastruktúra dedikált tooyour előfizetés. Más Vnetekről tooeach elérhetők, és tooyour helyszíni pont-pont VPN hálózatokhoz és dedikált [WAN-kapcsolatok](https://docs.microsoft.com/azure/expressroute/expressroute-introduction).

![Alapvető hálózati kapcsolat](media/azure-network-security/azure-network-security-fig-2.png)

Hello ismertetése a virtuális gépek toohost kiszolgálók használhatja az Azure-ban hello kérdés esetén hogyan a virtuális gépek tooa hálózati kapcsolódnak. hello válasz, hogy a virtuális gépek csatlakozni tooan [Azure Virtual Network](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview).

Az Azure virtuális hálózatok vannak hello virtuális hálózatok, mint a helyszíni használni a saját virtualizálási platform megoldások, például a Microsoft Hyper-V vagy VMware.

#### <a name="intra-vnet-connectivity"></a>Helyen belüli-VNet-kapcsolatot

Csatlakozhat a más Vnetekről tooeach, erőforrások engedélyezése tooeither VNet toocommunicate egymással keresztül kapcsolódó Vnetek. Valamelyike vagy mindegyike a következő beállítások tooconnect Vnetek tooeach más hello használhatja:

- **Társviszony-létesítés:** lehetővé teszi, hogy erőforrásokat csatlakoztatott toodifferent Azure Vnetekhez hello belül azonos Azure-beli hely toocommunicate egymással. hello sávszélesség és a késleltetés között vnet hello hello ugyanaz, mintha hello erőforrások csatlakoztatott toohello ugyanazt a virtuális hálózatot. toolearn társviszony-létesítést, kapcsolatos további információkért olvassa el [virtuális hálózati társviszony-létesítés](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview).

 ![Társviszony-létesítés](media/azure-network-security/azure-network-security-fig-3.png)

- **VNet – VNet-kapcsolatot:** lehetővé teszi, hogy erőforrásokat csatlakoztatott toodifferent Azure virtuális hálózaton belül hello ugyanazon vagy másik Azure helyét. Ellentétben a társviszony-létesítést, sávszélesség oka Vnetek közötti forgalmat kell áramlása az Azure VPN Gateway.

![VNet – VNet-kapcsolatot](media/azure-network-security/azure-network-security-fig-4.png)


További információk a Vneteket összekötő egy VNet – VNet kapcsolattal, olvassa el a hello toolearn [konfigurálja a VNet – VNet-kapcsolatot cikk](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal?toc=%2fazure%2fvirtual-network%2ftoc.json).

#### <a name="azure-virtual-network-capabilities"></a>Azure-beli virtuális hálózat képességek:

Ahogy látja, az Azure virtuális hálózat virtuális gépek tooconnect toohello hálózati biztosít, úgy, hogy csatlakozhassanak a tooother hálózati erőforrások biztonságos módon. Hálózati kapcsolat viszont csak hello elején. hello hello Azure Virtual Network szolgáltatás az alábbi képességeket teszi közzé hello Azure Virtual Network security jellemzői:

-   Elkülönítés

-   Internetkapcsolat

-   Az Azure erőforrás-kapcsolat

-   VNet-kapcsolatot

-   Helyszíni kapcsolatok

-   Forgalomszűrést végez

-   Útválasztás

**Elkülönítés**

Vnetek [elkülönített](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) egymástól. Létrehozhat külön Vnetek fejlesztési, tesztelési és használata hello azonos üzemi [CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) blokkok cím. Ezzel szemben, amelyek különböző CIDR címblokkokat és összekapcsolhatja az hálózatok több Vnetek is létrehozhat. Egy VNet érdekében több alhálózatra is szegmentálhatja.

Azure belső névfeloldást biztosít a virtuális gépek és [Felhőszolgáltatások](https://azure.microsoft.com/services/cloud-services/) szerepkörpéldányokat tooa VNet csatlakoztatva. Konfigurálhatja a virtuális hálózat toouse saját DNS-kiszolgálókat, az Azure belső névfeloldást használata helyett.

Minden Azure belül több Vnetek Megvalósíthat [előfizetés](https://docs.microsoft.com/azure/azure-glossary-cloud-terminology?toc=%2fazure%2fvirtual-network%2ftoc.json) és az Azure [régió](https://docs.microsoft.com/azure/azure-glossary-cloud-terminology?toc=%2fazure%2fvirtual-network%2ftoc.json). Minden egyes virtuális hálózat el különítve a más Vnetekről. Minden egyes vnet a következő műveletek végezhetők el:

-   Adjon meg egy egyéni privát IP-címtér nyilvános és titkos (az RFC 1918) címeket használnak. Azure rendeli hozzá az erőforrásokhoz csatlakoztatott toohello VNet egy magánhálózati IP-cím hello címterület, rendeli.

-   Hello VNet be egy vagy több alhálózatból szegmentálni, és hello VNet terület tooeach címalhálózata része lefoglalni.

-   Azure által biztosított névfeloldás használata, vagy adja meg az erőforrások használhatják a saját DNS-kiszolgáló csatlakoztatva tooa virtuális hálózat. További információk a névfeloldás a Vneteket, olvassa el a hello toolearn [névfeloldását virtuális gépek és Felhőszolgáltatások](https://docs.microsoft.com/azure/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances).

**Internetkapcsolat**

Minden [Azure virtuális gépek (VM)](https://docs.microsoft.com/azure/virtual-machines/windows/) és Felhőszolgáltatások szerepkörpéldányokat csatlakoztatott tooa VNet rendelkezik hozzáférési toohello Internet, alapértelmezés szerint. Bejövő hozzáférést toospecific erőforrások, igény szerint is engedélyezheti. (VM) és Felhőszolgáltatások szerepkörpéldányokat csatlakoztatott tooa VNet rendelkezik hozzáférési toohello Internet, alapértelmezés szerint. Bejövő hozzáférést toospecific erőforrások, igény szerint is engedélyezheti.

Tooa VNet összes erőforrások csatlakoztatott kimenő kapcsolat toohello Internet alapértelmezés szerint rendelkezik. hello erőforrás hello magánhálózati IP-címe a forrás hálózati címe (SNAT) tooa nyilvános IP-cím fordította hello Azure-infrastruktúra. Hello alapértelmezett kapcsolat egyéni Útválasztás és a forgalom szűrés alkalmazásával módosíthatja. További információk a kimenő internetkapcsolat működését, olvassa el a hello toolearn [ismertetése az Azure-ban kimenő kapcsolatok](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections?toc=%2fazure%2fvirtual-network%2ftoc.json).

toocommunicate bejövő hello Internet, illetve toocommunicate kimenő toohello Internet nélkül SNAT, egy erőforrást egy nyilvános IP-cím rendelendő tooAzure erőforrásokat. További információ a nyilvános IP-címek, olvassa el a hello toolearn [nyilvános IP-címek](https://docs.microsoft.com/azure/virtual-network/virtual-network-public-ip-address).

**Az Azure erőforrás-kapcsolat**

[Azure-erőforrások](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) például a Felhőszolgáltatások és virtuális gépek is csatlakoztatott toohello ugyanazt a virtuális hálózatot. hello erőforrások képesek-e csatlakozni más tooeach privát IP-címek, még akkor is, ha külön alhálózatokon vannak. Azure biztosít alapértelmezett között alhálózatok, a Vnetek és a helyszíni hálózatokhoz, így nem rendelkezik tooconfigure és útvonalak kezelése.

Több Azure-erőforrások tooa virtuális hálózat, például a virtuális gépek (VM), a Cloud Services, az App Service Environment-környezetek és a virtuálisgép-méretezési csoportok is elérheti. Virtuális gépek tooa alhálózati történik egy Vneten belül egy adott hálózati csatoló (NIC) keresztül csatlakozni. További információk a hálózati adapterek, olvassa el a hello toolearn [hálózati illesztőt](https://docs.microsoft.com/azure/virtual-network/virtual-network-network-interface).

**VNet-kapcsolatot**

[Vnetek](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) lehet csatlakoztatott tooeach más, erőforrások engedélyezése tooany VNet toocommunicate kapcsolódnak bármilyen olyan erőforrás bármely más virtuális hálózaton.

Csatlakozhat a más Vnetekről tooeach, erőforrások engedélyezése tooeither VNet toocommunicate egymással keresztül kapcsolódó Vnetek. Valamelyike vagy mindegyike a következő beállítások tooconnect Vnetek tooeach más hello használhatja:

- **Társviszony-létesítés:** lehetővé teszi, hogy erőforrásokat csatlakoztatott toodifferent Azure Vnetekhez hello belül azonos Azure-beli hely toocommunicate egymással. hello sávszélesség és a késleltetés között Vnetek van hello ugyanaz, mint ha hello erőforrások csatlakoztatott toohello hello azonos VNet.toolearn társviszony-létesítést, kapcsolatos további információkért olvassa el hello [virtuális hálózati társviszony-létesítés](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview).

- **VNet – VNet-kapcsolatot:** lehetővé teszi, hogy erőforrásokat csatlakoztatott toodifferent Azure virtuális hálózaton belül hello ugyanazon vagy másik Azure helyét. Ellentétben a társviszony-létesítést, sávszélesség oka Vnetek közötti forgalmat kell áramlása az Azure VPN Gateway. További információk a Vneteket összekötő VNet – VNet-kapcsolatot toolearn. toolearn több, olvassa el a hello [VNet – VNet-kapcsolatot konfiguráló](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal?toc=%2fazure%2fvirtual-network%2ftoc.json) .

**Helyszíni kapcsolatok**

A vnetek túl[helyszíni](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) keresztül magánhálózati kapcsolatok a hálózati és az Azure között, és a pont-pont VPN-kapcsolaton keresztül hello Internet hálózatok.

Csatlakozhat a helyszíni hálózati tooa virtuális hálózatot az alábbi beállítások hello tetszőleges kombinációját használja:

- **Pont-pont virtuális magánhálózati (VPN):** egyetlen Számítógéphez csatlakoztatott tooyour hálózati és hello VNet között. A kapcsolat típusa nem nagyszerű, ha Ön most csak az első lépések az Azure-ral, vagy a fejlesztők számára, mert azt igényli, hogy kevéssé vagy egyáltalán ne módosítások meglévő tooyour-hálózati. hello kapcsolat hello Internet hello PC és hello VNet között hello SSTP protokollal tooprovide titkosított kommunikációt használ. pont-pont típusú VPN hello késés is előre nem látható, mivel hello a forgalom halad át hello Internet.

- **Telephelyek közötti VPN:** a VPN-eszköz és az Azure VPN-átjáró között. A kapcsolat típusa lehetővé teszi, hogy bármely tooaccess egy Vnetet engedélyezi, hogy a helyi erőforrás. hello kapcsolat az IPsec/IKE VPN, amely titkosított kommunikáció keresztül hello Internet hello Azure VPN gateway a helyszíni eszközök között. a pont-pont kapcsolat hello késés is előre nem látható, mivel hello a forgalom halad át hello Internet.

- **Az Azure ExpressRoute:** hoznak létre egy ExpressRoute-partner keresztül a hálózat és az Azure-ban. Ezt a kapcsolatot a sajátja. Forgalom nem járja hello Internet. az ExpressRoute-kapcsolat hello késés is előre jelezhető, mivel a forgalom nem bejárása hello Internet. További információk az összes hello előző kapcsolati beállítások, olvassa el a hello toolearn [kapcsolat topológiai diagramot](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways?toc=%2fazure%2fvirtual-network%2ftoc.json).

**Forgalomszűrés**

Virtuális gép és a Felhőszolgáltatások szerepkörpéldányokat [hálózati forgalom](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) bejövő és kimenő alapján szűrhetők forrás IP-cím és port, cél IP-cím és port és protokoll.

Egyik vagy mindkét alábbi beállítások hello alhálózatokat közötti hálózati forgalom szűrheti:

- **Hálózati biztonsági csoportok (NSG):** minden NSG több bejövő és kimenő szabályokat, amelyek lehetővé teszik a forrás és cél IP-cím, port és protokoll toofilter forgalmat is tartalmazhat. Az NSG tooeach egy virtuális hálózati adapter is alkalmazhatja. Az NSG-toohello alhálózatot a hálózati adapter is alkalmazhat, vagy más Azure-erőforrás, csatlakozik-e. További információk az NSG-k, olvassa el a hello toolearn [hálózati biztonsági csoportok](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg).

- **Virtuális hálózati berendezések:** egy virtuális hálózati berendezések egy hálózati funkciót, például egy tűzfal végző szoftvert futtató virtuális gép. A hello Azure piactéren elérhető NVAs listájának megtekintése. NVAs is elérhetők, hogy WAN-optimalizálást és az egyéb hálózati forgalom funkciókat biztosít. NVAs általában használt felhasználói vagy a BGP-útvonalakat. Használhatja az NVA toofilter forgalom Vnetek között is.

**Útválasztás**

Azure alapértelmezett útválasztás konfigurálása a saját útvonalakat, vagy a hálózati átjáró BGP-útvonalakat segítségével igény szerint felülbírálható.

Az útvonaltáblák, amelyek lehetővé teszik az erőforrások csatlakoztatott tooany alhálózatának bármely virtuális hálózat toocommunicate egymással, alapértelmezés szerint az Azure létrehoz. Megvalósíthat vagy, vagy mindkét a következő beállítások toooverride hello hello alapértelmezett útvonalak Azure-hoz:

- **Felhasználó által definiált útvonalak:** hozhat létre egyéni útvonaltáblák útvonalat, ahol a forgalmat az irányított toofor szabályozó az egyes alhálózatokon. További információk a felhasználó által definiált útvonalak, olvassa el a hello toolearn [felhasználó által definiált útvonalak](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview).

- **BGP-útvonalakat:** Ha a virtuális hálózat tooyour a helyszíni hálózat az Azure VPN-átjáró vagy ExpressRoute kapcsolattal csatlakozik, a BGP-útvonalak tooyour Vnetek kerülhet.

### <a name="hybrid-internet-connectivity-connect-tooan-on-premises-network"></a>Hibrid internetkapcsolat: tooan a helyszíni hálózat
Csatlakozhat a helyszíni hálózati tooa virtuális hálózatot az alábbi beállítások hello tetszőleges kombinációját használja:

-   Internetkapcsolat

-   Pont – hely típusú VPN (P2S VPN)

-   Telephelyek közötti VPN (S2S VPN)

-   ExpressRoute

#### <a name="internet-connectivity"></a>Internetkapcsolat

Ahogyan neve is jelzi, internetkapcsolattal elérhetővé válnak a munkaterhelések hello Internet, az azzal, hogy Ön teszi közzé a különböző nyilvános végpontok tooworkloads, amely hello virtuális hálózaton belüli élő. Az ilyen terhelések sikerült elérhetővé tehető használatával [internetre terheléselosztó](https://docs.microsoft.com/azure/load-balancer/load-balancer-internet-overview) vagy toohello VM egyszerűen hozzárendelés, egy nyilvános IP-címmel. Ezzel a módszerrel lesz lehetséges a hello Internet toobe képes tooreach hogy, hogy a virtuális gép semmi megadni a gazdagép tűzfalának [hálózati biztonsági csoportok (NSG)](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg), és [felhaszn útvonalak](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview) teszi lehetővé toohappen.

Ebben a forgatókönyvben sikerült teszi közzé, amelyet a toobe nyilvános toohello internetes alkalmazás, és képes tooconnect tooit a kell bárhol, vagy a munkaterhelések hello konfigurációjától függően meghatározott helyeiről.

#### <a name="point-to-site-vpn-or-site-to-site-vpn"></a>Pont – hely típusú VPN vagy a telephelyek közötti VPN
E két esik, az ugyanilyen kategóriájú hello. Mindkét van szükségük a VNet toohave VPN-átjáró és csatlakoztathatja a VPN-ügyfél használata a munkaállomás hello részeként tooit [pont-hely konfigurációs](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal) vagy konfigurálhatja a helyszíni [VPN-eszköz](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-devices) toobe képes tooterminate a telephelyek közötti VPN. Ezzel a módszerrel a helyszíni eszközök tooresources hello virtuális hálózaton belül is elérheti.

A pont-pont (P2S) konfiguráció lehetővé teszi a biztonságos kapcsolatot létesíthet egy egyéni ügyfél tooa virtuális hálózatban. A pont–hely kapcsolat egy SSTP (Secure Socket Tunneling Protocol) használatával működő VPN-kapcsolat.

![Pont – hely típusú VPN](media/azure-network-security/azure-network-security-fig-5.png)

Pont – hely kapcsolatok hasznosak, ha azt szeretné, tooconnect tooyour VNet távoli helyről, például otthoni vagy egy konferencia center, vagy ha csak néhány ügyfelek számára, amelyek tooconnect tooa virtuális hálózat.

A pont–hely kapcsolatok nem igényelnek VPN-eszközt vagy nyilvános IP-címet a működéshez. Hello ügyfélszámítógépről hello VPN-kapcsolat létrehozása. Ezért P2S nem ajánlott módja tooconnect tooAzure abban az esetben, ha sok helyszíni eszközök és számítógépek tooyour Azure hálózati állandó kapcsolatot kell.

![Telephelyek közötti VPN](media/azure-network-security/azure-network-security-fig-6.png)

> [!Note]
> Pont – hely kapcsolatok kapcsolatos további információkért lásd: hello [pont-pont be v Q](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-classic-azure-portal).

Pont-pont VPN gateway-kapcsolattal használt tooconnect a helyszíni hálózati tooan Azure-beli virtuális hálózat (IKEv1 vagy IKEv2) IPsec/IKE VPN-alagúton keresztül.

Ilyen típusú kapcsolat egy VPN található helyszíni Eszközkezelési, amely rendelkezik egy külsőleg irányuló nyilvános IP-cím tooit igényel. A kapcsolat történik több mint hello Internet, és lehetővé teszi a túl "alagút" egy titkosított hivatkozást a hálózat és az Azure között lévő információkat. Telephelyek közötti VPN egy biztonságos, érett technológia, amely különböző méretű vállalatok által évtizedeken van telepítve. Bújtatás titkosítás használatával történik [IPsec-bújtatásos mód](https://technet.microsoft.com/library/cc786385.aspx).

A megbízható, megbízható és a meghatározott technológiája pedig a telephelyek közötti VPN belül hello alagút forgalmi hello Internet bejárása. Ezenkívül sávszélessége korlátozott viszonylag tooa legfeljebb körülbelül 200 MB/s.

A létesítmények közötti kapcsolatokat. szükséges egy rendkívüli szintű biztonsági vagy a teljesítmény, azt javasoljuk, hogy a létesítmények közötti kapcsolatot használjon Azure ExpressRoute. ExpressRoute dedikált WAN a helyszíni hely vagy egy Exchange-szolgáltató közötti kapcsolat. Mivel ez egy telco kapcsolat, az adatok nem utazik hello interneten keresztül, így nem kitett toohello internetes adatátvitel járó esetleges kockázatokat.

> [!Note]
> További információ a VPN-átjárók tudnivalók [VPN-átjáró](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).

#### <a name="dedicated-wan-link"></a>Dedikált WAN-kapcsolat
A Microsoft Azure ExpressRoute lehetővé teszi a helyszíni hálózatokhoz kiterjeszti hello Azure könnyíteni a kapcsolat szolgáltatóját egy dedikált titkos kapcsolaton keresztül.

Az ExpressRoute-kapcsolatok ne lépje át hello nyilvános internethez. Ez lehetővé teszi ExpressRoute kapcsolatok toooffer további megbízhatóságát, gyorsabb sebességű, kisebb késések fordulnak elő, és nagyobb biztonságot nyújtana tipikus kapcsolatok hello interneten keresztül.

![ Dedikált WAN-kapcsolat](media/azure-network-security/azure-network-security-fig-7.png)

> [!Note]
> Információ tooconnect a hálózati tooMicrosoft használatával ExpressRoute, lásd: [ExpressRoute modellek](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways) és [ExpressRoute műszaki áttekintés](https://docs.microsoft.com/azure/expressroute/expressroute-introduction).

Mint hello pont-pont VPN-beállításokat, a ExpressRoute is lehetővé teszi tooconnect tooresources, amelyek nem feltétlenül csak egy Vneten belül. Valójában attól függően, hogy hello SKU, csatlakoztathatja too10 Vnetek. Ha hello [prémium szintű bővítmény](https://docs.microsoft.com/azure/expressroute/expressroute-faqs), kapcsolatok tooup too100 Vnetek is előfordulhatnak, attól függően, hogy a sávszélesség. Mi az ilyen típusú kapcsolatokat megjelenését, például, olvasási kapcsolatos további toolearn [kapcsolat topológiai diagramot](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways?toc=%2fazure%2fvirtual-network%2ftoc.json).

### <a name="security-controls"></a>Biztonsági vezérlőket
Egy Azure virtuális hálózatra biztosít egy biztonságos, logikai hálózathoz el különítve a többi virtuális hálózatok, amely támogatja több biztonsági vezérlő, amelyet a helyszíni hálózatokhoz. Az ügyfelek létre saját struktúra használatával: alhálózatok – azok a saját privát IP-címtartományt használja, az útvonaltáblák konfigurálása, a hálózati biztonsági csoportok, hozzáférés-vezérlés: listák (ACL), az átjárók és virtuális készülékek toorun a munkaterhelések hello felhőben.

Az alábbiakban hello az Azure virtuális hálózaton használható biztonsági vezérlők:

-   Hálózati hozzáférés-vezérlést

-   Felhasználó által definiált útvonalak

-   Hálózati biztonsági készülékek

-   Application Gateway

-   Az Azure webalkalmazási tűzfal

-   Hálózati rendelkezésre állás vezérlő

#### <a name="network-access-controls"></a>Hálózati hozzáférés-vezérlést
Amíg hello Azure Virtual Network (VNet) Azure hálózatmodell hello legfontosabb feladatai közé tartoznak, és biztosítja a leválasztásra és a védelemre, hello [hálózati biztonsági csoportok (NSG)](https://blogs.msdn.microsoft.com/igorpag/2016/05/14/azure-network-security-groups-nsg-best-practices-and-lessons-learned/) hello fő eszközei használhatja a hálózati forgalom tooenforce és vezérlés hello hálózati szinten szabályokat.

![ Hálózati hozzáférés-vezérlést](media/azure-network-security/azure-network-security-fig-8.png)


Hozzáférést lehetővé tevő vagy megtagadására hello futó alkalmazások és szolgáltatások a virtuális hálózaton, az ügyfél hálózatokon keresztül közötti kapcsolatot nyújthassanak, rendszerek közötti kommunikációt, vagy közvetlen Internet-kommunikációt.

Hello diagramon Vnetek és NSG-ket is találhatók, egy adott rétegben hello Azure átfogó biztonsági verem, ahol az NSG-k, UDR és virtuális hálózati berendezések lehet a használt toocreate biztonsági határokat tooprotect hello alkalmazástelepítések hello védett hálózat.

NSG-ket egy 5 rekordos tooevaluate forgalom (és hello szabályokat be hello NSG-t használ):

-   [Forrás és cél IP-cím](https://support.microsoft.com/help/969029/the-functionality-for-source-ip-address-selection-in-windows-server-2008-and-in-windows-vista-differs-from-the-corresponding-functionality-in-earlier-versions-of-windows)

-   [Forrás és cél port](https://technet.microsoft.com/library/dd197515)

-   Protokoll: [Transmission Control Protocol (TCP)](https://technet.microsoft.com/library/cc940037.aspx) vagy [User Datagram Protocol (UDP)](https://technet.microsoft.com/library/cc940034.aspx)

Ez azt jelenti, szabályozhatja a hozzáférést egy virtuális és a csoport közötti virtuális gépeket, vagy egy egyetlen virtuális gép tooanother egyetlen virtuális gép, vagy teljes alhálózatok között. Ebben az esetben ne feledje, hogy ez a szűrés, egyszerű csomag Csomagvizsgálat nem teljes. Nincs protokoll érvényesítési vagy hálózati szintek Azonosítói vagy IP-CÍMEK a hálózati biztonsági csoport képességét.

Az NSG-t, meg kell ismernie a beépített szabályokat tartalmaz. Ezek a következők:

-   **Egy adott virtuális hálózaton belüli összes forgalmat engedélyezi:** hello minden futó virtuális gépek azonos Azure Virtual Network kommunikálhatnak egymással.

-   **Engedélyezi az Azure terheléselosztási tooinbound:** Ez a szabály lehetővé teszi minden forrás cím tooany célcím-forgalom hello Azure terheléselosztó.

-   **Az összes bejövő megtagadása:** Ez a szabály blokkolja az összes forgalom forrása hello Internet kifejezetten engedélyezett.

-   **Engedélyezi az összes forgalom kimenő Internet toohello:** Ez a szabály lehetővé teszi, hogy a virtuális gépek tooinitiate kapcsolatok toohello Internet. Ha nem szeretné, hogy ezek a kapcsolatok kezdeményezett, egy szabály tooblock toocreate kell ezeket a kapcsolatokat, vagy kényszerítése a kényszerített bújtatást.

#### <a name="system-routes-and-user-defined-routes"></a>Rendszerútvonalak és a felhasználó által definiált útvonalak

Amikor az Azure virtuális gépek (VM) tooa virtuális hálózathoz (VNet), azt észleli, hogy hello virtuális gépek automatikusan legyenek-e egymással képes toocommunicate hello hálózaton keresztül. Az átjáró toospecify nem kell annak ellenére, hogy a virtuális gépek hello külön alhálózatokon vannak.

hello is igaz hello virtuális gépek toohello közti kommunikációhoz nyilvános interneten, és még akkor is, tooyour a helyszíni hálózatra is, ha egy hibrid kapcsolat az Azure tooyour saját datacenter jelen.

![Rendszerútvonalak](media/azure-network-security/azure-network-security-fig-9.png)

Kommunikáció ilyen típusú áramlása azért lehetséges, mert az Azure rendszer útvonalak toodefine hogyan IP a forgalom sorát használja. Rendszerútvonalak vezérlő a következő forgatókönyvek hello kommunikáció hello áramlását:

-   Belül hello ugyanazon az alhálózaton.

-   Az alhálózati tooanother, egy Vneten belül.

-   A virtuális gépek toohello Internet.

-   Az egy VNet tooanother VNet egy VPN-átjárón keresztül.

-   Egy VNet tooanother VNet – VNet-társviszony létesítése – a ([szolgáltatás láncolás](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview)).

-   Virtuális hálózat tooyour a helyi hálózaton keresztül egy VPN-átjárón keresztül.

Sok vállalat rendelkezik szigorú biztonsági, és az összes hálózati csomagok tooenforce adott helyszíni vizsgálata igénylő megfelelőségi követelmények házirendeket. Azure nevű mechanizmust biztosít [kényszerített bújtatás](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-forced-tunneling) , amely irányítja a forgalmat a virtuális gépek tooon helyszíni hello egyéni útvonal létrehozása vagy által [Border Gateway Protocol (BGP)](https://docs.microsoft.com/windows-server/remote/remote-access/bgp/border-gateway-protocol-bgp) hirdetmények keresztül Expressroute-on vagy VPN.

Az Azure-ban kényszerített bújtatás konfigurálása virtuális hálózati felhasználó által definiált útvonalak (UDR) keresztül. Irányít át forgalmat tooan helyszíni hely alapértelmezett útvonalat toohello Azure VPN-átjáróként van kifejezve.

hello következő szakaszban azok hello aktuális hello útválasztási táblázathoz és korlátozása útvonalak egy Azure virtuális hálózat:

-   Minden egyes virtuális hálózati alhálózat van egy beépített, rendszer-útválasztási táblázatához. hello rendszer útvonaltábla rendelkezik a következő három csoport útvonalak hello:

 -  **Helyi VNet útvonalak:** közvetlenül toohello cél virtuális gépek hello az azonos virtuális hálózatban

 - **A helyi útvonalak:** toohello Azure VPN gateway

 -  **Alapértelmezett útvonal:** közvetlenül toohello Internet. Eldobott csomagok toohello magánhálózati IP-címek nem fedi le hello előző két útvonalak.

-   Felhasználó által definiált útvonalak hello számára készült hozzon létre egy útválasztási táblázat tooadd alapértelmezett útvonalat, és társíthatja hello útválasztási táblázat tooyour VNet alhálózati tooenable kényszerített bújtatás ezek alhálózatok.

-   Egy "alapértelmezett"nevű hely tooset kell hello létesítmények közötti helyi helyek csatlakoztatott toohello virtuális hálózat között.

-   A kényszerített bújtatás kell rendelni egy Vnetet, amely egy dinamikus útválasztási VPN-átjáró (nem a statikus átjárók) rendelkezik.

- A kényszerített bújtatás ExpressRoute a mechanizmus révén nincs konfigurálva, de ehelyett szerint engedélyezve van egy alapértelmezett útvonalat hirdet keresztül hello ExpressRoute BGP társviszony-létesítési munkameneteket.

> [!Note]
> További információkért lásd: hello [ExpressRoute dokumentációja](https://azure.microsoft.com/documentation/services/expressroute/) további információt.

#### <a name="network-security-appliances"></a>Hálózati biztonsági készülékek
Amíg a hálózati biztonsági csoportok és felhasználói útvonalakat biztosít egy bizonyos szintű hálózati biztonság: hello hello hálózati és a szállítási réteg [OSI-modell](https://en.wikipedia.org/wiki/OSI_model), toobe olyan helyzetekben, ahol azt szeretné, vagy tooenable kell készül biztonsági magasabb szinten hello hálózati vermet. Ilyen helyzetekben azt javasoljuk, hogy telepít virtuális hálózati biztonsági készülékek Azure partnerei által biztosított.

![Hálózati biztonsági készülékek](./media/azure-network-security/azure-network-security-fig-10.png)

Az Azure hálózati biztonsági készülékek VNet biztonság növelésével és hálózati funkciók, és számos gyártó hello keresztül elérhető [Azure piactér](https://azuremarketplace.microsoft.com). E virtuális biztonsági készülékek telepített tooprovide lehet:

-   Magas rendelkezésre állású tűzfalak

-   Behatolás megelőzése

-   Behatolásérzékelési

-   Webalkalmazási tűzfalak (WAFs)

-   WAN-optimalizálást

-   Útválasztás

-   Terheléselosztás

-   VPN

-   Tanúsítványkezelés

-   Active Directory

-   Többtényezős hitelesítés

#### <a name="application-gateway"></a>Alkalmazásátjáró

[A Microsoft Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction) van egy dedikált virtuális készülék, amely az alkalmazás kézbesítési vezérlőhöz (LÉPETT) szolgáltatás.

 ![Application Gateway](./media/azure-network-security/azure-network-security-fig-11.png)

Alkalmazásátjáró lehetővé teszi toooptimize webkiszolgáló farm teljesítmény és rendelkezésre állás kiürítésével a CPU intenzív SSL lezárást toohello az Alkalmazásátjáró (SSL-feladatkiszervezést). Emellett biztosítja az egyéb réteg 7 útválasztási képességeket telepít, például:

-   Ciklikus multiplexelés bejövő forgalom

-   A munkamenet cookie-alapú kapcsolat

-   URL-cím elérési út-alapú útválasztási

-   Képes toohost mögött egyetlen alkalmazás átjáró több webhelyek


A [webalkalmazási tűzfal (WAF)](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) hello Alkalmazásátjáró részeként is biztosítja. Ez védelmet biztosít a közös webes biztonsági rések és biztonsági rések tooweb alkalmazások. Alkalmazásátjáró konfigurálhat Internet felé néző átjáró, a belső átjárót, vagy mindkettőt.

Alkalmazás átjáró WAF észlelési vagy megelőzési módban futtatható. Gyakori használati eset rendszergazdák toorun észlelési mód tooobserve forgalmának kártevő minták szolgál. Amennyiben lehetséges észlel, alakítsa tooprevention mód blokkolja a gyanús bejövő forgalmat.

 ![Application Gateway](./media/azure-network-security/azure-network-security-fig-12.png)

Ezenkívül alkalmazás átjáró WAF segít figyelni a webalkalmazásokat integrálva van a valós idejű WAF-naplót használó támadások ellen [Azure figyelő](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview) és [az Azure Security Center](https://azure.microsoft.com/services/security-center/) tootrack WAF riasztások és egyszerűen figyelheti a trendeket.

hello JSON formátumú napló közvetlenül toohello ügyfél tárfiókjával kerül. Ezek a naplók teljes hozzáféréssel rendelkeznek, és a saját megőrzési házirendeket is alkalmazhat.

Ezek a naplók is betöltési módja, ha a saját analytics rendszer [Azure napló integrációs](https://aka.ms/AzLog). WAF naplók integrálva vannak [Operations Management Suite (OMS)](https://www.microsoft.com/cloud-platform/operations-management-suite) , hogy használhassa az OMS szolgáltatáshoz tooexecute kifinomult részletes lekérdezéseket.

#### <a name="azure-web-application-firewall-waf"></a>Az Azure webalkalmazási tűzfal (WAF)

Webalkalmazások egyre közös ismert biztonsági rések, például az SQL injektálási, helyközi parancsfájlok futtatására és más támadásoknak hello jelennek meg a biztonsági rések elleni támadásokat céljainak [OWASP első 10](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project). Megakadályozza az ilyen biztonsági réseket hello alkalmazásban szigorú karbantartási, javítását és hello alkalmazás topológia rétege ellenőrzés szükséges.

 ![Az Azure webalkalmazási tűzfal (WAF)](./media/azure-network-security/azure-network-security-fig-13.png)

Egy központi [webalkalmazási tűzfal (WAF)](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) webes támadások ellen, és a biztonságkezelés leegyszerűsíti az alkalmazások módosítása nélkül.

WAF megoldás is reagálhasson tooa biztonsági kockázatot jelentenek gyorsabb által egy ismert biztonsági rések egy központi helyen, és minden egyes webalkalmazás biztonságossá tétele érdekében. Meglévő alkalmazásátjárót könnyen lehet konvertált tooa webes alkalmazás engedélyezve van a tűzfal Alkalmazásátjáró.

#### <a name="network-availability-controls"></a>Hálózati rendelkezésre állás vezérlők

Nincsenek más beállítások toodistribute hálózati forgalmat a Microsoft Azure használatával. Ezek a lehetőségek különböző működési elveken alapulnak, különböző funkciókészlettel rendelkeznek és különböző forgatókönyveket támogatnak. Elkülönítve vagy egymással kombinálva egyaránt használhatók.

Következő vannak hello hálózati rendelkezésre állás vezérlők:

-   Azure Load Balancer

-   Application Gateway

-   Traffic Manager

**Az Azure terheléselosztó**

Magas rendelkezésre állás és a hálózati teljesítményt nyújt tooyour alkalmazások. A réteg 4 (TCP, UDP) terheléselosztó bejövő forgalmat egy elosztott terhelésű készlet definiált kifogástalan szolgáltatáspéldányok között ellátó.

 ![Azure Load Balancer](media/azure-network-security/azure-network-security-fig-14.png)


Az Azure Load Balancer beállítható úgy, hogy:

-   Gépek terhelést elosztani bejövő internetes forgalom toovirtual. Ez a konfiguráció az úgynevezett [internetre terheléselosztási](https://docs.microsoft.com/azure/load-balancer/load-balancer-internet-overview).

-   Betöltési oszthatja el a forgalmat egy virtuális hálózatban lévő virtuális gépek, virtuális gépek felhőszolgáltatások közötti vagy a helyszíni számítógépek és a létesítmények közötti virtuális hálózatban lévő virtuális gépek között. Ez a konfiguráció az úgynevezett [belső terheléselosztás](https://docs.microsoft.com/azure/load-balancer/load-balancer-internal-overview).

-   Külső forgalom tooa adott virtuális gép.

Hello felhőben található összes erőforrást kell egy nyilvános IP-cím toobe hello Internet elérhető. hello felhő-infrastruktúra az Azure-ban nem elérhető IP-címet használ az erőforrások. Azure nyilvános IP címek toocommunicate toohello Internet hálózati címfordítás (NAT) használ.

 **Alkalmazásátjáró**

 Alkalmazásátjáró hello (réteg 7 hello OSI hálózati referencia-verem) alkalmazás rétegben működik. Úgy működik, mint egy fordított proxy szolgáltatás hello ügyfélkapcsolat leáll, és a továbbítási kérelmek tooback-a befejezési végpontok.

 **Forgalomkezelő**

A Microsoft Azure Traffic Manager lehetővé teszi toocontrol hello terjesztési felhasználói forgalomnak a végpontok különböző adatközpontokban. Traffic Manager által támogatott szolgáltatás végpontok közé tartoznak az Azure virtuális gépeken, Web Apps, és a felhőalapú szolgáltatások. A Traffic Manager külső, nem Azure-végpontokkal együtt is használható.

A TRAFFIC Manager által használt hello tartománynévrendszer (DNS) toodirect ügyfél igényel toohello leginkább megfelelő végpont alapján egy [forgalom-útválasztási módszer](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-routing-methods) és hello végpontok hello állapotát. A TRAFFIC Manager forgalom-útválasztási módszert toosuit különböző alkalmazások igényeihez, végpont állapota számos biztosít [figyelési](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-monitoring), és automatikus feladatátvételt. A TRAFFIC Manager rugalmas toofailure, többek között a hello sikertelen volt-e egy teljes Azure-régióban.

Az Azure Traffic Manager lehetővé teszi toocontrol hello terjesztési forgalom között az alkalmazási végpontokat. A végpont bármilyen internetre-szolgáltatott belül vagy kívül Azure.

A TRAFFIC Manager biztosít két fő előnnyel jár:

-   Megfelelően fel számos tooone forgalmat [forgalom-útválasztási módszert](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-routing-methods).

-   [Folyamatosan figyelje a végpont állapotfigyelő](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-monitoring) és automatikus feladatátvétel, ha a végpontok sikertelen.

Amikor az ügyfél tooconnect tooa szolgáltatás, először azt kell tartoznia hello szolgáltatás tooan IP-cím hello DNS-nevét. hello majd ügyfélprogramja toothat IP-cím tooaccess hello szolgáltatás. A TRAFFIC Manager által használt DNS toodirect ügyfelek toospecific szolgáltatás végpontjait hello forgalom-útválasztási módszer hello szabályok alapján. Az ügyfelek kapcsolódnak kijelölt toohello végpont közvetlenül. A TRAFFIC Manager nincs olyan proxy vagy az átjáró. A TRAFFIC Manager hello ügyfél és a hello szolgáltatás közötti információátadáshoz hello forgalom nem jelenik meg.

### <a name="azure-network-validation"></a>Azure-hálózat ellenőrzése

Azure-hálózat érvényesítésre, amely az Azure-hálózat hello tooensure működik van konfigurálva, és érvényesítési végezhető hello szolgáltatást és alkalmazást elérhető toomonitor hello hálózat használatával. Azure hálózati figyelőt, végezheti el a naplózási formátum és diagnosztikai képességek, amelyek insights toounderstand ellenőrizniük meg a hálózati teljesítmény- és állapot. Ezek a képességek a portálhoz, a PowerShell, a CLI, a Rest API-t és a SDK keresztül érhetők el.

Az Azure Operational biztonsági hivatkozik toohello szolgáltatások, a vezérlők és a szolgáltatások rendelkezésre toousers védelmére az adatok, alkalmazások és egyéb eszközök a Microsoft Azure-ban. Az Azure Operational biztonsági épül egy keretrendszer, amely magában foglalja a hello tapasztalatok teszik a különböző képességeket, amelyek egyedi tooMicrosoft, beleértve a Microsoft biztonságos fejlesztési Életciklussal (SDL), a Microsoft biztonsági válasz Center hello hello keresztül program, és részletes tájékoztatást nyújthatnak a hello számítógépes biztonsági fenyegetésekről alkotott képet.

-   [Az Azure Operations Management Suite szolgáltatásban](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview)

-   [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro)

-   [Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview)

-   [Az Azure hálózati figyelőt](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview)

-   [Az Azure Storage analytics](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics)

-   Azure Resource Manager

#### <a name="azure-resource-manager"></a>Az Azure erőforrás-kezelő

lehet, hogy hello legfontosabb biztonsági szolgáltatás hello platform hello személyek és a Microsoft Azure-t üzemeltető folyamatok. Ez a szakasz ismerteti a Microsoft globális adatközpont infrastruktúrájában javíthatja és karbantartása a biztonsági, folytonosságának és adatvédelmi funkciókat.

az alkalmazás hello infrastruktúrája általában számos összetevőből áll – például egy virtuális gépből, tárfiókot, és virtuális hálózati vagy egy webalkalmazást, adatbázis, adatbázis-kiszolgáló és harmadik féltől származó szolgáltatással épül fel. Ezeket az összetevőket nem külön entitásokként látja, hanem egyetlen entitás kapcsolódó és egymással összefüggő részeiként. Szeretné, hogy toodeploy, kezelheti és figyelheti azokat csoportként. Az Azure Resource Manager lehetővé teszi a megoldás hello erőforrásokat toowork csoportként.

Telepíteni, frissíteni vagy hello összes erőforrást törli a megoldás egyetlen, koordinált műveletben. A telepítéshez egy sablon használatos, amely különböző, például tesztelési, átmeneti és üzemi környezetben is képes működni. A Resource Manager biztosít biztonsági, naplózási és címkézési szolgáltatásokat toohelp, kezelheti az erőforrásokat a telepítés utáni.

**Erőforrás-kezelő használatával hello előnyei**

A Resource Manager számos előnyt kínál:

-   Telepíthetik, felügyelhetik és erőforrások hello figyeli, hogy a megoldás egy csoportot, hanem erőforrások különálló kezelése.

-   Ismételten telepítheti a megoldást hello fejlesztési életciklus során, és lehet abban, hogy az erőforrások telepítése konzisztens lesz.

-   Az infrastruktúrát szkriptek helyett deklaratív sablonok segítségével is kezelheti.

-   Megadhatja, hogy hello függőségek között erőforrásokat, hogy azok hello megfelelő sorrendben legyenek telepítve.

-   Hozzáférés-vezérlési tooall szolgáltatásokat alkalmazhat az erőforráscsoportban, mivel a szerepköralapú hozzáférés-vezérlést (RBAC) natív módon integrálva hello felügyeleti platform.

-   Címkékkel láthatja tooresources toologically rendszerezése összes hello erőforrást az előfizetésében.

-   Tisztázhatja a szervezete egy címke megosztása erőforrások csoportja számára költségeinek megtekintésével.

> [!Note]
> Erőforrás-kezelő egy új módon toodeploy biztosít, és a megoldások kezelése. Ha követte hello korábbi telepítési modellt és toolearn hello változásokról, tekintse meg [Understanding Resource Manager üzembe helyezési és a klasszikus üzembe helyezési](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model).

## <a name="azure-network-logging-and-monitoring"></a>Azure-hálózat naplózás és figyelés

Az Azure számos eszközök toomonitor kínál, megakadályozása, észlelésében és kezelésében toonetwork biztonsági eseményeket. Hello leghatékonyabb eszközök elérhető tooyou milyenek többek között:

-   Network Watcher

-   Hálózati erőforrás szolgáltatásiszint-figyelés

-   Log Analytics

### <a name="network-watcher"></a>Hálózati figyelőt

[Hálózati figyelő](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) -forgatókönyv-alapú figyelési által biztosított hello szolgáltatások a hálózati figyelőt. A szolgáltatás része a csomagrögzítéssel, a következő ugrás, az IP-adatfolyam győződjön meg arról, biztonsági csoport megtekintése, NSG folyamata naplókat. Forgatókönyv szintű figyelési jeleníti meg egy záró tooend ezzel szemben tooindividual hálózati erőforrás figyelési hálózati erőforrásokhoz.

 ![Network Watcher](./media/azure-network-security/azure-network-security-fig-15.png)

Hálózati figyelőt regionális szolgáltatás, amely lehetővé teszi, hogy Ön toomonitor és diagnosztizálásához szintjén feltételek egy hálózati forgatókönyv, hogy, és az Azure-ból. Hálózati diagnosztikai és a képi megjelenítés eszközök is elérhetők a hálózati figyelőt segítenek megérteni, diagnosztizálása és szerezhet insights tooyour hálózati az Azure-ban.

Hálózati figyelőt a következő képességeket hello van:

#### <a name="topology"></a>topológia

[Topológia](https://docs.microsoft.com/azure/network-watcher/network-watcher-topology-overview) adja vissza a hálózati erőforrások egy grafikonon egy virtuális hálózatot. hello ábra hello összekapcsolása hello erőforrások toorepresent hello end tooend hálózati kapcsolatot ábrázolja. Hello portálon topológia adja vissza hello erőforrás-objektumból az adott virtuális hálózati alapján. hello kapcsolatok ezek ábrázolva kapcsolaton kívül hello hálózati figyelőt régió, hello erőforrások között, akkor is, ha a hello erőforrás csoport nem jelenik meg. hello portál nézetben visszaadott hello erőforrások hello hálózati összetevők, amelyek grafikon részhalmazát jelentik. toosee hello teljes listájának hálózati erőforrások megtekintéséhez használhatja [PowerShell](https://docs.microsoft.com/azure/network-watcher/network-watcher-topology-powershell) vagy [REST](https://docs.microsoft.com/azure/network-watcher/network-watcher-topology-rest).

Erőforrásokat ad vissza, azok között hello kapcsolat két kapcsolatok alapján van modellezve.

- **A befoglaltsági** -virtuális hálózat tartalmaz egy alhálózatot, amely tartalmazza a hálózati adaptert.

- **Kapcsolódó** – A hálózati adapter kapcsolódik a virtuális gépek.

#### <a name="variable-packet-capture"></a>Változó csomagrögzítéssel

Hálózati figyelő [változó csomagrögzítéssel](https://docs.microsoft.com/azure/network-watcher/network-watcher-packet-capture-overview) lehetővé teszi a virtuális gép toocreate csomag rögzítési munkamenetek tootrack forgalom tooand. Csomagrögzítéssel segít toodiagnose hálózati rendellenességeket mindkét reaktív és proactivity. Egyéb felhasználásra tartalmazzák a hálózati statisztikákat, való információk hálózati behatolások, toodebug ügyfél-kiszolgáló közötti kommunikációt, és még sok más összegyűjtéséhez.

Csomagrögzítéssel a virtuálisgép-bővítmény hálózati figyelőt keresztül távolról elindult. Ez a funkció megkönnyíti a hello okozta terheket egy csomagrögzítéssel manuálisan virtuális gépen futó hello szükséges, amely értékes időt takaríthat meg. Csomagrögzítéssel hello portál, a PowerShell, a CLI vagy a REST API-n keresztül is elindítható. Például hogyan csomagrögzítéssel is elindítható a virtuális gép a riasztások van.

#### <a name="ip-flow-verify"></a>IP-adatfolyam ellenőrzése

[IP-adatfolyamok ellenőrizze](https://docs.microsoft.com/azure/network-watcher/network-watcher-ip-flow-verify-overview) ellenőrzi, ha egy csomag engedélyezett vagy megtagadott tooor egy virtuális gép 5 rekordos adatok alapján. Ez az információ irányát, protokoll, helyi IP, távoli IP, helyi port és távoli port áll. Ha hello csomagok megtagadta a biztonsági csoport, hello csomagok megtagadva hello szabály hello nevét adja vissza. Minden cél- és IP-választható ki, miközben segít gyorsan diagnosztizálhatja a kapcsolat hibái vagy a toohello rendszergazdák internet pedig vagy toohello helyszíni környezetben.

IP-adatfolyamok ellenőrizze a hálózati adaptert egy virtuális gép célozza. Forgalom majd ellenőrizve hello konfigurált beállítások tooor az, hogy a hálózati illesztő alapján. Ezzel a képességgel olyan hasznos, erősítse meg, ha egy szabály a hálózati biztonsági csoport blokkolja a belépési vagy kilépési forgalom tooor virtuális gépről.

#### <a name="next-hop"></a>Következő ugrás

Meghatározza, hogy hello [következő Ugrás](https://docs.microsoft.com/azure/network-watcher/network-watcher-next-hop-overview) a csomagok irányítása a hello Azure hálózati háló, akkor toodiagnose engedélyezése minden felhasználó által definiált útvonalak konfigurálva. A virtuális gépről érkező forgalom mennyiségének tooa cél hello hatékony útvonalak társított hálózati alapján Következő ugrás lekérése hello következő ugrás típusa és a csomagok IP-cím egy adott virtuális gép és a hálózati adaptert. Ez segít toodetermine, ha hello csomagot irányított toohello cél vagy rendszer éppen fekete hello forgalom furatos.

Következő ugrás is hello következő ugrás társított hello útválasztási táblázatot ad vissza. Lekérdezésekor a következő ugrás, ha egy felhasználó által megadott útvonal hello útvonal van meghatározva, az adott útvonal eredmény. Egyéb esetben a következő ugrás "Rendszerútvonal" ad vissza.

#### <a name="security-group-view"></a>Biztonsági csoport megtekintése

Lekérdezi a virtuális gép által használt hello hatékony és alkalmazott biztonsági szabályokat. Hálózati biztonsági csoportok társított alhálózati szinten, vagy egy hálózati adapter szintjén. Ha tartozó alhálózati szinten, tooall hello Virtuálisgép-példány hello alhálózati érvényes. Hálózati [biztonsági csoport megtekintése](https://docs.microsoft.com/azure/network-watcher/network-watcher-security-group-view-overview) minden konfigurált hello NSG-ket és egy virtuális gép hello konfigurációs betekintést biztosít a hálózati és alhálózati szinten társított szabályokat adja vissza. Továbbá a hello hatékony biztonsági szabályok visszaadott minden hello hálózati adapterek virtuális gépen. Hálózati biztonsági csoport használatával nézet felmérheti a virtuális gépek, a hálózati biztonsági réseket, például a megnyitott portok. Ha a hálózati biztonsági csoport a várt módon működik alapján is ellenőrzéséhez egy [hello összehasonlítása konfigurálva és hatékony biztonsági szabályok hello](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-auditing-powershell).

#### <a name="nsg-flow-logging"></a>NSG Flow naplózás

 Adatfolyam-naplókat a hálózati biztonsági csoportok lehetővé teszik a toocapture naplók kapcsolódó tootraffic, amelyek számára engedélyezett vagy megtagadott hello szabályok hello csoportban. hello folyamata 5 rekordos információkat – forrás IP-cím, a cél IP-cím, a forrásport, a célport és a protokoll határozzák meg.

[Hálózati biztonsági csoport folyamata naplók](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview) azon szolgáltatása, amely lehetővé teszi az IP-bemenő és kimenő forgalmat a hálózati biztonsági csoporton keresztül tooview információt hálózati figyelőt.

#### <a name="virtual-network-gateway-and-connection-troubleshooting"></a>Virtuális hálózati átjáró és a kapcsolat hibaelhárítása

Hálózati figyelőt számos lényeges képességét biztosítja, felügyeletében toounderstanding a hálózati erőforrások az Azure-ban. Ezek a képességek egyik erőforrás hibaelhárítás. [Erőforrások hibaelhárítása](https://docs.microsoft.com/azure/network-watcher/network-watcher-troubleshoot-manage-rest) PowerShell, a parancssori felületen vagy a REST API hívása. Meghívásakor, a hálózati figyelőt megvizsgálja a virtuális hálózati átjáró vagy a kapcsolat hello állapotát, és adja vissza az eredményekről.

Ez a szakasz végigvezeti hello különböző felügyeleti feladatok, amelyek erőforrás hibaelhárítási aktuálisan elérhető.

-   [A virtuális hálózati átjáró hibaelhárítása](https://docs.microsoft.com/azure/network-watcher/network-watcher-troubleshoot-manage-rest)

-   [Végezzen hibaelhárítást a kapcsolaton](https://docs.microsoft.com/azure/network-watcher/network-watcher-troubleshoot-manage-rest)

#### <a name="network-subscription-limits"></a>Hálózati előfizetési korlátozásait

[Előfizetési korlátozásait a hálózati](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) biztosítanak hello használata hello hálózati erőforrás hello maximális számát a rendelkezésre álló erőforrások elleni régióban előfizetés részleteit.

#### <a name="configuring-diagnostics-log"></a>Diagnosztikai naplófájl konfigurálása

Hálózati figyelőt tartalmaz egy [diagnosztikai naplók](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) nézet. Ez a nézet az összes hálózati erőforrások, amelyek támogatják a diagnosztikai naplózás tartalmazza. Ebben a nézetben engedélyezése, és tiltsa le a hálózati erőforrások egyszerűen és gyorsan.

### <a name="network-resource-level-monitoring"></a>Hálózati erőforrás szolgáltatásiszint-figyelés

a következő funkciók hello erőforrás szintű figyelés érhetők el:

#### <a name="audit-log"></a>Napló

Hálózatok hello konfiguráció részeként műveleteket a rendszer naplózza. Ezek a naplók alapvető tooestablish különböző megfelelőségi vannak. Ezek a naplók megtekinthetők az Azure-portálon hello, vagy visszavonni a Microsoft eszközök, például a Power BI és a külső eszközök használatával. Naplók hello portálon, a PowerShell, a CLI és a Rest API-n keresztül érhetők el.

> [!Note]
> A naplókat további információkért lásd: [naplózási műveletek a Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-audit).
Naplók az összes hálózati erőforrás végzett műveletek érhetők el.


#### <a name="metrics"></a>Mérőszámok

Adatok gyűjtése le TELJESÍTMÉNYMÉRÉSEK és egy meghatározott időtartam során gyűjtött adatait. Adatok gyűjtése le jelenleg elérhető az Alkalmazásátjáró. Metrikák használt tootrigger riasztások küszöbérték alapján lehet. Alapértelmezés szerint az Azure Application Gateway figyeli a hello állapotát a háttér-készletben erőforrásait, és automatikusan eltávolítja az összes erőforrás hello készlet megfelelő állapotúnak számít. Alkalmazásátjáró toomonitor hello sérült példányok folytatódik, és hozzáadja őket biztonsági toohello kifogástalan háttér-készlet, amennyiben azok elérhetővé válnak, és válaszoljon toohealth-vizsgálat. Alkalmazásátjáró küldi hello állapotfigyelő mintavételt a hello ugyanazt a portot definiált hello háttér HTTP-beállítások. Ez a konfiguráció biztosítja, hogy hello mintavételi azonos port, hogy az ügyfelek dolgozna tooconnect toohello háttér hello teszteli.

> [!Note]
> Lásd: [átjáró Alkalmazásdiagnosztika](https://docs.microsoft.com/azure/application-gateway/application-gateway-probe-overview) tooview hogyan metrikák használt toocreate riasztásokat is lehet.

#### <a name="diagnostic-logs"></a>Diagnosztikai naplók

Rendszeres és önkéntes események hálózati erőforrások által létrehozott, és a storage-fiókok, az Event Hubs vagy Naplóelemzési tooan küldött bejelentkezve. Ezek a naplók erőforrás állapotának hello betekintést. Ezek a naplók eszközöket, például a Power BI és a Naplóelemzési tekintheti meg. Hogyan tooview diagnosztikai naplók, látogasson el toolearn [Naplóelemzési](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-networking-analytics).

Diagnosztikai naplók érhetők el [terheléselosztó](https://docs.microsoft.com/azure/load-balancer/load-balancer-monitor-log), [hálózati biztonsági csoportok](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log), útvonalakat, és [Alkalmazásátjáró](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics).

Hálózati figyelőt biztosít a diagnosztikai naplók megtekintése. Ez a nézet az összes hálózati erőforrások, amelyek támogatják a diagnosztikai naplózás tartalmazza. Ebben a nézetben engedélyezése, és tiltsa le a hálózati erőforrások egyszerűen és gyorsan.

### <a name="log-analytics"></a>Log Analytics

[Naplófájl Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) szolgáltatás a [Operations Management Suite (OMS)](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview) , amely figyeli a felhőalapú és helyszíni környezetben toomaintain, azok rendelkezésre állását és teljesítményét. Összegyűjti az több forrás erőforrások a felhőalapú és helyszíni környezetben és az egyéb felügyeleti eszközök tooprovide elemzés által létrehozott adatok.

A Naplóelemzési megoldások a hálózatok figyelése az alábbi hello kínálja:

-   Hálózati teljesítmény figyelése (NPM)

-   Az Azure Application Gateway elemzés

-   Azure hálózati biztonsági csoport elemzés

#### <a name="network-performance-monitor-npm"></a>Hálózati teljesítmény figyelése (NPM)
Hello [hálózati Teljesítményfigyelő](https://docs.microsoft.com/azure/log-analytics/log-analytics-network-performance-monitor) felügyeleti megoldás egy olyan hálózati felügyeleti megoldás, amely figyeli a hello állapot, a rendelkezésre állás és a hálózatok elérhetőség.

Használt toomonitor közötti kapcsolat:

-   Nyilvános felhő és a helyszíni

-   Adatközpontok és a felhasználó helye (fiókirodákban)

-   Egy többrétegű alkalmazást a különböző rétegeket tartalmazó alhálózat.


#### <a name="azure-application-gateway-analytics-in-log-analytics"></a>Az Azure alkalmazás átjáró elemzés a naplóelemzési

a következő naplók hello Alkalmazásátjárót támogatja:

-   ApplicationGatewayAccessLog

-   ApplicationGatewayPerformanceLog

-   ApplicationGatewayFirewallLog

a következő metrikák hello Alkalmazásátjárót támogatja:

-   5 perces átviteli sebesség

#### <a name="azure-network-security-group-analytics-in-log-analytics"></a>Az Azure hálózati biztonsági csoport elemzés a naplóelemzési

hello következő naplók kapcsolódnak támogatottak [hálózati biztonsági csoportok](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log):

- **NetworkSecurityGroupEvent:** mely NSG szabályok lettek alkalmazott tooVMs és a MAC-címe alapján példány szerepkörök bejegyzést tartalmaz. Ezek a szabályok hello állapota 60 másodpercenként gyűjti.

- **NetworkSecurityGroupRuleCounter:** Contains bejegyzéseket hányszor minden NSG-szabályok alkalmazott toodeny vagy -kezelési forgalom engedélyezése.

## <a name="next-steps"></a>Következő lépések
Többet szeretne tudni biztonsági ehhez beolvassa a részletes biztonsági témakörök:

-   [Naplóelemzési a hálózati biztonsági csoportokkal (NSG-k)](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log)

-   [Hálózati innovációinak adott meghajtó hello felhő megszakítása](https://azure.microsoft.com/blog/networking-innovations-that-drive-the-cloud-disruption/)

-   [SONiC: a hálózati kapcsoló szoftver, amely a rendszert működtet hello Microsoft globális felhő hello](https://azure.microsoft.com/blog/sonic-the-networking-switch-software-that-powers-the-microsoft-global-cloud/)

-   [Hogyan hoz létre a Microsoft a gyors és megbízható globális hálózati](https://azure.microsoft.com/blog/how-microsoft-builds-its-fast-and-reliable-global-network/)

-   [Megvilágítási hálózati innováció mentése](https://azure.microsoft.com/blog/lighting-up-network-innovation/)
