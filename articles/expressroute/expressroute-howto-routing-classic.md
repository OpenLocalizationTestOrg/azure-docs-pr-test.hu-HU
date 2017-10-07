---
title: "Hogyan tooconfigure útválasztás (társviszony) az ExpressRoute-kapcsolatcsoportot: Azure: klasszikus |} Microsoft Docs"
description: "Ez a cikk végigvezeti hello létrehozásához és a kiépítés hello saját, a nyilvános és a Microsoft társviszony-létesítést az ExpressRoute-kapcsolatcsoportot. Ez a cikk is bemutatja, hogyan toocheck hello állapotát, a frissítést, vagy a-kör társviszony törlése."
documentationcenter: na
services: expressroute
author: ganesr
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: a4bd39d2-373a-467a-8b06-36cfcc1027d2
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/21/2017
ms.author: ganesr;cherylmc
ms.openlocfilehash: dc5bcc4b86c3bc965a88281b6c2a660f3bc58eb1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-modify-peering-for-an-expressroute-circuit-classic"></a><span data-ttu-id="9dcd4-104">Létrehozásához és módosításához (klasszikus) ExpressRoute-kör társviszony</span><span class="sxs-lookup"><span data-stu-id="9dcd4-104">Create and modify peering for an ExpressRoute circuit (classic)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="9dcd4-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="9dcd4-105">Azure portal</span></span>](expressroute-howto-routing-portal-resource-manager.md)
> * [<span data-ttu-id="9dcd4-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="9dcd4-106">PowerShell</span></span>](expressroute-howto-routing-arm.md)
> * [<span data-ttu-id="9dcd4-107">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="9dcd4-107">Azure CLI</span></span>](howto-routing-cli.md)
> * [<span data-ttu-id="9dcd4-108">Video - privát társviszony-létesítés</span><span class="sxs-lookup"><span data-stu-id="9dcd4-108">Video - Private peering</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-private-peering-for-your-expressroute-circuit)
> * [<span data-ttu-id="9dcd4-109">Video - nyilvános társviszony-létesítés</span><span class="sxs-lookup"><span data-stu-id="9dcd4-109">Video - Public peering</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-public-peering-for-your-expressroute-circuit)
> * [<span data-ttu-id="9dcd4-110">Videó – a Microsoft társviszony-létesítés</span><span class="sxs-lookup"><span data-stu-id="9dcd4-110">Video - Microsoft peering</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-microsoft-peering-for-your-expressroute-circuit)
> * [<span data-ttu-id="9dcd4-111">PowerShell (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="9dcd4-111">PowerShell (classic)</span></span>](expressroute-howto-routing-classic.md)
> 

<span data-ttu-id="9dcd4-112">Ez a cikk bemutatja, hogyan hello lépéseket toocreate és kezelése a PowerShell és hello klasszikus üzembe helyezési modellel ExpressRoute-kapcsolatcsoportot útválasztási konfigurációja.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-112">This article walks you through hello steps toocreate and manage routing configuration for an ExpressRoute circuit using PowerShell and hello classic deployment model.</span></span> <span data-ttu-id="9dcd4-113">az alábbi hello lépéseit is bemutatja, hogyan toocheck hello állapotát, a frissítés, vagy törölje, majd kiosztásának megszüntetése ExpressRoute-kör társviszony.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-113">hello steps below will also show you how toocheck hello status, update, or delete and deprovision peerings for an ExpressRoute circuit.</span></span>

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

<span data-ttu-id="9dcd4-114">**Tudnivalók az Azure üzembe helyezési modelljeiről**</span><span class="sxs-lookup"><span data-stu-id="9dcd4-114">**About Azure deployment models**</span></span>

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]


## <a name="configuration-prerequisites"></a><span data-ttu-id="9dcd4-115">Konfigurációs előfeltételek</span><span class="sxs-lookup"><span data-stu-id="9dcd4-115">Configuration prerequisites</span></span>
* <span data-ttu-id="9dcd4-116">Szüksége lesz Azure Service Management (SM) PowerShell-parancsmagok hello hello legújabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-116">You will need hello latest version of hello Azure Service Management (SM) PowerShell cmdlets.</span></span> <span data-ttu-id="9dcd4-117">További információkért lásd: [Ismerkedés az Azure PowerShell-parancsmagok](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="9dcd4-117">For more information, see [Getting started with Azure PowerShell cmdlets](/powershell/azure/overview).</span></span>  
* <span data-ttu-id="9dcd4-118">Győződjön meg arról, hogy átolvasta hello [Előfeltételek](expressroute-prerequisites.md) lap hello [útválasztási követelmények](expressroute-routing.md) lap és hello [munkafolyamatok](expressroute-workflows.md) lapon konfigurálás elkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-118">Make sure that you have reviewed hello [prerequisites](expressroute-prerequisites.md) page, hello [routing requirements](expressroute-routing.md) page, and hello [workflows](expressroute-workflows.md) page before you begin configuration.</span></span>
* <span data-ttu-id="9dcd4-119">Egy aktív ExpressRoute-kapcsolatcsoportra lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-119">You must have an active ExpressRoute circuit.</span></span> <span data-ttu-id="9dcd4-120">Útmutatás alapján hello túl[ExpressRoute-kapcsolatcsoportot létrehozni](expressroute-howto-circuit-classic.md) , és folytassa a kapcsolat szolgáltatójánál előtt által engedélyezett hello áramkör.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-120">Follow hello instructions too[create an ExpressRoute circuit](expressroute-howto-circuit-classic.md) and have hello circuit enabled by your connectivity provider before you proceed.</span></span> <span data-ttu-id="9dcd4-121">hello ExpressRoute-kapcsolatcsoportot toobe képes toorun hello parancsmagok az alábbiakban meg kiépített és engedélyezett állapotban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-121">hello ExpressRoute circuit must be in a provisioned and enabled state for you toobe able toorun hello cmdlets described below.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9dcd4-122">Ezek az utasítások csak a 2. rétegbeli kapcsolatot szolgáltatásokat nyújtó szolgáltatók létre toocircuits vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-122">These instructions only apply toocircuits created with service providers offering Layer 2 connectivity services.</span></span> <span data-ttu-id="9dcd4-123">Ha 3. rétegbeli szolgáltatásokat kínáló szolgáltatóval rendelkezik (tipikusan IPVPN, mint az MPLS), a kapcsolatszolgáltató konfigurálja és felügyeli az útválasztást Ön helyett.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-123">If you are using a service provider offering managed Layer 3 services (typically an IPVPN, like MPLS), your connectivity provider will configure and manage routing for you.</span></span>
> 
> 

<span data-ttu-id="9dcd4-124">Egy, két vagy akár mindhárom társviszony-létesítést (Azure privát, Azure nyilvános és Microsoft) is konfigurálhatja egy adott ExpressRoute-kapcsolatcsoportban.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-124">You can configure one, two, or all three peerings (Azure private, Azure public and Microsoft) for an ExpressRoute circuit.</span></span> <span data-ttu-id="9dcd4-125">A társviszony-létesítéseket tetszőleges sorrendben konfigurálhatja.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-125">You can configure peerings in any order you choose.</span></span> <span data-ttu-id="9dcd4-126">Azonban kell győződjön meg arról, hogy elvégezte-e minden társviszony-létesítési egyszerre csak egy hello konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-126">However, you must make sure that you complete hello configuration of each peering one at a time.</span></span>


### <a name="log-in-tooyour-azure-account-and-select-a-subscription"></a><span data-ttu-id="9dcd4-127">Jelentkezzen be Azure-fiók tooyour, és válasszon egy előfizetést</span><span class="sxs-lookup"><span data-stu-id="9dcd4-127">Log in tooyour Azure account and select a subscription</span></span>
1. <span data-ttu-id="9dcd4-128">Nyissa meg a PowerShell-konzolt emelt szintű jogosultságokkal, és csatlakozzon a tooyour fiók.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-128">Open your PowerShell console with elevated rights and connect tooyour account.</span></span> <span data-ttu-id="9dcd4-129">A következő példa toohelp csatlakozás hello használata:</span><span class="sxs-lookup"><span data-stu-id="9dcd4-129">Use hello following example toohelp you connect:</span></span>

        Login-AzureRmAccount

2. <span data-ttu-id="9dcd4-130">Hello előfizetések hello fiók ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-130">Check hello subscriptions for hello account.</span></span>

        Get-AzureRmSubscription

3. <span data-ttu-id="9dcd4-131">Ha egynél több előfizetéssel rendelkezik, válassza ki a megjeleníteni kívánt toouse hello előfizetést.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-131">If you have more than one subscription, select hello subscription that you want toouse.</span></span>

        Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

4. <span data-ttu-id="9dcd4-132">Ezután használhatja a következő parancsmag tooadd hello az Azure-előfizetés tooPowerShell hello klasszikus telepítési modell.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-132">Next, use hello following cmdlet tooadd your Azure subscription tooPowerShell for hello classic deployment model.</span></span>

        Add-AzureAccount


## <a name="azure-private-peering"></a><span data-ttu-id="9dcd4-133">Azure privát társviszony-létesítés</span><span class="sxs-lookup"><span data-stu-id="9dcd4-133">Azure private peering</span></span>
<span data-ttu-id="9dcd4-134">Ez a szakasz útmutatás hogyan toocreate, beolvasása, frissítése és törlése hello Azure privát társviszony-létesítési konfiguráció ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-134">This section provides instructions on how toocreate, get, update, and delete hello Azure private peering configuration for an ExpressRoute circuit.</span></span> 

### <a name="toocreate-azure-private-peering"></a><span data-ttu-id="9dcd4-135">az Azure magánhálózati társviszony-létesítés toocreate</span><span class="sxs-lookup"><span data-stu-id="9dcd4-135">toocreate Azure private peering</span></span>
1. <span data-ttu-id="9dcd4-136">**ExpressRoute hello PowerShell modul importálása.**</span><span class="sxs-lookup"><span data-stu-id="9dcd4-136">**Import hello PowerShell module for ExpressRoute.**</span></span>
   
    <span data-ttu-id="9dcd4-137">Hello Azure és az ExpressRoute modulok importálni kell az hello PowerShell-munkamenetet a rendelés toostart hello ExpressRoute-parancsmagok használatával.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-137">You must import hello Azure and ExpressRoute modules into hello PowerShell session in order toostart using hello ExpressRoute cmdlets.</span></span> <span data-ttu-id="9dcd4-138">Futtatási hello következő tooimport hello Azure és az ExpressRoute-modulok parancsokat hello PowerShell-munkamenetbe.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-138">Run hello following commands tooimport hello Azure and ExpressRoute modules into hello PowerShell session.</span></span>  
   
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'
2. <span data-ttu-id="9dcd4-139">**ExpressRoute-kapcsolatcsoportot létrehozni.**</span><span class="sxs-lookup"><span data-stu-id="9dcd4-139">**Create an ExpressRoute circuit.**</span></span>
   
    <span data-ttu-id="9dcd4-140">Kövesse az utasításokat toocreate hello egy [ExpressRoute-kapcsolatcsoportot](expressroute-howto-circuit-classic.md) , illetve hozta-e hello kapcsolat szolgáltatóját.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-140">Follow hello instructions toocreate an [ExpressRoute circuit](expressroute-howto-circuit-classic.md) and have it provisioned by hello connectivity provider.</span></span> <span data-ttu-id="9dcd4-141">Ha a kapcsolat szolgáltatójánál 3. rétegbeli felügyelt szolgáltatásokat kínál, a kapcsolat szolgáltató tooenable magánhálózati társviszony-létesítést az Ön Azure kérhet.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-141">If your connectivity provider offers managed Layer 3 services, you can request your connectivity provider tooenable Azure private peering for you.</span></span> <span data-ttu-id="9dcd4-142">Ebben az esetben nincs szükség hello következő szakaszokban szereplő toofollow utasításokat.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-142">In that case, you won't need toofollow instructions listed in hello next sections.</span></span> <span data-ttu-id="9dcd4-143">Ha a kapcsolat szolgáltatójánál nem kezeli az útválasztást, a kapcsolat létrehozása után azonban kövesse az alábbi hello-utasításokat.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-143">However, if your connectivity provider does not manage routing for you, after creating your circuit, follow hello instructions below.</span></span> 
3. <span data-ttu-id="9dcd4-144">**Ellenőrizze a hello ExpressRoute-kapcsolatcsoport tooensure ki van építve.**</span><span class="sxs-lookup"><span data-stu-id="9dcd4-144">**Check hello ExpressRoute circuit tooensure it is provisioned.**</span></span>
   
    <span data-ttu-id="9dcd4-145">Ha ExpressRoute-kapcsolatcsoportot hello kiosztásakor és is engedélyezve van, először toosee ellenőriz.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-145">You must first check toosee if hello ExpressRoute circuit is Provisioned and also Enabled.</span></span> <span data-ttu-id="9dcd4-146">Tekintse meg az alábbi példa hello.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-146">See hello example below.</span></span>
   
        PS C:\> Get-AzureDedicatedCircuit -ServiceKey "*********************************"
   
        Bandwidth                        : 200
        CircuitName                      : MyTestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled
   
    <span data-ttu-id="9dcd4-147">Győződjön meg arról, hogy látható-e hello áramkör kiépítve és engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-147">Make sure that hello circuit shows as Provisioned and Enabled.</span></span> <span data-ttu-id="9dcd4-148">Ha nem, dolgozni a kapcsolat szolgáltató tooget a kapcsolatcsoport szükséges toohello állapotát és állapotát.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-148">If it doesn't, work with your connectivity provider tooget your circuit toohello required state and status.</span></span>
   
        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled
4. <span data-ttu-id="9dcd4-149">**Az Azure magánhálózati társviszony-létesítés hello kör megadása**</span><span class="sxs-lookup"><span data-stu-id="9dcd4-149">**Configure Azure private peering for hello circuit.**</span></span>
   
    <span data-ttu-id="9dcd4-150">Győződjön meg arról, hogy rendelkezik-e hello hello lépések folytatása előtt a következő elemek:</span><span class="sxs-lookup"><span data-stu-id="9dcd4-150">Make sure that you have hello following items before you proceed with hello next steps:</span></span>
   
   * <span data-ttu-id="9dcd4-151">/ 30-as alhálózat hello elsődleges kapcsolathoz.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-151">A /30 subnet for hello primary link.</span></span> <span data-ttu-id="9dcd4-152">Ez nem képezheti semmilyen, virtuális hálózatok számára lefoglalt címtér részét.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-152">This must not be part of any address space reserved for virtual networks.</span></span>
   * <span data-ttu-id="9dcd4-153">/ 30-as alhálózat hello másodlagos kapcsolathoz.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-153">A /30 subnet for hello secondary link.</span></span> <span data-ttu-id="9dcd4-154">Ez nem képezheti semmilyen, virtuális hálózatok számára lefoglalt címtér részét.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-154">This must not be part of any address space reserved for virtual networks.</span></span>
   * <span data-ttu-id="9dcd4-155">Egy érvényes VLAN-azonosító tooestablish a társviszony.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-155">A valid VLAN ID tooestablish this peering on.</span></span> <span data-ttu-id="9dcd4-156">Győződjön meg arról, hogy egyetlen másik társviszonya se a hello áramkör használ hello azonos VLAN-azonosítót.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-156">Ensure that no other peering in hello circuit uses hello same VLAN ID.</span></span>
   * <span data-ttu-id="9dcd4-157">Egy AS-szám a társviszony-létesítéshez.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-157">AS number for peering.</span></span> <span data-ttu-id="9dcd4-158">2 és 4 bájtos AS-számokat is használhat.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-158">You can use both 2-byte and 4-byte AS numbers.</span></span> <span data-ttu-id="9dcd4-159">Ehhez a társviszony-létesítéshez használhat privát AS-számokat is.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-159">You can use a private AS number for this peering.</span></span> <span data-ttu-id="9dcd4-160">Ne használja a 65515 számot.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-160">Ensure that you are not using 65515.</span></span>
   * <span data-ttu-id="9dcd4-161">Az MD5 kivonatoló, ha úgy dönt, hogy egy toouse.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-161">An MD5 hash if you choose toouse one.</span></span> <span data-ttu-id="9dcd4-162">**Ez nem kötelező**.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-162">**This is optional**.</span></span>
     
    <span data-ttu-id="9dcd4-163">A következő parancsmag tooconfigure Azure magánhálózati társviszony-létesítés a kör hello is futtathatja.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-163">You can run hello following cmdlet tooconfigure Azure private peering for your circuit.</span></span>
     
        <span data-ttu-id="9dcd4-164">Új AzureBGPPeering - AccessType titkos - ServiceKey "x" - PrimaryPeerSubnet "10.0.0.0/30" - SecondaryPeerSubnet "10.0.0.4/30" - PeerAsn 1234 - VlanId 100</span><span class="sxs-lookup"><span data-stu-id="9dcd4-164">New-AzureBGPPeering -AccessType Private -ServiceKey "*********************************" -PrimaryPeerSubnet "10.0.0.0/30" -SecondaryPeerSubnet "10.0.0.4/30" -PeerAsn 1234 -VlanId 100</span></span>
     
    <span data-ttu-id="9dcd4-165">Az alábbi hello parancsmag is használhatja, ha úgy dönt, toouse az MD5 kivonatoló.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-165">You can use hello cmdlet below if you choose toouse an MD5 hash.</span></span>
     
        <span data-ttu-id="9dcd4-166">Új AzureBGPPeering - AccessType titkos - ServiceKey "x" - PrimaryPeerSubnet "10.0.0.0/30" - SecondaryPeerSubnet "10.0.0.4/30" - PeerAsn 1234 - VlanId 100 - SharedKey "A1B2C3D4"</span><span class="sxs-lookup"><span data-stu-id="9dcd4-166">New-AzureBGPPeering -AccessType Private -ServiceKey "*********************************" -PrimaryPeerSubnet "10.0.0.0/30" -SecondaryPeerSubnet "10.0.0.4/30" -PeerAsn 1234 -VlanId 100 -SharedKey "A1B2C3D4"</span></span>
     
     > [!IMPORTANT]
     > <span data-ttu-id="9dcd4-167">Az AS-számot mindenképp társviszony-létesítési ASN-ként, és ne ügyfél ASN-ként adja meg.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-167">Ensure that you specify your AS number as peering ASN, not customer ASN.</span></span>
     > 
     > 

### <a name="tooview-azure-private-peering-details"></a><span data-ttu-id="9dcd4-168">tooview Azure magánhálózati társviszony-létesítés részletei</span><span class="sxs-lookup"><span data-stu-id="9dcd4-168">tooview Azure private peering details</span></span>
<span data-ttu-id="9dcd4-169">Konfigurációs részletekért hello a következő parancsmag használatával</span><span class="sxs-lookup"><span data-stu-id="9dcd4-169">You can get configuration details using hello following cmdlet</span></span>

    Get-AzureBGPPeering -AccessType Private -ServiceKey "*********************************"

    AdvertisedPublicPrefixes       : 
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 
    PeerAsn                        : 1234
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 10.0.0.0/30
    RoutingRegistryName            : 
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 10.0.0.4/30
    State                          : Enabled
    VlanId                         : 100


### <a name="tooupdate-azure-private-peering-configuration"></a><span data-ttu-id="9dcd4-170">tooupdate Azure magánhálózati társviszony-létesítési konfiguráció</span><span class="sxs-lookup"><span data-stu-id="9dcd4-170">tooupdate Azure private peering configuration</span></span>
<span data-ttu-id="9dcd4-171">Frissítheti, használja a következő parancsmag hello hello konfiguráció bármely részeként.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-171">You can update any part of hello configuration using hello following cmdlet.</span></span> <span data-ttu-id="9dcd4-172">Hello az alábbi példában a VLAN-Azonosítót a kapcsolatcsoport hello hello 100 too500 alatt módosul.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-172">In hello example below, hello VLAN ID of hello circuit is being updated from 100 too500.</span></span>

    Set-AzureBGPPeering -AccessType Private -ServiceKey "*********************************" -PrimaryPeerSubnet "10.0.0.0/30" -SecondaryPeerSubnet "10.0.0.4/30" -PeerAsn 1234 -VlanId 500 -SharedKey "A1B2C3D4"

### <a name="toodelete-azure-private-peering"></a><span data-ttu-id="9dcd4-173">az Azure magánhálózati társviszony-létesítés toodelete</span><span class="sxs-lookup"><span data-stu-id="9dcd4-173">toodelete Azure private peering</span></span>
<span data-ttu-id="9dcd4-174">Eltávolíthatja a társviszony-létesítési konfiguráció hello a következő parancsmag futtatásával.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-174">You can remove your peering configuration by running hello following cmdlet.</span></span>

> [!WARNING]
> <span data-ttu-id="9dcd4-175">Bizonyosodjon meg, hogy az összes virtuális hálózatot az ExpressRoute-kapcsolatcsoportot hello megszüntetni a parancsmag futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-175">You must ensure that all virtual networks are unlinked from hello ExpressRoute circuit before running this cmdlet.</span></span> 
> 
> 

    Remove-AzureBGPPeering -AccessType Private -ServiceKey "*********************************"


## <a name="azure-public-peering"></a><span data-ttu-id="9dcd4-176">Azure nyilvános társviszony-létesítés</span><span class="sxs-lookup"><span data-stu-id="9dcd4-176">Azure public peering</span></span>
<span data-ttu-id="9dcd4-177">Ez a szakasz útmutatás hogyan toocreate, beolvasása, frissítése és törlése hello Azure nyilvános társviszony-létesítési konfiguráció ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-177">This section provides instructions on how toocreate, get, update and delete hello Azure public peering configuration for an ExpressRoute circuit.</span></span>

### <a name="toocreate-azure-public-peering"></a><span data-ttu-id="9dcd4-178">az Azure nyilvános társviszony toocreate</span><span class="sxs-lookup"><span data-stu-id="9dcd4-178">toocreate Azure public peering</span></span>
1. <span data-ttu-id="9dcd4-179">**ExpressRoute hello PowerShell modul importálása.**</span><span class="sxs-lookup"><span data-stu-id="9dcd4-179">**Import hello PowerShell module for ExpressRoute.**</span></span>
   
    <span data-ttu-id="9dcd4-180">Hello Azure és az ExpressRoute modulok importálni kell az hello PowerShell-munkamenetet a rendelés toostart hello ExpressRoute-parancsmagok használatával.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-180">You must import hello Azure and ExpressRoute modules into hello PowerShell session in order toostart using hello ExpressRoute cmdlets.</span></span> <span data-ttu-id="9dcd4-181">Futtatási hello következő tooimport hello Azure és az ExpressRoute-modulok parancsokat hello PowerShell-munkamenetbe.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-181">Run hello following commands tooimport hello Azure and ExpressRoute modules into hello PowerShell session.</span></span> 
   
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'
2. <span data-ttu-id="9dcd4-182">**ExpressRoute-kapcsolatcsoport létrehozása**</span><span class="sxs-lookup"><span data-stu-id="9dcd4-182">**Create an ExpressRoute circuit**</span></span>
   
    <span data-ttu-id="9dcd4-183">Kövesse az utasításokat toocreate hello egy [ExpressRoute-kapcsolatcsoportot](expressroute-howto-circuit-classic.md) , illetve hozta-e hello kapcsolat szolgáltatóját.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-183">Follow hello instructions toocreate an [ExpressRoute circuit](expressroute-howto-circuit-classic.md) and have it provisioned by hello connectivity provider.</span></span> <span data-ttu-id="9dcd4-184">Ha a kapcsolat szolgáltatójánál 3. rétegbeli felügyelt szolgáltatásokat kínál, a kapcsolat szolgáltató tooenable Azure nyilvános társviszony-létesítés meg is igényelhet.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-184">If your connectivity provider offers managed Layer 3 services, you can request your connectivity provider tooenable Azure public peering for you.</span></span> <span data-ttu-id="9dcd4-185">Ebben az esetben nincs szükség hello következő szakaszokban szereplő toofollow utasításokat.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-185">In that case, you won't need toofollow instructions listed in hello next sections.</span></span> <span data-ttu-id="9dcd4-186">Ha a kapcsolat szolgáltatójánál nem kezeli az útválasztást, a kapcsolat létrehozása után azonban kövesse az alábbi hello-utasításokat.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-186">However, if your connectivity provider does not manage routing for you, after creating your circuit, follow hello instructions below.</span></span>
3. <span data-ttu-id="9dcd4-187">**Ellenőrizze a ExpressRoute-kapcsolatcsoport tooensure ki van építve**</span><span class="sxs-lookup"><span data-stu-id="9dcd4-187">**Check ExpressRoute circuit tooensure it is provisioned**</span></span>
   
    <span data-ttu-id="9dcd4-188">Ha ExpressRoute-kapcsolatcsoportot hello kiosztásakor és is engedélyezve van, először toosee ellenőriz.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-188">You must first check toosee if hello ExpressRoute circuit is Provisioned and also Enabled.</span></span> <span data-ttu-id="9dcd4-189">Tekintse meg az alábbi példa hello.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-189">See hello example below.</span></span>
   
        PS C:\> Get-AzureDedicatedCircuit -ServiceKey "*********************************"
   
        Bandwidth                        : 200
        CircuitName                      : MyTestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled
   
    <span data-ttu-id="9dcd4-190">Győződjön meg arról, hogy látható-e hello áramkör kiépítve és engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-190">Make sure that hello circuit shows as Provisioned and Enabled.</span></span> <span data-ttu-id="9dcd4-191">Ha nem, dolgozni a kapcsolat szolgáltató tooget a kapcsolatcsoport szükséges toohello állapotát és állapotát.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-191">If it doesn't, work with your connectivity provider tooget your circuit toohello required state and status.</span></span>
   
        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled
4. <span data-ttu-id="9dcd4-192">**Az Azure nyilvános társviszony-létesítés hello kapcsolat konfigurálása**</span><span class="sxs-lookup"><span data-stu-id="9dcd4-192">**Configure Azure public peering for hello circuit**</span></span>
   
    <span data-ttu-id="9dcd4-193">Győződjön meg arról, hogy rendelkezik-e hello előtt végrehajtásának folytatásához a következő információkat.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-193">Ensure that you have hello following information before you proceed further.</span></span>
   
   * <span data-ttu-id="9dcd4-194">/ 30-as alhálózat hello elsődleges kapcsolathoz.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-194">A /30 subnet for hello primary link.</span></span> <span data-ttu-id="9dcd4-195">Ennek egy érvényes nyilvános IPv4-előtagnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-195">This must be a valid public IPv4 prefix.</span></span>
   * <span data-ttu-id="9dcd4-196">/ 30-as alhálózat hello másodlagos kapcsolathoz.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-196">A /30 subnet for hello secondary link.</span></span> <span data-ttu-id="9dcd4-197">Ennek egy érvényes nyilvános IPv4-előtagnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-197">This must be a valid public IPv4 prefix.</span></span>
   * <span data-ttu-id="9dcd4-198">Egy érvényes VLAN-azonosító tooestablish a társviszony.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-198">A valid VLAN ID tooestablish this peering on.</span></span> <span data-ttu-id="9dcd4-199">Győződjön meg arról, hogy egyetlen másik társviszonya se a hello áramkör használ hello azonos VLAN-azonosítót.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-199">Ensure that no other peering in hello circuit uses hello same VLAN ID.</span></span>
   * <span data-ttu-id="9dcd4-200">Egy AS-szám a társviszony-létesítéshez.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-200">AS number for peering.</span></span> <span data-ttu-id="9dcd4-201">2 és 4 bájtos AS-számokat is használhat.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-201">You can use both 2-byte and 4-byte AS numbers.</span></span>
   * <span data-ttu-id="9dcd4-202">Az MD5 kivonatoló, ha úgy dönt, hogy egy toouse.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-202">An MD5 hash if you choose toouse one.</span></span> <span data-ttu-id="9dcd4-203">**Ez nem kötelező**.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-203">**This is optional**.</span></span>
     
    <span data-ttu-id="9dcd4-204">A következő parancsmag tooconfigure Azure nyilvános társviszony-létesítés a kör hello futtatása</span><span class="sxs-lookup"><span data-stu-id="9dcd4-204">You can run hello following cmdlet tooconfigure Azure public peering for your circuit</span></span>
     
        <span data-ttu-id="9dcd4-205">Új AzureBGPPeering - AccessType nyilvános - ServiceKey "x" - PrimaryPeerSubnet "131.107.0.0/30" - SecondaryPeerSubnet "131.107.0.4/30" - PeerAsn 1234 - VlanId 200</span><span class="sxs-lookup"><span data-stu-id="9dcd4-205">New-AzureBGPPeering -AccessType Public -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -PeerAsn 1234 -VlanId 200</span></span>
     
    <span data-ttu-id="9dcd4-206">Ha úgy dönt, toouse az MD5 kivonatoló hello parancsmag az alábbi használható</span><span class="sxs-lookup"><span data-stu-id="9dcd4-206">You can use hello cmdlet below if you choose toouse an MD5 hash</span></span>
     
        <span data-ttu-id="9dcd4-207">Új AzureBGPPeering - AccessType nyilvános - ServiceKey "x" - PrimaryPeerSubnet "131.107.0.0/30" - SecondaryPeerSubnet "131.107.0.4/30" - PeerAsn 1234 - VlanId 200 - SharedKey "A1B2C3D4"</span><span class="sxs-lookup"><span data-stu-id="9dcd4-207">New-AzureBGPPeering -AccessType Public -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -PeerAsn 1234 -VlanId 200 -SharedKey "A1B2C3D4"</span></span>
     
     > [!IMPORTANT]
     > <span data-ttu-id="9dcd4-208">Az AS-számot mindenképp társviszony-létesítési ASN-ként, és ne ügyfél ASN-ként adja meg.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-208">Ensure that you specify your AS number as peering ASN and not customer ASN.</span></span>
     > 
     > 

### <a name="tooview-azure-public-peering-details"></a><span data-ttu-id="9dcd4-209">tooview Azure nyilvános társviszony-létesítés részletei</span><span class="sxs-lookup"><span data-stu-id="9dcd4-209">tooview Azure public peering details</span></span>
<span data-ttu-id="9dcd4-210">Konfigurációs részletekért hello a következő parancsmag használatával</span><span class="sxs-lookup"><span data-stu-id="9dcd4-210">You can get configuration details using hello following cmdlet</span></span>

    Get-AzureBGPPeering -AccessType Public -ServiceKey "*********************************"

    AdvertisedPublicPrefixes       : 
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 
    PeerAsn                        : 1234
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 131.107.0.0/30
    RoutingRegistryName            : 
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 131.107.0.4/30
    State                          : Enabled
    VlanId                         : 200


### <a name="tooupdate-azure-public-peering-configuration"></a><span data-ttu-id="9dcd4-211">az Azure nyilvános társviszony-létesítési konfiguráció tooupdate</span><span class="sxs-lookup"><span data-stu-id="9dcd4-211">tooupdate Azure public peering configuration</span></span>
<span data-ttu-id="9dcd4-212">Frissítheti a hello konfigurációját a következő parancsmag hello segítségével valamely része</span><span class="sxs-lookup"><span data-stu-id="9dcd4-212">You can update any part of hello configuration using hello following cmdlet</span></span>

    Set-AzureBGPPeering -AccessType Public -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -PeerAsn 1234 -VlanId 600 -SharedKey "A1B2C3D4"

<span data-ttu-id="9dcd4-213">a fenti példa hello 200 too600 hello VLAN-Azonosítót a kapcsolatcsoport hello alatt módosul.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-213">hello VLAN ID of hello circuit is being updated from 200 too600 in hello above example.</span></span>

### <a name="toodelete-azure-public-peering"></a><span data-ttu-id="9dcd4-214">az Azure nyilvános társviszony toodelete</span><span class="sxs-lookup"><span data-stu-id="9dcd4-214">toodelete Azure public peering</span></span>
<span data-ttu-id="9dcd4-215">Eltávolíthatja a társviszony-létesítési konfiguráció hello a következő parancsmag futtatásával</span><span class="sxs-lookup"><span data-stu-id="9dcd4-215">You can remove your peering configuration by running hello following cmdlet</span></span>

    Remove-AzureBGPPeering -AccessType Public -ServiceKey "*********************************"

## <a name="microsoft-peering"></a><span data-ttu-id="9dcd4-216">Microsoft társviszony-létesítés</span><span class="sxs-lookup"><span data-stu-id="9dcd4-216">Microsoft peering</span></span>
<span data-ttu-id="9dcd4-217">Ez a szakasz útmutatás hogyan toocreate, beolvasása, frissítése és törlése hello Microsoft társviszony-létesítési konfiguráció az ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-217">This section provides instructions on how toocreate, get, update and delete hello Microsoft peering configuration for an ExpressRoute circuit.</span></span> 

### <a name="toocreate-microsoft-peering"></a><span data-ttu-id="9dcd4-218">toocreate Microsoft társviszony-létesítés</span><span class="sxs-lookup"><span data-stu-id="9dcd4-218">toocreate Microsoft peering</span></span>
1. <span data-ttu-id="9dcd4-219">**ExpressRoute hello PowerShell modul importálása.**</span><span class="sxs-lookup"><span data-stu-id="9dcd4-219">**Import hello PowerShell module for ExpressRoute.**</span></span>
   
    <span data-ttu-id="9dcd4-220">Hello Azure és az ExpressRoute modulok importálni kell az hello PowerShell-munkamenetet a rendelés toostart hello ExpressRoute-parancsmagok használatával.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-220">You must import hello Azure and ExpressRoute modules into hello PowerShell session in order toostart using hello ExpressRoute cmdlets.</span></span> <span data-ttu-id="9dcd4-221">Futtatási hello következő tooimport hello Azure és az ExpressRoute-modulok parancsokat hello PowerShell-munkamenetbe.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-221">Run hello following commands tooimport hello Azure and ExpressRoute modules into hello PowerShell session.</span></span>  
   
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'
2. <span data-ttu-id="9dcd4-222">**ExpressRoute-kapcsolatcsoport létrehozása**</span><span class="sxs-lookup"><span data-stu-id="9dcd4-222">**Create an ExpressRoute circuit**</span></span>
   
    <span data-ttu-id="9dcd4-223">Kövesse az utasításokat toocreate hello egy [ExpressRoute-kapcsolatcsoportot](expressroute-howto-circuit-classic.md) , illetve hozta-e hello kapcsolat szolgáltatóját.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-223">Follow hello instructions toocreate an [ExpressRoute circuit](expressroute-howto-circuit-classic.md) and have it provisioned by hello connectivity provider.</span></span> <span data-ttu-id="9dcd4-224">Ha a kapcsolat szolgáltatójánál 3. rétegbeli felügyelt szolgáltatásokat kínál, a kapcsolat szolgáltató tooenable magánhálózati társviszony-létesítést az Ön Azure kérhet.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-224">If your connectivity provider offers managed Layer 3 services, you can request your connectivity provider tooenable Azure private peering for you.</span></span> <span data-ttu-id="9dcd4-225">Ebben az esetben nincs szükség hello következő szakaszokban szereplő toofollow utasításokat.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-225">In that case, you won't need toofollow instructions listed in hello next sections.</span></span> <span data-ttu-id="9dcd4-226">Ha a kapcsolat szolgáltatójánál nem kezeli az útválasztást, a kapcsolat létrehozása után azonban kövesse az alábbi hello-utasításokat.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-226">However, if your connectivity provider does not manage routing for you, after creating your circuit, follow hello instructions below.</span></span>
3. <span data-ttu-id="9dcd4-227">**Ellenőrizze a ExpressRoute-kapcsolatcsoport tooensure ki van építve**</span><span class="sxs-lookup"><span data-stu-id="9dcd4-227">**Check ExpressRoute circuit tooensure it is provisioned**</span></span>
   
    <span data-ttu-id="9dcd4-228">Ha ExpressRoute-kapcsolatcsoportot hello kiépítve és engedélyezett állapotban van, először ellenőrizze toosee.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-228">You must first check toosee if hello ExpressRoute circuit is in Provisioned and Enabled state.</span></span>
   
        PS C:\> Get-AzureDedicatedCircuit -ServiceKey "*********************************"
   
        Bandwidth                        : 200
        CircuitName                      : MyTestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled
   
    <span data-ttu-id="9dcd4-229">Győződjön meg arról, hogy látható-e hello áramkör kiépítve és engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-229">Make sure that hello circuit shows as Provisioned and Enabled.</span></span> <span data-ttu-id="9dcd4-230">Ha nem, dolgozni a kapcsolat szolgáltató tooget a kapcsolatcsoport szükséges toohello állapotát és állapotát.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-230">If it doesn't, work with your connectivity provider tooget your circuit toohello required state and status.</span></span>
   
        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled
4. <span data-ttu-id="9dcd4-231">**A Microsoft hello-kör társviszony konfigurálása**</span><span class="sxs-lookup"><span data-stu-id="9dcd4-231">**Configure Microsoft peering for hello circuit**</span></span>
   
    <span data-ttu-id="9dcd4-232">Győződjön meg arról, hogy rendelkezik-e folytatni a következő információk előtt hello.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-232">Make sure that you have hello following information before you proceed.</span></span>
   
   * <span data-ttu-id="9dcd4-233">/ 30-as alhálózat hello elsődleges kapcsolathoz.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-233">A /30 subnet for hello primary link.</span></span> <span data-ttu-id="9dcd4-234">Ennek egy érvényes nyilvános IPv4-előtagnak kell lennie, amely az Ön tulajdonában van, és regisztrálva van egy RIR-/IRR-jegyzékben.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-234">This must be a valid public IPv4 prefix owned by you and registered in an RIR / IRR.</span></span>
   * <span data-ttu-id="9dcd4-235">/ 30-as alhálózat hello másodlagos kapcsolathoz.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-235">A /30 subnet for hello secondary link.</span></span> <span data-ttu-id="9dcd4-236">Ennek egy érvényes nyilvános IPv4-előtagnak kell lennie, amely az Ön tulajdonában van, és regisztrálva van egy RIR-/IRR-jegyzékben.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-236">This must be a valid public IPv4 prefix owned by you and registered in an RIR / IRR.</span></span>
   * <span data-ttu-id="9dcd4-237">Egy érvényes VLAN-azonosító tooestablish a társviszony.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-237">A valid VLAN ID tooestablish this peering on.</span></span> <span data-ttu-id="9dcd4-238">Győződjön meg arról, hogy egyetlen másik társviszonya se a hello áramkör használ hello azonos VLAN-azonosítót.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-238">Ensure that no other peering in hello circuit uses hello same VLAN ID.</span></span>
   * <span data-ttu-id="9dcd4-239">Egy AS-szám a társviszony-létesítéshez.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-239">AS number for peering.</span></span> <span data-ttu-id="9dcd4-240">2 és 4 bájtos AS-számokat is használhat.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-240">You can use both 2-byte and 4-byte AS numbers.</span></span>
   * <span data-ttu-id="9dcd4-241">Meghirdetett előtagok: meg kell adnia a lista az összes előtagok hello BGP munkameneten keresztül tervezi tooadvertise.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-241">Advertised prefixes: You must provide a list of all prefixes you plan tooadvertise over hello BGP session.</span></span> <span data-ttu-id="9dcd4-242">A rendszer kizárólag a nyilvános IP-cím-előtagokat fogadja el.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-242">Only public IP address prefixes are accepted.</span></span> <span data-ttu-id="9dcd4-243">Ha azt tervezi, hogy toosend előtagok készlete küldhet egy vesszővel elválasztott listában.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-243">You can send a comma separated list if you plan toosend a set of prefixes.</span></span> <span data-ttu-id="9dcd4-244">Ezeket az előtagokat kell lenniük egy RIR-ben regisztrált tooyou vagy IRR-ben.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-244">These prefixes must be registered tooyou in an RIR / IRR.</span></span>
   * <span data-ttu-id="9dcd4-245">Ügyfél ASN: Ha-előtagok nem regisztrált toohello társviszony-létesítés SZÁMOT vannak hirdetményt forgalmaz, megadhatja hello számú toowhich regisztrálva vannak.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-245">Customer ASN: If you are advertising prefixes that are not registered toohello peering AS number, you can specify hello AS number toowhich they are registered.</span></span> <span data-ttu-id="9dcd4-246">**Ez nem kötelező**.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-246">**This is optional**.</span></span>
   * <span data-ttu-id="9dcd4-247">Útválasztási beállításjegyzék-név: Hello RIR megadhat / BMR mely hello ellen, számát, és előtagok regisztrálva van.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-247">Routing Registry Name: You can specify hello RIR / IRR against which hello AS number and prefixes are registered.</span></span>
   * <span data-ttu-id="9dcd4-248">Az MD5 kivonatoló, ha úgy dönt, hogy egy toouse.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-248">An MD5 hash, if you choose toouse one.</span></span> <span data-ttu-id="9dcd4-249">**Ez nem kötelező.**</span><span class="sxs-lookup"><span data-stu-id="9dcd4-249">**This is optional.**</span></span>
     
    <span data-ttu-id="9dcd4-250">A következő parancsmag tooconfigure Microsoft pering a kapcsolatcsoport hello futtatása</span><span class="sxs-lookup"><span data-stu-id="9dcd4-250">You can run hello following cmdlet tooconfigure Microsoft pering for your circuit</span></span>
     
        <span data-ttu-id="9dcd4-251">Új AzureBGPPeering - AccessType Microsoft - ServiceKey "x" - PrimaryPeerSubnet "131.107.0.0/30" - SecondaryPeerSubnet "131.107.0.4/30" - VlanId 300 - PeerAsn 1234 - CustomerAsn 2245 - AdvertisedPublicPrefixes " 123.0.0.0/30 "- RoutingRegistryName"ARIN"- SharedKey"A1B2C3D4"</span><span class="sxs-lookup"><span data-stu-id="9dcd4-251">New-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -VlanId 300 -PeerAsn 1234 -CustomerAsn 2245 -AdvertisedPublicPrefixes "123.0.0.0/30" -RoutingRegistryName "ARIN" -SharedKey "A1B2C3D4"</span></span>

### <a name="tooview-microsoft-peering-details"></a><span data-ttu-id="9dcd4-252">tooview Microsoft társviszony-létesítési részletei</span><span class="sxs-lookup"><span data-stu-id="9dcd4-252">tooview Microsoft peering details</span></span>
<span data-ttu-id="9dcd4-253">Konfigurációs részletek hello a következő parancsmag használatával kérheti le.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-253">You can get configuration details using hello following cmdlet.</span></span>

    Get-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************"

    AdvertisedPublicPrefixes       : 123.0.0.0/30
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 2245
    PeerAsn                        : 1234
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 10.0.0.0/30
    RoutingRegistryName            : ARIN
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 10.0.0.4/30
    State                          : Enabled
    VlanId                         : 300


### <a name="tooupdate-microsoft-peering-configuration"></a><span data-ttu-id="9dcd4-254">a Microsoft társviszony-létesítési konfiguráció tooupdate</span><span class="sxs-lookup"><span data-stu-id="9dcd4-254">tooupdate Microsoft peering configuration</span></span>
<span data-ttu-id="9dcd4-255">Frissítheti, használja a következő parancsmag hello hello konfiguráció bármely részeként.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-255">You can update any part of hello configuration using hello following cmdlet.</span></span>

    Set-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -VlanId 300 -PeerAsn 1234 -CustomerAsn 2245 -AdvertisedPublicPrefixes "123.0.0.0/30" -RoutingRegistryName "ARIN" -SharedKey "A1B2C3D4"

### <a name="toodelete-microsoft-peering"></a><span data-ttu-id="9dcd4-256">toodelete Microsoft társviszony-létesítés</span><span class="sxs-lookup"><span data-stu-id="9dcd4-256">toodelete Microsoft peering</span></span>
<span data-ttu-id="9dcd4-257">Eltávolíthatja a társviszony-létesítési konfiguráció hello a következő parancsmag futtatásával.</span><span class="sxs-lookup"><span data-stu-id="9dcd4-257">You can remove your peering configuration by running hello following cmdlet.</span></span>

    Remove-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************"

## <a name="next-steps"></a><span data-ttu-id="9dcd4-258">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9dcd4-258">Next steps</span></span>
<span data-ttu-id="9dcd4-259">Ezt követően [csatolni a virtuális hálózat tooan ExpressRoute-kapcsolatcsoportot](expressroute-howto-linkvnet-classic.md).</span><span class="sxs-lookup"><span data-stu-id="9dcd4-259">Next, [Link a VNet tooan ExpressRoute circuit](expressroute-howto-linkvnet-classic.md).</span></span>

* <span data-ttu-id="9dcd4-260">Munkafolyamatokkal kapcsolatos további információkért lásd: [ExpressRoute-munkafolyamatokat](expressroute-workflows.md).</span><span class="sxs-lookup"><span data-stu-id="9dcd4-260">For more information about workflows, see [ExpressRoute workflows](expressroute-workflows.md).</span></span>
* <span data-ttu-id="9dcd4-261">A kapcsolatcsoportok társviszony-létesítéseivel kapcsolatos további információkért lásd: [ExpressRoute circuits and routing domains](expressroute-circuit-peerings.md) (ExpressRoute-kapcsolatcsoportok és útválasztási tartományok).</span><span class="sxs-lookup"><span data-stu-id="9dcd4-261">For more information about circuit peering, see [ExpressRoute circuits and routing domains](expressroute-circuit-peerings.md).</span></span>

