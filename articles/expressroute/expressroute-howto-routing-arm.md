---
title: "Hogyan tooconfigure útválasztás (társviszony) az ExpressRoute-kapcsolatcsoportot: erőforrás-kezelő: PowerShell: Azure |} Microsoft Docs"
description: "Ez a cikk végigvezeti hello létrehozásához és a kiépítés hello saját, a nyilvános és a Microsoft társviszony-létesítést az ExpressRoute-kapcsolatcsoportot. Ez a cikk is bemutatja, hogyan toocheck hello állapotát, a frissítést, vagy a-kör társviszony törlése."
documentationcenter: na
services: expressroute
author: ganesr
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 0a036d51-77ae-4fee-9ddb-35f040fbdcdf
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: ganesr;cherylmc
ms.openlocfilehash: eb3ddf5c05a086ac3e22c64417e51381ef465921
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-modify-peering-for-an-expressroute-circuit-using-powershell"></a><span data-ttu-id="4612f-104">Létrehozása és módosítása a powershellel ExpressRoute-kör társviszony</span><span class="sxs-lookup"><span data-stu-id="4612f-104">Create and modify peering for an ExpressRoute circuit using PowerShell</span></span>

<span data-ttu-id="4612f-105">Ez a cikk segítséget nyújt a létrehozása és kezelése a PowerShell használatával hello Resource Manager üzembe helyezési modellel ExpressRoute-kapcsolatcsoportot útválasztási konfigurációja.</span><span class="sxs-lookup"><span data-stu-id="4612f-105">This article helps you create and manage routing configuration for an ExpressRoute circuit in hello Resource Manager deployment model using PowerShell.</span></span> <span data-ttu-id="4612f-106">Is hello állapota, update vagy delete ellenőrizze és kiosztásának megszüntetése ExpressRoute-kör társviszony.</span><span class="sxs-lookup"><span data-stu-id="4612f-106">You can also check hello status, update, or delete and deprovision peerings for an ExpressRoute circuit.</span></span> <span data-ttu-id="4612f-107">Ha egy másik módszer toowork toouse a kapcsolatcsoport rendelkező, jelölje be a cikk a következő lista hello:</span><span class="sxs-lookup"><span data-stu-id="4612f-107">If you want toouse a different method toowork with your circuit, select an article from hello following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="4612f-108">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="4612f-108">Azure portal</span></span>](expressroute-howto-routing-portal-resource-manager.md)
> * [<span data-ttu-id="4612f-109">PowerShell</span><span class="sxs-lookup"><span data-stu-id="4612f-109">PowerShell</span></span>](expressroute-howto-routing-arm.md)
> * [<span data-ttu-id="4612f-110">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="4612f-110">Azure CLI</span></span>](howto-routing-cli.md)
> * [<span data-ttu-id="4612f-111">Video - privát társviszony-létesítés</span><span class="sxs-lookup"><span data-stu-id="4612f-111">Video - Private peering</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-private-peering-for-your-expressroute-circuit)
> * [<span data-ttu-id="4612f-112">Video - nyilvános társviszony-létesítés</span><span class="sxs-lookup"><span data-stu-id="4612f-112">Video - Public peering</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-public-peering-for-your-expressroute-circuit)
> * [<span data-ttu-id="4612f-113">Videó – a Microsoft társviszony-létesítés</span><span class="sxs-lookup"><span data-stu-id="4612f-113">Video - Microsoft peering</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-microsoft-peering-for-your-expressroute-circuit)
> * [<span data-ttu-id="4612f-114">PowerShell (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="4612f-114">PowerShell (classic)</span></span>](expressroute-howto-routing-classic.md)
> 

## <a name="configuration-prerequisites"></a><span data-ttu-id="4612f-115">Konfigurációs előfeltételek</span><span class="sxs-lookup"><span data-stu-id="4612f-115">Configuration prerequisites</span></span>

* <span data-ttu-id="4612f-116">Szüksége lesz Azure Resource Manager PowerShell-parancsmagok hello hello legújabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="4612f-116">You will need hello latest version of hello Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="4612f-117">További információkért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="4612f-117">For more information, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span> 
* <span data-ttu-id="4612f-118">Győződjön meg arról, hogy átolvasta hello [Előfeltételek](expressroute-prerequisites.md) lap hello [útválasztási követelmények](expressroute-routing.md) lap és hello [munkafolyamatok](expressroute-workflows.md) lapon konfigurálás elkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="4612f-118">Make sure that you have reviewed hello [prerequisites](expressroute-prerequisites.md) page, hello [routing requirements](expressroute-routing.md) page, and hello [workflows](expressroute-workflows.md) page before you begin configuration.</span></span>
* <span data-ttu-id="4612f-119">Egy aktív ExpressRoute-kapcsolatcsoportra lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="4612f-119">You must have an active ExpressRoute circuit.</span></span> <span data-ttu-id="4612f-120">Útmutatás alapján hello túl[ExpressRoute-kapcsolatcsoportot létrehozni](expressroute-howto-circuit-arm.md) , és folytassa a kapcsolat szolgáltatójánál előtt által engedélyezett hello áramkör.</span><span class="sxs-lookup"><span data-stu-id="4612f-120">Follow hello instructions too[Create an ExpressRoute circuit](expressroute-howto-circuit-arm.md) and have hello circuit enabled by your connectivity provider before you proceed.</span></span> <span data-ttu-id="4612f-121">hello ExpressRoute-kapcsolatcsoportot meg toobe képes toorun hello parancsmagok Ez a cikk a kiépített és engedélyezett állapotban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="4612f-121">hello ExpressRoute circuit must be in a provisioned and enabled state for you toobe able toorun hello cmdlets in this article.</span></span>

<span data-ttu-id="4612f-122">Ezek az utasítások csak a 2. rétegbeli kapcsolatot szolgáltatásokat nyújtó szolgáltatók létre toocircuits vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="4612f-122">These instructions only apply toocircuits created with service providers offering Layer 2 connectivity services.</span></span> <span data-ttu-id="4612f-123">A szolgáltató által kezelt használata réteg (általában egy IPVPN, például az MPLS) 3 szolgáltatások, a kapcsolat szolgáltatójánál konfigurálása és kezelése az Ön útválasztást.</span><span class="sxs-lookup"><span data-stu-id="4612f-123">If you are using a service provider that offers managed Layer 3 services (typically an IPVPN, like MPLS), your connectivity provider will configure and manage routing for you.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4612f-124">A Microsoft jelenleg hirdetményt hello felügyeleti portálon keresztül szolgáltatók által konfigurált esetében.</span><span class="sxs-lookup"><span data-stu-id="4612f-124">We currently do not advertise peerings configured by service providers through hello service management portal.</span></span> <span data-ttu-id="4612f-125">Dolgozunk azon, hogy hamarosan bevezethessük ezt a képességet.</span><span class="sxs-lookup"><span data-stu-id="4612f-125">We are working on enabling this capability soon.</span></span> <span data-ttu-id="4612f-126">A BGP társviszony konfigurálása előtt ellenőrizze a szolgáltató.</span><span class="sxs-lookup"><span data-stu-id="4612f-126">Check with your service provider before configuring BGP peerings.</span></span>
> 
> 

<span data-ttu-id="4612f-127">Egy, két vagy akár mindhárom társviszony-létesítést (Azure privát, Azure nyilvános és Microsoft) is konfigurálhatja egy adott ExpressRoute-kapcsolatcsoportban.</span><span class="sxs-lookup"><span data-stu-id="4612f-127">You can configure one, two, or all three peerings (Azure private, Azure public and Microsoft) for an ExpressRoute circuit.</span></span> <span data-ttu-id="4612f-128">A társviszony-létesítéseket tetszőleges sorrendben konfigurálhatja.</span><span class="sxs-lookup"><span data-stu-id="4612f-128">You can configure peerings in any order you choose.</span></span> <span data-ttu-id="4612f-129">Azonban kell győződjön meg arról, hogy elvégezte-e minden társviszony-létesítési egyszerre csak egy hello konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="4612f-129">However, you must make sure that you complete hello configuration of each peering one at a time.</span></span> 

## <a name="azure-private-peering"></a><span data-ttu-id="4612f-130">Azure privát társviszony-létesítés</span><span class="sxs-lookup"><span data-stu-id="4612f-130">Azure private peering</span></span>

<span data-ttu-id="4612f-131">Ez a szakasz segítséget nyújt a létrehozása, beolvasása, frissítése és törlése hello Azure privát társviszony-létesítési konfiguráció ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="4612f-131">This section helps you create, get, update, and delete hello Azure private peering configuration for an ExpressRoute circuit.</span></span>

### <a name="toocreate-azure-private-peering"></a><span data-ttu-id="4612f-132">az Azure magánhálózati társviszony-létesítés toocreate</span><span class="sxs-lookup"><span data-stu-id="4612f-132">toocreate Azure private peering</span></span>

1. <span data-ttu-id="4612f-133">ExpressRoute hello PowerShell modul importálása.</span><span class="sxs-lookup"><span data-stu-id="4612f-133">Import hello PowerShell module for ExpressRoute.</span></span>

  <span data-ttu-id="4612f-134">Telepítenie kell a hello legújabb PowerShell installer [PowerShell-galériában](http://www.powershellgallery.com/) és hello Azure Resource Manager modulok importálása hello PowerShell-munkamenetet a rendelés toostart hello ExpressRoute-parancsmagok használatával.</span><span class="sxs-lookup"><span data-stu-id="4612f-134">You must install hello latest PowerShell installer from [PowerShell Gallery](http://www.powershellgallery.com/) and import hello Azure Resource Manager modules into hello PowerShell session in order toostart using hello ExpressRoute cmdlets.</span></span> <span data-ttu-id="4612f-135">Rendszergazdaként kell toorun PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4612f-135">You will need toorun PowerShell as an Administrator.</span></span>

  ```powershell
  Install-Module AzureRM
  Install-AzureRM
  ```

  <span data-ttu-id="4612f-136">Importálja az összes hello AzureRM.* modulok hello ismert szemantikai verziója tartományon belül.</span><span class="sxs-lookup"><span data-stu-id="4612f-136">Import all of hello AzureRM.* modules within hello known semantic version range.</span></span>

  ```powershell
  Import-AzureRM
  ```

  <span data-ttu-id="4612f-137">Csak is importálhatja egy select modul hello ismert szemantikai verziója tartományon belül.</span><span class="sxs-lookup"><span data-stu-id="4612f-137">You can also just import a select module within hello known semantic version range.</span></span>

  ```powershell
  Import-Module AzureRM.Network 
  ```

  <span data-ttu-id="4612f-138">Jelentkezzen be tooyour fiókjával.</span><span class="sxs-lookup"><span data-stu-id="4612f-138">Sign in tooyour account.</span></span>

  ```powershell
  Login-AzureRmAccount
  ```

  <span data-ttu-id="4612f-139">Válassza ki a kívánt toocreate ExpressRoute-kapcsolatcsoportot hello előfizetést.</span><span class="sxs-lookup"><span data-stu-id="4612f-139">Select hello subscription you want toocreate ExpressRoute circuit.</span></span>

  ```powershell
  Select-AzureRmSubscription -SubscriptionId "<subscription ID>"
  ```
2. <span data-ttu-id="4612f-140">Hozzon létre egy ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="4612f-140">Create an ExpressRoute circuit.</span></span>

  <span data-ttu-id="4612f-141">Kövesse az utasításokat toocreate hello egy [ExpressRoute-kapcsolatcsoportot](expressroute-howto-circuit-arm.md) , illetve hozta-e hello kapcsolat szolgáltatóját.</span><span class="sxs-lookup"><span data-stu-id="4612f-141">Follow hello instructions toocreate an [ExpressRoute circuit](expressroute-howto-circuit-arm.md) and have it provisioned by hello connectivity provider.</span></span>

  <span data-ttu-id="4612f-142">Ha a kapcsolat szolgáltatójánál 3. rétegbeli felügyelt szolgáltatásokat kínál, a kapcsolat szolgáltató tooenable magánhálózati társviszony-létesítést az Ön Azure kérhet.</span><span class="sxs-lookup"><span data-stu-id="4612f-142">If your connectivity provider offers managed Layer 3 services, you can request your connectivity provider tooenable Azure private peering for you.</span></span> <span data-ttu-id="4612f-143">Ebben az esetben nincs szükség hello következő szakaszokban szereplő toofollow utasításokat.</span><span class="sxs-lookup"><span data-stu-id="4612f-143">In that case, you won't need toofollow instructions listed in hello next sections.</span></span> <span data-ttu-id="4612f-144">Azonban ha a kapcsolat szolgáltatójánál nem kezeli az útválasztást, a kapcsolat létrehozása után továbbra is hello lépések a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="4612f-144">However, if your connectivity provider does not manage routing for you, after creating your circuit, continue your configuration using hello next steps.</span></span>
3. <span data-ttu-id="4612f-145">Ellenőrizze a hello ExpressRoute körön toomake arról, hogy van üzembe helyezve és is engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="4612f-145">Check hello ExpressRoute circuit toomake sure it is provisioned and also enabled.</span></span> <span data-ttu-id="4612f-146">A következő példa hello használata:</span><span class="sxs-lookup"><span data-stu-id="4612f-146">Use hello following example:</span></span>

  ```powershell
  Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
  ```

  <span data-ttu-id="4612f-147">a rendszer a következő példa hasonló toohello hello választ:</span><span class="sxs-lookup"><span data-stu-id="4612f-147">hello response is similar toohello following example:</span></span>

  ```
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
  ```
4. <span data-ttu-id="4612f-148">Az Azure magánhálózati társviszony-létesítés hello kör megadása</span><span class="sxs-lookup"><span data-stu-id="4612f-148">Configure Azure private peering for hello circuit.</span></span> <span data-ttu-id="4612f-149">Győződjön meg arról, hogy rendelkezik-e hello hello lépések folytatása előtt a következő elemek:</span><span class="sxs-lookup"><span data-stu-id="4612f-149">Make sure that you have hello following items before you proceed with hello next steps:</span></span>

  * <span data-ttu-id="4612f-150">/ 30-as alhálózat hello elsődleges kapcsolathoz.</span><span class="sxs-lookup"><span data-stu-id="4612f-150">A /30 subnet for hello primary link.</span></span> <span data-ttu-id="4612f-151">hello alhálózat nem virtuális hálózatok számára fenntartott címtartomány részének kell lennie.</span><span class="sxs-lookup"><span data-stu-id="4612f-151">hello subnet must not be part of any address space reserved for virtual networks.</span></span>
  * <span data-ttu-id="4612f-152">/ 30-as alhálózat hello másodlagos kapcsolathoz.</span><span class="sxs-lookup"><span data-stu-id="4612f-152">A /30 subnet for hello secondary link.</span></span> <span data-ttu-id="4612f-153">hello alhálózat nem virtuális hálózatok számára fenntartott címtartomány részének kell lennie.</span><span class="sxs-lookup"><span data-stu-id="4612f-153">hello subnet must not be part of any address space reserved for virtual networks.</span></span>
  * <span data-ttu-id="4612f-154">Egy érvényes VLAN-azonosító tooestablish a társviszony.</span><span class="sxs-lookup"><span data-stu-id="4612f-154">A valid VLAN ID tooestablish this peering on.</span></span> <span data-ttu-id="4612f-155">Győződjön meg arról, hogy egyetlen másik társviszonya se a hello áramkör használ hello azonos VLAN-azonosítót.</span><span class="sxs-lookup"><span data-stu-id="4612f-155">Ensure that no other peering in hello circuit uses hello same VLAN ID.</span></span>
  * <span data-ttu-id="4612f-156">Egy AS-szám a társviszony-létesítéshez.</span><span class="sxs-lookup"><span data-stu-id="4612f-156">AS number for peering.</span></span> <span data-ttu-id="4612f-157">2 és 4 bájtos AS-számokat is használhat.</span><span class="sxs-lookup"><span data-stu-id="4612f-157">You can use both 2-byte and 4-byte AS numbers.</span></span> <span data-ttu-id="4612f-158">Ehhez a társviszony-létesítéshez használhat privát AS-számokat is.</span><span class="sxs-lookup"><span data-stu-id="4612f-158">You can use a private AS number for this peering.</span></span> <span data-ttu-id="4612f-159">Ne használja a 65515 számot.</span><span class="sxs-lookup"><span data-stu-id="4612f-159">Ensure that you are not using 65515.</span></span>
  * <span data-ttu-id="4612f-160">**Választható -** az MD5 kivonatoló, ha úgy dönt, hogy egy toouse.</span><span class="sxs-lookup"><span data-stu-id="4612f-160">**Optional -** An MD5 hash if you choose toouse one.</span></span>

  <span data-ttu-id="4612f-161">A következő példa tooconfigure magánhálózati társviszony-létesítés a kör Azure hello használata:</span><span class="sxs-lookup"><span data-stu-id="4612f-161">Use hello following example tooconfigure Azure private peering for your circuit:</span></span>

  ```powershell
  Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200

  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

  <span data-ttu-id="4612f-162">Ha úgy dönt, toouse az MD5 kivonatoló, használja a következő példa hello:</span><span class="sxs-lookup"><span data-stu-id="4612f-162">If you choose toouse an MD5 hash, use hello following example:</span></span>

  ```powershell
  Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200  -SharedKey "A1B2C3D4"
  ```

  > [!IMPORTANT]
  > <span data-ttu-id="4612f-163">Az AS-számot mindenképp társviszony-létesítési ASN-ként, és ne ügyfél ASN-ként adja meg.</span><span class="sxs-lookup"><span data-stu-id="4612f-163">Ensure that you specify your AS number as peering ASN, not customer ASN.</span></span>
  > 
  >

### <a name="tooview-azure-private-peering-details"></a><span data-ttu-id="4612f-164">tooview Azure magánhálózati társviszony-létesítés részletei</span><span class="sxs-lookup"><span data-stu-id="4612f-164">tooview Azure private peering details</span></span>

<span data-ttu-id="4612f-165">Konfigurációs részletek kaphat a következő példa hello használata:</span><span class="sxs-lookup"><span data-stu-id="4612f-165">You can get configuration details by using hello following example:</span></span>

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -Circuit $ckt
```

### <a name="tooupdate-azure-private-peering-configuration"></a><span data-ttu-id="4612f-166">tooupdate Azure magánhálózati társviszony-létesítési konfiguráció</span><span class="sxs-lookup"><span data-stu-id="4612f-166">tooupdate Azure private peering configuration</span></span>

<span data-ttu-id="4612f-167">Frissítheti, használja a következő példa hello hello konfiguráció bármely részeként.</span><span class="sxs-lookup"><span data-stu-id="4612f-167">You can update any part of hello configuration using hello following example.</span></span> <span data-ttu-id="4612f-168">Ebben a példában a 100 too500 hello VLAN-Azonosítót a kapcsolatcsoport hello frissül.</span><span class="sxs-lookup"><span data-stu-id="4612f-168">In this example, hello VLAN ID of hello circuit is being updated from 100 too500.</span></span>

```powershell
Set-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="toodelete-azure-private-peering"></a><span data-ttu-id="4612f-169">az Azure magánhálózati társviszony-létesítés toodelete</span><span class="sxs-lookup"><span data-stu-id="4612f-169">toodelete Azure private peering</span></span>

<span data-ttu-id="4612f-170">A társviszony-létesítési konfiguráció a következő példa hello futtatásával távolíthatja el:</span><span class="sxs-lookup"><span data-stu-id="4612f-170">You can remove your peering configuration by running hello following example:</span></span>

> [!WARNING]
> <span data-ttu-id="4612f-171">Bizonyosodjon meg, hogy az összes virtuális hálózatot az ExpressRoute-kapcsolatcsoportot hello megszüntetni ebben a példában futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="4612f-171">You must ensure that all virtual networks are unlinked from hello ExpressRoute circuit before running this example.</span></span> 
> 
> 

```powershell
Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

## <a name="azure-public-peering"></a><span data-ttu-id="4612f-172">Azure nyilvános társviszony-létesítés</span><span class="sxs-lookup"><span data-stu-id="4612f-172">Azure public peering</span></span>

<span data-ttu-id="4612f-173">Ez a szakasz segítséget nyújt a létrehozása, beolvasása, frissítése és hello Azure nyilvános társviszony-létesítési konfiguráció ExpressRoute-kapcsolatcsoportot törli.</span><span class="sxs-lookup"><span data-stu-id="4612f-173">This section helps you create, get, update, and delete hello Azure public peering configuration for an ExpressRoute circuit.</span></span>

### <a name="toocreate-azure-public-peering"></a><span data-ttu-id="4612f-174">az Azure nyilvános társviszony toocreate</span><span class="sxs-lookup"><span data-stu-id="4612f-174">toocreate Azure public peering</span></span>

1. <span data-ttu-id="4612f-175">ExpressRoute hello PowerShell modul importálása.</span><span class="sxs-lookup"><span data-stu-id="4612f-175">Import hello PowerShell module for ExpressRoute.</span></span>

  <span data-ttu-id="4612f-176">Telepítenie kell a hello legújabb PowerShell installer [PowerShell-galériában](http://www.powershellgallery.com/) és hello Azure Resource Manager modulok importálása hello PowerShell-munkamenetet a rendelés toostart hello ExpressRoute-parancsmagok használatával.</span><span class="sxs-lookup"><span data-stu-id="4612f-176">You must install hello latest PowerShell installer from [PowerShell Gallery](http://www.powershellgallery.com/) and import hello Azure Resource Manager modules into hello PowerShell session in order toostart using hello ExpressRoute cmdlets.</span></span> <span data-ttu-id="4612f-177">Rendszergazdaként kell toorun PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4612f-177">You will need toorun PowerShell as an Administrator.</span></span>

  ```powershell
  Install-Module AzureRM

  Install-AzureRM
```

  <span data-ttu-id="4612f-178">Importálja az összes hello AzureRM.* modulok hello ismert szemantikai verziója tartományon belül.</span><span class="sxs-lookup"><span data-stu-id="4612f-178">Import all of hello AzureRM.* modules within hello known semantic version range.</span></span>

  ```powershell
  Import-AzureRM
  ```

  <span data-ttu-id="4612f-179">Csak is importálhatja egy select modul hello ismert szemantikai verziója tartományon belül.</span><span class="sxs-lookup"><span data-stu-id="4612f-179">You can also just import a select module within hello known semantic version range.</span></span>

  ```powershell
  Import-Module AzureRM.Network
```

  <span data-ttu-id="4612f-180">Jelentkezzen be tooyour fiókjával.</span><span class="sxs-lookup"><span data-stu-id="4612f-180">Sign in tooyour account.</span></span>

  ```powershell
  Login-AzureRmAccount
  ```

  <span data-ttu-id="4612f-181">Válassza ki a kívánt toocreate ExpressRoute-kapcsolatcsoportot hello előfizetést.</span><span class="sxs-lookup"><span data-stu-id="4612f-181">Select hello subscription you want toocreate ExpressRoute circuit.</span></span>

  ```powershell
  Select-AzureRmSubscription -SubscriptionId "<subscription ID>"
  ```
2. <span data-ttu-id="4612f-182">Hozzon létre egy ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="4612f-182">Create an ExpressRoute circuit.</span></span>

  <span data-ttu-id="4612f-183">Kövesse az utasításokat toocreate hello egy [ExpressRoute-kapcsolatcsoportot](expressroute-howto-circuit-arm.md) , illetve hozta-e hello kapcsolat szolgáltatóját.</span><span class="sxs-lookup"><span data-stu-id="4612f-183">Follow hello instructions toocreate an [ExpressRoute circuit](expressroute-howto-circuit-arm.md) and have it provisioned by hello connectivity provider.</span></span>

  <span data-ttu-id="4612f-184">Ha a kapcsolat szolgáltatójánál 3. rétegbeli felügyelt szolgáltatásokat kínál, a kapcsolat szolgáltató tooenable magánhálózati társviszony-létesítést az Ön Azure kérhet.</span><span class="sxs-lookup"><span data-stu-id="4612f-184">If your connectivity provider offers managed Layer 3 services, you can request your connectivity provider tooenable Azure private peering for you.</span></span> <span data-ttu-id="4612f-185">Ebben az esetben nincs szükség hello következő szakaszokban szereplő toofollow utasításokat.</span><span class="sxs-lookup"><span data-stu-id="4612f-185">In that case, you won't need toofollow instructions listed in hello next sections.</span></span> <span data-ttu-id="4612f-186">Azonban ha a kapcsolat szolgáltatójánál nem kezeli az útválasztást, a kapcsolat létrehozása után továbbra is hello lépések a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="4612f-186">However, if your connectivity provider does not manage routing for you, after creating your circuit, continue your configuration using hello next steps.</span></span>
3. <span data-ttu-id="4612f-187">Ellenőrizze a hello ExpressRoute körön tooensure van üzembe helyezve, és is engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="4612f-187">Check hello ExpressRoute circuit tooensure it is provisioned and also enabled.</span></span> <span data-ttu-id="4612f-188">A következő példa hello használata:</span><span class="sxs-lookup"><span data-stu-id="4612f-188">Use hello following example:</span></span>

  ```powershell
  Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
  ```

  <span data-ttu-id="4612f-189">a rendszer a következő példa hasonló toohello hello választ:</span><span class="sxs-lookup"><span data-stu-id="4612f-189">hello response is similar toohello following example:</span></span>

  ```
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
  ```
4. <span data-ttu-id="4612f-190">Az Azure nyilvános társviszony-létesítés hello kör megadása</span><span class="sxs-lookup"><span data-stu-id="4612f-190">Configure Azure public peering for hello circuit.</span></span> <span data-ttu-id="4612f-191">Győződjön meg arról, hogy rendelkezik-e hello előtt végrehajtásának folytatásához a következő információkat.</span><span class="sxs-lookup"><span data-stu-id="4612f-191">Make sure that you have hello following information before you proceed further.</span></span>

  * <span data-ttu-id="4612f-192">/ 30-as alhálózat hello elsődleges kapcsolathoz.</span><span class="sxs-lookup"><span data-stu-id="4612f-192">A /30 subnet for hello primary link.</span></span> <span data-ttu-id="4612f-193">Ennek egy érvényes nyilvános IPv4-előtagnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="4612f-193">This must be a valid public IPv4 prefix.</span></span>
  * <span data-ttu-id="4612f-194">/ 30-as alhálózat hello másodlagos kapcsolathoz.</span><span class="sxs-lookup"><span data-stu-id="4612f-194">A /30 subnet for hello secondary link.</span></span> <span data-ttu-id="4612f-195">Ennek egy érvényes nyilvános IPv4-előtagnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="4612f-195">This must be a valid public IPv4 prefix.</span></span>
  * <span data-ttu-id="4612f-196">Egy érvényes VLAN-azonosító tooestablish a társviszony.</span><span class="sxs-lookup"><span data-stu-id="4612f-196">A valid VLAN ID tooestablish this peering on.</span></span> <span data-ttu-id="4612f-197">Győződjön meg arról, hogy egyetlen másik társviszonya se a hello áramkör használ hello azonos VLAN-azonosítót.</span><span class="sxs-lookup"><span data-stu-id="4612f-197">Ensure that no other peering in hello circuit uses hello same VLAN ID.</span></span>
  * <span data-ttu-id="4612f-198">Egy AS-szám a társviszony-létesítéshez.</span><span class="sxs-lookup"><span data-stu-id="4612f-198">AS number for peering.</span></span> <span data-ttu-id="4612f-199">2 és 4 bájtos AS-számokat is használhat.</span><span class="sxs-lookup"><span data-stu-id="4612f-199">You can use both 2-byte and 4-byte AS numbers.</span></span>
  * <span data-ttu-id="4612f-200">**Választható -** az MD5 kivonatoló, ha úgy dönt, hogy egy toouse.</span><span class="sxs-lookup"><span data-stu-id="4612f-200">**Optional -** An MD5 hash if you choose toouse one.</span></span>

  <span data-ttu-id="4612f-201">Futtassa a következő példa tooconfigure Azure nyilvános társviszony-létesítés a kör hello</span><span class="sxs-lookup"><span data-stu-id="4612f-201">Run hello following example tooconfigure Azure public peering for your circuit</span></span>

  ```powershell
  Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "12.0.0.0/30" -SecondaryPeerAddressPrefix "12.0.0.4/30" -VlanId 100

  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

  <span data-ttu-id="4612f-202">Ha úgy dönt, toouse az MD5 kivonatoló, használja a következő példa hello:</span><span class="sxs-lookup"><span data-stu-id="4612f-202">If you choose toouse an MD5 hash, use hello following example:</span></span>

  ```powershell
  Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "12.0.0.0/30" -SecondaryPeerAddressPrefix "12.0.0.4/30" -VlanId 100  -SharedKey "A1B2C3D4"

  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

  > [!IMPORTANT]
  > <span data-ttu-id="4612f-203">Az AS-számot mindenképp társviszony-létesítési ASN-ként, és ne ügyfél ASN-ként adja meg.</span><span class="sxs-lookup"><span data-stu-id="4612f-203">Ensure that you specify your AS number as peering ASN, not customer ASN.</span></span>
  > 
  >

### <a name="tooview-azure-public-peering-details"></a><span data-ttu-id="4612f-204">tooview Azure nyilvános társviszony-létesítés részletei</span><span class="sxs-lookup"><span data-stu-id="4612f-204">tooview Azure public peering details</span></span>

<span data-ttu-id="4612f-205">Konfigurációs részletek hello a következő parancsmag használatával kaphat:</span><span class="sxs-lookup"><span data-stu-id="4612f-205">You can get configuration details using hello following cmdlet:</span></span>

```powershell
  $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

  Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -Circuit $ckt
  ```

### <a name="tooupdate-azure-public-peering-configuration"></a><span data-ttu-id="4612f-206">az Azure nyilvános társviszony-létesítési konfiguráció tooupdate</span><span class="sxs-lookup"><span data-stu-id="4612f-206">tooupdate Azure public peering configuration</span></span>

<span data-ttu-id="4612f-207">Frissítheti, használja a következő példa hello hello konfiguráció bármely részeként.</span><span class="sxs-lookup"><span data-stu-id="4612f-207">You can update any part of hello configuration using hello following example.</span></span> <span data-ttu-id="4612f-208">Ebben a példában a 200 too600 hello VLAN-Azonosítót a kapcsolatcsoport hello frissül.</span><span class="sxs-lookup"><span data-stu-id="4612f-208">In this example, hello VLAN ID of hello circuit is being updated from 200 too600.</span></span>

```powershell
Set-AzureRmExpressRouteCircuitPeeringConfig  -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 600

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="toodelete-azure-public-peering"></a><span data-ttu-id="4612f-209">az Azure nyilvános társviszony toodelete</span><span class="sxs-lookup"><span data-stu-id="4612f-209">toodelete Azure public peering</span></span>

<span data-ttu-id="4612f-210">A társviszony-létesítési konfiguráció a következő példa hello futtatásával távolíthatja el:</span><span class="sxs-lookup"><span data-stu-id="4612f-210">You can remove your peering configuration by running hello following example:</span></span>

```powershell
Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

## <a name="microsoft-peering"></a><span data-ttu-id="4612f-211">Microsoft társviszony-létesítés</span><span class="sxs-lookup"><span data-stu-id="4612f-211">Microsoft peering</span></span>

<span data-ttu-id="4612f-212">Ez a szakasz segítséget nyújt a létrehozása, beolvasása, frissítése és törlése hello Microsoft társviszony-létesítési konfiguráció ExpressRoute-kör.</span><span class="sxs-lookup"><span data-stu-id="4612f-212">This section helps you create, get, update, and delete hello Microsoft peering configuration for an ExpressRoute circuit.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4612f-213">Előzetes tooAugust 1 Microsoft társviszony-létesítést az ExpressRoute-Kapcsolatcsoportok volt beállítva, akkor 2017 fog rendelkezni az összes szolgáltatás előtagok keresztül hello Microsoft társviszony-létesítést, meghirdetett, akkor is, ha nincsenek megadva útvonal szűrők.</span><span class="sxs-lookup"><span data-stu-id="4612f-213">Microsoft peering of ExpressRoute circuits that were configured prior tooAugust 1, 2017 will have all service prefixes advertised through hello Microsoft peering, even if route filters are not defined.</span></span> <span data-ttu-id="4612f-214">A Microsoft társviszony-létesítést az ExpressRoute-Kapcsolatcsoportok vannak konfigurálva, vagy azt követően 2017. augusztus 1. nem rendelkezik a előtagokat meghirdetett, amíg egy útvonal-szűrő nem csatlakoztatja toohello körön.</span><span class="sxs-lookup"><span data-stu-id="4612f-214">Microsoft peering of ExpressRoute circuits that are configured on or after August 1, 2017 will not have any prefixes advertised until a route filter is attached toohello circuit.</span></span> <span data-ttu-id="4612f-215">További információkért lásd: [konfigurálása a Microsoft társviszony-létesítéshez útvonal szűrő](how-to-routefilter-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="4612f-215">For more information, see [Configure a route filter for Microsoft peering](how-to-routefilter-powershell.md).</span></span>
> 
> 

### <a name="toocreate-microsoft-peering"></a><span data-ttu-id="4612f-216">toocreate Microsoft társviszony-létesítés</span><span class="sxs-lookup"><span data-stu-id="4612f-216">toocreate Microsoft peering</span></span>

1. <span data-ttu-id="4612f-217">ExpressRoute hello PowerShell modul importálása.</span><span class="sxs-lookup"><span data-stu-id="4612f-217">Import hello PowerShell module for ExpressRoute.</span></span>

  <span data-ttu-id="4612f-218">Telepítenie kell a hello legújabb PowerShell installer [PowerShell-galériában](http://www.powershellgallery.com/) és hello Azure Resource Manager modulok importálása hello PowerShell-munkamenetet a rendelés toostart hello ExpressRoute-parancsmagok használatával.</span><span class="sxs-lookup"><span data-stu-id="4612f-218">You must install hello latest PowerShell installer from [PowerShell Gallery](http://www.powershellgallery.com/) and import hello Azure Resource Manager modules into hello PowerShell session in order toostart using hello ExpressRoute cmdlets.</span></span> <span data-ttu-id="4612f-219">Rendszergazdaként kell toorun PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4612f-219">You will need toorun PowerShell as an Administrator.</span></span>

  ```powershell
  Install-Module AzureRM

  Install-AzureRM
  ```

  <span data-ttu-id="4612f-220">Importálja az összes hello AzureRM.* modulok hello ismert szemantikai verziója tartományon belül.</span><span class="sxs-lookup"><span data-stu-id="4612f-220">Import all of hello AzureRM.* modules within hello known semantic version range.</span></span>

  ```powershell
  Import-AzureRM
  ```

  <span data-ttu-id="4612f-221">Csak is importálhatja egy select modul hello ismert szemantikai verziója tartományon belül.</span><span class="sxs-lookup"><span data-stu-id="4612f-221">You can also just import a select module within hello known semantic version range.</span></span>

  ```powershell
  Import-Module AzureRM.Network
  ```

  <span data-ttu-id="4612f-222">Jelentkezzen be tooyour fiókjával.</span><span class="sxs-lookup"><span data-stu-id="4612f-222">Sign in tooyour account.</span></span>

  ```powershell
  Login-AzureRmAccount
  ```

  <span data-ttu-id="4612f-223">Válassza ki a kívánt toocreate ExpressRoute-kapcsolatcsoportot hello előfizetést.</span><span class="sxs-lookup"><span data-stu-id="4612f-223">Select hello subscription you want toocreate ExpressRoute circuit.</span></span>

  ```powershell
Select-AzureRmSubscription -SubscriptionId "<subscription ID>"
  ```
2. <span data-ttu-id="4612f-224">Hozzon létre egy ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="4612f-224">Create an ExpressRoute circuit.</span></span>

  <span data-ttu-id="4612f-225">Kövesse az utasításokat toocreate hello egy [ExpressRoute-kapcsolatcsoportot](expressroute-howto-circuit-arm.md) , illetve hozta-e hello kapcsolat szolgáltatóját.</span><span class="sxs-lookup"><span data-stu-id="4612f-225">Follow hello instructions toocreate an [ExpressRoute circuit](expressroute-howto-circuit-arm.md) and have it provisioned by hello connectivity provider.</span></span>

  <span data-ttu-id="4612f-226">Ha a kapcsolat szolgáltatójánál 3. rétegbeli felügyelt szolgáltatásokat kínál, a kapcsolat szolgáltató tooenable magánhálózati társviszony-létesítést az Ön Azure kérhet.</span><span class="sxs-lookup"><span data-stu-id="4612f-226">If your connectivity provider offers managed Layer 3 services, you can request your connectivity provider tooenable Azure private peering for you.</span></span> <span data-ttu-id="4612f-227">Ebben az esetben nincs szükség hello következő szakaszokban szereplő toofollow utasításokat.</span><span class="sxs-lookup"><span data-stu-id="4612f-227">In that case, you won't need toofollow instructions listed in hello next sections.</span></span> <span data-ttu-id="4612f-228">Azonban ha a kapcsolat szolgáltatójánál nem kezeli az útválasztást, a kapcsolat létrehozása után továbbra is hello lépések a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="4612f-228">However, if your connectivity provider does not manage routing for you, after creating your circuit, continue your configuration using hello next steps.</span></span>
3. <span data-ttu-id="4612f-229">Ellenőrizze a hello ExpressRoute körön toomake arról, hogy van üzembe helyezve és is engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="4612f-229">Check hello ExpressRoute circuit toomake sure it is provisioned and also enabled.</span></span> <span data-ttu-id="4612f-230">A következő példa hello használata:</span><span class="sxs-lookup"><span data-stu-id="4612f-230">Use hello following example:</span></span>

  ```powershell
  Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
  ```

  <span data-ttu-id="4612f-231">a rendszer a következő példa hasonló toohello hello választ:</span><span class="sxs-lookup"><span data-stu-id="4612f-231">hello response is similar toohello following example:</span></span>

  ```
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
  ```
4. <span data-ttu-id="4612f-232">Konfigurálja a Microsoft hello-kör társviszony-létesítés.</span><span class="sxs-lookup"><span data-stu-id="4612f-232">Configure Microsoft peering for hello circuit.</span></span> <span data-ttu-id="4612f-233">Győződjön meg arról, hogy rendelkezik-e folytatni a következő információk előtt hello.</span><span class="sxs-lookup"><span data-stu-id="4612f-233">Make sure that you have hello following information before you proceed.</span></span>

  * <span data-ttu-id="4612f-234">/ 30-as alhálózat hello elsődleges kapcsolathoz.</span><span class="sxs-lookup"><span data-stu-id="4612f-234">A /30 subnet for hello primary link.</span></span> <span data-ttu-id="4612f-235">Ennek egy érvényes nyilvános IPv4-előtagnak kell lennie, amely az Ön tulajdonában van, és regisztrálva van egy RIR-/IRR-jegyzékben.</span><span class="sxs-lookup"><span data-stu-id="4612f-235">This must be a valid public IPv4 prefix owned by you and registered in an RIR / IRR.</span></span>
  * <span data-ttu-id="4612f-236">/ 30-as alhálózat hello másodlagos kapcsolathoz.</span><span class="sxs-lookup"><span data-stu-id="4612f-236">A /30 subnet for hello secondary link.</span></span> <span data-ttu-id="4612f-237">Ennek egy érvényes nyilvános IPv4-előtagnak kell lennie, amely az Ön tulajdonában van, és regisztrálva van egy RIR-/IRR-jegyzékben.</span><span class="sxs-lookup"><span data-stu-id="4612f-237">This must be a valid public IPv4 prefix owned by you and registered in an RIR / IRR.</span></span>
  * <span data-ttu-id="4612f-238">Egy érvényes VLAN-azonosító tooestablish a társviszony.</span><span class="sxs-lookup"><span data-stu-id="4612f-238">A valid VLAN ID tooestablish this peering on.</span></span> <span data-ttu-id="4612f-239">Győződjön meg arról, hogy egyetlen másik társviszonya se a hello áramkör használ hello azonos VLAN-azonosítót.</span><span class="sxs-lookup"><span data-stu-id="4612f-239">Ensure that no other peering in hello circuit uses hello same VLAN ID.</span></span>
  * <span data-ttu-id="4612f-240">Egy AS-szám a társviszony-létesítéshez.</span><span class="sxs-lookup"><span data-stu-id="4612f-240">AS number for peering.</span></span> <span data-ttu-id="4612f-241">2 és 4 bájtos AS-számokat is használhat.</span><span class="sxs-lookup"><span data-stu-id="4612f-241">You can use both 2-byte and 4-byte AS numbers.</span></span>
  * <span data-ttu-id="4612f-242">Meghirdetett előtagok: meg kell adnia a lista az összes előtagok hello BGP munkameneten keresztül tervezi tooadvertise.</span><span class="sxs-lookup"><span data-stu-id="4612f-242">Advertised prefixes: You must provide a list of all prefixes you plan tooadvertise over hello BGP session.</span></span> <span data-ttu-id="4612f-243">A rendszer kizárólag a nyilvános IP-cím-előtagokat fogadja el.</span><span class="sxs-lookup"><span data-stu-id="4612f-243">Only public IP address prefixes are accepted.</span></span> <span data-ttu-id="4612f-244">Ha azt tervezi, toosend előtagok készlete, elküldheti a vesszővel tagolt listáját.</span><span class="sxs-lookup"><span data-stu-id="4612f-244">If you plan toosend a set of prefixes, you can send a comma-separated list.</span></span> <span data-ttu-id="4612f-245">Ezeket az előtagokat kell lenniük egy RIR-ben regisztrált tooyou vagy IRR-ben.</span><span class="sxs-lookup"><span data-stu-id="4612f-245">These prefixes must be registered tooyou in an RIR / IRR.</span></span>
  * <span data-ttu-id="4612f-246">**Választható -** ügyfél ASN: Ha hirdetési-előtagok nem regisztrált toohello társviszony-létesítés SZÁMOT, megadhatja hello számú toowhich regisztrálva vannak.</span><span class="sxs-lookup"><span data-stu-id="4612f-246">**Optional -** Customer ASN: If you are advertising prefixes that are not registered toohello peering AS number, you can specify hello AS number toowhich they are registered.</span></span>
  * <span data-ttu-id="4612f-247">Útválasztási beállításjegyzék-név: Hello RIR megadhat / BMR mely hello ellen, számát, és előtagok regisztrálva van.</span><span class="sxs-lookup"><span data-stu-id="4612f-247">Routing Registry Name: You can specify hello RIR / IRR against which hello AS number and prefixes are registered.</span></span>
  * <span data-ttu-id="4612f-248">**Választható -** az MD5 kivonatoló, ha úgy dönt, hogy egy toouse.</span><span class="sxs-lookup"><span data-stu-id="4612f-248">**Optional -** An MD5 hash if you choose toouse one.</span></span>

   <span data-ttu-id="4612f-249">A következő példa tooconfigure Microsoft társviszony-létesítés a kör hello használata:</span><span class="sxs-lookup"><span data-stu-id="4612f-249">Use hello following example tooconfigure Microsoft peering for your circuit:</span></span>

  ```powershell
  Add-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 300 -MicrosoftConfigAdvertisedPublicPrefixes "123.1.0.0/24" -MicrosoftConfigCustomerAsn 23 -MicrosoftConfigRoutingRegistryName "ARIN"

  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

### <a name="tooget-microsoft-peering-details"></a><span data-ttu-id="4612f-250">tooget Microsoft társviszony-létesítési részletei</span><span class="sxs-lookup"><span data-stu-id="4612f-250">tooget Microsoft peering details</span></span>

<span data-ttu-id="4612f-251">Kaphat a konfigurációs adatait a következő példa hello használata:</span><span class="sxs-lookup"><span data-stu-id="4612f-251">You can get configuration details using hello following example:</span></span>

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

Get-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt
```

### <a name="tooupdate-microsoft-peering-configuration"></a><span data-ttu-id="4612f-252">a Microsoft társviszony-létesítési konfiguráció tooupdate</span><span class="sxs-lookup"><span data-stu-id="4612f-252">tooupdate Microsoft peering configuration</span></span>

<span data-ttu-id="4612f-253">Frissítheti, használja a következő példa hello hello konfiguráció bármely részeként:</span><span class="sxs-lookup"><span data-stu-id="4612f-253">You can update any part of hello configuration using hello following example:</span></span>

```powershell
Set-AzureRmExpressRouteCircuitPeeringConfig  -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 300 -MicrosoftConfigAdvertisedPublicPrefixes "124.1.0.0/24" -MicrosoftConfigCustomerAsn 23 -MicrosoftConfigRoutingRegistryName "ARIN"

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="toodelete-microsoft-peering"></a><span data-ttu-id="4612f-254">toodelete Microsoft társviszony-létesítés</span><span class="sxs-lookup"><span data-stu-id="4612f-254">toodelete Microsoft peering</span></span>

<span data-ttu-id="4612f-255">A társviszony-létesítési konfiguráció hello a következő parancsmag futtatásával távolíthatja el:</span><span class="sxs-lookup"><span data-stu-id="4612f-255">You can remove your peering configuration by running hello following cmdlet:</span></span>

```powershell
Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

## <a name="next-steps"></a><span data-ttu-id="4612f-256">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4612f-256">Next steps</span></span>

<span data-ttu-id="4612f-257">Következő lépés, [csatolni a virtuális hálózat tooan ExpressRoute-kapcsolatcsoportot](expressroute-howto-linkvnet-arm.md).</span><span class="sxs-lookup"><span data-stu-id="4612f-257">Next step, [Link a VNet tooan ExpressRoute circuit](expressroute-howto-linkvnet-arm.md).</span></span>

* <span data-ttu-id="4612f-258">Az ExpressRoute-munkafolyamatokkal kapcsolatos további információkért lásd: [ExpressRoute workflows](expressroute-workflows.md) (ExpressRoute-munkafolyamatok).</span><span class="sxs-lookup"><span data-stu-id="4612f-258">For more information about ExpressRoute workflows, see [ExpressRoute workflows](expressroute-workflows.md).</span></span>
* <span data-ttu-id="4612f-259">A kapcsolatcsoportok társviszony-létesítéseivel kapcsolatos további információkért lásd: [ExpressRoute circuits and routing domains](expressroute-circuit-peerings.md) (ExpressRoute-kapcsolatcsoportok és útválasztási tartományok).</span><span class="sxs-lookup"><span data-stu-id="4612f-259">For more information about circuit peering, see [ExpressRoute circuits and routing domains](expressroute-circuit-peerings.md).</span></span>
* <span data-ttu-id="4612f-260">További információkért a virtuális hálózatok használatáról lásd: [Virtual network overview](../virtual-network/virtual-networks-overview.md) (Virtuális hálózatok áttekintése).</span><span class="sxs-lookup"><span data-stu-id="4612f-260">For more information about working with virtual networks, see [Virtual network overview](../virtual-network/virtual-networks-overview.md).</span></span>
