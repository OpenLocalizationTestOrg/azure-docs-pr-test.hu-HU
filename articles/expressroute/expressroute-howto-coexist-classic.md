---
title: "Párhuzamosan fennálló ExpressRoute- és helyek közötti VPN-kapcsolatok konfigurálása: klasszikus: Azure | Microsoft Docs"
description: "A cikk bemutatja az ExpressRoute- és egy helyek közötti VPN-kapcsolat konfigurálását, amelyek párhuzamosan használhatók a klasszikus üzembehelyezési modellben."
documentationcenter: na
services: expressroute
author: charwen
manager: carmonm
editor: 
tags: azure-service-management
ms.assetid: dcf1a5af-a289-466a-b812-0bfedbd2bda0
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/21/2017
ms.author: charwen
ms.openlocfilehash: 09d1649f0ca0cf4ca464d95b29461cad3fe51788
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="configure-expressroute-and-site-to-site-coexisting-connections-classic"></a><span data-ttu-id="a3529-103">Párhuzamos ExpressRoute- és párhuzamos helyek közötti kapcsolatok konfigurálása (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="a3529-103">Configure ExpressRoute and Site-to-Site coexisting connections (classic)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="a3529-104">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="a3529-104">PowerShell - Resource Manager</span></span>](expressroute-howto-coexist-resource-manager.md)
> * [<span data-ttu-id="a3529-105">PowerShell – Klasszikus</span><span class="sxs-lookup"><span data-stu-id="a3529-105">PowerShell - Classic</span></span>](expressroute-howto-coexist-classic.md)
> 
> 

<span data-ttu-id="a3529-106">A helyek közötti VPN és az ExpressRoute konfigurálásának lehetősége több előnnyel jár.</span><span class="sxs-lookup"><span data-stu-id="a3529-106">Having the ability to configure Site-to-Site VPN and ExpressRoute has several advantages.</span></span> <span data-ttu-id="a3529-107">A helyek közötti VPN-t konfigurálhatja biztonságos feladatátvételi útvonalként az ExpressRoute számára, vagy használhat helyek közötti VPN-eket is a nem ExpressRoute-on keresztül kapcsolódó helyekhez való csatlakozáshoz.</span><span class="sxs-lookup"><span data-stu-id="a3529-107">You can configure Site-to-Site VPN as a secure failover path for ExressRoute, or use Site-to-Site VPNs to connect to sites that are not connected through ExpressRoute.</span></span> <span data-ttu-id="a3529-108">A cikkben mindkét forgatókönyv lépéseit ismertetjük.</span><span class="sxs-lookup"><span data-stu-id="a3529-108">We will cover the steps to configure both scenarios in this article.</span></span> <span data-ttu-id="a3529-109">Ez a cikk a klasszikus üzembehelyezési modellre vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="a3529-109">This article applies to the classic deployment model.</span></span> <span data-ttu-id="a3529-110">Ez a konfiguráció a portálon nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="a3529-110">This configuration is not available in the portal.</span></span>

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

<span data-ttu-id="a3529-111">**Tudnivalók az Azure üzembehelyezési modellekről**</span><span class="sxs-lookup"><span data-stu-id="a3529-111">**About Azure deployment models**</span></span>

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

> [!IMPORTANT]
> <span data-ttu-id="a3529-112">Az ExpressRoute-kapcsolatcsoportokat előre konfigurálni kell, mielőtt végrehajtaná az alábbi utasításokat.</span><span class="sxs-lookup"><span data-stu-id="a3529-112">ExpressRoute circuits must be pre-configured before you follow the instructions below.</span></span> <span data-ttu-id="a3529-113">Mielőtt folytatná az alábbi lépésekkel, az útmutatásoknak megfelelően [hozzon létre egy ExpressRoute-kapcsolatcsoportot](expressroute-howto-circuit-classic.md) és [konfigurálja az útválasztást](expressroute-howto-routing-classic.md).</span><span class="sxs-lookup"><span data-stu-id="a3529-113">Make sure that you have followed the guides to [create an ExpressRoute circuit](expressroute-howto-circuit-classic.md) and [configure routing](expressroute-howto-routing-classic.md) before you follow the steps below.</span></span>
> 
> 

## <a name="limits-and-limitations"></a><span data-ttu-id="a3529-114">Korlátok és korlátozások</span><span class="sxs-lookup"><span data-stu-id="a3529-114">Limits and limitations</span></span>
* <span data-ttu-id="a3529-115">**A tranzit útválasztás nem támogatott.**</span><span class="sxs-lookup"><span data-stu-id="a3529-115">**Transit routing is not supported.**</span></span> <span data-ttu-id="a3529-116">Nem hajthat végre útválasztást (az Azure-on keresztül) a helyek közötti VPN használatával csatlakoztatott helyi hálózat és az ExpressRoute használatával csatlakoztatott helyi hálózat között.</span><span class="sxs-lookup"><span data-stu-id="a3529-116">You cannot route (via Azure) between your local network connected via Site-to-Site VPN and your local network connected via ExpressRoute.</span></span>
* <span data-ttu-id="a3529-117">**A pont–hely kapcsolat nem támogatott.**</span><span class="sxs-lookup"><span data-stu-id="a3529-117">**Point-to-site is not supported.**</span></span> <span data-ttu-id="a3529-118">Pont–hely típusú VPN-kapcsolatok nem engedélyezhetők az ExpressRoute-hoz csatlakozó VNet felé.</span><span class="sxs-lookup"><span data-stu-id="a3529-118">You can't enable point-to-site VPN connections to the same VNet that is connected to ExpressRoute.</span></span> <span data-ttu-id="a3529-119">A Pont–hely VPN és az ExpressRoute nem létezhet ugyanazon VNeten belül.</span><span class="sxs-lookup"><span data-stu-id="a3529-119">Point-to-site VPN and ExpressRoute cannot coexist for the same VNet.</span></span>
* <span data-ttu-id="a3529-120">**A kényszerített bújtatás nem engedélyezhető a helyek közötti VPN-átjárón.**</span><span class="sxs-lookup"><span data-stu-id="a3529-120">**Forced tunneling cannot be enabled on the Site-to-Site VPN gateway.**</span></span> <span data-ttu-id="a3529-121">Az internetes forgalmat csak a helyszíni hálózatra „kényszerítheti” az ExpressRoute-on keresztül.</span><span class="sxs-lookup"><span data-stu-id="a3529-121">You can only "force" all Internet-bound traffic back to your on-premises network via ExpressRoute.</span></span>
* <span data-ttu-id="a3529-122">**Az alapszintű termékváltozat átjárója nem támogatott.**</span><span class="sxs-lookup"><span data-stu-id="a3529-122">**Basic SKU gateway is not supported.**</span></span> <span data-ttu-id="a3529-123">Nem Basic SKU-átjárót kell használnia [ExpressRoute-](expressroute-about-virtual-network-gateways.md) és [VPN-átjáróként](../vpn-gateway/vpn-gateway-about-vpngateways.md).</span><span class="sxs-lookup"><span data-stu-id="a3529-123">You must use a non-Basic SKU gateway for both the [ExpressRoute gateway](expressroute-about-virtual-network-gateways.md) and the [VPN gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).</span></span>
* <span data-ttu-id="a3529-124">**Kizárólag az útvonalalapú VPN-átjáró támogatott.**</span><span class="sxs-lookup"><span data-stu-id="a3529-124">**Only route-based VPN gateway is supported.**</span></span> <span data-ttu-id="a3529-125">Útvonalalapú [VPN-átjárót](../vpn-gateway/vpn-gateway-about-vpngateways.md) kell használnia.</span><span class="sxs-lookup"><span data-stu-id="a3529-125">You must use a route-based [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).</span></span>
* <span data-ttu-id="a3529-126">**A VPN-átjáróhoz statikus útvonalat kell konfigurálni.**</span><span class="sxs-lookup"><span data-stu-id="a3529-126">**Static route should be configured for your VPN gateway.**</span></span> <span data-ttu-id="a3529-127">Ha a helyi hálózat az ExpressRoute-hoz és a helyek közötti VPN-hez is csatlakozik, konfigurálnia kell egy statikus útvonalat a helyi hálózaton a helyek közötti VPN-kapcsolat a nyilvános internetre történő átirányításához.</span><span class="sxs-lookup"><span data-stu-id="a3529-127">If your local network is connected to both ExpressRoute and a Site-to-Site VPN, you must have a static route configured in your local network to route the Site-to-Site VPN connection to the public Internet.</span></span>
* <span data-ttu-id="a3529-128">**Elsőként az ExpressRoute-átjárót kell konfigurálnia.**</span><span class="sxs-lookup"><span data-stu-id="a3529-128">**ExpressRoute gateway must be configured first.**</span></span> <span data-ttu-id="a3529-129">Először az ExpressRoute-átjárót kell létrehoznia, mielőtt felvenné a helyek közötti VPN-átjárót.</span><span class="sxs-lookup"><span data-stu-id="a3529-129">You must create the ExpressRoute gateway first before you add the Site-to-Site VPN gateway.</span></span>

## <a name="configuration-designs"></a><span data-ttu-id="a3529-130">Konfigurációs tervek</span><span class="sxs-lookup"><span data-stu-id="a3529-130">Configuration designs</span></span>
### <a name="configure-a-site-to-site-vpn-as-a-failover-path-for-expressroute"></a><span data-ttu-id="a3529-131">Helyek közötti VPN konfigurálása feladatátvételi útvonalként az ExpressRoute számára</span><span class="sxs-lookup"><span data-stu-id="a3529-131">Configure a Site-to-Site VPN as a failover path for ExpressRoute</span></span>
<span data-ttu-id="a3529-132">Konfigurálhat egy helyek közötti VPN-kapcsolatot tartalékként az ExpressRoute számára.</span><span class="sxs-lookup"><span data-stu-id="a3529-132">You can configure a Site-to-Site VPN connection as a backup for ExpressRoute.</span></span> <span data-ttu-id="a3529-133">Ez csak az Azure privát társviszony-létesítési útvonalhoz társított virtuális hálózatokra vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="a3529-133">This applies only to virtual networks linked to the Azure private peering path.</span></span> <span data-ttu-id="a3529-134">Az Azure nyilvános és a Microsoft társviszony-létesítésekhez nem létezik VPN-alapú feladatátvételi megoldás.</span><span class="sxs-lookup"><span data-stu-id="a3529-134">There is no VPN-based failover solution for services accessible through Azure public and Microsoft peerings.</span></span> <span data-ttu-id="a3529-135">Minden esetben az ExpressRoute-kapcsolatcsoport az elsődleges kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="a3529-135">The ExpressRoute circuit is always the primary link.</span></span> <span data-ttu-id="a3529-136">Az adatok csak akkor lesznek a helyek közötti VPN-útvonalon továbbítva, ha az ExpressRoute-kapcsolatcsoport meghibásodik.</span><span class="sxs-lookup"><span data-stu-id="a3529-136">Data will flow through the Site-to-Site VPN path only if the ExpressRoute circuit fails.</span></span> 

> [!NOTE]
> <span data-ttu-id="a3529-137">Bár a rendszer az ExpressRoute-kapcsolatcsoportot részesíti előnyben a helyek közötti VPN helyett, ha az útvonalak megegyeznek, az Azure a leghosszabb előtag-megfeleltetéssel választja ki a célcsomag útvonalát.</span><span class="sxs-lookup"><span data-stu-id="a3529-137">While ExpressRoute circuit is preferred over Site-to-Site VPN when both routes are the same, Azure will use the longest prefix match to choose the route towards the packet's destination.</span></span>
> 
> 

![Egyidejű jelenlét](media/expressroute-howto-coexist-classic/scenario1.jpg)

### <a name="configure-a-site-to-site-vpn-to-connect-to-sites-not-connected-through-expressroute"></a><span data-ttu-id="a3529-139">Helyek közötti VPN konfigurálása az ExpressRoute használatával nem csatlakozó helyekhez</span><span class="sxs-lookup"><span data-stu-id="a3529-139">Configure a Site-to-Site VPN to connect to sites not connected through ExpressRoute</span></span>
<span data-ttu-id="a3529-140">A hálózatát konfigurálhatja úgy is, hogy egyes helyek közvetlenül az Azure-hoz kapcsolódnak helyek közötti VPN-en keresztül, míg más helyek az ExpressRoute használatával kapcsolódnak.</span><span class="sxs-lookup"><span data-stu-id="a3529-140">You can configure your network where some sites connect directly to Azure over Site-to-Site VPN, and some sites connect through ExpressRoute.</span></span> 

![Egyidejű jelenlét](media/expressroute-howto-coexist-classic/scenario2.jpg)

> [!NOTE]
> <span data-ttu-id="a3529-142">Virtuális hálózatot nem konfigurálhat tranzit útválasztóként.</span><span class="sxs-lookup"><span data-stu-id="a3529-142">You cannot a configure a virtual network as a transit router.</span></span>
> 
> 

## <a name="selecting-the-steps-to-use"></a><span data-ttu-id="a3529-143">A használni kívánt lépések kiválasztása</span><span class="sxs-lookup"><span data-stu-id="a3529-143">Selecting the steps to use</span></span>
<span data-ttu-id="a3529-144">Két különböző eljáráscsoport közül választhat az egyidejűleg használható kapcsolatok konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="a3529-144">There are two different sets of procedures to choose from in order to configure connections that can coexist.</span></span> <span data-ttu-id="a3529-145">A konfigurálás választott módja attól függ, hogy rendelkezik-e meglévő virtuális hálózattal, amelyhez csatlakozni szeretne, vagy egy új virtuális hálózatot szeretne létrehozni.</span><span class="sxs-lookup"><span data-stu-id="a3529-145">The configuration procedure that you select will depend on whether you have an existing virtual network that you want to connect to, or you want to create a new virtual network.</span></span>

* <span data-ttu-id="a3529-146">Nem rendelkezem VNettel, és létre kell hoznom egyet.</span><span class="sxs-lookup"><span data-stu-id="a3529-146">I don't have a VNet and need to create one.</span></span>
  
    <span data-ttu-id="a3529-147">Ha még nem rendelkezik virtuális hálózattal, ez az eljárás lépésről lépésre végigvezeti az új virtuális hálózat létrehozásának folyamatán a klasszikus üzembehelyezési modellnek megfelelően, valamint az új ExpressRoute- és helyek közötti VPN-kapcsolatok létrehozásának folyamatán.</span><span class="sxs-lookup"><span data-stu-id="a3529-147">If you don’t already have a virtual network, this procedure will walk you through creating a new virtual network using the classic deployment model and creating new ExpressRoute and Site-to-Site VPN connections.</span></span> <span data-ttu-id="a3529-148">A konfiguráláshoz kövesse a cikk az [Új virtuális hálózat és egyidejű kapcsolatok létrehozása](#new) szakaszában foglalt lépéseket.</span><span class="sxs-lookup"><span data-stu-id="a3529-148">To configure, follow the steps in the article section [To create a new virtual network and coexisting connections](#new).</span></span>
* <span data-ttu-id="a3529-149">Már rendelkezem egy klasszikus üzembehelyezési modell szerinti VNettel.</span><span class="sxs-lookup"><span data-stu-id="a3529-149">I already have a classic deployment model VNet.</span></span>
  
    <span data-ttu-id="a3529-150">Előfordulhat, hogy már rendelkezik üzemelő virtuális hálózattal egy létező helyek közötti VPN- vagy ExpressRoute-kapcsolattal.</span><span class="sxs-lookup"><span data-stu-id="a3529-150">You may already have a virtual network in place with an existing Site-to-Site VPN connection or ExpressRoute connection.</span></span> <span data-ttu-id="a3529-151">A cikk [Egyidejű kapcsolatok konfigurálása meglévő VNet számára](#add) szakasza végigvezeti az átjáró törlésének, majd az új ExpressRoute- és helyek között VPN-kapcsolatok létrehozásának folyamatán.</span><span class="sxs-lookup"><span data-stu-id="a3529-151">The article section [To configure coexsiting connections for an already existing VNet](#add) will walk you through deleting the gateway, and then creating new ExpressRoute and Site-to-Site VPN connections.</span></span> <span data-ttu-id="a3529-152">Ügyeljen arra, hogy az új kapcsolatok létrehozásakor a lépéseket szigorú sorrendben kell végrehajtani.</span><span class="sxs-lookup"><span data-stu-id="a3529-152">Note that when creating the new connections, the steps must be completed in a very specific order.</span></span> <span data-ttu-id="a3529-153">Az átjárók és kapcsolatok létrehozására ne használja más cikkek utasításait.</span><span class="sxs-lookup"><span data-stu-id="a3529-153">Don't use the instructions in other articles to create your gateways and connections.</span></span>
  
    <span data-ttu-id="a3529-154">Ebben az eljárásban az egyidejű kapcsolatok létrehozásához törölnie kell az átjárót, majd új átjárókat kell konfigurálnia.</span><span class="sxs-lookup"><span data-stu-id="a3529-154">In this procedure, creating connections that can coexist will require you to delete your gateway, and then configure new gateways.</span></span> <span data-ttu-id="a3529-155">Ez azt jelenti, hogy a helyszínek közötti kapcsolatok esetében állásidővel kell számolnia, amíg törli és újra létrehozza az átjárót és a kapcsolatokat, azonban a virtuális gépeket és a szolgáltatásokat nem kell áttelepítenie egy új virtuális hálózatra.</span><span class="sxs-lookup"><span data-stu-id="a3529-155">This means you will have downtime for your cross-premises connections while you delete and recreate your gateway and connections, but you will not need to migrate any of your VMs or services to a new virtual network.</span></span> <span data-ttu-id="a3529-156">Ha megfelelően vannak konfigurálva, a virtuális gépek és a szolgáltatások továbbra is képesek lesznek kommunikálni a terheléselosztón keresztül az átjáró konfigurálása közben.</span><span class="sxs-lookup"><span data-stu-id="a3529-156">Your VMs and services will still be able to communicate out through the load balancer while you configure your gateway if they are configured to do so.</span></span>

## <span data-ttu-id="a3529-157"><a name="new"></a>Új virtuális hálózat és egyidejű kapcsolatok létrehozása</span><span class="sxs-lookup"><span data-stu-id="a3529-157"><a name="new"></a>To create a new virtual network and coexisting connections</span></span>
<span data-ttu-id="a3529-158">Az eljárás a VNetek, valamint az egyidejűleg jelenlévő helyek közötti és ExpressRoute-kapcsolatok létrehozásának módját ismerteti.</span><span class="sxs-lookup"><span data-stu-id="a3529-158">This procedure will walk you through creating a VNet and create Site-to-Site and ExpressRoute connections that will coexist.</span></span>

1. <span data-ttu-id="a3529-159">Az Azure PowerShell-parancsmagok legújabb verzióit kell telepítenie.</span><span class="sxs-lookup"><span data-stu-id="a3529-159">You'll need to install the latest version of the Azure PowerShell cmdlets.</span></span> <span data-ttu-id="a3529-160">A PowerShell-parancsmagok telepítésével kapcsolatban további információ: [Az Azure PowerShell telepítése és konfigurálása](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="a3529-160">See [How to install and configure Azure PowerShell](/powershell/azure/overview) for more information about installing the PowerShell cmdlets.</span></span> <span data-ttu-id="a3529-161">Vegye figyelembe, hogy az ehhez a konfigurációhoz használt parancsmagok eltérőek lehetnek az Ön által már ismertektől.</span><span class="sxs-lookup"><span data-stu-id="a3529-161">Note that the cmdlets that you'll use for this configuration may be slightly different than what you might be familiar with.</span></span> <span data-ttu-id="a3529-162">Ügyeljen arra, hogy az ebben az útmutatóban meghatározott parancsmagokat használja.</span><span class="sxs-lookup"><span data-stu-id="a3529-162">Be sure to use the cmdlets specified in these instructions.</span></span> 
2. <span data-ttu-id="a3529-163">Hozzon létre egy sémát a virtuális hálózat számára.</span><span class="sxs-lookup"><span data-stu-id="a3529-163">Create a schema for your virtual network.</span></span> <span data-ttu-id="a3529-164">Az új konfigurációs sémával kapcsolatos információkért lásd: [Azure Virtual Network konfigurációs séma](https://msdn.microsoft.com/library/azure/jj157100.aspx).</span><span class="sxs-lookup"><span data-stu-id="a3529-164">For more information about the configuration schema, see [Azure Virtual Network configuration schema](https://msdn.microsoft.com/library/azure/jj157100.aspx).</span></span>
   
    <span data-ttu-id="a3529-165">Amikor létrehozza a sémát, mindenképp a következő értékeket használja:</span><span class="sxs-lookup"><span data-stu-id="a3529-165">When you create your schema, make sure you use the following values:</span></span>
   
   * <span data-ttu-id="a3529-166">A virtuális hálózat átjáró-alhálózata /27 vagy egy rövidebb előtag kell legyen (például /26 vagy /25).</span><span class="sxs-lookup"><span data-stu-id="a3529-166">The gateway subnet for the virtual network must be /27 or a shorter prefix (such as /26 or /25).</span></span>
   * <span data-ttu-id="a3529-167">Az átjáró kapcsolattípusa: „Dedikált”.</span><span class="sxs-lookup"><span data-stu-id="a3529-167">The gateway connection type is "Dedicated".</span></span>
     
             <VirtualNetworkSite name="MyAzureVNET" Location="Central US">
               <AddressSpace>
                 <AddressPrefix>10.17.159.192/26</AddressPrefix>
               </AddressSpace>
               <Subnets>
                 <Subnet name="Subnet-1">
                   <AddressPrefix>10.17.159.192/27</AddressPrefix>
                 </Subnet>
                 <Subnet name="GatewaySubnet">
                   <AddressPrefix>10.17.159.224/27</AddressPrefix>
                 </Subnet>
               </Subnets>
               <Gateway>
                 <ConnectionsToLocalNetwork>
                   <LocalNetworkSiteRef name="MyLocalNetwork">
                     <Connection type="Dedicated" />
                   </LocalNetworkSiteRef>
                 </ConnectionsToLocalNetwork>
               </Gateway>
             </VirtualNetworkSite>
3. <span data-ttu-id="a3529-168">Az xml-sémafájl létrehozása és konfigurálása után töltse fel a fájlt.</span><span class="sxs-lookup"><span data-stu-id="a3529-168">After creating and configuring your xml schema file, upload the file.</span></span> <span data-ttu-id="a3529-169">Ezzel létrejön a virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="a3529-169">This will create your virtual network.</span></span>
   
    <span data-ttu-id="a3529-170">A fájl feltöltéséhez használja a következő parancsmagot, és cserélje le az értéket saját értékre.</span><span class="sxs-lookup"><span data-stu-id="a3529-170">Use the following cmdlet to upload your file, replacing the value with your own.</span></span>
   
        Set-AzureVNetConfig -ConfigurationPath 'C:\NetworkConfig.xml'
4. <span data-ttu-id="a3529-171"><a name="gw"></a>Hozzon létre egy ExpressRoute-átjárót.</span><span class="sxs-lookup"><span data-stu-id="a3529-171"><a name="gw"></a>Create an ExpressRoute gateway.</span></span> <span data-ttu-id="a3529-172">Ne felejtse megadni a GatewaySKU paraméterben a *Standard*, a *HighPerformance* vagy az *UltraPerformance* értéket, a GatewayType paraméterben pedig a *DynamicRouting* értéket.</span><span class="sxs-lookup"><span data-stu-id="a3529-172">Be sure to specify the GatewaySKU as *Standard*, *HighPerformance*, or *UltraPerformance* and the GatewayType as *DynamicRouting*.</span></span>
   
    <span data-ttu-id="a3529-173">Használja a következő mintát, és cserélje le az értékeket saját értékekre.</span><span class="sxs-lookup"><span data-stu-id="a3529-173">Use the following sample, substituting the values for your own.</span></span>
   
        New-AzureVNetGateway -VNetName MyAzureVNET -GatewayType DynamicRouting -GatewaySKU HighPerformance
5. <span data-ttu-id="a3529-174">Csatlakoztassa az ExpressRoute-átjárót az ExpressRoute-kapcsolatcsoporthoz.</span><span class="sxs-lookup"><span data-stu-id="a3529-174">Link the ExpressRoute gateway to the ExpressRoute circuit.</span></span> <span data-ttu-id="a3529-175">A lépés végrehajtásával létrejön a kapcsolat a helyszíni hálózat és az Azure között az ExpressRoute-on keresztül.</span><span class="sxs-lookup"><span data-stu-id="a3529-175">After this step has been completed, the connection between your on-premises network and Azure, through ExpressRoute, is established.</span></span>
   
        New-AzureDedicatedCircuitLink -ServiceKey <service-key> -VNetName MyAzureVNET
6. <span data-ttu-id="a3529-176"><a name="vpngw"></a>Ezután hozza létre a webhelyek közötti VPN-átjárót.</span><span class="sxs-lookup"><span data-stu-id="a3529-176"><a name="vpngw"></a>Next, create your Site-to-Site VPN gateway.</span></span> <span data-ttu-id="a3529-177">A GatewaySKU paraméterben a *Standard*, a *HighPerformance* vagy az *UltraPerformance* értéket, a GatewayType paraméterben pedig a *DynamicRouting* értéket kell megadnia.</span><span class="sxs-lookup"><span data-stu-id="a3529-177">The GatewaySKU must be *Standard*, *HighPerformance*, or *UltraPerformance* and the GatewayType must be *DynamicRouting*.</span></span>
   
        New-AzureVirtualNetworkGateway -VNetName MyAzureVNET -GatewayName S2SVPN -GatewayType DynamicRouting -GatewaySKU  HighPerformance
   
    <span data-ttu-id="a3529-178">A virtuális hálózati átjáró beállításainak, köztük az átjáróazonosítónak és a nyilvános IP-címnek a lekéréséhez használja a `Get-AzureVirtualNetworkGateway` parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="a3529-178">To retrieve the virtual network gateway settings, including the gateway ID and the public IP, use the `Get-AzureVirtualNetworkGateway` cmdlet.</span></span>
   
        Get-AzureVirtualNetworkGateway
   
        GatewayId            : 348ae011-ffa9-4add-b530-7cb30010565e
        GatewayName          : S2SVPN
        LastEventData        :
        GatewayType          : DynamicRouting
        LastEventTimeStamp   : 5/29/2015 4:41:41 PM
        LastEventMessage     : Successfully created a gateway for the following virtual network: GNSDesMoines
        LastEventID          : 23002
        State                : Provisioned
        VIPAddress           : 104.43.x.y
        DefaultSite          :
        GatewaySKU           : HighPerformance
        Location             :
        VnetId               : 979aabcf-e47f-4136-ab9b-b4780c1e1bd5
        SubnetId             :
        EnableBgp            : False
        OperationDescription : Get-AzureVirtualNetworkGateway
        OperationId          : 42773656-85e1-a6b6-8705-35473f1e6f6a
        OperationStatus      : Succeeded
7. <span data-ttu-id="a3529-179">Hozzon létre egy helyi VPN-átjáró entitást.</span><span class="sxs-lookup"><span data-stu-id="a3529-179">Create a local site VPN gateway entity.</span></span> <span data-ttu-id="a3529-180">Ez a parancs nem konfigurálja a helyszíni VPN-átjárót.</span><span class="sxs-lookup"><span data-stu-id="a3529-180">This command doesn’t configure your on-premises VPN gateway.</span></span> <span data-ttu-id="a3529-181">Ehelyett a helyi átjáró beállításai, például a nyilvános IP-cím és a helyszíni címtér megadására szolgál, hogy az Azure VPN-átjáró kapcsolódhasson hozzá.</span><span class="sxs-lookup"><span data-stu-id="a3529-181">Rather, it allows you to provide the local gateway settings, such as the public IP and the on-premises address space, so that the Azure VPN gateway can connect to it.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="a3529-182">A netcfg nem határozza meg a helyek közötti VPN helyi helyét.</span><span class="sxs-lookup"><span data-stu-id="a3529-182">The local site for the Site-to-Site VPN is not defined in the netcfg.</span></span> <span data-ttu-id="a3529-183">Ez a parancsmag a helyi hely paramétereinek meghatározására szolgál.</span><span class="sxs-lookup"><span data-stu-id="a3529-183">Instead, you must use this cmdlet to specify the local site parameters.</span></span> <span data-ttu-id="a3529-184">Ez egyik portálon és a netcfg-fájlon keresztül sem határozható meg.</span><span class="sxs-lookup"><span data-stu-id="a3529-184">You cannot define it using either portal, or the netcfg file.</span></span>
   > 
   > 
   
    <span data-ttu-id="a3529-185">Használja a következő mintát, és cserélje le az értékeket saját értékekre.</span><span class="sxs-lookup"><span data-stu-id="a3529-185">Use the following sample, replacing the values with your own.</span></span>
   
        New-AzureLocalNetworkGateway -GatewayName MyLocalNetwork -IpAddress <MyLocalGatewayIp> -AddressSpace <MyLocalNetworkAddress>
   
   > [!NOTE]
   > <span data-ttu-id="a3529-186">Ha a helyi hálózat több útvonalat is tartalmaz, tömbként egyszerre mindet megadhatja.</span><span class="sxs-lookup"><span data-stu-id="a3529-186">If your local network has multiple routes, you can pass them all in as an array.</span></span>  <span data-ttu-id="a3529-187">$MyLocalNetworkAddress = @("10.1.2.0/24","10.1.3.0/24","10.2.1.0/24")</span><span class="sxs-lookup"><span data-stu-id="a3529-187">$MyLocalNetworkAddress = @("10.1.2.0/24","10.1.3.0/24","10.2.1.0/24")</span></span>  
   > 
   > 

    <span data-ttu-id="a3529-188">A virtuális hálózati átjáró beállításainak, köztük az átjáróazonosítónak és a nyilvános IP-címnek a lekéréséhez használja a `Get-AzureVirtualNetworkGateway` parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="a3529-188">To retrieve the virtual network gateway settings, including the gateway ID and the public IP, use the `Get-AzureVirtualNetworkGateway` cmdlet.</span></span> <span data-ttu-id="a3529-189">Tekintse meg a következő példát.</span><span class="sxs-lookup"><span data-stu-id="a3529-189">See the following example.</span></span>

        Get-AzureLocalNetworkGateway

        GatewayId            : 532cb428-8c8c-4596-9a4f-7ae3a9fcd01b
        GatewayName          : MyLocalNetwork
        IpAddress            : 23.39.x.y
        AddressSpace         : {10.1.2.0/24}
        OperationDescription : Get-AzureLocalNetworkGateway
        OperationId          : ddc4bfae-502c-adc7-bd7d-1efbc00b3fe5
        OperationStatus      : Succeeded


1. <span data-ttu-id="a3529-190">Konfigurálja helyi VPN-eszközét, hogy az új átjáróhoz csatlakozzon.</span><span class="sxs-lookup"><span data-stu-id="a3529-190">Configure your local VPN device to connect to the new gateway.</span></span> <span data-ttu-id="a3529-191">A VPN-eszköz konfigurálása során használja a 6. lépésben lekért információkat.</span><span class="sxs-lookup"><span data-stu-id="a3529-191">Use the information that you retrieved in step 6 when configuring your VPN device.</span></span> <span data-ttu-id="a3529-192">A VPN-eszköz konfigurálásával kapcsolatos további információkért lásd: [VPN-eszköz konfigurálása](../vpn-gateway/vpn-gateway-about-vpn-devices.md).</span><span class="sxs-lookup"><span data-stu-id="a3529-192">For more information about VPN device configuration, see [VPN Device Configuration](../vpn-gateway/vpn-gateway-about-vpn-devices.md).</span></span>
2. <span data-ttu-id="a3529-193">Csatlakoztassa az Azure-on a helyek közötti VPN-átjárót a helyi átjáróhoz.</span><span class="sxs-lookup"><span data-stu-id="a3529-193">Link the Site-to-Site VPN gateway on Azure to the local gateway.</span></span>
   
    <span data-ttu-id="a3529-194">Ebben a példában a connectedEntityId a helyi átjáró, amely a `Get-AzureLocalNetworkGateway` futtatásával azonosítható.</span><span class="sxs-lookup"><span data-stu-id="a3529-194">In this example, connectedEntityId is the local gateway ID, which you can find by running `Get-AzureLocalNetworkGateway`.</span></span> <span data-ttu-id="a3529-195">A virtualNetworkGatewayId a `Get-AzureVirtualNetworkGateway` parancsmag használatával azonosítható.</span><span class="sxs-lookup"><span data-stu-id="a3529-195">You can find virtualNetworkGatewayId by using the `Get-AzureVirtualNetworkGateway` cmdlet.</span></span> <span data-ttu-id="a3529-196">A lépés után létrejön a kapcsolat a helyszíni hálózat és az Azure között a helyek közötti VPN-kapcsolaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="a3529-196">After this step, the connection between your local network and Azure via the Site-to-Site VPN connection is established.</span></span>

        New-AzureVirtualNetworkGatewayConnection -connectedEntityId <local-network-gateway-id> -gatewayConnectionName Azure2Local -gatewayConnectionType IPsec -sharedKey abc123 -virtualNetworkGatewayId <azure-s2s-vpn-gateway-id>

## <span data-ttu-id="a3529-197"><a name="add"></a>Egyidejű kapcsolatok konfigurálása meglévő VNet számára</span><span class="sxs-lookup"><span data-stu-id="a3529-197"><a name="add"></a>To configure coexsiting connections for an already existing VNet</span></span>
<span data-ttu-id="a3529-198">Ha már rendelkezik meglévő virtuális hálózattal, ellenőrizze az átjáró-alhálózat méretét.</span><span class="sxs-lookup"><span data-stu-id="a3529-198">If you have an existing virtual network, check the gateway subnet size.</span></span> <span data-ttu-id="a3529-199">Ha az átjáró-alhálózat /28 vagy /29, először törölnie kell a virtuális hálózati átjárót, és növelnie kell az átjáró-alhálózat méretét.</span><span class="sxs-lookup"><span data-stu-id="a3529-199">If the gateway subnet is /28 or /29, you must first delete the virtual network gateway and increase the gateway subnet size.</span></span> <span data-ttu-id="a3529-200">A jelen szakaszban ismertetett lépések bemutatják, mindez hogyan valósítható meg.</span><span class="sxs-lookup"><span data-stu-id="a3529-200">The steps in this section will show you how to do that.</span></span>

<span data-ttu-id="a3529-201">Ha az átjáró-alhálózat /27 vagy nagyobb, és a virtuális hálózat ExpressRoute-on keresztül csatlakozik, kihagyhatja az alábbi lépéseket, és továbbléphet a [„6. lépés – Helyek közötti VPN-átjáró létrehozása”](#vpngw) lépésre az előző szakaszban.</span><span class="sxs-lookup"><span data-stu-id="a3529-201">If the gateway subnet is /27 or larger and the virtual network is connected via ExpressRoute, you can skip the steps below and proceed to ["Step 6 - Create a Site-to-Site VPN gateway"](#vpngw) in the previous section.</span></span>

> [!NOTE]
> <span data-ttu-id="a3529-202">Amikor törli a meglévő átjárót, megszakad a helyi helyszínek kapcsolata a virtuális hálózattal, amíg ezen a konfiguráción dolgozik.</span><span class="sxs-lookup"><span data-stu-id="a3529-202">When you delete the existing gateway, your local premises will lose the connection to your virtual network while you are working on this configuration.</span></span>
> 
> 

1. <span data-ttu-id="a3529-203">Az Azure Resource Manager PowerShell-parancsmagjainak legújabb verzióit kell telepítenie.</span><span class="sxs-lookup"><span data-stu-id="a3529-203">You'll need to install the latest version of the Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="a3529-204">A PowerShell-parancsmagok telepítésével kapcsolatban további információ: [Az Azure PowerShell telepítése és konfigurálása](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="a3529-204">See [How to install and configure Azure PowerShell](/powershell/azure/overview) for more information about installing the PowerShell cmdlets.</span></span> <span data-ttu-id="a3529-205">Vegye figyelembe, hogy az ehhez a konfigurációhoz használt parancsmagok eltérőek lehetnek az Ön által már ismertektől.</span><span class="sxs-lookup"><span data-stu-id="a3529-205">Note that the cmdlets that you'll use for this configuration may be slightly different than what you might be familiar with.</span></span> <span data-ttu-id="a3529-206">Ügyeljen arra, hogy az ebben az útmutatóban meghatározott parancsmagokat használja.</span><span class="sxs-lookup"><span data-stu-id="a3529-206">Be sure to use the cmdlets specified in these instructions.</span></span> 
2. <span data-ttu-id="a3529-207">Törölje a meglévő ExpressRoute- vagy helyek közötti VPN-átjárót.</span><span class="sxs-lookup"><span data-stu-id="a3529-207">Delete the existing ExpressRoute or Site-to-Site VPN gateway.</span></span> <span data-ttu-id="a3529-208">Használja a következő parancsmagot, és cserélje le az értékeket saját értékekre.</span><span class="sxs-lookup"><span data-stu-id="a3529-208">Use the following cmdlet, replacing the values with your own.</span></span>
   
        Remove-AzureVNetGateway –VnetName MyAzureVNET
3. <span data-ttu-id="a3529-209">Exportálja a virtuális hálózati sémát.</span><span class="sxs-lookup"><span data-stu-id="a3529-209">Export the virtual network schema.</span></span> <span data-ttu-id="a3529-210">Használja a következő PowerShell-parancsmagot, és cserélje le az értékeket saját értékekre.</span><span class="sxs-lookup"><span data-stu-id="a3529-210">Use the following PowerShell cmdlet, replacing the values with your own.</span></span>
   
        Get-AzureVNetConfig –ExportToFile “C:\NetworkConfig.xml”
4. <span data-ttu-id="a3529-211">Szerkessze a hálózat konfigurációs sémáját, hogy az átjáró-alhálózat /27 vagy egy rövidebb előtag legyen (például /26 vagy /25).</span><span class="sxs-lookup"><span data-stu-id="a3529-211">Edit the network configuration file schema so that the gateway subnet is /27 or a shorter prefix (such as /26 or /25).</span></span> <span data-ttu-id="a3529-212">Tekintse meg a következő példát.</span><span class="sxs-lookup"><span data-stu-id="a3529-212">See the following example.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="a3529-213">Ha már nincs elegendő IP-cím a virtuális hálózaton az átjáró-alhálózat méretének növeléséhez, további IP-címtereket kell hozzáadnia.</span><span class="sxs-lookup"><span data-stu-id="a3529-213">If you don't have enough IP addresses left in your virtual network to increase the gateway subnet size, you need to add more IP address space.</span></span> <span data-ttu-id="a3529-214">Az új konfigurációs sémával kapcsolatos információkért lásd: [Azure Virtual Network konfigurációs séma](https://msdn.microsoft.com/library/azure/jj157100.aspx).</span><span class="sxs-lookup"><span data-stu-id="a3529-214">For more information about the configuration schema, see [Azure Virtual Network configuration schema](https://msdn.microsoft.com/library/azure/jj157100.aspx).</span></span>
   > 
   > 
   
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.17.159.224/27</AddressPrefix>
          </Subnet>
5. <span data-ttu-id="a3529-215">Ha az előző átjáró helyek közötti VPN volt, a kapcsolat típusát is át kell állítania **Dedicated** értékre.</span><span class="sxs-lookup"><span data-stu-id="a3529-215">If your previous gateway was a Site-to-Site VPN, you must also change the connection type to **Dedicated**.</span></span>
   
                 <Gateway>
                  <ConnectionsToLocalNetwork>
                    <LocalNetworkSiteRef name="MyLocalNetwork">
                      <Connection type="Dedicated" />
                    </LocalNetworkSiteRef>
                  </ConnectionsToLocalNetwork>
                </Gateway>
6. <span data-ttu-id="a3529-216">Ezen a ponton egy átjáró nélküli VNettel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="a3529-216">At this point, you'll have a VNet with no gateways.</span></span> <span data-ttu-id="a3529-217">Új átjárók létrehozásához és a kapcsolatok véglegesítéséhez folytathatja az előző lépéssorban foglalt [4. lépés – ExpressRoute-átjáró létrehozása](#gw) lépéssel.</span><span class="sxs-lookup"><span data-stu-id="a3529-217">To create new gateways and complete your connections, you can proceed with [Step 4 - Create an ExpressRoute gateway](#gw), found in the preceding set of steps.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a3529-218">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a3529-218">Next steps</span></span>
<span data-ttu-id="a3529-219">További információ az ExpressRoute-tal kapcsolatban: [ExpressRoute – Gyakori kérdések](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="a3529-219">For more information about ExpressRoute, see the [ExpressRoute FAQ](expressroute-faqs.md)</span></span>

