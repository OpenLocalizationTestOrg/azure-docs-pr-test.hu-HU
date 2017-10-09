### <span data-ttu-id="fca8d-101"><a name="gwipnoconnection"></a>toomodify hello helyi hálózati átjáró "GatewayIpAddress" - átjáró kapcsolat</span><span class="sxs-lookup"><span data-stu-id="fca8d-101"><a name="gwipnoconnection"></a> toomodify hello local network gateway 'GatewayIpAddress' - no gateway connection</span></span>

<span data-ttu-id="fca8d-102">Nyilvános IP-címének hello VPN-eszköz, amelyet az tooconnect toohas megváltozásakor toomodify hello helyi hálózati átjáró tooreflect módosító szüksége.</span><span class="sxs-lookup"><span data-stu-id="fca8d-102">If hello VPN device that you want tooconnect toohas changed its public IP address, you need toomodify hello local network gateway tooreflect that change.</span></span> <span data-ttu-id="fca8d-103">Hello példa toomodify a helyi hálózati átjáró, amely nem rendelkezik egy gateway-kapcsolatot használjon.</span><span class="sxs-lookup"><span data-stu-id="fca8d-103">Use hello example toomodify a local network gateway that does not have a gateway connection.</span></span>

<span data-ttu-id="fca8d-104">Ha módosítja ezt az értéket, módosíthatja is hello címelőtagok: hello ugyanannyi időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="fca8d-104">When modifying this value, you can also modify hello address prefixes at hello same time.</span></span> <span data-ttu-id="fca8d-105">Lehet, hogy toouse hello meglévő neve a helyi hálózati átjáró toooverwrite hello jelenlegi beállításai.</span><span class="sxs-lookup"><span data-stu-id="fca8d-105">Be sure toouse hello existing name of your local network gateway in order toooverwrite hello current settings.</span></span> <span data-ttu-id="fca8d-106">Ha más nevet használjon, akkor hozzon létre egy új helyi hálózati átjáró, helyett felülírja a meglévő hello.</span><span class="sxs-lookup"><span data-stu-id="fca8d-106">If you use a different name, you create a new local network gateway, instead of overwriting hello existing one.</span></span>

```powershell
New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
-Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
-GatewayIpAddress "5.4.3.2" -ResourceGroupName MyRGName
```

### <span data-ttu-id="fca8d-107"><a name="gwipwithconnection"></a>toomodify hello helyi hálózati átjáró "GatewayIpAddress" - meglévő gateway-kapcsolatot</span><span class="sxs-lookup"><span data-stu-id="fca8d-107"><a name="gwipwithconnection"></a>toomodify hello local network gateway 'GatewayIpAddress' - existing gateway connection</span></span>

<span data-ttu-id="fca8d-108">Nyilvános IP-címének hello VPN-eszköz, amelyet az tooconnect toohas megváltozásakor toomodify hello helyi hálózati átjáró tooreflect módosító szüksége.</span><span class="sxs-lookup"><span data-stu-id="fca8d-108">If hello VPN device that you want tooconnect toohas changed its public IP address, you need toomodify hello local network gateway tooreflect that change.</span></span> <span data-ttu-id="fca8d-109">Átjáró már létezik egy kapcsolat, ha először tooremove hello kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="fca8d-109">If a gateway connection already exists, you first need tooremove hello connection.</span></span> <span data-ttu-id="fca8d-110">Hello kapcsolat eltávolítása után módosítja a hello átjáró IP-címet, és hozza létre újra egy új kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="fca8d-110">After hello connection is removed, you can modify hello gateway IP address and recreate a new connection.</span></span> <span data-ttu-id="fca8d-111">Módosíthatja is hello címelőtagok: hello ugyanannyi időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="fca8d-111">You can also modify hello address prefixes at hello same time.</span></span> <span data-ttu-id="fca8d-112">Ez némi állásidőt jelent a VPN-kapcsolata számára.</span><span class="sxs-lookup"><span data-stu-id="fca8d-112">This results in some downtime for your VPN connection.</span></span> <span data-ttu-id="fca8d-113">Ha módosítja a hello átjáró IP-címe, nincs szükség a toodelete hello VPN-átjáró.</span><span class="sxs-lookup"><span data-stu-id="fca8d-113">When modifying hello gateway IP address, you don't need toodelete hello VPN gateway.</span></span> <span data-ttu-id="fca8d-114">Csak akkor kell tooremove hello kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="fca8d-114">You only need tooremove hello connection.</span></span>
 

1. <span data-ttu-id="fca8d-115">Távolítsa el a hello kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="fca8d-115">Remove hello connection.</span></span> <span data-ttu-id="fca8d-116">Hello neve a kapcsolat található hello "Get-AzureRmVirtualNetworkGatewayConnection" parancsmag használatával.</span><span class="sxs-lookup"><span data-stu-id="fca8d-116">You can find hello name of your connection by using hello 'Get-AzureRmVirtualNetworkGatewayConnection' cmdlet.</span></span>

  ```powershell
  Remove-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName `
  -ResourceGroupName MyRGName
  ```
2. <span data-ttu-id="fca8d-117">Módosítsa a hello "GatewayIpAddress" értékét.</span><span class="sxs-lookup"><span data-stu-id="fca8d-117">Modify hello 'GatewayIpAddress' value.</span></span> <span data-ttu-id="fca8d-118">Módosíthatja is hello címelőtagok: hello ugyanannyi időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="fca8d-118">You can also modify hello address prefixes at hello same time.</span></span> <span data-ttu-id="fca8d-119">Lehet, hogy toouse hello meglévő neve a helyi hálózati átjáró toooverwrite hello aktuális beállításokat.</span><span class="sxs-lookup"><span data-stu-id="fca8d-119">Be sure toouse hello existing name of your local network gateway toooverwrite hello current settings.</span></span> <span data-ttu-id="fca8d-120">Ha ezt elmulasztja, akkor hozzon létre egy új helyi hálózati átjáró, hello felülírása helyett a meglévőt.</span><span class="sxs-lookup"><span data-stu-id="fca8d-120">If you don't, you create a new local network gateway, instead of overwriting hello existing one.</span></span>

  ```powershell
  New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
  -Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
  -GatewayIpAddress "104.40.81.124" -ResourceGroupName MyRGName
  ```
3. <span data-ttu-id="fca8d-121">Hello kapcsolat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="fca8d-121">Create hello connection.</span></span> <span data-ttu-id="fca8d-122">Ebben a példában egy IPsec kapcsolattípust konfigurálunk.</span><span class="sxs-lookup"><span data-stu-id="fca8d-122">In this example, we configure an IPsec connection type.</span></span> <span data-ttu-id="fca8d-123">Ismételt a kapcsolatot, használja a megadott kapcsolattípus hello a konfigurációhoz.</span><span class="sxs-lookup"><span data-stu-id="fca8d-123">When you recreate your connection, use hello connection type that is specified for your configuration.</span></span> <span data-ttu-id="fca8d-124">További kapcsolattípusokat, lásd: hello [PowerShell-parancsmag](https://msdn.microsoft.com/library/mt603611.aspx) lap.</span><span class="sxs-lookup"><span data-stu-id="fca8d-124">For additional connection types, see hello [PowerShell cmdlet](https://msdn.microsoft.com/library/mt603611.aspx) page.</span></span>  <span data-ttu-id="fca8d-125">tooobtain hello pedig neve hello "Get-AzureRmVirtualNetworkGateway" parancsmag is futtathatja.</span><span class="sxs-lookup"><span data-stu-id="fca8d-125">tooobtain hello VirtualNetworkGateway name, you can run hello 'Get-AzureRmVirtualNetworkGateway' cmdlet.</span></span>
   
    <span data-ttu-id="fca8d-126">Hello változók megadása.</span><span class="sxs-lookup"><span data-stu-id="fca8d-126">Set hello variables.</span></span>

  ```powershell
  $local = Get-AzureRMLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
  $vnetgw = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName MyRGName
  ```
   
    <span data-ttu-id="fca8d-127">Hello kapcsolat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="fca8d-127">Create hello connection.</span></span>

  ```powershell 
  New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName -ResourceGroupName MyRGName `
  -Location "West US" `
  -VirtualNetworkGateway1 $vnetgw `
  -LocalNetworkGateway2 $local `
  -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
  ```