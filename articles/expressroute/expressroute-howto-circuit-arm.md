---
title: "Létrehozásához és módosításához ExpressRoute-kapcsolatcsoportot: PowerShell: Azure Resource Manager |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan toocreate, kiépítéséhez, győződjön meg arról, frissítése, törlése és kiosztásának megszüntetése ExpressRoute-kapcsolatcsoportot."
documentationcenter: na
services: expressroute
author: ganesr
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f997182e-9b25-4a7a-b079-b004221dadcc
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/12/2017
ms.author: ganesr;cherylmc
ms.openlocfilehash: 8d76c577a9cffdd393abac1b76cccc27d92e9e62
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-modify-an-expressroute-circuit-using-powershell"></a><span data-ttu-id="1470e-103">Létrehozása és módosítása a powershellel ExpressRoute-kapcsolatcsoportot</span><span class="sxs-lookup"><span data-stu-id="1470e-103">Create and modify an ExpressRoute circuit using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="1470e-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="1470e-104">Azure portal</span></span>](expressroute-howto-circuit-portal-resource-manager.md)
> * [<span data-ttu-id="1470e-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="1470e-105">PowerShell</span></span>](expressroute-howto-circuit-arm.md)
> * [<span data-ttu-id="1470e-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="1470e-106">Azure CLI</span></span>](howto-circuit-cli.md)
> * [<span data-ttu-id="1470e-107">Video - Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="1470e-107">Video - Azure portal</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [<span data-ttu-id="1470e-108">PowerShell (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="1470e-108">PowerShell (classic)</span></span>](expressroute-howto-circuit-classic.md)
>

<span data-ttu-id="1470e-109">Ez a cikk ismerteti, hogyan toocreate egy Azure ExpressRoute áramkör PowerShell-parancsmagok és hello Azure Resource Manager telepítési modell használatával.</span><span class="sxs-lookup"><span data-stu-id="1470e-109">This article describes how toocreate an Azure ExpressRoute circuit by using PowerShell cmdlets and hello Azure Resource Manager deployment model.</span></span> <span data-ttu-id="1470e-110">Ez a cikk hogyan hello kapcsolatcsoport állapotának toocheck hello a frissítést, vagy törölje és kiosztásának megszüntetése azt is bemutatja.</span><span class="sxs-lookup"><span data-stu-id="1470e-110">This article also shows you how toocheck hello status of hello circuit, update it, or delete and deprovision it.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="1470e-111">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="1470e-111">Before you begin</span></span>
* <span data-ttu-id="1470e-112">Hello hello Azure Resource Manager PowerShell-parancsmagok legújabb verziójának telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="1470e-112">Install hello latest version of hello Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="1470e-113">További információkért lásd: [áttekintés az Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="1470e-113">For more information, see [Overview of Azure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="1470e-114">Felülvizsgálati hello [Előfeltételek](expressroute-prerequisites.md) és [munkafolyamatok](expressroute-workflows.md) konfigurálás elkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="1470e-114">Review hello [prerequisites](expressroute-prerequisites.md) and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>


## <a name="create-and-provision-an-expressroute-circuit"></a><span data-ttu-id="1470e-115">Hozzon létre, és helyezze üzembe az ExpressRoute-kapcsolatcsoportot</span><span class="sxs-lookup"><span data-stu-id="1470e-115">Create and provision an ExpressRoute circuit</span></span>
### <a name="1-sign-in-tooyour-azure-account-and-select-your-subscription"></a><span data-ttu-id="1470e-116">1. Jelentkezzen be Azure-fiók tooyour, és jelölje ki az előfizetését</span><span class="sxs-lookup"><span data-stu-id="1470e-116">1. Sign in tooyour Azure account and select your subscription</span></span>
<span data-ttu-id="1470e-117">toobegin a konfigurációt, jelentkezzen be Azure-fiók tooyour.</span><span class="sxs-lookup"><span data-stu-id="1470e-117">toobegin your configuration, sign in tooyour Azure account.</span></span> <span data-ttu-id="1470e-118">A következő példák toohelp csatlakozás hello használata:</span><span class="sxs-lookup"><span data-stu-id="1470e-118">Use hello following examples toohelp you connect:</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="1470e-119">Ellenőrizze a hello előfizetések hello fiók:</span><span class="sxs-lookup"><span data-stu-id="1470e-119">Check hello subscriptions for hello account:</span></span>

```powershell
Get-AzureRmSubscription
```

<span data-ttu-id="1470e-120">Válassza ki egy ExpressRoute-áramkör a toocreate hello előfizetést:</span><span class="sxs-lookup"><span data-stu-id="1470e-120">Select hello subscription that you want toocreate an ExpressRoute circuit for:</span></span>

```powershell
Select-AzureRmSubscription -SubscriptionId "<subscription ID>"
```

### <a name="2-get-hello-list-of-supported-providers-locations-and-bandwidths"></a><span data-ttu-id="1470e-121">2. Támogatott szolgáltatók, a helyek és a sávszélesség hello listájának beolvasása</span><span class="sxs-lookup"><span data-stu-id="1470e-121">2. Get hello list of supported providers, locations, and bandwidths</span></span>
<span data-ttu-id="1470e-122">ExpressRoute-kapcsolatcsoportot létrehozni, meg kell hello támogatott kapcsolat szolgáltatókat, a helyek és a sávszélesség-beállítások listája.</span><span class="sxs-lookup"><span data-stu-id="1470e-122">Before you create an ExpressRoute circuit, you need hello list of supported connectivity providers, locations, and bandwidth options.</span></span>

<span data-ttu-id="1470e-123">PowerShell-parancsmag hello **Get-AzureRmExpressRouteServiceProvider** adja vissza ezt az információt fogja használni a későbbi lépésekben:</span><span class="sxs-lookup"><span data-stu-id="1470e-123">hello PowerShell cmdlet **Get-AzureRmExpressRouteServiceProvider** returns this information, which you’ll use in later steps:</span></span>

```powershell
Get-AzureRmExpressRouteServiceProvider
```

<span data-ttu-id="1470e-124">Ellenőrizze a toosee, ha a kapcsolat szolgáltatójánál van szó.</span><span class="sxs-lookup"><span data-stu-id="1470e-124">Check toosee if your connectivity provider is listed there.</span></span> <span data-ttu-id="1470e-125">Jegyezze fel a következő információ hello.</span><span class="sxs-lookup"><span data-stu-id="1470e-125">Make a note of hello following information.</span></span> <span data-ttu-id="1470e-126">Azt később szüksége expressroute-kapcsolatcsoporthoz létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="1470e-126">You'll need it later when you create a circuit.</span></span>

* <span data-ttu-id="1470e-127">Név</span><span class="sxs-lookup"><span data-stu-id="1470e-127">Name</span></span>
* <span data-ttu-id="1470e-128">PeeringLocations</span><span class="sxs-lookup"><span data-stu-id="1470e-128">PeeringLocations</span></span>
* <span data-ttu-id="1470e-129">BandwidthsOffered</span><span class="sxs-lookup"><span data-stu-id="1470e-129">BandwidthsOffered</span></span>

<span data-ttu-id="1470e-130">Most már készen áll a toocreate ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="1470e-130">You're now ready toocreate an ExpressRoute circuit.</span></span>   

### <a name="3-create-an-expressroute-circuit"></a><span data-ttu-id="1470e-131">3. ExpressRoute-kapcsolatcsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="1470e-131">3. Create an ExpressRoute circuit</span></span>
<span data-ttu-id="1470e-132">Ha még nem rendelkezik egy erőforráscsoport, kell létrehoznia egyet az ExpressRoute-kapcsolatcsoportot létrehozása előtt.</span><span class="sxs-lookup"><span data-stu-id="1470e-132">If you don't already have a resource group, you must create one before you create your ExpressRoute circuit.</span></span> <span data-ttu-id="1470e-133">Ezt hello a következő parancs futtatásával teheti meg:</span><span class="sxs-lookup"><span data-stu-id="1470e-133">You can do so by running hello following command:</span></span>

```powershell
New-AzureRmResourceGroup -Name "ExpressRouteResourceGroup" -Location "West US"
```


<span data-ttu-id="1470e-134">hello a következő példa bemutatja, hogyan toocreate egy 200 MB/s ExpressRoute áramkör a szilícium Valley Equinix keresztül.</span><span class="sxs-lookup"><span data-stu-id="1470e-134">hello following example shows how toocreate a 200-Mbps ExpressRoute circuit through Equinix in Silicon Valley.</span></span> <span data-ttu-id="1470e-135">Különböző szolgáltatók és más beállítások használata, amikor a kérést helyettesítse be ezt az információt.</span><span class="sxs-lookup"><span data-stu-id="1470e-135">If you're using a different provider and different settings, substitute that information when you make your request.</span></span> <span data-ttu-id="1470e-136">hello az alábbiakban látható egy példa egy kérelem egy új szolgáltatás kulcs:</span><span class="sxs-lookup"><span data-stu-id="1470e-136">hello following is an example request for a new service key:</span></span>

```powershell
New-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup" -Location "West US" -SkuTier Standard -SkuFamily MeteredData -ServiceProviderName "Equinix" -PeeringLocation "Silicon Valley" -BandwidthInMbps 200
```

<span data-ttu-id="1470e-137">Győződjön meg arról, hogy megadja hello megfelelő SKU réteg és SKU-család:</span><span class="sxs-lookup"><span data-stu-id="1470e-137">Make sure that you specify hello correct SKU tier and SKU family:</span></span>

* <span data-ttu-id="1470e-138">Termékváltozat szint határozza meg, egy ExpressRoute standard vagy ExpressRoute prémium bővítmény engedélyezve van-e.</span><span class="sxs-lookup"><span data-stu-id="1470e-138">SKU tier determines whether an ExpressRoute standard or an ExpressRoute premium add-on is enabled.</span></span> <span data-ttu-id="1470e-139">Megadhat *szabványos* tooget hello standard Termékváltozat vagy *prémium* hello prémium bővítménnyel.</span><span class="sxs-lookup"><span data-stu-id="1470e-139">You can specify *Standard* tooget hello standard SKU or *Premium* for hello premium add-on.</span></span>
* <span data-ttu-id="1470e-140">SKU-család hello számlázási típusa határozza meg.</span><span class="sxs-lookup"><span data-stu-id="1470e-140">SKU family determines hello billing type.</span></span> <span data-ttu-id="1470e-141">Megadhat *Metereddata* díjköteles adatforgalom tervhez és *Unlimiteddata* egy adatforgalmi a.</span><span class="sxs-lookup"><span data-stu-id="1470e-141">You can specify *Metereddata* for a metered data plan and *Unlimiteddata* for an unlimited data plan.</span></span> <span data-ttu-id="1470e-142">Módosíthatja a számlázási típusának hello *Metereddata* túl*Unlimiteddata*, de nem módosíthatja a hello típusát *Unlimiteddata* túl*Metereddata *.</span><span class="sxs-lookup"><span data-stu-id="1470e-142">You can change hello billing type from *Metereddata* too*Unlimiteddata*, but you can't change hello type from *Unlimiteddata* too*Metereddata*.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1470e-143">Az ExpressRoute-kapcsolatcsoportot jelenik meg a szolgáltatási kulcs hello pillanattól lesz terhelve.</span><span class="sxs-lookup"><span data-stu-id="1470e-143">Your ExpressRoute circuit will be billed from hello moment a service key is issued.</span></span> <span data-ttu-id="1470e-144">Győződjön meg arról, hogy készen áll a tooprovision hello áramkör hello kapcsolat szolgáltatójánál esetén elvégzi ezt a műveletet.</span><span class="sxs-lookup"><span data-stu-id="1470e-144">Ensure that you perform this operation when hello connectivity provider is ready tooprovision hello circuit.</span></span>
> 
> 

<span data-ttu-id="1470e-145">hello válasz hello szolgáltatás kulcsot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="1470e-145">hello response contains hello service key.</span></span> <span data-ttu-id="1470e-146">Részletes leírását, az összes hello paraméterek kaphat hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="1470e-146">You can get detailed descriptions of all hello parameters by running hello following command:</span></span>

```powershell
get-help New-AzureRmExpressRouteCircuit -detailed
```


### <a name="4-list-all-expressroute-circuits"></a><span data-ttu-id="1470e-147">4. A lista összes ExpressRoute-Kapcsolatcsoportok</span><span class="sxs-lookup"><span data-stu-id="1470e-147">4. List all ExpressRoute circuits</span></span>
<span data-ttu-id="1470e-148">az összes hello hozott létre, ExpressRoute-Kapcsolatcsoportok tooget futtatása hello **Get-AzureRmExpressRouteCircuit** parancs:</span><span class="sxs-lookup"><span data-stu-id="1470e-148">tooget a list of all hello ExpressRoute circuits that you created, run hello **Get-AzureRmExpressRouteCircuit** command:</span></span>

```powershell
Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
```

<span data-ttu-id="1470e-149">hello válasz a következő példa hasonló toohello fog kinézni:</span><span class="sxs-lookup"><span data-stu-id="1470e-149">hello response will look similar toohello following example:</span></span>

    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                          }
    CircuitProvisioningState          : Enabled
    ServiceProviderProvisioningState  : NotProvisioned
    ServiceProviderNotes              :
    ServiceProviderProperties         : {
                                          "ServiceProviderName": "Equinix",
                                          "PeeringLocation": "Silicon Valley",
                                          "BandwidthInMbps": 200
                                        }
    ServiceKey                        : **************************************
    Peerings                          : []

<span data-ttu-id="1470e-150">Ezt az információt bármikor hello használatával lekérhető `Get-AzureRmExpressRouteCircuit` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="1470e-150">You can retrieve this information at any time by using hello `Get-AzureRmExpressRouteCircuit` cmdlet.</span></span> <span data-ttu-id="1470e-151">Összes hello áramkör paraméter nélküli hívás hello így sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="1470e-151">Making hello call with no parameters lists all hello circuits.</span></span> <span data-ttu-id="1470e-152">A szolgáltatás kulcs megjelenik hello *ServiceKey* mező:</span><span class="sxs-lookup"><span data-stu-id="1470e-152">Your service key will be listed in hello *ServiceKey* field:</span></span>

```powershell
Get-AzureRmExpressRouteCircuit
```


<span data-ttu-id="1470e-153">hello válasz a következő példa hasonló toohello fog kinézni:</span><span class="sxs-lookup"><span data-stu-id="1470e-153">hello response will look similar toohello following example:</span></span>

    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                          }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : NotProvisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                         "ServiceProviderName": "Equinix",
                                         "PeeringLocation": "Silicon Valley",
                                         "BandwidthInMbps": 200
                                          }
    ServiceKey                       : **************************************
    Peerings                         : []


<span data-ttu-id="1470e-154">Részletes leírását, az összes hello paraméterek kaphat hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="1470e-154">You can get detailed descriptions of all hello parameters by running hello following command:</span></span>

```powershell
get-help Get-AzureRmExpressRouteCircuit -detailed
```

### <a name="5-send-hello-service-key-tooyour-connectivity-provider-for-provisioning"></a><span data-ttu-id="1470e-155">5. Kapcsolat kulcs tooyour hello szolgáltató küldése történő üzembe helyezéséhez</span><span class="sxs-lookup"><span data-stu-id="1470e-155">5. Send hello service key tooyour connectivity provider for provisioning</span></span>
<span data-ttu-id="1470e-156">*ServiceProviderProvisioningState* hello aktuális állapotának hello szolgáltatói oldalán kiépítési információkat nyújt.</span><span class="sxs-lookup"><span data-stu-id="1470e-156">*ServiceProviderProvisioningState* provides information about hello current state of provisioning on hello service-provider side.</span></span> <span data-ttu-id="1470e-157">Állapot hello állapot hello Microsoft ügyféloldali biztosít.</span><span class="sxs-lookup"><span data-stu-id="1470e-157">Status provides hello state on hello Microsoft side.</span></span> <span data-ttu-id="1470e-158">Kiépítés állapotok áramkör kapcsolatos további információkért lásd: hello [munkafolyamatok](expressroute-workflows.md#expressroute-circuit-provisioning-states) cikk.</span><span class="sxs-lookup"><span data-stu-id="1470e-158">For more information about circuit provisioning states, see hello [Workflows](expressroute-workflows.md#expressroute-circuit-provisioning-states) article.</span></span>

<span data-ttu-id="1470e-159">Amikor létrehoz egy új ExpressRoute-kapcsolatcsoportot, hello áramkör hello állapota a következő lehet:</span><span class="sxs-lookup"><span data-stu-id="1470e-159">When you create a new ExpressRoute circuit, hello circuit will be in hello following state:</span></span>

    ServiceProviderProvisioningState : NotProvisioned
    CircuitProvisioningState         : Enabled



<span data-ttu-id="1470e-160">hello áramkör toohello hello folyamatán, amely lehetővé teszi a hello kapcsolat hitelesítésszolgáltató esetén a következő állapota változik:</span><span class="sxs-lookup"><span data-stu-id="1470e-160">hello circuit will change toohello following state when hello connectivity provider is in hello process of enabling it for you:</span></span>

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled

<span data-ttu-id="1470e-161">Az Ön toobe képes toouse ExpressRoute-kapcsolatcsoportot az állapot a következő hello kell lennie:</span><span class="sxs-lookup"><span data-stu-id="1470e-161">For you toobe able toouse an ExpressRoute circuit, it must be in hello following state:</span></span>

    ServiceProviderProvisioningState : Provisioned
    CircuitProvisioningState         : Enabled

### <a name="6-periodically-check-hello-status-and-hello-state-of-hello-circuit-key"></a><span data-ttu-id="1470e-162">6. Rendszeres időközönként ellenőrizze a hello állapotát és hello áramkör kulcs hello állapota</span><span class="sxs-lookup"><span data-stu-id="1470e-162">6. Periodically check hello status and hello state of hello circuit key</span></span>
<span data-ttu-id="1470e-163">Hello állapotát és hello áramkör kulcs hello állapotának ellenőrzése lehetővé teszi a szolgáltató a kapcsolatcsoport engedélyezésekor.</span><span class="sxs-lookup"><span data-stu-id="1470e-163">Checking hello status and hello state of hello circuit key lets you know when your provider has enabled your circuit.</span></span> <span data-ttu-id="1470e-164">Hello áramkör konfigurálása után *ServiceProviderProvisioningState* jelenik meg *kiépítve*, ahogy az alábbi példa hello:</span><span class="sxs-lookup"><span data-stu-id="1470e-164">After hello circuit has been configured, *ServiceProviderProvisioningState* appears as *Provisioned*, as shown in hello following example:</span></span>

```powershell
Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
```


<span data-ttu-id="1470e-165">hello válasz a következő példa hasonló toohello fog kinézni:</span><span class="sxs-lookup"><span data-stu-id="1470e-165">hello response will look similar toohello following example:</span></span>

    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                       }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                         "ServiceProviderName": "Equinix",
                                         "PeeringLocation": "Silicon Valley",
                                         "BandwidthInMbps": 200
                                       }
    ServiceKey                       : **************************************
    Peerings                         : []

### <a name="7-create-your-routing-configuration"></a><span data-ttu-id="1470e-166">7. Az útválasztó-konfiguráció létrehozása</span><span class="sxs-lookup"><span data-stu-id="1470e-166">7. Create your routing configuration</span></span>
<span data-ttu-id="1470e-167">Részletes útmutatásért lásd: hello [ExpressRoute-áramkör útválasztási konfigurációja](expressroute-howto-routing-arm.md) toocreate a következő cikket, és módosítsa a kapcsolatcsoport esetében.</span><span class="sxs-lookup"><span data-stu-id="1470e-167">For step-by-step instructions, see hello [ExpressRoute circuit routing configuration](expressroute-howto-routing-arm.md) article toocreate and modify circuit peerings.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1470e-168">Ezek az utasítások csak a szolgáltatók által biztosított réteg 2 internetkapcsolati szolgáltatás használatával létrehozott toocircuits vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="1470e-168">These instructions only apply toocircuits that are created with service providers that offer layer 2 connectivity services.</span></span> <span data-ttu-id="1470e-169">A szolgáltató által kezelt használata réteg (általában az IP VPN, például az MPLS) 3 szolgáltatások, a kapcsolat szolgáltatójánál konfigurálása és kezelése az Ön útválasztást.</span><span class="sxs-lookup"><span data-stu-id="1470e-169">If you're using a service provider that offers managed layer 3 services (typically an IP VPN, like MPLS), your connectivity provider will configure and manage routing for you.</span></span>
> 
> 

### <a name="8-link-a-virtual-network-tooan-expressroute-circuit"></a><span data-ttu-id="1470e-170">8. Hivatkozásra egy virtuális hálózati tooan ExpressRoute-kapcsolatcsoportot</span><span class="sxs-lookup"><span data-stu-id="1470e-170">8. Link a virtual network tooan ExpressRoute circuit</span></span>
<span data-ttu-id="1470e-171">A következő hivatkozás egy virtuális hálózati tooyour ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="1470e-171">Next, link a virtual network tooyour ExpressRoute circuit.</span></span> <span data-ttu-id="1470e-172">Használjon hello [csatolása a virtuális hálózatok tooExpressRoute Kapcsolatcsoportok](expressroute-howto-linkvnet-arm.md) hello Resource Manager üzembe helyezési modellben való munka során a következő cikket.</span><span class="sxs-lookup"><span data-stu-id="1470e-172">Use hello [Linking virtual networks tooExpressRoute circuits](expressroute-howto-linkvnet-arm.md) article when you work with hello Resource Manager deployment model.</span></span>

## <a name="getting-hello-status-of-an-expressroute-circuit"></a><span data-ttu-id="1470e-173">Egy ExpressRoute-kapcsolatcsoportot hello állapotának beolvasása</span><span class="sxs-lookup"><span data-stu-id="1470e-173">Getting hello status of an ExpressRoute circuit</span></span>
<span data-ttu-id="1470e-174">Ezt az információt bármikor hello használatával lekérhető **Get-AzureRmExpressRouteCircuit** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="1470e-174">You can retrieve this information at any time by using hello **Get-AzureRmExpressRouteCircuit** cmdlet.</span></span> <span data-ttu-id="1470e-175">Összes hello áramkör paraméter nélküli hívás hello így sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="1470e-175">Making hello call with no parameters lists all hello circuits.</span></span>

```powershell
Get-AzureRmExpressRouteCircuit
```


<span data-ttu-id="1470e-176">hello válasz hasonló toohello, például a következő lesz:</span><span class="sxs-lookup"><span data-stu-id="1470e-176">hello response will be similar toohello following example:</span></span>

    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                       }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                            "ServiceProviderName": "Equinix",
                                            "PeeringLocation": "Silicon Valley",
                                            "BandwidthInMbps": 200
                                          }
    ServiceKey                       : **************************************
    Peerings                         : []


<span data-ttu-id="1470e-177">Egy adott ExpressRoute-kapcsolatcsoportot tájékoztatást kaphat a paraméter toohello hívásként hello erőforráscsoport-név és a kapcsolat neve átadásával:</span><span class="sxs-lookup"><span data-stu-id="1470e-177">You can get information on a specific ExpressRoute circuit by passing hello resource group name and circuit name as a parameter toohello call:</span></span>

```powershell
Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
```


<span data-ttu-id="1470e-178">hello válasz a következő példa hasonló toohello fog kinézni:</span><span class="sxs-lookup"><span data-stu-id="1470e-178">hello response will look similar toohello following example:</span></span>

    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                            "Tier": "Standard",
                                            "Family": "MeteredData"
                                          }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                         "ServiceProviderName": "Equinix",
                                         "PeeringLocation": "Silicon Valley",
                                         "BandwidthInMbps": 200
                                          }
    ServiceKey                       : **************************************
    Peerings                         : []


<span data-ttu-id="1470e-179">Részletes leírását, az összes hello paraméterek kaphat hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="1470e-179">You can get detailed descriptions of all hello parameters by running hello following command:</span></span>

```powershell
get-help get-azurededicatedcircuit -detailed
```

## <span data-ttu-id="1470e-180"><a name="modify"></a>Egy ExpressRoute-kapcsolatcsoportot módosítása</span><span class="sxs-lookup"><span data-stu-id="1470e-180"><a name="modify"></a>Modifying an ExpressRoute circuit</span></span>
<span data-ttu-id="1470e-181">ExpressRoute-kapcsolatcsoportot egyes tulajdonságait módosíthatja kapcsolat befolyásolása nélkül.</span><span class="sxs-lookup"><span data-stu-id="1470e-181">You can modify certain properties of an ExpressRoute circuit without impacting connectivity.</span></span>

<span data-ttu-id="1470e-182">Mindent hello állásidő nélkül a következő:</span><span class="sxs-lookup"><span data-stu-id="1470e-182">You can do hello following with no downtime:</span></span>

* <span data-ttu-id="1470e-183">Engedélyezi vagy letiltja az ExpressRoute-kapcsolatcsoportot prémium ExpressRoute bővítményt.</span><span class="sxs-lookup"><span data-stu-id="1470e-183">Enable or disable an ExpressRoute premium add-on for your ExpressRoute circuit.</span></span>
* <span data-ttu-id="1470e-184">Az ExpressRoute-kapcsolatcsoportot növekedése hello sávszélesség megadott érhető el kapacitás hello porton.</span><span class="sxs-lookup"><span data-stu-id="1470e-184">Increase hello bandwidth of your ExpressRoute circuit provided there is capacity available on hello port.</span></span> <span data-ttu-id="1470e-185">Alacsonyabb verziójúra változtatása hello sávszélesség, a kapcsolat nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="1470e-185">Downgrading hello bandwidth of a circuit is not supported.</span></span> 
* <span data-ttu-id="1470e-186">Hello mérési adatok díjköteles tooUnlimited adatokat a terv módosítása</span><span class="sxs-lookup"><span data-stu-id="1470e-186">Change hello metering plan from Metered Data tooUnlimited Data.</span></span> <span data-ttu-id="1470e-187">Korlátlan adatforgalom tooMetered adatok nem támogatott a terv mérési hello módosítása.</span><span class="sxs-lookup"><span data-stu-id="1470e-187">Changing hello metering plan from Unlimited Data tooMetered Data is not supported.</span></span>
* <span data-ttu-id="1470e-188">Engedélyezheti és letilthatja *klasszikus műveletek engedélyezése*.</span><span class="sxs-lookup"><span data-stu-id="1470e-188">You can enable and disable *Allow Classic Operations*.</span></span>

<span data-ttu-id="1470e-189">További információ a korlátai és korlátozásai, tekintse meg a toohello [ExpressRoute – gyakori kérdések](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="1470e-189">For more information on limits and limitations, refer toohello [ExpressRoute FAQ](expressroute-faqs.md).</span></span>

### <a name="tooenable-hello-expressroute-premium-add-on"></a><span data-ttu-id="1470e-190">tooenable hello ExpressRoute prémium szintű bővítmény</span><span class="sxs-lookup"><span data-stu-id="1470e-190">tooenable hello ExpressRoute premium add-on</span></span>
<span data-ttu-id="1470e-191">Hello ExpressRoute prémium szintű bővítmény a következő PowerShell-részlet hello segítségével engedélyezheti a meglévő kapcsolat:</span><span class="sxs-lookup"><span data-stu-id="1470e-191">You can enable hello ExpressRoute premium add-on for your existing circuit by using hello following PowerShell snippet:</span></span>

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.Sku.Tier = "Premium"
$ckt.sku.Name = "Premium_MeteredData"

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

<span data-ttu-id="1470e-192">hello áramkör most kell hello ExpressRoute prémium bővítmény szolgáltatások engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="1470e-192">hello circuit will now have hello ExpressRoute premium add-on features enabled.</span></span> <span data-ttu-id="1470e-193">Kezdjük el számlázási meg hello prémium bővítmény képességhez, amint hello parancs sikeresen lefutott.</span><span class="sxs-lookup"><span data-stu-id="1470e-193">We will begin billing you for hello premium add-on capability as soon as hello command has successfully run.</span></span>

### <a name="toodisable-hello-expressroute-premium-add-on"></a><span data-ttu-id="1470e-194">toodisable hello ExpressRoute prémium szintű bővítmény</span><span class="sxs-lookup"><span data-stu-id="1470e-194">toodisable hello ExpressRoute premium add-on</span></span>
> [!IMPORTANT]
> <span data-ttu-id="1470e-195">Ez a művelet sikertelen lehet erőforrásokat, amelyek nagyobbak, mint a megengedett hello szabványos kapcsolat használata.</span><span class="sxs-lookup"><span data-stu-id="1470e-195">This operation can fail if you're using resources that are greater than what is permitted for hello standard circuit.</span></span>
> 
> 

<span data-ttu-id="1470e-196">Vegye figyelembe a következőket hello:</span><span class="sxs-lookup"><span data-stu-id="1470e-196">Note hello following:</span></span>

* <span data-ttu-id="1470e-197">Ahhoz, hogy megállapításában, a prémium szintű toostandard, győződjön meg arról, hogy kapcsolt virtuális hálózatok hello száma toohello áramkör érték kisebb, mint 10.</span><span class="sxs-lookup"><span data-stu-id="1470e-197">Before you downgrade from premium toostandard, you must ensure that hello number of virtual networks that are linked toohello circuit is less than 10.</span></span> <span data-ttu-id="1470e-198">Ha nem így tesz, a frissítési kérelem sikertelen lesz, és azt prémium ütemben számlázza meg.</span><span class="sxs-lookup"><span data-stu-id="1470e-198">If you don't do this, your update request fails, and we will bill you at premium rates.</span></span>
* <span data-ttu-id="1470e-199">Minden virtuális hálózat más geopolitikai régiókban kell választható.</span><span class="sxs-lookup"><span data-stu-id="1470e-199">You must unlink all virtual networks in other geopolitical regions.</span></span> <span data-ttu-id="1470e-200">Ha nem így tesz, a frissítési kérelem sikertelen lesz, és azt prémium ütemben számlázza meg.</span><span class="sxs-lookup"><span data-stu-id="1470e-200">If you don't do this, your update request will fail, and we will bill you at premium rates.</span></span>
* <span data-ttu-id="1470e-201">Az útvonaltábla a magánhálózati társviszony-létesítés kisebb, mint 4000 útvonalait kell lennie.</span><span class="sxs-lookup"><span data-stu-id="1470e-201">Your route table must be less than 4,000 routes for private peering.</span></span> <span data-ttu-id="1470e-202">Az útvonal tábla mérete nagyobb, mint 4000 útvonalakat, ha a hello BGP-munkamenetet esik, és nem fog újra engedélyezve, amíg a hirdetett hello számához nem 4000 éri el.</span><span class="sxs-lookup"><span data-stu-id="1470e-202">If your route table size is greater than 4,000 routes, hello BGP session drops and won't be reenabled until hello number of advertised prefixes goes below 4,000.</span></span>

<span data-ttu-id="1470e-203">Letilthatja a hello ExpressRoute prémium szintű bővítmény hello meglévő kör hello a következő PowerShell-parancsmag segítségével:</span><span class="sxs-lookup"><span data-stu-id="1470e-203">You can disable hello ExpressRoute premium add-on for hello existing circuit by using hello following PowerShell cmdlet:</span></span>

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.Sku.Tier = "Standard"
$ckt.sku.Name = "Standard_MeteredData"

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="tooupdate-hello-expressroute-circuit-bandwidth"></a><span data-ttu-id="1470e-204">tooupdate hello ExpressRoute-kapcsolatcsoport sávszélessége</span><span class="sxs-lookup"><span data-stu-id="1470e-204">tooupdate hello ExpressRoute circuit bandwidth</span></span>
<span data-ttu-id="1470e-205">A támogatott sávszélesség-beállításokat a szolgáltató, ellenőrizze a hello [ExpressRoute – gyakori kérdések](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="1470e-205">For supported bandwidth options for your provider, check hello [ExpressRoute FAQ](expressroute-faqs.md).</span></span> <span data-ttu-id="1470e-206">Kiválaszthatja a mérete nagyobb, mint a meglévő expressroute hello méretét.</span><span class="sxs-lookup"><span data-stu-id="1470e-206">You can pick any size greater than hello size of your existing circuit.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1470e-207">Előfordulhat, hogy toorecreate hello ExpressRoute-kapcsolatcsoportot esetén nincs elég kapacitás hello meglévő porton.</span><span class="sxs-lookup"><span data-stu-id="1470e-207">You may have toorecreate hello ExpressRoute circuit if there is inadequate capacity on hello existing port.</span></span> <span data-ttu-id="1470e-208">Hello kapcsolatcsoport nem frissíthető, ha nincsenek további kapacitást érhető el az adott helyhez.</span><span class="sxs-lookup"><span data-stu-id="1470e-208">You cannot upgrade hello circuit if there is no additional capacity available at that location.</span></span>
>
> <span data-ttu-id="1470e-209">Nem csökken a hello sávszélesség az ExpressRoute-kapcsolatcsoportot megszakítása nélkül.</span><span class="sxs-lookup"><span data-stu-id="1470e-209">You cannot reduce hello bandwidth of an ExpressRoute circuit without disruption.</span></span> <span data-ttu-id="1470e-210">Alacsonyabb verziójúra változtatása sávszélesség toodeprovision hello ExpressRoute-kapcsolatcsoportot meg, és majd építenie az új ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="1470e-210">Downgrading bandwidth requires you toodeprovision hello ExpressRoute circuit and then reprovision a new ExpressRoute circuit.</span></span>
> 

<span data-ttu-id="1470e-211">Miután eldöntötte, hogy milyen van szüksége, használja a következő parancs tooresize hello a kapcsolatcsoport mérete:</span><span class="sxs-lookup"><span data-stu-id="1470e-211">After you decide what size you need, use hello following command tooresize your circuit:</span></span>

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.ServiceProviderProperties.BandwidthInMbps = 1000

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```


<span data-ttu-id="1470e-212">A kapcsolatcsoport fog hello Microsoft oldalán kell méretezni.</span><span class="sxs-lookup"><span data-stu-id="1470e-212">Your circuit will be sized up on hello Microsoft side.</span></span> <span data-ttu-id="1470e-213">Ezután vegye fel a kapcsolatot a kapcsolat szolgáltató tooupdate konfigurációit, az ügyféloldali toomatch ezt a módosítást.</span><span class="sxs-lookup"><span data-stu-id="1470e-213">Then you must contact your connectivity provider tooupdate configurations on their side toomatch this change.</span></span> <span data-ttu-id="1470e-214">Miután elvégezte ezt az értesítést, kezdjük el számlázást, a frissített hello sávszélesség beállítást.</span><span class="sxs-lookup"><span data-stu-id="1470e-214">After you make this notification, we will begin billing you for hello updated bandwidth option.</span></span>

### <a name="toomove-hello-sku-from-metered-toounlimited"></a><span data-ttu-id="1470e-215">toomove hello SKU a mért toounlimited</span><span class="sxs-lookup"><span data-stu-id="1470e-215">toomove hello SKU from metered toounlimited</span></span>
<span data-ttu-id="1470e-216">A következő PowerShell-részlet hello segítségével módosíthatja egy ExpressRoute-kapcsolatcsoport Termékváltozata hello:</span><span class="sxs-lookup"><span data-stu-id="1470e-216">You can change hello SKU of an ExpressRoute circuit by using hello following PowerShell snippet:</span></span>

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.Sku.Family = "UnlimitedData"
$ckt.sku.Name = "Premium_UnlimitedData"

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="toocontrol-access-toohello-classic-and-resource-manager-environments"></a><span data-ttu-id="1470e-217">toocontrol hozzáférés toohello klasszikus és Resource Manager-környezetben</span><span class="sxs-lookup"><span data-stu-id="1470e-217">toocontrol access toohello classic and Resource Manager environments</span></span>
<span data-ttu-id="1470e-218">Tekintse át a hello utasításait [áthelyezése ExpressRoute-Kapcsolatcsoportok hello klasszikus toohello Resource Manager üzembe helyezési modellben a](expressroute-howto-move-arm.md).</span><span class="sxs-lookup"><span data-stu-id="1470e-218">Review hello instructions in [Move ExpressRoute circuits from hello classic toohello Resource Manager deployment model](expressroute-howto-move-arm.md).</span></span>  

## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a><span data-ttu-id="1470e-219">Megszüntetés és az ExpressRoute-kapcsolatcsoport törlése</span><span class="sxs-lookup"><span data-stu-id="1470e-219">Deprovisioning and deleting an ExpressRoute circuit</span></span>
<span data-ttu-id="1470e-220">Vegye figyelembe a következőket hello:</span><span class="sxs-lookup"><span data-stu-id="1470e-220">Note hello following:</span></span>

* <span data-ttu-id="1470e-221">Az ExpressRoute-kapcsolatcsoportot hello összes virtuális hálózatot kell választható.</span><span class="sxs-lookup"><span data-stu-id="1470e-221">You must unlink all virtual networks from hello ExpressRoute circuit.</span></span> <span data-ttu-id="1470e-222">Ha ez a művelet nem sikerül, ellenőrizze a toosee, ha a virtuális hálózatok kapcsolódó toohello áramkör.</span><span class="sxs-lookup"><span data-stu-id="1470e-222">If this operation fails, check toosee if any virtual networks are linked toohello circuit.</span></span>
* <span data-ttu-id="1470e-223">Ha hello ExpressRoute körön szolgáltató üzembe helyezési állapota **kiépítési** vagy **kiépítve** együttműködve kell a service provider toodeprovision hello kapcsolatcsoport az oldalon.</span><span class="sxs-lookup"><span data-stu-id="1470e-223">If hello ExpressRoute circuit service provider provisioning state is **Provisioning** or **Provisioned** you must work with your service provider toodeprovision hello circuit on their side.</span></span> <span data-ttu-id="1470e-224">Rendszer továbbra is tooreserve erőforrások és kiszámlázni Önnek, amíg hello szolgáltató megszüntetési hello áramkör befejeződik, és értesíti a számunkra.</span><span class="sxs-lookup"><span data-stu-id="1470e-224">We will continue tooreserve resources and bill you until hello service provider completes deprovisioning hello circuit and notifies us.</span></span>
* <span data-ttu-id="1470e-225">Ha hello szolgáltató rendelkezik platformelőfizetés hello áramkör (üzembe helyezési állapota hello szolgáltató értéke túl**nincs kiépítve**) hello áramkör törölhet.</span><span class="sxs-lookup"><span data-stu-id="1470e-225">If hello service provider has deprovisioned hello circuit (hello service provider provisioning state is set too**Not provisioned**) you can then delete hello circuit.</span></span> <span data-ttu-id="1470e-226">Ezzel leállítja a számlázási hello kör</span><span class="sxs-lookup"><span data-stu-id="1470e-226">This will stop billing for hello circuit</span></span>

<span data-ttu-id="1470e-227">Az ExpressRoute-kapcsolatcsoport törlése hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="1470e-227">You can delete your ExpressRoute circuit by running hello following command:</span></span>

```powershell
Remove-AzureRmExpressRouteCircuit -ResourceGroupName "ExpressRouteResourceGroup" -Name "ExpressRouteARMCircuit"
```

## <a name="next-steps"></a><span data-ttu-id="1470e-228">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1470e-228">Next steps</span></span>

<span data-ttu-id="1470e-229">Miután létrehozta a kapcsolatcsoport, győződjön meg arról, hogy Ön hello a következő:</span><span class="sxs-lookup"><span data-stu-id="1470e-229">After you create your circuit, make sure that you do hello following:</span></span>

* [<span data-ttu-id="1470e-230">Létrehozásához és módosításához az ExpressRoute-kapcsolatcsoport esetében routing</span><span class="sxs-lookup"><span data-stu-id="1470e-230">Create and modify routing for your ExpressRoute circuit</span></span>](expressroute-howto-routing-arm.md)
* [<span data-ttu-id="1470e-231">A virtuális hálózati tooyour ExpressRoute-kapcsolatcsoportot hivatkozás</span><span class="sxs-lookup"><span data-stu-id="1470e-231">Link your virtual network tooyour ExpressRoute circuit</span></span>](expressroute-howto-linkvnet-arm.md)
