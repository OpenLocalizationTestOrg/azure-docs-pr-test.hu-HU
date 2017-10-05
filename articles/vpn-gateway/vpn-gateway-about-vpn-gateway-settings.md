---
title: "VPN-átjáró beállításokat a létesítmények közötti Azure-kapcsolatok |} Microsoft Docs"
description: "További tudnivalók az Azure virtuális hálózati átjárók VPN-átjáró beállításainak."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: ae665bc5-0089-45d0-a0d5-bc0ab4e79899
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/26/2017
ms.author: cherylmc
ms.openlocfilehash: 07aa6946b9c3994c5afc5c88837f23567b95d8a5
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="about-vpn-gateway-configuration-settings"></a><span data-ttu-id="ed315-103">VPN-átjáró konfigurációs beállításairól</span><span class="sxs-lookup"><span data-stu-id="ed315-103">About VPN Gateway configuration settings</span></span>

<span data-ttu-id="ed315-104">VPN-átjáró olyan virtuális hálózati átjáró által a titkosított forgalmat a virtuális hálózat és a helyszíni hely közötti nyilvános kapcsolaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="ed315-104">A VPN gateway is a type of virtual network gateway that sends encrypted traffic between your virtual network and your on-premises location across a public connection.</span></span> <span data-ttu-id="ed315-105">VPN-átjáró segítségével virtuális hálózatok közötti Azure gerincét között forgalmat küldeni.</span><span class="sxs-lookup"><span data-stu-id="ed315-105">You can also use a VPN gateway to send traffic between virtual networks across the Azure backbone.</span></span>

<span data-ttu-id="ed315-106">VPN gateway-kapcsolattal konfigurálható beállításokat tartalmaz, amelyek mindegyike több erőforrás konfigurációjának támaszkodik.</span><span class="sxs-lookup"><span data-stu-id="ed315-106">A VPN gateway connection relies on the configuration of multiple resources, each of which contains configurable settings.</span></span> <span data-ttu-id="ed315-107">Ebben a cikkben a szakaszok tárgyalják az erőforrások és a Resource Manager üzembe helyezési modellel létrehozott virtuális hálózat VPN-átjáró szoftverközponthoz kapcsolódó beállításokat.</span><span class="sxs-lookup"><span data-stu-id="ed315-107">The sections in this article discuss the resources and settings that relate to a VPN gateway for a virtual network created in Resource Manager deployment model.</span></span> <span data-ttu-id="ed315-108">Minden kapcsolat megoldás megtalálhatja leírásokat és topológiai diagramot a [VPN-átjáró](vpn-gateway-about-vpngateways.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="ed315-108">You can find descriptions and topology diagrams for each connection solution in the [About VPN Gateway](vpn-gateway-about-vpngateways.md) article.</span></span>

## <span data-ttu-id="ed315-109"><a name="gwtype"></a>Átjáró típusa</span><span class="sxs-lookup"><span data-stu-id="ed315-109"><a name="gwtype"></a>Gateway types</span></span>

<span data-ttu-id="ed315-110">Minden egyes virtuális hálózati csak egy virtuális hálózati átjáró típusonkénti van.</span><span class="sxs-lookup"><span data-stu-id="ed315-110">Each virtual network can only have one virtual network gateway of each type.</span></span> <span data-ttu-id="ed315-111">Ha a virtuális hálózati átjáró hoz létre, győződjön meg arról, hogy helyesek-e az átjáró típusa a konfigurációhoz.</span><span class="sxs-lookup"><span data-stu-id="ed315-111">When you are creating a virtual network gateway, you must make sure that the gateway type is correct for your configuration.</span></span>

<span data-ttu-id="ed315-112">A - GatewayType a lehetséges értékek a következők:</span><span class="sxs-lookup"><span data-stu-id="ed315-112">The available values for -GatewayType are:</span></span>

* <span data-ttu-id="ed315-113">Vpn</span><span class="sxs-lookup"><span data-stu-id="ed315-113">Vpn</span></span>
* <span data-ttu-id="ed315-114">ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="ed315-114">ExpressRoute</span></span>

<span data-ttu-id="ed315-115">A VPN-átjáró által igényelt a `-GatewayType` *Vpn*.</span><span class="sxs-lookup"><span data-stu-id="ed315-115">A VPN gateway requires the `-GatewayType` *Vpn*.</span></span>

<span data-ttu-id="ed315-116">Példa:</span><span class="sxs-lookup"><span data-stu-id="ed315-116">Example:</span></span>

```powershell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
-Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn `
-VpnType RouteBased
```

## <span data-ttu-id="ed315-117"><a name="gwsku"></a>Átjáró-termékváltozatok</span><span class="sxs-lookup"><span data-stu-id="ed315-117"><a name="gwsku"></a>Gateway SKUs</span></span>

[!INCLUDE [vpn-gateway-gwsku-include](../../includes/vpn-gateway-gwsku-include.md)]

### <a name="configure-the-gateway-sku"></a><span data-ttu-id="ed315-118">Az átjáró-Termékváltozat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ed315-118">Configure the gateway SKU</span></span>

#### <a name="azure-portal"></a><span data-ttu-id="ed315-119">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="ed315-119">Azure portal</span></span>

<span data-ttu-id="ed315-120">Ha egy erőforrás-kezelő virtuális hálózati átjáró létrehozásához használhatja az Azure-portálon, válassza a legördülő lista használatával az átjáró-Termékváltozat.</span><span class="sxs-lookup"><span data-stu-id="ed315-120">If you use the Azure portal to create a Resource Manager virtual network gateway, you can select the gateway SKU by using the dropdown.</span></span> <span data-ttu-id="ed315-121">A beállítások, lehetősége lesz az átjáró típusa és a választott VPN-típus felel meg.</span><span class="sxs-lookup"><span data-stu-id="ed315-121">The options you are presented with correspond to the Gateway type and VPN type that you select.</span></span>

#### <a name="powershell"></a><span data-ttu-id="ed315-122">PowerShell</span><span class="sxs-lookup"><span data-stu-id="ed315-122">PowerShell</span></span>

<span data-ttu-id="ed315-123">A következő PowerShell-példa meghatározza, hogy a `-GatewaySku` VpnGw1 szerint.</span><span class="sxs-lookup"><span data-stu-id="ed315-123">The following PowerShell example specifies the `-GatewaySku` as VpnGw1.</span></span>

```powershell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
-Location 'West US' -IpConfigurations $gwipconfig -GatewaySku VpnGw1 `
-GatewayType Vpn -VpnType RouteBased
```

#### <span data-ttu-id="ed315-124"><a name="resize"></a>Módosítás (átméretezési) egy átjáró-Termékváltozat</span><span class="sxs-lookup"><span data-stu-id="ed315-124"><a name="resize"></a>Change (resize) a gateway SKU</span></span>

<span data-ttu-id="ed315-125">Ha azt szeretné, és egy nagyobb teljesítményű Termékváltozat az átjáró-Termékváltozat frissítse, használhatja a `Resize-AzureRmVirtualNetworkGateway` PowerShell-parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="ed315-125">If you want to upgrade your gateway SKU to a more powerful SKU, you can use the `Resize-AzureRmVirtualNetworkGateway` PowerShell cmdlet.</span></span> <span data-ttu-id="ed315-126">Az átjáró-Termékváltozat-méretét, ez a parancsmag segítségével is visszaminősítheti.</span><span class="sxs-lookup"><span data-stu-id="ed315-126">You can also downgrade the gateway SKU size using this cmdlet.</span></span>

<span data-ttu-id="ed315-127">A következő PowerShell-példa látható egy átjáró VpnGw2 az átméretezett Termékváltozat.</span><span class="sxs-lookup"><span data-stu-id="ed315-127">The following PowerShell example shows a gateway SKU being resized to VpnGw2.</span></span>

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku VpnGw2
```

## <span data-ttu-id="ed315-128"><a name="connectiontype"></a>Kapcsolat típusai</span><span class="sxs-lookup"><span data-stu-id="ed315-128"><a name="connectiontype"></a>Connection types</span></span>

<span data-ttu-id="ed315-129">A Resource Manager üzembe helyezési modellel minden egyes konfiguráció szükséges egy adott virtuális hálózati átjáró kapcsolattípus.</span><span class="sxs-lookup"><span data-stu-id="ed315-129">In the Resource Manager deployment model, each configuration requires a specific virtual network gateway connection type.</span></span> <span data-ttu-id="ed315-130">A `-ConnectionType` lehetséges Resource Manager PowerShell-értékei a következők:</span><span class="sxs-lookup"><span data-stu-id="ed315-130">The available Resource Manager PowerShell values for `-ConnectionType` are:</span></span>

* <span data-ttu-id="ed315-131">IPsec</span><span class="sxs-lookup"><span data-stu-id="ed315-131">IPsec</span></span>
* <span data-ttu-id="ed315-132">Vnet2Vnet</span><span class="sxs-lookup"><span data-stu-id="ed315-132">Vnet2Vnet</span></span>
* <span data-ttu-id="ed315-133">ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="ed315-133">ExpressRoute</span></span>
* <span data-ttu-id="ed315-134">VPNClient</span><span class="sxs-lookup"><span data-stu-id="ed315-134">VPNClient</span></span>

<span data-ttu-id="ed315-135">A következő PowerShell-példa egy S2S kapcsolatot igényel a kapcsolat típusa létrehozhatunk *IPsec*.</span><span class="sxs-lookup"><span data-stu-id="ed315-135">In the following PowerShell example, we create a S2S connection that requires the connection type *IPsec*.</span></span>

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg `
-Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
-ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
```

## <span data-ttu-id="ed315-136"><a name="vpntype"></a>VPN-típusai</span><span class="sxs-lookup"><span data-stu-id="ed315-136"><a name="vpntype"></a>VPN types</span></span>

<span data-ttu-id="ed315-137">A virtuális hálózati átjáró VPN-átjáró konfigurációt létrehozásakor meg kell adnia egy VPN-típus.</span><span class="sxs-lookup"><span data-stu-id="ed315-137">When you create the virtual network gateway for a VPN gateway configuration, you must specify a VPN type.</span></span> <span data-ttu-id="ed315-138">A VPN-típus az Ön által a létrehozni kívánt kapcsolat topológia függ.</span><span class="sxs-lookup"><span data-stu-id="ed315-138">The VPN type that you choose depends on the connection topology that you want to create.</span></span> <span data-ttu-id="ed315-139">P2S kapcsolatot például egy RouteBased VPN típust igényli.</span><span class="sxs-lookup"><span data-stu-id="ed315-139">For example, a P2S connection requires a RouteBased VPN type.</span></span> <span data-ttu-id="ed315-140">A VPN-típus az Ön által használt hardver is függ.</span><span class="sxs-lookup"><span data-stu-id="ed315-140">A VPN type can also depend on the hardware that you are using.</span></span> <span data-ttu-id="ed315-141">S2S-konfigurációk esetén van szükség, a VPN-eszközön.</span><span class="sxs-lookup"><span data-stu-id="ed315-141">S2S configurations require a VPN device.</span></span> <span data-ttu-id="ed315-142">VPN-eszközök csak egy meghatározott VPN-típus támogatja.</span><span class="sxs-lookup"><span data-stu-id="ed315-142">Some VPN devices only support a certain VPN type.</span></span>

<span data-ttu-id="ed315-143">A VPN-típus választja meg kell felelnie a minden kapcsolat megoldásra vonatkozó követelményeket a szeretne létrehozni.</span><span class="sxs-lookup"><span data-stu-id="ed315-143">The VPN type you select must satisfy all the connection requirements for the solution you want to create.</span></span> <span data-ttu-id="ed315-144">Például, ha szeretne létrehozni egy S2S VPN gateway-kapcsolatot, és az azonos virtuális hálózat P2S VPN gateway-kapcsolattal, használhatja VPN-típus *RouteBased* mert P2S egy RouteBased VPN-típus szükséges.</span><span class="sxs-lookup"><span data-stu-id="ed315-144">For example, if you want to create a S2S VPN gateway connection and a P2S VPN gateway connection for the same virtual network, you would use VPN type *RouteBased* because P2S requires a RouteBased VPN type.</span></span> <span data-ttu-id="ed315-145">Módosítania kell, hogy a VPN-eszköz támogatja-e RouteBased VPN-kapcsolat ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="ed315-145">You would also need to verify that your VPN device supported a RouteBased VPN connection.</span></span> 

<span data-ttu-id="ed315-146">A virtuális hálózati átjáró létrehozása után nem módosíthatja a VPN-típus.</span><span class="sxs-lookup"><span data-stu-id="ed315-146">Once a virtual network gateway has been created, you can't change the VPN type.</span></span> <span data-ttu-id="ed315-147">Ki kell törölni a virtuális hálózati átjárót, és hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="ed315-147">You have to delete the virtual network gateway and create a new one.</span></span> <span data-ttu-id="ed315-148">Kétféle VPN-típus létezik:</span><span class="sxs-lookup"><span data-stu-id="ed315-148">There are two VPN types:</span></span>

[!INCLUDE [vpn-gateway-vpntype](../../includes/vpn-gateway-vpntype-include.md)]

<span data-ttu-id="ed315-149">A következő PowerShell-példa meghatározza, hogy a `-VpnType` , *RouteBased*.</span><span class="sxs-lookup"><span data-stu-id="ed315-149">The following PowerShell example specifies the `-VpnType` as *RouteBased*.</span></span> <span data-ttu-id="ed315-150">Egy átjáró létrehozásakor biztosítania kell, hogy -VpnType megfeleljen a konfigurációnak.</span><span class="sxs-lookup"><span data-stu-id="ed315-150">When you are creating a gateway, you must make sure that the -VpnType is correct for your configuration.</span></span>

```powershell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
-Location 'West US' -IpConfigurations $gwipconfig `
-GatewayType Vpn -VpnType RouteBased
```

## <span data-ttu-id="ed315-151"><a name="requirements"></a>Átjáró követelményei</span><span class="sxs-lookup"><span data-stu-id="ed315-151"><a name="requirements"></a>Gateway requirements</span></span>

[!INCLUDE [vpn-gateway-table-requirements](../../includes/vpn-gateway-table-requirements-include.md)]

## <span data-ttu-id="ed315-152"><a name="gwsub"></a>Átjáró-alhálózatot</span><span class="sxs-lookup"><span data-stu-id="ed315-152"><a name="gwsub"></a>Gateway subnet</span></span>

<span data-ttu-id="ed315-153">VPN-átjáró létrehozása előtt létre kell hoznia egy átjáró-alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="ed315-153">Before you create a VPN gateway, you must create a gateway subnet.</span></span> <span data-ttu-id="ed315-154">Az átjáró alhálózatának az IP-címek, amelyek a virtuális hálózati átjáró virtuális gépeket és szolgáltatásokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="ed315-154">The gateway subnet contains the IP addresses that the virtual network gateway VMs and services use.</span></span> <span data-ttu-id="ed315-155">A virtuális hálózati átjáró létrehozásakor átjáró virtuális gépek az átjáró alhálózatának telepítve és konfigurálva a VPN-átjáró szükséges beállításokat.</span><span class="sxs-lookup"><span data-stu-id="ed315-155">When you create your virtual network gateway, gateway VMs are deployed to the gateway subnet and configured with the required VPN gateway settings.</span></span> <span data-ttu-id="ed315-156">Soha ne telepítenie kell semmi mást (például további virtuális gépek) az átjáró-alhálózathoz.</span><span class="sxs-lookup"><span data-stu-id="ed315-156">You must never deploy anything else (for example, additional VMs) to the gateway subnet.</span></span> <span data-ttu-id="ed315-157">Az átjáró alhálózatának a "GatewaySubnet" nevű kell megfelelően működjön.</span><span class="sxs-lookup"><span data-stu-id="ed315-157">The gateway subnet must be named 'GatewaySubnet' to work properly.</span></span> <span data-ttu-id="ed315-158">Az átjáró alhálózatának elnevezése "GatewaySubnet" lehetővé teszi, hogy tudja, hogy ez az alhálózat, a virtuális hálózati átjáró virtuális gépek és szolgáltatások telepítése Azure.</span><span class="sxs-lookup"><span data-stu-id="ed315-158">Naming the gateway subnet 'GatewaySubnet' lets Azure know that this is the subnet to deploy the virtual network gateway VMs and services to.</span></span>

<span data-ttu-id="ed315-159">Az átjáróalhálózat létrehozásakor meg kell adnia, hogy hány IP-címet tartalmaz az alhálózat.</span><span class="sxs-lookup"><span data-stu-id="ed315-159">When you create the gateway subnet, you specify the number of IP addresses that the subnet contains.</span></span> <span data-ttu-id="ed315-160">Az átjáró alhálózatának IP-címek az átjáró virtuális gépek és az átjáró szolgáltatások foglal le.</span><span class="sxs-lookup"><span data-stu-id="ed315-160">The IP addresses in the gateway subnet are allocated to the gateway VMs and gateway services.</span></span> <span data-ttu-id="ed315-161">Bizonyos konfigurációk több IP-címre, mint a többi szükséges.</span><span class="sxs-lookup"><span data-stu-id="ed315-161">Some configurations require more IP addresses than others.</span></span> <span data-ttu-id="ed315-162">Nézze meg az utasításokat, amelyet szeretne létrehozni, és győződjön meg arról, hogy az átjáró alhálózatának szeretne létrehozni a konfiguráció megfelel ezeknek a követelményeknek.</span><span class="sxs-lookup"><span data-stu-id="ed315-162">Look at the instructions for the configuration that you want to create and verify that the gateway subnet you want to create meets those requirements.</span></span> <span data-ttu-id="ed315-163">Emellett érdemes lehet ellenőrizze, hogy az átjáró-alhálózatot tartalmaz elég IP-cím lehetséges jövőbeli további konfiguráció alkalmazásához.</span><span class="sxs-lookup"><span data-stu-id="ed315-163">Additionally, you may want to make sure your gateway subnet contains enough IP addresses to accommodate possible future additional configurations.</span></span> <span data-ttu-id="ed315-164">Létrehozhat egy átjáró-alhálózat mérete /29 legyen, de javasolt, hogy hozzon létre egy átjáró-alhálózatot /28 vagy nagyobb (/ 28, /27, /26 stb.).</span><span class="sxs-lookup"><span data-stu-id="ed315-164">While you can create a gateway subnet as small as /29, we recommend that you create a gateway subnet of /28 or larger (/28, /27, /26 etc.).</span></span> <span data-ttu-id="ed315-165">Ily módon való hozzáadásakor funkcióit a jövőben nem kell az átjáró szakadjon el, majd törölje és hozza létre újra az átjáró alhálózatának további IP-címek lehetővé.</span><span class="sxs-lookup"><span data-stu-id="ed315-165">That way, if you add functionality in the future, you won't have to tear your gateway, then delete and recreate the gateway subnet to allow for more IP addresses.</span></span>

<span data-ttu-id="ed315-166">A következő erőforrás-kezelő PowerShell-példa bemutatja egy GatewaySubnet nevű átjáró-alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="ed315-166">The following Resource Manager PowerShell example shows a gateway subnet named GatewaySubnet.</span></span> <span data-ttu-id="ed315-167">A CIDR-jelöléssel Meghatározza, hogy egy /27 lehetővé teszi, hogy elegendő IP-címek a jelenleg létező legtöbb konfiguráció látható.</span><span class="sxs-lookup"><span data-stu-id="ed315-167">You can see the CIDR notation specifies a /27, which allows for enough IP addresses for most configurations that currently exist.</span></span>

```powershell
Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/27
```

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

## <span data-ttu-id="ed315-168"><a name="lng"></a>Helyi hálózati átjáró</span><span class="sxs-lookup"><span data-stu-id="ed315-168"><a name="lng"></a>Local network gateways</span></span>

<span data-ttu-id="ed315-169">A VPN-átjáró konfiguráció létrehozásakor a helyi hálózati átjáró gyakran jelöli a helyszíni hely.</span><span class="sxs-lookup"><span data-stu-id="ed315-169">When creating a VPN gateway configuration, the local network gateway often represents your on-premises location.</span></span> <span data-ttu-id="ed315-170">A klasszikus üzembe helyezési modellben a helyi hálózati átjárót a Helyi névvel illettük.</span><span class="sxs-lookup"><span data-stu-id="ed315-170">In the classic deployment model, the local network gateway was referred to as a Local Site.</span></span> 

<span data-ttu-id="ed315-171">Nevezze el a helyi hálózati átjáró, a helyszíni VPN-eszköz nyilvános IP-címét, és adja meg a címelőtagokat a helyszíni hely található.</span><span class="sxs-lookup"><span data-stu-id="ed315-171">You give the local network gateway a name, the public IP address of the on-premises VPN device, and specify the address prefixes that are located on the on-premises location.</span></span> <span data-ttu-id="ed315-172">Azure ellenőrzi, hogy a hálózati forgalom cél címelőtagokat, a konfiguráció a helyi hálózati átjáró megadott olvas, és ennek megfelelően irányítja a csomagokat.</span><span class="sxs-lookup"><span data-stu-id="ed315-172">Azure looks at the destination address prefixes for network traffic, consults the configuration that you have specified for your local network gateway, and routes packets accordingly.</span></span> <span data-ttu-id="ed315-173">Helyi hálózati átjáró VPN gateway-kapcsolatot használó VNet – VNet-konfigurációk is adja meg.</span><span class="sxs-lookup"><span data-stu-id="ed315-173">You also specify local network gateways for VNet-to-VNet configurations that use a VPN gateway connection.</span></span>

<span data-ttu-id="ed315-174">A következő PowerShell-példa létrehoz egy új helyi hálózati átjáró:</span><span class="sxs-lookup"><span data-stu-id="ed315-174">The following PowerShell example creates a new local network gateway:</span></span>

```powershell
New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
-Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.5.51.0/24'
```

<span data-ttu-id="ed315-175">Néha módosítania kell a helyi hálózati átjáró beállításainak.</span><span class="sxs-lookup"><span data-stu-id="ed315-175">Sometimes you need to modify the local network gateway settings.</span></span> <span data-ttu-id="ed315-176">Például ha fel vagy módosít a címtartományt, vagy ha megváltozik-e a VPN-eszköz IP-címét.</span><span class="sxs-lookup"><span data-stu-id="ed315-176">For example, when you add or modify the address range, or if the IP address of the VPN device changes.</span></span> <span data-ttu-id="ed315-177">A klasszikus vnet ezeket a beállításokat a klasszikus portál a helyi hálózatok lapon módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="ed315-177">For a classic VNet, you can change these settings in the classic portal on the Local Networks page.</span></span> <span data-ttu-id="ed315-178">Az erőforrás-kezelő, lásd: [PowerShell használatával a helyi hálózati átjáró beállításainak módosítása](vpn-gateway-modify-local-network-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="ed315-178">For Resource Manager, see [Modify local network gateway settings using PowerShell](vpn-gateway-modify-local-network-gateway.md).</span></span>

## <span data-ttu-id="ed315-179"><a name="resources"></a>REST API-k és a PowerShell-parancsmagok</span><span class="sxs-lookup"><span data-stu-id="ed315-179"><a name="resources"></a>REST APIs and PowerShell cmdlets</span></span>

<span data-ttu-id="ed315-180">További technikai erőforrások és konkrét szintaxis használatával kapcsolatos követelményeket REST API-k, a PowerShell-parancsmagok vagy az Azure CLI VPN Gateway-konfigurációk esetén tekintse meg a következő oldalakon:</span><span class="sxs-lookup"><span data-stu-id="ed315-180">For additional technical resources and specific syntax requirements when using REST APIs, PowerShell cmdlets, or Azure CLI for VPN Gateway configurations, see the following pages:</span></span>

| <span data-ttu-id="ed315-181">**Klasszikus**</span><span class="sxs-lookup"><span data-stu-id="ed315-181">**Classic**</span></span> | <span data-ttu-id="ed315-182">**Resource Manager**</span><span class="sxs-lookup"><span data-stu-id="ed315-182">**Resource Manager**</span></span> |
| --- | --- |
| [<span data-ttu-id="ed315-183">PowerShell</span><span class="sxs-lookup"><span data-stu-id="ed315-183">PowerShell</span></span>](/powershell/module/azure#networking) |[<span data-ttu-id="ed315-184">PowerShell</span><span class="sxs-lookup"><span data-stu-id="ed315-184">PowerShell</span></span>](/powershell/module/azurerm.network#vpn) |
| [<span data-ttu-id="ed315-185">REST API</span><span class="sxs-lookup"><span data-stu-id="ed315-185">REST API</span></span>](https://msdn.microsoft.com/library/jj154113) |[<span data-ttu-id="ed315-186">REST API</span><span class="sxs-lookup"><span data-stu-id="ed315-186">REST API</span></span>](/rest/api/network/virtualnetworkgateways) |
| <span data-ttu-id="ed315-187">Nem támogatott</span><span class="sxs-lookup"><span data-stu-id="ed315-187">Not supported</span></span> | [<span data-ttu-id="ed315-188">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="ed315-188">Azure CLI</span></span>](/cli/azure/network/vnet-gateway)|

## <a name="next-steps"></a><span data-ttu-id="ed315-189">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ed315-189">Next steps</span></span>

<span data-ttu-id="ed315-190">Használható kapcsolat konfigurációkkal kapcsolatos további információkért lásd: [VPN-átjáró](vpn-gateway-about-vpngateways.md).</span><span class="sxs-lookup"><span data-stu-id="ed315-190">For more information about available connection configurations, see [About VPN Gateway](vpn-gateway-about-vpngateways.md).</span></span>