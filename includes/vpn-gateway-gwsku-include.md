A virtuális hálózati átjáró létrehozásához meg kell toospecify hello átjáró, amelyet az toouse Termékváltozat. Válassza ki, amelyek megfelelnek a munkaterhelések, teljesítmények, szolgáltatások és SLA-k hello típusú alapján hello SKU.

[!INCLUDE [classic SKU](./vpn-gateway-classic-sku-support-include.md)]

[!INCLUDE [Aggregated throughput by SKU](./vpn-gateway-table-gwtype-aggtput-include.md)]

###  <a name="workloads"></a>Termelés *vs.* Dev-Test számítási feladatok

Lejáró SLA-k és szolgáltatáskészletek toohello közötti különbségeket, azt javasoljuk termékváltozatok követő üzemi hello *és* fejlesztői-teszt:

| **Számítási feladat**                       | **Termékváltozatok**               |
| ---                                | ---                    |
| **Termelés, kritikus fontosságú számítási feladatok** | VpnGw1, VpnGw2, VpnGw3 |
| **Dev-test vagy a koncepció igazolása**   | Basic                  |
|                                    |                        |

Régi hello használata SKU, hello éles SKU javaslatok rendszer Standard és a HighPerformance SKU. Információk a hello régi termékváltozatok: [Gateway SKU-n (örökölt SKU)](../articles/vpn-gateway/vpn-gateway-about-skus-legacy.md).

###  <a name="feature"></a>Átjáró termékváltozatainak szolgáltatáskészletei

hello új átjáró termékváltozatok hello átjárók kínált hello szolgáltatáskészletek egyszerűsítésére:

| **Termékváltozat**| **Szolgáltatások**|
| ---    | ---         |
|**Basic**   | **Útvonalalapú VPN**: 10 alagút P2S-sel<br><br>**Házirend-alapú VPN** (IKEv1): 1 alagút, P2S nélkül|
| **VpnGw1, VpnGw2 és VpnGw3** | **Útvonalalapú VPN**: too30 alagutak (*), P2S, a BGP-be aktív-aktív, az egyéni IPsec/IKE házirend, ExpressRoute-és VPN létezzenek |
|        |             |

(*) "PolicyBasedTrafficSelectors" tooconnect konfigurálhatja egy útvonalalapú VPN-átjáró (VpnGw1, VpnGw2, VpnGw3) toomultiple helyszíni tűzfal csoportházirend-alapú eszközök. Tekintse meg a túl[csatlakozás VPN-átjárók toomultiple a helyi csoportházirend-alapú VPN-eszközök PowerShell-lel](../articles/vpn-gateway/vpn-gateway-connect-multiple-policybased-rm-ps.md) részleteiről.

###  <a name="resize"></a>Az átjárók termékváltozatainak átméretezése

1. Az átméretezés során a VpnGw1, a VpnGw2 és a VpnGw3 termékváltozatok közül választhat.
2. Az hello régi gateway SKU használatakor átméretezheti, Basic, Standard és a HighPerformance termékváltozatok között.
2. Ön **nem** méretezze át a Basic vagy Standard/HighPerformance termékváltozatok toohello új VpnGw1/VpnGw2/VpnGw3 SKU. Meg kell, ehelyett [áttelepítése](#migrate) toohello új SKU.

###  <a name="migrate"></a>Régi termékváltozatok toohello áttelepítése új termékváltozatok

[!INCLUDE [Migrate SKU](./vpn-gateway-migrate-legacy-sku-include.md)]
