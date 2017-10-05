---
title: "Bérlői rendszergazda jogosultságszintjének emelése – az Azure AD |} Microsoft Docs"
description: "Ez a témakör ismerteti a beépített szerepkörök szerepköralapú hozzáférés-vezérlés (RBAC)."
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
ms.openlocfilehash: bf64a92b359a6f68d84fa5ee17eda64ed6371990
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="elevate-access-as-a-tenant-admin-with-role-based-access-control"></a><span data-ttu-id="6b520-103">Bérlői rendszergazdaként a szerepköralapú hozzáférés-vezérlés jogosultságszintjének emelése</span><span class="sxs-lookup"><span data-stu-id="6b520-103">Elevate access as a tenant admin with Role-Based Access Control</span></span>

<span data-ttu-id="6b520-104">Szerepköralapú hozzáférés-vezérlés segít a bérlői rendszergazdák úgy, hogy azok engedélyeket adhat magasabb, mint a normál ideiglenes jogosultságszint-emelései beolvasása a hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="6b520-104">Role-based Access Control helps tenant administrators get temporary elevations in access so that they can grant higher permissions than normal.</span></span> <span data-ttu-id="6b520-105">A bérlői rendszergazda emelhet saját magát, a felhasználói hozzáférés adminisztrátora szerepkörhöz szükség esetén.</span><span class="sxs-lookup"><span data-stu-id="6b520-105">A tenant admin can elevate herself to the User Access Administrator role when needed.</span></span> <span data-ttu-id="6b520-106">Ez a szerepkör biztosítja a bérlői rendszergazdai engedélyeket az saját magát vagy más szerepkörök a "/" hatókörben.</span><span class="sxs-lookup"><span data-stu-id="6b520-106">That role gives the tenant admin permissions to grant herself or others roles at the "/" scope.</span></span>

<span data-ttu-id="6b520-107">Ez a funkció fontos, mert lehetővé teszi a bérlői rendszergazda szerepel a szervezet összes előfizetés megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="6b520-107">This feature is important because it allows the tenant admin to see all the subscriptions that exist in an organization.</span></span> <span data-ttu-id="6b520-108">Automation-alkalmazások (például a számlázással és a naplózás) is lehetővé teszi az előfizetések eléréséhez, és adjon meg egy pontos képet a szervezet a számlázással vagy az eszköz felügyeleti állapotát.</span><span class="sxs-lookup"><span data-stu-id="6b520-108">It also allows for automation apps (like invoicing and auditing) to access all the subscriptions and provide an accurate view of the state of the organization for billing or asset management.</span></span>  

## <a name="how-to-use-elevateaccess-to-give-tenant-access"></a><span data-ttu-id="6b520-109">A bérlői hozzáférésének elevateAccess használata</span><span class="sxs-lookup"><span data-stu-id="6b520-109">How to use elevateAccess to give tenant access</span></span>

<span data-ttu-id="6b520-110">A folyamat alapvetően együttműködik az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="6b520-110">The basic process works with the following steps:</span></span>

1. <span data-ttu-id="6b520-111">Használja a többi, hívja *elevateAccess*, amely engedélyezi a felhasználói hozzáférés adminisztrátora szerepkör a(z) "/" hatókör.</span><span class="sxs-lookup"><span data-stu-id="6b520-111">Using REST, call *elevateAccess*, which grants you the User Access Administrator role at "/" scope.</span></span>

    ```
    POST https://management.azure.com/providers/Microsoft.Authorization/elevateAccess?api-version=2016-07-01
    ```

2. <span data-ttu-id="6b520-112">Hozzon létre egy [szerepkör-hozzárendelés](/rest/api/authorization/roleassignments) bármely hatókörből szerepköröket hozzárendelni.</span><span class="sxs-lookup"><span data-stu-id="6b520-112">Create a [role assignment](/rest/api/authorization/roleassignments) to assign any role at any scope.</span></span> <span data-ttu-id="6b520-113">A következő példa bemutatja a tulajdonságait: az olvasó szerepkört rendelni "/" hatóköre:</span><span class="sxs-lookup"><span data-stu-id="6b520-113">The following example shows the properties for assigning the Reader role at "/" scope:</span></span>

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

3. <span data-ttu-id="6b520-114">Egy felhasználó hozzáférési rendszergazdai közben meg is is, szerepkör-hozzárendelések törlése "/" hatókör.</span><span class="sxs-lookup"><span data-stu-id="6b520-114">While a User Access Admin, you can also delete role assignments at "/" scope.</span></span>

4. <span data-ttu-id="6b520-115">Vonja vissza a felhasználói hozzáférés rendszergazdai jogosultságokkal, amíg azokat újra van szükség.</span><span class="sxs-lookup"><span data-stu-id="6b520-115">Revoke your User Access Admin privileges until they're needed again.</span></span>


## <a name="how-to-undo-the-elevateaccess-action"></a><span data-ttu-id="6b520-116">A elevateAccess művelet visszavonása</span><span class="sxs-lookup"><span data-stu-id="6b520-116">How to undo the elevateAccess action</span></span>

<span data-ttu-id="6b520-117">A hívás esetén *elevateAccess* , szerepkör-hozzárendelés létrehozása a szolgáltatást, ezért ezek a jogosultságok visszavonásához szüksége lesz a hozzárendelést.</span><span class="sxs-lookup"><span data-stu-id="6b520-117">When you call *elevateAccess* you create a role assignment for yourself, so to revoke those privileges you need to delete the assignment.</span></span>

1.  <span data-ttu-id="6b520-118">Hívás [GET roleDefinitions](/rest/api/authorization/roledefinitions#RoleDefinitions_Get) ahol roleName = a felhasználói hozzáférés adminisztrátora a felhasználói hozzáférés adminisztrátora szerepkör GUID nevének megállapítása érdekében.</span><span class="sxs-lookup"><span data-stu-id="6b520-118">Call [GET roleDefinitions](/rest/api/authorization/roledefinitions#RoleDefinitions_Get) where roleName = User Access Administrator to determine the name GUID of the User Access Administrator role.</span></span> <span data-ttu-id="6b520-119">A válasz kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="6b520-119">The response should look like this:</span></span>

    ```
    {"value":[{"properties":{
    "roleName":"User Access Administrator",
    "type":"BuiltInRole",
    "description":"Lets you manage user access to Azure resources.",
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

    <span data-ttu-id="6b520-120">Mentse a GUID-Azonosítójának a *neve* paraméter, ebben az esetben **18d7d88d-d35e-4fb5-a5c3-7773c20a72d9**.</span><span class="sxs-lookup"><span data-stu-id="6b520-120">Save the GUID from the *name* parameter, in this case **18d7d88d-d35e-4fb5-a5c3-7773c20a72d9**.</span></span>

2. <span data-ttu-id="6b520-121">Hívás [GET roleAssignments](/rest/api/authorization/roleassignments#RoleAssignments_Get) ahol principalId saját ObjectId =.</span><span class="sxs-lookup"><span data-stu-id="6b520-121">Call [GET roleAssignments](/rest/api/authorization/roleassignments#RoleAssignments_Get) where principalId = your own ObjectId.</span></span> <span data-ttu-id="6b520-122">A parancs megjeleníti az összes hozzárendelésének a bérlő.</span><span class="sxs-lookup"><span data-stu-id="6b520-122">This lists all your assignments in the tenant.</span></span> <span data-ttu-id="6b520-123">A keres, ahol a hatókör az "/", és a roledefinitionid-értékkel végződik-e a szerepkör nevét, az 1. lépésben található GUID.</span><span class="sxs-lookup"><span data-stu-id="6b520-123">Look for the one where the scope is "/" and the RoleDefinitionId ends with the role name GUID you found in step 1.</span></span> <span data-ttu-id="6b520-124">A szerepkör-hozzárendelés kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="6b520-124">The role assignment should look like this:</span></span>

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

    <span data-ttu-id="6b520-125">Ebben az esetben a GUID-Azonosítójának mentése a *neve* paraméter, ebben az esetben **e7dd75bc-06f6-4e71-9014-ee96a929d099**.</span><span class="sxs-lookup"><span data-stu-id="6b520-125">Again, save the GUID from the *name* parameter, in this case **e7dd75bc-06f6-4e71-9014-ee96a929d099**.</span></span>

3. <span data-ttu-id="6b520-126">Végezetül hívás [DELETE roleAssignments](/rest/api/authorization/roleassignments#RoleAssignments_DeleteById) ahol roleAssignmentId = a neve, a 2. lépésben található GUID.</span><span class="sxs-lookup"><span data-stu-id="6b520-126">Finally, call [DELETE roleAssignments](/rest/api/authorization/roleassignments#RoleAssignments_DeleteById) where roleAssignmentId = the name GUID you found in step 2.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6b520-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6b520-127">Next steps</span></span>

- <span data-ttu-id="6b520-128">További információ [szerepköralapú hozzáférés-vezérlés többi kezelése](role-based-access-control-manage-access-rest.md)</span><span class="sxs-lookup"><span data-stu-id="6b520-128">Learn more about [managing Role-Based Access Control with REST](role-based-access-control-manage-access-rest.md)</span></span>

- <span data-ttu-id="6b520-129">[Hozzáférés-hozzárendelések kezelése](role-based-access-control-manage-assignments.md) az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="6b520-129">[Manage access assignments](role-based-access-control-manage-assignments.md) in the Azure portal</span></span>
