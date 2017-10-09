hello VNet – VNet – gyakori kérdések tooVPN átjárókapcsolatokhoz vonatkozik. Ha társviszonyt szeretne létesíteni virtuális hálózatok között, lásd: [Társviszony létesítése virtuális hálózatok között](../articles/virtual-network/virtual-network-peering-overview.md)

### <a name="does-azure-charge-for-traffic-between-vnets"></a>Felszámol az Azure díjat a virtuális hálózatok közötti adatforgalomért?

Forgalmi VNet – VNet hello belül azonos régiót szabad mindkét irányban a használata esetén a VPN gateway-kapcsolatot. Közötti régió VNet – VNet kimenő forgalom hello kimenő többek VNet adatátviteli hello forrás régiók alapján számítjuk fel. Tekintse meg a toohello [árképzést ismertető oldalra VPN-átjáró](https://azure.microsoft.com/pricing/details/vpn-gateway/) részleteiről. Ha a Vnetek helyett Vnetben társviszony-létesítést, VPN-átjáró kapcsolódik, lásd: hello [árképzést ismertető oldalra virtuális hálózati](https://azure.microsoft.com/pricing/details/virtual-network/).

### <a name="does-vnet-to-vnet-traffic-travel-across-hello-internet"></a>Nem VNet – VNet forgalmát haladnak keresztül hello Internet?

Nem. VNet – VNet forgalmát a Microsoft Azure gerincét, nem hello Internet hello áthaladó.

### <a name="is-vnet-to-vnet-traffic-secure"></a>Biztonságos-e a virtuális hálózatok közötti adatforgalom?

Igen, az adatforgalmat IPsec/IKE-titkosítás védi.

### <a name="do-i-need-a-vpn-device-tooconnect-vnets-together"></a>Kell egy VPN-eszköz tooconnect Vnetek együtt?

Nem. Az Azure Virtual Networkök összekapcsolása nem igényel VPN-eszközöket, hacsak nem szükséges a létesítmények közötti kapcsolat.

### <a name="do-my-vnets-need-toobe-in-hello-same-region"></a>A Vnetek szükség van a hello toobe ugyanabban a régióban?

Nem. virtuális hálózatok hello lehet hello ugyanazon vagy másik Azure-régiók (hely).

### <a name="if-hello-vnets-are-not-in-hello-same-subscription-do-hello-subscriptions-need-toobe-associated-with-hello-same-ad-tenant"></a>Ha nincsenek a Vnetek hello hello azonos előfizetés, hello előfizetések kell hello ugyanazt az AD-bérlőhöz társított toobe?

Nem.

### <a name="can-i-use-vnet-to-vnet-along-with-multi-site-connections"></a>A virtuális hálózatok közötti kapcsolatot használhatom többhelyes kapcsolatokhoz?

Igen. A virtuális hálózati kapcsolat használható többhelyes virtuális VPN-ekkel együtt.

### <a name="how-many-on-premises-sites-and-virtual-networks-can-one-virtual-network-connect-to"></a>Hány helyszíni helyhez és virtuális hálózathoz kapcsolódhat egyetlen virtuális hálózat?

Lásd [Az átjáróra vonatkozó követelmények](../articles/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#requirements) táblázatot.

### <a name="can-i-use-vnet-to-vnet-tooconnect-vms-or-cloud-services-outside-of-a-vnet"></a>Használja a VNet – VNet tooconnect virtuális gépeken vagy felhőszolgáltatásokon egy virtuális hálózaton kívül?

Nem. A virtuális hálózatok közötti kapcsolat támogatja a virtuális hálózatok csatlakoztatását, Nem támogatja a nem virtuális hálózatban lévő virtuális gépek és felhőszolgáltatások csatlakoztatását.

### <a name="can-a-cloud-service-or-a-load-balancing-endpoint-span-vnets"></a>A felhőszolgáltatások és a terheléselosztási végpontok átívelhetnek több virtuális hálózaton?

Nem. A felhőszolgáltatás és a terheléselosztási végpont nem ívelhet át több virtuális hálózaton, akkor sem, ha ezek össze vannak kapcsolva.

### <a name="can-i-used-a-policybased-vpn-type-for-vnet-to-vnet-or-multi-site-connections"></a>Használhatok házirendalapú VPN-típust a virtuális hálózatok közötti vagy többhelyes kapcsolatokhoz?

Nem. A virtuális hálózatok közötti és többhelyes kapcsolatokhoz útvonalalapú (korábbi nevén dinamikus útválasztású) VPN-típussal rendelkező Azure VPN Gateway átjárók szükségesek.

### <a name="can-i-connect-a-vnet-with-a-routebased-vpn-type-tooanother-vnet-with-a-policybased-vpn-type"></a>I csatlakozhatnak egy virtuális hálózat egy RouteBased VPN-típus tooanother VNet PolicyBased VPN típusú?

Nem, mindkét virtuális hálózatnak útvonalalapú (korábban dinamikus útválasztású) VPN-t KELL használnia.

### <a name="do-vpn-tunnels-share-bandwidth"></a>A VPN-alagutak osztoznak a sávszélességen?

Igen. Hello virtuális hálózat összes VPN-alagutat megosztása hello Azure VPN gateway hello rendelkezésre álló sávszélességet, és hello azonos VPN gateway hasznos üzemidő SLA-t az Azure-ban.

### <a name="are-redundant-tunnels-supported"></a>Támogatottak a redundáns alagutak?

A virtuális hálózatok párjai közötti redundáns alagutak nem támogatottak, amikor a virtuális hálózati átjáró aktív-aktívként van konfigurálva.

### <a name="can-i-have-overlapping-address-spaces-for-vnet-to-vnet-configurations"></a>Lehetnek átfedő címterek a virtuális hálózatok közötti konfigurációkhoz?

Nem. Nem lehetnek átfedő IP-címtartományok.

### <a name="can-there-be-overlapping-address-spaces-among-connected-virtual-networks-and-on-premises-local-sites"></a>Lehetnek-e egymással átfedésben lévő címterek a csatlakoztatott virtuális hálózatok és helyszíni helyek között?

Nem. Nem lehetnek átfedő IP-címtartományok.



