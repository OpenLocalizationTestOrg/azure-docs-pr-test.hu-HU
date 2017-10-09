---
title: "Párhuzamosan fennálló ExpressRoute- és helyek közötti VPN-kapcsolatok konfigurálása: Resource Manager: Azure | Microsoft Docs"
description: "A cikk bemutatja az ExpressRoute- és egy helyek közötti VPN-kapcsolat konfigurálását, amelyek párhuzamosan használhatók a Resource Manager-alapú üzemi modellben."
documentationcenter: na
services: expressroute
author: charwen
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: c7717b14-3da3-4a6d-b78e-a5020766bc2c
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/19/2017
ms.author: charwen,cherylmc
ms.openlocfilehash: efda9f89d95617c8c4e75af91b20631dc468d4db
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-expressroute-and-site-to-site-coexisting-connections"></a><span data-ttu-id="85cd9-103">Párhuzamos ExpressRoute- és párhuzamos telephelyközi kapcsolatok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="85cd9-103">Configure ExpressRoute and Site-to-Site coexisting connections</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="85cd9-104">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="85cd9-104">PowerShell - Resource Manager</span></span>](expressroute-howto-coexist-resource-manager.md)
> * [<span data-ttu-id="85cd9-105">PowerShell – Klasszikus</span><span class="sxs-lookup"><span data-stu-id="85cd9-105">PowerShell - Classic</span></span>](expressroute-howto-coexist-classic.md)
> 
> 

<span data-ttu-id="85cd9-106">Az egyidejű helyek közötti VPN- és ExpressRoute-kapcsolatok konfigurálása több előnnyel jár.</span><span class="sxs-lookup"><span data-stu-id="85cd9-106">Configuring Site-to-Site VPN and ExpressRoute coexisting connections has several advantages.</span></span> <span data-ttu-id="85cd9-107">Biztonságos feladatátvételi elérési útnak a telephelyek közötti VPN konfigurálása ExressRoute, vagy használja a pont-pont VPN tooconnect toosites nem ExpressRoute keresztül csatlakozó.</span><span class="sxs-lookup"><span data-stu-id="85cd9-107">You can configure a Site-to-Site VPN as a secure failover path for ExressRoute, or use Site-to-Site VPNs tooconnect toosites that are not connected through ExpressRoute.</span></span> <span data-ttu-id="85cd9-108">Mindkét forgatókönyvet ebben a cikkben azt fedezi hello lépéseket tooconfigure.</span><span class="sxs-lookup"><span data-stu-id="85cd9-108">We cover hello steps tooconfigure both scenarios in this article.</span></span> <span data-ttu-id="85cd9-109">Ez a cikk toohello Resource Manager üzembe helyezési modellben vonatkozik, és használja a PowerShell.</span><span class="sxs-lookup"><span data-stu-id="85cd9-109">This article applies toohello Resource Manager deployment model and uses PowerShell.</span></span> <span data-ttu-id="85cd9-110">Ez a konfiguráció hello Azure-portál nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="85cd9-110">This configuration is not available in hello Azure portal.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="85cd9-111">ExpressRoute-Kapcsolatcsoportok előre konfigurálni kell az alábbi hello utasítások végrehajtása előtt.</span><span class="sxs-lookup"><span data-stu-id="85cd9-111">ExpressRoute circuits must be pre-configured before you follow hello instructions below.</span></span> <span data-ttu-id="85cd9-112">Győződjön meg arról, hogy követte hello útmutatók túl[ExpressRoute-kapcsolatcsoportot létrehozni](expressroute-howto-circuit-arm.md) és [konfigurálja az útválasztást](expressroute-howto-routing-arm.md) folytatás előtt.</span><span class="sxs-lookup"><span data-stu-id="85cd9-112">Make sure that you have followed hello guides too[create an ExpressRoute circuit](expressroute-howto-circuit-arm.md) and [configure routing](expressroute-howto-routing-arm.md) before you proceed.</span></span>
> 
> 

## <a name="limits-and-limitations"></a><span data-ttu-id="85cd9-113">Korlátok és korlátozások</span><span class="sxs-lookup"><span data-stu-id="85cd9-113">Limits and limitations</span></span>
* <span data-ttu-id="85cd9-114">**A tranzit útválasztás nem támogatott.**</span><span class="sxs-lookup"><span data-stu-id="85cd9-114">**Transit routing is not supported.**</span></span> <span data-ttu-id="85cd9-115">Nem hajthat végre útválasztást (az Azure-on keresztül) a helyek közötti VPN használatával csatlakoztatott helyi hálózat és az ExpressRoute használatával csatlakoztatott helyi hálózat között.</span><span class="sxs-lookup"><span data-stu-id="85cd9-115">You cannot route (via Azure) between your local network connected via Site-to-Site VPN and your local network connected via ExpressRoute.</span></span>
* <span data-ttu-id="85cd9-116">**A Basic SKU-átjáró nem támogatott.**</span><span class="sxs-lookup"><span data-stu-id="85cd9-116">**Basic SKU gateway is not supported.**</span></span> <span data-ttu-id="85cd9-117">Nem alapszintű Termékváltozat átjáró kell használnia az mindkét hello [ExpressRoute-átjáró](expressroute-about-virtual-network-gateways.md) és hello [VPN-átjáró](../vpn-gateway/vpn-gateway-about-vpngateways.md).</span><span class="sxs-lookup"><span data-stu-id="85cd9-117">You must use a non-Basic SKU gateway for both hello [ExpressRoute gateway](expressroute-about-virtual-network-gateways.md) and hello [VPN gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).</span></span>
* <span data-ttu-id="85cd9-118">**Kizárólag az útvonalalapú VPN-átjáró támogatott.**</span><span class="sxs-lookup"><span data-stu-id="85cd9-118">**Only route-based VPN gateway is supported.**</span></span> <span data-ttu-id="85cd9-119">Útvonalalapú [VPN-átjárót](../vpn-gateway/vpn-gateway-about-vpngateways.md) kell használnia.</span><span class="sxs-lookup"><span data-stu-id="85cd9-119">You must use a route-based [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).</span></span>
* <span data-ttu-id="85cd9-120">**A VPN-átjáróhoz statikus útvonalat kell konfigurálni.**</span><span class="sxs-lookup"><span data-stu-id="85cd9-120">**Static route should be configured for your VPN gateway.**</span></span> <span data-ttu-id="85cd9-121">Ha a helyi hálózathoz csatlakoztatott tooboth ExpressRoute és a pont-pont VPN, rendelkeznie kell a helyi hálózati tooroute hello pont-pont VPN-kapcsolat toohello konfigurált statikus útvonal nyilvános internethez.</span><span class="sxs-lookup"><span data-stu-id="85cd9-121">If your local network is connected tooboth ExpressRoute and a Site-to-Site VPN, you must have a static route configured in your local network tooroute hello Site-to-Site VPN connection toohello public Internet.</span></span>
* <span data-ttu-id="85cd9-122">**ExpressRoute-átjárót először meg kell adni, és a kapcsolódó tooa körön.**</span><span class="sxs-lookup"><span data-stu-id="85cd9-122">**ExpressRoute gateway must be configured first and linked tooa circuit.**</span></span> <span data-ttu-id="85cd9-123">Meg kell hello ExpressRoute-átjárót először hozza létre, és tooa áramkör hello pont-pont VPN-átjáró hozzáadása előtt.</span><span class="sxs-lookup"><span data-stu-id="85cd9-123">You must create hello ExpressRoute gateway first and link it tooa circuit before you add hello Site-to-Site VPN gateway.</span></span>

## <a name="configuration-designs"></a><span data-ttu-id="85cd9-124">Konfigurációs tervek</span><span class="sxs-lookup"><span data-stu-id="85cd9-124">Configuration designs</span></span>
### <a name="configure-a-site-to-site-vpn-as-a-failover-path-for-expressroute"></a><span data-ttu-id="85cd9-125">Helyek közötti VPN konfigurálása feladatátvételi útvonalként az ExpressRoute számára</span><span class="sxs-lookup"><span data-stu-id="85cd9-125">Configure a Site-to-Site VPN as a failover path for ExpressRoute</span></span>
<span data-ttu-id="85cd9-126">Konfigurálhat egy helyek közötti VPN-kapcsolatot tartalékként az ExpressRoute számára.</span><span class="sxs-lookup"><span data-stu-id="85cd9-126">You can configure a Site-to-Site VPN connection as a backup for ExpressRoute.</span></span> <span data-ttu-id="85cd9-127">Ez vonatkozik csak toovirtual hálózatok csatolt toohello Azure magánhálózati társviszony-létesítési elérési útja.</span><span class="sxs-lookup"><span data-stu-id="85cd9-127">This applies only toovirtual networks linked toohello Azure private peering path.</span></span> <span data-ttu-id="85cd9-128">Az Azure nyilvános és a Microsoft társviszony-létesítésekhez nem létezik VPN-alapú feladatátvételi megoldás.</span><span class="sxs-lookup"><span data-stu-id="85cd9-128">There is no VPN-based failover solution for services accessible through Azure public and Microsoft peerings.</span></span> <span data-ttu-id="85cd9-129">hello ExpressRoute-kapcsolatcsoportot mindig hello elsődleges kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="85cd9-129">hello ExpressRoute circuit is always hello primary link.</span></span> <span data-ttu-id="85cd9-130">Adatáramlás hello telephelyek közötti VPN elérési útján csak akkor, ha ExpressRoute-kapcsolatcsoportot hello sikertelen.</span><span class="sxs-lookup"><span data-stu-id="85cd9-130">Data flows through hello Site-to-Site VPN path only if hello ExpressRoute circuit fails.</span></span>

> [!NOTE]
> <span data-ttu-id="85cd9-131">Habár ExpressRoute-kapcsolatcsoportot pont-pont VPN-kapcsolaton keresztül előnyben részesített a mindkét útvonalak vannak hello azonos, Azure használandó hello leghosszabb előtag egyezés toochoose hello útvonal hello csomagok cél felé.</span><span class="sxs-lookup"><span data-stu-id="85cd9-131">While ExpressRoute circuit is preferred over Site-to-Site VPN when both routes are hello same, Azure will use hello longest prefix match toochoose hello route towards hello packet's destination.</span></span>
> 
> 

![Egyidejű jelenlét](media/expressroute-howto-coexist-resource-manager/scenario1.jpg)

### <a name="configure-a-site-to-site-vpn-tooconnect-toosites-not-connected-through-expressroute"></a><span data-ttu-id="85cd9-133">Egy ExpressRoute keresztül nem kapcsolódik telephelyek közötti VPN-tooconnect toosites konfigurálása</span><span class="sxs-lookup"><span data-stu-id="85cd9-133">Configure a Site-to-Site VPN tooconnect toosites not connected through ExpressRoute</span></span>
<span data-ttu-id="85cd9-134">Beállíthatja, hogy a hálózat, ahol egyes helyek csatlakoznak közvetlenül tooAzure pont-pont VPN-kapcsolaton keresztül, és az egyes helyek ExpressRoute keresztül csatlakoznak.</span><span class="sxs-lookup"><span data-stu-id="85cd9-134">You can configure your network where some sites connect directly tooAzure over Site-to-Site VPN, and some sites connect through ExpressRoute.</span></span> 

![Egyidejű jelenlét](media/expressroute-howto-coexist-resource-manager/scenario2.jpg)

> [!NOTE]
> <span data-ttu-id="85cd9-136">Virtuális hálózatot nem konfigurálhat tranzit útválasztóként.</span><span class="sxs-lookup"><span data-stu-id="85cd9-136">You cannot configure a virtual network as a transit router.</span></span>
> 
> 

## <a name="selecting-hello-steps-toouse"></a><span data-ttu-id="85cd9-137">Hello lépéseket toouse kiválasztása</span><span class="sxs-lookup"><span data-stu-id="85cd9-137">Selecting hello steps toouse</span></span>
<span data-ttu-id="85cd9-138">Az eljárások toochoose két csoportja van.</span><span class="sxs-lookup"><span data-stu-id="85cd9-138">There are two different sets of procedures toochoose from.</span></span> <span data-ttu-id="85cd9-139">kiválasztott hello konfigurációs eljárás attól függ, hogy van-e egy meglévő virtuális hálózatot, hogy azt szeretné, hogy tooconnect, vagy azt szeretné, hogy egy új virtuális hálózat toocreate.</span><span class="sxs-lookup"><span data-stu-id="85cd9-139">hello configuration procedure that you select depends on whether you have an existing virtual network that you want tooconnect to, or you want toocreate a new virtual network.</span></span>

* <span data-ttu-id="85cd9-140">Nem rendelkezik egy VNet és egy toocreate kell.</span><span class="sxs-lookup"><span data-stu-id="85cd9-140">I don't have a VNet and need toocreate one.</span></span>
  
    <span data-ttu-id="85cd9-141">Ha még nem rendelkezik virtuális hálózattal, ez az eljárás lépésről lépésre végigvezeti az új virtuális hálózat létrehozásának folyamatán a Resource Manager-alapú üzemi modellnek megfelelően, valamint az új ExpressRoute- és helyek közötti VPN-kapcsolatok létrehozásának folyamatán.</span><span class="sxs-lookup"><span data-stu-id="85cd9-141">If you don’t already have a virtual network, this procedure walks you through creating a new virtual network using Resource Manager deployment model and creating new ExpressRoute and Site-to-Site VPN connections.</span></span> <span data-ttu-id="85cd9-142">egy virtuális hálózati tooconfigure kövesse hello [toocreate egy új virtuális hálózat és vizsgálatát a kísérő kapcsolatok](#new).</span><span class="sxs-lookup"><span data-stu-id="85cd9-142">tooconfigure a virtual network, follow hello steps in [toocreate a new virtual network and coexisting connections](#new).</span></span>
* <span data-ttu-id="85cd9-143">Már rendelkezem egy Resource Manager-alapú üzemi modell szerinti VNettel.</span><span class="sxs-lookup"><span data-stu-id="85cd9-143">I already have a Resource Manager deployment model VNet.</span></span>
  
    <span data-ttu-id="85cd9-144">Előfordulhat, hogy már rendelkezik üzemelő virtuális hálózattal egy létező helyek közötti VPN- vagy ExpressRoute-kapcsolattal.</span><span class="sxs-lookup"><span data-stu-id="85cd9-144">You may already have a virtual network in place with an existing Site-to-Site VPN connection or ExpressRoute connection.</span></span> <span data-ttu-id="85cd9-145">Hello [tooconfigure vizsgálatát a kísérő kapcsolatok egy már meglévő vnet](#add) szakasz végigvezeti hello átjáró törlése folyamatban, és ezután az új ExpressRoute és a pont-pont VPN-kapcsolatok létrehozására.</span><span class="sxs-lookup"><span data-stu-id="85cd9-145">hello [tooconfigure coexisting connections for an already existing VNet](#add) section walks you through deleting hello gateway, and then creating new ExpressRoute and Site-to-Site VPN connections.</span></span> <span data-ttu-id="85cd9-146">Amikor hello hoz létre új kapcsolatokat, meghatározott sorrendben hello a lépéseket kell végrehajtania.</span><span class="sxs-lookup"><span data-stu-id="85cd9-146">When creating hello new connections, hello steps must be completed in a specific order.</span></span> <span data-ttu-id="85cd9-147">Ne használjon más cikkek toocreate hello utasításait, az átjárók és kapcsolatok.</span><span class="sxs-lookup"><span data-stu-id="85cd9-147">Don't use hello instructions in other articles toocreate your gateways and connections.</span></span>
  
    <span data-ttu-id="85cd9-148">Ebben az eljárásban, amely egyszerre is használható kapcsolatok létrehozására, akkor toodelete igényel az átjáró, és adja meg új átjárók.</span><span class="sxs-lookup"><span data-stu-id="85cd9-148">In this procedure, creating connections that can coexist requires you toodelete your gateway, and then configure new gateways.</span></span> <span data-ttu-id="85cd9-149">Hogy állásidő a létesítmények közötti kapcsolatok során törölje és hozza létre újra az átjárót és a kapcsolatok, de nem kell toomigrate bármely, a virtuális gépek vagy szolgáltatások tooa új virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="85cd9-149">You will have downtime for your cross-premises connections while you delete and recreate your gateway and connections, but you will not need toomigrate any of your VMs or services tooa new virtual network.</span></span> <span data-ttu-id="85cd9-150">A virtuális gépek és szolgáltatások is ki tudja toocommunicate keresztül hello terheléselosztó amíg az átjáró konfigurálásának, ha így konfigurálva toodo.</span><span class="sxs-lookup"><span data-stu-id="85cd9-150">Your VMs and services will still be able toocommunicate out through hello load balancer while you configure your gateway if they are configured toodo so.</span></span>

## <span data-ttu-id="85cd9-151"><a name="new"></a>új virtuális hálózat toocreate és vizsgálatát a kísérő kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="85cd9-151"><a name="new"></a>toocreate a new virtual network and coexisting connections</span></span>
<span data-ttu-id="85cd9-152">Az eljárás a VNetek, valamint az egyidejűleg jelenlévő helyek közötti és ExpressRoute-kapcsolatok létrehozásának módját ismerteti.</span><span class="sxs-lookup"><span data-stu-id="85cd9-152">This procedure walks you through creating a VNet and Site-to-Site and ExpressRoute connections that will coexist.</span></span>

1. <span data-ttu-id="85cd9-153">Hello hello Azure PowerShell-parancsmagok legújabb verziójának telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="85cd9-153">Install hello latest version of hello Azure PowerShell cmdlets.</span></span> <span data-ttu-id="85cd9-154">Hello parancsmagok telepítésével kapcsolatos információkért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="85cd9-154">For information about installing hello cmdlets, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span> <span data-ttu-id="85cd9-155">lehet, hogy mi, előfordulhat, hogy ismernie kell a kis mértékben eltérő hello parancsmagok, amelyekkel ehhez a konfigurációhoz.</span><span class="sxs-lookup"><span data-stu-id="85cd9-155">hello cmdlets that you use for this configuration may be slightly different than what you might be familiar with.</span></span> <span data-ttu-id="85cd9-156">Adható meg, hogy toouse hello parancsmagok ezeket az utasításokat.</span><span class="sxs-lookup"><span data-stu-id="85cd9-156">Be sure toouse hello cmdlets specified in these instructions.</span></span>
2. <span data-ttu-id="85cd9-157">Jelentkezzen be tooyour fiókot, és hello környezet beállítása.</span><span class="sxs-lookup"><span data-stu-id="85cd9-157">Log in tooyour account and set up hello environment.</span></span>

  ```powershell
  login-AzureRmAccount
  Select-AzureRmSubscription -SubscriptionName 'yoursubscription'
  $location = "Central US"
  $resgrp = New-AzureRmResourceGroup -Name "ErVpnCoex" -Location $location
  $VNetASN = 65010
  ```
3. <span data-ttu-id="85cd9-158">Hozzon létre egy virtuális hálózatot az átjáró-alhálózattal együtt.</span><span class="sxs-lookup"><span data-stu-id="85cd9-158">Create a virtual network including Gateway Subnet.</span></span> <span data-ttu-id="85cd9-159">Hello virtuális hálózati konfigurációjával kapcsolatos további információkért lásd: [Azure virtuális hálózat konfigurálása](../virtual-network/virtual-networks-create-vnet-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="85cd9-159">For more information about hello virtual network configuration, see [Azure Virtual Network configuration](../virtual-network/virtual-networks-create-vnet-arm-ps.md).</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="85cd9-160">Átjáró alhálózati hello /27 vagy egy rövidebb előtaggal (például /26 vagy /25) kell lennie.</span><span class="sxs-lookup"><span data-stu-id="85cd9-160">hello Gateway Subnet must be /27 or a shorter prefix (such as /26 or /25).</span></span>
   > 
   > 
   
    <span data-ttu-id="85cd9-161">Hozzon létre egy új VNetet.</span><span class="sxs-lookup"><span data-stu-id="85cd9-161">Create a new VNet.</span></span>

  ```powershell
  $vnet = New-AzureRmVirtualNetwork -Name "CoexVnet" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -AddressPrefix "10.200.0.0/16"
  ```
   
    <span data-ttu-id="85cd9-162">Adjon hozzá alhálózatokat.</span><span class="sxs-lookup"><span data-stu-id="85cd9-162">Add subnets.</span></span>

  ```powershell
  Add-AzureRmVirtualNetworkSubnetConfig -Name "App" -VirtualNetwork $vnet -AddressPrefix "10.200.1.0/24"
  Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet -AddressPrefix "10.200.255.0/24"
  ```
   
    <span data-ttu-id="85cd9-163">Hello VNet-konfiguráció mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="85cd9-163">Save hello VNet configuration.</span></span>

  ```powershell
  $vnet = Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```
4. <span data-ttu-id="85cd9-164"><a name="gw"></a>Hozzon létre egy ExpressRoute-átjárót.</span><span class="sxs-lookup"><span data-stu-id="85cd9-164"><a name="gw"></a>Create an ExpressRoute gateway.</span></span> <span data-ttu-id="85cd9-165">Hello ExpressRoute-átjáró konfigurációjával kapcsolatos további információkért lásd: [ExpressRoute-átjáró konfigurációs](expressroute-howto-add-gateway-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="85cd9-165">For more information about hello ExpressRoute gateway configuration, see [ExpressRoute gateway configuration](expressroute-howto-add-gateway-resource-manager.md).</span></span> <span data-ttu-id="85cd9-166">hello GatewaySKU kell *szabványos*, *HighPerformance*, vagy *UltraPerformance*.</span><span class="sxs-lookup"><span data-stu-id="85cd9-166">hello GatewaySKU must be *Standard*, *HighPerformance*, or *UltraPerformance*.</span></span>

  ```powershell
  $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
  $gwIP = New-AzureRmPublicIpAddress -Name "ERGatewayIP" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -AllocationMethod Dynamic
  $gwConfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name "ERGatewayIpConfig" -SubnetId $gwSubnet.Id -PublicIpAddressId $gwIP.Id
  $gw = New-AzureRmVirtualNetworkGateway -Name "ERGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -IpConfigurations $gwConfig -GatewayType "ExpressRoute" -GatewaySku Standard
  ```
5. <span data-ttu-id="85cd9-167">Hello ExpressRoute átjáró toohello ExpressRoute-kapcsolatcsoportot hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="85cd9-167">Link hello ExpressRoute gateway toohello ExpressRoute circuit.</span></span> <span data-ttu-id="85cd9-168">Ez a lépés befejezése után, a helyszíni hálózat között az Azure ExpressRoute, keresztül hello kapcsolat létrejött.</span><span class="sxs-lookup"><span data-stu-id="85cd9-168">After this step has been completed, hello connection between your on-premises network and Azure, through ExpressRoute, is established.</span></span> <span data-ttu-id="85cd9-169">Hello hivatkozás művelettel kapcsolatos további információkért lásd: [hivatkozás Vnetek tooExpressRoute](expressroute-howto-linkvnet-arm.md).</span><span class="sxs-lookup"><span data-stu-id="85cd9-169">For more information about hello link operation, see [Link VNets tooExpressRoute](expressroute-howto-linkvnet-arm.md).</span></span>

  ```powershell
  $ckt = Get-AzureRmExpressRouteCircuit -Name "YourCircuit" -ResourceGroupName "YourCircuitResourceGroup"
  New-AzureRmVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -VirtualNetworkGateway1 $gw -PeerId $ckt.Id -ConnectionType ExpressRoute
  ```
6. <span data-ttu-id="85cd9-170"><a name="vpngw"></a>Ezután hozza létre a webhelyek közötti VPN-átjárót.</span><span class="sxs-lookup"><span data-stu-id="85cd9-170"><a name="vpngw"></a>Next, create your Site-to-Site VPN gateway.</span></span> <span data-ttu-id="85cd9-171">Hello VPN gateway konfigurációjával kapcsolatos további információkért lásd: [egy virtuális hálózat konfigurálása webhelyek kapcsolattal rendelkező](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="85cd9-171">For more information about hello VPN gateway configuration, see [Configure a VNet with a Site-to-Site connection](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md).</span></span> <span data-ttu-id="85cd9-172">hello GatewaySKU kell *szabványos*, *HighPerformance*, vagy *UltraPerformance*.</span><span class="sxs-lookup"><span data-stu-id="85cd9-172">hello GatewaySKU must be *Standard*, *HighPerformance*, or *UltraPerformance*.</span></span> <span data-ttu-id="85cd9-173">hello VpnType kell *RouteBased*.</span><span class="sxs-lookup"><span data-stu-id="85cd9-173">hello VpnType must *RouteBased*.</span></span>

  ```powershell
  $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
  $gwIP = New-AzureRmPublicIpAddress -Name "VPNGatewayIP" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -AllocationMethod Dynamic
  $gwConfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name "VPNGatewayIpConfig" -SubnetId $gwSubnet.Id -PublicIpAddressId $gwIP.Id
  New-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -IpConfigurations $gwConfig -GatewayType "Vpn" -VpnType "RouteBased" -GatewaySku "Standard"
  ```
   
    <span data-ttu-id="85cd9-174">Az Azure-os VPN-átjáró támogatja a BGP útválasztási protokollt.</span><span class="sxs-lookup"><span data-stu-id="85cd9-174">Azure VPN gateway supports BGP routing protocol.</span></span> <span data-ttu-id="85cd9-175">Megadhatja az ASN (AS-szám) virtuális hálózat hello - Asn kapcsoló hello a következő parancs hozzáadásával.</span><span class="sxs-lookup"><span data-stu-id="85cd9-175">You can specify ASN (AS Number) for that Virtual Network by adding hello -Asn switch in hello following command.</span></span> <span data-ttu-id="85cd9-176">Nem adja meg, hogy paramétert fog alapértelmezett tooAS number 65515.</span><span class="sxs-lookup"><span data-stu-id="85cd9-176">Not specifying that parameter will default tooAS number 65515.</span></span>

  ```powershell
  $azureVpn = New-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -IpConfigurations $gwConfig -GatewayType "Vpn" -VpnType "RouteBased" -GatewaySku "Standard" -Asn $VNetASN
  ```
   
    <span data-ttu-id="85cd9-177">BGP társviszony-létesítést IP hello és hello Azure által a VPN-átjáró hello $azureVpn.BgpSettings.BgpPeeringAddress és $azureVpn.BgpSettings.Asn SZÁMOT.</span><span class="sxs-lookup"><span data-stu-id="85cd9-177">You can find hello BGP peering IP and hello AS number that Azure uses for hello VPN gateway in $azureVpn.BgpSettings.BgpPeeringAddress and $azureVpn.BgpSettings.Asn.</span></span> <span data-ttu-id="85cd9-178">További információk: [A BGP konfigurálása](../vpn-gateway/vpn-gateway-bgp-resource-manager-ps.md) az Azure-alapú VPN-átjáróhoz.</span><span class="sxs-lookup"><span data-stu-id="85cd9-178">For more information, see [Configure BGP](../vpn-gateway/vpn-gateway-bgp-resource-manager-ps.md) for Azure VPN gateway.</span></span>
7. <span data-ttu-id="85cd9-179">Hozzon létre egy helyi VPN-átjáró entitást.</span><span class="sxs-lookup"><span data-stu-id="85cd9-179">Create a local site VPN gateway entity.</span></span> <span data-ttu-id="85cd9-180">Ez a parancs nem konfigurálja a helyszíni VPN-átjárót.</span><span class="sxs-lookup"><span data-stu-id="85cd9-180">This command doesn’t configure your on-premises VPN gateway.</span></span> <span data-ttu-id="85cd9-181">Ehelyett tooprovide hello helyi átjáró beállításai, például a hello nyilvános IP-cím lehetővé teszi, hello helyszíni címtér, így hello Azure VPN-átjáró képes kapcsolódni tooit.</span><span class="sxs-lookup"><span data-stu-id="85cd9-181">Rather, it allows you tooprovide hello local gateway settings, such as hello public IP and hello on-premises address space, so that hello Azure VPN gateway can connect tooit.</span></span>
   
    <span data-ttu-id="85cd9-182">Ha a helyi VPN-eszköz csak akkor támogatja a statikus útválasztást, konfigurálhatja hello statikus útvonalak a következő módon hello:</span><span class="sxs-lookup"><span data-stu-id="85cd9-182">If your local VPN device only supports static routing, you can configure hello static routes in hello following way:</span></span>

  ```powershell
  $MyLocalNetworkAddress = @("10.100.0.0/16","10.101.0.0/16","10.102.0.0/16")
  $localVpn = New-AzureRmLocalNetworkGateway -Name "LocalVPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -GatewayIpAddress *<Public IP>* -AddressPrefix $MyLocalNetworkAddress
  ```
   
    <span data-ttu-id="85cd9-183">Ha a helyi VPN-eszköz támogatja a hello BGP és azt szeretné, hogy a dinamikus útválasztási tooenable, kell tooknow hello BGP társviszony-létesítés IP-cím és hello, szám, amely a helyi VPN-eszközt használ.</span><span class="sxs-lookup"><span data-stu-id="85cd9-183">If your local VPN device supports hello BGP and you want tooenable dynamic routing, you need tooknow hello BGP peering IP and hello AS number that your local VPN device uses.</span></span>

  ```powershell
  $localVPNPublicIP = "<Public IP>"
  $localBGPPeeringIP = "<Private IP for hello BGP session>"
  $localBGPASN = "<ASN>"
  $localAddressPrefix = $localBGPPeeringIP + "/32"
  $localVpn = New-AzureRmLocalNetworkGateway -Name "LocalVPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -GatewayIpAddress $localVPNPublicIP -AddressPrefix $localAddressPrefix -BgpPeeringAddress $localBGPPeeringIP -Asn $localBGPASN
  ```
8. <span data-ttu-id="85cd9-184">A helyi VPN-eszköz tooconnect toohello új Azure VPN átjáró konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="85cd9-184">Configure your local VPN device tooconnect toohello new Azure VPN gateway.</span></span> <span data-ttu-id="85cd9-185">A VPN-eszköz konfigurálásával kapcsolatos további információkért lásd: [VPN-eszköz konfigurálása](../vpn-gateway/vpn-gateway-about-vpn-devices.md).</span><span class="sxs-lookup"><span data-stu-id="85cd9-185">For more information about VPN device configuration, see [VPN Device Configuration](../vpn-gateway/vpn-gateway-about-vpn-devices.md).</span></span>
9. <span data-ttu-id="85cd9-186">Hivatkozás hello telephelyek közötti VPN átjáró Azure toohello helyi átjáró.</span><span class="sxs-lookup"><span data-stu-id="85cd9-186">Link hello Site-to-Site VPN gateway on Azure toohello local gateway.</span></span>

  ```powershell
  $azureVpn = Get-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName
  New-AzureRmVirtualNetworkGatewayConnection -Name "VPNConnection" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -VirtualNetworkGateway1 $azureVpn -LocalNetworkGateway2 $localVpn -ConnectionType IPsec -SharedKey <yourkey>
  ```

## <span data-ttu-id="85cd9-187"><a name="add"></a>egy már meglévő virtuális hálózatot tooconfigure vizsgálatát a kísérő kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="85cd9-187"><a name="add"></a>tooconfigure coexisting connections for an already existing VNet</span></span>
<span data-ttu-id="85cd9-188">Ha egy meglévő virtuális hálózattal rendelkezik, ellenőrizze a hello átjáró alhálózati méretét.</span><span class="sxs-lookup"><span data-stu-id="85cd9-188">If you have an existing virtual network, check hello gateway subnet size.</span></span> <span data-ttu-id="85cd9-189">Ha hello átjáróalhálózatot /28 vagy /29, először hello virtuális hálózati átjáró törlése és hello átjáró alhálózati méretének növelése.</span><span class="sxs-lookup"><span data-stu-id="85cd9-189">If hello gateway subnet is /28 or /29, you must first delete hello virtual network gateway and increase hello gateway subnet size.</span></span> <span data-ttu-id="85cd9-190">hello lépésekből ebben a szakaszban megtudhatja, hogyan toodo, amely.</span><span class="sxs-lookup"><span data-stu-id="85cd9-190">hello steps in this section show you how toodo that.</span></span>

<span data-ttu-id="85cd9-191">Ha hello átjáróalhálózatot /27 vagy nagyobb és hello virtuális hálózati ExpressRoute keresztül csatlakozik, hello lépéseket kihagyhatja, és folytassa túl["6. lépés – --webhelyek közötti VPN átjáró létrehozása"](#vpngw) hello előző szakaszban.</span><span class="sxs-lookup"><span data-stu-id="85cd9-191">If hello gateway subnet is /27 or larger and hello virtual network is connected via ExpressRoute, you can skip hello steps below and proceed too["Step 6 - Create a Site-to-Site VPN gateway"](#vpngw) in hello previous section.</span></span> 

> [!NOTE]
> <span data-ttu-id="85cd9-192">Meglévő átjáró hello törlésekor a helyi helyi hello kapcsolat tooyour virtuális hálózati elvész, ezt a konfigurációt a munka során.</span><span class="sxs-lookup"><span data-stu-id="85cd9-192">When you delete hello existing gateway, your local premises will lose hello connection tooyour virtual network while you are working on this configuration.</span></span> 
> 
> 

1. <span data-ttu-id="85cd9-193">Tooinstall hello legújabb verziójára hello Azure PowerShell-parancsmagok lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="85cd9-193">You'll need tooinstall hello latest version of hello Azure PowerShell cmdlets.</span></span> <span data-ttu-id="85cd9-194">Parancsmagok telepítésével kapcsolatos további információkért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="85cd9-194">For more information about installing cmdlets, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span> <span data-ttu-id="85cd9-195">lehet, hogy mi, előfordulhat, hogy ismernie kell a kis mértékben eltérő hello parancsmagok, amelyekkel ehhez a konfigurációhoz.</span><span class="sxs-lookup"><span data-stu-id="85cd9-195">hello cmdlets that you use for this configuration may be slightly different than what you might be familiar with.</span></span> <span data-ttu-id="85cd9-196">Adható meg, hogy toouse hello parancsmagok ezeket az utasításokat.</span><span class="sxs-lookup"><span data-stu-id="85cd9-196">Be sure toouse hello cmdlets specified in these instructions.</span></span> 
2. <span data-ttu-id="85cd9-197">Hello meglévő expressroute-on vagy a telephelyek közötti VPN-átjáró törlése.</span><span class="sxs-lookup"><span data-stu-id="85cd9-197">Delete hello existing ExpressRoute or Site-to-Site VPN gateway.</span></span>

  ```powershell 
  Remove-AzureRmVirtualNetworkGateway -Name <yourgatewayname> -ResourceGroupName <yourresourcegroup>
  ```
3. <span data-ttu-id="85cd9-198">Törölje az átjáró-alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="85cd9-198">Delete Gateway Subnet.</span></span>

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -Name <yourvnetname> -ResourceGroupName <yourresourcegroup> Remove-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet
  ```
4. <span data-ttu-id="85cd9-199">Adjon hozzá egy átjáró-alhálózatot, amely legfeljebb /27.</span><span class="sxs-lookup"><span data-stu-id="85cd9-199">Add a Gateway Subnet that is /27 or larger.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="85cd9-200">Ha még nem rendelkezik elegendő a virtuális hálózati tooincrease hello átjáró alhálózat méretét az IP-címek, meg kell tooadd további IP-címterület.</span><span class="sxs-lookup"><span data-stu-id="85cd9-200">If you don't have enough IP addresses left in your virtual network tooincrease hello gateway subnet size, you need tooadd more IP address space.</span></span>
   > 
   > 

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -Name <yourvnetname> -ResourceGroupName <yourresourcegroup>
  Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet -AddressPrefix "10.200.255.0/24"
  ```
   
    <span data-ttu-id="85cd9-201">Hello VNet-konfiguráció mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="85cd9-201">Save hello VNet configuration.</span></span>

  ```powershell
  $vnet = Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```
5. <span data-ttu-id="85cd9-202">Ezen a ponton egy átjáró nélküli VNettel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="85cd9-202">At this point, you have a VNet with no gateways.</span></span> <span data-ttu-id="85cd9-203">új toocreate-átjárók és a kapcsolatokat, folytassa a [4. lépés – hozzon létre egy ExpressRoute-átjáró](#gw), míg a talált a fenti lépéseket hello.</span><span class="sxs-lookup"><span data-stu-id="85cd9-203">toocreate new gateways and complete your connections, you can proceed with [Step 4 - Create an ExpressRoute gateway](#gw), found in hello preceding set of steps.</span></span>

## <a name="tooadd-point-to-site-configuration-toohello-vpn-gateway"></a><span data-ttu-id="85cd9-204">tooadd pont-hely konfigurációs toohello VPN-átjáró</span><span class="sxs-lookup"><span data-stu-id="85cd9-204">tooadd point-to-site configuration toohello VPN gateway</span></span>
<span data-ttu-id="85cd9-205">A lépésekkel hello tooadd pont-hely konfigurációs tooyour VPN-átjáró egy létezzenek beállítás alatt.</span><span class="sxs-lookup"><span data-stu-id="85cd9-205">You can follow hello steps below tooadd Point-to-Site configuration tooyour VPN gateway in a co-existence setup.</span></span>

1. <span data-ttu-id="85cd9-206">Adja hozzá a VPN-ügyfél címterét.</span><span class="sxs-lookup"><span data-stu-id="85cd9-206">Add VPN Client address pool.</span></span>

  ```powershell
  $azureVpn = Get-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName
  Set-AzureRmVirtualNetworkGatewayVpnClientConfig -VirtualNetworkGateway $azureVpn -VpnClientAddressPool "10.251.251.0/24"
  ```
2. <span data-ttu-id="85cd9-207">Töltse fel a hello VPN legfelső szintű tanúsítvány tooAzure a VPN-átjárót.</span><span class="sxs-lookup"><span data-stu-id="85cd9-207">Upload hello VPN root certificate tooAzure for your VPN gateway.</span></span> <span data-ttu-id="85cd9-208">Ebben a példában feltételezzük hello legfelső szintű tanúsítvány a következő PowerShell-parancsmagok hello futtatják hello helyi számítógép tárolja.</span><span class="sxs-lookup"><span data-stu-id="85cd9-208">In this example, it's assumed that hello root certificate is stored in hello local machine where hello following PowerShell cmdlets are run.</span></span>

  ```powershell
  $p2sCertFullName = "RootErVpnCoexP2S.cer" 
  $p2sCertMatchName = "RootErVpnCoexP2S" 
  $p2sCertToUpload=get-childitem Cert:\CurrentUser\My | Where-Object {$_.Subject -match $p2sCertMatchName} 
  if ($p2sCertToUpload.count -eq 1){write-host "cert found"} else {write-host "cert not found" exit} 
  $p2sCertData = [System.Convert]::ToBase64String($p2sCertToUpload.RawData) Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $p2sCertFullName -VirtualNetworkGatewayname $azureVpn.Name -ResourceGroupName $resgrp.ResourceGroupName -PublicCertData $p2sCertData
  ```

<span data-ttu-id="85cd9-209">A pont-hely VPN-ekkel kapcsolatos további információkért lásd: [Pont-hely kapcsolat konfigurálása](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="85cd9-209">For more information on Point-to-Site VPN, see [Configure a Point-to-Site connection](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="85cd9-210">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="85cd9-210">Next steps</span></span>
<span data-ttu-id="85cd9-211">ExpressRoute kapcsolatos további információkért lásd: hello [ExpressRoute – gyakori kérdések](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="85cd9-211">For more information about ExpressRoute, see hello [ExpressRoute FAQ](expressroute-faqs.md).</span></span>
