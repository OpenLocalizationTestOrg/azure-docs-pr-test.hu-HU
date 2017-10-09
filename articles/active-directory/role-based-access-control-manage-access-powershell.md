---
title: "Szerepköralapú hozzáférés-vezérlést (RBAC) az Azure PowerShell aaaManage |} Microsoft Docs"
description: "Hogyan toomanage RBAC az Azure PowerShell, beleértve a szerepköröket, a szerepkörök hozzárendelése és a szerepkör-hozzárendelések törlése."
services: active-directory
documentationcenter: 
author: andredm7
manager: femila
ms.assetid: 9e225dba-9044-4b13-b573-2f30d77925a9
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/12/2017
ms.author: andredm
ms.reviewer: rqureshi
ms.openlocfilehash: fa44991113e75b345177867b0bede38de4373e04
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-role-based-access-control-with-azure-powershell"></a><span data-ttu-id="da99c-103">Szerepköralapú hozzáférés-vezérlés kezelése az Azure PowerShell-lel</span><span class="sxs-lookup"><span data-stu-id="da99c-103">Manage Role-Based Access Control with Azure PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="da99c-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="da99c-104">PowerShell</span></span>](role-based-access-control-manage-access-powershell.md)
> * [<span data-ttu-id="da99c-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="da99c-105">Azure CLI</span></span>](role-based-access-control-manage-access-azure-cli.md)
> * [<span data-ttu-id="da99c-106">REST API</span><span class="sxs-lookup"><span data-stu-id="da99c-106">REST API</span></span>](role-based-access-control-manage-access-rest.md)

<span data-ttu-id="da99c-107">Szerepköralapú hozzáférés-vezérlést (RBAC) használhatja hello Azure-portál és az Azure erőforrás-kezelési API toomanage hozzáférés tooyour előfizetés részletes szinten.</span><span class="sxs-lookup"><span data-stu-id="da99c-107">You can use Role-Based Access Control (RBAC) in hello Azure portal and Azure Resource Management API toomanage access tooyour subscription at a fine-grained level.</span></span> <span data-ttu-id="da99c-108">Ez a szolgáltatás egyes szerepkörök toothem egy adott hatókör hozzárendelése szerint engedélyezheti a hozzáférést az Active Directory felhasználók, csoportok vagy szolgáltatásnevekről.</span><span class="sxs-lookup"><span data-stu-id="da99c-108">With this feature, you can grant access for Active Directory users, groups, or service principals by assigning some roles toothem at a particular scope.</span></span>

<span data-ttu-id="da99c-109">PowerShell toomanage RBAC használata előtt a következő előfeltételek hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="da99c-109">Before you can use PowerShell toomanage RBAC, you need hello following prerequisites:</span></span>

* <span data-ttu-id="da99c-110">Az Azure PowerShell 0.8.8 verzió vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="da99c-110">Azure PowerShell version 0.8.8 or later.</span></span> <span data-ttu-id="da99c-111">tooinstall hello legújabb verzióját, és rendelje azt az Azure-előfizetéshez, lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="da99c-111">tooinstall hello latest version and associate it with your Azure subscription, see [how tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="da99c-112">Az Azure Resource Manager parancsmagok.</span><span class="sxs-lookup"><span data-stu-id="da99c-112">Azure Resource Manager cmdlets.</span></span> <span data-ttu-id="da99c-113">Telepítse a hello [Azure Resource Manager parancsmagjainak](/powershell/azure/overview) a PowerShellben.</span><span class="sxs-lookup"><span data-stu-id="da99c-113">Install hello [Azure Resource Manager cmdlets](/powershell/azure/overview) in PowerShell.</span></span>

## <a name="list-roles"></a><span data-ttu-id="da99c-114">Lista szerepkörök</span><span class="sxs-lookup"><span data-stu-id="da99c-114">List roles</span></span>
### <a name="list-all-available-roles"></a><span data-ttu-id="da99c-115">Elérhető szerepkörök felsorolása</span><span class="sxs-lookup"><span data-stu-id="da99c-115">List all available roles</span></span>
<span data-ttu-id="da99c-116">toolist RBAC-hozzárendelés és tooinspect hello műveletek toowhich azok engedélyezheti a hozzáférést, a rendelkezésre álló használnak `Get-AzureRmRoleDefinition`.</span><span class="sxs-lookup"><span data-stu-id="da99c-116">toolist RBAC roles that are available for assignment and tooinspect hello operations toowhich they grant access, use `Get-AzureRmRoleDefinition`.</span></span>

```
Get-AzureRmRoleDefinition | FT Name, Description
```

![Az RBAC PowerShell-Get AzureRmRoleDefinition – képernyőkép](./media/role-based-access-control-manage-access-powershell/1-get-azure-rm-role-definition1.png)

### <a name="list-actions-of-a-role"></a><span data-ttu-id="da99c-118">Egy szerepkör lista műveletek</span><span class="sxs-lookup"><span data-stu-id="da99c-118">List actions of a role</span></span>
<span data-ttu-id="da99c-119">egy adott szerepkör esetében toolist hello műveletek `Get-AzureRmRoleDefinition <role name>`.</span><span class="sxs-lookup"><span data-stu-id="da99c-119">toolist hello actions for a specific role, use `Get-AzureRmRoleDefinition <role name>`.</span></span>

```
Get-AzureRmRoleDefinition Contributor | FL Actions, NotActions

(Get-AzureRmRoleDefinition "Virtual Machine Contributor").Actions
```

![Az RBAC PowerShell-Get AzureRmRoleDefinition egy adott szerepkör esetében – képernyőkép](./media/role-based-access-control-manage-access-powershell/1-get-azure-rm-role-definition2.png)

## <a name="see-who-has-access"></a><span data-ttu-id="da99c-121">Lásd a kinek van hozzáférése:</span><span class="sxs-lookup"><span data-stu-id="da99c-121">See who has access</span></span>
<span data-ttu-id="da99c-122">toolist Szerepalapú hozzáférés-hozzárendelés, használjon `Get-AzureRmRoleAssignment`.</span><span class="sxs-lookup"><span data-stu-id="da99c-122">toolist RBAC access assignments, use `Get-AzureRmRoleAssignment`.</span></span>

### <a name="list-role-assignments-at-a-specific-scope"></a><span data-ttu-id="da99c-123">Szerepkör-hozzárendelések listáját egy adott hatókörhöz</span><span class="sxs-lookup"><span data-stu-id="da99c-123">List role assignments at a specific scope</span></span>
<span data-ttu-id="da99c-124">A megadott előfizetés, erőforráscsoportból vagy erőforrás összes hello hozzáférés hozzárendelését tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="da99c-124">You can see all hello access assignments for a specified subscription, resource group, or resource.</span></span> <span data-ttu-id="da99c-125">Például toosee hello összes hello aktív hozzárendelést egy erőforráscsoport, használjon `Get-AzureRmRoleAssignment -ResourceGroupName <resource group name>`.</span><span class="sxs-lookup"><span data-stu-id="da99c-125">For example, toosee hello all hello active assignments for a resource group, use `Get-AzureRmRoleAssignment -ResourceGroupName <resource group name>`.</span></span>

```
Get-AzureRmRoleAssignment -ResourceGroupName Pharma-Sales-ProjectForcast | FL DisplayName, RoleDefinitionName, Scope
```

![Az RBAC - Get-AzureRmRoleAssignment erőforráscsoport - PowerShell képernyőképe](./media/role-based-access-control-manage-access-powershell/4-get-azure-rm-role-assignment1.png)

### <a name="list-roles-assigned-tooa-user"></a><span data-ttu-id="da99c-127">Lista tooa felhasználói szerepkörrel</span><span class="sxs-lookup"><span data-stu-id="da99c-127">List roles assigned tooa user</span></span>
<span data-ttu-id="da99c-128">minden hello szerepkör tooa megadott felhasználói és toowhich hello felhasználó tartozik, toohello csoportok hello szerepkör toolist használata `Get-AzureRmRoleAssignment -SignInName <User email> -ExpandPrincipalGroups`.</span><span class="sxs-lookup"><span data-stu-id="da99c-128">toolist all hello roles that are assigned tooa specified user and hello roles that are assigned toohello groups toowhich hello user belongs, use `Get-AzureRmRoleAssignment -SignInName <User email> -ExpandPrincipalGroups`.</span></span>

```
Get-AzureRmRoleAssignment -SignInName sameert@aaddemo.com | FL DisplayName, RoleDefinitionName, Scope

Get-AzureRmRoleAssignment -SignInName sameert@aaddemo.com -ExpandPrincipalGroups | FL DisplayName, RoleDefinitionName, Scope
```

![Az RBAC PowerShell-Get AzureRmRoleAssignment egy felhasználó számára – képernyőkép](./media/role-based-access-control-manage-access-powershell/4-get-azure-rm-role-assignment2.png)

### <a name="list-classic-service-administrator-and-coadmin-role-assignments"></a><span data-ttu-id="da99c-130">Lista klasszikus szolgáltatás-rendszergazda és a szerepkör-hozzárendelések társfelügyeletű</span><span class="sxs-lookup"><span data-stu-id="da99c-130">List classic service administrator and coadmin role assignments</span></span>
<span data-ttu-id="da99c-131">hozzáférés-hozzárendelései toolist hello klasszikus előfizetési rendszergazda, és coadministrators, használja:</span><span class="sxs-lookup"><span data-stu-id="da99c-131">toolist access assignments for hello classic subscription administrator and coadministrators, use:</span></span>

    Get-AzureRmRoleAssignment -IncludeClassicAdministrators

## <a name="grant-access"></a><span data-ttu-id="da99c-132">Hozzáférés biztosítása</span><span class="sxs-lookup"><span data-stu-id="da99c-132">Grant access</span></span>
### <a name="search-for-object-ids"></a><span data-ttu-id="da99c-133">Objektumazonosítók keresése</span><span class="sxs-lookup"><span data-stu-id="da99c-133">Search for object IDs</span></span>
<span data-ttu-id="da99c-134">szerepkör tooassign, kell tooidentify hello objektum (felhasználó, csoport vagy alkalmazás) és a hello hatókör.</span><span class="sxs-lookup"><span data-stu-id="da99c-134">tooassign a role, you need tooidentify both hello object (user, group, or application) and hello scope.</span></span>

<span data-ttu-id="da99c-135">Ha nem tudja hello előfizetés-azonosító, megtalálja a hello **előfizetések** hello Azure-portálon paneljét.</span><span class="sxs-lookup"><span data-stu-id="da99c-135">If you don't know hello subscription ID, you can find it in hello **Subscriptions** blade on hello Azure portal.</span></span> <span data-ttu-id="da99c-136">Hogyan hello előfizetés-azonosító, a tooquery: toolearn [Get-AzureSubscription](/powershell/module/azure/get-azuresubscription?view=azuresmps-3.7.0) az MSDN Webhelyén.</span><span class="sxs-lookup"><span data-stu-id="da99c-136">toolearn how tooquery for hello subscription ID, see [Get-AzureSubscription](/powershell/module/azure/get-azuresubscription?view=azuresmps-3.7.0) on MSDN.</span></span>

<span data-ttu-id="da99c-137">tooget hello objektum azonosítója az Azure AD-csoport használja:</span><span class="sxs-lookup"><span data-stu-id="da99c-137">tooget hello object ID for an Azure AD group, use:</span></span>

    Get-AzureRmADGroup -SearchString <group name in quotes>

<span data-ttu-id="da99c-138">tooget hello Objektumazonosító egy egyszerű Azure AD szolgáltatás vagy alkalmazás használja:</span><span class="sxs-lookup"><span data-stu-id="da99c-138">tooget hello object ID for an Azure AD service principal or application, use:</span></span>

    Get-AzureRmADServicePrincipal -SearchString <service name in quotes>

### <a name="assign-a-role-tooan-application-at-hello-subscription-scope"></a><span data-ttu-id="da99c-139">Egy szerepkör tooan alkalmazást hello előfizetés hatókörből</span><span class="sxs-lookup"><span data-stu-id="da99c-139">Assign a role tooan application at hello subscription scope</span></span>
<span data-ttu-id="da99c-140">toogrant access tooan alkalmazás hello előfizetés hatókörben, használja:</span><span class="sxs-lookup"><span data-stu-id="da99c-140">toogrant access tooan application at hello subscription scope, use:</span></span>

    New-AzureRmRoleAssignment -ObjectId <application id> -RoleDefinitionName <role name> -Scope <subscription id>

![Az RBAC PowerShell új AzureRmRoleAssignment – képernyőkép](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment2.png)

### <a name="assign-a-role-tooa-user-at-hello-resource-group-scope"></a><span data-ttu-id="da99c-142">Egy szerepkör tooa felhasználó hello erőforrás csoport hatóköre:</span><span class="sxs-lookup"><span data-stu-id="da99c-142">Assign a role tooa user at hello resource group scope</span></span>
<span data-ttu-id="da99c-143">toogrant hozzáférés tooa felhasználójának hello erőforrás csoport hatóköre használja:</span><span class="sxs-lookup"><span data-stu-id="da99c-143">toogrant access tooa user at hello resource group scope, use:</span></span>

    New-AzureRmRoleAssignment -SignInName <email of user> -RoleDefinitionName <role name in quotes> -ResourceGroupName <resource group name>

![Az RBAC PowerShell új AzureRmRoleAssignment – képernyőkép](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment3.png)

### <a name="assign-a-role-tooa-group-at-hello-resource-scope"></a><span data-ttu-id="da99c-145">Egy szerepkör tooa csoport hello erőforrás hatókörből hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="da99c-145">Assign a role tooa group at hello resource scope</span></span>
<span data-ttu-id="da99c-146">toogrant hozzáférési tooa csoport hello erőforrás hatókörben, használja:</span><span class="sxs-lookup"><span data-stu-id="da99c-146">toogrant access tooa group at hello resource scope, use:</span></span>

    New-AzureRmRoleAssignment -ObjectId <object id> -RoleDefinitionName <role name in quotes> -ResourceName <resource name> -ResourceType <resource type> -ParentResource <parent resource> -ResourceGroupName <resource group name>

![Az RBAC PowerShell új AzureRmRoleAssignment – képernyőkép](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment4.png)

## <a name="remove-access"></a><span data-ttu-id="da99c-148">Megszünteti a hozzáférést</span><span class="sxs-lookup"><span data-stu-id="da99c-148">Remove access</span></span>
<span data-ttu-id="da99c-149">tooremove hozzáférés a felhasználók, csoportok és alkalmazások számára:</span><span class="sxs-lookup"><span data-stu-id="da99c-149">tooremove access for users, groups, and applications, use:</span></span>

    Remove-AzureRmRoleAssignment -ObjectId <object id> -RoleDefinitionName <role name> -Scope <scope such as subscription id>

![Az RBAC PowerShell-Remove AzureRmRoleAssignment – képernyőkép](./media/role-based-access-control-manage-access-powershell/3-remove-azure-rm-role-assignment.png)

## <a name="create-a-custom-role"></a><span data-ttu-id="da99c-151">Egyéni szerepkör létrehozása</span><span class="sxs-lookup"><span data-stu-id="da99c-151">Create a custom role</span></span>
<span data-ttu-id="da99c-152">egy egyéni biztonsági szerepkört, toocreate hello használata ```New-AzureRmRoleDefinition``` parancsot.</span><span class="sxs-lookup"><span data-stu-id="da99c-152">toocreate a custom role, use hello ```New-AzureRmRoleDefinition``` command.</span></span> <span data-ttu-id="da99c-153">Kétféleképpen a szerkezetének kialakítása hello szerepkör PSRoleDefinitionObject vagy egy JSON-sablon használatával.</span><span class="sxs-lookup"><span data-stu-id="da99c-153">There are two methods of structuring hello role, using PSRoleDefinitionObject or a JSON template.</span></span> 

## <a name="get-actions-for-a-resource-provider"></a><span data-ttu-id="da99c-154">Egy erőforrás-szolgáltató műveleteinek beolvasása</span><span class="sxs-lookup"><span data-stu-id="da99c-154">Get Actions for a Resource Provider</span></span>
<span data-ttu-id="da99c-155">Egyéni szerepkörök teljesen új hoz létre, esetén fontos tooknow összes lehetséges műveleteket az erőforrás-szolgáltatók hello hello.</span><span class="sxs-lookup"><span data-stu-id="da99c-155">When You are creating custom roles from scratch, it is important tooknow all hello possible operations from hello resource providers.</span></span>
<span data-ttu-id="da99c-156">Használjon hello ```Get-AzureRMProviderOperation``` parancs tooget ezt az információt.</span><span class="sxs-lookup"><span data-stu-id="da99c-156">Use hello ```Get-AzureRMProviderOperation``` command tooget this information.</span></span>
<span data-ttu-id="da99c-157">Például ha azt szeretné, hogy toocheck hello elérhető műveletek a virtuális gép használja ezt a parancsot:</span><span class="sxs-lookup"><span data-stu-id="da99c-157">For example, if you want toocheck all hello available operations for virtual Machine use this command:</span></span>

```
Get-AzureRMProviderOperation "Microsoft.Compute/virtualMachines/*" | FT OperationName, Operation , Description -AutoSize
```

### <a name="create-role-with-psroledefinitionobject"></a><span data-ttu-id="da99c-158">PSRoleDefinitionObject szerepkör létrehozása</span><span class="sxs-lookup"><span data-stu-id="da99c-158">Create role with PSRoleDefinitionObject</span></span>
<span data-ttu-id="da99c-159">PowerShell toocreate egy egyéni biztonsági szerepkört használatakor, új, vagy hello valamelyikével [beépített szerepkörök](role-based-access-built-in-roles.md) kiindulási pontként.</span><span class="sxs-lookup"><span data-stu-id="da99c-159">When you use PowerShell toocreate a custom role, you can start from scratch or use one of hello [built-in roles](role-based-access-built-in-roles.md) as a starting point.</span></span> <span data-ttu-id="da99c-160">Ebben a szakaszban hello például egy beépített szerepkör kezdődik, és majd testreszabása további jogosultságokkal.</span><span class="sxs-lookup"><span data-stu-id="da99c-160">hello example in this section starts with a built-in role and then customizes it with more privileges.</span></span> <span data-ttu-id="da99c-161">Hello attribútumok tooadd hello szerkesztése *műveletek*, *notActions*, vagy *hatókörök* , és mentse egy új szerepkörként hello módosítások.</span><span class="sxs-lookup"><span data-stu-id="da99c-161">Edit hello attributes tooadd hello *Actions*, *notActions*, or *scopes* that you want, and then save hello changes as a new role.</span></span>

<span data-ttu-id="da99c-162">hello alábbi példa kezdődik hello *virtuális gép közreműködő* szerepkör és használja, hogy egy egyéni biztonsági szerepkört toocreate nevű *virtuális gépet üzemeltető*.</span><span class="sxs-lookup"><span data-stu-id="da99c-162">hello following example starts with hello *Virtual Machine Contributor* role and uses that toocreate a custom role called *Virtual Machine Operator*.</span></span> <span data-ttu-id="da99c-163">hello új szerepkörök hozzáférési tooall olvasási műveletek a *Microsoft.Compute*, *Microsoft.Storage*, és *Microsoft.Network* erőforrás-szolgáltatók és biztosít hozzáférést toostart, indítsa újra, és a virtuális gépek figyelése.</span><span class="sxs-lookup"><span data-stu-id="da99c-163">hello new role grants access tooall read operations of *Microsoft.Compute*, *Microsoft.Storage*, and *Microsoft.Network* resource providers and grants access toostart, restart, and monitor virtual machines.</span></span> <span data-ttu-id="da99c-164">hello egyéni biztonsági szerepkört is használható két előfizetésekhez.</span><span class="sxs-lookup"><span data-stu-id="da99c-164">hello custom role can be used in two subscriptions.</span></span>

```
$role = Get-AzureRmRoleDefinition "Virtual Machine Contributor"
$role.Id = $null
$role.Name = "Virtual Machine Operator"
$role.Description = "Can monitor and restart virtual machines."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Storage/*/read")
$role.Actions.Add("Microsoft.Network/*/read")
$role.Actions.Add("Microsoft.Compute/*/read")
$role.Actions.Add("Microsoft.Compute/virtualMachines/start/action")
$role.Actions.Add("Microsoft.Compute/virtualMachines/restart/action")
$role.Actions.Add("Microsoft.Authorization/*/read")
$role.Actions.Add("Microsoft.Resources/subscriptions/resourceGroups/read")
$role.Actions.Add("Microsoft.Insights/alertRules/*")
$role.Actions.Add("Microsoft.Support/*")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e")
$role.AssignableScopes.Add("/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624")
New-AzureRmRoleDefinition -Role $role
```

![Az RBAC PowerShell-Get AzureRmRoleDefinition – képernyőkép](./media/role-based-access-control-manage-access-powershell/2-new-azurermroledefinition.png)

### <a name="create-role-with-json-template"></a><span data-ttu-id="da99c-166">A JSON-sablon szerepkör létrehozása</span><span class="sxs-lookup"><span data-stu-id="da99c-166">Create role with JSON template</span></span>
<span data-ttu-id="da99c-167">A JSON-sablon hello egyéni szerepkör hello adatforrása definíciója is használható.</span><span class="sxs-lookup"><span data-stu-id="da99c-167">A JSON template can be used as hello source definition for hello custom role.</span></span> <span data-ttu-id="da99c-168">hello alábbi példa létrehoz egy egyéni biztonsági szerepkört, amely lehetővé teszi az olvasási hozzáférés toostorage és számítási erőforrásokat, toosupport eléréséhez és hozzáadja az adott szerepkörhöz tootwo előfizetések.</span><span class="sxs-lookup"><span data-stu-id="da99c-168">hello following example creates a custom role that allows read access toostorage and compute resources, access toosupport, and adds that role tootwo subscriptions.</span></span> <span data-ttu-id="da99c-169">Hozzon létre egy új fájlt `C:\CustomRoles\customrole1.json` az alábbi példa hello.</span><span class="sxs-lookup"><span data-stu-id="da99c-169">Create a new file `C:\CustomRoles\customrole1.json` with hello following example.</span></span> <span data-ttu-id="da99c-170">hello azonosítóját kell beállítani. túl`null` a kezdeti szerepkör létrehozásakor egy új ID automatikusan létrejön.</span><span class="sxs-lookup"><span data-stu-id="da99c-170">hello Id should be set too`null` on initial role creation as a new ID is generated automatically.</span></span> 

```
{
  "Name": "Custom Role 1",
  "Id": null,
  "IsCustom": true,
  "Description": "Allows for read access tooAzure storage and compute resources and access toosupport",
  "Actions": [
    "Microsoft.Compute/*/read",
    "Microsoft.Storage/*/read",
    "Microsoft.Support/*"
  ],
  "NotActions": [
  ],
  "AssignableScopes": [
    "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
    "/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624"
  ]
}
```
<span data-ttu-id="da99c-171">tooadd hello szerepkör toohello előfizetések, futtassa a következő PowerShell-paranccsal hello:</span><span class="sxs-lookup"><span data-stu-id="da99c-171">tooadd hello role toohello subscriptions, run hello following PowerShell command:</span></span>
```
New-AzureRmRoleDefinition -InputFile "C:\CustomRoles\customrole1.json"
```

## <a name="modify-a-custom-role"></a><span data-ttu-id="da99c-172">Egyéni szerepkör módosítása</span><span class="sxs-lookup"><span data-stu-id="da99c-172">Modify a custom role</span></span>
<span data-ttu-id="da99c-173">Hasonló toocreating egy egyéni biztonsági szerepkört, módosíthatja egy meglévő egyéni szerepkör hello PSRoleDefinitionObject vagy egy JSON-sablon használatával.</span><span class="sxs-lookup"><span data-stu-id="da99c-173">Similar toocreating a custom role, you can modify an existing custom role using either hello PSRoleDefinitionObject or a JSON template.</span></span>

### <a name="modify-role-with-psroledefinitionobject"></a><span data-ttu-id="da99c-174">A PSRoleDefinitionObject szerepkör módosítása</span><span class="sxs-lookup"><span data-stu-id="da99c-174">Modify role with PSRoleDefinitionObject</span></span>
<span data-ttu-id="da99c-175">egy egyéni biztonsági szerepkört, toomodify először használja a hello `Get-AzureRmRoleDefinition` tooretrieve hello szerepkör-definíció parancsot.</span><span class="sxs-lookup"><span data-stu-id="da99c-175">toomodify a custom role, first, use hello `Get-AzureRmRoleDefinition` command tooretrieve hello role definition.</span></span> <span data-ttu-id="da99c-176">Második módosításokat szükséges hello toohello szerepkör-definíció.</span><span class="sxs-lookup"><span data-stu-id="da99c-176">Second, make hello desired changes toohello role definition.</span></span> <span data-ttu-id="da99c-177">Végül, használja a hello `Set-AzureRmRoleDefinition` parancs toosave hello módosítani a szerepkör-definíció.</span><span class="sxs-lookup"><span data-stu-id="da99c-177">Finally, use hello `Set-AzureRmRoleDefinition` command toosave hello modified role definition.</span></span>

<span data-ttu-id="da99c-178">hello következő példakóddal felveheti a hello `Microsoft.Insights/diagnosticSettings/*` művelet toohello *virtuális gépet üzemeltető* egyéni biztonsági szerepkört.</span><span class="sxs-lookup"><span data-stu-id="da99c-178">hello following example adds hello `Microsoft.Insights/diagnosticSettings/*` operation toohello *Virtual Machine Operator* custom role.</span></span>

```
$role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
$role.Actions.Add("Microsoft.Insights/diagnosticSettings/*")
Set-AzureRmRoleDefinition -Role $role
```

![Az RBAC PowerShell-Set AzureRmRoleDefinition – képernyőkép](./media/role-based-access-control-manage-access-powershell/3-set-azurermroledefinition-1.png)

<span data-ttu-id="da99c-180">hello következő példakóddal felveheti az Azure-előfizetés toohello hozzárendelhető hatóköröknek a hello *virtuális gépet üzemeltető* egyéni biztonsági szerepkört.</span><span class="sxs-lookup"><span data-stu-id="da99c-180">hello following example adds an Azure subscription toohello assignable scopes of hello *Virtual Machine Operator* custom role.</span></span>

```
Get-AzureRmSubscription - SubscriptionName Production3

$role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
$role.AssignableScopes.Add("/subscriptions/34370e90-ac4a-4bf9-821f-85eeedead1a2")
Set-AzureRmRoleDefinition -Role $role
```

![Az RBAC PowerShell-Set AzureRmRoleDefinition – képernyőkép](./media/role-based-access-control-manage-access-powershell/3-set-azurermroledefinition-2.png)

### <a name="modify-role-with-json-template"></a><span data-ttu-id="da99c-182">Szerepkör JSON-sablon módosítása</span><span class="sxs-lookup"><span data-stu-id="da99c-182">Modify role with JSON template</span></span>
<span data-ttu-id="da99c-183">Hello előző JSON-sablon használatával könnyen módosíthatja egy meglévő egyéni szerepkör tooadd vagy távolítsa el a műveletek.</span><span class="sxs-lookup"><span data-stu-id="da99c-183">Using hello previous JSON template, you can easily modify an existing custom role tooadd or remove Actions.</span></span> <span data-ttu-id="da99c-184">Hello JSON-sablont módosítani, és adja hozzá a hello olvasási művelet, a hálózatkezeléshez, ahogy az alábbi példa hello.</span><span class="sxs-lookup"><span data-stu-id="da99c-184">Update hello JSON template and add hello read action for networking as shown in hello following example.</span></span> <span data-ttu-id="da99c-185">nincsenek összesítve alkalmazott tooan meglévő definíciót, ami azt jelenti, hogy hello szerepkör jelenik meg, pontosan hello sablonban megadott hello definíciók hello sablon szerepel.</span><span class="sxs-lookup"><span data-stu-id="da99c-185">hello definitions listed in hello template are not cumulatively applied tooan existing definition, meaning that hello role appears exactly as you specify in hello template.</span></span> <span data-ttu-id="da99c-186">Meg kell tooupdate hello azonosító mezőben hello azonosítójú hello szerepkör is.</span><span class="sxs-lookup"><span data-stu-id="da99c-186">You also need tooupdate hello Id field with hello ID of hello role.</span></span> <span data-ttu-id="da99c-187">Ha még nem meg arról, hogy mi az az érték, használhatja a hello `Get-AzureRmRoleDefinition` parancsmag tooget ezt az információt.</span><span class="sxs-lookup"><span data-stu-id="da99c-187">If you aren't sure what this value is, you can use hello `Get-AzureRmRoleDefinition` cmdlet tooget this information.</span></span>

```
{
  "Name": "Custom Role 1",
  "Id": "acce7ded-2559-449d-bcd5-e9604e50bad1",
  "IsCustom": true,
  "Description": "Allows for read access tooAzure storage and compute resources and access toosupport",
  "Actions": [
    "Microsoft.Compute/*/read",
    "Microsoft.Storage/*/read",
    "Microsoft.Network/*/read",
    "Microsoft.Support/*"
  ],
  "NotActions": [
  ],
  "AssignableScopes": [
    "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
    "/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624"
  ]
}
```

<span data-ttu-id="da99c-188">tooupdate hello meglévő szerepkör, futtassa a következő PowerShell-paranccsal hello:</span><span class="sxs-lookup"><span data-stu-id="da99c-188">tooupdate hello existing role, run hello following PowerShell command:</span></span>
```
Set-AzureRmRoleDefinition -InputFile "C:\CustomRoles\customrole1.json"
```

## <a name="delete-a-custom-role"></a><span data-ttu-id="da99c-189">Egyéni szerepkör törléséhez</span><span class="sxs-lookup"><span data-stu-id="da99c-189">Delete a custom role</span></span>
<span data-ttu-id="da99c-190">egy egyéni biztonsági szerepkört, toodelete hello használata `Remove-AzureRmRoleDefinition` parancsot.</span><span class="sxs-lookup"><span data-stu-id="da99c-190">toodelete a custom role, use hello `Remove-AzureRmRoleDefinition` command.</span></span>

<span data-ttu-id="da99c-191">hello következő példában eltávolítjuk hello *virtuális gépet üzemeltető* egyéni biztonsági szerepkört.</span><span class="sxs-lookup"><span data-stu-id="da99c-191">hello following example removes hello *Virtual Machine Operator* custom role.</span></span>

```
Get-AzureRmRoleDefinition "Virtual Machine Operator"

Get-AzureRmRoleDefinition "Virtual Machine Operator" | Remove-AzureRmRoleDefinition
```

![Az RBAC PowerShell-Remove AzureRmRoleDefinition – képernyőkép](./media/role-based-access-control-manage-access-powershell/4-remove-azurermroledefinition.png)

## <a name="list-custom-roles"></a><span data-ttu-id="da99c-193">Egyéni szerepkörök listája</span><span class="sxs-lookup"><span data-stu-id="da99c-193">List custom roles</span></span>
<span data-ttu-id="da99c-194">toolist hello szerepkörök, amelyek rendelhető hozzá hatókör, használja a hello `Get-AzureRmRoleDefinition` parancsot.</span><span class="sxs-lookup"><span data-stu-id="da99c-194">toolist hello roles that are available for assignment at a scope, use hello `Get-AzureRmRoleDefinition` command.</span></span>

<span data-ttu-id="da99c-195">a következő példa hello hello kiválasztott előfizetésben kiosztására használható összes szerepkörtől sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="da99c-195">hello following example lists all roles that are available for assignment in hello selected subscription.</span></span>

```
Get-AzureRmRoleDefinition | FT Name, IsCustom
```

![Az RBAC PowerShell-Get AzureRmRoleDefinition – képernyőkép](./media/role-based-access-control-manage-access-powershell/5-get-azurermroledefinition-1.png)

<span data-ttu-id="da99c-197">A következő példa hello, hello *virtuális gépet üzemeltető* egyéni szerepkör nem érhető el a hello *Production4* előfizetés mert, hogy az előfizetés nem hello  **AssignableScopes** hello szerepkör.</span><span class="sxs-lookup"><span data-stu-id="da99c-197">In hello following example, hello *Virtual Machine Operator* custom role isn’t available in hello *Production4* subscription because that subscription isn’t in hello **AssignableScopes** of hello role.</span></span>

![Az RBAC PowerShell-Get AzureRmRoleDefinition – képernyőkép](./media/role-based-access-control-manage-access-powershell/5-get-azurermroledefinition2.png)

## <a name="see-also"></a><span data-ttu-id="da99c-199">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="da99c-199">See also</span></span>
* <span data-ttu-id="da99c-200">[Using Azure PowerShell használata az Azure Resource Manager eszközzel](../powershell-azure-resource-manager.md)
  [!INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]</span><span class="sxs-lookup"><span data-stu-id="da99c-200">[Using Azure PowerShell with Azure Resource Manager](../powershell-azure-resource-manager.md)
[!INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]</span></span>

