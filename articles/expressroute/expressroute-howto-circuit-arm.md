---
title: "Létrehozásához és módosításához ExpressRoute-kapcsolatcsoportot: PowerShell: Azure Resource Manager |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan létrehozásához, telepítéséhez, győződjön meg arról, frissítése, törlése és kiosztásának megszüntetése ExpressRoute-kapcsolatcsoportot."
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
ms.openlocfilehash: 8bfae39d84aaac3b9527084df9dcfbd51f591dfe
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="create-and-modify-an-expressroute-circuit-using-powershell"></a><span data-ttu-id="2d9dd-103">Létrehozása és módosítása a powershellel ExpressRoute-kapcsolatcsoportot</span><span class="sxs-lookup"><span data-stu-id="2d9dd-103">Create and modify an ExpressRoute circuit using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="2d9dd-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="2d9dd-104">Azure portal</span></span>](expressroute-howto-circuit-portal-resource-manager.md)
> * [<span data-ttu-id="2d9dd-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="2d9dd-105">PowerShell</span></span>](expressroute-howto-circuit-arm.md)
> * [<span data-ttu-id="2d9dd-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="2d9dd-106">Azure CLI</span></span>](howto-circuit-cli.md)
> * [<span data-ttu-id="2d9dd-107">Video - Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="2d9dd-107">Video - Azure portal</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [<span data-ttu-id="2d9dd-108">PowerShell (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="2d9dd-108">PowerShell (classic)</span></span>](expressroute-howto-circuit-classic.md)
>

<span data-ttu-id="2d9dd-109">Ez a cikk ismerteti, hogyan hozhat létre Azure ExpressRoute-kapcsolatcsoportot PowerShell-parancsmagok és az Azure Resource Manager telepítési modell használatával.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-109">This article describes how to create an Azure ExpressRoute circuit by using PowerShell cmdlets and the Azure Resource Manager deployment model.</span></span> <span data-ttu-id="2d9dd-110">Ez a cikk is bemutatja, hogyan ellenőrizze a kapcsolatcsoport állapotát, a frissítést, vagy törlés és kiosztásának megszüntetése azt.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-110">This article also shows you how to check the status of the circuit, update it, or delete and deprovision it.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="2d9dd-111">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="2d9dd-111">Before you begin</span></span>
* <span data-ttu-id="2d9dd-112">Telepítse az Azure Resource Manager PowerShell-parancsmagjainak legújabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-112">Install the latest version of the Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="2d9dd-113">További információkért lásd: [áttekintés az Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="2d9dd-113">For more information, see [Overview of Azure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="2d9dd-114">Tekintse át a [Előfeltételek](expressroute-prerequisites.md) és [munkafolyamatok](expressroute-workflows.md) konfigurálás elkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-114">Review the [prerequisites](expressroute-prerequisites.md) and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>


## <a name="create-and-provision-an-expressroute-circuit"></a><span data-ttu-id="2d9dd-115">Hozzon létre, és helyezze üzembe az ExpressRoute-kapcsolatcsoportot</span><span class="sxs-lookup"><span data-stu-id="2d9dd-115">Create and provision an ExpressRoute circuit</span></span>
### <a name="1-sign-in-to-your-azure-account-and-select-your-subscription"></a><span data-ttu-id="2d9dd-116">1. Jelentkezzen be az Azure-fiókjával, és jelölje ki az előfizetését</span><span class="sxs-lookup"><span data-stu-id="2d9dd-116">1. Sign in to your Azure account and select your subscription</span></span>
<span data-ttu-id="2d9dd-117">A konfiguráció első lépésként jelentkezzen be az Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-117">To begin your configuration, sign in to your Azure account.</span></span> <span data-ttu-id="2d9dd-118">Az alábbi példák segítségével csatlakozzon:</span><span class="sxs-lookup"><span data-stu-id="2d9dd-118">Use the following examples to help you connect:</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="2d9dd-119">Ellenőrizze a fiókhoz tartozó előfizetések:</span><span class="sxs-lookup"><span data-stu-id="2d9dd-119">Check the subscriptions for the account:</span></span>

```powershell
Get-AzureRmSubscription
```

<span data-ttu-id="2d9dd-120">Válassza ki az előfizetést, az ExpressRoute-kapcsolatcsoportot létrehozni kívánt:</span><span class="sxs-lookup"><span data-stu-id="2d9dd-120">Select the subscription that you want to create an ExpressRoute circuit for:</span></span>

```powershell
Select-AzureRmSubscription -SubscriptionId "<subscription ID>"
```

### <a name="2-get-the-list-of-supported-providers-locations-and-bandwidths"></a><span data-ttu-id="2d9dd-121">2. A támogatott szolgáltatók, a helyek és a sávszélesség listájának lekérdezése</span><span class="sxs-lookup"><span data-stu-id="2d9dd-121">2. Get the list of supported providers, locations, and bandwidths</span></span>
<span data-ttu-id="2d9dd-122">ExpressRoute-kapcsolatcsoportot létrehozni, meg kell támogatott kapcsolat szolgáltatókat, a helyek és a sávszélesség-beállítások listája.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-122">Before you create an ExpressRoute circuit, you need the list of supported connectivity providers, locations, and bandwidth options.</span></span>

<span data-ttu-id="2d9dd-123">A PowerShell-parancsmag **Get-AzureRmExpressRouteServiceProvider** adja vissza ezt az információt fogja használni a későbbi lépésekben:</span><span class="sxs-lookup"><span data-stu-id="2d9dd-123">The PowerShell cmdlet **Get-AzureRmExpressRouteServiceProvider** returns this information, which you’ll use in later steps:</span></span>

```powershell
Get-AzureRmExpressRouteServiceProvider
```

<span data-ttu-id="2d9dd-124">Ellenőrizze, hogy ha a kapcsolat szolgáltatójánál nem szerepel-e.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-124">Check to see if your connectivity provider is listed there.</span></span> <span data-ttu-id="2d9dd-125">Jegyezze fel a következő információkat.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-125">Make a note of the following information.</span></span> <span data-ttu-id="2d9dd-126">Azt később szüksége expressroute-kapcsolatcsoporthoz létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-126">You'll need it later when you create a circuit.</span></span>

* <span data-ttu-id="2d9dd-127">Név</span><span class="sxs-lookup"><span data-stu-id="2d9dd-127">Name</span></span>
* <span data-ttu-id="2d9dd-128">PeeringLocations</span><span class="sxs-lookup"><span data-stu-id="2d9dd-128">PeeringLocations</span></span>
* <span data-ttu-id="2d9dd-129">BandwidthsOffered</span><span class="sxs-lookup"><span data-stu-id="2d9dd-129">BandwidthsOffered</span></span>

<span data-ttu-id="2d9dd-130">Most már készen áll az ExpressRoute-kapcsolatcsoportot létrehozni.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-130">You're now ready to create an ExpressRoute circuit.</span></span>   

### <a name="3-create-an-expressroute-circuit"></a><span data-ttu-id="2d9dd-131">3. ExpressRoute-kapcsolatcsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="2d9dd-131">3. Create an ExpressRoute circuit</span></span>
<span data-ttu-id="2d9dd-132">Ha még nem rendelkezik egy erőforráscsoport, kell létrehoznia egyet az ExpressRoute-kapcsolatcsoportot létrehozása előtt.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-132">If you don't already have a resource group, you must create one before you create your ExpressRoute circuit.</span></span> <span data-ttu-id="2d9dd-133">Ehhez futtassa a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="2d9dd-133">You can do so by running the following command:</span></span>

```powershell
New-AzureRmResourceGroup -Name "ExpressRouteResourceGroup" -Location "West US"
```


<span data-ttu-id="2d9dd-134">A következő példa bemutatja, hogyan szilícium Valley egy 200 MB/s Equinix keresztül ExpressRoute-kapcsolatcsoportot létrehozni.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-134">The following example shows how to create a 200-Mbps ExpressRoute circuit through Equinix in Silicon Valley.</span></span> <span data-ttu-id="2d9dd-135">Különböző szolgáltatók és más beállítások használata, amikor a kérést helyettesítse be ezt az információt.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-135">If you're using a different provider and different settings, substitute that information when you make your request.</span></span> <span data-ttu-id="2d9dd-136">A következő egy példa egy kérelem egy új szolgáltatás kulcs:</span><span class="sxs-lookup"><span data-stu-id="2d9dd-136">The following is an example request for a new service key:</span></span>

```powershell
New-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup" -Location "West US" -SkuTier Standard -SkuFamily MeteredData -ServiceProviderName "Equinix" -PeeringLocation "Silicon Valley" -BandwidthInMbps 200
```

<span data-ttu-id="2d9dd-137">Győződjön meg arról, hogy megadja a helyes SKU réteg és SKU-család:</span><span class="sxs-lookup"><span data-stu-id="2d9dd-137">Make sure that you specify the correct SKU tier and SKU family:</span></span>

* <span data-ttu-id="2d9dd-138">Termékváltozat szint határozza meg, egy ExpressRoute standard vagy ExpressRoute prémium bővítmény engedélyezve van-e.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-138">SKU tier determines whether an ExpressRoute standard or an ExpressRoute premium add-on is enabled.</span></span> <span data-ttu-id="2d9dd-139">Megadhat *szabványos* lekérni a standard Termékváltozat vagy *prémium* a prémium szintű bővítménnyel.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-139">You can specify *Standard* to get the standard SKU or *Premium* for the premium add-on.</span></span>
* <span data-ttu-id="2d9dd-140">SKU-család határozza meg a számlázási.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-140">SKU family determines the billing type.</span></span> <span data-ttu-id="2d9dd-141">Megadhat *Metereddata* díjköteles adatforgalom tervhez és *Unlimiteddata* egy adatforgalmi a.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-141">You can specify *Metereddata* for a metered data plan and *Unlimiteddata* for an unlimited data plan.</span></span> <span data-ttu-id="2d9dd-142">Módosíthatja a számlázási típusának *Metereddata* való *Unlimiteddata*, de nem módosíthatja a típusát a *Unlimiteddata* való *Metereddata*.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-142">You can change the billing type from *Metereddata* to *Unlimiteddata*, but you can't change the type from *Unlimiteddata* to *Metereddata*.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2d9dd-143">Az ExpressRoute-kapcsolatcsoportot abban a pillanatban a szolgáltatási kulcs kiadott lesz terhelve.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-143">Your ExpressRoute circuit will be billed from the moment a service key is issued.</span></span> <span data-ttu-id="2d9dd-144">Győződjön meg arról, hogy a művelet végrehajtása, ha a kapcsolat szolgáltatójánál kiépíteni a kapcsolat készen áll-e.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-144">Ensure that you perform this operation when the connectivity provider is ready to provision the circuit.</span></span>
> 
> 

<span data-ttu-id="2d9dd-145">A válasz tartalmazza a szolgáltatási kulcs.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-145">The response contains the service key.</span></span> <span data-ttu-id="2d9dd-146">A paraméterek részletes leírását a következő parancs futtatásával szerezheti be:</span><span class="sxs-lookup"><span data-stu-id="2d9dd-146">You can get detailed descriptions of all the parameters by running the following command:</span></span>

```powershell
get-help New-AzureRmExpressRouteCircuit -detailed
```


### <a name="4-list-all-expressroute-circuits"></a><span data-ttu-id="2d9dd-147">4. A lista összes ExpressRoute-Kapcsolatcsoportok</span><span class="sxs-lookup"><span data-stu-id="2d9dd-147">4. List all ExpressRoute circuits</span></span>
<span data-ttu-id="2d9dd-148">Minden létrehozott ExpressRoute-Kapcsolatcsoportok listájának lekéréséhez futtassa a **Get-AzureRmExpressRouteCircuit** parancs:</span><span class="sxs-lookup"><span data-stu-id="2d9dd-148">To get a list of all the ExpressRoute circuits that you created, run the **Get-AzureRmExpressRouteCircuit** command:</span></span>

```powershell
Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
```

<span data-ttu-id="2d9dd-149">A válasz a következőhöz hasonló fog kinézni:</span><span class="sxs-lookup"><span data-stu-id="2d9dd-149">The response will look similar to the following example:</span></span>

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

<span data-ttu-id="2d9dd-150">Ezt az információt bármikor használatával kérheti le a `Get-AzureRmExpressRouteCircuit` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-150">You can retrieve this information at any time by using the `Get-AzureRmExpressRouteCircuit` cmdlet.</span></span> <span data-ttu-id="2d9dd-151">A kapcsolatok a következő hívással paraméter nélküli sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-151">Making the call with no parameters lists all the circuits.</span></span> <span data-ttu-id="2d9dd-152">A szolgáltatás kulcs megjelenik a *ServiceKey* mező:</span><span class="sxs-lookup"><span data-stu-id="2d9dd-152">Your service key will be listed in the *ServiceKey* field:</span></span>

```powershell
Get-AzureRmExpressRouteCircuit
```


<span data-ttu-id="2d9dd-153">A válasz a következőhöz hasonló fog kinézni:</span><span class="sxs-lookup"><span data-stu-id="2d9dd-153">The response will look similar to the following example:</span></span>

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


<span data-ttu-id="2d9dd-154">A paraméterek részletes leírását a következő parancs futtatásával szerezheti be:</span><span class="sxs-lookup"><span data-stu-id="2d9dd-154">You can get detailed descriptions of all the parameters by running the following command:</span></span>

```powershell
get-help Get-AzureRmExpressRouteCircuit -detailed
```

### <a name="5-send-the-service-key-to-your-connectivity-provider-for-provisioning"></a><span data-ttu-id="2d9dd-155">5. A szolgáltatás kulcs küldése a kapcsolat szolgáltatójánál történő üzembe helyezéséhez</span><span class="sxs-lookup"><span data-stu-id="2d9dd-155">5. Send the service key to your connectivity provider for provisioning</span></span>
<span data-ttu-id="2d9dd-156">*ServiceProviderProvisioningState* információkat nyújt azokról a szolgáltatói oldalon kiépítés aktuális állapotával.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-156">*ServiceProviderProvisioningState* provides information about the current state of provisioning on the service-provider side.</span></span> <span data-ttu-id="2d9dd-157">Állapot állapotát biztosít a Microsoft oldalon.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-157">Status provides the state on the Microsoft side.</span></span> <span data-ttu-id="2d9dd-158">Kiépítés állapotok áramkör kapcsolatos további információkért tekintse meg a [munkafolyamatok](expressroute-workflows.md#expressroute-circuit-provisioning-states) cikk.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-158">For more information about circuit provisioning states, see the [Workflows](expressroute-workflows.md#expressroute-circuit-provisioning-states) article.</span></span>

<span data-ttu-id="2d9dd-159">Amikor létrehoz egy új ExpressRoute-kapcsolatcsoportot, a kapcsolatcsoport lesz a következő állapotot okozta:</span><span class="sxs-lookup"><span data-stu-id="2d9dd-159">When you create a new ExpressRoute circuit, the circuit will be in the following state:</span></span>

    ServiceProviderProvisioningState : NotProvisioned
    CircuitProvisioningState         : Enabled



<span data-ttu-id="2d9dd-160">A kapcsolatcsoport fogja módosítani a következő állapotot folyamatban van, lehetővé téve, a kapcsolat hitelesítésszolgáltató esetén:</span><span class="sxs-lookup"><span data-stu-id="2d9dd-160">The circuit will change to the following state when the connectivity provider is in the process of enabling it for you:</span></span>

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled

<span data-ttu-id="2d9dd-161">Meg kell tudni használni az ExpressRoute-kapcsolatcsoportot a következő állapotban kell lennie:</span><span class="sxs-lookup"><span data-stu-id="2d9dd-161">For you to be able to use an ExpressRoute circuit, it must be in the following state:</span></span>

    ServiceProviderProvisioningState : Provisioned
    CircuitProvisioningState         : Enabled

### <a name="6-periodically-check-the-status-and-the-state-of-the-circuit-key"></a><span data-ttu-id="2d9dd-162">6. Ellenőrizze rendszeresen a kapcsolatcsoport kulcs állapotát és az állapot</span><span class="sxs-lookup"><span data-stu-id="2d9dd-162">6. Periodically check the status and the state of the circuit key</span></span>
<span data-ttu-id="2d9dd-163">Az állapot és a kapcsolatcsoport kulcs állapotának ellenőrzése értesíti Önt arról, amikor a szolgáltató a kapcsolatcsoport engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-163">Checking the status and the state of the circuit key lets you know when your provider has enabled your circuit.</span></span> <span data-ttu-id="2d9dd-164">A kapcsolat konfigurálása után *ServiceProviderProvisioningState* jelenik meg *kiépítve*, a következő példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="2d9dd-164">After the circuit has been configured, *ServiceProviderProvisioningState* appears as *Provisioned*, as shown in the following example:</span></span>

```powershell
Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
```


<span data-ttu-id="2d9dd-165">A válasz a következőhöz hasonló fog kinézni:</span><span class="sxs-lookup"><span data-stu-id="2d9dd-165">The response will look similar to the following example:</span></span>

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

### <a name="7-create-your-routing-configuration"></a><span data-ttu-id="2d9dd-166">7. Az útválasztó-konfiguráció létrehozása</span><span class="sxs-lookup"><span data-stu-id="2d9dd-166">7. Create your routing configuration</span></span>
<span data-ttu-id="2d9dd-167">Részletes útmutatásért lásd: a [ExpressRoute-áramkör útválasztási konfigurációja](expressroute-howto-routing-arm.md) cikk létrehozásához és módosításához a kapcsolatcsoport esetében.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-167">For step-by-step instructions, see the [ExpressRoute circuit routing configuration](expressroute-howto-routing-arm.md) article to create and modify circuit peerings.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2d9dd-168">Ezek az utasítások csak a szolgáltatók által biztosított réteg 2 internetkapcsolati szolgáltatás használatával létrehozott kapcsolatok vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-168">These instructions only apply to circuits that are created with service providers that offer layer 2 connectivity services.</span></span> <span data-ttu-id="2d9dd-169">A szolgáltató által kezelt használata réteg (általában az IP VPN, például az MPLS) 3 szolgáltatások, a kapcsolat szolgáltatójánál konfigurálása és kezelése az Ön útválasztást.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-169">If you're using a service provider that offers managed layer 3 services (typically an IP VPN, like MPLS), your connectivity provider will configure and manage routing for you.</span></span>
> 
> 

### <a name="8-link-a-virtual-network-to-an-expressroute-circuit"></a><span data-ttu-id="2d9dd-170">8. Virtuális hálózat összekapcsolása egy ExpressRoute-kapcsolatcsoporttal</span><span class="sxs-lookup"><span data-stu-id="2d9dd-170">8. Link a virtual network to an ExpressRoute circuit</span></span>
<span data-ttu-id="2d9dd-171">A következő csatolja az ExpressRoute-kapcsolatcsoportot egy virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-171">Next, link a virtual network to your ExpressRoute circuit.</span></span> <span data-ttu-id="2d9dd-172">Használja a [virtuális hálózatok összekapcsolása ExpressRoute-Kapcsolatcsoportok](expressroute-howto-linkvnet-arm.md) a Resource Manager üzembe helyezési modellel végzett munka során a következő cikket.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-172">Use the [Linking virtual networks to ExpressRoute circuits](expressroute-howto-linkvnet-arm.md) article when you work with the Resource Manager deployment model.</span></span>

## <a name="getting-the-status-of-an-expressroute-circuit"></a><span data-ttu-id="2d9dd-173">Egy ExpressRoute-kapcsolatcsoport állapotának beolvasása</span><span class="sxs-lookup"><span data-stu-id="2d9dd-173">Getting the status of an ExpressRoute circuit</span></span>
<span data-ttu-id="2d9dd-174">Ezt az információt bármikor használatával kérheti le a **Get-AzureRmExpressRouteCircuit** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-174">You can retrieve this information at any time by using the **Get-AzureRmExpressRouteCircuit** cmdlet.</span></span> <span data-ttu-id="2d9dd-175">A kapcsolatok a következő hívással paraméter nélküli sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-175">Making the call with no parameters lists all the circuits.</span></span>

```powershell
Get-AzureRmExpressRouteCircuit
```


<span data-ttu-id="2d9dd-176">A válasz a következőhöz hasonló lesz:</span><span class="sxs-lookup"><span data-stu-id="2d9dd-176">The response will be similar to the following example:</span></span>

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


<span data-ttu-id="2d9dd-177">Egy adott ExpressRoute-kapcsolatcsoportot tájékoztatást kaphat az erőforráscsoport-név és a kapcsolat neve átadott paraméterként a hívást:</span><span class="sxs-lookup"><span data-stu-id="2d9dd-177">You can get information on a specific ExpressRoute circuit by passing the resource group name and circuit name as a parameter to the call:</span></span>

```powershell
Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
```


<span data-ttu-id="2d9dd-178">A válasz a következőhöz hasonló fog kinézni:</span><span class="sxs-lookup"><span data-stu-id="2d9dd-178">The response will look similar to the following example:</span></span>

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


<span data-ttu-id="2d9dd-179">A paraméterek részletes leírását a következő parancs futtatásával szerezheti be:</span><span class="sxs-lookup"><span data-stu-id="2d9dd-179">You can get detailed descriptions of all the parameters by running the following command:</span></span>

```powershell
get-help get-azurededicatedcircuit -detailed
```

## <span data-ttu-id="2d9dd-180"><a name="modify"></a>Egy ExpressRoute-kapcsolatcsoportot módosítása</span><span class="sxs-lookup"><span data-stu-id="2d9dd-180"><a name="modify"></a>Modifying an ExpressRoute circuit</span></span>
<span data-ttu-id="2d9dd-181">ExpressRoute-kapcsolatcsoportot egyes tulajdonságait módosíthatja kapcsolat befolyásolása nélkül.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-181">You can modify certain properties of an ExpressRoute circuit without impacting connectivity.</span></span>

<span data-ttu-id="2d9dd-182">Állásidő nélkül a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="2d9dd-182">You can do the following with no downtime:</span></span>

* <span data-ttu-id="2d9dd-183">Engedélyezi vagy letiltja az ExpressRoute-kapcsolatcsoportot prémium ExpressRoute bővítményt.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-183">Enable or disable an ExpressRoute premium add-on for your ExpressRoute circuit.</span></span>
* <span data-ttu-id="2d9dd-184">Növelje a ExpressRoute-kapcsolatcsoportot sávszélességét, feltéve, hogy kapacitás érhető el a port.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-184">Increase the bandwidth of your ExpressRoute circuit provided there is capacity available on the port.</span></span> <span data-ttu-id="2d9dd-185">Alacsonyabb verziójúra változtatása a sávszélességet a kapcsolat nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-185">Downgrading the bandwidth of a circuit is not supported.</span></span> 
* <span data-ttu-id="2d9dd-186">Díjköteles adatforgalom korlátlan adatokhoz a mérési terv módosítása</span><span class="sxs-lookup"><span data-stu-id="2d9dd-186">Change the metering plan from Metered Data to Unlimited Data.</span></span> <span data-ttu-id="2d9dd-187">A mérési terv módosítása az korlátlan adatforgalom díjköteles adatok nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-187">Changing the metering plan from Unlimited Data to Metered Data is not supported.</span></span>
* <span data-ttu-id="2d9dd-188">Engedélyezheti és letilthatja *klasszikus műveletek engedélyezése*.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-188">You can enable and disable *Allow Classic Operations*.</span></span>

<span data-ttu-id="2d9dd-189">Korlátai és korlátozásai további információkért tekintse meg a [ExpressRoute – gyakori kérdések](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="2d9dd-189">For more information on limits and limitations, refer to the [ExpressRoute FAQ](expressroute-faqs.md).</span></span>

### <a name="to-enable-the-expressroute-premium-add-on"></a><span data-ttu-id="2d9dd-190">A prémium szintű ExpressRoute-bővítmény engedélyezése</span><span class="sxs-lookup"><span data-stu-id="2d9dd-190">To enable the ExpressRoute premium add-on</span></span>
<span data-ttu-id="2d9dd-191">A prémium szintű ExpressRoute-bővítmény a következő PowerShell-részlet használatával engedélyezheti a meglévő kapcsolat:</span><span class="sxs-lookup"><span data-stu-id="2d9dd-191">You can enable the ExpressRoute premium add-on for your existing circuit by using the following PowerShell snippet:</span></span>

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.Sku.Tier = "Premium"
$ckt.sku.Name = "Premium_MeteredData"

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

<span data-ttu-id="2d9dd-192">A kapcsolatcsoport most lesz engedélyezett ExpressRoute prémium bővítmény funkciók.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-192">The circuit will now have the ExpressRoute premium add-on features enabled.</span></span> <span data-ttu-id="2d9dd-193">Kezdjük el számlázási, a prémium szintű bővítmény képességhez, amint sikeresen futtatta a parancsot.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-193">We will begin billing you for the premium add-on capability as soon as the command has successfully run.</span></span>

### <a name="to-disable-the-expressroute-premium-add-on"></a><span data-ttu-id="2d9dd-194">A prémium szintű ExpressRoute-bővítmény letiltása</span><span class="sxs-lookup"><span data-stu-id="2d9dd-194">To disable the ExpressRoute premium add-on</span></span>
> [!IMPORTANT]
> <span data-ttu-id="2d9dd-195">A művelet sikertelen lesz, amely nagyobb, mint mi a szabványos kör megengedett erőforrások használata.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-195">This operation can fail if you're using resources that are greater than what is permitted for the standard circuit.</span></span>
> 
> 

<span data-ttu-id="2d9dd-196">Vegye figyelembe a következőket:</span><span class="sxs-lookup"><span data-stu-id="2d9dd-196">Note the following:</span></span>

* <span data-ttu-id="2d9dd-197">Mielőtt visszaminősítését prémiumról szabvány, meg kell győződnie arról, hogy a kapcsolatcsoport csatolt virtuális hálózatok száma kisebb, mint 10.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-197">Before you downgrade from premium to standard, you must ensure that the number of virtual networks that are linked to the circuit is less than 10.</span></span> <span data-ttu-id="2d9dd-198">Ha nem így tesz, a frissítési kérelem sikertelen lesz, és azt prémium ütemben számlázza meg.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-198">If you don't do this, your update request fails, and we will bill you at premium rates.</span></span>
* <span data-ttu-id="2d9dd-199">Minden virtuális hálózat más geopolitikai régiókban kell választható.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-199">You must unlink all virtual networks in other geopolitical regions.</span></span> <span data-ttu-id="2d9dd-200">Ha nem így tesz, a frissítési kérelem sikertelen lesz, és azt prémium ütemben számlázza meg.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-200">If you don't do this, your update request will fail, and we will bill you at premium rates.</span></span>
* <span data-ttu-id="2d9dd-201">Az útvonaltábla a magánhálózati társviszony-létesítés kisebb, mint 4000 útvonalait kell lennie.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-201">Your route table must be less than 4,000 routes for private peering.</span></span> <span data-ttu-id="2d9dd-202">Az útvonal tábla mérete nagyobb, mint 4000 útvonalakat, ha a BGP-munkamenetet esik, és nem fog újra engedélyezve, amíg a hirdetett számához nem 4000 éri el.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-202">If your route table size is greater than 4,000 routes, the BGP session drops and won't be reenabled until the number of advertised prefixes goes below 4,000.</span></span>

<span data-ttu-id="2d9dd-203">Letilthatja a meglévő kapcsolat az ExpressRoute prémium szintű bővítmény a következő PowerShell-parancsmag segítségével:</span><span class="sxs-lookup"><span data-stu-id="2d9dd-203">You can disable the ExpressRoute premium add-on for the existing circuit by using the following PowerShell cmdlet:</span></span>

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.Sku.Tier = "Standard"
$ckt.sku.Name = "Standard_MeteredData"

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="to-update-the-expressroute-circuit-bandwidth"></a><span data-ttu-id="2d9dd-204">Az ExpressRoute-kapcsolatcsoport sávszélessége frissítése</span><span class="sxs-lookup"><span data-stu-id="2d9dd-204">To update the ExpressRoute circuit bandwidth</span></span>
<span data-ttu-id="2d9dd-205">Támogatott sávszélesség-beállítások a szolgáltató, ellenőrizze a [ExpressRoute – gyakori kérdések](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="2d9dd-205">For supported bandwidth options for your provider, check the [ExpressRoute FAQ](expressroute-faqs.md).</span></span> <span data-ttu-id="2d9dd-206">Kiválaszthatja a mérete nagyobb, mint a meglévő expressroute méretét.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-206">You can pick any size greater than the size of your existing circuit.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2d9dd-207">Előfordulhat, hogy újra létrehozni az ExpressRoute-kapcsolatcsoport, ha nincs elég kapacitás a meglévő porton.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-207">You may have to recreate the ExpressRoute circuit if there is inadequate capacity on the existing port.</span></span> <span data-ttu-id="2d9dd-208">A kapcsolat nem frissíthető, ha nincsenek további kapacitást érhető el az adott helyhez.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-208">You cannot upgrade the circuit if there is no additional capacity available at that location.</span></span>
>
> <span data-ttu-id="2d9dd-209">Nem csökkenti a sávszélesség az ExpressRoute-kapcsolatcsoportot megszakítása nélkül.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-209">You cannot reduce the bandwidth of an ExpressRoute circuit without disruption.</span></span> <span data-ttu-id="2d9dd-210">Alacsonyabb verziójúra változtatása a sávszélesség szükséges, hogy az ExpressRoute-kapcsolatcsoport kiosztásának megszüntetése és majd építenie az új ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-210">Downgrading bandwidth requires you to deprovision the ExpressRoute circuit and then reprovision a new ExpressRoute circuit.</span></span>
> 

<span data-ttu-id="2d9dd-211">Miután eldöntötte, hogy milyen méretű van szüksége, az alábbi parancs segítségével méretezze át a a kapcsolatcsoport:</span><span class="sxs-lookup"><span data-stu-id="2d9dd-211">After you decide what size you need, use the following command to resize your circuit:</span></span>

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.ServiceProviderProperties.BandwidthInMbps = 1000

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```


<span data-ttu-id="2d9dd-212">A kapcsolatcsoport lesz a Microsoft oldalon kell méretezni.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-212">Your circuit will be sized up on the Microsoft side.</span></span> <span data-ttu-id="2d9dd-213">Majd kérje meg a kapcsolat szolgáltatójánál, ez a módosítás megfelelő a ügyféloldali konfigurációjának frissítése.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-213">Then you must contact your connectivity provider to update configurations on their side to match this change.</span></span> <span data-ttu-id="2d9dd-214">Miután elvégezte ezt az értesítést, kezdjük el számlázást, a frissített sávszélesség-beállítás.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-214">After you make this notification, we will begin billing you for the updated bandwidth option.</span></span>

### <a name="to-move-the-sku-from-metered-to-unlimited"></a><span data-ttu-id="2d9dd-215">A Termékváltozat áthelyezése díjköteles korlátlanra</span><span class="sxs-lookup"><span data-stu-id="2d9dd-215">To move the SKU from metered to unlimited</span></span>
<span data-ttu-id="2d9dd-216">A következő PowerShell-részlet segítségével módosíthatja egy ExpressRoute-kapcsolatcsoport Termékváltozata:</span><span class="sxs-lookup"><span data-stu-id="2d9dd-216">You can change the SKU of an ExpressRoute circuit by using the following PowerShell snippet:</span></span>

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.Sku.Family = "UnlimitedData"
$ckt.sku.Name = "Premium_UnlimitedData"

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="to-control-access-to-the-classic-and-resource-manager-environments"></a><span data-ttu-id="2d9dd-217">A klasszikus és Resource Manager-környezetben való hozzáférés szabályozása</span><span class="sxs-lookup"><span data-stu-id="2d9dd-217">To control access to the classic and Resource Manager environments</span></span>
<span data-ttu-id="2d9dd-218">Tekintse át a utasításait [a Resource Manager üzembe helyezési modellben a klasszikus áthelyezése ExpressRoute-Kapcsolatcsoportok](expressroute-howto-move-arm.md).</span><span class="sxs-lookup"><span data-stu-id="2d9dd-218">Review the instructions in [Move ExpressRoute circuits from the classic to the Resource Manager deployment model](expressroute-howto-move-arm.md).</span></span>  

## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a><span data-ttu-id="2d9dd-219">Megszüntetés és az ExpressRoute-kapcsolatcsoport törlése</span><span class="sxs-lookup"><span data-stu-id="2d9dd-219">Deprovisioning and deleting an ExpressRoute circuit</span></span>
<span data-ttu-id="2d9dd-220">Vegye figyelembe a következőket:</span><span class="sxs-lookup"><span data-stu-id="2d9dd-220">Note the following:</span></span>

* <span data-ttu-id="2d9dd-221">Az ExpressRoute-kapcsolatcsoport az összes virtuális hálózatot kell választható.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-221">You must unlink all virtual networks from the ExpressRoute circuit.</span></span> <span data-ttu-id="2d9dd-222">Ha ez a művelet sikertelen, ellenőrizze, hogy ha a virtuális hálózatok a kapcsolatcsoport van csatolva.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-222">If this operation fails, check to see if any virtual networks are linked to the circuit.</span></span>
* <span data-ttu-id="2d9dd-223">Ha az ExpressRoute-kapcsolatcsoport szolgáltatás szolgáltató üzembe helyezési állapota **kiépítési** vagy **kiépítve** kiosztásának megszüntetése a kapcsolatcsoport az oldalon, hogy a szolgáltató együttműködve.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-223">If the ExpressRoute circuit service provider provisioning state is **Provisioning** or **Provisioned** you must work with your service provider to deprovision the circuit on their side.</span></span> <span data-ttu-id="2d9dd-224">Folytatjuk erőforrásokat és kiszámlázni Önnek, amíg a szolgáltató befejeződött, a kapcsolat megszüntetés, és értesítést küld nekünk.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-224">We will continue to reserve resources and bill you until the service provider completes deprovisioning the circuit and notifies us.</span></span>
* <span data-ttu-id="2d9dd-225">Ha a szolgáltató rendelkezik platformelőfizetés a kapcsolatcsoport (üzembe helyezési állapota szolgáltató értéke **nincs kiépítve**) a kapcsolatcsoport törölhet.</span><span class="sxs-lookup"><span data-stu-id="2d9dd-225">If the service provider has deprovisioned the circuit (the service provider provisioning state is set to **Not provisioned**) you can then delete the circuit.</span></span> <span data-ttu-id="2d9dd-226">A kör számlázási leállítása</span><span class="sxs-lookup"><span data-stu-id="2d9dd-226">This will stop billing for the circuit</span></span>

<span data-ttu-id="2d9dd-227">Az ExpressRoute-kapcsolatcsoport törlése a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="2d9dd-227">You can delete your ExpressRoute circuit by running the following command:</span></span>

```powershell
Remove-AzureRmExpressRouteCircuit -ResourceGroupName "ExpressRouteResourceGroup" -Name "ExpressRouteARMCircuit"
```

## <a name="next-steps"></a><span data-ttu-id="2d9dd-228">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2d9dd-228">Next steps</span></span>

<span data-ttu-id="2d9dd-229">Miután létrehozta a kapcsolatcsoport, győződjön meg arról, hogy akkor tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="2d9dd-229">After you create your circuit, make sure that you do the following:</span></span>

* [<span data-ttu-id="2d9dd-230">Létrehozásához és módosításához az ExpressRoute-kapcsolatcsoport esetében routing</span><span class="sxs-lookup"><span data-stu-id="2d9dd-230">Create and modify routing for your ExpressRoute circuit</span></span>](expressroute-howto-routing-arm.md)
* [<span data-ttu-id="2d9dd-231">A virtuális hálózat csatolása az ExpressRoute-kapcsolatcsoportot</span><span class="sxs-lookup"><span data-stu-id="2d9dd-231">Link your virtual network to your ExpressRoute circuit</span></span>](expressroute-howto-linkvnet-arm.md)
