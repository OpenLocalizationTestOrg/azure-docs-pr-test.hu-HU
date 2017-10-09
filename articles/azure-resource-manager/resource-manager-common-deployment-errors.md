---
title: "aaaTroubleshoot gyakran az Azure-telepítés hibákat |} Microsoft Docs"
description: "Ismerteti, hogyan tooresolve gyakori hibák Azure Resource Manager használatával erőforrások tooAzure központi telepítésekor."
services: azure-resource-manager
documentationcenter: 
tags: top-support-issue
author: tfitzmac
manager: timlt
editor: tysonn
keywords: "központi telepítési hiba, az azure-telepítés tooazure központi telepítése"
ms.assetid: c002a9be-4de5-4963-bd14-b54aa3d8fa59
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: support-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/17/2017
ms.author: tomfitz
ms.openlocfilehash: 8571e9941879eb5586e4258a785b6a09247da771
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-common-azure-deployment-errors-with-azure-resource-manager"></a><span data-ttu-id="92b73-104">Hibaelhárítás általános az Azure-telepítés az Azure Resource Manager eszközzel</span><span class="sxs-lookup"><span data-stu-id="92b73-104">Troubleshoot common Azure deployment errors with Azure Resource Manager</span></span>
<span data-ttu-id="92b73-105">Ez a témakör ismerteti, hogyan oldhatja meg néhány gyakori merülhetnek fel az Azure-telepítés hibáit.</span><span class="sxs-lookup"><span data-stu-id="92b73-105">This topic describes how you can resolve some common Azure deployment errors you may encounter.</span></span>

<span data-ttu-id="92b73-106">Ez a témakör ismerteti a következő hibakódok hello:</span><span class="sxs-lookup"><span data-stu-id="92b73-106">hello following error codes are described in this topic:</span></span>

* [<span data-ttu-id="92b73-107">AccountNameInvalid</span><span class="sxs-lookup"><span data-stu-id="92b73-107">AccountNameInvalid</span></span>](#accountnameinvalid)
* [<span data-ttu-id="92b73-108">A bejelentkezés nem sikerült</span><span class="sxs-lookup"><span data-stu-id="92b73-108">Authorization failed</span></span>](#authorization-failed)
* [<span data-ttu-id="92b73-109">BadRequest</span><span class="sxs-lookup"><span data-stu-id="92b73-109">BadRequest</span></span>](#badrequest)
* [<span data-ttu-id="92b73-110">Sikertelen</span><span class="sxs-lookup"><span data-stu-id="92b73-110">DeploymentFailed</span></span>](#deploymentfailed)
* [<span data-ttu-id="92b73-111">DisallowedOperation</span><span class="sxs-lookup"><span data-stu-id="92b73-111">DisallowedOperation</span></span>](#disallowedoperation)
* [<span data-ttu-id="92b73-112">InvalidContentLink</span><span class="sxs-lookup"><span data-stu-id="92b73-112">InvalidContentLink</span></span>](#invalidcontentlink)
* [<span data-ttu-id="92b73-113">InvalidTemplate</span><span class="sxs-lookup"><span data-stu-id="92b73-113">InvalidTemplate</span></span>](#invalidtemplate)
* [<span data-ttu-id="92b73-114">MissingSubscriptionRegistration</span><span class="sxs-lookup"><span data-stu-id="92b73-114">MissingSubscriptionRegistration</span></span>](#noregisteredproviderfound)
* [<span data-ttu-id="92b73-115">NotFound</span><span class="sxs-lookup"><span data-stu-id="92b73-115">NotFound</span></span>](#notfound)
* [<span data-ttu-id="92b73-116">NoRegisteredProviderFound</span><span class="sxs-lookup"><span data-stu-id="92b73-116">NoRegisteredProviderFound</span></span>](#noregisteredproviderfound)
* [<span data-ttu-id="92b73-117">OperationNotAllowed</span><span class="sxs-lookup"><span data-stu-id="92b73-117">OperationNotAllowed</span></span>](#quotaexceeded)
* [<span data-ttu-id="92b73-118">ParentResourceNotFound</span><span class="sxs-lookup"><span data-stu-id="92b73-118">ParentResourceNotFound</span></span>](#parentresourcenotfound)
* [<span data-ttu-id="92b73-119">QuotaExceeded</span><span class="sxs-lookup"><span data-stu-id="92b73-119">QuotaExceeded</span></span>](#quotaexceeded)
* [<span data-ttu-id="92b73-120">RequestDisallowedByPolicy</span><span class="sxs-lookup"><span data-stu-id="92b73-120">RequestDisallowedByPolicy</span></span>](#requestdisallowedbypolicy)
* [<span data-ttu-id="92b73-121">ResourceNotFound</span><span class="sxs-lookup"><span data-stu-id="92b73-121">ResourceNotFound</span></span>](#notfound)
* [<span data-ttu-id="92b73-122">SkuNotAvailable</span><span class="sxs-lookup"><span data-stu-id="92b73-122">SkuNotAvailable</span></span>](#skunotavailable)
* [<span data-ttu-id="92b73-123">StorageAccountAlreadyExists</span><span class="sxs-lookup"><span data-stu-id="92b73-123">StorageAccountAlreadyExists</span></span>](#storagenamenotunique)
* [<span data-ttu-id="92b73-124">StorageAccountAlreadyTaken</span><span class="sxs-lookup"><span data-stu-id="92b73-124">StorageAccountAlreadyTaken</span></span>](#storagenamenotunique)

## <a name="deploymentfailed"></a><span data-ttu-id="92b73-125">Sikertelen</span><span class="sxs-lookup"><span data-stu-id="92b73-125">DeploymentFailed</span></span>

<span data-ttu-id="92b73-126">Ez a hibakód azt jelzi, általános központi telepítési hiba, de nincs szüksége toostart hibaelhárítási hello hibakód.</span><span class="sxs-lookup"><span data-stu-id="92b73-126">This error code indicates a general deployment error, but it is not hello error code you need toostart troubleshooting.</span></span> <span data-ttu-id="92b73-127">hello segítő ténylegesen hello elhárításához hibakód általában ez a hiba alatt egy szinttel.</span><span class="sxs-lookup"><span data-stu-id="92b73-127">hello error code that actually helps you resolve hello issue is usually one level below this error.</span></span> <span data-ttu-id="92b73-128">Például hello következő kép bemutatja hello **RequestDisallowedByPolicy** hello központi telepítési hiba alá tartozó hibakód.</span><span class="sxs-lookup"><span data-stu-id="92b73-128">For example, hello following image shows hello **RequestDisallowedByPolicy** error code that is under hello deployment error.</span></span>

![hibakód megjelenítése](./media/resource-manager-common-deployment-errors/error-code.png)

## <a name="skunotavailable"></a><span data-ttu-id="92b73-130">SkuNotAvailable</span><span class="sxs-lookup"><span data-stu-id="92b73-130">SkuNotAvailable</span></span>

<span data-ttu-id="92b73-131">Egy erőforrás (általában egy virtuális gép) telepítésekor a következő kód és a hiba hibaüzenet hello jelenhet meg:</span><span class="sxs-lookup"><span data-stu-id="92b73-131">When deploying a resource (typically a virtual machine), you may receive hello following error code and error message:</span></span>

```
Code: SkuNotAvailable
Message: hello requested tier for resource '<resource>' is currently not available in location '<location>' 
for subscription '<subscriptionID>'. Please try another tier or deploy tooa different location.
```

<span data-ttu-id="92b73-132">Hibaüzenet jelenik a hello erőforrás (például a Virtuálisgép-méretet) kiválasztott Termékváltozat nem érhető el a kiválasztott hello helyéhez.</span><span class="sxs-lookup"><span data-stu-id="92b73-132">You receive this error when hello resource SKU you have selected (such as VM size) is not available for hello location you have selected.</span></span> <span data-ttu-id="92b73-133">tooresolve probléma kell toodetermine termékváltozatok rendelkezésre álló régióban.</span><span class="sxs-lookup"><span data-stu-id="92b73-133">tooresolve this issue, you need toodetermine which SKUs are available in a region.</span></span> <span data-ttu-id="92b73-134">Használhatja a PowerShell, hello portál vagy a REST művelet toofind elérhető termékváltozatok.</span><span class="sxs-lookup"><span data-stu-id="92b73-134">You can use PowerShell, hello portal, or a REST operation toofind available SKUs.</span></span>

- <span data-ttu-id="92b73-135">A PowerShell, használjon [Get-AzureRmComputeResourceSku](/powershell/module/azurerm.compute/get-azurermcomputeresourcesku) és hely szerint szűrő.</span><span class="sxs-lookup"><span data-stu-id="92b73-135">For PowerShell, use [Get-AzureRmComputeResourceSku](/powershell/module/azurerm.compute/get-azurermcomputeresourcesku) and filter by location.</span></span> <span data-ttu-id="92b73-136">Hello legújabb verziójának PowerShell ennél a parancsnál kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="92b73-136">You must have hello latest version of PowerShell for this command.</span></span>

  ```powershell
  Get-AzureRmComputeResourceSku | where {$_.Locations.Contains("southcentralus")}
  ```

  <span data-ttu-id="92b73-137">hello eredmények tartalmazzák a hello hely SKU listáját, valamint, hogy termékváltozat korlátozások.</span><span class="sxs-lookup"><span data-stu-id="92b73-137">hello results include a list of SKUs for hello location and any restrictions for that SKU.</span></span>

  ```powershell
  ResourceType                Name      Locations Restriction                      Capability Value
  ------------                ----      --------- -----------                      ---------- -----
  availabilitySets         Classic southcentralus             MaximumPlatformFaultDomainCount     3
  availabilitySets         Aligned southcentralus             MaximumPlatformFaultDomainCount     3
  virtualMachines      Standard_A0 southcentralus
  virtualMachines      Standard_A1 southcentralus
  virtualMachines      Standard_A2 southcentralus
  ```

- <span data-ttu-id="92b73-138">toouse hello [portal](https://portal.azure.com), toohello portal be, és vegyen fel egy erőforrást hello felületen keresztül.</span><span class="sxs-lookup"><span data-stu-id="92b73-138">toouse hello [portal](https://portal.azure.com), log in toohello portal and add a resource through hello interface.</span></span> <span data-ttu-id="92b73-139">Hello értékeket állíthat be, megjelenik a hello elérhető termékváltozatok az adott erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="92b73-139">As you set hello values, you see hello available SKUs for that resource.</span></span> <span data-ttu-id="92b73-140">Nem kell toocomplete hello központi telepítés.</span><span class="sxs-lookup"><span data-stu-id="92b73-140">You do not need toocomplete hello deployment.</span></span>

    ![elérhető termékváltozatok](./media/resource-manager-common-deployment-errors/view-sku.png)

- <span data-ttu-id="92b73-142">toouse hello REST API-t a virtuális gépek küldése hello kérelem a következő:</span><span class="sxs-lookup"><span data-stu-id="92b73-142">toouse hello REST API for virtual machines, send hello following request:</span></span>

  ```HTTP 
  GET
  https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Compute/skus?api-version=2016-03-30
  ```

  <span data-ttu-id="92b73-143">Elérhető termékváltozatok és régiók visszaküldi hello a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="92b73-143">It returns available SKUs and regions in hello following format:</span></span>

  ```json
  {
    "value": [
      {
        "resourceType": "virtualMachines",
        "name": "Standard_A0",
        "tier": "Standard",
        "size": "A0",
        "locations": [
          "eastus"
        ],
        "restrictions": []
      },
      {
        "resourceType": "virtualMachines",
        "name": "Standard_A1",
        "tier": "Standard",
        "size": "A1",
        "locations": [
          "eastus"
        ],
        "restrictions": []
      },
      ...
    ]
  }    
  ```

<span data-ttu-id="92b73-144">Ha egy adott régióban vagy egy másik régióban, az üzleti igényeinek megfelelő megfelelő Termékváltozat nem toofind, küldje el a [SKU kérelem](https://aka.ms/skurestriction) tooAzure támogatása.</span><span class="sxs-lookup"><span data-stu-id="92b73-144">If you are unable toofind a suitable SKU in that region or an alternative region that meets your business needs, submit a [SKU request](https://aka.ms/skurestriction) tooAzure Support.</span></span>

## <a name="disallowedoperation"></a><span data-ttu-id="92b73-145">DisallowedOperation</span><span class="sxs-lookup"><span data-stu-id="92b73-145">DisallowedOperation</span></span>

```
Code: DisallowedOperation
Message: hello current subscription type is not permitted tooperform operations on any provider 
namespace. Please use a different subscription.
```

<span data-ttu-id="92b73-146">Ez a hibaüzenet jelenik meg, ha az előfizetés, amely nem megengedett tooaccess bármely Azure-szolgáltatások nem Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="92b73-146">If you receive this error, you are using a subscription that is not permitted tooaccess any Azure services other than Azure Active Directory.</span></span> <span data-ttu-id="92b73-147">Lehetséges, hogy az ilyen típusú előfizetés tooaccess hello klasszikus portál hiszen toodeploy erőforrások nem engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="92b73-147">You might have this type of subscription when you need tooaccess hello classic portal but are not permitted toodeploy resources.</span></span> <span data-ttu-id="92b73-148">tooresolve probléma előfizetést kell használnia egy engedély toodeploy erőforrásokat tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="92b73-148">tooresolve this issue, you must use a subscription that has permission toodeploy resources.</span></span>  

<span data-ttu-id="92b73-149">tooview a PowerShell használatával, az elérhető előfizetések használata:</span><span class="sxs-lookup"><span data-stu-id="92b73-149">tooview your available subscriptions with PowerShell, use:</span></span>

```powershell
Get-AzureRmSubscription
```

<span data-ttu-id="92b73-150">És tooset hello előfizetésben, használja:</span><span class="sxs-lookup"><span data-stu-id="92b73-150">And, tooset hello current subscription, use:</span></span>

```powershell
Set-AzureRmContext -SubscriptionName {subscription-name}
```

<span data-ttu-id="92b73-151">tooview Azure CLI 2.0, az elérhető előfizetések használata:</span><span class="sxs-lookup"><span data-stu-id="92b73-151">tooview your available subscriptions with Azure CLI 2.0, use:</span></span>

```azurecli
az account list
```

<span data-ttu-id="92b73-152">És tooset hello előfizetésben, használja:</span><span class="sxs-lookup"><span data-stu-id="92b73-152">And, tooset hello current subscription, use:</span></span>

```azurecli
az account set --subscription {subscription-name}
```

## <a name="invalidtemplate"></a><span data-ttu-id="92b73-153">InvalidTemplate</span><span class="sxs-lookup"><span data-stu-id="92b73-153">InvalidTemplate</span></span>
<span data-ttu-id="92b73-154">Ez a hiba oka lehet több különböző típusú hibák.</span><span class="sxs-lookup"><span data-stu-id="92b73-154">This error can result from several different types of errors.</span></span>

- <span data-ttu-id="92b73-155">Szintaktikai hiba</span><span class="sxs-lookup"><span data-stu-id="92b73-155">Syntax error</span></span>

   <span data-ttu-id="92b73-156">Ha hello sablont nem sikerült érvényesítési jelző hibaüzenetet kapja, előfordulhat, hogy szintaktikai hiba a sablonban.</span><span class="sxs-lookup"><span data-stu-id="92b73-156">If you receive an error message that indicates hello template failed validation, you may have a syntax problem in your template.</span></span>

  ```
  Code=InvalidTemplate
  Message=Deployment template validation failed
  ```

   <span data-ttu-id="92b73-157">Ez a hiba oka az, könnyen toomake sablon kifejezések bonyolult lehet.</span><span class="sxs-lookup"><span data-stu-id="92b73-157">This error is easy toomake because template expressions can be intricate.</span></span> <span data-ttu-id="92b73-158">Például hello következő hozzárendelést egy tárfiók zárójelek közé egy készletét, három funkció, három különböző kerek zárójeleket tartalmazhatnak, szimpla idézőjelben egy készletét és tartalmaz egy tulajdonság:</span><span class="sxs-lookup"><span data-stu-id="92b73-158">For example, hello following name assignment for a storage account contains one set of brackets, three functions, three sets of parentheses, one set of single quotes, and one property:</span></span>

  ```json
  "name": "[concat('storage', uniqueString(resourceGroup().id))]",
  ```

   <span data-ttu-id="92b73-159">Ha nem ad meg hello megfelelő szintaxist, hello sablon egy érték, amely eltér attól a levelezéshez hoz létre.</span><span class="sxs-lookup"><span data-stu-id="92b73-159">If you do not provide hello matching syntax, hello template produces a value that is different than your intention.</span></span>

   <span data-ttu-id="92b73-160">Hiba az ilyen típusú megjelenésekor gondosan tekintse át a hello kifejezési szintaxist.</span><span class="sxs-lookup"><span data-stu-id="92b73-160">When you receive this type of error, carefully review hello expression syntax.</span></span> <span data-ttu-id="92b73-161">Érdemes lehet például egy JSON-szerkesztővel [Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) vagy [Visual Studio Code](resource-manager-vs-code.md), amely képes figyelmeztessen a szintaktikai hibákat.</span><span class="sxs-lookup"><span data-stu-id="92b73-161">Consider using a JSON editor like [Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) or [Visual Studio Code](resource-manager-vs-code.md), which can warn you about syntax errors.</span></span>

- <span data-ttu-id="92b73-162">Helytelen szegmens hosszának</span><span class="sxs-lookup"><span data-stu-id="92b73-162">Incorrect segment lengths</span></span>

   <span data-ttu-id="92b73-163">Egy másik érvénytelen a sablon-hiba akkor fordul elő, ha hello erőforrás neve nem hello megfelelő formátumban.</span><span class="sxs-lookup"><span data-stu-id="92b73-163">Another invalid template error occurs when hello resource name is not in hello correct format.</span></span>

  ```
  Code=InvalidTemplate
  Message=Deployment template validation failed: 'hello template resource {resource-name}'
  for type {resource-type} has incorrect segment lengths.
  ```

   <span data-ttu-id="92b73-164">Egy legfelső szintű erőforrás hello neve eltér az hello erőforrás típusa egy kisebb szegmenst kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="92b73-164">A root level resource must have one less segment in hello name than in hello resource type.</span></span> <span data-ttu-id="92b73-165">Minden szegmensben perjellel különbözteti meg van.</span><span class="sxs-lookup"><span data-stu-id="92b73-165">Each segment is differentiated by a slash.</span></span> <span data-ttu-id="92b73-166">A következő példa hello, hello típus két szegmensek és hello neve rendelkezik egy szegmens, akkor egy **érvényes**.</span><span class="sxs-lookup"><span data-stu-id="92b73-166">In hello following example, hello type has two segments and hello name has one segment, so it is a **valid name**.</span></span>

  ```json
  {
    "type": "Microsoft.Web/serverfarms",
    "name": "myHostingPlanName",
    ...
  }
  ```

   <span data-ttu-id="92b73-167">De a következő példa hello **nem egy érvényes nevet** mert hello szegmensek azonos számú hello típusként.</span><span class="sxs-lookup"><span data-stu-id="92b73-167">But hello next example is **not a valid name** because it has hello same number of segments as hello type.</span></span>

  ```json
  {
    "type": "Microsoft.Web/serverfarms",
    "name": "appPlan/myHostingPlanName",
    ...
  }
  ```

   <span data-ttu-id="92b73-168">Az alsóbb szintű erőforrásai, hello típusa és neve rendelkezik hello szegmensek azonos számú.</span><span class="sxs-lookup"><span data-stu-id="92b73-168">For child resources, hello type and name have hello same number of segments.</span></span> <span data-ttu-id="92b73-169">A létrehozható szegmensek számát az így, mert hello teljes nevét és típusát hello gyermek tartalmazza hello szülő nevét és típusát.</span><span class="sxs-lookup"><span data-stu-id="92b73-169">This number of segments makes sense because hello full name and type for hello child includes hello parent name and type.</span></span> <span data-ttu-id="92b73-170">Ezért hello teljes név még egy szegmens kevesebb mint hello teljes típusa.</span><span class="sxs-lookup"><span data-stu-id="92b73-170">Therefore, hello full name still has one less segment than hello full type.</span></span>

  ```json
  "resources": [
      {
          "type": "Microsoft.KeyVault/vaults",
          "name": "contosokeyvault",
          ...
          "resources": [
              {
                  "type": "secrets",
                  "name": "appPassword",
                  ...
              }
          ]
      }
  ]
  ```

   <span data-ttu-id="92b73-171">Első hello szegmensek jobb erőforrás-kezelő típusú erőforrás-szolgáltató között alkalmazott megkapni.</span><span class="sxs-lookup"><span data-stu-id="92b73-171">Getting hello segments right can be tricky with Resource Manager types that are applied across resource providers.</span></span> <span data-ttu-id="92b73-172">Például az erőforrás zárolási tooa webhely alkalmazása szükséges négy szegmensek típus.</span><span class="sxs-lookup"><span data-stu-id="92b73-172">For example, applying a resource lock tooa web site requires a type with four segments.</span></span> <span data-ttu-id="92b73-173">Ezért hello neve a következő három szegmensek:</span><span class="sxs-lookup"><span data-stu-id="92b73-173">Therefore, hello name is three segments:</span></span>

  ```json
  {
      "type": "Microsoft.Web/sites/providers/locks",
      "name": "[concat(variables('siteName'),'/Microsoft.Authorization/MySiteLock')]",
      ...
  }
  ```

- <span data-ttu-id="92b73-174">Másolás index nem várt.</span><span class="sxs-lookup"><span data-stu-id="92b73-174">Copy index is not expected</span></span>

   <span data-ttu-id="92b73-175">Ez tapasztal **InvalidTemplate** hiba a hello alkalmazott **másolási** elem tooa részét hello sablont, amely nem támogatja ezt az elemet.</span><span class="sxs-lookup"><span data-stu-id="92b73-175">You encounter this **InvalidTemplate** error when you have applied hello **copy** element tooa part of hello template that does not support this element.</span></span> <span data-ttu-id="92b73-176">Hello másolási elem tooa erőforrás típusa csak alkalmazhatók.</span><span class="sxs-lookup"><span data-stu-id="92b73-176">You can only apply hello copy element tooa resource type.</span></span> <span data-ttu-id="92b73-177">Másolás tooa tulajdonság erőforrástípus belül nem tudja alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="92b73-177">You cannot apply copy tooa property within a resource type.</span></span> <span data-ttu-id="92b73-178">Például másolás tooa virtuális gép telepítését, de a virtuális gép operációs rendszer toohello lemezek nem alkalmazható.</span><span class="sxs-lookup"><span data-stu-id="92b73-178">For example, you apply copy tooa virtual machine, but you cannot apply it toohello OS disks for a virtual machine.</span></span> <span data-ttu-id="92b73-179">Bizonyos esetekben átválthat egy gyermek erőforrás tooa szülő erőforrás toocreate a másolási ciklust.</span><span class="sxs-lookup"><span data-stu-id="92b73-179">In some cases, you can convert a child resource tooa parent resource toocreate a copy loop.</span></span> <span data-ttu-id="92b73-180">Másolás használatával kapcsolatos további információkért lásd: [erőforrások több példánya létrehozása az Azure Resource Manager](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="92b73-180">For more information about using copy, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span>

- <span data-ttu-id="92b73-181">Érvénytelen paraméter</span><span class="sxs-lookup"><span data-stu-id="92b73-181">Parameter is not valid</span></span>

   <span data-ttu-id="92b73-182">Ha hello sablon határozza meg a paraméter megengedett értékei, és megad egy értéket, amely nem egy ezeket az értékeket, egy üzenet hasonló toohello, a következő hiba jelenhet meg:</span><span class="sxs-lookup"><span data-stu-id="92b73-182">If hello template specifies permitted values for a parameter, and you provide a value that is not one of those values, you receive a message similar toohello following error:</span></span>

  ```
  Code=InvalidTemplate;
  Message=Deployment template validation failed: 'hello provided value {parameter value}
  for hello template parameter {parameter name} is not valid. hello parameter value is not
  part of hello allowed values
  ``` 

   <span data-ttu-id="92b73-183">Ellenőrizze hello hello sablon az engedélyezett értékek, és adjon meg egy központi telepítése során.</span><span class="sxs-lookup"><span data-stu-id="92b73-183">Double check hello allowed values in hello template, and provide one during deployment.</span></span>

- <span data-ttu-id="92b73-184">Körkörös függőséget észlelt</span><span class="sxs-lookup"><span data-stu-id="92b73-184">Circular dependency detected</span></span>

   <span data-ttu-id="92b73-185">Hibaüzenet az erőforrások lehetnek egymással függőségi viszonyban oly módon, hogy megakadályozza, hogy a hello telepítési indítását.</span><span class="sxs-lookup"><span data-stu-id="92b73-185">You receive this error when resources depend on each other in a way that prevents hello deployment from starting.</span></span> <span data-ttu-id="92b73-186">Egy egymástól függő szolgáltatásainak összevonás a két vagy több erőforrás, várjon, amíg más erőforrások, amelyek is várnak.</span><span class="sxs-lookup"><span data-stu-id="92b73-186">A combination of interdependencies makes two or more resource wait for other resources that are also waiting.</span></span> <span data-ttu-id="92b73-187">Például resource1 resource3 függ, resource2 resource1 függ, és resource3 resource2 függ.</span><span class="sxs-lookup"><span data-stu-id="92b73-187">For example, resource1 depends on resource3, resource2 depends on resource1, and resource3 depends on resource2.</span></span> <span data-ttu-id="92b73-188">Eltávolítja a szükségtelen függőségek általában megoldhatja a problémát.</span><span class="sxs-lookup"><span data-stu-id="92b73-188">You can usually solve this problem by removing unnecessary dependencies.</span></span> 

<a id="notfound" />
### <a name="notfound-and-resourcenotfound"></a><span data-ttu-id="92b73-189">NotFound és ResourceNotFound</span><span class="sxs-lookup"><span data-stu-id="92b73-189">NotFound and ResourceNotFound</span></span>
<span data-ttu-id="92b73-190">Ha a sablon tartalmazza, amely nem oldható fel erőforrás hello neve, hasonló hibaüzenetet kap:</span><span class="sxs-lookup"><span data-stu-id="92b73-190">When your template includes hello name of a resource that cannot be resolved, you receive an error similar to:</span></span>

```
Code=NotFound;
Message=Cannot find ServerFarm with name exampleplan.
```

<span data-ttu-id="92b73-191">Ha a kívánt toodeploy hello hello sablonban erőforrás hiányzik, ellenőrizze, hogy szükséges-e tooadd függőség.</span><span class="sxs-lookup"><span data-stu-id="92b73-191">If you are attempting toodeploy hello missing resource in hello template, check whether you need tooadd a dependency.</span></span> <span data-ttu-id="92b73-192">Erőforrás-kezelő optimalizálja a központi telepítési erőforrások létrehozásával párhuzamosan, amikor lehetséges.</span><span class="sxs-lookup"><span data-stu-id="92b73-192">Resource Manager optimizes deployment by creating resources in parallel, when possible.</span></span> <span data-ttu-id="92b73-193">Ha egy erőforrást egy másik erőforrás után kell telepíteni, akkor kell-e toouse hello **dependsOn** elem található a sablon toocreate a függőség hello egyéb erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="92b73-193">If one resource must be deployed after another resource, you need toouse hello **dependsOn** element in your template toocreate a dependency on hello other resource.</span></span> <span data-ttu-id="92b73-194">Például a webes alkalmazás telepítésekor hello App Service-csomag léteznie kell.</span><span class="sxs-lookup"><span data-stu-id="92b73-194">For example, when deploying a web app, hello App Service plan must exist.</span></span> <span data-ttu-id="92b73-195">Ha nem adott meg, hogy hello webalkalmazás hello App Service-csomag függ, erőforrás-kezelő létrehoz-e az hello mindkét erőforrás ugyanannyi időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="92b73-195">If you have not specified that hello web app depends on hello App Service plan, Resource Manager creates both resources at hello same time.</span></span> <span data-ttu-id="92b73-196">Hibaüzenet jelenik meg, amely meghatározza, hogy adott hello App Service-csomag erőforrás nem található, mert nem létezik még tooset hello webalkalmazásban tulajdonság megkísérlése során.</span><span class="sxs-lookup"><span data-stu-id="92b73-196">You receive an error stating that hello App Service plan resource cannot be found, because it does not exist yet when attempting tooset a property on hello web app.</span></span> <span data-ttu-id="92b73-197">Megakadályozható, hogy ezt a hibát úgy, hogy hello függőségi hello web app alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="92b73-197">You prevent this error by setting hello dependency in hello web app.</span></span>

```json
{
  "apiVersion": "2015-08-01",
  "type": "Microsoft.Web/sites",
  "dependsOn": [
    "[variables('hostingPlanName')]"
  ],
  ...
}
```

<span data-ttu-id="92b73-198">Függőségi hiba elhárításával kapcsolatos javaslatok, lásd: [ellenőrizze a telepítési sorrendjét](#check-deployment-sequence).</span><span class="sxs-lookup"><span data-stu-id="92b73-198">For suggestions on troubleshooting dependency errors, see [Check deployment sequence](#check-deployment-sequence).</span></span>

<span data-ttu-id="92b73-199">Ez a hiba sem hello erőforrás létezik egy másik erőforráscsoportban található, mint egy telepítendő hello látni.</span><span class="sxs-lookup"><span data-stu-id="92b73-199">You also see this error when hello resource exists in a different resource group than hello one being deployed to.</span></span> <span data-ttu-id="92b73-200">Ebben az esetben használja a hello [resourceId függvény](resource-group-template-functions-resource.md#resourceid) tooget hello teljesen minősített hello erőforrás nevét.</span><span class="sxs-lookup"><span data-stu-id="92b73-200">In that case, use hello [resourceId function](resource-group-template-functions-resource.md#resourceid) tooget hello fully qualified name of hello resource.</span></span>

```json
"properties": {
    "name": "[parameters('siteName')]",
    "serverFarmId": "[resourceId('plangroup', 'Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
}
```

<span data-ttu-id="92b73-201">Ha úgy próbálja toouse hello [hivatkozás](resource-group-template-functions-resource.md#reference) vagy [listKeys](resource-group-template-functions-resource.md#listkeys) működik együtt a erőforrása, amely nem oldható fel, megjelenik a következő hiba hello:</span><span class="sxs-lookup"><span data-stu-id="92b73-201">If you attempt toouse hello [reference](resource-group-template-functions-resource.md#reference) or [listKeys](resource-group-template-functions-resource.md#listkeys) functions with a resource that cannot be resolved, you receive hello following error:</span></span>

```
Code=ResourceNotFound;
Message=hello Resource 'Microsoft.Storage/storageAccounts/{storage name}' under resource
group {resource group name} was not found.
```

<span data-ttu-id="92b73-202">Egy kifejezés, amely magában foglalja a hello keressen **hivatkozás** függvény.</span><span class="sxs-lookup"><span data-stu-id="92b73-202">Look for an expression that includes hello **reference** function.</span></span> <span data-ttu-id="92b73-203">A ellenőrizze, hogy helyesek-e a hello paraméterértékeket.</span><span class="sxs-lookup"><span data-stu-id="92b73-203">Double check that hello parameter values are correct.</span></span>

## <a name="parentresourcenotfound"></a><span data-ttu-id="92b73-204">ParentResourceNotFound</span><span class="sxs-lookup"><span data-stu-id="92b73-204">ParentResourceNotFound</span></span>

<span data-ttu-id="92b73-205">Amikor egy erőforrást egy szülő tooanother erőforrás, hello szülő erőforrás hello gyermek-erőforrás létrehozása előtt léteznie kell.</span><span class="sxs-lookup"><span data-stu-id="92b73-205">When one resource is a parent tooanother resource, hello parent resource must exist before creating hello child resource.</span></span> <span data-ttu-id="92b73-206">Ha még nem létezik, hello a következő hiba jelenhet meg:</span><span class="sxs-lookup"><span data-stu-id="92b73-206">If it does not yet exist, you receive hello following error:</span></span>

```
Code=ParentResourceNotFound;
Message=Can not perform requested operation on nested resource. Parent resource 'exampleserver' not found."
```

<span data-ttu-id="92b73-207">hello gyermek-erőforrás nevét hello hello szülő nevének tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="92b73-207">hello name of hello child resource includes hello parent name.</span></span> <span data-ttu-id="92b73-208">Például egy SQL-adatbázis előfordulhat, hogy meghatározása:</span><span class="sxs-lookup"><span data-stu-id="92b73-208">For example, a SQL Database might be defined as:</span></span>

```json
{
  "type": "Microsoft.Sql/servers/databases",
  "name": "[concat(variables('databaseServerName'), '/', parameters('databaseName'))]",
  ...
```

<span data-ttu-id="92b73-209">De ha nincs megadva a függőség hello szülő erőforrás, hello gyermek erőforrás előfordulhat, hogy telepítve előtt hello szülő.</span><span class="sxs-lookup"><span data-stu-id="92b73-209">But, if you do not specify a dependency on hello parent resource, hello child resource may get deployed before hello parent.</span></span> <span data-ttu-id="92b73-210">tooresolve Ez a hiba egy függőséget tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="92b73-210">tooresolve this error, include a dependency.</span></span>

```json
"dependsOn": [
    "[variables('databaseServerName')]"
]
```

<a id="storagenamenotunique" />

## <a name="storageaccountalreadyexists-and-storageaccountalreadytaken"></a><span data-ttu-id="92b73-211">StorageAccountAlreadyExists és StorageAccountAlreadyTaken</span><span class="sxs-lookup"><span data-stu-id="92b73-211">StorageAccountAlreadyExists and StorageAccountAlreadyTaken</span></span>
<span data-ttu-id="92b73-212">Storage-fiókok meg kell adnia egy nevet, amely egyedi egész Azure hello erőforrás.</span><span class="sxs-lookup"><span data-stu-id="92b73-212">For storage accounts, you must provide a name for hello resource that is unique across Azure.</span></span> <span data-ttu-id="92b73-213">Ha nem ad meg egy egyedi nevet, például hibaüzenetet kap:</span><span class="sxs-lookup"><span data-stu-id="92b73-213">If you do not provide a unique name, you receive an error like:</span></span>

```
Code=StorageAccountAlreadyTaken
Message=hello storage account named mystorage is already taken.
```

<span data-ttu-id="92b73-214">Létrehozhat egy egyedi nevet az elnevezési hello hello eredményét a szereplő [uniqueString](resource-group-template-functions-string.md#uniquestring) függvény.</span><span class="sxs-lookup"><span data-stu-id="92b73-214">You can create a unique name by concatenating your naming convention with hello result of hello [uniqueString](resource-group-template-functions-string.md#uniquestring) function.</span></span>

```json
"name": "[concat('storage', uniqueString(resourceGroup().id))]",
"type": "Microsoft.Storage/storageAccounts",
```

<span data-ttu-id="92b73-215">Ha telepít egy tárfiók hello azonos nevet, amint egy meglévő tárfiók az előfizetésben, de adjon meg egy másik helyre, jelző hello tárfiók már létezik egy másik helyen lévő hibaüzenetet kap.</span><span class="sxs-lookup"><span data-stu-id="92b73-215">If you deploy a storage account with hello same name as an existing storage account in your subscription, but provide a different location, you receive an error indicating hello storage account already exists in a different location.</span></span> <span data-ttu-id="92b73-216">Vagy törölje a meglévő tárfiók hello, vagy adjon meg hello ugyanazon a helyen, a meglévő tárfiók hello.</span><span class="sxs-lookup"><span data-stu-id="92b73-216">Either delete hello existing storage account, or provide hello same location as hello existing storage account.</span></span>

## <a name="accountnameinvalid"></a><span data-ttu-id="92b73-217">AccountNameInvalid</span><span class="sxs-lookup"><span data-stu-id="92b73-217">AccountNameInvalid</span></span>
<span data-ttu-id="92b73-218">Megjelenik a hello **AccountNameInvalid** hiba a tárolási fiók nevét, amely tartalmazza az toogive kísérlet tiltott karakter.</span><span class="sxs-lookup"><span data-stu-id="92b73-218">You see hello **AccountNameInvalid** error when attempting toogive a storage account a name that includes prohibited characters.</span></span> <span data-ttu-id="92b73-219">Tárfiókok neve 3 – 24 karakter hosszúságúnak kell és számokat és kisbetűket tartalmazhatnak csak.</span><span class="sxs-lookup"><span data-stu-id="92b73-219">Storage account names must be between 3 and 24 characters in length and use numbers and lower-case letters only.</span></span> <span data-ttu-id="92b73-220">Hello [uniqueString](resource-group-template-functions-string.md#uniquestring) függvény 13 karaktert adja vissza.</span><span class="sxs-lookup"><span data-stu-id="92b73-220">hello [uniqueString](resource-group-template-functions-string.md#uniquestring) function returns 13 characters.</span></span> <span data-ttu-id="92b73-221">Ha Ön egy előtag toohello összefűzésére **uniqueString** vezethet, adjon meg egy előtag 11 karakter vagy kevesebb.</span><span class="sxs-lookup"><span data-stu-id="92b73-221">If you concatenate a prefix toohello **uniqueString** result, provide a prefix that is 11 characters or less.</span></span>

## <a name="badrequest"></a><span data-ttu-id="92b73-222">BadRequest</span><span class="sxs-lookup"><span data-stu-id="92b73-222">BadRequest</span></span>

<span data-ttu-id="92b73-223">Ha egy tulajdonság érvénytelen értéket ad meg BadRequest állapot merülhetnek fel.</span><span class="sxs-lookup"><span data-stu-id="92b73-223">You may encounter a BadRequest status when you provide an invalid value for a property.</span></span> <span data-ttu-id="92b73-224">Például ha egy tárfiók Termékváltozata helytelen értéket ad meg, hello központi telepítés sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="92b73-224">For example, if you provide an incorrect SKU value for a storage account, hello deployment fails.</span></span> <span data-ttu-id="92b73-225">érvényes értékei toodetermine tulajdonság, nézze meg hello [REST API](/rest/api) hello erőforrástípus telepít.</span><span class="sxs-lookup"><span data-stu-id="92b73-225">toodetermine valid values for property, look at hello [REST API](/rest/api) for hello resource type you are deploying.</span></span>

<a id="noregisteredproviderfound" />

## <a name="noregisteredproviderfound-and-missingsubscriptionregistration"></a><span data-ttu-id="92b73-226">NoRegisteredProviderFound és MissingSubscriptionRegistration</span><span class="sxs-lookup"><span data-stu-id="92b73-226">NoRegisteredProviderFound and MissingSubscriptionRegistration</span></span>
<span data-ttu-id="92b73-227">Erőforrás telepítésekor hibakód a következő hello kap, és üzenet:</span><span class="sxs-lookup"><span data-stu-id="92b73-227">When deploying resource, you may receive hello following error code and message:</span></span>

```
Code: NoRegisteredProviderFound
Message: No registered resource provider found for location {location}
and API version {api-version} for type {resource-type}.
```

<span data-ttu-id="92b73-228">Vagy hasonló üzenet jelenhet meg:</span><span class="sxs-lookup"><span data-stu-id="92b73-228">Or, you may receive a similar message that states:</span></span>

```
Code: MissingSubscriptionRegistration
Message: hello subscription is not registered toouse namespace {resource-provider-namespace}
```

<span data-ttu-id="92b73-229">Ezek a hibák három okok egyike jelenhet meg:</span><span class="sxs-lookup"><span data-stu-id="92b73-229">You receive these errors for one of three reasons:</span></span>

1. <span data-ttu-id="92b73-230">hello erőforrás-szolgáltató nincs regisztrálva az előfizetéséhez</span><span class="sxs-lookup"><span data-stu-id="92b73-230">hello resource provider has not been registered for your subscription</span></span>
2. <span data-ttu-id="92b73-231">Hello erőforrástípus nem támogatott API-verzió</span><span class="sxs-lookup"><span data-stu-id="92b73-231">API version not supported for hello resource type</span></span>
3. <span data-ttu-id="92b73-232">Hely hello erőforrástípus nem támogatott</span><span class="sxs-lookup"><span data-stu-id="92b73-232">Location not supported for hello resource type</span></span>

<span data-ttu-id="92b73-233">hello hibaüzenet hello támogatott helyek és API-verziók javaslatokat adjon meg.</span><span class="sxs-lookup"><span data-stu-id="92b73-233">hello error message should give you suggestions for hello supported locations and API versions.</span></span> <span data-ttu-id="92b73-234">Módosíthatja a sablon tooone hello a javasolt értékek.</span><span class="sxs-lookup"><span data-stu-id="92b73-234">You can change your template tooone of hello suggested values.</span></span> <span data-ttu-id="92b73-235">A legtöbb szolgáltatók által hello Azure portal vagy hello parancssori felületet használ, de nem mindegyik automatikusan regisztrált.</span><span class="sxs-lookup"><span data-stu-id="92b73-235">Most providers are registered automatically by hello Azure portal or hello command-line interface you are using, but not all.</span></span> <span data-ttu-id="92b73-236">Ha még nem használta az egy adott erőforrás-szolgáltató előtt, szükség lehet tooregister ezt a szolgáltatót.</span><span class="sxs-lookup"><span data-stu-id="92b73-236">If you have not used a particular resource provider before, you may need tooregister that provider.</span></span> <span data-ttu-id="92b73-237">További információ a PowerShell vagy Azure parancssori felületen keresztül erőforrás-szolgáltatók felderíthetők.</span><span class="sxs-lookup"><span data-stu-id="92b73-237">You can discover more about resource providers through PowerShell or Azure CLI.</span></span>

<span data-ttu-id="92b73-238">**Portál**</span><span class="sxs-lookup"><span data-stu-id="92b73-238">**Portal**</span></span>

<span data-ttu-id="92b73-239">Hello regisztrációs állapotát tekintheti meg, és regisztrálja egy erőforrás-szolgáltató névtere hello portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="92b73-239">You can see hello registration status and register a resource provider namespace through hello portal.</span></span>

1. <span data-ttu-id="92b73-240">Az előfizetéshez, válasszon **erőforrás-szolgáltató**.</span><span class="sxs-lookup"><span data-stu-id="92b73-240">For your subscription, select **Resource providers**.</span></span>

   ![Válassza ki az erőforrás-szolgáltató](./media/resource-manager-common-deployment-errors/select-resource-provider.png)

2. <span data-ttu-id="92b73-242">Nézze meg hello erőforrás-szolgáltatók listáját, és szükség esetén válasszon ki hello **regisztrálása** hivatkozás tooregister hello erőforrás-szolgáltató hello típusú toodeploy próbált.</span><span class="sxs-lookup"><span data-stu-id="92b73-242">Look at hello list of resource providers, and if necessary, select hello **Register** link tooregister hello resource provider of hello type you are trying toodeploy.</span></span>

   ![erőforrás-szolgáltatók felsorolása](./media/resource-manager-common-deployment-errors/list-resource-providers.png)

<span data-ttu-id="92b73-244">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="92b73-244">**PowerShell**</span></span>

<span data-ttu-id="92b73-245">toosee a regisztrációs állapotát, használja **Get-AzureRmResourceProvider**.</span><span class="sxs-lookup"><span data-stu-id="92b73-245">toosee your registration status, use **Get-AzureRmResourceProvider**.</span></span>

```powershell
Get-AzureRmResourceProvider -ListAvailable
```

<span data-ttu-id="92b73-246">a szolgáltató tooregister használja **Register-AzureRmResourceProvider** és hello nevezze hello erőforrás-szolgáltató tooregister kívánja.</span><span class="sxs-lookup"><span data-stu-id="92b73-246">tooregister a provider, use **Register-AzureRmResourceProvider** and provide hello name of hello resource provider you wish tooregister.</span></span>

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Cdn
```

<span data-ttu-id="92b73-247">egy adott típusú erőforrás, tooget hello támogatott helyek használja:</span><span class="sxs-lookup"><span data-stu-id="92b73-247">tooget hello supported locations for a particular type of resource, use:</span></span>

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations
```

<span data-ttu-id="92b73-248">tooget hello támogatott API-verziók egy adott típusú erőforrás használata:</span><span class="sxs-lookup"><span data-stu-id="92b73-248">tooget hello supported API versions for a particular type of resource, use:</span></span>

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).ApiVersions
```

<span data-ttu-id="92b73-249">**Azure CLI**</span><span class="sxs-lookup"><span data-stu-id="92b73-249">**Azure CLI**</span></span>

<span data-ttu-id="92b73-250">toosee hello szolgáltató regisztrálva van, hogy használja-hello `azure provider list` parancsot.</span><span class="sxs-lookup"><span data-stu-id="92b73-250">toosee whether hello provider is registered, use hello `azure provider list` command.</span></span>

```azurecli
az provider list
```

<span data-ttu-id="92b73-251">egy erőforrás-szolgáltató tooregister hello használata `azure provider register` parancsot, és adja meg a hello *névtér* tooregister.</span><span class="sxs-lookup"><span data-stu-id="92b73-251">tooregister a resource provider, use hello `azure provider register` command, and specify hello *namespace* tooregister.</span></span>

```azurecli
az provider register --namespace Microsoft.Cdn
```

<span data-ttu-id="92b73-252">toosee hello támogatott helyek és egy erőforrástípusra API-verziók használata:</span><span class="sxs-lookup"><span data-stu-id="92b73-252">toosee hello supported locations and API versions for a resource type, use:</span></span>

```azurecli
az provider show -n Microsoft.Web --query "resourceTypes[?resourceType=='sites'].locations"
```

<a id="quotaexceeded" />

## <a name="quotaexceeded-and-operationnotallowed"></a><span data-ttu-id="92b73-253">QuotaExceeded és OperationNotAllowed</span><span class="sxs-lookup"><span data-stu-id="92b73-253">QuotaExceeded and OperationNotAllowed</span></span>
<span data-ttu-id="92b73-254">Problémák állhatnak, ha a központi telepítés meghaladja a kvótát, legyen az erőforráscsoportot, előfizetések, fiókok és egyéb hatókörök.</span><span class="sxs-lookup"><span data-stu-id="92b73-254">You might have issues when deployment exceeds a quota, which could be per resource group, subscriptions, accounts, and other scopes.</span></span> <span data-ttu-id="92b73-255">Az előfizetés lehet például a régió magok száma toolimit hello konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="92b73-255">For example, your subscription may be configured toolimit hello number of cores for a region.</span></span> <span data-ttu-id="92b73-256">Ha úgy próbálja toodeploy engedélyezett összeg hello mint, több maggal rendelkező virtuális gép, hibaüzenet azzal hello kvóta túl lett lépve.</span><span class="sxs-lookup"><span data-stu-id="92b73-256">If you attempt toodeploy a virtual machine with more cores than hello permitted amount, you receive an error stating hello quota has been exceeded.</span></span>
<span data-ttu-id="92b73-257">Teljes kvóta információkért lásd: [Azure-előfizetés és szolgáltatási korlátok, kvóták és megkötések](../azure-subscription-service-limits.md).</span><span class="sxs-lookup"><span data-stu-id="92b73-257">For complete quota information, see [Azure subscription and service limits, quotas, and constraints](../azure-subscription-service-limits.md).</span></span>

<span data-ttu-id="92b73-258">tooexamine az előfizetése magokra vonatkozó kvótákat mag, hello használhatja `azure vm list-usage` hello Azure CLI parancsot.</span><span class="sxs-lookup"><span data-stu-id="92b73-258">tooexamine your subscription's quotas for cores, you can use hello `azure vm list-usage` command in hello Azure CLI.</span></span> <span data-ttu-id="92b73-259">a következő példa hello mutatja be, hogy hello core kvóta, egy ingyenes próbafiók érték 4:</span><span class="sxs-lookup"><span data-stu-id="92b73-259">hello following example illustrates that hello core quota for a free trial account is 4:</span></span>

```azurecli
az vm list-usage --location "South Central US"
```

<span data-ttu-id="92b73-260">Amely adja vissza:</span><span class="sxs-lookup"><span data-stu-id="92b73-260">Which returns:</span></span>

```azurecli
[
  {
    "currentValue": 0,
    "limit": 2000,
    "name": {
      "localizedValue": "Availability Sets",
      "value": "availabilitySets"
    }
  },
  ...
]
```

<span data-ttu-id="92b73-261">Ha négynél több maggal hello USA nyugati régiója régióban egy sablont, kap egy központi telepítési hiba, amely a következőhöz hasonló:</span><span class="sxs-lookup"><span data-stu-id="92b73-261">If you deploy a template that creates more than four cores in hello West US region, you get a deployment error that looks like:</span></span>

```
Code=OperationNotAllowed
Message=Operation results in exceeding quota limits of Core.
Maximum allowed: 4, Current in use: 4, Additional requested: 2.
```

<span data-ttu-id="92b73-262">Vagy a PowerShellben használható hello **Get-AzureRmVMUsage** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="92b73-262">Or in PowerShell, you can use hello **Get-AzureRmVMUsage** cmdlet.</span></span>

```powershell
Get-AzureRmVMUsage
```

<span data-ttu-id="92b73-263">Amely adja vissza:</span><span class="sxs-lookup"><span data-stu-id="92b73-263">Which returns:</span></span>

```powershell
...
CurrentValue : 0
Limit        : 4
Name         : {
                 "value": "cores",
                 "localizedValue": "Total Regional Cores"
               }
Unit         : null
...
```

<span data-ttu-id="92b73-264">Ezekben az esetekben toohello portálon lépjen és a fájl egy támogatási probléma tooraise hello régió toodeploy szeretné a kvótát.</span><span class="sxs-lookup"><span data-stu-id="92b73-264">In these cases, you should go toohello portal and file a support issue tooraise your quota for hello region into which you want toodeploy.</span></span>

> [!NOTE]
> <span data-ttu-id="92b73-265">Ne feledje, hogy az erőforráscsoportok, hello kvóta minden egyes régió, nem a teljes előfizetés hello.</span><span class="sxs-lookup"><span data-stu-id="92b73-265">Remember that for resource groups, hello quota is for each individual region, not for hello entire subscription.</span></span> <span data-ttu-id="92b73-266">Toodeploy 30 magok az USA nyugati régiója van szüksége, az USA nyugati régiója a 30 erőforrás-kezelő magok tooask lesz.</span><span class="sxs-lookup"><span data-stu-id="92b73-266">If you need toodeploy 30 cores in West US, you have tooask for 30 Resource Manager cores in West US.</span></span> <span data-ttu-id="92b73-267">Ha toodeploy 30 magok hello régiók toowhich valamelyikében kell rendelkezik hozzáféréssel, kérdezze meg a 30 erőforrás-kezelő magok minden régióban.</span><span class="sxs-lookup"><span data-stu-id="92b73-267">If you need toodeploy 30 cores in any of hello regions toowhich you have access, you should ask for 30 Resource Manager cores in all regions.</span></span>
>
>

## <a name="invalidcontentlink"></a><span data-ttu-id="92b73-268">InvalidContentLink</span><span class="sxs-lookup"><span data-stu-id="92b73-268">InvalidContentLink</span></span>
<span data-ttu-id="92b73-269">Ha a hello hibaüzenetet kapja:</span><span class="sxs-lookup"><span data-stu-id="92b73-269">When you receive hello error message:</span></span>

```
Code=InvalidContentLink
Message=Unable toodownload deployment content from ...
```

<span data-ttu-id="92b73-270">Valószínűleg próbált toolink tooa beágyazott sablont, amely nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="92b73-270">You have most likely attempted toolink tooa nested template that is not available.</span></span> <span data-ttu-id="92b73-271">Ellenőrizze hello hello beágyazott sablonhoz megadott URI.</span><span class="sxs-lookup"><span data-stu-id="92b73-271">Double check hello URI you provided for hello nested template.</span></span> <span data-ttu-id="92b73-272">Ha hello sablon szerepel egy tárfiókot, ellenőrizze, hogy elérhető-e a hello URI.</span><span class="sxs-lookup"><span data-stu-id="92b73-272">If hello template exists in a storage account, make sure hello URI is accessible.</span></span> <span data-ttu-id="92b73-273">Szükség lehet egy SAS-jogkivonat toopass.</span><span class="sxs-lookup"><span data-stu-id="92b73-273">You may need toopass a SAS token.</span></span> <span data-ttu-id="92b73-274">További információ: [Kapcsolt sablonok használata az Azure Resource Manager eszközben](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="92b73-274">For more information, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>

## <a name="requestdisallowedbypolicy"></a><span data-ttu-id="92b73-275">RequestDisallowedByPolicy</span><span class="sxs-lookup"><span data-stu-id="92b73-275">RequestDisallowedByPolicy</span></span>
<span data-ttu-id="92b73-276">Hibaüzenet az előfizetése tartalmaz egy erőforrás-házirendet, amely megakadályozza a művelet kívánt tooperform üzembe helyezése során.</span><span class="sxs-lookup"><span data-stu-id="92b73-276">You receive this error when your subscription includes a resource policy that prevents an action you are trying tooperform during deployment.</span></span> <span data-ttu-id="92b73-277">Hello hibaüzenet jelenik meg keresse meg hello házirend-azonosító.</span><span class="sxs-lookup"><span data-stu-id="92b73-277">In hello error message, look for hello policy identifier.</span></span>

```
Policy identifier(s): '/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition'
```

<span data-ttu-id="92b73-278">A **PowerShell**, adja meg, hogy a házirend az azonosítója, mint hello **azonosító** hello-szabályzatot, amely blokkolja a telepítési paraméter tooretrieve részleteit.</span><span class="sxs-lookup"><span data-stu-id="92b73-278">In **PowerShell**, provide that policy identifier as hello **Id** parameter tooretrieve details about hello policy that blocked your deployment.</span></span>

```powershell
(Get-AzureRmPolicyDefinition -Id "/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition").Properties.policyRule | ConvertTo-Json
```

<span data-ttu-id="92b73-279">A **Azure CLI**, adja meg a házirend-definíció hello hello nevét:</span><span class="sxs-lookup"><span data-stu-id="92b73-279">In **Azure CLI**, provide hello name of hello policy definition:</span></span>

```azurecli
az policy definition show --name regionPolicyAssignment
```

<span data-ttu-id="92b73-280">További információkért tekintse meg a következő cikkek hello:</span><span class="sxs-lookup"><span data-stu-id="92b73-280">For more information, see hello following articles:</span></span>

- [<span data-ttu-id="92b73-281">RequestDisallowedByPolicy hiba</span><span class="sxs-lookup"><span data-stu-id="92b73-281">RequestDisallowedByPolicy error</span></span>](resource-manager-policy-requestdisallowedbypolicy-error.md)
- <span data-ttu-id="92b73-282">[Házirend toomanage erőforrásainak használatához, és hozzáférést](resource-manager-policy.md).</span><span class="sxs-lookup"><span data-stu-id="92b73-282">[Use Policy toomanage resources and control access](resource-manager-policy.md).</span></span>

## <a name="authorization-failed"></a><span data-ttu-id="92b73-283">A bejelentkezés nem sikerült</span><span class="sxs-lookup"><span data-stu-id="92b73-283">Authorization failed</span></span>
<span data-ttu-id="92b73-284">Hiba jelenhet üzembe helyezése során, mert hello fiók vagy egyszerű toodeploy hello erőforrások próbált szolgáltatás nem rendelkezik hozzáférési tooperform ezeket a műveleteket.</span><span class="sxs-lookup"><span data-stu-id="92b73-284">You may receive an error during deployment because hello account or service principal attempting toodeploy hello resources does not have access tooperform those actions.</span></span> <span data-ttu-id="92b73-285">Az Azure Active Directory lehetővé teszi, hogy Ön vagy a rendszergazda toocontrol, mely identitások nagy fokú pontosságot milyen erőforrások eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="92b73-285">Azure Active Directory enables you or your administrator toocontrol which identities can access what resources with a great degree of precision.</span></span> <span data-ttu-id="92b73-286">Például toohello olvasó szerepkört az Ön fiókjához van társítva, ha még nem tud toocreate erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="92b73-286">For example, if your account is assigned toohello Reader role, you are not able toocreate resources.</span></span> <span data-ttu-id="92b73-287">Ebben az esetben megjelenik egy hibaüzenet üzent arról, hogy engedélyezése nem sikerült.</span><span class="sxs-lookup"><span data-stu-id="92b73-287">In that case, you see an error message indicating that authorization failed.</span></span>

<span data-ttu-id="92b73-288">Szerepköralapú hozzáférés-vezérléssel kapcsolatos további információkért lásd: [átruházásához hozzáférés-vezérlés](../active-directory/role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="92b73-288">For more information about role-based access control, see [Azure Role-Based Access Control](../active-directory/role-based-access-control-configure.md).</span></span>


## <a name="next-steps"></a><span data-ttu-id="92b73-289">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="92b73-289">Next steps</span></span>
* <span data-ttu-id="92b73-290">toolearn kapcsolatos naplózási műveletek, lásd: [naplózási műveletek a Resource Manager](resource-group-audit.md).</span><span class="sxs-lookup"><span data-stu-id="92b73-290">toolearn about auditing actions, see [Audit operations with Resource Manager](resource-group-audit.md).</span></span>
* <span data-ttu-id="92b73-291">toolearn kapcsolatos műveletek toodetermine hello hibák központi telepítése során lásd: [üzembe helyezési műveleteinek megtekintése](resource-manager-deployment-operations.md).</span><span class="sxs-lookup"><span data-stu-id="92b73-291">toolearn about actions toodetermine hello errors during deployment, see [View deployment operations](resource-manager-deployment-operations.md).</span></span>
