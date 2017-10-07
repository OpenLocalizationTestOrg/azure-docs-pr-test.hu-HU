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
# <a name="manage-role-based-access-control-with-hello-azure-command-line-interface"></a>Szerepköralapú hozzáférés-vezérlés az Azure parancssori felület hello kezelése
> [!div class="op_single_selector"]
> * [PowerShell](role-based-access-control-manage-access-powershell.md)
> * [Azure CLI](role-based-access-control-manage-access-azure-cli.md)
> * [REST API](role-based-access-control-manage-access-rest.md)


Hello Azure-portál és az Azure Resource Manager API toomanage hozzáférés tooyour előfizetéshez és a részletes szinten erőforrások szerepköralapú hozzáférés-vezérlést (RBAC) is használhatja. Ez a szolgáltatás egyes szerepkörök toothem egy adott hatókör hozzárendelése szerint engedélyezheti a hozzáférést az Active Directory felhasználók, csoportok vagy szolgáltatásnevekről.

Hello Azure parancssori felület (CLI) toomanage RBAC használata előtt a következő előfeltételek hello kell rendelkeznie:

* Az Azure CLI 0.8.8 verzió vagy újabb. tooinstall hello legújabb verzióját, és rendelje azt az Azure-előfizetéshez, lásd: [telepítése és konfigurálása az Azure parancssori felület hello](../cli-install-nodejs.md).
* Az Azure Resource Manager az Azure parancssori felület. Nyissa meg túl[Using hello Azure CLI az erőforrás-kezelő hello](../xplat-cli-azure-resource-manager.md) további részleteket.

## <a name="list-roles"></a>Lista szerepkörök
### <a name="list-all-available-roles"></a>Elérhető szerepkörök felsorolása
toolist elérhető szerepkörök, használja:

        azure role list

hello következő példa bemutatja hello listája *elérhető szerepkörök*.

```
azure role list --json | jq '.[] | {"roleName":.properties.roleName, "description":.properties.description}'
```

![RBAC Azure - azure szerepkör lista - parancssori képernyőképe](./media/role-based-access-control-manage-access-azure-cli/1-azure-role-list.png)

### <a name="list-actions-of-a-role"></a>Egy szerepkör lista műveletek
egy szerepkör toolist hello műveletek használata:

    azure role show "<role name>"

hello alábbi példa azt mutatja, hello hello műveletek *közreműködő* és *virtuális gép közreműködő* szerepkörök.

```
azure role show "contributor" --json | jq '.[] | {"Actions":.properties.permissions[0].actions,"NotActions":properties.permissions[0].notActions}'

azure role show "virtual machine contributor" --json | jq '.[] | .properties.permissions[0].actions'
```

![Szerepalapú Azure parancssori - azure szerepkör megjelenítése – képernyőkép](./media/role-based-access-control-manage-access-azure-cli/1-azure-role-show.png)

## <a name="list-access"></a>A hozzáférési lista
### <a name="list-role-assignments-effective-on-a-resource-group"></a>Lista szerepkör-hozzárendelések lép az erőforráscsoport
toolist hello szerepkör-hozzárendelések meglévő erőforráscsoport használata:

    azure role assignment list --resource-group <resource group name>

hello alábbi példa azt mutatja hello szerepkör-hozzárendelések a hello *pharma-értékesítési-projecforcast* csoport.

```
azure role assignment list --resource-group pharma-sales-projecforcast --json | jq '.[] | {"DisplayName":.properties.aADObject.displayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'
```

![Szerepalapú Azure parancssori - társításának listája azure szerepkör csoport – képernyőkép](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-assignment-list-1.png)

### <a name="list-role-assignments-for-a-user"></a>Szerepkör-hozzárendelések listáját egy felhasználó számára
toolist hello szerepkör-hozzárendelések egy adott felhasználó és hello hozzárendelések rendelt tooa felhasználói csoportok használata:

    azure role assignment list --signInName <user email>

Megtekintheti a csoportok hello parancs módosításával örökölt szerepkör-hozzárendelések is:

    azure role assignment list --expandPrincipalGroups --signInName <user email>

hello alábbi példa azt mutatja, toohello kapott hello szerepkör-hozzárendelések  *sameert@aaddemo.com*  felhasználó. Ez magában foglalja a hozzárendelt közvetlenül toohello felhasználói szerepkörök és csoportok öröklődtek szerepkörtől.

```
azure role assignment list --signInName sameert@aaddemo.com --json | jq '.[] | {"DisplayName":.properties.aADObject.DisplayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'

azure role assignment list --expandPrincipalGroups --signInName sameert@aaddemo.com --json | jq '.[] | {"DisplayName":.properties.aADObject.DisplayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'
```

![RBAC Azure - hozzárendelés lista azure szerepkör felhasználó - parancssori képernyőképe](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-assignment-list-2.png)

## <a name="grant-access"></a>Hozzáférés biztosítása
toogrant hozzáférés Miután azonosította, amelyet az tooassign hello szerepkör, használja:

    azure role assignment create

### <a name="assign-a-role-toogroup-at-hello-subscription-scope"></a>Egy szerepkör toogroup hello előfizetés hatókör hozzárendelése
egy szerepkör hello előfizetési hatókört, használja a tooa csoport tooassign:

    azure role assignment create --objectId  <group object id> --roleName <name of role> --subscription <subscription> --scope <subscription/subscription id>

hello alábbi példa hozzárendel hello *olvasó* szerepkör túl*Christine Koch Team* : hello *előfizetés* hatókör.

![Szerepalapú Azure parancssori - azure szerepkör-hozzárendelés létrehozása csoport – képernyőkép](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-1.png)

### <a name="assign-a-role-tooan-application-at-hello-subscription-scope"></a>Egy szerepkör tooan alkalmazást hello előfizetés hatókörből
tooassign hello előfizetési hatókört, használja a szerepkör tooan alkalmazást:

    azure role assignment create --objectId  <applications object id> --roleName <name of role> --subscription <subscription> --scope <subscription/subscription id>

hello következő példa engedélyezi hello *közreműködő* szerepkör tooan *az Azure AD* alkalmazás hello a kiválasztott előfizetéshez.

 ![Szerepalapú Azure parancssori - alkalmazás azure szerepkör-hozzárendelés létrehozása](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-2.png)

### <a name="assign-a-role-tooa-user-at-hello-resource-group-scope"></a>Egy szerepkör tooa felhasználó hello erőforrás csoport hatóköre:
a szerepkör tooa felhasználók hello erőforrás csoport hatóköre, használja a tooassign:

    azure role assignment create --signInName  <user email address> --roleName "<name of role>" --resourceGroup <resource group name>

hello következő példa engedélyezi hello *virtuális gép közreműködő* szerepkör túl *samert@aaddemo.com*  hello felhasználójának *Pharma-értékesítési-ProjectForcast* erőforrás csoport hatóköre.

![Az RBAC Azure parancssori - azure szerepkör-hozzárendelés létrehozása felhasználó – képernyőkép](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-3.png)

### <a name="assign-a-role-tooa-group-at-hello-resource-scope"></a>Egy szerepkör tooa csoport hello erőforrás hatókörből hozzárendelése
egy szerepkör tooa csoport hello erőforrás hatókörben, használjon tooassign:

    azure role assignment create --objectId <group id> --role "<name of role>" --resource-name <resource group name> --resource-type <resource group type> --parent <resource group parent> --resource-group <resource group>

hello következő példa engedélyezi hello *virtuális gép közreműködő* szerepkör tooan *az Azure AD* csoporthoz egy *alhálózati*.

![Szerepalapú Azure parancssori - azure szerepkör-hozzárendelés létrehozása csoport – képernyőkép](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-4.png)

## <a name="remove-access"></a>Megszünteti a hozzáférést
egy szerepkör-hozzárendelés tooremove használja:

    azure role assignment delete --objectId <object id toofrom which tooremove role> --roleName "<role name>"

hello következő példában eltávolítjuk hello *virtuális gép közreműködő* hello a szerepkör-hozzárendelés  *sammert@aaddemo.com*  hello felhasználója *Pharma-értékesítési-ProjectForcast* erőforráscsoport.
hello példa hello szerepkör-hozzárendelés majd eltávolítása egy csoportból hello az előfizetésben.

![RBAC Azure - azure szerepkör-hozzárendelés törlése - parancssori képernyőképe](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-assignment-delete.png)

## <a name="create-a-custom-role"></a>Egyéni szerepkör létrehozása
egy egyéni biztonsági szerepkört, toocreate használja:

    azure role create --inputfile <file path>

hello alábbi példa létrehoz egy egyéni biztonsági szerepkört nevű *virtuális gépet üzemeltető*. Az egyéni szerepkör engedélyezi a hozzáférést tooall olvasási műveletek a *Microsoft.Compute*, *Microsoft.Storage*, és *Microsoft.Network* erőforrás-szolgáltatók és biztosít hozzáférést toostart, indítsa újra, és a virtuális gépek figyelése. Az egyéni szerepkör két előfizetések használható. A példa egy JSON-fájl bemenetként.

![JSON - egyéni szerepkör-definíció – képernyőkép](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-create-1.png)

![Szerepalapú Azure parancssori - azure szerepkör létrehozása – képernyőfelvétel](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-create-2.png)

## <a name="modify-a-custom-role"></a>Egyéni szerepkör módosítása
toomodify egy egyéni biztonsági szerepkört, először az hello `azure role show` tooretrieve szerepkör-definíció parancsot. Ezután ellenőrizze hello szükséges módosításokat toohello szerepkör csomagdefiníciós fájl. Végül `azure role set` toosave hello módosítani a szerepkör-definíció.

    azure role set --inputfile <file path>

hello következő példakóddal felveheti a hello *Microsoft.Insights/diagnosticSettings/* művelet toohello **műveletek**, és az Azure-előfizetés toohello **AssignableScopes**hello virtuális gépet üzemeltető egyéni szerepkör.

![JSON - módosítása egyéni szerepkör-definíció – képernyőkép](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-set-1.png)

![RBAC Azure - azure szerepkör set - parancssori képernyőképe](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-set2.png)

## <a name="delete-a-custom-role"></a>Egyéni szerepkör törléséhez
toodelete egy egyéni biztonsági szerepkört, először az hello `azure role show` parancs toodetermine hello **azonosító** hello szerepkör. Ezután használja az hello `azure role delete` parancs toodelete hello szerepkör hello megadásával **azonosító**.

hello következő példában eltávolítjuk hello *virtuális gépet üzemeltető* egyéni biztonsági szerepkört.

![Szerepalapú Azure parancssori - azure szerepkör törlésének – képernyőkép](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-delete.png)

## <a name="list-custom-roles"></a>Egyéni szerepkörök listája
toolist hello szerepkörök, amelyek rendelhető hozzá hatókör, használja a hello `azure role list` parancsot.

a következő parancs hello hello kiválasztott előfizetésben kiosztására használható összes szerepkörtől sorolja fel.

```
azure role list --json | jq '.[] | {"name":.properties.roleName, type:.properties.type}'
```

![RBAC Azure - azure szerepkör lista - parancssori képernyőképe](./media/role-based-access-control-manage-access-azure-cli/5-azure-role-list1.png)

A következő példa hello, hello *virtuális gépet üzemeltető* egyéni szerepkör nem érhető el a hello *Production4* előfizetés mert, hogy az előfizetés nem hello  **AssignableScopes** hello szerepkör.

```
azure role list --json | jq '.[] | if .properties.type == "CustomRole" then .properties.roleName else empty end'
```

![RBAC Azure - egyéni szerepkörök listája azure szerepkör - parancssori képernyőképe](./media/role-based-access-control-manage-access-azure-cli/5-azure-role-list2.png)

## <a name="next-steps"></a>Következő lépések
[!INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]

