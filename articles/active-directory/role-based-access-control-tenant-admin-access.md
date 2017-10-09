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
# <a name="elevate-access-as-a-tenant-admin-with-role-based-access-control"></a><span data-ttu-id="1bbe0-103">Bérlői rendszergazdaként a szerepköralapú hozzáférés-vezérlés jogosultságszintjének emelése</span><span class="sxs-lookup"><span data-stu-id="1bbe0-103">Elevate access as a tenant admin with Role-Based Access Control</span></span>

<span data-ttu-id="1bbe0-104">Szerepköralapú hozzáférés-vezérlés segít a bérlői rendszergazdák úgy, hogy azok engedélyeket adhat magasabb, mint a normál ideiglenes jogosultságszint-emelései beolvasása a hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="1bbe0-104">Role-based Access Control helps tenant administrators get temporary elevations in access so that they can grant higher permissions than normal.</span></span> <span data-ttu-id="1bbe0-105">A bérlői rendszergazda toohello felhasználói hozzáférés adminisztrátora szerepkör szükség esetén megszemélyesítve saját magát.</span><span class="sxs-lookup"><span data-stu-id="1bbe0-105">A tenant admin can elevate herself toohello User Access Administrator role when needed.</span></span> <span data-ttu-id="1bbe0-106">Ez a szerepkör hello ad bérlői rendszergazdai engedélyek toogrant saját magát vagy mások hello helyrendszerszerepköreivel "/" hatókörben.</span><span class="sxs-lookup"><span data-stu-id="1bbe0-106">That role gives hello tenant admin permissions toogrant herself or others roles at hello "/" scope.</span></span>

<span data-ttu-id="1bbe0-107">Ez a funkció fontos, mert lehetővé teszi a hello Bérlői rendszergazda toosee összes hello a szervezet meglévő előfizetések.</span><span class="sxs-lookup"><span data-stu-id="1bbe0-107">This feature is important because it allows hello tenant admin toosee all hello subscriptions that exist in an organization.</span></span> <span data-ttu-id="1bbe0-108">Lehetővé teszi, hogy automation alkalmazások (például a számlázással és a naplózás) tooaccess hello előfizetéseket és a számlázással vagy az eszköz felügyeleti hello szervezet hello állapotát egy pontos képet ad.</span><span class="sxs-lookup"><span data-stu-id="1bbe0-108">It also allows for automation apps (like invoicing and auditing) tooaccess all hello subscriptions and provide an accurate view of hello state of hello organization for billing or asset management.</span></span>  

## <a name="how-toouse-elevateaccess-toogive-tenant-access"></a><span data-ttu-id="1bbe0-109">Hogyan toouse elevateAccess toogive bérlői hozzáférés</span><span class="sxs-lookup"><span data-stu-id="1bbe0-109">How toouse elevateAccess toogive tenant access</span></span>

<span data-ttu-id="1bbe0-110">hello alapvető folyamat együttműködve hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="1bbe0-110">hello basic process works with hello following steps:</span></span>

1. <span data-ttu-id="1bbe0-111">Használja a többi, hívja *elevateAccess*, mely biztosít, akkor a felhasználói hozzáférés adminisztrátora szerepkör hello "/" hatókör.</span><span class="sxs-lookup"><span data-stu-id="1bbe0-111">Using REST, call *elevateAccess*, which grants you hello User Access Administrator role at "/" scope.</span></span>

    ```
    POST https://management.azure.com/providers/Microsoft.Authorization/elevateAccess?api-version=2016-07-01
    ```

2. <span data-ttu-id="1bbe0-112">Hozzon létre egy [szerepkör-hozzárendelés](/rest/api/authorization/roleassignments) tooassign szerepköröket bármely hatókörben.</span><span class="sxs-lookup"><span data-stu-id="1bbe0-112">Create a [role assignment](/rest/api/authorization/roleassignments) tooassign any role at any scope.</span></span> <span data-ttu-id="1bbe0-113">a következő példa hello hello tulajdonságok hozzárendelésére szolgáló hello olvasó szerepkört: "/" hatókör jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="1bbe0-113">hello following example shows hello properties for assigning hello Reader role at "/" scope:</span></span>

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

3. <span data-ttu-id="1bbe0-114">Egy felhasználó hozzáférési rendszergazdai közben meg is is, szerepkör-hozzárendelések törlése "/" hatókör.</span><span class="sxs-lookup"><span data-stu-id="1bbe0-114">While a User Access Admin, you can also delete role assignments at "/" scope.</span></span>

4. <span data-ttu-id="1bbe0-115">Vonja vissza a felhasználói hozzáférés rendszergazdai jogosultságokkal, amíg azokat újra van szükség.</span><span class="sxs-lookup"><span data-stu-id="1bbe0-115">Revoke your User Access Admin privileges until they're needed again.</span></span>


## <a name="how-tooundo-hello-elevateaccess-action"></a><span data-ttu-id="1bbe0-116">Hogyan tooundo hello elevateAccess művelet</span><span class="sxs-lookup"><span data-stu-id="1bbe0-116">How tooundo hello elevateAccess action</span></span>

<span data-ttu-id="1bbe0-117">A hívás esetén *elevateAccess* szerepkör-hozzárendelés létrehozása a szolgáltatást, így toorevoke azokat jogosultsággal meg kell toodelete hello hozzárendelés.</span><span class="sxs-lookup"><span data-stu-id="1bbe0-117">When you call *elevateAccess* you create a role assignment for yourself, so toorevoke those privileges you need toodelete hello assignment.</span></span>

1.  <span data-ttu-id="1bbe0-118">Hívás [GET roleDefinitions](/rest/api/authorization/roledefinitions#RoleDefinitions_Get) ahol roleName = felhasználói hozzáférés adminisztrátora toodetermine hello neve hello felhasználói hozzáférés adminisztrátora szerepkör GUID Azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="1bbe0-118">Call [GET roleDefinitions](/rest/api/authorization/roledefinitions#RoleDefinitions_Get) where roleName = User Access Administrator toodetermine hello name GUID of hello User Access Administrator role.</span></span> <span data-ttu-id="1bbe0-119">hello válasz kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="1bbe0-119">hello response should look like this:</span></span>

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

    <span data-ttu-id="1bbe0-120">Hello GUID mentése hello *neve* paraméter, ebben az esetben **18d7d88d-d35e-4fb5-a5c3-7773c20a72d9**.</span><span class="sxs-lookup"><span data-stu-id="1bbe0-120">Save hello GUID from hello *name* parameter, in this case **18d7d88d-d35e-4fb5-a5c3-7773c20a72d9**.</span></span>

2. <span data-ttu-id="1bbe0-121">Hívás [GET roleAssignments](/rest/api/authorization/roleassignments#RoleAssignments_Get) ahol principalId saját ObjectId =.</span><span class="sxs-lookup"><span data-stu-id="1bbe0-121">Call [GET roleAssignments](/rest/api/authorization/roleassignments#RoleAssignments_Get) where principalId = your own ObjectId.</span></span> <span data-ttu-id="1bbe0-122">A parancs megjeleníti az összes hozzárendelésének hello bérlő.</span><span class="sxs-lookup"><span data-stu-id="1bbe0-122">This lists all your assignments in hello tenant.</span></span> <span data-ttu-id="1bbe0-123">Keressen egy hello ahol hello hatókör az "/" hello roledefinitionid-értékkel végződik hello szerepkörrel rendelkező néven GUID meg az 1. lépésben található.</span><span class="sxs-lookup"><span data-stu-id="1bbe0-123">Look for hello one where hello scope is "/" and hello RoleDefinitionId ends with hello role name GUID you found in step 1.</span></span> <span data-ttu-id="1bbe0-124">szerepkör-hozzárendelés hello kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="1bbe0-124">hello role assignment should look like this:</span></span>

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

    <span data-ttu-id="1bbe0-125">Ebben az esetben mentése hello GUID hello *neve* paraméter, ebben az esetben **e7dd75bc-06f6-4e71-9014-ee96a929d099**.</span><span class="sxs-lookup"><span data-stu-id="1bbe0-125">Again, save hello GUID from hello *name* parameter, in this case **e7dd75bc-06f6-4e71-9014-ee96a929d099**.</span></span>

3. <span data-ttu-id="1bbe0-126">Végezetül hívás [DELETE roleAssignments](/rest/api/authorization/roleassignments#RoleAssignments_DeleteById) ahol roleAssignmentId = hello neve, a 2. lépésben található GUID.</span><span class="sxs-lookup"><span data-stu-id="1bbe0-126">Finally, call [DELETE roleAssignments](/rest/api/authorization/roleassignments#RoleAssignments_DeleteById) where roleAssignmentId = hello name GUID you found in step 2.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1bbe0-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1bbe0-127">Next steps</span></span>

- <span data-ttu-id="1bbe0-128">További információ [szerepköralapú hozzáférés-vezérlés többi kezelése](role-based-access-control-manage-access-rest.md)</span><span class="sxs-lookup"><span data-stu-id="1bbe0-128">Learn more about [managing Role-Based Access Control with REST](role-based-access-control-manage-access-rest.md)</span></span>

- <span data-ttu-id="1bbe0-129">[Hozzáférés-hozzárendelések kezelése](role-based-access-control-manage-assignments.md) a hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="1bbe0-129">[Manage access assignments](role-based-access-control-manage-assignments.md) in hello Azure portal</span></span>
