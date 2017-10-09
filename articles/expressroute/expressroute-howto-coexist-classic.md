---
title: "Párhuzamosan fennálló ExpressRoute- és helyek közötti VPN-kapcsolatok konfigurálása: klasszikus: Azure | Microsoft Docs"
description: "Ez a cikk bemutatja, hogyan ExpressRoute és hello klasszikus telepítési modell egyszerre is használható, pont-pont VPN-kapcsolat konfigurálása."
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
ms.openlocfilehash: abb30fff55e8ec243f2920c5b2f70c43717755fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-expressroute-and-site-to-site-coexisting-connections-classic"></a><span data-ttu-id="ea45a-103">Párhuzamos ExpressRoute- és párhuzamos helyek közötti kapcsolatok konfigurálása (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="ea45a-103">Configure ExpressRoute and Site-to-Site coexisting connections (classic)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="ea45a-104">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="ea45a-104">PowerShell - Resource Manager</span></span>](expressroute-howto-coexist-resource-manager.md)
> * [<span data-ttu-id="ea45a-105">PowerShell – Klasszikus</span><span class="sxs-lookup"><span data-stu-id="ea45a-105">PowerShell - Classic</span></span>](expressroute-howto-coexist-classic.md)
> 
> 

<span data-ttu-id="ea45a-106">Hello képességét tooconfigure telephelyek közötti VPN és ExpressRoute több előnye is van.</span><span class="sxs-lookup"><span data-stu-id="ea45a-106">Having hello ability tooconfigure Site-to-Site VPN and ExpressRoute has several advantages.</span></span> <span data-ttu-id="ea45a-107">Telephelyek közötti VPN elérési útnak biztonságos feladatátvétel konfigurálása ExressRoute, vagy használja a pont-pont VPN tooconnect toosites nem ExpressRoute keresztül csatlakozó.</span><span class="sxs-lookup"><span data-stu-id="ea45a-107">You can configure Site-to-Site VPN as a secure failover path for ExressRoute, or use Site-to-Site VPNs tooconnect toosites that are not connected through ExpressRoute.</span></span> <span data-ttu-id="ea45a-108">Bemutatjuk hello lépéseket tooconfigure mindkét forgatókönyvet ebben a cikkben.</span><span class="sxs-lookup"><span data-stu-id="ea45a-108">We will cover hello steps tooconfigure both scenarios in this article.</span></span> <span data-ttu-id="ea45a-109">Ez a cikk vonatkozik toohello klasszikus üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="ea45a-109">This article applies toohello classic deployment model.</span></span> <span data-ttu-id="ea45a-110">Ez a konfiguráció hello portál nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="ea45a-110">This configuration is not available in hello portal.</span></span>

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

<span data-ttu-id="ea45a-111">**Tudnivalók az Azure üzembe helyezési modelljeiről**</span><span class="sxs-lookup"><span data-stu-id="ea45a-111">**About Azure deployment models**</span></span>

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

> [!IMPORTANT]
> <span data-ttu-id="ea45a-112">ExpressRoute-Kapcsolatcsoportok előre konfigurálni kell az alábbi hello utasítások végrehajtása előtt.</span><span class="sxs-lookup"><span data-stu-id="ea45a-112">ExpressRoute circuits must be pre-configured before you follow hello instructions below.</span></span> <span data-ttu-id="ea45a-113">Győződjön meg arról, hogy követte hello útmutatók túl[ExpressRoute-kapcsolatcsoportot létrehozni](expressroute-howto-circuit-classic.md) és [konfigurálja az útválasztást](expressroute-howto-routing-classic.md) az alábbi hello lépések végrehajtása előtt.</span><span class="sxs-lookup"><span data-stu-id="ea45a-113">Make sure that you have followed hello guides too[create an ExpressRoute circuit](expressroute-howto-circuit-classic.md) and [configure routing](expressroute-howto-routing-classic.md) before you follow hello steps below.</span></span>
> 
> 

## <a name="limits-and-limitations"></a><span data-ttu-id="ea45a-114">Korlátok és korlátozások</span><span class="sxs-lookup"><span data-stu-id="ea45a-114">Limits and limitations</span></span>
* <span data-ttu-id="ea45a-115">**A tranzit útválasztás nem támogatott.**</span><span class="sxs-lookup"><span data-stu-id="ea45a-115">**Transit routing is not supported.**</span></span> <span data-ttu-id="ea45a-116">Nem hajthat végre útválasztást (az Azure-on keresztül) a helyek közötti VPN használatával csatlakoztatott helyi hálózat és az ExpressRoute használatával csatlakoztatott helyi hálózat között.</span><span class="sxs-lookup"><span data-stu-id="ea45a-116">You cannot route (via Azure) between your local network connected via Site-to-Site VPN and your local network connected via ExpressRoute.</span></span>
* <span data-ttu-id="ea45a-117">**A pont–hely kapcsolat nem támogatott.**</span><span class="sxs-lookup"><span data-stu-id="ea45a-117">**Point-to-site is not supported.**</span></span> <span data-ttu-id="ea45a-118">Nem lehet engedélyezni a pont-pont VPN kapcsolatok toohello ugyanazt a virtuális hálózatot, amely csatlakoztatott tooExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="ea45a-118">You can't enable point-to-site VPN connections toohello same VNet that is connected tooExpressRoute.</span></span> <span data-ttu-id="ea45a-119">Pont-pont VPN- és ExpressRoute nem lehet hello az azonos virtuális hálózaton.</span><span class="sxs-lookup"><span data-stu-id="ea45a-119">Point-to-site VPN and ExpressRoute cannot coexist for hello same VNet.</span></span>
* <span data-ttu-id="ea45a-120">**A kényszerített bújtatás hello telephelyek közötti VPN-átjáró nem engedélyezhető.**</span><span class="sxs-lookup"><span data-stu-id="ea45a-120">**Forced tunneling cannot be enabled on hello Site-to-Site VPN gateway.**</span></span> <span data-ttu-id="ea45a-121">Csak "kényszerítheti" minden Internet adathoz kötött forgalom hátsó tooyour a helyi hálózaton keresztül ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="ea45a-121">You can only "force" all Internet-bound traffic back tooyour on-premises network via ExpressRoute.</span></span>
* <span data-ttu-id="ea45a-122">**A Basic SKU-átjáró nem támogatott.**</span><span class="sxs-lookup"><span data-stu-id="ea45a-122">**Basic SKU gateway is not supported.**</span></span> <span data-ttu-id="ea45a-123">Nem alapszintű Termékváltozat átjáró kell használnia az mindkét hello [ExpressRoute-átjáró](expressroute-about-virtual-network-gateways.md) és hello [VPN-átjáró](../vpn-gateway/vpn-gateway-about-vpngateways.md).</span><span class="sxs-lookup"><span data-stu-id="ea45a-123">You must use a non-Basic SKU gateway for both hello [ExpressRoute gateway](expressroute-about-virtual-network-gateways.md) and hello [VPN gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).</span></span>
* <span data-ttu-id="ea45a-124">**Kizárólag az útvonalalapú VPN-átjáró támogatott.**</span><span class="sxs-lookup"><span data-stu-id="ea45a-124">**Only route-based VPN gateway is supported.**</span></span> <span data-ttu-id="ea45a-125">Útvonalalapú [VPN-átjárót](../vpn-gateway/vpn-gateway-about-vpngateways.md) kell használnia.</span><span class="sxs-lookup"><span data-stu-id="ea45a-125">You must use a route-based [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).</span></span>
* <span data-ttu-id="ea45a-126">**A VPN-átjáróhoz statikus útvonalat kell konfigurálni.**</span><span class="sxs-lookup"><span data-stu-id="ea45a-126">**Static route should be configured for your VPN gateway.**</span></span> <span data-ttu-id="ea45a-127">Ha a helyi hálózathoz csatlakoztatott tooboth ExpressRoute és a pont-pont VPN, rendelkeznie kell a helyi hálózati tooroute hello pont-pont VPN-kapcsolat toohello konfigurált statikus útvonal nyilvános internethez.</span><span class="sxs-lookup"><span data-stu-id="ea45a-127">If your local network is connected tooboth ExpressRoute and a Site-to-Site VPN, you must have a static route configured in your local network tooroute hello Site-to-Site VPN connection toohello public Internet.</span></span>
* <span data-ttu-id="ea45a-128">**Elsőként az ExpressRoute-átjárót kell konfigurálnia.**</span><span class="sxs-lookup"><span data-stu-id="ea45a-128">**ExpressRoute gateway must be configured first.**</span></span> <span data-ttu-id="ea45a-129">Létre kell hoznia hello ExpressRoute-átjárót először hello telephelyek közötti VPN átjáró hozzáadása előtt.</span><span class="sxs-lookup"><span data-stu-id="ea45a-129">You must create hello ExpressRoute gateway first before you add hello Site-to-Site VPN gateway.</span></span>

## <a name="configuration-designs"></a><span data-ttu-id="ea45a-130">Konfigurációs tervek</span><span class="sxs-lookup"><span data-stu-id="ea45a-130">Configuration designs</span></span>
### <a name="configure-a-site-to-site-vpn-as-a-failover-path-for-expressroute"></a><span data-ttu-id="ea45a-131">Helyek közötti VPN konfigurálása feladatátvételi útvonalként az ExpressRoute számára</span><span class="sxs-lookup"><span data-stu-id="ea45a-131">Configure a Site-to-Site VPN as a failover path for ExpressRoute</span></span>
<span data-ttu-id="ea45a-132">Konfigurálhat egy helyek közötti VPN-kapcsolatot tartalékként az ExpressRoute számára.</span><span class="sxs-lookup"><span data-stu-id="ea45a-132">You can configure a Site-to-Site VPN connection as a backup for ExpressRoute.</span></span> <span data-ttu-id="ea45a-133">Ez vonatkozik csak toovirtual hálózatok csatolt toohello Azure magánhálózati társviszony-létesítési elérési útja.</span><span class="sxs-lookup"><span data-stu-id="ea45a-133">This applies only toovirtual networks linked toohello Azure private peering path.</span></span> <span data-ttu-id="ea45a-134">Az Azure nyilvános és a Microsoft társviszony-létesítésekhez nem létezik VPN-alapú feladatátvételi megoldás.</span><span class="sxs-lookup"><span data-stu-id="ea45a-134">There is no VPN-based failover solution for services accessible through Azure public and Microsoft peerings.</span></span> <span data-ttu-id="ea45a-135">hello ExpressRoute-kapcsolatcsoportot mindig hello elsődleges kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="ea45a-135">hello ExpressRoute circuit is always hello primary link.</span></span> <span data-ttu-id="ea45a-136">Adatok halad keresztül hello telephelyek közötti VPN elérési út csak akkor, ha ExpressRoute-kapcsolatcsoportot hello sikertelen.</span><span class="sxs-lookup"><span data-stu-id="ea45a-136">Data will flow through hello Site-to-Site VPN path only if hello ExpressRoute circuit fails.</span></span> 

> [!NOTE]
> <span data-ttu-id="ea45a-137">Habár ExpressRoute-kapcsolatcsoportot pont-pont VPN-kapcsolaton keresztül előnyben részesített a mindkét útvonalak vannak hello azonos, Azure használandó hello leghosszabb előtag egyezés toochoose hello útvonal hello csomagok cél felé.</span><span class="sxs-lookup"><span data-stu-id="ea45a-137">While ExpressRoute circuit is preferred over Site-to-Site VPN when both routes are hello same, Azure will use hello longest prefix match toochoose hello route towards hello packet's destination.</span></span>
> 
> 

![Egyidejű jelenlét](media/expressroute-howto-coexist-classic/scenario1.jpg)

### <a name="configure-a-site-to-site-vpn-tooconnect-toosites-not-connected-through-expressroute"></a><span data-ttu-id="ea45a-139">Egy ExpressRoute keresztül nem kapcsolódik telephelyek közötti VPN-tooconnect toosites konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ea45a-139">Configure a Site-to-Site VPN tooconnect toosites not connected through ExpressRoute</span></span>
<span data-ttu-id="ea45a-140">Beállíthatja, hogy a hálózat, ahol egyes helyek csatlakoznak közvetlenül tooAzure pont-pont VPN-kapcsolaton keresztül, és az egyes helyek ExpressRoute keresztül csatlakoznak.</span><span class="sxs-lookup"><span data-stu-id="ea45a-140">You can configure your network where some sites connect directly tooAzure over Site-to-Site VPN, and some sites connect through ExpressRoute.</span></span> 

![Egyidejű jelenlét](media/expressroute-howto-coexist-classic/scenario2.jpg)

> [!NOTE]
> <span data-ttu-id="ea45a-142">Virtuális hálózatot nem konfigurálhat tranzit útválasztóként.</span><span class="sxs-lookup"><span data-stu-id="ea45a-142">You cannot a configure a virtual network as a transit router.</span></span>
> 
> 

## <a name="selecting-hello-steps-toouse"></a><span data-ttu-id="ea45a-143">Hello lépéseket toouse kiválasztása</span><span class="sxs-lookup"><span data-stu-id="ea45a-143">Selecting hello steps toouse</span></span>
<span data-ttu-id="ea45a-144">Nincsenek az eljárások toochoose a rendelés tooconfigure kapcsolatok, amely egyszerre is használható két különböző csoportjai számára.</span><span class="sxs-lookup"><span data-stu-id="ea45a-144">There are two different sets of procedures toochoose from in order tooconfigure connections that can coexist.</span></span> <span data-ttu-id="ea45a-145">hello konfigurációs eljárás választhat, hogy van-e egy meglévő virtuális hálózatot, hogy azt szeretné, hogy tooconnect, vagy azt szeretné, hogy egy új virtuális hálózat toocreate függ.</span><span class="sxs-lookup"><span data-stu-id="ea45a-145">hello configuration procedure that you select will depend on whether you have an existing virtual network that you want tooconnect to, or you want toocreate a new virtual network.</span></span>

* <span data-ttu-id="ea45a-146">Nem rendelkezik egy VNet és egy toocreate kell.</span><span class="sxs-lookup"><span data-stu-id="ea45a-146">I don't have a VNet and need toocreate one.</span></span>
  
    <span data-ttu-id="ea45a-147">Ha még nem rendelkezik virtuális hálózat, ez az eljárás végigvezeti hello klasszikus üzembe helyezési modellel, és új ExpressRoute és a pont-pont VPN-kapcsolatok létrehozása új virtuális hálózat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="ea45a-147">If you don’t already have a virtual network, this procedure will walk you through creating a new virtual network using hello classic deployment model and creating new ExpressRoute and Site-to-Site VPN connections.</span></span> <span data-ttu-id="ea45a-148">tooconfigure, hajtsa végre hello hello cikk szakaszban található lépéseket [toocreate egy új virtuális hálózat és vizsgálatát a kísérő kapcsolatok](#new).</span><span class="sxs-lookup"><span data-stu-id="ea45a-148">tooconfigure, follow hello steps in hello article section [toocreate a new virtual network and coexisting connections](#new).</span></span>
* <span data-ttu-id="ea45a-149">Már rendelkezem egy klasszikus üzembehelyezési modell szerinti VNettel.</span><span class="sxs-lookup"><span data-stu-id="ea45a-149">I already have a classic deployment model VNet.</span></span>
  
    <span data-ttu-id="ea45a-150">Előfordulhat, hogy már rendelkezik üzemelő virtuális hálózattal egy létező helyek közötti VPN- vagy ExpressRoute-kapcsolattal.</span><span class="sxs-lookup"><span data-stu-id="ea45a-150">You may already have a virtual network in place with an existing Site-to-Site VPN connection or ExpressRoute connection.</span></span> <span data-ttu-id="ea45a-151">a cikk szakasz hello [tooconfigure coexsiting kapcsolatok egy már meglévő vnet](#add) hello átjáró törlése folyamatban, és új ExpressRoute- és telephelyek közötti VPN-kapcsolatok létrehozására, majd részletesen ismerteti.</span><span class="sxs-lookup"><span data-stu-id="ea45a-151">hello article section [tooconfigure coexsiting connections for an already existing VNet](#add) will walk you through deleting hello gateway, and then creating new ExpressRoute and Site-to-Site VPN connections.</span></span> <span data-ttu-id="ea45a-152">Vegye figyelembe, hogy ha hello új kapcsolatok létrehozására, hello lépéseket kell végrehajtania nagyon meghatározott sorrendben.</span><span class="sxs-lookup"><span data-stu-id="ea45a-152">Note that when creating hello new connections, hello steps must be completed in a very specific order.</span></span> <span data-ttu-id="ea45a-153">Ne használjon más cikkek toocreate hello utasításait, az átjárók és kapcsolatok.</span><span class="sxs-lookup"><span data-stu-id="ea45a-153">Don't use hello instructions in other articles toocreate your gateways and connections.</span></span>
  
    <span data-ttu-id="ea45a-154">Ebben az eljárásban, amely egyszerre is használható kapcsolatok létrehozására lesz igényelnek, toodelete az átjáró, és adja meg új átjárók.</span><span class="sxs-lookup"><span data-stu-id="ea45a-154">In this procedure, creating connections that can coexist will require you toodelete your gateway, and then configure new gateways.</span></span> <span data-ttu-id="ea45a-155">Ez azt jelenti, hogy a létesítmények közötti kapcsolatok állásidő során törölje és hozza létre újra az átjárót és a kapcsolatok, de nem kell toomigrate bármely, a virtuális gépek vagy szolgáltatások tooa új virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="ea45a-155">This means you will have downtime for your cross-premises connections while you delete and recreate your gateway and connections, but you will not need toomigrate any of your VMs or services tooa new virtual network.</span></span> <span data-ttu-id="ea45a-156">A virtuális gépek és szolgáltatások is ki tudja toocommunicate keresztül hello terheléselosztó amíg az átjáró konfigurálásának, ha így konfigurálva toodo.</span><span class="sxs-lookup"><span data-stu-id="ea45a-156">Your VMs and services will still be able toocommunicate out through hello load balancer while you configure your gateway if they are configured toodo so.</span></span>

## <span data-ttu-id="ea45a-157"><a name="new"></a>új virtuális hálózat toocreate és vizsgálatát a kísérő kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="ea45a-157"><a name="new"></a>toocreate a new virtual network and coexisting connections</span></span>
<span data-ttu-id="ea45a-158">Az eljárás a VNetek, valamint az egyidejűleg jelenlévő helyek közötti és ExpressRoute-kapcsolatok létrehozásának módját ismerteti.</span><span class="sxs-lookup"><span data-stu-id="ea45a-158">This procedure will walk you through creating a VNet and create Site-to-Site and ExpressRoute connections that will coexist.</span></span>

1. <span data-ttu-id="ea45a-159">Tooinstall hello legújabb verziójára hello Azure PowerShell-parancsmagok lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="ea45a-159">You'll need tooinstall hello latest version of hello Azure PowerShell cmdlets.</span></span> <span data-ttu-id="ea45a-160">Lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) hello PowerShell-parancsmagok telepítéséről további információt.</span><span class="sxs-lookup"><span data-stu-id="ea45a-160">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) for more information about installing hello PowerShell cmdlets.</span></span> <span data-ttu-id="ea45a-161">Előfordulhat, hogy ehhez a konfigurációhoz fogjuk hello parancsmagok némileg eltérő mi, előfordulhat, hogy ismernie kell.</span><span class="sxs-lookup"><span data-stu-id="ea45a-161">Note that hello cmdlets that you'll use for this configuration may be slightly different than what you might be familiar with.</span></span> <span data-ttu-id="ea45a-162">Adható meg, hogy toouse hello parancsmagok ezeket az utasításokat.</span><span class="sxs-lookup"><span data-stu-id="ea45a-162">Be sure toouse hello cmdlets specified in these instructions.</span></span> 
2. <span data-ttu-id="ea45a-163">Hozzon létre egy sémát a virtuális hálózat számára.</span><span class="sxs-lookup"><span data-stu-id="ea45a-163">Create a schema for your virtual network.</span></span> <span data-ttu-id="ea45a-164">Hello konfigurációs séma kapcsolatos további információkért lásd: [Azure Virtual Network konfigurációs séma](https://msdn.microsoft.com/library/azure/jj157100.aspx).</span><span class="sxs-lookup"><span data-stu-id="ea45a-164">For more information about hello configuration schema, see [Azure Virtual Network configuration schema](https://msdn.microsoft.com/library/azure/jj157100.aspx).</span></span>
   
    <span data-ttu-id="ea45a-165">A séma létrehozásakor ellenőrizze, a következő értékek hello használja:</span><span class="sxs-lookup"><span data-stu-id="ea45a-165">When you create your schema, make sure you use hello following values:</span></span>
   
   * <span data-ttu-id="ea45a-166">hello átjáróalhálózatot hello virtuális hálózat /27 vagy egy rövidebb előtaggal (például /26 vagy /25) kell lennie.</span><span class="sxs-lookup"><span data-stu-id="ea45a-166">hello gateway subnet for hello virtual network must be /27 or a shorter prefix (such as /26 or /25).</span></span>
   * <span data-ttu-id="ea45a-167">hello átjáró kapcsolat típusa "Dedikálnak".</span><span class="sxs-lookup"><span data-stu-id="ea45a-167">hello gateway connection type is "Dedicated".</span></span>
     
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
3. <span data-ttu-id="ea45a-168">Miután létrehozásához, és az XML-sémafájl konfigurálásához, hello-fájl feltöltése.</span><span class="sxs-lookup"><span data-stu-id="ea45a-168">After creating and configuring your xml schema file, upload hello file.</span></span> <span data-ttu-id="ea45a-169">Ezzel létrejön a virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="ea45a-169">This will create your virtual network.</span></span>
   
    <span data-ttu-id="ea45a-170">A következő parancsmag tooupload hello a fájlt, hello érték helyett a saját használja.</span><span class="sxs-lookup"><span data-stu-id="ea45a-170">Use hello following cmdlet tooupload your file, replacing hello value with your own.</span></span>
   
        Set-AzureVNetConfig -ConfigurationPath 'C:\NetworkConfig.xml'
4. <span data-ttu-id="ea45a-171"><a name="gw"></a>Hozzon létre egy ExpressRoute-átjárót.</span><span class="sxs-lookup"><span data-stu-id="ea45a-171"><a name="gw"></a>Create an ExpressRoute gateway.</span></span> <span data-ttu-id="ea45a-172">Lehet, hogy toospecify, GatewaySKU hello *szabványos*, *HighPerformance*, vagy *UltraPerformance* és hello GatewayType, *DynamicRouting*.</span><span class="sxs-lookup"><span data-stu-id="ea45a-172">Be sure toospecify hello GatewaySKU as *Standard*, *HighPerformance*, or *UltraPerformance* and hello GatewayType as *DynamicRouting*.</span></span>
   
    <span data-ttu-id="ea45a-173">A következő mintát, és hello értékeket a saját hello használata.</span><span class="sxs-lookup"><span data-stu-id="ea45a-173">Use hello following sample, substituting hello values for your own.</span></span>
   
        New-AzureVNetGateway -VNetName MyAzureVNET -GatewayType DynamicRouting -GatewaySKU HighPerformance
5. <span data-ttu-id="ea45a-174">Hello ExpressRoute átjáró toohello ExpressRoute-kapcsolatcsoportot hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="ea45a-174">Link hello ExpressRoute gateway toohello ExpressRoute circuit.</span></span> <span data-ttu-id="ea45a-175">Ez a lépés befejezése után, a helyszíni hálózat között az Azure ExpressRoute, keresztül hello kapcsolat létrejött.</span><span class="sxs-lookup"><span data-stu-id="ea45a-175">After this step has been completed, hello connection between your on-premises network and Azure, through ExpressRoute, is established.</span></span>
   
        New-AzureDedicatedCircuitLink -ServiceKey <service-key> -VNetName MyAzureVNET
6. <span data-ttu-id="ea45a-176"><a name="vpngw"></a>Ezután hozza létre a webhelyek közötti VPN-átjárót.</span><span class="sxs-lookup"><span data-stu-id="ea45a-176"><a name="vpngw"></a>Next, create your Site-to-Site VPN gateway.</span></span> <span data-ttu-id="ea45a-177">hello GatewaySKU kell *szabványos*, *HighPerformance*, vagy *UltraPerformance* és hello GatewayType kell *DynamicRouting*.</span><span class="sxs-lookup"><span data-stu-id="ea45a-177">hello GatewaySKU must be *Standard*, *HighPerformance*, or *UltraPerformance* and hello GatewayType must be *DynamicRouting*.</span></span>
   
        New-AzureVirtualNetworkGateway -VNetName MyAzureVNET -GatewayName S2SVPN -GatewayType DynamicRouting -GatewaySKU  HighPerformance
   
    <span data-ttu-id="ea45a-178">tooretrieve hello virtuális hálózati átjáró beállításának, köztük hello Átjáróazonosító és hello nyilvános IP-cím, használja a hello `Get-AzureVirtualNetworkGateway` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="ea45a-178">tooretrieve hello virtual network gateway settings, including hello gateway ID and hello public IP, use hello `Get-AzureVirtualNetworkGateway` cmdlet.</span></span>
   
        Get-AzureVirtualNetworkGateway
   
        GatewayId            : 348ae011-ffa9-4add-b530-7cb30010565e
        GatewayName          : S2SVPN
        LastEventData        :
        GatewayType          : DynamicRouting
        LastEventTimeStamp   : 5/29/2015 4:41:41 PM
        LastEventMessage     : Successfully created a gateway for hello following virtual network: GNSDesMoines
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
7. <span data-ttu-id="ea45a-179">Hozzon létre egy helyi VPN-átjáró entitást.</span><span class="sxs-lookup"><span data-stu-id="ea45a-179">Create a local site VPN gateway entity.</span></span> <span data-ttu-id="ea45a-180">Ez a parancs nem konfigurálja a helyszíni VPN-átjárót.</span><span class="sxs-lookup"><span data-stu-id="ea45a-180">This command doesn’t configure your on-premises VPN gateway.</span></span> <span data-ttu-id="ea45a-181">Ehelyett tooprovide hello helyi átjáró beállításai, például a hello nyilvános IP-cím lehetővé teszi, hello helyszíni címtér, így hello Azure VPN-átjáró képes kapcsolódni tooit.</span><span class="sxs-lookup"><span data-stu-id="ea45a-181">Rather, it allows you tooprovide hello local gateway settings, such as hello public IP and hello on-premises address space, so that hello Azure VPN gateway can connect tooit.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="ea45a-182">hello hello telephelyek közötti VPN a hely nincs definiálva a hello netcfg.</span><span class="sxs-lookup"><span data-stu-id="ea45a-182">hello local site for hello Site-to-Site VPN is not defined in hello netcfg.</span></span> <span data-ttu-id="ea45a-183">Ehelyett ez a parancsmag toospecify hello helyi paraméterek kell használnia.</span><span class="sxs-lookup"><span data-stu-id="ea45a-183">Instead, you must use this cmdlet toospecify hello local site parameters.</span></span> <span data-ttu-id="ea45a-184">Nem lehet definiálni, vagy a portálon, vagy hello netcfg fájl használatával.</span><span class="sxs-lookup"><span data-stu-id="ea45a-184">You cannot define it using either portal, or hello netcfg file.</span></span>
   > 
   > 
   
    <span data-ttu-id="ea45a-185">A következő mintát, hello értékeket cserélje le a saját hello használata.</span><span class="sxs-lookup"><span data-stu-id="ea45a-185">Use hello following sample, replacing hello values with your own.</span></span>
   
        New-AzureLocalNetworkGateway -GatewayName MyLocalNetwork -IpAddress <MyLocalGatewayIp> -AddressSpace <MyLocalNetworkAddress>
   
   > [!NOTE]
   > <span data-ttu-id="ea45a-186">Ha a helyi hálózat több útvonalat is tartalmaz, tömbként egyszerre mindet megadhatja.</span><span class="sxs-lookup"><span data-stu-id="ea45a-186">If your local network has multiple routes, you can pass them all in as an array.</span></span>  <span data-ttu-id="ea45a-187">$MyLocalNetworkAddress = @("10.1.2.0/24","10.1.3.0/24","10.2.1.0/24")</span><span class="sxs-lookup"><span data-stu-id="ea45a-187">$MyLocalNetworkAddress = @("10.1.2.0/24","10.1.3.0/24","10.2.1.0/24")</span></span>  
   > 
   > 

    <span data-ttu-id="ea45a-188">tooretrieve hello virtuális hálózati átjáró beállításának, köztük hello Átjáróazonosító és hello nyilvános IP-cím, használja a hello `Get-AzureVirtualNetworkGateway` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="ea45a-188">tooretrieve hello virtual network gateway settings, including hello gateway ID and hello public IP, use hello `Get-AzureVirtualNetworkGateway` cmdlet.</span></span> <span data-ttu-id="ea45a-189">Tekintse meg a következő példa hello.</span><span class="sxs-lookup"><span data-stu-id="ea45a-189">See hello following example.</span></span>

        Get-AzureLocalNetworkGateway

        GatewayId            : 532cb428-8c8c-4596-9a4f-7ae3a9fcd01b
        GatewayName          : MyLocalNetwork
        IpAddress            : 23.39.x.y
        AddressSpace         : {10.1.2.0/24}
        OperationDescription : Get-AzureLocalNetworkGateway
        OperationId          : ddc4bfae-502c-adc7-bd7d-1efbc00b3fe5
        OperationStatus      : Succeeded


1. <span data-ttu-id="ea45a-190">A helyi VPN-eszköz tooconnect toohello új átjáró konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="ea45a-190">Configure your local VPN device tooconnect toohello new gateway.</span></span> <span data-ttu-id="ea45a-191">A VPN-eszköz konfigurálásakor az 6. lépésben lekért hello információkat használja.</span><span class="sxs-lookup"><span data-stu-id="ea45a-191">Use hello information that you retrieved in step 6 when configuring your VPN device.</span></span> <span data-ttu-id="ea45a-192">A VPN-eszköz konfigurálásával kapcsolatos további információkért lásd: [VPN-eszköz konfigurálása](../vpn-gateway/vpn-gateway-about-vpn-devices.md).</span><span class="sxs-lookup"><span data-stu-id="ea45a-192">For more information about VPN device configuration, see [VPN Device Configuration](../vpn-gateway/vpn-gateway-about-vpn-devices.md).</span></span>
2. <span data-ttu-id="ea45a-193">Hivatkozás hello telephelyek közötti VPN átjáró Azure toohello helyi átjáró.</span><span class="sxs-lookup"><span data-stu-id="ea45a-193">Link hello Site-to-Site VPN gateway on Azure toohello local gateway.</span></span>
   
    <span data-ttu-id="ea45a-194">Ebben a példában a connectedEntityId: hello helyi átjáró azonosítója, amely futtatásával `Get-AzureLocalNetworkGateway`.</span><span class="sxs-lookup"><span data-stu-id="ea45a-194">In this example, connectedEntityId is hello local gateway ID, which you can find by running `Get-AzureLocalNetworkGateway`.</span></span> <span data-ttu-id="ea45a-195">Hello segítségével virtualNetworkGatewayId található `Get-AzureVirtualNetworkGateway` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="ea45a-195">You can find virtualNetworkGatewayId by using hello `Get-AzureVirtualNetworkGateway` cmdlet.</span></span> <span data-ttu-id="ea45a-196">Ez a lépés után hello telephelyek közötti VPN-kapcsolaton keresztül a helyi hálózat és az Azure közötti hello kapcsolat létrejött.</span><span class="sxs-lookup"><span data-stu-id="ea45a-196">After this step, hello connection between your local network and Azure via hello Site-to-Site VPN connection is established.</span></span>

        New-AzureVirtualNetworkGatewayConnection -connectedEntityId <local-network-gateway-id> -gatewayConnectionName Azure2Local -gatewayConnectionType IPsec -sharedKey abc123 -virtualNetworkGatewayId <azure-s2s-vpn-gateway-id>

## <span data-ttu-id="ea45a-197"><a name="add"></a>egy már meglévő virtuális hálózatot tooconfigure coexsiting kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="ea45a-197"><a name="add"></a>tooconfigure coexsiting connections for an already existing VNet</span></span>
<span data-ttu-id="ea45a-198">Ha egy meglévő virtuális hálózattal rendelkezik, ellenőrizze a hello átjáró alhálózati méretét.</span><span class="sxs-lookup"><span data-stu-id="ea45a-198">If you have an existing virtual network, check hello gateway subnet size.</span></span> <span data-ttu-id="ea45a-199">Ha hello átjáróalhálózatot /28 vagy /29, először hello virtuális hálózati átjáró törlése és hello átjáró alhálózati méretének növelése.</span><span class="sxs-lookup"><span data-stu-id="ea45a-199">If hello gateway subnet is /28 or /29, you must first delete hello virtual network gateway and increase hello gateway subnet size.</span></span> <span data-ttu-id="ea45a-200">hello lépéseket ebben a szakaszban megtudhatja, hogyan toodo, amely.</span><span class="sxs-lookup"><span data-stu-id="ea45a-200">hello steps in this section will show you how toodo that.</span></span>

<span data-ttu-id="ea45a-201">Ha hello átjáróalhálózatot /27 vagy nagyobb és hello virtuális hálózati ExpressRoute keresztül csatlakozik, hello lépéseket kihagyhatja, és folytassa túl["6. lépés – --webhelyek közötti VPN átjáró létrehozása"](#vpngw) hello előző szakaszban.</span><span class="sxs-lookup"><span data-stu-id="ea45a-201">If hello gateway subnet is /27 or larger and hello virtual network is connected via ExpressRoute, you can skip hello steps below and proceed too["Step 6 - Create a Site-to-Site VPN gateway"](#vpngw) in hello previous section.</span></span>

> [!NOTE]
> <span data-ttu-id="ea45a-202">Meglévő átjáró hello törlésekor a helyi helyi hello kapcsolat tooyour virtuális hálózati elvész, ezt a konfigurációt a munka során.</span><span class="sxs-lookup"><span data-stu-id="ea45a-202">When you delete hello existing gateway, your local premises will lose hello connection tooyour virtual network while you are working on this configuration.</span></span>
> 
> 

1. <span data-ttu-id="ea45a-203">Tooinstall hello legújabb verziójára hello Azure Resource Manager PowerShell-parancsmagok lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="ea45a-203">You'll need tooinstall hello latest version of hello Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="ea45a-204">Lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) hello PowerShell-parancsmagok telepítéséről további információt.</span><span class="sxs-lookup"><span data-stu-id="ea45a-204">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) for more information about installing hello PowerShell cmdlets.</span></span> <span data-ttu-id="ea45a-205">Előfordulhat, hogy ehhez a konfigurációhoz fogjuk hello parancsmagok némileg eltérő mi, előfordulhat, hogy ismernie kell.</span><span class="sxs-lookup"><span data-stu-id="ea45a-205">Note that hello cmdlets that you'll use for this configuration may be slightly different than what you might be familiar with.</span></span> <span data-ttu-id="ea45a-206">Adható meg, hogy toouse hello parancsmagok ezeket az utasításokat.</span><span class="sxs-lookup"><span data-stu-id="ea45a-206">Be sure toouse hello cmdlets specified in these instructions.</span></span> 
2. <span data-ttu-id="ea45a-207">Hello meglévő expressroute-on vagy a telephelyek közötti VPN-átjáró törlése.</span><span class="sxs-lookup"><span data-stu-id="ea45a-207">Delete hello existing ExpressRoute or Site-to-Site VPN gateway.</span></span> <span data-ttu-id="ea45a-208">A következő parancsmag, hello értékeket cserélje le a saját hello használata.</span><span class="sxs-lookup"><span data-stu-id="ea45a-208">Use hello following cmdlet, replacing hello values with your own.</span></span>
   
        Remove-AzureVNetGateway –VnetName MyAzureVNET
3. <span data-ttu-id="ea45a-209">Hello virtuális hálózati séma exportálása.</span><span class="sxs-lookup"><span data-stu-id="ea45a-209">Export hello virtual network schema.</span></span> <span data-ttu-id="ea45a-210">A következő PowerShell-parancsmag, hello értékeket cserélje le a saját hello használata.</span><span class="sxs-lookup"><span data-stu-id="ea45a-210">Use hello following PowerShell cmdlet, replacing hello values with your own.</span></span>
   
        Get-AzureVNetConfig –ExportToFile “C:\NetworkConfig.xml”
4. <span data-ttu-id="ea45a-211">Hello hálózati konfigurációs fájl séma módosítsa hello átjáróalhálózatot, de /27 egy rövidebb előtaggal (például /26 vagy /25).</span><span class="sxs-lookup"><span data-stu-id="ea45a-211">Edit hello network configuration file schema so that hello gateway subnet is /27 or a shorter prefix (such as /26 or /25).</span></span> <span data-ttu-id="ea45a-212">Tekintse meg a következő példa hello.</span><span class="sxs-lookup"><span data-stu-id="ea45a-212">See hello following example.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="ea45a-213">Ha még nem rendelkezik elegendő a virtuális hálózati tooincrease hello átjáró alhálózat méretét az IP-címek, meg kell tooadd további IP-címterület.</span><span class="sxs-lookup"><span data-stu-id="ea45a-213">If you don't have enough IP addresses left in your virtual network tooincrease hello gateway subnet size, you need tooadd more IP address space.</span></span> <span data-ttu-id="ea45a-214">Hello konfigurációs séma kapcsolatos további információkért lásd: [Azure Virtual Network konfigurációs séma](https://msdn.microsoft.com/library/azure/jj157100.aspx).</span><span class="sxs-lookup"><span data-stu-id="ea45a-214">For more information about hello configuration schema, see [Azure Virtual Network configuration schema](https://msdn.microsoft.com/library/azure/jj157100.aspx).</span></span>
   > 
   > 
   
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.17.159.224/27</AddressPrefix>
          </Subnet>
5. <span data-ttu-id="ea45a-215">Ha az előző átjáró volt a telephelyek közötti VPN, módosítania kell hello kapcsolat típusa túl**dedikált**.</span><span class="sxs-lookup"><span data-stu-id="ea45a-215">If your previous gateway was a Site-to-Site VPN, you must also change hello connection type too**Dedicated**.</span></span>
   
                 <Gateway>
                  <ConnectionsToLocalNetwork>
                    <LocalNetworkSiteRef name="MyLocalNetwork">
                      <Connection type="Dedicated" />
                    </LocalNetworkSiteRef>
                  </ConnectionsToLocalNetwork>
                </Gateway>
6. <span data-ttu-id="ea45a-216">Ezen a ponton egy átjáró nélküli VNettel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="ea45a-216">At this point, you'll have a VNet with no gateways.</span></span> <span data-ttu-id="ea45a-217">új toocreate-átjárók és a kapcsolatokat, folytassa a [4. lépés – hozzon létre egy ExpressRoute-átjáró](#gw), míg a talált a fenti lépéseket hello.</span><span class="sxs-lookup"><span data-stu-id="ea45a-217">toocreate new gateways and complete your connections, you can proceed with [Step 4 - Create an ExpressRoute gateway](#gw), found in hello preceding set of steps.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ea45a-218">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ea45a-218">Next steps</span></span>
<span data-ttu-id="ea45a-219">ExpressRoute kapcsolatos további információkért lásd: hello [ExpressRoute – gyakori kérdések](expressroute-faqs.md)</span><span class="sxs-lookup"><span data-stu-id="ea45a-219">For more information about ExpressRoute, see hello [ExpressRoute FAQ](expressroute-faqs.md)</span></span>

