---
title: "aaaNetwork biztonsági fogalmak & követelmények az Azure-ban |} Microsoft Docs"
description: " Ez a cikk a könnyen toounderstand, mi a Microsoft Azure toooffer van hálózati biztonsági hello területén. A hálózati biztonsági Alapfogalmak és követelmények egyszerű magyarázata nyújtunk, és milyen Azure adatok toooffer vannak az egyes ezeket a területeket. "
services: security
documentationcenter: na
author: TomShinder
manager: MBaldwin
editor: TomSh
ms.assetid: bedf411a-0781-47b9-9742-d524cf3dbfc1
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/27/2017
ms.author: terrylan
ms.openlocfilehash: 87d336064b880ddcf90ae4fcb79b7823367682b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-network-security-overview"></a>Az Azure biztonsági áttekintése
Microsoft Azure tartalmaz egy robusztus hálózati infrastruktúra toosupport, az alkalmazás és szolgáltatás hálózati kapcsolati követelményeinek. Hálózati kapcsolat lehetséges helyszíni között, az Azure-ban lévő erőforrások között, és Azure megtalálható erőforrásokhoz, és az hello Internet tooand és az Azure.

hello Ez a cikk célja toomake könnyebb a toounderstand, mi a Microsoft Azure rendelkezik toooffer hello területén hálózati biztonság. Itt azt magyarázattal alapszintű hálózati biztonsági alapfogalmait és a követelmények. Azt is adja meg, milyen Azure információk toooffer rendelkezik minden ezeket a területeket, valamint hivatkozásokat tartalmaz, a mélyrehatóbb ismereteket szerezhet érdekes területek toohelp.

Azure biztonsági áttekintése cikkben a következő területeken hello koncentrál:

* Az Azure hálózatkezelés
* Hálózati hozzáférés-vezérlés
* Biztonságos távoli hozzáférés és a létesítmények közötti kapcsolat
* Rendelkezésre állás
* Névfeloldás
* DMZ-architektúra
* Figyelés és a fenyegetések észlelése


## <a name="azure-networking"></a>Azure-hálózatok
Virtuális gépek hálózati kapcsolattal kell. toosupport ezt a követelményt, Azure igényel a csatlakozó virtuális gépek toobe tooan Azure-beli virtuális hálózathoz. Egy Azure virtuális hálózatra egy logikai szerkezet hello fizikai Azure hálózati háló platformra épül. Minden logikai Azure Virtual Network el különítve a többi Azure virtuális hálózatok. Ezzel a megoldással biztosítja, hogy a hálózati forgalmat a központi telepítés nem érhető el tooother Microsoft Azure-ügyfelek.

További információ:

* [Virtual Network áttekintése](../virtual-network/virtual-networks-overview.md)


## <a name="network-access-control"></a>Hálózati hozzáférés-vezérlés
Hálózati hozzáférés-vezérlés az hello eljárás az adott eszköz vagy egy Azure virtuális hálózatban található alhálózatok kapcsolat tooand korlátozására. hello hálózati hozzáférés-vezérlés célja toolimit hozzáférés tooyour virtuális gépek és szolgáltatások tooapproved felhasználók és eszközök. Hozzáférés-vezérlést alapuló engedélyezése vagy megtagadása kapcsolatok tooand döntéseinek a virtuális gépet vagy szolgáltatást.

Azure számos különböző típusú hálózati hozzáférés-vezérlés például támogatja:

* Hálózati réteg vezérlő
* Útvonal-vezérlés és a kényszerített bújtatás
* Virtuális hálózati biztonsági készülékek

### <a name="network-layer-control"></a>Hálózati réteg vezérlő
Minden biztonságos telepítéséhez szükséges hálózati hozzáférés-vezérlés bizonyos mértékét. hello hálózati hozzáférés-vezérlés célja toorestrict virtuális gép kommunikációs toohello szükséges rendszerek és, hogy a más kommunikációs kísérlet le vannak tiltva.

Ha módosítania kell az alapvető hálózati szintű hozzáférés-vezérlés (az IP-cím és hello TCP vagy UDP protokoll alapján), majd használhatja hálózati biztonsági csoportok. A hálózati biztonsági csoport (NSG) egy tűzfal alapszintű csomag és toocontrol hozzáférést alapján lehetővé teszi egy [5 rekordos](https://www.techopedia.com/definition/28190/5-tuple). Az NSG-k alkalmazás réteg ellenőrző nem ad meg, vagy hitelesített hozzáférés-vezérlést.

További információ:

* [Hálózati biztonsági csoportok](../virtual-network/virtual-networks-nsg.md)

### <a name="route-control-and-forced-tunneling"></a>Útvonal-vezérléshez és a kényszerített bújtatás
hello képességét toocontrol útválasztási viselkedés az Azure virtuális hálózatok a kritikus hálózati biztonság és a hozzáférés képessége. Ha útválasztási helytelenül van konfigurálva, alkalmazások és szolgáltatások a virtuális gépen futó toounauthorized eszközt, beleértve a birtokolt és a potenciális támadók által üzemeltetett rendszerek lehet, hogy csatlakozzon.

Azure hálózatkezelés hello képességét toocustomize hello útválasztási viselkedés támogatja az Azure virtuális hálózat hálózati forgalmát. Ez lehetővé teszi tooalter hello alapértelmezett útválasztási táblabejegyzéseket az Azure virtuális hálózatban. Útválasztási viselkedés irányítását segítségével győződjön meg arról, hogy egy adott eszköz vagy eszközcsoportra származó összes forgalmat be- vagy a virtuális hálózaton keresztül egy konkrét helyre.

Például lehetséges, hogy a virtuális hálózati biztonsági készülékek a Azure virtuális hálózaton. Azt szeretné, hogy toomake meg arról, hogy az Azure virtuális hálózat összes forgalom tooand végighalad az adott virtuális biztonsági készülékek. Ehhez konfigurálásával [felhasználó által megadott útvonalak](../virtual-network/virtual-networks-udr-overview.md) az Azure-ban.

[A kényszerített bújtatás](https://www.petri.com/azure-forced-tunneling) egy olyan mechanizmus, amely a szolgáltatások nincsenek tooensure használható egy kapcsolat toodevices engedélyezett tooinitiate hello Internet. Vegye figyelembe, hogy ez eltér a bejövő kapcsolatok fogadását, és ezután válaszol toothem. Előtér-webkiszolgálók kell toorespond toorequests Internet gazdagépekről, és a forgalom Internet-forrása engedélyezve van, bejövő toothese webes kiszolgálókat és webkiszolgálókat hello toorespond engedélyezett.

Mi nem szeretné, hogy tooallow egy előtér-kiszolgáló tooinitiate egy kimenő kérelem. Az ilyen kérelmeket előfordulhat, hogy képviselnek biztonsági kockázatot jelent, mert ezek a kapcsolatok használt toodownload kártevő lehet. Akkor is, ha szeretné, ezek előtér-kiszolgálók tooinitiate kimenő kérelmek toohello Internet, érdemes tooforce őket toogo keresztül a helyszíni webes proxykat, hogy az URL-cím szűrést és a naplózás előnyeit élvezheti.

Ehelyett érdemes választani toouse kényszerített bújtatás tooprevent ez. Ha engedélyezi a kényszerített bújtatás, minden kapcsolatok toohello Internet kényszerítve vannak-e a helyszíni átjárón keresztül. Ha kihasználja a felhasználó által megadott útvonalak kényszerített bújtatás konfigurálása

További információ:

* [Mik azok a felhasználó által megadott útvonalak és IP-továbbítás](../virtual-network/virtual-networks-udr-overview.md)

### <a name="virtual-network-security-appliances"></a>Virtuális hálózati biztonsági készülékek
Hálózati biztonsági csoportok, a felhasználó által megadott útvonalak és a kényszerített bújtatás biztosítanak az Ön biztonsági hello hello hálózati és a szállítási réteg, amíg [OSI-modell](https://en.wikipedia.org/wiki/OSI_model), előfordulhat, hogy ha azt szeretné, hogy magasabb szintű tooenable biztonsági alkalommal mint hello hálózathoz.

Például a biztonsági követelmények állhatnak:

* Hitelesítési és engedélyezési hozzáférési tooyour alkalmazás engedélyezése előtt
* -Behatolásérzékelési és a behatolás-válasz
* Alkalmazás réteg vizsgálatát a magas szintű protokollok
* URL-CÍMEK szűrése
* Hálózati szintű víruskereső és kártevőirtó
* Botot elleni védelem
* Alkalmazás-hozzáférés-vezérlés
* További DDoS-védelem (fent hello DDoS védelem megadott hello Azure-hálót, maga)

Továbbfejlesztett hálózati biztonsági funkció az Azure partneri megoldás használatával végezheti el. Található hello legfrissebb Azure hálózati biztonsági partnermegoldások hello felkeresésével [Azure piactér](https://azure.microsoft.com/marketplace/) és a keresést a "biztonság" és "hálózati biztonság."

## <a name="secure-remote-access-and-cross-premises-connectivity"></a>Biztonságos távoli hozzáférést és helyszínek közötti kapcsolat
A telepítő, beállítási és kezelési Azure-erőforrások kell toobe távolról. Emellett érdemes lehet toodeploy [hibrid informatikai](http://social.technet.microsoft.com/wiki/contents/articles/18120.hybrid-cloud-infrastructure-design-considerations.aspx) összetevők a helyszíni megoldások és hello Azure nyilvános felhőjében található. Ezek a forgatókönyvek biztonságos távoli hozzáférést igényelnek.

Azure hálózatkezelés hello a következő biztonságos távoli hozzáférés forgatókönyveket támogatja:

* Csatlakozás egyedi munkaállomások tooan Azure-beli virtuális hálózathoz
* Csatlakozás a helyi hálózati tooan Azure-beli virtuális hálózathoz egy VPN
* A helyszíni hálózati tooan Azure-beli virtuális hálózathoz csatlakozzon egy dedikált WAN-kapcsolat
* Csatlakozás az Azure virtuális hálózatok tooeach más

### <a name="connect-individual-workstations-tooan-azure-virtual-network"></a>Csatlakozás egyedi munkaállomások tooan Azure-beli virtuális hálózathoz
Előfordulhat, ha azt szeretné, tooenable egyedi fejlesztők és a műveleti személyzet toomanage virtuális gépek és szolgáltatások az Azure-ban. Például el kell érni tooa virtuális gépet egy Azure virtuális hálózaton, és a biztonsági házirend nem engedélyezi az RDP és az SSH távelérési tooindividual virtuális gépek. Ebben az esetben egy pont – hely VPN-kapcsolatot is használhatja.

pont-pont VPN-kapcsolat használ hello hello [az SSTP VPN](https://technet.microsoft.com/library/cc731352.aspx) protokoll tooenable tooset magán- és biztonságos kapcsolat hello felhasználói és hello Azure virtuális hálózat között. Miután hello VPN-kapcsolat létrejött, hello felhasználó fog tudni tooRDP vagy SSH keresztül hello VPN csatolhatja a virtuális gép hello Azure virtuális hálózat (feltéve, hogy hello felhasználói hitelesítheti és engedélyezve van).

További információ:

* [Egy pont – hely kapcsolat tooa virtuális hálózat PowerShell konfigurálása](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

### <a name="connect-your-on-premises-network-tooan-azure-virtual-network-with-a-vpn"></a>Csatlakozás a helyszíni hálózat tooan Azure-beli virtuális hálózathoz egy VPN
Érdemes lehet tooconnect a teljes vállalati hálózaton, vagy részeit, tooan Azure-beli virtuális hálózathoz. Ez az a hibrid informatikai gyakori forgatókönyvek ahol vállalat [kiterjesztése a helyszíni adatközpontját az Azure](https://gallery.technet.microsoft.com/Datacenter-extension-687b1d84). Sok esetben a vállalatok a szolgáltatás az Azure és részei a helyszínen, például amikor egy megoldás tartalmaz az Azure és a háttér-adatbázisok a helyszíni előtér-webkiszolgálók részei fogja futtatni. Az ilyen típusú "létesítmények közötti" azt is ellenőrizze található Azure felügyeleti erőforrások több biztonságos, és engedélyezze a forgatókönyvek az Azure Active Directory-tartományvezérlők kiterjesztése például.

Egyirányú tooaccomplish toouse azt egy [telephelyek közötti VPN](https://www.techopedia.com/definition/30747/site-to-site-vpn). hello a telephelyek közötti VPN és közötti pont-pont típusú VPN különbség, hogy pont-pont típusú VPN csatlakozik-e egy Azure-beli virtuális hálózatra, egyetlen eszköz tooan, amíg a telephelyek közötti VPN egy teljes hálózatokhoz (például a helyszíni hálózat) tooan Azure-beli virtuális hálózatra csatlakozik . Pont-pont VPN tooan Azure Virtual Network hello rendkívül biztonságos IPsec alagút módú VPN protokollt használja.

További információ:

* [Erőforrás-kezelő VNet hello Azure portál használatával pont-pont VPN-kapcsolat létrehozása](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)
* [A VPN Gateway tervezése és kialakítása](../vpn-gateway/vpn-gateway-plan-design.md)

### <a name="connect-your-on-premises-network-tooan-azure-virtual-network-with-a-dedicated-wan-link"></a>A helyszíni hálózat tooan Azure-beli virtuális hálózathoz csatlakozzon egy dedikált WAN-kapcsolat
Pont-pont és a pont-pont VPN-kapcsolatok érvényben engedélyezése közötti kapcsolatot nyújthassanak. Azonban egyes szervezetek tekinti azokat a következő hátrányokkal toohave hello:

* VPN-kapcsolatok adat átvitele hello Internet – Ez mutatja a kapcsolatok toopotential biztonsági problémák adminisztrációjával kapcsolatos adatmozgatás nyilvános hálózaton keresztül. Ezenkívül megbízhatósági és rendelkezésre állás a hálózati kapcsolatok használata esetén nem biztosítható.
* VPN-kapcsolatok tooAzure virtuális hálózatok az egyes alkalmazások és a céljából, amint azok maximális kimenő körülbelül 200 Mbps sebességnél korlátozott sávszélesség tekinthetők meg.

A szervezeteknek, amelyek általában kell hello legmagasabb szintű biztonság és elérhetőség a létesítmények közötti kapcsolatok dedikált WAN-kapcsolatok tooconnect tooremote helyek használja. Azure biztosítja, hogy hello képességét toouse használható tooconnect a helyszíni hálózati tooan Azure Virtual Network dedikált WAN-kapcsolaton. Ez az Azure ExpressRoute keresztül engedélyezhető.

További információ:

* [ExpressRoute műszaki áttekintés](../expressroute/expressroute-introduction.md)

### <a name="connect-azure-virtual-networks-tooeach-other"></a>Csatlakozás az Azure virtuális hálózatok tooEach más
Lehetséges, hogy toouse sok Azure virtuális hálózatok a központi telepítés. Számos oka miért erre akkor lehet szükség. Hello okok valamelyike miatt előfordulhat, hogy toosimplify felügyeleti; egy másik biztonsági okok miatt lehet. Hello kifejlesztésének és az Azure különböző virtuális hálózatokon lévő erőforrások üzembe indoklása, függetlenül előfordulhat, ha azt szeretné, hogy az egyes hello hálózatok tooconnect egymással erőforrások alkalommal.

Egy beállítás lenne egy Azure virtuális hálózat tooconnect tooservices egy másik Azure virtuális hálózat a szolgáltatások által "ismétlési vissza" hello interneten keresztül. hello kapcsolat ehhez egy Azure virtuális hálózatra, hello interneten keresztül, és ezután térjen vissza toohello cél Azure-beli virtuális hálózathoz. Ezt a beállítást hello kapcsolat toohello biztonsági problémák rejlő tooany Internet-alapú kommunikációt tesz elérhetővé.

Jobb megoldás lehet egy Azure virtuális hálózat-Azure virtuális hálózati telephelyek közötti VPN toocreate. Az Azure virtuális hálózat Azure virtuális hálózati telephelyek közötti VPN használ hello azonos [IPsec-bújtatásos mód](https://technet.microsoft.com/library/cc786385.aspx) hello létesítmények közötti pont-pont VPN-kapcsolat fent említett protokoll.

hello egy Azure virtuális hálózat-Azure virtuális hálózati telephelyek közötti VPN előnye, hogy hello VPN-kapcsolaton keresztül hello Azure hálózati háló helyett hello interneten keresztül csatlakozó létrejön. Ez lehetővé teszi, hogy Ön egy további biztonsági réteget toosite-site VPN hello interneten keresztül csatlakozó képest.

További információ:

* [Egy VNet – VNet-kapcsolat beállítása az Azure Resource Manager és a PowerShell használatával](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

## <a name="availability"></a>Rendelkezésre állás
Rendelkezésre állás bármilyen biztonsági program nyilvános kulcsokra épülő. Ha a felhasználók és a rendszer nem tud hozzáférni milyen azok kell keresztül hello tooaccess hálózati, hello szolgáltatás sérült tekinthető. Azure rendelkezik hálózatkezelési technológiákat, hogy a következő magas rendelkezésre állású mechanizmusok támogatási hello:

* HTTP-alapú terheléselosztás
* Hálózati szintű terheléselosztás
* Globális terheléselosztás

Terheléselosztási egy tervezett mechanizmus tooequally terjeszteni a kapcsolatokat több eszközök között. terheléselosztás hello céljai a következők:

* Növelje a rendelkezésre állása – betölteni a egyenleg kapcsolatok több eszközön, egy vagy több hello eszközök elérhetetlenné válhat, és a fennmaradó online eszközök hello hello szolgáltatás továbbra is tooserve hello tartalmat hello szolgáltatás
* A teljesítmény növelése – Ha az egyenleg kapcsolatok több eszközön, egyetlen eszközt nincs tootake hello processzor találati. Ehelyett hello és memóriáját iránti igények kielégítése érdekében a hello tartalom megoszlik több eszközön.

### <a name="http-based-load-balancing"></a>HTTP-alapú terheléselosztás
A szervezetek, hogy futtassa web-alapú szolgáltatások gyakran kívánják-e egy HTTP-alapú terheléselosztó előtt ezeket a webes szolgáltatások toohelp toohave megfelelő szintű teljesítmény és a magas rendelkezésre állás biztosítását. Ezzel szemben a hálózatalapú tootraditional terheléselosztóra, hello terhelés terheléselosztási döntések HTTP-alapú a terheléselosztók által vannak jellemzők alapján hello HTTP protokoll, nem a hello hálózati és az átviteli rétegbeli protokollokkal.

tooprovide, HTTP-alapú terheléselosztást a web-alapú szolgáltatások, Azure biztosítja, hogy hello Azure Application Gateway. hello Azure Application Gateway támogatja:

* HTTP-alapú terheléselosztás – terhelés terheléselosztási döntéseket alapján jellemző különleges toohello HTTP protokollal
* Munkamenet cookie-alapú kapcsolat – Ez a funkció gondoskodik arról, hogy a kapcsolatok létre tooone hello kiszolgálók, hogy a terheléselosztó mögött hello ügyfél és kiszolgáló közötti érintetlen marad. Ez biztosítja, hogy a tranzakciók stabilitását.
* SSL kiszervezési – ha ügyfél létrejön a kapcsolat hello terheléselosztással, hogy a munkamenet hello ügyfél és a hello terheléselosztót között titkosítva van-e a hello HTTPS (SSL /) protokollt. Azonban a rendelés tooincrease teljesítményét, internetkapcsolata hello beállítás toohave hello hello terheléselosztó és hello terheléselosztó használata hello (titkosítatlanul) HTTP protokollja mögött hello webkiszolgáló között. Ennek oka az "SSL kiszervezési" hivatkozott tooas hello webkiszolgálók hello terheléselosztó mögött hello processzor van szükség a titkosítás nem észlelnek, és ezért kell tudni tooservice kérelmek gyorsabban.
* URL-alapú tartalom útválasztás – Ez a funkció a hello terhelés terheléselosztó toomake döntést arról, ha a tooforward kapcsolatok hello cél URL-címe alapján lehetővé teszi. Ez a mint megoldások, amelyek IP-címek alapján terheléselosztási döntések betöltése sokkal nagyobb rugalmasságot biztosít.

További információ:

* [Átjáró – áttekintés](../application-gateway/application-gateway-introduction.md)

### <a name="network-level-load-balancing"></a>Hálózati szintű terheléselosztás
Ezzel szemben tooHTTP-alapú terheléselosztást, a hálózati szintű terheléselosztás teszi IP-cím és port (TCP és UDP) számok alapján terheléselosztási döntések betölteni.
Hálózati szintű az Azure-ban az terheléselosztást hello Azure terheléselosztó hello előnyei férhet. Hello Azure Load Balancer néhány főbb jellemzői a következők:

* Hálózati szintű terheléselosztás IP-cím és port számok alapján
* Bármely alkalmazás layer protokoll támogatása
* Felhőalapú szolgáltatások szerepkörpéldányokat és egyenlegének tooAzure virtuális gépek betöltése
* Internet felé néző (külső terheléselosztás) és a nem internetes is használható (belső terheléselosztás) alkalmazások és a virtuális gépek
* Végpont figyelés, amely használt toodetermine Ha hello terheléselosztó mögött hello szolgáltatások váltak nem érhető el

További információ:

* [Internetkapcsolattal rendelkező terheléselosztó több virtuális gép vagy szolgáltatás között](../load-balancer/load-balancer-internet-overview.md)
* [Belső terheléselosztó áttekintése](../load-balancer/load-balancer-internal-overview.md)

### <a name="global-load-balancing"></a>Globális terheléselosztás
Egyes szervezetek lehetséges érdemes hello legmagasabb szintű elérhetőség. Egyirányú tooreach a célja a toohost alkalmazások globálisan elosztott adatközpontokban. Egy alkalmazás adatközpontokban hello world belül üzemel esetén lehetséges, egy teljes geopolitikai régió toobecome nem érhető el, és továbbra is rendelkeznie hello alkalmazás és futnak.

Ezenkívül toohello rendelkezésre állási előnyeit úgy globálisan elosztott adatközpontokban üzemeltetéséhez, is kaphat a teljesítménybeli előnyökben. A teljesítmény előnyök olyan mechanizmus, amely arra utasítja a hello szolgáltatás toohello datacenter legközelebbi toohello eszköz, amely lehetővé teszi a hello kérelem kérelmek használatával érhető el.

Globális terheléselosztás adja meg mindkét ezek előnyeit. Az Azure ettől kezdve az Azure Traffic Manager globális terheléselosztást hello előnyeit.

További információ:

* [A Traffic Manager ismertetése](../traffic-manager/traffic-manager-overview.md)


## <a name="name-resolution"></a>Névfeloldás
Névfeloldás, akkor minden szolgáltatás Azure működteti a kritikus függvény. Biztonsági szempontból sérült biztonság esetén hello név feloldása függvény tooan támadó átirányítása kérelmek vezethet a helyek tooan támadó helyről. Biztonságos névfeloldás feltétele a felhőben üzemeltetett szolgáltatások.

A névfeloldás tooaddress van szüksége a két típusa van:

* Belső névfeloldást – belső névfeloldást szolgáltatások az Azure virtuális hálózatok, a helyszíni hálózatokhoz vagy mindkettőt használja. A belső névfeloldáshoz használt nevek hello interneten keresztül nem érhetők el. Optimális biztonsági fontos, hogy a belső név feloldása séma nincs elérhető tooexternal felhasználók.
* Külső nevek feloldása – külső névfeloldás felhasználókkal és eszközökkel kívül a helyszíni és az Azure virtuális hálózatot használja. Ezek a hello nevét, amely látható toohello Internet, illetve használt toodirect csatlakozási tooyour felhőalapú szolgáltatás.

A belső névfeloldást két lehetőség közül választhat:

* Egy Azure virtuális hálózat DNS-kiszolgáló – egy új, Azure virtuális hálózatban, létrehozásakor a DNS-kiszolgáló létrejön. A DNS-kiszolgáló képes névfeloldásra hello Azure virtuális hálózatban található hello gépek. A DNS-kiszolgáló nem konfigurálható, és hello Azure-hálót kezelő, így azt egy biztonságos név feloldása megoldás kezeli.
* Kapcsolja a saját DNS-kiszolgáló – lehetősége van hello saját kiválasztása az Azure virtuális hálózat DNS-kiszolgáló helyezésének. A DNS-kiszolgáló lehet egy Active Directory integrált DNS-kiszolgáló vagy egy Azure-partner, az Azure piactér hello által biztosított dedikált DNS-kiszolgáló megoldást.

További információ:

* [Virtual Network áttekintése](../virtual-network/virtual-networks-overview.md)
* [Egy virtuális hálózatot (VNet) által használt DNS-kiszolgálók kezelése](../virtual-network/virtual-network-manage-network.md#dns-servers)

A külső DNS-feloldás két lehetőség közül választhat:

* A saját külső DNS kiszolgáló helyszíni üzemeltetéséhez
* A saját külső DNS-kiszolgáló üzemeltetésére szolgáltató

Sok nagy méretű szervezeteknek saját DNS kiszolgálók helyszíni fogja futtatni. Ehhez úgy hálózati szakértelmét és a globális jelenlét toodo hello miatt.

A legtöbb esetben a DNS-névfeloldás szolgáltatások szolgáltató jobb toohost. Ezek a szolgáltatók hello hálózati szakértelmét és a globális jelenlét tooensure nagyon magas rendelkezésre állásúvá tenni a névfeloldási szolgáltatásokat rendelkeznek. Rendelkezésre állási elengedhetetlen DNS szolgáltatási, mert a név feloldása sikertelen szolgáltatásokat, ha nem fog tudni tooreach kell az internetes szolgáltatások.

Azure biztosít magas rendelkezésre állású és performant külső DNS-megoldást az Azure DNS hello formában. A külső név feloldása megoldás hello világszerte Azure DNS-infrastruktúra kihasználja. Toohost lehetővé teszi a tartományt az Azure használatával hello ugyanazon hitelesítő adatokkal, API-k, eszközök és számlázási, a más Azure-szolgáltatásokkal. Azure részeként hello erős biztonsági vezérlők hello platform épített is örököl.

További információ:

* [Az Azure DNS áttekintése](../dns/dns-overview.md)

## <a name="dmz-architecture"></a>DMZ-architektúra
Számos vállalati szervezet használja DMZ-k toosegment a hálózatok toocreate puffer-zónának hello Internet és a szolgáltatások között. hello hálózati hello DMZ része egy alacsony biztonsági szintű zónában minősülnek, és nem kiemelten értékesnek kerülnek, a hálózati szegmenst. Hálózati biztonsági eszközök a hálózati adapternek hello DMZ szegmens és egy másik hálózati illesztő csatlakoztatott tooa hálózat, amelyen a virtuális gépeket és szolgáltatásokat, amely a bejövő kapcsolatokat fogadjon hello Internet általában láthatja.

Számos DMZ tervezési és hello döntési toodeploy DMZ és majd DMZ toouse milyen típusú Ha úgy dönt, toouse alapul a hálózati biztonsági követelményeinek.

További információ:

* [A Microsoft Felhőszolgáltatásai és a hálózati biztonság](../best-practices-network-security.md)


## <a name="monitoring-and-threat-detection"></a>Figyelés és a fenyegetések észlelése

Azure toohelp képességeket biztosít a korai megkereshetők, figyelés és hello képességét toocollect, és nézze meg hálózati adatforgalommal a kulcsfontosságú terület.

### <a name="azure-network-watcher"></a>Az Azure hálózati figyelőt
Azure hálózati figyelőt képességeket kínál, amelyek segítenek a hibaelhárításban, valamint a adjon meg egy teljesen új eszközök tooassist hello biztonsági problémákat azonosítása a nagy számú tartalmazza.

[Biztonsági csoport megtekintése ](/network-watcher/network-watcher-security-group-view-overview.md) elősegíti a virtuális gépek naplózási és biztonsági megfelelőségét, és használt tooperform hello alaptervek házirendek a szervezet tooeffective szabályok határozzák meg a virtuális gépek mindegyikének összehasonlításával programozott eseményeket. Ez segít azonosítani a konfigurációs eltéréseket.

[Csomagrögzítéssel](/network-watcher/network-watcher-packet-capture-overview.md) lehetővé teszi a toocapture hálózati forgalom tooand hello virtuális gépről. Segítése lehetővé tételével toocollect hálózati statisztikákat és hello hibaelhárítás alkalmazás mellett problémák csomagrögzítéssel lehet hasznos információt hálózati behatolások hello vizsgálata során. Ez a funkció az Azure Functions hálózatrögzítéseket toostart válasz toospecific Azure együtt is használható riasztásokat.

További információ az Azure hálózati figyelőt, és hogyan tesztelése hello funkciók némelyike a labs toostart hello körülnézhet [Azure hálózati figyelőt figyelési áttekintés](/network-watcher/network-watcher-monitoring-overview.md)

>[!NOTE]
Az Azure hálózati figyelőt még nyilvános előzetes, előfordulhat, hogy nem rendelkezik hello azonos szintű rendelkezésre állást és megbízhatóságot, amelyek általában a rendelkezésre állási kiadás szolgáltatásként. Egyes funkciók nem támogatott, van, korlátozott képességeit, és előfordulhat, hogy nem érhető el az összes Azure helyét. Az értesítésekhez hello legfrissebb a rendelkezésre állás és a szolgáltatás állapotának ellenőrzése hello [Azure frissítések lap](https://azure.microsoft.com/updates/?product=network-watcher)

### <a name="azure-security-center"></a>Azure Security Center
A Security Center segítséget nyújtanak a megakadályozása, észlelésében és kezelésében toothreats, és itt a lássák, és szabályozhatják, az Azure-erőforrások biztonságának hello nőtt. Biztosít integrált biztonsági monitorozást és az Azure-előfizetésekkel, megkönnyíti a nehezen észlelhető fenyegetések azonosítását szabályzatkezelést, és számos biztonsági megoldással együttműködik.

Azure Security Center segítségével optimalizálása és figyelheti a hálózati biztonsági megoldást:

* Hálózati biztonsági javaslatokat biztosít
* A hálózati biztonsági konfiguráció hello állapotának figyelése
* Riasztás, akkor alapú toonetwork fenyegetések mindkét hello végpont és a hálózati szintű

További információ:

* [A Security Center bemutatása tooAzure](../security-center/security-center-intro.md)


### <a name="logging"></a>Naplózás
Hálózati szintű naplózási, akkor egy olyan hálózati biztonsági forgatókönyv kulcs függvény. Az Azure-hálózati biztonsági csoportok tooget hálózati szintű naplózási adatok használt információkat bejelentkezhet. Az NSG-naplózást is, olvasható be a következőről:

* [Tevékenységi naplóit](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) – ezek a naplók használt tooview összes művelet benyújtott tooyour Azure előfizetések. Ezek a naplók alapértelmezés szerint engedélyezve vannak, és hello Azure-portálon belül használható. Ezek volt korábbi nevén "Naplófájlok" vagy "Működési Logs".
* Eseménynaplók – ezek a naplók ismertetik, milyen NSG-szabályok alkalmazása megtörtént.
* Számláló – ezek a naplók segítségével minden NSG-szabály lett alkalmazott toodeny hány alkalommal tudja vagy -kezelési forgalom engedélyezése.

Is [Microsoft Power BI](https://powerbi.microsoft.com/what-is-power-bi/), hatékony adatábrázolási eszközzel, tooview, és ezek a naplók elemzése.

További információ:

* [Naplóelemzési a hálózati biztonsági csoportokkal (NSG-k)](../virtual-network/virtual-network-nsg-manage-log.md)
