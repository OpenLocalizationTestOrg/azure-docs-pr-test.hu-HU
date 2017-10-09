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
# <a name="manage-role-based-access-control-with-azure-powershell"></a>Szerepköralapú hozzáférés-vezérlés kezelése az Azure PowerShell-lel
> [!div class="op_single_selector"]
> * [PowerShell](role-based-access-control-manage-access-powershell.md)
> * [Azure CLI](role-based-access-control-manage-access-azure-cli.md)
> * [REST API](role-based-access-control-manage-access-rest.md)

Szerepköralapú hozzáférés-vezérlést (RBAC) használhatja hello Azure-portál és az Azure erőforrás-kezelési API toomanage hozzáférés tooyour előfizetés részletes szinten. Ez a szolgáltatás egyes szerepkörök toothem egy adott hatókör hozzárendelése szerint engedélyezheti a hozzáférést az Active Directory felhasználók, csoportok vagy szolgáltatásnevekről.

PowerShell toomanage RBAC használata előtt a következő előfeltételek hello szüksége:

* Az Azure PowerShell 0.8.8 verzió vagy újabb. tooinstall hello legújabb verzióját, és rendelje azt az Azure-előfizetéshez, lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).
* Az Azure Resource Manager parancsmagok. Telepítse a hello [Azure Resource Manager parancsmagjainak](/powershell/azure/overview) a PowerShellben.

## <a name="list-roles"></a>Lista szerepkörök
### <a name="list-all-available-roles"></a>Elérhető szerepkörök felsorolása
toolist RBAC-hozzárendelés és tooinspect hello műveletek toowhich azok engedélyezheti a hozzáférést, a rendelkezésre álló használnak `Get-AzureRmRoleDefinition`.

```
Get-AzureRmRoleDefinition | FT Name, Description
```

![Az RBAC PowerShell-Get AzureRmRoleDefinition – képernyőkép](./media/role-based-access-control-manage-access-powershell/1-get-azure-rm-role-definition1.png)

### <a name="list-actions-of-a-role"></a>Egy szerepkör lista műveletek
egy adott szerepkör esetében toolist hello műveletek `Get-AzureRmRoleDefinition <role name>`.

```
Get-AzureRmRoleDefinition Contributor | FL Actions, NotActions

(Get-AzureRmRoleDefinition "Virtual Machine Contributor").Actions
```

![Az RBAC PowerShell-Get AzureRmRoleDefinition egy adott szerepkör esetében – képernyőkép](./media/role-based-access-control-manage-access-powershell/1-get-azure-rm-role-definition2.png)

## <a name="see-who-has-access"></a>Lásd a kinek van hozzáférése:
toolist Szerepalapú hozzáférés-hozzárendelés, használjon `Get-AzureRmRoleAssignment`.

### <a name="list-role-assignments-at-a-specific-scope"></a>Szerepkör-hozzárendelések listáját egy adott hatókörhöz
A megadott előfizetés, erőforráscsoportból vagy erőforrás összes hello hozzáférés hozzárendelését tekintheti meg. Például toosee hello összes hello aktív hozzárendelést egy erőforráscsoport, használjon `Get-AzureRmRoleAssignment -ResourceGroupName <resource group name>`.

```
Get-AzureRmRoleAssignment -ResourceGroupName Pharma-Sales-ProjectForcast | FL DisplayName, RoleDefinitionName, Scope
```

![Az RBAC - Get-AzureRmRoleAssignment erőforráscsoport - PowerShell képernyőképe](./media/role-based-access-control-manage-access-powershell/4-get-azure-rm-role-assignment1.png)

### <a name="list-roles-assigned-tooa-user"></a>Lista tooa felhasználói szerepkörrel
minden hello szerepkör tooa megadott felhasználói és toowhich hello felhasználó tartozik, toohello csoportok hello szerepkör toolist használata `Get-AzureRmRoleAssignment -SignInName <User email> -ExpandPrincipalGroups`.

```
Get-AzureRmRoleAssignment -SignInName sameert@aaddemo.com | FL DisplayName, RoleDefinitionName, Scope

Get-AzureRmRoleAssignment -SignInName sameert@aaddemo.com -ExpandPrincipalGroups | FL DisplayName, RoleDefinitionName, Scope
```

![Az RBAC PowerShell-Get AzureRmRoleAssignment egy felhasználó számára – képernyőkép](./media/role-based-access-control-manage-access-powershell/4-get-azure-rm-role-assignment2.png)

### <a name="list-classic-service-administrator-and-coadmin-role-assignments"></a>Lista klasszikus szolgáltatás-rendszergazda és a szerepkör-hozzárendelések társfelügyeletű
hozzáférés-hozzárendelései toolist hello klasszikus előfizetési rendszergazda, és coadministrators, használja:

    Get-AzureRmRoleAssignment -IncludeClassicAdministrators

## <a name="grant-access"></a>Hozzáférés biztosítása
### <a name="search-for-object-ids"></a>Objektumazonosítók keresése
szerepkör tooassign, kell tooidentify hello objektum (felhasználó, csoport vagy alkalmazás) és a hello hatókör.

Ha nem tudja hello előfizetés-azonosító, megtalálja a hello **előfizetések** hello Azure-portálon paneljét. Hogyan hello előfizetés-azonosító, a tooquery: toolearn [Get-AzureSubscription](/powershell/module/azure/get-azuresubscription?view=azuresmps-3.7.0) az MSDN Webhelyén.

tooget hello objektum azonosítója az Azure AD-csoport használja:

    Get-AzureRmADGroup -SearchString <group name in quotes>

tooget hello Objektumazonosító egy egyszerű Azure AD szolgáltatás vagy alkalmazás használja:

    Get-AzureRmADServicePrincipal -SearchString <service name in quotes>

### <a name="assign-a-role-tooan-application-at-hello-subscription-scope"></a>Egy szerepkör tooan alkalmazást hello előfizetés hatókörből
toogrant access tooan alkalmazás hello előfizetés hatókörben, használja:

    New-AzureRmRoleAssignment -ObjectId <application id> -RoleDefinitionName <role name> -Scope <subscription id>

![Az RBAC PowerShell új AzureRmRoleAssignment – képernyőkép](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment2.png)

### <a name="assign-a-role-tooa-user-at-hello-resource-group-scope"></a>Egy szerepkör tooa felhasználó hello erőforrás csoport hatóköre:
toogrant hozzáférés tooa felhasználójának hello erőforrás csoport hatóköre használja:

    New-AzureRmRoleAssignment -SignInName <email of user> -RoleDefinitionName <role name in quotes> -ResourceGroupName <resource group name>

![Az RBAC PowerShell új AzureRmRoleAssignment – képernyőkép](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment3.png)

### <a name="assign-a-role-tooa-group-at-hello-resource-scope"></a>Egy szerepkör tooa csoport hello erőforrás hatókörből hozzárendelése
toogrant hozzáférési tooa csoport hello erőforrás hatókörben, használja:

    New-AzureRmRoleAssignment -ObjectId <object id> -RoleDefinitionName <role name in quotes> -ResourceName <resource name> -ResourceType <resource type> -ParentResource <parent resource> -ResourceGroupName <resource group name>

![Az RBAC PowerShell új AzureRmRoleAssignment – képernyőkép](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment4.png)

## <a name="remove-access"></a>Megszünteti a hozzáférést
tooremove hozzáférés a felhasználók, csoportok és alkalmazások számára:

    Remove-AzureRmRoleAssignment -ObjectId <object id> -RoleDefinitionName <role name> -Scope <scope such as subscription id>

![Az RBAC PowerShell-Remove AzureRmRoleAssignment – képernyőkép](./media/role-based-access-control-manage-access-powershell/3-remove-azure-rm-role-assignment.png)

## <a name="create-a-custom-role"></a>Egyéni szerepkör létrehozása
egy egyéni biztonsági szerepkört, toocreate hello használata ```New-AzureRmRoleDefinition``` parancsot. Kétféleképpen a szerkezetének kialakítása hello szerepkör PSRoleDefinitionObject vagy egy JSON-sablon használatával. 

## <a name="get-actions-for-a-resource-provider"></a>Egy erőforrás-szolgáltató műveleteinek beolvasása
Egyéni szerepkörök teljesen új hoz létre, esetén fontos tooknow összes lehetséges műveleteket az erőforrás-szolgáltatók hello hello.
Használjon hello ```Get-AzureRMProviderOperation``` parancs tooget ezt az információt.
Például ha azt szeretné, hogy toocheck hello elérhető műveletek a virtuális gép használja ezt a parancsot:

```
Get-AzureRMProviderOperation "Microsoft.Compute/virtualMachines/*" | FT OperationName, Operation , Description -AutoSize
```

### <a name="create-role-with-psroledefinitionobject"></a>PSRoleDefinitionObject szerepkör létrehozása
PowerShell toocreate egy egyéni biztonsági szerepkört használatakor, új, vagy hello valamelyikével [beépített szerepkörök](role-based-access-built-in-roles.md) kiindulási pontként. Ebben a szakaszban hello például egy beépített szerepkör kezdődik, és majd testreszabása további jogosultságokkal. Hello attribútumok tooadd hello szerkesztése *műveletek*, *notActions*, vagy *hatókörök* , és mentse egy új szerepkörként hello módosítások.

hello alábbi példa kezdődik hello *virtuális gép közreműködő* szerepkör és használja, hogy egy egyéni biztonsági szerepkört toocreate nevű *virtuális gépet üzemeltető*. hello új szerepkörök hozzáférési tooall olvasási műveletek a *Microsoft.Compute*, *Microsoft.Storage*, és *Microsoft.Network* erőforrás-szolgáltatók és biztosít hozzáférést toostart, indítsa újra, és a virtuális gépek figyelése. hello egyéni biztonsági szerepkört is használható két előfizetésekhez.

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

### <a name="create-role-with-json-template"></a>A JSON-sablon szerepkör létrehozása
A JSON-sablon hello egyéni szerepkör hello adatforrása definíciója is használható. hello alábbi példa létrehoz egy egyéni biztonsági szerepkört, amely lehetővé teszi az olvasási hozzáférés toostorage és számítási erőforrásokat, toosupport eléréséhez és hozzáadja az adott szerepkörhöz tootwo előfizetések. Hozzon létre egy új fájlt `C:\CustomRoles\customrole1.json` az alábbi példa hello. hello azonosítóját kell beállítani. túl`null` a kezdeti szerepkör létrehozásakor egy új ID automatikusan létrejön. 

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
tooadd hello szerepkör toohello előfizetések, futtassa a következő PowerShell-paranccsal hello:
```
New-AzureRmRoleDefinition -InputFile "C:\CustomRoles\customrole1.json"
```

## <a name="modify-a-custom-role"></a>Egyéni szerepkör módosítása
Hasonló toocreating egy egyéni biztonsági szerepkört, módosíthatja egy meglévő egyéni szerepkör hello PSRoleDefinitionObject vagy egy JSON-sablon használatával.

### <a name="modify-role-with-psroledefinitionobject"></a>A PSRoleDefinitionObject szerepkör módosítása
egy egyéni biztonsági szerepkört, toomodify először használja a hello `Get-AzureRmRoleDefinition` tooretrieve hello szerepkör-definíció parancsot. Második módosításokat szükséges hello toohello szerepkör-definíció. Végül, használja a hello `Set-AzureRmRoleDefinition` parancs toosave hello módosítani a szerepkör-definíció.

hello következő példakóddal felveheti a hello `Microsoft.Insights/diagnosticSettings/*` művelet toohello *virtuális gépet üzemeltető* egyéni biztonsági szerepkört.

```
$role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
$role.Actions.Add("Microsoft.Insights/diagnosticSettings/*")
Set-AzureRmRoleDefinition -Role $role
```

![Az RBAC PowerShell-Set AzureRmRoleDefinition – képernyőkép](./media/role-based-access-control-manage-access-powershell/3-set-azurermroledefinition-1.png)

hello következő példakóddal felveheti az Azure-előfizetés toohello hozzárendelhető hatóköröknek a hello *virtuális gépet üzemeltető* egyéni biztonsági szerepkört.

```
Get-AzureRmSubscription - SubscriptionName Production3

$role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
$role.AssignableScopes.Add("/subscriptions/34370e90-ac4a-4bf9-821f-85eeedead1a2")
Set-AzureRmRoleDefinition -Role $role
```

![Az RBAC PowerShell-Set AzureRmRoleDefinition – képernyőkép](./media/role-based-access-control-manage-access-powershell/3-set-azurermroledefinition-2.png)

### <a name="modify-role-with-json-template"></a>Szerepkör JSON-sablon módosítása
Hello előző JSON-sablon használatával könnyen módosíthatja egy meglévő egyéni szerepkör tooadd vagy távolítsa el a műveletek. Hello JSON-sablont módosítani, és adja hozzá a hello olvasási művelet, a hálózatkezeléshez, ahogy az alábbi példa hello. nincsenek összesítve alkalmazott tooan meglévő definíciót, ami azt jelenti, hogy hello szerepkör jelenik meg, pontosan hello sablonban megadott hello definíciók hello sablon szerepel. Meg kell tooupdate hello azonosító mezőben hello azonosítójú hello szerepkör is. Ha még nem meg arról, hogy mi az az érték, használhatja a hello `Get-AzureRmRoleDefinition` parancsmag tooget ezt az információt.

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

tooupdate hello meglévő szerepkör, futtassa a következő PowerShell-paranccsal hello:
```
Set-AzureRmRoleDefinition -InputFile "C:\CustomRoles\customrole1.json"
```

## <a name="delete-a-custom-role"></a>Egyéni szerepkör törléséhez
egy egyéni biztonsági szerepkört, toodelete hello használata `Remove-AzureRmRoleDefinition` parancsot.

hello következő példában eltávolítjuk hello *virtuális gépet üzemeltető* egyéni biztonsági szerepkört.

```
Get-AzureRmRoleDefinition "Virtual Machine Operator"

Get-AzureRmRoleDefinition "Virtual Machine Operator" | Remove-AzureRmRoleDefinition
```

![Az RBAC PowerShell-Remove AzureRmRoleDefinition – képernyőkép](./media/role-based-access-control-manage-access-powershell/4-remove-azurermroledefinition.png)

## <a name="list-custom-roles"></a>Egyéni szerepkörök listája
toolist hello szerepkörök, amelyek rendelhető hozzá hatókör, használja a hello `Get-AzureRmRoleDefinition` parancsot.

a következő példa hello hello kiválasztott előfizetésben kiosztására használható összes szerepkörtől sorolja fel.

```
Get-AzureRmRoleDefinition | FT Name, IsCustom
```

![Az RBAC PowerShell-Get AzureRmRoleDefinition – képernyőkép](./media/role-based-access-control-manage-access-powershell/5-get-azurermroledefinition-1.png)

A következő példa hello, hello *virtuális gépet üzemeltető* egyéni szerepkör nem érhető el a hello *Production4* előfizetés mert, hogy az előfizetés nem hello  **AssignableScopes** hello szerepkör.

![Az RBAC PowerShell-Get AzureRmRoleDefinition – képernyőkép](./media/role-based-access-control-manage-access-powershell/5-get-azurermroledefinition2.png)

## <a name="see-also"></a>Lásd még:
* [Using Azure PowerShell használata az Azure Resource Manager eszközzel](../powershell-azure-resource-manager.md)
  [!INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]

