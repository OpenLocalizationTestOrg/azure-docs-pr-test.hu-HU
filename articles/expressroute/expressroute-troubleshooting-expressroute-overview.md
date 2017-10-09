---
title: "A kapcsolat ellenőrzése: Azure ExpressRoute-hibaelhárítási útmutatója |} Microsoft Docs"
description: "Ezen a lapon útmutatás hibaelhárítási és az ExpressRoute-kapcsolatcsoportot end tooend létesített kapcsolatok ellenőrzése."
documentationcenter: na
services: expressroute
author: rambk
manager: tracsman
editor: 
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/01/2017
ms.author: cherylmc
ms.openlocfilehash: 713c39c7eafd77a4380b2a91902a9686f2ce1d85
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="verifying-expressroute-connectivity"></a><span data-ttu-id="aa61a-103">Az ExpressRoute-kapcsolat ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="aa61a-103">Verifying ExpressRoute connectivity</span></span>
<span data-ttu-id="aa61a-104">ExpressRoute, egy a helyszíni hálózat kibővítve hello Microsoft felhő be, hogy a kapcsolat szolgáltatójánál megkönnyíthető titkos kapcsolaton keresztül, a következő három különböző hálózati zónák hello foglal magában:</span><span class="sxs-lookup"><span data-stu-id="aa61a-104">ExpressRoute, which extends an on-premises network into hello Microsoft cloud over a private connection that is facilitated by a connectivity provider, involves hello following three distinct network zones:</span></span>

-   <span data-ttu-id="aa61a-105">Ügyfél hálózati</span><span class="sxs-lookup"><span data-stu-id="aa61a-105">Customer Network</span></span>
-   <span data-ttu-id="aa61a-106">Szolgáltató</span><span class="sxs-lookup"><span data-stu-id="aa61a-106">Provider Network</span></span>
-   <span data-ttu-id="aa61a-107">A Microsoft Datacenter</span><span class="sxs-lookup"><span data-stu-id="aa61a-107">Microsoft Datacenter</span></span>

<span data-ttu-id="aa61a-108">hello a jelen dokumentum célja toohelp felhasználói tooidentify ahol (vagy ha a) a kapcsolódási problémát létezik, és belül melyik zónát, és ezáltal tooseek segítség a megfelelő csapat tooresolve hello probléma.</span><span class="sxs-lookup"><span data-stu-id="aa61a-108">hello purpose of this document is toohelp user tooidentify where (or even if) a connectivity issue exists and within which zone, thereby tooseek help from appropriate team tooresolve hello issue.</span></span> <span data-ttu-id="aa61a-109">Ha a Microsoft támogatási szolgálatához szükséges tooresolve problémát, a támogatási jegy megnyitása [Microsoft Support][Support].</span><span class="sxs-lookup"><span data-stu-id="aa61a-109">If Microsoft support is needed tooresolve an issue, open a support ticket with [Microsoft Support][Support].</span></span>

> [!IMPORTANT]
> <span data-ttu-id="aa61a-110">Ez a dokumentum tervezett toohelp felderítésére és egyszerű problémák elhárítására.</span><span class="sxs-lookup"><span data-stu-id="aa61a-110">This document is intended toohelp diagnosing and fixing simple issues.</span></span> <span data-ttu-id="aa61a-111">Már nem tervezett toobe helyettesíti a Microsoft támogatási szolgálatához.</span><span class="sxs-lookup"><span data-stu-id="aa61a-111">It is not intended toobe a replacement for Microsoft support.</span></span> <span data-ttu-id="aa61a-112">A támogatási jegy megnyitása [Microsoft Support] [ Support] nem toosolve hello problémát hello útmutatást esetén.</span><span class="sxs-lookup"><span data-stu-id="aa61a-112">Open a support ticket with [Microsoft Support][Support] if you are unable toosolve hello problem using hello guidance provided.</span></span>
>
>

## <a name="overview"></a><span data-ttu-id="aa61a-113">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="aa61a-113">Overview</span></span>
<span data-ttu-id="aa61a-114">hello alábbi ábrán látható hello logikai kapcsolat ExpressRoute használó ügyfél hálózati tooMicrosoft hálózat.</span><span class="sxs-lookup"><span data-stu-id="aa61a-114">hello following diagram shows hello logical connectivity of a customer network tooMicrosoft network using ExpressRoute.</span></span>
<span data-ttu-id="aa61a-115">[![1]][1]</span><span class="sxs-lookup"><span data-stu-id="aa61a-115">[![1]][1]</span></span>

<span data-ttu-id="aa61a-116">Diagram megelőző hello hello számok kulcs hálózati pontokat meg.</span><span class="sxs-lookup"><span data-stu-id="aa61a-116">In hello preceding diagram, hello numbers indicate key network points.</span></span> <span data-ttu-id="aa61a-117">hello hálózati pontokat gyakran ez a cikk keresztül által hivatkozott a hozzájuk társított számokat.</span><span class="sxs-lookup"><span data-stu-id="aa61a-117">hello network points are referenced often through this article by their associated number.</span></span>

<span data-ttu-id="aa61a-118">Attól függően, hogy hello ExpressRoute kapcsolat modell (felhőalapú Exchange történő együttes elhelyezés, Point-to-Point protokoll Ethernet-kapcsolat vagy bármely elem közöttiként (IPVPN)) hello hálózati 3. és 4 is lehetnek kapcsolók (2. rétegbeli eszközök).</span><span class="sxs-lookup"><span data-stu-id="aa61a-118">Depending on hello ExpressRoute connectivity model (Cloud Exchange Co-location, Point-to-Point Ethernet Connection, or Any-to-any (IPVPN)) hello network points 3 and 4 may be switches (Layer 2 devices).</span></span> <span data-ttu-id="aa61a-119">hello kulcs hálózati pontok mutatja a következők:</span><span class="sxs-lookup"><span data-stu-id="aa61a-119">hello key network points illustrated are as follows:</span></span>

1.  <span data-ttu-id="aa61a-120">Ügyfél számítási eszközt (például kiszolgáló vagy számítógép)</span><span class="sxs-lookup"><span data-stu-id="aa61a-120">Customer compute device (for example, a server or PC)</span></span>
2.  <span data-ttu-id="aa61a-121">Tanúsítványigénylési webszolgáltatás: Ügyfél peremhálózati útválasztók</span><span class="sxs-lookup"><span data-stu-id="aa61a-121">CEs: Customer edge routers</span></span> 
3.  <span data-ttu-id="aa61a-122">PEs (CE hozzáférhető): szolgáltató peremhálózati útválasztók/kapcsolók, amelyek az ügyfél peremhálózati útválasztók használata során szembesülnek.</span><span class="sxs-lookup"><span data-stu-id="aa61a-122">PEs (CE facing): Provider edge routers/switches that are facing customer edge routers.</span></span> <span data-ttu-id="aa61a-123">Tooas PE-CEs ebben a dokumentumban említett.</span><span class="sxs-lookup"><span data-stu-id="aa61a-123">Referred tooas PE-CEs in this document.</span></span>
4.  <span data-ttu-id="aa61a-124">PEs (MSEE hozzáférhető): szolgáltató peremhálózati útválasztók/kapcsolók használata során szembesülnek MSEEs.</span><span class="sxs-lookup"><span data-stu-id="aa61a-124">PEs (MSEE facing): Provider edge routers/switches that are facing MSEEs.</span></span> <span data-ttu-id="aa61a-125">Tooas PE-MSEEs ebben a dokumentumban említett.</span><span class="sxs-lookup"><span data-stu-id="aa61a-125">Referred tooas PE-MSEEs in this document.</span></span>
5.  <span data-ttu-id="aa61a-126">MSEEs: A Microsoft vállalati peremhálózati (MSEE) ExpressRoute útválasztók</span><span class="sxs-lookup"><span data-stu-id="aa61a-126">MSEEs: Microsoft Enterprise Edge (MSEE) ExpressRoute routers</span></span>
6.  <span data-ttu-id="aa61a-127">(VNet) virtuális hálózati átjáró</span><span class="sxs-lookup"><span data-stu-id="aa61a-127">Virtual Network (VNet) Gateway</span></span>
7.  <span data-ttu-id="aa61a-128">Számítási hello Azure VNet-eszközön</span><span class="sxs-lookup"><span data-stu-id="aa61a-128">Compute device on hello Azure VNet</span></span>

<span data-ttu-id="aa61a-129">Hello felhőalapú Exchange közös elhelyezés vagy Point-to-Point protokoll Ethernet-kapcsolat modellek használata esetén hello ügyfél peremhálózati útválasztója (2) a BGP társviszony-létesítés (5) MSEEs hozzák létre.</span><span class="sxs-lookup"><span data-stu-id="aa61a-129">If hello Cloud Exchange Co-location or Point-to-Point Ethernet Connection connectivity models are used, hello customer edge router (2) would establish BGP peering with MSEEs (5).</span></span> <span data-ttu-id="aa61a-130">3. és 4 hálózati pontok volna továbbra is létezik, de lehet némileg átlátszó 2. rétegbeli eszközök.</span><span class="sxs-lookup"><span data-stu-id="aa61a-130">Network points 3 and 4 would still exist but be somewhat transparent as Layer 2 devices.</span></span>

<span data-ttu-id="aa61a-131">Hello bármely elem közöttiként (IPVPN) kapcsolat modellt használja, ha hello PEs (MSEE hozzáférhető) (4) hozzák létre a BGP társviszony-létesítés MSEEs (5).</span><span class="sxs-lookup"><span data-stu-id="aa61a-131">If hello Any-to-any (IPVPN) connectivity model is used, hello PEs (MSEE facing) (4) would establish BGP peering with MSEEs (5).</span></span> <span data-ttu-id="aa61a-132">Útvonalak majd továbbítja hátsó toohello ügyfél hálózati hello IPVPN szolgáltatás szolgáltató hálózaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="aa61a-132">Routes would then propagate back toohello customer network via hello IPVPN service provider network.</span></span>

>[!NOTE]
><span data-ttu-id="aa61a-133">Microsoft ExpressRoute magas rendelkezésre állás, a BGP-munkamenetek között MSEEs (5) és a PE-MSEEs (4) a redundáns pár van szükség.</span><span class="sxs-lookup"><span data-stu-id="aa61a-133">For ExpressRoute high availability, Microsoft requires a redundant pair of BGP sessions between MSEEs (5) and PE-MSEEs (4).</span></span> <span data-ttu-id="aa61a-134">Hálózati elérési út redundáns két szintén javasolt felhasználói hálózat és a PE-CEs között.</span><span class="sxs-lookup"><span data-stu-id="aa61a-134">A redundant pair of network paths is also encouraged between customer network and PE-CEs.</span></span> <span data-ttu-id="aa61a-135">Bármely elem közöttiként (IPVPN) kapcsolat modellben, azonban egyetlen CE eszközt (2) lehet csatlakoztatott tooone vagy további PEs (3).</span><span class="sxs-lookup"><span data-stu-id="aa61a-135">However, in Any-to-any (IPVPN) connection model, a single CE device (2) may be connected tooone or more PEs (3).</span></span>
>
>

<span data-ttu-id="aa61a-136">egy ExpressRoute-kapcsolatcsoportot, a következő lépéseket hello toovalidate (a hello hálózati pont kapcsolódó hello számot jelzi) tartoznak:</span><span class="sxs-lookup"><span data-stu-id="aa61a-136">toovalidate an ExpressRoute circuit, hello following steps are covered (with hello network point indicated by hello associated number):</span></span>
1. [<span data-ttu-id="aa61a-137">Ellenőrizze a kiépítésével és állapota (5)</span><span class="sxs-lookup"><span data-stu-id="aa61a-137">Validate circuit provisioning and state (5)</span></span>](#validate-circuit-provisioning-and-state)
2. [<span data-ttu-id="aa61a-138">Ellenőrizze a legalább egy ExpressRoute-társviszony létesítése – konfigurálva (5)</span><span class="sxs-lookup"><span data-stu-id="aa61a-138">Validate at least one ExpressRoute peering is configured (5)</span></span>](#validate-peering-configuration)
3. [<span data-ttu-id="aa61a-139">ARP ellenőrzése a Microsoft és hello szolgáltató (4. és 5 közötti kapcsolat) között</span><span class="sxs-lookup"><span data-stu-id="aa61a-139">Validate ARP between Microsoft and hello service provider (link between 4 and 5)</span></span>](#validate-arp-between-microsoft-and-the-service-provider)
4. [<span data-ttu-id="aa61a-140">A BGP és útvonalak hello MSEE (BGP-4 too5, és 5 too6, ha csatlakozik egy VNet között) a ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="aa61a-140">Validate BGP and routes on hello MSEE (BGP between 4 too5, and 5 too6 if a VNet is connected)</span></span>](#validate-bgp-and-routes-on-the-msee)
5. [<span data-ttu-id="aa61a-141">Ellenőrizze a hello forgalom statisztika (5 áthaladó forgalom)</span><span class="sxs-lookup"><span data-stu-id="aa61a-141">Check hello Traffic Statistics (Traffic passing through 5)</span></span>](#check-the-traffic-statistics)

<span data-ttu-id="aa61a-142">További ellenőrzések és ellenőrzések hozzáadódnak hello jövőbeli, próbálkozzon újra a havi!</span><span class="sxs-lookup"><span data-stu-id="aa61a-142">More validations and checks will be added in hello future, check back monthly!</span></span>

##<a name="validate-circuit-provisioning-and-state"></a><span data-ttu-id="aa61a-143">Kiépítésével és állapotának ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="aa61a-143">Validate circuit provisioning and state</span></span>
<span data-ttu-id="aa61a-144">Hello kapcsolat modell függetlenül ExpressRoute-kapcsolatcsoportot rendelkezik létrehozott toobe, és így a szolgáltatás hozza létre a kiépítésével kulcsot.</span><span class="sxs-lookup"><span data-stu-id="aa61a-144">Regardless of hello connectivity model, an ExpressRoute circuit has toobe created and thus a service key generated for circuit provisioning.</span></span> <span data-ttu-id="aa61a-145">A redundáns 2. rétegbeli kapcsolatot PE-MSEEs (4) és MSEEs (5) között ExpressRoute-kapcsolatcsoportot kiépítés hoz létre.</span><span class="sxs-lookup"><span data-stu-id="aa61a-145">Provisioning an ExpressRoute circuit establishes a redundant Layer 2 connections between PE-MSEEs (4) and MSEEs (5).</span></span> <span data-ttu-id="aa61a-146">Hogyan toocreate, módosítás, kiépíteni, és ellenőrizze a ExpressRoute-kapcsolatcsoportot további információkért lásd: hello cikk [létrehozása és módosítása az ExpressRoute-kapcsolatcsoportot][CreateCircuit].</span><span class="sxs-lookup"><span data-stu-id="aa61a-146">For more information on how toocreate, modify, provision, and verify an ExpressRoute circuit, see hello article [Create and modify an ExpressRoute circuit][CreateCircuit].</span></span>

>[!TIP]
><span data-ttu-id="aa61a-147">A szolgáltatás kulcs egyedileg azonosítja az ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="aa61a-147">A service key uniquely identifies an ExpressRoute circuit.</span></span> <span data-ttu-id="aa61a-148">A kulcs mindenképpen szükséges a legtöbb ebben a dokumentumban említett hello powershell-parancsokat.</span><span class="sxs-lookup"><span data-stu-id="aa61a-148">This key is required for most of hello powershell commands mentioned in this document.</span></span> <span data-ttu-id="aa61a-149">Emellett kell segítségre van szüksége Microsoft és az ExpressRoute-partner tootroubleshoot ExpressRoute problémát, adja meg a hello szolgáltatás fő tooreadily hello áramkör azonosításához.</span><span class="sxs-lookup"><span data-stu-id="aa61a-149">Also, should you need assistance from Microsoft or from an ExpressRoute partner tootroubleshoot an ExpressRoute issue, provide hello service key tooreadily identify hello circuit.</span></span>
>
>

###<a name="verification-via-hello-azure-portal"></a><span data-ttu-id="aa61a-150">Ellenőrzési hello Azure-portálon keresztül</span><span class="sxs-lookup"><span data-stu-id="aa61a-150">Verification via hello Azure portal</span></span>
<span data-ttu-id="aa61a-151">Hello Azure-portálon, az ExpressRoute-kapcsolatcsoportot hello állapotának ellenőrizhetők kiválasztásával ![2][2] hello a bal oldali oldalsó sáv menüre, és jelölje be hello ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="aa61a-151">In hello Azure portal, hello status of an ExpressRoute circuit can be checked by selecting ![2][2] on hello left-side-bar menu and then selecting hello ExpressRoute circuit.</span></span> <span data-ttu-id="aa61a-152">Egy ExpressRoute kiválasztása áramkör "Összes erőforrás" alatt szereplő hello ExpressRoute-kapcsolatcsoport panel megnyitása.</span><span class="sxs-lookup"><span data-stu-id="aa61a-152">Selecting an ExpressRoute circuit listed under "All resources" opens hello ExpressRoute circuit blade.</span></span> <span data-ttu-id="aa61a-153">A hello ![3][3] hello panelen hello essentials találhatók, ahogy az alábbi képernyőfelvétel a hello ExpressRoute szakaszában:</span><span class="sxs-lookup"><span data-stu-id="aa61a-153">In hello ![3][3] section of hello blade, hello ExpressRoute essentials are listed as shown in hello following screen shot:</span></span>

<span data-ttu-id="aa61a-154">![4][4]</span><span class="sxs-lookup"><span data-stu-id="aa61a-154">![4][4]</span></span>    

<span data-ttu-id="aa61a-155">Az ExpressRoute Essentials hello *állapot áramkör* hello kapcsolatnak az ügyféloldali Microsoft hello hello állapotát jelzi.</span><span class="sxs-lookup"><span data-stu-id="aa61a-155">In hello ExpressRoute Essentials, *Circuit status* indicates hello status of hello circuit on hello Microsoft side.</span></span> <span data-ttu-id="aa61a-156">*Szolgáltató állapota* azt jelzi, hogy lett-e hello áramkör *kiépítve/nem létesített* hello szolgáltatói oldalán.</span><span class="sxs-lookup"><span data-stu-id="aa61a-156">*Provider status* indicates if hello circuit has been *Provisioned/Not provisioned* on hello service-provider side.</span></span> 

<span data-ttu-id="aa61a-157">Toobe működési egy ExpressRoute körön, hello *állapot áramkör* kell *engedélyezve* és hello *szolgáltató állapota* kell *kiépítve*.</span><span class="sxs-lookup"><span data-stu-id="aa61a-157">For an ExpressRoute circuit toobe operational, hello *Circuit status* must be *Enabled* and hello *Provider status* must be *Provisioned*.</span></span>

>[!NOTE]
><span data-ttu-id="aa61a-158">Ha hello *állapot áramkör* van nincs engedélyezve, forduljon a [Microsoft Support][Support].</span><span class="sxs-lookup"><span data-stu-id="aa61a-158">If hello *Circuit status* is not enabled, contact [Microsoft Support][Support].</span></span> <span data-ttu-id="aa61a-159">Ha hello *szolgáltató állapota* van nincs telepítve, forduljon a szolgáltatójához.</span><span class="sxs-lookup"><span data-stu-id="aa61a-159">If hello *Provider status* is not provisioned, contact your service provider.</span></span>
>
>

###<a name="verification-via-powershell"></a><span data-ttu-id="aa61a-160">PowerShell ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="aa61a-160">Verification via PowerShell</span></span>
<span data-ttu-id="aa61a-161">toolist összes hello ExpressRoute-Kapcsolatcsoportok erőforráscsoportban, a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="aa61a-161">toolist all hello ExpressRoute circuits in a Resource Group, use hello following command:</span></span>

    Get-AzureRmExpressRouteCircuit -ResourceGroupName "Test-ER-RG"

>[!TIP]
><span data-ttu-id="aa61a-162">Az erőforráscsoport neve hello Azure-portálon keresztül érheti el.</span><span class="sxs-lookup"><span data-stu-id="aa61a-162">You can get your resource group name through hello Azure portal.</span></span> <span data-ttu-id="aa61a-163">Lásd az hello előző ebben a dokumentumban, és vegye figyelembe, hogy hello erőforráscsoport-név szerepel hello példa képernyőképet.</span><span class="sxs-lookup"><span data-stu-id="aa61a-163">See hello previous subsection of this document and note that hello resource group name is listed in hello example screen shot.</span></span>
>
>

<span data-ttu-id="aa61a-164">egy adott ExpressRoute-kapcsolatcsoportot erőforráscsoportban, a következő parancs használata hello tooselect:</span><span class="sxs-lookup"><span data-stu-id="aa61a-164">tooselect a particular ExpressRoute circuit in a Resource Group, use hello following command:</span></span>

    Get-AzureRmExpressRouteCircuit -ResourceGroupName "Test-ER-RG" -Name "Test-ER-Ckt"

<span data-ttu-id="aa61a-165">A rendszer egy minta választ:</span><span class="sxs-lookup"><span data-stu-id="aa61a-165">A sample response is:</span></span>

    Name                             : Test-ER-Ckt
    ResourceGroupName                : Test-ER-RG
    Location                         : westus2
    Id                               : /subscriptions/***************************/resourceGroups/Test-ER-RG/providers/***********/expressRouteCircuits/Test-ER-Ckt
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                        "Name": "Standard_UnlimitedData",
                                        "Tier": "Standard",
                                        "Family": "UnlimitedData"
                                        }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned
    ServiceProviderNotes             : 
    ServiceProviderProperties        : {
                                        "ServiceProviderName": "****",
                                        "PeeringLocation": "******",
                                        "BandwidthInMbps": 100
                                        }
    ServiceKey                       : **************************************
    Peerings                         : []
    Authorizations                   : []

<span data-ttu-id="aa61a-166">tooconfirm ExpressRoute-kapcsolatcsoportot működik, ha fordítson különös figyelmet toohello a következő mezőket:</span><span class="sxs-lookup"><span data-stu-id="aa61a-166">tooconfirm if an ExpressRoute circuit is operational, pay particular attention toohello following fields:</span></span>

    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned

>[!NOTE]
><span data-ttu-id="aa61a-167">Ha hello *CircuitProvisioningState* van nincs engedélyezve, forduljon a [Microsoft Support][Support].</span><span class="sxs-lookup"><span data-stu-id="aa61a-167">If hello *CircuitProvisioningState* is not enabled, contact [Microsoft Support][Support].</span></span> <span data-ttu-id="aa61a-168">Ha hello *ServiceProviderProvisioningState* van nincs telepítve, forduljon a szolgáltatójához.</span><span class="sxs-lookup"><span data-stu-id="aa61a-168">If hello *ServiceProviderProvisioningState* is not provisioned, contact your service provider.</span></span>
>
>

###<a name="verification-via-powershell-classic"></a><span data-ttu-id="aa61a-169">Ellenőrzési PowerShell (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="aa61a-169">Verification via PowerShell (Classic)</span></span>
<span data-ttu-id="aa61a-170">toolist összes hello ExpressRoute-Kapcsolatcsoportok adott előfizetésen belül, a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="aa61a-170">toolist all hello ExpressRoute circuits under a subscription, use hello following command:</span></span>

    Get-AzureDedicatedCircuit

<span data-ttu-id="aa61a-171">egy adott ExpressRoute-kapcsolatcsoportot tooselect hello a következő parancsot használja:</span><span class="sxs-lookup"><span data-stu-id="aa61a-171">tooselect a particular ExpressRoute circuit, use hello following command:</span></span>

    Get-AzureDedicatedCircuit -ServiceKey **************************************

<span data-ttu-id="aa61a-172">A rendszer egy minta választ:</span><span class="sxs-lookup"><span data-stu-id="aa61a-172">A sample response is:</span></span>

    andwidth                         : 100
    BillingType                      : UnlimitedData
    CircuitName                      : Test-ER-Ckt
    Location                         : westus2
    ServiceKey                       : **************************************
    ServiceProviderName              : ****
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

<span data-ttu-id="aa61a-173">tooconfirm ExpressRoute-kapcsolatcsoportot működik, ha nagy a következő mezők különös figyelmet toohello: ServiceProviderProvisioningState: kiépített állapot: engedélyezve</span><span class="sxs-lookup"><span data-stu-id="aa61a-173">tooconfirm if an ExpressRoute circuit is operational, pay particular attention toohello following fields: ServiceProviderProvisioningState : Provisioned Status                           : Enabled</span></span>

>[!NOTE]
><span data-ttu-id="aa61a-174">Ha hello *állapot* van nincs engedélyezve, forduljon a [Microsoft Support][Support].</span><span class="sxs-lookup"><span data-stu-id="aa61a-174">If hello *Status* is not enabled, contact [Microsoft Support][Support].</span></span> <span data-ttu-id="aa61a-175">Ha hello *ServiceProviderProvisioningState* van nincs telepítve, forduljon a szolgáltatójához.</span><span class="sxs-lookup"><span data-stu-id="aa61a-175">If hello *ServiceProviderProvisioningState* is not provisioned, contact your service provider.</span></span>
>
>

##<a name="validate-peering-configuration"></a><span data-ttu-id="aa61a-176">Társviszony-létesítési konfiguráció ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="aa61a-176">Validate Peering Configuration</span></span>
<span data-ttu-id="aa61a-177">Hello szolgáltatónak befejezett hello hello ExpressRoute-kapcsolatcsoportot kiépítés, miután egy útválasztási konfigurációja hello ExpressRoute-kapcsolatcsoport között MSEE-PRs (4) és MSEEs (5) keresztül hozhatók létre.</span><span class="sxs-lookup"><span data-stu-id="aa61a-177">After hello service provider has completed hello provisioning hello ExpressRoute circuit, a routing configuration can be created over hello ExpressRoute circuit between MSEE-PRs (4) and MSEEs (5).</span></span> <span data-ttu-id="aa61a-178">Minden egyes ExpressRoute-kapcsolatcsoportot rendelkezhet egy, kettő vagy három útválasztási környezetek engedélyezve: Azure magánhálózati társviszony-létesítés (forgalom tooprivate virtuális hálózatok az Azure-ban), az Azure nyilvános társviszony-létesítés (forgalom toopublic IP-címek az Azure-ban) és a Microsoft társviszony-létesítés (forgalom tooOffice 365 és Dynamics 365).</span><span class="sxs-lookup"><span data-stu-id="aa61a-178">Each ExpressRoute circuit can have one, two, or three routing contexts enabled: Azure private peering (traffic tooprivate virtual networks in Azure), Azure public peering (traffic toopublic IP addresses in Azure), and Microsoft peering (traffic tooOffice 365 and Dynamics 365).</span></span> <span data-ttu-id="aa61a-179">További információt a toocreate és útválasztási konfigurációjának módosításához hello cikke [létrehozása és módosítása az ExpressRoute-kapcsolatcsoportot útválasztási][CreatePeering].</span><span class="sxs-lookup"><span data-stu-id="aa61a-179">For more information on how toocreate and modify routing configuration, see hello article [Create and modify routing for an ExpressRoute circuit][CreatePeering].</span></span>

###<a name="verification-via-hello-azure-portal"></a><span data-ttu-id="aa61a-180">Ellenőrzési hello Azure-portálon keresztül</span><span class="sxs-lookup"><span data-stu-id="aa61a-180">Verification via hello Azure portal</span></span>
>[!IMPORTANT]
><span data-ttu-id="aa61a-181">Egy ismert hiba van hello Azure portál, amelynek ExpressRoute-társviszony vannak *nem* hello szolgáltató konfiguráltságát hello portálon is látható.</span><span class="sxs-lookup"><span data-stu-id="aa61a-181">There is a known bug in hello Azure portal wherein ExpressRoute peerings are *NOT* shown in hello portal if configured by hello service provider.</span></span> <span data-ttu-id="aa61a-182">ExpressRoute-társviszony hello portál vagy PowerShell hozzáadása *hello szolgáltatás Szolgáltatóbeállítások felülírja*.</span><span class="sxs-lookup"><span data-stu-id="aa61a-182">Adding ExpressRoute peerings via hello portal or PowerShell *overwrites hello service provider settings*.</span></span> <span data-ttu-id="aa61a-183">Ez a művelet megszakítja a hello hello ExpressRoute-kapcsolatcsoportot az Útválasztás és hello szolgáltatás szolgáltató toorestore hello beállítások hello támogatnia kell a, és helyreállítani a normál útválasztást.</span><span class="sxs-lookup"><span data-stu-id="aa61a-183">This action breaks hello routing on hello ExpressRoute circuit and requires hello support of hello service provider toorestore hello settings and reestablish normal routing.</span></span> <span data-ttu-id="aa61a-184">Hello ExpressRoute-társviszony csak akkor módosítsa, ha biztos, hogy hello szolgáltató biztosítja a csak a 2. réteg szolgáltatásokat!</span><span class="sxs-lookup"><span data-stu-id="aa61a-184">Only modify hello ExpressRoute peerings if it is certain that hello service provider is providing layer 2 services only!</span></span>
>
>

<p/>
>[!NOTE]
><span data-ttu-id="aa61a-185">Ha 3 rétegbeli feltéve, hogy által hello szolgáltatás szolgáltató és hello esetében üres hello portálon, a PowerShell lehet használt toosee hello szolgáltatást konfigurált szolgáltató beállításai.</span><span class="sxs-lookup"><span data-stu-id="aa61a-185">If layer 3 is provided by hello service provider and hello peerings are blank in hello portal, PowerShell can be used toosee hello service provider configured settings.</span></span>
>
>

<span data-ttu-id="aa61a-186">Hello Azure-portálon, az ExpressRoute-kapcsolatcsoportot állapotának ellenőrizhetők kiválasztásával ![2][2] hello a bal oldali oldalsó sáv menüre, és jelölje be hello ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="aa61a-186">In hello Azure portal, status of an ExpressRoute circuit can be checked by selecting ![2][2] on hello left-side-bar menu and then selecting hello ExpressRoute circuit.</span></span> <span data-ttu-id="aa61a-187">Egy ExpressRoute kiválasztása "Összes erőforrás" alatt szereplő áramkör nyitna hello ExpressRoute-kapcsolatcsoport panelen.</span><span class="sxs-lookup"><span data-stu-id="aa61a-187">Selecting an ExpressRoute circuit listed under "All resources" would open hello ExpressRoute circuit blade.</span></span> <span data-ttu-id="aa61a-188">A hello ![3][3] hello panelen hello essentials kellene szerepelnie, ahogy az alábbi képernyőfelvétel a hello ExpressRoute szakaszában:</span><span class="sxs-lookup"><span data-stu-id="aa61a-188">In hello ![3][3] section of hello blade, hello ExpressRoute essentials would be listed as shown in hello following screen shot:</span></span>

<span data-ttu-id="aa61a-189">![5][5]</span><span class="sxs-lookup"><span data-stu-id="aa61a-189">![5][5]</span></span>

<span data-ttu-id="aa61a-190">A fenti példa hello mint beállításértékeket Azure magánhálózati társviszony-létesítési útválasztási környezet engedélyezve van, mivel nyilvános Azure és a Microsoft társviszony-létesítési útválasztási környezetek nem engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="aa61a-190">In hello preceding example, as noted Azure private peering routing context is enabled, whereas Azure public and Microsoft peering routing contexts are not enabled.</span></span> <span data-ttu-id="aa61a-191">Sikeresen engedélyezve a társviszony-létesítési környezetben is kellene hello elsődleges és másodlagos Point-to-Point protokoll (BGP szükséges) alhálózat szerepel.</span><span class="sxs-lookup"><span data-stu-id="aa61a-191">A successfully enabled peering context would also have hello primary and secondary point-to-point (required for BGP) subnets listed.</span></span> <span data-ttu-id="aa61a-192">hello/30-as alhálózat hello MSEEs hello illesztő IP-címét és a PE-MSEEs használ.</span><span class="sxs-lookup"><span data-stu-id="aa61a-192">hello /30 subnets are used for hello interface IP address of hello MSEEs and PE-MSEEs.</span></span> 

>[!NOTE]
><span data-ttu-id="aa61a-193">Társviszony-létesítés nem engedélyezett, ha ellenőrizze, hogy egyeznek-e hozzárendelve hello elsődleges és másodlagos alhálózat PE-MSEEs hello konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="aa61a-193">If a peering is not enabled, check if hello primary and secondary subnets assigned match hello configuration on PE-MSEEs.</span></span> <span data-ttu-id="aa61a-194">Ha nem, toochange hello konfigurációs MSEE útválasztók, tekintse meg a túl[létrehozása és módosítása az ExpressRoute-kapcsolatcsoportot útválasztási][CreatePeering]</span><span class="sxs-lookup"><span data-stu-id="aa61a-194">If not, toochange hello configuration on MSEE routers, refer too[Create and modify routing for an ExpressRoute circuit][CreatePeering]</span></span>
>
>

###<a name="verification-via-powershell"></a><span data-ttu-id="aa61a-195">PowerShell ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="aa61a-195">Verification via PowerShell</span></span>
<span data-ttu-id="aa61a-196">tooget hello Azure titkos társviszony konfigurációs részletek, használja a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="aa61a-196">tooget hello Azure private peering configuration details, use hello following commands:</span></span>

    $ckt = Get-AzureRmExpressRouteCircuit -ResourceGroupName "Test-ER-RG" -Name "Test-ER-Ckt"
    Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -Circuit $ckt

<span data-ttu-id="aa61a-197">A minta választ, egy sikeresen konfigurált magánhálózati társviszony-létesítés, a következő:</span><span class="sxs-lookup"><span data-stu-id="aa61a-197">A sample response, for a successfully configured private peering, is:</span></span>

    Name                       : AzurePrivatePeering
    Id                         : /subscriptions/***************************/resourceGroups/Test-ER-RG/providers/***********/expressRouteCircuits/Test-ER-Ckt/peerings/AzurePrivatePeering
    Etag                       : W/"################################"
    PeeringType                : AzurePrivatePeering
    AzureASN                   : 12076
    PeerASN                    : ####
    PrimaryPeerAddressPrefix   : 172.16.0.0/30
    SecondaryPeerAddressPrefix : 172.16.0.4/30
    PrimaryAzurePort           : 
    SecondaryAzurePort         : 
    SharedKey                  : 
    VlanId                     : 300
    MicrosoftPeeringConfig     : null
    ProvisioningState          : Succeeded

 <span data-ttu-id="aa61a-198">Sikeresen engedélyezve a társviszony-létesítési környezetben hello elsődleges és másodlagos címelőtagokat felsorolt kellene lennie.</span><span class="sxs-lookup"><span data-stu-id="aa61a-198">A successfully enabled peering context would have hello primary and secondary address prefixes listed.</span></span> <span data-ttu-id="aa61a-199">hello/30-as alhálózat hello MSEEs hello illesztő IP-címét és a PE-MSEEs használ.</span><span class="sxs-lookup"><span data-stu-id="aa61a-199">hello /30 subnets are used for hello interface IP address of hello MSEEs and PE-MSEEs.</span></span>

<span data-ttu-id="aa61a-200">tooget hello Azure nyilvános társviszony konfigurációs részletek, használja a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="aa61a-200">tooget hello Azure public peering configuration details, use hello following commands:</span></span>

    $ckt = Get-AzureRmExpressRouteCircuit -ResourceGroupName "Test-ER-RG" -Name "Test-ER-Ckt"
    Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -Circuit $ckt

<span data-ttu-id="aa61a-201">tooget hello Microsoft társviszony-létesítési konfiguráció részletei, a következő parancsok hello használata:</span><span class="sxs-lookup"><span data-stu-id="aa61a-201">tooget hello Microsoft peering configuration details, use hello following commands:</span></span>

    $ckt = Get-AzureRmExpressRouteCircuit -ResourceGroupName "Test-ER-RG" -Name "Test-ER-Ckt"
    Get-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -Circuit $ckt

<span data-ttu-id="aa61a-202">Ha nincs konfigurálva a társviszony-létesítés, nem lenne egy hibaüzenetet.</span><span class="sxs-lookup"><span data-stu-id="aa61a-202">If a peering is not configured, there would be an error message.</span></span> <span data-ttu-id="aa61a-203">Mintát választ, ha hello közölt társviszony-létesítés (Azure nyilvános társviszony-létesítés ebben a példában) belül hello körhöz nincs konfigurálva:</span><span class="sxs-lookup"><span data-stu-id="aa61a-203">A sample response, when hello stated peering (Azure Public peering in this example) is not configured within hello circuit:</span></span>

    Get-AzureRmExpressRouteCircuitPeeringConfig : Sequence contains no matching element
    At line:1 char:1
        + Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering ...
        + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
            + CategoryInfo          : CloseError: (:) [Get-AzureRmExpr...itPeeringConfig], InvalidOperationException
            + FullyQualifiedErrorId : Microsoft.Azure.Commands.Network.GetAzureExpressRouteCircuitPeeringConfigCommand


<p/>
>[!NOTE]
><span data-ttu-id="aa61a-204">Ha nincs engedélyezve a társviszony-létesítés, ellenőrizze, ha hello hozzárendelt elsődleges és másodlagos alhálózat egyezés hello konfigurációs hello csatolt PE-MSEE.</span><span class="sxs-lookup"><span data-stu-id="aa61a-204">If a peering is not enabled, check if hello primary and secondary subnets assigned match hello configuration on hello linked PE-MSEE.</span></span> <span data-ttu-id="aa61a-205">Ellenőrizze, hogy hello kijavíthatja is *VlanId*, *AzureASN*, és *PeerASN* MSEEs használja, és ha ezeket az értékeket leképezi a hello lévőktől toohello csatolt PE-MSEE.</span><span class="sxs-lookup"><span data-stu-id="aa61a-205">Also check if hello correct *VlanId*, *AzureASN*, and *PeerASN* are used on MSEEs and if these values maps toohello ones used on hello linked PE-MSEE.</span></span> <span data-ttu-id="aa61a-206">Az MD5 kivonatoló választása esetén hello megosztott kulcsot kell ugyanazon a MSEE és a PE-MSEE pár.</span><span class="sxs-lookup"><span data-stu-id="aa61a-206">If MD5 hashing is chosen, hello shared key should be same on MSEE and PE-MSEE pair.</span></span> <span data-ttu-id="aa61a-207">toochange hello konfigurációs hello MSEE útválasztók, tekintse meg a túl [létrehozása és módosítása az ExpressRoute-kapcsolatcsoportot útválasztási] [CreatePeering].</span><span class="sxs-lookup"><span data-stu-id="aa61a-207">toochange hello configuration on hello MSEE routers, refer too[Create and modify routing for an ExpressRoute circuit][CreatePeering].</span></span>  
>
>

### <a name="verification-via-powershell-classic"></a><span data-ttu-id="aa61a-208">Ellenőrzési PowerShell (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="aa61a-208">Verification via PowerShell (Classic)</span></span>
<span data-ttu-id="aa61a-209">tooget hello Azure titkos társviszony konfigurációs részletek hello a következő parancsot használja:</span><span class="sxs-lookup"><span data-stu-id="aa61a-209">tooget hello Azure private peering configuration details, use hello following command:</span></span>

    Get-AzureBGPPeering -AccessType Private -ServiceKey "*********************************"

<span data-ttu-id="aa61a-210">A rendszer egy minta választ, egy sikeresen konfigurált magánhálózati társviszony-létesítés esetében:</span><span class="sxs-lookup"><span data-stu-id="aa61a-210">A sample response, for a successfully configured private peering is:</span></span>

    AdvertisedPublicPrefixes       : 
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 
    PeerAsn                        : ####
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 10.0.0.0/30
    RoutingRegistryName            : 
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 10.0.0.4/30
    State                          : Enabled
    VlanId                         : 100

<span data-ttu-id="aa61a-211">A sikeres legyen, engedélyezve társviszony-létesítési környezetben hello elsődleges és másodlagos társ alhálózatok felsorolt kellene lennie.</span><span class="sxs-lookup"><span data-stu-id="aa61a-211">A successfully, enabled peering context would have hello primary and secondary peer subnets listed.</span></span> <span data-ttu-id="aa61a-212">hello/30-as alhálózat hello MSEEs hello illesztő IP-címét és a PE-MSEEs használ.</span><span class="sxs-lookup"><span data-stu-id="aa61a-212">hello /30 subnets are used for hello interface IP address of hello MSEEs and PE-MSEEs.</span></span>

<span data-ttu-id="aa61a-213">tooget hello Azure nyilvános társviszony konfigurációs részletek, használja a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="aa61a-213">tooget hello Azure public peering configuration details, use hello following commands:</span></span>

    Get-AzureBGPPeering -AccessType Public -ServiceKey "*********************************"

<span data-ttu-id="aa61a-214">tooget hello Microsoft társviszony-létesítési konfiguráció részletei, a következő parancsok hello használata:</span><span class="sxs-lookup"><span data-stu-id="aa61a-214">tooget hello Microsoft peering configuration details, use hello following commands:</span></span>

    Get-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************"

>[!IMPORTANT]
><span data-ttu-id="aa61a-215">3 rétegbeli társviszony hello szolgáltató által beállított, hello ExpressRoute-társviszony hello portál vagy PowerShell beállítás felülírja a hello szolgáltatás szolgáltató beállításai.</span><span class="sxs-lookup"><span data-stu-id="aa61a-215">If layer 3 peerings were set by hello service provider, setting hello ExpressRoute peerings via hello portal or PowerShell overwrites hello service provider settings.</span></span> <span data-ttu-id="aa61a-216">Hello szolgáltató ügyféloldali társviszony-létesítési beállításainak visszaállításáról hello támogatás hello szolgáltató szükséges.</span><span class="sxs-lookup"><span data-stu-id="aa61a-216">Resetting hello provider side peering settings requires hello support of hello service provider.</span></span> <span data-ttu-id="aa61a-217">Hello ExpressRoute-társviszony csak akkor módosítsa, ha biztos, hogy hello szolgáltató biztosítja a csak a 2. réteg szolgáltatásokat!</span><span class="sxs-lookup"><span data-stu-id="aa61a-217">Only modify hello ExpressRoute peerings if it is certain that hello service provider is providing layer 2 services only!</span></span>
>
>

<p/>
>[!NOTE]
><span data-ttu-id="aa61a-218">Ha nincs engedélyezve a társviszony-létesítés, ellenőrizze, ha hello elsődleges és másodlagos társ rendelt alhálózatok egyezés hello konfigurációs hello csatolt PE-MSEE.</span><span class="sxs-lookup"><span data-stu-id="aa61a-218">If a peering is not enabled, check if hello primary and secondary peer subnets assigned match hello configuration on hello linked PE-MSEE.</span></span> <span data-ttu-id="aa61a-219">Ellenőrizze, hogy hello kijavíthatja is *VlanId*, *AzureAsn*, és *PeerAsn* MSEEs használja, és ha ezeket az értékeket leképezi a hello lévőktől toohello csatolt PE-MSEE.</span><span class="sxs-lookup"><span data-stu-id="aa61a-219">Also check if hello correct *VlanId*, *AzureAsn*, and *PeerAsn* are used on MSEEs and if these values maps toohello ones used on hello linked PE-MSEE.</span></span> <span data-ttu-id="aa61a-220">toochange hello konfigurációs hello MSEE útválasztók, tekintse meg a túl [létrehozása és módosítása az ExpressRoute-kapcsolatcsoportot útválasztási] [CreatePeering].</span><span class="sxs-lookup"><span data-stu-id="aa61a-220">toochange hello configuration on hello MSEE routers, refer too[Create and modify routing for an ExpressRoute circuit][CreatePeering].</span></span>
>
>

## <a name="validate-arp-between-microsoft-and-hello-service-provider"></a><span data-ttu-id="aa61a-221">ARP ellenőrzése a Microsoft és hello szolgáltató között</span><span class="sxs-lookup"><span data-stu-id="aa61a-221">Validate ARP between Microsoft and hello service provider</span></span>
<span data-ttu-id="aa61a-222">Ez a szakasz (klasszikus) PowerShell-parancsokat használ.</span><span class="sxs-lookup"><span data-stu-id="aa61a-222">This section uses PowerShell (Classic) commands.</span></span> <span data-ttu-id="aa61a-223">Ha használt PowerShell Azure Resource Manager parancsok, győződjön meg arról, hogy rendelkezik-e rendszergazdai vagy társadminisztrátori hozzáférés toohello előfizetésébe [a klasszikus Azure portálon][OldPortal].</span><span class="sxs-lookup"><span data-stu-id="aa61a-223">If you have been using PowerShell Azure Resource Manager commands, ensure that you have admin/co-admin access toohello subscription via [Azure classic portal][OldPortal].</span></span> <span data-ttu-id="aa61a-224">A hibaelhárítás Azure Resource Manager segítségével parancsok tekintse meg az toohello [első ARP táblák hello Resource Manager üzembe helyezési modellel] [ ARP] dokumentum.</span><span class="sxs-lookup"><span data-stu-id="aa61a-224">For troubleshooting using Azure Resource Manager commands please refer toohello [Getting ARP tables in hello Resource Manager deployment model][ARP] document.</span></span>

>[!NOTE]
><span data-ttu-id="aa61a-225">tooget ARP, hello Azure-portálon, mind az Azure Resource Manager PowerShell-parancsok használhatók.</span><span class="sxs-lookup"><span data-stu-id="aa61a-225">tooget ARP, both hello Azure portal and Azure Resource Manager PowerShell commands can be used.</span></span> <span data-ttu-id="aa61a-226">Ha hiba történik, hello Azure Resource Manager PowerShell-parancsokkal, klasszikus PowerShell-parancsok klasszikus PowerShell parancsokat is dolgozhat Azure Resource Manager ExpressRoute-Kapcsolatcsoportok használható.</span><span class="sxs-lookup"><span data-stu-id="aa61a-226">If errors are encountered with hello Azure Resource Manager PowerShell commands, classic PowerShell commands should work as Classic PowerShell commands also work with Azure Resource Manager ExpressRoute circuits.</span></span>
>
>

<span data-ttu-id="aa61a-227">tooget hello hello elsődleges MSEE útválasztó hello magánhálózati társviszony-létesítés ARP a táblázatot, a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="aa61a-227">tooget hello ARP table from hello primary MSEE router for hello private peering, use hello following command:</span></span>

    Get-AzureDedicatedCircuitPeeringArpInfo -AccessType Private -Path Primary -ServiceKey "*********************************"

<span data-ttu-id="aa61a-228">Egy példa egy válasz hello parancs sikeres forgatókönyvben hello:</span><span class="sxs-lookup"><span data-stu-id="aa61a-228">An example response for hello command, in hello successful scenario:</span></span>

    ARP Info:

                 Age           Interface           IpAddress          MacAddress
                 113             On-Prem       10.0.0.1           e8ed.f335.4ca9
                   0           Microsoft       10.0.0.2           7c0e.ce85.4fc9

<span data-ttu-id="aa61a-229">Hasonló módon ellenőrizheti a hello hello MSEE a hello ARP tábla *elsődleges*/*másodlagos* elérési útja, a *titkos* /  *Nyilvános*/*Microsoft* esetében.</span><span class="sxs-lookup"><span data-stu-id="aa61a-229">Similarly, you can check hello ARP table from hello MSEE in hello *Primary*/*Secondary* path, for *Private*/*Public*/*Microsoft* peerings.</span></span>

<span data-ttu-id="aa61a-230">hello alábbi példa azt mutatja be a társviszony-létesítés hello parancs válasz hello nem létezik.</span><span class="sxs-lookup"><span data-stu-id="aa61a-230">hello following example shows hello response of hello command for a peering does not exist.</span></span>

    ARP Info:
       
>[!NOTE]
><span data-ttu-id="aa61a-231">Ha hello ARP-táblázat nem rendelkezik a hello felületek IP-címek hozzárendelt tooMAC címeket, felülvizsgálati hello a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="aa61a-231">If hello ARP table does not have IP addresses of hello interfaces mapped tooMAC addresses, review hello following information:</span></span>
>1. <span data-ttu-id="aa61a-232">Ha hello hivatkozás közötti hozzárendelt első IP-cím hello hello/30-as alhálózat MSEE-PR hello és MSEE MSEE-PR. hello felületén szolgál</span><span class="sxs-lookup"><span data-stu-id="aa61a-232">If hello first IP address of hello /30 subnet assigned for hello link between hello MSEE-PR and MSEE is used on hello interface of MSEE-PR.</span></span> <span data-ttu-id="aa61a-233">Azure mindig MSEEs hello második IP-címet használja.</span><span class="sxs-lookup"><span data-stu-id="aa61a-233">Azure always uses hello second IP address for MSEEs.</span></span>
>2. <span data-ttu-id="aa61a-234">Győződjön meg arról, ha hello ügyfél (C-címke) és VLAN-címkék szolgáltatás (S-címke) felel meg a MSEE-PR és MSEE pár mindkét.</span><span class="sxs-lookup"><span data-stu-id="aa61a-234">Verify if hello customer (C-Tag) and service (S-Tag) VLAN tags match both on MSEE-PR and MSEE pair.</span></span>
>
>

## <a name="validate-bgp-and-routes-on-hello-msee"></a><span data-ttu-id="aa61a-235">Ellenőrizze a BGP és hello MSEE útvonalak</span><span class="sxs-lookup"><span data-stu-id="aa61a-235">Validate BGP and routes on hello MSEE</span></span>
<span data-ttu-id="aa61a-236">Ez a szakasz (klasszikus) PowerShell-parancsokat használ.</span><span class="sxs-lookup"><span data-stu-id="aa61a-236">This section uses PowerShell (Classic) commands.</span></span> <span data-ttu-id="aa61a-237">Ha használt PowerShell Azure Resource Manager parancsok, győződjön meg arról, hogy rendelkezik-e rendszergazdai vagy társadminisztrátori hozzáférés toohello előfizetésébe [a klasszikus Azure portálon][OldPortal]</span><span class="sxs-lookup"><span data-stu-id="aa61a-237">If you have been using PowerShell Azure Resource Manager commands, ensure that you have admin/co-admin access toohello subscription via [Azure classic portal][OldPortal]</span></span>

>[!NOTE]
><span data-ttu-id="aa61a-238">tooget BGP információ mindkét hello Azure-portál és az Azure Resource Manager PowerShell-parancsok is használhatók.</span><span class="sxs-lookup"><span data-stu-id="aa61a-238">tooget BGP information, both hello Azure portal and Azure Resource Manager PowerShell commands can be used.</span></span> <span data-ttu-id="aa61a-239">Ha hiba történik, hello Azure Resource Manager PowerShell-parancsokkal, klasszikus PowerShell-parancsok klasszikus PowerShell parancsokat is dolgozhat Azure Resource Manager ExpressRoute-Kapcsolatcsoportok használható.</span><span class="sxs-lookup"><span data-stu-id="aa61a-239">If errors are encountered with hello Azure Resource Manager PowerShell commands, classic PowerShell commands should work as classic PowerShell commands also work with Azure Resource Manager ExpressRoute circuits.</span></span>
>
>

<span data-ttu-id="aa61a-240">tooget hello egy adott útválasztási környezet összefoglaló útválasztási táblázathoz (BGP szomszéd), a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="aa61a-240">tooget hello routing table (BGP neighbor) summary for a particular routing context, use hello following command:</span></span>

    Get-AzureDedicatedCircuitPeeringRouteTableSummary -AccessType Private -Path Primary -ServiceKey "*********************************"

<span data-ttu-id="aa61a-241">Egy példa egy válasz a következő:</span><span class="sxs-lookup"><span data-stu-id="aa61a-241">An example response is:</span></span>

    Route Table Summary:

            Neighbor                   V                  AS              UpDown         StatePfxRcd
            10.0.0.1                   4                ####                8w4d                  50

<span data-ttu-id="aa61a-242">Ahogy az előző példa hello, hello parancs a hello útválasztási környezet létrehozása mennyi ideig hasznos toodetermine.</span><span class="sxs-lookup"><span data-stu-id="aa61a-242">As shown in hello preceding example, hello command is useful toodetermine for how long hello routing context has been established.</span></span> <span data-ttu-id="aa61a-243">Azt is hello társviszony-létesítési útválasztó által hirdetett útvonal előtagok számát jelzi.</span><span class="sxs-lookup"><span data-stu-id="aa61a-243">It also indicates number of route prefixes advertised by hello peering router.</span></span>

>[!NOTE]
><span data-ttu-id="aa61a-244">Ha hello állapota aktív vagy inaktív, ellenőrizze, ha hello elsődleges és másodlagos társ rendelt alhálózatok egyezés hello konfigurációs hello csatolt PE-MSEE.</span><span class="sxs-lookup"><span data-stu-id="aa61a-244">If hello state is in Active or Idle, check if hello primary and secondary peer subnets assigned match hello configuration on hello linked PE-MSEE.</span></span> <span data-ttu-id="aa61a-245">Ellenőrizze, hogy hello kijavíthatja is *VlanId*, *AzureAsn*, és *PeerAsn* MSEEs használja, és ha ezeket az értékeket leképezi a hello lévőktől toohello csatolt PE-MSEE.</span><span class="sxs-lookup"><span data-stu-id="aa61a-245">Also check if hello correct *VlanId*, *AzureAsn*, and *PeerAsn* are used on MSEEs and if these values maps toohello ones used on hello linked PE-MSEE.</span></span> <span data-ttu-id="aa61a-246">Az MD5 kivonatoló választása esetén hello megosztott kulcsot kell ugyanazon a MSEE és a PE-MSEE pár.</span><span class="sxs-lookup"><span data-stu-id="aa61a-246">If MD5 hashing is chosen, hello shared key should be same on MSEE and PE-MSEE pair.</span></span> <span data-ttu-id="aa61a-247">toochange hello konfigurációs hello MSEE útválasztók, tekintse meg a túl[létrehozása és módosítása az ExpressRoute-kapcsolatcsoportot útválasztási][CreatePeering].</span><span class="sxs-lookup"><span data-stu-id="aa61a-247">toochange hello configuration on hello MSEE routers, refer too[Create and modify routing for an ExpressRoute circuit][CreatePeering].</span></span>
>
>

<p/>
>[!NOTE]
><span data-ttu-id="aa61a-248">Egyes célhoz keresztül adott társviszony-létesítés nem érhetők el, ha ellenőrizze hello útválasztási táblázatot az hello MSEEs tartozó toohello adott társviszony-létesítési környezetben.</span><span class="sxs-lookup"><span data-stu-id="aa61a-248">If certain destinations are not reachable over a particular peering, check hello route table of hello MSEEs belonging toohello particular peering context.</span></span> <span data-ttu-id="aa61a-249">Ha egy egyező előtag (Ez lehet gelt IP) hello útválasztási táblázatban szerepel, akkor ellenőrizze Ha tűzfalak/NSG/hozzáférés-vezérlési listák hello elérési úton, és ha azok hello forgalom engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="aa61a-249">If a matching prefix (could be NATed IP) is present in hello routing table, then check if there are firewalls/NSG/ACLs on hello path and if they permit hello traffic.</span></span>
>
>

<span data-ttu-id="aa61a-250">tooget hello teljes útválasztási táblázatot MSEE a hello *elsődleges* adott hello elérési útját *titkos* útválasztási környezetben, a következő parancs használata hello:</span><span class="sxs-lookup"><span data-stu-id="aa61a-250">tooget hello full routing table from MSEE on hello *Primary* path for hello particular *Private* routing context, use hello following command:</span></span>

    Get-AzureDedicatedCircuitPeeringRouteTableInfo -AccessType Private -Path Primary -ServiceKey "*********************************"

<span data-ttu-id="aa61a-251">Egy példa sikeres hello parancs eredménye:</span><span class="sxs-lookup"><span data-stu-id="aa61a-251">An example successful outcome for hello command is:</span></span>

    Route Table Info:

             Network             NextHop              LocPrf              Weight                Path
         10.1.0.0/16            10.0.0.1                                       0    #### ##### #####     
         10.2.0.0/16            10.0.0.1                                       0    #### ##### #####
    ...

<span data-ttu-id="aa61a-252">Hasonlóképpen, ellenőrizheti a hello útválasztási táblázatot hello MSEE a hello *elsődleges*/*másodlagos* elérési útja, a *titkos* / *Nyilvános*/*Microsoft* társviszony-létesítési környezetben.</span><span class="sxs-lookup"><span data-stu-id="aa61a-252">Similarly, you can check hello routing table from hello MSEE in hello *Primary*/*Secondary* path, for *Private*/*Public*/*Microsoft* a peering context.</span></span>

<span data-ttu-id="aa61a-253">hello alábbi példa azt mutatja be a társviszony-létesítés hello parancs válasz hello nem létezik:</span><span class="sxs-lookup"><span data-stu-id="aa61a-253">hello following example shows hello response of hello command for a peering does not exist:</span></span>

    Route Table Info:

##<a name="check-hello-traffic-statistics"></a><span data-ttu-id="aa61a-254">Ellenőrizze a hello forgalom statisztikák</span><span class="sxs-lookup"><span data-stu-id="aa61a-254">Check hello Traffic Statistics</span></span>
<span data-ttu-id="aa61a-255">tooget hello elsődleges és másodlagos elérési forgalom statisztika--bájt bejövő és kimenő adatforgalma – kombinált társviszony-létesítési környezetének, a következő parancs használata hello:</span><span class="sxs-lookup"><span data-stu-id="aa61a-255">tooget hello combined primary and secondary path traffic statistics--bytes in and out--of a peering context, use hello following command:</span></span>

    Get-AzureDedicatedCircuitStats -ServiceKey 97f85950-01dd-4d30-a73c-bf683b3a6e5c -AccessType Private

<span data-ttu-id="aa61a-256">Egy minta hello parancs kimenete:</span><span class="sxs-lookup"><span data-stu-id="aa61a-256">A sample output of hello command is:</span></span>

    PrimaryBytesIn PrimaryBytesOut SecondaryBytesIn SecondaryBytesOut
    -------------- --------------- ---------------- -----------------
         240780020       239863857        240565035         239628474

<span data-ttu-id="aa61a-257">A minta a nem létező társviszony-létesítés hello parancs kimenete:</span><span class="sxs-lookup"><span data-stu-id="aa61a-257">A sample output of hello command for a non-existent peering is:</span></span>

    Get-AzureDedicatedCircuitStats : ResourceNotFound: Can not find any subinterface for peering type 'Public' for circuit '97f85950-01dd-4d30-a73c-bf683b3a6e5c' .
    At line:1 char:1
    + Get-AzureDedicatedCircuitStats -ServiceKey 97f85950-01dd-4d30-a73c-bf ...
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : CloseError: (:) [Get-AzureDedicatedCircuitStats], CloudException
        + FullyQualifiedErrorId : Microsoft.WindowsAzure.Commands.ExpressRoute.GetAzureDedicatedCircuitPeeringStatsCommand

## <a name="next-steps"></a><span data-ttu-id="aa61a-258">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="aa61a-258">Next Steps</span></span>
<span data-ttu-id="aa61a-259">További információt vagy a help tekintse meg a következő hivatkozások hello:</span><span class="sxs-lookup"><span data-stu-id="aa61a-259">For more information or help, check out hello following links:</span></span>

- <span data-ttu-id="aa61a-260">[A Microsoft támogatás][Support]</span><span class="sxs-lookup"><span data-stu-id="aa61a-260">[Microsoft Support][Support]</span></span>
- <span data-ttu-id="aa61a-261">[Létrehozásához és módosításához ExpressRoute-kapcsolatcsoportot][CreateCircuit]</span><span class="sxs-lookup"><span data-stu-id="aa61a-261">[Create and modify an ExpressRoute circuit][CreateCircuit]</span></span>
- <span data-ttu-id="aa61a-262">[Létrehozásához és módosításához útválasztás az ExpressRoute-kapcsolatcsoportot][CreatePeering]</span><span class="sxs-lookup"><span data-stu-id="aa61a-262">[Create and modify routing for an ExpressRoute circuit][CreatePeering]</span></span>

<!--Image References-->
[1]: ./media/expressroute-troubleshooting-expressroute-overview/expressroute-logical-diagram.png "Logikai Expressroute-kapcsolat"
[2]: ./media/expressroute-troubleshooting-expressroute-overview/portal-all-resources.png "Minden erőforrás ikon"
[3]: ./media/expressroute-troubleshooting-expressroute-overview/portal-overview.png "Áttekintés ikon"
[4]: ./media/expressroute-troubleshooting-expressroute-overview/portal-circuit-status.png "ExpressRoute Essentials minta képernyőképe"
[5]: ./media/expressroute-troubleshooting-expressroute-overview/portal-private-peering.png "ExpressRoute Essentials minta képernyőképe"

<!--Link References-->
[Support]: https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[CreateCircuit]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-circuit-portal-resource-manager 
[CreatePeering]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-routing-portal-resource-manager
[OldPortal]: https://manage.windowsazure.com
[ARP]: https://docs.microsoft.com/en-us/azure/expressroute/expressroute-troubleshooting-arp-resource-manager






