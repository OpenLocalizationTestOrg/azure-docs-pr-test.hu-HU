---
title: "Hozzáférés-vezérlés aaaRole alapú REST - az Azure AD |} Microsoft Docs"
description: "Hello REST API szerepköralapú hozzáférés-vezérlés kezelése"
services: active-directory
documentationcenter: na
author: andredm7
manager: femila
editor: 
ms.assetid: 1f90228a-7aac-4ea7-ad82-b57d222ab128
ms.service: active-directory
ms.workload: multiple
ms.tgt_pltfrm: rest-api
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: andredm
ms.openlocfilehash: ccd402fd4fe4583288076cac23753dd067694681
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-role-based-access-control-with-hello-rest-api"></a>Szerepköralapú hozzáférés-vezérlés a REST API hello kezelése
> [!div class="op_single_selector"]
> * [PowerShell](role-based-access-control-manage-access-powershell.md)
> * [Azure CLI](role-based-access-control-manage-access-azure-cli.md)
> * [REST API](role-based-access-control-manage-access-rest.md)

Szerepköralapú hozzáférés-vezérlés (RBAC) a hello Azure-portálon, és az Azure Resource Manager API segítségével felügyelheti a hozzáférési tooyour előfizetés és a minden részletre kiterjedő szinten erőforrásokat. Ez a szolgáltatás egyes szerepkörök toothem egy adott hatókör hozzárendelése szerint engedélyezheti a hozzáférést az Active Directory felhasználók, csoportok vagy szolgáltatásnevekről.

## <a name="list-all-role-assignments"></a>Az összes szerepkör-hozzárendelések felsorolása
Listák összes hello szerepkör-hozzárendelések a megadott hello hatókörét, és subscopes.

szerepkör-hozzárendelések toolist, hozzá kell férnie túl`Microsoft.Authorization/roleAssignments/read` művelet hello hatókörben. Minden hello beépített szerepkörök kapnak hozzáférést toothis műveletet. További információ a szerepkör-hozzárendelések és az Azure-erőforrások kezelése hozzáférés: [átruházásához hozzáférés-vezérlés](role-based-access-control-configure.md).

### <a name="request"></a>Kérés
Használjon hello **beolvasása** hello URI a következő metódust:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments?api-version={api-version}&$filter={filter}

Belül hello URI ellenőrizze a következő helyettesítések toocustomize hello a kérést:

1. Cserélje le *{hatókör}* hello hatókörrel, amelynek kívánja toolist hello szerepkör-hozzárendelések. hello a következő példák bemutatják, hogyan toospecify hello különböző szintű hatóköre:

   * Előfizetés: /subscriptions/ {előfizetés-azonosító}  
   * Erőforráscsoport: /subscriptions/ {előfizetés-azonosító} / resourceGroups/myresourcegroup1  
   * Erőforrás: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  
2. Cserélje le *{api-version}* a 2015-07-01.
3. Cserélje le *{szűrő}* hello feltétellel, hogy kívánja-e tooapply toofilter hello szerepkör társításának listája:

   * Lista szerepkör-hozzárendelések csak hello megadva hatókör, nem a szerepkör-hozzárendelések hello subscopes, beleértve:`atScope()`    
   * Egy adott felhasználó, csoport vagy alkalmazás szerepkör-hozzárendelések listában:`principalId%20eq%20'{objectId of user, group, or service principal}'`  
   * Egy adott felhasználó, például csoportokból örökölt szerepkör-hozzárendelések lista |}`assignedTo('{objectId of user}')`

### <a name="response"></a>Válasz
Állapotkód: 200

```
{
  "value": [
    {
      "properties": {
        "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7",
        "principalId": "2f9d4375-cbf1-48e8-83c9-2a0be4cb33fb",
        "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
        "createdOn": "2015-10-08T07:28:24.3905077Z",
        "updatedOn": "2015-10-08T07:28:24.3905077Z",
        "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
        "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
      },
      "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleAssignments/baa6e199-ad19-4667-b768-623fde31aedd",
      "type": "Microsoft.Authorization/roleAssignments",
      "name": "baa6e199-ad19-4667-b768-623fde31aedd"
    }
  ],
  "nextLink": null
}

```

## <a name="get-information-about-a-role-assignment"></a>Szerepkör-hozzárendelés adatainak beolvasása
Egyetlen szerepkör-hozzárendelés hello szerepkör-hozzárendelés azonosítója által megadott információ lekérése.

szerepkör-hozzárendelés tooget információt, hozzá kell férnie túl`Microsoft.Authorization/roleAssignments/read` műveletet. Minden hello beépített szerepkörök kapnak hozzáférést toothis műveletet. További információ a szerepkör-hozzárendelések és az Azure-erőforrások kezelése hozzáférés: [átruházásához hozzáférés-vezérlés](role-based-access-control-configure.md).

### <a name="request"></a>Kérés
Használjon hello **beolvasása** hello URI a következő metódust:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

Belül hello URI ellenőrizze a következő helyettesítések toocustomize hello a kérést:

1. Cserélje le *{hatókör}* hello hatókörrel, amelynek kívánja toolist hello szerepkör-hozzárendelések. hello a következő példák bemutatják, hogyan toospecify hello különböző szintű hatóköre:

   * Előfizetés: /subscriptions/ {előfizetés-azonosító}  
   * Erőforráscsoport: /subscriptions/ {előfizetés-azonosító} / resourceGroups/myresourcegroup1  
   * Erőforrás: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  
2. Cserélje le *{szerepkör-hozzárendelés-azonosító}* hello szerepkör-hozzárendelés hello GUID azonosítója.
3. Cserélje le *{api-version}* a 2015-07-01.

### <a name="response"></a>Válasz
Állapotkód: 200

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c",
    "principalId": "672f1afa-526a-4ef6-819c-975c7cd79022",
    "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
    "createdOn": "2015-10-05T08:36:26.4014813Z",
    "updatedOn": "2015-10-05T08:36:26.4014813Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleAssignments/196965ae-6088-4121-a92a-f1e33fdcc73e",
  "type": "Microsoft.Authorization/roleAssignments",
  "name": "196965ae-6088-4121-a92a-f1e33fdcc73e"
}

```

## <a name="create-a-role-assignment"></a>Szerepkör-hozzárendelés létrehozása
Szerepkör létrehozása hello megadott egyszerű támogatást nyújtó hello megadott szerepkörhöz megadott hello-hozzárendelés hatókörét.

szerepkör-hozzárendelés toocreate, hozzá kell férnie túl`Microsoft.Authorization/roleAssignments/write` műveletet. Hello beépített szerepkörök, csak *tulajdonos* és *felhasználói hozzáférés adminisztrátora* kapnak hozzáférést toothis műveletet. További információ a szerepkör-hozzárendelések és az Azure-erőforrások kezelése hozzáférés: [átruházásához hozzáférés-vezérlés](role-based-access-control-configure.md).

### <a name="request"></a>Kérés
Használjon hello **PUT** hello URI a következő metódust:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

Belül hello URI ellenőrizze a következő helyettesítések toocustomize hello a kérést:

1. Cserélje le *{hatókör}* hello hatókörrel, amellyel kívánja toocreate hello szerepkör-hozzárendelések. Ha a szerepkör-hozzárendelés létrehozása a szülő hatókörben, minden gyermekhatókörében öröklése hello azonos szerepkör-hozzárendelés. hello a következő példák bemutatják, hogyan toospecify hello különböző szintű hatóköre:

   * Előfizetés: /subscriptions/ {előfizetés-azonosító}  
   * Erőforráscsoport: /subscriptions/ {előfizetés-azonosító} / resourceGroups/myresourcegroup1   
   * Erőforrás: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  
2. Cserélje le *{szerepkör-hozzárendelés-azonosító}* egy új GUID, amely válik hello új szerepkör-hozzárendelés hello GUID azonosítója.
3. Cserélje le *{api-version}* a 2015-07-01.

Hello kérelemtörzset adja meg a hello hello formátuma a következő értékeket:

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
    "principalId": "5ac84765-1c8c-4994-94b2-629461bd191b"
  }
}

```

| Elem neve | Szükséges | Típus | Leírás |
| --- | --- | --- | --- |
| roledefinitionid-értékkel |Igen |Karakterlánc |hello szerepkör hello azonosítója hello hello azonosító formátuma:`{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id-guid}` |
| principalId |Igen |Karakterlánc |az Azure AD hello rendszerbiztonsági tag (felhasználó, csoport vagy egyszerű szolgáltatásnév) objectId toowhich hello szerepkör van hozzárendelve. |

### <a name="response"></a>Válasz
Állapotkód: 201-et

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
    "principalId": "5ac84765-1c8c-4994-94b2-629461bd191b",
    "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND",
    "createdOn": "2015-12-16T00:27:19.6447515Z",
    "updatedOn": "2015-12-16T00:27:19.6447515Z",
    "createdBy": null,
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND/providers/Microsoft.Authorization/roleAssignments/2e9e86c8-0e91-4958-b21f-20f51f27bab2",
  "type": "Microsoft.Authorization/roleAssignments",
  "name": "2e9e86c8-0e91-4958-b21f-20f51f27bab2"
}

```

## <a name="delete-a-role-assignment"></a>Szerepkör-hozzárendelés törlése
Hello szerepkör-hozzárendelés törlése a megadott hatókörben.

szerepkör-hozzárendelés toodelete, rendelkeznie kell hozzáféréssel toohello `Microsoft.Authorization/roleAssignments/delete` műveletet. Hello beépített szerepkörök, csak *tulajdonos* és *felhasználói hozzáférés adminisztrátora* kapnak hozzáférést toothis műveletet. További információ a szerepkör-hozzárendelések és az Azure-erőforrások kezelése hozzáférés: [átruházásához hozzáférés-vezérlés](role-based-access-control-configure.md).

### <a name="request"></a>Kérés
Használjon hello **törlése** hello URI a következő metódust:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

Belül hello URI ellenőrizze a következő helyettesítések toocustomize hello a kérést:

1. Cserélje le *{hatókör}* hello hatókörrel, amellyel kívánja toocreate hello szerepkör-hozzárendelések. hello a következő példák bemutatják, hogyan toospecify hello különböző szintű hatóköre:

   * Előfizetés: /subscriptions/ {előfizetés-azonosító}  
   * Erőforráscsoport: /subscriptions/ {előfizetés-azonosító} / resourceGroups/myresourcegroup1  
   * Erőforrás: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  
2. Cserélje le *{szerepkör-hozzárendelés-azonosító}* a hello GUID azonosító.
3. Cserélje le *{api-version}* a 2015-07-01.

### <a name="response"></a>Válasz
Állapotkód: 200

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
    "principalId": "5ac84765-1c8c-4994-94b2-629461bd191b",
    "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND",
    "createdOn": "2015-12-17T23:21:40.8921564Z",
    "updatedOn": "2015-12-17T23:21:40.8921564Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND/providers/Microsoft.Authorization/roleAssignments/5eec22ee-ea5c-431e-8f41-82c560706fd2",
  "type": "Microsoft.Authorization/roleAssignments",
  "name": "5eec22ee-ea5c-431e-8f41-82c560706fd2"
}

```

## <a name="list-all-roles"></a>Az összes szerepkör felsorolása
Felsorolja az összes hello szerepkör-hozzárendelés megadott hello elérhető hatókör.

toolist szerepkörök, hozzá kell férnie túl`Microsoft.Authorization/roleDefinitions/read` művelet hello hatókörben. Minden hello beépített szerepkörök kapnak hozzáférést toothis műveletet. További információ a szerepkör-hozzárendelések és az Azure-erőforrások kezelése hozzáférés: [átruházásához hozzáférés-vezérlés](role-based-access-control-configure.md).

### <a name="request"></a>Kérés
Használjon hello **beolvasása** hello URI a következő metódust:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions?api-version={api-version}&$filter={filter}

Belül hello URI ellenőrizze a következő helyettesítések toocustomize hello a kérést:

1. Cserélje le *{hatókör}* , amelynek kívánja toolist hello szerepkörök hello hatókörrel. hello a következő példák bemutatják, hogyan toospecify hello különböző szintű hatóköre:

   * Előfizetés: /subscriptions/ {előfizetés-azonosító}  
   * Erőforráscsoport: /subscriptions/ {előfizetés-azonosító} / resourceGroups/myresourcegroup1  
   * Erőforrás /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  
2. Cserélje le *{api-version}* a 2015-07-01.
3. Cserélje le *{szűrő}* hello feltétellel, hogy kívánja-e tooapply toofilter hello szerepkörök listája:

   * Lista szerepkörökhöz rendelhető hozzá hello megadott hatókörrel és a gyermekhatókör bármelyikét:`atScopeAndBelow()`
   * Keresse meg egy szerepkör használatával pontos megjelenített név: `roleName%20eq%20'{role-display-name}'`. Képernyőn hello URL-kódolású hello pontos megjelenített névbe hello szerepkör. Például`$filter=roleName%20eq%20'Virtual%20Machine%20Contributor'` |

### <a name="response"></a>Válasz
Állapotkód: 200

```
{
  "value": [
    {
      "properties": {
        "roleName": "Virtual Machine Contributor",
        "type": "BuiltInRole",
        "description": "Lets you manage virtual machines, but not access toothem, and not hello virtual network or storage account they\u2019re connected to.",
        "assignableScopes": [
          "/"
        ],
        "permissions": [
          {
            "actions": [
              "Microsoft.Authorization/*/read",
              "Microsoft.Compute/availabilitySets/*",
              "Microsoft.Compute/locations/*",
              "Microsoft.Compute/virtualMachines/*",
              "Microsoft.Compute/virtualMachineScaleSets/*",
              "Microsoft.Insights/alertRules/*",
              "Microsoft.Network/applicationGateways/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatRules/join/action",
              "Microsoft.Network/loadBalancers/read",
              "Microsoft.Network/locations/*",
              "Microsoft.Network/networkInterfaces/*",
              "Microsoft.Network/networkSecurityGroups/join/action",
              "Microsoft.Network/networkSecurityGroups/read",
              "Microsoft.Network/publicIPAddresses/join/action",
              "Microsoft.Network/publicIPAddresses/read",
              "Microsoft.Network/virtualNetworks/read",
              "Microsoft.Network/virtualNetworks/subnets/join/action",
              "Microsoft.Resources/deployments/*",
              "Microsoft.Resources/subscriptions/resourceGroups/read",
              "Microsoft.Storage/storageAccounts/listKeys/action",
              "Microsoft.Storage/storageAccounts/read",
              "Microsoft.Support/*"
            ],
            "notActions": []
          }
        ],
        "createdOn": "2015-06-02T00:18:27.3542698Z",
        "updatedOn": "2015-12-08T03:16:55.6170255Z",
        "createdBy": null,
        "updatedBy": null
      },
      "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
      "type": "Microsoft.Authorization/roleDefinitions",
      "name": "9980e02c-c2be-4d73-94e8-173b1dc7cf3c"
    }
  ],
  "nextLink": null
}

```

## <a name="get-information-about-a-role"></a>Egy szerepkör adatainak beolvasása
Egyetlen szerepkör hello szerepkör-definíció azonosítója által megadott információ lekérése. a megjelenített név megadásával egyetlen szerepkör tooget információkat lásd: [listában az összes szerepkör](role-based-access-control-manage-access-rest.md#list-all-roles).

egy szerepkör tooget információt, hozzá kell férnie túl`Microsoft.Authorization/roleDefinitions/read` műveletet. Minden hello beépített szerepkörök kapnak hozzáférést toothis műveletet. További információ a szerepkör-hozzárendelések és az Azure-erőforrások kezelése hozzáférés: [átruházásához hozzáférés-vezérlés](role-based-access-control-configure.md).

### <a name="request"></a>Kérés
Használjon hello **beolvasása** hello URI a következő metódust:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

Belül hello URI ellenőrizze a következő helyettesítések toocustomize hello a kérést:

1. Cserélje le *{hatókör}* hello hatókörrel, amelynek kívánja toolist hello szerepkör-hozzárendelések. hello a következő példák bemutatják, hogyan toospecify hello különböző szintű hatóköre:

   * Előfizetés: /subscriptions/ {előfizetés-azonosító}  
   * Erőforráscsoport: /subscriptions/ {előfizetés-azonosító} / resourceGroups/myresourcegroup1  
   * Erőforrás: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  
2. Cserélje le *{szerepkör-definíció-azonosító}* hello szerepkör-definíció hello GUID azonosítója.
3. Cserélje le *{api-version}* a 2015-07-01.

### <a name="response"></a>Válasz
Állapotkód: 200

```
{
  "value": [
    {
      "properties": {
        "roleName": "Virtual Machine Contributor",
        "type": "BuiltInRole",
        "description": "Lets you manage virtual machines, but not access toothem, and not hello virtual network or storage account they\u2019re connected to.",
        "assignableScopes": [
          "/"
        ],
        "permissions": [
          {
            "actions": [
              "Microsoft.Authorization/*/read",
              "Microsoft.Compute/availabilitySets/*",
              "Microsoft.Compute/locations/*",
              "Microsoft.Compute/virtualMachines/*",
              "Microsoft.Compute/virtualMachineScaleSets/*",
              "Microsoft.Insights/alertRules/*",
              "Microsoft.Network/applicationGateways/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatRules/join/action",
              "Microsoft.Network/loadBalancers/read",
              "Microsoft.Network/locations/*",
              "Microsoft.Network/networkInterfaces/*",
              "Microsoft.Network/networkSecurityGroups/join/action",
              "Microsoft.Network/networkSecurityGroups/read",
              "Microsoft.Network/publicIPAddresses/join/action",
              "Microsoft.Network/publicIPAddresses/read",
              "Microsoft.Network/virtualNetworks/read",
              "Microsoft.Network/virtualNetworks/subnets/join/action",
              "Microsoft.Resources/deployments/*",
              "Microsoft.Resources/subscriptions/resourceGroups/read",
              "Microsoft.Storage/storageAccounts/listKeys/action",
              "Microsoft.Storage/storageAccounts/read",
              "Microsoft.Support/*"
            ],
            "notActions": []
          }
        ],
        "createdOn": "2015-06-02T00:18:27.3542698Z",
        "updatedOn": "2015-12-08T03:16:55.6170255Z",
        "createdBy": null,
        "updatedBy": null
      },
      "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
      "type": "Microsoft.Authorization/roleDefinitions",
      "name": "9980e02c-c2be-4d73-94e8-173b1dc7cf3c"
    }
  ],
  "nextLink": null
}

```

## <a name="create-a-custom-role"></a>Egyéni szerepkör létrehozása
Hozzon létre egy egyéni biztonsági szerepkört.

toocreate egy egyéni biztonsági szerepkört, hozzá kell férnie túl`Microsoft.Authorization/roleDefinitions/write` összes hello művelet `AssignableScopes`. Hello beépített szerepkörök, csak *tulajdonos* és *felhasználói hozzáférés adminisztrátora* kapnak hozzáférést toothis műveletet. További információ a szerepkör-hozzárendelések és az Azure-erőforrások kezelése hozzáférés: [átruházásához hozzáférés-vezérlés](role-based-access-control-configure.md).

### <a name="request"></a>Kérés
Használjon hello **PUT** hello URI a következő metódust:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

Belül hello URI ellenőrizze a következő helyettesítések toocustomize hello a kérést:

1. Cserélje le *{hatókör}* hello az első *AssignableScope* hello egyéni szerepkör. hello a következő példák bemutatják, hogyan toospecify hello különböző szintű hatóköre.

   * Előfizetés: /subscriptions/ {előfizetés-azonosító}  
   * Erőforráscsoport: /subscriptions/ {előfizetés-azonosító} / resourceGroups/myresourcegroup1  
   * Erőforrás: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  
2. Cserélje le *{szerepkör-definíció-azonosító}* egy új GUID, amely válik hello új egyéni szerepkör hello GUID azonosítója.
3. Cserélje le *{api-version}* a 2015-07-01.

Hello kérelemtörzset adja meg a hello hello formátuma a következő értékeket:

```
{
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "properties": {
    "roleName": "Virtual Machine Operator",
    "description": "Lets you monitor virtual machines and restart them.",
    "type": "CustomRole",
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ]
  }
}

```

| Elem neve | Szükséges | Típus | Leírás |
| --- | --- | --- | --- |
| név |Igen |Karakterlánc |Egyéni szerepkör hello GUID azonosítója |
| properties.roleName |Igen |Karakterlánc |Hello egyéni szerepkör nevét jeleníti meg. Legfeljebb 128 karakter. |
| Properties.Description |Nem |Karakterlánc |Hello egyéni szerepkör leírása Maximális méret 1024 karakternél. |
| Properties.Type |Igen |Karakterlánc |Állítsa be a túl "CustomRole." |
| Properties.permissions.Actions |Igen |String] |A művelet tömbjét karakterláncok hello egyéni szerepkör által biztosított megadását hello műveletek. |
| properties.permissions.notActions |Nem |String] |Hello műveletek tooexclude hello egyéni szerepkör által biztosított hello műveletekből művelet karakterláncokkal tömbjét. |
| properties.assignableScopes |Igen |String] |A mely hello egyéni biztonsági szerepkört is használható hatókörök tömbjét. |

### <a name="response"></a>Válasz
Állapotkód: 201-et

```
{
  "properties": {
    "roleName": "Virtual Machine Operator",
    "type": "CustomRole",
    "description": "Lets you monitor virtual machines and restart them.",
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ],
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "createdOn": "2015-12-18T00:10:51.4662695Z",
    "updatedOn": "2015-12-18T00:10:51.4662695Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "type": "Microsoft.Authorization/roleDefinitions",
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7"
}

```

## <a name="update-a-custom-role"></a>Frissítés egy egyéni biztonsági szerepkört
Módosítani egy egyéni biztonsági szerepkört.

toomodify egy egyéni biztonsági szerepkört, hozzá kell férnie túl`Microsoft.Authorization/roleDefinitions/write` összes hello művelet `AssignableScopes`. Hello beépített szerepkörök, csak *tulajdonos* és *felhasználói hozzáférés adminisztrátora* kapnak hozzáférést toothis műveletet. További információ a szerepkör-hozzárendelések és az Azure-erőforrások kezelése hozzáférés: [átruházásához hozzáférés-vezérlés](role-based-access-control-configure.md).

### <a name="request"></a>Kérés
Használjon hello **PUT** hello URI a következő metódust:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

Belül hello URI ellenőrizze a következő helyettesítések toocustomize hello a kérést:

1. Cserélje le *{hatókör}* hello az első *AssignableScope* hello egyéni szerepkör. hello a következő példák bemutatják, hogyan toospecify hello különböző szintű hatóköre:

   * Előfizetés: /subscriptions/ {előfizetés-azonosító}  
   * Erőforráscsoport: /subscriptions/ {előfizetés-azonosító} / resourceGroups/myresourcegroup1  
   * Erőforrás: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  
2. Cserélje le *{szerepkör-definíció-azonosító}* hello egyéni szerepkör hello GUID azonosítója.
3. Cserélje le *{api-version}* a 2015-07-01.

Hello kérelemtörzset adja meg a hello hello formátuma a következő értékeket:

```
{
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "properties": {
    "roleName": "Virtual Machine Operator",
    "description": "Lets you monitor virtual machines and restart them.",
    "type": "CustomRole",
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ]
  }
}

```

| Elem neve | Szükséges | Típus | Leírás |
| --- | --- | --- | --- |
| név |Igen |Karakterlánc |Egyéni szerepkör hello GUID azonosítója |
| properties.roleName |Igen |Karakterlánc |Hello frissített egyéni szerepkör nevét jeleníti meg. |
| Properties.Description |Nem |Karakterlánc |Hello leírása frissítve egyéni biztonsági szerepkört. |
| Properties.Type |Igen |Karakterlánc |Állítsa be a túl "CustomRole." |
| Properties.permissions.Actions |Igen |String] |Hello műveletek toowhich hello megadása művelet karakterláncok frissítése egyéni szerepkör engedélyezi a hozzáférést. |
| properties.permissions.notActions |Nem |String] |Művelet tömbje karakterláncok megadását hello műveletek tooexclude hello műveletek, melyik hello frissítése egyéni szerepkör biztosít. |
| properties.assignableScopes |Igen |String] |A tömb hatókörök mely hello a frissített egyéni biztonsági szerepkört is használható. |

### <a name="response"></a>Válasz
Állapotkód: 201-et

```
{
  "properties": {
    "roleName": "Virtual Machine Operator",
    "type": "CustomRole",
    "description": "Lets you monitor virtual machines and restart them.",
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ],
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "createdOn": "2015-12-18T00:10:51.4662695Z",
    "updatedOn": "2015-12-18T00:10:51.4662695Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "type": "Microsoft.Authorization/roleDefinitions",
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7"
}

```

## <a name="delete-a-custom-role"></a>Egyéni szerepkör törléséhez
Egyéni szerepkör törléséhez.

toodelete egy egyéni biztonsági szerepkört, hozzá kell férnie túl`Microsoft.Authorization/roleDefinitions/delete` összes hello művelet `AssignableScopes`. Hello beépített szerepkörök, csak *tulajdonos* és *felhasználói hozzáférés adminisztrátora* kapnak hozzáférést toothis műveletet. További információ a szerepkör-hozzárendelések és az Azure-erőforrások kezelése hozzáférés: [átruházásához hozzáférés-vezérlés](role-based-access-control-configure.md).

### <a name="request"></a>Kérés
Használjon hello **törlése** hello URI a következő metódust:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

Belül hello URI ellenőrizze a következő helyettesítések toocustomize hello a kérést:

1. Cserélje le *{hatókör}* hello hatókörrel, amellyel toodelete hello szerepkör-definíció kívánja. hello a következő példák bemutatják, hogyan toospecify hello különböző szintű hatóköre:

   * Előfizetés: /subscriptions/ {előfizetés-azonosító}  
   * Erőforráscsoport: /subscriptions/ {előfizetés-azonosító} / resourceGroups/myresourcegroup1  
   * Erőforrás: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  
2. Cserélje le *{szerepkör-definíció-azonosító}* a hello GUID szerepkör-definíció azonosítója hello egyéni szerepkör.
3. Cserélje le *{api-version}* a 2015-07-01.

### <a name="response"></a>Válasz
Állapotkód: 200

```
{
  "properties": {
    "roleName": "Virtual Machine Operator",
    "type": "CustomRole",
    "description": "Lets you monitor virtual machines and restart them.",
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ],
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "createdOn": "2015-12-16T00:07:02.9236555Z",
    "updatedOn": "2015-12-16T00:07:02.9236555Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/0bd62a70-e1b8-4e0b-a7c2-75cab365c95b",
  "type": "Microsoft.Authorization/roleDefinitions",
  "name": "0bd62a70-e1b8-4e0b-a7c2-75cab365c95b"
}

```

## <a name="next-steps"></a>Következő lépések

[!INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]
