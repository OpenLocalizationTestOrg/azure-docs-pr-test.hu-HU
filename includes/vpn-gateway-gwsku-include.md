<span data-ttu-id="f16ae-101">A virtuális hálózati átjáró létrehozásához meg kell toospecify hello átjáró, amelyet az toouse Termékváltozat.</span><span class="sxs-lookup"><span data-stu-id="f16ae-101">When you create a virtual network gateway, you need toospecify hello gateway SKU that you want toouse.</span></span> <span data-ttu-id="f16ae-102">Válassza ki, amelyek megfelelnek a munkaterhelések, teljesítmények, szolgáltatások és SLA-k hello típusú alapján hello SKU.</span><span class="sxs-lookup"><span data-stu-id="f16ae-102">Select hello SKUs that satisfy your requirements based on hello types of workloads, throughputs, features, and SLAs.</span></span>

[!INCLUDE [classic SKU](./vpn-gateway-classic-sku-support-include.md)]

[!INCLUDE [Aggregated throughput by SKU](./vpn-gateway-table-gwtype-aggtput-include.md)]

###  <span data-ttu-id="f16ae-103"><a name="workloads"></a>Termelés *vs.* Dev-Test számítási feladatok</span><span class="sxs-lookup"><span data-stu-id="f16ae-103"><a name="workloads"></a>Production *vs.* Dev-Test Workloads</span></span>

<span data-ttu-id="f16ae-104">Lejáró SLA-k és szolgáltatáskészletek toohello közötti különbségeket, azt javasoljuk termékváltozatok követő üzemi hello *és* fejlesztői-teszt:</span><span class="sxs-lookup"><span data-stu-id="f16ae-104">Due toohello differences in SLAs and feature sets, we recommend hello following SKUs for production *vs.* dev-test:</span></span>

| <span data-ttu-id="f16ae-105">**Számítási feladat**</span><span class="sxs-lookup"><span data-stu-id="f16ae-105">**Workload**</span></span>                       | <span data-ttu-id="f16ae-106">**Termékváltozatok**</span><span class="sxs-lookup"><span data-stu-id="f16ae-106">**SKUs**</span></span>               |
| ---                                | ---                    |
| <span data-ttu-id="f16ae-107">**Termelés, kritikus fontosságú számítási feladatok**</span><span class="sxs-lookup"><span data-stu-id="f16ae-107">**Production, critical workloads**</span></span> | <span data-ttu-id="f16ae-108">VpnGw1, VpnGw2, VpnGw3</span><span class="sxs-lookup"><span data-stu-id="f16ae-108">VpnGw1, VpnGw2, VpnGw3</span></span> |
| <span data-ttu-id="f16ae-109">**Dev-test vagy a koncepció igazolása**</span><span class="sxs-lookup"><span data-stu-id="f16ae-109">**Dev-test or proof of concept**</span></span>   | <span data-ttu-id="f16ae-110">Basic</span><span class="sxs-lookup"><span data-stu-id="f16ae-110">Basic</span></span>                  |
|                                    |                        |

<span data-ttu-id="f16ae-111">Régi hello használata SKU, hello éles SKU javaslatok rendszer Standard és a HighPerformance SKU.</span><span class="sxs-lookup"><span data-stu-id="f16ae-111">If you are using hello old SKUs, hello production SKU recommendations are Standard and HighPerformance SKUs.</span></span> <span data-ttu-id="f16ae-112">Információk a hello régi termékváltozatok: [Gateway SKU-n (örökölt SKU)](../articles/vpn-gateway/vpn-gateway-about-skus-legacy.md).</span><span class="sxs-lookup"><span data-stu-id="f16ae-112">For information on hello old SKUs, see [Gateway SKUs (legacy SKUs)](../articles/vpn-gateway/vpn-gateway-about-skus-legacy.md).</span></span>

###  <span data-ttu-id="f16ae-113"><a name="feature"></a>Átjáró termékváltozatainak szolgáltatáskészletei</span><span class="sxs-lookup"><span data-stu-id="f16ae-113"><a name="feature"></a>Gateway SKU feature sets</span></span>

<span data-ttu-id="f16ae-114">hello új átjáró termékváltozatok hello átjárók kínált hello szolgáltatáskészletek egyszerűsítésére:</span><span class="sxs-lookup"><span data-stu-id="f16ae-114">hello new gateway SKUs streamline hello feature sets offered on hello gateways:</span></span>

| <span data-ttu-id="f16ae-115">**Termékváltozat**</span><span class="sxs-lookup"><span data-stu-id="f16ae-115">**SKU**</span></span>| <span data-ttu-id="f16ae-116">**Szolgáltatások**</span><span class="sxs-lookup"><span data-stu-id="f16ae-116">**Features**</span></span>|
| ---    | ---         |
|<span data-ttu-id="f16ae-117">**Basic**</span><span class="sxs-lookup"><span data-stu-id="f16ae-117">**Basic**</span></span>   | <span data-ttu-id="f16ae-118">**Útvonalalapú VPN**: 10 alagút P2S-sel</span><span class="sxs-lookup"><span data-stu-id="f16ae-118">**Route-based VPN**: 10 tunnels with P2S</span></span><br><br><span data-ttu-id="f16ae-119">**Házirend-alapú VPN** (IKEv1): 1 alagút, P2S nélkül</span><span class="sxs-lookup"><span data-stu-id="f16ae-119">**Policy-based VPN**: (IKEv1): 1 tunnel; no P2S</span></span>|
| <span data-ttu-id="f16ae-120">**VpnGw1, VpnGw2 és VpnGw3**</span><span class="sxs-lookup"><span data-stu-id="f16ae-120">**VpnGw1, VpnGw2, and VpnGw3**</span></span> | <span data-ttu-id="f16ae-121">**Útvonalalapú VPN**: too30 alagutak (*), P2S, a BGP-be aktív-aktív, az egyéni IPsec/IKE házirend, ExpressRoute-és VPN létezzenek</span><span class="sxs-lookup"><span data-stu-id="f16ae-121">**Route-based VPN**: up too30 tunnels (*), P2S, BGP, active-active, custom IPsec/IKE policy, ExpressRoute/VPN co-existence</span></span> |
|        |             |

<span data-ttu-id="f16ae-122">(*) "PolicyBasedTrafficSelectors" tooconnect konfigurálhatja egy útvonalalapú VPN-átjáró (VpnGw1, VpnGw2, VpnGw3) toomultiple helyszíni tűzfal csoportházirend-alapú eszközök.</span><span class="sxs-lookup"><span data-stu-id="f16ae-122">(*) You can configure "PolicyBasedTrafficSelectors" tooconnect a route-based VPN gateway (VpnGw1, VpnGw2, VpnGw3) toomultiple on-premises policy-based firewall devices.</span></span> <span data-ttu-id="f16ae-123">Tekintse meg a túl[csatlakozás VPN-átjárók toomultiple a helyi csoportházirend-alapú VPN-eszközök PowerShell-lel](../articles/vpn-gateway/vpn-gateway-connect-multiple-policybased-rm-ps.md) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="f16ae-123">Refer too[Connect VPN gateways toomultiple on-premises policy-based VPN devices using PowerShell](../articles/vpn-gateway/vpn-gateway-connect-multiple-policybased-rm-ps.md) for details.</span></span>

###  <span data-ttu-id="f16ae-124"><a name="resize"></a>Az átjárók termékváltozatainak átméretezése</span><span class="sxs-lookup"><span data-stu-id="f16ae-124"><a name="resize"></a>Resizing gateway SKUs</span></span>

1. <span data-ttu-id="f16ae-125">Az átméretezés során a VpnGw1, a VpnGw2 és a VpnGw3 termékváltozatok közül választhat.</span><span class="sxs-lookup"><span data-stu-id="f16ae-125">You can resize between VpnGw1, VpnGw2, and VpnGw3 SKUs.</span></span>
2. <span data-ttu-id="f16ae-126">Az hello régi gateway SKU használatakor átméretezheti, Basic, Standard és a HighPerformance termékváltozatok között.</span><span class="sxs-lookup"><span data-stu-id="f16ae-126">When working with hello old gateway SKUs, you can resize between Basic, Standard, and HighPerformance SKUs.</span></span>
2. <span data-ttu-id="f16ae-127">Ön **nem** méretezze át a Basic vagy Standard/HighPerformance termékváltozatok toohello új VpnGw1/VpnGw2/VpnGw3 SKU.</span><span class="sxs-lookup"><span data-stu-id="f16ae-127">You **cannot** resize from Basic/Standard/HighPerformance SKUs toohello new VpnGw1/VpnGw2/VpnGw3 SKUs.</span></span> <span data-ttu-id="f16ae-128">Meg kell, ehelyett [áttelepítése](#migrate) toohello új SKU.</span><span class="sxs-lookup"><span data-stu-id="f16ae-128">You must, instead, [migrate](#migrate) toohello new SKUs.</span></span>

###  <span data-ttu-id="f16ae-129"><a name="migrate"></a>Régi termékváltozatok toohello áttelepítése új termékváltozatok</span><span class="sxs-lookup"><span data-stu-id="f16ae-129"><a name="migrate"></a>Migrating from old SKUs toohello new SKUs</span></span>

[!INCLUDE [Migrate SKU](./vpn-gateway-migrate-legacy-sku-include.md)]
