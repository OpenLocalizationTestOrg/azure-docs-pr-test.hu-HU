---
title: "Útválasztási (társviszony-létesítés) a egy ExpressRoute-áramkör konfigurálása: erőforrás-kezelő: PowerShell: Azure |} Microsoft Docs"
description: "A cikk az ExpressRoute-kapcsolatcsoportok privát, nyilvános és Microsoft társviszony-létesítéses létrehozásának és kiépítésének lépéseit ismerteti. A cikk azt is bemutatja, hogyan ellenőrizheti a kapcsolatcsoport társviszonyainak állapotát, illetve hogyan frissítheti vagy törölheti őket."
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
ms.openlocfilehash: af68955b78239832e413e1b59e033d7d3da8d599
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="create-and-modify-peering-for-an-expressroute-circuit-using-powershell"></a><span data-ttu-id="d68ba-104">Létrehozása és módosítása a powershellel ExpressRoute-kör társviszony</span><span class="sxs-lookup"><span data-stu-id="d68ba-104">Create and modify peering for an ExpressRoute circuit using PowerShell</span></span>

<span data-ttu-id="d68ba-105">Ez a cikk segítséget nyújt a létrehozása és kezelése a PowerShell használatával Resource Manager üzembe helyezési modellel ExpressRoute-kapcsolatcsoportot útválasztási konfigurációja.</span><span class="sxs-lookup"><span data-stu-id="d68ba-105">This article helps you create and manage routing configuration for an ExpressRoute circuit in the Resource Manager deployment model using PowerShell.</span></span> <span data-ttu-id="d68ba-106">Ellenőrizze az állapot, frissítési vagy törlési is, és az ExpressRoute-kör társviszony kiosztásának megszüntetése.</span><span class="sxs-lookup"><span data-stu-id="d68ba-106">You can also check the status, update, or delete and deprovision peerings for an ExpressRoute circuit.</span></span> <span data-ttu-id="d68ba-107">Ha más módszert használja a kapcsolatcsoport dolgozni szeretne, válassza ki egy cikk az alábbi listából:</span><span class="sxs-lookup"><span data-stu-id="d68ba-107">If you want to use a different method to work with your circuit, select an article from the following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="d68ba-108">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="d68ba-108">Azure portal</span></span>](expressroute-howto-routing-portal-resource-manager.md)
> * [<span data-ttu-id="d68ba-109">PowerShell</span><span class="sxs-lookup"><span data-stu-id="d68ba-109">PowerShell</span></span>](expressroute-howto-routing-arm.md)
> * [<span data-ttu-id="d68ba-110">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="d68ba-110">Azure CLI</span></span>](howto-routing-cli.md)
> * [<span data-ttu-id="d68ba-111">Video - privát társviszony-létesítés</span><span class="sxs-lookup"><span data-stu-id="d68ba-111">Video - Private peering</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-private-peering-for-your-expressroute-circuit)
> * [<span data-ttu-id="d68ba-112">Video - nyilvános társviszony-létesítés</span><span class="sxs-lookup"><span data-stu-id="d68ba-112">Video - Public peering</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-public-peering-for-your-expressroute-circuit)
> * [<span data-ttu-id="d68ba-113">Videó – a Microsoft társviszony-létesítés</span><span class="sxs-lookup"><span data-stu-id="d68ba-113">Video - Microsoft peering</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-microsoft-peering-for-your-expressroute-circuit)
> * [<span data-ttu-id="d68ba-114">PowerShell (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="d68ba-114">PowerShell (classic)</span></span>](expressroute-howto-routing-classic.md)
> 

## <a name="configuration-prerequisites"></a><span data-ttu-id="d68ba-115">Konfigurációs előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d68ba-115">Configuration prerequisites</span></span>

* <span data-ttu-id="d68ba-116">Szüksége lesz az Azure Resource Manager PowerShell-parancsmagok legújabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="d68ba-116">You will need the latest version of the Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="d68ba-117">További információt [az Azure PowerShell telepítésével és konfigurálásával](/powershell/azure/overview) foglalkozó témakörben talál.</span><span class="sxs-lookup"><span data-stu-id="d68ba-117">For more information, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span> 
* <span data-ttu-id="d68ba-118">A konfigurálás megkezdése előtt mindenképp tekintse át az [előfeltételek](expressroute-prerequisites.md), az [útválasztási követelmények](expressroute-routing.md) és a [munkafolyamatok](expressroute-workflows.md) lapot.</span><span class="sxs-lookup"><span data-stu-id="d68ba-118">Make sure that you have reviewed the [prerequisites](expressroute-prerequisites.md) page, the [routing requirements](expressroute-routing.md) page, and the [workflows](expressroute-workflows.md) page before you begin configuration.</span></span>
* <span data-ttu-id="d68ba-119">Egy aktív ExpressRoute-kapcsolatcsoportra lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="d68ba-119">You must have an active ExpressRoute circuit.</span></span> <span data-ttu-id="d68ba-120">Kövesse az [ExpressRoute-kapcsolatcsoport létrehozása](expressroute-howto-circuit-arm.md) részben foglalt lépéseket, és engedélyeztesse a kapcsolatcsoportot kapcsolatszolgáltatójával, mielőtt továbblépne.</span><span class="sxs-lookup"><span data-stu-id="d68ba-120">Follow the instructions to [Create an ExpressRoute circuit](expressroute-howto-circuit-arm.md) and have the circuit enabled by your connectivity provider before you proceed.</span></span> <span data-ttu-id="d68ba-121">Az ExpressRoute-kapcsolatcsoport ahhoz, hogy ebben a cikkben a parancsmagok futtatásához kiépített és engedélyezett állapotban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="d68ba-121">The ExpressRoute circuit must be in a provisioned and enabled state for you to be able to run the cmdlets in this article.</span></span>

<span data-ttu-id="d68ba-122">Az utasítások csak 2. rétegbeli kapcsolatszolgáltatásokat kínáló szolgáltatóknál létrehozott kapcsolatcsoportokra vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="d68ba-122">These instructions only apply to circuits created with service providers offering Layer 2 connectivity services.</span></span> <span data-ttu-id="d68ba-123">A szolgáltató által kezelt használata réteg (általában egy IPVPN, például az MPLS) 3 szolgáltatások, a kapcsolat szolgáltatójánál konfigurálása és kezelése az Ön útválasztást.</span><span class="sxs-lookup"><span data-stu-id="d68ba-123">If you are using a service provider that offers managed Layer 3 services (typically an IPVPN, like MPLS), your connectivity provider will configure and manage routing for you.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d68ba-124">A szolgáltatásfelügyeleti portálon jelenleg nem hirdetünk szolgáltatók által konfigurált társviszony-létesítéseket.</span><span class="sxs-lookup"><span data-stu-id="d68ba-124">We currently do not advertise peerings configured by service providers through the service management portal.</span></span> <span data-ttu-id="d68ba-125">Dolgozunk azon, hogy hamarosan bevezethessük ezt a képességet.</span><span class="sxs-lookup"><span data-stu-id="d68ba-125">We are working on enabling this capability soon.</span></span> <span data-ttu-id="d68ba-126">A BGP társviszony konfigurálása előtt ellenőrizze a szolgáltató.</span><span class="sxs-lookup"><span data-stu-id="d68ba-126">Check with your service provider before configuring BGP peerings.</span></span>
> 
> 

<span data-ttu-id="d68ba-127">Egy, két vagy akár mindhárom társviszony-létesítést (Azure privát, Azure nyilvános és Microsoft) is konfigurálhatja egy adott ExpressRoute-kapcsolatcsoportban.</span><span class="sxs-lookup"><span data-stu-id="d68ba-127">You can configure one, two, or all three peerings (Azure private, Azure public and Microsoft) for an ExpressRoute circuit.</span></span> <span data-ttu-id="d68ba-128">A társviszony-létesítéseket tetszőleges sorrendben konfigurálhatja.</span><span class="sxs-lookup"><span data-stu-id="d68ba-128">You can configure peerings in any order you choose.</span></span> <span data-ttu-id="d68ba-129">Az egyes társviszony-létesítéseket azonban mindenképp egyenként kell végrehajtania.</span><span class="sxs-lookup"><span data-stu-id="d68ba-129">However, you must make sure that you complete the configuration of each peering one at a time.</span></span> 

## <a name="azure-private-peering"></a><span data-ttu-id="d68ba-130">Azure privát társviszony-létesítés</span><span class="sxs-lookup"><span data-stu-id="d68ba-130">Azure private peering</span></span>

<span data-ttu-id="d68ba-131">Ez a szakasz segítséget nyújt a létrehozása, beolvasása, frissítése és törölni az Azure magánhálózati társviszony-létesítési ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="d68ba-131">This section helps you create, get, update, and delete the Azure private peering configuration for an ExpressRoute circuit.</span></span>

### <a name="to-create-azure-private-peering"></a><span data-ttu-id="d68ba-132">Azure privát társviszony-létesítés létrehozása</span><span class="sxs-lookup"><span data-stu-id="d68ba-132">To create Azure private peering</span></span>

1. <span data-ttu-id="d68ba-133">Importálja az ExpressRoute PowerShell-modulját.</span><span class="sxs-lookup"><span data-stu-id="d68ba-133">Import the PowerShell module for ExpressRoute.</span></span>

  <span data-ttu-id="d68ba-134">Telepítse a legújabb PowerShell-telepítőt a [PowerShell-galériából](http://www.powershellgallery.com/), és importálja az Azure Resource Manager-modulokat a PowerShell-munkamenetbe az ExpressRoute-parancsmagok használatának elkezdéséhez.</span><span class="sxs-lookup"><span data-stu-id="d68ba-134">You must install the latest PowerShell installer from [PowerShell Gallery](http://www.powershellgallery.com/) and import the Azure Resource Manager modules into the PowerShell session in order to start using the ExpressRoute cmdlets.</span></span> <span data-ttu-id="d68ba-135">A PowerShellt rendszergazdaként kell futtatnia.</span><span class="sxs-lookup"><span data-stu-id="d68ba-135">You will need to run PowerShell as an Administrator.</span></span>

  ```powershell
  Install-Module AzureRM
  Install-AzureRM
  ```

  <span data-ttu-id="d68ba-136">Importálja a AzureRM.* modulok ismert szemantikai verziója tartományon belül.</span><span class="sxs-lookup"><span data-stu-id="d68ba-136">Import all of the AzureRM.* modules within the known semantic version range.</span></span>

  ```powershell
  Import-AzureRM
  ```

  <span data-ttu-id="d68ba-137">Az ismert szemantikai verziója tartományon belüli select modul csak is importálhatja.</span><span class="sxs-lookup"><span data-stu-id="d68ba-137">You can also just import a select module within the known semantic version range.</span></span>

  ```powershell
  Import-Module AzureRM.Network 
  ```

  <span data-ttu-id="d68ba-138">Jelentkezzen be a fiókjába.</span><span class="sxs-lookup"><span data-stu-id="d68ba-138">Sign in to your account.</span></span>

  ```powershell
  Login-AzureRmAccount
  ```

  <span data-ttu-id="d68ba-139">Válassza ki az előfizetést, ExpressRoute-kapcsolatcsoportot létrehozni kívánja.</span><span class="sxs-lookup"><span data-stu-id="d68ba-139">Select the subscription you want to create ExpressRoute circuit.</span></span>

  ```powershell
  Select-AzureRmSubscription -SubscriptionId "<subscription ID>"
  ```
2. <span data-ttu-id="d68ba-140">Hozzon létre egy ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="d68ba-140">Create an ExpressRoute circuit.</span></span>

  <span data-ttu-id="d68ba-141">Kövesse az utasításokat az [ExpressRoute-kapcsolatcsoport](expressroute-howto-circuit-arm.md) létrehozásához, és kérje meg kapcsolatszolgáltatóját, hogy ossza ki azt.</span><span class="sxs-lookup"><span data-stu-id="d68ba-141">Follow the instructions to create an [ExpressRoute circuit](expressroute-howto-circuit-arm.md) and have it provisioned by the connectivity provider.</span></span>

  <span data-ttu-id="d68ba-142">Ha kapcsolatszolgáltatója felügyelt 3. rétegbeli szolgáltatásokat kínál, igényelheti tőle, hogy engedélyezze Önnek az Azure privát társviszony-létesítést.</span><span class="sxs-lookup"><span data-stu-id="d68ba-142">If your connectivity provider offers managed Layer 3 services, you can request your connectivity provider to enable Azure private peering for you.</span></span> <span data-ttu-id="d68ba-143">Ebben az esetben nem szükséges a következő szakaszokban foglalt lépéseket végrehajtania.</span><span class="sxs-lookup"><span data-stu-id="d68ba-143">In that case, you won't need to follow instructions listed in the next sections.</span></span> <span data-ttu-id="d68ba-144">Azonban ha a kapcsolat szolgáltatójánál nem kezeli az útválasztást, a kapcsolat létrehozása után továbbra is a konfiguráció a következő lépéseket.</span><span class="sxs-lookup"><span data-stu-id="d68ba-144">However, if your connectivity provider does not manage routing for you, after creating your circuit, continue your configuration using the next steps.</span></span>
3. <span data-ttu-id="d68ba-145">Ellenőrizze, hogy üzembe helyezve, és is engedélyezve van az ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="d68ba-145">Check the ExpressRoute circuit to make sure it is provisioned and also enabled.</span></span> <span data-ttu-id="d68ba-146">Használja a következő példát:</span><span class="sxs-lookup"><span data-stu-id="d68ba-146">Use the following example:</span></span>

  ```powershell
  Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
  ```

  <span data-ttu-id="d68ba-147">A rendszer a választ az alábbi példához hasonló:</span><span class="sxs-lookup"><span data-stu-id="d68ba-147">The response is similar to the following example:</span></span>

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
4. <span data-ttu-id="d68ba-148">Konfigurálja az Azure privát társviszony-létesítést a kapcsolatcsoport számára.</span><span class="sxs-lookup"><span data-stu-id="d68ba-148">Configure Azure private peering for the circuit.</span></span> <span data-ttu-id="d68ba-149">Mielőtt folytatná a következő lépésekkel, ellenőrizze az alábbi elemek meglétét:</span><span class="sxs-lookup"><span data-stu-id="d68ba-149">Make sure that you have the following items before you proceed with the next steps:</span></span>

  * <span data-ttu-id="d68ba-150">Egy /30 alhálózat az elsődleges kapcsolat számára.</span><span class="sxs-lookup"><span data-stu-id="d68ba-150">A /30 subnet for the primary link.</span></span> <span data-ttu-id="d68ba-151">Az alhálózat nem virtuális hálózatok számára fenntartott címtartomány részének kell lennie.</span><span class="sxs-lookup"><span data-stu-id="d68ba-151">The subnet must not be part of any address space reserved for virtual networks.</span></span>
  * <span data-ttu-id="d68ba-152">Egy /30 alhálózat a másodlagos kapcsolat számára.</span><span class="sxs-lookup"><span data-stu-id="d68ba-152">A /30 subnet for the secondary link.</span></span> <span data-ttu-id="d68ba-153">Az alhálózat nem virtuális hálózatok számára fenntartott címtartomány részének kell lennie.</span><span class="sxs-lookup"><span data-stu-id="d68ba-153">The subnet must not be part of any address space reserved for virtual networks.</span></span>
  * <span data-ttu-id="d68ba-154">Egy érvényes VLAN-azonosító a tárviszony-létesítés létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="d68ba-154">A valid VLAN ID to establish this peering on.</span></span> <span data-ttu-id="d68ba-155">Győződjön meg róla, hogy a kapcsolatcsoporton egy másik társviszony-létesítés sem használja ugyanezt a VLAN-azonosítót.</span><span class="sxs-lookup"><span data-stu-id="d68ba-155">Ensure that no other peering in the circuit uses the same VLAN ID.</span></span>
  * <span data-ttu-id="d68ba-156">Egy AS-szám a társviszony-létesítéshez.</span><span class="sxs-lookup"><span data-stu-id="d68ba-156">AS number for peering.</span></span> <span data-ttu-id="d68ba-157">2 és 4 bájtos AS-számokat is használhat.</span><span class="sxs-lookup"><span data-stu-id="d68ba-157">You can use both 2-byte and 4-byte AS numbers.</span></span> <span data-ttu-id="d68ba-158">Ehhez a társviszony-létesítéshez használhat privát AS-számokat is.</span><span class="sxs-lookup"><span data-stu-id="d68ba-158">You can use a private AS number for this peering.</span></span> <span data-ttu-id="d68ba-159">Ne használja a 65515 számot.</span><span class="sxs-lookup"><span data-stu-id="d68ba-159">Ensure that you are not using 65515.</span></span>
  * <span data-ttu-id="d68ba-160">**Választható -** az MD5 kivonatoló, ha úgy dönt, hogy egyetlen módszer alkalmazása.</span><span class="sxs-lookup"><span data-stu-id="d68ba-160">**Optional -** An MD5 hash if you choose to use one.</span></span>

  <span data-ttu-id="d68ba-161">Az alábbi példát követve Azure magánhálózati társviszony-létesítés a kapcsolat konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="d68ba-161">Use the following example to configure Azure private peering for your circuit:</span></span>

  ```powershell
  Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200

  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

  <span data-ttu-id="d68ba-162">Ha az MD5 kivonatoló használatát választja, használja a következő példát:</span><span class="sxs-lookup"><span data-stu-id="d68ba-162">If you choose to use an MD5 hash, use the following example:</span></span>

  ```powershell
  Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200  -SharedKey "A1B2C3D4"
  ```

  > [!IMPORTANT]
  > <span data-ttu-id="d68ba-163">Az AS-számot mindenképp társviszony-létesítési ASN-ként, és ne ügyfél ASN-ként adja meg.</span><span class="sxs-lookup"><span data-stu-id="d68ba-163">Ensure that you specify your AS number as peering ASN, not customer ASN.</span></span>
  > 
  >

### <a name="to-view-azure-private-peering-details"></a><span data-ttu-id="d68ba-164">Azure privát társviszony-létesítés részleteinek megtekintése</span><span class="sxs-lookup"><span data-stu-id="d68ba-164">To view Azure private peering details</span></span>

<span data-ttu-id="d68ba-165">Konfigurációs részletek kaphat az alábbi példa:</span><span class="sxs-lookup"><span data-stu-id="d68ba-165">You can get configuration details by using the following example:</span></span>

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -Circuit $ckt
```

### <a name="to-update-azure-private-peering-configuration"></a><span data-ttu-id="d68ba-166">Azure privát társviszony-létesítés konfigurációjának frissítése</span><span class="sxs-lookup"><span data-stu-id="d68ba-166">To update Azure private peering configuration</span></span>

<span data-ttu-id="d68ba-167">Frissítheti, az alábbi példa konfiguráció bármely részeként.</span><span class="sxs-lookup"><span data-stu-id="d68ba-167">You can update any part of the configuration using the following example.</span></span> <span data-ttu-id="d68ba-168">Ebben a példában a VLAN-Azonosítót a áramkör frissül 100 500.</span><span class="sxs-lookup"><span data-stu-id="d68ba-168">In this example, the VLAN ID of the circuit is being updated from 100 to 500.</span></span>

```powershell
Set-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="to-delete-azure-private-peering"></a><span data-ttu-id="d68ba-169">Azure privát társviszony-létesítés törlése</span><span class="sxs-lookup"><span data-stu-id="d68ba-169">To delete Azure private peering</span></span>

<span data-ttu-id="d68ba-170">Futtassa a következő példa a társviszony-létesítési konfiguráció eltávolítása:</span><span class="sxs-lookup"><span data-stu-id="d68ba-170">You can remove your peering configuration by running the following example:</span></span>

> [!WARNING]
> <span data-ttu-id="d68ba-171">Bizonyosodjon meg, hogy az összes virtuális hálózatot megszüntetni az ExpressRoute-kapcsolatcsoport az ebben a példában futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="d68ba-171">You must ensure that all virtual networks are unlinked from the ExpressRoute circuit before running this example.</span></span> 
> 
> 

```powershell
Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

## <a name="azure-public-peering"></a><span data-ttu-id="d68ba-172">Azure nyilvános társviszony-létesítés</span><span class="sxs-lookup"><span data-stu-id="d68ba-172">Azure public peering</span></span>

<span data-ttu-id="d68ba-173">Ez a szakasz segítséget nyújt a létrehozása, beolvasása, frissítése és törlése az Azure nyilvános társviszony-létesítési konfiguráció ExpressRoute-kör.</span><span class="sxs-lookup"><span data-stu-id="d68ba-173">This section helps you create, get, update, and delete the Azure public peering configuration for an ExpressRoute circuit.</span></span>

### <a name="to-create-azure-public-peering"></a><span data-ttu-id="d68ba-174">Azure nyilvános társviszony-létesítés létrehozása</span><span class="sxs-lookup"><span data-stu-id="d68ba-174">To create Azure public peering</span></span>

1. <span data-ttu-id="d68ba-175">Importálja az ExpressRoute PowerShell-modulját.</span><span class="sxs-lookup"><span data-stu-id="d68ba-175">Import the PowerShell module for ExpressRoute.</span></span>

  <span data-ttu-id="d68ba-176">Telepítse a legújabb PowerShell-telepítőt a [PowerShell-galériából](http://www.powershellgallery.com/), és importálja az Azure Resource Manager-modulokat a PowerShell-munkamenetbe az ExpressRoute-parancsmagok használatának elkezdéséhez.</span><span class="sxs-lookup"><span data-stu-id="d68ba-176">You must install the latest PowerShell installer from [PowerShell Gallery](http://www.powershellgallery.com/) and import the Azure Resource Manager modules into the PowerShell session in order to start using the ExpressRoute cmdlets.</span></span> <span data-ttu-id="d68ba-177">A PowerShellt rendszergazdaként kell futtatnia.</span><span class="sxs-lookup"><span data-stu-id="d68ba-177">You will need to run PowerShell as an Administrator.</span></span>

  ```powershell
  Install-Module AzureRM

  Install-AzureRM
```

  <span data-ttu-id="d68ba-178">Importálja a AzureRM.* modulok ismert szemantikai verziója tartományon belül.</span><span class="sxs-lookup"><span data-stu-id="d68ba-178">Import all of the AzureRM.* modules within the known semantic version range.</span></span>

  ```powershell
  Import-AzureRM
  ```

  <span data-ttu-id="d68ba-179">Az ismert szemantikai verziója tartományon belüli select modul csak is importálhatja.</span><span class="sxs-lookup"><span data-stu-id="d68ba-179">You can also just import a select module within the known semantic version range.</span></span>

  ```powershell
  Import-Module AzureRM.Network
```

  <span data-ttu-id="d68ba-180">Jelentkezzen be a fiókjába.</span><span class="sxs-lookup"><span data-stu-id="d68ba-180">Sign in to your account.</span></span>

  ```powershell
  Login-AzureRmAccount
  ```

  <span data-ttu-id="d68ba-181">Válassza ki az előfizetést, ExpressRoute-kapcsolatcsoportot létrehozni kívánja.</span><span class="sxs-lookup"><span data-stu-id="d68ba-181">Select the subscription you want to create ExpressRoute circuit.</span></span>

  ```powershell
  Select-AzureRmSubscription -SubscriptionId "<subscription ID>"
  ```
2. <span data-ttu-id="d68ba-182">Hozzon létre egy ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="d68ba-182">Create an ExpressRoute circuit.</span></span>

  <span data-ttu-id="d68ba-183">Kövesse az utasításokat az [ExpressRoute-kapcsolatcsoport](expressroute-howto-circuit-arm.md) létrehozásához, és kérje meg kapcsolatszolgáltatóját, hogy ossza ki azt.</span><span class="sxs-lookup"><span data-stu-id="d68ba-183">Follow the instructions to create an [ExpressRoute circuit](expressroute-howto-circuit-arm.md) and have it provisioned by the connectivity provider.</span></span>

  <span data-ttu-id="d68ba-184">Ha kapcsolatszolgáltatója felügyelt 3. rétegbeli szolgáltatásokat kínál, igényelheti tőle, hogy engedélyezze Önnek az Azure privát társviszony-létesítést.</span><span class="sxs-lookup"><span data-stu-id="d68ba-184">If your connectivity provider offers managed Layer 3 services, you can request your connectivity provider to enable Azure private peering for you.</span></span> <span data-ttu-id="d68ba-185">Ebben az esetben nem szükséges a következő szakaszokban foglalt lépéseket végrehajtania.</span><span class="sxs-lookup"><span data-stu-id="d68ba-185">In that case, you won't need to follow instructions listed in the next sections.</span></span> <span data-ttu-id="d68ba-186">Azonban ha a kapcsolat szolgáltatójánál nem kezeli az útválasztást, a kapcsolat létrehozása után továbbra is a konfiguráció a következő lépéseket.</span><span class="sxs-lookup"><span data-stu-id="d68ba-186">However, if your connectivity provider does not manage routing for you, after creating your circuit, continue your configuration using the next steps.</span></span>
3. <span data-ttu-id="d68ba-187">Ellenőrizze a ExpressRoute-kapcsolatcsoportot annak érdekében van üzembe helyezve, és is engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="d68ba-187">Check the ExpressRoute circuit to ensure it is provisioned and also enabled.</span></span> <span data-ttu-id="d68ba-188">Használja a következő példát:</span><span class="sxs-lookup"><span data-stu-id="d68ba-188">Use the following example:</span></span>

  ```powershell
  Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
  ```

  <span data-ttu-id="d68ba-189">A rendszer a választ az alábbi példához hasonló:</span><span class="sxs-lookup"><span data-stu-id="d68ba-189">The response is similar to the following example:</span></span>

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
4. <span data-ttu-id="d68ba-190">Konfigurálja az Azure nyilvános társviszony-létesítést a kapcsolatcsoporthoz.</span><span class="sxs-lookup"><span data-stu-id="d68ba-190">Configure Azure public peering for the circuit.</span></span> <span data-ttu-id="d68ba-191">Győződjön meg arról, hogy rendelkezik a következő adatokat, mielőtt végrehajtásának folytatásához.</span><span class="sxs-lookup"><span data-stu-id="d68ba-191">Make sure that you have the following information before you proceed further.</span></span>

  * <span data-ttu-id="d68ba-192">Egy /30 alhálózat az elsődleges kapcsolat számára.</span><span class="sxs-lookup"><span data-stu-id="d68ba-192">A /30 subnet for the primary link.</span></span> <span data-ttu-id="d68ba-193">Ennek egy érvényes nyilvános IPv4-előtagnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="d68ba-193">This must be a valid public IPv4 prefix.</span></span>
  * <span data-ttu-id="d68ba-194">Egy /30 alhálózat a másodlagos kapcsolat számára.</span><span class="sxs-lookup"><span data-stu-id="d68ba-194">A /30 subnet for the secondary link.</span></span> <span data-ttu-id="d68ba-195">Ennek egy érvényes nyilvános IPv4-előtagnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="d68ba-195">This must be a valid public IPv4 prefix.</span></span>
  * <span data-ttu-id="d68ba-196">Egy érvényes VLAN-azonosító a tárviszony-létesítés létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="d68ba-196">A valid VLAN ID to establish this peering on.</span></span> <span data-ttu-id="d68ba-197">Győződjön meg róla, hogy a kapcsolatcsoporton egy másik társviszony-létesítés sem használja ugyanezt a VLAN-azonosítót.</span><span class="sxs-lookup"><span data-stu-id="d68ba-197">Ensure that no other peering in the circuit uses the same VLAN ID.</span></span>
  * <span data-ttu-id="d68ba-198">Egy AS-szám a társviszony-létesítéshez.</span><span class="sxs-lookup"><span data-stu-id="d68ba-198">AS number for peering.</span></span> <span data-ttu-id="d68ba-199">2 és 4 bájtos AS-számokat is használhat.</span><span class="sxs-lookup"><span data-stu-id="d68ba-199">You can use both 2-byte and 4-byte AS numbers.</span></span>
  * <span data-ttu-id="d68ba-200">**Választható -** az MD5 kivonatoló, ha úgy dönt, hogy egyetlen módszer alkalmazása.</span><span class="sxs-lookup"><span data-stu-id="d68ba-200">**Optional -** An MD5 hash if you choose to use one.</span></span>

  <span data-ttu-id="d68ba-201">Futtassa az alábbi példa az Azure nyilvános társviszony-létesítés a kapcsolat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d68ba-201">Run the following example to configure Azure public peering for your circuit</span></span>

  ```powershell
  Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "12.0.0.0/30" -SecondaryPeerAddressPrefix "12.0.0.4/30" -VlanId 100

  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

  <span data-ttu-id="d68ba-202">Ha az MD5 kivonatoló használatát választja, használja a következő példát:</span><span class="sxs-lookup"><span data-stu-id="d68ba-202">If you choose to use an MD5 hash, use the following example:</span></span>

  ```powershell
  Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "12.0.0.0/30" -SecondaryPeerAddressPrefix "12.0.0.4/30" -VlanId 100  -SharedKey "A1B2C3D4"

  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

  > [!IMPORTANT]
  > <span data-ttu-id="d68ba-203">Az AS-számot mindenképp társviszony-létesítési ASN-ként, és ne ügyfél ASN-ként adja meg.</span><span class="sxs-lookup"><span data-stu-id="d68ba-203">Ensure that you specify your AS number as peering ASN, not customer ASN.</span></span>
  > 
  >

### <a name="to-view-azure-public-peering-details"></a><span data-ttu-id="d68ba-204">Azure nyilvános társviszony-létesítés részleteinek megtekintése</span><span class="sxs-lookup"><span data-stu-id="d68ba-204">To view Azure public peering details</span></span>

<span data-ttu-id="d68ba-205">Konfigurációs adatait az alábbi parancsmag segítségével szerezheti be:</span><span class="sxs-lookup"><span data-stu-id="d68ba-205">You can get configuration details using the following cmdlet:</span></span>

```powershell
  $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

  Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -Circuit $ckt
  ```

### <a name="to-update-azure-public-peering-configuration"></a><span data-ttu-id="d68ba-206">Azure nyilvános társviszony-létesítés konfigurációjának frissítése</span><span class="sxs-lookup"><span data-stu-id="d68ba-206">To update Azure public peering configuration</span></span>

<span data-ttu-id="d68ba-207">Frissítheti, az alábbi példa konfiguráció bármely részeként.</span><span class="sxs-lookup"><span data-stu-id="d68ba-207">You can update any part of the configuration using the following example.</span></span> <span data-ttu-id="d68ba-208">Ebben a példában a VLAN-Azonosítót a kapcsolatcsoport, frissül a 200-as 600 értékre.</span><span class="sxs-lookup"><span data-stu-id="d68ba-208">In this example, the VLAN ID of the circuit is being updated from 200 to 600.</span></span>

```powershell
Set-AzureRmExpressRouteCircuitPeeringConfig  -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 600

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="to-delete-azure-public-peering"></a><span data-ttu-id="d68ba-209">Azure nyilvános társviszony-létesítés törlése</span><span class="sxs-lookup"><span data-stu-id="d68ba-209">To delete Azure public peering</span></span>

<span data-ttu-id="d68ba-210">Futtassa a következő példa a társviszony-létesítési konfiguráció eltávolítása:</span><span class="sxs-lookup"><span data-stu-id="d68ba-210">You can remove your peering configuration by running the following example:</span></span>

```powershell
Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

## <a name="microsoft-peering"></a><span data-ttu-id="d68ba-211">Microsoft társviszony-létesítés</span><span class="sxs-lookup"><span data-stu-id="d68ba-211">Microsoft peering</span></span>

<span data-ttu-id="d68ba-212">Ez a szakasz segítséget nyújt a létrehozása, beolvasása, frissítése és a Microsoft társviszony-létesítési konfiguráció az ExpressRoute-kapcsolatcsoportot törli.</span><span class="sxs-lookup"><span data-stu-id="d68ba-212">This section helps you create, get, update, and delete the Microsoft peering configuration for an ExpressRoute circuit.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d68ba-213">A Microsoft társviszony-létesítést az ExpressRoute-Kapcsolatcsoportok 2017. augusztus 1. előtt konfigurált meghirdetett keresztül a Microsoft társviszony-létesítést, még akkor is, ha az útvonal-szűrők nem definiált összes szolgáltatás előtagok fog rendelkezni.</span><span class="sxs-lookup"><span data-stu-id="d68ba-213">Microsoft peering of ExpressRoute circuits that were configured prior to August 1, 2017 will have all service prefixes advertised through the Microsoft peering, even if route filters are not defined.</span></span> <span data-ttu-id="d68ba-214">A Microsoft társviszony-létesítést az ExpressRoute-Kapcsolatcsoportok vannak konfigurálva, vagy azt követően 2017. augusztus 1. nem rendelkezik a előtagokat amíg útvonal szűrő nem csatlakoztatja a kapcsolatcsoport hirdetve.</span><span class="sxs-lookup"><span data-stu-id="d68ba-214">Microsoft peering of ExpressRoute circuits that are configured on or after August 1, 2017 will not have any prefixes advertised until a route filter is attached to the circuit.</span></span> <span data-ttu-id="d68ba-215">További információkért lásd: [konfigurálása a Microsoft társviszony-létesítéshez útvonal szűrő](how-to-routefilter-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="d68ba-215">For more information, see [Configure a route filter for Microsoft peering](how-to-routefilter-powershell.md).</span></span>
> 
> 

### <a name="to-create-microsoft-peering"></a><span data-ttu-id="d68ba-216">Microsoft társviszony-létesítés létrehozása</span><span class="sxs-lookup"><span data-stu-id="d68ba-216">To create Microsoft peering</span></span>

1. <span data-ttu-id="d68ba-217">Importálja az ExpressRoute PowerShell-modulját.</span><span class="sxs-lookup"><span data-stu-id="d68ba-217">Import the PowerShell module for ExpressRoute.</span></span>

  <span data-ttu-id="d68ba-218">Telepítse a legújabb PowerShell-telepítőt a [PowerShell-galériából](http://www.powershellgallery.com/), és importálja az Azure Resource Manager-modulokat a PowerShell-munkamenetbe az ExpressRoute-parancsmagok használatának elkezdéséhez.</span><span class="sxs-lookup"><span data-stu-id="d68ba-218">You must install the latest PowerShell installer from [PowerShell Gallery](http://www.powershellgallery.com/) and import the Azure Resource Manager modules into the PowerShell session in order to start using the ExpressRoute cmdlets.</span></span> <span data-ttu-id="d68ba-219">A PowerShellt rendszergazdaként kell futtatnia.</span><span class="sxs-lookup"><span data-stu-id="d68ba-219">You will need to run PowerShell as an Administrator.</span></span>

  ```powershell
  Install-Module AzureRM

  Install-AzureRM
  ```

  <span data-ttu-id="d68ba-220">Importálja a AzureRM.* modulok ismert szemantikai verziója tartományon belül.</span><span class="sxs-lookup"><span data-stu-id="d68ba-220">Import all of the AzureRM.* modules within the known semantic version range.</span></span>

  ```powershell
  Import-AzureRM
  ```

  <span data-ttu-id="d68ba-221">Az ismert szemantikai verziója tartományon belüli select modul csak is importálhatja.</span><span class="sxs-lookup"><span data-stu-id="d68ba-221">You can also just import a select module within the known semantic version range.</span></span>

  ```powershell
  Import-Module AzureRM.Network
  ```

  <span data-ttu-id="d68ba-222">Jelentkezzen be a fiókjába.</span><span class="sxs-lookup"><span data-stu-id="d68ba-222">Sign in to your account.</span></span>

  ```powershell
  Login-AzureRmAccount
  ```

  <span data-ttu-id="d68ba-223">Válassza ki az előfizetést, ExpressRoute-kapcsolatcsoportot létrehozni kívánja.</span><span class="sxs-lookup"><span data-stu-id="d68ba-223">Select the subscription you want to create ExpressRoute circuit.</span></span>

  ```powershell
Select-AzureRmSubscription -SubscriptionId "<subscription ID>"
  ```
2. <span data-ttu-id="d68ba-224">Hozzon létre egy ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="d68ba-224">Create an ExpressRoute circuit.</span></span>

  <span data-ttu-id="d68ba-225">Kövesse az utasításokat az [ExpressRoute-kapcsolatcsoport](expressroute-howto-circuit-arm.md) létrehozásához, és kérje meg kapcsolatszolgáltatóját, hogy ossza ki azt.</span><span class="sxs-lookup"><span data-stu-id="d68ba-225">Follow the instructions to create an [ExpressRoute circuit](expressroute-howto-circuit-arm.md) and have it provisioned by the connectivity provider.</span></span>

  <span data-ttu-id="d68ba-226">Ha kapcsolatszolgáltatója felügyelt 3. rétegbeli szolgáltatásokat kínál, igényelheti tőle, hogy engedélyezze Önnek az Azure privát társviszony-létesítést.</span><span class="sxs-lookup"><span data-stu-id="d68ba-226">If your connectivity provider offers managed Layer 3 services, you can request your connectivity provider to enable Azure private peering for you.</span></span> <span data-ttu-id="d68ba-227">Ebben az esetben nem szükséges a következő szakaszokban foglalt lépéseket végrehajtania.</span><span class="sxs-lookup"><span data-stu-id="d68ba-227">In that case, you won't need to follow instructions listed in the next sections.</span></span> <span data-ttu-id="d68ba-228">Azonban ha a kapcsolat szolgáltatójánál nem kezeli az útválasztást, a kapcsolat létrehozása után továbbra is a konfiguráció a következő lépéseket.</span><span class="sxs-lookup"><span data-stu-id="d68ba-228">However, if your connectivity provider does not manage routing for you, after creating your circuit, continue your configuration using the next steps.</span></span>
3. <span data-ttu-id="d68ba-229">Ellenőrizze, hogy üzembe helyezve, és is engedélyezve van az ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="d68ba-229">Check the ExpressRoute circuit to make sure it is provisioned and also enabled.</span></span> <span data-ttu-id="d68ba-230">Használja a következő példát:</span><span class="sxs-lookup"><span data-stu-id="d68ba-230">Use the following example:</span></span>

  ```powershell
  Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
  ```

  <span data-ttu-id="d68ba-231">A rendszer a választ az alábbi példához hasonló:</span><span class="sxs-lookup"><span data-stu-id="d68ba-231">The response is similar to the following example:</span></span>

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
4. <span data-ttu-id="d68ba-232">Konfigurálja a Microsoft társviszony-létesítést a kapcsolatcsoporthoz.</span><span class="sxs-lookup"><span data-stu-id="d68ba-232">Configure Microsoft peering for the circuit.</span></span> <span data-ttu-id="d68ba-233">Mielőtt folytatná, ellenőrizze az alábbi információk meglétét.</span><span class="sxs-lookup"><span data-stu-id="d68ba-233">Make sure that you have the following information before you proceed.</span></span>

  * <span data-ttu-id="d68ba-234">Egy /30 alhálózat az elsődleges kapcsolat számára.</span><span class="sxs-lookup"><span data-stu-id="d68ba-234">A /30 subnet for the primary link.</span></span> <span data-ttu-id="d68ba-235">Ennek egy érvényes nyilvános IPv4-előtagnak kell lennie, amely az Ön tulajdonában van, és regisztrálva van egy RIR-/IRR-jegyzékben.</span><span class="sxs-lookup"><span data-stu-id="d68ba-235">This must be a valid public IPv4 prefix owned by you and registered in an RIR / IRR.</span></span>
  * <span data-ttu-id="d68ba-236">Egy /30 alhálózat a másodlagos kapcsolat számára.</span><span class="sxs-lookup"><span data-stu-id="d68ba-236">A /30 subnet for the secondary link.</span></span> <span data-ttu-id="d68ba-237">Ennek egy érvényes nyilvános IPv4-előtagnak kell lennie, amely az Ön tulajdonában van, és regisztrálva van egy RIR-/IRR-jegyzékben.</span><span class="sxs-lookup"><span data-stu-id="d68ba-237">This must be a valid public IPv4 prefix owned by you and registered in an RIR / IRR.</span></span>
  * <span data-ttu-id="d68ba-238">Egy érvényes VLAN-azonosító a tárviszony-létesítés létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="d68ba-238">A valid VLAN ID to establish this peering on.</span></span> <span data-ttu-id="d68ba-239">Győződjön meg róla, hogy a kapcsolatcsoporton egy másik társviszony-létesítés sem használja ugyanezt a VLAN-azonosítót.</span><span class="sxs-lookup"><span data-stu-id="d68ba-239">Ensure that no other peering in the circuit uses the same VLAN ID.</span></span>
  * <span data-ttu-id="d68ba-240">Egy AS-szám a társviszony-létesítéshez.</span><span class="sxs-lookup"><span data-stu-id="d68ba-240">AS number for peering.</span></span> <span data-ttu-id="d68ba-241">2 és 4 bájtos AS-számokat is használhat.</span><span class="sxs-lookup"><span data-stu-id="d68ba-241">You can use both 2-byte and 4-byte AS numbers.</span></span>
  * <span data-ttu-id="d68ba-242">Meghirdetett előtagok: Meg kell adnia a BGP-munkamenetben meghirdetni kívánt összes előtag listáját.</span><span class="sxs-lookup"><span data-stu-id="d68ba-242">Advertised prefixes: You must provide a list of all prefixes you plan to advertise over the BGP session.</span></span> <span data-ttu-id="d68ba-243">A rendszer kizárólag a nyilvános IP-cím-előtagokat fogadja el.</span><span class="sxs-lookup"><span data-stu-id="d68ba-243">Only public IP address prefixes are accepted.</span></span> <span data-ttu-id="d68ba-244">Ha szeretne elküldhető a előtagokat, elküldheti egy vesszővel elválasztott listában.</span><span class="sxs-lookup"><span data-stu-id="d68ba-244">If you plan to send a set of prefixes, you can send a comma-separated list.</span></span> <span data-ttu-id="d68ba-245">Az előtagoknak egy RIR/IRR jegyzékben regisztrálva kell lenniük az Ön neve alatt.</span><span class="sxs-lookup"><span data-stu-id="d68ba-245">These prefixes must be registered to you in an RIR / IRR.</span></span>
  * <span data-ttu-id="d68ba-246">**Választható -** ügyfél ASN: Ha nincs regisztrálva a társviszony-létesítés SZÁMOT hirdetési előtagok, megadhatja a AS számot, amelyhez regisztrálja azokat a rendszer.</span><span class="sxs-lookup"><span data-stu-id="d68ba-246">**Optional -** Customer ASN: If you are advertising prefixes that are not registered to the peering AS number, you can specify the AS number to which they are registered.</span></span>
  * <span data-ttu-id="d68ba-247">Útválasztási jegyzék neve: Megadhatja az RIR/IRR jegyzék nevét, amelyben az AS-szám és az előtagok regisztrálva vannak.</span><span class="sxs-lookup"><span data-stu-id="d68ba-247">Routing Registry Name: You can specify the RIR / IRR against which the AS number and prefixes are registered.</span></span>
  * <span data-ttu-id="d68ba-248">**Választható -** az MD5 kivonatoló, ha úgy dönt, hogy egyetlen módszer alkalmazása.</span><span class="sxs-lookup"><span data-stu-id="d68ba-248">**Optional -** An MD5 hash if you choose to use one.</span></span>

   <span data-ttu-id="d68ba-249">Az alábbi példa használatával konfigurálja a kör társviszony Microsoft:</span><span class="sxs-lookup"><span data-stu-id="d68ba-249">Use the following example to configure Microsoft peering for your circuit:</span></span>

  ```powershell
  Add-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 300 -MicrosoftConfigAdvertisedPublicPrefixes "123.1.0.0/24" -MicrosoftConfigCustomerAsn 23 -MicrosoftConfigRoutingRegistryName "ARIN"

  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

### <a name="to-get-microsoft-peering-details"></a><span data-ttu-id="d68ba-250">Microsoft társviszony-létesítés részleteinek lekérése</span><span class="sxs-lookup"><span data-stu-id="d68ba-250">To get Microsoft peering details</span></span>

<span data-ttu-id="d68ba-251">Az alábbi példa konfigurációs részletek szerezheti be:</span><span class="sxs-lookup"><span data-stu-id="d68ba-251">You can get configuration details using the following example:</span></span>

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

Get-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt
```

### <a name="to-update-microsoft-peering-configuration"></a><span data-ttu-id="d68ba-252">Microsoft társviszony-létesítés konfigurációjának frissítése</span><span class="sxs-lookup"><span data-stu-id="d68ba-252">To update Microsoft peering configuration</span></span>

<span data-ttu-id="d68ba-253">Frissítheti, az alábbi példa konfiguráció bármely részeként:</span><span class="sxs-lookup"><span data-stu-id="d68ba-253">You can update any part of the configuration using the following example:</span></span>

```powershell
Set-AzureRmExpressRouteCircuitPeeringConfig  -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 300 -MicrosoftConfigAdvertisedPublicPrefixes "124.1.0.0/24" -MicrosoftConfigCustomerAsn 23 -MicrosoftConfigRoutingRegistryName "ARIN"

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="to-delete-microsoft-peering"></a><span data-ttu-id="d68ba-254">Microsoft társviszony-létesítés törlése</span><span class="sxs-lookup"><span data-stu-id="d68ba-254">To delete Microsoft peering</span></span>

<span data-ttu-id="d68ba-255">A társviszony-létesítési konfiguráció a következő parancsmag futtatásával távolíthatja el:</span><span class="sxs-lookup"><span data-stu-id="d68ba-255">You can remove your peering configuration by running the following cmdlet:</span></span>

```powershell
Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

## <a name="next-steps"></a><span data-ttu-id="d68ba-256">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d68ba-256">Next steps</span></span>

<span data-ttu-id="d68ba-257">A következő lépés egy [VNet csatlakoztatása egy ExpressRoute-kapcsolatcsoporthoz](expressroute-howto-linkvnet-arm.md).</span><span class="sxs-lookup"><span data-stu-id="d68ba-257">Next step, [Link a VNet to an ExpressRoute circuit](expressroute-howto-linkvnet-arm.md).</span></span>

* <span data-ttu-id="d68ba-258">Az ExpressRoute-munkafolyamatokkal kapcsolatos további információkért lásd: [ExpressRoute workflows](expressroute-workflows.md) (ExpressRoute-munkafolyamatok).</span><span class="sxs-lookup"><span data-stu-id="d68ba-258">For more information about ExpressRoute workflows, see [ExpressRoute workflows](expressroute-workflows.md).</span></span>
* <span data-ttu-id="d68ba-259">A kapcsolatcsoportok társviszony-létesítéseivel kapcsolatos további információkért lásd: [ExpressRoute circuits and routing domains](expressroute-circuit-peerings.md) (ExpressRoute-kapcsolatcsoportok és útválasztási tartományok).</span><span class="sxs-lookup"><span data-stu-id="d68ba-259">For more information about circuit peering, see [ExpressRoute circuits and routing domains](expressroute-circuit-peerings.md).</span></span>
* <span data-ttu-id="d68ba-260">További információkért a virtuális hálózatok használatáról lásd: [Virtual network overview](../virtual-network/virtual-networks-overview.md) (Virtuális hálózatok áttekintése).</span><span class="sxs-lookup"><span data-stu-id="d68ba-260">For more information about working with virtual networks, see [Virtual network overview](../virtual-network/virtual-networks-overview.md).</span></span>
