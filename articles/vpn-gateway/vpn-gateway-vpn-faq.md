---
title: "aaaAzure VPN Gateway – gyakori kérdések |} Microsoft Docs"
description: "hello VPN Gateway – gyakori kérdések. Gyakori kérdések a Microsoft Azure Virtual Network telephelyek közötti kapcsolatairól, a hibrid konfigurációjú kapcsolatokról és a VPN-átjárókról."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
ms.assetid: 6ce36765-250e-444b-bfc7-5f9ec7ce0742
ms.service: vpn-gateway
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/30/2017
ms.author: cherylmc,yushwang
ms.openlocfilehash: e1737f5832728f513e31f97cc7e752147faaaeb4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="vpn-gateway-faq"></a>VPN Gateway – gyakori kérdések

## <a name="connecting"></a>Kapcsolódó toovirtual hálózatok

### <a name="can-i-connect-virtual-networks-in-different-azure-regions"></a>Összekapcsolhatok eltérő Azure-régiókban található virtuális hálózatokat?

Igen. Nincs régiókorlátozás. Egy virtuális hálózati képes kapcsolódni a virtuális hálózat tooanother hello ugyanabban a régióban, vagy egy másik Azure-régióban. 

### <a name="can-i-connect-virtual-networks-in-different-subscriptions"></a>Összekapcsolhatok egymással különböző előfizetésekben található virtuális hálózatokat?

Igen.

### <a name="can-i-connect-toomultiple-sites-from-a-single-virtual-network"></a>Csatlakoztathatok toomultiple helyek egyetlen virtuális hálózatról?

Toomultiple helyek hello Azure REST API-k és a Windows PowerShell használatával is elérheti. Lásd: hello [többhelyes és VNet – VNet-kapcsolatot](#V2VMulti) feltett.

### <a name="what-are-my-cross-premises-connection-options"></a>Milyen lehetőségeim vannak a létesítmények közötti kapcsolódásra?

a következő hello létesítmények közötti kapcsolatok támogatottak:

* Helyek közötti kapcsolat – VPN-kapcsolat IPsec (IKE v1 és IKE v2) használatával. Ehhez a kapcsolattípushoz VPN-eszköz vagy RRAS szükséges. További információ: [Helyek közötti kapcsolat](vpn-gateway-howto-site-to-site-resource-manager-portal.md).
* Pont–hely kapcsolat – VPN-kapcsolat SSTP (Secure Socket Tunneling Protocol) használatával. Ehhez a kapcsolattípushoz nem szükséges VPN-eszköz. További információ: [Pont–hely kapcsolat](vpn-gateway-howto-point-to-site-resource-manager-portal.md).
* VNet – VNet – ilyen típusú kapcsolat van hello ugyanaz, mint a pont-pont konfigurációja. VNet tooVNet (IKE v1 és IKE v2) IPSec VPN-kapcsolat. nem szükséges hozzá VPN-eszköz. További információ: [Virtuális hálózatok közötti kapcsolat](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md).
* Többhelyes – Ez az a pont-pont konfigurációja, amely lehetővé teszi tooconnect változata több helyszíni helyek tooa virtuális hálózat. További információ: [Többhelyes kapcsolat](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md).
* ExpressRoute – ExpressRoute a közvetlen kapcsolat tooAzure a WAN nem hello keresztül VPN-kapcsolatot a nyilvános internethez. További információkért lásd: hello [ExpressRoute műszaki áttekintés](../expressroute/expressroute-introduction.md) és hello [ExpressRoute – gyakori kérdések](../expressroute/expressroute-faqs.md).

További információk a VPN Gateway-kapcsolatokról: [Információk a VPN Gateway-ről](vpn-gateway-about-vpngateways.md).

### <a name="what-is-hello-difference-between-a-site-to-site-connection-and-point-to-site"></a>Mi az a pont-pont kapcsolat és a pont-pont hello különbségének?

A **helyek közötti** (IPsec/IKE VPN-alagút) konfigurációk az Ön telephelye és az Azure között vannak. Ez azt jelenti, hogy lehet csatlakoztatni a számítógépek, a helyi tooany virtuális gép vagy szerepkör példánya, attól függően, hogyan kezeli a tooconfigure az Útválasztás és engedélyek a virtuális hálózaton belül található. Ez ideális megoldás folyamatosan elérhető létesítmények közötti kapcsolatokhoz, és hibrid konfigurációkhoz is használható. Ilyen típusú kapcsolat egy IPsec VPN-készülék (hardvereszköz vagy letölthető készülék), amelyen telepítenie kell a hálózati hello szélén támaszkodik. toocreate Ez a kapcsolat típusa kell rendelkeznie, amely nincs a NAT mögött külsőleg irányuló IPv4-cím

**Pont-pont** (keresztül az SSTP VPN) konfigurációk lehetővé teszik, hogy egyetlen számítógépről bárhonnan csatlakozzon a virtuális hálózaton található tooanything. Hello Windows beépített VPN-ügyfél használ. Hello pont-pont konfigurációjának részeként telepítse a tanúsítványt és a VPN-ügyfél konfigurációs csomagot, amely hello beállításokat tartalmazza, amelyek lehetővé teszik a számítógép tooconnect tooany virtuális gép vagy szerepkörpéldány hello virtuális hálózaton belül. Is nagy tooconnect tooa virtuális hálózaton, de nincsenek a helyszínen. Az esetén is jó választás, nem rendelkezik hozzáférési tooVPN hardver vagy külsőleg irányuló IPv4-cím, is, amelyek szükségesek a webhelyek kapcsolatot.

Konfigurálhatja a virtuális hálózati toouse mindkét pont-pont és az átjáró írja be a pont-hely párhuzamosan, amennyiben az útvonalalapú VPN-nel webhelyek kapcsolatot hoz létre. Útvonalalapú VPN nevezik dinamikus átjárók hello klasszikus üzembe helyezési modellben.

## <a name="gateways"></a>Virtuális hálózati átjárók

### <a name="is-a-vpn-gateway-a-virtual-network-gateway"></a>A VPN Gateway virtuális hálózati átjáró?

A VPN Gateway a virtuális hálózati átjárók egy típusa. A VPN Gateway titkosított adatforgalmat továbbít nyilvános kapcsolaton keresztül a virtuális hálózat és az Ön telephelye között. Egy VPN-átjáró toosend virtuális hálózatok közötti forgalmat is használható. Amikor létrehoz egy VPN-átjáró, hello - GatewayType érték "Vpn" használata. További információ: [Információk a VPN Gateway konfigurációs beállításairól](vpn-gateway-about-vpn-gateway-settings.md).

### <a name="what-is-a-policy-based-static-routing-gateway"></a>Mik azok a házirendalapú (statikus útválasztású) átjárók?

A házirendalapú átjárók házirendalapú VPN-kapcsolatokat valósítanak meg. Házirendalapú VPN titkosításához, és irányítják a csomagokat a helyszíni hálózat és a hello Azure VNet közötti címelőtag hello kombinációi alapján IPsec-alagutakon keresztül. hello házirend (vagy Forgalomválasztó) általában van meghatározva egy hozzáférési listaként hello VPN-konfigurációban.

### <a name="what-is-a-route-based-dynamic-routing-gateway"></a>Mik azok az útvonalalapú (dinamikus útválasztású) átjárók?

Útválasztó-alapú átjárók megvalósítása hello Útvonalalapú. Útvonalalapú "útvonalakat" használja az hello IP-továbbítási vagy útvonalválasztási tábla toodirect csomagok be a megfelelő alagútkapcsolatokhoz irányítsák. hello alagútkapcsolatok ezután vagy titkosítására vagy visszafejtésére hello csomagok mindkét hello alagutat. házirend vagy forgalom választó hello Útvonalalapú vannak beállítva, bármely elem közöttiként (vagy helyettesítő karakterek).

### <a name="do-i-need-a-gatewaysubnet"></a>Szükségem van GatewaySubnetre?

Igen. hello átjáróalhálózatot hello IP-címek, amelyek hello virtuális hálózati átjáró szolgáltatásokat tartalmazza. Toocreate egy átjáró-alhálózatot a virtuális hálózat a rendelés tooconfigure a virtuális hálózati átjáró szükséges. Minden átjáró-alhálózat névvel kell ellátni "GatewaySubnet" toowork megfelelően. Ne nevezze el másként az átjáróalhálózatát, És nem telepítheti a virtuális gépek vagy semmi mást toohello átjáró-alhálózatot.

Amikor hello átjáró-alhálózatot hoz létre, megadhatja a hello száma alhálózat hello IP-címeket tartalmaz. hello IP-címek hello átjáróalhálózatot toohello átjárószolgáltatás foglal le. Bizonyos konfigurációk esetén van szükség, további IP-címek toobe lefoglalt toohello átjáró szolgáltatások mint mások. Azt szeretné, hogy meg arról, hogy az átjáró-alhálózatot tartalmaz elég IP-címek tooaccommodate jövőbeli növekedésre, és a lehetséges további új kapcsolat konfigurációk toomake. Tehát, míg egyes konfigurációkhoz létrehozhat kicsi, akár /29-es méretű átjáróalhálózatot is, ajánlott /27-est vagy nagyobbat létrehozni (/27, /26, /25 stb.). Hello hello konfigurációs követelményei nézze meg, hogy szeretné, hogy toocreate, és győződjön meg arról, hogy rendelkezik hello átjáró-alhálózatot ezen követelményeknek megfelelő.

### <a name="can-i-deploy-virtual-machines-or-role-instances-toomy-gateway-subnet"></a>Virtuális gép vagy szerepkör példányok toomy átjáróalhálózatot tudjanak telepíteni?

Nem.

### <a name="can-i-get-my-vpn-gateway-ip-address-before-i-create-it"></a>Megszerezhetem a VPN-átjáróm IP-címét, mielőtt létrehozom az átjárót?

Nem. Az átjáró első tooget hello IP-cím van toocreate. hello IP-cím módosításainak Ha törölje és hozza létre újra a VPN-átjárót.

### <a name="can-i-request-a-static-public-ip-address-for-my-vpn-gateway"></a>Kérhetek statikus nyilvános IP-címet a VPN-átjáróm számára?

Nem. Csak a dinamikus IP-cím hozzárendelése támogatott. Ez azonban nem jelenti azt, hogy hello IP-cím hozzá van rendelve tooyour VPN-átjáró után módosítja. hello egyetlen alkalom hello VPN átjáró IP-cím módosításainak mikor van hello átjáró törlődik, és újból létrehozza. hello VPN átjáró nyilvános IP-cím átméretezése, alaphelyzetbe állítása vagy más belső karbantartás vagy frissítés a VPN-átjáró nem változik. 

### <a name="how-does-my-vpn-tunnel-get-authenticated"></a>Hogyan történik a VPN-alagút hitelesítése?

Az Azure VPN PSK (előmegosztott kulcsos) hitelesítést használ. Előmegosztott kulcs (PSK) azt elő, ha a Microsoft hello VPN-alagút létrehozása. Hello automatikusan létrehozott PSK tooyour hello beállítása előmegosztott kulcs PowerShell-parancsmag vagy a REST API-t saját módosíthatja.

### <a name="can-i-use-hello-set-pre-shared-key-api-tooconfigure-my-policy-based-static-routing-gateway-vpn"></a>Használhatok hello beállítása előmegosztott kulcs API tooconfigure a házirendalapú VPN gateway (statikus útválasztás)?

Igen, a hello beállítása előmegosztott kulcs API és PowerShell parancsmag lehet használt tooconfigure Azure házirendalapú (statikus) VPN és a útvonalalapú (dinamikus) útválasztási VPN.

### <a name="can-i-use-other-authentication-options"></a>Használhatok más hitelesítési módszert?

Folyamatban, korlátozott toousing előmegosztott kulccsal (PSK) a hitelesítéshez.

### <a name="how-do-i-specify-which-traffic-goes-through-hello-vpn-gateway"></a>Hogyan adja meg amely forgalom halad át hello VPN-átjáró?

#### <a name="resource-manager-deployment-model"></a>Resource Manager-alapú üzemi modell

* PowerShell: "Címelőtagja" toospecify forgalom hello helyi hálózati átjáró használja.
* Azure-portálon: keresse meg a helyi hálózati átjáró toohello > konfigurációs > címterének.

#### <a name="classic-deployment-model"></a>Klasszikus üzemi modell

* Azure-portálon: keresse meg a klasszikus virtuális hálózatot toohello > VPN-kapcsolatok > telephelyek közötti VPN-kapcsolatok > helyi webhely neve > helyi > ügyfél címterület. 
* Klasszikus portál: minden tartomány hello átjárón keresztül küldött kívánt helyi hálózatok hello hálózatok oldalon a virtuális hálózat hozzáadása. 

### <a name="can-i-configure-forced-tunneling"></a>Konfigurálhatok kényszerített bújtatást?

Igen. Lásd: [Kényszerített bújtatás konfigurálása](vpn-gateway-about-forced-tunneling.md).

### <a name="can-i-set-up-my-own-vpn-server-in-azure-and-use-it-tooconnect-toomy-on-premises-network"></a>Állítsa be az Azure-ban saját VPN-kiszolgáló és tooconnect toomy a helyszíni hálózat használni?

Igen, akkor telepítheti a saját VPN-átjárók vagy a kiszolgálók a hello Azure piactér vagy a saját VPN-útválasztók létrehozása az Azure-ban. Felhasználó által definiált útvonalak tooconfigure van szüksége a virtuális hálózat tooensure továbbítódik megfelelően a helyszíni hálózatokhoz és a virtuális alhálózatok között.

### <a name="why-are-certain-ports-opened-on-my-vpn-gateway"></a>Miért vannak egyes portok nyitva a VPN-átjárómon?

Ezek szükségesek az Azure-infrastruktúra kommunikációjához. A portokat Azure-tanúsítványok védik (zárják le). Megfelelő tanúsítványok nélkül külső entitások, beleértve azokat átjárók hello ügyfelek nem lesz képes toocause bármely hatása ezekre a végpontokra.

VPN-átjáró alapvetően többhelyű eszköz koppintva hello ügyfél magánhálózati és egy hálózati adapter felé néző hello nyilvános hálózat egyetlen hálózati adapterrel. Azure-infrastruktúra entitások nem koppintson megfelelőségi okokból ügyfél saját hálózatba, így tooutilize nyilvános végpontok infrastruktúra kommunikációhoz szükségük. az Azure biztonsági naplózási rendszeresen beolvasott hello nyilvános végpontok.

### <a name="more-information-about-gateway-types-requirements-and-throughput"></a>További információk az átjárótípusokról, a követelményekről és az adatátviteli sebességről

További információ: [Információk a VPN Gateway konfigurációs beállításairól](vpn-gateway-about-vpn-gateway-settings.md).

## <a name="s2s"></a>Helyek közötti kapcsolatok és VPN-eszközök

### <a name="what-should-i-consider-when-selecting-a-vpn-device"></a>Mit érdemes figyelembe venni a VPN-eszköz kiválasztásakor?

Eszközszállítói partnereinkkel különböző standard helyek közötti VPN-eszközöket ellenőriztünk. Ismert kompatibilis VPN-eszközök, a megfelelő konfigurációs utasításokról vagy mintákat és eszköz specifikációk listája megtalálható hello [kapcsolatos VPN-eszközök](vpn-gateway-about-vpn-devices.md) cikk. Minden eszköz ismert kompatibilis tulajdonosaként hello eszközcsaládok a virtuális hálózati együtt kell működnie. toohelp a VPN-eszköz beállításához tekintse meg a toohello eszköz konfigurációs minta vagy tooappropriate eszköz termékcsalád megfelelő hivatkozásra.

### <a name="where-can-i-find-vpn-device-configuration-settings"></a>Hol találom a VPN-eszközök konfigurációs beállításait?

[!INCLUDE [vpn devices](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]

### <a name="how-do-i-edit-vpn-device-configuration-samples"></a>Hogyan szerkeszthetem a VPN-eszközök konfigurációs mintáit?

Az eszközök konfigurációs mintáinak szerkesztésével kapcsolatos információkért tekintse meg a [minták szerkesztésével](vpn-gateway-about-vpn-devices.md#editing) kapcsolatos részt.

### <a name="where-do-i-find-ipsec-and-ike-parameters"></a>Hol találom az IPsec/IKE-paramétereket?

Az IPsec/IKE-paraméterekkel kapcsolatos információkért tekintse meg a [paraméterekkel](vpn-gateway-about-vpn-devices.md#ipsec) kapcsolatos részt.

### <a name="why-does-my-policy-based-vpn-tunnel-go-down-when-traffic-is-idle"></a>Miért áll le a házirendalapú VPN-alagutam, amikor nincs adatforgalom?

Ez normális működés házirendalapú (más néven statikus útválasztású) VPN-átjárók esetében. Hello forgalom hello alagúton keresztül tétlen állapotában több mint 5 percig, hello alagút bontva lesz. Indulásakor forgalom továbbítására mindkét irányban hello alagút azonnal hozni.

### <a name="can-i-use-software-vpns-tooconnect-tooazure"></a>Használhatja a szoftver VPN tooconnect tooAzure?

A helyek közötti létesítmények közötti konfigurációkhoz támogatottak a Windows Server 2012 útválasztási és távelérési (RRAS) kiszolgálók is.

Egyéb szoftverek VPN-megoldásait kell működniük az átjáró, mindaddig, amíg azok megfelelnek tooindustry szabványos IPsec-megvalósítások. További konfigurációs és támogatás hello hello szoftver gyártójához forduljon.

## <a name="P2S"></a>Pont–hely kapcsolatok

[!INCLUDE [vpn-gateway-point-to-site-faq-include](../../includes/vpn-gateway-point-to-site-faq-include.md)]

## <a name="V2VMulti"></a>Virtuális hálózatok közötti kapcsolat és többhelyes kapcsolatok

[!INCLUDE [vpn-gateway-vnet-vnet-faq-include](../../includes/vpn-gateway-vnet-vnet-faq-include.md)]

### <a name="can-i-use-azure-vpn-gateway-tootransit-traffic-between-my-on-premises-sites-or-tooanother-virtual-network"></a>Használható Azure VPN gateway tootransit forgalom a helyszíni helyek vagy tooanother virtuális hálózat között?

**Resource Manager-alapú üzemi modell**<br>
Igen. Lásd: hello [BGP](#bgp) szakaszban olvashat.

**Klasszikus üzemi modell**<br>
Átmenő forgalom Azure VPN-átjárón keresztül lehetséges hello klasszikus üzembe helyezési modellel, de támaszkodik a statikusan megadott címterek hello hálózati konfigurációs fájlban. A BGP még nem támogatott az Azure virtuális hálózatok és a VPN-átjárók hello klasszikus telepítési modell használatával. BGP nélkül az átviteli címterek manuális meghatározása sok hibalehetőséggel jár, ezért nem ajánlott.

### <a name="does-azure-generate-hello-same-ipsecike-pre-shared-key-for-all-my-vpn-connections-for-hello-same-virtual-network"></a>Nem Azure készítése hello ugyanazon IPsec/IKE előre megosztott kulcs minden a VPN-kapcsolatok hello az azonos virtuális hálózati?

Nem, az Azure alapértelmezés szerint különböző előmegosztott kulcsokat hoz létre a különböző VPN-kapcsolatokhoz. Hello beállítása VPN Gateway kulcs REST API-t vagy a PowerShell parancsmag tooset hello kulcsérték inkább is használhatja. hello kulcs 1 too128 karakter közötti hosszúságú alfanumerikus karakterláncot kell megadni.

### <a name="do-i-get-more-bandwidth-with-more-site-to-site-vpns-than-for-a-single-virtual-network"></a>Nagyobb sávszélességhez jutok több helyek közötti VPN használatával, mint egyetlen virtuális hálózattal?

Nem, az összes VPN-alagutat, pont-pont VPN, többek között megosztani hello ugyanazon Azure VPN gateway és hello rendelkezésre álló sávszélességet.

### <a name="can-i-configure-multiple-tunnels-between-my-virtual-network-and-my-on-premises-site-using-multi-site-vpn"></a>Konfigurálhatok több alagutat a virtuális hálózatom és a helyszíni helyem között többhelyes VPN használatával?

Igen, de be kell állítania a BGP az mindkét alagutak toohello ugyanazon a helyen.

### <a name="can-i-use-point-to-site-vpns-with-my-virtual-network-with-multiple-vpn-tunnels"></a>Használhatok pont–hely VPN-t több VPN-alagúttal a virtuális hálózatomhoz?

Igen, pont-pont (P2S) VPN hello VPN-átjárók toomultiple a helyszíni helyek és a többi virtuális hálózatok összekötő használható.

### <a name="can-i-connect-a-virtual-network-with-ipsec-vpns-toomy-expressroute-circuit"></a>Csatlakoztathatok egy virtuális hálózati IPsec VPN toomy ExpressRoute-kapcsolatcsoportot?

Igen, ez támogatott. További információk: [Párhuzamosan fennálló ExpressRoute- és helyek közötti VPN-kapcsolatok konfigurálása](../expressroute/expressroute-howto-coexist-classic.md).

## <a name="ipsecike"></a>IPsec/Internetes kulcscsere házirendje

[!INCLUDE [vpn-gateway-ipsecikepolicy-faq-include](../../includes/vpn-gateway-ipsecikepolicy-faq-include.md)]


## <a name="bgp"></a>BGP

[!INCLUDE [vpn-gateway-bgp-faq-include](../../includes/vpn-gateway-bpg-faq-include.md)]

## <a name="vms"></a>Létesítmények közötti kapcsolatok és virtuális gépek

### <a name="if-my-virtual-machine-is-in-a-virtual-network-and-i-have-a-cross-premises-connection-how-should-i-connect-toohello-vm"></a>Ha a virtuális gép olyan virtuális hálózatot, és a létesítmények közötti kapcsolaton, hogyan kell kapcsolható toohello VM?

Több lehetősége van. Ha engedélyezve van a virtuális gép RDP, hello magánhálózati IP-cím használatával képes kapcsolódni a virtuális gép tooyour. Ebben az esetben kellene megadnia hello magánhálózati IP-cím és hello port, amelyet tooconnect túl (általában 3389-es). A virtuális gép hello forgalom tooconfigure hello portra lesz szüksége.

A privát IP-cím, amely rendelkezik található hello megegyezik egy másik virtuális gépről is csatlakozhat tooyour virtuális gép virtuális hálózati. RDP tooyour virtuális gépet nem lehet hello magánhálózati IP-cím használatával, ha a virtuális hálózaton kívüli helyről kapcsolódik. Például ha egy pont – hely virtuális hálózatot, és nem a kapcsolat létrehozása a számítógépről, nem csatlakozhat toohello virtuális gépek magánhálózati IP-címet.

### <a name="if-my-virtual-machine-is-in-a-virtual-network-with-cross-premises-connectivity-does-all-hello-traffic-from-my-vm-go-through-that-connection"></a>Ha a virtuális gépet tartalmazó virtuális hálózatok közötti kapcsolatot nyújthassanak, nem a virtuális gép teljes forgalmát hello halad át ezt a kapcsolatot?

Nem. Csak az olyan hello forgalom, amely rendelkezik a cél hello virtuális hálózat helyi hálózati IP-címtartományok megadott lévő IP hello virtuális hálózati átjárón keresztül halad. Forgalom esetében a cél IP hello virtuális hálózaton belül található hello virtuális hálózaton belül marad. Más forgalom hello load balancer toohello nyilvános hálózatokon keresztül küld el, vagy a kényszerített bújtatás használata esetén küldi hello Azure VPN-átjárón keresztül.

### <a name="how-do-i-troubleshoot-an-rdp-connection-tooa-vm"></a>Hogyan hibáinak elhárítása az RDP-kapcsolat tooa VM

[!INCLUDE [Troubleshoot VM connection](../../includes/vpn-gateway-connect-vm-troubleshoot-include.md)]


## <a name="faq"></a>Virtuális hálózat – gyakori kérdések

További virtuális hálózati adatok hello megjelenített [virtuális hálózati gyakran ismételt kérdések](../virtual-network/virtual-networks-faq.md).

## <a name="next-steps"></a>Következő lépések

* További információk a VPN Gatewayről: [Információk a VPN Gatewayről](vpn-gateway-about-vpngateways.md).
* További információk a VPN Gateway konfigurációs beállításairól: [Információk a VPN Gateway konfigurációs beállításairól](vpn-gateway-about-vpn-gateway-settings.md).
