---
title: "Hozzon létre egyéni szerepkörök az Azure RBAC |} Microsoft Docs"
description: "Megtudhatja, hogyan pontosabb identitáskezeléshez átruházásához hozzáférés-vezérléssel egyéni szerepkörök definiálása az Azure-előfizetésben."
services: active-directory
documentationcenter: 
author: andredm7
manager: femila
ms.assetid: e4206ea9-52c3-47ee-af29-f6eef7566fa5
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/11/2017
ms.author: andredm
ms.reviewer: rqureshi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8e72f2c8095d13c4b6df3c6576bd58806a3c0f2f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="create-custom-roles-for-azure-role-based-access-control"></a><span data-ttu-id="ecfb4-103">Hozzon létre egyéni szerepkörök átruházásához hozzáférés-vezérlés</span><span class="sxs-lookup"><span data-stu-id="ecfb4-103">Create custom roles for Azure Role-Based Access Control</span></span>
<span data-ttu-id="ecfb4-104">Hozzon létre egy egyéni biztonsági szerepkört a átruházásához hozzáférés-vezérlés (RBAC) Ha az adott hozzáférési igényeinek a beépített szerepkörök egyike.</span><span class="sxs-lookup"><span data-stu-id="ecfb4-104">Create a custom role in Azure Role-Based Access Control (RBAC) if none of the built-in roles meet your specific access needs.</span></span> <span data-ttu-id="ecfb4-105">Egyéni szerepkörök segítségével hozhatók létre [Azure PowerShell](role-based-access-control-manage-access-powershell.md), [Azure parancssori felület](role-based-access-control-manage-access-azure-cli.md) (CLI), és a [REST API](role-based-access-control-manage-access-rest.md).</span><span class="sxs-lookup"><span data-stu-id="ecfb4-105">Custom roles can be created using [Azure PowerShell](role-based-access-control-manage-access-powershell.md), [Azure Command-Line Interface](role-based-access-control-manage-access-azure-cli.md) (CLI), and the [REST API](role-based-access-control-manage-access-rest.md).</span></span> <span data-ttu-id="ecfb4-106">Hasonlóan a beépített szerepkörök egyéni szerepkörök hozzárendelése felhasználók, csoportok és alkalmazások előfizetés, erőforráscsoport és erőforrás-hatókörök.</span><span class="sxs-lookup"><span data-stu-id="ecfb4-106">Just like built-in roles, you can assign custom roles to users, groups, and applications at subscription, resource group, and resource scopes.</span></span> <span data-ttu-id="ecfb4-107">Egyéni szerepkörök az Azure AD-bérlő tárolódnak, és előfizetések között megosztható legyen.</span><span class="sxs-lookup"><span data-stu-id="ecfb4-107">Custom roles are stored in an Azure AD tenant and can be shared across subscriptions.</span></span>

<span data-ttu-id="ecfb4-108">Mindegyik bérlő legfeljebb 2000 egyéni szerepköröket hozhatnak létre.</span><span class="sxs-lookup"><span data-stu-id="ecfb4-108">Each tenant can create up to 2000 custom roles.</span></span> 

<span data-ttu-id="ecfb4-109">A következő példa bemutatja egy egyéni biztonsági szerepkört a figyelés és a virtuális gépek újraindításával:</span><span class="sxs-lookup"><span data-stu-id="ecfb4-109">The following example shows a custom role for monitoring and restarting virtual machines:</span></span>

```
{
  "Name": "Virtual Machine Operator",
  "Id": "cadb4a5a-4e7a-47be-84db-05cad13b6769",
  "IsCustom": true,
  "Description": "Can monitor and restart virtual machines.",
  "Actions": [
    "Microsoft.Storage/*/read",
    "Microsoft.Network/*/read",
    "Microsoft.Compute/*/read",
    "Microsoft.Compute/virtualMachines/start/action",
    "Microsoft.Compute/virtualMachines/restart/action",
    "Microsoft.Authorization/*/read",
    "Microsoft.Resources/subscriptions/resourceGroups/read",
    "Microsoft.Insights/alertRules/*",
    "Microsoft.Insights/diagnosticSettings/*",
    "Microsoft.Support/*"
  ],
  "NotActions": [

  ],
  "AssignableScopes": [
    "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
    "/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624",
    "/subscriptions/34370e90-ac4a-4bf9-821f-85eeedeae1a2"
  ]
}
```
## <a name="actions"></a><span data-ttu-id="ecfb4-110">Műveletek</span><span class="sxs-lookup"><span data-stu-id="ecfb4-110">Actions</span></span>
<span data-ttu-id="ecfb4-111">A **műveletek** egy egyéni biztonsági szerepkört a tulajdonság határozza meg, amelyhez a szerepkör hozzáférést biztosít az Azure műveletek.</span><span class="sxs-lookup"><span data-stu-id="ecfb4-111">The **Actions** property of a custom role specifies the Azure operations to which the role grants access.</span></span> <span data-ttu-id="ecfb4-112">Művelet karakterláncok, amelyek azonosítják az Azure erőforrás-szolgáltatók biztonságos műveletek gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="ecfb4-112">It is a collection of operation strings that identify securable operations of Azure resource providers.</span></span> <span data-ttu-id="ecfb4-113">Művelet karakterláncok kövesse formátuma `Microsoft.<ProviderName>/<ChildResourceType>/<action>`.</span><span class="sxs-lookup"><span data-stu-id="ecfb4-113">Operation strings follow the format of `Microsoft.<ProviderName>/<ChildResourceType>/<action>`.</span></span> <span data-ttu-id="ecfb4-114">Helyettesítő karakterek művelet karakterláncokat (\*) hozzáférést biztosíthat a művelet karakterláncnak megfelelő összes művelethez.</span><span class="sxs-lookup"><span data-stu-id="ecfb4-114">Operation strings that contain wildcards (\*) grant access to all operations that match the operation string.</span></span> <span data-ttu-id="ecfb4-115">Például:</span><span class="sxs-lookup"><span data-stu-id="ecfb4-115">For instance:</span></span>

* <span data-ttu-id="ecfb4-116">`*/read`biztosít hozzáférést az olvasási műveletek az összes Azure-erőforrás-szolgáltató minden erőforrástípus esetén.</span><span class="sxs-lookup"><span data-stu-id="ecfb4-116">`*/read` grants access to read operations for all resource types of all Azure resource providers.</span></span>
* <span data-ttu-id="ecfb4-117">`Microsoft.Compute/*`engedélyezi a hozzáférést a Microsoft.Compute erőforrás-szolgáltató az összes erőforrástípus összes műveletet.</span><span class="sxs-lookup"><span data-stu-id="ecfb4-117">`Microsoft.Compute/*` grants access to all operations for all resource types in the Microsoft.Compute resource provider.</span></span>
* <span data-ttu-id="ecfb4-118">`Microsoft.Network/*/read`olvasási műveletek összes erőforrás típusához a Microsoft.Network erőforrás-szolgáltató az Azure biztosít hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="ecfb4-118">`Microsoft.Network/*/read` grants access to read operations for all resource types in the Microsoft.Network resource provider of Azure.</span></span>
* <span data-ttu-id="ecfb4-119">`Microsoft.Compute/virtualMachines/*`minden műveletet a virtuális gépek és a gyermek típusú erőforrások hozzáférést biztosít.</span><span class="sxs-lookup"><span data-stu-id="ecfb4-119">`Microsoft.Compute/virtualMachines/*` grants access to all operations of virtual machines and its child resource types.</span></span>
* <span data-ttu-id="ecfb4-120">`Microsoft.Web/sites/restart/Action`a hozzáférési webhely újraindítására.</span><span class="sxs-lookup"><span data-stu-id="ecfb4-120">`Microsoft.Web/sites/restart/Action` grants access to restart websites.</span></span>

<span data-ttu-id="ecfb4-121">Használjon `Get-AzureRmProviderOperation` (a PowerShell) vagy `azure provider operations show` (az Azure CLI) az Azure erőforrás-szolgáltatók műveletek.</span><span class="sxs-lookup"><span data-stu-id="ecfb4-121">Use `Get-AzureRmProviderOperation` (in PowerShell) or `azure provider operations show` (in Azure CLI) to list operations of Azure resource providers.</span></span> <span data-ttu-id="ecfb4-122">Ezek a parancsok annak ellenőrzéséhez, hogy egy művelet karakterlánc érvényes, és bontsa ki a helyettesítő művelet karakterláncok is használhatja.</span><span class="sxs-lookup"><span data-stu-id="ecfb4-122">You may also use these commands to verify that an operation string is valid, and to expand wildcard operation strings.</span></span>

```
Get-AzureRMProviderOperation Microsoft.Compute/virtualMachines/*/action | FT Operation, OperationName

Get-AzureRMProviderOperation Microsoft.Network/*
```

![PowerShell képernyőfelvétel - Get-AzureRMProviderOperation](./media/role-based-access-control-configure/1-get-azurermprovideroperation-1.png)

```
azure provider operations show "Microsoft.Compute/virtualMachines/*/action" --js on | jq '.[] | .operation'

azure provider operations show "Microsoft.Network/*"
```

![<span data-ttu-id="ecfb4-124">Az Azure CLI képernyőfelvétel - azure szolgáltató műveletek megjelenítése "Microsoft.Compute/virtualMachines/ \* /művelet"</span><span class="sxs-lookup"><span data-stu-id="ecfb4-124">Azure CLI screenshot - azure provider operations show "Microsoft.Compute/virtualMachines/\*/action"</span></span> ](./media/role-based-access-control-configure/1-azure-provider-operations-show.png)

## <a name="notactions"></a><span data-ttu-id="ecfb4-125">NotActions</span><span class="sxs-lookup"><span data-stu-id="ecfb4-125">NotActions</span></span>
<span data-ttu-id="ecfb4-126">Használja a **NotActions** tulajdonság, ha engedélyezni szeretné műveletek készletét könnyebben definiálva tiltott operatív kizárásával.</span><span class="sxs-lookup"><span data-stu-id="ecfb4-126">Use the **NotActions** property if the set of operations that you wish to allow is more easily defined by excluding restricted operations.</span></span> <span data-ttu-id="ecfb4-127">Egyéni szerepkör által biztosított hozzáférést számított kivonja a **NotActions** műveletek a **műveletek** műveletek.</span><span class="sxs-lookup"><span data-stu-id="ecfb4-127">The access granted by a custom role is computed by subtracting the **NotActions** operations from the **Actions** operations.</span></span>

> [!NOTE]
> <span data-ttu-id="ecfb4-128">Ha egy felhasználó tartozik, amely nem tartalmazza egy műveletet a szerepkör **NotActions**, és hozzá van rendelve egy második szerepkör, amely hozzáférést biztosít a műveletet, hogy a felhasználó számára engedélyezett a művelet elvégzéséhez.</span><span class="sxs-lookup"><span data-stu-id="ecfb4-128">If a user is assigned a role that excludes an operation in **NotActions**, and is assigned a second role that grants access to the same operation, the user is allowed to perform that operation.</span></span> <span data-ttu-id="ecfb4-129">**NotActions** nincs megtagadási szabály – egyszerűen csak egy kényelmes módot nyújt az engedélyezett műveletek készlet létrehozása, ha az adott műveletek ki lesznek zárva.</span><span class="sxs-lookup"><span data-stu-id="ecfb4-129">**NotActions** is not a deny rule – it is simply a convenient way to create a set of allowed operations when specific operations need to be excluded.</span></span>
>
>

## <a name="assignablescopes"></a><span data-ttu-id="ecfb4-130">AssignableScopes</span><span class="sxs-lookup"><span data-stu-id="ecfb4-130">AssignableScopes</span></span>
<span data-ttu-id="ecfb4-131">A **AssignableScopes** az egyéni szerepkör tulajdonság határozza meg a hatókörök (előfizetések, erőforráscsoport-sablonok vagy az erőforrások) belül, amely az egyéni szerepkör érhető el a hozzárendeléshez.</span><span class="sxs-lookup"><span data-stu-id="ecfb4-131">The **AssignableScopes** property of the custom role specifies the scopes (subscriptions, resource groups, or resources) within which the custom role is available for assignment.</span></span> <span data-ttu-id="ecfb4-132">Az egyéni biztonsági szerepkört csak az előfizetések vagy az azt igénylő erőforráscsoportok hozzárendelés elérhetővé teszi, és nem a felhasználói élmény beállítása a előfizetések és az erőforráscsoportok többi megzavarhatják.</span><span class="sxs-lookup"><span data-stu-id="ecfb4-132">You can make the custom role available for assignment in only the subscriptions or resource groups that require it, and not clutter user experience for the rest of the subscriptions or resource groups.</span></span>

<span data-ttu-id="ecfb4-133">Érvényes hozzárendelhető hatókörök például:</span><span class="sxs-lookup"><span data-stu-id="ecfb4-133">Examples of valid assignable scopes include:</span></span>

* <span data-ttu-id="ecfb4-134">"/ subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e", "/ subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624" - teszi két előfizetések rendelhető hozzá a szerepkört.</span><span class="sxs-lookup"><span data-stu-id="ecfb4-134">“/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e”, “/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624” - makes the role available for assignment in two subscriptions.</span></span>
* <span data-ttu-id="ecfb4-135">"/ subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e" - hoz egy-egy előfizetéshez rendelhető hozzá a szerepkört.</span><span class="sxs-lookup"><span data-stu-id="ecfb4-135">“/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e” - makes the role available for assignment in a single subscription.</span></span>
* <span data-ttu-id="ecfb4-136">"/ előfizetések/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/hálózati" - elérhetővé teszi a szerepkör hozzárendelése csak az a hálózati erőforrás csoportba.</span><span class="sxs-lookup"><span data-stu-id="ecfb4-136">“/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network” - makes the role available for assignment only in the Network resource group.</span></span>

> [!NOTE]
> <span data-ttu-id="ecfb4-137">Segítségével kell legalább egy előfizetést, erőforrás vagy az erőforrás-azonosító.</span><span class="sxs-lookup"><span data-stu-id="ecfb4-137">You must use at least one subscription, resource group, or resource ID.</span></span>
>
>

## <a name="custom-roles-access-control"></a><span data-ttu-id="ecfb4-138">Egyéni szerepkörök hozzáférés-vezérlés</span><span class="sxs-lookup"><span data-stu-id="ecfb4-138">Custom roles access control</span></span>
<span data-ttu-id="ecfb4-139">A **AssignableScopes** tulajdonsága az egyéni biztonsági szerepkört is vezérli, akik megtekintése, módosítása és törlése a szerepkör.</span><span class="sxs-lookup"><span data-stu-id="ecfb4-139">The **AssignableScopes** property of the custom role also controls who can view, modify, and delete the role.</span></span>

* <span data-ttu-id="ecfb4-140">Aki hozhat létre egy egyéni biztonsági szerepkört?</span><span class="sxs-lookup"><span data-stu-id="ecfb4-140">Who can create a custom role?</span></span>
    <span data-ttu-id="ecfb4-141">Tulajdonosok (és a felhasználói hozzáférés rendszergazdák) előfizetések, erőforráscsoport-sablonok és erőforrások hozhat létre egyéni szerepkörök használatra ezeket.</span><span class="sxs-lookup"><span data-stu-id="ecfb4-141">Owners (and User Access Administrators) of subscriptions, resource groups, and resources can create custom roles for use in those scopes.</span></span>
    <span data-ttu-id="ecfb4-142">A szerepkör létrehozása a felhasználó végezhet kell `Microsoft.Authorization/roleDefinition/write` minden műveletet a **AssignableScopes** a szerepkör.</span><span class="sxs-lookup"><span data-stu-id="ecfb4-142">The user creating the role needs to be able to perform `Microsoft.Authorization/roleDefinition/write` operation on all the **AssignableScopes** of the role.</span></span>
* <span data-ttu-id="ecfb4-143">Módosíthatja, akik egy egyéni biztonsági szerepkört?</span><span class="sxs-lookup"><span data-stu-id="ecfb4-143">Who can modify a custom role?</span></span>
    <span data-ttu-id="ecfb4-144">Tulajdonosok (és a felhasználói hozzáférés rendszergazdák) előfizetések, erőforráscsoport-sablonok és erőforrások ezeket az egyéni szerepkörök módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="ecfb4-144">Owners (and User Access Administrators) of subscriptions, resource groups, and resources can modify custom roles in those scopes.</span></span> <span data-ttu-id="ecfb4-145">Felhasználók kell tudni elvégezni a `Microsoft.Authorization/roleDefinition/write` minden műveletet a **AssignableScopes** egy egyéni szerepkör.</span><span class="sxs-lookup"><span data-stu-id="ecfb4-145">Users need to be able to perform the `Microsoft.Authorization/roleDefinition/write` operation on all the **AssignableScopes** of a custom role.</span></span>
* <span data-ttu-id="ecfb4-146">Kik tekinthetik meg egyéni szerepkörök?</span><span class="sxs-lookup"><span data-stu-id="ecfb4-146">Who can view custom roles?</span></span>
    <span data-ttu-id="ecfb4-147">Minden beépített szerepkörök az Azure RBAC kiosztására használható szerepkörtől megtekintésének engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="ecfb4-147">All built-in roles in Azure RBAC allow viewing of roles that are available for assignment.</span></span> <span data-ttu-id="ecfb4-148">Felhasználók, akik hajthat végre a `Microsoft.Authorization/roleDefinition/read` hatókörre művelet megtekintheti az adott hatókörben kiosztására használható RBAC szerepköröket.</span><span class="sxs-lookup"><span data-stu-id="ecfb4-148">Users who can perform the `Microsoft.Authorization/roleDefinition/read` operation at a scope can view the RBAC roles that are available for assignment at that scope.</span></span>

## <a name="see-also"></a><span data-ttu-id="ecfb4-149">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="ecfb4-149">See also</span></span>
* <span data-ttu-id="ecfb4-150">[Szerepköralapú hozzáférés-vezérlés](role-based-access-control-configure.md): az RBAC első lépései az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="ecfb4-150">[Role Based Access Control](role-based-access-control-configure.md): Get started with RBAC in the Azure portal.</span></span>
* <span data-ttu-id="ecfb4-151">Útmutató: a hozzáférés kezelése:</span><span class="sxs-lookup"><span data-stu-id="ecfb4-151">Learn how to manage access with:</span></span>
  * [<span data-ttu-id="ecfb4-152">PowerShell</span><span class="sxs-lookup"><span data-stu-id="ecfb4-152">PowerShell</span></span>](role-based-access-control-manage-access-powershell.md)
  * [<span data-ttu-id="ecfb4-153">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="ecfb4-153">Azure CLI</span></span>](role-based-access-control-manage-access-azure-cli.md)
  * [<span data-ttu-id="ecfb4-154">REST API</span><span class="sxs-lookup"><span data-stu-id="ecfb4-154">REST API</span></span>](role-based-access-control-manage-access-rest.md)
* <span data-ttu-id="ecfb4-155">[Beépített szerepkörök](role-based-access-built-in-roles.md): részletes információkat szolgáltatva a szerepköröket, az RBAC szabványos tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="ecfb4-155">[Built-in roles](role-based-access-built-in-roles.md): Get details about the roles that come standard in RBAC.</span></span>
