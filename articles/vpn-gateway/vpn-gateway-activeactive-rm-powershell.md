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
ms.openlocfilehash: a9f71b566ffdb163f95634835f64589a700d712f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="configure-active-active-s2s-vpn-connections-with-azure-vpn-gateways"></a><span data-ttu-id="9bcb5-103">Az Azure VPN Gatewayek aktív-aktív S2S VPN-kapcsolatok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9bcb5-103">Configure active-active S2S VPN connections with Azure VPN Gateways</span></span>

<span data-ttu-id="9bcb5-104">Ez a cikk végigvezeti a aktív-aktív létesítmények közötti és VNet – VNet kapcsolatokhoz a Resource Manager üzembe helyezési modellben és a PowerShell használatával létrehozni.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-104">This article walks you through the steps to create active-active cross-premises and VNet-to-VNet connections using the Resource Manager deployment model and PowerShell.</span></span>

## <a name="about-highly-available-cross-premises-connections"></a><span data-ttu-id="9bcb5-105">Magas rendelkezésre állású létesítmények közötti kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="9bcb5-105">About Highly Available Cross-Premises Connections</span></span>
<span data-ttu-id="9bcb5-106">A létesítmények közötti és VNet – VNet-kapcsolatot a magas rendelkezésre állás eléréséhez kell több VPN-átjáró telepítése, és a hálózatok és az Azure közötti több párhuzamos kapcsolatok létesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-106">To achieve high availability for cross-premises and VNet-to-VNet connectivity, you should deploy multiple VPN gateways and establish multiple parallel connections between your networks and Azure.</span></span> <span data-ttu-id="9bcb5-107">Ellenőrizze a [magas rendelkezésre álló létesítmények közötti és VNet – VNet-kapcsolatot](vpn-gateway-highlyavailable.md) kapcsolati lehetőségek és topológiájának áttekintését.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-107">Please see [Highly Available Cross-Premises and VNet-to-VNet Connectivity](vpn-gateway-highlyavailable.md) for an overview of connectivity options and topology.</span></span>

<span data-ttu-id="9bcb5-108">Ez a cikk leírja egy aktív-aktív létesítmények közötti VPN-kapcsolat és két virtuális hálózatok közötti aktív-aktív kapcsolat beállításához:</span><span class="sxs-lookup"><span data-stu-id="9bcb5-108">This article provides the instructions to set up an active-active cross-premises VPN connection, and active-active connection between two virtual networks:</span></span>

* [<span data-ttu-id="9bcb5-109">1. rész – létrehozása és konfigurálása az Azure VPN gateway aktív-aktív módban</span><span class="sxs-lookup"><span data-stu-id="9bcb5-109">Part 1 - Create and configure your Azure VPN gateway in active-active mode</span></span>](#aagateway)
* [<span data-ttu-id="9bcb5-110">2. rész – aktív-aktív létesítmények közötti kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-110">Part 2 - Establish active-active cross-premises connections</span></span>](#aacrossprem)
* [<span data-ttu-id="9bcb5-111">3. rész – aktív-aktív VNet – VNet-kapcsolatot létrehozni</span><span class="sxs-lookup"><span data-stu-id="9bcb5-111">Part 3 - Establish active-active VNet-to-VNet connections</span></span>](#aav2v)
* [<span data-ttu-id="9bcb5-112">4. rész - frissítés meglévő átjáró aktív-aktív és aktív-készenléti állapotban lévő között</span><span class="sxs-lookup"><span data-stu-id="9bcb5-112">Part 4 - Update existing gateway between active-active and active-standby</span></span>](#aaupdate)

<span data-ttu-id="9bcb5-113">Ezek együtt egy összetettebb, magas rendelkezésre állású hálózati topológia az igényeinek megfelelő létrehozásához kombinálhatja.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-113">You can combine these together to build a more complex, highly available network topology that meets your needs.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9bcb5-114">Vegye figyelembe, hogy a az aktív-aktív mód csak a következő termékváltozatok használja:</span><span class="sxs-lookup"><span data-stu-id="9bcb5-114">Please note that the active-active mode uses only the following SKUs:</span></span> 
  * <span data-ttu-id="9bcb5-115">VpnGw1, VpnGw2, VpnGw3</span><span class="sxs-lookup"><span data-stu-id="9bcb5-115">VpnGw1, VpnGw2, VpnGw3</span></span>
  * <span data-ttu-id="9bcb5-116">(A régi örökölt SKU) HighPerformance</span><span class="sxs-lookup"><span data-stu-id="9bcb5-116">HighPerformance (for old legacy SKUs)</span></span>
> 
> 

## <span data-ttu-id="9bcb5-117"><a name ="aagateway"></a>1. rész – létrehozása és aktív-aktív VPN-átjárók konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9bcb5-117"><a name ="aagateway"></a>Part 1 - Create and configure active-active VPN gateways</span></span>
<span data-ttu-id="9bcb5-118">Az alábbi lépéseket az Azure VPN gateway állít be aktív-aktív módot.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-118">The following steps will configure your Azure VPN gateway in active-active modes.</span></span> <span data-ttu-id="9bcb5-119">Az aktív-aktív és az aktív-készenléti állapotban lévő átjárók közötti legfőbb különbségek:</span><span class="sxs-lookup"><span data-stu-id="9bcb5-119">The key differences between the active-active and active-standby gateways:</span></span>

* <span data-ttu-id="9bcb5-120">Két nyilvános IP-címekkel rendelkező két átjáró IP-konfigurációk létrehozásához szükséges</span><span class="sxs-lookup"><span data-stu-id="9bcb5-120">You need to create two Gateway IP configurations with two public IP addresses</span></span>
* <span data-ttu-id="9bcb5-121">Be kell állítania a EnableActiveActiveFeature jelzőt</span><span class="sxs-lookup"><span data-stu-id="9bcb5-121">You need set the EnableActiveActiveFeature flag</span></span>
* <span data-ttu-id="9bcb5-122">Az átjáró-Termékváltozat VpnGw1, VpnGw2, VpnGw3 vagy HighPerformance (örökölt SKU) kell lennie.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-122">The gateway SKU must be VpnGw1, VpnGw2, VpnGw3, or HighPerformance (legacy SKU).</span></span>

<span data-ttu-id="9bcb5-123">A többi tulajdonság ugyanazok, mint a nem aktív-aktív átjárókat.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-123">The other properties are the same as the non-active-active gateways.</span></span> 

### <a name="before-you-begin"></a><span data-ttu-id="9bcb5-124">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="9bcb5-124">Before you begin</span></span>
* <span data-ttu-id="9bcb5-125">Győződjön meg arról, hogy rendelkezik Azure-előfizetéssel.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-125">Verify that you have an Azure subscription.</span></span> <span data-ttu-id="9bcb5-126">Ha még nincs Azure-előfizetése, aktiválhatja [MSDN-előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), vagy regisztrálhat egy [ingyenes fiókot](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9bcb5-126">If you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or sign up for a [free account](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="9bcb5-127">Előfordulhat, hogy telepítenie kell az Azure Resource Manager PowerShell-parancsmagjait.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-127">You'll need to install the Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="9bcb5-128">Lásd: [áttekintés az Azure PowerShell](/powershell/azure/overview) a PowerShell-parancsmagok telepítéséről további információt.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-128">See [Overview of Azure PowerShell](/powershell/azure/overview) for more information about installing the PowerShell cmdlets.</span></span>

### <a name="step-1---create-and-configure-vnet1"></a><span data-ttu-id="9bcb5-129">1. lépés – létrehozása és VNet1 konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9bcb5-129">Step 1 - Create and configure VNet1</span></span>
#### <a name="1-declare-your-variables"></a><span data-ttu-id="9bcb5-130">1. A változók deklarálása</span><span class="sxs-lookup"><span data-stu-id="9bcb5-130">1. Declare your variables</span></span>
<span data-ttu-id="9bcb5-131">Ezt a gyakorlatot a változók deklarálásával kezdjük.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-131">For this exercise, we'll start by declaring our variables.</span></span> <span data-ttu-id="9bcb5-132">Az alábbi példa a gyakorlathoz használt értékekkel deklarálja a változókat.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-132">The example below declares the variables using the values for this exercise.</span></span> <span data-ttu-id="9bcb5-133">Az éles konfigurációhoz ne felejtse el ezeket az értékeket a saját értékeire cserélni.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-133">Be sure to replace the values with your own when configuring for production.</span></span> <span data-ttu-id="9bcb5-134">Ezeket a változókat akkor használhatja, ha azért hajtja végre a lépéseket, hogy megismerje ezt a konfigurációtípust.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-134">You can use these variables if you are running through the steps to become familiar with this type of configuration.</span></span> <span data-ttu-id="9bcb5-135">Módosítsa a változókat, majd másolja és illessze be őket a PowerShell-konzolra.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-135">Modify the variables, and then copy and paste into your PowerShell console.</span></span>

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

#### <a name="2-connect-to-your-subscription-and-create-a-new-resource-group"></a><span data-ttu-id="9bcb5-136">2. Csatlakozás az előfizetéshez, és hozzon létre egy új erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="9bcb5-136">2. Connect to your subscription and create a new resource group</span></span>
<span data-ttu-id="9bcb5-137">A Resource Manager parancsmagjainak használatához váltson át PowerShell módba.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-137">Make sure you switch to PowerShell mode to use the Resource Manager cmdlets.</span></span> <span data-ttu-id="9bcb5-138">További információ: [A Windows PowerShell használata a Resource Managerrel](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="9bcb5-138">For more information, see [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

<span data-ttu-id="9bcb5-139">Nyissa meg a PowerShell konzolt, és csatlakozzon a fiókjához.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-139">Open your PowerShell console and connect to your account.</span></span> <span data-ttu-id="9bcb5-140">A következő minta segíthet a kapcsolódásban:</span><span class="sxs-lookup"><span data-stu-id="9bcb5-140">Use the following sample to help you connect:</span></span>

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $Sub1
New-AzureRmResourceGroup -Name $RG1 -Location $Location1
```

#### <a name="3-create-testvnet1"></a><span data-ttu-id="9bcb5-141">3. A TestVNet1 létrehozása</span><span class="sxs-lookup"><span data-stu-id="9bcb5-141">3. Create TestVNet1</span></span>
<span data-ttu-id="9bcb5-142">Az alábbi minta létrehoz egy TestVNet1 nevű virtuális hálózatot és három alhálózatot, amelyek neve a következő: GatewaySubnet, FrontEnd és Backend.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-142">The sample below creates a virtual network named TestVNet1 and three subnets, one called GatewaySubnet, one called FrontEnd, and one called Backend.</span></span> <span data-ttu-id="9bcb5-143">Az értékek behelyettesítésekor fontos, hogy az átjáróalhálózat neve mindenképp GatewaySubnet legyen.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-143">When substituting values, it's important that you always name your gateway subnet specifically GatewaySubnet.</span></span> <span data-ttu-id="9bcb5-144">Ha ezt másként nevezi el, az átjáró létrehozása meghiúsul.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-144">If you name it something else, your gateway creation will fail.</span></span>

```powershell
$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
$besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
$gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1
```

### <a name="step-2---create-the-vpn-gateway-for-testvnet1-with-active-active-mode"></a><span data-ttu-id="9bcb5-145">2. lépés – a VPN-átjáró létrehozása TestVNet1 aktív-aktív üzemmódban</span><span class="sxs-lookup"><span data-stu-id="9bcb5-145">Step 2 - Create the VPN gateway for TestVNet1 with active-active mode</span></span>
#### <a name="1-create-the-public-ip-addresses-and-gateway-ip-configurations"></a><span data-ttu-id="9bcb5-146">1. A nyilvános IP-címek és átjáró IP-konfigurációk létrehozása</span><span class="sxs-lookup"><span data-stu-id="9bcb5-146">1. Create the public IP addresses and gateway IP configurations</span></span>
<span data-ttu-id="9bcb5-147">Kérelem osztható ki a Vnethez tartozó létrehozhat az átjáró két nyilvános IP-címeket.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-147">Request two public IP addresses to be allocated to the gateway you will create for your VNet.</span></span> <span data-ttu-id="9bcb5-148">Az alhálózat és a szükséges IP-konfigurációk is fogja definiálni.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-148">You'll also define the subnet and IP configurations required.</span></span>

```powershell
$gw1pip1 = New-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
$gw1pip2 = New-AzureRmPublicIpAddress -Name $GW1IPName2 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic

$vnet1 = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
$subnet1 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
$gw1ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf1 -Subnet $subnet1 -PublicIpAddress $gw1pip1
$gw1ipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf2 -Subnet $subnet1 -PublicIpAddress $gw1pip2
```

#### <a name="2-create-the-vpn-gateway-with-active-active-configuration"></a><span data-ttu-id="9bcb5-149">2. A VPN-átjáró aktív-aktív konfiguráció létrehozása</span><span class="sxs-lookup"><span data-stu-id="9bcb5-149">2. Create the VPN gateway with active-active configuration</span></span>
<span data-ttu-id="9bcb5-150">Hozza létre a TestVNet1 virtuális hálózati átjáróját.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-150">Create the virtual network gateway for TestVNet1.</span></span> <span data-ttu-id="9bcb5-151">Vegye figyelembe, hogy két GatewayIpConfig bejegyzést is tartalmaz, és a EnableActiveActiveFeature jelző be van állítva.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-151">Note that there are two GatewayIpConfig entries, and the EnableActiveActiveFeature flag is set.</span></span> <span data-ttu-id="9bcb5-152">Az átjáró létrehozása akár 45 percet vagy hosszabb időt is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-152">Creating a gateway can take a while (45 minutes or more to complete).</span></span>

```powershell
New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gw1ipconf1,$gw1ipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1 -Asn $VNet1ASN -EnableActiveActiveFeature -Debug
```

#### <a name="3-obtain-the-gateway-public-ip-addresses-and-the-bgp-peer-ip-address"></a><span data-ttu-id="9bcb5-153">3. Az átjáró nyilvános IP-címek és a BGP-Társgép IP-cím beszerzése</span><span class="sxs-lookup"><span data-stu-id="9bcb5-153">3. Obtain the gateway public IP addresses and the BGP Peer IP address</span></span>
<span data-ttu-id="9bcb5-154">Az átjáró létrehozása után szüksége lesz az Azure VPN Gateway a BGP-Társgép IP-cím beszerzése.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-154">Once the gateway is created, you will need to obtain the BGP Peer IP address on the Azure VPN Gateway.</span></span> <span data-ttu-id="9bcb5-155">Ez a cím konfigurálása az Azure VPN Gateway a BGP-partner a helyszíni VPN-eszközök esetében van szükség.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-155">This address is needed to configure the Azure VPN Gateway as a BGP Peer for your on-premises VPN devices.</span></span>

```powershell
$gw1pip1 = Get-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1
$gw1pip2 = Get-AzureRmPublicIpAddress -Name $GW1IPName2 -ResourceGroupName $RG1
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
```

<span data-ttu-id="9bcb5-156">Az alábbi parancsmagok segítségével a VPN-átjáró és az egyes átjárópéldány megfelelő BGP-Társgép IP-címeit számára lefoglalt két nyilvános IP-címe:</span><span class="sxs-lookup"><span data-stu-id="9bcb5-156">Use the following cmdlets to show the two public IP addresses allocated for your VPN gateway, and their corresponding BGP Peer IP addresses for each gateway instance:</span></span>

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

<span data-ttu-id="9bcb5-157">A nyilvános IP sorrendjének szünteti meg a példány, és a megfelelő BGP társviszony-létesítés címek megegyeznek.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-157">The order of the public IP addresses for the gateway instances and the corresponding BGP Peering Addresses are the same.</span></span> <span data-ttu-id="9bcb5-158">Ebben a példában az átjáró nyilvános IP-címe 40.112.190.5 rendelkező virtuális gépet fog használni a BGP társviszony-létesítés cím 10.12.255.4, és az átjáró 138.91.156.129 10.12.255.5 fogja használni.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-158">In this example, the gateway VM with public IP of 40.112.190.5 will use 10.12.255.4 as its BGP Peering Address, and the gateway with 138.91.156.129 will use 10.12.255.5.</span></span> <span data-ttu-id="9bcb5-159">Ezek az információk szükségesek a az aktív-aktív átjáró csatlakozik a helyi VPN-eszközök beállításához.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-159">This information is needed when you set up your on premises VPN devices connecting to the active-active gateway.</span></span> <span data-ttu-id="9bcb5-160">Az átjáró összes címre alatt az ábrán látható:</span><span class="sxs-lookup"><span data-stu-id="9bcb5-160">The gateway is shown in the diagram below with all addresses:</span></span>

![aktív-aktív átjáró](./media/vpn-gateway-activeactive-rm-powershell/active-active-gw.png)

<span data-ttu-id="9bcb5-162">Az átjáró létrehozása után az átjáró segítségével aktív-aktív létesítmények közötti és VNet – VNet-kapcsolatot létesíteni.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-162">Once the gateway is created, you can use this gateway to establish active-active cross-premises or VNet-to-VNet connection.</span></span> <span data-ttu-id="9bcb5-163">A következő szakaszok haladhat végig a lépéseken a gyakorlatban befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-163">The following sections will walk through the steps to complete the exercise.</span></span>

## <span data-ttu-id="9bcb5-164"><a name ="aacrossprem"></a>2. rész – egy aktív-aktív létesítmények közötti kapcsolat</span><span class="sxs-lookup"><span data-stu-id="9bcb5-164"><a name ="aacrossprem"></a>Part 2 - Establish an active-active cross-premises connection</span></span>
<span data-ttu-id="9bcb5-165">A létesítmények közötti kapcsolatot szüksége a helyszíni VPN-eszköz képviselő helyi hálózati átjáró és a kapcsolat az Azure VPN gateway kapcsolódni a helyi hálózati átjáró létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-165">To establish a cross-premises connection, you need to create a Local Network Gateway to represent your on-premises VPN device, and a Connection to connect the Azure VPN gateway with the local network gateway.</span></span> <span data-ttu-id="9bcb5-166">Ebben a példában az Azure VPN gateway aktív-aktív módban van.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-166">In this example, the Azure VPN gateway is in active-active mode.</span></span> <span data-ttu-id="9bcb5-167">Ennek eredményeképpen ellenére, hogy csak az egyiket a helyszíni VPN-eszközön (helyi hálózati átjáró) és egy kapcsolati erőforrást, mindkét Azure VPN gateway példányok fog létrehozni a S2S VPN-alagutat a helyszíni eszközök.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-167">As a result, even though there is only one on-premises VPN device (local network gateway) and one connection resource, both Azure VPN gateway instances will establish S2S VPN tunnels with the on-premises device.</span></span>

<span data-ttu-id="9bcb5-168">A folytatás előtt ellenőrizze, hogy befejeződött [1. rész](#aagateway) ebben a gyakorlatban.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-168">Before proceeding, please make sure you have completed [Part 1](#aagateway) of this exercise.</span></span>

### <a name="step-1---create-and-configure-the-local-network-gateway"></a><span data-ttu-id="9bcb5-169">1. lépés – létrehozása és a helyi hálózati átjáró konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9bcb5-169">Step 1 - Create and configure the local network gateway</span></span>
#### <a name="1-declare-your-variables"></a><span data-ttu-id="9bcb5-170">1. A változók deklarálása</span><span class="sxs-lookup"><span data-stu-id="9bcb5-170">1. Declare your variables</span></span>
<span data-ttu-id="9bcb5-171">Ebben a gyakorlatban továbbra is a konfiguráció az ábrán is látható.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-171">This exercise will continue to build the configuration shown in the diagram.</span></span> <span data-ttu-id="9bcb5-172">Ne felejtse el az értékeket olyanokra cserélni, amelyeket a saját konfigurációjához kíván használni.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-172">Be sure to replace the values with the ones that you want to use for your configuration.</span></span>

```powershell
$RG5 = "TestAARG5"
$Location5 = "West US"
$LNGName51 = "Site5_1"
$LNGPrefix51 = "10.52.255.253/32"
$LNGIP51 = "131.107.72.22"
$LNGASN5 = 65050
$BGPPeerIP51 = "10.52.255.253"
```

<span data-ttu-id="9bcb5-173">Több helyi hálózati átjáró paraméterek kapcsolatban ügyeljen a következőkre:</span><span class="sxs-lookup"><span data-stu-id="9bcb5-173">A couple of things to note regarding the local network gateway parameters:</span></span>

* <span data-ttu-id="9bcb5-174">A helyi hálózati átjáró lehet az ugyanazon vagy másik helyen és az erőforráscsoport a VPN-átjáróként.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-174">The local network gateway can be in the same or different location and resource group as the VPN gateway.</span></span> <span data-ttu-id="9bcb5-175">Ez a példa bemutatja azokat a különböző erőforráscsoportokra, de az azonos Azure-hely.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-175">This example shows them in different resource groups but in the same Azure location.</span></span>
* <span data-ttu-id="9bcb5-176">Ha csak egy helyszíni VPN-eszköz a fentiek szerint, az aktív-aktív kapcsolat együttműködhet, függetlenül a BGP protokollt.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-176">If there is only one on-premises VPN device as shown above, the active-active connection can work with or without BGP protocol.</span></span> <span data-ttu-id="9bcb5-177">A példa BGP a létesítmények közötti kapcsolathoz.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-177">This example uses BGP for the cross-premises connection.</span></span>
* <span data-ttu-id="9bcb5-178">A BGP engedélyezve van, a előtagot kell deklarálni helyi hálózati átjáró esetén az állomás címe a VPN-eszköz a BGP-Társgép IP-cím.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-178">If BGP is enabled, the prefix you need to declare for the local network gateway is the host address of your BGP Peer IP address on your VPN device.</span></span> <span data-ttu-id="9bcb5-179">Egy /32 ebben az esetben a "10.52.255.253/32" előtaggal.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-179">In this case, it's a /32 prefix of "10.52.255.253/32".</span></span>
* <span data-ttu-id="9bcb5-180">Ne feledje a helyszíni hálózatokhoz és az Azure VNet közötti különböző BGP ASN kell használnia.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-180">As a reminder, you must use different BGP ASNs between your on-premises networks and Azure VNet.</span></span> <span data-ttu-id="9bcb5-181">Ha a egyeznek, a virtuális hálózat ASN módosításához, ha a helyszíni VPN-eszköz már használja az ASN a más BGP szomszédok egyenrangú szüksége.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-181">If they are the same, you need to change your VNet ASN if your on-premises VPN device already uses the ASN to peer with other BGP neighbors.</span></span>

#### <a name="2-create-the-local-network-gateway-for-site5"></a><span data-ttu-id="9bcb5-182">2. A helyi hálózati átjáró létrehozása Site5</span><span class="sxs-lookup"><span data-stu-id="9bcb5-182">2. Create the local network gateway for Site5</span></span>
<span data-ttu-id="9bcb5-183">Mielőtt folytatja, győződjön meg arról, hogy továbbra is csatlakozik az 1. előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-183">Before you continue, please make sure you are still connected to Subscription 1.</span></span> <span data-ttu-id="9bcb5-184">Az erőforráscsoport létrehozása, ha az még nem jött létre.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-184">Create the resource group if it is not yet created.</span></span>

```powershell
New-AzureRmResourceGroup -Name $RG5 -Location $Location5
New-AzureRmLocalNetworkGateway -Name $LNGName51 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP51 -AddressPrefix $LNGPrefix51 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP51
```

### <a name="step-2---connect-the-vnet-gateway-and-local-network-gateway"></a><span data-ttu-id="9bcb5-185">2. lépés – a virtuális hálózat átjáró és a helyi hálózati átjáró</span><span class="sxs-lookup"><span data-stu-id="9bcb5-185">Step 2 - Connect the VNet gateway and local network gateway</span></span>
#### <a name="1-get-the-two-gateways"></a><span data-ttu-id="9bcb5-186">1. A két átjáró beolvasása</span><span class="sxs-lookup"><span data-stu-id="9bcb5-186">1. Get the two gateways</span></span>

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng5gw1 = Get-AzureRmLocalNetworkGateway  -Name $LNGName51 -ResourceGroupName $RG5
```

#### <a name="2-create-the-testvnet1-to-site5-connection"></a><span data-ttu-id="9bcb5-187">2. A TestVNet1 Site5 kapcsolat létrehozása</span><span class="sxs-lookup"><span data-stu-id="9bcb5-187">2. Create the TestVNet1 to Site5 connection</span></span>
<span data-ttu-id="9bcb5-188">Ebben a lépésben hoz létre a kapcsolat a TestVNet1 való Site5_1 a "EnableBGP" $True értékre.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-188">In this step, you will create the connection from TestVNet1 to Site5_1 with "EnableBGP" set to $True.</span></span>

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name $Connection151 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw1 -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP True
```

#### <a name="3-vpn-and-bgp-parameters-for-your-on-premises-vpn-device"></a><span data-ttu-id="9bcb5-189">3. A helyszíni VPN-eszköz VPN és BGP paraméterei</span><span class="sxs-lookup"><span data-stu-id="9bcb5-189">3. VPN and BGP parameters for your on-premises VPN device</span></span>
<span data-ttu-id="9bcb5-190">Az alábbi példa az ebben a gyakorlatban a helyszíni VPN-eszköz a BGP konfigurációs szakaszba módba lép paramétereket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="9bcb5-190">The example below lists the parameters you will enter into the BGP configuration section on your on-premises VPN device for this exercise:</span></span>

    - <span data-ttu-id="9bcb5-191">Site5 ASN: 65050</span><span class="sxs-lookup"><span data-stu-id="9bcb5-191">Site5 ASN            : 65050</span></span>
    - <span data-ttu-id="9bcb5-192">BGP-IP-Site5: 10.52.255.253</span><span class="sxs-lookup"><span data-stu-id="9bcb5-192">Site5 BGP IP         : 10.52.255.253</span></span>
    - <span data-ttu-id="9bcb5-193">Értesítés előtagok: (példa) 10.51.0.0/16 és 10.52.0.0/16</span><span class="sxs-lookup"><span data-stu-id="9bcb5-193">Prefixes to announce : (for example) 10.51.0.0/16 and 10.52.0.0/16</span></span>
    - <span data-ttu-id="9bcb5-194">Az Azure VNet ASN: 65010</span><span class="sxs-lookup"><span data-stu-id="9bcb5-194">Azure VNet ASN       : 65010</span></span>
    - <span data-ttu-id="9bcb5-195">Az Azure VNet BGP-IP-1: 10.12.255.4 az alagutat a 40.112.190.5</span><span class="sxs-lookup"><span data-stu-id="9bcb5-195">Azure VNet BGP IP 1  : 10.12.255.4 for tunnel to 40.112.190.5</span></span>
    - <span data-ttu-id="9bcb5-196">Az Azure VNet BGP IP 2: 10.12.255.5 az alagutat a 138.91.156.129</span><span class="sxs-lookup"><span data-stu-id="9bcb5-196">Azure VNet BGP IP 2  : 10.12.255.5 for tunnel to 138.91.156.129</span></span>
    - <span data-ttu-id="9bcb5-197">Statikus útvonal: cél 10.12.255.4/32, a következő ugrás a VPN-alagút csatoló 40.112.190.5 a cél 10.12.255.5/32, a következő ugrás 138.91.156.129 a csatoló a VPN-alagút</span><span class="sxs-lookup"><span data-stu-id="9bcb5-197">Static routes        : Destination 10.12.255.4/32, nexthop the VPN tunnel interface to 40.112.190.5                        Destination 10.12.255.5/32, nexthop the VPN tunnel interface to 138.91.156.129</span></span>
    - <span data-ttu-id="9bcb5-198">Többszörös ugrási eBGP: az ebgp-t az eszközön engedélyezve van, ha szükséges, győződjön meg arról, hogy a "Többszörös ugrási" lehetőséget</span><span class="sxs-lookup"><span data-stu-id="9bcb5-198">eBGP Multihop        : Ensure the "multihop" option for eBGP is enabled on your device if needed</span></span>

<span data-ttu-id="9bcb5-199">A kapcsolatot kell kialakítani, néhány perc múlva, és a BGP társviszony-létesítési munkamenetet IPsec-kapcsolat létrejötte után indul el.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-199">The connection should be established after a few minutes, and the BGP peering session will start once the IPsec connection is established.</span></span> <span data-ttu-id="9bcb5-200">Ebben a példában, amennyiben csak egy helyszíni VPN-eszköz, ami azt eredményezi, az alábbi ábrán van beállítva:</span><span class="sxs-lookup"><span data-stu-id="9bcb5-200">This example so far has configured only one on-premises VPN device, resulting in the diagram shown below:</span></span>

![aktív-aktív-crossprem](./media/vpn-gateway-activeactive-rm-powershell/active-active.png)

### <a name="step-3---connect-two-on-premises-vpn-devices-to-the-active-active-vpn-gateway"></a><span data-ttu-id="9bcb5-202">3. lépés - a két helyszíni VPN-eszközök csatlakozni az aktív-aktív VPN-átjáró</span><span class="sxs-lookup"><span data-stu-id="9bcb5-202">Step 3 - Connect two on-premises VPN devices to the active-active VPN gateway</span></span>
<span data-ttu-id="9bcb5-203">Ha két VPN-eszközök, a helyi hálózaton, kettős redundancia érhet el, a második VPN-eszköz az Azure VPN gatewayhez csatlakozó.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-203">If you have two VPN devices at the same on-premises network, you can achieve dual redundancy by connecting the Azure VPN gateway to the second VPN device.</span></span>

#### <a name="1-create-the-second-local-network-gateway-for-site5"></a><span data-ttu-id="9bcb5-204">1. A második helyi hálózati átjáró létrehozása Site5</span><span class="sxs-lookup"><span data-stu-id="9bcb5-204">1. Create the second local network gateway for Site5</span></span>
<span data-ttu-id="9bcb5-205">Vegye figyelembe, hogy az átjáró IP-címe, a címelőtagot és a BGP társviszony-létesítési címét, a második helyi hálózati átjáró nem lehet átfedésben az előző helyi hálózati átjáró ugyanabban a helyi hálózaton.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-205">Note that the gateway IP address, address prefix, and BGP peering address for the second local network gateway must not overlap with the previous local network gateway for the same on-premises network.</span></span>

```powershell
$LNGName52 = "Site5_2"
$LNGPrefix52 = "10.52.255.254/32"
$LNGIP52 = "131.107.72.23"
$BGPPeerIP52 = "10.52.255.254"

New-AzureRmLocalNetworkGateway -Name $LNGName52 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP52 -AddressPrefix $LNGPrefix52 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP52
```

#### <a name="2-connect-the-vnet-gateway-and-the-second-local-network-gateway"></a><span data-ttu-id="9bcb5-206">2. Csatlakoztassa a virtuális hálózat átjáró, a második helyi hálózati átjáró</span><span class="sxs-lookup"><span data-stu-id="9bcb5-206">2. Connect the VNet gateway and the second local network gateway</span></span>
<span data-ttu-id="9bcb5-207">Hozza létre a kapcsolatot TestVNet1 Site5_2 a "EnableBGP" $True értékre</span><span class="sxs-lookup"><span data-stu-id="9bcb5-207">Create the connection from TestVNet1 to Site5_2 with "EnableBGP" set to $True</span></span>

```powershell
$lng5gw2 = Get-AzureRmLocalNetworkGateway -Name $LNGName52 -ResourceGroupName $RG5

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection152 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw2 -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP True
```

#### <a name="3-vpn-and-bgp-parameters-for-your-second-on-premises-vpn-device"></a><span data-ttu-id="9bcb5-208">3. A második a helyszíni VPN-eszköz VPN és BGP paraméterei</span><span class="sxs-lookup"><span data-stu-id="9bcb5-208">3. VPN and BGP parameters for your second on-premises VPN device</span></span>
<span data-ttu-id="9bcb5-209">Ehhez hasonlóan listája alább a paraméterek megadja azokat a második VPN-eszköz:</span><span class="sxs-lookup"><span data-stu-id="9bcb5-209">Similarly, below lists the parameters you will enter into the second VPN device:</span></span>

```
- Site5 ASN            : 65050
- Site5 BGP IP         : 10.52.255.254
- Prefixes to announce : (for example) 10.51.0.0/16 and 10.52.0.0/16
- Azure VNet ASN       : 65010
- Azure VNet BGP IP 1  : 10.12.255.4 for tunnel to 40.112.190.5
- Azure VNet BGP IP 2  : 10.12.255.5 for tunnel to 138.91.156.129
- Static routes        : Destination 10.12.255.4/32, nexthop the VPN tunnel interface to 40.112.190.5
                         Destination 10.12.255.5/32, nexthop the VPN tunnel interface to 138.91.156.129
- eBGP Multihop        : Ensure the "multihop" option for eBGP is enabled on your device if needed
```

<span data-ttu-id="9bcb5-210">Miután létrejött a kapcsolat (alagutak) is meg kell kettős redundáns VPN-eszközök és a helyszíni hálózat és az Azure csatlakozás alagutak:</span><span class="sxs-lookup"><span data-stu-id="9bcb5-210">Once the connection (tunnels) are established, you will have dual redundant VPN devices and tunnels connecting your on-premises network and Azure:</span></span>

![kettős-redundancia-crossprem](./media/vpn-gateway-activeactive-rm-powershell/dual-redundancy.png)

## <span data-ttu-id="9bcb5-212"><a name ="aav2v"></a>3. rész – egy aktív-aktív VNet – VNet-kapcsolatot létesíteni</span><span class="sxs-lookup"><span data-stu-id="9bcb5-212"><a name ="aav2v"></a>Part 3 - Establish an active-active VNet-to-VNet connection</span></span>
<span data-ttu-id="9bcb5-213">Ez a szakasz egy aktív-aktív VNet – VNet-kapcsolatot hoz létre a BGP.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-213">This section creates an active-active VNet-to-VNet connection with BGP.</span></span> 

<span data-ttu-id="9bcb5-214">Az alábbi útmutató a fent leírt lépések folytatása.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-214">The instructions below continue from the previous steps listed above.</span></span> <span data-ttu-id="9bcb5-215">Meg kell adnia a [1. rész](#aagateway) létrehozása és konfigurálása a TestVNet1 és a VPN-átjáró BGP-hez.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-215">You must complete [Part 1](#aagateway) to create and configure TestVNet1 and the VPN Gateway with BGP.</span></span> 

### <a name="step-1---create-testvnet2-and-the-vpn-gateway"></a><span data-ttu-id="9bcb5-216">1. lépés – TestVNet2 és a VPN-átjáró létrehozása</span><span class="sxs-lookup"><span data-stu-id="9bcb5-216">Step 1 - Create TestVNet2 and the VPN gateway</span></span>
<span data-ttu-id="9bcb5-217">Győződjön meg arról, hogy az új virtuális hálózat, TestVNet2, az IP-címtér nem fedi át a virtuális hálózat tartomány egyik fontos.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-217">It is important to make sure that the IP address space of the new virtual network, TestVNet2, does not overlap with any of your VNet ranges.</span></span>

<span data-ttu-id="9bcb5-218">Ebben a példában a virtuális hálózatok ugyanahhoz az előfizetéshez tartozik.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-218">In this example, the virtual networks belong to the same subscription.</span></span> <span data-ttu-id="9bcb5-219">Beállíthatja a VNet – VNet kapcsolatokhoz különböző előfizetések; Tekintse meg [VNet – VNet-kapcsolatot konfiguráló](vpn-gateway-vnet-vnet-rm-ps.md) többet is megtudhat.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-219">You can set up VNet-to-VNet connections between different subscriptions; please refer to [Configure a VNet-to-VNet connection](vpn-gateway-vnet-vnet-rm-ps.md) to learn more details.</span></span> <span data-ttu-id="9bcb5-220">Mindenképpen adja hozzá a "-EnableBgp $True" a BGP engedélyezéséhez kapcsolatok létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-220">Make sure you add the "-EnableBgp $True" when creating the connections to enable BGP.</span></span>

#### <a name="1-declare-your-variables"></a><span data-ttu-id="9bcb5-221">1. A változók deklarálása</span><span class="sxs-lookup"><span data-stu-id="9bcb5-221">1. Declare your variables</span></span>
<span data-ttu-id="9bcb5-222">Ne felejtse el az értékeket olyanokra cserélni, amelyeket a saját konfigurációjához kíván használni.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-222">Be sure to replace the values with the ones that you want to use for your configuration.</span></span>

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

#### <a name="2-create-testvnet2-in-the-new-resource-group"></a><span data-ttu-id="9bcb5-223">2. TestVNet2 az új erőforráscsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="9bcb5-223">2. Create TestVNet2 in the new resource group</span></span>

```powershell
New-AzureRmResourceGroup -Name $RG2 -Location $Location2

$fesub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName2 -AddressPrefix $FESubPrefix2
$besub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName2 -AddressPrefix $BESubPrefix2
$gwsub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName2 -AddressPrefix $GWSubPrefix2

New-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2 -Location $Location2 -AddressPrefix $VNetPrefix21,$VNetPrefix22 -Subnet $fesub2,$besub2,$gwsub2
```

#### <a name="3-create-the-active-active-vpn-gateway-for-testvnet2"></a><span data-ttu-id="9bcb5-224">3. Az aktív-aktív VPN-átjáró TestVNet2 létrehozása</span><span class="sxs-lookup"><span data-stu-id="9bcb5-224">3. Create the active-active VPN gateway for TestVNet2</span></span>
<span data-ttu-id="9bcb5-225">Kérelem osztható ki a Vnethez tartozó létrehozhat az átjáró két nyilvános IP-címeket.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-225">Request two public IP addresses to be allocated to the gateway you will create for your VNet.</span></span> <span data-ttu-id="9bcb5-226">Az alhálózat és a szükséges IP-konfigurációk is fogja definiálni.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-226">You'll also define the subnet and IP configurations required.</span></span>

```powershell
$gw2pip1 = New-AzureRmPublicIpAddress -Name $GW2IPName1 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic
$gw2pip2 = New-AzureRmPublicIpAddress -Name $GW2IPName2 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic

$vnet2 = Get-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
$subnet2 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
$gw2ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW2IPconf1 -Subnet $subnet2 -PublicIpAddress $gw2pip1
$gw2ipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW2IPconf2 -Subnet $subnet2 -PublicIpAddress $gw2pip2
```

<span data-ttu-id="9bcb5-227">Hozzon létre a VPN-átjáró AS számát és a "EnableActiveActiveFeature" jelzőt.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-227">Create the VPN gateway with the AS number and the "EnableActiveActiveFeature" flag.</span></span> <span data-ttu-id="9bcb5-228">Vegye figyelembe, hogy az Azure VPN gatewayek kell bírálja felül az alapértelmezett ASN.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-228">Note that you must override the default ASN on your Azure VPN gateways.</span></span> <span data-ttu-id="9bcb5-229">Az ASN-eket a csatlakoztatott Vnetek a BGP és a tranzit Útválasztás engedélyezése különbözőnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-229">The ASNs for the connected VNets must be different to enable BGP and transit routing.</span></span>

```powershell
New-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gw2ipconf1,$gw2ipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1 -Asn $VNet2ASN -EnableActiveActiveFeature
```

### <a name="step-2---connect-the-testvnet1-and-testvnet2-gateways"></a><span data-ttu-id="9bcb5-230">2. lépés - a TestVNet1 és TestVNet2 átjárók csatlakozás</span><span class="sxs-lookup"><span data-stu-id="9bcb5-230">Step 2 - Connect the TestVNet1 and TestVNet2 gateways</span></span>
<span data-ttu-id="9bcb5-231">Ebben a példában két átjáró ugyanahhoz az előfizetéshez vannak.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-231">In this example, both gateways are in the same subscription.</span></span> <span data-ttu-id="9bcb5-232">Ebben a lépésben a PowerShell-munkamenetben hajthatja végre.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-232">You can complete this step in the same PowerShell session.</span></span>

#### <a name="1-get-both-gateways"></a><span data-ttu-id="9bcb5-233">1. Mindkét átjárók beolvasása</span><span class="sxs-lookup"><span data-stu-id="9bcb5-233">1. Get both gateways</span></span>
<span data-ttu-id="9bcb5-234">Jelentkezzen be, és kapcsolódjon az 1. előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-234">Make sure you log in and connect to Subscription 1.</span></span>

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
$vnet2gw = Get-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2
```

#### <a name="2-create-both-connections"></a><span data-ttu-id="9bcb5-235">2. Mindkét kapcsolatok létrehozása</span><span class="sxs-lookup"><span data-stu-id="9bcb5-235">2. Create both connections</span></span>
<span data-ttu-id="9bcb5-236">Ebben a lépésben hoz létre a kapcsolat TestVNet1 és TestVNet2, és a kapcsolat a TestVNet2 TestVNet1 számára.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-236">In this step, you will create the connection from TestVNet1 to TestVNet2, and the connection from TestVNet2 to TestVNet1.</span></span>

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True
```

> [!IMPORTANT]
> <span data-ttu-id="9bcb5-237">Győződjön meg arról, a BGP engedélyezéséhez mindkét kapcsolatokhoz.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-237">Be sure to enable BGP for BOTH connections.</span></span>
> 
> 

<span data-ttu-id="9bcb5-238">Társviszony-létesítési munkamenetet a kapcsolat néhány percet, és a BGP kell meghatározzák ezeket a lépéseket befejezését követően lesz mentése után a VNet – VNet kapcsolódást kettős redundanciával:</span><span class="sxs-lookup"><span data-stu-id="9bcb5-238">After completing these steps, the connection will be establish in a few minutes, and the BGP peering session will be up once the VNet-to-VNet connection is completed with dual redundancy:</span></span>

![aktív-aktív-v2v](./media/vpn-gateway-activeactive-rm-powershell/vnet-to-vnet.png)

## <span data-ttu-id="9bcb5-240"><a name ="aaupdate"></a>4. rész - frissítés meglévő átjáró aktív-aktív és aktív-készenléti állapotban lévő között</span><span class="sxs-lookup"><span data-stu-id="9bcb5-240"><a name ="aaupdate"></a>Part 4 - Update existing gateway between active-active and active-standby</span></span>
<span data-ttu-id="9bcb5-241">Az utolsó szakasza ismerteti, hogyan konfigurálhat egy meglévő Azure VPN-átjárót aktív-készenléti állapotban lévő aktív-aktív módban, vagy fordítva.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-241">The last section will describe how you can configure an existing Azure VPN gateway from active-standby to active-active mode, or vice versa.</span></span>

> [!NOTE]
> <span data-ttu-id="9bcb5-242">Ez a szakasz a lépésekből áll egy örökölt Termékváltozat (régi SKU) átméretezni egy már létrehozott VPN-átjáró a normál HighPerformance.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-242">This section includes the steps to resize a legacy SKU (old SKU) of an already created VPN gateway from Standard to HighPerformance.</span></span> <span data-ttu-id="9bcb5-243">Ezeket a lépéseket ne frissítse egy régi örökölt SKU egy új SKU.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-243">These steps do not upgrade an old legacy SKU to one of the new SKUs.</span></span>
> 
> 

### <a name="configure-an-active-standby-gateway-to-active-active-gateway"></a><span data-ttu-id="9bcb5-244">Egy aktív-készenléti állapotban lévő átjáró aktív-aktív átjáró konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9bcb5-244">Configure an active-standby gateway to active-active gateway</span></span>
#### <a name="1-gateway-parameters"></a><span data-ttu-id="9bcb5-245">1. Átjáró paraméterek</span><span class="sxs-lookup"><span data-stu-id="9bcb5-245">1. Gateway parameters</span></span>
<span data-ttu-id="9bcb5-246">A következő példa egy aktív-készenléti állapotban lévő átjáró alakít át egy aktív-aktív átjárót.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-246">The following example converts an active-standby gateway into an active-active gateway.</span></span> <span data-ttu-id="9bcb5-247">Hozzon létre egy másik nyilvános IP-címet, majd adja hozzá a második átjáró IP-konfiguráció kell.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-247">You need to create another public IP address, then add a second Gateway IP configuration.</span></span> <span data-ttu-id="9bcb5-248">Alább látható használt paraméterek:</span><span class="sxs-lookup"><span data-stu-id="9bcb5-248">Below shows the parameters used:</span></span>

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

#### <a name="2-create-the-public-ip-address-then-add-the-second-gateway-ip-configuration"></a><span data-ttu-id="9bcb5-249">2. A nyilvános IP-cím létrehozása, majd adja hozzá a második átjáró IP-konfiguráció</span><span class="sxs-lookup"><span data-stu-id="9bcb5-249">2. Create the public IP address, then add the second gateway IP configuration</span></span>

```powershell
$gwpip2 = New-AzureRmPublicIpAddress -Name $GWIPName2 -ResourceGroupName $RG -Location $location -AllocationMethod Dynamic
Add-AzureRmVirtualNetworkGatewayIpConfig -VirtualNetworkGateway $gw -Name $GWIPconf2 -Subnet $subnet -PublicIpAddress $gwpip2
```

#### <a name="3-enable-active-active-mode-and-update-the-gateway"></a><span data-ttu-id="9bcb5-250">3. Aktív-aktív mód engedélyezése és az átjáró frissítése</span><span class="sxs-lookup"><span data-stu-id="9bcb5-250">3. Enable active-active mode and update the gateway</span></span>
<span data-ttu-id="9bcb5-251">Meg kell adni az átjáróobjektum a PowerShellben a tényleges frissítés indításához.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-251">You must set the gateway object in PowerShell to trigger the actual update.</span></span> <span data-ttu-id="9bcb5-252">A virtuális hálózati átjáró Termékváltozata is kell változtatni (átméretezett) HighPerformance szabványként korábban létrehozása óta.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-252">The SKU of the virtual network gateway must also be changed (resized) to HighPerformance since it was created previously as Standard.</span></span>

```powershell
Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -EnableActiveActiveFeature -GatewaySku HighPerformance
```

<span data-ttu-id="9bcb5-253">A frissítés a 30-45 percet is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-253">This update can take 30 to 45 minutes.</span></span>

### <a name="configure-an-active-active-gateway-to-active-standby-gateway"></a><span data-ttu-id="9bcb5-254">Egy aktív-aktív átjáró aktív-készenléti állapotban lévő átjáró konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9bcb5-254">Configure an active-active gateway to active-standby gateway</span></span>
#### <a name="1-gateway-parameters"></a><span data-ttu-id="9bcb5-255">1. Átjáró paraméterek</span><span class="sxs-lookup"><span data-stu-id="9bcb5-255">1. Gateway parameters</span></span>
<span data-ttu-id="9bcb5-256">Az ugyanezen paraméterekkel a fenti használatához el az eltávolítani kívánt IP-konfiguráció neve.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-256">Use the same parameters as above, get the name of the IP configuration you want to remove.</span></span>

```powershell
$GWName = "TestVNetAA1GW"
$RG = "TestVPNActiveActive01"

$gw = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
$ipconfname = $gw.IpConfigurations[1].Name
```

#### <a name="2-remove-the-gateway-ip-configuration-and-disable-the-active-active-mode"></a><span data-ttu-id="9bcb5-257">2. Távolítsa el az átjáró IP-konfiguráció és az aktív-aktív mód letiltása</span><span class="sxs-lookup"><span data-stu-id="9bcb5-257">2. Remove the gateway IP configuration and disable the active-active mode</span></span>
<span data-ttu-id="9bcb5-258">Hasonlóképpen be kell állítani a az átjáróobjektum PowerShell indításához az aktuális frissítés.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-258">Similarly, you must set the gateway object in PowerShell to trigger the actual update.</span></span>

```powershell
Remove-AzureRmVirtualNetworkGatewayIpConfig -Name $ipconfname -VirtualNetworkGateway $gw
Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -DisableActiveActiveFeature
```

<span data-ttu-id="9bcb5-259">A frissítés 45 percig is eltarthat 30-ig.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-259">This update can take up to 30 to  45 minutes.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9bcb5-260">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9bcb5-260">Next steps</span></span>
<span data-ttu-id="9bcb5-261">Miután a kapcsolat létrejött, hozzáadhat virtuális gépeket a virtuális hálózataihoz.</span><span class="sxs-lookup"><span data-stu-id="9bcb5-261">Once your connection is complete, you can add virtual machines to your virtual networks.</span></span> <span data-ttu-id="9bcb5-262">A lépésekért lásd: [Virtuális gép létrehozása](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="9bcb5-262">See [Create a Virtual Machine](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) for steps.</span></span>