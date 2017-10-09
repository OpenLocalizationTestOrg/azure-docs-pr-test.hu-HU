---
title: "aaaTenant admin jogosultságszintjének emelése – az Azure AD |} Microsoft Docs"
description: "Ez a témakör ismerteti a beépített szerepkörök, szerepkör-alapú hozzáférés-vezérlés (RBAC) hello."
services: active-directory
documentationcenter: 
author: andredm7
manager: femila
editor: rqureshi
ms.assetid: b547c5a5-2da2-4372-9938-481cb962d2d6
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andredm
ms.openlocfilehash: 7996f2af3277dc40e2a1766cc4a7862a2399cdef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="elevate-access-as-a-tenant-admin-with-role-based-access-control"></a>Bérlői rendszergazdaként a szerepköralapú hozzáférés-vezérlés jogosultságszintjének emelése

Szerepköralapú hozzáférés-vezérlés segít a bérlői rendszergazdák úgy, hogy azok engedélyeket adhat magasabb, mint a normál ideiglenes jogosultságszint-emelései beolvasása a hozzáférést. A bérlői rendszergazda toohello felhasználói hozzáférés adminisztrátora szerepkör szükség esetén megszemélyesítve saját magát. Ez a szerepkör hello ad bérlői rendszergazdai engedélyek toogrant saját magát vagy mások hello helyrendszerszerepköreivel "/" hatókörben.

Ez a funkció fontos, mert lehetővé teszi a hello Bérlői rendszergazda toosee összes hello a szervezet meglévő előfizetések. Lehetővé teszi, hogy automation alkalmazások (például a számlázással és a naplózás) tooaccess hello előfizetéseket és a számlázással vagy az eszköz felügyeleti hello szervezet hello állapotát egy pontos képet ad.  

## <a name="how-toouse-elevateaccess-toogive-tenant-access"></a>Hogyan toouse elevateAccess toogive bérlői hozzáférés

hello alapvető folyamat együttműködve hello a következő lépéseket:

1. Használja a többi, hívja *elevateAccess*, mely biztosít, akkor a felhasználói hozzáférés adminisztrátora szerepkör hello "/" hatókör.

    ```
    POST https://management.azure.com/providers/Microsoft.Authorization/elevateAccess?api-version=2016-07-01
    ```

2. Hozzon létre egy [szerepkör-hozzárendelés](/rest/api/authorization/roleassignments) tooassign szerepköröket bármely hatókörben. a következő példa hello hello tulajdonságok hozzárendelésére szolgáló hello olvasó szerepkört: "/" hatókör jeleníti meg:

    ```
    { "properties":{
    "roleDefinitionId": "providers/Microsoft.Authorization/roleDefinitions/acdd72a7338548efbd42f606fba81ae7",
    "principalId": "cbc5e050-d7cd-4310-813b-4870be8ef5bb",
    "scope": "/"
    },
    "id": "providers/Microsoft.Authorization/roleAssignments/64736CA0-56D7-4A94-A551-973C2FE7888B",
    "type": "Microsoft.Authorization/roleAssignments",
    "name": "64736CA0-56D7-4A94-A551-973C2FE7888B"
    }
    ```

3. Egy felhasználó hozzáférési rendszergazdai közben meg is is, szerepkör-hozzárendelések törlése "/" hatókör.

4. Vonja vissza a felhasználói hozzáférés rendszergazdai jogosultságokkal, amíg azokat újra van szükség.


## <a name="how-tooundo-hello-elevateaccess-action"></a>Hogyan tooundo hello elevateAccess művelet

A hívás esetén *elevateAccess* szerepkör-hozzárendelés létrehozása a szolgáltatást, így toorevoke azokat jogosultsággal meg kell toodelete hello hozzárendelés.

1.  Hívás [GET roleDefinitions](/rest/api/authorization/roledefinitions#RoleDefinitions_Get) ahol roleName = felhasználói hozzáférés adminisztrátora toodetermine hello neve hello felhasználói hozzáférés adminisztrátora szerepkör GUID Azonosítóját. hello válasz kell kinéznie:

    ```
    {"value":[{"properties":{
    "roleName":"User Access Administrator",
    "type":"BuiltInRole",
    "description":"Lets you manage user access tooAzure resources.",
    "assignableScopes":["/"],
    "permissions":[{"actions":["*/read","Microsoft.Authorization/*","Microsoft.Support/*"],"notActions":[]}],
    "createdOn":"0001-01-01T08:00:00.0000000Z",
    "updatedOn":"2016-05-31T23:14:04.6964687Z",
    "createdBy":null,
    "updatedBy":null},
    "id":"/providers/Microsoft.Authorization/roleDefinitions/18d7d88d-d35e-4fb5-a5c3-7773c20a72d9",
    "type":"Microsoft.Authorization/roleDefinitions",
    "name":"18d7d88d-d35e-4fb5-a5c3-7773c20a72d9"}],
    "nextLink":null}
    ```

    Hello GUID mentése hello *neve* paraméter, ebben az esetben **18d7d88d-d35e-4fb5-a5c3-7773c20a72d9**.

2. Hívás [GET roleAssignments](/rest/api/authorization/roleassignments#RoleAssignments_Get) ahol principalId saját ObjectId =. A parancs megjeleníti az összes hozzárendelésének hello bérlő. Keressen egy hello ahol hello hatókör az "/" hello roledefinitionid-értékkel végződik hello szerepkörrel rendelkező néven GUID meg az 1. lépésben található. szerepkör-hozzárendelés hello kell kinéznie:

    ```
    {"value":[{"properties":{
    "roleDefinitionId":"/providers/Microsoft.Authorization/roleDefinitions/18d7d88d-d35e-4fb5-a5c3-7773c20a72d9",
    "principalId":"{objectID}",
    "scope":"/",
    "createdOn":"2016-08-17T19:21:16.3422480Z",
    "updatedOn":"2016-08-17T19:21:16.3422480Z",
    "createdBy":"93ce6722-3638-4222-b582-78b75c5c6d65",
    "updatedBy":"93ce6722-3638-4222-b582-78b75c5c6d65"},
    "id":"/providers/Microsoft.Authorization/roleAssignments/e7dd75bc-06f6-4e71-9014-ee96a929d099",
    "type":"Microsoft.Authorization/roleAssignments",
    "name":"e7dd75bc-06f6-4e71-9014-ee96a929d099"}],
    "nextLink":null}
    ```

    Ebben az esetben mentése hello GUID hello *neve* paraméter, ebben az esetben **e7dd75bc-06f6-4e71-9014-ee96a929d099**.

3. Végezetül hívás [DELETE roleAssignments](/rest/api/authorization/roleassignments#RoleAssignments_DeleteById) ahol roleAssignmentId = hello neve, a 2. lépésben található GUID.

## <a name="next-steps"></a>Következő lépések

- További információ [szerepköralapú hozzáférés-vezérlés többi kezelése](role-based-access-control-manage-access-rest.md)

- [Hozzáférés-hozzárendelések kezelése](role-based-access-control-manage-assignments.md) a hello Azure-portálon
