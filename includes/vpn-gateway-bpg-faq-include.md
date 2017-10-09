### <a name="is-bgp-supported-on-all-azure-vpn-gateway-skus"></a>Minden Azure VPN Gateway SKU-n támogatott a BGP?
Nem, a BGP az Azure **Standard** és a **HighPerformance** VPN Gatewayeken támogatott. Az **alapszintű** SKU NEM támogatott.

### <a name="can-i-use-bgp-with-azure-policy-based-vpn-gateways"></a>Használhatom a BGP-t Azure házirendalapú VPN Gatewayekkel?
Nem, a BGP csak az útvonalalapú VPN Gatewayeken támogatott.

### <a name="can-i-use-private-asns-autonomous-system-numbers"></a>Használhatok privát ASN-eket (Autonomous System Numbers)?
Igen, a helyszíni hálózatokhoz és az Azure virtuális hálózatokhoz is használhat saját nyilvános ASN-eket vagy titkos ASN-eket.

### <a name="are-there-asns-reserved-by-azure"></a>Vannak ASN-ek, amelyeket az Azure lefoglal?
Igen, hello következő ASN-ek számára vannak fenntartva az Azure külső és belső esetében:

* Nyilvános ASN-ek: 8075, 8076, 12076
* Privát ASN-ek: 65515, 65517, 65518, 65519, 65520

VPN-átjárók tooAzure kapcsolódáskor ezek ASN-eket nem lehet meghatározni a helyszíni VPN-eszközök.

### <a name="are-there-any-other-asns-that-i-cant-use"></a>Vannak olyan ASN-ek, amelyeket nem használhatok?
Igen, a következő ASN-eket hello vannak [IANA által fenntartott](http://www.iana.org/assignments/iana-as-numbers-special-registry/iana-as-numbers-special-registry.xhtml) és az Azure VPN-átjáró nem konfigurálható:

23456, 64496–64511, 65535–65551 és 429496729

### <a name="can-i-use-hello-same-asn-for-both-on-premises-vpn-networks-and-azure-vnets"></a>Használhatok hello ugyanazt az ASN mind a helyszíni VPN-hálózatok és az Azure Vnetekhez?
Nem, a helyszíni hálózatokhoz és az Azure VNetekhez különböző ASN-eket kell hozzárendelnie, ha BGP-vel kapcsolja össze őket. Az Azure VPN Gatewayek alapértelmezett hozzárendelt ASN-je a 65515, függetlenül attól, hogy a BGP engedélyezve van-e a létesítmények közötti kapcsolathoz. Alapértelmezett módo felülbírálhatja egy eltérő ASN hozzárendelésével hello VPN-átjáró létrehozásakor, vagy hello ASN hello átjáró létrehozása után módosítható. A helyszíni ASN-eket toohello megfelelő Azure helyi hálózati átjáróhoz kell tooassign.

### <a name="what-address-prefixes-will-azure-vpn-gateways-advertise-toome"></a>Milyen címet előtagok lesz Azure VPN gatewayek toome?
Az Azure VPN gatewayek hello útvonalak tooyour a helyszíni BGP eszközöket a következő:

* Az Ön VNet-címelőtagjait
* Minden helyi hálózati átjárók csatlakoztatott toohello Azure VPN gateway címelőtagjait
* Más BGP társviszony-létesítési munkameneteket csatlakoztatott toohello Azure VPN-átjáró, tanult elérési utakat **kivéve alapértelmezett elérési uta VNet-előtagok átfedést**.

### <a name="can-i-advertise-default-route-00000-tooazure-vpn-gateways"></a>Hirdethető alapértelmezett útvonalat (0.0.0.0/0) tooAzure VPN gatewayek?
Igen.

Ne feledje, ez arra kényszeríti az összes virtuális hálózat kimenő forgalmat a helyszíni hely felé, és megakadályozza, hogy a hello virtuális hálózat virtuális gépek hello Internet közvetlenül, ilyen RDP vagy hello Internet toohello virtuális gépek az SSH nyilvános kommunikációt fogad.

### <a name="can-i-advertise-hello-exact-prefixes-as-my-virtual-network-prefixes"></a>Is hello pontos előtagok is hivatkozik, a virtuális hálózati előtagok?

Nem, ugyanaz, mint a virtuális hálózati címelőtagokat egyikét sem előtagok hirdetési hello blokkolva lesz, vagy hello Azure platformon szerint szűrve. Olyan előtagot azonban meghirdethet, amelynek a virtuális hálózaton belüli állomások a részhalmazát alkotják. 

Ha a virtuális hálózat hello cím terület 10.0.0.0/16 használ, például sikerült 10.0.0.0/8 esetében. A 10.0.0.0/16 vagy 10.0.0.0/24 előtagot azonban nem hirdetheti meg.

### <a name="can-i-use-bgp-with-my-vnet-to-vnet-connections"></a>Használhatom a BGP-t a VNet–VNet kapcsolatokhoz?
Igen, a BGP-t létesítmények közötti és VNet–VNet kapcsolatokhoz is használhatja.

### <a name="can-i-mix-bgp-with-non-bgp-connections-for-my-azure-vpn-gateways"></a>Kombinálhatom a BGP-t nem BGP-kapcsolatokkal az Azure VPN Gatewayeknél?
Igen, kombinálhatja a mindkét BGP, és nem BGP-kapcsolatokat ugyanazon Azure VPN gatewaynél hello.

### <a name="does-azure-vpn-gateway-support-bgp-transit-routing"></a>Támogatja az Azure VPN Gateway a BGP-tranzit útválasztást?
Igen, a BGP-tranzit útválasztás támogatott, Azure VPN gatewayek hello kivétellel **nem** alapértelmezett útvonalak BGP-társakat tooother esetében. tooenable tranzit útválasztás több Azure VPN gatewayek keresztül, engedélyeznie kell a BGP az összes köztes VNet – VNet kapcsolatokhoz.

### <a name="can-i-have-more-than-one-tunnel-between-azure-vpn-gateway-and-my-on-premises-network"></a>Használhatok több alagutat az Azure VPN Gateway és a helyszíni hálózat között?
Igen, több S2S VPN-alagutat is létrehozhat az Azure VPN Gateway és a helyszíni hálózata között. Vegye figyelembe, hogy ezek alagutak beleszámítanak az Azure VPN gatewayek az alagutak száma hello és BGP engedélyeznie kell a mindkét alagutat.

Például két redundáns alagútja van az Azure VPN gateway és egy a helyszíni hálózat között, ha azok fog 2 alagutat használnak fel az Azure VPN gateway (Standard esetében 10) HighPerformance esetében pedig 30 teljes kvótája hello kívül.

### <a name="can-i-have-multiple-tunnels-between-two-azure-vnets-with-bgp"></a>Használhatok több alagutat két BGP-vel rendelkező Azure VNet között?
Igen, de legalább egy virtuális hálózati átjárók hello aktív-aktív konfigurációban kell lennie.

### <a name="can-i-use-bgp-for-s2s-vpn-in-an-expressroutes2s-vpn-co-existence-configuration"></a>Használhatok BGP-t az S2S VPN-hez egy olyan konfigurációban, amelyben az ExpressRoute és az S2S VPN is jelen van?
Igen. 

### <a name="what-address-does-azure-vpn-gateway-use-for-bgp-peer-ip"></a>Milyen címet használ az Azure VPN Gateway a BGP-társgép IP-címéhez?
hello Azure VPN gateway egyetlen IP-címnek hello hello virtuális hálózathoz definiált GatewaySubnet-tartományból származó foglal le. Alapértelmezés szerint hello utolsó előtti címe hello tartományon. Például ha 10.12.255.0/27, kezdve a 10.12.255.0 too10.12.255.31, hello hello Azure VPN gateway a BGP társ IP-címe 10.12.255.30 lesz. Ezek az információk található hello Azure VPN gateway információinak felsorolásához.

### <a name="what-are-hello-requirements-for-hello-bgp-peer-ip-addresses-on-my-vpn-device"></a>Mik azok a VPN-eszközön a BGP-Társgép IP-címek hello hello követelményei?
A helyszíni BGP-Társgép címe **kell nem** kell hello ugyanaz, mint a VPN-eszköz hello nyilvános IP-címe. Egy másik IP-cím használatát hello VPN-eszköz a BGP társ IP-címhez. Lehet, hogy a hozzárendelt toohello hello eszköz visszacsatolási címhez. Adja meg ezt a címet hello megfelelő helyi hálózati átjáró képviselő hello helyen.

### <a name="what-should-i-specify-as-my-address-prefixes-for-hello-local-network-gateway-when-i-use-bgp"></a>Mit adjak meg a címelőtagokat hello helyi hálózati átjáró címelőtagjaként a BGP?
Az Azure helyi hálózati átjáró hello kezdeti címelőtagokat a helyszíni hálózat hello határozza meg. A BGP-vel hello állomás előtagot kell rendelnie (/ 32 előtag) hello címterület a helyszíni hálózat, a BGP-Társgép IP-cím. Ha a BGP-Társgép IP-10.52.255.254, meg kell adni a hello a helyszíni hálózatot képviselő helyi hálózati átjáró hello localNetworkAddressSpace mint "10.52.255.254/32". Ez az tooensure, amely hello Azure VPN gateway létrehozza hello BGP munkamenetet hello S2S VPN-alagúton keresztül.

### <a name="what-should-i-add-toomy-on-premises-vpn-device-for-hello-bgp-peering-session"></a>Mit kell felvennem toomy a helyszíni VPN-eszköz a BGP társviszony-létesítési munkamenetet hello?
Meg kell mutató gazdaútvonalat hello Azure BGP-Társgép IP-cím, a VPN-eszköz toohello IPsec S2S VPN-alagútra mutat. Például "10.12.255.30" hello Azure VPN-Társgép IP-esetén meg kell mutató gazdaútvonalat a "10.12.255.30" hello megfelelő IPsec alagútkapcsolatának a következő ugrás felülettel rendelkező a VPN-eszköz.

