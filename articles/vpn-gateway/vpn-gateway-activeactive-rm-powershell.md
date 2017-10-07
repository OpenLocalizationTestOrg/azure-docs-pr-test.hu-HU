---
title: "Aktív-aktív S2S VPN-kapcsolatokat a VPN-átjárók konfigurálása: Azure Resource Manager: PowerShell |} Microsoft Docs"
description: "Ez a cikk bemutatja, hogyan aktív-aktív kapcsolatok konfigurálása az Azure VPN Gatewayek Azure Resource Manager és a PowerShell használatával."
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: azure-resource-manager
ms.assetid: 238cd9b3-f1ce-4341-b18e-7390935604fa
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/16/2017
ms.author: yushwang
ms.openlocfilehash: 964eedc7698e42bf0e082f0105845f2a339daf57
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-active-active-s2s-vpn-connections-with-azure-vpn-gateways"></a><span data-ttu-id="b04fd-103">Az Azure VPN Gatewayek aktív-aktív S2S VPN-kapcsolatok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b04fd-103">Configure active-active S2S VPN connections with Azure VPN Gateways</span></span>

<span data-ttu-id="b04fd-104">Ez a cikk bemutatja, hogyan hello lépéseket toocreate aktív-aktív létesítmények közötti és VNet – VNet kapcsolatokhoz hello Resource Manager üzembe helyezési modellben és a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="b04fd-104">This article walks you through hello steps toocreate active-active cross-premises and VNet-to-VNet connections using hello Resource Manager deployment model and PowerShell.</span></span>

## <a name="about-highly-available-cross-premises-connections"></a><span data-ttu-id="b04fd-105">Magas rendelkezésre állású létesítmények közötti kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="b04fd-105">About Highly Available Cross-Premises Connections</span></span>
<span data-ttu-id="b04fd-106">tooachieve magas rendelkezésre állású létesítmények közötti és VNet – VNet-kapcsolatot, kell több VPN-átjáró telepítése és a hálózatok és az Azure közötti több párhuzamos kapcsolatok létesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="b04fd-106">tooachieve high availability for cross-premises and VNet-to-VNet connectivity, you should deploy multiple VPN gateways and establish multiple parallel connections between your networks and Azure.</span></span> <span data-ttu-id="b04fd-107">Ellenőrizze a [magas rendelkezésre álló létesítmények közötti és VNet – VNet-kapcsolatot](vpn-gateway-highlyavailable.md) kapcsolati lehetőségek és topológiájának áttekintését.</span><span class="sxs-lookup"><span data-stu-id="b04fd-107">Please see [Highly Available Cross-Premises and VNet-to-VNet Connectivity](vpn-gateway-highlyavailable.md) for an overview of connectivity options and topology.</span></span>

<span data-ttu-id="b04fd-108">Ez a cikk bemutatja, hello fel egy aktív-aktív tooset létesítmények közötti VPN-kapcsolat és két virtuális hálózatok közötti aktív-aktív kapcsolat:</span><span class="sxs-lookup"><span data-stu-id="b04fd-108">This article provides hello instructions tooset up an active-active cross-premises VPN connection, and active-active connection between two virtual networks:</span></span>

* [<span data-ttu-id="b04fd-109">1. rész – létrehozása és konfigurálása az Azure VPN gateway aktív-aktív módban</span><span class="sxs-lookup"><span data-stu-id="b04fd-109">Part 1 - Create and configure your Azure VPN gateway in active-active mode</span></span>](#aagateway)
* [<span data-ttu-id="b04fd-110">2. rész – aktív-aktív létesítmények közötti kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="b04fd-110">Part 2 - Establish active-active cross-premises connections</span></span>](#aacrossprem)
* [<span data-ttu-id="b04fd-111">3. rész – aktív-aktív VNet – VNet-kapcsolatot létrehozni</span><span class="sxs-lookup"><span data-stu-id="b04fd-111">Part 3 - Establish active-active VNet-to-VNet connections</span></span>](#aav2v)
* [<span data-ttu-id="b04fd-112">4. rész - frissítés meglévő átjáró aktív-aktív és aktív-készenléti állapotban lévő között</span><span class="sxs-lookup"><span data-stu-id="b04fd-112">Part 4 - Update existing gateway between active-active and active-standby</span></span>](#aaupdate)

<span data-ttu-id="b04fd-113">Ezek együtt toobuild egy összetettebb, magas rendelkezésre állású hálózati topológia az igényeinek megfelelő kombinálhatja.</span><span class="sxs-lookup"><span data-stu-id="b04fd-113">You can combine these together toobuild a more complex, highly available network topology that meets your needs.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b04fd-114">Vegye figyelembe, hogy hello aktív-aktív üzemmód használja a következő termékváltozatok csak hello:</span><span class="sxs-lookup"><span data-stu-id="b04fd-114">Please note that hello active-active mode uses only hello following SKUs:</span></span> 
  * <span data-ttu-id="b04fd-115">VpnGw1, VpnGw2, VpnGw3</span><span class="sxs-lookup"><span data-stu-id="b04fd-115">VpnGw1, VpnGw2, VpnGw3</span></span>
  * <span data-ttu-id="b04fd-116">(A régi örökölt SKU) HighPerformance</span><span class="sxs-lookup"><span data-stu-id="b04fd-116">HighPerformance (for old legacy SKUs)</span></span>
> 
> 

## <span data-ttu-id="b04fd-117"><a name ="aagateway"></a>1. rész – létrehozása és aktív-aktív VPN-átjárók konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b04fd-117"><a name ="aagateway"></a>Part 1 - Create and configure active-active VPN gateways</span></span>
<span data-ttu-id="b04fd-118">a lépéseket követve hello konfigurálja az Azure VPN gateway aktív-aktív üzemmódban.</span><span class="sxs-lookup"><span data-stu-id="b04fd-118">hello following steps will configure your Azure VPN gateway in active-active modes.</span></span> <span data-ttu-id="b04fd-119">hello hello aktív-aktív és az aktív-készenléti állapotban lévő átjárók közötti fontosabb különbségeket:</span><span class="sxs-lookup"><span data-stu-id="b04fd-119">hello key differences between hello active-active and active-standby gateways:</span></span>

* <span data-ttu-id="b04fd-120">Két átjáró IP-konfigurációk toocreate két nyilvános IP-címmel van szüksége</span><span class="sxs-lookup"><span data-stu-id="b04fd-120">You need toocreate two Gateway IP configurations with two public IP addresses</span></span>
* <span data-ttu-id="b04fd-121">Hello EnableActiveActiveFeature jelzőt kell beállítani</span><span class="sxs-lookup"><span data-stu-id="b04fd-121">You need set hello EnableActiveActiveFeature flag</span></span>
* <span data-ttu-id="b04fd-122">hello gateway SKU VpnGw1, VpnGw2, VpnGw3 vagy HighPerformance (örökölt SKU) kell lennie.</span><span class="sxs-lookup"><span data-stu-id="b04fd-122">hello gateway SKU must be VpnGw1, VpnGw2, VpnGw3, or HighPerformance (legacy SKU).</span></span>

<span data-ttu-id="b04fd-123">hello más tulajdonságainak vannak hello ugyanaz, mint a hello nem aktív-aktív átjárókat.</span><span class="sxs-lookup"><span data-stu-id="b04fd-123">hello other properties are hello same as hello non-active-active gateways.</span></span> 

### <a name="before-you-begin"></a><span data-ttu-id="b04fd-124">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="b04fd-124">Before you begin</span></span>
* <span data-ttu-id="b04fd-125">Győződjön meg arról, hogy rendelkezik Azure-előfizetéssel.</span><span class="sxs-lookup"><span data-stu-id="b04fd-125">Verify that you have an Azure subscription.</span></span> <span data-ttu-id="b04fd-126">Ha még nincs Azure-előfizetése, aktiválhatja [MSDN-előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), vagy regisztrálhat egy [ingyenes fiókot](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b04fd-126">If you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or sign up for a [free account](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="b04fd-127">Tooinstall hello Azure Resource Manager PowerShell-parancsmagok lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="b04fd-127">You'll need tooinstall hello Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="b04fd-128">Lásd: [áttekintés az Azure PowerShell](/powershell/azure/overview) hello PowerShell-parancsmagok telepítéséről további információt.</span><span class="sxs-lookup"><span data-stu-id="b04fd-128">See [Overview of Azure PowerShell](/powershell/azure/overview) for more information about installing hello PowerShell cmdlets.</span></span>

### <a name="step-1---create-and-configure-vnet1"></a><span data-ttu-id="b04fd-129">1. lépés – létrehozása és VNet1 konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b04fd-129">Step 1 - Create and configure VNet1</span></span>
#### <a name="1-declare-your-variables"></a><span data-ttu-id="b04fd-130">1. A változók deklarálása</span><span class="sxs-lookup"><span data-stu-id="b04fd-130">1. Declare your variables</span></span>
<span data-ttu-id="b04fd-131">Ezt a gyakorlatot a változók deklarálásával kezdjük.</span><span class="sxs-lookup"><span data-stu-id="b04fd-131">For this exercise, we'll start by declaring our variables.</span></span> <span data-ttu-id="b04fd-132">az alábbi példa hello hello értékekkel ehhez a gyakorlathoz hello változók deklarálja.</span><span class="sxs-lookup"><span data-stu-id="b04fd-132">hello example below declares hello variables using hello values for this exercise.</span></span> <span data-ttu-id="b04fd-133">Lehet, hogy tooreplace hello értékeket a saját üzemi konfigurálásakor.</span><span class="sxs-lookup"><span data-stu-id="b04fd-133">Be sure tooreplace hello values with your own when configuring for production.</span></span> <span data-ttu-id="b04fd-134">Ezek a változók is használhatja, ha ismeri a konfiguráció az ilyen típusú hello lépéseket toobecome keresztül futtatja.</span><span class="sxs-lookup"><span data-stu-id="b04fd-134">You can use these variables if you are running through hello steps toobecome familiar with this type of configuration.</span></span> <span data-ttu-id="b04fd-135">Hello változók módosítása, majd másolja és illessze be a PowerShell-konzolban.</span><span class="sxs-lookup"><span data-stu-id="b04fd-135">Modify hello variables, and then copy and paste into your PowerShell console.</span></span>

```powershell
$Sub1 = "Ross"
$RG1 = "TestAARG1"
$Location1 = "West US"
$VNetName1 = "TestVNet1"
$FESubName1 = "FrontEnd"
$BESubName1 = "Backend"
$GWSubName1 = "GatewaySubnet"
$VNetPrefix11 = "10.11.0.0/16"
$VNetPrefix12 = "10.12.0.0/16"
$FESubPrefix1 = "10.11.0.0/24"
$BESubPrefix1 = "10.12.0.0/24"
$GWSubPrefix1 = "10.12.255.0/27"
$VNet1ASN = 65010
$DNS1 = "8.8.8.8"
$GWName1 = "VNet1GW"
$GW1IPName1 = "VNet1GWIP1"
$GW1IPName2 = "VNet1GWIP2"
$GW1IPconf1 = "gw1ipconf1"
$GW1IPconf2 = "gw1ipconf2"
$Connection12 = "VNet1toVNet2"
$Connection151 = "VNet1toSite5_1"
$Connection152 = "VNet1toSite5_2"
```

#### <a name="2-connect-tooyour-subscription-and-create-a-new-resource-group"></a><span data-ttu-id="b04fd-136">2. Csatlakozás tooyour előfizetés, és hozzon létre egy új erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="b04fd-136">2. Connect tooyour subscription and create a new resource group</span></span>
<span data-ttu-id="b04fd-137">Váltson át tooPowerShell mód toouse hello erőforrás-kezelő parancsmagokat.</span><span class="sxs-lookup"><span data-stu-id="b04fd-137">Make sure you switch tooPowerShell mode toouse hello Resource Manager cmdlets.</span></span> <span data-ttu-id="b04fd-138">További információ: [A Windows PowerShell használata a Resource Managerrel](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="b04fd-138">For more information, see [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

<span data-ttu-id="b04fd-139">Nyissa meg a PowerShell-konzolt, és csatlakozzon a tooyour fiók.</span><span class="sxs-lookup"><span data-stu-id="b04fd-139">Open your PowerShell console and connect tooyour account.</span></span> <span data-ttu-id="b04fd-140">A következő minta toohelp csatlakozás hello használata:</span><span class="sxs-lookup"><span data-stu-id="b04fd-140">Use hello following sample toohelp you connect:</span></span>

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $Sub1
New-AzureRmResourceGroup -Name $RG1 -Location $Location1
```

#### <a name="3-create-testvnet1"></a><span data-ttu-id="b04fd-141">3. A TestVNet1 létrehozása</span><span class="sxs-lookup"><span data-stu-id="b04fd-141">3. Create TestVNet1</span></span>
<span data-ttu-id="b04fd-142">hello minta az alábbi nevű TestVNet1 és három alhálózatok, egy hívott GatewaySubnet, egy hívott előtér és egy hívott háttér virtuális hálózatot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="b04fd-142">hello sample below creates a virtual network named TestVNet1 and three subnets, one called GatewaySubnet, one called FrontEnd, and one called Backend.</span></span> <span data-ttu-id="b04fd-143">Az értékek behelyettesítésekor fontos, hogy az átjáróalhálózat neve mindenképp GatewaySubnet legyen.</span><span class="sxs-lookup"><span data-stu-id="b04fd-143">When substituting values, it's important that you always name your gateway subnet specifically GatewaySubnet.</span></span> <span data-ttu-id="b04fd-144">Ha ezt másként nevezi el, az átjáró létrehozása meghiúsul.</span><span class="sxs-lookup"><span data-stu-id="b04fd-144">If you name it something else, your gateway creation will fail.</span></span>

```powershell
$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
$besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
$gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1
```

### <a name="step-2---create-hello-vpn-gateway-for-testvnet1-with-active-active-mode"></a><span data-ttu-id="b04fd-145">2. lépés – hello VPN-átjáró létrehozása TestVNet1 aktív-aktív üzemmódban</span><span class="sxs-lookup"><span data-stu-id="b04fd-145">Step 2 - Create hello VPN gateway for TestVNet1 with active-active mode</span></span>
#### <a name="1-create-hello-public-ip-addresses-and-gateway-ip-configurations"></a><span data-ttu-id="b04fd-146">1. Hello nyilvános IP-címek és az átjáró IP-konfigurációk létrehozása</span><span class="sxs-lookup"><span data-stu-id="b04fd-146">1. Create hello public IP addresses and gateway IP configurations</span></span>
<span data-ttu-id="b04fd-147">Kérelem két nyilvános IP-címek toobe lefoglalt toohello átjáró fog létrehozni a virtuális hálózat számára.</span><span class="sxs-lookup"><span data-stu-id="b04fd-147">Request two public IP addresses toobe allocated toohello gateway you will create for your VNet.</span></span> <span data-ttu-id="b04fd-148">Hello alhálózatot és IP-konfiguráció szükséges is fogja definiálni.</span><span class="sxs-lookup"><span data-stu-id="b04fd-148">You'll also define hello subnet and IP configurations required.</span></span>

```powershell
$gw1pip1 = New-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
$gw1pip2 = New-AzureRmPublicIpAddress -Name $GW1IPName2 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic

$vnet1 = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
$subnet1 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
$gw1ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf1 -Subnet $subnet1 -PublicIpAddress $gw1pip1
$gw1ipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf2 -Subnet $subnet1 -PublicIpAddress $gw1pip2
```

#### <a name="2-create-hello-vpn-gateway-with-active-active-configuration"></a><span data-ttu-id="b04fd-149">2. Aktív-aktív konfigurációval hello VPN-átjáró létrehozása</span><span class="sxs-lookup"><span data-stu-id="b04fd-149">2. Create hello VPN gateway with active-active configuration</span></span>
<span data-ttu-id="b04fd-150">TestVNet1 hello virtuális hálózati átjáró létrehozása</span><span class="sxs-lookup"><span data-stu-id="b04fd-150">Create hello virtual network gateway for TestVNet1.</span></span> <span data-ttu-id="b04fd-151">Vegye figyelembe, hogy két GatewayIpConfig bejegyzést is tartalmaz, és hello EnableActiveActiveFeature jelző be van állítva.</span><span class="sxs-lookup"><span data-stu-id="b04fd-151">Note that there are two GatewayIpConfig entries, and hello EnableActiveActiveFeature flag is set.</span></span> <span data-ttu-id="b04fd-152">Átjáró létrehozása időt vehet igénybe (45 perc vagy több toocomplete).</span><span class="sxs-lookup"><span data-stu-id="b04fd-152">Creating a gateway can take a while (45 minutes or more toocomplete).</span></span>

```powershell
New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gw1ipconf1,$gw1ipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1 -Asn $VNet1ASN -EnableActiveActiveFeature -Debug
```

#### <a name="3-obtain-hello-gateway-public-ip-addresses-and-hello-bgp-peer-ip-address"></a><span data-ttu-id="b04fd-153">3. Hello átjáró nyilvános IP-címek és hello BGP-Társgép IP-cím beszerzése</span><span class="sxs-lookup"><span data-stu-id="b04fd-153">3. Obtain hello gateway public IP addresses and hello BGP Peer IP address</span></span>
<span data-ttu-id="b04fd-154">Hello átjáró létrehozása után kell tooobtain hello hello Azure VPN Gateway a BGP-Társgép IP-cím.</span><span class="sxs-lookup"><span data-stu-id="b04fd-154">Once hello gateway is created, you will need tooobtain hello BGP Peer IP address on hello Azure VPN Gateway.</span></span> <span data-ttu-id="b04fd-155">Ez a cím szükséges tooconfigure hello Azure VPN Gateway, mint a BGP-partner a helyszíni VPN-eszközök.</span><span class="sxs-lookup"><span data-stu-id="b04fd-155">This address is needed tooconfigure hello Azure VPN Gateway as a BGP Peer for your on-premises VPN devices.</span></span>

```powershell
$gw1pip1 = Get-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1
$gw1pip2 = Get-AzureRmPublicIpAddress -Name $GW1IPName2 -ResourceGroupName $RG1
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
```

<span data-ttu-id="b04fd-156">A következő parancsmagok tooshow hello két nyilvános IP-címet a VPN-átjáró és az egyes átjárópéldány megfelelő BGP-Társgép IP-címeit számára lefoglalt hello használata:</span><span class="sxs-lookup"><span data-stu-id="b04fd-156">Use hello following cmdlets tooshow hello two public IP addresses allocated for your VPN gateway, and their corresponding BGP Peer IP addresses for each gateway instance:</span></span>

```powershell

    PS D:\> $gw1pip1.IpAddress
    40.112.190.5

    PS D:\> $gw1pip2.IpAddress
    138.91.156.129

    PS D:\> $vnet1gw.BgpSettingsText
    {
      "Asn": 65010,
      "BgpPeeringAddress": "10.12.255.4,10.12.255.5",
      "PeerWeight": 0
    }
```

<span data-ttu-id="b04fd-157">hello ahhoz, hogy hello példányai és a megfelelő BGP társviszony-létesítés cím hello hello nyilvános IP-címek hello azonos.</span><span class="sxs-lookup"><span data-stu-id="b04fd-157">hello order of hello public IP addresses for hello gateway instances and hello corresponding BGP Peering Addresses are hello same.</span></span> <span data-ttu-id="b04fd-158">Ebben a példában hello átjáró nyilvános IP-címe 40.112.190.5 rendelkező virtuális gépet a BGP társviszony-létesítés cím 10.12.255.4 fog használni, és 138.91.156.129 hello átjáró 10.12.255.5 fogja használni.</span><span class="sxs-lookup"><span data-stu-id="b04fd-158">In this example, hello gateway VM with public IP of 40.112.190.5 will use 10.12.255.4 as its BGP Peering Address, and hello gateway with 138.91.156.129 will use 10.12.255.5.</span></span> <span data-ttu-id="b04fd-159">Ezek az információk szükségesek beállításakor a helyszíni összekötő toohello aktív-aktív átjáró VPN-eszközök.</span><span class="sxs-lookup"><span data-stu-id="b04fd-159">This information is needed when you set up your on premises VPN devices connecting toohello active-active gateway.</span></span> <span data-ttu-id="b04fd-160">hello átjáró összes címre alatt hello ábrán látható:</span><span class="sxs-lookup"><span data-stu-id="b04fd-160">hello gateway is shown in hello diagram below with all addresses:</span></span>

![aktív-aktív átjáró](./media/vpn-gateway-activeactive-rm-powershell/active-active-gw.png)

<span data-ttu-id="b04fd-162">Hello átjáró létrehozása után a átjáró tooestablish aktív-aktív létesítmények közötti és VNet – VNet-kapcsolatot is használhatja.</span><span class="sxs-lookup"><span data-stu-id="b04fd-162">Once hello gateway is created, you can use this gateway tooestablish active-active cross-premises or VNet-to-VNet connection.</span></span> <span data-ttu-id="b04fd-163">a következő szakaszok hello keresztül hello lépéseket toocomplete hello gyakorlat részletesen ismerteti.</span><span class="sxs-lookup"><span data-stu-id="b04fd-163">hello following sections will walk through hello steps toocomplete hello exercise.</span></span>

## <span data-ttu-id="b04fd-164"><a name ="aacrossprem"></a>2. rész – egy aktív-aktív létesítmények közötti kapcsolat</span><span class="sxs-lookup"><span data-stu-id="b04fd-164"><a name ="aacrossprem"></a>Part 2 - Establish an active-active cross-premises connection</span></span>
<span data-ttu-id="b04fd-165">tooestablish létesítmények közötti kapcsolatot, a helyi hálózati átjáró toorepresent toocreate szüksége, és a helyszíni VPN-eszköz kapcsolat tooconnect hello Azure VPN-átjáró hello helyi hálózati átjáró.</span><span class="sxs-lookup"><span data-stu-id="b04fd-165">tooestablish a cross-premises connection, you need toocreate a Local Network Gateway toorepresent your on-premises VPN device, and a Connection tooconnect hello Azure VPN gateway with hello local network gateway.</span></span> <span data-ttu-id="b04fd-166">Ebben a példában hello Azure VPN gateway aktív-aktív módban van.</span><span class="sxs-lookup"><span data-stu-id="b04fd-166">In this example, hello Azure VPN gateway is in active-active mode.</span></span> <span data-ttu-id="b04fd-167">Ennek eredményeképpen ellenére, hogy csak az egyiket a helyszíni VPN-eszközön (helyi hálózati átjáró) és egy kapcsolati erőforrást, mindkét Azure VPN gateway példányok fog létrehozni a S2S VPN-alagutat hello helyszíni eszköz.</span><span class="sxs-lookup"><span data-stu-id="b04fd-167">As a result, even though there is only one on-premises VPN device (local network gateway) and one connection resource, both Azure VPN gateway instances will establish S2S VPN tunnels with hello on-premises device.</span></span>

<span data-ttu-id="b04fd-168">A folytatás előtt ellenőrizze, hogy befejeződött [1. rész](#aagateway) ebben a gyakorlatban.</span><span class="sxs-lookup"><span data-stu-id="b04fd-168">Before proceeding, please make sure you have completed [Part 1](#aagateway) of this exercise.</span></span>

### <a name="step-1---create-and-configure-hello-local-network-gateway"></a><span data-ttu-id="b04fd-169">1. lépés – létrehozása és hello helyi hálózati átjáró konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b04fd-169">Step 1 - Create and configure hello local network gateway</span></span>
#### <a name="1-declare-your-variables"></a><span data-ttu-id="b04fd-170">1. A változók deklarálása</span><span class="sxs-lookup"><span data-stu-id="b04fd-170">1. Declare your variables</span></span>
<span data-ttu-id="b04fd-171">Ebben a gyakorlatban továbbra is toobuild hello konfigurációs hello ábrán is látható.</span><span class="sxs-lookup"><span data-stu-id="b04fd-171">This exercise will continue toobuild hello configuration shown in hello diagram.</span></span> <span data-ttu-id="b04fd-172">Lehet, hogy tooreplace hello értékeket hasonlíthatja hello megjeleníteni kívánt toouse a konfigurációhoz.</span><span class="sxs-lookup"><span data-stu-id="b04fd-172">Be sure tooreplace hello values with hello ones that you want toouse for your configuration.</span></span>

```powershell
$RG5 = "TestAARG5"
$Location5 = "West US"
$LNGName51 = "Site5_1"
$LNGPrefix51 = "10.52.255.253/32"
$LNGIP51 = "131.107.72.22"
$LNGASN5 = 65050
$BGPPeerIP51 = "10.52.255.253"
```

<span data-ttu-id="b04fd-173">Néhány dolgot toonote hello helyi hálózati átjáró paraméterek kapcsolatban:</span><span class="sxs-lookup"><span data-stu-id="b04fd-173">A couple of things toonote regarding hello local network gateway parameters:</span></span>

* <span data-ttu-id="b04fd-174">hello helyi hálózati átjáró lehetnek azonos hello, vagy más helyre és az erőforrás csoport mint hello VPN-átjáró.</span><span class="sxs-lookup"><span data-stu-id="b04fd-174">hello local network gateway can be in hello same or different location and resource group as hello VPN gateway.</span></span> <span data-ttu-id="b04fd-175">Ez a példa bemutatja azokat a különböző erőforráscsoportokra, de hello Azure ugyanazon a helyen.</span><span class="sxs-lookup"><span data-stu-id="b04fd-175">This example shows them in different resource groups but in hello same Azure location.</span></span>
* <span data-ttu-id="b04fd-176">Ha csak egy helyszíni VPN-eszköz a fentiek szerint, hello aktív-aktív kapcsolat együttműködhet, függetlenül a BGP protokollt.</span><span class="sxs-lookup"><span data-stu-id="b04fd-176">If there is only one on-premises VPN device as shown above, hello active-active connection can work with or without BGP protocol.</span></span> <span data-ttu-id="b04fd-177">A példa BGP hello létesítmények közötti kapcsolathoz.</span><span class="sxs-lookup"><span data-stu-id="b04fd-177">This example uses BGP for hello cross-premises connection.</span></span>
* <span data-ttu-id="b04fd-178">Ha a BGP engedélyezve van, a toodeclare hello helyi hálózati átjáró szükséges hello előtag az hello állomás címe a VPN-eszköz a BGP-Társgép IP-cím.</span><span class="sxs-lookup"><span data-stu-id="b04fd-178">If BGP is enabled, hello prefix you need toodeclare for hello local network gateway is hello host address of your BGP Peer IP address on your VPN device.</span></span> <span data-ttu-id="b04fd-179">Egy /32 ebben az esetben a "10.52.255.253/32" előtaggal.</span><span class="sxs-lookup"><span data-stu-id="b04fd-179">In this case, it's a /32 prefix of "10.52.255.253/32".</span></span>
* <span data-ttu-id="b04fd-180">Ne feledje a helyszíni hálózatokhoz és az Azure VNet közötti különböző BGP ASN kell használnia.</span><span class="sxs-lookup"><span data-stu-id="b04fd-180">As a reminder, you must use different BGP ASNs between your on-premises networks and Azure VNet.</span></span> <span data-ttu-id="b04fd-181">Ha azok hello vannak ugyanaz, meg kell toochange a VNet ASN Ha a helyszíni VPN-eszköz már használ hello ASN toopeer más BGP szomszédok.</span><span class="sxs-lookup"><span data-stu-id="b04fd-181">If they are hello same, you need toochange your VNet ASN if your on-premises VPN device already uses hello ASN toopeer with other BGP neighbors.</span></span>

#### <a name="2-create-hello-local-network-gateway-for-site5"></a><span data-ttu-id="b04fd-182">2. Site5 hello helyi hálózati átjáró létrehozása</span><span class="sxs-lookup"><span data-stu-id="b04fd-182">2. Create hello local network gateway for Site5</span></span>
<span data-ttu-id="b04fd-183">A folytatás előtt győződjön meg arról, hogy továbbra is csatlakoztatott tooSubscription 1.</span><span class="sxs-lookup"><span data-stu-id="b04fd-183">Before you continue, please make sure you are still connected tooSubscription 1.</span></span> <span data-ttu-id="b04fd-184">Hello erőforráscsoport létrehozása, ha az még nem jött létre.</span><span class="sxs-lookup"><span data-stu-id="b04fd-184">Create hello resource group if it is not yet created.</span></span>

```powershell
New-AzureRmResourceGroup -Name $RG5 -Location $Location5
New-AzureRmLocalNetworkGateway -Name $LNGName51 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP51 -AddressPrefix $LNGPrefix51 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP51
```

### <a name="step-2---connect-hello-vnet-gateway-and-local-network-gateway"></a><span data-ttu-id="b04fd-185">2. lépés – hello hálózatok átjáró és a helyi hálózati átjáró</span><span class="sxs-lookup"><span data-stu-id="b04fd-185">Step 2 - Connect hello VNet gateway and local network gateway</span></span>
#### <a name="1-get-hello-two-gateways"></a><span data-ttu-id="b04fd-186">1. Hello két átjáró beolvasása</span><span class="sxs-lookup"><span data-stu-id="b04fd-186">1. Get hello two gateways</span></span>

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng5gw1 = Get-AzureRmLocalNetworkGateway  -Name $LNGName51 -ResourceGroupName $RG5
```

#### <a name="2-create-hello-testvnet1-toosite5-connection"></a><span data-ttu-id="b04fd-187">2. Hello TestVNet1 tooSite5 kapcsolat létrehozása</span><span class="sxs-lookup"><span data-stu-id="b04fd-187">2. Create hello TestVNet1 tooSite5 connection</span></span>
<span data-ttu-id="b04fd-188">Ebben a lépésben együtt fogja létrehozni hello kapcsolat a TestVNet1 tooSite5_1 túl beállítása "EnableBGP" $True.</span><span class="sxs-lookup"><span data-stu-id="b04fd-188">In this step, you will create hello connection from TestVNet1 tooSite5_1 with "EnableBGP" set too$True.</span></span>

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name $Connection151 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw1 -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP True
```

#### <a name="3-vpn-and-bgp-parameters-for-your-on-premises-vpn-device"></a><span data-ttu-id="b04fd-189">3. A helyszíni VPN-eszköz VPN és BGP paraméterei</span><span class="sxs-lookup"><span data-stu-id="b04fd-189">3. VPN and BGP parameters for your on-premises VPN device</span></span>
<span data-ttu-id="b04fd-190">az alábbi példa hello beírja hello BGP konfigurációs szakasz azokat a helyszíni VPN-eszköz ebben a gyakorlatban hello paramétereket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="b04fd-190">hello example below lists hello parameters you will enter into hello BGP configuration section on your on-premises VPN device for this exercise:</span></span>

    - <span data-ttu-id="b04fd-191">Site5 ASN: 65050</span><span class="sxs-lookup"><span data-stu-id="b04fd-191">Site5 ASN            : 65050</span></span>
    - <span data-ttu-id="b04fd-192">BGP-IP-Site5: 10.52.255.253</span><span class="sxs-lookup"><span data-stu-id="b04fd-192">Site5 BGP IP         : 10.52.255.253</span></span>
    - <span data-ttu-id="b04fd-193">Előtagok tooannounce: (példa) 10.51.0.0/16 és 10.52.0.0/16</span><span class="sxs-lookup"><span data-stu-id="b04fd-193">Prefixes tooannounce : (for example) 10.51.0.0/16 and 10.52.0.0/16</span></span>
    - <span data-ttu-id="b04fd-194">Az Azure VNet ASN: 65010</span><span class="sxs-lookup"><span data-stu-id="b04fd-194">Azure VNet ASN       : 65010</span></span>
    - <span data-ttu-id="b04fd-195">Az Azure VNet BGP-IP-1: 10.12.255.4 az alagutat too40.112.190.5</span><span class="sxs-lookup"><span data-stu-id="b04fd-195">Azure VNet BGP IP 1  : 10.12.255.4 for tunnel too40.112.190.5</span></span>
    - <span data-ttu-id="b04fd-196">Az Azure VNet BGP IP 2: 10.12.255.5 az alagutat too138.91.156.129</span><span class="sxs-lookup"><span data-stu-id="b04fd-196">Azure VNet BGP IP 2  : 10.12.255.5 for tunnel too138.91.156.129</span></span>
    - <span data-ttu-id="b04fd-197">Statikus útvonal: cél 10.12.255.4/32, a következő ugrás hello VPN alagút felület too40.112.190.5 cél 10.12.255.5/32, a következő ugrás hello VPN alagút felület too138.91.156.129</span><span class="sxs-lookup"><span data-stu-id="b04fd-197">Static routes        : Destination 10.12.255.4/32, nexthop hello VPN tunnel interface too40.112.190.5                        Destination 10.12.255.5/32, nexthop hello VPN tunnel interface too138.91.156.129</span></span>
    - <span data-ttu-id="b04fd-198">Többszörös ugrási eBGP: az ebgp-t az eszközön engedélyezve van, ha szükséges, győződjön meg arról, hogy hello "Többszörös ugrási" beállítás</span><span class="sxs-lookup"><span data-stu-id="b04fd-198">eBGP Multihop        : Ensure hello "multihop" option for eBGP is enabled on your device if needed</span></span>

<span data-ttu-id="b04fd-199">hello kell kapcsolatot után néhány percet, és hello BGP társviszony-létesítési munkamenetet hello IPsec-kapcsolat létrejötte után elindul.</span><span class="sxs-lookup"><span data-stu-id="b04fd-199">hello connection should be established after a few minutes, and hello BGP peering session will start once hello IPsec connection is established.</span></span> <span data-ttu-id="b04fd-200">Ebben a példában, amennyiben van beállítva csak egy helyszíni VPN-eszközön, lent látható módon hello diagram eredményezi:</span><span class="sxs-lookup"><span data-stu-id="b04fd-200">This example so far has configured only one on-premises VPN device, resulting in hello diagram shown below:</span></span>

![aktív-aktív-crossprem](./media/vpn-gateway-activeactive-rm-powershell/active-active.png)

### <a name="step-3---connect-two-on-premises-vpn-devices-toohello-active-active-vpn-gateway"></a><span data-ttu-id="b04fd-202">3. lépés - csatlakozás két helyszíni VPN eszközök toohello aktív-aktív VPN-átjáró</span><span class="sxs-lookup"><span data-stu-id="b04fd-202">Step 3 - Connect two on-premises VPN devices toohello active-active VPN gateway</span></span>
<span data-ttu-id="b04fd-203">Ha két VPN-eszközök: hello megegyezik a helyszíni hálózat, kettős redundancia érhet el, kapcsolódó hello Azure VPN gateway toohello második VPN-eszköz.</span><span class="sxs-lookup"><span data-stu-id="b04fd-203">If you have two VPN devices at hello same on-premises network, you can achieve dual redundancy by connecting hello Azure VPN gateway toohello second VPN device.</span></span>

#### <a name="1-create-hello-second-local-network-gateway-for-site5"></a><span data-ttu-id="b04fd-204">1. Site5 hello második helyi hálózati átjáró létrehozása</span><span class="sxs-lookup"><span data-stu-id="b04fd-204">1. Create hello second local network gateway for Site5</span></span>
<span data-ttu-id="b04fd-205">Vegye figyelembe, hogy hello átjáró IP-cím, a címelőtagot és hello második helyi hálózati átjáró BGP társviszony-létesítési címe nem lehet átfedésben hello hello előző helyi hálózati átjáró megegyezik a helyi hálózaton.</span><span class="sxs-lookup"><span data-stu-id="b04fd-205">Note that hello gateway IP address, address prefix, and BGP peering address for hello second local network gateway must not overlap with hello previous local network gateway for hello same on-premises network.</span></span>

```powershell
$LNGName52 = "Site5_2"
$LNGPrefix52 = "10.52.255.254/32"
$LNGIP52 = "131.107.72.23"
$BGPPeerIP52 = "10.52.255.254"

New-AzureRmLocalNetworkGateway -Name $LNGName52 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP52 -AddressPrefix $LNGPrefix52 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP52
```

#### <a name="2-connect-hello-vnet-gateway-and-hello-second-local-network-gateway"></a><span data-ttu-id="b04fd-206">2. Hello hálózatok átjáró és hello második helyi hálózati átjáró</span><span class="sxs-lookup"><span data-stu-id="b04fd-206">2. Connect hello VNet gateway and hello second local network gateway</span></span>
<span data-ttu-id="b04fd-207">Hozzon létre hello kapcsolat a TestVNet1 tooSite5_2 túl beállítása "EnableBGP" $True</span><span class="sxs-lookup"><span data-stu-id="b04fd-207">Create hello connection from TestVNet1 tooSite5_2 with "EnableBGP" set too$True</span></span>

```powershell
$lng5gw2 = Get-AzureRmLocalNetworkGateway -Name $LNGName52 -ResourceGroupName $RG5

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection152 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw2 -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP True
```

#### <a name="3-vpn-and-bgp-parameters-for-your-second-on-premises-vpn-device"></a><span data-ttu-id="b04fd-208">3. A második a helyszíni VPN-eszköz VPN és BGP paraméterei</span><span class="sxs-lookup"><span data-stu-id="b04fd-208">3. VPN and BGP parameters for your second on-premises VPN device</span></span>
<span data-ttu-id="b04fd-209">Hasonlóan alábbi listák hello paraméter megadja a hello második VPN-eszköz:</span><span class="sxs-lookup"><span data-stu-id="b04fd-209">Similarly, below lists hello parameters you will enter into hello second VPN device:</span></span>

```
- Site5 ASN            : 65050
- Site5 BGP IP         : 10.52.255.254
- Prefixes tooannounce : (for example) 10.51.0.0/16 and 10.52.0.0/16
- Azure VNet ASN       : 65010
- Azure VNet BGP IP 1  : 10.12.255.4 for tunnel too40.112.190.5
- Azure VNet BGP IP 2  : 10.12.255.5 for tunnel too138.91.156.129
- Static routes        : Destination 10.12.255.4/32, nexthop hello VPN tunnel interface too40.112.190.5
                         Destination 10.12.255.5/32, nexthop hello VPN tunnel interface too138.91.156.129
- eBGP Multihop        : Ensure hello "multihop" option for eBGP is enabled on your device if needed
```

<span data-ttu-id="b04fd-210">Miután hello (alagutak) van kapcsolat, meg kell kettős redundáns VPN-eszközök és a helyszíni hálózat és az Azure csatlakozás alagutak:</span><span class="sxs-lookup"><span data-stu-id="b04fd-210">Once hello connection (tunnels) are established, you will have dual redundant VPN devices and tunnels connecting your on-premises network and Azure:</span></span>

![kettős-redundancia-crossprem](./media/vpn-gateway-activeactive-rm-powershell/dual-redundancy.png)

## <span data-ttu-id="b04fd-212"><a name ="aav2v"></a>3. rész – egy aktív-aktív VNet – VNet-kapcsolatot létesíteni</span><span class="sxs-lookup"><span data-stu-id="b04fd-212"><a name ="aav2v"></a>Part 3 - Establish an active-active VNet-to-VNet connection</span></span>
<span data-ttu-id="b04fd-213">Ez a szakasz egy aktív-aktív VNet – VNet-kapcsolatot hoz létre a BGP.</span><span class="sxs-lookup"><span data-stu-id="b04fd-213">This section creates an active-active VNet-to-VNet connection with BGP.</span></span> 

<span data-ttu-id="b04fd-214">az alábbi utasítások hello hello előző lépéseiből fent felsorolt továbbra is.</span><span class="sxs-lookup"><span data-stu-id="b04fd-214">hello instructions below continue from hello previous steps listed above.</span></span> <span data-ttu-id="b04fd-215">Meg kell adnia a [1. rész](#aagateway) toocreate hello VPN Gateway a BGP és TestVNet1 konfigurálni.</span><span class="sxs-lookup"><span data-stu-id="b04fd-215">You must complete [Part 1](#aagateway) toocreate and configure TestVNet1 and hello VPN Gateway with BGP.</span></span> 

### <a name="step-1---create-testvnet2-and-hello-vpn-gateway"></a><span data-ttu-id="b04fd-216">1. lépés – TestVNet2 és hello VPN-átjáró létrehozása</span><span class="sxs-lookup"><span data-stu-id="b04fd-216">Step 1 - Create TestVNet2 and hello VPN gateway</span></span>
<span data-ttu-id="b04fd-217">Fontos, hogy egyetlen virtuális hálózat tartományt nem átfedésben hello IP-címtér hello új virtuális hálózat TestVNet2, toomake.</span><span class="sxs-lookup"><span data-stu-id="b04fd-217">It is important toomake sure that hello IP address space of hello new virtual network, TestVNet2, does not overlap with any of your VNet ranges.</span></span>

<span data-ttu-id="b04fd-218">Ebben a példában a virtuális hálózatok hello toohello tartozik ugyanahhoz az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="b04fd-218">In this example, hello virtual networks belong toohello same subscription.</span></span> <span data-ttu-id="b04fd-219">Beállíthatja a VNet – VNet kapcsolatokhoz különböző előfizetések; Tekintse meg a túl[VNet – VNet-kapcsolatot konfiguráló](vpn-gateway-vnet-vnet-rm-ps.md) toolearn további részletek.</span><span class="sxs-lookup"><span data-stu-id="b04fd-219">You can set up VNet-to-VNet connections between different subscriptions; please refer too[Configure a VNet-to-VNet connection](vpn-gateway-vnet-vnet-rm-ps.md) toolearn more details.</span></span> <span data-ttu-id="b04fd-220">Ellenőrizze, hogy hello "-EnableBgp $True" Ha hello kapcsolatok tooenable BGP létrehozása.</span><span class="sxs-lookup"><span data-stu-id="b04fd-220">Make sure you add hello "-EnableBgp $True" when creating hello connections tooenable BGP.</span></span>

#### <a name="1-declare-your-variables"></a><span data-ttu-id="b04fd-221">1. A változók deklarálása</span><span class="sxs-lookup"><span data-stu-id="b04fd-221">1. Declare your variables</span></span>
<span data-ttu-id="b04fd-222">Lehet, hogy tooreplace hello értékeket hasonlíthatja hello megjeleníteni kívánt toouse a konfigurációhoz.</span><span class="sxs-lookup"><span data-stu-id="b04fd-222">Be sure tooreplace hello values with hello ones that you want toouse for your configuration.</span></span>

```powershell
$RG2 = "TestAARG2"
$Location2 = "East US"
$VNetName2 = "TestVNet2"
$FESubName2 = "FrontEnd"
$BESubName2 = "Backend"
$GWSubName2 = "GatewaySubnet"
$VNetPrefix21 = "10.21.0.0/16"
$VNetPrefix22 = "10.22.0.0/16"
$FESubPrefix2 = "10.21.0.0/24"
$BESubPrefix2 = "10.22.0.0/24"
$GWSubPrefix2 = "10.22.255.0/27"
$VNet2ASN = 65020
$DNS2 = "8.8.8.8"
$GWName2 = "VNet2GW"
$GW2IPName1 = "VNet2GWIP1"
$GW2IPconf1 = "gw2ipconf1"
$GW2IPName2 = "VNet2GWIP2"
$GW2IPconf2 = "gw2ipconf2"
$Connection21 = "VNet2toVNet1"
$Connection12 = "VNet1toVNet2"
```

#### <a name="2-create-testvnet2-in-hello-new-resource-group"></a><span data-ttu-id="b04fd-223">2. TestVNet2 hello új erőforráscsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="b04fd-223">2. Create TestVNet2 in hello new resource group</span></span>

```powershell
New-AzureRmResourceGroup -Name $RG2 -Location $Location2

$fesub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName2 -AddressPrefix $FESubPrefix2
$besub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName2 -AddressPrefix $BESubPrefix2
$gwsub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName2 -AddressPrefix $GWSubPrefix2

New-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2 -Location $Location2 -AddressPrefix $VNetPrefix21,$VNetPrefix22 -Subnet $fesub2,$besub2,$gwsub2
```

#### <a name="3-create-hello-active-active-vpn-gateway-for-testvnet2"></a><span data-ttu-id="b04fd-224">3. TestVNet2 hello aktív-aktív VPN-átjáró létrehozása</span><span class="sxs-lookup"><span data-stu-id="b04fd-224">3. Create hello active-active VPN gateway for TestVNet2</span></span>
<span data-ttu-id="b04fd-225">Kérelem két nyilvános IP-címek toobe lefoglalt toohello átjáró fog létrehozni a virtuális hálózat számára.</span><span class="sxs-lookup"><span data-stu-id="b04fd-225">Request two public IP addresses toobe allocated toohello gateway you will create for your VNet.</span></span> <span data-ttu-id="b04fd-226">Hello alhálózatot és IP-konfiguráció szükséges is fogja definiálni.</span><span class="sxs-lookup"><span data-stu-id="b04fd-226">You'll also define hello subnet and IP configurations required.</span></span>

```powershell
$gw2pip1 = New-AzureRmPublicIpAddress -Name $GW2IPName1 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic
$gw2pip2 = New-AzureRmPublicIpAddress -Name $GW2IPName2 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic

$vnet2 = Get-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
$subnet2 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
$gw2ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW2IPconf1 -Subnet $subnet2 -PublicIpAddress $gw2pip1
$gw2ipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW2IPconf2 -Subnet $subnet2 -PublicIpAddress $gw2pip2
```

<span data-ttu-id="b04fd-227">MINT száma és hello "EnableActiveActiveFeature" jelző hello hello VPN-átjáró hozható létre.</span><span class="sxs-lookup"><span data-stu-id="b04fd-227">Create hello VPN gateway with hello AS number and hello "EnableActiveActiveFeature" flag.</span></span> <span data-ttu-id="b04fd-228">Vegye figyelembe, hogy az Azure VPN gatewayek kell bírálja felül hello alapértelmezett ASN.</span><span class="sxs-lookup"><span data-stu-id="b04fd-228">Note that you must override hello default ASN on your Azure VPN gateways.</span></span> <span data-ttu-id="b04fd-229">hello ASN-eket a hello csatlakoztatott Vnetekhez különböző tooenable BGP és a tranzit útválasztást kell lennie.</span><span class="sxs-lookup"><span data-stu-id="b04fd-229">hello ASNs for hello connected VNets must be different tooenable BGP and transit routing.</span></span>

```powershell
New-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gw2ipconf1,$gw2ipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1 -Asn $VNet2ASN -EnableActiveActiveFeature
```

### <a name="step-2---connect-hello-testvnet1-and-testvnet2-gateways"></a><span data-ttu-id="b04fd-230">2. lépés – hello TestVNet1 és TestVNet2 átjárók csatlakozás</span><span class="sxs-lookup"><span data-stu-id="b04fd-230">Step 2 - Connect hello TestVNet1 and TestVNet2 gateways</span></span>
<span data-ttu-id="b04fd-231">Ebben a példában mindkét átjárók vannak hello ugyanahhoz az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="b04fd-231">In this example, both gateways are in hello same subscription.</span></span> <span data-ttu-id="b04fd-232">Ennek végrehajtásáról a hello ugyanazon PowerShell-munkamenetben.</span><span class="sxs-lookup"><span data-stu-id="b04fd-232">You can complete this step in hello same PowerShell session.</span></span>

#### <a name="1-get-both-gateways"></a><span data-ttu-id="b04fd-233">1. Mindkét átjárók beolvasása</span><span class="sxs-lookup"><span data-stu-id="b04fd-233">1. Get both gateways</span></span>
<span data-ttu-id="b04fd-234">Ellenőrizze, hogy jelentkezzen be, és csatlakozzon a tooSubscription 1.</span><span class="sxs-lookup"><span data-stu-id="b04fd-234">Make sure you log in and connect tooSubscription 1.</span></span>

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
$vnet2gw = Get-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2
```

#### <a name="2-create-both-connections"></a><span data-ttu-id="b04fd-235">2. Mindkét kapcsolatok létrehozása</span><span class="sxs-lookup"><span data-stu-id="b04fd-235">2. Create both connections</span></span>
<span data-ttu-id="b04fd-236">Ebben a lépésben létre fog hozni TestVNet1 tooTestVNet2 hello kapcsolatát, illetve hello kapcsolat TestVNet2 tooTestVNet1 a.</span><span class="sxs-lookup"><span data-stu-id="b04fd-236">In this step, you will create hello connection from TestVNet1 tooTestVNet2, and hello connection from TestVNet2 tooTestVNet1.</span></span>

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True
```

> [!IMPORTANT]
> <span data-ttu-id="b04fd-237">Lehet, hogy tooenable BGP mindkét kapcsolatokhoz.</span><span class="sxs-lookup"><span data-stu-id="b04fd-237">Be sure tooenable BGP for BOTH connections.</span></span>
> 
> 

<span data-ttu-id="b04fd-238">A lépések elvégzése után hello kapcsolat fog létrehozni, néhány perc múlva, és hello BGP társviszony-létesítési munkamenetet kell mentése hello VNet – VNet-kapcsolatot kettős redundanciával befejezése után:</span><span class="sxs-lookup"><span data-stu-id="b04fd-238">After completing these steps, hello connection will be establish in a few minutes, and hello BGP peering session will be up once hello VNet-to-VNet connection is completed with dual redundancy:</span></span>

![aktív-aktív-v2v](./media/vpn-gateway-activeactive-rm-powershell/vnet-to-vnet.png)

## <span data-ttu-id="b04fd-240"><a name ="aaupdate"></a>4. rész - frissítés meglévő átjáró aktív-aktív és aktív-készenléti állapotban lévő között</span><span class="sxs-lookup"><span data-stu-id="b04fd-240"><a name ="aaupdate"></a>Part 4 - Update existing gateway between active-active and active-standby</span></span>
<span data-ttu-id="b04fd-241">hello utolsó részből megtudhatja, hogyan konfigurálhat egy meglévő Azure VPN gateway aktív tooactive aktív készenléti, vagy fordítva.</span><span class="sxs-lookup"><span data-stu-id="b04fd-241">hello last section will describe how you can configure an existing Azure VPN gateway from active-standby tooactive-active mode, or vice versa.</span></span>

> [!NOTE]
> <span data-ttu-id="b04fd-242">Ez a szakasz egy már létrehozott VPN-átjáró a szabványos tooHighPerformance hello lépéseket tooresize egy örökölt Termékváltozat (régi SKU) tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="b04fd-242">This section includes hello steps tooresize a legacy SKU (old SKU) of an already created VPN gateway from Standard tooHighPerformance.</span></span> <span data-ttu-id="b04fd-243">Ezeket a lépéseket ne frissítse egy régi örökölt SKU tooone hello az új SKU.</span><span class="sxs-lookup"><span data-stu-id="b04fd-243">These steps do not upgrade an old legacy SKU tooone of hello new SKUs.</span></span>
> 
> 

### <a name="configure-an-active-standby-gateway-tooactive-active-gateway"></a><span data-ttu-id="b04fd-244">Egy aktív-készenléti állapotban lévő tooactive aktív átjárók konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b04fd-244">Configure an active-standby gateway tooactive-active gateway</span></span>
#### <a name="1-gateway-parameters"></a><span data-ttu-id="b04fd-245">1. Átjáró paraméterek</span><span class="sxs-lookup"><span data-stu-id="b04fd-245">1. Gateway parameters</span></span>
<span data-ttu-id="b04fd-246">a következő példa hello egy aktív-készenléti állapotban lévő átjáró alakít át egy aktív-aktív átjárót.</span><span class="sxs-lookup"><span data-stu-id="b04fd-246">hello following example converts an active-standby gateway into an active-active gateway.</span></span> <span data-ttu-id="b04fd-247">Toocreate kell egy másik nyilvános IP-címet, majd adja hozzá a második átjáró IP-konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="b04fd-247">You need toocreate another public IP address, then add a second Gateway IP configuration.</span></span> <span data-ttu-id="b04fd-248">Az alábbi hello paramétereket használni:</span><span class="sxs-lookup"><span data-stu-id="b04fd-248">Below shows hello parameters used:</span></span>

```powershell
$GWName = "TestVNetAA1GW"
$VNetName = "TestVNetAA1"
$RG = "TestVPNActiveActive01"
$GWIPName2 = "gwpip2"
$GWIPconf2 = "gw1ipconf2"

$vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
$subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
$gw = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
$location = $gw.Location
```

#### <a name="2-create-hello-public-ip-address-then-add-hello-second-gateway-ip-configuration"></a><span data-ttu-id="b04fd-249">2. Hello nyilvános IP-cím létrehozása, és vegye fel a hello második átjáró IP-konfiguráció</span><span class="sxs-lookup"><span data-stu-id="b04fd-249">2. Create hello public IP address, then add hello second gateway IP configuration</span></span>

```powershell
$gwpip2 = New-AzureRmPublicIpAddress -Name $GWIPName2 -ResourceGroupName $RG -Location $location -AllocationMethod Dynamic
Add-AzureRmVirtualNetworkGatewayIpConfig -VirtualNetworkGateway $gw -Name $GWIPconf2 -Subnet $subnet -PublicIpAddress $gwpip2
```

#### <a name="3-enable-active-active-mode-and-update-hello-gateway"></a><span data-ttu-id="b04fd-250">3. Aktív-aktív mód és a frissítés hello átjáró engedélyezése</span><span class="sxs-lookup"><span data-stu-id="b04fd-250">3. Enable active-active mode and update hello gateway</span></span>
<span data-ttu-id="b04fd-251">Meg kell adni hello átjáró objektum PowerShell tootrigger hello tényleges frissítés.</span><span class="sxs-lookup"><span data-stu-id="b04fd-251">You must set hello gateway object in PowerShell tootrigger hello actual update.</span></span> <span data-ttu-id="b04fd-252">hello hello virtuális hálózati átjáró Termékváltozata is módosítani kell (átméretezett) tooHighPerformance szabványként korábban létrehozása óta.</span><span class="sxs-lookup"><span data-stu-id="b04fd-252">hello SKU of hello virtual network gateway must also be changed (resized) tooHighPerformance since it was created previously as Standard.</span></span>

```powershell
Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -EnableActiveActiveFeature -GatewaySku HighPerformance
```

<span data-ttu-id="b04fd-253">A frissítés too45 30 percet is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="b04fd-253">This update can take 30 too45 minutes.</span></span>

### <a name="configure-an-active-active-gateway-tooactive-standby-gateway"></a><span data-ttu-id="b04fd-254">Egy aktív-aktív tooactive-készenléti átjárók konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b04fd-254">Configure an active-active gateway tooactive-standby gateway</span></span>
#### <a name="1-gateway-parameters"></a><span data-ttu-id="b04fd-255">1. Átjáró paraméterek</span><span class="sxs-lookup"><span data-stu-id="b04fd-255">1. Gateway parameters</span></span>
<span data-ttu-id="b04fd-256">Használjon hello azonos, a fenti paraméterek hello IP-konfigurációt kívánja hello nevének beolvasása tooremove.</span><span class="sxs-lookup"><span data-stu-id="b04fd-256">Use hello same parameters as above, get hello name of hello IP configuration you want tooremove.</span></span>

```powershell
$GWName = "TestVNetAA1GW"
$RG = "TestVPNActiveActive01"

$gw = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
$ipconfname = $gw.IpConfigurations[1].Name
```

#### <a name="2-remove-hello-gateway-ip-configuration-and-disable-hello-active-active-mode"></a><span data-ttu-id="b04fd-257">2. Távolítsa el a hello átjáró IP-konfigurációt, és hello aktív-aktív mód letiltása</span><span class="sxs-lookup"><span data-stu-id="b04fd-257">2. Remove hello gateway IP configuration and disable hello active-active mode</span></span>
<span data-ttu-id="b04fd-258">Hasonlóképpen be kell állítani a hello átjáró objektum PowerShell tootrigger hello tényleges frissítés.</span><span class="sxs-lookup"><span data-stu-id="b04fd-258">Similarly, you must set hello gateway object in PowerShell tootrigger hello actual update.</span></span>

```powershell
Remove-AzureRmVirtualNetworkGatewayIpConfig -Name $ipconfname -VirtualNetworkGateway $gw
Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -DisableActiveActiveFeature
```

<span data-ttu-id="b04fd-259">A frissítés be too30 túl 45 percig is tarthat.</span><span class="sxs-lookup"><span data-stu-id="b04fd-259">This update can take up too30 too 45 minutes.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b04fd-260">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b04fd-260">Next steps</span></span>
<span data-ttu-id="b04fd-261">Ha a kapcsolat befejeződött, a virtuális gépek tooyour virtuális hálózatok is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="b04fd-261">Once your connection is complete, you can add virtual machines tooyour virtual networks.</span></span> <span data-ttu-id="b04fd-262">A lépésekért lásd: [Virtuális gép létrehozása](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b04fd-262">See [Create a Virtual Machine](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) for steps.</span></span>
