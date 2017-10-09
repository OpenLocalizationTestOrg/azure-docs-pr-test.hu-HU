---
title: "Létrehozása és módosítása az Azure ExpressRoute-kapcsolatcsoportot: CLI |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan toocreate, kiépítéséhez, győződjön meg arról, frissítése, törlése és kiosztásának megszüntetése parancssori felület használatával ExpressRoute-kapcsolatcsoportot."
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
ms.date: 07/25/2017
ms.author: anzaman;cherylmc
ms.openlocfilehash: 396e325658a59afadb209bb525cbb59ac775ae6b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-modify-an-expressroute-circuit-using-cli"></a><span data-ttu-id="c32f2-103">Létrehozásához és módosításához az ExpressRoute-kapcsolatcsoportot parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="c32f2-103">Create and modify an ExpressRoute circuit using CLI</span></span>


<span data-ttu-id="c32f2-104">Ez a cikk ismerteti, hogyan toocreate használatával Azure ExpressRoute-kapcsolatcsoportot hello parancssori felület (CLI).</span><span class="sxs-lookup"><span data-stu-id="c32f2-104">This article describes how toocreate an Azure ExpressRoute circuit by using hello Command Line Interface (CLI).</span></span> <span data-ttu-id="c32f2-105">Ez a cikk is bemutatja, hogyan toocheck hello állapotát, frissítéséhez vagy törlése és expressroute-kapcsolatcsoporthoz kiosztásának megszüntetése.</span><span class="sxs-lookup"><span data-stu-id="c32f2-105">This article also shows you how toocheck hello status, update, or delete and deprovision a circuit.</span></span> <span data-ttu-id="c32f2-106">Ha azt szeretné, hogy egy másik módszer toowork az ExpressRoute-Kapcsolatcsoportok toouse, hello cikk választhat a következő lista hello:</span><span class="sxs-lookup"><span data-stu-id="c32f2-106">If you want toouse a different method toowork with ExpressRoute circuits, you can select hello article from hello following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="c32f2-107">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="c32f2-107">Azure portal</span></span>](expressroute-howto-circuit-portal-resource-manager.md)
> * [<span data-ttu-id="c32f2-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="c32f2-108">PowerShell</span></span>](expressroute-howto-circuit-arm.md)
> * [<span data-ttu-id="c32f2-109">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="c32f2-109">Azure CLI</span></span>](howto-circuit-cli.md)
> * [<span data-ttu-id="c32f2-110">Video - Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="c32f2-110">Video - Azure portal</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [<span data-ttu-id="c32f2-111">PowerShell (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="c32f2-111">PowerShell (classic)</span></span>](expressroute-howto-circuit-classic.md)
> 

## <a name="before-you-begin"></a><span data-ttu-id="c32f2-112">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="c32f2-112">Before you begin</span></span>

* <span data-ttu-id="c32f2-113">Megkezdése előtt telepítse a legújabb verziójának hello hello parancssori felület parancsait (2.0-s vagy újabb).</span><span class="sxs-lookup"><span data-stu-id="c32f2-113">Before beginning, install hello latest version of hello CLI commands (2.0 or later).</span></span> <span data-ttu-id="c32f2-114">Hello parancssori felület parancsait telepítésével kapcsolatos információkért lásd: [Azure CLI 2.0 telepítése](/cli/azure/install-azure-cli) és [Ismerkedés az Azure CLI 2.0](/cli/azure/get-started-with-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="c32f2-114">For information about installing hello CLI commands, see [Install Azure CLI 2.0](/cli/azure/install-azure-cli) and [Get Started with Azure CLI 2.0](/cli/azure/get-started-with-azure-cli).</span></span>
* <span data-ttu-id="c32f2-115">Felülvizsgálati hello [Előfeltételek](expressroute-prerequisites.md) és [munkafolyamatok](expressroute-workflows.md) konfigurálás elkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="c32f2-115">Review hello [prerequisites](expressroute-prerequisites.md) and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>

## <a name="create-and-provision-an-expressroute-circuit"></a><span data-ttu-id="c32f2-116">Hozzon létre, és helyezze üzembe az ExpressRoute-kapcsolatcsoportot</span><span class="sxs-lookup"><span data-stu-id="c32f2-116">Create and provision an ExpressRoute circuit</span></span>

### <a name="1-sign-in-tooyour-azure-account-and-select-your-subscription"></a><span data-ttu-id="c32f2-117">1. Jelentkezzen be Azure-fiók tooyour, és jelölje ki az előfizetését</span><span class="sxs-lookup"><span data-stu-id="c32f2-117">1. Sign in tooyour Azure account and select your subscription</span></span>

<span data-ttu-id="c32f2-118">toobegin a konfigurációt, jelentkezzen be Azure-fiók tooyour.</span><span class="sxs-lookup"><span data-stu-id="c32f2-118">toobegin your configuration, sign in tooyour Azure account.</span></span> <span data-ttu-id="c32f2-119">A következő példák toohelp csatlakozás hello használata:</span><span class="sxs-lookup"><span data-stu-id="c32f2-119">Use hello following examples toohelp you connect:</span></span>

```azurecli
az login
```

<span data-ttu-id="c32f2-120">Hello előfizetések hello fiók ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="c32f2-120">Check hello subscriptions for hello account.</span></span>

```azurecli
az account list
```

<span data-ttu-id="c32f2-121">Válassza ki a kívánt toocreate ExpressRoute-kapcsolatcsoportot hello előfizetést.</span><span class="sxs-lookup"><span data-stu-id="c32f2-121">Select hello subscription for which you want toocreate an ExpressRoute circuit.</span></span>

```azurecli
az account set --subscription "<subscription ID>"
```

### <a name="2-get-hello-list-of-supported-providers-locations-and-bandwidths"></a><span data-ttu-id="c32f2-122">2. Támogatott szolgáltatók, a helyek és a sávszélesség hello listájának beolvasása</span><span class="sxs-lookup"><span data-stu-id="c32f2-122">2. Get hello list of supported providers, locations, and bandwidths</span></span>

<span data-ttu-id="c32f2-123">ExpressRoute-kapcsolatcsoportot létrehozni, meg kell hello támogatott kapcsolat szolgáltatókat, a helyek és a sávszélesség-beállítások listája.</span><span class="sxs-lookup"><span data-stu-id="c32f2-123">Before you create an ExpressRoute circuit, you need hello list of supported connectivity providers, locations, and bandwidth options.</span></span> <span data-ttu-id="c32f2-124">hello CLI parancs "az hálózati expressroute lista--szolgáltatók" ezt az információt fogja használni a későbbi lépésekben adja vissza:</span><span class="sxs-lookup"><span data-stu-id="c32f2-124">hello CLI command 'az network express-route list-service-providers' returns this information, which you’ll use in later steps:</span></span>

```azurecli
az network express-route list-service-providers
```

<span data-ttu-id="c32f2-125">a rendszer a következő példa hasonló toohello hello választ:</span><span class="sxs-lookup"><span data-stu-id="c32f2-125">hello response is similar toohello following example:</span></span>

```azurecli
[
  {
    "bandwidthsOffered": [
      {
        "offerName": "50Mbps",
        "valueInMbps": 50
      },
      {
        "offerName": "100Mbps",
        "valueInMbps": 100
      },
      {
        "offerName": "200Mbps",
        "valueInMbps": 200
      },
      {
        "offerName": "500Mbps",
        "valueInMbps": 500
      },
      {
        "offerName": "1Gbps",
        "valueInMbps": 1000
      },
      {
        "offerName": "2Gbps",
        "valueInMbps": 2000
      },
      {
        "offerName": "5Gbps",
        "valueInMbps": 5000
      },
      {
        "offerName": "10Gbps",
        "valueInMbps": 10000
      }
    ],
    "id": "/subscriptions//resourceGroups//providers/Microsoft.Network/expressRouteServiceProviders/",
    "location": null,
    "name": "AARNet",
    "peeringLocations": [
      "Melbourne",
      "Sydney"
    ],
    "provisioningState": "Succeeded",
    "resourceGroup": "",
    "tags": null,
    "type": "Microsoft.Network/expressRouteServiceProviders"
  },
```

<span data-ttu-id="c32f2-126">Ellenőrizze a hello válasz toosee, ha a kapcsolat szolgáltatójánál szerepel.</span><span class="sxs-lookup"><span data-stu-id="c32f2-126">Check hello response toosee if your connectivity provider is listed.</span></span> <span data-ttu-id="c32f2-127">Jegyezze fel az adatokat, amelyeket a kapcsolat létrehozásakor kell a következő hello:</span><span class="sxs-lookup"><span data-stu-id="c32f2-127">Make a note of hello following information, which you will need when you create a circuit:</span></span>

* <span data-ttu-id="c32f2-128">Név</span><span class="sxs-lookup"><span data-stu-id="c32f2-128">Name</span></span>
* <span data-ttu-id="c32f2-129">PeeringLocations</span><span class="sxs-lookup"><span data-stu-id="c32f2-129">PeeringLocations</span></span>
* <span data-ttu-id="c32f2-130">BandwidthsOffered</span><span class="sxs-lookup"><span data-stu-id="c32f2-130">BandwidthsOffered</span></span>

<span data-ttu-id="c32f2-131">Most már készen áll a toocreate ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="c32f2-131">You're now ready toocreate an ExpressRoute circuit.</span></span>

### <a name="3-create-an-expressroute-circuit"></a><span data-ttu-id="c32f2-132">3. ExpressRoute-kapcsolatcsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="c32f2-132">3. Create an ExpressRoute circuit</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c32f2-133">Az ExpressRoute-kapcsolatcsoport ki egy szolgáltatási kulcs hello pillanattól lesz számlázva.</span><span class="sxs-lookup"><span data-stu-id="c32f2-133">Your ExpressRoute circuit is billed from hello moment a service key is issued.</span></span> <span data-ttu-id="c32f2-134">Ehhez a művelethez készen tooprovision hello áramkör hello kapcsolat szolgáltató esetén.</span><span class="sxs-lookup"><span data-stu-id="c32f2-134">Perform this operation when hello connectivity provider is ready tooprovision hello circuit.</span></span>
> 
> 

<span data-ttu-id="c32f2-135">Ha még nem rendelkezik egy erőforráscsoport, kell létrehoznia egyet az ExpressRoute-kapcsolatcsoportot létrehozása előtt.</span><span class="sxs-lookup"><span data-stu-id="c32f2-135">If you don't already have a resource group, you must create one before you create your ExpressRoute circuit.</span></span> <span data-ttu-id="c32f2-136">Létrehozhat egy erőforráscsoport hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="c32f2-136">You can create a resource group by running hello following command:</span></span>

```azurecli
az group create -n ExpressRouteResourceGroup -l "West US"
```

<span data-ttu-id="c32f2-137">hello a következő példa bemutatja, hogyan toocreate egy 200 MB/s ExpressRoute áramkör a szilícium Valley Equinix keresztül.</span><span class="sxs-lookup"><span data-stu-id="c32f2-137">hello following example shows how toocreate a 200-Mbps ExpressRoute circuit through Equinix in Silicon Valley.</span></span> <span data-ttu-id="c32f2-138">Különböző szolgáltatók és más beállítások használata, amikor a kérést helyettesítse be ezt az információt.</span><span class="sxs-lookup"><span data-stu-id="c32f2-138">If you're using a different provider and different settings, substitute that information when you make your request.</span></span> 

<span data-ttu-id="c32f2-139">Győződjön meg arról, hogy megadja hello megfelelő SKU réteg és SKU-család:</span><span class="sxs-lookup"><span data-stu-id="c32f2-139">Make sure that you specify hello correct SKU tier and SKU family:</span></span>

* <span data-ttu-id="c32f2-140">Termékváltozat szint határozza meg, egy ExpressRoute standard vagy ExpressRoute prémium bővítmény engedélyezve van-e.</span><span class="sxs-lookup"><span data-stu-id="c32f2-140">SKU tier determines whether an ExpressRoute standard or an ExpressRoute premium add-on is enabled.</span></span> <span data-ttu-id="c32f2-141">Megadhatja a "Standard" tooget hello standard Termékváltozat vagy "Prémium" hello prémium bővítménnyel.</span><span class="sxs-lookup"><span data-stu-id="c32f2-141">You can specify 'Standard' tooget hello standard SKU or 'Premium' for hello premium add-on.</span></span>
* <span data-ttu-id="c32f2-142">SKU-család hello számlázási típusa határozza meg.</span><span class="sxs-lookup"><span data-stu-id="c32f2-142">SKU family determines hello billing type.</span></span> <span data-ttu-id="c32f2-143">Megadhat "Metereddata" mért adatforgalmi és a "Unlimiteddata" egy adatforgalmi.</span><span class="sxs-lookup"><span data-stu-id="c32f2-143">You can specify 'Metereddata' for a metered data plan and 'Unlimiteddata' for an unlimited data plan.</span></span> <span data-ttu-id="c32f2-144">Hello számlázási típusának "Metereddata"too'Unlimiteddata", de nem módosítható hello típusa"Unlimiteddata"too'Metereddata" módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="c32f2-144">You can change hello billing type from 'Metereddata' too'Unlimiteddata', but you can't change hello type from 'Unlimiteddata' too'Metereddata'.</span></span>


<span data-ttu-id="c32f2-145">Az ExpressRoute-kapcsolatcsoport ki egy szolgáltatási kulcs hello pillanattól lesz számlázva.</span><span class="sxs-lookup"><span data-stu-id="c32f2-145">Your ExpressRoute circuit is billed from hello moment a service key is issued.</span></span> <span data-ttu-id="c32f2-146">a következő példa hello tulajdonképpen egy kérelem egy új szolgáltatás kulcs:</span><span class="sxs-lookup"><span data-stu-id="c32f2-146">hello following example is a request for a new service key:</span></span>

```azurecli
az network express-route create --bandwidth 200 -n MyCircuit --peering-location "Silicon Valley" -g ExpressRouteResourceGroup --provider "Equinix" -l "West US" --sku-family MeteredData --sku-tier Standard
```

<span data-ttu-id="c32f2-147">hello válasz hello szolgáltatás kulcsot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="c32f2-147">hello response contains hello service key.</span></span>

### <a name="4-list-all-expressroute-circuits"></a><span data-ttu-id="c32f2-148">4. A lista összes ExpressRoute-Kapcsolatcsoportok</span><span class="sxs-lookup"><span data-stu-id="c32f2-148">4. List all ExpressRoute circuits</span></span>

<span data-ttu-id="c32f2-149">minden hello ExpressRoute-Kapcsolatcsoportok létrehozott, listája tooget hello "az hálózati expressroute list" parancs futtatásával.</span><span class="sxs-lookup"><span data-stu-id="c32f2-149">tooget a list of all hello ExpressRoute circuits that you created, run hello 'az network express-route list' command.</span></span> <span data-ttu-id="c32f2-150">Ez a parancs használatával ezt az információt bármikor kérheti le.</span><span class="sxs-lookup"><span data-stu-id="c32f2-150">You can retrieve this information at any time by using this command.</span></span> <span data-ttu-id="c32f2-151">toolist összes körök, paraméter nélküli hívás hello ellenőrizze.</span><span class="sxs-lookup"><span data-stu-id="c32f2-151">toolist all circuits, make hello call with no parameters.</span></span>

```azurecli
az network express-route list
```

<span data-ttu-id="c32f2-152">A szolgáltatás kulcs szerepel hello *ServiceKey* hello válasz mezőjében.</span><span class="sxs-lookup"><span data-stu-id="c32f2-152">Your service key is listed in hello *ServiceKey* field of hello response.</span></span>

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
"serviceProviderProvisioningState": "NotProvisioned",
"sku": {
  "family": "UnlimitedData",
  "name": "Standard_MeteredData",
  "tier": "Standard"
},
"tags": null,
"type": "Microsoft.Network/expressRouteCircuits]
```

<span data-ttu-id="c32f2-153">Részletes leírását, az összes hello paraméterek kaphat futó hello parancs használatával hello "-h" paraméter.</span><span class="sxs-lookup"><span data-stu-id="c32f2-153">You can get detailed descriptions of all hello parameters by running hello command using hello '-h' parameter.</span></span>

```azurecli
az network express-route list -h
```

### <a name="5-send-hello-service-key-tooyour-connectivity-provider-for-provisioning"></a><span data-ttu-id="c32f2-154">5. Kapcsolat kulcs tooyour hello szolgáltató küldése történő üzembe helyezéséhez</span><span class="sxs-lookup"><span data-stu-id="c32f2-154">5. Send hello service key tooyour connectivity provider for provisioning</span></span>

<span data-ttu-id="c32f2-155">"ServiceProviderProvisioningState" hello aktuális állapotának hello szolgáltatói oldalán kiépítési információkat biztosít.</span><span class="sxs-lookup"><span data-stu-id="c32f2-155">'ServiceProviderProvisioningState' provides information about hello current state of provisioning on hello service-provider side.</span></span> <span data-ttu-id="c32f2-156">hello állapot hello állapot hello Microsoft ügyféloldali biztosít.</span><span class="sxs-lookup"><span data-stu-id="c32f2-156">hello status provides hello state on hello Microsoft side.</span></span> <span data-ttu-id="c32f2-157">További információkért lásd: hello [munkafolyamatok cikk](expressroute-workflows.md#expressroute-circuit-provisioning-states).</span><span class="sxs-lookup"><span data-stu-id="c32f2-157">For more information, see hello [Workflows article](expressroute-workflows.md#expressroute-circuit-provisioning-states).</span></span>

<span data-ttu-id="c32f2-158">Amikor létrehoz egy új ExpressRoute-kapcsolatcsoportot, hello kapcsolatcsoport állapotát követő hello van:</span><span class="sxs-lookup"><span data-stu-id="c32f2-158">When you create a new ExpressRoute circuit, hello circuit is in hello following state:</span></span>

```azurecli
"serviceProviderProvisioningState": "NotProvisioned"
"circuitProvisioningState": "Enabled"
```

<span data-ttu-id="c32f2-159">hello áramkör módosítások toohello állapotát követő hello folyamatán, amely lehetővé teszi a hello kapcsolat hitelesítésszolgáltató esetén:</span><span class="sxs-lookup"><span data-stu-id="c32f2-159">hello circuit changes toohello following state when hello connectivity provider is in hello process of enabling it for you:</span></span>

```azurecli
"serviceProviderProvisioningState": "Provisioning"
"circuitProvisioningState": "Enabled"
```

<span data-ttu-id="c32f2-160">Az Ön toobe képes toouse ExpressRoute-kapcsolatcsoportot az állapot a következő hello kell lennie:</span><span class="sxs-lookup"><span data-stu-id="c32f2-160">For you toobe able toouse an ExpressRoute circuit, it must be in hello following state:</span></span>

```azurecli
"serviceProviderProvisioningState": "Provisioned"
"circuitProvisioningState": "Enabled
```

### <a name="6-periodically-check-hello-status-and-hello-state-of-hello-circuit-key"></a><span data-ttu-id="c32f2-161">6. Rendszeres időközönként ellenőrizze a hello állapotát és hello áramkör kulcs hello állapota</span><span class="sxs-lookup"><span data-stu-id="c32f2-161">6. Periodically check hello status and hello state of hello circuit key</span></span>

<span data-ttu-id="c32f2-162">Hello állapotát és hello áramkör kulcs hello állapotának ellenőrzése lehetővé teszi a szolgáltató a kapcsolatcsoport engedélyezésekor.</span><span class="sxs-lookup"><span data-stu-id="c32f2-162">Checking hello status and hello state of hello circuit key lets you know when your provider has enabled your circuit.</span></span> <span data-ttu-id="c32f2-163">Hello áramkör konfigurálása után "ServiceProviderProvisioningState" jelenik meg "Kiépítve", ahogy az alábbi példa hello:</span><span class="sxs-lookup"><span data-stu-id="c32f2-163">After hello circuit has been configured, 'ServiceProviderProvisioningState' appears as 'Provisioned', as shown in hello following example:</span></span>

```azurecli
az network express-route show --resource-group ExpressRouteResourceGroup --name MyCircuit
```

<span data-ttu-id="c32f2-164">a rendszer a következő példa hasonló toohello hello választ:</span><span class="sxs-lookup"><span data-stu-id="c32f2-164">hello response is similar toohello following example:</span></span>

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
"serviceProviderProvisioningState": "NotProvisioned",
"sku": {
  "family": "UnlimitedData",
  "name": "Standard_MeteredData",
  "tier": "Standard"
},
"tags": null,
"type": "Microsoft.Network/expressRouteCircuits]
```

### <a name="7-create-your-routing-configuration"></a><span data-ttu-id="c32f2-165">7. Az útválasztó-konfiguráció létrehozása</span><span class="sxs-lookup"><span data-stu-id="c32f2-165">7. Create your routing configuration</span></span>

<span data-ttu-id="c32f2-166">Részletes útmutatásért lásd: hello [ExpressRoute-áramkör útválasztási konfigurációja](howto-routing-cli.md) toocreate a következő cikket, és módosítsa a kapcsolatcsoport esetében.</span><span class="sxs-lookup"><span data-stu-id="c32f2-166">For step-by-step instructions, see hello [ExpressRoute circuit routing configuration](howto-routing-cli.md) article toocreate and modify circuit peerings.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c32f2-167">Ezek az utasítások csak a szolgáltatók által biztosított réteg 2 internetkapcsolati szolgáltatás használatával létrehozott toocircuits vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="c32f2-167">These instructions only apply toocircuits that are created with service providers that offer layer 2 connectivity services.</span></span> <span data-ttu-id="c32f2-168">A szolgáltató által kezelt használata réteg (általában az IP VPN, például az MPLS) 3 szolgáltatások, a kapcsolat szolgáltatójánál konfigurálja és kezeli az Ön útválasztást.</span><span class="sxs-lookup"><span data-stu-id="c32f2-168">If you're using a service provider that offers managed layer 3 services (typically an IP VPN, like MPLS), your connectivity provider configures and manages routing for you.</span></span>
> 
> 

### <a name="8-link-a-virtual-network-tooan-expressroute-circuit"></a><span data-ttu-id="c32f2-169">8. Hivatkozásra egy virtuális hálózati tooan ExpressRoute-kapcsolatcsoportot</span><span class="sxs-lookup"><span data-stu-id="c32f2-169">8. Link a virtual network tooan ExpressRoute circuit</span></span>

<span data-ttu-id="c32f2-170">A következő hivatkozás egy virtuális hálózati tooyour ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="c32f2-170">Next, link a virtual network tooyour ExpressRoute circuit.</span></span> <span data-ttu-id="c32f2-171">Használjon hello [csatolása a virtuális hálózatok tooExpressRoute Kapcsolatcsoportok](howto-linkvnet-cli.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="c32f2-171">Use hello [Linking virtual networks tooExpressRoute circuits](howto-linkvnet-cli.md) article.</span></span>

## <span data-ttu-id="c32f2-172"><a name="modify"></a>Egy ExpressRoute-kapcsolatcsoportot módosítása</span><span class="sxs-lookup"><span data-stu-id="c32f2-172"><a name="modify"></a>Modifying an ExpressRoute circuit</span></span>

<span data-ttu-id="c32f2-173">ExpressRoute-kapcsolatcsoportot egyes tulajdonságait módosíthatja kapcsolat befolyásolása nélkül.</span><span class="sxs-lookup"><span data-stu-id="c32f2-173">You can modify certain properties of an ExpressRoute circuit without impacting connectivity.</span></span> <span data-ttu-id="c32f2-174">A következő módosításokat állásidő nélkül hello végezheti el:</span><span class="sxs-lookup"><span data-stu-id="c32f2-174">You can make hello following changes with no downtime:</span></span>

* <span data-ttu-id="c32f2-175">Engedélyezheti vagy ExpressRoute prémium bővítményeket tiltsa le az ExpressRoute-kapcsolatcsoport esetében.</span><span class="sxs-lookup"><span data-stu-id="c32f2-175">You can enable or disable an ExpressRoute premium add-on for your ExpressRoute circuit.</span></span>
* <span data-ttu-id="c32f2-176">Feltéve, hogy kapacitás elérhető hello porton hello az ExpressRoute-kapcsolatcsoport sávszélessége növelhető.</span><span class="sxs-lookup"><span data-stu-id="c32f2-176">You can increase hello bandwidth of your ExpressRoute circuit provided there is capacity available on hello port.</span></span> <span data-ttu-id="c32f2-177">Alacsonyabb verziójúra változtatása hello sávszélesség, a kapcsolat azonban nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="c32f2-177">However, downgrading hello bandwidth of a circuit is not supported.</span></span> 
* <span data-ttu-id="c32f2-178">Díjköteles adatforgalom tooUnlimited adatok hello mérési terv módosítható.</span><span class="sxs-lookup"><span data-stu-id="c32f2-178">You can change hello metering plan from Metered Data tooUnlimited Data.</span></span> <span data-ttu-id="c32f2-179">Korlátlan adatforgalom tooMetered adatok nem támogatott a terv mérési hello azonban módosítása.</span><span class="sxs-lookup"><span data-stu-id="c32f2-179">However, changing hello metering plan from Unlimited Data tooMetered Data is not supported.</span></span>
* <span data-ttu-id="c32f2-180">Engedélyezheti és letilthatja *klasszikus műveletek engedélyezése*.</span><span class="sxs-lookup"><span data-stu-id="c32f2-180">You can enable and disable *Allow Classic Operations*.</span></span>

<span data-ttu-id="c32f2-181">További információ a korlátai és korlátozásai: hello [ExpressRoute – gyakori kérdések](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="c32f2-181">For more information on limits and limitations, see hello [ExpressRoute FAQ](expressroute-faqs.md).</span></span>

### <a name="tooenable-hello-expressroute-premium-add-on"></a><span data-ttu-id="c32f2-182">tooenable hello ExpressRoute prémium szintű bővítmény</span><span class="sxs-lookup"><span data-stu-id="c32f2-182">tooenable hello ExpressRoute premium add-on</span></span>

<span data-ttu-id="c32f2-183">A meglévő kör hello ExpressRoute prémium szintű bővítmény engedélyezheti hello a következő parancs használatával:</span><span class="sxs-lookup"><span data-stu-id="c32f2-183">You can enable hello ExpressRoute premium add-on for your existing circuit by using hello following command:</span></span>

```azurecli
az network express-route update -n MyCircuit -g ExpressRouteResourceGroup --sku-tier Premium
```

<span data-ttu-id="c32f2-184">hello áramkör hello ExpressRoute prémium bővítmény engedélyezett szolgáltatások most már rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="c32f2-184">hello circuit now has hello ExpressRoute premium add-on features enabled.</span></span> <span data-ttu-id="c32f2-185">Az első lépések számlázási meg hello prémium bővítmény képességhez, amint hello parancs sikeresen lefutott.</span><span class="sxs-lookup"><span data-stu-id="c32f2-185">We begin billing you for hello premium add-on capability as soon as hello command has successfully run.</span></span>

### <a name="toodisable-hello-expressroute-premium-add-on"></a><span data-ttu-id="c32f2-186">toodisable hello ExpressRoute prémium szintű bővítmény</span><span class="sxs-lookup"><span data-stu-id="c32f2-186">toodisable hello ExpressRoute premium add-on</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c32f2-187">Ez a művelet sikertelen lehet erőforrásokat, amelyek nagyobbak, mint a megengedett hello szabványos kapcsolat használata.</span><span class="sxs-lookup"><span data-stu-id="c32f2-187">This operation can fail if you're using resources that are greater than what is permitted for hello standard circuit.</span></span>
> 
> 

<span data-ttu-id="c32f2-188">Mielőtt hello ExpressRoute prémium bővítmény letiltása, a következő feltételek hello megismerése:</span><span class="sxs-lookup"><span data-stu-id="c32f2-188">Before disabling hello ExpressRoute premium add-on, understand hello following criteria:</span></span>

* <span data-ttu-id="c32f2-189">Ahhoz, hogy megállapításában, a prémium szintű toostandard, győződjön meg arról, hogy rendelkezik-e legfeljebb 10 virtuális hálózatok csatolt toohello körön.</span><span class="sxs-lookup"><span data-stu-id="c32f2-189">Before you downgrade from premium toostandard, you must make sure that you have fewer than 10 virtual networks linked toohello circuit.</span></span> <span data-ttu-id="c32f2-190">Ha több mint 10, a frissítési kérelem sikertelen lesz, és a prémium ütemben számlázás meg.</span><span class="sxs-lookup"><span data-stu-id="c32f2-190">If you have more than 10, your update request fails, and we bill you at premium rates.</span></span>
* <span data-ttu-id="c32f2-191">Minden virtuális hálózat más geopolitikai régiókban kell választható.</span><span class="sxs-lookup"><span data-stu-id="c32f2-191">You must unlink all virtual networks in other geopolitical regions.</span></span> <span data-ttu-id="c32f2-192">Az összes virtuális hálózat nem választható, ha a frissítési kérelem sikertelen lesz, és a prémium ütemben számlázás meg.</span><span class="sxs-lookup"><span data-stu-id="c32f2-192">If you don't unlink all your virtual networks, your update request fails and we bill you at premium rates.</span></span>
* <span data-ttu-id="c32f2-193">Az útvonaltábla a magánhálózati társviszony-létesítés kisebb, mint 4000 útvonalait kell lennie.</span><span class="sxs-lookup"><span data-stu-id="c32f2-193">Your route table must be less than 4,000 routes for private peering.</span></span> <span data-ttu-id="c32f2-194">Ha az útvonal tábla mérete nagyobb, mint 4000 útvonalakat, esik hello BGP-munkamenetet.</span><span class="sxs-lookup"><span data-stu-id="c32f2-194">If your route table size is greater than 4,000 routes, hello BGP session drops.</span></span> <span data-ttu-id="c32f2-195">hello munkamenet nem kell újra, amíg hirdetett előtagok hello száma nem éri el 4000 engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="c32f2-195">hello session won't be reenabled until hello number of advertised prefixes is below 4,000.</span></span>

<span data-ttu-id="c32f2-196">A következő példa hello segítségével letilthatja hello ExpressRoute prémium szintű bővítmény hello meglévő kapcsolat:</span><span class="sxs-lookup"><span data-stu-id="c32f2-196">You can disable hello ExpressRoute premium add-on for hello existing circuit by using hello following example:</span></span>

```azurecli
az network express-route update -n MyCircuit -g ExpressRouteResourceGroup --sku-tier Standard
```

### <a name="tooupdate-hello-expressroute-circuit-bandwidth"></a><span data-ttu-id="c32f2-197">tooupdate hello ExpressRoute-kapcsolatcsoport sávszélessége</span><span class="sxs-lookup"><span data-stu-id="c32f2-197">tooupdate hello ExpressRoute circuit bandwidth</span></span>

<span data-ttu-id="c32f2-198">Hello támogatott sávszélesség-beállítások a szolgáltató, ellenőrizze a hello [ExpressRoute – gyakori kérdések](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="c32f2-198">For hello supported bandwidth options for your provider, check hello [ExpressRoute FAQ](expressroute-faqs.md).</span></span> <span data-ttu-id="c32f2-199">Kiválaszthatja a mérete nagyobb, mint a meglévő expressroute hello méretét.</span><span class="sxs-lookup"><span data-stu-id="c32f2-199">You can pick any size greater than hello size of your existing circuit.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c32f2-200">Ha nincs elég kapacitás hello meglévő porton, előfordulhat, hogy toorecreate hello ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="c32f2-200">If there is inadequate capacity on hello existing port, you may have toorecreate hello ExpressRoute circuit.</span></span> <span data-ttu-id="c32f2-201">Hello kapcsolatcsoport nem frissíthető, ha nincsenek további kapacitást érhető el az adott helyhez.</span><span class="sxs-lookup"><span data-stu-id="c32f2-201">You cannot upgrade hello circuit if there is no additional capacity available at that location.</span></span>
>
> <span data-ttu-id="c32f2-202">Nem csökken a hello sávszélesség az ExpressRoute-kapcsolatcsoportot megszakítása nélkül.</span><span class="sxs-lookup"><span data-stu-id="c32f2-202">You cannot reduce hello bandwidth of an ExpressRoute circuit without disruption.</span></span> <span data-ttu-id="c32f2-203">Alacsonyabb verziójúra változtatása sávszélesség meg toodeprovision hello ExpressRoute-kapcsolatcsoportot igényel, és majd építenie az új ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="c32f2-203">Downgrading bandwidth requires you toodeprovision hello ExpressRoute circuit, and then reprovision a new ExpressRoute circuit.</span></span>
>

<span data-ttu-id="c32f2-204">Miután eldöntötte, hogy van szüksége, használja a következő parancs tooresize hello a kapcsolatcsoport hello mérete:</span><span class="sxs-lookup"><span data-stu-id="c32f2-204">After you decide hello size you need, use hello following command tooresize your circuit:</span></span>

```azurecli
az network express-route update -n MyCircuit -g ExpressRouteResourceGroup --bandwidth 1000
```

<span data-ttu-id="c32f2-205">A kör mérete hello Microsoft oldalán.</span><span class="sxs-lookup"><span data-stu-id="c32f2-205">Your circuit is sized up on hello Microsoft side.</span></span> <span data-ttu-id="c32f2-206">Ezt követően vegye fel a kapcsolatot a kapcsolat szolgáltató tooupdate konfigurációit, az ügyféloldali toomatch ezt a módosítást.</span><span class="sxs-lookup"><span data-stu-id="c32f2-206">Next, you must contact your connectivity provider tooupdate configurations on their side toomatch this change.</span></span> <span data-ttu-id="c32f2-207">Miután elvégezte ezt az értesítést, az első lépések számlázást, a frissített hello sávszélesség beállítást.</span><span class="sxs-lookup"><span data-stu-id="c32f2-207">After you make this notification, we begin billing you for hello updated bandwidth option.</span></span>

### <a name="toomove-hello-sku-from-metered-toounlimited"></a><span data-ttu-id="c32f2-208">toomove hello SKU a mért toounlimited</span><span class="sxs-lookup"><span data-stu-id="c32f2-208">toomove hello SKU from metered toounlimited</span></span>

<span data-ttu-id="c32f2-209">A következő példa hello segítségével módosíthatja egy ExpressRoute-kapcsolatcsoport Termékváltozata hello:</span><span class="sxs-lookup"><span data-stu-id="c32f2-209">You can change hello SKU of an ExpressRoute circuit by using hello following example:</span></span>

```azurecli
az network express-route update -n MyCircuit -g ExpressRouteResourceGroup --sku-family UnlimitedData
```

### <a name="toocontrol-access-toohello-classic-and-resource-manager-environments"></a><span data-ttu-id="c32f2-210">toocontrol hozzáférés toohello klasszikus és Resource Manager-környezetben</span><span class="sxs-lookup"><span data-stu-id="c32f2-210">toocontrol access toohello classic and Resource Manager environments</span></span>

<span data-ttu-id="c32f2-211">Tekintse át a hello utasításait [áthelyezése ExpressRoute-Kapcsolatcsoportok hello klasszikus toohello Resource Manager üzembe helyezési modellben a](expressroute-howto-move-arm.md).</span><span class="sxs-lookup"><span data-stu-id="c32f2-211">Review hello instructions in [Move ExpressRoute circuits from hello classic toohello Resource Manager deployment model](expressroute-howto-move-arm.md).</span></span>

## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a><span data-ttu-id="c32f2-212">Megszüntetés és az ExpressRoute-kapcsolatcsoport törlése</span><span class="sxs-lookup"><span data-stu-id="c32f2-212">Deprovisioning and deleting an ExpressRoute circuit</span></span>

<span data-ttu-id="c32f2-213">toodeprovision és a delete, ExpressRoute-kapcsolatcsoportot tudja, hogy a következő feltételek hello:</span><span class="sxs-lookup"><span data-stu-id="c32f2-213">toodeprovision and delete an ExpressRoute circuit, make sure you understand hello following criteria:</span></span>

* <span data-ttu-id="c32f2-214">Az ExpressRoute-kapcsolatcsoportot hello összes virtuális hálózatot kell választható.</span><span class="sxs-lookup"><span data-stu-id="c32f2-214">You must unlink all virtual networks from hello ExpressRoute circuit.</span></span> <span data-ttu-id="c32f2-215">Ha ez a művelet nem sikerül, ellenőrizze a toosee, ha a virtuális hálózatok kapcsolódó toohello áramkör.</span><span class="sxs-lookup"><span data-stu-id="c32f2-215">If this operation fails, check toosee if any virtual networks are linked toohello circuit.</span></span>
* <span data-ttu-id="c32f2-216">Ha hello ExpressRoute körön szolgáltató üzembe helyezési állapota **kiépítési** vagy **kiépítve**, együttműködve kell a service provider toodeprovision hello kapcsolatcsoport az oldalon.</span><span class="sxs-lookup"><span data-stu-id="c32f2-216">If hello ExpressRoute circuit service provider provisioning state is **Provisioning** or **Provisioned**, you must work with your service provider toodeprovision hello circuit on their side.</span></span> <span data-ttu-id="c32f2-217">A Microsoft továbbra is tooreserve erőforrásokat, és kiszámlázni Önnek, amíg hello szolgáltató megszüntetési hello áramkör befejeződik, és értesítést küld nekünk.</span><span class="sxs-lookup"><span data-stu-id="c32f2-217">We continue tooreserve resources and bill you until hello service provider completes deprovisioning hello circuit and notifies us.</span></span>
* <span data-ttu-id="c32f2-218">Ha hello szolgáltató rendelkezik platformelőfizetés hello kapcsolatcsoport törlése hello áramkör.</span><span class="sxs-lookup"><span data-stu-id="c32f2-218">You can delete hello circuit if hello service provider has deprovisioned hello circuit.</span></span> <span data-ttu-id="c32f2-219">Ha expressroute-kapcsolatcsoporthoz van platformelőfizetés, üzembe helyezési állapota hello szolgáltató túl van-e állítva**nincs kiépítve**.</span><span class="sxs-lookup"><span data-stu-id="c32f2-219">When a circuit is deprovisioned, hello service provider provisioning state is set too**Not provisioned**.</span></span> <span data-ttu-id="c32f2-220">Ezzel leállítja a számlázási hello kör.</span><span class="sxs-lookup"><span data-stu-id="c32f2-220">This stops billing for hello circuit.</span></span>

<span data-ttu-id="c32f2-221">Az ExpressRoute-kapcsolatcsoport törlése hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="c32f2-221">You can delete your ExpressRoute circuit by running hello following command:</span></span>

```azurecli
az network express-route delete  -n MyCircuit -g ExpressRouteResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="c32f2-222">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c32f2-222">Next steps</span></span>

<span data-ttu-id="c32f2-223">Miután létrehozta a kapcsolatcsoport, győződjön meg arról, hogy hello következő feladatokat:</span><span class="sxs-lookup"><span data-stu-id="c32f2-223">After you create your circuit, make sure that you do hello following tasks:</span></span>

* [<span data-ttu-id="c32f2-224">Létrehozásához és módosításához az ExpressRoute-kapcsolatcsoport esetében routing</span><span class="sxs-lookup"><span data-stu-id="c32f2-224">Create and modify routing for your ExpressRoute circuit</span></span>](howto-routing-cli.md)
* [<span data-ttu-id="c32f2-225">A virtuális hálózati tooyour ExpressRoute-kapcsolatcsoportot hivatkozás</span><span class="sxs-lookup"><span data-stu-id="c32f2-225">Link your virtual network tooyour ExpressRoute circuit</span></span>](howto-linkvnet-cli.md)
