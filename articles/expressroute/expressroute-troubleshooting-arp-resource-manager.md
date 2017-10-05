---
title: "ARP-tábla beolvasása: erőforrás-kezelő: Azure ExpressRoute-hibaelhárítási |} Microsoft Docs"
description: "Ezen a lapon utasításokkal szolgál az ExpressRoute-kapcsolatcsoportot az ARP tábla beolvasása"
documentationcenter: na
services: expressroute
author: ganesr
manager: carolz
editor: tysonn
ms.assetid: 0a6bf1d5-6baf-44dd-87d3-1ebd2fd08bdc
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/30/2017
ms.author: ganesr
ms.openlocfilehash: a65b1ba2998eae33b3e73bd2492fbbf025eb5946
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="getting-arp-tables-in-the-resource-manager-deployment-model"></a><span data-ttu-id="93b45-103">A Resource Manager üzembe helyezési modellel tábla ARP beolvasása</span><span class="sxs-lookup"><span data-stu-id="93b45-103">Getting ARP tables in the Resource Manager deployment model</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="93b45-104">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="93b45-104">PowerShell - Resource Manager</span></span>](expressroute-troubleshooting-arp-resource-manager.md)
> * [<span data-ttu-id="93b45-105">PowerShell – Klasszikus</span><span class="sxs-lookup"><span data-stu-id="93b45-105">PowerShell - Classic</span></span>](expressroute-troubleshooting-arp-classic.md)
> 
> 

<span data-ttu-id="93b45-106">Ez a cikk végigvezeti az ExpressRoute-kapcsolatcsoportot ARP táblázatokban további lépéseket.</span><span class="sxs-lookup"><span data-stu-id="93b45-106">This article walks you through the steps to learn the ARP tables for your ExpressRoute circuit.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="93b45-107">Ez a dokumentum olyan egyszerű problémák megoldásában segítséget.</span><span class="sxs-lookup"><span data-stu-id="93b45-107">This document is intended to help you diagnose and fix simple issues.</span></span> <span data-ttu-id="93b45-108">Nem célja a helyettesítheti a Microsoft támogatási szolgálatához.</span><span class="sxs-lookup"><span data-stu-id="93b45-108">It is not intended to be a replacement for Microsoft support.</span></span> <span data-ttu-id="93b45-109">Meg kell nyitnia a támogatási jegy [Microsoft támogatási szolgálatához](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) Ha nem tudja megoldani a problémát az alább ismertetett útmutatás.</span><span class="sxs-lookup"><span data-stu-id="93b45-109">You must open a support ticket with [Microsoft support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) if you are unable to solve the problem using the guidance described below.</span></span>
> 
> 

## <a name="address-resolution-protocol-arp-and-arp-tables"></a><span data-ttu-id="93b45-110">Cím Resolution Protocol (ARP) és a ARP</span><span class="sxs-lookup"><span data-stu-id="93b45-110">Address Resolution Protocol (ARP) and ARP tables</span></span>
<span data-ttu-id="93b45-111">Address Resolution Protocol (ARP) definiálva a 2. réteg protokoll [RFC 826](https://tools.ietf.org/html/rfc826).</span><span class="sxs-lookup"><span data-stu-id="93b45-111">Address Resolution Protocol (ARP) is a layer 2 protocol defined in [RFC 826](https://tools.ietf.org/html/rfc826).</span></span> <span data-ttu-id="93b45-112">ARP szolgál az Ethernet-cím (MAC-cím) IP-címet hozzárendelni.</span><span class="sxs-lookup"><span data-stu-id="93b45-112">ARP is used to map the Ethernet address (MAC address) with an ip address.</span></span>

<span data-ttu-id="93b45-113">A táblázat ARP az ipv4-cím és MAC-címet adott társviszony-létesítés leképezéseket.</span><span class="sxs-lookup"><span data-stu-id="93b45-113">The ARP table provides a mapping of the ipv4 address and MAC address for a particular peering.</span></span> <span data-ttu-id="93b45-114">Egy ExpressRoute-kapcsolatcsoport társviszonyt ARP táblázatban a következő információkkal csatolóhoz (elsődleges és másodlagos)</span><span class="sxs-lookup"><span data-stu-id="93b45-114">The ARP table for an ExpressRoute circuit peering provides the following information for each interface (primary and secondary)</span></span>

1. <span data-ttu-id="93b45-115">A helyi útválasztó illesztő IP-címet a MAC-cím hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="93b45-115">Mapping of on-premises router interface ip address to the MAC address</span></span>
2. <span data-ttu-id="93b45-116">ExpressRoute útválasztó illesztő IP-címet a MAC-cím hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="93b45-116">Mapping of ExpressRoute router interface ip address to the MAC address</span></span>
3. <span data-ttu-id="93b45-117">A leképezési korát</span><span class="sxs-lookup"><span data-stu-id="93b45-117">Age of the mapping</span></span>

<span data-ttu-id="93b45-118">ARP-táblázatok érvényesíteni a 2. réteg segítségével, és alapvető hibaelhárítási réteg 2 kapcsolódási problémák.</span><span class="sxs-lookup"><span data-stu-id="93b45-118">ARP tables can help validate layer 2 configuration and troubleshooting basic layer 2 connectivity issues.</span></span> 

<span data-ttu-id="93b45-119">Példa ARP-táblázat:</span><span class="sxs-lookup"><span data-stu-id="93b45-119">Example ARP table:</span></span> 

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1   ffff.eeee.dddd
          0 Microsoft         10.0.0.2   aaaa.bbbb.cccc


<span data-ttu-id="93b45-120">A következő szakasz tájékoztatást nyújt a láthatók az ExpressRoute peremhálózati útválasztók ARP-táblázatok megtekintésének.</span><span class="sxs-lookup"><span data-stu-id="93b45-120">The following section provides information on how you can view the ARP tables seen by the ExpressRoute edge routers.</span></span> 

## <a name="prerequisites-for-learning-arp-tables"></a><span data-ttu-id="93b45-121">ARP-táblázatok tanulási előfeltételei</span><span class="sxs-lookup"><span data-stu-id="93b45-121">Prerequisites for learning ARP tables</span></span>
<span data-ttu-id="93b45-122">Győződjön meg arról, hogy rendelkezik a következő, mielőtt további előrehaladás</span><span class="sxs-lookup"><span data-stu-id="93b45-122">Ensure that you have the following before you progress further</span></span>

* <span data-ttu-id="93b45-123">Egy érvényes ExpressRoute-kapcsolatcsoportot legalább egy társviszony-létesítés konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="93b45-123">A Valid ExpressRoute circuit configured with at least one peering.</span></span> <span data-ttu-id="93b45-124">A kapcsolatcsoport teljesen kell beállítani a kapcsolat szolgáltatóját.</span><span class="sxs-lookup"><span data-stu-id="93b45-124">The circuit must be fully configured by the connectivity provider.</span></span> <span data-ttu-id="93b45-125">Ön (vagy a kapcsolat szolgáltatóját) kell konfigurálni kell a társviszony (Azure saját, az Azure nyilvános és a Microsoft) közül legalább egy ebben a kapcsolatcsoportban.</span><span class="sxs-lookup"><span data-stu-id="93b45-125">You (or your connectivity provider) must have configured at least one of the peerings (Azure private, Azure public and Microsoft) on this circuit.</span></span>
* <span data-ttu-id="93b45-126">A társviszony (Azure saját, az Azure nyilvános és a Microsoft) konfigurálásához használt IP-címtartományok.</span><span class="sxs-lookup"><span data-stu-id="93b45-126">IP address ranges used for configuring the peerings (Azure private, Azure public and Microsoft).</span></span> <span data-ttu-id="93b45-127">Tekintse át az ip-cím hozzárendelés szereplő példák a [ExpressRoute útválasztási követelmények lapon](expressroute-routing.md) megértéséhez hogyan IP-címek vannak leképezve az ügyféloldali és az ExpressRoute oldalán felületek segítségével.</span><span class="sxs-lookup"><span data-stu-id="93b45-127">Review the ip address assignment examples in the [ExpressRoute routing requirements page](expressroute-routing.md) to get an understanding of how ip addresses are mapped to interfaces on your side and on the ExpressRoute side.</span></span> <span data-ttu-id="93b45-128">A társviszony-létesítési konfiguráció tájékoztatást kaphat megtekintésével a [ExpressRoute-társviszony-létesítési konfiguráció lapon](expressroute-howto-routing-arm.md).</span><span class="sxs-lookup"><span data-stu-id="93b45-128">You can get information on the peering configuration by reviewing the [ExpressRoute peering configuration page](expressroute-howto-routing-arm.md).</span></span>
* <span data-ttu-id="93b45-129">A hálózati csoport adatait / ezek IP-címekkel rendelkező használt adapterek MAC-címet a kapcsolat szolgáltatóját.</span><span class="sxs-lookup"><span data-stu-id="93b45-129">Information from your networking team / connectivity provider on the MAC addresses of interfaces used with these IP addresses.</span></span>
* <span data-ttu-id="93b45-130">A legújabb PowerShell-modult az Azure-ba (1,50 vagy újabb verzió) kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="93b45-130">You must have the latest PowerShell module for Azure (version 1.50 or newer).</span></span>

## <a name="getting-the-arp-tables-for-your-expressroute-circuit"></a><span data-ttu-id="93b45-131">Az ExpressRoute-kapcsolatcsoportot az ARP tábla beolvasása</span><span class="sxs-lookup"><span data-stu-id="93b45-131">Getting the ARP tables for your ExpressRoute circuit</span></span>
<span data-ttu-id="93b45-132">Ez a szakasz utasításokat biztosít a ARP-táblázatok / társviszony a PowerShell használatával megtekinteni.</span><span class="sxs-lookup"><span data-stu-id="93b45-132">This section provides instructions on how you can view the ARP tables per peering using PowerShell.</span></span> <span data-ttu-id="93b45-133">Ön vagy a kapcsolat szolgáltatójánál úgy kell konfigurálnia a társviszony-létesítés mielőtt elmélyedne a további.</span><span class="sxs-lookup"><span data-stu-id="93b45-133">You or your connectivity provider must have configured the peering before progressing further.</span></span> <span data-ttu-id="93b45-134">Minden kapcsolat van két elérési útnak (elsődleges és másodlagos).</span><span class="sxs-lookup"><span data-stu-id="93b45-134">Each circuit has two paths (primary and secondary).</span></span> <span data-ttu-id="93b45-135">Az egyes elérési utakat ARP-táblázat egymástól függetlenül ellenőrizheti.</span><span class="sxs-lookup"><span data-stu-id="93b45-135">You can check the ARP table for each path independently.</span></span>

### <a name="arp-tables-for-azure-private-peering"></a><span data-ttu-id="93b45-136">Az Azure magánhálózati társviszony-létesítés ARP-táblázatok</span><span class="sxs-lookup"><span data-stu-id="93b45-136">ARP tables for Azure private peering</span></span>
<span data-ttu-id="93b45-137">A következő parancsmag biztosít az ARP táblák Azure magánhálózati társviszony-létesítés</span><span class="sxs-lookup"><span data-stu-id="93b45-137">The following cmdlet provides the ARP tables for Azure private peering</span></span>

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"

        # ARP table for Azure private peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePrivatePeering -DevicePath Primary

        # ARP table for Azure private peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePrivatePeering -DevicePath Secondary 

<span data-ttu-id="93b45-138">Az elérési utak közül legalább egy alább minta kimenet</span><span class="sxs-lookup"><span data-stu-id="93b45-138">Sample output is shown below for one of the paths</span></span>

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1   ffff.eeee.dddd
          0 Microsoft         10.0.0.2   aaaa.bbbb.cccc


### <a name="arp-tables-for-azure-public-peering"></a><span data-ttu-id="93b45-139">Az Azure nyilvános társviszony ARP-táblázatok</span><span class="sxs-lookup"><span data-stu-id="93b45-139">ARP tables for Azure public peering</span></span>
<span data-ttu-id="93b45-140">A következő parancsmag biztosít az ARP táblák az Azure nyilvános társviszony-létesítés</span><span class="sxs-lookup"><span data-stu-id="93b45-140">The following cmdlet provides the ARP tables for Azure public peering</span></span>

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"

        # ARP table for Azure public peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePublicPeering -DevicePath Primary

        # ARP table for Azure public peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePublicPeering -DevicePath Secondary 


<span data-ttu-id="93b45-141">Az elérési utak közül legalább egy alább minta kimenet</span><span class="sxs-lookup"><span data-stu-id="93b45-141">Sample output is shown below for one of the paths</span></span>

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           64.0.0.1   ffff.eeee.dddd
          0 Microsoft         64.0.0.2   aaaa.bbbb.cccc


### <a name="arp-tables-for-microsoft-peering"></a><span data-ttu-id="93b45-142">A Microsoft társviszony-létesítéshez ARP-táblázatok</span><span class="sxs-lookup"><span data-stu-id="93b45-142">ARP tables for Microsoft peering</span></span>
<span data-ttu-id="93b45-143">A következő parancsmag biztosít az ARP táblák Microsoft társviszony-létesítés</span><span class="sxs-lookup"><span data-stu-id="93b45-143">The following cmdlet provides the ARP tables for Microsoft peering</span></span>

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"

        # ARP table for Microsoft peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType MicrosoftPeering -DevicePath Primary

        # ARP table for Microsoft peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType MicrosoftPeering -DevicePath Secondary 


<span data-ttu-id="93b45-144">Az elérési utak közül legalább egy alább minta kimenet</span><span class="sxs-lookup"><span data-stu-id="93b45-144">Sample output is shown below for one of the paths</span></span>

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1   ffff.eeee.dddd
          0 Microsoft         65.0.0.2   aaaa.bbbb.cccc


## <a name="how-to-use-this-information"></a><span data-ttu-id="93b45-145">Ezek az információk használata</span><span class="sxs-lookup"><span data-stu-id="93b45-145">How to use this information</span></span>
<span data-ttu-id="93b45-146">A társviszony ARP-táblázat segítségével határozza meg 2. réteg konfiguráció és a kapcsolat ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="93b45-146">The ARP table of a peering can be used to determine validate layer 2 configuration and connectivity.</span></span> <span data-ttu-id="93b45-147">Ez a szakasz áttekintést ARP-táblázatok megjelenését a különböző helyzetekben.</span><span class="sxs-lookup"><span data-stu-id="93b45-147">This section provides an overview of how ARP tables will look under different scenarios.</span></span>

### <a name="arp-table-when-a-circuit-is-in-operational-state-expected-state"></a><span data-ttu-id="93b45-148">ARP-táblázat Ha expressroute-kapcsolatcsoporthoz működési állapot (várt állapot)</span><span class="sxs-lookup"><span data-stu-id="93b45-148">ARP table when a circuit is in operational state (expected state)</span></span>
* <span data-ttu-id="93b45-149">ARP-táblázat egy bejegyzést a helyszíni oldalon egy érvényes IP-cím és MAC-cím és egy hasonló bejegyzést a Microsoft oldalon lesz.</span><span class="sxs-lookup"><span data-stu-id="93b45-149">The ARP table will have an entry for the on-premises side with a valid IP address and MAC address and a similar entry for the Microsoft side.</span></span> 
* <span data-ttu-id="93b45-150">A helyi IP-cím utolsó oktettje mindig lesz páratlan szám.</span><span class="sxs-lookup"><span data-stu-id="93b45-150">The last octet of the on-premises ip address will always be an odd number.</span></span>
* <span data-ttu-id="93b45-151">A Microsoft IP-cím utolsó oktettje mindig lesz páros szám.</span><span class="sxs-lookup"><span data-stu-id="93b45-151">The last octet of the Microsoft ip address will always be an even number.</span></span>
* <span data-ttu-id="93b45-152">Az azonos MAC-cím jelenik meg a Microsoft oldalon az összes 3 társviszony (elsődleges / másodlagos).</span><span class="sxs-lookup"><span data-stu-id="93b45-152">The same MAC address will appear on the Microsoft side for all 3 peerings (primary / secondary).</span></span> 

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1   ffff.eeee.dddd
          0 Microsoft         65.0.0.2   aaaa.bbbb.cccc

### <a name="arp-table-when-on-premises--connectivity-provider-side-has-problems"></a><span data-ttu-id="93b45-153">ARP táblából helyszíni / szolgáltató kiszolgálóoldali csatlakozási problémák vannak</span><span class="sxs-lookup"><span data-stu-id="93b45-153">ARP table when on-premises / connectivity provider side has problems</span></span>
<span data-ttu-id="93b45-154">Ha a helyszíni problémák vagy láthatja, hogy vagy csak egy bejegyzés az ARP-táblázat vagy a helyszíni MAC-cím fog megjelenni a kapcsolat szolgáltatójánál hiányos jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="93b45-154">If there are issues with the on-premises or connectivity provider you may see that either only one entry will appear in the ARP table or the on-prem MAC address will show incomplete.</span></span> <span data-ttu-id="93b45-155">Ez megjeleníti a MAC-cím és a Microsoft oldal használt IP-cím közötti leképezést.</span><span class="sxs-lookup"><span data-stu-id="93b45-155">This will show the mapping between the MAC address and IP address used in the Microsoft side.</span></span> 
  
       Age InterfaceProperty IpAddress  MacAddress    
       --- ----------------- ---------  ----------    
         0 Microsoft         65.0.0.2   aaaa.bbbb.cccc

<span data-ttu-id="93b45-156">vagy</span><span class="sxs-lookup"><span data-stu-id="93b45-156">or</span></span>
       
       Age InterfaceProperty IpAddress  MacAddress    
       --- ----------------- ---------  ----------   
         0 On-Prem           65.0.0.1   Incomplete
         0 Microsoft         65.0.0.2   aaaa.bbbb.cccc


> [!NOTE]
> <span data-ttu-id="93b45-157">A kapcsolat szolgáltatójánál, az ilyen problémák hibakeresését támogatási kérést nyithat.</span><span class="sxs-lookup"><span data-stu-id="93b45-157">Open a support request with your connectivity provider to debug such issues.</span></span> <span data-ttu-id="93b45-158">Ha az ARP-táblázat nem rendelkezik a MAC-címek hozzárendelve felületek IP-címét, áttekintheti a következőket:</span><span class="sxs-lookup"><span data-stu-id="93b45-158">If the ARP table does not have IP addresses of the interfaces mapped to MAC addresses, review the following information:</span></span>
> 
> 1. <span data-ttu-id="93b45-159">Ha az első IP-címét a/30-as alhálózat hozzárendelt MSEE-PR és MSEE közötti kapcsolat MSEE-PR. felületén használja a rendszer</span><span class="sxs-lookup"><span data-stu-id="93b45-159">If the first IP address of the /30 subnet assigned for the link between the MSEE-PR and MSEE is used on the interface of MSEE-PR.</span></span> <span data-ttu-id="93b45-160">Azure mindig MSEEs a második IP-címet használja.</span><span class="sxs-lookup"><span data-stu-id="93b45-160">Azure always uses the second IP address for MSEEs.</span></span>
> 2. <span data-ttu-id="93b45-161">Győződjön meg arról, ha az ügyfél (C-címke) és a VLAN-címkék szolgáltatás (S-címke) felel meg a MSEE-PR és MSEE pár mindkét.</span><span class="sxs-lookup"><span data-stu-id="93b45-161">Verify if the customer (C-Tag) and service (S-Tag) VLAN tags match both on MSEE-PR and MSEE pair.</span></span>
> 

### <a name="arp-table-when-microsoft-side-has-problems"></a><span data-ttu-id="93b45-162">Ha a Microsoft ügyféloldali problémák vannak az ARP-táblázat</span><span class="sxs-lookup"><span data-stu-id="93b45-162">ARP table when Microsoft side has problems</span></span>
* <span data-ttu-id="93b45-163">Nem látják az ARP-táblázat társviszony-létesítés Ha problémák vannak a Microsoft oldalán látható.</span><span class="sxs-lookup"><span data-stu-id="93b45-163">You will not see an ARP table shown for a peering if there are issues on the Microsoft side.</span></span> 
* <span data-ttu-id="93b45-164">A támogatási jegy megnyitása [Microsoft támogatási szolgálatához](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span><span class="sxs-lookup"><span data-stu-id="93b45-164">Open a support ticket with [Microsoft support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span></span> <span data-ttu-id="93b45-165">Adja meg, hogy rendelkezik-e 2. rétegbeli kapcsolatot kapcsolatos problémát.</span><span class="sxs-lookup"><span data-stu-id="93b45-165">Specify that you have an issue with layer 2 connectivity.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="93b45-166">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="93b45-166">Next Steps</span></span>
* <span data-ttu-id="93b45-167">Ellenőrizze a ExpressRoute-kapcsolatcsoportot 3. rétegbeli konfigurációi</span><span class="sxs-lookup"><span data-stu-id="93b45-167">Validate Layer 3 configurations for your ExpressRoute circuit</span></span>
  * <span data-ttu-id="93b45-168">A BGP-munkamenetek állapotának megállapításához ellenőrizze az összefoglaló lekérése útvonal</span><span class="sxs-lookup"><span data-stu-id="93b45-168">Get route summary to determine the state of BGP sessions</span></span> 
  * <span data-ttu-id="93b45-169">Annak meghatározásához, hogy mely előtagok ExpressRoute között van-e hirdetve útvonaltábla beolvasása</span><span class="sxs-lookup"><span data-stu-id="93b45-169">Get route table to determine which prefixes are advertised across ExpressRoute</span></span>
* <span data-ttu-id="93b45-170">Be- / kimeneti bájt megtekintésével adatátvitel ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="93b45-170">Validate data transfer by reviewing bytes in / out</span></span>
* <span data-ttu-id="93b45-171">A támogatási jegy megnyitása [Microsoft támogatási szolgálatához](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) Ha továbbra is problémákat tapasztal.</span><span class="sxs-lookup"><span data-stu-id="93b45-171">Open a support ticket with [Microsoft support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) if you are still experiencing issues.</span></span>

