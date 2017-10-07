---
title: "ARP-tábla beolvasása: erőforrás-kezelő: Azure ExpressRoute-hibaelhárítási |} Microsoft Docs"
description: "Ezen a lapon útmutatás beolvasásakor hello ARP táblák az ExpressRoute-kapcsolatcsoportot"
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
ms.openlocfilehash: c386b031814d40ef6ea3ce5e0eaaab9634470e8f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="getting-arp-tables-in-hello-resource-manager-deployment-model"></a><span data-ttu-id="a1fb9-103">Hello Resource Manager üzembe helyezési modellel tábla ARP beolvasása</span><span class="sxs-lookup"><span data-stu-id="a1fb9-103">Getting ARP tables in hello Resource Manager deployment model</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="a1fb9-104">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="a1fb9-104">PowerShell - Resource Manager</span></span>](expressroute-troubleshooting-arp-resource-manager.md)
> * [<span data-ttu-id="a1fb9-105">PowerShell – Klasszikus</span><span class="sxs-lookup"><span data-stu-id="a1fb9-105">PowerShell - Classic</span></span>](expressroute-troubleshooting-arp-classic.md)
> 
> 

<span data-ttu-id="a1fb9-106">Ez a cikk bemutatja, hogyan hello lépéseket toolearn hello ARP táblák az ExpressRoute-kapcsolatcsoport esetében.</span><span class="sxs-lookup"><span data-stu-id="a1fb9-106">This article walks you through hello steps toolearn hello ARP tables for your ExpressRoute circuit.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="a1fb9-107">Ez a dokumentum tervezett toohelp diagnosztizálhatja és egyszerű problémák megoldásával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="a1fb9-107">This document is intended toohelp you diagnose and fix simple issues.</span></span> <span data-ttu-id="a1fb9-108">Már nem tervezett toobe helyettesíti a Microsoft támogatási szolgálatához.</span><span class="sxs-lookup"><span data-stu-id="a1fb9-108">It is not intended toobe a replacement for Microsoft support.</span></span> <span data-ttu-id="a1fb9-109">Meg kell nyitnia a támogatási jegy [Microsoft támogatási szolgálatához](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) nem toosolve hello problémát az alább ismertetett hello útmutatást esetén.</span><span class="sxs-lookup"><span data-stu-id="a1fb9-109">You must open a support ticket with [Microsoft support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) if you are unable toosolve hello problem using hello guidance described below.</span></span>
> 
> 

## <a name="address-resolution-protocol-arp-and-arp-tables"></a><span data-ttu-id="a1fb9-110">Cím Resolution Protocol (ARP) és a ARP</span><span class="sxs-lookup"><span data-stu-id="a1fb9-110">Address Resolution Protocol (ARP) and ARP tables</span></span>
<span data-ttu-id="a1fb9-111">Address Resolution Protocol (ARP) definiálva a 2. réteg protokoll [RFC 826](https://tools.ietf.org/html/rfc826).</span><span class="sxs-lookup"><span data-stu-id="a1fb9-111">Address Resolution Protocol (ARP) is a layer 2 protocol defined in [RFC 826](https://tools.ietf.org/html/rfc826).</span></span> <span data-ttu-id="a1fb9-112">ARP használt toomap hello Ethernet-címe (MAC-cím) IP-címmel.</span><span class="sxs-lookup"><span data-stu-id="a1fb9-112">ARP is used toomap hello Ethernet address (MAC address) with an ip address.</span></span>

<span data-ttu-id="a1fb9-113">ARP-táblázat hello biztosítja a leképezést hello IPv4-cím és MAC-címet adott társviszony-létesítés.</span><span class="sxs-lookup"><span data-stu-id="a1fb9-113">hello ARP table provides a mapping of hello ipv4 address and MAC address for a particular peering.</span></span> <span data-ttu-id="a1fb9-114">egy ExpressRoute-kapcsolatcsoport társviszonyt az ARP-táblázat hello biztosít hello a következő információkat az egyes csatolókra (elsődleges és másodlagos)</span><span class="sxs-lookup"><span data-stu-id="a1fb9-114">hello ARP table for an ExpressRoute circuit peering provides hello following information for each interface (primary and secondary)</span></span>

1. <span data-ttu-id="a1fb9-115">A helyi útválasztó illesztő ip cím toohello MAC-cím hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="a1fb9-115">Mapping of on-premises router interface ip address toohello MAC address</span></span>
2. <span data-ttu-id="a1fb9-116">Az ExpressRoute útválasztó illesztő IP-cím toohello MAC címe leképezése</span><span class="sxs-lookup"><span data-stu-id="a1fb9-116">Mapping of ExpressRoute router interface ip address toohello MAC address</span></span>
3. <span data-ttu-id="a1fb9-117">Hello leképezési korát</span><span class="sxs-lookup"><span data-stu-id="a1fb9-117">Age of hello mapping</span></span>

<span data-ttu-id="a1fb9-118">ARP-táblázatok érvényesíteni a 2. réteg segítségével, és alapvető hibaelhárítási réteg 2 kapcsolódási problémák.</span><span class="sxs-lookup"><span data-stu-id="a1fb9-118">ARP tables can help validate layer 2 configuration and troubleshooting basic layer 2 connectivity issues.</span></span> 

<span data-ttu-id="a1fb9-119">Példa ARP-táblázat:</span><span class="sxs-lookup"><span data-stu-id="a1fb9-119">Example ARP table:</span></span> 

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1   ffff.eeee.dddd
          0 Microsoft         10.0.0.2   aaaa.bbbb.cccc


<span data-ttu-id="a1fb9-120">hello következő részben megtudhatja hogyan megtekintheti hello szerinti hello ExpressRoute peremhálózati útválasztók ARP-táblázatok.</span><span class="sxs-lookup"><span data-stu-id="a1fb9-120">hello following section provides information on how you can view hello ARP tables seen by hello ExpressRoute edge routers.</span></span> 

## <a name="prerequisites-for-learning-arp-tables"></a><span data-ttu-id="a1fb9-121">ARP-táblázatok tanulási előfeltételei</span><span class="sxs-lookup"><span data-stu-id="a1fb9-121">Prerequisites for learning ARP tables</span></span>
<span data-ttu-id="a1fb9-122">Gondoskodjon arról, hogy további előrehaladás hello követően</span><span class="sxs-lookup"><span data-stu-id="a1fb9-122">Ensure that you have hello following before you progress further</span></span>

* <span data-ttu-id="a1fb9-123">Egy érvényes ExpressRoute-kapcsolatcsoportot legalább egy társviszony-létesítés konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="a1fb9-123">A Valid ExpressRoute circuit configured with at least one peering.</span></span> <span data-ttu-id="a1fb9-124">hello áramkör a hello kapcsolat szolgáltatójánál teljesen kell konfigurálni.</span><span class="sxs-lookup"><span data-stu-id="a1fb9-124">hello circuit must be fully configured by hello connectivity provider.</span></span> <span data-ttu-id="a1fb9-125">Ön (vagy a kapcsolat szolgáltatóját) kell konfigurált hello esetében (Azure saját, az Azure nyilvános és a Microsoft) közül legalább egy ebben a kapcsolatcsoportban.</span><span class="sxs-lookup"><span data-stu-id="a1fb9-125">You (or your connectivity provider) must have configured at least one of hello peerings (Azure private, Azure public and Microsoft) on this circuit.</span></span>
* <span data-ttu-id="a1fb9-126">IP-címtartományok hello esetében (Azure saját, az Azure nyilvános és a Microsoft) konfigurálására használható.</span><span class="sxs-lookup"><span data-stu-id="a1fb9-126">IP address ranges used for configuring hello peerings (Azure private, Azure public and Microsoft).</span></span> <span data-ttu-id="a1fb9-127">Tekintse át a hello ip cím hozzárendelés példák hello [ExpressRoute útválasztási követelmények lapon](expressroute-routing.md) tooget megismerhesse, milyen IP-címek vannak leképezve az ügyféloldali és hello ExpressRoute ügyféloldali toointerfaces.</span><span class="sxs-lookup"><span data-stu-id="a1fb9-127">Review hello ip address assignment examples in hello [ExpressRoute routing requirements page](expressroute-routing.md) tooget an understanding of how ip addresses are mapped toointerfaces on your side and on hello ExpressRoute side.</span></span> <span data-ttu-id="a1fb9-128">Hello társviszony-létesítési konfiguráció tájékoztatást kaphat hello megtekintésével [ExpressRoute-társviszony-létesítési konfiguráció lapon](expressroute-howto-routing-arm.md).</span><span class="sxs-lookup"><span data-stu-id="a1fb9-128">You can get information on hello peering configuration by reviewing hello [ExpressRoute peering configuration page](expressroute-howto-routing-arm.md).</span></span>
* <span data-ttu-id="a1fb9-129">A hálózati csoport adatait / ezek IP-címekkel rendelkező használt hello adapterek MAC-címek a kapcsolat szolgáltatóját.</span><span class="sxs-lookup"><span data-stu-id="a1fb9-129">Information from your networking team / connectivity provider on hello MAC addresses of interfaces used with these IP addresses.</span></span>
* <span data-ttu-id="a1fb9-130">Hello legújabb PowerShell-modult az Azure-ba (1,50 vagy újabb verzió) kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="a1fb9-130">You must have hello latest PowerShell module for Azure (version 1.50 or newer).</span></span>

## <a name="getting-hello-arp-tables-for-your-expressroute-circuit"></a><span data-ttu-id="a1fb9-131">Az ExpressRoute-kapcsolatcsoportot hello ARP tábla beolvasása</span><span class="sxs-lookup"><span data-stu-id="a1fb9-131">Getting hello ARP tables for your ExpressRoute circuit</span></span>
<span data-ttu-id="a1fb9-132">A szakasz ismerteti, hogyan megtekintheti utasításokat hello ARP-táblázatok / társviszony a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="a1fb9-132">This section provides instructions on how you can view hello ARP tables per peering using PowerShell.</span></span> <span data-ttu-id="a1fb9-133">Ön vagy a kapcsolat szolgáltatójánál úgy kell konfigurálnia hello mielőtt elmélyedne a további társviszony-létesítés.</span><span class="sxs-lookup"><span data-stu-id="a1fb9-133">You or your connectivity provider must have configured hello peering before progressing further.</span></span> <span data-ttu-id="a1fb9-134">Minden kapcsolat van két elérési útnak (elsődleges és másodlagos).</span><span class="sxs-lookup"><span data-stu-id="a1fb9-134">Each circuit has two paths (primary and secondary).</span></span> <span data-ttu-id="a1fb9-135">ARP-táblázat az egyes elérési utakat hello egymástól függetlenül ellenőrizheti.</span><span class="sxs-lookup"><span data-stu-id="a1fb9-135">You can check hello ARP table for each path independently.</span></span>

### <a name="arp-tables-for-azure-private-peering"></a><span data-ttu-id="a1fb9-136">Az Azure magánhálózati társviszony-létesítés ARP-táblázatok</span><span class="sxs-lookup"><span data-stu-id="a1fb9-136">ARP tables for Azure private peering</span></span>
<span data-ttu-id="a1fb9-137">a következő parancsmag hello biztosít hello ARP táblák Azure magánhálózati társviszony-létesítés</span><span class="sxs-lookup"><span data-stu-id="a1fb9-137">hello following cmdlet provides hello ARP tables for Azure private peering</span></span>

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"

        # ARP table for Azure private peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePrivatePeering -DevicePath Primary

        # ARP table for Azure private peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePrivatePeering -DevicePath Secondary 

<span data-ttu-id="a1fb9-138">Minta kimenet hello elérési utak közül legalább egy alább láthatók</span><span class="sxs-lookup"><span data-stu-id="a1fb9-138">Sample output is shown below for one of hello paths</span></span>

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1   ffff.eeee.dddd
          0 Microsoft         10.0.0.2   aaaa.bbbb.cccc


### <a name="arp-tables-for-azure-public-peering"></a><span data-ttu-id="a1fb9-139">Az Azure nyilvános társviszony ARP-táblázatok</span><span class="sxs-lookup"><span data-stu-id="a1fb9-139">ARP tables for Azure public peering</span></span>
<span data-ttu-id="a1fb9-140">a következő parancsmag hello biztosít hello ARP táblák az Azure nyilvános társviszony-létesítés</span><span class="sxs-lookup"><span data-stu-id="a1fb9-140">hello following cmdlet provides hello ARP tables for Azure public peering</span></span>

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"

        # ARP table for Azure public peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePublicPeering -DevicePath Primary

        # ARP table for Azure public peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePublicPeering -DevicePath Secondary 


<span data-ttu-id="a1fb9-141">Minta kimenet hello elérési utak közül legalább egy alább láthatók</span><span class="sxs-lookup"><span data-stu-id="a1fb9-141">Sample output is shown below for one of hello paths</span></span>

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           64.0.0.1   ffff.eeee.dddd
          0 Microsoft         64.0.0.2   aaaa.bbbb.cccc


### <a name="arp-tables-for-microsoft-peering"></a><span data-ttu-id="a1fb9-142">A Microsoft társviszony-létesítéshez ARP-táblázatok</span><span class="sxs-lookup"><span data-stu-id="a1fb9-142">ARP tables for Microsoft peering</span></span>
<span data-ttu-id="a1fb9-143">a következő parancsmag hello biztosít hello ARP táblák Microsoft társviszony-létesítés</span><span class="sxs-lookup"><span data-stu-id="a1fb9-143">hello following cmdlet provides hello ARP tables for Microsoft peering</span></span>

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"

        # ARP table for Microsoft peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType MicrosoftPeering -DevicePath Primary

        # ARP table for Microsoft peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType MicrosoftPeering -DevicePath Secondary 


<span data-ttu-id="a1fb9-144">Minta kimenet hello elérési utak közül legalább egy alább láthatók</span><span class="sxs-lookup"><span data-stu-id="a1fb9-144">Sample output is shown below for one of hello paths</span></span>

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1   ffff.eeee.dddd
          0 Microsoft         65.0.0.2   aaaa.bbbb.cccc


## <a name="how-toouse-this-information"></a><span data-ttu-id="a1fb9-145">Hogyan toouse ezt az információt</span><span class="sxs-lookup"><span data-stu-id="a1fb9-145">How toouse this information</span></span>
<span data-ttu-id="a1fb9-146">hello ARP-táblázat társviszony használható toodetermine 2. réteg konfiguráció és a kapcsolat ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="a1fb9-146">hello ARP table of a peering can be used toodetermine validate layer 2 configuration and connectivity.</span></span> <span data-ttu-id="a1fb9-147">Ez a szakasz áttekintést ARP-táblázatok megjelenését a különböző helyzetekben.</span><span class="sxs-lookup"><span data-stu-id="a1fb9-147">This section provides an overview of how ARP tables will look under different scenarios.</span></span>

### <a name="arp-table-when-a-circuit-is-in-operational-state-expected-state"></a><span data-ttu-id="a1fb9-148">ARP-táblázat Ha expressroute-kapcsolatcsoporthoz működési állapot (várt állapot)</span><span class="sxs-lookup"><span data-stu-id="a1fb9-148">ARP table when a circuit is in operational state (expected state)</span></span>
* <span data-ttu-id="a1fb9-149">hello ARP-táblázat egy bejegyzést a hello helyszíni oldalon egy érvényes IP-cím és MAC-cím és egy hasonló bejegyzést hello Microsoft ügyféloldali lesz.</span><span class="sxs-lookup"><span data-stu-id="a1fb9-149">hello ARP table will have an entry for hello on-premises side with a valid IP address and MAC address and a similar entry for hello Microsoft side.</span></span> 
* <span data-ttu-id="a1fb9-150">hello hello a helyi IP-cím utolsó oktettje mindig lesz páratlan szám.</span><span class="sxs-lookup"><span data-stu-id="a1fb9-150">hello last octet of hello on-premises ip address will always be an odd number.</span></span>
* <span data-ttu-id="a1fb9-151">Microsoft IP-cím hello hello utolsó oktettje mindig lesz páros szám.</span><span class="sxs-lookup"><span data-stu-id="a1fb9-151">hello last octet of hello Microsoft ip address will always be an even number.</span></span>
* <span data-ttu-id="a1fb9-152">azonos MAC-cím fog megjelenni minden 3 társviszony (elsődleges / másodlagos) a Microsoft ügyféloldali hello hello.</span><span class="sxs-lookup"><span data-stu-id="a1fb9-152">hello same MAC address will appear on hello Microsoft side for all 3 peerings (primary / secondary).</span></span> 

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1   ffff.eeee.dddd
          0 Microsoft         65.0.0.2   aaaa.bbbb.cccc

### <a name="arp-table-when-on-premises--connectivity-provider-side-has-problems"></a><span data-ttu-id="a1fb9-153">ARP táblából helyszíni / szolgáltató kiszolgálóoldali csatlakozási problémák vannak</span><span class="sxs-lookup"><span data-stu-id="a1fb9-153">ARP table when on-premises / connectivity provider side has problems</span></span>
<span data-ttu-id="a1fb9-154">Ha a helyszíni hello problémák vannak, vagy megjelenik, hogy a kapcsolat szolgáltatójánál láthatja, hogy vagy csak egy bejegyzés megjelenik hello ARP tábla vagy hello helyszíni MAC-cím nem teljes.</span><span class="sxs-lookup"><span data-stu-id="a1fb9-154">If there are issues with hello on-premises or connectivity provider you may see that either only one entry will appear in hello ARP table or hello on-prem MAC address will show incomplete.</span></span> <span data-ttu-id="a1fb9-155">Ez azt mutatja majd hello leképezési hello MAC-cím és a Microsoft ügyféloldali hello használt IP-cím között.</span><span class="sxs-lookup"><span data-stu-id="a1fb9-155">This will show hello mapping between hello MAC address and IP address used in hello Microsoft side.</span></span> 
  
       Age InterfaceProperty IpAddress  MacAddress    
       --- ----------------- ---------  ----------    
         0 Microsoft         65.0.0.2   aaaa.bbbb.cccc

<span data-ttu-id="a1fb9-156">vagy</span><span class="sxs-lookup"><span data-stu-id="a1fb9-156">or</span></span>
       
       Age InterfaceProperty IpAddress  MacAddress    
       --- ----------------- ---------  ----------   
         0 On-Prem           65.0.0.1   Incomplete
         0 Microsoft         65.0.0.2   aaaa.bbbb.cccc


> [!NOTE]
> <span data-ttu-id="a1fb9-157">Nyisson meg egy támogatási kérést a kapcsolat szolgáltató toodebug ezek a problémák.</span><span class="sxs-lookup"><span data-stu-id="a1fb9-157">Open a support request with your connectivity provider toodebug such issues.</span></span> <span data-ttu-id="a1fb9-158">Ha hello ARP-táblázat nem rendelkezik a hello felületek IP-címek hozzárendelt tooMAC címeket, felülvizsgálati hello a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="a1fb9-158">If hello ARP table does not have IP addresses of hello interfaces mapped tooMAC addresses, review hello following information:</span></span>
> 
> 1. <span data-ttu-id="a1fb9-159">Ha hello hivatkozás közötti hozzárendelt első IP-cím hello hello/30-as alhálózat MSEE-PR hello és MSEE MSEE-PR. hello felületén szolgál</span><span class="sxs-lookup"><span data-stu-id="a1fb9-159">If hello first IP address of hello /30 subnet assigned for hello link between hello MSEE-PR and MSEE is used on hello interface of MSEE-PR.</span></span> <span data-ttu-id="a1fb9-160">Azure mindig MSEEs hello második IP-címet használja.</span><span class="sxs-lookup"><span data-stu-id="a1fb9-160">Azure always uses hello second IP address for MSEEs.</span></span>
> 2. <span data-ttu-id="a1fb9-161">Győződjön meg arról, ha hello ügyfél (C-címke) és VLAN-címkék szolgáltatás (S-címke) felel meg a MSEE-PR és MSEE pár mindkét.</span><span class="sxs-lookup"><span data-stu-id="a1fb9-161">Verify if hello customer (C-Tag) and service (S-Tag) VLAN tags match both on MSEE-PR and MSEE pair.</span></span>
> 

### <a name="arp-table-when-microsoft-side-has-problems"></a><span data-ttu-id="a1fb9-162">Ha a Microsoft ügyféloldali problémák vannak az ARP-táblázat</span><span class="sxs-lookup"><span data-stu-id="a1fb9-162">ARP table when Microsoft side has problems</span></span>
* <span data-ttu-id="a1fb9-163">Nem látják az ARP-táblázat társviszony-létesítés Ha problémák vannak a hello Microsoft oldalán látható.</span><span class="sxs-lookup"><span data-stu-id="a1fb9-163">You will not see an ARP table shown for a peering if there are issues on hello Microsoft side.</span></span> 
* <span data-ttu-id="a1fb9-164">A támogatási jegy megnyitása [Microsoft támogatási szolgálatához](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span><span class="sxs-lookup"><span data-stu-id="a1fb9-164">Open a support ticket with [Microsoft support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span></span> <span data-ttu-id="a1fb9-165">Adja meg, hogy rendelkezik-e 2. rétegbeli kapcsolatot kapcsolatos problémát.</span><span class="sxs-lookup"><span data-stu-id="a1fb9-165">Specify that you have an issue with layer 2 connectivity.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="a1fb9-166">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a1fb9-166">Next Steps</span></span>
* <span data-ttu-id="a1fb9-167">Ellenőrizze a ExpressRoute-kapcsolatcsoportot 3. rétegbeli konfigurációi</span><span class="sxs-lookup"><span data-stu-id="a1fb9-167">Validate Layer 3 configurations for your ExpressRoute circuit</span></span>
  * <span data-ttu-id="a1fb9-168">Útvonal összefoglaló toodetermine hello BGP-munkamenetek állapotának beolvasása</span><span class="sxs-lookup"><span data-stu-id="a1fb9-168">Get route summary toodetermine hello state of BGP sessions</span></span> 
  * <span data-ttu-id="a1fb9-169">Útválasztási táblázat toodetermine mely előtagok ExpressRoute között van-e hirdetve beolvasása</span><span class="sxs-lookup"><span data-stu-id="a1fb9-169">Get route table toodetermine which prefixes are advertised across ExpressRoute</span></span>
* <span data-ttu-id="a1fb9-170">Be- / kimeneti bájt megtekintésével adatátvitel ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="a1fb9-170">Validate data transfer by reviewing bytes in / out</span></span>
* <span data-ttu-id="a1fb9-171">A támogatási jegy megnyitása [Microsoft támogatási szolgálatához](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) Ha továbbra is problémákat tapasztal.</span><span class="sxs-lookup"><span data-stu-id="a1fb9-171">Open a support ticket with [Microsoft support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) if you are still experiencing issues.</span></span>

