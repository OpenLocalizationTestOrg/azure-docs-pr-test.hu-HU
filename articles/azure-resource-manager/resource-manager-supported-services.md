---
title: "Az Azure erőforrás-szolgáltatók és erőforrástípusok |} Microsoft Docs"
description: "Az erőforrás-szolgáltató, amely támogatja az erőforrás-kezelő, hogy a sémáikat és elérhető API-verzió és az erőforrásokat is üzemeltető régiók ismerteti."
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
ms.openlocfilehash: 6a9128f45d4199404019cee594842d59c7f1aaf3
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="resource-providers-and-types"></a><span data-ttu-id="20e58-103">Erőforrás-szolgáltatók és típusát</span><span class="sxs-lookup"><span data-stu-id="20e58-103">Resource providers and types</span></span>

<span data-ttu-id="20e58-104">Erőforrások való telepítésekor, gyakran kell az erőforrás-szolgáltatók és típusok vonatkozó információk lekéréséhez.</span><span class="sxs-lookup"><span data-stu-id="20e58-104">When deploying resources, you frequently need to retrieve information about the resource providers and types.</span></span> <span data-ttu-id="20e58-105">Ebből a cikkből megismerheti, hogy:</span><span class="sxs-lookup"><span data-stu-id="20e58-105">In this article, you learn to:</span></span>

* <span data-ttu-id="20e58-106">Megtekintheti az összes erőforrás-szolgáltató az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="20e58-106">View all resource providers in Azure</span></span>
* <span data-ttu-id="20e58-107">Egy erőforrás-szolgáltató regisztrációs állapotának ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="20e58-107">Check registration status of a resource provider</span></span>
* <span data-ttu-id="20e58-108">Egy erőforrás-szolgáltató regisztrálása</span><span class="sxs-lookup"><span data-stu-id="20e58-108">Register a resource provider</span></span>
* <span data-ttu-id="20e58-109">Egy erőforrás-szolgáltató típusú erőforrások megtekintése</span><span class="sxs-lookup"><span data-stu-id="20e58-109">View resource types for a resource provider</span></span>
* <span data-ttu-id="20e58-110">Egy erőforrástípus érvényes helyek megtekintése</span><span class="sxs-lookup"><span data-stu-id="20e58-110">View valid locations for a resource type</span></span>
* <span data-ttu-id="20e58-111">Az erőforrástípus érvénytelen API-verziók megtekintése</span><span class="sxs-lookup"><span data-stu-id="20e58-111">View valid API versions for a resource type</span></span>

<span data-ttu-id="20e58-112">Ezeket a lépéseket a portálon, PowerShell vagy az Azure parancssori felület használatával végezheti el.</span><span class="sxs-lookup"><span data-stu-id="20e58-112">You can perform these steps through the portal, PowerShell, or Azure CLI.</span></span>

## <a name="powershell"></a><span data-ttu-id="20e58-113">PowerShell</span><span class="sxs-lookup"><span data-stu-id="20e58-113">PowerShell</span></span>

<span data-ttu-id="20e58-114">Azure-ban, és a regisztrációs állapotot az előfizetéshez tartozó összes erőforrás-szolgáltató megjelenítéséhez használja:</span><span class="sxs-lookup"><span data-stu-id="20e58-114">To see all resource providers in Azure, and the registration status for your subscription, use:</span></span>

```powershell
Get-AzureRmResourceProvider -ListAvailable | Select-Object ProviderNamespace, RegistrationState
```

<span data-ttu-id="20e58-115">Amely hasonló eredményeket ad vissza:</span><span class="sxs-lookup"><span data-stu-id="20e58-115">Which returns results similar to:</span></span>

```powershell
ProviderNamespace                RegistrationState
-------------------------------- ------------------
Microsoft.ClassicCompute         Registered
Microsoft.ClassicNetwork         Registered
Microsoft.ClassicStorage         Registered
Microsoft.CognitiveServices      Registered
...
```

<span data-ttu-id="20e58-116">Az erőforrás-szolgáltató dolgozni az előfizetés egy erőforrás-szolgáltató regisztrálása konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="20e58-116">Registering a resource provider configures your subscription to work with the resource provider.</span></span> <span data-ttu-id="20e58-117">A regisztrálási hatóköre mindig az előfizetést.</span><span class="sxs-lookup"><span data-stu-id="20e58-117">The scope for registration is always the subscription.</span></span> <span data-ttu-id="20e58-118">Alapértelmezés szerint sok erőforrás-szolgáltató automatikusan regisztrálva van.</span><span class="sxs-lookup"><span data-stu-id="20e58-118">By default, many resource providers are automatically registered.</span></span> <span data-ttu-id="20e58-119">Szükség lehet, hogy manuálisan kell regisztrálni egy erőforrás-szolgáltató.</span><span class="sxs-lookup"><span data-stu-id="20e58-119">However, you may need to manually register some resource providers.</span></span> <span data-ttu-id="20e58-120">Egy erőforrás-szolgáltató regisztrálása, engedéllyel kell rendelkeznie a `/register/action` műveletet az erőforrás-szolgáltató.</span><span class="sxs-lookup"><span data-stu-id="20e58-120">To register a resource provider, you must have permission to perform the `/register/action` operation for the resource provider.</span></span> <span data-ttu-id="20e58-121">A közreműködői és tulajdonos szerepkör tartalmazza ezt a műveletet.</span><span class="sxs-lookup"><span data-stu-id="20e58-121">This operation is included in the Contributor and Owner roles.</span></span>

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
```

<span data-ttu-id="20e58-122">Amely hasonló eredményeket ad vissza:</span><span class="sxs-lookup"><span data-stu-id="20e58-122">Which returns results similar to:</span></span>

```powershell
ProviderNamespace : Microsoft.Batch
RegistrationState : Registering
ResourceTypes     : {batchAccounts, operations, locations, locations/quotas}
Locations         : {West Europe, East US, East US 2, West US...}
```

<span data-ttu-id="20e58-123">Egy erőforrás-szolgáltató nem törölhető, ha az előfizetés erőforrástípusok adott erőforrás-szolgáltató továbbra is fennáll.</span><span class="sxs-lookup"><span data-stu-id="20e58-123">You cannot unregister a resource provider when you still have resource types from that resource provider in your subscription.</span></span>

<span data-ttu-id="20e58-124">Egy adott erőforrás-szolgáltató adatainak megjelenítéséhez használja:</span><span class="sxs-lookup"><span data-stu-id="20e58-124">To see information for a particular resource provider, use:</span></span>

```powershell
Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
```

<span data-ttu-id="20e58-125">Amely hasonló eredményeket ad vissza:</span><span class="sxs-lookup"><span data-stu-id="20e58-125">Which returns results similar to:</span></span>

```powershell
{ProviderNamespace : Microsoft.Batch
RegistrationState : Registered
ResourceTypes     : {batchAccounts}
Locations         : {West Europe, East US, East US 2, West US...}

...
```

<span data-ttu-id="20e58-126">Erőforrás-szolgáltató az erőforrástípusok megjelenítéséhez használja:</span><span class="sxs-lookup"><span data-stu-id="20e58-126">To see the resource types for a resource provider, use:</span></span>

```powershell
(Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes.ResourceTypeName
```

<span data-ttu-id="20e58-127">Amely adja vissza:</span><span class="sxs-lookup"><span data-stu-id="20e58-127">Which returns:</span></span>

```powershell
batchAccounts
operations
locations
locations/quotas
```

<span data-ttu-id="20e58-128">Az API-verzió felel meg a REST API-műveleteket az erőforrás-szolgáltató által kiadott verzióját.</span><span class="sxs-lookup"><span data-stu-id="20e58-128">The API version corresponds to a version of REST API operations that are released by the resource provider.</span></span> <span data-ttu-id="20e58-129">Egy erőforrás-szolgáltató lehetővé teszi, hogy az új funkciók, mivel feloldja a REST API egy új verziója.</span><span class="sxs-lookup"><span data-stu-id="20e58-129">As a resource provider enables new features, it releases a new version of the REST API.</span></span> 

<span data-ttu-id="20e58-130">Az elérhető API-verzió az erőforrástípus használatához:</span><span class="sxs-lookup"><span data-stu-id="20e58-130">To get the available API versions for a resource type, use:</span></span>

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes | Where-Object ResourceTypeName -eq batchAccounts).ApiVersions
```

<span data-ttu-id="20e58-131">Amely adja vissza:</span><span class="sxs-lookup"><span data-stu-id="20e58-131">Which returns:</span></span>

```powershell
2017-05-01
2017-01-01
2015-12-01
2015-09-01
2015-07-01
```

<span data-ttu-id="20e58-132">Erőforrás-kezelő minden régióban támogatott, de előfordulhat, hogy minden régióban nem támogatott az erőforrások telepítése.</span><span class="sxs-lookup"><span data-stu-id="20e58-132">Resource Manager is supported in all regions, but the resources you deploy might not be supported in all regions.</span></span> <span data-ttu-id="20e58-133">Ezenkívül előfordulhat, az előfizetés, amelyek meggátolják, hogy az erőforrás-t támogató egyes régiókban használatával korlátozásait.</span><span class="sxs-lookup"><span data-stu-id="20e58-133">In addition, there may be limitations on your subscription that prevent you from using some regions that support the resource.</span></span> 

<span data-ttu-id="20e58-134">A támogatott helyek az erőforrástípus használatához.</span><span class="sxs-lookup"><span data-stu-id="20e58-134">To get the supported locations for a resource type, use.</span></span>

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes | Where-Object ResourceTypeName -eq batchAccounts).Locations
```

<span data-ttu-id="20e58-135">Amely adja vissza:</span><span class="sxs-lookup"><span data-stu-id="20e58-135">Which returns:</span></span>

```powershell
West Europe
East US
East US 2
West US
...
```

## <a name="azure-cli"></a><span data-ttu-id="20e58-136">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="20e58-136">Azure CLI</span></span>
<span data-ttu-id="20e58-137">Azure-ban, és a regisztrációs állapotot az előfizetéshez tartozó összes erőforrás-szolgáltató megjelenítéséhez használja:</span><span class="sxs-lookup"><span data-stu-id="20e58-137">To see all resource providers in Azure, and the registration status for your subscription, use:</span></span>

```azurecli
az provider list --query "[].{Provider:namespace, Status:registrationState}" --out table
```

<span data-ttu-id="20e58-138">Amely hasonló eredményeket ad vissza:</span><span class="sxs-lookup"><span data-stu-id="20e58-138">Which returns results similar to:</span></span>

```azurecli
Provider                         Status
-------------------------------- ----------------
Microsoft.ClassicCompute         Registered
Microsoft.ClassicNetwork         Registered
Microsoft.ClassicStorage         Registered
Microsoft.CognitiveServices      Registered
...
```

<span data-ttu-id="20e58-139">Az erőforrás-szolgáltató dolgozni az előfizetés egy erőforrás-szolgáltató regisztrálása konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="20e58-139">Registering a resource provider configures your subscription to work with the resource provider.</span></span> <span data-ttu-id="20e58-140">A regisztrálási hatóköre mindig az előfizetést.</span><span class="sxs-lookup"><span data-stu-id="20e58-140">The scope for registration is always the subscription.</span></span> <span data-ttu-id="20e58-141">Alapértelmezés szerint sok erőforrás-szolgáltató automatikusan regisztrálva van.</span><span class="sxs-lookup"><span data-stu-id="20e58-141">By default, many resource providers are automatically registered.</span></span> <span data-ttu-id="20e58-142">Szükség lehet, hogy manuálisan kell regisztrálni egy erőforrás-szolgáltató.</span><span class="sxs-lookup"><span data-stu-id="20e58-142">However, you may need to manually register some resource providers.</span></span> <span data-ttu-id="20e58-143">Egy erőforrás-szolgáltató regisztrálása, engedéllyel kell rendelkeznie a `/register/action` műveletet az erőforrás-szolgáltató.</span><span class="sxs-lookup"><span data-stu-id="20e58-143">To register a resource provider, you must have permission to perform the `/register/action` operation for the resource provider.</span></span> <span data-ttu-id="20e58-144">A közreműködői és tulajdonos szerepkör tartalmazza ezt a műveletet.</span><span class="sxs-lookup"><span data-stu-id="20e58-144">This operation is included in the Contributor and Owner roles.</span></span>

```azurecli
az provider register --namespace Microsoft.Batch
```

<span data-ttu-id="20e58-145">Egy üzenet, hogy a regisztrációs visszaadó folyamatban.</span><span class="sxs-lookup"><span data-stu-id="20e58-145">Which returns a message that registration is on-going.</span></span>

<span data-ttu-id="20e58-146">Egy erőforrás-szolgáltató nem törölhető, ha az előfizetés erőforrástípusok adott erőforrás-szolgáltató továbbra is fennáll.</span><span class="sxs-lookup"><span data-stu-id="20e58-146">You cannot unregister a resource provider when you still have resource types from that resource provider in your subscription.</span></span>

<span data-ttu-id="20e58-147">Egy adott erőforrás-szolgáltató adatainak megjelenítéséhez használja:</span><span class="sxs-lookup"><span data-stu-id="20e58-147">To see information for a particular resource provider, use:</span></span>

```azurecli
az provider show --namespace Microsoft.Batch
```

<span data-ttu-id="20e58-148">Amely hasonló eredményeket ad vissza:</span><span class="sxs-lookup"><span data-stu-id="20e58-148">Which returns results similar to:</span></span>

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

<span data-ttu-id="20e58-149">Erőforrás-szolgáltató az erőforrástípusok megjelenítéséhez használja:</span><span class="sxs-lookup"><span data-stu-id="20e58-149">To see the resource types for a resource provider, use:</span></span>

```azurecli
az provider show --namespace Microsoft.Batch --query "resourceTypes[*].resourceType" --out table
```

<span data-ttu-id="20e58-150">Amely adja vissza:</span><span class="sxs-lookup"><span data-stu-id="20e58-150">Which returns:</span></span>

```azurecli
Result
---------------
batchAccounts
operations
locations
locations/quotas
```

<span data-ttu-id="20e58-151">Az API-verzió felel meg a REST API-műveleteket az erőforrás-szolgáltató által kiadott verzióját.</span><span class="sxs-lookup"><span data-stu-id="20e58-151">The API version corresponds to a version of REST API operations that are released by the resource provider.</span></span> <span data-ttu-id="20e58-152">Egy erőforrás-szolgáltató lehetővé teszi, hogy az új funkciók, mivel feloldja a REST API egy új verziója.</span><span class="sxs-lookup"><span data-stu-id="20e58-152">As a resource provider enables new features, it releases a new version of the REST API.</span></span> 

<span data-ttu-id="20e58-153">Az elérhető API-verzió az erőforrástípus használatához:</span><span class="sxs-lookup"><span data-stu-id="20e58-153">To get the available API versions for a resource type, use:</span></span>

```azurecli
az provider show --namespace Microsoft.Batch --query "resourceTypes[?resourceType=='batchAccounts'].apiVersions | [0]" --out table
```

<span data-ttu-id="20e58-154">Amely adja vissza:</span><span class="sxs-lookup"><span data-stu-id="20e58-154">Which returns:</span></span>

```azurecli
Result
---------------
2017-05-01
2017-01-01
2015-12-01
2015-09-01
2015-07-01
```

<span data-ttu-id="20e58-155">Erőforrás-kezelő minden régióban támogatott, de előfordulhat, hogy minden régióban nem támogatott az erőforrások telepítése.</span><span class="sxs-lookup"><span data-stu-id="20e58-155">Resource Manager is supported in all regions, but the resources you deploy might not be supported in all regions.</span></span> <span data-ttu-id="20e58-156">Ezenkívül előfordulhat, az előfizetés, amelyek meggátolják, hogy az erőforrás-t támogató egyes régiókban használatával korlátozásait.</span><span class="sxs-lookup"><span data-stu-id="20e58-156">In addition, there may be limitations on your subscription that prevent you from using some regions that support the resource.</span></span> 

<span data-ttu-id="20e58-157">A támogatott helyek az erőforrástípus használatához.</span><span class="sxs-lookup"><span data-stu-id="20e58-157">To get the supported locations for a resource type, use.</span></span>

```azurecli
az provider show --namespace Microsoft.Batch --query "resourceTypes[?resourceType=='batchAccounts'].locations | [0]" --out table
```

<span data-ttu-id="20e58-158">Amely adja vissza:</span><span class="sxs-lookup"><span data-stu-id="20e58-158">Which returns:</span></span>

```azurecli
Result
---------------
West Europe
East US
East US 2
West US
...
```

## <a name="portal"></a><span data-ttu-id="20e58-159">Portál</span><span class="sxs-lookup"><span data-stu-id="20e58-159">Portal</span></span>

<span data-ttu-id="20e58-160">Azure-ban, és a regisztrációs állapotot az előfizetéshez tartozó összes erőforrás-szolgáltató megtekintéséhez válasszon **előfizetések**.</span><span class="sxs-lookup"><span data-stu-id="20e58-160">To see all resource providers in Azure, and the registration status for your subscription, select **Subscriptions**.</span></span>

![az előfizetések kiválasztása](./media/resource-manager-supported-services/select-subscriptions.png)

<span data-ttu-id="20e58-162">Válassza ki az előfizetés megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="20e58-162">Choose the subscription to view.</span></span>

![Adja meg az előfizetést](./media/resource-manager-supported-services/subscription.png)

<span data-ttu-id="20e58-164">Válassza ki **erőforrás-szolgáltató** és megtekintheti az elérhető erőforrás-szolgáltatók listáját.</span><span class="sxs-lookup"><span data-stu-id="20e58-164">Select **Resource providers** and view the list of available resource providers.</span></span>

![erőforrás-szolgáltatók megjelenítése](./media/resource-manager-supported-services/show-resource-providers.png)

<span data-ttu-id="20e58-166">Az erőforrás-szolgáltató dolgozni az előfizetés egy erőforrás-szolgáltató regisztrálása konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="20e58-166">Registering a resource provider configures your subscription to work with the resource provider.</span></span> <span data-ttu-id="20e58-167">A regisztrálási hatóköre mindig az előfizetést.</span><span class="sxs-lookup"><span data-stu-id="20e58-167">The scope for registration is always the subscription.</span></span> <span data-ttu-id="20e58-168">Alapértelmezés szerint sok erőforrás-szolgáltató automatikusan regisztrálva van.</span><span class="sxs-lookup"><span data-stu-id="20e58-168">By default, many resource providers are automatically registered.</span></span> <span data-ttu-id="20e58-169">Szükség lehet, hogy manuálisan kell regisztrálni egy erőforrás-szolgáltató.</span><span class="sxs-lookup"><span data-stu-id="20e58-169">However, you may need to manually register some resource providers.</span></span> <span data-ttu-id="20e58-170">Egy erőforrás-szolgáltató regisztrálása, engedéllyel kell rendelkeznie a `/register/action` műveletet az erőforrás-szolgáltató.</span><span class="sxs-lookup"><span data-stu-id="20e58-170">To register a resource provider, you must have permission to perform the `/register/action` operation for the resource provider.</span></span> <span data-ttu-id="20e58-171">A közreműködői és tulajdonos szerepkör tartalmazza ezt a műveletet.</span><span class="sxs-lookup"><span data-stu-id="20e58-171">This operation is included in the Contributor and Owner roles.</span></span> <span data-ttu-id="20e58-172">Válasszon egy erőforrás-szolgáltató regisztrálásához **regisztrálása**.</span><span class="sxs-lookup"><span data-stu-id="20e58-172">To register a resource provider, select **Register**.</span></span>

![erőforrás-szolgáltató regisztrálása](./media/resource-manager-supported-services/register-provider.png)

<span data-ttu-id="20e58-174">Egy erőforrás-szolgáltató nem törölhető, ha az előfizetés erőforrástípusok adott erőforrás-szolgáltató továbbra is fennáll.</span><span class="sxs-lookup"><span data-stu-id="20e58-174">You cannot unregister a resource provider when you still have resource types from that resource provider in your subscription.</span></span>

<span data-ttu-id="20e58-175">Megtekintéséhez válasszon egy adott erőforrás-szolgáltató adatait **további szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="20e58-175">To see information for a particular resource provider, select **More services**.</span></span>

![Jelölje ki a további szolgáltatások](./media/resource-manager-supported-services/more-services.png)

<span data-ttu-id="20e58-177">Keresse meg **erőforrás-kezelő** , és jelölje ki az elérhető lehetőségek közül.</span><span class="sxs-lookup"><span data-stu-id="20e58-177">Search for **Resource Explorer** and select it from the available options.</span></span>

![Válassza ki az erőforrás-kezelő](./media/resource-manager-supported-services/select-resource-explorer.png)

<span data-ttu-id="20e58-179">Válassza ki **szolgáltatók**.</span><span class="sxs-lookup"><span data-stu-id="20e58-179">Select **Providers**.</span></span>

![Szolgáltatók kiválasztása](./media/resource-manager-supported-services/select-providers.png)

<span data-ttu-id="20e58-181">Válassza ki az erőforrás-szolgáltató és a megtekinteni kívánt erőforrástípust.</span><span class="sxs-lookup"><span data-stu-id="20e58-181">Select the resource provider and resource type that you want to view.</span></span>

![Válassza ki az erőforrás típusa](./media/resource-manager-supported-services/select-resource-type.png)

<span data-ttu-id="20e58-183">Erőforrás-kezelő minden régióban támogatott, de előfordulhat, hogy minden régióban nem támogatott az erőforrások telepítése.</span><span class="sxs-lookup"><span data-stu-id="20e58-183">Resource Manager is supported in all regions, but the resources you deploy might not be supported in all regions.</span></span> <span data-ttu-id="20e58-184">Ezenkívül előfordulhat, az előfizetés, amelyek meggátolják, hogy az erőforrás-t támogató egyes régiókban használatával korlátozásait.</span><span class="sxs-lookup"><span data-stu-id="20e58-184">In addition, there may be limitations on your subscription that prevent you from using some regions that support the resource.</span></span> <span data-ttu-id="20e58-185">Az erőforrás-kezelő az erőforrástípushoz érvényes helyek jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="20e58-185">The resource explorer displays valid locations for the resource type.</span></span>

![Hely megjelenítése](./media/resource-manager-supported-services/show-locations.png)

<span data-ttu-id="20e58-187">Az API-verzió felel meg a REST API-műveleteket az erőforrás-szolgáltató által kiadott verzióját.</span><span class="sxs-lookup"><span data-stu-id="20e58-187">The API version corresponds to a version of REST API operations that are released by the resource provider.</span></span> <span data-ttu-id="20e58-188">Egy erőforrás-szolgáltató lehetővé teszi, hogy az új funkciók, mivel feloldja a REST API egy új verziója.</span><span class="sxs-lookup"><span data-stu-id="20e58-188">As a resource provider enables new features, it releases a new version of the REST API.</span></span> <span data-ttu-id="20e58-189">Az erőforrás-kezelő jeleníti meg az erőforrástípus érvénytelen API-verziók.</span><span class="sxs-lookup"><span data-stu-id="20e58-189">The resource explorer displays valid API versions for the resource type.</span></span>

![API-verziók megjelenítése](./media/resource-manager-supported-services/show-api-versions.png)

## <a name="next-steps"></a><span data-ttu-id="20e58-191">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="20e58-191">Next steps</span></span>
* <span data-ttu-id="20e58-192">Resource Manager-sablonok létrehozásával kapcsolatos további tudnivalókért lásd: [Azure Resource Manager-sablonok készítése](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="20e58-192">To learn about creating Resource Manager templates, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="20e58-193">Erőforrások telepítésével kapcsolatos további tudnivalókért lásd: [Azure Resource Manager-sablon az alkalmazás központi telepítését](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="20e58-193">To learn about deploying resources, see [Deploy an application with Azure Resource Manager template](resource-group-template-deploy.md).</span></span>
* <span data-ttu-id="20e58-194">Az erőforrás-szolgáltató műveletek megtekintése: [Azure REST API](/rest/api/).</span><span class="sxs-lookup"><span data-stu-id="20e58-194">To view the operations for a resource provider, see [Azure REST API](/rest/api/).</span></span>

