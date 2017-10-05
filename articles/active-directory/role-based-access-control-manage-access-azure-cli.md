---
title: "Az Azure CLI szerepköralapú hozzáférés-vezérlés (RBAC) kezelése |} Microsoft Docs"
description: "Megtudhatja, hogyan kezelheti a szerepköralapú hozzáférés-vezérlést (RBAC) az Azure parancssori felületével, szerepkörök és a szerepkör műveletek listázása és szerepkörök hozzárendelése az előfizetés és az alkalmazás hatókörhöz."
services: active-directory
documentationcenter: 
author: andredm7
manager: femila
ms.assetid: 3483ee01-8177-49e7-b337-4d5cb14f5e32
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/12/2017
ms.author: andredm
ms.reviewer: rqureshi
ms.openlocfilehash: ad644de6d23950e699d99042d27381336626caab
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="manage-role-based-access-control-with-the-azure-command-line-interface"></a><span data-ttu-id="a2cb0-103">Szerepköralapú hozzáférés-vezérlés az Azure parancssori felületével kezelése</span><span class="sxs-lookup"><span data-stu-id="a2cb0-103">Manage Role-Based Access Control with the Azure command-line interface</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="a2cb0-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="a2cb0-104">PowerShell</span></span>](role-based-access-control-manage-access-powershell.md)
> * [<span data-ttu-id="a2cb0-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="a2cb0-105">Azure CLI</span></span>](role-based-access-control-manage-access-azure-cli.md)
> * [<span data-ttu-id="a2cb0-106">REST API</span><span class="sxs-lookup"><span data-stu-id="a2cb0-106">REST API</span></span>](role-based-access-control-manage-access-rest.md)


<span data-ttu-id="a2cb0-107">Szerepköralapú hozzáférés-vezérlést (RBAC) az Azure portálon, és az Azure Resource Manager API segítségével kezelheti az előfizetést és a minden részletre kiterjedő szinten erőforrásokhoz való hozzáférését.</span><span class="sxs-lookup"><span data-stu-id="a2cb0-107">You can use Role-Based Access Control (RBAC) in the Azure portal and Azure Resource Manager API to manage access to your subscription and resources at a fine-grained level.</span></span> <span data-ttu-id="a2cb0-108">Ez a szolgáltatás egyes szerepkörök hozzárendelése el egy adott hatókörhöz szerint engedélyezheti a hozzáférést az Active Directory felhasználók, csoportok vagy szolgáltatásnevekről.</span><span class="sxs-lookup"><span data-stu-id="a2cb0-108">With this feature, you can grant access for Active Directory users, groups, or service principals by assigning some roles to them at a particular scope.</span></span>

<span data-ttu-id="a2cb0-109">Az Azure parancssori felület (CLI) kezelheti az RBAC használata előtt rendelkeznie kell a következő előfeltételek teljesülését:</span><span class="sxs-lookup"><span data-stu-id="a2cb0-109">Before you can use the Azure command-line interface (CLI) to manage RBAC, you must have the following prerequisites:</span></span>

* <span data-ttu-id="a2cb0-110">Az Azure CLI 0.8.8 verzió vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="a2cb0-110">Azure CLI version 0.8.8 or later.</span></span> <span data-ttu-id="a2cb0-111">Telepítse a legújabb verziót, és társítsa azt az Azure-előfizetése, [telepítése és konfigurálása az Azure parancssori felület](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="a2cb0-111">To install the latest version and associate it with your Azure subscription, see [Install and Configure the Azure CLI](../cli-install-nodejs.md).</span></span>
* <span data-ttu-id="a2cb0-112">Az Azure Resource Manager az Azure parancssori felület.</span><span class="sxs-lookup"><span data-stu-id="a2cb0-112">Azure Resource Manager in Azure CLI.</span></span> <span data-ttu-id="a2cb0-113">Ugrás a [az Azure parancssori felület használatával az erőforrás-kezelővel](../xplat-cli-azure-resource-manager.md) további részleteket.</span><span class="sxs-lookup"><span data-stu-id="a2cb0-113">Go to [Using the Azure CLI with the Resource Manager](../xplat-cli-azure-resource-manager.md) for more details.</span></span>

## <a name="list-roles"></a><span data-ttu-id="a2cb0-114">Lista szerepkörök</span><span class="sxs-lookup"><span data-stu-id="a2cb0-114">List roles</span></span>
### <a name="list-all-available-roles"></a><span data-ttu-id="a2cb0-115">Elérhető szerepkörök felsorolása</span><span class="sxs-lookup"><span data-stu-id="a2cb0-115">List all available roles</span></span>
<span data-ttu-id="a2cb0-116">Az elérhető szerepkörök listájában, használja:</span><span class="sxs-lookup"><span data-stu-id="a2cb0-116">To list all available roles, use:</span></span>

        azure role list

<span data-ttu-id="a2cb0-117">A következő példa bemutatja a listája *elérhető szerepkörök*.</span><span class="sxs-lookup"><span data-stu-id="a2cb0-117">The following example shows the list of *all available roles*.</span></span>

```
azure role list --json | jq '.[] | {"roleName":.properties.roleName, "description":.properties.description}'
```

![RBAC Azure - azure szerepkör lista - parancssori képernyőképe](./media/role-based-access-control-manage-access-azure-cli/1-azure-role-list.png)

### <a name="list-actions-of-a-role"></a><span data-ttu-id="a2cb0-119">Egy szerepkör lista műveletek</span><span class="sxs-lookup"><span data-stu-id="a2cb0-119">List actions of a role</span></span>
<span data-ttu-id="a2cb0-120">A műveletek a szerepkörök listájában, használja:</span><span class="sxs-lookup"><span data-stu-id="a2cb0-120">To list the actions of a role, use:</span></span>

    azure role show "<role name>"

<span data-ttu-id="a2cb0-121">A következő példa bemutatja a műveleteket a *közreműködő* és *virtuális gép közreműködő* szerepkörök.</span><span class="sxs-lookup"><span data-stu-id="a2cb0-121">The following example shows the actions of the *Contributor* and *Virtual Machine Contributor* roles.</span></span>

```
azure role show "contributor" --json | jq '.[] | {"Actions":.properties.permissions[0].actions,"NotActions":properties.permissions[0].notActions}'

azure role show "virtual machine contributor" --json | jq '.[] | .properties.permissions[0].actions'
```

![Szerepalapú Azure parancssori - azure szerepkör megjelenítése – képernyőkép](./media/role-based-access-control-manage-access-azure-cli/1-azure-role-show.png)

## <a name="list-access"></a><span data-ttu-id="a2cb0-123">A hozzáférési lista</span><span class="sxs-lookup"><span data-stu-id="a2cb0-123">List access</span></span>
### <a name="list-role-assignments-effective-on-a-resource-group"></a><span data-ttu-id="a2cb0-124">Lista szerepkör-hozzárendelések lép az erőforráscsoport</span><span class="sxs-lookup"><span data-stu-id="a2cb0-124">List role assignments effective on a resource group</span></span>
<span data-ttu-id="a2cb0-125">A szerepkör-hozzárendelések meglévő erőforráscsoport listájában, használja:</span><span class="sxs-lookup"><span data-stu-id="a2cb0-125">To list the role assignments that exist in a resource group, use:</span></span>

    azure role assignment list --resource-group <resource group name>

<span data-ttu-id="a2cb0-126">A következő példa bemutatja a szerepkör-hozzárendelések a *pharma-értékesítési-projecforcast* csoport.</span><span class="sxs-lookup"><span data-stu-id="a2cb0-126">The following example shows the role assignments in the *pharma-sales-projecforcast* group.</span></span>

```
azure role assignment list --resource-group pharma-sales-projecforcast --json | jq '.[] | {"DisplayName":.properties.aADObject.displayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'
```

![Szerepalapú Azure parancssori - társításának listája azure szerepkör csoport – képernyőkép](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-assignment-list-1.png)

### <a name="list-role-assignments-for-a-user"></a><span data-ttu-id="a2cb0-128">Szerepkör-hozzárendelések listáját egy felhasználó számára</span><span class="sxs-lookup"><span data-stu-id="a2cb0-128">List role assignments for a user</span></span>
<span data-ttu-id="a2cb0-129">A szerepkör-hozzárendelések egy adott felhasználó és felhasználói csoportok hozzárendelése listájában, használja:</span><span class="sxs-lookup"><span data-stu-id="a2cb0-129">To list the role assignments for a specific user and the assignments that are assigned to a user's groups, use:</span></span>

    azure role assignment list --signInName <user email>

<span data-ttu-id="a2cb0-130">A parancs módosításával csoportok örökölt szerepkör-hozzárendelések is látható:</span><span class="sxs-lookup"><span data-stu-id="a2cb0-130">You can also see role assignments that are inherited from groups by modifying the command:</span></span>

    azure role assignment list --expandPrincipalGroups --signInName <user email>

<span data-ttu-id="a2cb0-131">A következő példa bemutatja a szerepkör-hozzárendeléseket, kapott a  *sameert@aaddemo.com*  felhasználó.</span><span class="sxs-lookup"><span data-stu-id="a2cb0-131">The following example shows the role assignments that are granted to the *sameert@aaddemo.com* user.</span></span> <span data-ttu-id="a2cb0-132">Ez magában foglalja, amely közvetlenül a felhasználóhoz rendelt szerepkörök és csoportok öröklődtek szerepkörtől.</span><span class="sxs-lookup"><span data-stu-id="a2cb0-132">This includes roles that are assigned directly to the user and roles that are inherited from groups.</span></span>

```
azure role assignment list --signInName sameert@aaddemo.com --json | jq '.[] | {"DisplayName":.properties.aADObject.DisplayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'

azure role assignment list --expandPrincipalGroups --signInName sameert@aaddemo.com --json | jq '.[] | {"DisplayName":.properties.aADObject.DisplayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'
```

![RBAC Azure - hozzárendelés lista azure szerepkör felhasználó - parancssori képernyőképe](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-assignment-list-2.png)

## <a name="grant-access"></a><span data-ttu-id="a2cb0-134">Hozzáférés biztosítása</span><span class="sxs-lookup"><span data-stu-id="a2cb0-134">Grant access</span></span>
<span data-ttu-id="a2cb0-135">Miután azonosította a hozzárendelni kívánt szerepkör hozzáférés biztosításához használja:</span><span class="sxs-lookup"><span data-stu-id="a2cb0-135">To grant access after you have identified the role that you want to assign, use:</span></span>

    azure role assignment create

### <a name="assign-a-role-to-group-at-the-subscription-scope"></a><span data-ttu-id="a2cb0-136">Az előfizetési hatókört a csoport szerepkör hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="a2cb0-136">Assign a role to group at the subscription scope</span></span>
<span data-ttu-id="a2cb0-137">Rendelhet hozzá egy szerepkört az előfizetés hatókörben egy csoportot, használja:</span><span class="sxs-lookup"><span data-stu-id="a2cb0-137">To assign a role to a group at the subscription scope, use:</span></span>

    azure role assignment create --objectId  <group object id> --roleName <name of role> --subscription <subscription> --scope <subscription/subscription id>

<span data-ttu-id="a2cb0-138">Az alábbi példa a *olvasó* szerepkör *Christine Koch Team* , a *előfizetés* hatókör.</span><span class="sxs-lookup"><span data-stu-id="a2cb0-138">The following example assigns the *Reader* role to *Christine Koch's Team* at the *subscription* scope.</span></span>

![Szerepalapú Azure parancssori - azure szerepkör-hozzárendelés létrehozása csoport – képernyőkép](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-1.png)

### <a name="assign-a-role-to-an-application-at-the-subscription-scope"></a><span data-ttu-id="a2cb0-140">Az alkalmazást az előfizetési hatókört szerepkör hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="a2cb0-140">Assign a role to an application at the subscription scope</span></span>
<span data-ttu-id="a2cb0-141">Szerepkör hozzárendelése az előfizetési hatókört az alkalmazást, használja:</span><span class="sxs-lookup"><span data-stu-id="a2cb0-141">To assign a role to an application at the subscription scope, use:</span></span>

    azure role assignment create --objectId  <applications object id> --roleName <name of role> --subscription <subscription> --scope <subscription/subscription id>

<span data-ttu-id="a2cb0-142">A következő példa engedélyezi a *közreműködő* szerepkört egy *az Azure AD* alkalmazást a kijelölt előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="a2cb0-142">The following example grants the *Contributor* role to an *Azure AD* application on the selected subscription.</span></span>

 ![Szerepalapú Azure parancssori - alkalmazás azure szerepkör-hozzárendelés létrehozása](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-2.png)

### <a name="assign-a-role-to-a-user-at-the-resource-group-scope"></a><span data-ttu-id="a2cb0-144">A szerepkör hozzárendelése egy felhasználóhoz a erőforrás csoport hatóköre:</span><span class="sxs-lookup"><span data-stu-id="a2cb0-144">Assign a role to a user at the resource group scope</span></span>
<span data-ttu-id="a2cb0-145">A szerepkör hozzárendelése egy felhasználóhoz a erőforrás csoport hatókörben, használja:</span><span class="sxs-lookup"><span data-stu-id="a2cb0-145">To assign a role to a user at the resource group scope, use:</span></span>

    azure role assignment create --signInName  <user email address> --roleName "<name of role>" --resourceGroup <resource group name>

<span data-ttu-id="a2cb0-146">A következő példa engedélyezi a *virtuális gép közreműködő* szerepkör  *samert@aaddemo.com*  felhasználójának a *Pharma-értékesítési-ProjectForcast* erőforrás csoport hatóköre.</span><span class="sxs-lookup"><span data-stu-id="a2cb0-146">The following example grants the *Virtual Machine Contributor* role to *samert@aaddemo.com* user at the *Pharma-Sales-ProjectForcast* resource group scope.</span></span>

![Az RBAC Azure parancssori - azure szerepkör-hozzárendelés létrehozása felhasználó – képernyőkép](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-3.png)

### <a name="assign-a-role-to-a-group-at-the-resource-scope"></a><span data-ttu-id="a2cb0-148">A szerepkör hozzárendelése a erőforrás hatókörben csoporthoz</span><span class="sxs-lookup"><span data-stu-id="a2cb0-148">Assign a role to a group at the resource scope</span></span>
<span data-ttu-id="a2cb0-149">A szerepkör hozzárendelése egy csoporthoz az erőforrás-hatókörben, használja:</span><span class="sxs-lookup"><span data-stu-id="a2cb0-149">To assign a role to a group at the resource scope, use:</span></span>

    azure role assignment create --objectId <group id> --role "<name of role>" --resource-name <resource group name> --resource-type <resource group type> --parent <resource group parent> --resource-group <resource group>

<span data-ttu-id="a2cb0-150">A következő példa engedélyezi a *virtuális gép közreműködő* szerepkört egy *az Azure AD* csoporthoz egy *alhálózati*.</span><span class="sxs-lookup"><span data-stu-id="a2cb0-150">The following example grants the *Virtual Machine Contributor* role to an *Azure AD* group on a *subnet*.</span></span>

![Szerepalapú Azure parancssori - azure szerepkör-hozzárendelés létrehozása csoport – képernyőkép](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-4.png)

## <a name="remove-access"></a><span data-ttu-id="a2cb0-152">Megszünteti a hozzáférést</span><span class="sxs-lookup"><span data-stu-id="a2cb0-152">Remove access</span></span>
<span data-ttu-id="a2cb0-153">Szerepkör-hozzárendelés eltávolításához használja:</span><span class="sxs-lookup"><span data-stu-id="a2cb0-153">To remove a role assignment, use:</span></span>

    azure role assignment delete --objectId <object id to from which to remove role> --roleName "<role name>"

<span data-ttu-id="a2cb0-154">A következő példában eltávolítjuk a *virtuális gép közreműködő* a szerepkör-hozzárendelés a  *sammert@aaddemo.com*  felhasználó számára a *Pharma-értékesítési-ProjectForcast* erőforrás a csoport.</span><span class="sxs-lookup"><span data-stu-id="a2cb0-154">The following example removes the *Virtual Machine Contributor* role assignment from the *sammert@aaddemo.com* user on the *Pharma-Sales-ProjectForcast* resource group.</span></span>
<span data-ttu-id="a2cb0-155">A példa a szerepkör-hozzárendelés majd eltávolítja az előfizetésben a csoportból.</span><span class="sxs-lookup"><span data-stu-id="a2cb0-155">The example then removes the role assignment from a group on the subscription.</span></span>

![RBAC Azure - azure szerepkör-hozzárendelés törlése - parancssori képernyőképe](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-assignment-delete.png)

## <a name="create-a-custom-role"></a><span data-ttu-id="a2cb0-157">Egyéni szerepkör létrehozása</span><span class="sxs-lookup"><span data-stu-id="a2cb0-157">Create a custom role</span></span>
<span data-ttu-id="a2cb0-158">Segítségével hozhat létre egy egyéni biztonsági szerepkört:</span><span class="sxs-lookup"><span data-stu-id="a2cb0-158">To create a custom role, use:</span></span>

    azure role create --inputfile <file path>

<span data-ttu-id="a2cb0-159">Az alábbi példa létrehoz egy egyéni biztonsági szerepkört nevű *virtuális gépet üzemeltető*.</span><span class="sxs-lookup"><span data-stu-id="a2cb0-159">The following example creates a custom role called *Virtual Machine Operator*.</span></span> <span data-ttu-id="a2cb0-160">Az egyéni szerepkör hozzáférést biztosít az összes olvasási műveletek a *Microsoft.Compute*, *Microsoft.Storage*, és *Microsoft.Network* erőforrás-szolgáltatók és engedélyezi a hozzáférést Indítsa el, és indítsa újra a virtuális gépek figyelése.</span><span class="sxs-lookup"><span data-stu-id="a2cb0-160">This custom role grants access to all read operations of *Microsoft.Compute*, *Microsoft.Storage*, and *Microsoft.Network* resource providers and grants access to start, restart, and monitor virtual machines.</span></span> <span data-ttu-id="a2cb0-161">Az egyéni szerepkör két előfizetések használható.</span><span class="sxs-lookup"><span data-stu-id="a2cb0-161">This custom role can be used in two subscriptions.</span></span> <span data-ttu-id="a2cb0-162">A példa egy JSON-fájl bemenetként.</span><span class="sxs-lookup"><span data-stu-id="a2cb0-162">This example uses a JSON file as an input.</span></span>

![JSON - egyéni szerepkör-definíció – képernyőkép](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-create-1.png)

![Szerepalapú Azure parancssori - azure szerepkör létrehozása – képernyőfelvétel](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-create-2.png)

## <a name="modify-a-custom-role"></a><span data-ttu-id="a2cb0-165">Egyéni szerepkör módosítása</span><span class="sxs-lookup"><span data-stu-id="a2cb0-165">Modify a custom role</span></span>
<span data-ttu-id="a2cb0-166">Ha módosít egy egyéni biztonsági szerepkört, először az a `azure role show` parancsot a szerepkör-definíció lekéréséhez.</span><span class="sxs-lookup"><span data-stu-id="a2cb0-166">To modify a custom role, first use the `azure role show` command to retrieve role definition.</span></span> <span data-ttu-id="a2cb0-167">A szerepkör-definíciós fájlt, végezze el a szükséges módosításokat.</span><span class="sxs-lookup"><span data-stu-id="a2cb0-167">Second, make the desired changes to the role definition file.</span></span> <span data-ttu-id="a2cb0-168">Végül `azure role set` menteni a módosított szerepkör-definíció.</span><span class="sxs-lookup"><span data-stu-id="a2cb0-168">Finally, use `azure role set` to save the modified role definition.</span></span>

    azure role set --inputfile <file path>

<span data-ttu-id="a2cb0-169">A következő példakóddal felveheti a *Microsoft.Insights/diagnosticSettings/* művelet a **műveletek**, és az Azure-előfizetés a **AssignableScopes** , a Virtuális gép operátor egyéni biztonsági szerepkört.</span><span class="sxs-lookup"><span data-stu-id="a2cb0-169">The following example adds the *Microsoft.Insights/diagnosticSettings/* operation to the **Actions**, and an Azure subscription to the **AssignableScopes** of the Virtual Machine Operator custom role.</span></span>

![JSON - módosítása egyéni szerepkör-definíció – képernyőkép](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-set-1.png)

![RBAC Azure - azure szerepkör set - parancssori képernyőképe](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-set2.png)

## <a name="delete-a-custom-role"></a><span data-ttu-id="a2cb0-172">Egyéni szerepkör törléséhez</span><span class="sxs-lookup"><span data-stu-id="a2cb0-172">Delete a custom role</span></span>
<span data-ttu-id="a2cb0-173">Egyéni szerepkör törléséhez először használja a `azure role show` parancsot annak meghatározásához, a **azonosító** a szerepkör.</span><span class="sxs-lookup"><span data-stu-id="a2cb0-173">To delete a custom role, first use the `azure role show` command to determine the **ID** of the role.</span></span> <span data-ttu-id="a2cb0-174">Ezt követően a `azure role delete` parancs megadásával a szerepkör törlése a **azonosító**.</span><span class="sxs-lookup"><span data-stu-id="a2cb0-174">Then, use the `azure role delete` command to delete the role by specifying the **ID**.</span></span>

<span data-ttu-id="a2cb0-175">A következő példában eltávolítjuk a *virtuális gépet üzemeltető* egyéni biztonsági szerepkört.</span><span class="sxs-lookup"><span data-stu-id="a2cb0-175">The following example removes the *Virtual Machine Operator* custom role.</span></span>

![Szerepalapú Azure parancssori - azure szerepkör törlésének – képernyőkép](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-delete.png)

## <a name="list-custom-roles"></a><span data-ttu-id="a2cb0-177">Egyéni szerepkörök listája</span><span class="sxs-lookup"><span data-stu-id="a2cb0-177">List custom roles</span></span>
<span data-ttu-id="a2cb0-178">A szerepkörök, amelyek rendelhető hozzá hatókör kilistázhatja a `azure role list` parancsot.</span><span class="sxs-lookup"><span data-stu-id="a2cb0-178">To list the roles that are available for assignment at a scope, use the `azure role list` command.</span></span>

<span data-ttu-id="a2cb0-179">A következő parancs megjeleníti az összes szerepkör, amely a kijelölt előfizetés kiosztására használható.</span><span class="sxs-lookup"><span data-stu-id="a2cb0-179">The following command lists all roles that are available for assignment in the selected subscription.</span></span>

```
azure role list --json | jq '.[] | {"name":.properties.roleName, type:.properties.type}'
```

![RBAC Azure - azure szerepkör lista - parancssori képernyőképe](./media/role-based-access-control-manage-access-azure-cli/5-azure-role-list1.png)

<span data-ttu-id="a2cb0-181">A következő példában a *virtuális gépet üzemeltető* egyéni szerepkör nem érhető el a *Production4* előfizetés, mert az adott előfizetéshez nem szerepel a **AssignableScopes** a szerepkör.</span><span class="sxs-lookup"><span data-stu-id="a2cb0-181">In the following example, the *Virtual Machine Operator* custom role isn’t available in the *Production4* subscription because that subscription isn’t in the **AssignableScopes** of the role.</span></span>

```
azure role list --json | jq '.[] | if .properties.type == "CustomRole" then .properties.roleName else empty end'
```

![RBAC Azure - egyéni szerepkörök listája azure szerepkör - parancssori képernyőképe](./media/role-based-access-control-manage-access-azure-cli/5-azure-role-list2.png)

## <a name="next-steps"></a><span data-ttu-id="a2cb0-183">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a2cb0-183">Next steps</span></span>
[!INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]

