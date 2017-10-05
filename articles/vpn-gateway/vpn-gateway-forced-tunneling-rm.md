---
title: "Az Azure-webhelyek kapcsolatok kényszerített bújtatás konfigurálása: erőforrás-kezelő |} Microsoft Docs"
description: "Hogyan lehet irányítani, vagy \"kényszerített\" minden internetre irányuló forgalomnak a helyszíni helyre."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: cbe58db8-b598-4c9f-ac88-62c865eb8721
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: cherylmc
ms.openlocfilehash: 207c53924863eb51ee369fe46d5ad12fb1905c53
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="configure-forced-tunneling-using-the-azure-resource-manager-deployment-model"></a><span data-ttu-id="44764-103">Kényszerített bújtatás konfigurálása az Azure Resource Manager-alapú üzemi modellel</span><span class="sxs-lookup"><span data-stu-id="44764-103">Configure forced tunneling using the Azure Resource Manager deployment model</span></span>

<span data-ttu-id="44764-104">A kényszerített bújtatás lehetővé teszi az átirányítási vagy "kényszerített" minden internetre irányuló forgalomnak biztonsági másolatot a helyszíni helyre vizsgálati és naplózási pont-pont VPN-alagúton keresztül.</span><span class="sxs-lookup"><span data-stu-id="44764-104">Forced tunneling lets you redirect or "force" all Internet-bound traffic back to your on-premises location via a Site-to-Site VPN tunnel for inspection and auditing.</span></span> <span data-ttu-id="44764-105">Ez az kritikus fontosságú biztonsági előfeltétele annak, hogy a legtöbb vállalati informatikai házirendek.</span><span class="sxs-lookup"><span data-stu-id="44764-105">This is a critical security requirement for most enterprise IT policies.</span></span> <span data-ttu-id="44764-106">Nélkül a kényszerített bújtatás, kötött internetes forgalmat a virtuális gépek Azure-ban mindig traverses Azure hálózati infrastruktúráról közvetlenül kimenő csatlakozik az internethez, a beállítás lehetővé teszi vizsgálja meg, vagy a forgalom naplózása nélkül.</span><span class="sxs-lookup"><span data-stu-id="44764-106">Without forced tunneling, Internet-bound traffic from your VMs in Azure always traverses from Azure network infrastructure directly out to the Internet, without the option to allow you to inspect or audit the traffic.</span></span> <span data-ttu-id="44764-107">Jogosulatlan Internet-hozzáférés is eredményezhet, információfelfedés vagy más típusú biztonsági problémákat.</span><span class="sxs-lookup"><span data-stu-id="44764-107">Unauthorized Internet access can potentially lead to information disclosure or other types of security breaches.</span></span>

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

<span data-ttu-id="44764-108">Ez a cikk bemutatja, hogyan konfigurálása, a kényszerített bújtatás a Resource Manager üzembe helyezési modellel használatával létrehozott virtuális hálózatokat.</span><span class="sxs-lookup"><span data-stu-id="44764-108">This article walks you through configuring forced tunneling for virtual networks created using the Resource Manager deployment model.</span></span> <span data-ttu-id="44764-109">A kényszerített bújtatás PowerShell, nem a portálon keresztül használatával konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="44764-109">Forced tunneling can be configured by using PowerShell, not through the portal.</span></span> <span data-ttu-id="44764-110">Ha azt szeretné, a klasszikus üzembe helyezési modell kényszerített bújtatás konfigurálása, a következő legördülő listából válassza a klasszikus cikk:</span><span class="sxs-lookup"><span data-stu-id="44764-110">If you want to configure forced tunneling for the classic deployment model, select classic article from the following dropdown list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="44764-111">PowerShell – Klasszikus</span><span class="sxs-lookup"><span data-stu-id="44764-111">PowerShell - Classic</span></span>](vpn-gateway-about-forced-tunneling.md)
> * [<span data-ttu-id="44764-112">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="44764-112">PowerShell - Resource Manager</span></span>](vpn-gateway-forced-tunneling-rm.md)
> 
> 

## <a name="about-forced-tunneling"></a><span data-ttu-id="44764-113">Hamarosan kényszerített bújtatás</span><span class="sxs-lookup"><span data-stu-id="44764-113">About forced tunneling</span></span>

<span data-ttu-id="44764-114">A következő ábra bemutatja, hogyan kényszerített bújtatás működik.</span><span class="sxs-lookup"><span data-stu-id="44764-114">The following diagram illustrates how forced tunneling works.</span></span> 

![Alagúthasználat kényszerítése](./media/vpn-gateway-forced-tunneling-rm/forced-tunnel.png)

<span data-ttu-id="44764-116">A fenti példában a Frontend alhálózathoz nem kényszeríti a bújtatott.</span><span class="sxs-lookup"><span data-stu-id="44764-116">In the example above, the Frontend subnet is not forced tunneled.</span></span> <span data-ttu-id="44764-117">A munkaterhelések Frontend alhálózaton fogadja el, és a felhasználói kérésekre válaszol az internetről közvetlenül is.</span><span class="sxs-lookup"><span data-stu-id="44764-117">The workloads in the Frontend subnet can continue to accept and respond to customer requests from the Internet directly.</span></span> <span data-ttu-id="44764-118">A közepes réteg és a háttérkiszolgáló alhálózatok kényszerítve vannak bújtatott.</span><span class="sxs-lookup"><span data-stu-id="44764-118">The Mid-tier and Backend subnets are forced tunneled.</span></span> <span data-ttu-id="44764-119">A kimenő kapcsolatokat ezek két alhálózat-Internet kell kényszerített vagy egy helyszíni hely keresztül az S2S VPN-alagutat átirányítva.</span><span class="sxs-lookup"><span data-stu-id="44764-119">Any outbound connections from these two subnets to the Internet will be forced or redirected back to an on-premises site via one of the S2S VPN tunnels.</span></span>

<span data-ttu-id="44764-120">Ez lehetővé teszi, hogy korlátozza és vizsgálja meg a virtuális gépek Internet-hozzáféréssel, vagy miközben továbbra is a többrétegű szolgáltatás architektúrája szükséges engedélyezése a felhőalapú Azure-szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="44764-120">This allows you to restrict and inspect Internet access from your virtual machines or cloud services in Azure, while continuing to enable your multi-tier service architecture required.</span></span> <span data-ttu-id="44764-121">Ha a virtuális hálózatok egyetlen internetre munkaterhelés, is alkalmazhat a teljes virtuális hálózatok a kényszerített bújtatás.</span><span class="sxs-lookup"><span data-stu-id="44764-121">If there are no Internet-facing workloads in your virtual networks, you also can apply forced tunneling to the entire virtual networks.</span></span>

## <a name="requirements-and-considerations"></a><span data-ttu-id="44764-122">Követelmények és szempontok</span><span class="sxs-lookup"><span data-stu-id="44764-122">Requirements and considerations</span></span>

<span data-ttu-id="44764-123">Az Azure-ban kényszerített bújtatás konfigurálása virtuális hálózati felhasználó által definiált útvonalak keresztül.</span><span class="sxs-lookup"><span data-stu-id="44764-123">Forced tunneling in Azure is configured via virtual network user-defined routes.</span></span> <span data-ttu-id="44764-124">Egy helyszíni hely irányít át forgalmat az Azure VPN gatewayhez alapértelmezett útvonalat kifejezve.</span><span class="sxs-lookup"><span data-stu-id="44764-124">Redirecting traffic to an on-premises site is expressed as a Default Route to the Azure VPN gateway.</span></span> <span data-ttu-id="44764-125">További információ a felhasználó által definiált útválasztást és a virtuális hálózatok: [felhasználó által definiált útvonalak és IP-továbbítás](../virtual-network/virtual-networks-udr-overview.md).</span><span class="sxs-lookup"><span data-stu-id="44764-125">For more information about user-defined routing and virtual networks, see [User-defined routes and IP forwarding](../virtual-network/virtual-networks-udr-overview.md).</span></span>

* <span data-ttu-id="44764-126">Minden egyes virtuális hálózati alhálózat van egy beépített, rendszer-útválasztási táblázatához.</span><span class="sxs-lookup"><span data-stu-id="44764-126">Each virtual network subnet has a built-in, system routing table.</span></span> <span data-ttu-id="44764-127">A rendszer útválasztási táblázatban az útvonalak a következő három csoport rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="44764-127">The system routing table has the following three groups of routes:</span></span>
  
  * <span data-ttu-id="44764-128">**Helyi VNet útvonalak:** közvetlenül és a cél virtuális gépek ugyanabban a virtuális hálózatban.</span><span class="sxs-lookup"><span data-stu-id="44764-128">**Local VNet routes:** Directly to the destination VMs in the same virtual network.</span></span>
  * <span data-ttu-id="44764-129">**A helyi útvonalak:** számára az Azure VPN gateway.</span><span class="sxs-lookup"><span data-stu-id="44764-129">**On-premises routes:** To the Azure VPN gateway.</span></span>
  * <span data-ttu-id="44764-130">**Alapértelmezett útvonal:** közvetlenül kell az internethez.</span><span class="sxs-lookup"><span data-stu-id="44764-130">**Default route:** Directly to the Internet.</span></span> <span data-ttu-id="44764-131">A privát IP-címek nem fedi le az előző két útvonalakat a csomagok megszakadnak.</span><span class="sxs-lookup"><span data-stu-id="44764-131">Packets destined to the private IP addresses not covered by the previous two routes are dropped.</span></span>
* <span data-ttu-id="44764-132">Ez az eljárás adjon hozzá egy alapértelmezett útvonalat, és társíthatja az útvonaltábla útválasztási táblázatot hozhat létre felhasználó által definiált útvonalak (UDR) segítségével a virtuális hálózat alhálózat ezek alhálózatok kényszerített bújtatás engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="44764-132">This procedure uses user-defined routes (UDR) to create a routing table to add a default route, and then associate the routing table to your VNet subnet(s) to enable forced tunneling on those subnets.</span></span>
* <span data-ttu-id="44764-133">A kényszerített bújtatás kell rendelni egy Vnetet, amelynek az útvonalalapú VPN-átjáró.</span><span class="sxs-lookup"><span data-stu-id="44764-133">Forced tunneling must be associated with a VNet that has a route-based VPN gateway.</span></span> <span data-ttu-id="44764-134">Egy "alapértelmezett"nevű hely a létesítmények közötti helyi helyek között a virtuális hálózathoz be kell.</span><span class="sxs-lookup"><span data-stu-id="44764-134">You need to set a "default site" among the cross-premises local sites connected to the virtual network.</span></span> <span data-ttu-id="44764-135">Emellett a helyszíni VPN-eszköz kell konfigurálni 0.0.0.0/0 forgalom választók használatával.</span><span class="sxs-lookup"><span data-stu-id="44764-135">Also, the on-premises VPN device must be configured using 0.0.0.0/0 as traffic selectors.</span></span> 
* <span data-ttu-id="44764-136">A kényszerített bújtatás ExpressRoute a mechanizmus révén nincs konfigurálva, de ehelyett szerint engedélyezve van egy alapértelmezett útvonalat hirdet a ExpressRoute BGP társviszony-létesítési munkameneteket keresztül.</span><span class="sxs-lookup"><span data-stu-id="44764-136">ExpressRoute forced tunneling is not configured via this mechanism, but instead, is enabled by advertising a default route via the ExpressRoute BGP peering sessions.</span></span> <span data-ttu-id="44764-137">További információkért lásd: a [ExpressRoute dokumentációja](https://azure.microsoft.com/documentation/services/expressroute/).</span><span class="sxs-lookup"><span data-stu-id="44764-137">For more information, see the [ExpressRoute Documentation](https://azure.microsoft.com/documentation/services/expressroute/).</span></span>

## <a name="configuration-overview"></a><span data-ttu-id="44764-138">Konfigurálása – áttekintés</span><span class="sxs-lookup"><span data-stu-id="44764-138">Configuration overview</span></span>

<span data-ttu-id="44764-139">Az alábbi eljárás segítségével hozhat létre, az erőforráscsoportot és egy virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="44764-139">The following procedure helps you create a resource group and a VNet.</span></span> <span data-ttu-id="44764-140">Majd hozhat létre a VPN-átjáró, és konfigurálja a kényszerített bújtatást.</span><span class="sxs-lookup"><span data-stu-id="44764-140">You'll then create a VPN gateway and configure forced tunneling.</span></span> <span data-ttu-id="44764-141">Ezzel az eljárással a virtuális hálózat MultiTier – VNet három alhálózatok rendelkezik: "Előtér", "Midtier" és "Háttér", a négy létesítmények közötti kapcsolatok: "DefaultSiteHQ", és három ágak.</span><span class="sxs-lookup"><span data-stu-id="44764-141">In this procedure, the virtual network 'MultiTier-VNet' has three subnets: 'Frontend', 'Midtier', and 'Backend', with four cross-premises connections: 'DefaultSiteHQ', and three Branches.</span></span>

<span data-ttu-id="44764-142">Az eljárás lépéseit a kényszerített bújtatást, az alapértelmezett hely kapcsolat a "DefaultSiteHQ", és állítja be a "Midtier" és "Háttér" alhálózatok használandó kényszerített bújtatás.</span><span class="sxs-lookup"><span data-stu-id="44764-142">The procedure steps set the 'DefaultSiteHQ' as the default site connection for forced tunneling, and configure the 'Midtier' and 'Backend' subnets to use forced tunneling.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="44764-143">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="44764-143">Before you begin</span></span>

<span data-ttu-id="44764-144">Telepítse az Azure Resource Manager PowerShell-parancsmagjainak legújabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="44764-144">Install the latest version of the Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="44764-145">A PowerShell-parancsmagok telepítésével kapcsolatban további információ: [Az Azure PowerShell telepítése és konfigurálása](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="44764-145">See [How to install and configure Azure PowerShell](/powershell/azure/overview) for more information about installing the PowerShell cmdlets.</span></span>

### <a name="to-log-in"></a><span data-ttu-id="44764-146">A bejelentkezéshez</span><span class="sxs-lookup"><span data-stu-id="44764-146">To log in</span></span>

[!INCLUDE [To log in](../../includes/vpn-gateway-ps-login-include.md)]

## <a name="configure-forced-tunneling"></a><span data-ttu-id="44764-147">Kényszerített bújtatás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="44764-147">Configure forced tunneling</span></span>

1. <span data-ttu-id="44764-148">Hozzon létre egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="44764-148">Create a resource group.</span></span>

  ```powershell
  New-AzureRmResourceGroup -Name 'ForcedTunneling' -Location 'North Europe'
  ```
2. <span data-ttu-id="44764-149">Hozzon létre egy virtuális hálózatot, és adja meg az alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="44764-149">Create a virtual network and specify subnets.</span></span>

  ```powershell 
  $s1 = New-AzureRmVirtualNetworkSubnetConfig -Name "Frontend" -AddressPrefix "10.1.0.0/24"
  $s2 = New-AzureRmVirtualNetworkSubnetConfig -Name "Midtier" -AddressPrefix "10.1.1.0/24"
  $s3 = New-AzureRmVirtualNetworkSubnetConfig -Name "Backend" -AddressPrefix "10.1.2.0/24"
  $s4 = New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix "10.1.200.0/28"
  $vnet = New-AzureRmVirtualNetwork -Name "MultiTier-VNet" -Location "North Europe" -ResourceGroupName "ForcedTunneling" -AddressPrefix "10.1.0.0/16" -Subnet $s1,$s2,$s3,$s4
  ```
3. <span data-ttu-id="44764-150">Helyi hálózati átjáró létrehozása.</span><span class="sxs-lookup"><span data-stu-id="44764-150">Create the local network gateways.</span></span>

  ```powershell
  $lng1 = New-AzureRmLocalNetworkGateway -Name "DefaultSiteHQ" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.111" -AddressPrefix "192.168.1.0/24"
  $lng2 = New-AzureRmLocalNetworkGateway -Name "Branch1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.112" -AddressPrefix "192.168.2.0/24"
  $lng3 = New-AzureRmLocalNetworkGateway -Name "Branch2" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.113" -AddressPrefix "192.168.3.0/24"
  $lng4 = New-AzureRmLocalNetworkGateway -Name "Branch3" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.114" -AddressPrefix "192.168.4.0/24"
  ```
4. <span data-ttu-id="44764-151">Az útvonaltábla és útvonal-szabály létrehozása.</span><span class="sxs-lookup"><span data-stu-id="44764-151">Create the route table and route rule.</span></span>

  ```powershell
  New-AzureRmRouteTable –Name "MyRouteTable" -ResourceGroupName "ForcedTunneling" –Location "North Europe"
  $rt = Get-AzureRmRouteTable –Name "MyRouteTable" -ResourceGroupName "ForcedTunneling" 
  Add-AzureRmRouteConfig -Name "DefaultRoute" -AddressPrefix "0.0.0.0/0" -NextHopType VirtualNetworkGateway -RouteTable $rt
  Set-AzureRmRouteTable -RouteTable $rt
  ```
5. <span data-ttu-id="44764-152">A Midtier és a háttérkiszolgáló alhálózatokra útvonaltábla társítani.</span><span class="sxs-lookup"><span data-stu-id="44764-152">Associate the route table to the Midtier and Backend subnets.</span></span>

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -Name "MultiTier-Vnet" -ResourceGroupName "ForcedTunneling"
  Set-AzureRmVirtualNetworkSubnetConfig -Name "MidTier" -VirtualNetwork $vnet -AddressPrefix "10.1.1.0/24" -RouteTable $rt
  Set-AzureRmVirtualNetworkSubnetConfig -Name "Backend" -VirtualNetwork $vnet -AddressPrefix "10.1.2.0/24" -RouteTable $rt
  Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```
6. <span data-ttu-id="44764-153">Hozzon létre egy alapértelmezett webhelyet az átjárót.</span><span class="sxs-lookup"><span data-stu-id="44764-153">Create the Gateway with a default site.</span></span> <span data-ttu-id="44764-154">Ebben a lépésben egy ideig, néha 45 perc vagy több, mert a létrehozandó és az átjáró konfigurálása vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="44764-154">This step takes some time to complete, sometimes 45 minutes or more, because you are creating and configuring the gateway.</span></span><br> <span data-ttu-id="44764-155">A **- GatewayDefaultSite** a parancsmag paraméter, amely lehetővé teszi, hogy működjenek, ezért ügyeljen arra, ezt a beállítást megfelelően konfigurálja a kényszerített útválasztási konfigurációja.</span><span class="sxs-lookup"><span data-stu-id="44764-155">The **-GatewayDefaultSite** is the cmdlet parameter that allows the forced routing configuration to work, so take care to configure this setting properly.</span></span> <span data-ttu-id="44764-156">Ez a paraméter értéke a PowerShell 1.0-s vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="44764-156">This parameter is available in PowerShell 1.0 or later.</span></span>

  ```powershell
  $pip = New-AzureRmPublicIpAddress -Name "GatewayIP" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -AllocationMethod Dynamic
  $gwsubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
  $ipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name "gwIpConfig" -SubnetId $gwsubnet.Id -PublicIpAddressId $pip.Id
  New-AzureRmVirtualNetworkGateway -Name "Gateway1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -IpConfigurations $ipconfig -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1 -GatewayDefaultSite $lng1 -EnableBgp $false
  ```
7. <span data-ttu-id="44764-157">A telephelyek közötti VPN-kapcsolatok létesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="44764-157">Establish the Site-to-Site VPN connections.</span></span>

  ```powershell
  $gateway = Get-AzureRmVirtualNetworkGateway -Name "Gateway1" -ResourceGroupName "ForcedTunneling"
  $lng1 = Get-AzureRmLocalNetworkGateway -Name "DefaultSiteHQ" -ResourceGroupName "ForcedTunneling" 
  $lng2 = Get-AzureRmLocalNetworkGateway -Name "Branch1" -ResourceGroupName "ForcedTunneling" 
  $lng3 = Get-AzureRmLocalNetworkGateway -Name "Branch2" -ResourceGroupName "ForcedTunneling" 
  $lng4 = Get-AzureRmLocalNetworkGateway -Name "Branch3" -ResourceGroupName "ForcedTunneling" 
    
  New-AzureRmVirtualNetworkGatewayConnection -Name "Connection1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng1 -ConnectionType IPsec -SharedKey "preSharedKey"
  New-AzureRmVirtualNetworkGatewayConnection -Name "Connection2" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng2 -ConnectionType IPsec -SharedKey "preSharedKey"
  New-AzureRmVirtualNetworkGatewayConnection -Name "Connection3" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng3 -ConnectionType IPsec -SharedKey "preSharedKey"
  New-AzureRmVirtualNetworkGatewayConnection -Name "Connection4" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng4 -ConnectionType IPsec -SharedKey "preSharedKey"
    
  Get-AzureRmVirtualNetworkGatewayConnection -Name "Connection1" -ResourceGroupName "ForcedTunneling"
  ```