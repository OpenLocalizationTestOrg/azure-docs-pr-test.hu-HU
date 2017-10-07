---
title: "aaaAzure erőforrás-szolgáltatók és erőforrástípusok |} Microsoft Docs"
description: "Erőforrás-kezelő, a sémák és elérhető API-verzió támogató hello erőforrás-szolgáltatók és hello régiók hello erőforrásokat is üzemeltető ismerteti."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 3c7a6fe4-371a-40da-9ebe-b574f583305b
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: tomfitz
ms.openlocfilehash: 23db1d3808a20166f3b44ec801e1bcc46fbb9bd3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="resource-providers-and-types"></a><span data-ttu-id="70b9e-103">Erőforrás-szolgáltatók és típusát</span><span class="sxs-lookup"><span data-stu-id="70b9e-103">Resource providers and types</span></span>

<span data-ttu-id="70b9e-104">Erőforrások telepítésekor gyakran hello erőforrás-szolgáltatók és típusok tooretrieve információra van szüksége.</span><span class="sxs-lookup"><span data-stu-id="70b9e-104">When deploying resources, you frequently need tooretrieve information about hello resource providers and types.</span></span> <span data-ttu-id="70b9e-105">Ebből a cikkből megismerheti, hogy:</span><span class="sxs-lookup"><span data-stu-id="70b9e-105">In this article, you learn to:</span></span>

* <span data-ttu-id="70b9e-106">Megtekintheti az összes erőforrás-szolgáltató az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="70b9e-106">View all resource providers in Azure</span></span>
* <span data-ttu-id="70b9e-107">Egy erőforrás-szolgáltató regisztrációs állapotának ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="70b9e-107">Check registration status of a resource provider</span></span>
* <span data-ttu-id="70b9e-108">Egy erőforrás-szolgáltató regisztrálása</span><span class="sxs-lookup"><span data-stu-id="70b9e-108">Register a resource provider</span></span>
* <span data-ttu-id="70b9e-109">Egy erőforrás-szolgáltató típusú erőforrások megtekintése</span><span class="sxs-lookup"><span data-stu-id="70b9e-109">View resource types for a resource provider</span></span>
* <span data-ttu-id="70b9e-110">Egy erőforrástípus érvényes helyek megtekintése</span><span class="sxs-lookup"><span data-stu-id="70b9e-110">View valid locations for a resource type</span></span>
* <span data-ttu-id="70b9e-111">Az erőforrástípus érvénytelen API-verziók megtekintése</span><span class="sxs-lookup"><span data-stu-id="70b9e-111">View valid API versions for a resource type</span></span>

<span data-ttu-id="70b9e-112">Ezeket a lépéseket hello portal, a PowerShell vagy az Azure parancssori felület használatával végezheti el.</span><span class="sxs-lookup"><span data-stu-id="70b9e-112">You can perform these steps through hello portal, PowerShell, or Azure CLI.</span></span>

## <a name="powershell"></a><span data-ttu-id="70b9e-113">PowerShell</span><span class="sxs-lookup"><span data-stu-id="70b9e-113">PowerShell</span></span>

<span data-ttu-id="70b9e-114">toosee Azure-ban, és hello regisztrációs állapotát az előfizetéshez tartozó összes erőforrás-szolgáltató használata:</span><span class="sxs-lookup"><span data-stu-id="70b9e-114">toosee all resource providers in Azure, and hello registration status for your subscription, use:</span></span>

```powershell
Get-AzureRmResourceProvider -ListAvailable | Select-Object ProviderNamespace, RegistrationState
```

<span data-ttu-id="70b9e-115">Amely hasonló eredményeket ad vissza:</span><span class="sxs-lookup"><span data-stu-id="70b9e-115">Which returns results similar to:</span></span>

```powershell
ProviderNamespace                RegistrationState
-------------------------------- ------------------
Microsoft.ClassicCompute         Registered
Microsoft.ClassicNetwork         Registered
Microsoft.ClassicStorage         Registered
Microsoft.CognitiveServices      Registered
...
```

<span data-ttu-id="70b9e-116">Egy erőforrás-szolgáltató regisztrálása konfigurálja az előfizetés toowork hello erőforrás-szolgáltató.</span><span class="sxs-lookup"><span data-stu-id="70b9e-116">Registering a resource provider configures your subscription toowork with hello resource provider.</span></span> <span data-ttu-id="70b9e-117">hello regisztrálási hatóköre mindig hello előfizetés.</span><span class="sxs-lookup"><span data-stu-id="70b9e-117">hello scope for registration is always hello subscription.</span></span> <span data-ttu-id="70b9e-118">Alapértelmezés szerint sok erőforrás-szolgáltató automatikusan regisztrálva van.</span><span class="sxs-lookup"><span data-stu-id="70b9e-118">By default, many resource providers are automatically registered.</span></span> <span data-ttu-id="70b9e-119">Azonban szükség lehet toomanually néhány erőforrás-szolgáltató regisztrálása.</span><span class="sxs-lookup"><span data-stu-id="70b9e-119">However, you may need toomanually register some resource providers.</span></span> <span data-ttu-id="70b9e-120">egy erőforrás-szolgáltató tooregister, rendelkeznie kell engedéllyel tooperform hello `/register/action` hello erőforrás-szolgáltató a műveletet.</span><span class="sxs-lookup"><span data-stu-id="70b9e-120">tooregister a resource provider, you must have permission tooperform hello `/register/action` operation for hello resource provider.</span></span> <span data-ttu-id="70b9e-121">Ez a művelet hello közreműködői és tulajdonos szerepkörök tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="70b9e-121">This operation is included in hello Contributor and Owner roles.</span></span>

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
```

<span data-ttu-id="70b9e-122">Amely hasonló eredményeket ad vissza:</span><span class="sxs-lookup"><span data-stu-id="70b9e-122">Which returns results similar to:</span></span>

```powershell
ProviderNamespace : Microsoft.Batch
RegistrationState : Registering
ResourceTypes     : {batchAccounts, operations, locations, locations/quotas}
Locations         : {West Europe, East US, East US 2, West US...}
```

<span data-ttu-id="70b9e-123">Egy erőforrás-szolgáltató nem törölhető, ha az előfizetés erőforrástípusok adott erőforrás-szolgáltató továbbra is fennáll.</span><span class="sxs-lookup"><span data-stu-id="70b9e-123">You cannot unregister a resource provider when you still have resource types from that resource provider in your subscription.</span></span>

<span data-ttu-id="70b9e-124">egy adott erőforrás-szolgáltató, használjon toosee információi:</span><span class="sxs-lookup"><span data-stu-id="70b9e-124">toosee information for a particular resource provider, use:</span></span>

```powershell
Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
```

<span data-ttu-id="70b9e-125">Amely hasonló eredményeket ad vissza:</span><span class="sxs-lookup"><span data-stu-id="70b9e-125">Which returns results similar to:</span></span>

```powershell
{ProviderNamespace : Microsoft.Batch
RegistrationState : Registered
ResourceTypes     : {batchAccounts}
Locations         : {West Europe, East US, East US 2, West US...}

...
```

<span data-ttu-id="70b9e-126">toosee hello erőforrástípusai egy erőforrás-szolgáltató használata:</span><span class="sxs-lookup"><span data-stu-id="70b9e-126">toosee hello resource types for a resource provider, use:</span></span>

```powershell
(Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes.ResourceTypeName
```

<span data-ttu-id="70b9e-127">Amely adja vissza:</span><span class="sxs-lookup"><span data-stu-id="70b9e-127">Which returns:</span></span>

```powershell
batchAccounts
operations
locations
locations/quotas
```

<span data-ttu-id="70b9e-128">hello API-verzió felel meg a közzétett hello erőforrás-szolgáltató REST API-műveleteket tooa verzióját.</span><span class="sxs-lookup"><span data-stu-id="70b9e-128">hello API version corresponds tooa version of REST API operations that are released by hello resource provider.</span></span> <span data-ttu-id="70b9e-129">Egy erőforrás-szolgáltató lehetővé teszi, hogy az új funkciók, mivel felszabadít hello REST API-t egy új verziója.</span><span class="sxs-lookup"><span data-stu-id="70b9e-129">As a resource provider enables new features, it releases a new version of hello REST API.</span></span> 

<span data-ttu-id="70b9e-130">használja a tooget hello elérhető API-verziók egy erőforrás típusa:</span><span class="sxs-lookup"><span data-stu-id="70b9e-130">tooget hello available API versions for a resource type, use:</span></span>

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes | Where-Object ResourceTypeName -eq batchAccounts).ApiVersions
```

<span data-ttu-id="70b9e-131">Amely adja vissza:</span><span class="sxs-lookup"><span data-stu-id="70b9e-131">Which returns:</span></span>

```powershell
2017-05-01
2017-01-01
2015-12-01
2015-09-01
2015-07-01
```

<span data-ttu-id="70b9e-132">Erőforrás-kezelő támogatott minden régióban, de előfordulhat, hogy minden régióban nem támogatott hello erőforrások telepítése.</span><span class="sxs-lookup"><span data-stu-id="70b9e-132">Resource Manager is supported in all regions, but hello resources you deploy might not be supported in all regions.</span></span> <span data-ttu-id="70b9e-133">Ezenkívül előfordulhat, az előfizetés, amelyek meggátolják, hogy egyes régiókban hello erőforrás-t támogató használatával korlátozásait.</span><span class="sxs-lookup"><span data-stu-id="70b9e-133">In addition, there may be limitations on your subscription that prevent you from using some regions that support hello resource.</span></span> 

<span data-ttu-id="70b9e-134">tooget hello támogatott helyek az erőforrás-típust használja.</span><span class="sxs-lookup"><span data-stu-id="70b9e-134">tooget hello supported locations for a resource type, use.</span></span>

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes | Where-Object ResourceTypeName -eq batchAccounts).Locations
```

<span data-ttu-id="70b9e-135">Amely adja vissza:</span><span class="sxs-lookup"><span data-stu-id="70b9e-135">Which returns:</span></span>

```powershell
West Europe
East US
East US 2
West US
...
```

## <a name="azure-cli"></a><span data-ttu-id="70b9e-136">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="70b9e-136">Azure CLI</span></span>
<span data-ttu-id="70b9e-137">toosee Azure-ban, és hello regisztrációs állapotát az előfizetéshez tartozó összes erőforrás-szolgáltató használata:</span><span class="sxs-lookup"><span data-stu-id="70b9e-137">toosee all resource providers in Azure, and hello registration status for your subscription, use:</span></span>

```azurecli
az provider list --query "[].{Provider:namespace, Status:registrationState}" --out table
```

<span data-ttu-id="70b9e-138">Amely hasonló eredményeket ad vissza:</span><span class="sxs-lookup"><span data-stu-id="70b9e-138">Which returns results similar to:</span></span>

```azurecli
Provider                         Status
-------------------------------- ----------------
Microsoft.ClassicCompute         Registered
Microsoft.ClassicNetwork         Registered
Microsoft.ClassicStorage         Registered
Microsoft.CognitiveServices      Registered
...
```

<span data-ttu-id="70b9e-139">Egy erőforrás-szolgáltató regisztrálása konfigurálja az előfizetés toowork hello erőforrás-szolgáltató.</span><span class="sxs-lookup"><span data-stu-id="70b9e-139">Registering a resource provider configures your subscription toowork with hello resource provider.</span></span> <span data-ttu-id="70b9e-140">hello regisztrálási hatóköre mindig hello előfizetés.</span><span class="sxs-lookup"><span data-stu-id="70b9e-140">hello scope for registration is always hello subscription.</span></span> <span data-ttu-id="70b9e-141">Alapértelmezés szerint sok erőforrás-szolgáltató automatikusan regisztrálva van.</span><span class="sxs-lookup"><span data-stu-id="70b9e-141">By default, many resource providers are automatically registered.</span></span> <span data-ttu-id="70b9e-142">Azonban szükség lehet toomanually néhány erőforrás-szolgáltató regisztrálása.</span><span class="sxs-lookup"><span data-stu-id="70b9e-142">However, you may need toomanually register some resource providers.</span></span> <span data-ttu-id="70b9e-143">egy erőforrás-szolgáltató tooregister, rendelkeznie kell engedéllyel tooperform hello `/register/action` hello erőforrás-szolgáltató a műveletet.</span><span class="sxs-lookup"><span data-stu-id="70b9e-143">tooregister a resource provider, you must have permission tooperform hello `/register/action` operation for hello resource provider.</span></span> <span data-ttu-id="70b9e-144">Ez a művelet hello közreműködői és tulajdonos szerepkörök tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="70b9e-144">This operation is included in hello Contributor and Owner roles.</span></span>

```azurecli
az provider register --namespace Microsoft.Batch
```

<span data-ttu-id="70b9e-145">Egy üzenet, hogy a regisztrációs visszaadó folyamatban.</span><span class="sxs-lookup"><span data-stu-id="70b9e-145">Which returns a message that registration is on-going.</span></span>

<span data-ttu-id="70b9e-146">Egy erőforrás-szolgáltató nem törölhető, ha az előfizetés erőforrástípusok adott erőforrás-szolgáltató továbbra is fennáll.</span><span class="sxs-lookup"><span data-stu-id="70b9e-146">You cannot unregister a resource provider when you still have resource types from that resource provider in your subscription.</span></span>

<span data-ttu-id="70b9e-147">egy adott erőforrás-szolgáltató, használjon toosee információi:</span><span class="sxs-lookup"><span data-stu-id="70b9e-147">toosee information for a particular resource provider, use:</span></span>

```azurecli
az provider show --namespace Microsoft.Batch
```

<span data-ttu-id="70b9e-148">Amely hasonló eredményeket ad vissza:</span><span class="sxs-lookup"><span data-stu-id="70b9e-148">Which returns results similar to:</span></span>

```azurecli
{
    "id": "/subscriptions/####-####/providers/Microsoft.Batch",
    "namespace": "Microsoft.Batch",
    "registrationsState": "Registering",
    "resourceTypes:" [
        ...
    ]
}
```

<span data-ttu-id="70b9e-149">toosee hello erőforrástípusai egy erőforrás-szolgáltató használata:</span><span class="sxs-lookup"><span data-stu-id="70b9e-149">toosee hello resource types for a resource provider, use:</span></span>

```azurecli
az provider show --namespace Microsoft.Batch --query "resourceTypes[*].resourceType" --out table
```

<span data-ttu-id="70b9e-150">Amely adja vissza:</span><span class="sxs-lookup"><span data-stu-id="70b9e-150">Which returns:</span></span>

```azurecli
Result
---------------
batchAccounts
operations
locations
locations/quotas
```

<span data-ttu-id="70b9e-151">hello API-verzió felel meg a közzétett hello erőforrás-szolgáltató REST API-műveleteket tooa verzióját.</span><span class="sxs-lookup"><span data-stu-id="70b9e-151">hello API version corresponds tooa version of REST API operations that are released by hello resource provider.</span></span> <span data-ttu-id="70b9e-152">Egy erőforrás-szolgáltató lehetővé teszi, hogy az új funkciók, mivel felszabadít hello REST API-t egy új verziója.</span><span class="sxs-lookup"><span data-stu-id="70b9e-152">As a resource provider enables new features, it releases a new version of hello REST API.</span></span> 

<span data-ttu-id="70b9e-153">használja a tooget hello elérhető API-verziók egy erőforrás típusa:</span><span class="sxs-lookup"><span data-stu-id="70b9e-153">tooget hello available API versions for a resource type, use:</span></span>

```azurecli
az provider show --namespace Microsoft.Batch --query "resourceTypes[?resourceType=='batchAccounts'].apiVersions | [0]" --out table
```

<span data-ttu-id="70b9e-154">Amely adja vissza:</span><span class="sxs-lookup"><span data-stu-id="70b9e-154">Which returns:</span></span>

```azurecli
Result
---------------
2017-05-01
2017-01-01
2015-12-01
2015-09-01
2015-07-01
```

<span data-ttu-id="70b9e-155">Erőforrás-kezelő támogatott minden régióban, de előfordulhat, hogy minden régióban nem támogatott hello erőforrások telepítése.</span><span class="sxs-lookup"><span data-stu-id="70b9e-155">Resource Manager is supported in all regions, but hello resources you deploy might not be supported in all regions.</span></span> <span data-ttu-id="70b9e-156">Ezenkívül előfordulhat, az előfizetés, amelyek meggátolják, hogy egyes régiókban hello erőforrás-t támogató használatával korlátozásait.</span><span class="sxs-lookup"><span data-stu-id="70b9e-156">In addition, there may be limitations on your subscription that prevent you from using some regions that support hello resource.</span></span> 

<span data-ttu-id="70b9e-157">tooget hello támogatott helyek az erőforrás-típust használja.</span><span class="sxs-lookup"><span data-stu-id="70b9e-157">tooget hello supported locations for a resource type, use.</span></span>

```azurecli
az provider show --namespace Microsoft.Batch --query "resourceTypes[?resourceType=='batchAccounts'].locations | [0]" --out table
```

<span data-ttu-id="70b9e-158">Amely adja vissza:</span><span class="sxs-lookup"><span data-stu-id="70b9e-158">Which returns:</span></span>

```azurecli
Result
---------------
West Europe
East US
East US 2
West US
...
```

## <a name="portal"></a><span data-ttu-id="70b9e-159">Portál</span><span class="sxs-lookup"><span data-stu-id="70b9e-159">Portal</span></span>

<span data-ttu-id="70b9e-160">toosee Azure-ban, és hello regisztrációs állapotát az előfizetéshez tartozó összes erőforrás-szolgáltató kiválasztása **előfizetések**.</span><span class="sxs-lookup"><span data-stu-id="70b9e-160">toosee all resource providers in Azure, and hello registration status for your subscription, select **Subscriptions**.</span></span>

![az előfizetések kiválasztása](./media/resource-manager-supported-services/select-subscriptions.png)

<span data-ttu-id="70b9e-162">Válassza ki a hello előfizetés tooview.</span><span class="sxs-lookup"><span data-stu-id="70b9e-162">Choose hello subscription tooview.</span></span>

![Adja meg az előfizetést](./media/resource-manager-supported-services/subscription.png)

<span data-ttu-id="70b9e-164">Válassza ki **erőforrás-szolgáltató** és elérhető erőforrás-szolgáltatók hello listájának megtekintése.</span><span class="sxs-lookup"><span data-stu-id="70b9e-164">Select **Resource providers** and view hello list of available resource providers.</span></span>

![erőforrás-szolgáltatók megjelenítése](./media/resource-manager-supported-services/show-resource-providers.png)

<span data-ttu-id="70b9e-166">Egy erőforrás-szolgáltató regisztrálása konfigurálja az előfizetés toowork hello erőforrás-szolgáltató.</span><span class="sxs-lookup"><span data-stu-id="70b9e-166">Registering a resource provider configures your subscription toowork with hello resource provider.</span></span> <span data-ttu-id="70b9e-167">hello regisztrálási hatóköre mindig hello előfizetés.</span><span class="sxs-lookup"><span data-stu-id="70b9e-167">hello scope for registration is always hello subscription.</span></span> <span data-ttu-id="70b9e-168">Alapértelmezés szerint sok erőforrás-szolgáltató automatikusan regisztrálva van.</span><span class="sxs-lookup"><span data-stu-id="70b9e-168">By default, many resource providers are automatically registered.</span></span> <span data-ttu-id="70b9e-169">Azonban szükség lehet toomanually néhány erőforrás-szolgáltató regisztrálása.</span><span class="sxs-lookup"><span data-stu-id="70b9e-169">However, you may need toomanually register some resource providers.</span></span> <span data-ttu-id="70b9e-170">egy erőforrás-szolgáltató tooregister, rendelkeznie kell engedéllyel tooperform hello `/register/action` hello erőforrás-szolgáltató a műveletet.</span><span class="sxs-lookup"><span data-stu-id="70b9e-170">tooregister a resource provider, you must have permission tooperform hello `/register/action` operation for hello resource provider.</span></span> <span data-ttu-id="70b9e-171">Ez a művelet hello közreműködői és tulajdonos szerepkörök tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="70b9e-171">This operation is included in hello Contributor and Owner roles.</span></span> <span data-ttu-id="70b9e-172">egy erőforrás-szolgáltató tooregister válasszon **regisztrálása**.</span><span class="sxs-lookup"><span data-stu-id="70b9e-172">tooregister a resource provider, select **Register**.</span></span>

![erőforrás-szolgáltató regisztrálása](./media/resource-manager-supported-services/register-provider.png)

<span data-ttu-id="70b9e-174">Egy erőforrás-szolgáltató nem törölhető, ha az előfizetés erőforrástípusok adott erőforrás-szolgáltató továbbra is fennáll.</span><span class="sxs-lookup"><span data-stu-id="70b9e-174">You cannot unregister a resource provider when you still have resource types from that resource provider in your subscription.</span></span>

<span data-ttu-id="70b9e-175">toosee adatai egy adott erőforrás-szolgáltató kiválasztása **további szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="70b9e-175">toosee information for a particular resource provider, select **More services**.</span></span>

![Jelölje ki a további szolgáltatások](./media/resource-manager-supported-services/more-services.png)

<span data-ttu-id="70b9e-177">Keresse meg **erőforrás-kezelő** , és jelölje ki a hello rendelkezésre álló lehetőségek közül.</span><span class="sxs-lookup"><span data-stu-id="70b9e-177">Search for **Resource Explorer** and select it from hello available options.</span></span>

![Válassza ki az erőforrás-kezelő](./media/resource-manager-supported-services/select-resource-explorer.png)

<span data-ttu-id="70b9e-179">Válassza ki **szolgáltatók**.</span><span class="sxs-lookup"><span data-stu-id="70b9e-179">Select **Providers**.</span></span>

![Szolgáltatók kiválasztása](./media/resource-manager-supported-services/select-providers.png)

<span data-ttu-id="70b9e-181">Jelölje be hello erőforrás-szolgáltató és az erőforrás írja be a megjeleníteni kívánt tooview.</span><span class="sxs-lookup"><span data-stu-id="70b9e-181">Select hello resource provider and resource type that you want tooview.</span></span>

![Válassza ki az erőforrás típusa](./media/resource-manager-supported-services/select-resource-type.png)

<span data-ttu-id="70b9e-183">Erőforrás-kezelő támogatott minden régióban, de előfordulhat, hogy minden régióban nem támogatott hello erőforrások telepítése.</span><span class="sxs-lookup"><span data-stu-id="70b9e-183">Resource Manager is supported in all regions, but hello resources you deploy might not be supported in all regions.</span></span> <span data-ttu-id="70b9e-184">Ezenkívül előfordulhat, az előfizetés, amelyek meggátolják, hogy egyes régiókban hello erőforrás-t támogató használatával korlátozásait.</span><span class="sxs-lookup"><span data-stu-id="70b9e-184">In addition, there may be limitations on your subscription that prevent you from using some regions that support hello resource.</span></span> <span data-ttu-id="70b9e-185">hello erőforrás-kezelő hello erőforrástípus érvényes helyét jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="70b9e-185">hello resource explorer displays valid locations for hello resource type.</span></span>

![Hely megjelenítése](./media/resource-manager-supported-services/show-locations.png)

<span data-ttu-id="70b9e-187">hello API-verzió felel meg a közzétett hello erőforrás-szolgáltató REST API-műveleteket tooa verzióját.</span><span class="sxs-lookup"><span data-stu-id="70b9e-187">hello API version corresponds tooa version of REST API operations that are released by hello resource provider.</span></span> <span data-ttu-id="70b9e-188">Egy erőforrás-szolgáltató lehetővé teszi, hogy az új funkciók, mivel felszabadít hello REST API-t egy új verziója.</span><span class="sxs-lookup"><span data-stu-id="70b9e-188">As a resource provider enables new features, it releases a new version of hello REST API.</span></span> <span data-ttu-id="70b9e-189">az erőforrás-kezelő hello hello erőforrástípus érvénytelen API-verziók jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="70b9e-189">hello resource explorer displays valid API versions for hello resource type.</span></span>

![API-verziók megjelenítése](./media/resource-manager-supported-services/show-api-versions.png)

## <a name="next-steps"></a><span data-ttu-id="70b9e-191">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="70b9e-191">Next steps</span></span>
* <span data-ttu-id="70b9e-192">toolearn Resource Manager-sablonok létrehozásával kapcsolatban lásd: [Azure Resource Manager-sablonok készítése](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="70b9e-192">toolearn about creating Resource Manager templates, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="70b9e-193">toolearn erőforrások telepítésével kapcsolatban lásd: [Azure Resource Manager-sablon az alkalmazás központi telepítését](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="70b9e-193">toolearn about deploying resources, see [Deploy an application with Azure Resource Manager template](resource-group-template-deploy.md).</span></span>
* <span data-ttu-id="70b9e-194">tooview hello műveletek erőforrás-szolgáltató, lásd: [Azure REST API](/rest/api/).</span><span class="sxs-lookup"><span data-stu-id="70b9e-194">tooview hello operations for a resource provider, see [Azure REST API](/rest/api/).</span></span>

