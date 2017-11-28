---
title: "aaaVPN átjáró beállításainak létesítmények közötti Azure-kapcsolatok |} Microsoft Docs"
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
ms.openlocfilehash: 01229d99fa37e30e00aa00f939f488d631b5593c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="about-vpn-gateway-configuration-settings"></a><span data-ttu-id="59def-103">VPN-átjáró konfigurációs beállításairól</span><span class="sxs-lookup"><span data-stu-id="59def-103">About VPN Gateway configuration settings</span></span>

<span data-ttu-id="59def-104">VPN-átjáró olyan virtuális hálózati átjáró által a titkosított forgalmat a virtuális hálózat és a helyszíni hely közötti nyilvános kapcsolaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="59def-104">A VPN gateway is a type of virtual network gateway that sends encrypted traffic between your virtual network and your on-premises location across a public connection.</span></span> <span data-ttu-id="59def-105">A VPN-átjáró toosend forgalmát hello Azure gerincét között virtuális hálózatok közötti is használható.</span><span class="sxs-lookup"><span data-stu-id="59def-105">You can also use a VPN gateway toosend traffic between virtual networks across hello Azure backbone.</span></span>

<span data-ttu-id="59def-106">VPN gateway-kapcsolattal konfigurálható beállításokat tartalmaz, amelyek mindegyike több erőforrás konfigurációjának hello támaszkodik.</span><span class="sxs-lookup"><span data-stu-id="59def-106">A VPN gateway connection relies on hello configuration of multiple resources, each of which contains configurable settings.</span></span> <span data-ttu-id="59def-107">Ez a cikk hello részeiben hello erőforrások és tooa VPN-átjáró a Resource Manager üzembe helyezési modellel létrehozott virtuális hálózatban a szoftverközponthoz kapcsolódó beállításokat ismertetik.</span><span class="sxs-lookup"><span data-stu-id="59def-107">hello sections in this article discuss hello resources and settings that relate tooa VPN gateway for a virtual network created in Resource Manager deployment model.</span></span> <span data-ttu-id="59def-108">Minden kapcsolat megoldás hello megtalálhatja leírásokat és topológiai diagramot [VPN-átjáró](vpn-gateway-about-vpngateways.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="59def-108">You can find descriptions and topology diagrams for each connection solution in hello [About VPN Gateway](vpn-gateway-about-vpngateways.md) article.</span></span>

## <span data-ttu-id="59def-109"><a name="gwtype"></a>Átjáró típusa</span><span class="sxs-lookup"><span data-stu-id="59def-109"><a name="gwtype"></a>Gateway types</span></span>

<span data-ttu-id="59def-110">Minden egyes virtuális hálózati csak egy virtuális hálózati átjáró típusonkénti van.</span><span class="sxs-lookup"><span data-stu-id="59def-110">Each virtual network can only have one virtual network gateway of each type.</span></span> <span data-ttu-id="59def-111">Ha a virtuális hálózati átjáró hoz létre, győződjön meg arról, hogy helyesek-e hello átjáró típusa a konfigurációhoz.</span><span class="sxs-lookup"><span data-stu-id="59def-111">When you are creating a virtual network gateway, you must make sure that hello gateway type is correct for your configuration.</span></span>

<span data-ttu-id="59def-112">hello érhető el - GatewayType értékei:</span><span class="sxs-lookup"><span data-stu-id="59def-112">hello available values for -GatewayType are:</span></span>

* <span data-ttu-id="59def-113">Vpn</span><span class="sxs-lookup"><span data-stu-id="59def-113">Vpn</span></span>
* <span data-ttu-id="59def-114">ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="59def-114">ExpressRoute</span></span>

<span data-ttu-id="59def-115">A VPN-átjáró által igényelt hello `-GatewayType` *Vpn*.</span><span class="sxs-lookup"><span data-stu-id="59def-115">A VPN gateway requires hello `-GatewayType` *Vpn*.</span></span>

<span data-ttu-id="59def-116">Példa:</span><span class="sxs-lookup"><span data-stu-id="59def-116">Example:</span></span>

```powershell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
-Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn `
-VpnType RouteBased
```

## <span data-ttu-id="59def-117"><a name="gwsku"></a>Átjáró-termékváltozatok</span><span class="sxs-lookup"><span data-stu-id="59def-117"><a name="gwsku"></a>Gateway SKUs</span></span>

[!INCLUDE [vpn-gateway-gwsku-include](../../includes/vpn-gateway-gwsku-include.md)]

### <a name="configure-hello-gateway-sku"></a><span data-ttu-id="59def-118">Hello gateway SKU konfigurálása</span><span class="sxs-lookup"><span data-stu-id="59def-118">Configure hello gateway SKU</span></span>

#### <a name="azure-portal"></a><span data-ttu-id="59def-119">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="59def-119">Azure portal</span></span>

<span data-ttu-id="59def-120">Az Azure portál toocreate a Resource Manager virtuális hálózati átjáró hello használatakor választhatja hello gateway SKU hello legördülő lista használatával.</span><span class="sxs-lookup"><span data-stu-id="59def-120">If you use hello Azure portal toocreate a Resource Manager virtual network gateway, you can select hello gateway SKU by using hello dropdown.</span></span> <span data-ttu-id="59def-121">hello beállítások lehetősége lesz toohello átjáró típusa és a választott VPN-típus felel meg.</span><span class="sxs-lookup"><span data-stu-id="59def-121">hello options you are presented with correspond toohello Gateway type and VPN type that you select.</span></span>

#### <a name="powershell"></a><span data-ttu-id="59def-122">PowerShell</span><span class="sxs-lookup"><span data-stu-id="59def-122">PowerShell</span></span>

<span data-ttu-id="59def-123">hello következő PowerShell-példa meghatározza hello `-GatewaySku` VpnGw1 szerint.</span><span class="sxs-lookup"><span data-stu-id="59def-123">hello following PowerShell example specifies hello `-GatewaySku` as VpnGw1.</span></span>

```powershell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
-Location 'West US' -IpConfigurations $gwipconfig -GatewaySku VpnGw1 `
-GatewayType Vpn -VpnType RouteBased
```

#### <span data-ttu-id="59def-124"><a name="resize"></a>Módosítás (átméretezési) egy átjáró-Termékváltozat</span><span class="sxs-lookup"><span data-stu-id="59def-124"><a name="resize"></a>Change (resize) a gateway SKU</span></span>

<span data-ttu-id="59def-125">Ha azt szeretné, tooupgrade az átjáró-Termékváltozat tooa nagyobb teljesítményű Termékváltozat hello használható `Resize-AzureRmVirtualNetworkGateway` PowerShell-parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="59def-125">If you want tooupgrade your gateway SKU tooa more powerful SKU, you can use hello `Resize-AzureRmVirtualNetworkGateway` PowerShell cmdlet.</span></span> <span data-ttu-id="59def-126">Visszaállítható hello átjáró Termékváltozat-méretét a parancsmag segítségével is.</span><span class="sxs-lookup"><span data-stu-id="59def-126">You can also downgrade hello gateway SKU size using this cmdlet.</span></span>

<span data-ttu-id="59def-127">a következő PowerShell-példa hello mutatja egy átjáró SKU átméretezett tooVpnGw2.</span><span class="sxs-lookup"><span data-stu-id="59def-127">hello following PowerShell example shows a gateway SKU being resized tooVpnGw2.</span></span>

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku VpnGw2
```

## <span data-ttu-id="59def-128"><a name="connectiontype"></a>Kapcsolat típusai</span><span class="sxs-lookup"><span data-stu-id="59def-128"><a name="connectiontype"></a>Connection types</span></span>

<span data-ttu-id="59def-129">Hello Resource Manager üzembe helyezési modellel minden egyes konfiguráció szükséges egy adott virtuális hálózati átjáró kapcsolattípus.</span><span class="sxs-lookup"><span data-stu-id="59def-129">In hello Resource Manager deployment model, each configuration requires a specific virtual network gateway connection type.</span></span> <span data-ttu-id="59def-130">hello érhető el erőforrás-kezelő PowerShell értékei `-ConnectionType` vannak:</span><span class="sxs-lookup"><span data-stu-id="59def-130">hello available Resource Manager PowerShell values for `-ConnectionType` are:</span></span>

* <span data-ttu-id="59def-131">IPsec</span><span class="sxs-lookup"><span data-stu-id="59def-131">IPsec</span></span>
* <span data-ttu-id="59def-132">Vnet2Vnet</span><span class="sxs-lookup"><span data-stu-id="59def-132">Vnet2Vnet</span></span>
* <span data-ttu-id="59def-133">ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="59def-133">ExpressRoute</span></span>
* <span data-ttu-id="59def-134">VPNClient</span><span class="sxs-lookup"><span data-stu-id="59def-134">VPNClient</span></span>

<span data-ttu-id="59def-135">A következő PowerShell-példa hello, hello kapcsolattípus igénylő S2S kapcsolatot létrehozhatunk *IPsec*.</span><span class="sxs-lookup"><span data-stu-id="59def-135">In hello following PowerShell example, we create a S2S connection that requires hello connection type *IPsec*.</span></span>

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg `
-Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
-ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
```

## <span data-ttu-id="59def-136"><a name="vpntype"></a>VPN-típusai</span><span class="sxs-lookup"><span data-stu-id="59def-136"><a name="vpntype"></a>VPN types</span></span>

<span data-ttu-id="59def-137">Hello virtuális hálózati átjáró VPN-átjáró konfigurációt létrehozásakor meg kell adnia egy VPN-típus.</span><span class="sxs-lookup"><span data-stu-id="59def-137">When you create hello virtual network gateway for a VPN gateway configuration, you must specify a VPN type.</span></span> <span data-ttu-id="59def-138">VPN-típus az Ön által hello függ hello kapcsolat topológia, amelyet az toocreate.</span><span class="sxs-lookup"><span data-stu-id="59def-138">hello VPN type that you choose depends on hello connection topology that you want toocreate.</span></span> <span data-ttu-id="59def-139">P2S kapcsolatot például egy RouteBased VPN típust igényli.</span><span class="sxs-lookup"><span data-stu-id="59def-139">For example, a P2S connection requires a RouteBased VPN type.</span></span> <span data-ttu-id="59def-140">A VPN-típus hello hardver által használt is függ.</span><span class="sxs-lookup"><span data-stu-id="59def-140">A VPN type can also depend on hello hardware that you are using.</span></span> <span data-ttu-id="59def-141">S2S-konfigurációk esetén van szükség, a VPN-eszközön.</span><span class="sxs-lookup"><span data-stu-id="59def-141">S2S configurations require a VPN device.</span></span> <span data-ttu-id="59def-142">VPN-eszközök csak egy meghatározott VPN-típus támogatja.</span><span class="sxs-lookup"><span data-stu-id="59def-142">Some VPN devices only support a certain VPN type.</span></span>

<span data-ttu-id="59def-143">VPN-típus kiválasztott hello meg kell felelnie a összes hello kapcsolat hello megoldás követelményei toocreate szeretné.</span><span class="sxs-lookup"><span data-stu-id="59def-143">hello VPN type you select must satisfy all hello connection requirements for hello solution you want toocreate.</span></span> <span data-ttu-id="59def-144">Például, ha azt szeretné, hogy toocreate egy S2S VPN gateway-kapcsolatot és a P2S VPN gateway-kapcsolattal hello azonos virtuális hálózatban, VPN-típus használhatja *RouteBased* mert P2S egy RouteBased VPN-típus szükséges.</span><span class="sxs-lookup"><span data-stu-id="59def-144">For example, if you want toocreate a S2S VPN gateway connection and a P2S VPN gateway connection for hello same virtual network, you would use VPN type *RouteBased* because P2S requires a RouteBased VPN type.</span></span> <span data-ttu-id="59def-145">Módosítania kell, hogy a VPN-eszköz támogatja-e RouteBased VPN-kapcsolat tooverify.</span><span class="sxs-lookup"><span data-stu-id="59def-145">You would also need tooverify that your VPN device supported a RouteBased VPN connection.</span></span> 

<span data-ttu-id="59def-146">A virtuális hálózati átjáró létrehozása után hello VPN típusa nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="59def-146">Once a virtual network gateway has been created, you can't change hello VPN type.</span></span> <span data-ttu-id="59def-147">Lehetősége van toodelete hello virtuális hálózati átjáró, és hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="59def-147">You have toodelete hello virtual network gateway and create a new one.</span></span> <span data-ttu-id="59def-148">Kétféle VPN-típus létezik:</span><span class="sxs-lookup"><span data-stu-id="59def-148">There are two VPN types:</span></span>

[!INCLUDE [vpn-gateway-vpntype](../../includes/vpn-gateway-vpntype-include.md)]

<span data-ttu-id="59def-149">hello következő PowerShell-példa meghatározza hello `-VpnType` , *RouteBased*.</span><span class="sxs-lookup"><span data-stu-id="59def-149">hello following PowerShell example specifies hello `-VpnType` as *RouteBased*.</span></span> <span data-ttu-id="59def-150">Amikor egy átjáró hoz létre, meg kell győződnie arról, hogy hello – VpnType a konfigurációja helyességéről.</span><span class="sxs-lookup"><span data-stu-id="59def-150">When you are creating a gateway, you must make sure that hello -VpnType is correct for your configuration.</span></span>

```powershell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
-Location 'West US' -IpConfigurations $gwipconfig `
-GatewayType Vpn -VpnType RouteBased
```

## <span data-ttu-id="59def-151"><a name="requirements"></a>Átjáró követelményei</span><span class="sxs-lookup"><span data-stu-id="59def-151"><a name="requirements"></a>Gateway requirements</span></span>

[!INCLUDE [vpn-gateway-table-requirements](../../includes/vpn-gateway-table-requirements-include.md)]

## <span data-ttu-id="59def-152"><a name="gwsub"></a>Átjáró-alhálózatot</span><span class="sxs-lookup"><span data-stu-id="59def-152"><a name="gwsub"></a>Gateway subnet</span></span>

<span data-ttu-id="59def-153">VPN-átjáró létrehozása előtt létre kell hoznia egy átjáró-alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="59def-153">Before you create a VPN gateway, you must create a gateway subnet.</span></span> <span data-ttu-id="59def-154">hello átjáróalhálózatot hello IP-cím szerepel, hogy hello virtuális hálózati átjáró virtuális gépek és szolgáltatások használatát.</span><span class="sxs-lookup"><span data-stu-id="59def-154">hello gateway subnet contains hello IP addresses that hello virtual network gateway VMs and services use.</span></span> <span data-ttu-id="59def-155">A virtuális hálózati átjáró létrehozásakor átjáró virtuális gépeket telepített toohello átjáró-alhálózatot, és szükséges hello VPN gateway beállításokat.</span><span class="sxs-lookup"><span data-stu-id="59def-155">When you create your virtual network gateway, gateway VMs are deployed toohello gateway subnet and configured with hello required VPN gateway settings.</span></span> <span data-ttu-id="59def-156">Semmi másnak (például további virtuális gépek) toohello átjáróalhálózatot soha nem kell telepítenie.</span><span class="sxs-lookup"><span data-stu-id="59def-156">You must never deploy anything else (for example, additional VMs) toohello gateway subnet.</span></span> <span data-ttu-id="59def-157">hello átjáróalhálózatot névvel kell ellátni "GatewaySubnet" toowork megfelelően.</span><span class="sxs-lookup"><span data-stu-id="59def-157">hello gateway subnet must be named 'GatewaySubnet' toowork properly.</span></span> <span data-ttu-id="59def-158">Hello átjáró alhálózatának elnevezése "GatewaySubnet" megadásával engedélyezhető Azure arról, hogy ez hello alhálózati toodeploy hello virtuális hálózati átjáró virtuális gépek és szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="59def-158">Naming hello gateway subnet 'GatewaySubnet' lets Azure know that this is hello subnet toodeploy hello virtual network gateway VMs and services to.</span></span>

<span data-ttu-id="59def-159">Amikor hello átjáró-alhálózatot hoz létre, megadhatja a hello száma alhálózat hello IP-címeket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="59def-159">When you create hello gateway subnet, you specify hello number of IP addresses that hello subnet contains.</span></span> <span data-ttu-id="59def-160">hello átjáróalhálózatot hello IP-címek lefoglalt toohello átjáró virtuális gépek és szolgáltatások átjáró.</span><span class="sxs-lookup"><span data-stu-id="59def-160">hello IP addresses in hello gateway subnet are allocated toohello gateway VMs and gateway services.</span></span> <span data-ttu-id="59def-161">Bizonyos konfigurációk több IP-címre, mint a többi szükséges.</span><span class="sxs-lookup"><span data-stu-id="59def-161">Some configurations require more IP addresses than others.</span></span> <span data-ttu-id="59def-162">Szeretné, hogy toocreate, és győződjön meg arról, hogy azt szeretné, hogy toocreate hello átjáróalhálózatot megfelel ezeknek a követelményeknek hello konfigurációs hello kapcsolatos útmutatásért tekintse meg.</span><span class="sxs-lookup"><span data-stu-id="59def-162">Look at hello instructions for hello configuration that you want toocreate and verify that hello gateway subnet you want toocreate meets those requirements.</span></span> <span data-ttu-id="59def-163">Emellett érdemes lehet, hogy az átjáró-alhálózatot tartalmaz elég IP-címek tooaccommodate lehetséges jövőbeli további konfigurációk toomake.</span><span class="sxs-lookup"><span data-stu-id="59def-163">Additionally, you may want toomake sure your gateway subnet contains enough IP addresses tooaccommodate possible future additional configurations.</span></span> <span data-ttu-id="59def-164">Létrehozhat egy átjáró-alhálózat mérete /29 legyen, de javasolt, hogy hozzon létre egy átjáró-alhálózatot /28 vagy nagyobb (/ 28, /27, /26 stb.).</span><span class="sxs-lookup"><span data-stu-id="59def-164">While you can create a gateway subnet as small as /29, we recommend that you create a gateway subnet of /28 or larger (/28, /27, /26 etc.).</span></span> <span data-ttu-id="59def-165">Így ha a jövőbeli, hello funkciókkal, nem rendelkezik tootear az átjáró, majd törölje és hozza létre hello átjáró alhálózati tooallow további IP-címekhez.</span><span class="sxs-lookup"><span data-stu-id="59def-165">That way, if you add functionality in hello future, you won't have tootear your gateway, then delete and recreate hello gateway subnet tooallow for more IP addresses.</span></span>

<span data-ttu-id="59def-166">hello következő erőforrás-kezelő PowerShell példa bemutatja egy GatewaySubnet nevű átjáró-alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="59def-166">hello following Resource Manager PowerShell example shows a gateway subnet named GatewaySubnet.</span></span> <span data-ttu-id="59def-167">Hello CIDR-jelöléssel Meghatározza, hogy egy /27 lehetővé teszi, hogy elegendő IP-címek a jelenleg létező legtöbb konfiguráció látható.</span><span class="sxs-lookup"><span data-stu-id="59def-167">You can see hello CIDR notation specifies a /27, which allows for enough IP addresses for most configurations that currently exist.</span></span>

```powershell
Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/27
```

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

## <span data-ttu-id="59def-168"><a name="lng"></a>Helyi hálózati átjáró</span><span class="sxs-lookup"><span data-stu-id="59def-168"><a name="lng"></a>Local network gateways</span></span>

<span data-ttu-id="59def-169">A VPN-átjáró konfigurációs létrehozásakor hello helyi hálózati átjáró gyakran jelöli a helyszíni hely.</span><span class="sxs-lookup"><span data-stu-id="59def-169">When creating a VPN gateway configuration, hello local network gateway often represents your on-premises location.</span></span> <span data-ttu-id="59def-170">Hello klasszikus üzembe helyezési modellel hello helyi hálózati átjáró volt a hivatkozott tooas a helyi webhelyhez.</span><span class="sxs-lookup"><span data-stu-id="59def-170">In hello classic deployment model, hello local network gateway was referred tooas a Local Site.</span></span> 

<span data-ttu-id="59def-171">Nevezze el hello helyi hálózati átjáró, hello hello a helyszíni VPN-eszköz nyilvános IP-címe és hello címelőtagokat hello helyszíni helyen található meg.</span><span class="sxs-lookup"><span data-stu-id="59def-171">You give hello local network gateway a name, hello public IP address of hello on-premises VPN device, and specify hello address prefixes that are located on hello on-premises location.</span></span> <span data-ttu-id="59def-172">Azure hello cél címelőtagokat a hálózati forgalom megvizsgálja, a helyi hálózati átjáró megadott hello konfigurációs olvas, és ennek megfelelően irányítja a csomagokat.</span><span class="sxs-lookup"><span data-stu-id="59def-172">Azure looks at hello destination address prefixes for network traffic, consults hello configuration that you have specified for your local network gateway, and routes packets accordingly.</span></span> <span data-ttu-id="59def-173">Helyi hálózati átjáró VPN gateway-kapcsolatot használó VNet – VNet-konfigurációk is adja meg.</span><span class="sxs-lookup"><span data-stu-id="59def-173">You also specify local network gateways for VNet-to-VNet configurations that use a VPN gateway connection.</span></span>

<span data-ttu-id="59def-174">a következő PowerShell-példa hello hoz létre egy új helyi hálózati átjáró:</span><span class="sxs-lookup"><span data-stu-id="59def-174">hello following PowerShell example creates a new local network gateway:</span></span>

```powershell
New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
-Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.5.51.0/24'
```

<span data-ttu-id="59def-175">Néha kell toomodify hello helyi hálózati átjáró beállításainak.</span><span class="sxs-lookup"><span data-stu-id="59def-175">Sometimes you need toomodify hello local network gateway settings.</span></span> <span data-ttu-id="59def-176">Például ha fel vagy módosít hello címtartományt, vagy ha hello VPN-eszköz IP-címe hello megváltozik.</span><span class="sxs-lookup"><span data-stu-id="59def-176">For example, when you add or modify hello address range, or if hello IP address of hello VPN device changes.</span></span> <span data-ttu-id="59def-177">A klasszikus vnet ezeket a beállításokat a klasszikus portálon hello hello helyi hálózatok lapon módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="59def-177">For a classic VNet, you can change these settings in hello classic portal on hello Local Networks page.</span></span> <span data-ttu-id="59def-178">Az erőforrás-kezelő, lásd: [PowerShell használatával a helyi hálózati átjáró beállításainak módosítása](vpn-gateway-modify-local-network-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="59def-178">For Resource Manager, see [Modify local network gateway settings using PowerShell](vpn-gateway-modify-local-network-gateway.md).</span></span>

## <span data-ttu-id="59def-179"><a name="resources"></a>REST API-k és a PowerShell-parancsmagok</span><span class="sxs-lookup"><span data-stu-id="59def-179"><a name="resources"></a>REST APIs and PowerShell cmdlets</span></span>

<span data-ttu-id="59def-180">További technikai erőforrások és a konkrét szintaxis használatával kapcsolatos követelményeket REST API-k, a PowerShell-parancsmagok vagy az Azure CLI VPN Gateway-konfigurációk esetén lásd a következő lapok hello:</span><span class="sxs-lookup"><span data-stu-id="59def-180">For additional technical resources and specific syntax requirements when using REST APIs, PowerShell cmdlets, or Azure CLI for VPN Gateway configurations, see hello following pages:</span></span>

| <span data-ttu-id="59def-181">**Klasszikus**</span><span class="sxs-lookup"><span data-stu-id="59def-181">**Classic**</span></span> | <span data-ttu-id="59def-182">**Resource Manager**</span><span class="sxs-lookup"><span data-stu-id="59def-182">**Resource Manager**</span></span> |
| --- | --- |
| [<span data-ttu-id="59def-183">PowerShell</span><span class="sxs-lookup"><span data-stu-id="59def-183">PowerShell</span></span>](/powershell/module/azure#networking) |[<span data-ttu-id="59def-184">PowerShell</span><span class="sxs-lookup"><span data-stu-id="59def-184">PowerShell</span></span>](/powershell/module/azurerm.network#vpn) |
| [<span data-ttu-id="59def-185">REST API</span><span class="sxs-lookup"><span data-stu-id="59def-185">REST API</span></span>](https://msdn.microsoft.com/library/jj154113) |[<span data-ttu-id="59def-186">REST API</span><span class="sxs-lookup"><span data-stu-id="59def-186">REST API</span></span>](/rest/api/network/virtualnetworkgateways) |
| <span data-ttu-id="59def-187">Nem támogatott</span><span class="sxs-lookup"><span data-stu-id="59def-187">Not supported</span></span> | [<span data-ttu-id="59def-188">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="59def-188">Azure CLI</span></span>](/cli/azure/network/vnet-gateway)|

## <a name="next-steps"></a><span data-ttu-id="59def-189">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="59def-189">Next steps</span></span>

<span data-ttu-id="59def-190">Használható kapcsolat konfigurációkkal kapcsolatos további információkért lásd: [VPN-átjáró](vpn-gateway-about-vpngateways.md).</span><span class="sxs-lookup"><span data-stu-id="59def-190">For more information about available connection configurations, see [About VPN Gateway](vpn-gateway-about-vpngateways.md).</span></span>