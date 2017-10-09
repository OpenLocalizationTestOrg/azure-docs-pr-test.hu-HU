hello következő táblázatban hello átjárótípusok és hello becsült összesített átviteli Gateway SKU. Ez a táblázat toohello Resource Manager és klasszikus üzembe helyezési modellre vonatkozik. 

A díjszabás a különböző átjáró-termékváltozatok esetében nem változik. További információkért lásd: [VPN Gateway-díjszabás](https://azure.microsoft.com/pricing/details/vpn-gateway).

Vegye figyelembe, hogy hello UltraPerformance gateway SKU nem szerepel ebben a táblában. Hello UltraPerformance SKU kapcsolatos információkért lásd: hello [ExpressRoute](../articles/expressroute/expressroute-about-virtual-network-gateways.md) dokumentációját.

|  | **VPN Gateway teljesítménye (1)** | **VPN Gateway IPsec-alagútjainak maximális száma (2)** | **ExpressRoute-átjáró teljesítménye** | **VPN Gateway és ExpressRoute párhuzamos használata** |
| --- | --- | --- | --- | --- |
| **Alapszintű termékváltozat (3)(5)(6)** |100 Mbps |10 |500 Mbps (6) |Nem |
| **Standard termékváltozat (4)(5)** |100 Mbps |10 |1000 Mbps |Igen |
| **Nagy teljesítményű termékváltozat (4)** |200 Mbps |30 |2000 Mbps |Igen |


(1) hello VPN átviteli egy durva hello mérések közötti hello a Vnetek alapján becsült érték azonos Azure-régiót. Nincs a létesítmények közötti kapcsolatok garantált átviteli hello interneten keresztül. Hello lehetséges maximális átviteli mérési.

(2) alagutak száma hello tooRouteBased VPN hivatkozik. A házirendalapú VPN csak egyetlen, helyek közötti VPN-alagutat támogat.

(3) BGP hello alapszintű Termékváltozat nem támogatott.

(4) Ez a termékváltozat nem támogatja a házirendalapú VPN-eket. Alapszintű Termékváltozat hello csak azok támogatottak.

(5) Ez a termékváltozat nem támogatja az aktív/aktív módú S2S VPN Gateway-kapcsolatokat. Aktív-aktív HighPerformance SKU hello támogatott.

(6) elavult alapszintű Termékváltozat ExpressRoute való használatra.
