### <span data-ttu-id="a65a6-101"><a name="noconnection"></a>toomodify helyi hálózati átjáró IP-cím előtagokat - átjáró kapcsolat</span><span class="sxs-lookup"><span data-stu-id="a65a6-101"><a name="noconnection"></a>toomodify local network gateway IP address prefixes - no gateway connection</span></span>

<span data-ttu-id="a65a6-102">tooadd további címelőtagokat:</span><span class="sxs-lookup"><span data-stu-id="a65a6-102">tooadd additional address prefixes:</span></span>

```powershell
$local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
-AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')
```

<span data-ttu-id="a65a6-103">tooremove címelőtagok:</span><span class="sxs-lookup"><span data-stu-id="a65a6-103">tooremove address prefixes:</span></span><br>
<span data-ttu-id="a65a6-104">Hagyja ki hello előtagokat, amely már nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="a65a6-104">Leave out hello prefixes that you no longer need.</span></span> <span data-ttu-id="a65a6-105">Ebben a példában a Microsoft már nem 20.0.0.0/24 előtagra (a hello előző példában), ezért frissítjük hello helyi hálózati átjáró, kivéve a ezt az előtagot.</span><span class="sxs-lookup"><span data-stu-id="a65a6-105">In this example, we no longer need prefix 20.0.0.0/24 (from hello previous example), so we update hello local network gateway, excluding that prefix.</span></span>

```powershell
$local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
-AddressPrefix @('10.0.0.0/24','30.0.0.0/24')
```

### <span data-ttu-id="a65a6-106"><a name="withconnection"></a>toomodify helyi hálózati átjáró IP-cím előtagokat - meglévő gateway-kapcsolatot</span><span class="sxs-lookup"><span data-stu-id="a65a6-106"><a name="withconnection"></a>toomodify local network gateway IP address prefixes - existing gateway connection</span></span>

<span data-ttu-id="a65a6-107">Ha átjáró kapcsolat és szeretné, hogy tooadd, vagy távolítsa el a hello a helyi hálózati átjáróban tárolt IP-címelőtagokat, akkor toodo hello lépések sorrendben.</span><span class="sxs-lookup"><span data-stu-id="a65a6-107">If you have a gateway connection and want tooadd or remove hello IP address prefixes contained in your local network gateway, you need toodo hello following steps, in order.</span></span> <span data-ttu-id="a65a6-108">Ez némi állásidőt jelent a VPN-kapcsolata számára.</span><span class="sxs-lookup"><span data-stu-id="a65a6-108">This results in some downtime for your VPN connection.</span></span> <span data-ttu-id="a65a6-109">Ha módosítja az IP-cím előtagokat, nincs szükség a toodelete hello VPN-átjáró.</span><span class="sxs-lookup"><span data-stu-id="a65a6-109">When modifying IP address prefixes, you don't need toodelete hello VPN gateway.</span></span> <span data-ttu-id="a65a6-110">Csak akkor kell tooremove hello kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="a65a6-110">You only need tooremove hello connection.</span></span>


1. <span data-ttu-id="a65a6-111">Távolítsa el a hello kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="a65a6-111">Remove hello connection.</span></span>

  ```powershell
  Remove-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName -ResourceGroupName MyRGName
  ```
2. <span data-ttu-id="a65a6-112">Módosítsa a helyi hálózati átjáró hello címelőtagok.</span><span class="sxs-lookup"><span data-stu-id="a65a6-112">Modify hello address prefixes for your local network gateway.</span></span>
   
  <span data-ttu-id="a65a6-113">Állítsa be a hello LocalNetworkGateway hello változót.</span><span class="sxs-lookup"><span data-stu-id="a65a6-113">Set hello variable for hello LocalNetworkGateway.</span></span>

  ```powershell
  $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName
  ```
   
  <span data-ttu-id="a65a6-114">Hello előtagok módosítása.</span><span class="sxs-lookup"><span data-stu-id="a65a6-114">Modify hello prefixes.</span></span>
   
  ```powershell
  Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
  -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')
  ```
3. <span data-ttu-id="a65a6-115">Hello kapcsolat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="a65a6-115">Create hello connection.</span></span> <span data-ttu-id="a65a6-116">Ebben a példában egy IPsec kapcsolattípust konfigurálunk.</span><span class="sxs-lookup"><span data-stu-id="a65a6-116">In this example, we configure an IPsec connection type.</span></span> <span data-ttu-id="a65a6-117">Ismételt a kapcsolatot, használja a megadott kapcsolattípus hello a konfigurációhoz.</span><span class="sxs-lookup"><span data-stu-id="a65a6-117">When you recreate your connection, use hello connection type that is specified for your configuration.</span></span> <span data-ttu-id="a65a6-118">További kapcsolattípusokat, lásd: hello [PowerShell-parancsmag](https://msdn.microsoft.com/library/mt603611.aspx) lap.</span><span class="sxs-lookup"><span data-stu-id="a65a6-118">For additional connection types, see hello [PowerShell cmdlet](https://msdn.microsoft.com/library/mt603611.aspx) page.</span></span>
   
  <span data-ttu-id="a65a6-119">Állítsa be a hello pedig hello változót.</span><span class="sxs-lookup"><span data-stu-id="a65a6-119">Set hello variable for hello VirtualNetworkGateway.</span></span>

  ```powershell
  $gateway1 = Get-AzureRmVirtualNetworkGateway -Name RMGateway  -ResourceGroupName MyRGName
  ```
   
  <span data-ttu-id="a65a6-120">Hello kapcsolat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="a65a6-120">Create hello connection.</span></span> <span data-ttu-id="a65a6-121">A példa hello változó a 2. lépésben beállított $local.</span><span class="sxs-lookup"><span data-stu-id="a65a6-121">This example uses hello variable $local that you set in step 2.</span></span>

  ```powershell
  New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName `
  -ResourceGroupName MyRGName -Location 'West US' `
  -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
  -ConnectionType IPsec `
  -RoutingWeight 10 -SharedKey 'abc123'
  ```