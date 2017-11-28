---
title: "Az Azure PowerShell szerepköralapú hozzáférés-vezérlés (RBAC) kezelése |} Microsoft Docs"
description: "Hogyan kezelheti az Azure PowerShell, beleértve a szerepköröket, a szerepkörök hozzárendelése és a szerepkör-hozzárendelések törlése RBAC."
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
ms.openlocfilehash: d7b11df21650b5cb27f9c3dd8306f8d12664185e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="manage-role-based-access-control-with-azure-powershell"></a><span data-ttu-id="e9d49-103">Szerepköralapú hozzáférés-vezérlés kezelése az Azure PowerShell-lel</span><span class="sxs-lookup"><span data-stu-id="e9d49-103">Manage Role-Based Access Control with Azure PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="e9d49-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="e9d49-104">PowerShell</span></span>](role-based-access-control-manage-access-powershell.md)
> * [<span data-ttu-id="e9d49-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="e9d49-105">Azure CLI</span></span>](role-based-access-control-manage-access-azure-cli.md)
> * [<span data-ttu-id="e9d49-106">REST API</span><span class="sxs-lookup"><span data-stu-id="e9d49-106">REST API</span></span>](role-based-access-control-manage-access-rest.md)

<span data-ttu-id="e9d49-107">Szerepköralapú hozzáférés-vezérlést (RBAC) az Azure portálon, és az Azure erőforrás-kezelési API segítségével hozzáférésének kezelése az előfizetéshez a minden részletre kiterjedő szinten.</span><span class="sxs-lookup"><span data-stu-id="e9d49-107">You can use Role-Based Access Control (RBAC) in the Azure portal and Azure Resource Management API to manage access to your subscription at a fine-grained level.</span></span> <span data-ttu-id="e9d49-108">Ez a szolgáltatás egyes szerepkörök hozzárendelése el egy adott hatókörhöz szerint engedélyezheti a hozzáférést az Active Directory felhasználók, csoportok vagy szolgáltatásnevekről.</span><span class="sxs-lookup"><span data-stu-id="e9d49-108">With this feature, you can grant access for Active Directory users, groups, or service principals by assigning some roles to them at a particular scope.</span></span>

<span data-ttu-id="e9d49-109">PowerShell kezeléséhez RBAC használata előtt kell a következő előfeltételek teljesülését:</span><span class="sxs-lookup"><span data-stu-id="e9d49-109">Before you can use PowerShell to manage RBAC, you need the following prerequisites:</span></span>

* <span data-ttu-id="e9d49-110">Az Azure PowerShell 0.8.8 verzió vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="e9d49-110">Azure PowerShell version 0.8.8 or later.</span></span> <span data-ttu-id="e9d49-111">Telepítse a legújabb verziót, és társítsa azt az Azure-előfizetése, [telepítése és konfigurálása az Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="e9d49-111">To install the latest version and associate it with your Azure subscription, see [how to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="e9d49-112">Az Azure Resource Manager parancsmagok.</span><span class="sxs-lookup"><span data-stu-id="e9d49-112">Azure Resource Manager cmdlets.</span></span> <span data-ttu-id="e9d49-113">Telepítse a [Azure Resource Manager parancsmagjainak](/powershell/azure/overview) a PowerShellben.</span><span class="sxs-lookup"><span data-stu-id="e9d49-113">Install the [Azure Resource Manager cmdlets](/powershell/azure/overview) in PowerShell.</span></span>

## <a name="list-roles"></a><span data-ttu-id="e9d49-114">Lista szerepkörök</span><span class="sxs-lookup"><span data-stu-id="e9d49-114">List roles</span></span>
### <a name="list-all-available-roles"></a><span data-ttu-id="e9d49-115">Elérhető szerepkörök felsorolása</span><span class="sxs-lookup"><span data-stu-id="e9d49-115">List all available roles</span></span>
<span data-ttu-id="e9d49-116">A lista RBAC szerepkörökhöz kiosztására használható, és a műveletek, amelyhez azokat hozzáférést, a vizsgálandó használó `Get-AzureRmRoleDefinition`.</span><span class="sxs-lookup"><span data-stu-id="e9d49-116">To list RBAC roles that are available for assignment and to inspect the operations to which they grant access, use `Get-AzureRmRoleDefinition`.</span></span>

```
Get-AzureRmRoleDefinition | FT Name, Description
```

![Az RBAC PowerShell-Get AzureRmRoleDefinition – képernyőkép](./media/role-based-access-control-manage-access-powershell/1-get-azure-rm-role-definition1.png)

### <a name="list-actions-of-a-role"></a><span data-ttu-id="e9d49-118">Egy szerepkör lista műveletek</span><span class="sxs-lookup"><span data-stu-id="e9d49-118">List actions of a role</span></span>
<span data-ttu-id="e9d49-119">Kilistázhatja az adott szerepkörhöz műveletek `Get-AzureRmRoleDefinition <role name>`.</span><span class="sxs-lookup"><span data-stu-id="e9d49-119">To list the actions for a specific role, use `Get-AzureRmRoleDefinition <role name>`.</span></span>

```
Get-AzureRmRoleDefinition Contributor | FL Actions, NotActions

(Get-AzureRmRoleDefinition "Virtual Machine Contributor").Actions
```

![Az RBAC PowerShell-Get AzureRmRoleDefinition egy adott szerepkör esetében – képernyőkép](./media/role-based-access-control-manage-access-powershell/1-get-azure-rm-role-definition2.png)

## <a name="see-who-has-access"></a><span data-ttu-id="e9d49-121">Lásd a kinek van hozzáférése:</span><span class="sxs-lookup"><span data-stu-id="e9d49-121">See who has access</span></span>
<span data-ttu-id="e9d49-122">A listában a Szerepalapú hozzáférés-hozzárendelések használja `Get-AzureRmRoleAssignment`.</span><span class="sxs-lookup"><span data-stu-id="e9d49-122">To list RBAC access assignments, use `Get-AzureRmRoleAssignment`.</span></span>

### <a name="list-role-assignments-at-a-specific-scope"></a><span data-ttu-id="e9d49-123">Szerepkör-hozzárendelések listáját egy adott hatókörhöz</span><span class="sxs-lookup"><span data-stu-id="e9d49-123">List role assignments at a specific scope</span></span>
<span data-ttu-id="e9d49-124">A megadott előfizetés, a erőforráscsoport vagy az erőforrás hozzáférés-hozzárendelések tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="e9d49-124">You can see all the access assignments for a specified subscription, resource group, or resource.</span></span> <span data-ttu-id="e9d49-125">Például az összes aktív hozzárendelése egy erőforráscsoport, használja a `Get-AzureRmRoleAssignment -ResourceGroupName <resource group name>`.</span><span class="sxs-lookup"><span data-stu-id="e9d49-125">For example, to see the all the active assignments for a resource group, use `Get-AzureRmRoleAssignment -ResourceGroupName <resource group name>`.</span></span>

```
Get-AzureRmRoleAssignment -ResourceGroupName Pharma-Sales-ProjectForcast | FL DisplayName, RoleDefinitionName, Scope
```

![Az RBAC - Get-AzureRmRoleAssignment erőforráscsoport - PowerShell képernyőképe](./media/role-based-access-control-manage-access-powershell/4-get-azure-rm-role-assignment1.png)

### <a name="list-roles-assigned-to-a-user"></a><span data-ttu-id="e9d49-127">A felhasználóhoz rendelt lista szerepkörök</span><span class="sxs-lookup"><span data-stu-id="e9d49-127">List roles assigned to a user</span></span>
<span data-ttu-id="e9d49-128">Kilistázhatja az adott felhasználóhoz rendelt összes szerepkör és a szerepköröket, amelyek a csoportok, amelyhez a felhasználó tartozik `Get-AzureRmRoleAssignment -SignInName <User email> -ExpandPrincipalGroups`.</span><span class="sxs-lookup"><span data-stu-id="e9d49-128">To list all the roles that are assigned to a specified user and the roles that are assigned to the groups to which the user belongs, use `Get-AzureRmRoleAssignment -SignInName <User email> -ExpandPrincipalGroups`.</span></span>

```
Get-AzureRmRoleAssignment -SignInName sameert@aaddemo.com | FL DisplayName, RoleDefinitionName, Scope

Get-AzureRmRoleAssignment -SignInName sameert@aaddemo.com -ExpandPrincipalGroups | FL DisplayName, RoleDefinitionName, Scope
```

![Az RBAC PowerShell-Get AzureRmRoleAssignment egy felhasználó számára – képernyőkép](./media/role-based-access-control-manage-access-powershell/4-get-azure-rm-role-assignment2.png)

### <a name="list-classic-service-administrator-and-coadmin-role-assignments"></a><span data-ttu-id="e9d49-130">Lista klasszikus szolgáltatás-rendszergazda és a szerepkör-hozzárendelések társfelügyeletű</span><span class="sxs-lookup"><span data-stu-id="e9d49-130">List classic service administrator and coadmin role assignments</span></span>
<span data-ttu-id="e9d49-131">Lista access-hozzárendelések a hagyományos előfizetés rendszergazdai és coadministrators, használja:</span><span class="sxs-lookup"><span data-stu-id="e9d49-131">To list access assignments for the classic subscription administrator and coadministrators, use:</span></span>

    Get-AzureRmRoleAssignment -IncludeClassicAdministrators

## <a name="grant-access"></a><span data-ttu-id="e9d49-132">Hozzáférés biztosítása</span><span class="sxs-lookup"><span data-stu-id="e9d49-132">Grant access</span></span>
### <a name="search-for-object-ids"></a><span data-ttu-id="e9d49-133">Objektumazonosítók keresése</span><span class="sxs-lookup"><span data-stu-id="e9d49-133">Search for object IDs</span></span>
<span data-ttu-id="e9d49-134">A szerepkör hozzárendeléséhez kell azonosítani az objektum (felhasználó, csoport vagy alkalmazás) és a hatókör.</span><span class="sxs-lookup"><span data-stu-id="e9d49-134">To assign a role, you need to identify both the object (user, group, or application) and the scope.</span></span>

<span data-ttu-id="e9d49-135">Ha az előfizetés-azonosító nem tudja, megtalálja a a **előfizetések** panel az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="e9d49-135">If you don't know the subscription ID, you can find it in the **Subscriptions** blade on the Azure portal.</span></span> <span data-ttu-id="e9d49-136">Az előfizetés-azonosító lekérdezése, lásd: [Get-AzureSubscription](/powershell/module/azure/get-azuresubscription?view=azuresmps-3.7.0) az MSDN Webhelyén.</span><span class="sxs-lookup"><span data-stu-id="e9d49-136">To learn how to query for the subscription ID, see [Get-AzureSubscription](/powershell/module/azure/get-azuresubscription?view=azuresmps-3.7.0) on MSDN.</span></span>

<span data-ttu-id="e9d49-137">Az objektum azonosítója az Azure AD-csoport, amelyet:</span><span class="sxs-lookup"><span data-stu-id="e9d49-137">To get the object ID for an Azure AD group, use:</span></span>

    Get-AzureRmADGroup -SearchString <group name in quotes>

<span data-ttu-id="e9d49-138">Az objektum azonosítója az Azure AD szolgáltatás egyszerű vagy az alkalmazás használatához:</span><span class="sxs-lookup"><span data-stu-id="e9d49-138">To get the object ID for an Azure AD service principal or application, use:</span></span>

    Get-AzureRmADServicePrincipal -SearchString <service name in quotes>

### <a name="assign-a-role-to-an-application-at-the-subscription-scope"></a><span data-ttu-id="e9d49-139">Az alkalmazást az előfizetési hatókört szerepkör hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="e9d49-139">Assign a role to an application at the subscription scope</span></span>
<span data-ttu-id="e9d49-140">Az előfizetés hatókörből alkalmazáshoz való hozzáférés biztosításához használja:</span><span class="sxs-lookup"><span data-stu-id="e9d49-140">To grant access to an application at the subscription scope, use:</span></span>

    New-AzureRmRoleAssignment -ObjectId <application id> -RoleDefinitionName <role name> -Scope <subscription id>

![Az RBAC PowerShell új AzureRmRoleAssignment – képernyőkép](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment2.png)

### <a name="assign-a-role-to-a-user-at-the-resource-group-scope"></a><span data-ttu-id="e9d49-142">A szerepkör hozzárendelése egy felhasználóhoz a erőforrás csoport hatóköre:</span><span class="sxs-lookup"><span data-stu-id="e9d49-142">Assign a role to a user at the resource group scope</span></span>
<span data-ttu-id="e9d49-143">Hozzáférés biztosítása a felhasználók a erőforrás hatókörét, használja:</span><span class="sxs-lookup"><span data-stu-id="e9d49-143">To grant access to a user at the resource group scope, use:</span></span>

    New-AzureRmRoleAssignment -SignInName <email of user> -RoleDefinitionName <role name in quotes> -ResourceGroupName <resource group name>

![Az RBAC PowerShell új AzureRmRoleAssignment – képernyőkép](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment3.png)

### <a name="assign-a-role-to-a-group-at-the-resource-scope"></a><span data-ttu-id="e9d49-145">A szerepkör hozzárendelése a erőforrás hatókörben csoporthoz</span><span class="sxs-lookup"><span data-stu-id="e9d49-145">Assign a role to a group at the resource scope</span></span>
<span data-ttu-id="e9d49-146">Hozzáférés biztosítása a erőforrás hatókörben egy csoportot, használja:</span><span class="sxs-lookup"><span data-stu-id="e9d49-146">To grant access to a group at the resource scope, use:</span></span>

    New-AzureRmRoleAssignment -ObjectId <object id> -RoleDefinitionName <role name in quotes> -ResourceName <resource name> -ResourceType <resource type> -ParentResource <parent resource> -ResourceGroupName <resource group name>

![Az RBAC PowerShell új AzureRmRoleAssignment – képernyőkép](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment4.png)

## <a name="remove-access"></a><span data-ttu-id="e9d49-148">Megszünteti a hozzáférést</span><span class="sxs-lookup"><span data-stu-id="e9d49-148">Remove access</span></span>
<span data-ttu-id="e9d49-149">A felhasználók, csoportok és alkalmazások elérésének eltávolításához használja:</span><span class="sxs-lookup"><span data-stu-id="e9d49-149">To remove access for users, groups, and applications, use:</span></span>

    Remove-AzureRmRoleAssignment -ObjectId <object id> -RoleDefinitionName <role name> -Scope <scope such as subscription id>

![Az RBAC PowerShell-Remove AzureRmRoleAssignment – képernyőkép](./media/role-based-access-control-manage-access-powershell/3-remove-azure-rm-role-assignment.png)

## <a name="create-a-custom-role"></a><span data-ttu-id="e9d49-151">Egyéni szerepkör létrehozása</span><span class="sxs-lookup"><span data-stu-id="e9d49-151">Create a custom role</span></span>
<span data-ttu-id="e9d49-152">Egy egyéni biztonsági szerepkört hozhat létre a ```New-AzureRmRoleDefinition``` parancsot.</span><span class="sxs-lookup"><span data-stu-id="e9d49-152">To create a custom role, use the ```New-AzureRmRoleDefinition``` command.</span></span> <span data-ttu-id="e9d49-153">Kétféleképpen a szerkezetének kialakítása a szerepkörre, PSRoleDefinitionObject vagy egy JSON-sablon használatával.</span><span class="sxs-lookup"><span data-stu-id="e9d49-153">There are two methods of structuring the role, using PSRoleDefinitionObject or a JSON template.</span></span> 

## <a name="get-actions-for-a-resource-provider"></a><span data-ttu-id="e9d49-154">Egy erőforrás-szolgáltató műveleteinek beolvasása</span><span class="sxs-lookup"><span data-stu-id="e9d49-154">Get Actions for a Resource Provider</span></span>
<span data-ttu-id="e9d49-155">Ha teljesen új egyéni szerepkörök hoz létre, fontos tudni, hogy az erőforrás-szolgáltató az összes lehetséges műveletet.</span><span class="sxs-lookup"><span data-stu-id="e9d49-155">When You are creating custom roles from scratch, it is important to know all the possible operations from the resource providers.</span></span>
<span data-ttu-id="e9d49-156">Használja a ```Get-AzureRMProviderOperation``` parancs használatával beszerezheti az ezeket az információkat.</span><span class="sxs-lookup"><span data-stu-id="e9d49-156">Use the ```Get-AzureRMProviderOperation``` command to get this information.</span></span>
<span data-ttu-id="e9d49-157">Például ha ellenőrizni kívánja az elérhető műveletek a virtuális gép használja ezt a parancsot:</span><span class="sxs-lookup"><span data-stu-id="e9d49-157">For example, if you want to check all the available operations for virtual Machine use this command:</span></span>

```
Get-AzureRMProviderOperation "Microsoft.Compute/virtualMachines/*" | FT OperationName, Operation , Description -AutoSize
```

### <a name="create-role-with-psroledefinitionobject"></a><span data-ttu-id="e9d49-158">PSRoleDefinitionObject szerepkör létrehozása</span><span class="sxs-lookup"><span data-stu-id="e9d49-158">Create role with PSRoleDefinitionObject</span></span>
<span data-ttu-id="e9d49-159">Amikor a PowerShell segítségével hozzon létre egy egyéni biztonsági szerepkört, új, vagy próbálkozzon a [beépített szerepkörök](role-based-access-built-in-roles.md) kiindulási pontként.</span><span class="sxs-lookup"><span data-stu-id="e9d49-159">When you use PowerShell to create a custom role, you can start from scratch or use one of the [built-in roles](role-based-access-built-in-roles.md) as a starting point.</span></span> <span data-ttu-id="e9d49-160">A jelen szakaszban ismertetett példa egy beépített szerepkör kezdődik, és majd testreszabása további engedélyekkel.</span><span class="sxs-lookup"><span data-stu-id="e9d49-160">The example in this section starts with a built-in role and then customizes it with more privileges.</span></span> <span data-ttu-id="e9d49-161">Szerkessze a attribútumokat kíván hozzáadni a *műveletek*, *notActions*, vagy *hatókörök* , és mentse a módosításokat egy új szerepkörként.</span><span class="sxs-lookup"><span data-stu-id="e9d49-161">Edit the attributes to add the *Actions*, *notActions*, or *scopes* that you want, and then save the changes as a new role.</span></span>

<span data-ttu-id="e9d49-162">Az alábbi példa kezdődik-e a *virtuális gép közreműködő* szerepkör és a által használt egyéni szerepkör létrehozása, amely nevű *virtuális gépet üzemeltető*.</span><span class="sxs-lookup"><span data-stu-id="e9d49-162">The following example starts with the *Virtual Machine Contributor* role and uses that to create a custom role called *Virtual Machine Operator*.</span></span> <span data-ttu-id="e9d49-163">Az új szerepkör hozzáférést biztosít az összes olvasási műveletek a *Microsoft.Compute*, *Microsoft.Storage*, és *Microsoft.Network* erőforrás szolgáltatók és biztosít hozzáférést indítása , indítsa újra, és a virtuális gépek figyelése.</span><span class="sxs-lookup"><span data-stu-id="e9d49-163">The new role grants access to all read operations of *Microsoft.Compute*, *Microsoft.Storage*, and *Microsoft.Network* resource providers and grants access to start, restart, and monitor virtual machines.</span></span> <span data-ttu-id="e9d49-164">Az egyéni biztonsági szerepkört is használható két előfizetésekhez.</span><span class="sxs-lookup"><span data-stu-id="e9d49-164">The custom role can be used in two subscriptions.</span></span>

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

### <a name="create-role-with-json-template"></a><span data-ttu-id="e9d49-166">A JSON-sablon szerepkör létrehozása</span><span class="sxs-lookup"><span data-stu-id="e9d49-166">Create role with JSON template</span></span>
<span data-ttu-id="e9d49-167">A JSON-sablon az egyéni szerepkör használható legyen a forrás-definíció.</span><span class="sxs-lookup"><span data-stu-id="e9d49-167">A JSON template can be used as the source definition for the custom role.</span></span> <span data-ttu-id="e9d49-168">Az alábbi példa létrehoz egy egyéni biztonsági szerepkört, amely olvasási hozzáférést biztosít a tárolási és számítási erőforrásokat, hozzáférés támogatásához, és adja a szerepkört két előfizetések.</span><span class="sxs-lookup"><span data-stu-id="e9d49-168">The following example creates a custom role that allows read access to storage and compute resources, access to support, and adds that role to two subscriptions.</span></span> <span data-ttu-id="e9d49-169">Hozzon létre egy új fájlt `C:\CustomRoles\customrole1.json` a következő példa a.</span><span class="sxs-lookup"><span data-stu-id="e9d49-169">Create a new file `C:\CustomRoles\customrole1.json` with the following example.</span></span> <span data-ttu-id="e9d49-170">A azonosítóját kell beállítani. `null` a kezdeti szerepkör létrehozásakor egy új ID automatikusan létrejön.</span><span class="sxs-lookup"><span data-stu-id="e9d49-170">The Id should be set to `null` on initial role creation as a new ID is generated automatically.</span></span> 

```
{
  "Name": "Custom Role 1",
  "Id": null,
  "IsCustom": true,
  "Description": "Allows for read access to Azure storage and compute resources and access to support",
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
<span data-ttu-id="e9d49-171">Adja hozzá a szerepkört az előfizetés, futtassa a következő PowerShell-parancsot:</span><span class="sxs-lookup"><span data-stu-id="e9d49-171">To add the role to the subscriptions, run the following PowerShell command:</span></span>
```
New-AzureRmRoleDefinition -InputFile "C:\CustomRoles\customrole1.json"
```

## <a name="modify-a-custom-role"></a><span data-ttu-id="e9d49-172">Egyéni szerepkör módosítása</span><span class="sxs-lookup"><span data-stu-id="e9d49-172">Modify a custom role</span></span>
<span data-ttu-id="e9d49-173">Hasonló egyéni szerepkör létrehozása, módosíthatja egy meglévő egyéni szerepkört a PSRoleDefinitionObject vagy egy JSON-sablon használatával.</span><span class="sxs-lookup"><span data-stu-id="e9d49-173">Similar to creating a custom role, you can modify an existing custom role using either the PSRoleDefinitionObject or a JSON template.</span></span>

### <a name="modify-role-with-psroledefinitionobject"></a><span data-ttu-id="e9d49-174">A PSRoleDefinitionObject szerepkör módosítása</span><span class="sxs-lookup"><span data-stu-id="e9d49-174">Modify role with PSRoleDefinitionObject</span></span>
<span data-ttu-id="e9d49-175">Szeretné módosítani egy egyéni biztonsági szerepkört, először használja a `Get-AzureRmRoleDefinition` parancs beolvasása a szerepkör-definíció.</span><span class="sxs-lookup"><span data-stu-id="e9d49-175">To modify a custom role, first, use the `Get-AzureRmRoleDefinition` command to retrieve the role definition.</span></span> <span data-ttu-id="e9d49-176">A szerepkör-definíció, végezze el a szükséges módosításokat.</span><span class="sxs-lookup"><span data-stu-id="e9d49-176">Second, make the desired changes to the role definition.</span></span> <span data-ttu-id="e9d49-177">Végül a `Set-AzureRmRoleDefinition` parancs menteni a módosított szerepkör-definíció.</span><span class="sxs-lookup"><span data-stu-id="e9d49-177">Finally, use the `Set-AzureRmRoleDefinition` command to save the modified role definition.</span></span>

<span data-ttu-id="e9d49-178">A következő példakóddal felveheti a `Microsoft.Insights/diagnosticSettings/*` művelet a *virtuális gépet üzemeltető* egyéni biztonsági szerepkört.</span><span class="sxs-lookup"><span data-stu-id="e9d49-178">The following example adds the `Microsoft.Insights/diagnosticSettings/*` operation to the *Virtual Machine Operator* custom role.</span></span>

```
$role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
$role.Actions.Add("Microsoft.Insights/diagnosticSettings/*")
Set-AzureRmRoleDefinition -Role $role
```

![Az RBAC PowerShell-Set AzureRmRoleDefinition – képernyőkép](./media/role-based-access-control-manage-access-powershell/3-set-azurermroledefinition-1.png)

<span data-ttu-id="e9d49-180">A következő példa egy Azure-előfizetés hozzáadása a hozzárendelhető hatókörök a *virtuális gépet üzemeltető* egyéni biztonsági szerepkört.</span><span class="sxs-lookup"><span data-stu-id="e9d49-180">The following example adds an Azure subscription to the assignable scopes of the *Virtual Machine Operator* custom role.</span></span>

```
Get-AzureRmSubscription - SubscriptionName Production3

$role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
$role.AssignableScopes.Add("/subscriptions/34370e90-ac4a-4bf9-821f-85eeedead1a2")
Set-AzureRmRoleDefinition -Role $role
```

![Az RBAC PowerShell-Set AzureRmRoleDefinition – képernyőkép](./media/role-based-access-control-manage-access-powershell/3-set-azurermroledefinition-2.png)

### <a name="modify-role-with-json-template"></a><span data-ttu-id="e9d49-182">Szerepkör JSON-sablon módosítása</span><span class="sxs-lookup"><span data-stu-id="e9d49-182">Modify role with JSON template</span></span>
<span data-ttu-id="e9d49-183">Az előző JSON-sablon használatával könnyen módosíthatja egy meglévő egyéni szerepkör hozzáadásához vagy eltávolításához műveletek.</span><span class="sxs-lookup"><span data-stu-id="e9d49-183">Using the previous JSON template, you can easily modify an existing custom role to add or remove Actions.</span></span> <span data-ttu-id="e9d49-184">A JSON-sablont módosítani, és adja hozzá az olvasási művelet a hálózatkezeléshez, a következő példában látható módon.</span><span class="sxs-lookup"><span data-stu-id="e9d49-184">Update the JSON template and add the read action for networking as shown in the following example.</span></span> <span data-ttu-id="e9d49-185">A sablon szerepel a definíciók összesítve nem érvényesek egy meglévő definíciójához, ami azt jelenti, hogy a szerepkör pontosan úgy adja meg, ha a sablon megjelenik-e.</span><span class="sxs-lookup"><span data-stu-id="e9d49-185">The definitions listed in the template are not cumulatively applied to an existing definition, meaning that the role appears exactly as you specify in the template.</span></span> <span data-ttu-id="e9d49-186">Is módosítania az azonosítót tartalmazó mezőt, azonosító: a szerepkör.</span><span class="sxs-lookup"><span data-stu-id="e9d49-186">You also need to update the Id field with the ID of the role.</span></span> <span data-ttu-id="e9d49-187">Ha nem tudja biztosan az értéket nem, akkor használhatja a `Get-AzureRmRoleDefinition` parancsmagot, hogy megkapja ezt az információt.</span><span class="sxs-lookup"><span data-stu-id="e9d49-187">If you aren't sure what this value is, you can use the `Get-AzureRmRoleDefinition` cmdlet to get this information.</span></span>

```
{
  "Name": "Custom Role 1",
  "Id": "acce7ded-2559-449d-bcd5-e9604e50bad1",
  "IsCustom": true,
  "Description": "Allows for read access to Azure storage and compute resources and access to support",
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

<span data-ttu-id="e9d49-188">A meglévő szerepkör frissítéséhez futtassa a következő PowerShell-parancsot:</span><span class="sxs-lookup"><span data-stu-id="e9d49-188">To update the existing role, run the following PowerShell command:</span></span>
```
Set-AzureRmRoleDefinition -InputFile "C:\CustomRoles\customrole1.json"
```

## <a name="delete-a-custom-role"></a><span data-ttu-id="e9d49-189">Egyéni szerepkör törléséhez</span><span class="sxs-lookup"><span data-stu-id="e9d49-189">Delete a custom role</span></span>
<span data-ttu-id="e9d49-190">Egyéni szerepkör törléséhez használja a `Remove-AzureRmRoleDefinition` parancsot.</span><span class="sxs-lookup"><span data-stu-id="e9d49-190">To delete a custom role, use the `Remove-AzureRmRoleDefinition` command.</span></span>

<span data-ttu-id="e9d49-191">A következő példában eltávolítjuk a *virtuális gépet üzemeltető* egyéni biztonsági szerepkört.</span><span class="sxs-lookup"><span data-stu-id="e9d49-191">The following example removes the *Virtual Machine Operator* custom role.</span></span>

```
Get-AzureRmRoleDefinition "Virtual Machine Operator"

Get-AzureRmRoleDefinition "Virtual Machine Operator" | Remove-AzureRmRoleDefinition
```

![Az RBAC PowerShell-Remove AzureRmRoleDefinition – képernyőkép](./media/role-based-access-control-manage-access-powershell/4-remove-azurermroledefinition.png)

## <a name="list-custom-roles"></a><span data-ttu-id="e9d49-193">Egyéni szerepkörök listája</span><span class="sxs-lookup"><span data-stu-id="e9d49-193">List custom roles</span></span>
<span data-ttu-id="e9d49-194">A szerepkörök, amelyek rendelhető hozzá hatókör kilistázhatja a `Get-AzureRmRoleDefinition` parancsot.</span><span class="sxs-lookup"><span data-stu-id="e9d49-194">To list the roles that are available for assignment at a scope, use the `Get-AzureRmRoleDefinition` command.</span></span>

<span data-ttu-id="e9d49-195">Az alábbi példa felsorolja az összes szerepkör, amely a kijelölt előfizetés kiosztására használható.</span><span class="sxs-lookup"><span data-stu-id="e9d49-195">The following example lists all roles that are available for assignment in the selected subscription.</span></span>

```
Get-AzureRmRoleDefinition | FT Name, IsCustom
```

![Az RBAC PowerShell-Get AzureRmRoleDefinition – képernyőkép](./media/role-based-access-control-manage-access-powershell/5-get-azurermroledefinition-1.png)

<span data-ttu-id="e9d49-197">A következő példában a *virtuális gépet üzemeltető* egyéni szerepkör nem érhető el a *Production4* előfizetés, mert az adott előfizetéshez nem szerepel a **AssignableScopes** a szerepkör.</span><span class="sxs-lookup"><span data-stu-id="e9d49-197">In the following example, the *Virtual Machine Operator* custom role isn’t available in the *Production4* subscription because that subscription isn’t in the **AssignableScopes** of the role.</span></span>

![Az RBAC PowerShell-Get AzureRmRoleDefinition – képernyőkép](./media/role-based-access-control-manage-access-powershell/5-get-azurermroledefinition2.png)

## <a name="see-also"></a><span data-ttu-id="e9d49-199">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="e9d49-199">See also</span></span>
* <span data-ttu-id="e9d49-200">[Using Azure PowerShell használata az Azure Resource Manager eszközzel](../powershell-azure-resource-manager.md)
  [!INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]</span><span class="sxs-lookup"><span data-stu-id="e9d49-200">[Using Azure PowerShell with Azure Resource Manager](../powershell-azure-resource-manager.md)
[!INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]</span></span>

