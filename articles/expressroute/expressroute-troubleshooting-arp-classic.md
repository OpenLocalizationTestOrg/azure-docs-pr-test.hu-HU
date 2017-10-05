---
title: "ARP-tábla beolvasása: klasszikus: Azure ExpressRoute-hibaelhárítási |} Microsoft Docs"
description: "Ezen a lapon a ARP tábla beolvasása az ExpressRoute-kapcsolatcsoportot ismerteti."
documentationcenter: na
services: expressroute
author: ganesr
manager: carolz
editor: tysonn
ms.assetid: b5856acf-03c2-4933-8111-6ce12998d92a
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/30/2017
ms.author: ganesr
ms.openlocfilehash: fcc847b7e30fd55ca759830e0254ab7542e7663e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="getting-arp-tables-in-the-classic-deployment-model"></a><span data-ttu-id="84691-103">A klasszikus üzembe helyezési modellel tábla ARP beolvasása</span><span class="sxs-lookup"><span data-stu-id="84691-103">Getting ARP tables in the classic deployment model</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="84691-104">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="84691-104">PowerShell - Resource Manager</span></span>](expressroute-troubleshooting-arp-resource-manager.md)
> * [<span data-ttu-id="84691-105">PowerShell – Klasszikus</span><span class="sxs-lookup"><span data-stu-id="84691-105">PowerShell - Classic</span></span>](expressroute-troubleshooting-arp-classic.md)
> 
> 

<span data-ttu-id="84691-106">Ez a cikk végigvezeti az Address Resolution Protocol (ARP) tábla beolvasása az Azure ExpressRoute-kör számára.</span><span class="sxs-lookup"><span data-stu-id="84691-106">This article walks you through the steps for getting the Address Resolution Protocol (ARP) tables for your Azure ExpressRoute circuit.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="84691-107">Ez a dokumentum olyan egyszerű problémák megoldásában segítséget.</span><span class="sxs-lookup"><span data-stu-id="84691-107">This document is intended to help you diagnose and fix simple issues.</span></span> <span data-ttu-id="84691-108">Nem célja a helyettesítheti a Microsoft támogatási szolgálatához.</span><span class="sxs-lookup"><span data-stu-id="84691-108">It is not intended to be a replacement for Microsoft support.</span></span> <span data-ttu-id="84691-109">Az alábbi útmutató segítségével nem tudja megoldani a problémát, ha a támogatási kérést nyithat [Azure a Microsoft Súgó és támogatás](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span><span class="sxs-lookup"><span data-stu-id="84691-109">If you can't solve the problem by using the following guidance, open a support request with [Microsoft Azure Help+support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span></span>
> 
> 

## <a name="address-resolution-protocol-arp-and-arp-tables"></a><span data-ttu-id="84691-110">Cím Resolution Protocol (ARP) és a ARP</span><span class="sxs-lookup"><span data-stu-id="84691-110">Address Resolution Protocol (ARP) and ARP tables</span></span>
<span data-ttu-id="84691-111">ARP egy 2. rétegbeli protokoll, amely meghatározott [RFC 826](https://tools.ietf.org/html/rfc826).</span><span class="sxs-lookup"><span data-stu-id="84691-111">ARP is a Layer 2 protocol that's defined in [RFC 826](https://tools.ietf.org/html/rfc826).</span></span> <span data-ttu-id="84691-112">ARP szolgál az Ethernet-címe (MAC-cím) leképezése egy IP-címet.</span><span class="sxs-lookup"><span data-stu-id="84691-112">ARP is used to map an Ethernet address (MAC address) to an IP address.</span></span>

<span data-ttu-id="84691-113">Egy táblázat ARP az IPv4-cím és MAC-címet adott társviszony-létesítés leképezéseket.</span><span class="sxs-lookup"><span data-stu-id="84691-113">An ARP table provides a mapping of the IPv4 address and MAC address for a particular peering.</span></span> <span data-ttu-id="84691-114">Egy ExpressRoute-kapcsolatcsoport társviszonyt az ARP-táblázat az egyes csatolókra (elsődleges és másodlagos) a következő adatokat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="84691-114">The ARP table for an ExpressRoute circuit peering provides the following information for each interface (primary and secondary):</span></span>

1. <span data-ttu-id="84691-115">A MAC-címet a helyi útválasztó illesztő IP-címet leképezése</span><span class="sxs-lookup"><span data-stu-id="84691-115">Mapping of an on-premises router interface IP address to a MAC address</span></span>
2. <span data-ttu-id="84691-116">ExpressRoute útválasztó illesztő IP-címet a MAC-cím hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="84691-116">Mapping of an ExpressRoute router interface IP address to a MAC address</span></span>
3. <span data-ttu-id="84691-117">A leképezési korát</span><span class="sxs-lookup"><span data-stu-id="84691-117">The age of the mapping</span></span>

<span data-ttu-id="84691-118">ARP-táblázatok segíthet a 2. rétegbeli konfigurációjának ellenőrzése és a 2. rétegbeli kapcsolatot alapvető problémák elhárításához.</span><span class="sxs-lookup"><span data-stu-id="84691-118">ARP tables can help with validating Layer 2 configuration and with troubleshooting basic Layer 2 connectivity issues.</span></span>

<span data-ttu-id="84691-119">Az alábbiakban egy egy ARP-táblázata – példa látható:</span><span class="sxs-lookup"><span data-stu-id="84691-119">Following is an example of an ARP table:</span></span>

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


<span data-ttu-id="84691-120">Az alábbi szakasz ismerteti a láthatók az ExpressRoute peremhálózati útválasztók ARP táblákhoz megtekintése.</span><span class="sxs-lookup"><span data-stu-id="84691-120">The following section provides information about how to view the ARP tables that are seen by the ExpressRoute edge routers.</span></span>

## <a name="prerequisites-for-using-arp-tables"></a><span data-ttu-id="84691-121">ARP-táblázatok használatára vonatkozó Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="84691-121">Prerequisites for using ARP tables</span></span>
<span data-ttu-id="84691-122">Győződjön meg arról, hogy rendelkezik a következő, a folytatás előtt:</span><span class="sxs-lookup"><span data-stu-id="84691-122">Ensure that you have the following before you continue:</span></span>

* <span data-ttu-id="84691-123">Egy érvényes ExpressRoute-kapcsolatcsoportot legalább egy társviszony-létesítés konfigurálva van.</span><span class="sxs-lookup"><span data-stu-id="84691-123">A valid ExpressRoute circuit that's configured with at least one peering.</span></span> <span data-ttu-id="84691-124">A kapcsolatcsoport teljesen kell beállítani a kapcsolat szolgáltatóját.</span><span class="sxs-lookup"><span data-stu-id="84691-124">The circuit must be fully configured by the connectivity provider.</span></span> <span data-ttu-id="84691-125">Ön (vagy a kapcsolat szolgáltatóját) kell konfigurálnia a társviszony (Azure saját, az Azure nyilvános, vagy a Microsoft) közül legalább egy ebben a kapcsolatcsoportban.</span><span class="sxs-lookup"><span data-stu-id="84691-125">You (or your connectivity provider) must configure at least one of the peerings (Azure private, Azure public, or Microsoft) on this circuit.</span></span>
* <span data-ttu-id="84691-126">IP-címtartományok, amelyeket a társviszony (Azure saját, az Azure nyilvános, és a Microsoft) konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="84691-126">IP address ranges that are used for configuring the peerings (Azure private, Azure public, and Microsoft).</span></span> <span data-ttu-id="84691-127">Tekintse át az IP-cím hozzárendelés szereplő példák a [ExpressRoute útválasztási követelmények lapon](expressroute-routing.md) megismerhesse, milyen IP-címek vannak leképezve a aise és az ExpressRoute oldalán felületek segítségével.</span><span class="sxs-lookup"><span data-stu-id="84691-127">Review the IP address assignment examples in the [ExpressRoute routing requirements page](expressroute-routing.md) to get an understanding of how IP addresses are mapped to interfaces on your aise and on the ExpressRoute side.</span></span> <span data-ttu-id="84691-128">Kaphat a társviszony-létesítési konfigurációs információinak megtekintésével a [ExpressRoute-társviszony-létesítési konfiguráció lapon](expressroute-howto-routing-classic.md).</span><span class="sxs-lookup"><span data-stu-id="84691-128">You can get information about the peering configuration by reviewing the [ExpressRoute peering configuration page](expressroute-howto-routing-classic.md).</span></span>
* <span data-ttu-id="84691-129">Származó információkat a hálózati csoport vagy a kapcsolat szolgáltatójánál MAC-címet az IP-címeket használt csatolók.</span><span class="sxs-lookup"><span data-stu-id="84691-129">Information from your networking team or connectivity provider about the MAC addresses of the interfaces that are used with these IP addresses.</span></span>
* <span data-ttu-id="84691-130">A legújabb Windows PowerShell-modult az Azure-ba (1,50 vagy újabb verzió).</span><span class="sxs-lookup"><span data-stu-id="84691-130">The latest Windows PowerShell module for Azure (version 1.50 or later).</span></span>

## <a name="arp-tables-for-your-expressroute-circuit"></a><span data-ttu-id="84691-131">Az ExpressRoute-kapcsolatcsoportot ARP-táblázatok</span><span class="sxs-lookup"><span data-stu-id="84691-131">ARP tables for your ExpressRoute circuit</span></span>
<span data-ttu-id="84691-132">Ez a témakör leírja, hogyan megtekintéséhez az ARP-táblázatok az egyes társviszony-létesítés PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="84691-132">This section provides instructions about how to view the ARP tables for each type of peering by using PowerShell.</span></span> <span data-ttu-id="84691-133">A folytatás előtt Ön vagy a kapcsolat szolgáltatójánál be kell állítania a társviszony-létesítést.</span><span class="sxs-lookup"><span data-stu-id="84691-133">Before you continue, either you or your connectivity provider needs to configure the peering.</span></span> <span data-ttu-id="84691-134">Minden kapcsolat van két elérési útnak (elsődleges és másodlagos).</span><span class="sxs-lookup"><span data-stu-id="84691-134">Each circuit has two paths (primary and secondary).</span></span> <span data-ttu-id="84691-135">Az egyes elérési utakat ARP-táblázat egymástól függetlenül ellenőrizheti.</span><span class="sxs-lookup"><span data-stu-id="84691-135">You can check the ARP table for each path independently.</span></span>

### <a name="arp-tables-for-azure-private-peering"></a><span data-ttu-id="84691-136">Az Azure magánhálózati társviszony-létesítés ARP-táblázatok</span><span class="sxs-lookup"><span data-stu-id="84691-136">ARP tables for Azure private peering</span></span>
<span data-ttu-id="84691-137">A következő parancsmag nyújt az ARP táblák Azure magánhálózati társviszony-létesítés.</span><span class="sxs-lookup"><span data-stu-id="84691-137">The following cmdlet provides the ARP tables for Azure private peering:</span></span>

        # Required variables
        $ckt = "<your Service Key here>

        # ARP table for Azure private peering--primary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Private -Path Primary

        # ARP table for Azure private peering--secondary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Private -Path Secondary

<span data-ttu-id="84691-138">Az alábbiakban látható a elérési utak közül legalább egy minta kimenet:</span><span class="sxs-lookup"><span data-stu-id="84691-138">Following is sample output for one of the paths:</span></span>

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-azure-public-peering"></a><span data-ttu-id="84691-139">Az Azure nyilvános társviszony ARP-táblázatok:</span><span class="sxs-lookup"><span data-stu-id="84691-139">ARP tables for Azure public peering:</span></span>
<span data-ttu-id="84691-140">A következő parancsmag biztosít az ARP táblák az Azure nyilvános társviszony:</span><span class="sxs-lookup"><span data-stu-id="84691-140">The following cmdlet provides the ARP tables for Azure public peering:</span></span>

        # Required variables
        $ckt = "<your Service Key here>

        # ARP table for Azure public peering--primary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Public -Path Primary

        # ARP table for Azure public peering--secondary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Public -Path Secondary

<span data-ttu-id="84691-141">Az alábbiakban látható a elérési utak közül legalább egy minta kimenet:</span><span class="sxs-lookup"><span data-stu-id="84691-141">Following is sample output for one of the paths:</span></span>

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


<span data-ttu-id="84691-142">Az alábbiakban látható a elérési utak közül legalább egy minta kimenet:</span><span class="sxs-lookup"><span data-stu-id="84691-142">Following is sample output for one of the paths:</span></span>

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           64.0.0.1 ffff.eeee.dddd
          0 Microsoft         64.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-microsoft-peering"></a><span data-ttu-id="84691-143">A Microsoft társviszony-létesítéshez ARP-táblázatok</span><span class="sxs-lookup"><span data-stu-id="84691-143">ARP tables for Microsoft peering</span></span>
<span data-ttu-id="84691-144">A következő parancsmag táblák nyújt az ARP a Microsoft társviszony-létesítéshez:</span><span class="sxs-lookup"><span data-stu-id="84691-144">The following cmdlet provides the ARP tables for Microsoft peering:</span></span>

    # ARP table for Microsoft peering--primary path
    Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Microsoft -Path Primary

    # ARP table for Microsoft peering--secondary path
    Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Microsoft -Path Secondary


<span data-ttu-id="84691-145">Minta kimenet az elérési utak közül legalább egy alább találja:</span><span class="sxs-lookup"><span data-stu-id="84691-145">Sample output is shown below for one of the paths:</span></span>

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc


## <a name="how-to-use-this-information"></a><span data-ttu-id="84691-146">Ezek az információk használata</span><span class="sxs-lookup"><span data-stu-id="84691-146">How to use this information</span></span>
<span data-ttu-id="84691-147">A társviszony ARP-táblázat segítségével 2. rétegbeli konfiguráció és a kapcsolat ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="84691-147">The ARP table of a peering can be used to validate Layer 2 configuration and connectivity.</span></span> <span data-ttu-id="84691-148">Ez a szakasz áttekintést ARP-táblázatok megjelenését különböző helyzetekben.</span><span class="sxs-lookup"><span data-stu-id="84691-148">This section provides an overview of how ARP tables look in different scenarios.</span></span>

### <a name="arp-table-when-a-circuit-is-in-an-operational-expected-state"></a><span data-ttu-id="84691-149">ARP táblából, ha expressroute-kapcsolatcsoporthoz (várt) működési állapotban van.</span><span class="sxs-lookup"><span data-stu-id="84691-149">ARP table when a circuit is in an operational (expected) state</span></span>
* <span data-ttu-id="84691-150">ARP-táblázat bejegyzést tartalmaz, a helyszíni oldalának egy érvényes IP-cím és MAC-címmel rendelkező, és a Microsoft oldalon egy hasonló bejegyzést.</span><span class="sxs-lookup"><span data-stu-id="84691-150">The ARP table has an entry for the on-premises side with a valid IP and MAC address, and a similar entry for the Microsoft side.</span></span>
* <span data-ttu-id="84691-151">A helyi IP-cím utolsó oktettje mindig páratlan szám.</span><span class="sxs-lookup"><span data-stu-id="84691-151">The last octet of the on-premises IP address is always an odd number.</span></span>
* <span data-ttu-id="84691-152">A Microsoft IP-cím utolsó oktettje mindig páros szám-e.</span><span class="sxs-lookup"><span data-stu-id="84691-152">The last octet of the Microsoft IP address is always an even number.</span></span>
* <span data-ttu-id="84691-153">Az azonos MAC-cím jelenik meg a Microsoft az összes három esetében (elsődleges és másodlagos).</span><span class="sxs-lookup"><span data-stu-id="84691-153">The same MAC address appears on the Microsoft side for all three peerings (primary/secondary).</span></span>

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

### <a name="arp-table-when-its-on-premises-or-when-the-connectivity-provider-side-has-problems"></a><span data-ttu-id="84691-154">Ha a helyszíni, vagy ha a kapcsolat-szolgáltató oldalon problémák vannak az ARP-táblázat</span><span class="sxs-lookup"><span data-stu-id="84691-154">ARP table when it's on-premises or when the connectivity-provider side has problems</span></span>
 <span data-ttu-id="84691-155">Csak egy bejegyzés megjelenik az ARP-táblázatban.</span><span class="sxs-lookup"><span data-stu-id="84691-155">Only one entry appears in the ARP table.</span></span> <span data-ttu-id="84691-156">Azt mutatja, hogy a MAC-cím és az IP-cím, a Microsoft oldalon használt közötti leképezést.</span><span class="sxs-lookup"><span data-stu-id="84691-156">It shows the mapping between the MAC address and the IP address that's used on the Microsoft side.</span></span>

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

> [!NOTE]
> <span data-ttu-id="84691-157">Ehhez hasonló problémát tapasztal, a kapcsolat szolgáltatójánál megoldási támogatási kérést nyithat.</span><span class="sxs-lookup"><span data-stu-id="84691-157">If you experience an issue like this, open a support request with your connectivity provider to resolve it.</span></span>
> 
> 

### <a name="arp-table-when-the-microsoft-side-has-problems"></a><span data-ttu-id="84691-158">Ha a Microsoft oldalon problémák vannak az ARP-táblázat</span><span class="sxs-lookup"><span data-stu-id="84691-158">ARP table when the Microsoft side has problems</span></span>
* <span data-ttu-id="84691-159">Nem látják az ARP-táblázat társviszony-létesítés Ha problémák vannak a Microsoft oldalán látható.</span><span class="sxs-lookup"><span data-stu-id="84691-159">You will not see an ARP table shown for a peering if there are issues on the Microsoft side.</span></span>
* <span data-ttu-id="84691-160">A támogatási kérést nyithat [Azure a Microsoft Súgó és támogatás](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span><span class="sxs-lookup"><span data-stu-id="84691-160">Open a support request with [Microsoft Azure Help+support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span></span> <span data-ttu-id="84691-161">Adja meg, hogy rendelkezik-e 2. rétegbeli kapcsolatot kapcsolatos problémát.</span><span class="sxs-lookup"><span data-stu-id="84691-161">Specify that you have an issue with Layer 2 connectivity.</span></span>

## <a name="next-steps"></a><span data-ttu-id="84691-162">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="84691-162">Next steps</span></span>
* <span data-ttu-id="84691-163">Ellenőrizze a ExpressRoute-kapcsolatcsoportot 3. rétegbeli konfigurációi:</span><span class="sxs-lookup"><span data-stu-id="84691-163">Validate Layer 3 configurations for your ExpressRoute circuit:</span></span>
  * <span data-ttu-id="84691-164">Útvonal BGP-munkamenetek állapotának megállapításához ellenőrizze az összefoglaló lekérése.</span><span class="sxs-lookup"><span data-stu-id="84691-164">Get a route summary to determine the state of BGP sessions.</span></span>
  * <span data-ttu-id="84691-165">Egy útválasztási táblázatot meghatározni, melyik előtagok ExpressRoute között van-e hirdetve beolvasása.</span><span class="sxs-lookup"><span data-stu-id="84691-165">Get a route table to determine which prefixes are advertised across ExpressRoute.</span></span>
* <span data-ttu-id="84691-166">Adatátvitel érvényesítése küldött bájtok megtekintésével.</span><span class="sxs-lookup"><span data-stu-id="84691-166">Validate data transfer by reviewing bytes in and out.</span></span>
* <span data-ttu-id="84691-167">A támogatási kérést nyithat [Azure a Microsoft Súgó és támogatás](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) Ha továbbra is problémákat tapasztal.</span><span class="sxs-lookup"><span data-stu-id="84691-167">Open a support request with [Microsoft Azure Help+support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) if you are still experiencing issues.</span></span>

