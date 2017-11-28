---
title: "Az Azure-webhelyek kapcsolatok kényszerített bújtatás konfigurálása: erőforrás-kezelő |} Microsoft Docs"
description: "Hogyan tooredirect vagy \"kényszerített\" az összes internetes adathoz kötött hátsó forgalom-tooyour helyszíni hely."
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
ms.openlocfilehash: 6bc52c04ab0749a674c9863be5e4f9a9f7c98df4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-forced-tunneling-using-hello-azure-resource-manager-deployment-model"></a><span data-ttu-id="33b05-103">Konfigurálja a kényszerített bújtatás használata hello Azure Resource Manager telepítési modell</span><span class="sxs-lookup"><span data-stu-id="33b05-103">Configure forced tunneling using hello Azure Resource Manager deployment model</span></span>

<span data-ttu-id="33b05-104">A kényszerített bújtatás lehetővé átirányítási vagy "kényszerített" minden Internet adathoz kötött forgalom hátsó tooyour helyszíni hely vizsgálati és naplózási pont-pont VPN-alagúton keresztül.</span><span class="sxs-lookup"><span data-stu-id="33b05-104">Forced tunneling lets you redirect or "force" all Internet-bound traffic back tooyour on-premises location via a Site-to-Site VPN tunnel for inspection and auditing.</span></span> <span data-ttu-id="33b05-105">Ez az kritikus fontosságú biztonsági előfeltétele annak, hogy a legtöbb vállalati informatikai házirendek.</span><span class="sxs-lookup"><span data-stu-id="33b05-105">This is a critical security requirement for most enterprise IT policies.</span></span> <span data-ttu-id="33b05-106">Kényszerített bújtatás nélkül internetre irányuló forgalomnak a virtuális gépek Azure-ban mindig halad át közvetlenül kimenő Internet, toohello nélkül hello beállítás tooallow Azure hálózati infrastruktúráról, tooinspect vagy a napló hello forgalmat.</span><span class="sxs-lookup"><span data-stu-id="33b05-106">Without forced tunneling, Internet-bound traffic from your VMs in Azure always traverses from Azure network infrastructure directly out toohello Internet, without hello option tooallow you tooinspect or audit hello traffic.</span></span> <span data-ttu-id="33b05-107">Jogosulatlan Internet-hozzáférés is eredményezhet, tooinformation nyilvánosságra vagy más típusú biztonsági problémákat.</span><span class="sxs-lookup"><span data-stu-id="33b05-107">Unauthorized Internet access can potentially lead tooinformation disclosure or other types of security breaches.</span></span>

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

<span data-ttu-id="33b05-108">Ez a cikk bemutatja, hogyan konfigurálása, a kényszerített bújtatás hello Resource Manager üzembe helyezési modellben használatával létrehozott virtuális hálózatokat.</span><span class="sxs-lookup"><span data-stu-id="33b05-108">This article walks you through configuring forced tunneling for virtual networks created using hello Resource Manager deployment model.</span></span> <span data-ttu-id="33b05-109">A kényszerített bújtatás powershellel hello portálon keresztül nem konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="33b05-109">Forced tunneling can be configured by using PowerShell, not through hello portal.</span></span> <span data-ttu-id="33b05-110">Ha a kényszerített bújtatás hello klasszikus telepítési modell tooconfigure, jelölje be a klasszikus cikk a legördülő listából a következő hello:</span><span class="sxs-lookup"><span data-stu-id="33b05-110">If you want tooconfigure forced tunneling for hello classic deployment model, select classic article from hello following dropdown list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="33b05-111">PowerShell – Klasszikus</span><span class="sxs-lookup"><span data-stu-id="33b05-111">PowerShell - Classic</span></span>](vpn-gateway-about-forced-tunneling.md)
> * [<span data-ttu-id="33b05-112">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="33b05-112">PowerShell - Resource Manager</span></span>](vpn-gateway-forced-tunneling-rm.md)
> 
> 

## <a name="about-forced-tunneling"></a><span data-ttu-id="33b05-113">Hamarosan kényszerített bújtatás</span><span class="sxs-lookup"><span data-stu-id="33b05-113">About forced tunneling</span></span>

<span data-ttu-id="33b05-114">hello a következő ábra bemutatja, hogyan kényszerített bújtatás működik.</span><span class="sxs-lookup"><span data-stu-id="33b05-114">hello following diagram illustrates how forced tunneling works.</span></span> 

![Alagúthasználat kényszerítése](./media/vpn-gateway-forced-tunneling-rm/forced-tunnel.png)

<span data-ttu-id="33b05-116">Hello a fenti példában a Frontend alhálózathoz nem kényszeríti bújtatott hello.</span><span class="sxs-lookup"><span data-stu-id="33b05-116">In hello example above, hello Frontend subnet is not forced tunneled.</span></span> <span data-ttu-id="33b05-117">hello munkaterhelések hello Frontend alhálózaton továbbra is tooaccept és hello Internet toocustomer kérelmek közvetlenül válaszol.</span><span class="sxs-lookup"><span data-stu-id="33b05-117">hello workloads in hello Frontend subnet can continue tooaccept and respond toocustomer requests from hello Internet directly.</span></span> <span data-ttu-id="33b05-118">hello közepes réteg és a háttérkiszolgáló alhálózatok kényszerítve vannak-e bújtatott.</span><span class="sxs-lookup"><span data-stu-id="33b05-118">hello Mid-tier and Backend subnets are forced tunneled.</span></span> <span data-ttu-id="33b05-119">A kimenő kapcsolatokat, az alábbi két alhálózat toohello Internet lesz kényszerített vagy átirányított vissza tooan hely helyszíni S2S VPN-je bújtatja hello keresztül.</span><span class="sxs-lookup"><span data-stu-id="33b05-119">Any outbound connections from these two subnets toohello Internet will be forced or redirected back tooan on-premises site via one of hello S2S VPN tunnels.</span></span>

<span data-ttu-id="33b05-120">Ez lehetővé teszi a toorestrict és vizsgálhatja meg a virtuális gépek Internet-hozzáféréssel, vagy felhőalapú szolgáltatások Azure, miközben továbbra tooenable a többrétegű szolgáltatások architektúra szükséges.</span><span class="sxs-lookup"><span data-stu-id="33b05-120">This allows you toorestrict and inspect Internet access from your virtual machines or cloud services in Azure, while continuing tooenable your multi-tier service architecture required.</span></span> <span data-ttu-id="33b05-121">Ha a virtuális hálózatok egyetlen internetre munkaterhelés, is alkalmazhat a kényszerített bújtatás toohello teljes virtuális hálózatok.</span><span class="sxs-lookup"><span data-stu-id="33b05-121">If there are no Internet-facing workloads in your virtual networks, you also can apply forced tunneling toohello entire virtual networks.</span></span>

## <a name="requirements-and-considerations"></a><span data-ttu-id="33b05-122">Követelmények és szempontok</span><span class="sxs-lookup"><span data-stu-id="33b05-122">Requirements and considerations</span></span>

<span data-ttu-id="33b05-123">Az Azure-ban kényszerített bújtatás konfigurálása virtuális hálózati felhasználó által definiált útvonalak keresztül.</span><span class="sxs-lookup"><span data-stu-id="33b05-123">Forced tunneling in Azure is configured via virtual network user-defined routes.</span></span> <span data-ttu-id="33b05-124">Irányít át forgalmat tooan helyszíni hely alapértelmezett útvonalat toohello Azure VPN-átjáróként van kifejezve.</span><span class="sxs-lookup"><span data-stu-id="33b05-124">Redirecting traffic tooan on-premises site is expressed as a Default Route toohello Azure VPN gateway.</span></span> <span data-ttu-id="33b05-125">További információ a felhasználó által definiált útválasztást és a virtuális hálózatok: [felhasználó által definiált útvonalak és IP-továbbítás](../virtual-network/virtual-networks-udr-overview.md).</span><span class="sxs-lookup"><span data-stu-id="33b05-125">For more information about user-defined routing and virtual networks, see [User-defined routes and IP forwarding](../virtual-network/virtual-networks-udr-overview.md).</span></span>

* <span data-ttu-id="33b05-126">Minden egyes virtuális hálózati alhálózat van egy beépített, rendszer-útválasztási táblázatához.</span><span class="sxs-lookup"><span data-stu-id="33b05-126">Each virtual network subnet has a built-in, system routing table.</span></span> <span data-ttu-id="33b05-127">hello rendszer útvonaltábla rendelkezik a következő három csoport útvonalak hello:</span><span class="sxs-lookup"><span data-stu-id="33b05-127">hello system routing table has hello following three groups of routes:</span></span>
  
  * <span data-ttu-id="33b05-128">**Helyi VNet útvonalak:** közvetlenül toohello cél virtuális gépek hello az azonos virtuális hálózatban.</span><span class="sxs-lookup"><span data-stu-id="33b05-128">**Local VNet routes:** Directly toohello destination VMs in hello same virtual network.</span></span>
  * <span data-ttu-id="33b05-129">**A helyi útvonalak:** toohello Azure VPN gateway.</span><span class="sxs-lookup"><span data-stu-id="33b05-129">**On-premises routes:** toohello Azure VPN gateway.</span></span>
  * <span data-ttu-id="33b05-130">**Alapértelmezett útvonal:** közvetlenül toohello Internet.</span><span class="sxs-lookup"><span data-stu-id="33b05-130">**Default route:** Directly toohello Internet.</span></span> <span data-ttu-id="33b05-131">Eldobott csomagok toohello magánhálózati IP-címek nem fedi le hello előző két útvonalak.</span><span class="sxs-lookup"><span data-stu-id="33b05-131">Packets destined toohello private IP addresses not covered by hello previous two routes are dropped.</span></span>
* <span data-ttu-id="33b05-132">Ezt az eljárást használja a felhasználó által definiált útvonalak (UDR) toocreate útválasztási táblázat tooadd alapértelmezett útvonalat, és társíthatja az útválasztási táblázat tooyour VNet alhálózat tooenable kényszerített bújtatás ezek alhálózatok hello.</span><span class="sxs-lookup"><span data-stu-id="33b05-132">This procedure uses user-defined routes (UDR) toocreate a routing table tooadd a default route, and then associate hello routing table tooyour VNet subnet(s) tooenable forced tunneling on those subnets.</span></span>
* <span data-ttu-id="33b05-133">A kényszerített bújtatás kell rendelni egy Vnetet, amelynek az útvonalalapú VPN-átjáró.</span><span class="sxs-lookup"><span data-stu-id="33b05-133">Forced tunneling must be associated with a VNet that has a route-based VPN gateway.</span></span> <span data-ttu-id="33b05-134">Egy "alapértelmezett"nevű hely tooset kell hello létesítmények közötti helyi helyek csatlakoztatott toohello virtuális hálózat között.</span><span class="sxs-lookup"><span data-stu-id="33b05-134">You need tooset a "default site" among hello cross-premises local sites connected toohello virtual network.</span></span> <span data-ttu-id="33b05-135">Emellett hello a helyszíni VPN-eszköz 0.0.0.0/0 forgalom választók használatával kell konfigurálni.</span><span class="sxs-lookup"><span data-stu-id="33b05-135">Also, hello on-premises VPN device must be configured using 0.0.0.0/0 as traffic selectors.</span></span> 
* <span data-ttu-id="33b05-136">A kényszerített bújtatás ExpressRoute a mechanizmus révén nincs konfigurálva, de ehelyett szerint engedélyezve van egy alapértelmezett útvonalat hirdet keresztül hello ExpressRoute BGP társviszony-létesítési munkameneteket.</span><span class="sxs-lookup"><span data-stu-id="33b05-136">ExpressRoute forced tunneling is not configured via this mechanism, but instead, is enabled by advertising a default route via hello ExpressRoute BGP peering sessions.</span></span> <span data-ttu-id="33b05-137">További információkért lásd: hello [ExpressRoute dokumentációja](https://azure.microsoft.com/documentation/services/expressroute/).</span><span class="sxs-lookup"><span data-stu-id="33b05-137">For more information, see hello [ExpressRoute Documentation](https://azure.microsoft.com/documentation/services/expressroute/).</span></span>

## <a name="configuration-overview"></a><span data-ttu-id="33b05-138">Konfigurálása – áttekintés</span><span class="sxs-lookup"><span data-stu-id="33b05-138">Configuration overview</span></span>

<span data-ttu-id="33b05-139">hello a következő eljárás segítségével hozhat létre, az erőforráscsoportot és egy virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="33b05-139">hello following procedure helps you create a resource group and a VNet.</span></span> <span data-ttu-id="33b05-140">Majd hozhat létre a VPN-átjáró, és konfigurálja a kényszerített bújtatást.</span><span class="sxs-lookup"><span data-stu-id="33b05-140">You'll then create a VPN gateway and configure forced tunneling.</span></span> <span data-ttu-id="33b05-141">Ezzel az eljárással hello virtuális hálózati MultiTier – VNet három alhálózatot tartalmaz: "Előtér", "Midtier" és "Háttér", a négy létesítmények közötti kapcsolatok: "DefaultSiteHQ", és három ágak.</span><span class="sxs-lookup"><span data-stu-id="33b05-141">In this procedure, hello virtual network 'MultiTier-VNet' has three subnets: 'Frontend', 'Midtier', and 'Backend', with four cross-premises connections: 'DefaultSiteHQ', and three Branches.</span></span>

<span data-ttu-id="33b05-142">hello lépésekkel állítsa be "DefaultSiteHQ" hello, hello alapértelmezett webhely kapcsolat a kényszerített bújtatás, és hello "Midtier" és "Háttér" alhálózatok toouse kényszerített bújtatás konfigurálásakor.</span><span class="sxs-lookup"><span data-stu-id="33b05-142">hello procedure steps set hello 'DefaultSiteHQ' as hello default site connection for forced tunneling, and configure hello 'Midtier' and 'Backend' subnets toouse forced tunneling.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="33b05-143">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="33b05-143">Before you begin</span></span>

<span data-ttu-id="33b05-144">Hello hello Azure Resource Manager PowerShell-parancsmagok legújabb verziójának telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="33b05-144">Install hello latest version of hello Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="33b05-145">Lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) hello PowerShell-parancsmagok telepítéséről további információt.</span><span class="sxs-lookup"><span data-stu-id="33b05-145">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) for more information about installing hello PowerShell cmdlets.</span></span>

### <a name="toolog-in"></a><span data-ttu-id="33b05-146">a toolog</span><span class="sxs-lookup"><span data-stu-id="33b05-146">toolog in</span></span>

[!INCLUDE [toolog in](../../includes/vpn-gateway-ps-login-include.md)]

## <a name="configure-forced-tunneling"></a><span data-ttu-id="33b05-147">Kényszerített bújtatás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="33b05-147">Configure forced tunneling</span></span>

1. <span data-ttu-id="33b05-148">Hozzon létre egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="33b05-148">Create a resource group.</span></span>

  ```powershell
  New-AzureRmResourceGroup -Name 'ForcedTunneling' -Location 'North Europe'
  ```
2. <span data-ttu-id="33b05-149">Hozzon létre egy virtuális hálózatot, és adja meg az alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="33b05-149">Create a virtual network and specify subnets.</span></span>

  ```powershell 
  $s1 = New-AzureRmVirtualNetworkSubnetConfig -Name "Frontend" -AddressPrefix "10.1.0.0/24"
  $s2 = New-AzureRmVirtualNetworkSubnetConfig -Name "Midtier" -AddressPrefix "10.1.1.0/24"
  $s3 = New-AzureRmVirtualNetworkSubnetConfig -Name "Backend" -AddressPrefix "10.1.2.0/24"
  $s4 = New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix "10.1.200.0/28"
  $vnet = New-AzureRmVirtualNetwork -Name "MultiTier-VNet" -Location "North Europe" -ResourceGroupName "ForcedTunneling" -AddressPrefix "10.1.0.0/16" -Subnet $s1,$s2,$s3,$s4
  ```
3. <span data-ttu-id="33b05-150">Hello helyi hálózati átjáró létrehozása.</span><span class="sxs-lookup"><span data-stu-id="33b05-150">Create hello local network gateways.</span></span>

  ```powershell
  $lng1 = New-AzureRmLocalNetworkGateway -Name "DefaultSiteHQ" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.111" -AddressPrefix "192.168.1.0/24"
  $lng2 = New-AzureRmLocalNetworkGateway -Name "Branch1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.112" -AddressPrefix "192.168.2.0/24"
  $lng3 = New-AzureRmLocalNetworkGateway -Name "Branch2" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.113" -AddressPrefix "192.168.3.0/24"
  $lng4 = New-AzureRmLocalNetworkGateway -Name "Branch3" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.114" -AddressPrefix "192.168.4.0/24"
  ```
4. <span data-ttu-id="33b05-151">Hello útvonal tábla- és útvonal-szabály létrehozása.</span><span class="sxs-lookup"><span data-stu-id="33b05-151">Create hello route table and route rule.</span></span>

  ```powershell
  New-AzureRmRouteTable –Name "MyRouteTable" -ResourceGroupName "ForcedTunneling" –Location "North Europe"
  $rt = Get-AzureRmRouteTable –Name "MyRouteTable" -ResourceGroupName "ForcedTunneling" 
  Add-AzureRmRouteConfig -Name "DefaultRoute" -AddressPrefix "0.0.0.0/0" -NextHopType VirtualNetworkGateway -RouteTable $rt
  Set-AzureRmRouteTable -RouteTable $rt
  ```
5. <span data-ttu-id="33b05-152">Hello útvonal tábla toohello Midtier és a háttérkiszolgáló alhálózatok társítása.</span><span class="sxs-lookup"><span data-stu-id="33b05-152">Associate hello route table toohello Midtier and Backend subnets.</span></span>

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -Name "MultiTier-Vnet" -ResourceGroupName "ForcedTunneling"
  Set-AzureRmVirtualNetworkSubnetConfig -Name "MidTier" -VirtualNetwork $vnet -AddressPrefix "10.1.1.0/24" -RouteTable $rt
  Set-AzureRmVirtualNetworkSubnetConfig -Name "Backend" -VirtualNetwork $vnet -AddressPrefix "10.1.2.0/24" -RouteTable $rt
  Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```
6. <span data-ttu-id="33b05-153">Hozzon létre egy alapértelmezett webhelyet hello átjáró.</span><span class="sxs-lookup"><span data-stu-id="33b05-153">Create hello Gateway with a default site.</span></span> <span data-ttu-id="33b05-154">Ez a lépés bizonyos idő toocomplete, néha 45 percig vagy tovább vesz igénybe, mert létrehozandó és hello átjáró konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="33b05-154">This step takes some time toocomplete, sometimes 45 minutes or more, because you are creating and configuring hello gateway.</span></span><br> <span data-ttu-id="33b05-155">Hello **- GatewayDefaultSite** , amely lehetővé teszi, hogy hello kényszerített útválasztási konfigurációs toowork parancsmag-paraméterben hello, így telhet, mire gondot tooconfigure megfelelően ezt a beállítást.</span><span class="sxs-lookup"><span data-stu-id="33b05-155">hello **-GatewayDefaultSite** is hello cmdlet parameter that allows hello forced routing configuration toowork, so take care tooconfigure this setting properly.</span></span> <span data-ttu-id="33b05-156">Ez a paraméter értéke a PowerShell 1.0-s vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="33b05-156">This parameter is available in PowerShell 1.0 or later.</span></span>

  ```powershell
  $pip = New-AzureRmPublicIpAddress -Name "GatewayIP" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -AllocationMethod Dynamic
  $gwsubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
  $ipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name "gwIpConfig" -SubnetId $gwsubnet.Id -PublicIpAddressId $pip.Id
  New-AzureRmVirtualNetworkGateway -Name "Gateway1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -IpConfigurations $ipconfig -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1 -GatewayDefaultSite $lng1 -EnableBgp $false
  ```
7. <span data-ttu-id="33b05-157">Pont-pont hello VPN-kapcsolatok létesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="33b05-157">Establish hello Site-to-Site VPN connections.</span></span>

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