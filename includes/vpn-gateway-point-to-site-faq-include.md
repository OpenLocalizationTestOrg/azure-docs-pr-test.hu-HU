### <a name="what-client-operating-systems-can-i-use-with-point-to-site"></a>Milyen ügyfél operációs rendszereket használhatok pont–hely kapcsolatokhoz?

a következő ügyféloldali operációs rendszerek hello támogatottak:

* Windows 7 (32 bites és 64 bites)
* Windows Server 2008 R2 (csak 64 bites)
* Windows 8 (32 bites és 64 bites)
* Windows 8.1 (32 bites és 64 bites)
* Windows Server 2012 (csak 64 bites)
* Windows Server 2012 R2 (csak 64 bites)
* Windows 10

### <a name="can-i-use-any-software-vpn-client-for-point-to-site-that-supports-sstp"></a>Használhatok szoftveres VPN-ügyfelet az SSTP-t támogató pont–hely kapcsolatokhoz?

Nem. A rendszer korlátozott csak toohello Windows operációsrendszer-verziók fent felsorolt támogatja.

### <a name="how-many-vpn-client-endpoints-can-i-have-in-my-point-to-site-configuration"></a>Hány VPN-ügyfélvégpont lehet a pont–hely konfigurációban?

Támogatott too128 VPN ügyfelek toobe képes tooconnect tooa virtuális hálózat létrehozása a hello azonos idő.

### <a name="can-i-use-my-own-internal-pki-root-ca-for-point-to-site-connectivity"></a>Használhatom a saját PKI legfelső szintű hitelesítésszolgáltatómat a pont–hely kapcsolathoz?

Igen. Korábban csak önaláírt főtanúsítványt lehetett használni. Továbbra is 20 főtanúsítvány tölthető fel.

### <a name="can-i-traverse-proxies-and-firewalls-using-point-to-site-capability"></a>Lehetővé teszi-e a pont–hely kapcsolat a proxykon és tűzfalakon való áthaladást?

Igen. Tűzfalon keresztüli SSTP (Secure Socket Tunneling Protocol) tootunnel használjuk. Ez az alagút HTTPS-kapcsolatként jelenik meg.

### <a name="if-i-restart-a-client-computer-configured-for-point-to-site-will-hello-vpn-automatically-reconnect"></a>Ha a pont-pont konfigurált ügyfélszámítógép újraindításához automatikusan újra fognak hello VPN csatlakozni?

Alapértelmezés szerint hello ügyfélszámítógép csak akkor nem állítja hello VPN-kapcsolatot automatikusan.

### <a name="does-point-to-site-support-auto-reconnect-and-ddns-on-hello-vpn-clients"></a>Nem pont-hely támogatja az automatikus-újracsatlakozni, és a DDNS hello VPN-ügyfelek?

Az automatikus újrakapcsolódás és a DDNS jelenleg nem támogatott a pont–hely VPN-kapcsolatokhoz.

### <a name="can-i-have-site-to-site-and-point-to-site-configurations-coexist-for-hello-same-virtual-network"></a>Beállítható, hogy pont-pont, és együttműködjenek pont-pont konfigurációk hello az azonos virtuális hálózaton?

Igen. Mindkét megoldás működhet, ha az átjárójához RouteBased (útvonalapú) VPN-típust használ. Hello klasszikus üzembe helyezési modellel, a dinamikus átjárók kell. Nem támogatási pont-pont statikus útválasztó VPN-átjáró vagy hello használó átjárók végezzük `-VpnType PolicyBased` parancsmag.

### <a name="can-i-configure-a-point-to-site-client-tooconnect-toomultiple-virtual-networks-at-hello-same-time"></a>Beállítható az a pont-pont ügyfél tooconnect toomultiple virtuális hálózatok: hello ugyanannyi időt vesz igénybe?

Igen, ez lehetséges. Azonban hello virtuális hálózatok nem átfedő IP-előtagokat és hello pont-pont címterek nem fedhetik hello virtuális hálózatok között.

### <a name="how-much-throughput-can-i-expect-through-site-to-site-or-point-to-site-connections"></a>Milyen átviteli sebességre számíthatok a helyek közötti és a pont–hely kapcsolatok esetében?

Nehéz toomaintain hello pontos átviteli sebességének hello VPN-alagutat. Az IPsec és az SSTP erős titkosítást használó VPN-protokoll. Átviteli sebesség hello késleltetés és a sávszélesség a helyszíni és az hello Internet között is korlátozza.
