---
title: "Azure-erőforrások módosítható zárolása |} Microsoft Docs"
description: "Egyes felhasználók a frissítése vagy törlése a kritikus fontosságú Azure-erőforrások a zárolási összes felhasználók és szerepkörök alkalmazásával."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 53c57e8f-741c-4026-80e0-f4c02638c98b
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: tomfitz
ms.openlocfilehash: 44c87b00f4fc63dbfd45a07d9a8cddc5eaf1a65c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="lock-resources-to-prevent-unexpected-changes"></a><span data-ttu-id="773f0-103">Váratlan módosítható erőforrások zárolása</span><span class="sxs-lookup"><span data-stu-id="773f0-103">Lock resources to prevent unexpected changes</span></span> 
<span data-ttu-id="773f0-104">Ha Ön rendszergazda szükség lehet egy előfizetés, erőforráscsoportból vagy erőforrás véletlen törlése vagy a kritikus erőforrásokat módosítása a munkahely más felhasználóinak megelőzése érdekében zárolja.</span><span class="sxs-lookup"><span data-stu-id="773f0-104">As an administrator, you may need to lock a subscription, resource group, or resource to prevent other users in your organization from accidentally deleting or modifying critical resources.</span></span> <span data-ttu-id="773f0-105">Beállíthatja a zárolási szint beállítása azokhoz a **CanNotDelete** vagy **ReadOnly**.</span><span class="sxs-lookup"><span data-stu-id="773f0-105">You can set the lock level to **CanNotDelete** or **ReadOnly**.</span></span> 

* <span data-ttu-id="773f0-106">**CanNotDelete** azt jelenti, hogy a jogosult felhasználók továbbra is olvasni és módosítani az erőforrást, de azokat nem lehet törölni az erőforrás.</span><span class="sxs-lookup"><span data-stu-id="773f0-106">**CanNotDelete** means authorized users can still read and modify a resource, but they can't delete the resource.</span></span> 
* <span data-ttu-id="773f0-107">**Csak olvasható** azt jelenti, hogy a jogosult felhasználók olvashatják egy erőforrást, de nem lehet törölni vagy az erőforrás frissítése.</span><span class="sxs-lookup"><span data-stu-id="773f0-107">**ReadOnly** means authorized users can read a resource, but they can't delete or update the resource.</span></span> <span data-ttu-id="773f0-108">A zárolás alkalmazása hasonlít összes jogosult felhasználó által megadott engedélyek korlátozása a **olvasó** szerepkör.</span><span class="sxs-lookup"><span data-stu-id="773f0-108">Applying this lock is similar to restricting all authorized users to the permissions granted by the **Reader** role.</span></span> 

## <a name="how-locks-are-applied"></a><span data-ttu-id="773f0-109">Hogyan alkalmazza a zárolásokat</span><span class="sxs-lookup"><span data-stu-id="773f0-109">How locks are applied</span></span>

<span data-ttu-id="773f0-110">A szülő hatókörben zárolást alkalmazásakor hatókörön belüli összes erőforrás öröklik az azonos zárolása.</span><span class="sxs-lookup"><span data-stu-id="773f0-110">When you apply a lock at a parent scope, all resources within that scope inherit the same lock.</span></span> <span data-ttu-id="773f0-111">A későbbiekben még akkor is, erőforrások örökölt a zárolást.</span><span class="sxs-lookup"><span data-stu-id="773f0-111">Even resources you add later inherit the lock from the parent.</span></span> <span data-ttu-id="773f0-112">Az öröklés a legszigorúbb zár lép érvénybe.</span><span class="sxs-lookup"><span data-stu-id="773f0-112">The most restrictive lock in the inheritance takes precedence.</span></span>

<span data-ttu-id="773f0-113">Szerepköralapú hozzáférés-vezérlés, ellentétben a felügyeleti zárolás korlátozás alkalmazandó összes felhasználók és szerepkörök használhat.</span><span class="sxs-lookup"><span data-stu-id="773f0-113">Unlike role-based access control, you use management locks to apply a restriction across all users and roles.</span></span> <span data-ttu-id="773f0-114">A felhasználók és szerepkörök engedélyeinek beállításával kapcsolatos további tudnivalókért lásd: [Azure szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="773f0-114">To learn about setting permissions for users and roles, see [Azure Role-based Access Control](../active-directory/role-based-access-control-configure.md).</span></span>

<span data-ttu-id="773f0-115">Csak a fordul elő a felügyeleti vezérlősík, amely küldött műveletek műveletek érvényes erőforrás-kezelő zárolások `https://management.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="773f0-115">Resource Manager locks apply only to operations that happen in the management plane, which consists of operations sent to `https://management.azure.com`.</span></span> <span data-ttu-id="773f0-116">A zárolásokat korlátozza, hogy milyen erőforrások feladatokat a saját.</span><span class="sxs-lookup"><span data-stu-id="773f0-116">The locks do not restrict how resources perform their own functions.</span></span> <span data-ttu-id="773f0-117">Erőforrás módosítások korlátozott, de erőforrás műveletek nem korlátozottak.</span><span class="sxs-lookup"><span data-stu-id="773f0-117">Resource changes are restricted, but resource operations are not restricted.</span></span> <span data-ttu-id="773f0-118">Például egy SQL-adatbázis csak olvasható zárolást megakadályozza az törölhessék vagy módosíthassák az adatbázist, de nem akadályozza meg a létrehozása, frissítése vagy törlése az adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="773f0-118">For example, a ReadOnly lock on a SQL Database prevents you from deleting or modifying the database, but it does not prevent you from creating, updating, or deleting data in the database.</span></span> <span data-ttu-id="773f0-119">Adatok tranzakciója engedélyezettek, mert a rendszer nem küldi el a műveletek `https://management.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="773f0-119">Data transactions are permitted because those operations are not sent to `https://management.azure.com`.</span></span>

<span data-ttu-id="773f0-120">Alkalmazása **ReadOnly** váratlan eredményekhez vezethet, mivel az egyes műveletek, amelyek az adatok, például olvasási műveletek ténylegesen szükséges további műveleteket.</span><span class="sxs-lookup"><span data-stu-id="773f0-120">Applying **ReadOnly** can lead to unexpected results because some operations that seem like read operations actually require additional actions.</span></span> <span data-ttu-id="773f0-121">Például, ha kialakít egy **ReadOnly** zárolását egy tárfiókot minden felhasználót megakadályoz a kulcsainak listázása.</span><span class="sxs-lookup"><span data-stu-id="773f0-121">For example, placing a **ReadOnly** lock on a storage account prevents all users from listing the keys.</span></span> <span data-ttu-id="773f0-122">A listában kulcsok művelet segítségével egy POST kérést kezel, mert a visszaadott kulcsok érhetők el az írási műveletek.</span><span class="sxs-lookup"><span data-stu-id="773f0-122">The list keys operation is handled through a POST request because the returned keys are available for write operations.</span></span> <span data-ttu-id="773f0-123">Például ha kialakít egy **csak olvasható** egy App Service erőforrás zárolását megakadályozza, hogy a Visual Studio Server Explorer megjelenítése az erőforrás fájlok, mert adott interakció írási hozzáféréssel kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="773f0-123">For another example, placing a **ReadOnly** lock on an App Service resource prevents Visual Studio Server Explorer from displaying files for the resource because that interaction requires write access.</span></span>

## <a name="who-can-create-or-delete-locks-in-your-organization"></a><span data-ttu-id="773f0-124">Aki létrehozhat vagy törölhet zárolásokat a szervezetében</span><span class="sxs-lookup"><span data-stu-id="773f0-124">Who can create or delete locks in your organization</span></span>
<span data-ttu-id="773f0-125">Hozzon létre, vagy törölje a felügyeleti zárolás, hozzáféréssel kell rendelkeznie `Microsoft.Authorization/*` vagy `Microsoft.Authorization/locks/*` műveletek.</span><span class="sxs-lookup"><span data-stu-id="773f0-125">To create or delete management locks, you must have access to `Microsoft.Authorization/*` or `Microsoft.Authorization/locks/*` actions.</span></span> <span data-ttu-id="773f0-126">A beépített szerepkörök, csak **tulajdonos** és **felhasználói hozzáférés adminisztrátora** kapnak a csatolási műveleteket.</span><span class="sxs-lookup"><span data-stu-id="773f0-126">Of the built-in roles, only **Owner** and **User Access Administrator** are granted those actions.</span></span>

## <a name="portal"></a><span data-ttu-id="773f0-127">Portál</span><span class="sxs-lookup"><span data-stu-id="773f0-127">Portal</span></span>
[!INCLUDE [resource-manager-lock-resources](../../includes/resource-manager-lock-resources.md)]

## <a name="template"></a><span data-ttu-id="773f0-128">Sablon</span><span class="sxs-lookup"><span data-stu-id="773f0-128">Template</span></span>
<span data-ttu-id="773f0-129">A következő példa bemutatja a sablont, amely a zárolási létrehoz egy tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="773f0-129">The following example shows a template that creates a lock on a storage account.</span></span> <span data-ttu-id="773f0-130">A storage-fiók alkalmazásának a zárolás valósul meg paraméterként.</span><span class="sxs-lookup"><span data-stu-id="773f0-130">The storage account on which to apply the lock is provided as a parameter.</span></span> <span data-ttu-id="773f0-131">A zárolás nevét hozza létre az erőforrásnév hozzáfűzésével **/Microsoft.Authorization/** és a nevét, a zárolás, ebben az esetben **myLock**.</span><span class="sxs-lookup"><span data-stu-id="773f0-131">The name of the lock is created by concatenating the resource name with **/Microsoft.Authorization/** and the name of the lock, in this case **myLock**.</span></span>

<span data-ttu-id="773f0-132">A megadott típus jellemző az erőforrás típusát.</span><span class="sxs-lookup"><span data-stu-id="773f0-132">The type provided is specific to the resource type.</span></span> <span data-ttu-id="773f0-133">A tároláshoz állítsa be az "Microsoft.Storage/storageaccounts/providers/locks" típust.</span><span class="sxs-lookup"><span data-stu-id="773f0-133">For storage, set the type to "Microsoft.Storage/storageaccounts/providers/locks".</span></span>

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "lockedResource": {
          "type": "string"
        }
      },
      "resources": [
        {
          "name": "[concat(parameters('lockedResource'), '/Microsoft.Authorization/myLock')]",
          "type": "Microsoft.Storage/storageAccounts/providers/locks",
          "apiVersion": "2015-01-01",
          "properties": {
            "level": "CannotDelete"
          }
        }
      ]
    }

## <a name="powershell"></a><span data-ttu-id="773f0-134">PowerShell</span><span class="sxs-lookup"><span data-stu-id="773f0-134">PowerShell</span></span>
<span data-ttu-id="773f0-135">Ön zárolása az erőforrások az Azure PowerShell használatával központilag telepítenek a [New-AzureRmResourceLock](/powershell/module/azurerm.resources/new-azurermresourcelock) parancsot.</span><span class="sxs-lookup"><span data-stu-id="773f0-135">You lock deployed resources with Azure PowerShell by using the [New-AzureRmResourceLock](/powershell/module/azurerm.resources/new-azurermresourcelock) command.</span></span>

<span data-ttu-id="773f0-136">Egy erőforrás zárolását, adja meg az erőforrást, az erőforrás típusa és az erőforráscsoport neve nevét.</span><span class="sxs-lookup"><span data-stu-id="773f0-136">To lock a resource, provide the name of the resource, its resource type, and its resource group name.</span></span>

```powershell
New-AzureRmResourceLock -LockLevel CanNotDelete -LockName LockSite `
  -ResourceName examplesite -ResourceType Microsoft.Web/sites `
  -ResourceGroupName exampleresourcegroup
```

<span data-ttu-id="773f0-137">Erőforráscsoport zárolásához adja meg az erőforráscsoport nevét.</span><span class="sxs-lookup"><span data-stu-id="773f0-137">To lock a resource group, provide the name of the resource group.</span></span>

```powershell
New-AzureRmResourceLock -LockName LockGroup -LockLevel CanNotDelete `
  -ResourceGroupName exampleresourcegroup
```

<span data-ttu-id="773f0-138">A zárolási információ használatához [Get-AzureRmResourceLock](/powershell/module/azurerm.resources/get-azurermresourcelock).</span><span class="sxs-lookup"><span data-stu-id="773f0-138">To get information about a lock, use [Get-AzureRmResourceLock](/powershell/module/azurerm.resources/get-azurermresourcelock).</span></span> <span data-ttu-id="773f0-139">Az előfizetés összes zárolás beszerzéséhez használja:</span><span class="sxs-lookup"><span data-stu-id="773f0-139">To get all the locks in your subscription, use:</span></span>

```powershell
Get-AzureRmResourceLock
```

<span data-ttu-id="773f0-140">Az összes zárolás erőforrás, amelyet:</span><span class="sxs-lookup"><span data-stu-id="773f0-140">To get all locks for a resource, use:</span></span>

```powershell
Get-AzureRmResourceLock -ResourceName examplesite -ResourceType Microsoft.Web/sites `
  -ResourceGroupName exampleresourcegroup
```

<span data-ttu-id="773f0-141">Az összes zárolás erőforráscsoport megtekintéséhez használja:</span><span class="sxs-lookup"><span data-stu-id="773f0-141">To get all locks for a resource group, use:</span></span>

```powershell
Get-AzureRmResourceLock -ResourceGroupName exampleresourcegroup
```

<span data-ttu-id="773f0-142">Az Azure PowerShell biztosít a más parancsok működő zárolásokat, például a [Set-AzureRmResourceLock](/powershell/module/azurerm.resources/set-azurermresourcelock) zárolást, frissítése és [Remove-AzureRmResourceLock](/powershell/module/azurerm.resources/remove-azurermresourcelock) zárolást törlése.</span><span class="sxs-lookup"><span data-stu-id="773f0-142">Azure PowerShell provides other commands for working locks, such as [Set-AzureRmResourceLock](/powershell/module/azurerm.resources/set-azurermresourcelock) to update a lock, and [Remove-AzureRmResourceLock](/powershell/module/azurerm.resources/remove-azurermresourcelock) to delete a lock.</span></span>

## <a name="azure-cli"></a><span data-ttu-id="773f0-143">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="773f0-143">Azure CLI</span></span>

<span data-ttu-id="773f0-144">Meg zárolási az erőforrások az Azure parancssori felület használatával központilag telepítenek a [az zárolási létrehozása](/cli/azure/lock#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="773f0-144">You lock deployed resources with Azure CLI by using the [az lock create](/cli/azure/lock#create) command.</span></span>

<span data-ttu-id="773f0-145">Egy erőforrás zárolását, adja meg az erőforrást, az erőforrás típusa és az erőforráscsoport neve nevét.</span><span class="sxs-lookup"><span data-stu-id="773f0-145">To lock a resource, provide the name of the resource, its resource type, and its resource group name.</span></span>

```azurecli
az lock create --name LockSite --lock-type CanNotDelete \
  --resource-group exampleresourcegroup --resource-name examplesite \
  --resource-type Microsoft.Web/sites
```

<span data-ttu-id="773f0-146">Erőforráscsoport zárolásához adja meg az erőforráscsoport nevét.</span><span class="sxs-lookup"><span data-stu-id="773f0-146">To lock a resource group, provide the name of the resource group.</span></span>

```azurecli
az lock create --name LockGroup --lock-type CanNotDelete \
  --resource-group exampleresourcegroup
```

<span data-ttu-id="773f0-147">A zárolási információ használatához [az zárolási lista](/cli/azure/lock#list).</span><span class="sxs-lookup"><span data-stu-id="773f0-147">To get information about a lock, use [az lock list](/cli/azure/lock#list).</span></span> <span data-ttu-id="773f0-148">Az előfizetés összes zárolás beszerzéséhez használja:</span><span class="sxs-lookup"><span data-stu-id="773f0-148">To get all the locks in your subscription, use:</span></span>

```azurecli
az lock list
```

<span data-ttu-id="773f0-149">Az összes zárolás erőforrás, amelyet:</span><span class="sxs-lookup"><span data-stu-id="773f0-149">To get all locks for a resource, use:</span></span>

```azurecli
az lock list --resource-group exampleresourcegroup --resource-name examplesite \
  --namespace Microsoft.Web --resource-type sites --parent ""
```

<span data-ttu-id="773f0-150">Az összes zárolás erőforráscsoport megtekintéséhez használja:</span><span class="sxs-lookup"><span data-stu-id="773f0-150">To get all locks for a resource group, use:</span></span>

```azurecli
az lock list --resource-group exampleresourcegroup
```

<span data-ttu-id="773f0-151">Az Azure CLI biztosít a más parancsok működő zárolásokat, például a [az zárolási frissítés](/cli/azure/lock#update) frissíteni a zárolást, és [az zárolási törlése](/cli/azure/lock#delete) zárolást törlése.</span><span class="sxs-lookup"><span data-stu-id="773f0-151">Azure CLI provides other commands for working locks, such as [az lock update](/cli/azure/lock#update) to update a lock, and [az lock delete](/cli/azure/lock#delete) to delete a lock.</span></span>

## <a name="rest-api"></a><span data-ttu-id="773f0-152">REST API</span><span class="sxs-lookup"><span data-stu-id="773f0-152">REST API</span></span>
<span data-ttu-id="773f0-153">A telepített erőforrások zárolhatja a [REST API-t a felügyeleti zárolás](https://docs.microsoft.com/rest/api/resources/managementlocks).</span><span class="sxs-lookup"><span data-stu-id="773f0-153">You can lock deployed resources with the [REST API for management locks](https://docs.microsoft.com/rest/api/resources/managementlocks).</span></span> <span data-ttu-id="773f0-154">A REST API lehetővé teszi létrehozása és törlése a zárolások és meglévő zárolások vonatkozó információk lekéréséhez.</span><span class="sxs-lookup"><span data-stu-id="773f0-154">The REST API enables you to create and delete locks, and retrieve information about existing locks.</span></span>

<span data-ttu-id="773f0-155">A zárolási létrehozásához futtassa:</span><span class="sxs-lookup"><span data-stu-id="773f0-155">To create a lock, run:</span></span>

    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/locks/{lock-name}?api-version={api-version}

<span data-ttu-id="773f0-156">A hatókör lehet egy előfizetés, az erőforráscsoportot, vagy az erőforrás.</span><span class="sxs-lookup"><span data-stu-id="773f0-156">The scope could be a subscription, resource group, or resource.</span></span> <span data-ttu-id="773f0-157">A zárolás nevét, a zárolás hívni kívánt függetlenül.</span><span class="sxs-lookup"><span data-stu-id="773f0-157">The lock-name is whatever you want to call the lock.</span></span> <span data-ttu-id="773f0-158">Api-version, használjon **2015-01-01**.</span><span class="sxs-lookup"><span data-stu-id="773f0-158">For api-version, use **2015-01-01**.</span></span>

<span data-ttu-id="773f0-159">A kérelemben egy JSON-objektum, amely meghatározza a zárolás tulajdonságait tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="773f0-159">In the request, include a JSON object that specifies the properties for the lock.</span></span>

    {
      "properties": {
        "level": "CanNotDelete",
        "notes": "Optional text notes."
      }
    } 

## <a name="next-steps"></a><span data-ttu-id="773f0-160">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="773f0-160">Next steps</span></span>
* <span data-ttu-id="773f0-161">Működő erőforrás zárral kapcsolatos további információkért lásd: [zár le az Azure-erőforrások](http://blogs.msdn.com/b/cloud_solution_architect/archive/2015/06/18/lock-down-your-azure-resources.aspx)</span><span class="sxs-lookup"><span data-stu-id="773f0-161">For more information about working with resource locks, see [Lock Down Your Azure Resources](http://blogs.msdn.com/b/cloud_solution_architect/archive/2015/06/18/lock-down-your-azure-resources.aspx)</span></span>
* <span data-ttu-id="773f0-162">Az erőforrások logikailag rendszerezéséhez, lásd: [az erőforrások rendszerezése címkék használatával](resource-group-using-tags.md)</span><span class="sxs-lookup"><span data-stu-id="773f0-162">To learn about logically organizing your resources, see [Using tags to organize your resources](resource-group-using-tags.md)</span></span>
* <span data-ttu-id="773f0-163">Mely erőforrás található erőforráscsoport módosításához lásd [erőforrások áthelyezése új erőforráscsoportba](resource-group-move-resources.md)</span><span class="sxs-lookup"><span data-stu-id="773f0-163">To change which resource group a resource resides in, see [Move resources to new resource group](resource-group-move-resources.md)</span></span>
* <span data-ttu-id="773f0-164">Az előfizetés testreszabott házirendekkel korlátozások és egyezmények alkalmazhat.</span><span class="sxs-lookup"><span data-stu-id="773f0-164">You can apply restrictions and conventions across your subscription with customized policies.</span></span> <span data-ttu-id="773f0-165">További információ: [Erőforrások kezelése és hozzáférés szabályozása házirendekkel](resource-manager-policy.md).</span><span class="sxs-lookup"><span data-stu-id="773f0-165">For more information, see [Use Policy to manage resources and control access](resource-manager-policy.md).</span></span>
* <span data-ttu-id="773f0-166">Nagyvállalatoknak az [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md) (Azure nagyvállalati struktúra - előíró előfizetés-irányítás) című cikk nyújt útmutatást az előfizetéseknek a Resource Managerrel való hatékony kezeléséről.</span><span class="sxs-lookup"><span data-stu-id="773f0-166">For guidance on how enterprises can use Resource Manager to effectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

