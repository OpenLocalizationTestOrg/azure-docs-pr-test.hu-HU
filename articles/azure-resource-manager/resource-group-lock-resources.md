---
title: "aaaLock Azure-erőforrások tooprevent módosítások |} Microsoft Docs"
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
ms.openlocfilehash: 1f0d8911b2b129069bd2f01a9a4ec0a3cf700118
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="lock-resources-tooprevent-unexpected-changes"></a><span data-ttu-id="43c64-103">Erőforrások tooprevent zárolása váratlan változások</span><span class="sxs-lookup"><span data-stu-id="43c64-103">Lock resources tooprevent unexpected changes</span></span> 
<span data-ttu-id="43c64-104">Ha Ön rendszergazda szükség lehet toolock előfizetés, erőforrás vagy az erőforrás tooprevent más felhasználók véletlen törlése vagy a kritikus erőforrásokat módosítása a szervezetében.</span><span class="sxs-lookup"><span data-stu-id="43c64-104">As an administrator, you may need toolock a subscription, resource group, or resource tooprevent other users in your organization from accidentally deleting or modifying critical resources.</span></span> <span data-ttu-id="43c64-105">Beállíthatja azt hello zárolási szint túl**CanNotDelete** vagy **ReadOnly**.</span><span class="sxs-lookup"><span data-stu-id="43c64-105">You can set hello lock level too**CanNotDelete** or **ReadOnly**.</span></span> 

* <span data-ttu-id="43c64-106">**CanNotDelete** azt jelenti, hogy a jogosult felhasználók továbbra is olvasni és módosítani az erőforrást, de azokat nem lehet törölni a hello erőforrás.</span><span class="sxs-lookup"><span data-stu-id="43c64-106">**CanNotDelete** means authorized users can still read and modify a resource, but they can't delete hello resource.</span></span> 
* <span data-ttu-id="43c64-107">**Csak olvasható** azt jelenti, hogy a jogosult felhasználók olvashatják egy erőforrást, de nem lehet törölni vagy hello erőforrás frissítése.</span><span class="sxs-lookup"><span data-stu-id="43c64-107">**ReadOnly** means authorized users can read a resource, but they can't delete or update hello resource.</span></span> <span data-ttu-id="43c64-108">A zárolás alkalmazása az összes jogosult felhasználók toohello engedélyekre hello hasonló toorestricting **olvasó** szerepkör.</span><span class="sxs-lookup"><span data-stu-id="43c64-108">Applying this lock is similar toorestricting all authorized users toohello permissions granted by hello **Reader** role.</span></span> 

## <a name="how-locks-are-applied"></a><span data-ttu-id="43c64-109">Hogyan alkalmazza a zárolásokat</span><span class="sxs-lookup"><span data-stu-id="43c64-109">How locks are applied</span></span>

<span data-ttu-id="43c64-110">A szülő hatókörben zárolást alkalmazásakor hatókörön belüli összes erőforrás öröklése hello azonos zárolása.</span><span class="sxs-lookup"><span data-stu-id="43c64-110">When you apply a lock at a parent scope, all resources within that scope inherit hello same lock.</span></span> <span data-ttu-id="43c64-111">Később fel még akkor is, erőforrások örökölt hello zárolási hello.</span><span class="sxs-lookup"><span data-stu-id="43c64-111">Even resources you add later inherit hello lock from hello parent.</span></span> <span data-ttu-id="43c64-112">hello szigorúbb zárolást hello öröklési lép érvénybe.</span><span class="sxs-lookup"><span data-stu-id="43c64-112">hello most restrictive lock in hello inheritance takes precedence.</span></span>

<span data-ttu-id="43c64-113">Szerepköralapú hozzáférés-vezérlés, ellentétben a felügyeleti zárolás tooapply korlátozás összes felhasználók és szerepkörök használhat.</span><span class="sxs-lookup"><span data-stu-id="43c64-113">Unlike role-based access control, you use management locks tooapply a restriction across all users and roles.</span></span> <span data-ttu-id="43c64-114">toolearn vonatkozó engedélyek beállítása a felhasználók és szerepkörök, lásd: [Azure szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="43c64-114">toolearn about setting permissions for users and roles, see [Azure Role-based Access Control](../active-directory/role-based-access-control-configure.md).</span></span>

<span data-ttu-id="43c64-115">Erőforrás-kezelő zárolások alkalmazása csak a fordul elő hello felügyeleti vezérlősík, amely túl elküldött műveletek toooperations`https://management.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="43c64-115">Resource Manager locks apply only toooperations that happen in hello management plane, which consists of operations sent too`https://management.azure.com`.</span></span> <span data-ttu-id="43c64-116">Hogyan erőforrások feladatokat a saját hello zárolások feloldása nem korlátozzák.</span><span class="sxs-lookup"><span data-stu-id="43c64-116">hello locks do not restrict how resources perform their own functions.</span></span> <span data-ttu-id="43c64-117">Erőforrás módosítások korlátozott, de erőforrás műveletek nem korlátozottak.</span><span class="sxs-lookup"><span data-stu-id="43c64-117">Resource changes are restricted, but resource operations are not restricted.</span></span> <span data-ttu-id="43c64-118">Például egy SQL-adatbázis csak olvasható zárolást megakadályozza, hogy törli vagy módosítása hello adatbázis, de nem akadályozza meg a létrehozása, frissítése vagy törlése hello adatbázis adatait.</span><span class="sxs-lookup"><span data-stu-id="43c64-118">For example, a ReadOnly lock on a SQL Database prevents you from deleting or modifying hello database, but it does not prevent you from creating, updating, or deleting data in hello database.</span></span> <span data-ttu-id="43c64-119">Adatok tranzakciója engedélyezettek, mert ezek a műveletek nem kerülnek túl`https://management.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="43c64-119">Data transactions are permitted because those operations are not sent too`https://management.azure.com`.</span></span>

<span data-ttu-id="43c64-120">Alkalmazása **ReadOnly** toounexpected eredmények vezethet, mivel az egyes műveletek, amelyek az adatok, például olvasási műveletek ténylegesen szükséges további műveleteket.</span><span class="sxs-lookup"><span data-stu-id="43c64-120">Applying **ReadOnly** can lead toounexpected results because some operations that seem like read operations actually require additional actions.</span></span> <span data-ttu-id="43c64-121">Például, ha kialakít egy **ReadOnly** zárolását egy tárfiókot minden felhasználót megakadályoz hello kulcsainak listázása.</span><span class="sxs-lookup"><span data-stu-id="43c64-121">For example, placing a **ReadOnly** lock on a storage account prevents all users from listing hello keys.</span></span> <span data-ttu-id="43c64-122">hello listában kulcsok művelet segítségével egy POST kérést kezel, mert hello vissza kulcsok elérhetők az írási műveletek.</span><span class="sxs-lookup"><span data-stu-id="43c64-122">hello list keys operation is handled through a POST request because hello returned keys are available for write operations.</span></span> <span data-ttu-id="43c64-123">Például ha helyez el egy **csak olvasható** egy App Service erőforrás zárolását megakadályozza, hogy a Visual Studio Server Explorer hello erőforrás fájl jelenik meg, mert az adott interakció írási hozzáféréssel kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="43c64-123">For another example, placing a **ReadOnly** lock on an App Service resource prevents Visual Studio Server Explorer from displaying files for hello resource because that interaction requires write access.</span></span>

## <a name="who-can-create-or-delete-locks-in-your-organization"></a><span data-ttu-id="43c64-124">Aki létrehozhat vagy törölhet zárolásokat a szervezetében</span><span class="sxs-lookup"><span data-stu-id="43c64-124">Who can create or delete locks in your organization</span></span>
<span data-ttu-id="43c64-125">toocreate vagy delete felügyeleti zárolás, hozzá kell férnie túl`Microsoft.Authorization/*` vagy `Microsoft.Authorization/locks/*` műveletek.</span><span class="sxs-lookup"><span data-stu-id="43c64-125">toocreate or delete management locks, you must have access too`Microsoft.Authorization/*` or `Microsoft.Authorization/locks/*` actions.</span></span> <span data-ttu-id="43c64-126">Hello beépített szerepkörök, csak **tulajdonos** és **felhasználói hozzáférés adminisztrátora** kapnak a csatolási műveleteket.</span><span class="sxs-lookup"><span data-stu-id="43c64-126">Of hello built-in roles, only **Owner** and **User Access Administrator** are granted those actions.</span></span>

## <a name="portal"></a><span data-ttu-id="43c64-127">Portál</span><span class="sxs-lookup"><span data-stu-id="43c64-127">Portal</span></span>
[!INCLUDE [resource-manager-lock-resources](../../includes/resource-manager-lock-resources.md)]

## <a name="template"></a><span data-ttu-id="43c64-128">Sablon</span><span class="sxs-lookup"><span data-stu-id="43c64-128">Template</span></span>
<span data-ttu-id="43c64-129">hello következő példa bemutatja a sablont, amely a zárolási létrehoz egy tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="43c64-129">hello following example shows a template that creates a lock on a storage account.</span></span> <span data-ttu-id="43c64-130">a mely tooapply hello zárolási paraméterként megadott hello tárfiók.</span><span class="sxs-lookup"><span data-stu-id="43c64-130">hello storage account on which tooapply hello lock is provided as a parameter.</span></span> <span data-ttu-id="43c64-131">hello hello zárolás neve által létrehozott hello erőforrás nevét hozzáfűzésével **/Microsoft.Authorization/** és hello zárolás név ebben az esetben hello **myLock**.</span><span class="sxs-lookup"><span data-stu-id="43c64-131">hello name of hello lock is created by concatenating hello resource name with **/Microsoft.Authorization/** and hello name of hello lock, in this case **myLock**.</span></span>

<span data-ttu-id="43c64-132">hello megadott típus adott toohello erőforrástípus.</span><span class="sxs-lookup"><span data-stu-id="43c64-132">hello type provided is specific toohello resource type.</span></span> <span data-ttu-id="43c64-133">A tároláshoz beállítása hello típus too"Microsoft.Storage/storageaccounts/providers/locks".</span><span class="sxs-lookup"><span data-stu-id="43c64-133">For storage, set hello type too"Microsoft.Storage/storageaccounts/providers/locks".</span></span>

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

## <a name="powershell"></a><span data-ttu-id="43c64-134">PowerShell</span><span class="sxs-lookup"><span data-stu-id="43c64-134">PowerShell</span></span>
<span data-ttu-id="43c64-135">Akkor zárolási lett telepítve az Azure PowerShell erőforrások hello [New-AzureRmResourceLock](/powershell/module/azurerm.resources/new-azurermresourcelock) parancsot.</span><span class="sxs-lookup"><span data-stu-id="43c64-135">You lock deployed resources with Azure PowerShell by using hello [New-AzureRmResourceLock](/powershell/module/azurerm.resources/new-azurermresourcelock) command.</span></span>

<span data-ttu-id="43c64-136">toolock egy erőforrást, adja meg a hello hello erőforrás nevét, az erőforrás típusa és az erőforráscsoport neve.</span><span class="sxs-lookup"><span data-stu-id="43c64-136">toolock a resource, provide hello name of hello resource, its resource type, and its resource group name.</span></span>

```powershell
New-AzureRmResourceLock -LockLevel CanNotDelete -LockName LockSite `
  -ResourceName examplesite -ResourceType Microsoft.Web/sites `
  -ResourceGroupName exampleresourcegroup
```

<span data-ttu-id="43c64-137">toolock egy erőforráscsoport, hello hello erőforráscsoport nevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="43c64-137">toolock a resource group, provide hello name of hello resource group.</span></span>

```powershell
New-AzureRmResourceLock -LockName LockGroup -LockLevel CanNotDelete `
  -ResourceGroupName exampleresourcegroup
```

<span data-ttu-id="43c64-138">a zárolási, használjon tooget információt [Get-AzureRmResourceLock](/powershell/module/azurerm.resources/get-azurermresourcelock).</span><span class="sxs-lookup"><span data-stu-id="43c64-138">tooget information about a lock, use [Get-AzureRmResourceLock](/powershell/module/azurerm.resources/get-azurermresourcelock).</span></span> <span data-ttu-id="43c64-139">tooget az előfizetésében szereplő összes hello zárolás használja:</span><span class="sxs-lookup"><span data-stu-id="43c64-139">tooget all hello locks in your subscription, use:</span></span>

```powershell
Get-AzureRmResourceLock
```

<span data-ttu-id="43c64-140">tooget összes zárolás erőforrás esetén használja:</span><span class="sxs-lookup"><span data-stu-id="43c64-140">tooget all locks for a resource, use:</span></span>

```powershell
Get-AzureRmResourceLock -ResourceName examplesite -ResourceType Microsoft.Web/sites `
  -ResourceGroupName exampleresourcegroup
```

<span data-ttu-id="43c64-141">tooget erőforráscsoport, az összes zárolás használja:</span><span class="sxs-lookup"><span data-stu-id="43c64-141">tooget all locks for a resource group, use:</span></span>

```powershell
Get-AzureRmResourceLock -ResourceGroupName exampleresourcegroup
```

<span data-ttu-id="43c64-142">Az Azure PowerShell biztosít a más parancsok működő zárolásokat, például a [Set-AzureRmResourceLock](/powershell/module/azurerm.resources/set-azurermresourcelock) tooupdate zárolást, és [Remove-AzureRmResourceLock](/powershell/module/azurerm.resources/remove-azurermresourcelock) toodelete zárolást.</span><span class="sxs-lookup"><span data-stu-id="43c64-142">Azure PowerShell provides other commands for working locks, such as [Set-AzureRmResourceLock](/powershell/module/azurerm.resources/set-azurermresourcelock) tooupdate a lock, and [Remove-AzureRmResourceLock](/powershell/module/azurerm.resources/remove-azurermresourcelock) toodelete a lock.</span></span>

## <a name="azure-cli"></a><span data-ttu-id="43c64-143">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="43c64-143">Azure CLI</span></span>

<span data-ttu-id="43c64-144">Ön zárolási lett telepítve az Azure parancssori Felülettel rendelkező erőforrásokat hello [az zárolási létrehozása](/cli/azure/lock#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="43c64-144">You lock deployed resources with Azure CLI by using hello [az lock create](/cli/azure/lock#create) command.</span></span>

<span data-ttu-id="43c64-145">toolock egy erőforrást, adja meg a hello hello erőforrás nevét, az erőforrás típusa és az erőforráscsoport neve.</span><span class="sxs-lookup"><span data-stu-id="43c64-145">toolock a resource, provide hello name of hello resource, its resource type, and its resource group name.</span></span>

```azurecli
az lock create --name LockSite --lock-type CanNotDelete \
  --resource-group exampleresourcegroup --resource-name examplesite \
  --resource-type Microsoft.Web/sites
```

<span data-ttu-id="43c64-146">toolock egy erőforráscsoport, hello hello erőforráscsoport nevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="43c64-146">toolock a resource group, provide hello name of hello resource group.</span></span>

```azurecli
az lock create --name LockGroup --lock-type CanNotDelete \
  --resource-group exampleresourcegroup
```

<span data-ttu-id="43c64-147">a zárolási, használjon tooget információt [az zárolási lista](/cli/azure/lock#list).</span><span class="sxs-lookup"><span data-stu-id="43c64-147">tooget information about a lock, use [az lock list](/cli/azure/lock#list).</span></span> <span data-ttu-id="43c64-148">tooget az előfizetésében szereplő összes hello zárolás használja:</span><span class="sxs-lookup"><span data-stu-id="43c64-148">tooget all hello locks in your subscription, use:</span></span>

```azurecli
az lock list
```

<span data-ttu-id="43c64-149">tooget összes zárolás erőforrás esetén használja:</span><span class="sxs-lookup"><span data-stu-id="43c64-149">tooget all locks for a resource, use:</span></span>

```azurecli
az lock list --resource-group exampleresourcegroup --resource-name examplesite \
  --namespace Microsoft.Web --resource-type sites --parent ""
```

<span data-ttu-id="43c64-150">tooget erőforráscsoport, az összes zárolás használja:</span><span class="sxs-lookup"><span data-stu-id="43c64-150">tooget all locks for a resource group, use:</span></span>

```azurecli
az lock list --resource-group exampleresourcegroup
```

<span data-ttu-id="43c64-151">Az Azure CLI biztosít a más parancsok működő zárolásokat, például a [az zárolási frissítés](/cli/azure/lock#update) tooupdate zárolást, és [az zárolási törlése](/cli/azure/lock#delete) toodelete zárolást.</span><span class="sxs-lookup"><span data-stu-id="43c64-151">Azure CLI provides other commands for working locks, such as [az lock update](/cli/azure/lock#update) tooupdate a lock, and [az lock delete](/cli/azure/lock#delete) toodelete a lock.</span></span>

## <a name="rest-api"></a><span data-ttu-id="43c64-152">REST API</span><span class="sxs-lookup"><span data-stu-id="43c64-152">REST API</span></span>
<span data-ttu-id="43c64-153">Zárolhatja hello telepített erőforrások [REST API-t a felügyeleti zárolás](https://docs.microsoft.com/rest/api/resources/managementlocks).</span><span class="sxs-lookup"><span data-stu-id="43c64-153">You can lock deployed resources with hello [REST API for management locks](https://docs.microsoft.com/rest/api/resources/managementlocks).</span></span> <span data-ttu-id="43c64-154">hello REST API lehetővé teszi a toocreate zárolásainak törlése és meglévő zárolások vonatkozó információk lekéréséhez.</span><span class="sxs-lookup"><span data-stu-id="43c64-154">hello REST API enables you toocreate and delete locks, and retrieve information about existing locks.</span></span>

<span data-ttu-id="43c64-155">a zárolási toocreate futtatása:</span><span class="sxs-lookup"><span data-stu-id="43c64-155">toocreate a lock, run:</span></span>

    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/locks/{lock-name}?api-version={api-version}

<span data-ttu-id="43c64-156">hello hatókör lehet egy előfizetés, az erőforráscsoportot, vagy az erőforrás.</span><span class="sxs-lookup"><span data-stu-id="43c64-156">hello scope could be a subscription, resource group, or resource.</span></span> <span data-ttu-id="43c64-157">hello zárolási-név bármilyen kívánt toocall hello zárolása.</span><span class="sxs-lookup"><span data-stu-id="43c64-157">hello lock-name is whatever you want toocall hello lock.</span></span> <span data-ttu-id="43c64-158">Api-version, használjon **2015-01-01**.</span><span class="sxs-lookup"><span data-stu-id="43c64-158">For api-version, use **2015-01-01**.</span></span>

<span data-ttu-id="43c64-159">Hello kérelem egy JSON-objektum, amely meghatározza a hello zárolási hello tulajdonságait tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="43c64-159">In hello request, include a JSON object that specifies hello properties for hello lock.</span></span>

    {
      "properties": {
        "level": "CanNotDelete",
        "notes": "Optional text notes."
      }
    } 

## <a name="next-steps"></a><span data-ttu-id="43c64-160">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="43c64-160">Next steps</span></span>
* <span data-ttu-id="43c64-161">Működő erőforrás zárral kapcsolatos további információkért lásd: [zár le az Azure-erőforrások](http://blogs.msdn.com/b/cloud_solution_architect/archive/2015/06/18/lock-down-your-azure-resources.aspx)</span><span class="sxs-lookup"><span data-stu-id="43c64-161">For more information about working with resource locks, see [Lock Down Your Azure Resources](http://blogs.msdn.com/b/cloud_solution_architect/archive/2015/06/18/lock-down-your-azure-resources.aspx)</span></span>
* <span data-ttu-id="43c64-162">az erőforrások logikailag szervezése toolearn lásd [Using címkéket tooorganize az erőforrások](resource-group-using-tags.md)</span><span class="sxs-lookup"><span data-stu-id="43c64-162">toolearn about logically organizing your resources, see [Using tags tooorganize your resources](resource-group-using-tags.md)</span></span>
* <span data-ttu-id="43c64-163">toochange, melyik erőforráscsoport erőforrás fájlcsoportban helyezkedik el, lásd: [erőforrások toonew erőforrás csoport áthelyezése](resource-group-move-resources.md)</span><span class="sxs-lookup"><span data-stu-id="43c64-163">toochange which resource group a resource resides in, see [Move resources toonew resource group](resource-group-move-resources.md)</span></span>
* <span data-ttu-id="43c64-164">Az előfizetés testreszabott házirendekkel korlátozások és egyezmények alkalmazhat.</span><span class="sxs-lookup"><span data-stu-id="43c64-164">You can apply restrictions and conventions across your subscription with customized policies.</span></span> <span data-ttu-id="43c64-165">További információkért lásd: [vezérlési hozzáférési és használati feltételei toomanage erőforrások](resource-manager-policy.md).</span><span class="sxs-lookup"><span data-stu-id="43c64-165">For more information, see [Use Policy toomanage resources and control access](resource-manager-policy.md).</span></span>
* <span data-ttu-id="43c64-166">A vállalatok használatát erőforrás-kezelő tooeffectively segítségükkel előfizetések kezelése című [Azure enterprise scaffold - előíró előfizetés irányítás](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="43c64-166">For guidance on how enterprises can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

