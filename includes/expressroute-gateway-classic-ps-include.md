<span data-ttu-id="37d68-101">Létre kell hoznia egy VNet és egy átjáró-alhálózatot, először a következő feladatok hello használata előtt.</span><span class="sxs-lookup"><span data-stu-id="37d68-101">You must create a VNet and a gateway subnet first, before working on hello following tasks.</span></span> <span data-ttu-id="37d68-102">Hello cikke [hello klasszikus portál használatával virtuális hálózat konfigurálása](../articles/expressroute/expressroute-howto-vnet-portal-classic.md) további információt.</span><span class="sxs-lookup"><span data-stu-id="37d68-102">See hello article [Configure a Virtual Network using hello classic portal](../articles/expressroute/expressroute-howto-vnet-portal-classic.md) for more information.</span></span>   

## <a name="add-a-gateway"></a><span data-ttu-id="37d68-103">Átjáró hozzáadása</span><span class="sxs-lookup"><span data-stu-id="37d68-103">Add a gateway</span></span>
<span data-ttu-id="37d68-104">Hello paranccsal toocreate átjáró alatt.</span><span class="sxs-lookup"><span data-stu-id="37d68-104">Use hello command below toocreate a gateway.</span></span> <span data-ttu-id="37d68-105">Lehet, hogy toosubstitute bármely értékeket a saját.</span><span class="sxs-lookup"><span data-stu-id="37d68-105">Be sure toosubstitute any values for your own.</span></span>

    New-AzureVirtualNetworkGateway -VNetName "MyAzureVNET" -GatewayName "ERGateway" -GatewayType Dedicated -GatewaySKU  Standard

## <a name="verify-hello-gateway-was-created"></a><span data-ttu-id="37d68-106">Hello átjáró létrejött-e</span><span class="sxs-lookup"><span data-stu-id="37d68-106">Verify hello gateway was created</span></span>
<span data-ttu-id="37d68-107">Hello parancsot használja, amelyek átjáró hello tooverify létrejött.</span><span class="sxs-lookup"><span data-stu-id="37d68-107">Use hello command below tooverify that hello gateway has been created.</span></span> <span data-ttu-id="37d68-108">A parancs segítségével lekérdezhető hello átjáró azonosítója, amely a többi művelet kell is.</span><span class="sxs-lookup"><span data-stu-id="37d68-108">This command also retrieves hello gateway ID, which you need for other operations.</span></span>

    Get-AzureVirtualNetworkGateway

## <a name="resize-a-gateway"></a><span data-ttu-id="37d68-109">Átjáró méretezése</span><span class="sxs-lookup"><span data-stu-id="37d68-109">Resize a gateway</span></span>
<span data-ttu-id="37d68-110">A több [Gateway SKU-n](../articles/expressroute/expressroute-about-virtual-network-gateways.md).</span><span class="sxs-lookup"><span data-stu-id="37d68-110">There are a number of [Gateway SKUs](../articles/expressroute/expressroute-about-virtual-network-gateways.md).</span></span> <span data-ttu-id="37d68-111">Hello parancs toochange hello átjáró-Termékváltozat követően bármikor használhatja.</span><span class="sxs-lookup"><span data-stu-id="37d68-111">You can use hello following command toochange hello Gateway SKU at any time.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="37d68-112">Ez a parancs UltraPerformance átjáró nem működik.</span><span class="sxs-lookup"><span data-stu-id="37d68-112">This command doesn't work for UltraPerformance gateway.</span></span> <span data-ttu-id="37d68-113">toochange a tooan UltraPerformance átjárók, először távolítsa el a meglévő ExpressRoute-átjáró hello, és ezután hozzon létre újat UltraPerformance.</span><span class="sxs-lookup"><span data-stu-id="37d68-113">toochange your gateway tooan UltraPerformance gateway, first remove hello existing ExpressRoute gateway, and then create a new UltraPerformance gateway.</span></span> <span data-ttu-id="37d68-114">toodowngrade az átjáró UltraPerformance-átjáróról, először el kell távolítani hello UltraPerformance átjáró, és hozzon létre egy új átjárót.</span><span class="sxs-lookup"><span data-stu-id="37d68-114">toodowngrade your gateway from an UltraPerformance gateway, first remove hello UltraPerformance gateway, and then create a new gateway.</span></span> 
> 
> 

    Resize-AzureVirtualNetworkGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance

## <a name="remove-a-gateway"></a><span data-ttu-id="37d68-115">Átjáró eltávolítása</span><span class="sxs-lookup"><span data-stu-id="37d68-115">Remove a gateway</span></span>
<span data-ttu-id="37d68-116">Alább tooremove átjáró hello paranccsal</span><span class="sxs-lookup"><span data-stu-id="37d68-116">Use hello command below tooremove a gateway</span></span>

    Remove-AzureVirtualNetworkGateway -GatewayId <Gateway ID>