---
title: "Az Azure ExpressRoute-kapcsolatcsoportot útválasztás konfigurálása: CLI |} Microsoft Docs"
description: "Ez a cikk segítséget nyújt a létrehozása, és helyezze üzembe a magánfelhő, nyilvános és a Microsoft társviszony-létesítést az ExpressRoute-kapcsolatcsoportot. A cikk azt is bemutatja, hogyan ellenőrizheti a kapcsolatcsoport társviszonyainak állapotát, illetve hogyan frissítheti vagy törölheti őket."
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: anzaman,cherylmc
ms.openlocfilehash: fbf0bd9a139c22bbd63755f6df445f6596aaccc5
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="create-and-modify-routing-for-an-expressroute-circuit-using-cli"></a><span data-ttu-id="31fab-104">Létrehozásához és módosításához útválasztás az ExpressRoute-kapcsolatcsoportot parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="31fab-104">Create and modify routing for an ExpressRoute circuit using CLI</span></span>

<span data-ttu-id="31fab-105">Ez a cikk segít hozhat létre és kezelhet a Resource Manager üzembe helyezési modellel parancssori felület használatával ExpressRoute-kapcsolatcsoportot útválasztási konfigurációja.</span><span class="sxs-lookup"><span data-stu-id="31fab-105">This article helps you create and manage routing configuration for an ExpressRoute circuit in the Resource Manager deployment model using CLI.</span></span> <span data-ttu-id="31fab-106">Ellenőrizze az állapot, frissítési vagy törlési is, és az ExpressRoute-kör társviszony kiosztásának megszüntetése.</span><span class="sxs-lookup"><span data-stu-id="31fab-106">You can also check the status, update, or delete and deprovision peerings for an ExpressRoute circuit.</span></span> <span data-ttu-id="31fab-107">Ha más módszert használja a kapcsolatcsoport dolgozni szeretne, válassza ki egy cikk az alábbi listából:</span><span class="sxs-lookup"><span data-stu-id="31fab-107">If you want to use a different method to work with your circuit, select an article from the following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="31fab-108">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="31fab-108">Azure portal</span></span>](expressroute-howto-routing-portal-resource-manager.md)
> * [<span data-ttu-id="31fab-109">PowerShell</span><span class="sxs-lookup"><span data-stu-id="31fab-109">PowerShell</span></span>](expressroute-howto-routing-arm.md)
> * [<span data-ttu-id="31fab-110">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="31fab-110">Azure CLI</span></span>](howto-routing-cli.md)
> * [<span data-ttu-id="31fab-111">Video - privát társviszony-létesítés</span><span class="sxs-lookup"><span data-stu-id="31fab-111">Video - Private peering</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-private-peering-for-your-expressroute-circuit)
> * [<span data-ttu-id="31fab-112">Video - nyilvános társviszony-létesítés</span><span class="sxs-lookup"><span data-stu-id="31fab-112">Video - Public peering</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-public-peering-for-your-expressroute-circuit)
> * [<span data-ttu-id="31fab-113">Videó – a Microsoft társviszony-létesítés</span><span class="sxs-lookup"><span data-stu-id="31fab-113">Video - Microsoft peering</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-microsoft-peering-for-your-expressroute-circuit)
> * [<span data-ttu-id="31fab-114">PowerShell (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="31fab-114">PowerShell (classic)</span></span>](expressroute-howto-routing-classic.md)
> 

## <a name="configuration-prerequisites"></a><span data-ttu-id="31fab-115">Konfigurációs előfeltételek</span><span class="sxs-lookup"><span data-stu-id="31fab-115">Configuration prerequisites</span></span>

* <span data-ttu-id="31fab-116">A folyamat elkezdése előtt telepítse a CLI-parancsok legújabb verzióit (2.0-s vagy újabb).</span><span class="sxs-lookup"><span data-stu-id="31fab-116">Before beginning, install the latest version of the CLI commands (2.0 or later).</span></span> <span data-ttu-id="31fab-117">Információk a CLI-parancsok telepítéséről: [Az Azure CLI 2.0-s verziójának telepítése](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="31fab-117">For information about installing the CLI commands, see [Install Azure CLI 2.0](/cli/azure/install-azure-cli).</span></span>
* <span data-ttu-id="31fab-118">Győződjön meg arról, hogy tekintse át a [Előfeltételek](expressroute-prerequisites.md), [útválasztási követelmények](expressroute-routing.md), és [munkafolyamat](expressroute-workflows.md) lapok: konfigurálás elkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="31fab-118">Make sure that you have reviewed the [prerequisites](expressroute-prerequisites.md), [routing requirements](expressroute-routing.md), and [workflow](expressroute-workflows.md) pages before you begin configuration.</span></span>
* <span data-ttu-id="31fab-119">Egy aktív ExpressRoute-kapcsolatcsoportra lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="31fab-119">You must have an active ExpressRoute circuit.</span></span> <span data-ttu-id="31fab-120">Kövesse az [ExpressRoute-kapcsolatcsoport létrehozása](howto-circuit-cli.md) részben foglalt lépéseket, és engedélyeztesse a kapcsolatcsoportot kapcsolatszolgáltatójával, mielőtt továbblépne.</span><span class="sxs-lookup"><span data-stu-id="31fab-120">Follow the instructions to [Create an ExpressRoute circuit](howto-circuit-cli.md) and have the circuit enabled by your connectivity provider before you proceed.</span></span> <span data-ttu-id="31fab-121">Az ExpressRoute-kapcsolatcsoport ahhoz, hogy ebben a cikkben a parancsok futtatásához kiépített és engedélyezett állapotban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="31fab-121">The ExpressRoute circuit must be in a provisioned and enabled state for you to be able to run the commands in this article.</span></span>

<span data-ttu-id="31fab-122">Az utasítások csak 2. rétegbeli kapcsolatszolgáltatásokat kínáló szolgáltatóknál létrehozott kapcsolatcsoportokra vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="31fab-122">These instructions only apply to circuits created with service providers offering Layer 2 connectivity services.</span></span> <span data-ttu-id="31fab-123">A szolgáltató által kezelt használata réteg (általában egy IPVPN, például az MPLS) 3 szolgáltatások, a kapcsolat szolgáltatójánál konfigurálása és kezelése az Ön útválasztást.</span><span class="sxs-lookup"><span data-stu-id="31fab-123">If you are using a service provider that offers managed Layer 3 services (typically an IPVPN, like MPLS), your connectivity provider will configure and manage routing for you.</span></span>

<span data-ttu-id="31fab-124">Beállíthatja, hogy egy, kettő vagy ExpressRoute-kör összes három esetében (Azure saját, az Azure nyilvános, és a Microsoft).</span><span class="sxs-lookup"><span data-stu-id="31fab-124">You can configure one, two, or all three peerings (Azure private, Azure public, and Microsoft) for an ExpressRoute circuit.</span></span> <span data-ttu-id="31fab-125">A társviszony-létesítéseket tetszőleges sorrendben konfigurálhatja.</span><span class="sxs-lookup"><span data-stu-id="31fab-125">You can configure peerings in any order you choose.</span></span> <span data-ttu-id="31fab-126">Az egyes társviszony-létesítéseket azonban mindenképp egyenként kell végrehajtania.</span><span class="sxs-lookup"><span data-stu-id="31fab-126">However, you must make sure that you complete the configuration of each peering one at a time.</span></span>

## <a name="azure-private-peering"></a><span data-ttu-id="31fab-127">Azure privát társviszony-létesítés</span><span class="sxs-lookup"><span data-stu-id="31fab-127">Azure private peering</span></span>

<span data-ttu-id="31fab-128">Ez a szakasz segítséget nyújt a létrehozása, beolvasása, frissítése és törölni az Azure magánhálózati társviszony-létesítési ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="31fab-128">This section helps you create, get, update, and delete the Azure private peering configuration for an ExpressRoute circuit.</span></span>

### <a name="to-create-azure-private-peering"></a><span data-ttu-id="31fab-129">Azure privát társviszony-létesítés létrehozása</span><span class="sxs-lookup"><span data-stu-id="31fab-129">To create Azure private peering</span></span>

1. <span data-ttu-id="31fab-130">Az Azure parancssori felület legújabb verziójának telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="31fab-130">Install the latest version of Azure CLI.</span></span> <span data-ttu-id="31fab-131">A legújabb verziót, az Azure parancssori felület (CLI) kell használnia. * tekintse át a [Előfeltételek](expressroute-prerequisites.md) és [munkafolyamatok](expressroute-workflows.md) konfigurálás elkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="31fab-131">You must use the latest version of the Azure Command-line Interface (CLI).* Review the [prerequisites](expressroute-prerequisites.md) and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>

  ```azurecli
  az login
  ```

  <span data-ttu-id="31fab-132">Válassza ki az előfizetést, amelyben az ExpressRoute-kapcsolatcsoportot létre kívánja hozni.</span><span class="sxs-lookup"><span data-stu-id="31fab-132">Select the subscription you want to create ExpressRoute circuit</span></span>

  ```azurecli
  az account set --subscription "<subscription ID>"
  ```
2. <span data-ttu-id="31fab-133">Hozzon létre egy ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="31fab-133">Create an ExpressRoute circuit.</span></span> <span data-ttu-id="31fab-134">Kövesse az utasításokat az [ExpressRoute-kapcsolatcsoport](howto-circuit-cli.md) létrehozásához, és kérje meg kapcsolatszolgáltatóját, hogy ossza ki azt.</span><span class="sxs-lookup"><span data-stu-id="31fab-134">Follow the instructions to create an [ExpressRoute circuit](howto-circuit-cli.md) and have it provisioned by the connectivity provider.</span></span>

  <span data-ttu-id="31fab-135">Ha kapcsolatszolgáltatója felügyelt 3. rétegbeli szolgáltatásokat kínál, igényelheti tőle, hogy engedélyezze Önnek az Azure privát társviszony-létesítést.</span><span class="sxs-lookup"><span data-stu-id="31fab-135">If your connectivity provider offers managed Layer 3 services, you can request your connectivity provider to enable Azure private peering for you.</span></span> <span data-ttu-id="31fab-136">Ebben az esetben nem szükséges a következő szakaszokban foglalt lépéseket végrehajtania.</span><span class="sxs-lookup"><span data-stu-id="31fab-136">In that case, you won't need to follow instructions listed in the next sections.</span></span> <span data-ttu-id="31fab-137">Azonban ha a kapcsolat szolgáltatójánál nem kezeli az útválasztást, a kapcsolat létrehozása után továbbra is a konfiguráció a következő lépéseket.</span><span class="sxs-lookup"><span data-stu-id="31fab-137">However, if your connectivity provider does not manage routing for you, after creating your circuit, continue your configuration using the next steps.</span></span>
3. <span data-ttu-id="31fab-138">Ellenőrizze, hogy üzembe helyezve, és is engedélyezve van az ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="31fab-138">Check the ExpressRoute circuit to make sure it is provisioned and also enabled.</span></span> <span data-ttu-id="31fab-139">Használja a következő példát:</span><span class="sxs-lookup"><span data-stu-id="31fab-139">Use the following example:</span></span>

  ```azurecli
  az network express-route show --resource-group ExpressRouteResourceGroup --name MyCircuit
  ```

  <span data-ttu-id="31fab-140">A rendszer a választ az alábbi példához hasonló:</span><span class="sxs-lookup"><span data-stu-id="31fab-140">The response is similar to the following example:</span></span>

  ```azurecli
  "allowClassicOperations": false,
  "authorizations": [],
  "circuitProvisioningState": "Enabled",
  "etag": "W/\"1262c492-ffef-4a63-95a8-a6002736b8c4\"",
  "gatewayManagerEtag": null,
  "id": "/subscriptions/81ab786c-56eb-4a4d-bb5f-f60329772466/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/MyCircuit",
  "location": "westus",
  "name": "MyCircuit",
  "peerings": [],
  "provisioningState": "Succeeded",
  "resourceGroup": "ExpressRouteResourceGroup",
  "serviceKey": "1d05cf70-1db5-419f-ad86-1ca62c3c125b",
  "serviceProviderNotes": null,
  "serviceProviderProperties": {
  "bandwidthInMbps": 200,
  "peeringLocation": "Silicon Valley",
  "serviceProviderName": "Equinix"
  },
  "serviceProviderProvisioningState": "Provisioned",
  "sku": {
    "family": "UnlimitedData",
    "name": "Standard_MeteredData",
    "tier": "Standard"
  },
  "tags": null,
  "type": "Microsoft.Network/expressRouteCircuits]
  ```

4. <span data-ttu-id="31fab-141">Konfigurálja az Azure privát társviszony-létesítést a kapcsolatcsoport számára.</span><span class="sxs-lookup"><span data-stu-id="31fab-141">Configure Azure private peering for the circuit.</span></span> <span data-ttu-id="31fab-142">Mielőtt folytatná a következő lépésekkel, ellenőrizze az alábbi elemek meglétét:</span><span class="sxs-lookup"><span data-stu-id="31fab-142">Make sure that you have the following items before you proceed with the next steps:</span></span>

  * <span data-ttu-id="31fab-143">Egy /30 alhálózat az elsődleges kapcsolat számára.</span><span class="sxs-lookup"><span data-stu-id="31fab-143">A /30 subnet for the primary link.</span></span> <span data-ttu-id="31fab-144">Az alhálózat nem virtuális hálózatok számára fenntartott címtartomány részének kell lennie.</span><span class="sxs-lookup"><span data-stu-id="31fab-144">The subnet must not be part of any address space reserved for virtual networks.</span></span>
  * <span data-ttu-id="31fab-145">Egy /30 alhálózat a másodlagos kapcsolat számára.</span><span class="sxs-lookup"><span data-stu-id="31fab-145">A /30 subnet for the secondary link.</span></span> <span data-ttu-id="31fab-146">Az alhálózat nem virtuális hálózatok számára fenntartott címtartomány részének kell lennie.</span><span class="sxs-lookup"><span data-stu-id="31fab-146">The subnet must not be part of any address space reserved for virtual networks.</span></span>
  * <span data-ttu-id="31fab-147">Egy érvényes VLAN-azonosító a tárviszony-létesítés létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="31fab-147">A valid VLAN ID to establish this peering on.</span></span> <span data-ttu-id="31fab-148">Győződjön meg róla, hogy a kapcsolatcsoporton egy másik társviszony-létesítés sem használja ugyanezt a VLAN-azonosítót.</span><span class="sxs-lookup"><span data-stu-id="31fab-148">Ensure that no other peering in the circuit uses the same VLAN ID.</span></span>
  * <span data-ttu-id="31fab-149">Egy AS-szám a társviszony-létesítéshez.</span><span class="sxs-lookup"><span data-stu-id="31fab-149">AS number for peering.</span></span> <span data-ttu-id="31fab-150">2 és 4 bájtos AS-számokat is használhat.</span><span class="sxs-lookup"><span data-stu-id="31fab-150">You can use both 2-byte and 4-byte AS numbers.</span></span> <span data-ttu-id="31fab-151">Ehhez a társviszony-létesítéshez használhat privát AS-számokat is.</span><span class="sxs-lookup"><span data-stu-id="31fab-151">You can use a private AS number for this peering.</span></span> <span data-ttu-id="31fab-152">Ne használja a 65515 számot.</span><span class="sxs-lookup"><span data-stu-id="31fab-152">Ensure that you are not using 65515.</span></span>
  * <span data-ttu-id="31fab-153">**Választható -** az MD5 kivonatoló, ha úgy dönt, hogy egyetlen módszer alkalmazása.</span><span class="sxs-lookup"><span data-stu-id="31fab-153">**Optional -** An MD5 hash if you choose to use one.</span></span>

  <span data-ttu-id="31fab-154">Az alábbi példát követve Azure magánhálózati társviszony-létesítés a kapcsolat konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="31fab-154">Use the following example to configure Azure private peering for your circuit:</span></span>

  ```azurecli
  az network express-route peering create --circuit-name MyCircuit --peer-asn 100 --primary-peer-subnet 10.0.0.0/30 -g ExpressRouteResourceGroup --secondary-peer-subnet 10.0.0.4/30 --vlan-id 200 --peering-type AzurePrivatePeering
  ```

  <span data-ttu-id="31fab-155">Ha az MD5 kivonatoló használatát választja, használja a következő példát:</span><span class="sxs-lookup"><span data-stu-id="31fab-155">If you choose to use an MD5 hash, use the following example:</span></span>

  ```azurecli
  az network express-route peering create --circuit-name MyCircuit --peer-asn 100 --primary-peer-subnet 10.0.0.0/30 -g ExpressRouteResourceGroup --secondary-peer-subnet 10.0.0.4/30 --vlan-id 200 --peering-type AzurePrivatePeering --SharedKey "A1B2C3D4"
  ```

  > [!IMPORTANT]
  > <span data-ttu-id="31fab-156">Az AS-számot mindenképp társviszony-létesítési ASN-ként, és ne ügyfél ASN-ként adja meg.</span><span class="sxs-lookup"><span data-stu-id="31fab-156">Ensure that you specify your AS number as peering ASN, not customer ASN.</span></span>
  > 
  > 

### <a name="to-view-azure-private-peering-details"></a><span data-ttu-id="31fab-157">Azure privát társviszony-létesítés részleteinek megtekintése</span><span class="sxs-lookup"><span data-stu-id="31fab-157">To view Azure private peering details</span></span>

<span data-ttu-id="31fab-158">Konfigurációs részletek kaphat az alábbi példa:</span><span class="sxs-lookup"><span data-stu-id="31fab-158">You can get configuration details by using the following example:</span></span>

```azurecli
az network express-route peering show -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePrivatePeering
```

<span data-ttu-id="31fab-159">A kimenet a következő példához hasonló:</span><span class="sxs-lookup"><span data-stu-id="31fab-159">The output is similar to the following example:</span></span>

```azurecli
{
  "azureAsn": 12076,
  "etag": "W/\"2e97be83-a684-4f29-bf3c-96191e270666\"",
  "gatewayManagerEtag": "18",
  "id": "/subscriptions/9a0c2943-e0c2-4608-876c-e0ddffd1211b/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/MyCircuit/peerings/AzurePrivatePeering",
  "ipv6PeeringConfig": null,
  "lastModifiedBy": "Customer",
  "microsoftPeeringConfig": null,
  "name": "AzurePrivatePeering",
  "peerAsn": 7671,
  "peeringType": "AzurePrivatePeering",
  "primaryAzurePort": "",
  "primaryPeerAddressPrefix": "",
  "provisioningState": "Succeeded",
  "resourceGroup": "ExpressRouteResourceGroup",
  "routeFilter": null,
  "secondaryAzurePort": "",
  "secondaryPeerAddressPrefix": "",
  "sharedKey": null,
  "state": "Enabled",
  "stats": null,
  "vlanId": 100
}
```

### <a name="to-update-azure-private-peering-configuration"></a><span data-ttu-id="31fab-160">Azure privát társviszony-létesítés konfigurációjának frissítése</span><span class="sxs-lookup"><span data-stu-id="31fab-160">To update Azure private peering configuration</span></span>

<span data-ttu-id="31fab-161">Frissítheti, az alábbi példa konfiguráció bármely részeként.</span><span class="sxs-lookup"><span data-stu-id="31fab-161">You can update any part of the configuration using the following example.</span></span> <span data-ttu-id="31fab-162">Ebben a példában a VLAN-Azonosítót a áramkör frissül 100 500.</span><span class="sxs-lookup"><span data-stu-id="31fab-162">In this example, the VLAN ID of the circuit is being updated from 100 to 500.</span></span>

```azurecli
az network express-route peering update --vlan-id 500 -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePrivatePeering
```

### <a name="to-delete-azure-private-peering"></a><span data-ttu-id="31fab-163">Azure privát társviszony-létesítés törlése</span><span class="sxs-lookup"><span data-stu-id="31fab-163">To delete Azure private peering</span></span>

<span data-ttu-id="31fab-164">Futtassa a következő példa a társviszony-létesítési konfiguráció eltávolítása:</span><span class="sxs-lookup"><span data-stu-id="31fab-164">You can remove your peering configuration by running the following example:</span></span>

> [!WARNING]
> <span data-ttu-id="31fab-165">Bizonyosodjon meg, hogy az összes virtuális hálózatot megszüntetni az ExpressRoute-kapcsolatcsoport az ebben a példában futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="31fab-165">You must ensure that all virtual networks are unlinked from the ExpressRoute circuit before running this example.</span></span> 
> 
> 

```azurecli
az network express-route peering delete -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePrivatePeering
```

## <a name="azure-public-peering"></a><span data-ttu-id="31fab-166">Azure nyilvános társviszony-létesítés</span><span class="sxs-lookup"><span data-stu-id="31fab-166">Azure public peering</span></span>

<span data-ttu-id="31fab-167">Ez a szakasz segítséget nyújt a létrehozása, beolvasása, frissítése és törlése az Azure nyilvános társviszony-létesítési konfiguráció ExpressRoute-kör.</span><span class="sxs-lookup"><span data-stu-id="31fab-167">This section helps you create, get, update, and delete the Azure public peering configuration for an ExpressRoute circuit.</span></span>

### <a name="to-create-azure-public-peering"></a><span data-ttu-id="31fab-168">Azure nyilvános társviszony-létesítés létrehozása</span><span class="sxs-lookup"><span data-stu-id="31fab-168">To create Azure public peering</span></span>

1. <span data-ttu-id="31fab-169">Az Azure parancssori felület legújabb verziójának telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="31fab-169">Install the latest version of Azure CLI.</span></span> <span data-ttu-id="31fab-170">A legújabb verziót, az Azure parancssori felület (CLI) kell használnia. * tekintse át a [Előfeltételek](expressroute-prerequisites.md) és [munkafolyamatok](expressroute-workflows.md) konfigurálás elkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="31fab-170">You must use the latest version of the Azure Command-line Interface (CLI).* Review the [prerequisites](expressroute-prerequisites.md) and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>

  ```azurecli
  az login
  ```

  <span data-ttu-id="31fab-171">Válassza ki az előfizetést, amelyekhez ExpressRoute-kapcsolatcsoportot létrehozni.</span><span class="sxs-lookup"><span data-stu-id="31fab-171">Select the subscription for which you want to create ExpressRoute circuit.</span></span>

  ```azurecli
  az account set --subscription "<subscription ID>"
  ```
2. <span data-ttu-id="31fab-172">Hozzon létre egy ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="31fab-172">Create an ExpressRoute circuit.</span></span>  <span data-ttu-id="31fab-173">Kövesse az utasításokat az [ExpressRoute-kapcsolatcsoport](howto-circuit-cli.md) létrehozásához, és kérje meg kapcsolatszolgáltatóját, hogy ossza ki azt.</span><span class="sxs-lookup"><span data-stu-id="31fab-173">Follow the instructions to create an [ExpressRoute circuit](howto-circuit-cli.md) and have it provisioned by the connectivity provider.</span></span>

  <span data-ttu-id="31fab-174">Ha a kapcsolat szolgáltatójánál felügyelt 3. rétegbeli szolgáltatásokat nyújt, kérheti, hogy a kapcsolat szolgáltatójánál lehetővé teszi, hogy az Azure magánhálózati társviszony-létesítés meg.</span><span class="sxs-lookup"><span data-stu-id="31fab-174">If your connectivity provider offers managed Layer 3 services, you can request that your connectivity provider enables Azure private peering for you.</span></span> <span data-ttu-id="31fab-175">Ebben az esetben nem szükséges a következő szakaszokban foglalt lépéseket végrehajtania.</span><span class="sxs-lookup"><span data-stu-id="31fab-175">In that case, you won't need to follow instructions listed in the next sections.</span></span> <span data-ttu-id="31fab-176">Azonban ha a kapcsolat szolgáltatójánál nem kezeli az útválasztást, a kapcsolat létrehozása után továbbra is a konfiguráció a következő lépéseket.</span><span class="sxs-lookup"><span data-stu-id="31fab-176">However, if your connectivity provider does not manage routing for you, after creating your circuit, continue your configuration using the next steps.</span></span>
3. <span data-ttu-id="31fab-177">Ellenőrizze a ExpressRoute-kapcsolatcsoportot annak érdekében van üzembe helyezve, és is engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="31fab-177">Check the ExpressRoute circuit to ensure it is provisioned and also enabled.</span></span> <span data-ttu-id="31fab-178">Használja a következő példát:</span><span class="sxs-lookup"><span data-stu-id="31fab-178">Use the following example:</span></span>

  ```azurecli
  az network express-route list
  ```

  <span data-ttu-id="31fab-179">A rendszer a választ az alábbi példához hasonló:</span><span class="sxs-lookup"><span data-stu-id="31fab-179">The response is similar to the following example:</span></span>

  ```azurecli
  "allowClassicOperations": false,
  "authorizations": [],
  "circuitProvisioningState": "Enabled",
  "etag": "W/\"1262c492-ffef-4a63-95a8-a6002736b8c4\"",
  "gatewayManagerEtag": null,
  "id": "/subscriptions/81ab786c-56eb-4a4d-bb5f-f60329772466/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/MyCircuit",
  "location": "westus",
  "name": "MyCircuit",
  "peerings": [],
  "provisioningState": "Succeeded",
  "resourceGroup": "ExpressRouteResourceGroup",
  "serviceKey": "1d05cf70-1db5-419f-ad86-1ca62c3c125b",
  "serviceProviderNotes": null,
  "serviceProviderProperties": {
    "bandwidthInMbps": 200,
    "peeringLocation": "Silicon Valley",
    "serviceProviderName": "Equinix"
  },
  "serviceProviderProvisioningState": "Provisioned",
  "sku": {
    "family": "UnlimitedData",
    "name": "Standard_MeteredData",
    "tier": "Standard"
  },
  "tags": null,
  "type": "Microsoft.Network/expressRouteCircuits]
  ```

4. <span data-ttu-id="31fab-180">Konfigurálja az Azure nyilvános társviszony-létesítést a kapcsolatcsoporthoz.</span><span class="sxs-lookup"><span data-stu-id="31fab-180">Configure Azure public peering for the circuit.</span></span> <span data-ttu-id="31fab-181">Győződjön meg arról, hogy rendelkezik a következő adatokat, mielőtt végrehajtásának folytatásához.</span><span class="sxs-lookup"><span data-stu-id="31fab-181">Make sure that you have the following information before you proceed further.</span></span>

  * <span data-ttu-id="31fab-182">Egy /30 alhálózat az elsődleges kapcsolat számára.</span><span class="sxs-lookup"><span data-stu-id="31fab-182">A /30 subnet for the primary link.</span></span> <span data-ttu-id="31fab-183">Ennek egy érvényes nyilvános IPv4-előtagnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="31fab-183">This must be a valid public IPv4 prefix.</span></span>
  * <span data-ttu-id="31fab-184">Egy /30 alhálózat a másodlagos kapcsolat számára.</span><span class="sxs-lookup"><span data-stu-id="31fab-184">A /30 subnet for the secondary link.</span></span> <span data-ttu-id="31fab-185">Ennek egy érvényes nyilvános IPv4-előtagnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="31fab-185">This must be a valid public IPv4 prefix.</span></span>
  * <span data-ttu-id="31fab-186">Egy érvényes VLAN-azonosító a tárviszony-létesítés létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="31fab-186">A valid VLAN ID to establish this peering on.</span></span> <span data-ttu-id="31fab-187">Győződjön meg róla, hogy a kapcsolatcsoporton egy másik társviszony-létesítés sem használja ugyanezt a VLAN-azonosítót.</span><span class="sxs-lookup"><span data-stu-id="31fab-187">Ensure that no other peering in the circuit uses the same VLAN ID.</span></span>
  * <span data-ttu-id="31fab-188">Egy AS-szám a társviszony-létesítéshez.</span><span class="sxs-lookup"><span data-stu-id="31fab-188">AS number for peering.</span></span> <span data-ttu-id="31fab-189">2 és 4 bájtos AS-számokat is használhat.</span><span class="sxs-lookup"><span data-stu-id="31fab-189">You can use both 2-byte and 4-byte AS numbers.</span></span>
  * <span data-ttu-id="31fab-190">**Választható -** az MD5 kivonatoló, ha úgy dönt, hogy egyetlen módszer alkalmazása.</span><span class="sxs-lookup"><span data-stu-id="31fab-190">**Optional -** An MD5 hash if you choose to use one.</span></span>

  <span data-ttu-id="31fab-191">Futtassa az alábbi példát követve konfigurálhatja az Azure nyilvános társviszony-létesítés a kör:</span><span class="sxs-lookup"><span data-stu-id="31fab-191">Run the following example to configure Azure public peering for your circuit:</span></span>

  ```azurecli
  az network express-route peering create --circuit-name MyCircuit --peer-asn 100 --primary-peer-subnet 12.0.0.0/30 -g ExpressRouteResourceGroup --secondary-peer-subnet 12.0.0.4/30 --vlan-id 200 --peering-type AzurePublicPeering
  ```

  <span data-ttu-id="31fab-192">Ha az MD5 kivonatoló használatát választja, használja a következő példát:</span><span class="sxs-lookup"><span data-stu-id="31fab-192">If you choose to use an MD5 hash, use the following example:</span></span>

  ```azurecli
  az network express-route peering create --circuit-name MyCircuit --peer-asn 100 --primary-peer-subnet 12.0.0.0/30 -g ExpressRouteResourceGroup --secondary-peer-subnet 12.0.0.4/30 --vlan-id 200 --peering-type AzurePublicPeering --SharedKey "A1B2C3D4"
  ```

  > [!IMPORTANT]
  > <span data-ttu-id="31fab-193">Az AS-számot mindenképp társviszony-létesítési ASN-ként, és ne ügyfél ASN-ként adja meg.</span><span class="sxs-lookup"><span data-stu-id="31fab-193">Ensure that you specify your AS number as peering ASN, not customer ASN.</span></span>

### <a name="to-view-azure-public-peering-details"></a><span data-ttu-id="31fab-194">Azure nyilvános társviszony-létesítés részleteinek megtekintése</span><span class="sxs-lookup"><span data-stu-id="31fab-194">To view Azure public peering details</span></span>

<span data-ttu-id="31fab-195">Az alábbi példa konfigurációs részletek szerezheti be:</span><span class="sxs-lookup"><span data-stu-id="31fab-195">You can get configuration details using the following example:</span></span>

```azurecli
az network express-route peering show -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePublicPeering
```

<span data-ttu-id="31fab-196">A kimenet a következő példához hasonló:</span><span class="sxs-lookup"><span data-stu-id="31fab-196">The output is similar to the following example:</span></span>

```azurecli
{
  "azureAsn": 12076,
  "etag": "W/\"2e97be83-a684-4f29-bf3c-96191e270666\"",
  "gatewayManagerEtag": "18",
  "id": "/subscriptions/9a0c2943-e0c2-4608-876c-e0ddffd1211b/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/MyCircuit/peerings/AzurePublicPeering",
  "lastModifiedBy": "Customer",
  "microsoftPeeringConfig": null,
  "name": "AzurePublicPeering",
  "peerAsn": 7671,
  "peeringType": "AzurePublicPeering",
  "primaryAzurePort": "",
  "primaryPeerAddressPrefix": "",
  "provisioningState": "Succeeded",
  "resourceGroup": "ExpressRouteResourceGroup",
  "routeFilter": null,
  "secondaryAzurePort": "",
  "secondaryPeerAddressPrefix": "",
  "sharedKey": null,
  "state": "Enabled",
  "stats": null,
  "vlanId": 100
}
```

### <a name="to-update-azure-public-peering-configuration"></a><span data-ttu-id="31fab-197">Azure nyilvános társviszony-létesítés konfigurációjának frissítése</span><span class="sxs-lookup"><span data-stu-id="31fab-197">To update Azure public peering configuration</span></span>

<span data-ttu-id="31fab-198">Frissítheti, az alábbi példa konfiguráció bármely részeként.</span><span class="sxs-lookup"><span data-stu-id="31fab-198">You can update any part of the configuration using the following example.</span></span> <span data-ttu-id="31fab-199">Ebben a példában a VLAN-Azonosítót a kapcsolatcsoport, frissül a 200-as 600 értékre.</span><span class="sxs-lookup"><span data-stu-id="31fab-199">In this example, the VLAN ID of the circuit is being updated from 200 to 600.</span></span>

```azurecli
az network express-route peering update --vlan-id 600 -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePublicPeering
```

### <a name="to-delete-azure-public-peering"></a><span data-ttu-id="31fab-200">Azure nyilvános társviszony-létesítés törlése</span><span class="sxs-lookup"><span data-stu-id="31fab-200">To delete Azure public peering</span></span>

<span data-ttu-id="31fab-201">Futtassa a következő példa a társviszony-létesítési konfiguráció eltávolítása:</span><span class="sxs-lookup"><span data-stu-id="31fab-201">You can remove your peering configuration by running the following example:</span></span>

```azurecli
az network express-route peering delete -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePublicPeering
```

## <a name="microsoft-peering"></a><span data-ttu-id="31fab-202">Microsoft társviszony-létesítés</span><span class="sxs-lookup"><span data-stu-id="31fab-202">Microsoft peering</span></span>

<span data-ttu-id="31fab-203">Ez a szakasz segítséget nyújt a létrehozása, beolvasása, frissítése és a Microsoft társviszony-létesítési konfiguráció az ExpressRoute-kapcsolatcsoportot törli.</span><span class="sxs-lookup"><span data-stu-id="31fab-203">This section helps you create, get, update, and delete the Microsoft peering configuration for an ExpressRoute circuit.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="31fab-204">A Microsoft társviszony-létesítést az ExpressRoute-Kapcsolatcsoportok 2017. augusztus 1. előtt konfigurált meghirdetett keresztül a Microsoft társviszony-létesítést, még akkor is, ha az útvonal-szűrők nem definiált összes szolgáltatás előtagok fog rendelkezni.</span><span class="sxs-lookup"><span data-stu-id="31fab-204">Microsoft peering of ExpressRoute circuits that were configured prior to August 1, 2017 will have all service prefixes advertised through the Microsoft peering, even if route filters are not defined.</span></span> <span data-ttu-id="31fab-205">A Microsoft társviszony-létesítést az ExpressRoute-Kapcsolatcsoportok vannak konfigurálva, vagy azt követően 2017. augusztus 1. nem rendelkezik a előtagokat amíg útvonal szűrő nem csatlakoztatja a kapcsolatcsoport hirdetve.</span><span class="sxs-lookup"><span data-stu-id="31fab-205">Microsoft peering of ExpressRoute circuits that are configured on or after August 1, 2017 will not have any prefixes advertised until a route filter is attached to the circuit.</span></span> <span data-ttu-id="31fab-206">További információkért lásd: [konfigurálása a Microsoft társviszony-létesítéshez útvonal szűrő](how-to-routefilter-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="31fab-206">For more information, see [Configure a route filter for Microsoft peering](how-to-routefilter-powershell.md).</span></span>
> 
> 

### <a name="to-create-microsoft-peering"></a><span data-ttu-id="31fab-207">Microsoft társviszony-létesítés létrehozása</span><span class="sxs-lookup"><span data-stu-id="31fab-207">To create Microsoft peering</span></span>

1. <span data-ttu-id="31fab-208">Az Azure parancssori felület legújabb verziójának telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="31fab-208">Install the latest version of Azure CLI.</span></span> <span data-ttu-id="31fab-209">Az az Azure parancssori felület (CLI) legújabb verzióját használja. * tekintse át a [Előfeltételek](expressroute-prerequisites.md) és [munkafolyamatok](expressroute-workflows.md) konfigurálás elkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="31fab-209">Use the latest version of the Azure Command-line Interface (CLI).* Review the [prerequisites](expressroute-prerequisites.md) and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>

  ```azurecli
  az login
  ```

  <span data-ttu-id="31fab-210">Válassza ki az előfizetést, amelyekhez ExpressRoute-kapcsolatcsoportot létrehozni.</span><span class="sxs-lookup"><span data-stu-id="31fab-210">Select the subscription for which you want to create ExpressRoute circuit.</span></span>

  ```azurecli
  az account set --subscription "<subscription ID>"
  ```
2. <span data-ttu-id="31fab-211">Hozzon létre egy ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="31fab-211">Create an ExpressRoute circuit.</span></span> <span data-ttu-id="31fab-212">Kövesse az utasításokat az [ExpressRoute-kapcsolatcsoport](howto-circuit-cli.md) létrehozásához, és kérje meg kapcsolatszolgáltatóját, hogy ossza ki azt.</span><span class="sxs-lookup"><span data-stu-id="31fab-212">Follow the instructions to create an [ExpressRoute circuit](howto-circuit-cli.md) and have it provisioned by the connectivity provider.</span></span>

  <span data-ttu-id="31fab-213">Ha kapcsolatszolgáltatója felügyelt 3. rétegbeli szolgáltatásokat kínál, igényelheti tőle, hogy engedélyezze Önnek az Azure privát társviszony-létesítést.</span><span class="sxs-lookup"><span data-stu-id="31fab-213">If your connectivity provider offers managed Layer 3 services, you can request your connectivity provider to enable Azure private peering for you.</span></span> <span data-ttu-id="31fab-214">Ebben az esetben nem szükséges a következő szakaszokban foglalt lépéseket végrehajtania.</span><span class="sxs-lookup"><span data-stu-id="31fab-214">In that case, you won't need to follow instructions listed in the next sections.</span></span> <span data-ttu-id="31fab-215">Azonban ha a kapcsolat szolgáltatójánál nem kezeli az útválasztást, a kapcsolat létrehozása után továbbra is a konfiguráció a következő lépéseket.</span><span class="sxs-lookup"><span data-stu-id="31fab-215">However, if your connectivity provider does not manage routing for you, after creating your circuit, continue your configuration using the next steps.</span></span>

3. <span data-ttu-id="31fab-216">Ellenőrizze, hogy üzembe helyezve, és is engedélyezve van az ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="31fab-216">Check the ExpressRoute circuit to make sure it is provisioned and also enabled.</span></span> <span data-ttu-id="31fab-217">Használja a következő példát:</span><span class="sxs-lookup"><span data-stu-id="31fab-217">Use the following example:</span></span>

  ```azurecli
  az network express-route list
  ```

  <span data-ttu-id="31fab-218">A rendszer a választ az alábbi példához hasonló:</span><span class="sxs-lookup"><span data-stu-id="31fab-218">The response is similar to the following example:</span></span>

  ```azurecli
  "allowClassicOperations": false,
  "authorizations": [],
  "circuitProvisioningState": "Enabled",
  "etag": "W/\"1262c492-ffef-4a63-95a8-a6002736b8c4\"",
  "gatewayManagerEtag": null,
  "id": "/subscriptions/81ab786c-56eb-4a4d-bb5f-f60329772466/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/MyCircuit",
  "location": "westus",
  "name": "MyCircuit",
  "peerings": [],
  "provisioningState": "Succeeded",
  "resourceGroup": "ExpressRouteResourceGroup",
  "serviceKey": "1d05cf70-1db5-419f-ad86-1ca62c3c125b",
  "serviceProviderNotes": null,
  "serviceProviderProperties": {
    "bandwidthInMbps": 200,
    "peeringLocation": "Silicon Valley",
    "serviceProviderName": "Equinix"
  },
  "serviceProviderProvisioningState": "Provisioned",
  "sku": {
    "family": "UnlimitedData",
    "name": "Standard_MeteredData",
    "tier": "Standard"
  },
  "tags": null,
  "type": "Microsoft.Network/expressRouteCircuits]
  ```

4. <span data-ttu-id="31fab-219">Konfigurálja a Microsoft társviszony-létesítést a kapcsolatcsoporthoz.</span><span class="sxs-lookup"><span data-stu-id="31fab-219">Configure Microsoft peering for the circuit.</span></span> <span data-ttu-id="31fab-220">Mielőtt folytatná, ellenőrizze az alábbi információk meglétét.</span><span class="sxs-lookup"><span data-stu-id="31fab-220">Make sure that you have the following information before you proceed.</span></span>

  * <span data-ttu-id="31fab-221">Egy /30 alhálózat az elsődleges kapcsolat számára.</span><span class="sxs-lookup"><span data-stu-id="31fab-221">A /30 subnet for the primary link.</span></span> <span data-ttu-id="31fab-222">Ennek egy érvényes nyilvános IPv4-előtagnak kell lennie, amely az Ön tulajdonában van, és regisztrálva van egy RIR-/IRR-jegyzékben.</span><span class="sxs-lookup"><span data-stu-id="31fab-222">This must be a valid public IPv4 prefix owned by you and registered in an RIR / IRR.</span></span>
  * <span data-ttu-id="31fab-223">Egy /30 alhálózat a másodlagos kapcsolat számára.</span><span class="sxs-lookup"><span data-stu-id="31fab-223">A /30 subnet for the secondary link.</span></span> <span data-ttu-id="31fab-224">Ennek egy érvényes nyilvános IPv4-előtagnak kell lennie, amely az Ön tulajdonában van, és regisztrálva van egy RIR-/IRR-jegyzékben.</span><span class="sxs-lookup"><span data-stu-id="31fab-224">This must be a valid public IPv4 prefix owned by you and registered in an RIR / IRR.</span></span>
  * <span data-ttu-id="31fab-225">Egy érvényes VLAN-azonosító a tárviszony-létesítés létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="31fab-225">A valid VLAN ID to establish this peering on.</span></span> <span data-ttu-id="31fab-226">Győződjön meg róla, hogy a kapcsolatcsoporton egy másik társviszony-létesítés sem használja ugyanezt a VLAN-azonosítót.</span><span class="sxs-lookup"><span data-stu-id="31fab-226">Ensure that no other peering in the circuit uses the same VLAN ID.</span></span>
  * <span data-ttu-id="31fab-227">Egy AS-szám a társviszony-létesítéshez.</span><span class="sxs-lookup"><span data-stu-id="31fab-227">AS number for peering.</span></span> <span data-ttu-id="31fab-228">2 és 4 bájtos AS-számokat is használhat.</span><span class="sxs-lookup"><span data-stu-id="31fab-228">You can use both 2-byte and 4-byte AS numbers.</span></span>
  * <span data-ttu-id="31fab-229">Meghirdetett előtagok: Meg kell adnia a BGP-munkamenetben meghirdetni kívánt összes előtag listáját.</span><span class="sxs-lookup"><span data-stu-id="31fab-229">Advertised prefixes: You must provide a list of all prefixes you plan to advertise over the BGP session.</span></span> <span data-ttu-id="31fab-230">A rendszer kizárólag a nyilvános IP-cím-előtagokat fogadja el.</span><span class="sxs-lookup"><span data-stu-id="31fab-230">Only public IP address prefixes are accepted.</span></span> <span data-ttu-id="31fab-231">Ha szeretne elküldhető a előtagokat, elküldheti egy vesszővel elválasztott listában.</span><span class="sxs-lookup"><span data-stu-id="31fab-231">If you plan to send a set of prefixes, you can send a comma-separated list.</span></span> <span data-ttu-id="31fab-232">Az előtagoknak egy RIR/IRR jegyzékben regisztrálva kell lenniük az Ön neve alatt.</span><span class="sxs-lookup"><span data-stu-id="31fab-232">These prefixes must be registered to you in an RIR / IRR.</span></span>
  * <span data-ttu-id="31fab-233">**Választható -** ügyfél ASN: Ha nincs regisztrálva a társviszony-létesítés SZÁMOT hirdetési előtagok, megadhatja a AS számot, amelyhez regisztrálja azokat a rendszer.</span><span class="sxs-lookup"><span data-stu-id="31fab-233">**Optional -** Customer ASN: If you are advertising prefixes that are not registered to the peering AS number, you can specify the AS number to which they are registered.</span></span>
  * <span data-ttu-id="31fab-234">Útválasztási jegyzék neve: Megadhatja az RIR/IRR jegyzék nevét, amelyben az AS-szám és az előtagok regisztrálva vannak.</span><span class="sxs-lookup"><span data-stu-id="31fab-234">Routing Registry Name: You can specify the RIR / IRR against which the AS number and prefixes are registered.</span></span>
  * <span data-ttu-id="31fab-235">**Választható -** az MD5 kivonatoló, ha úgy dönt, hogy egyetlen módszer alkalmazása.</span><span class="sxs-lookup"><span data-stu-id="31fab-235">**Optional -** An MD5 hash if you choose to use one.</span></span>

   <span data-ttu-id="31fab-236">Futtassa a következő példa a kör társviszony Microsoft konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="31fab-236">Run the following example to configure Microsoft peering for your circuit:</span></span>

  ```azurecli
  az network express-route peering create --circuit-name MyCircuit --peer-asn 100 --primary-peer-subnet 123.0.0.0/30 -g ExpressRouteResourceGroup --secondary-peer-subnet 123.0.0.4/30 --vlan-id 300 --peering-type MicrosoftPeering --advertised-public-prefixes 123.1.0.0/24
  ```

### <a name="to-get-microsoft-peering-details"></a><span data-ttu-id="31fab-237">Microsoft társviszony-létesítés részleteinek lekérése</span><span class="sxs-lookup"><span data-stu-id="31fab-237">To get Microsoft peering details</span></span>

<span data-ttu-id="31fab-238">Konfigurációs részletek kaphat az alábbi példa:</span><span class="sxs-lookup"><span data-stu-id="31fab-238">You can get configuration details by using the following example:</span></span>

```azurecli
az network express-route peering show -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzureMicrosoftPeering
```

<span data-ttu-id="31fab-239">A kimenet a következő példához hasonló:</span><span class="sxs-lookup"><span data-stu-id="31fab-239">The output is similar to the following example:</span></span>

```azurecli
{
  "azureAsn": 12076,
  "etag": "W/\"2e97be83-a684-4f29-bf3c-96191e270666\"",
  "gatewayManagerEtag": "18",
  "id": "/subscriptions/9a0c2943-e0c2-4608-876c-e0ddffd1211b/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/MyCircuit/peerings/AzureMicrosoftPeering",
  "lastModifiedBy": "Customer",
  "microsoftPeeringConfig": {
    "advertisedPublicPrefixes": [
        ""
      ],
     "advertisedPublicPrefixesState": "",
     "customerASN": ,
     "routingRegistryName": ""
  }
  "name": "AzureMicrosoftPeering",
  "peerAsn": ,
  "peeringType": "AzureMicrosoftPeering",
  "primaryAzurePort": "",
  "primaryPeerAddressPrefix": "",
  "provisioningState": "Succeeded",
  "resourceGroup": "ExpressRouteResourceGroup",
  "routeFilter": null,
  "secondaryAzurePort": "",
  "secondaryPeerAddressPrefix": "",
  "sharedKey": null,
  "state": "Enabled",
  "stats": null,
  "vlanId": 100
}
```

### <a name="to-update-microsoft-peering-configuration"></a><span data-ttu-id="31fab-240">Microsoft társviszony-létesítés konfigurációjának frissítése</span><span class="sxs-lookup"><span data-stu-id="31fab-240">To update Microsoft peering configuration</span></span>

<span data-ttu-id="31fab-241">Frissítheti, a konfiguráció bármely részeként.</span><span class="sxs-lookup"><span data-stu-id="31fab-241">You can update any part of the configuration.</span></span> <span data-ttu-id="31fab-242">A hirdetett előtagjait, a kapcsolatcsoport 124.1.0.0/24 az alábbi példában a programból 123.1.0.0/24 frissített:</span><span class="sxs-lookup"><span data-stu-id="31fab-242">The advertised prefixes of the circuit are being updated from 123.1.0.0/24 to 124.1.0.0/24 in the following example:</span></span>

```azurecli
az network express-route peering update --circuit-name MyCircuit -g ExpressRouteResourceGroup --peering-type MicrosoftPeering --advertised-public-prefixes 124.1.0.0/24
```

### <a name="to-delete-microsoft-peering"></a><span data-ttu-id="31fab-243">Microsoft társviszony-létesítés törlése</span><span class="sxs-lookup"><span data-stu-id="31fab-243">To delete Microsoft peering</span></span>

<span data-ttu-id="31fab-244">Futtassa a következő példa a társviszony-létesítési konfiguráció eltávolítása:</span><span class="sxs-lookup"><span data-stu-id="31fab-244">You can remove your peering configuration by running the following example:</span></span>

```azurecli
az network express-route peering delete -g ExpressRouteResourceGroup --circuit-name MyCircuit --name MicrosoftPeering
```

## <a name="next-steps"></a><span data-ttu-id="31fab-245">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="31fab-245">Next steps</span></span>

<span data-ttu-id="31fab-246">A következő lépés egy [VNet csatlakoztatása egy ExpressRoute-kapcsolatcsoporthoz](howto-linkvnet-cli.md).</span><span class="sxs-lookup"><span data-stu-id="31fab-246">Next step, [Link a VNet to an ExpressRoute circuit](howto-linkvnet-cli.md).</span></span>

* <span data-ttu-id="31fab-247">Az ExpressRoute-munkafolyamatokkal kapcsolatos további információkért lásd: [ExpressRoute workflows](expressroute-workflows.md) (ExpressRoute-munkafolyamatok).</span><span class="sxs-lookup"><span data-stu-id="31fab-247">For more information about ExpressRoute workflows, see [ExpressRoute workflows](expressroute-workflows.md).</span></span>
* <span data-ttu-id="31fab-248">A kapcsolatcsoportok társviszony-létesítéseivel kapcsolatos további információkért lásd: [ExpressRoute circuits and routing domains](expressroute-circuit-peerings.md) (ExpressRoute-kapcsolatcsoportok és útválasztási tartományok).</span><span class="sxs-lookup"><span data-stu-id="31fab-248">For more information about circuit peering, see [ExpressRoute circuits and routing domains](expressroute-circuit-peerings.md).</span></span>
* <span data-ttu-id="31fab-249">További információkért a virtuális hálózatok használatáról lásd: [Virtual network overview](../virtual-network/virtual-networks-overview.md) (Virtuális hálózatok áttekintése).</span><span class="sxs-lookup"><span data-stu-id="31fab-249">For more information about working with virtual networks, see [Virtual network overview](../virtual-network/virtual-networks-overview.md).</span></span>