<span data-ttu-id="1eaef-101">hello VNet – VNet – gyakori kérdések tooVPN átjárókapcsolatokhoz vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="1eaef-101">hello VNet-to-VNet FAQ applies tooVPN Gateway connections.</span></span> <span data-ttu-id="1eaef-102">Ha társviszonyt szeretne létesíteni virtuális hálózatok között, lásd: [Társviszony létesítése virtuális hálózatok között](../articles/virtual-network/virtual-network-peering-overview.md)</span><span class="sxs-lookup"><span data-stu-id="1eaef-102">If you are looking for VNet Peering, see [Virtual Network Peering](../articles/virtual-network/virtual-network-peering-overview.md)</span></span>

### <a name="does-azure-charge-for-traffic-between-vnets"></a><span data-ttu-id="1eaef-103">Felszámol az Azure díjat a virtuális hálózatok közötti adatforgalomért?</span><span class="sxs-lookup"><span data-stu-id="1eaef-103">Does Azure charge for traffic between VNets?</span></span>

<span data-ttu-id="1eaef-104">Forgalmi VNet – VNet hello belül azonos régiót szabad mindkét irányban a használata esetén a VPN gateway-kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="1eaef-104">VNet-to-VNet traffic within hello same region is free for both directions when using a VPN gateway connection.</span></span> <span data-ttu-id="1eaef-105">Közötti régió VNet – VNet kimenő forgalom hello kimenő többek VNet adatátviteli hello forrás régiók alapján számítjuk fel.</span><span class="sxs-lookup"><span data-stu-id="1eaef-105">Cross region VNet-to-VNet egress traffic is charged with hello outbound inter-VNet data transfer rates based on hello source regions.</span></span> <span data-ttu-id="1eaef-106">Tekintse meg a toohello [árképzést ismertető oldalra VPN-átjáró](https://azure.microsoft.com/pricing/details/vpn-gateway/) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="1eaef-106">Refer toohello [VPN Gateway pricing page](https://azure.microsoft.com/pricing/details/vpn-gateway/) for details.</span></span> <span data-ttu-id="1eaef-107">Ha a Vnetek helyett Vnetben társviszony-létesítést, VPN-átjáró kapcsolódik, lásd: hello [árképzést ismertető oldalra virtuális hálózati](https://azure.microsoft.com/pricing/details/virtual-network/).</span><span class="sxs-lookup"><span data-stu-id="1eaef-107">If you are connecting your VNets using VNet Peering, rather than VPN Gateway, see hello [Virtual Network pricing page](https://azure.microsoft.com/pricing/details/virtual-network/).</span></span>

### <a name="does-vnet-to-vnet-traffic-travel-across-hello-internet"></a><span data-ttu-id="1eaef-108">Nem VNet – VNet forgalmát haladnak keresztül hello Internet?</span><span class="sxs-lookup"><span data-stu-id="1eaef-108">Does VNet-to-VNet traffic travel across hello Internet?</span></span>

<span data-ttu-id="1eaef-109">Nem.</span><span class="sxs-lookup"><span data-stu-id="1eaef-109">No.</span></span> <span data-ttu-id="1eaef-110">VNet – VNet forgalmát a Microsoft Azure gerincét, nem hello Internet hello áthaladó.</span><span class="sxs-lookup"><span data-stu-id="1eaef-110">VNet-to-VNet traffic travels across hello Microsoft Azure backbone, not hello Internet.</span></span>

### <a name="is-vnet-to-vnet-traffic-secure"></a><span data-ttu-id="1eaef-111">Biztonságos-e a virtuális hálózatok közötti adatforgalom?</span><span class="sxs-lookup"><span data-stu-id="1eaef-111">Is VNet-to-VNet traffic secure?</span></span>

<span data-ttu-id="1eaef-112">Igen, az adatforgalmat IPsec/IKE-titkosítás védi.</span><span class="sxs-lookup"><span data-stu-id="1eaef-112">Yes, it is protected by IPsec/IKE encryption.</span></span>

### <a name="do-i-need-a-vpn-device-tooconnect-vnets-together"></a><span data-ttu-id="1eaef-113">Kell egy VPN-eszköz tooconnect Vnetek együtt?</span><span class="sxs-lookup"><span data-stu-id="1eaef-113">Do I need a VPN device tooconnect VNets together?</span></span>

<span data-ttu-id="1eaef-114">Nem.</span><span class="sxs-lookup"><span data-stu-id="1eaef-114">No.</span></span> <span data-ttu-id="1eaef-115">Az Azure Virtual Networkök összekapcsolása nem igényel VPN-eszközöket, hacsak nem szükséges a létesítmények közötti kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="1eaef-115">Connecting multiple Azure virtual networks together doesn't require a VPN device unless cross-premises connectivity is required.</span></span>

### <a name="do-my-vnets-need-toobe-in-hello-same-region"></a><span data-ttu-id="1eaef-116">A Vnetek szükség van a hello toobe ugyanabban a régióban?</span><span class="sxs-lookup"><span data-stu-id="1eaef-116">Do my VNets need toobe in hello same region?</span></span>

<span data-ttu-id="1eaef-117">Nem.</span><span class="sxs-lookup"><span data-stu-id="1eaef-117">No.</span></span> <span data-ttu-id="1eaef-118">virtuális hálózatok hello lehet hello ugyanazon vagy másik Azure-régiók (hely).</span><span class="sxs-lookup"><span data-stu-id="1eaef-118">hello virtual networks can be in hello same or different Azure regions (locations).</span></span>

### <a name="if-hello-vnets-are-not-in-hello-same-subscription-do-hello-subscriptions-need-toobe-associated-with-hello-same-ad-tenant"></a><span data-ttu-id="1eaef-119">Ha nincsenek a Vnetek hello hello azonos előfizetés, hello előfizetések kell hello ugyanazt az AD-bérlőhöz társított toobe?</span><span class="sxs-lookup"><span data-stu-id="1eaef-119">If hello VNets are not in hello same subscription, do hello subscriptions need toobe associated with hello same AD tenant?</span></span>

<span data-ttu-id="1eaef-120">Nem.</span><span class="sxs-lookup"><span data-stu-id="1eaef-120">No.</span></span>

### <a name="can-i-use-vnet-to-vnet-along-with-multi-site-connections"></a><span data-ttu-id="1eaef-121">A virtuális hálózatok közötti kapcsolatot használhatom többhelyes kapcsolatokhoz?</span><span class="sxs-lookup"><span data-stu-id="1eaef-121">Can I use VNet-to-VNet along with multi-site connections?</span></span>

<span data-ttu-id="1eaef-122">Igen.</span><span class="sxs-lookup"><span data-stu-id="1eaef-122">Yes.</span></span> <span data-ttu-id="1eaef-123">A virtuális hálózati kapcsolat használható többhelyes virtuális VPN-ekkel együtt.</span><span class="sxs-lookup"><span data-stu-id="1eaef-123">Virtual network connectivity can be used simultaneously with multi-site VPNs.</span></span>

### <a name="how-many-on-premises-sites-and-virtual-networks-can-one-virtual-network-connect-to"></a><span data-ttu-id="1eaef-124">Hány helyszíni helyhez és virtuális hálózathoz kapcsolódhat egyetlen virtuális hálózat?</span><span class="sxs-lookup"><span data-stu-id="1eaef-124">How many on-premises sites and virtual networks can one virtual network connect to?</span></span>

<span data-ttu-id="1eaef-125">Lásd [Az átjáróra vonatkozó követelmények](../articles/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#requirements) táblázatot.</span><span class="sxs-lookup"><span data-stu-id="1eaef-125">See [Gateway requirements](../articles/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#requirements) table.</span></span>

### <a name="can-i-use-vnet-to-vnet-tooconnect-vms-or-cloud-services-outside-of-a-vnet"></a><span data-ttu-id="1eaef-126">Használja a VNet – VNet tooconnect virtuális gépeken vagy felhőszolgáltatásokon egy virtuális hálózaton kívül?</span><span class="sxs-lookup"><span data-stu-id="1eaef-126">Can I use VNet-to-VNet tooconnect VMs or cloud services outside of a VNet?</span></span>

<span data-ttu-id="1eaef-127">Nem.</span><span class="sxs-lookup"><span data-stu-id="1eaef-127">No.</span></span> <span data-ttu-id="1eaef-128">A virtuális hálózatok közötti kapcsolat támogatja a virtuális hálózatok csatlakoztatását,</span><span class="sxs-lookup"><span data-stu-id="1eaef-128">VNet-to-VNet supports connecting virtual networks.</span></span> <span data-ttu-id="1eaef-129">Nem támogatja a nem virtuális hálózatban lévő virtuális gépek és felhőszolgáltatások csatlakoztatását.</span><span class="sxs-lookup"><span data-stu-id="1eaef-129">It does not support connecting virtual machines or cloud services that are not in a virtual network.</span></span>

### <a name="can-a-cloud-service-or-a-load-balancing-endpoint-span-vnets"></a><span data-ttu-id="1eaef-130">A felhőszolgáltatások és a terheléselosztási végpontok átívelhetnek több virtuális hálózaton?</span><span class="sxs-lookup"><span data-stu-id="1eaef-130">Can a cloud service or a load balancing endpoint span VNets?</span></span>

<span data-ttu-id="1eaef-131">Nem.</span><span class="sxs-lookup"><span data-stu-id="1eaef-131">No.</span></span> <span data-ttu-id="1eaef-132">A felhőszolgáltatás és a terheléselosztási végpont nem ívelhet át több virtuális hálózaton, akkor sem, ha ezek össze vannak kapcsolva.</span><span class="sxs-lookup"><span data-stu-id="1eaef-132">A cloud service or a load balancing endpoint can't span across virtual networks, even if they are connected together.</span></span>

### <a name="can-i-used-a-policybased-vpn-type-for-vnet-to-vnet-or-multi-site-connections"></a><span data-ttu-id="1eaef-133">Használhatok házirendalapú VPN-típust a virtuális hálózatok közötti vagy többhelyes kapcsolatokhoz?</span><span class="sxs-lookup"><span data-stu-id="1eaef-133">Can I used a PolicyBased VPN type for VNet-to-VNet or Multi-Site connections?</span></span>

<span data-ttu-id="1eaef-134">Nem.</span><span class="sxs-lookup"><span data-stu-id="1eaef-134">No.</span></span> <span data-ttu-id="1eaef-135">A virtuális hálózatok közötti és többhelyes kapcsolatokhoz útvonalalapú (korábbi nevén dinamikus útválasztású) VPN-típussal rendelkező Azure VPN Gateway átjárók szükségesek.</span><span class="sxs-lookup"><span data-stu-id="1eaef-135">VNet-to-VNet and Multi-Site connections require Azure VPN gateways with RouteBased (previously called Dynamic Routing) VPN types.</span></span>

### <a name="can-i-connect-a-vnet-with-a-routebased-vpn-type-tooanother-vnet-with-a-policybased-vpn-type"></a><span data-ttu-id="1eaef-136">I csatlakozhatnak egy virtuális hálózat egy RouteBased VPN-típus tooanother VNet PolicyBased VPN típusú?</span><span class="sxs-lookup"><span data-stu-id="1eaef-136">Can I connect a VNet with a RouteBased VPN Type tooanother VNet with a PolicyBased VPN type?</span></span>

<span data-ttu-id="1eaef-137">Nem, mindkét virtuális hálózatnak útvonalalapú (korábban dinamikus útválasztású) VPN-t KELL használnia.</span><span class="sxs-lookup"><span data-stu-id="1eaef-137">No, both virtual networks MUST be using route-based (previously called Dynamic Routing) VPNs.</span></span>

### <a name="do-vpn-tunnels-share-bandwidth"></a><span data-ttu-id="1eaef-138">A VPN-alagutak osztoznak a sávszélességen?</span><span class="sxs-lookup"><span data-stu-id="1eaef-138">Do VPN tunnels share bandwidth?</span></span>

<span data-ttu-id="1eaef-139">Igen.</span><span class="sxs-lookup"><span data-stu-id="1eaef-139">Yes.</span></span> <span data-ttu-id="1eaef-140">Hello virtuális hálózat összes VPN-alagutat megosztása hello Azure VPN gateway hello rendelkezésre álló sávszélességet, és hello azonos VPN gateway hasznos üzemidő SLA-t az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="1eaef-140">All VPN tunnels of hello virtual network share hello available bandwidth on hello Azure VPN gateway and hello same VPN gateway uptime SLA in Azure.</span></span>

### <a name="are-redundant-tunnels-supported"></a><span data-ttu-id="1eaef-141">Támogatottak a redundáns alagutak?</span><span class="sxs-lookup"><span data-stu-id="1eaef-141">Are redundant tunnels supported?</span></span>

<span data-ttu-id="1eaef-142">A virtuális hálózatok párjai közötti redundáns alagutak nem támogatottak, amikor a virtuális hálózati átjáró aktív-aktívként van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="1eaef-142">Redundant tunnels between a pair of virtual networks are supported when one virtual network gateway is configured as active-active.</span></span>

### <a name="can-i-have-overlapping-address-spaces-for-vnet-to-vnet-configurations"></a><span data-ttu-id="1eaef-143">Lehetnek átfedő címterek a virtuális hálózatok közötti konfigurációkhoz?</span><span class="sxs-lookup"><span data-stu-id="1eaef-143">Can I have overlapping address spaces for VNet-to-VNet configurations?</span></span>

<span data-ttu-id="1eaef-144">Nem.</span><span class="sxs-lookup"><span data-stu-id="1eaef-144">No.</span></span> <span data-ttu-id="1eaef-145">Nem lehetnek átfedő IP-címtartományok.</span><span class="sxs-lookup"><span data-stu-id="1eaef-145">You can't have overlapping IP address ranges.</span></span>

### <a name="can-there-be-overlapping-address-spaces-among-connected-virtual-networks-and-on-premises-local-sites"></a><span data-ttu-id="1eaef-146">Lehetnek-e egymással átfedésben lévő címterek a csatlakoztatott virtuális hálózatok és helyszíni helyek között?</span><span class="sxs-lookup"><span data-stu-id="1eaef-146">Can there be overlapping address spaces among connected virtual networks and on-premises local sites?</span></span>

<span data-ttu-id="1eaef-147">Nem.</span><span class="sxs-lookup"><span data-stu-id="1eaef-147">No.</span></span> <span data-ttu-id="1eaef-148">Nem lehetnek átfedő IP-címtartományok.</span><span class="sxs-lookup"><span data-stu-id="1eaef-148">You can't have overlapping IP address ranges.</span></span>



