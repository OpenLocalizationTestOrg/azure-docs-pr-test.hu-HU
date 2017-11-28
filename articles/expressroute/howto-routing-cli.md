---
title: "Hogyan tooconfigure útválasztást Azure ExpressRoute-kapcsolatcsoportot: CLI |} Microsoft Docs"
description: "Ez a cikk segít létrehozásának és kiosztásának hello saját, a nyilvános és a Microsoft társviszony-létesítést az ExpressRoute-kapcsolatcsoportot. Ez a cikk is bemutatja, hogyan toocheck hello állapotát, a frissítést, vagy a-kör társviszony törlése."
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
ms.openlocfilehash: 33130af050045527cdb316e77821c6d101b6a82a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-modify-routing-for-an-expressroute-circuit-using-cli"></a><span data-ttu-id="961eb-104">Létrehozásához és módosításához útválasztás az ExpressRoute-kapcsolatcsoportot parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="961eb-104">Create and modify routing for an ExpressRoute circuit using CLI</span></span>

<span data-ttu-id="961eb-105">Ez a cikk segít hozhat létre és kezelhet egy ExpressRoute-kapcsolatcsoportot hello Resource Manager üzembe helyezési modellel parancssori felület használatával útválasztási konfigurációja.</span><span class="sxs-lookup"><span data-stu-id="961eb-105">This article helps you create and manage routing configuration for an ExpressRoute circuit in hello Resource Manager deployment model using CLI.</span></span> <span data-ttu-id="961eb-106">Is hello állapota, update vagy delete ellenőrizze és kiosztásának megszüntetése ExpressRoute-kör társviszony.</span><span class="sxs-lookup"><span data-stu-id="961eb-106">You can also check hello status, update, or delete and deprovision peerings for an ExpressRoute circuit.</span></span> <span data-ttu-id="961eb-107">Ha egy másik módszer toowork toouse a kapcsolatcsoport rendelkező, jelölje be a cikk a következő lista hello:</span><span class="sxs-lookup"><span data-stu-id="961eb-107">If you want toouse a different method toowork with your circuit, select an article from hello following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="961eb-108">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="961eb-108">Azure portal</span></span>](expressroute-howto-routing-portal-resource-manager.md)
> * [<span data-ttu-id="961eb-109">PowerShell</span><span class="sxs-lookup"><span data-stu-id="961eb-109">PowerShell</span></span>](expressroute-howto-routing-arm.md)
> * [<span data-ttu-id="961eb-110">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="961eb-110">Azure CLI</span></span>](howto-routing-cli.md)
> * [<span data-ttu-id="961eb-111">Video - privát társviszony-létesítés</span><span class="sxs-lookup"><span data-stu-id="961eb-111">Video - Private peering</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-private-peering-for-your-expressroute-circuit)
> * [<span data-ttu-id="961eb-112">Video - nyilvános társviszony-létesítés</span><span class="sxs-lookup"><span data-stu-id="961eb-112">Video - Public peering</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-public-peering-for-your-expressroute-circuit)
> * [<span data-ttu-id="961eb-113">Videó – a Microsoft társviszony-létesítés</span><span class="sxs-lookup"><span data-stu-id="961eb-113">Video - Microsoft peering</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-microsoft-peering-for-your-expressroute-circuit)
> * [<span data-ttu-id="961eb-114">PowerShell (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="961eb-114">PowerShell (classic)</span></span>](expressroute-howto-routing-classic.md)
> 

## <a name="configuration-prerequisites"></a><span data-ttu-id="961eb-115">Konfigurációs előfeltételek</span><span class="sxs-lookup"><span data-stu-id="961eb-115">Configuration prerequisites</span></span>

* <span data-ttu-id="961eb-116">Megkezdése előtt telepítse a legújabb verziójának hello hello parancssori felület parancsait (2.0-s vagy újabb).</span><span class="sxs-lookup"><span data-stu-id="961eb-116">Before beginning, install hello latest version of hello CLI commands (2.0 or later).</span></span> <span data-ttu-id="961eb-117">Hello parancssori felület parancsait telepítésével kapcsolatos információkért lásd: [Azure CLI 2.0 telepítése](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="961eb-117">For information about installing hello CLI commands, see [Install Azure CLI 2.0](/cli/azure/install-azure-cli).</span></span>
* <span data-ttu-id="961eb-118">Győződjön meg arról, hogy átolvasta hello [Előfeltételek](expressroute-prerequisites.md), [útválasztási követelmények](expressroute-routing.md), és [munkafolyamat](expressroute-workflows.md) lapok: konfigurálás elkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="961eb-118">Make sure that you have reviewed hello [prerequisites](expressroute-prerequisites.md), [routing requirements](expressroute-routing.md), and [workflow](expressroute-workflows.md) pages before you begin configuration.</span></span>
* <span data-ttu-id="961eb-119">Egy aktív ExpressRoute-kapcsolatcsoportra lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="961eb-119">You must have an active ExpressRoute circuit.</span></span> <span data-ttu-id="961eb-120">Útmutatás alapján hello túl[ExpressRoute-kapcsolatcsoportot létrehozni](howto-circuit-cli.md) , és folytassa a kapcsolat szolgáltatójánál előtt által engedélyezett hello áramkör.</span><span class="sxs-lookup"><span data-stu-id="961eb-120">Follow hello instructions too[Create an ExpressRoute circuit](howto-circuit-cli.md) and have hello circuit enabled by your connectivity provider before you proceed.</span></span> <span data-ttu-id="961eb-121">hello ExpressRoute-kapcsolatcsoportot meg toobe képes toorun hello parancsok ebben a cikkben kiépített és engedélyezett állapotban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="961eb-121">hello ExpressRoute circuit must be in a provisioned and enabled state for you toobe able toorun hello commands in this article.</span></span>

<span data-ttu-id="961eb-122">Ezek az utasítások csak a 2. rétegbeli kapcsolatot szolgáltatásokat nyújtó szolgáltatók létre toocircuits vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="961eb-122">These instructions only apply toocircuits created with service providers offering Layer 2 connectivity services.</span></span> <span data-ttu-id="961eb-123">A szolgáltató által kezelt használata réteg (általában egy IPVPN, például az MPLS) 3 szolgáltatások, a kapcsolat szolgáltatójánál konfigurálása és kezelése az Ön útválasztást.</span><span class="sxs-lookup"><span data-stu-id="961eb-123">If you are using a service provider that offers managed Layer 3 services (typically an IPVPN, like MPLS), your connectivity provider will configure and manage routing for you.</span></span>

<span data-ttu-id="961eb-124">Beállíthatja, hogy egy, kettő vagy ExpressRoute-kör összes három esetében (Azure saját, az Azure nyilvános, és a Microsoft).</span><span class="sxs-lookup"><span data-stu-id="961eb-124">You can configure one, two, or all three peerings (Azure private, Azure public, and Microsoft) for an ExpressRoute circuit.</span></span> <span data-ttu-id="961eb-125">A társviszony-létesítéseket tetszőleges sorrendben konfigurálhatja.</span><span class="sxs-lookup"><span data-stu-id="961eb-125">You can configure peerings in any order you choose.</span></span> <span data-ttu-id="961eb-126">Azonban kell győződjön meg arról, hogy elvégezte-e minden társviszony-létesítési egyszerre csak egy hello konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="961eb-126">However, you must make sure that you complete hello configuration of each peering one at a time.</span></span>

## <a name="azure-private-peering"></a><span data-ttu-id="961eb-127">Azure privát társviszony-létesítés</span><span class="sxs-lookup"><span data-stu-id="961eb-127">Azure private peering</span></span>

<span data-ttu-id="961eb-128">Ez a szakasz segítséget nyújt a létrehozása, beolvasása, frissítése és törlése hello Azure privát társviszony-létesítési konfiguráció ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="961eb-128">This section helps you create, get, update, and delete hello Azure private peering configuration for an ExpressRoute circuit.</span></span>

### <a name="toocreate-azure-private-peering"></a><span data-ttu-id="961eb-129">az Azure magánhálózati társviszony-létesítés toocreate</span><span class="sxs-lookup"><span data-stu-id="961eb-129">toocreate Azure private peering</span></span>

1. <span data-ttu-id="961eb-130">Hello Azure parancssori felület legújabb verziójának telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="961eb-130">Install hello latest version of Azure CLI.</span></span> <span data-ttu-id="961eb-131">Hello hello Azure parancssori felület (CLI) legújabb verzióját kell használnia. * felülvizsgálati hello [Előfeltételek](expressroute-prerequisites.md) és [munkafolyamatok](expressroute-workflows.md) konfigurálás elkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="961eb-131">You must use hello latest version of hello Azure Command-line Interface (CLI).* Review hello [prerequisites](expressroute-prerequisites.md) and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>

  ```azurecli
  az login
  ```

  <span data-ttu-id="961eb-132">Válassza ki a kívánt toocreate ExpressRoute-kapcsolatcsoportot hello előfizetés</span><span class="sxs-lookup"><span data-stu-id="961eb-132">Select hello subscription you want toocreate ExpressRoute circuit</span></span>

  ```azurecli
  az account set --subscription "<subscription ID>"
  ```
2. <span data-ttu-id="961eb-133">Hozzon létre egy ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="961eb-133">Create an ExpressRoute circuit.</span></span> <span data-ttu-id="961eb-134">Kövesse az utasításokat toocreate hello egy [ExpressRoute-kapcsolatcsoportot](howto-circuit-cli.md) , illetve hozta-e hello kapcsolat szolgáltatóját.</span><span class="sxs-lookup"><span data-stu-id="961eb-134">Follow hello instructions toocreate an [ExpressRoute circuit](howto-circuit-cli.md) and have it provisioned by hello connectivity provider.</span></span>

  <span data-ttu-id="961eb-135">Ha a kapcsolat szolgáltatójánál 3. rétegbeli felügyelt szolgáltatásokat kínál, a kapcsolat szolgáltató tooenable magánhálózati társviszony-létesítést az Ön Azure kérhet.</span><span class="sxs-lookup"><span data-stu-id="961eb-135">If your connectivity provider offers managed Layer 3 services, you can request your connectivity provider tooenable Azure private peering for you.</span></span> <span data-ttu-id="961eb-136">Ebben az esetben nincs szükség hello következő szakaszokban szereplő toofollow utasításokat.</span><span class="sxs-lookup"><span data-stu-id="961eb-136">In that case, you won't need toofollow instructions listed in hello next sections.</span></span> <span data-ttu-id="961eb-137">Azonban ha a kapcsolat szolgáltatójánál nem kezeli az útválasztást, a kapcsolat létrehozása után továbbra is hello lépések a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="961eb-137">However, if your connectivity provider does not manage routing for you, after creating your circuit, continue your configuration using hello next steps.</span></span>
3. <span data-ttu-id="961eb-138">Ellenőrizze a hello ExpressRoute körön toomake arról, hogy van üzembe helyezve és is engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="961eb-138">Check hello ExpressRoute circuit toomake sure it is provisioned and also enabled.</span></span> <span data-ttu-id="961eb-139">A következő példa hello használata:</span><span class="sxs-lookup"><span data-stu-id="961eb-139">Use hello following example:</span></span>

  ```azurecli
  az network express-route show --resource-group ExpressRouteResourceGroup --name MyCircuit
  ```

  <span data-ttu-id="961eb-140">a rendszer a következő példa hasonló toohello hello választ:</span><span class="sxs-lookup"><span data-stu-id="961eb-140">hello response is similar toohello following example:</span></span>

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

4. <span data-ttu-id="961eb-141">Az Azure magánhálózati társviszony-létesítés hello kör megadása</span><span class="sxs-lookup"><span data-stu-id="961eb-141">Configure Azure private peering for hello circuit.</span></span> <span data-ttu-id="961eb-142">Győződjön meg arról, hogy rendelkezik-e hello hello lépések folytatása előtt a következő elemek:</span><span class="sxs-lookup"><span data-stu-id="961eb-142">Make sure that you have hello following items before you proceed with hello next steps:</span></span>

  * <span data-ttu-id="961eb-143">/ 30-as alhálózat hello elsődleges kapcsolathoz.</span><span class="sxs-lookup"><span data-stu-id="961eb-143">A /30 subnet for hello primary link.</span></span> <span data-ttu-id="961eb-144">hello alhálózat nem virtuális hálózatok számára fenntartott címtartomány részének kell lennie.</span><span class="sxs-lookup"><span data-stu-id="961eb-144">hello subnet must not be part of any address space reserved for virtual networks.</span></span>
  * <span data-ttu-id="961eb-145">/ 30-as alhálózat hello másodlagos kapcsolathoz.</span><span class="sxs-lookup"><span data-stu-id="961eb-145">A /30 subnet for hello secondary link.</span></span> <span data-ttu-id="961eb-146">hello alhálózat nem virtuális hálózatok számára fenntartott címtartomány részének kell lennie.</span><span class="sxs-lookup"><span data-stu-id="961eb-146">hello subnet must not be part of any address space reserved for virtual networks.</span></span>
  * <span data-ttu-id="961eb-147">Egy érvényes VLAN-azonosító tooestablish a társviszony.</span><span class="sxs-lookup"><span data-stu-id="961eb-147">A valid VLAN ID tooestablish this peering on.</span></span> <span data-ttu-id="961eb-148">Győződjön meg arról, hogy egyetlen másik társviszonya se a hello áramkör használ hello azonos VLAN-azonosítót.</span><span class="sxs-lookup"><span data-stu-id="961eb-148">Ensure that no other peering in hello circuit uses hello same VLAN ID.</span></span>
  * <span data-ttu-id="961eb-149">Egy AS-szám a társviszony-létesítéshez.</span><span class="sxs-lookup"><span data-stu-id="961eb-149">AS number for peering.</span></span> <span data-ttu-id="961eb-150">2 és 4 bájtos AS-számokat is használhat.</span><span class="sxs-lookup"><span data-stu-id="961eb-150">You can use both 2-byte and 4-byte AS numbers.</span></span> <span data-ttu-id="961eb-151">Ehhez a társviszony-létesítéshez használhat privát AS-számokat is.</span><span class="sxs-lookup"><span data-stu-id="961eb-151">You can use a private AS number for this peering.</span></span> <span data-ttu-id="961eb-152">Ne használja a 65515 számot.</span><span class="sxs-lookup"><span data-stu-id="961eb-152">Ensure that you are not using 65515.</span></span>
  * <span data-ttu-id="961eb-153">**Választható -** az MD5 kivonatoló, ha úgy dönt, hogy egy toouse.</span><span class="sxs-lookup"><span data-stu-id="961eb-153">**Optional -** An MD5 hash if you choose toouse one.</span></span>

  <span data-ttu-id="961eb-154">A következő példa tooconfigure magánhálózati társviszony-létesítés a kör Azure hello használata:</span><span class="sxs-lookup"><span data-stu-id="961eb-154">Use hello following example tooconfigure Azure private peering for your circuit:</span></span>

  ```azurecli
  az network express-route peering create --circuit-name MyCircuit --peer-asn 100 --primary-peer-subnet 10.0.0.0/30 -g ExpressRouteResourceGroup --secondary-peer-subnet 10.0.0.4/30 --vlan-id 200 --peering-type AzurePrivatePeering
  ```

  <span data-ttu-id="961eb-155">Ha úgy dönt, toouse az MD5 kivonatoló, használja a következő példa hello:</span><span class="sxs-lookup"><span data-stu-id="961eb-155">If you choose toouse an MD5 hash, use hello following example:</span></span>

  ```azurecli
  az network express-route peering create --circuit-name MyCircuit --peer-asn 100 --primary-peer-subnet 10.0.0.0/30 -g ExpressRouteResourceGroup --secondary-peer-subnet 10.0.0.4/30 --vlan-id 200 --peering-type AzurePrivatePeering --SharedKey "A1B2C3D4"
  ```

  > [!IMPORTANT]
  > <span data-ttu-id="961eb-156">Az AS-számot mindenképp társviszony-létesítési ASN-ként, és ne ügyfél ASN-ként adja meg.</span><span class="sxs-lookup"><span data-stu-id="961eb-156">Ensure that you specify your AS number as peering ASN, not customer ASN.</span></span>
  > 
  > 

### <a name="tooview-azure-private-peering-details"></a><span data-ttu-id="961eb-157">tooview Azure magánhálózati társviszony-létesítés részletei</span><span class="sxs-lookup"><span data-stu-id="961eb-157">tooview Azure private peering details</span></span>

<span data-ttu-id="961eb-158">Konfigurációs részletek kaphat a következő példa hello használata:</span><span class="sxs-lookup"><span data-stu-id="961eb-158">You can get configuration details by using hello following example:</span></span>

```azurecli
az network express-route peering show -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePrivatePeering
```

<span data-ttu-id="961eb-159">hello hasonló toohello a következő példa a kimenetre:</span><span class="sxs-lookup"><span data-stu-id="961eb-159">hello output is similar toohello following example:</span></span>

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

### <a name="tooupdate-azure-private-peering-configuration"></a><span data-ttu-id="961eb-160">tooupdate Azure magánhálózati társviszony-létesítési konfiguráció</span><span class="sxs-lookup"><span data-stu-id="961eb-160">tooupdate Azure private peering configuration</span></span>

<span data-ttu-id="961eb-161">Frissítheti, használja a következő példa hello hello konfiguráció bármely részeként.</span><span class="sxs-lookup"><span data-stu-id="961eb-161">You can update any part of hello configuration using hello following example.</span></span> <span data-ttu-id="961eb-162">Ebben a példában a 100 too500 hello VLAN-Azonosítót a kapcsolatcsoport hello frissül.</span><span class="sxs-lookup"><span data-stu-id="961eb-162">In this example, hello VLAN ID of hello circuit is being updated from 100 too500.</span></span>

```azurecli
az network express-route peering update --vlan-id 500 -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePrivatePeering
```

### <a name="toodelete-azure-private-peering"></a><span data-ttu-id="961eb-163">az Azure magánhálózati társviszony-létesítés toodelete</span><span class="sxs-lookup"><span data-stu-id="961eb-163">toodelete Azure private peering</span></span>

<span data-ttu-id="961eb-164">A társviszony-létesítési konfiguráció a következő példa hello futtatásával távolíthatja el:</span><span class="sxs-lookup"><span data-stu-id="961eb-164">You can remove your peering configuration by running hello following example:</span></span>

> [!WARNING]
> <span data-ttu-id="961eb-165">Bizonyosodjon meg, hogy az összes virtuális hálózatot az ExpressRoute-kapcsolatcsoportot hello megszüntetni ebben a példában futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="961eb-165">You must ensure that all virtual networks are unlinked from hello ExpressRoute circuit before running this example.</span></span> 
> 
> 

```azurecli
az network express-route peering delete -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePrivatePeering
```

## <a name="azure-public-peering"></a><span data-ttu-id="961eb-166">Azure nyilvános társviszony-létesítés</span><span class="sxs-lookup"><span data-stu-id="961eb-166">Azure public peering</span></span>

<span data-ttu-id="961eb-167">Ez a szakasz segítséget nyújt a létrehozása, beolvasása, frissítése és hello Azure nyilvános társviszony-létesítési konfiguráció ExpressRoute-kapcsolatcsoportot törli.</span><span class="sxs-lookup"><span data-stu-id="961eb-167">This section helps you create, get, update, and delete hello Azure public peering configuration for an ExpressRoute circuit.</span></span>

### <a name="toocreate-azure-public-peering"></a><span data-ttu-id="961eb-168">az Azure nyilvános társviszony toocreate</span><span class="sxs-lookup"><span data-stu-id="961eb-168">toocreate Azure public peering</span></span>

1. <span data-ttu-id="961eb-169">Hello Azure parancssori felület legújabb verziójának telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="961eb-169">Install hello latest version of Azure CLI.</span></span> <span data-ttu-id="961eb-170">Hello hello Azure parancssori felület (CLI) legújabb verzióját kell használnia. * felülvizsgálati hello [Előfeltételek](expressroute-prerequisites.md) és [munkafolyamatok](expressroute-workflows.md) konfigurálás elkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="961eb-170">You must use hello latest version of hello Azure Command-line Interface (CLI).* Review hello [prerequisites](expressroute-prerequisites.md) and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>

  ```azurecli
  az login
  ```

  <span data-ttu-id="961eb-171">Válassza ki a kívánt toocreate ExpressRoute-kapcsolatcsoportot hello előfizetést.</span><span class="sxs-lookup"><span data-stu-id="961eb-171">Select hello subscription for which you want toocreate ExpressRoute circuit.</span></span>

  ```azurecli
  az account set --subscription "<subscription ID>"
  ```
2. <span data-ttu-id="961eb-172">Hozzon létre egy ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="961eb-172">Create an ExpressRoute circuit.</span></span>  <span data-ttu-id="961eb-173">Kövesse az utasításokat toocreate hello egy [ExpressRoute-kapcsolatcsoportot](howto-circuit-cli.md) , illetve hozta-e hello kapcsolat szolgáltatóját.</span><span class="sxs-lookup"><span data-stu-id="961eb-173">Follow hello instructions toocreate an [ExpressRoute circuit](howto-circuit-cli.md) and have it provisioned by hello connectivity provider.</span></span>

  <span data-ttu-id="961eb-174">Ha a kapcsolat szolgáltatójánál felügyelt 3. rétegbeli szolgáltatásokat nyújt, kérheti, hogy a kapcsolat szolgáltatójánál lehetővé teszi, hogy az Azure magánhálózati társviszony-létesítés meg.</span><span class="sxs-lookup"><span data-stu-id="961eb-174">If your connectivity provider offers managed Layer 3 services, you can request that your connectivity provider enables Azure private peering for you.</span></span> <span data-ttu-id="961eb-175">Ebben az esetben nincs szükség hello következő szakaszokban szereplő toofollow utasításokat.</span><span class="sxs-lookup"><span data-stu-id="961eb-175">In that case, you won't need toofollow instructions listed in hello next sections.</span></span> <span data-ttu-id="961eb-176">Azonban ha a kapcsolat szolgáltatójánál nem kezeli az útválasztást, a kapcsolat létrehozása után továbbra is hello lépések a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="961eb-176">However, if your connectivity provider does not manage routing for you, after creating your circuit, continue your configuration using hello next steps.</span></span>
3. <span data-ttu-id="961eb-177">Ellenőrizze a hello ExpressRoute körön tooensure van üzembe helyezve, és is engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="961eb-177">Check hello ExpressRoute circuit tooensure it is provisioned and also enabled.</span></span> <span data-ttu-id="961eb-178">A következő példa hello használata:</span><span class="sxs-lookup"><span data-stu-id="961eb-178">Use hello following example:</span></span>

  ```azurecli
  az network express-route list
  ```

  <span data-ttu-id="961eb-179">a rendszer a következő példa hasonló toohello hello választ:</span><span class="sxs-lookup"><span data-stu-id="961eb-179">hello response is similar toohello following example:</span></span>

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

4. <span data-ttu-id="961eb-180">Az Azure nyilvános társviszony-létesítés hello kör megadása</span><span class="sxs-lookup"><span data-stu-id="961eb-180">Configure Azure public peering for hello circuit.</span></span> <span data-ttu-id="961eb-181">Győződjön meg arról, hogy rendelkezik-e hello előtt végrehajtásának folytatásához a következő információkat.</span><span class="sxs-lookup"><span data-stu-id="961eb-181">Make sure that you have hello following information before you proceed further.</span></span>

  * <span data-ttu-id="961eb-182">/ 30-as alhálózat hello elsődleges kapcsolathoz.</span><span class="sxs-lookup"><span data-stu-id="961eb-182">A /30 subnet for hello primary link.</span></span> <span data-ttu-id="961eb-183">Ennek egy érvényes nyilvános IPv4-előtagnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="961eb-183">This must be a valid public IPv4 prefix.</span></span>
  * <span data-ttu-id="961eb-184">/ 30-as alhálózat hello másodlagos kapcsolathoz.</span><span class="sxs-lookup"><span data-stu-id="961eb-184">A /30 subnet for hello secondary link.</span></span> <span data-ttu-id="961eb-185">Ennek egy érvényes nyilvános IPv4-előtagnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="961eb-185">This must be a valid public IPv4 prefix.</span></span>
  * <span data-ttu-id="961eb-186">Egy érvényes VLAN-azonosító tooestablish a társviszony.</span><span class="sxs-lookup"><span data-stu-id="961eb-186">A valid VLAN ID tooestablish this peering on.</span></span> <span data-ttu-id="961eb-187">Győződjön meg arról, hogy egyetlen másik társviszonya se a hello áramkör használ hello azonos VLAN-azonosítót.</span><span class="sxs-lookup"><span data-stu-id="961eb-187">Ensure that no other peering in hello circuit uses hello same VLAN ID.</span></span>
  * <span data-ttu-id="961eb-188">Egy AS-szám a társviszony-létesítéshez.</span><span class="sxs-lookup"><span data-stu-id="961eb-188">AS number for peering.</span></span> <span data-ttu-id="961eb-189">2 és 4 bájtos AS-számokat is használhat.</span><span class="sxs-lookup"><span data-stu-id="961eb-189">You can use both 2-byte and 4-byte AS numbers.</span></span>
  * <span data-ttu-id="961eb-190">**Választható -** az MD5 kivonatoló, ha úgy dönt, hogy egy toouse.</span><span class="sxs-lookup"><span data-stu-id="961eb-190">**Optional -** An MD5 hash if you choose toouse one.</span></span>

  <span data-ttu-id="961eb-191">Futtassa a következő példa tooconfigure Azure nyilvános társviszony-létesítés a kör hello:</span><span class="sxs-lookup"><span data-stu-id="961eb-191">Run hello following example tooconfigure Azure public peering for your circuit:</span></span>

  ```azurecli
  az network express-route peering create --circuit-name MyCircuit --peer-asn 100 --primary-peer-subnet 12.0.0.0/30 -g ExpressRouteResourceGroup --secondary-peer-subnet 12.0.0.4/30 --vlan-id 200 --peering-type AzurePublicPeering
  ```

  <span data-ttu-id="961eb-192">Ha úgy dönt, toouse az MD5 kivonatoló, használja a következő példa hello:</span><span class="sxs-lookup"><span data-stu-id="961eb-192">If you choose toouse an MD5 hash, use hello following example:</span></span>

  ```azurecli
  az network express-route peering create --circuit-name MyCircuit --peer-asn 100 --primary-peer-subnet 12.0.0.0/30 -g ExpressRouteResourceGroup --secondary-peer-subnet 12.0.0.4/30 --vlan-id 200 --peering-type AzurePublicPeering --SharedKey "A1B2C3D4"
  ```

  > [!IMPORTANT]
  > <span data-ttu-id="961eb-193">Az AS-számot mindenképp társviszony-létesítési ASN-ként, és ne ügyfél ASN-ként adja meg.</span><span class="sxs-lookup"><span data-stu-id="961eb-193">Ensure that you specify your AS number as peering ASN, not customer ASN.</span></span>

### <a name="tooview-azure-public-peering-details"></a><span data-ttu-id="961eb-194">tooview Azure nyilvános társviszony-létesítés részletei</span><span class="sxs-lookup"><span data-stu-id="961eb-194">tooview Azure public peering details</span></span>

<span data-ttu-id="961eb-195">Kaphat a konfigurációs adatait a következő példa hello használata:</span><span class="sxs-lookup"><span data-stu-id="961eb-195">You can get configuration details using hello following example:</span></span>

```azurecli
az network express-route peering show -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePublicPeering
```

<span data-ttu-id="961eb-196">hello hasonló toohello a következő példa a kimenetre:</span><span class="sxs-lookup"><span data-stu-id="961eb-196">hello output is similar toohello following example:</span></span>

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

### <a name="tooupdate-azure-public-peering-configuration"></a><span data-ttu-id="961eb-197">az Azure nyilvános társviszony-létesítési konfiguráció tooupdate</span><span class="sxs-lookup"><span data-stu-id="961eb-197">tooupdate Azure public peering configuration</span></span>

<span data-ttu-id="961eb-198">Frissítheti, használja a következő példa hello hello konfiguráció bármely részeként.</span><span class="sxs-lookup"><span data-stu-id="961eb-198">You can update any part of hello configuration using hello following example.</span></span> <span data-ttu-id="961eb-199">Ebben a példában a 200 too600 hello VLAN-Azonosítót a kapcsolatcsoport hello frissül.</span><span class="sxs-lookup"><span data-stu-id="961eb-199">In this example, hello VLAN ID of hello circuit is being updated from 200 too600.</span></span>

```azurecli
az network express-route peering update --vlan-id 600 -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePublicPeering
```

### <a name="toodelete-azure-public-peering"></a><span data-ttu-id="961eb-200">az Azure nyilvános társviszony toodelete</span><span class="sxs-lookup"><span data-stu-id="961eb-200">toodelete Azure public peering</span></span>

<span data-ttu-id="961eb-201">A társviszony-létesítési konfiguráció a következő példa hello futtatásával távolíthatja el:</span><span class="sxs-lookup"><span data-stu-id="961eb-201">You can remove your peering configuration by running hello following example:</span></span>

```azurecli
az network express-route peering delete -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePublicPeering
```

## <a name="microsoft-peering"></a><span data-ttu-id="961eb-202">Microsoft társviszony-létesítés</span><span class="sxs-lookup"><span data-stu-id="961eb-202">Microsoft peering</span></span>

<span data-ttu-id="961eb-203">Ez a szakasz segítséget nyújt a létrehozása, beolvasása, frissítése és törlése hello Microsoft társviszony-létesítési konfiguráció ExpressRoute-kör.</span><span class="sxs-lookup"><span data-stu-id="961eb-203">This section helps you create, get, update, and delete hello Microsoft peering configuration for an ExpressRoute circuit.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="961eb-204">Előzetes tooAugust 1 Microsoft társviszony-létesítést az ExpressRoute-Kapcsolatcsoportok volt beállítva, akkor 2017 fog rendelkezni az összes szolgáltatás előtagok keresztül hello Microsoft társviszony-létesítést, meghirdetett, akkor is, ha nincsenek megadva útvonal szűrők.</span><span class="sxs-lookup"><span data-stu-id="961eb-204">Microsoft peering of ExpressRoute circuits that were configured prior tooAugust 1, 2017 will have all service prefixes advertised through hello Microsoft peering, even if route filters are not defined.</span></span> <span data-ttu-id="961eb-205">A Microsoft társviszony-létesítést az ExpressRoute-Kapcsolatcsoportok vannak konfigurálva, vagy azt követően 2017. augusztus 1. nem rendelkezik a előtagokat meghirdetett, amíg egy útvonal-szűrő nem csatlakoztatja toohello körön.</span><span class="sxs-lookup"><span data-stu-id="961eb-205">Microsoft peering of ExpressRoute circuits that are configured on or after August 1, 2017 will not have any prefixes advertised until a route filter is attached toohello circuit.</span></span> <span data-ttu-id="961eb-206">További információkért lásd: [konfigurálása a Microsoft társviszony-létesítéshez útvonal szűrő](how-to-routefilter-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="961eb-206">For more information, see [Configure a route filter for Microsoft peering](how-to-routefilter-powershell.md).</span></span>
> 
> 

### <a name="toocreate-microsoft-peering"></a><span data-ttu-id="961eb-207">toocreate Microsoft társviszony-létesítés</span><span class="sxs-lookup"><span data-stu-id="961eb-207">toocreate Microsoft peering</span></span>

1. <span data-ttu-id="961eb-208">Hello Azure parancssori felület legújabb verziójának telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="961eb-208">Install hello latest version of Azure CLI.</span></span> <span data-ttu-id="961eb-209">Használjon hello legújabb verziójára hello Azure parancssori felület (CLI). * felülvizsgálati hello [Előfeltételek](expressroute-prerequisites.md) és [munkafolyamatok](expressroute-workflows.md) konfigurálás elkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="961eb-209">Use hello latest version of hello Azure Command-line Interface (CLI).* Review hello [prerequisites](expressroute-prerequisites.md) and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>

  ```azurecli
  az login
  ```

  <span data-ttu-id="961eb-210">Válassza ki a kívánt toocreate ExpressRoute-kapcsolatcsoportot hello előfizetést.</span><span class="sxs-lookup"><span data-stu-id="961eb-210">Select hello subscription for which you want toocreate ExpressRoute circuit.</span></span>

  ```azurecli
  az account set --subscription "<subscription ID>"
  ```
2. <span data-ttu-id="961eb-211">Hozzon létre egy ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="961eb-211">Create an ExpressRoute circuit.</span></span> <span data-ttu-id="961eb-212">Kövesse az utasításokat toocreate hello egy [ExpressRoute-kapcsolatcsoportot](howto-circuit-cli.md) , illetve hozta-e hello kapcsolat szolgáltatóját.</span><span class="sxs-lookup"><span data-stu-id="961eb-212">Follow hello instructions toocreate an [ExpressRoute circuit](howto-circuit-cli.md) and have it provisioned by hello connectivity provider.</span></span>

  <span data-ttu-id="961eb-213">Ha a kapcsolat szolgáltatójánál 3. rétegbeli felügyelt szolgáltatásokat kínál, a kapcsolat szolgáltató tooenable magánhálózati társviszony-létesítést az Ön Azure kérhet.</span><span class="sxs-lookup"><span data-stu-id="961eb-213">If your connectivity provider offers managed Layer 3 services, you can request your connectivity provider tooenable Azure private peering for you.</span></span> <span data-ttu-id="961eb-214">Ebben az esetben nincs szükség hello következő szakaszokban szereplő toofollow utasításokat.</span><span class="sxs-lookup"><span data-stu-id="961eb-214">In that case, you won't need toofollow instructions listed in hello next sections.</span></span> <span data-ttu-id="961eb-215">Azonban ha a kapcsolat szolgáltatójánál nem kezeli az útválasztást, a kapcsolat létrehozása után továbbra is hello lépések a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="961eb-215">However, if your connectivity provider does not manage routing for you, after creating your circuit, continue your configuration using hello next steps.</span></span>

3. <span data-ttu-id="961eb-216">Ellenőrizze a hello ExpressRoute körön toomake arról, hogy van üzembe helyezve és is engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="961eb-216">Check hello ExpressRoute circuit toomake sure it is provisioned and also enabled.</span></span> <span data-ttu-id="961eb-217">A következő példa hello használata:</span><span class="sxs-lookup"><span data-stu-id="961eb-217">Use hello following example:</span></span>

  ```azurecli
  az network express-route list
  ```

  <span data-ttu-id="961eb-218">a rendszer a következő példa hasonló toohello hello választ:</span><span class="sxs-lookup"><span data-stu-id="961eb-218">hello response is similar toohello following example:</span></span>

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

4. <span data-ttu-id="961eb-219">Konfigurálja a Microsoft hello-kör társviszony-létesítés.</span><span class="sxs-lookup"><span data-stu-id="961eb-219">Configure Microsoft peering for hello circuit.</span></span> <span data-ttu-id="961eb-220">Győződjön meg arról, hogy rendelkezik-e folytatni a következő információk előtt hello.</span><span class="sxs-lookup"><span data-stu-id="961eb-220">Make sure that you have hello following information before you proceed.</span></span>

  * <span data-ttu-id="961eb-221">/ 30-as alhálózat hello elsődleges kapcsolathoz.</span><span class="sxs-lookup"><span data-stu-id="961eb-221">A /30 subnet for hello primary link.</span></span> <span data-ttu-id="961eb-222">Ennek egy érvényes nyilvános IPv4-előtagnak kell lennie, amely az Ön tulajdonában van, és regisztrálva van egy RIR-/IRR-jegyzékben.</span><span class="sxs-lookup"><span data-stu-id="961eb-222">This must be a valid public IPv4 prefix owned by you and registered in an RIR / IRR.</span></span>
  * <span data-ttu-id="961eb-223">/ 30-as alhálózat hello másodlagos kapcsolathoz.</span><span class="sxs-lookup"><span data-stu-id="961eb-223">A /30 subnet for hello secondary link.</span></span> <span data-ttu-id="961eb-224">Ennek egy érvényes nyilvános IPv4-előtagnak kell lennie, amely az Ön tulajdonában van, és regisztrálva van egy RIR-/IRR-jegyzékben.</span><span class="sxs-lookup"><span data-stu-id="961eb-224">This must be a valid public IPv4 prefix owned by you and registered in an RIR / IRR.</span></span>
  * <span data-ttu-id="961eb-225">Egy érvényes VLAN-azonosító tooestablish a társviszony.</span><span class="sxs-lookup"><span data-stu-id="961eb-225">A valid VLAN ID tooestablish this peering on.</span></span> <span data-ttu-id="961eb-226">Győződjön meg arról, hogy egyetlen másik társviszonya se a hello áramkör használ hello azonos VLAN-azonosítót.</span><span class="sxs-lookup"><span data-stu-id="961eb-226">Ensure that no other peering in hello circuit uses hello same VLAN ID.</span></span>
  * <span data-ttu-id="961eb-227">Egy AS-szám a társviszony-létesítéshez.</span><span class="sxs-lookup"><span data-stu-id="961eb-227">AS number for peering.</span></span> <span data-ttu-id="961eb-228">2 és 4 bájtos AS-számokat is használhat.</span><span class="sxs-lookup"><span data-stu-id="961eb-228">You can use both 2-byte and 4-byte AS numbers.</span></span>
  * <span data-ttu-id="961eb-229">Meghirdetett előtagok: meg kell adnia a lista az összes előtagok hello BGP munkameneten keresztül tervezi tooadvertise.</span><span class="sxs-lookup"><span data-stu-id="961eb-229">Advertised prefixes: You must provide a list of all prefixes you plan tooadvertise over hello BGP session.</span></span> <span data-ttu-id="961eb-230">A rendszer kizárólag a nyilvános IP-cím-előtagokat fogadja el.</span><span class="sxs-lookup"><span data-stu-id="961eb-230">Only public IP address prefixes are accepted.</span></span> <span data-ttu-id="961eb-231">Ha azt tervezi, toosend előtagok készlete, elküldheti a vesszővel tagolt listáját.</span><span class="sxs-lookup"><span data-stu-id="961eb-231">If you plan toosend a set of prefixes, you can send a comma-separated list.</span></span> <span data-ttu-id="961eb-232">Ezeket az előtagokat kell lenniük egy RIR-ben regisztrált tooyou vagy IRR-ben.</span><span class="sxs-lookup"><span data-stu-id="961eb-232">These prefixes must be registered tooyou in an RIR / IRR.</span></span>
  * <span data-ttu-id="961eb-233">**Választható -** ügyfél ASN: Ha hirdetési-előtagok nem regisztrált toohello társviszony-létesítés SZÁMOT, megadhatja hello számú toowhich regisztrálva vannak.</span><span class="sxs-lookup"><span data-stu-id="961eb-233">**Optional -** Customer ASN: If you are advertising prefixes that are not registered toohello peering AS number, you can specify hello AS number toowhich they are registered.</span></span>
  * <span data-ttu-id="961eb-234">Útválasztási beállításjegyzék-név: Hello RIR megadhat / BMR mely hello ellen, számát, és előtagok regisztrálva van.</span><span class="sxs-lookup"><span data-stu-id="961eb-234">Routing Registry Name: You can specify hello RIR / IRR against which hello AS number and prefixes are registered.</span></span>
  * <span data-ttu-id="961eb-235">**Választható -** az MD5 kivonatoló, ha úgy dönt, hogy egy toouse.</span><span class="sxs-lookup"><span data-stu-id="961eb-235">**Optional -** An MD5 hash if you choose toouse one.</span></span>

   <span data-ttu-id="961eb-236">Futtassa a következő példa tooconfigure Microsoft társviszony-létesítés a kör hello:</span><span class="sxs-lookup"><span data-stu-id="961eb-236">Run hello following example tooconfigure Microsoft peering for your circuit:</span></span>

  ```azurecli
  az network express-route peering create --circuit-name MyCircuit --peer-asn 100 --primary-peer-subnet 123.0.0.0/30 -g ExpressRouteResourceGroup --secondary-peer-subnet 123.0.0.4/30 --vlan-id 300 --peering-type MicrosoftPeering --advertised-public-prefixes 123.1.0.0/24
  ```

### <a name="tooget-microsoft-peering-details"></a><span data-ttu-id="961eb-237">tooget Microsoft társviszony-létesítési részletei</span><span class="sxs-lookup"><span data-stu-id="961eb-237">tooget Microsoft peering details</span></span>

<span data-ttu-id="961eb-238">Konfigurációs részletek kaphat a következő példa hello használata:</span><span class="sxs-lookup"><span data-stu-id="961eb-238">You can get configuration details by using hello following example:</span></span>

```azurecli
az network express-route peering show -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzureMicrosoftPeering
```

<span data-ttu-id="961eb-239">hello hasonló toohello a következő példa a kimenetre:</span><span class="sxs-lookup"><span data-stu-id="961eb-239">hello output is similar toohello following example:</span></span>

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

### <a name="tooupdate-microsoft-peering-configuration"></a><span data-ttu-id="961eb-240">a Microsoft társviszony-létesítési konfiguráció tooupdate</span><span class="sxs-lookup"><span data-stu-id="961eb-240">tooupdate Microsoft peering configuration</span></span>

<span data-ttu-id="961eb-241">Frissítheti, hello konfiguráció bármely részeként.</span><span class="sxs-lookup"><span data-stu-id="961eb-241">You can update any part of hello configuration.</span></span> <span data-ttu-id="961eb-242">hello meghirdetett hello áramkör előtagok programból 123.1.0.0/24 too124.1.0.0/24 hello a következő példa a frissített:</span><span class="sxs-lookup"><span data-stu-id="961eb-242">hello advertised prefixes of hello circuit are being updated from 123.1.0.0/24 too124.1.0.0/24 in hello following example:</span></span>

```azurecli
az network express-route peering update --circuit-name MyCircuit -g ExpressRouteResourceGroup --peering-type MicrosoftPeering --advertised-public-prefixes 124.1.0.0/24
```

### <a name="toodelete-microsoft-peering"></a><span data-ttu-id="961eb-243">toodelete Microsoft társviszony-létesítés</span><span class="sxs-lookup"><span data-stu-id="961eb-243">toodelete Microsoft peering</span></span>

<span data-ttu-id="961eb-244">A társviszony-létesítési konfiguráció a következő példa hello futtatásával távolíthatja el:</span><span class="sxs-lookup"><span data-stu-id="961eb-244">You can remove your peering configuration by running hello following example:</span></span>

```azurecli
az network express-route peering delete -g ExpressRouteResourceGroup --circuit-name MyCircuit --name MicrosoftPeering
```

## <a name="next-steps"></a><span data-ttu-id="961eb-245">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="961eb-245">Next steps</span></span>

<span data-ttu-id="961eb-246">Következő lépés, [csatolni a virtuális hálózat tooan ExpressRoute-kapcsolatcsoportot](howto-linkvnet-cli.md).</span><span class="sxs-lookup"><span data-stu-id="961eb-246">Next step, [Link a VNet tooan ExpressRoute circuit](howto-linkvnet-cli.md).</span></span>

* <span data-ttu-id="961eb-247">Az ExpressRoute-munkafolyamatokkal kapcsolatos további információkért lásd: [ExpressRoute workflows](expressroute-workflows.md) (ExpressRoute-munkafolyamatok).</span><span class="sxs-lookup"><span data-stu-id="961eb-247">For more information about ExpressRoute workflows, see [ExpressRoute workflows](expressroute-workflows.md).</span></span>
* <span data-ttu-id="961eb-248">A kapcsolatcsoportok társviszony-létesítéseivel kapcsolatos további információkért lásd: [ExpressRoute circuits and routing domains](expressroute-circuit-peerings.md) (ExpressRoute-kapcsolatcsoportok és útválasztási tartományok).</span><span class="sxs-lookup"><span data-stu-id="961eb-248">For more information about circuit peering, see [ExpressRoute circuits and routing domains](expressroute-circuit-peerings.md).</span></span>
* <span data-ttu-id="961eb-249">További információkért a virtuális hálózatok használatáról lásd: [Virtual network overview](../virtual-network/virtual-networks-overview.md) (Virtuális hálózatok áttekintése).</span><span class="sxs-lookup"><span data-stu-id="961eb-249">For more information about working with virtual networks, see [Virtual network overview](../virtual-network/virtual-networks-overview.md).</span></span>