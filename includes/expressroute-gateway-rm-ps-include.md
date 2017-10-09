<span data-ttu-id="b3a00-101">Ez a feladat használható egy Vnetet hello lépéseket a következő konfigurációs hivatkozáslista hello hello értékei alapján.</span><span class="sxs-lookup"><span data-stu-id="b3a00-101">hello steps for this task use a VNet based on hello values in hello following configuration reference list.</span></span> <span data-ttu-id="b3a00-102">További beállításokat és a nevek azt is ezen a listán.</span><span class="sxs-lookup"><span data-stu-id="b3a00-102">Additional settings and names are also outlined in this list.</span></span> <span data-ttu-id="b3a00-103">Nem használjuk a lista közvetlenül a hello műveletekkel, bár jelenleg felvenni a következő felsorolásban szereplő hello értékek alapján változók.</span><span class="sxs-lookup"><span data-stu-id="b3a00-103">We don't use this list directly in any of hello steps, although we do add variables based on hello values in this list.</span></span> <span data-ttu-id="b3a00-104">Másolhatja hello lista toouse referenciaként hello értékeket cserélje le a saját.</span><span class="sxs-lookup"><span data-stu-id="b3a00-104">You can copy hello list toouse as a reference, replacing hello values with your own.</span></span>

<span data-ttu-id="b3a00-105">**Konfigurációs hivatkozáslista**</span><span class="sxs-lookup"><span data-stu-id="b3a00-105">**Configuration reference list**</span></span>

* <span data-ttu-id="b3a00-106">Virtuális hálózati név = "TestVNet"</span><span class="sxs-lookup"><span data-stu-id="b3a00-106">Virtual Network Name = "TestVNet"</span></span>
* <span data-ttu-id="b3a00-107">Virtuális hálózati címterület = 192.168.0.0/16</span><span class="sxs-lookup"><span data-stu-id="b3a00-107">Virtual Network address space = 192.168.0.0/16</span></span>
* <span data-ttu-id="b3a00-108">Erőforráscsoport = "TestRG"</span><span class="sxs-lookup"><span data-stu-id="b3a00-108">Resource Group = "TestRG"</span></span>
* <span data-ttu-id="b3a00-109">Alhalozat_1 Name = "Előtér"</span><span class="sxs-lookup"><span data-stu-id="b3a00-109">Subnet1 Name = "FrontEnd"</span></span> 
* <span data-ttu-id="b3a00-110">Címterület Alhalozat_1 = "192.168.1.0/24"</span><span class="sxs-lookup"><span data-stu-id="b3a00-110">Subnet1 address space = "192.168.1.0/24"</span></span>
* <span data-ttu-id="b3a00-111">Átjáró alhálózati név: "GatewaySubnet" mindig neve egy átjáró-alhálózatot kell *GatewaySubnet*.</span><span class="sxs-lookup"><span data-stu-id="b3a00-111">Gateway Subnet name: "GatewaySubnet" You must always name a gateway subnet *GatewaySubnet*.</span></span>
* <span data-ttu-id="b3a00-112">Átjáró alhálózati címtartományt = "192.168.200.0/26"</span><span class="sxs-lookup"><span data-stu-id="b3a00-112">Gateway Subnet address space = "192.168.200.0/26"</span></span>
* <span data-ttu-id="b3a00-113">A régióban = "USA keleti régiója"</span><span class="sxs-lookup"><span data-stu-id="b3a00-113">Region = "East US"</span></span>
* <span data-ttu-id="b3a00-114">Átjáró Name = "GW"</span><span class="sxs-lookup"><span data-stu-id="b3a00-114">Gateway Name = "GW"</span></span>
* <span data-ttu-id="b3a00-115">Átjáró IP-név = "GWIP"</span><span class="sxs-lookup"><span data-stu-id="b3a00-115">Gateway IP Name = "GWIP"</span></span>
* <span data-ttu-id="b3a00-116">Átjáró IP-konfiguráció neve = "gwipconf"</span><span class="sxs-lookup"><span data-stu-id="b3a00-116">Gateway IP configuration Name = "gwipconf"</span></span>
* <span data-ttu-id="b3a00-117">Típus = "ExpressRoute" Ez a típus egy ExpressRoute-konfiguráció szükséges.</span><span class="sxs-lookup"><span data-stu-id="b3a00-117">Type = "ExpressRoute" This type is required for an ExpressRoute configuration.</span></span>
* <span data-ttu-id="b3a00-118">Átjáró nyilvános IP-név = "gwpip"</span><span class="sxs-lookup"><span data-stu-id="b3a00-118">Gateway Public IP Name = "gwpip"</span></span>

## <a name="add-a-gateway"></a><span data-ttu-id="b3a00-119">Átjáró hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b3a00-119">Add a gateway</span></span>
1. <span data-ttu-id="b3a00-120">Csatlakozás Azure-előfizetés tooyour.</span><span class="sxs-lookup"><span data-stu-id="b3a00-120">Connect tooyour Azure Subscription.</span></span>

  ```powershell 
  Login-AzureRmAccount
  Get-AzureRmSubscription 
  Select-AzureRmSubscription -SubscriptionName "Name of subscription"
  ```
2. <span data-ttu-id="b3a00-121">Deklarálja a változókat, ehhez a gyakorlathoz.</span><span class="sxs-lookup"><span data-stu-id="b3a00-121">Declare your variables for this exercise.</span></span> <span data-ttu-id="b3a00-122">Lehet, hogy tooedit hello minta tooreflect hello beállításokat, hogy szeretné-e toouse.</span><span class="sxs-lookup"><span data-stu-id="b3a00-122">Be sure tooedit hello sample tooreflect hello settings that you want toouse.</span></span>

  ```powershell 
  $RG = "TestRG"
  $Location = "East US"
  $GWName = "GW"
  $GWIPName = "GWIP"
  $GWIPconfName = "gwipconf"
  $VNetName = "TestVNet"
  ```
3. <span data-ttu-id="b3a00-123">Hello virtuális hálózat objektumot tárolja változóként.</span><span class="sxs-lookup"><span data-stu-id="b3a00-123">Store hello virtual network object as a variable.</span></span>

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
  ```
4. <span data-ttu-id="b3a00-124">Adjon hozzá egy átjáró alhálózati tooyour virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="b3a00-124">Add a gateway subnet tooyour Virtual Network.</span></span> <span data-ttu-id="b3a00-125">hello átjáróalhálózatot "GatewaySubnet" nevet kell kapniuk.</span><span class="sxs-lookup"><span data-stu-id="b3a00-125">hello gateway subnet must be named "GatewaySubnet".</span></span> <span data-ttu-id="b3a00-126">Hozzon létre egy átjáró-alhálózatot, amely /27 vagy nagyobb (26, / / 25, stb.).</span><span class="sxs-lookup"><span data-stu-id="b3a00-126">You should create a gateway subnet that is /27 or larger (/26, /25, etc.).</span></span>

  ```powershell
  Add-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet -AddressPrefix 192.168.200.0/26
  ```
5. <span data-ttu-id="b3a00-127">Hello konfigurációjának beállítása</span><span class="sxs-lookup"><span data-stu-id="b3a00-127">Set hello configuration.</span></span>

  ```powershell
  Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```
6. <span data-ttu-id="b3a00-128">Hello átjáróalhálózatot tárolására változóként.</span><span class="sxs-lookup"><span data-stu-id="b3a00-128">Store hello gateway subnet as a variable.</span></span>

  ```powershell
  $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
  ```
7. <span data-ttu-id="b3a00-129">Kérjen egy nyilvános IP-címet.</span><span class="sxs-lookup"><span data-stu-id="b3a00-129">Request a public IP address.</span></span> <span data-ttu-id="b3a00-130">hello IP-címre van szükség hello átjáró létrehozása előtt.</span><span class="sxs-lookup"><span data-stu-id="b3a00-130">hello IP address is requested before creating hello gateway.</span></span> <span data-ttu-id="b3a00-131">Nem adható meg, hogy szeretné-e toouse; hello IP-cím dinamikusan történik.</span><span class="sxs-lookup"><span data-stu-id="b3a00-131">You cannot specify hello IP address that you want toouse; it’s dynamically allocated.</span></span> <span data-ttu-id="b3a00-132">Hello következő konfigurációs szakasz az IP-címet fogja használni.</span><span class="sxs-lookup"><span data-stu-id="b3a00-132">You'll use this IP address in hello next configuration section.</span></span> <span data-ttu-id="b3a00-133">hello AllocationMethod dinamikus kell lennie.</span><span class="sxs-lookup"><span data-stu-id="b3a00-133">hello AllocationMethod must be Dynamic.</span></span>

  ```powershell
  $pip = New-AzureRmPublicIpAddress -Name $GWIPName  -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic
  ```
8. <span data-ttu-id="b3a00-134">Hozza létre az átjáró hello konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="b3a00-134">Create hello configuration for your gateway.</span></span> <span data-ttu-id="b3a00-135">hello átjáró konfigurációs hello alhálózatot és a hello nyilvános IP-cím toouse határoz meg.</span><span class="sxs-lookup"><span data-stu-id="b3a00-135">hello gateway configuration defines hello subnet and hello public IP address toouse.</span></span> <span data-ttu-id="b3a00-136">Ebben a lépésben meg hello konfigurációs hello átjáró létrehozásakor használható.</span><span class="sxs-lookup"><span data-stu-id="b3a00-136">In this step, you are specifying hello configuration that will be used when you create hello gateway.</span></span> <span data-ttu-id="b3a00-137">Ez a lépés nem hoz létre hello átjáró objektum.</span><span class="sxs-lookup"><span data-stu-id="b3a00-137">This step does not actually create hello gateway object.</span></span> <span data-ttu-id="b3a00-138">Alább toocreate hello minta az átjáró konfigurációt használja.</span><span class="sxs-lookup"><span data-stu-id="b3a00-138">Use hello sample below toocreate your gateway configuration.</span></span>

  ```powershell
  $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip
  ```
9. <span data-ttu-id="b3a00-139">Hello átjáró létrehozása.</span><span class="sxs-lookup"><span data-stu-id="b3a00-139">Create hello gateway.</span></span> <span data-ttu-id="b3a00-140">Ebben a lépésben hello **- GatewayType** különösen fontos.</span><span class="sxs-lookup"><span data-stu-id="b3a00-140">In this step, hello **-GatewayType** is especially important.</span></span> <span data-ttu-id="b3a00-141">Hello értéket kell használnia **ExpressRoute**.</span><span class="sxs-lookup"><span data-stu-id="b3a00-141">You must use hello value **ExpressRoute**.</span></span> <span data-ttu-id="b3a00-142">Miután ezek a parancsmagok, hello átjáró 45 perc vagy több toocreate vehet igénybe.</span><span class="sxs-lookup"><span data-stu-id="b3a00-142">After running these cmdlets, hello gateway can take 45 minutes or more toocreate.</span></span>

  ```powershell
  New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG -Location $Location -IpConfigurations $ipconf -GatewayType Expressroute -GatewaySku Standard
  ```

## <a name="verify-hello-gateway-was-created"></a><span data-ttu-id="b3a00-143">Hello átjáró létrejött-e</span><span class="sxs-lookup"><span data-stu-id="b3a00-143">Verify hello gateway was created</span></span>
<span data-ttu-id="b3a00-144">A következő parancsokat, amelyek átjáró hello tooverify létrejött hello használata:</span><span class="sxs-lookup"><span data-stu-id="b3a00-144">Use hello following commands tooverify that hello gateway has been created:</span></span>

```powershell
Get-AzureRmVirtualNetworkGateway -ResourceGroupName $RG
```

## <a name="resize-a-gateway"></a><span data-ttu-id="b3a00-145">Átjáró méretezése</span><span class="sxs-lookup"><span data-stu-id="b3a00-145">Resize a gateway</span></span>
<span data-ttu-id="b3a00-146">A több [Gateway SKU-n](../articles/expressroute/expressroute-about-virtual-network-gateways.md).</span><span class="sxs-lookup"><span data-stu-id="b3a00-146">There are a number of [Gateway SKUs](../articles/expressroute/expressroute-about-virtual-network-gateways.md).</span></span> <span data-ttu-id="b3a00-147">Hello parancs toochange hello átjáró-Termékváltozat követően bármikor használhatja.</span><span class="sxs-lookup"><span data-stu-id="b3a00-147">You can use hello following command toochange hello Gateway SKU at any time.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b3a00-148">Ez a parancs UltraPerformance átjáró nem működik.</span><span class="sxs-lookup"><span data-stu-id="b3a00-148">This command doesn't work for UltraPerformance gateway.</span></span> <span data-ttu-id="b3a00-149">toochange a tooan UltraPerformance átjárók, először távolítsa el a meglévő ExpressRoute-átjáró hello, és ezután hozzon létre újat UltraPerformance.</span><span class="sxs-lookup"><span data-stu-id="b3a00-149">toochange your gateway tooan UltraPerformance gateway, first remove hello existing ExpressRoute gateway, and then create a new UltraPerformance gateway.</span></span> <span data-ttu-id="b3a00-150">toodowngrade az átjáró UltraPerformance-átjáróról, először el kell távolítani hello UltraPerformance átjáró, és hozzon létre egy új átjárót.</span><span class="sxs-lookup"><span data-stu-id="b3a00-150">toodowngrade your gateway from an UltraPerformance gateway, first remove hello UltraPerformance gateway, and then create a new gateway.</span></span>
> 
> 

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance
```

## <a name="remove-a-gateway"></a><span data-ttu-id="b3a00-151">Átjáró eltávolítása</span><span class="sxs-lookup"><span data-stu-id="b3a00-151">Remove a gateway</span></span>
<span data-ttu-id="b3a00-152">A következő parancs tooremove átjáró hello használata:</span><span class="sxs-lookup"><span data-stu-id="b3a00-152">Use hello following command tooremove a gateway:</span></span>

```powershell
Remove-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
```