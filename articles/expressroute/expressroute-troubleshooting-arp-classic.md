---
title: "ARP-tábla beolvasása: klasszikus: Azure ExpressRoute-hibaelhárítási |} Microsoft Docs"
description: "Ezen a lapon leírja az első hello ARP táblák ExpressRoute-kapcsolatcsoportot."
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
ms.openlocfilehash: 2b01304a38fa0e0def27dbd7c391d7ad8bbdabff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="getting-arp-tables-in-hello-classic-deployment-model"></a><span data-ttu-id="31987-103">Hello klasszikus üzembe helyezési modellel tábla ARP beolvasása</span><span class="sxs-lookup"><span data-stu-id="31987-103">Getting ARP tables in hello classic deployment model</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="31987-104">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="31987-104">PowerShell - Resource Manager</span></span>](expressroute-troubleshooting-arp-resource-manager.md)
> * [<span data-ttu-id="31987-105">PowerShell – Klasszikus</span><span class="sxs-lookup"><span data-stu-id="31987-105">PowerShell - Classic</span></span>](expressroute-troubleshooting-arp-classic.md)
> 
> 

<span data-ttu-id="31987-106">Ez a cikk végigvezeti hello hello Address Resolution Protocol (ARP) táblák beolvasása az Azure ExpressRoute-kapcsolatcsoport esetében.</span><span class="sxs-lookup"><span data-stu-id="31987-106">This article walks you through hello steps for getting hello Address Resolution Protocol (ARP) tables for your Azure ExpressRoute circuit.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="31987-107">Ez a dokumentum tervezett toohelp diagnosztizálhatja és egyszerű problémák megoldásával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="31987-107">This document is intended toohelp you diagnose and fix simple issues.</span></span> <span data-ttu-id="31987-108">Már nem tervezett toobe helyettesíti a Microsoft támogatási szolgálatához.</span><span class="sxs-lookup"><span data-stu-id="31987-108">It is not intended toobe a replacement for Microsoft support.</span></span> <span data-ttu-id="31987-109">Ha hello probléma a következő útmutatást hello segítségével sem tudja megoldani, a támogatási kérést nyithat [Azure a Microsoft Súgó és támogatás](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span><span class="sxs-lookup"><span data-stu-id="31987-109">If you can't solve hello problem by using hello following guidance, open a support request with [Microsoft Azure Help+support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span></span>
> 
> 

## <a name="address-resolution-protocol-arp-and-arp-tables"></a><span data-ttu-id="31987-110">Cím Resolution Protocol (ARP) és a ARP</span><span class="sxs-lookup"><span data-stu-id="31987-110">Address Resolution Protocol (ARP) and ARP tables</span></span>
<span data-ttu-id="31987-111">ARP egy 2. rétegbeli protokoll, amely meghatározott [RFC 826](https://tools.ietf.org/html/rfc826).</span><span class="sxs-lookup"><span data-stu-id="31987-111">ARP is a Layer 2 protocol that's defined in [RFC 826](https://tools.ietf.org/html/rfc826).</span></span> <span data-ttu-id="31987-112">ARP használt toomap Ethernet-címét (MAC) tooan IP-cím.</span><span class="sxs-lookup"><span data-stu-id="31987-112">ARP is used toomap an Ethernet address (MAC address) tooan IP address.</span></span>

<span data-ttu-id="31987-113">Az ARP-táblázat hello IPv4-cím és MAC-címet adott társviszony-létesítés leképezéseket.</span><span class="sxs-lookup"><span data-stu-id="31987-113">An ARP table provides a mapping of hello IPv4 address and MAC address for a particular peering.</span></span> <span data-ttu-id="31987-114">hello egy ExpressRoute-kapcsolatcsoport társviszonyt az ARP-táblázat minden egyes (elsődleges és másodlagos) kapcsolat adatai a következő hello biztosítja:</span><span class="sxs-lookup"><span data-stu-id="31987-114">hello ARP table for an ExpressRoute circuit peering provides hello following information for each interface (primary and secondary):</span></span>

1. <span data-ttu-id="31987-115">A helyi útválasztó illesztő IP-cím tooa MAC cím leképezése</span><span class="sxs-lookup"><span data-stu-id="31987-115">Mapping of an on-premises router interface IP address tooa MAC address</span></span>
2. <span data-ttu-id="31987-116">Az ExpressRoute útválasztó illesztő IP-cím tooa MAC címének leképezése</span><span class="sxs-lookup"><span data-stu-id="31987-116">Mapping of an ExpressRoute router interface IP address tooa MAC address</span></span>
3. <span data-ttu-id="31987-117">hello korát hello leképezése</span><span class="sxs-lookup"><span data-stu-id="31987-117">hello age of hello mapping</span></span>

<span data-ttu-id="31987-118">ARP-táblázatok segíthet a 2. rétegbeli konfigurációjának ellenőrzése és a 2. rétegbeli kapcsolatot alapvető problémák elhárításához.</span><span class="sxs-lookup"><span data-stu-id="31987-118">ARP tables can help with validating Layer 2 configuration and with troubleshooting basic Layer 2 connectivity issues.</span></span>

<span data-ttu-id="31987-119">Az alábbiakban egy egy ARP-táblázata – példa látható:</span><span class="sxs-lookup"><span data-stu-id="31987-119">Following is an example of an ARP table:</span></span>

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


<span data-ttu-id="31987-120">hello a következő szakasz ismerteti, hogyan tooview hello láthatók hello ExpressRoute peremhálózati útválasztók ARP-táblázatok.</span><span class="sxs-lookup"><span data-stu-id="31987-120">hello following section provides information about how tooview hello ARP tables that are seen by hello ExpressRoute edge routers.</span></span>

## <a name="prerequisites-for-using-arp-tables"></a><span data-ttu-id="31987-121">ARP-táblázatok használatára vonatkozó Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="31987-121">Prerequisites for using ARP tables</span></span>
<span data-ttu-id="31987-122">Győződjön meg arról, hogy rendelkezik hello következő, a folytatás előtt:</span><span class="sxs-lookup"><span data-stu-id="31987-122">Ensure that you have hello following before you continue:</span></span>

* <span data-ttu-id="31987-123">Egy érvényes ExpressRoute-kapcsolatcsoportot legalább egy társviszony-létesítés konfigurálva van.</span><span class="sxs-lookup"><span data-stu-id="31987-123">A valid ExpressRoute circuit that's configured with at least one peering.</span></span> <span data-ttu-id="31987-124">hello áramkör a hello kapcsolat szolgáltatójánál teljesen kell konfigurálni.</span><span class="sxs-lookup"><span data-stu-id="31987-124">hello circuit must be fully configured by hello connectivity provider.</span></span> <span data-ttu-id="31987-125">Ön (vagy a kapcsolat szolgáltatóját) kell konfigurálnia hello esetében (Azure saját, az Azure nyilvános, vagy a Microsoft) közül legalább egy ebben a kapcsolatcsoportban.</span><span class="sxs-lookup"><span data-stu-id="31987-125">You (or your connectivity provider) must configure at least one of hello peerings (Azure private, Azure public, or Microsoft) on this circuit.</span></span>
* <span data-ttu-id="31987-126">IP-címtartományok hello esetében (Azure saját, az Azure nyilvános, és a Microsoft) konfigurálásához használt.</span><span class="sxs-lookup"><span data-stu-id="31987-126">IP address ranges that are used for configuring hello peerings (Azure private, Azure public, and Microsoft).</span></span> <span data-ttu-id="31987-127">Tekintse át a hello IP cím hozzárendelés példák hello [ExpressRoute útválasztási követelmények lapon](expressroute-routing.md) tooget megismerhesse, milyen IP-címek vannak leképezve a aise és hello ExpressRoute oldalán toointerfaces.</span><span class="sxs-lookup"><span data-stu-id="31987-127">Review hello IP address assignment examples in hello [ExpressRoute routing requirements page](expressroute-routing.md) tooget an understanding of how IP addresses are mapped toointerfaces on your aise and on hello ExpressRoute side.</span></span> <span data-ttu-id="31987-128">Hello társviszony-létesítési konfiguráció információt kaphat a hello megtekintésével [ExpressRoute-társviszony-létesítési konfiguráció lapon](expressroute-howto-routing-classic.md).</span><span class="sxs-lookup"><span data-stu-id="31987-128">You can get information about hello peering configuration by reviewing hello [ExpressRoute peering configuration page](expressroute-howto-routing-classic.md).</span></span>
* <span data-ttu-id="31987-129">Információ a hálózati csoport vagy a kapcsolat szolgáltatójánál hello MAC-címek hello felületek, amelyek a következő IP-címek fel.</span><span class="sxs-lookup"><span data-stu-id="31987-129">Information from your networking team or connectivity provider about hello MAC addresses of hello interfaces that are used with these IP addresses.</span></span>
* <span data-ttu-id="31987-130">hello legújabb Windows PowerShell-modult az Azure-ba (1,50 vagy újabb verzió).</span><span class="sxs-lookup"><span data-stu-id="31987-130">hello latest Windows PowerShell module for Azure (version 1.50 or later).</span></span>

## <a name="arp-tables-for-your-expressroute-circuit"></a><span data-ttu-id="31987-131">Az ExpressRoute-kapcsolatcsoportot ARP-táblázatok</span><span class="sxs-lookup"><span data-stu-id="31987-131">ARP tables for your ExpressRoute circuit</span></span>
<span data-ttu-id="31987-132">Ez a szakasz bemutatja, hogyan tooview hello ARP táblák az egyes társviszony-létesítés PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="31987-132">This section provides instructions about how tooview hello ARP tables for each type of peering by using PowerShell.</span></span> <span data-ttu-id="31987-133">A folytatás előtt, vagy a kapcsolat szolgáltatójánál kell a tooconfigure hello társviszony-létesítés.</span><span class="sxs-lookup"><span data-stu-id="31987-133">Before you continue, either you or your connectivity provider needs tooconfigure hello peering.</span></span> <span data-ttu-id="31987-134">Minden kapcsolat van két elérési útnak (elsődleges és másodlagos).</span><span class="sxs-lookup"><span data-stu-id="31987-134">Each circuit has two paths (primary and secondary).</span></span> <span data-ttu-id="31987-135">ARP-táblázat az egyes elérési utakat hello egymástól függetlenül ellenőrizheti.</span><span class="sxs-lookup"><span data-stu-id="31987-135">You can check hello ARP table for each path independently.</span></span>

### <a name="arp-tables-for-azure-private-peering"></a><span data-ttu-id="31987-136">Az Azure magánhálózati társviszony-létesítés ARP-táblázatok</span><span class="sxs-lookup"><span data-stu-id="31987-136">ARP tables for Azure private peering</span></span>
<span data-ttu-id="31987-137">a következő parancsmag hello nyújt hello ARP táblák Azure magánhálózati társviszony-létesítés.</span><span class="sxs-lookup"><span data-stu-id="31987-137">hello following cmdlet provides hello ARP tables for Azure private peering:</span></span>

        # Required variables
        $ckt = "<your Service Key here>

        # ARP table for Azure private peering--primary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Private -Path Primary

        # ARP table for Azure private peering--secondary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Private -Path Secondary

<span data-ttu-id="31987-138">Az alábbiakban látható minta kimenet hello elérési utak egyike:</span><span class="sxs-lookup"><span data-stu-id="31987-138">Following is sample output for one of hello paths:</span></span>

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-azure-public-peering"></a><span data-ttu-id="31987-139">Az Azure nyilvános társviszony ARP-táblázatok:</span><span class="sxs-lookup"><span data-stu-id="31987-139">ARP tables for Azure public peering:</span></span>
<span data-ttu-id="31987-140">a következő parancsmag hello biztosít hello ARP táblák az Azure nyilvános társviszony:</span><span class="sxs-lookup"><span data-stu-id="31987-140">hello following cmdlet provides hello ARP tables for Azure public peering:</span></span>

        # Required variables
        $ckt = "<your Service Key here>

        # ARP table for Azure public peering--primary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Public -Path Primary

        # ARP table for Azure public peering--secondary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Public -Path Secondary

<span data-ttu-id="31987-141">Az alábbiakban látható minta kimenet hello elérési utak egyike:</span><span class="sxs-lookup"><span data-stu-id="31987-141">Following is sample output for one of hello paths:</span></span>

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


<span data-ttu-id="31987-142">Az alábbiakban látható minta kimenet hello elérési utak egyike:</span><span class="sxs-lookup"><span data-stu-id="31987-142">Following is sample output for one of hello paths:</span></span>

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           64.0.0.1 ffff.eeee.dddd
          0 Microsoft         64.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-microsoft-peering"></a><span data-ttu-id="31987-143">A Microsoft társviszony-létesítéshez ARP-táblázatok</span><span class="sxs-lookup"><span data-stu-id="31987-143">ARP tables for Microsoft peering</span></span>
<span data-ttu-id="31987-144">a következő parancsmag hello táblák biztosít hello ARP a Microsoft társviszony-létesítéshez:</span><span class="sxs-lookup"><span data-stu-id="31987-144">hello following cmdlet provides hello ARP tables for Microsoft peering:</span></span>

    # ARP table for Microsoft peering--primary path
    Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Microsoft -Path Primary

    # ARP table for Microsoft peering--secondary path
    Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Microsoft -Path Secondary


<span data-ttu-id="31987-145">Minta kimenet hello elérési utak közül legalább egy alább találja:</span><span class="sxs-lookup"><span data-stu-id="31987-145">Sample output is shown below for one of hello paths:</span></span>

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc


## <a name="how-toouse-this-information"></a><span data-ttu-id="31987-146">Hogyan toouse ezt az információt</span><span class="sxs-lookup"><span data-stu-id="31987-146">How toouse this information</span></span>
<span data-ttu-id="31987-147">ARP-táblázat társviszony hello használt toovalidate 2. rétegbeli konfiguráció és a kapcsolat lehet.</span><span class="sxs-lookup"><span data-stu-id="31987-147">hello ARP table of a peering can be used toovalidate Layer 2 configuration and connectivity.</span></span> <span data-ttu-id="31987-148">Ez a szakasz áttekintést ARP-táblázatok megjelenését különböző helyzetekben.</span><span class="sxs-lookup"><span data-stu-id="31987-148">This section provides an overview of how ARP tables look in different scenarios.</span></span>

### <a name="arp-table-when-a-circuit-is-in-an-operational-expected-state"></a><span data-ttu-id="31987-149">ARP táblából, ha expressroute-kapcsolatcsoporthoz (várt) működési állapotban van.</span><span class="sxs-lookup"><span data-stu-id="31987-149">ARP table when a circuit is in an operational (expected) state</span></span>
* <span data-ttu-id="31987-150">ARP-táblázat hello bejegyzést tartalmaz, hello helyszíni oldalának egy érvényes IP-cím és MAC-címmel rendelkező, és egy hasonló bejegyzést hello Microsoft oldalán.</span><span class="sxs-lookup"><span data-stu-id="31987-150">hello ARP table has an entry for hello on-premises side with a valid IP and MAC address, and a similar entry for hello Microsoft side.</span></span>
* <span data-ttu-id="31987-151">hello hello a helyi IP-cím utolsó oktettje mindig páratlan szám.</span><span class="sxs-lookup"><span data-stu-id="31987-151">hello last octet of hello on-premises IP address is always an odd number.</span></span>
* <span data-ttu-id="31987-152">Microsoft IP-cím hello hello utolsó oktettje mindig páros szám-e.</span><span class="sxs-lookup"><span data-stu-id="31987-152">hello last octet of hello Microsoft IP address is always an even number.</span></span>
* <span data-ttu-id="31987-153">azonos MAC-cím jelenik meg az összes három esetében (elsődleges és másodlagos) Microsoft ügyféloldali hello hello.</span><span class="sxs-lookup"><span data-stu-id="31987-153">hello same MAC address appears on hello Microsoft side for all three peerings (primary/secondary).</span></span>

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

### <a name="arp-table-when-its-on-premises-or-when-hello-connectivity-provider-side-has-problems"></a><span data-ttu-id="31987-154">Ha a helyszíni, vagy ha hello kapcsolat-szolgáltató ügyféloldali problémák vannak az ARP-táblázat</span><span class="sxs-lookup"><span data-stu-id="31987-154">ARP table when it's on-premises or when hello connectivity-provider side has problems</span></span>
 <span data-ttu-id="31987-155">Csak egy bejegyzés hello ARP-táblázat jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="31987-155">Only one entry appears in hello ARP table.</span></span> <span data-ttu-id="31987-156">Azt illusztrálja, hello leképezése hello MAC és a Microsoft ügyféloldali hello használt hello IP-címet.</span><span class="sxs-lookup"><span data-stu-id="31987-156">It shows hello mapping between hello MAC address and hello IP address that's used on hello Microsoft side.</span></span>

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

> [!NOTE]
> <span data-ttu-id="31987-157">Ehhez hasonló problémát tapasztal, nyissa meg a támogatási kérelem és a kapcsolat szolgáltató tooresolve.</span><span class="sxs-lookup"><span data-stu-id="31987-157">If you experience an issue like this, open a support request with your connectivity provider tooresolve it.</span></span>
> 
> 

### <a name="arp-table-when-hello-microsoft-side-has-problems"></a><span data-ttu-id="31987-158">Ha Microsoft ügyféloldali hello problémák vannak az ARP-táblázat</span><span class="sxs-lookup"><span data-stu-id="31987-158">ARP table when hello Microsoft side has problems</span></span>
* <span data-ttu-id="31987-159">Nem látják az ARP-táblázat társviszony-létesítés Ha problémák vannak a hello Microsoft oldalán látható.</span><span class="sxs-lookup"><span data-stu-id="31987-159">You will not see an ARP table shown for a peering if there are issues on hello Microsoft side.</span></span>
* <span data-ttu-id="31987-160">A támogatási kérést nyithat [Azure a Microsoft Súgó és támogatás](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span><span class="sxs-lookup"><span data-stu-id="31987-160">Open a support request with [Microsoft Azure Help+support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span></span> <span data-ttu-id="31987-161">Adja meg, hogy rendelkezik-e 2. rétegbeli kapcsolatot kapcsolatos problémát.</span><span class="sxs-lookup"><span data-stu-id="31987-161">Specify that you have an issue with Layer 2 connectivity.</span></span>

## <a name="next-steps"></a><span data-ttu-id="31987-162">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="31987-162">Next steps</span></span>
* <span data-ttu-id="31987-163">Ellenőrizze a ExpressRoute-kapcsolatcsoportot 3. rétegbeli konfigurációi:</span><span class="sxs-lookup"><span data-stu-id="31987-163">Validate Layer 3 configurations for your ExpressRoute circuit:</span></span>
  * <span data-ttu-id="31987-164">A BGP-munkamenetek útvonal összefoglaló toodetermine hello állapotának beolvasása.</span><span class="sxs-lookup"><span data-stu-id="31987-164">Get a route summary toodetermine hello state of BGP sessions.</span></span>
  * <span data-ttu-id="31987-165">Egy útvonal tábla toodetermine mely előtagok ExpressRoute között van-e hirdetve beolvasása.</span><span class="sxs-lookup"><span data-stu-id="31987-165">Get a route table toodetermine which prefixes are advertised across ExpressRoute.</span></span>
* <span data-ttu-id="31987-166">Adatátvitel érvényesítése küldött bájtok megtekintésével.</span><span class="sxs-lookup"><span data-stu-id="31987-166">Validate data transfer by reviewing bytes in and out.</span></span>
* <span data-ttu-id="31987-167">A támogatási kérést nyithat [Azure a Microsoft Súgó és támogatás](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) Ha továbbra is problémákat tapasztal.</span><span class="sxs-lookup"><span data-stu-id="31987-167">Open a support request with [Microsoft Azure Help+support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) if you are still experiencing issues.</span></span>

