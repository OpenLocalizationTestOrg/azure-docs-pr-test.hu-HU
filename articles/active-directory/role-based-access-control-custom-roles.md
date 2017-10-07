---
title: "aaaCreate egyéni szerepkörök az Azure RBAC |} Microsoft Docs"
description: "Megtudhatja, hogyan toodefine egyéni szerepkörök-based hozzáférés-vezérléssel pontosabb identitáskezeléshez az Azure-előfizetése."
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
ms.openlocfilehash: 60df12632ef6c086d5feeb1809196d7c4ee5e021
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-custom-roles-for-azure-role-based-access-control"></a><span data-ttu-id="2e103-103">Hozzon létre egyéni szerepkörök átruházásához hozzáférés-vezérlés</span><span class="sxs-lookup"><span data-stu-id="2e103-103">Create custom roles for Azure Role-Based Access Control</span></span>
<span data-ttu-id="2e103-104">Hozzon létre egy egyéni biztonsági szerepkört a átruházásához hozzáférés-vezérlés (RBAC) Ha az adott hozzáférési igényeinek hello beépített szerepkörök egyike.</span><span class="sxs-lookup"><span data-stu-id="2e103-104">Create a custom role in Azure Role-Based Access Control (RBAC) if none of hello built-in roles meet your specific access needs.</span></span> <span data-ttu-id="2e103-105">Egyéni szerepkörök segítségével hozhatók létre [Azure PowerShell](role-based-access-control-manage-access-powershell.md), [Azure parancssori felület](role-based-access-control-manage-access-azure-cli.md) (CLI), és hello [REST API](role-based-access-control-manage-access-rest.md).</span><span class="sxs-lookup"><span data-stu-id="2e103-105">Custom roles can be created using [Azure PowerShell](role-based-access-control-manage-access-powershell.md), [Azure Command-Line Interface](role-based-access-control-manage-access-azure-cli.md) (CLI), and hello [REST API](role-based-access-control-manage-access-rest.md).</span></span> <span data-ttu-id="2e103-106">Hasonlóan a beépített szerepkörök rendelhet hozzá egyéni szerepkörök toousers, csoportok és alkalmazások előfizetés, erőforráscsoport és erőforrás-hatókörök.</span><span class="sxs-lookup"><span data-stu-id="2e103-106">Just like built-in roles, you can assign custom roles toousers, groups, and applications at subscription, resource group, and resource scopes.</span></span> <span data-ttu-id="2e103-107">Egyéni szerepkörök az Azure AD-bérlő tárolódnak, és előfizetések között megosztható legyen.</span><span class="sxs-lookup"><span data-stu-id="2e103-107">Custom roles are stored in an Azure AD tenant and can be shared across subscriptions.</span></span>

<span data-ttu-id="2e103-108">Mindegyik bérlő mentése too2000 egyéni szerepkörök hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="2e103-108">Each tenant can create up too2000 custom roles.</span></span> 

<span data-ttu-id="2e103-109">hello alábbi példa bemutatja egy egyéni biztonsági szerepkört a figyelés és a virtuális gépek újraindításával:</span><span class="sxs-lookup"><span data-stu-id="2e103-109">hello following example shows a custom role for monitoring and restarting virtual machines:</span></span>

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
## <a name="actions"></a><span data-ttu-id="2e103-110">Műveletek</span><span class="sxs-lookup"><span data-stu-id="2e103-110">Actions</span></span>
<span data-ttu-id="2e103-111">Hello **műveletek** egy egyéni biztonsági szerepkört a tulajdonság határozza meg, hogy hello Azure üzemeltetése toowhich hello szerepkör engedélyezi a hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="2e103-111">hello **Actions** property of a custom role specifies hello Azure operations toowhich hello role grants access.</span></span> <span data-ttu-id="2e103-112">Művelet karakterláncok, amelyek azonosítják az Azure erőforrás-szolgáltatók biztonságos műveletek gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="2e103-112">It is a collection of operation strings that identify securable operations of Azure resource providers.</span></span> <span data-ttu-id="2e103-113">Művelet karakterláncok kövesse hello formátuma `Microsoft.<ProviderName>/<ChildResourceType>/<action>`.</span><span class="sxs-lookup"><span data-stu-id="2e103-113">Operation strings follow hello format of `Microsoft.<ProviderName>/<ChildResourceType>/<action>`.</span></span> <span data-ttu-id="2e103-114">Helyettesítő karakterek művelet karakterláncokat (\*) hozzáférést hello művelet karakterláncnak megfelelő tooall műveletek.</span><span class="sxs-lookup"><span data-stu-id="2e103-114">Operation strings that contain wildcards (\*) grant access tooall operations that match hello operation string.</span></span> <span data-ttu-id="2e103-115">Például:</span><span class="sxs-lookup"><span data-stu-id="2e103-115">For instance:</span></span>

* <span data-ttu-id="2e103-116">`*/read`a hozzáférési tooread műveletek az összes Azure-erőforrás-szolgáltató minden erőforrástípus esetén.</span><span class="sxs-lookup"><span data-stu-id="2e103-116">`*/read` grants access tooread operations for all resource types of all Azure resource providers.</span></span>
* <span data-ttu-id="2e103-117">`Microsoft.Compute/*`a hozzáférési tooall műveletek hello Microsoft.Compute erőforrás-szolgáltató összes erőforrás típusához.</span><span class="sxs-lookup"><span data-stu-id="2e103-117">`Microsoft.Compute/*` grants access tooall operations for all resource types in hello Microsoft.Compute resource provider.</span></span>
* <span data-ttu-id="2e103-118">`Microsoft.Network/*/read`a hozzáférési tooread műveletek Azure hello Microsoft.Network erőforrás-szolgáltató az összes erőforrástípus.</span><span class="sxs-lookup"><span data-stu-id="2e103-118">`Microsoft.Network/*/read` grants access tooread operations for all resource types in hello Microsoft.Network resource provider of Azure.</span></span>
* <span data-ttu-id="2e103-119">`Microsoft.Compute/virtualMachines/*`a hozzáférési tooall műveletek a virtuális gépek és a gyermek típusú erőforrások.</span><span class="sxs-lookup"><span data-stu-id="2e103-119">`Microsoft.Compute/virtualMachines/*` grants access tooall operations of virtual machines and its child resource types.</span></span>
* <span data-ttu-id="2e103-120">`Microsoft.Web/sites/restart/Action`a hozzáférési toorestart webhelyeket.</span><span class="sxs-lookup"><span data-stu-id="2e103-120">`Microsoft.Web/sites/restart/Action` grants access toorestart websites.</span></span>

<span data-ttu-id="2e103-121">Használjon `Get-AzureRmProviderOperation` (a PowerShell) vagy `azure provider operations show` (az Azure CLI) toolist műveletek az Azure erőforrás-szolgáltatót.</span><span class="sxs-lookup"><span data-stu-id="2e103-121">Use `Get-AzureRmProviderOperation` (in PowerShell) or `azure provider operations show` (in Azure CLI) toolist operations of Azure resource providers.</span></span> <span data-ttu-id="2e103-122">A parancsok tooverify, hogy egy művelet karakterlánc érvénytelen, és a tooexpand helyettesítő művelet sztringek is használhatja.</span><span class="sxs-lookup"><span data-stu-id="2e103-122">You may also use these commands tooverify that an operation string is valid, and tooexpand wildcard operation strings.</span></span>

```
Get-AzureRMProviderOperation Microsoft.Compute/virtualMachines/*/action | FT Operation, OperationName

Get-AzureRMProviderOperation Microsoft.Network/*
```

![PowerShell képernyőfelvétel - Get-AzureRMProviderOperation](./media/role-based-access-control-configure/1-get-azurermprovideroperation-1.png)

```
azure provider operations show "Microsoft.Compute/virtualMachines/*/action" --js on | jq '.[] | .operation'

azure provider operations show "Microsoft.Network/*"
```

![<span data-ttu-id="2e103-124">Az Azure CLI képernyőfelvétel - azure szolgáltató műveletek megjelenítése "Microsoft.Compute/virtualMachines/ \* /művelet"</span><span class="sxs-lookup"><span data-stu-id="2e103-124">Azure CLI screenshot - azure provider operations show "Microsoft.Compute/virtualMachines/\*/action"</span></span> ](./media/role-based-access-control-configure/1-azure-provider-operations-show.png)

## <a name="notactions"></a><span data-ttu-id="2e103-125">NotActions</span><span class="sxs-lookup"><span data-stu-id="2e103-125">NotActions</span></span>
<span data-ttu-id="2e103-126">Használjon hello **NotActions** tulajdonságot, ha hello beállítása, hogy kívánja-e tooallow műveletek könnyebben definiált tiltott operatív kizárásával.</span><span class="sxs-lookup"><span data-stu-id="2e103-126">Use hello **NotActions** property if hello set of operations that you wish tooallow is more easily defined by excluding restricted operations.</span></span> <span data-ttu-id="2e103-127">hello egy egyéni szerepkör által biztosított hozzáférést számított hello kivonásával **NotActions** hello műveletek **műveletek** műveletek.</span><span class="sxs-lookup"><span data-stu-id="2e103-127">hello access granted by a custom role is computed by subtracting hello **NotActions** operations from hello **Actions** operations.</span></span>

> [!NOTE]
> <span data-ttu-id="2e103-128">Ha egy felhasználó tartozik, amely nem tartalmazza egy műveletet a szerepkör **NotActions**, és hozzá van rendelve egy második szerepkör, amely hozzáférést biztosít műveletet, hello felhasználói toohello engedélyezett tooperform művelet.</span><span class="sxs-lookup"><span data-stu-id="2e103-128">If a user is assigned a role that excludes an operation in **NotActions**, and is assigned a second role that grants access toohello same operation, hello user is allowed tooperform that operation.</span></span> <span data-ttu-id="2e103-129">**NotActions** nincs megtagadási szabály – Ha az adott műveletek kizárt toobe egyszerűen csak egy kényelmes módszert arra toocreate engedélyezett műveletkészlet.</span><span class="sxs-lookup"><span data-stu-id="2e103-129">**NotActions** is not a deny rule – it is simply a convenient way toocreate a set of allowed operations when specific operations need toobe excluded.</span></span>
>
>

## <a name="assignablescopes"></a><span data-ttu-id="2e103-130">AssignableScopes</span><span class="sxs-lookup"><span data-stu-id="2e103-130">AssignableScopes</span></span>
<span data-ttu-id="2e103-131">Hello **AssignableScopes** hello egyéni szerepkör tulajdonság határozza meg a hello hatókörök (előfizetések, erőforráscsoport-sablonok vagy az erőforrások) belül mely hello egyéni szerepkör érhető el a hozzárendeléshez.</span><span class="sxs-lookup"><span data-stu-id="2e103-131">hello **AssignableScopes** property of hello custom role specifies hello scopes (subscriptions, resource groups, or resources) within which hello custom role is available for assignment.</span></span> <span data-ttu-id="2e103-132">Elérhetővé teheti hello egyéni szerepkör hozzárendelés csak hello előfizetésekhez vagy erőforráscsoportokhoz olyan igényli azt, és nem zsúfoltságát felhasználói élményt a hello előfizetések vagy erőforráscsoportok hello részeinek.</span><span class="sxs-lookup"><span data-stu-id="2e103-132">You can make hello custom role available for assignment in only hello subscriptions or resource groups that require it, and not clutter user experience for hello rest of hello subscriptions or resource groups.</span></span>

<span data-ttu-id="2e103-133">Érvényes hozzárendelhető hatókörök például:</span><span class="sxs-lookup"><span data-stu-id="2e103-133">Examples of valid assignable scopes include:</span></span>

* <span data-ttu-id="2e103-134">"/ subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e", "/ subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624" - elérhetővé hello szerepkör hozzárendelése két előfizetésekhez.</span><span class="sxs-lookup"><span data-stu-id="2e103-134">“/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e”, “/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624” - makes hello role available for assignment in two subscriptions.</span></span>
* <span data-ttu-id="2e103-135">"/ subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e" - hello szerepkör elérhetővé teszi a hozzárendeléshez egyetlen előfizetéssel.</span><span class="sxs-lookup"><span data-stu-id="2e103-135">“/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e” - makes hello role available for assignment in a single subscription.</span></span>
* <span data-ttu-id="2e103-136">"/ előfizetések/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/hálózati" - teszi hello szerepkör csak a hello hálózati erőforráscsoport kiosztására használható.</span><span class="sxs-lookup"><span data-stu-id="2e103-136">“/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network” - makes hello role available for assignment only in hello Network resource group.</span></span>

> [!NOTE]
> <span data-ttu-id="2e103-137">Segítségével kell legalább egy előfizetést, erőforrás vagy az erőforrás-azonosító.</span><span class="sxs-lookup"><span data-stu-id="2e103-137">You must use at least one subscription, resource group, or resource ID.</span></span>
>
>

## <a name="custom-roles-access-control"></a><span data-ttu-id="2e103-138">Egyéni szerepkörök hozzáférés-vezérlés</span><span class="sxs-lookup"><span data-stu-id="2e103-138">Custom roles access control</span></span>
<span data-ttu-id="2e103-139">Hello **AssignableScopes** hello egyéni szerepkör tulajdonság is vezérli, akik megtekintése, módosítása és törlése hello szerepkör.</span><span class="sxs-lookup"><span data-stu-id="2e103-139">hello **AssignableScopes** property of hello custom role also controls who can view, modify, and delete hello role.</span></span>

* <span data-ttu-id="2e103-140">Aki hozhat létre egy egyéni biztonsági szerepkört?</span><span class="sxs-lookup"><span data-stu-id="2e103-140">Who can create a custom role?</span></span>
    <span data-ttu-id="2e103-141">Tulajdonosok (és a felhasználói hozzáférés rendszergazdák) előfizetések, erőforráscsoport-sablonok és erőforrások hozhat létre egyéni szerepkörök használatra ezeket.</span><span class="sxs-lookup"><span data-stu-id="2e103-141">Owners (and User Access Administrators) of subscriptions, resource groups, and resources can create custom roles for use in those scopes.</span></span>
    <span data-ttu-id="2e103-142">hello hello szerepkör létrehozó felhasználónak kell toobe képes tooperform `Microsoft.Authorization/roleDefinition/write` összes hello művelet **AssignableScopes** hello szerepkör.</span><span class="sxs-lookup"><span data-stu-id="2e103-142">hello user creating hello role needs toobe able tooperform `Microsoft.Authorization/roleDefinition/write` operation on all hello **AssignableScopes** of hello role.</span></span>
* <span data-ttu-id="2e103-143">Módosíthatja, akik egy egyéni biztonsági szerepkört?</span><span class="sxs-lookup"><span data-stu-id="2e103-143">Who can modify a custom role?</span></span>
    <span data-ttu-id="2e103-144">Tulajdonosok (és a felhasználói hozzáférés rendszergazdák) előfizetések, erőforráscsoport-sablonok és erőforrások ezeket az egyéni szerepkörök módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="2e103-144">Owners (and User Access Administrators) of subscriptions, resource groups, and resources can modify custom roles in those scopes.</span></span> <span data-ttu-id="2e103-145">A felhasználóknak kell toobe képes tooperform hello `Microsoft.Authorization/roleDefinition/write` összes hello művelet **AssignableScopes** egy egyéni szerepkör.</span><span class="sxs-lookup"><span data-stu-id="2e103-145">Users need toobe able tooperform hello `Microsoft.Authorization/roleDefinition/write` operation on all hello **AssignableScopes** of a custom role.</span></span>
* <span data-ttu-id="2e103-146">Kik tekinthetik meg egyéni szerepkörök?</span><span class="sxs-lookup"><span data-stu-id="2e103-146">Who can view custom roles?</span></span>
    <span data-ttu-id="2e103-147">Minden beépített szerepkörök az Azure RBAC kiosztására használható szerepkörtől megtekintésének engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="2e103-147">All built-in roles in Azure RBAC allow viewing of roles that are available for assignment.</span></span> <span data-ttu-id="2e103-148">Felhasználók, akik végezheti hello `Microsoft.Authorization/roleDefinition/read` hatókörre művelet hello RBAC szerepkörtől érhetők el, hogy a hatókör-hozzárendelés tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="2e103-148">Users who can perform hello `Microsoft.Authorization/roleDefinition/read` operation at a scope can view hello RBAC roles that are available for assignment at that scope.</span></span>

## <a name="see-also"></a><span data-ttu-id="2e103-149">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="2e103-149">See also</span></span>
* <span data-ttu-id="2e103-150">[Szerepköralapú hozzáférés-vezérlés](role-based-access-control-configure.md): első lépések az RBAC a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="2e103-150">[Role Based Access Control](role-based-access-control-configure.md): Get started with RBAC in hello Azure portal.</span></span>
* <span data-ttu-id="2e103-151">Megtudhatja, hogyan toomanage elérni:</span><span class="sxs-lookup"><span data-stu-id="2e103-151">Learn how toomanage access with:</span></span>
  * [<span data-ttu-id="2e103-152">PowerShell</span><span class="sxs-lookup"><span data-stu-id="2e103-152">PowerShell</span></span>](role-based-access-control-manage-access-powershell.md)
  * [<span data-ttu-id="2e103-153">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="2e103-153">Azure CLI</span></span>](role-based-access-control-manage-access-azure-cli.md)
  * [<span data-ttu-id="2e103-154">REST API</span><span class="sxs-lookup"><span data-stu-id="2e103-154">REST API</span></span>](role-based-access-control-manage-access-rest.md)
* <span data-ttu-id="2e103-155">[Beépített szerepkörök](role-based-access-built-in-roles.md): részletes információkat szolgáltatva hello szerepköröket, az RBAC szabványos tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="2e103-155">[Built-in roles](role-based-access-built-in-roles.md): Get details about hello roles that come standard in RBAC.</span></span>
