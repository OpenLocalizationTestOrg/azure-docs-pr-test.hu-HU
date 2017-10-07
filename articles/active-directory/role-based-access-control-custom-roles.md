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
# <a name="create-custom-roles-for-azure-role-based-access-control"></a>Hozzon létre egyéni szerepkörök átruházásához hozzáférés-vezérlés
Hozzon létre egy egyéni biztonsági szerepkört a átruházásához hozzáférés-vezérlés (RBAC) Ha az adott hozzáférési igényeinek hello beépített szerepkörök egyike. Egyéni szerepkörök segítségével hozhatók létre [Azure PowerShell](role-based-access-control-manage-access-powershell.md), [Azure parancssori felület](role-based-access-control-manage-access-azure-cli.md) (CLI), és hello [REST API](role-based-access-control-manage-access-rest.md). Hasonlóan a beépített szerepkörök rendelhet hozzá egyéni szerepkörök toousers, csoportok és alkalmazások előfizetés, erőforráscsoport és erőforrás-hatókörök. Egyéni szerepkörök az Azure AD-bérlő tárolódnak, és előfizetések között megosztható legyen.

Mindegyik bérlő mentése too2000 egyéni szerepkörök hozhat létre. 

hello alábbi példa bemutatja egy egyéni biztonsági szerepkört a figyelés és a virtuális gépek újraindításával:

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
## <a name="actions"></a>Műveletek
Hello **műveletek** egy egyéni biztonsági szerepkört a tulajdonság határozza meg, hogy hello Azure üzemeltetése toowhich hello szerepkör engedélyezi a hozzáférést. Művelet karakterláncok, amelyek azonosítják az Azure erőforrás-szolgáltatók biztonságos műveletek gyűjteménye. Művelet karakterláncok kövesse hello formátuma `Microsoft.<ProviderName>/<ChildResourceType>/<action>`. Helyettesítő karakterek művelet karakterláncokat (\*) hozzáférést hello művelet karakterláncnak megfelelő tooall műveletek. Például:

* `*/read`a hozzáférési tooread műveletek az összes Azure-erőforrás-szolgáltató minden erőforrástípus esetén.
* `Microsoft.Compute/*`a hozzáférési tooall műveletek hello Microsoft.Compute erőforrás-szolgáltató összes erőforrás típusához.
* `Microsoft.Network/*/read`a hozzáférési tooread műveletek Azure hello Microsoft.Network erőforrás-szolgáltató az összes erőforrástípus.
* `Microsoft.Compute/virtualMachines/*`a hozzáférési tooall műveletek a virtuális gépek és a gyermek típusú erőforrások.
* `Microsoft.Web/sites/restart/Action`a hozzáférési toorestart webhelyeket.

Használjon `Get-AzureRmProviderOperation` (a PowerShell) vagy `azure provider operations show` (az Azure CLI) toolist műveletek az Azure erőforrás-szolgáltatót. A parancsok tooverify, hogy egy művelet karakterlánc érvénytelen, és a tooexpand helyettesítő művelet sztringek is használhatja.

```
Get-AzureRMProviderOperation Microsoft.Compute/virtualMachines/*/action | FT Operation, OperationName

Get-AzureRMProviderOperation Microsoft.Network/*
```

![PowerShell képernyőfelvétel - Get-AzureRMProviderOperation](./media/role-based-access-control-configure/1-get-azurermprovideroperation-1.png)

```
azure provider operations show "Microsoft.Compute/virtualMachines/*/action" --js on | jq '.[] | .operation'

azure provider operations show "Microsoft.Network/*"
```

![Az Azure CLI képernyőfelvétel - azure szolgáltató műveletek megjelenítése "Microsoft.Compute/virtualMachines/ \* /művelet" ](./media/role-based-access-control-configure/1-azure-provider-operations-show.png)

## <a name="notactions"></a>NotActions
Használjon hello **NotActions** tulajdonságot, ha hello beállítása, hogy kívánja-e tooallow műveletek könnyebben definiált tiltott operatív kizárásával. hello egy egyéni szerepkör által biztosított hozzáférést számított hello kivonásával **NotActions** hello műveletek **műveletek** műveletek.

> [!NOTE]
> Ha egy felhasználó tartozik, amely nem tartalmazza egy műveletet a szerepkör **NotActions**, és hozzá van rendelve egy második szerepkör, amely hozzáférést biztosít műveletet, hello felhasználói toohello engedélyezett tooperform művelet. **NotActions** nincs megtagadási szabály – Ha az adott műveletek kizárt toobe egyszerűen csak egy kényelmes módszert arra toocreate engedélyezett műveletkészlet.
>
>

## <a name="assignablescopes"></a>AssignableScopes
Hello **AssignableScopes** hello egyéni szerepkör tulajdonság határozza meg a hello hatókörök (előfizetések, erőforráscsoport-sablonok vagy az erőforrások) belül mely hello egyéni szerepkör érhető el a hozzárendeléshez. Elérhetővé teheti hello egyéni szerepkör hozzárendelés csak hello előfizetésekhez vagy erőforráscsoportokhoz olyan igényli azt, és nem zsúfoltságát felhasználói élményt a hello előfizetések vagy erőforráscsoportok hello részeinek.

Érvényes hozzárendelhető hatókörök például:

* "/ subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e", "/ subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624" - elérhetővé hello szerepkör hozzárendelése két előfizetésekhez.
* "/ subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e" - hello szerepkör elérhetővé teszi a hozzárendeléshez egyetlen előfizetéssel.
* "/ előfizetések/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/hálózati" - teszi hello szerepkör csak a hello hálózati erőforráscsoport kiosztására használható.

> [!NOTE]
> Segítségével kell legalább egy előfizetést, erőforrás vagy az erőforrás-azonosító.
>
>

## <a name="custom-roles-access-control"></a>Egyéni szerepkörök hozzáférés-vezérlés
Hello **AssignableScopes** hello egyéni szerepkör tulajdonság is vezérli, akik megtekintése, módosítása és törlése hello szerepkör.

* Aki hozhat létre egy egyéni biztonsági szerepkört?
    Tulajdonosok (és a felhasználói hozzáférés rendszergazdák) előfizetések, erőforráscsoport-sablonok és erőforrások hozhat létre egyéni szerepkörök használatra ezeket.
    hello hello szerepkör létrehozó felhasználónak kell toobe képes tooperform `Microsoft.Authorization/roleDefinition/write` összes hello művelet **AssignableScopes** hello szerepkör.
* Módosíthatja, akik egy egyéni biztonsági szerepkört?
    Tulajdonosok (és a felhasználói hozzáférés rendszergazdák) előfizetések, erőforráscsoport-sablonok és erőforrások ezeket az egyéni szerepkörök módosíthatja. A felhasználóknak kell toobe képes tooperform hello `Microsoft.Authorization/roleDefinition/write` összes hello művelet **AssignableScopes** egy egyéni szerepkör.
* Kik tekinthetik meg egyéni szerepkörök?
    Minden beépített szerepkörök az Azure RBAC kiosztására használható szerepkörtől megtekintésének engedélyezése. Felhasználók, akik végezheti hello `Microsoft.Authorization/roleDefinition/read` hatókörre művelet hello RBAC szerepkörtől érhetők el, hogy a hatókör-hozzárendelés tekintheti meg.

## <a name="see-also"></a>Lásd még:
* [Szerepköralapú hozzáférés-vezérlés](role-based-access-control-configure.md): első lépések az RBAC a hello Azure-portálon.
* Megtudhatja, hogyan toomanage elérni:
  * [PowerShell](role-based-access-control-manage-access-powershell.md)
  * [Azure CLI](role-based-access-control-manage-access-azure-cli.md)
  * [REST API](role-based-access-control-manage-access-rest.md)
* [Beépített szerepkörök](role-based-access-built-in-roles.md): részletes információkat szolgáltatva hello szerepköröket, az RBAC szabványos tartalmazza.
