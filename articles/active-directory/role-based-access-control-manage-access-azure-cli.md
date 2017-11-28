---
title: "aaaManage szerepköralapú hozzáférés-vezérlést (RBAC) Azure parancssori felülettel |} Microsoft Docs"
description: "Ismerje meg, hogyan toomanage szerepköralapú hozzáférés-vezérlés (RBAC) hello Azure parancssori felület listaelem szerepkörök és a szerepkör műveletek és a szerepkörök hozzárendelése toohello előfizetés és az alkalmazás hatókörök."
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
ms.openlocfilehash: 438418e5f6ee9b98908c9c264d516eb722a4e26d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-role-based-access-control-with-hello-azure-command-line-interface"></a><span data-ttu-id="053cd-103">Szerepköralapú hozzáférés-vezérlés az Azure parancssori felület hello kezelése</span><span class="sxs-lookup"><span data-stu-id="053cd-103">Manage Role-Based Access Control with hello Azure command-line interface</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="053cd-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="053cd-104">PowerShell</span></span>](role-based-access-control-manage-access-powershell.md)
> * [<span data-ttu-id="053cd-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="053cd-105">Azure CLI</span></span>](role-based-access-control-manage-access-azure-cli.md)
> * [<span data-ttu-id="053cd-106">REST API</span><span class="sxs-lookup"><span data-stu-id="053cd-106">REST API</span></span>](role-based-access-control-manage-access-rest.md)


<span data-ttu-id="053cd-107">Hello Azure-portál és az Azure Resource Manager API toomanage hozzáférés tooyour előfizetéshez és a részletes szinten erőforrások szerepköralapú hozzáférés-vezérlést (RBAC) is használhatja.</span><span class="sxs-lookup"><span data-stu-id="053cd-107">You can use Role-Based Access Control (RBAC) in hello Azure portal and Azure Resource Manager API toomanage access tooyour subscription and resources at a fine-grained level.</span></span> <span data-ttu-id="053cd-108">Ez a szolgáltatás egyes szerepkörök toothem egy adott hatókör hozzárendelése szerint engedélyezheti a hozzáférést az Active Directory felhasználók, csoportok vagy szolgáltatásnevekről.</span><span class="sxs-lookup"><span data-stu-id="053cd-108">With this feature, you can grant access for Active Directory users, groups, or service principals by assigning some roles toothem at a particular scope.</span></span>

<span data-ttu-id="053cd-109">Hello Azure parancssori felület (CLI) toomanage RBAC használata előtt a következő előfeltételek hello kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="053cd-109">Before you can use hello Azure command-line interface (CLI) toomanage RBAC, you must have hello following prerequisites:</span></span>

* <span data-ttu-id="053cd-110">Az Azure CLI 0.8.8 verzió vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="053cd-110">Azure CLI version 0.8.8 or later.</span></span> <span data-ttu-id="053cd-111">tooinstall hello legújabb verzióját, és rendelje azt az Azure-előfizetéshez, lásd: [telepítése és konfigurálása az Azure parancssori felület hello](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="053cd-111">tooinstall hello latest version and associate it with your Azure subscription, see [Install and Configure hello Azure CLI](../cli-install-nodejs.md).</span></span>
* <span data-ttu-id="053cd-112">Az Azure Resource Manager az Azure parancssori felület.</span><span class="sxs-lookup"><span data-stu-id="053cd-112">Azure Resource Manager in Azure CLI.</span></span> <span data-ttu-id="053cd-113">Nyissa meg túl[Using hello Azure CLI az erőforrás-kezelő hello](../xplat-cli-azure-resource-manager.md) további részleteket.</span><span class="sxs-lookup"><span data-stu-id="053cd-113">Go too[Using hello Azure CLI with hello Resource Manager](../xplat-cli-azure-resource-manager.md) for more details.</span></span>

## <a name="list-roles"></a><span data-ttu-id="053cd-114">Lista szerepkörök</span><span class="sxs-lookup"><span data-stu-id="053cd-114">List roles</span></span>
### <a name="list-all-available-roles"></a><span data-ttu-id="053cd-115">Elérhető szerepkörök felsorolása</span><span class="sxs-lookup"><span data-stu-id="053cd-115">List all available roles</span></span>
<span data-ttu-id="053cd-116">toolist elérhető szerepkörök, használja:</span><span class="sxs-lookup"><span data-stu-id="053cd-116">toolist all available roles, use:</span></span>

        azure role list

<span data-ttu-id="053cd-117">hello következő példa bemutatja hello listája *elérhető szerepkörök*.</span><span class="sxs-lookup"><span data-stu-id="053cd-117">hello following example shows hello list of *all available roles*.</span></span>

```
azure role list --json | jq '.[] | {"roleName":.properties.roleName, "description":.properties.description}'
```

![RBAC Azure - azure szerepkör lista - parancssori képernyőképe](./media/role-based-access-control-manage-access-azure-cli/1-azure-role-list.png)

### <a name="list-actions-of-a-role"></a><span data-ttu-id="053cd-119">Egy szerepkör lista műveletek</span><span class="sxs-lookup"><span data-stu-id="053cd-119">List actions of a role</span></span>
<span data-ttu-id="053cd-120">egy szerepkör toolist hello műveletek használata:</span><span class="sxs-lookup"><span data-stu-id="053cd-120">toolist hello actions of a role, use:</span></span>

    azure role show "<role name>"

<span data-ttu-id="053cd-121">hello alábbi példa azt mutatja, hello hello műveletek *közreműködő* és *virtuális gép közreműködő* szerepkörök.</span><span class="sxs-lookup"><span data-stu-id="053cd-121">hello following example shows hello actions of hello *Contributor* and *Virtual Machine Contributor* roles.</span></span>

```
azure role show "contributor" --json | jq '.[] | {"Actions":.properties.permissions[0].actions,"NotActions":properties.permissions[0].notActions}'

azure role show "virtual machine contributor" --json | jq '.[] | .properties.permissions[0].actions'
```

![Szerepalapú Azure parancssori - azure szerepkör megjelenítése – képernyőkép](./media/role-based-access-control-manage-access-azure-cli/1-azure-role-show.png)

## <a name="list-access"></a><span data-ttu-id="053cd-123">A hozzáférési lista</span><span class="sxs-lookup"><span data-stu-id="053cd-123">List access</span></span>
### <a name="list-role-assignments-effective-on-a-resource-group"></a><span data-ttu-id="053cd-124">Lista szerepkör-hozzárendelések lép az erőforráscsoport</span><span class="sxs-lookup"><span data-stu-id="053cd-124">List role assignments effective on a resource group</span></span>
<span data-ttu-id="053cd-125">toolist hello szerepkör-hozzárendelések meglévő erőforráscsoport használata:</span><span class="sxs-lookup"><span data-stu-id="053cd-125">toolist hello role assignments that exist in a resource group, use:</span></span>

    azure role assignment list --resource-group <resource group name>

<span data-ttu-id="053cd-126">hello alábbi példa azt mutatja hello szerepkör-hozzárendelések a hello *pharma-értékesítési-projecforcast* csoport.</span><span class="sxs-lookup"><span data-stu-id="053cd-126">hello following example shows hello role assignments in hello *pharma-sales-projecforcast* group.</span></span>

```
azure role assignment list --resource-group pharma-sales-projecforcast --json | jq '.[] | {"DisplayName":.properties.aADObject.displayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'
```

![Szerepalapú Azure parancssori - társításának listája azure szerepkör csoport – képernyőkép](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-assignment-list-1.png)

### <a name="list-role-assignments-for-a-user"></a><span data-ttu-id="053cd-128">Szerepkör-hozzárendelések listáját egy felhasználó számára</span><span class="sxs-lookup"><span data-stu-id="053cd-128">List role assignments for a user</span></span>
<span data-ttu-id="053cd-129">toolist hello szerepkör-hozzárendelések egy adott felhasználó és hello hozzárendelések rendelt tooa felhasználói csoportok használata:</span><span class="sxs-lookup"><span data-stu-id="053cd-129">toolist hello role assignments for a specific user and hello assignments that are assigned tooa user's groups, use:</span></span>

    azure role assignment list --signInName <user email>

<span data-ttu-id="053cd-130">Megtekintheti a csoportok hello parancs módosításával örökölt szerepkör-hozzárendelések is:</span><span class="sxs-lookup"><span data-stu-id="053cd-130">You can also see role assignments that are inherited from groups by modifying hello command:</span></span>

    azure role assignment list --expandPrincipalGroups --signInName <user email>

<span data-ttu-id="053cd-131">hello alábbi példa azt mutatja, toohello kapott hello szerepkör-hozzárendelések  *sameert@aaddemo.com*  felhasználó.</span><span class="sxs-lookup"><span data-stu-id="053cd-131">hello following example shows hello role assignments that are granted toohello *sameert@aaddemo.com* user.</span></span> <span data-ttu-id="053cd-132">Ez magában foglalja a hozzárendelt közvetlenül toohello felhasználói szerepkörök és csoportok öröklődtek szerepkörtől.</span><span class="sxs-lookup"><span data-stu-id="053cd-132">This includes roles that are assigned directly toohello user and roles that are inherited from groups.</span></span>

```
azure role assignment list --signInName sameert@aaddemo.com --json | jq '.[] | {"DisplayName":.properties.aADObject.DisplayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'

azure role assignment list --expandPrincipalGroups --signInName sameert@aaddemo.com --json | jq '.[] | {"DisplayName":.properties.aADObject.DisplayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'
```

![RBAC Azure - hozzárendelés lista azure szerepkör felhasználó - parancssori képernyőképe](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-assignment-list-2.png)

## <a name="grant-access"></a><span data-ttu-id="053cd-134">Hozzáférés biztosítása</span><span class="sxs-lookup"><span data-stu-id="053cd-134">Grant access</span></span>
<span data-ttu-id="053cd-135">toogrant hozzáférés Miután azonosította, amelyet az tooassign hello szerepkör, használja:</span><span class="sxs-lookup"><span data-stu-id="053cd-135">toogrant access after you have identified hello role that you want tooassign, use:</span></span>

    azure role assignment create

### <a name="assign-a-role-toogroup-at-hello-subscription-scope"></a><span data-ttu-id="053cd-136">Egy szerepkör toogroup hello előfizetés hatókör hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="053cd-136">Assign a role toogroup at hello subscription scope</span></span>
<span data-ttu-id="053cd-137">egy szerepkör hello előfizetési hatókört, használja a tooa csoport tooassign:</span><span class="sxs-lookup"><span data-stu-id="053cd-137">tooassign a role tooa group at hello subscription scope, use:</span></span>

    azure role assignment create --objectId  <group object id> --roleName <name of role> --subscription <subscription> --scope <subscription/subscription id>

<span data-ttu-id="053cd-138">hello alábbi példa hozzárendel hello *olvasó* szerepkör túl*Christine Koch Team* : hello *előfizetés* hatókör.</span><span class="sxs-lookup"><span data-stu-id="053cd-138">hello following example assigns hello *Reader* role too*Christine Koch's Team* at hello *subscription* scope.</span></span>

![Szerepalapú Azure parancssori - azure szerepkör-hozzárendelés létrehozása csoport – képernyőkép](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-1.png)

### <a name="assign-a-role-tooan-application-at-hello-subscription-scope"></a><span data-ttu-id="053cd-140">Egy szerepkör tooan alkalmazást hello előfizetés hatókörből</span><span class="sxs-lookup"><span data-stu-id="053cd-140">Assign a role tooan application at hello subscription scope</span></span>
<span data-ttu-id="053cd-141">tooassign hello előfizetési hatókört, használja a szerepkör tooan alkalmazást:</span><span class="sxs-lookup"><span data-stu-id="053cd-141">tooassign a role tooan application at hello subscription scope, use:</span></span>

    azure role assignment create --objectId  <applications object id> --roleName <name of role> --subscription <subscription> --scope <subscription/subscription id>

<span data-ttu-id="053cd-142">hello következő példa engedélyezi hello *közreműködő* szerepkör tooan *az Azure AD* alkalmazás hello a kiválasztott előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="053cd-142">hello following example grants hello *Contributor* role tooan *Azure AD* application on hello selected subscription.</span></span>

 ![Szerepalapú Azure parancssori - alkalmazás azure szerepkör-hozzárendelés létrehozása](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-2.png)

### <a name="assign-a-role-tooa-user-at-hello-resource-group-scope"></a><span data-ttu-id="053cd-144">Egy szerepkör tooa felhasználó hello erőforrás csoport hatóköre:</span><span class="sxs-lookup"><span data-stu-id="053cd-144">Assign a role tooa user at hello resource group scope</span></span>
<span data-ttu-id="053cd-145">a szerepkör tooa felhasználók hello erőforrás csoport hatóköre, használja a tooassign:</span><span class="sxs-lookup"><span data-stu-id="053cd-145">tooassign a role tooa user at hello resource group scope, use:</span></span>

    azure role assignment create --signInName  <user email address> --roleName "<name of role>" --resourceGroup <resource group name>

<span data-ttu-id="053cd-146">hello következő példa engedélyezi hello *virtuális gép közreműködő* szerepkör túl *samert@aaddemo.com*  hello felhasználójának *Pharma-értékesítési-ProjectForcast* erőforrás csoport hatóköre.</span><span class="sxs-lookup"><span data-stu-id="053cd-146">hello following example grants hello *Virtual Machine Contributor* role too*samert@aaddemo.com* user at hello *Pharma-Sales-ProjectForcast* resource group scope.</span></span>

![Az RBAC Azure parancssori - azure szerepkör-hozzárendelés létrehozása felhasználó – képernyőkép](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-3.png)

### <a name="assign-a-role-tooa-group-at-hello-resource-scope"></a><span data-ttu-id="053cd-148">Egy szerepkör tooa csoport hello erőforrás hatókörből hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="053cd-148">Assign a role tooa group at hello resource scope</span></span>
<span data-ttu-id="053cd-149">egy szerepkör tooa csoport hello erőforrás hatókörben, használjon tooassign:</span><span class="sxs-lookup"><span data-stu-id="053cd-149">tooassign a role tooa group at hello resource scope, use:</span></span>

    azure role assignment create --objectId <group id> --role "<name of role>" --resource-name <resource group name> --resource-type <resource group type> --parent <resource group parent> --resource-group <resource group>

<span data-ttu-id="053cd-150">hello következő példa engedélyezi hello *virtuális gép közreműködő* szerepkör tooan *az Azure AD* csoporthoz egy *alhálózati*.</span><span class="sxs-lookup"><span data-stu-id="053cd-150">hello following example grants hello *Virtual Machine Contributor* role tooan *Azure AD* group on a *subnet*.</span></span>

![Szerepalapú Azure parancssori - azure szerepkör-hozzárendelés létrehozása csoport – képernyőkép](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-4.png)

## <a name="remove-access"></a><span data-ttu-id="053cd-152">Megszünteti a hozzáférést</span><span class="sxs-lookup"><span data-stu-id="053cd-152">Remove access</span></span>
<span data-ttu-id="053cd-153">egy szerepkör-hozzárendelés tooremove használja:</span><span class="sxs-lookup"><span data-stu-id="053cd-153">tooremove a role assignment, use:</span></span>

    azure role assignment delete --objectId <object id toofrom which tooremove role> --roleName "<role name>"

<span data-ttu-id="053cd-154">hello következő példában eltávolítjuk hello *virtuális gép közreműködő* hello a szerepkör-hozzárendelés  *sammert@aaddemo.com*  hello felhasználója *Pharma-értékesítési-ProjectForcast* erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="053cd-154">hello following example removes hello *Virtual Machine Contributor* role assignment from hello *sammert@aaddemo.com* user on hello *Pharma-Sales-ProjectForcast* resource group.</span></span>
<span data-ttu-id="053cd-155">hello példa hello szerepkör-hozzárendelés majd eltávolítása egy csoportból hello az előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="053cd-155">hello example then removes hello role assignment from a group on hello subscription.</span></span>

![RBAC Azure - azure szerepkör-hozzárendelés törlése - parancssori képernyőképe](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-assignment-delete.png)

## <a name="create-a-custom-role"></a><span data-ttu-id="053cd-157">Egyéni szerepkör létrehozása</span><span class="sxs-lookup"><span data-stu-id="053cd-157">Create a custom role</span></span>
<span data-ttu-id="053cd-158">egy egyéni biztonsági szerepkört, toocreate használja:</span><span class="sxs-lookup"><span data-stu-id="053cd-158">toocreate a custom role, use:</span></span>

    azure role create --inputfile <file path>

<span data-ttu-id="053cd-159">hello alábbi példa létrehoz egy egyéni biztonsági szerepkört nevű *virtuális gépet üzemeltető*.</span><span class="sxs-lookup"><span data-stu-id="053cd-159">hello following example creates a custom role called *Virtual Machine Operator*.</span></span> <span data-ttu-id="053cd-160">Az egyéni szerepkör engedélyezi a hozzáférést tooall olvasási műveletek a *Microsoft.Compute*, *Microsoft.Storage*, és *Microsoft.Network* erőforrás-szolgáltatók és biztosít hozzáférést toostart, indítsa újra, és a virtuális gépek figyelése.</span><span class="sxs-lookup"><span data-stu-id="053cd-160">This custom role grants access tooall read operations of *Microsoft.Compute*, *Microsoft.Storage*, and *Microsoft.Network* resource providers and grants access toostart, restart, and monitor virtual machines.</span></span> <span data-ttu-id="053cd-161">Az egyéni szerepkör két előfizetések használható.</span><span class="sxs-lookup"><span data-stu-id="053cd-161">This custom role can be used in two subscriptions.</span></span> <span data-ttu-id="053cd-162">A példa egy JSON-fájl bemenetként.</span><span class="sxs-lookup"><span data-stu-id="053cd-162">This example uses a JSON file as an input.</span></span>

![JSON - egyéni szerepkör-definíció – képernyőkép](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-create-1.png)

![Szerepalapú Azure parancssori - azure szerepkör létrehozása – képernyőfelvétel](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-create-2.png)

## <a name="modify-a-custom-role"></a><span data-ttu-id="053cd-165">Egyéni szerepkör módosítása</span><span class="sxs-lookup"><span data-stu-id="053cd-165">Modify a custom role</span></span>
<span data-ttu-id="053cd-166">toomodify egy egyéni biztonsági szerepkört, először az hello `azure role show` tooretrieve szerepkör-definíció parancsot.</span><span class="sxs-lookup"><span data-stu-id="053cd-166">toomodify a custom role, first use hello `azure role show` command tooretrieve role definition.</span></span> <span data-ttu-id="053cd-167">Ezután ellenőrizze hello szükséges módosításokat toohello szerepkör csomagdefiníciós fájl.</span><span class="sxs-lookup"><span data-stu-id="053cd-167">Second, make hello desired changes toohello role definition file.</span></span> <span data-ttu-id="053cd-168">Végül `azure role set` toosave hello módosítani a szerepkör-definíció.</span><span class="sxs-lookup"><span data-stu-id="053cd-168">Finally, use `azure role set` toosave hello modified role definition.</span></span>

    azure role set --inputfile <file path>

<span data-ttu-id="053cd-169">hello következő példakóddal felveheti a hello *Microsoft.Insights/diagnosticSettings/* művelet toohello **műveletek**, és az Azure-előfizetés toohello **AssignableScopes**hello virtuális gépet üzemeltető egyéni szerepkör.</span><span class="sxs-lookup"><span data-stu-id="053cd-169">hello following example adds hello *Microsoft.Insights/diagnosticSettings/* operation toohello **Actions**, and an Azure subscription toohello **AssignableScopes** of hello Virtual Machine Operator custom role.</span></span>

![JSON - módosítása egyéni szerepkör-definíció – képernyőkép](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-set-1.png)

![RBAC Azure - azure szerepkör set - parancssori képernyőképe](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-set2.png)

## <a name="delete-a-custom-role"></a><span data-ttu-id="053cd-172">Egyéni szerepkör törléséhez</span><span class="sxs-lookup"><span data-stu-id="053cd-172">Delete a custom role</span></span>
<span data-ttu-id="053cd-173">toodelete egy egyéni biztonsági szerepkört, először az hello `azure role show` parancs toodetermine hello **azonosító** hello szerepkör.</span><span class="sxs-lookup"><span data-stu-id="053cd-173">toodelete a custom role, first use hello `azure role show` command toodetermine hello **ID** of hello role.</span></span> <span data-ttu-id="053cd-174">Ezután használja az hello `azure role delete` parancs toodelete hello szerepkör hello megadásával **azonosító**.</span><span class="sxs-lookup"><span data-stu-id="053cd-174">Then, use hello `azure role delete` command toodelete hello role by specifying hello **ID**.</span></span>

<span data-ttu-id="053cd-175">hello következő példában eltávolítjuk hello *virtuális gépet üzemeltető* egyéni biztonsági szerepkört.</span><span class="sxs-lookup"><span data-stu-id="053cd-175">hello following example removes hello *Virtual Machine Operator* custom role.</span></span>

![Szerepalapú Azure parancssori - azure szerepkör törlésének – képernyőkép](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-delete.png)

## <a name="list-custom-roles"></a><span data-ttu-id="053cd-177">Egyéni szerepkörök listája</span><span class="sxs-lookup"><span data-stu-id="053cd-177">List custom roles</span></span>
<span data-ttu-id="053cd-178">toolist hello szerepkörök, amelyek rendelhető hozzá hatókör, használja a hello `azure role list` parancsot.</span><span class="sxs-lookup"><span data-stu-id="053cd-178">toolist hello roles that are available for assignment at a scope, use hello `azure role list` command.</span></span>

<span data-ttu-id="053cd-179">a következő parancs hello hello kiválasztott előfizetésben kiosztására használható összes szerepkörtől sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="053cd-179">hello following command lists all roles that are available for assignment in hello selected subscription.</span></span>

```
azure role list --json | jq '.[] | {"name":.properties.roleName, type:.properties.type}'
```

![RBAC Azure - azure szerepkör lista - parancssori képernyőképe](./media/role-based-access-control-manage-access-azure-cli/5-azure-role-list1.png)

<span data-ttu-id="053cd-181">A következő példa hello, hello *virtuális gépet üzemeltető* egyéni szerepkör nem érhető el a hello *Production4* előfizetés mert, hogy az előfizetés nem hello  **AssignableScopes** hello szerepkör.</span><span class="sxs-lookup"><span data-stu-id="053cd-181">In hello following example, hello *Virtual Machine Operator* custom role isn’t available in hello *Production4* subscription because that subscription isn’t in hello **AssignableScopes** of hello role.</span></span>

```
azure role list --json | jq '.[] | if .properties.type == "CustomRole" then .properties.roleName else empty end'
```

![RBAC Azure - egyéni szerepkörök listája azure szerepkör - parancssori képernyőképe](./media/role-based-access-control-manage-access-azure-cli/5-azure-role-list2.png)

## <a name="next-steps"></a><span data-ttu-id="053cd-183">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="053cd-183">Next steps</span></span>
[!INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]

