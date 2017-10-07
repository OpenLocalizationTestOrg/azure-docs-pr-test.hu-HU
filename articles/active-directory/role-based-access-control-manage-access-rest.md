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
# <a name="manage-role-based-access-control-with-hello-rest-api"></a><span data-ttu-id="db51e-103">Szerepköralapú hozzáférés-vezérlés a REST API hello kezelése</span><span class="sxs-lookup"><span data-stu-id="db51e-103">Manage Role-Based Access Control with hello REST API</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="db51e-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="db51e-104">PowerShell</span></span>](role-based-access-control-manage-access-powershell.md)
> * [<span data-ttu-id="db51e-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="db51e-105">Azure CLI</span></span>](role-based-access-control-manage-access-azure-cli.md)
> * [<span data-ttu-id="db51e-106">REST API</span><span class="sxs-lookup"><span data-stu-id="db51e-106">REST API</span></span>](role-based-access-control-manage-access-rest.md)

<span data-ttu-id="db51e-107">Szerepköralapú hozzáférés-vezérlés (RBAC) a hello Azure-portálon, és az Azure Resource Manager API segítségével felügyelheti a hozzáférési tooyour előfizetés és a minden részletre kiterjedő szinten erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="db51e-107">Role-Based Access Control (RBAC) in hello Azure portal and Azure Resource Manager API helps you manage access tooyour subscription and resources at a fine-grained level.</span></span> <span data-ttu-id="db51e-108">Ez a szolgáltatás egyes szerepkörök toothem egy adott hatókör hozzárendelése szerint engedélyezheti a hozzáférést az Active Directory felhasználók, csoportok vagy szolgáltatásnevekről.</span><span class="sxs-lookup"><span data-stu-id="db51e-108">With this feature, you can grant access for Active Directory users, groups, or service principals by assigning some roles toothem at a particular scope.</span></span>

## <a name="list-all-role-assignments"></a><span data-ttu-id="db51e-109">Az összes szerepkör-hozzárendelések felsorolása</span><span class="sxs-lookup"><span data-stu-id="db51e-109">List all role assignments</span></span>
<span data-ttu-id="db51e-110">Listák összes hello szerepkör-hozzárendelések a megadott hello hatókörét, és subscopes.</span><span class="sxs-lookup"><span data-stu-id="db51e-110">Lists all hello role assignments at hello specified scope and subscopes.</span></span>

<span data-ttu-id="db51e-111">szerepkör-hozzárendelések toolist, hozzá kell férnie túl`Microsoft.Authorization/roleAssignments/read` művelet hello hatókörben.</span><span class="sxs-lookup"><span data-stu-id="db51e-111">toolist role assignments, you must have access too`Microsoft.Authorization/roleAssignments/read` operation at hello scope.</span></span> <span data-ttu-id="db51e-112">Minden hello beépített szerepkörök kapnak hozzáférést toothis műveletet.</span><span class="sxs-lookup"><span data-stu-id="db51e-112">All hello built-in roles are granted access toothis operation.</span></span> <span data-ttu-id="db51e-113">További információ a szerepkör-hozzárendelések és az Azure-erőforrások kezelése hozzáférés: [átruházásához hozzáférés-vezérlés](role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="db51e-113">For more information about role assignments and managing access for Azure resources, see [Azure Role-Based Access Control](role-based-access-control-configure.md).</span></span>

### <a name="request"></a><span data-ttu-id="db51e-114">Kérés</span><span class="sxs-lookup"><span data-stu-id="db51e-114">Request</span></span>
<span data-ttu-id="db51e-115">Használjon hello **beolvasása** hello URI a következő metódust:</span><span class="sxs-lookup"><span data-stu-id="db51e-115">Use hello **GET** method with hello following URI:</span></span>

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments?api-version={api-version}&$filter={filter}

<span data-ttu-id="db51e-116">Belül hello URI ellenőrizze a következő helyettesítések toocustomize hello a kérést:</span><span class="sxs-lookup"><span data-stu-id="db51e-116">Within hello URI, make hello following substitutions toocustomize your request:</span></span>

1. <span data-ttu-id="db51e-117">Cserélje le *{hatókör}* hello hatókörrel, amelynek kívánja toolist hello szerepkör-hozzárendelések.</span><span class="sxs-lookup"><span data-stu-id="db51e-117">Replace *{scope}* with hello scope for which you wish toolist hello role assignments.</span></span> <span data-ttu-id="db51e-118">hello a következő példák bemutatják, hogyan toospecify hello különböző szintű hatóköre:</span><span class="sxs-lookup"><span data-stu-id="db51e-118">hello following examples show how toospecify hello scope for different levels:</span></span>

   * <span data-ttu-id="db51e-119">Előfizetés: /subscriptions/ {előfizetés-azonosító}</span><span class="sxs-lookup"><span data-stu-id="db51e-119">Subscription: /subscriptions/{subscription-id}</span></span>  
   * <span data-ttu-id="db51e-120">Erőforráscsoport: /subscriptions/ {előfizetés-azonosító} / resourceGroups/myresourcegroup1</span><span class="sxs-lookup"><span data-stu-id="db51e-120">Resource Group: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1</span></span>  
   * <span data-ttu-id="db51e-121">Erőforrás: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span><span class="sxs-lookup"><span data-stu-id="db51e-121">Resource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span></span>  
2. <span data-ttu-id="db51e-122">Cserélje le *{api-version}* a 2015-07-01.</span><span class="sxs-lookup"><span data-stu-id="db51e-122">Replace *{api-version}* with 2015-07-01.</span></span>
3. <span data-ttu-id="db51e-123">Cserélje le *{szűrő}* hello feltétellel, hogy kívánja-e tooapply toofilter hello szerepkör társításának listája:</span><span class="sxs-lookup"><span data-stu-id="db51e-123">Replace *{filter}* with hello condition that you wish tooapply toofilter hello role assignment list:</span></span>

   * <span data-ttu-id="db51e-124">Lista szerepkör-hozzárendelések csak hello megadva hatókör, nem a szerepkör-hozzárendelések hello subscopes, beleértve:`atScope()`</span><span class="sxs-lookup"><span data-stu-id="db51e-124">List role assignments for only hello specified scope, not including hello role assignments at subscopes: `atScope()`</span></span>    
   * <span data-ttu-id="db51e-125">Egy adott felhasználó, csoport vagy alkalmazás szerepkör-hozzárendelések listában:`principalId%20eq%20'{objectId of user, group, or service principal}'`</span><span class="sxs-lookup"><span data-stu-id="db51e-125">List role assignments for a specific user, group, or application: `principalId%20eq%20'{objectId of user, group, or service principal}'`</span></span>  
   * <span data-ttu-id="db51e-126">Egy adott felhasználó, például csoportokból örökölt szerepkör-hozzárendelések lista |}`assignedTo('{objectId of user}')`</span><span class="sxs-lookup"><span data-stu-id="db51e-126">List role assignments for a specific user, including ones inherited from groups | `assignedTo('{objectId of user}')`</span></span>

### <a name="response"></a><span data-ttu-id="db51e-127">Válasz</span><span class="sxs-lookup"><span data-stu-id="db51e-127">Response</span></span>
<span data-ttu-id="db51e-128">Állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="db51e-128">Status code: 200</span></span>

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

## <a name="get-information-about-a-role-assignment"></a><span data-ttu-id="db51e-129">Szerepkör-hozzárendelés adatainak beolvasása</span><span class="sxs-lookup"><span data-stu-id="db51e-129">Get information about a role assignment</span></span>
<span data-ttu-id="db51e-130">Egyetlen szerepkör-hozzárendelés hello szerepkör-hozzárendelés azonosítója által megadott információ lekérése.</span><span class="sxs-lookup"><span data-stu-id="db51e-130">Gets information about a single role assignment specified by hello role assignment identifier.</span></span>

<span data-ttu-id="db51e-131">szerepkör-hozzárendelés tooget információt, hozzá kell férnie túl`Microsoft.Authorization/roleAssignments/read` műveletet.</span><span class="sxs-lookup"><span data-stu-id="db51e-131">tooget information about a role assignment, you must have access too`Microsoft.Authorization/roleAssignments/read` operation.</span></span> <span data-ttu-id="db51e-132">Minden hello beépített szerepkörök kapnak hozzáférést toothis műveletet.</span><span class="sxs-lookup"><span data-stu-id="db51e-132">All hello built-in roles are granted access toothis operation.</span></span> <span data-ttu-id="db51e-133">További információ a szerepkör-hozzárendelések és az Azure-erőforrások kezelése hozzáférés: [átruházásához hozzáférés-vezérlés](role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="db51e-133">For more information about role assignments and managing access for Azure resources, see [Azure Role-Based Access Control](role-based-access-control-configure.md).</span></span>

### <a name="request"></a><span data-ttu-id="db51e-134">Kérés</span><span class="sxs-lookup"><span data-stu-id="db51e-134">Request</span></span>
<span data-ttu-id="db51e-135">Használjon hello **beolvasása** hello URI a következő metódust:</span><span class="sxs-lookup"><span data-stu-id="db51e-135">Use hello **GET** method with hello following URI:</span></span>

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

<span data-ttu-id="db51e-136">Belül hello URI ellenőrizze a következő helyettesítések toocustomize hello a kérést:</span><span class="sxs-lookup"><span data-stu-id="db51e-136">Within hello URI, make hello following substitutions toocustomize your request:</span></span>

1. <span data-ttu-id="db51e-137">Cserélje le *{hatókör}* hello hatókörrel, amelynek kívánja toolist hello szerepkör-hozzárendelések.</span><span class="sxs-lookup"><span data-stu-id="db51e-137">Replace *{scope}* with hello scope for which you wish toolist hello role assignments.</span></span> <span data-ttu-id="db51e-138">hello a következő példák bemutatják, hogyan toospecify hello különböző szintű hatóköre:</span><span class="sxs-lookup"><span data-stu-id="db51e-138">hello following examples show how toospecify hello scope for different levels:</span></span>

   * <span data-ttu-id="db51e-139">Előfizetés: /subscriptions/ {előfizetés-azonosító}</span><span class="sxs-lookup"><span data-stu-id="db51e-139">Subscription: /subscriptions/{subscription-id}</span></span>  
   * <span data-ttu-id="db51e-140">Erőforráscsoport: /subscriptions/ {előfizetés-azonosító} / resourceGroups/myresourcegroup1</span><span class="sxs-lookup"><span data-stu-id="db51e-140">Resource Group: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1</span></span>  
   * <span data-ttu-id="db51e-141">Erőforrás: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span><span class="sxs-lookup"><span data-stu-id="db51e-141">Resource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span></span>  
2. <span data-ttu-id="db51e-142">Cserélje le *{szerepkör-hozzárendelés-azonosító}* hello szerepkör-hozzárendelés hello GUID azonosítója.</span><span class="sxs-lookup"><span data-stu-id="db51e-142">Replace *{role-assignment-id}* with hello GUID identifier of hello role assignment.</span></span>
3. <span data-ttu-id="db51e-143">Cserélje le *{api-version}* a 2015-07-01.</span><span class="sxs-lookup"><span data-stu-id="db51e-143">Replace *{api-version}* with 2015-07-01.</span></span>

### <a name="response"></a><span data-ttu-id="db51e-144">Válasz</span><span class="sxs-lookup"><span data-stu-id="db51e-144">Response</span></span>
<span data-ttu-id="db51e-145">Állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="db51e-145">Status code: 200</span></span>

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

## <a name="create-a-role-assignment"></a><span data-ttu-id="db51e-146">Szerepkör-hozzárendelés létrehozása</span><span class="sxs-lookup"><span data-stu-id="db51e-146">Create a Role Assignment</span></span>
<span data-ttu-id="db51e-147">Szerepkör létrehozása hello megadott egyszerű támogatást nyújtó hello megadott szerepkörhöz megadott hello-hozzárendelés hatókörét.</span><span class="sxs-lookup"><span data-stu-id="db51e-147">Create a role assignment at hello specified scope for hello specified principal granting hello specified role.</span></span>

<span data-ttu-id="db51e-148">szerepkör-hozzárendelés toocreate, hozzá kell férnie túl`Microsoft.Authorization/roleAssignments/write` műveletet.</span><span class="sxs-lookup"><span data-stu-id="db51e-148">toocreate a role assignment, you must have access too`Microsoft.Authorization/roleAssignments/write` operation.</span></span> <span data-ttu-id="db51e-149">Hello beépített szerepkörök, csak *tulajdonos* és *felhasználói hozzáférés adminisztrátora* kapnak hozzáférést toothis műveletet.</span><span class="sxs-lookup"><span data-stu-id="db51e-149">Of hello built-in roles, only *Owner* and *User Access Administrator* are granted access toothis operation.</span></span> <span data-ttu-id="db51e-150">További információ a szerepkör-hozzárendelések és az Azure-erőforrások kezelése hozzáférés: [átruházásához hozzáférés-vezérlés](role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="db51e-150">For more information about role assignments and managing access for Azure resources, see [Azure Role-Based Access Control](role-based-access-control-configure.md).</span></span>

### <a name="request"></a><span data-ttu-id="db51e-151">Kérés</span><span class="sxs-lookup"><span data-stu-id="db51e-151">Request</span></span>
<span data-ttu-id="db51e-152">Használjon hello **PUT** hello URI a következő metódust:</span><span class="sxs-lookup"><span data-stu-id="db51e-152">Use hello **PUT** method with hello following URI:</span></span>

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

<span data-ttu-id="db51e-153">Belül hello URI ellenőrizze a következő helyettesítések toocustomize hello a kérést:</span><span class="sxs-lookup"><span data-stu-id="db51e-153">Within hello URI, make hello following substitutions toocustomize your request:</span></span>

1. <span data-ttu-id="db51e-154">Cserélje le *{hatókör}* hello hatókörrel, amellyel kívánja toocreate hello szerepkör-hozzárendelések.</span><span class="sxs-lookup"><span data-stu-id="db51e-154">Replace *{scope}* with hello scope at which you wish toocreate hello role assignments.</span></span> <span data-ttu-id="db51e-155">Ha a szerepkör-hozzárendelés létrehozása a szülő hatókörben, minden gyermekhatókörében öröklése hello azonos szerepkör-hozzárendelés.</span><span class="sxs-lookup"><span data-stu-id="db51e-155">When you create a role assignment at a parent scope, all child scopes inherit hello same role assignment.</span></span> <span data-ttu-id="db51e-156">hello a következő példák bemutatják, hogyan toospecify hello különböző szintű hatóköre:</span><span class="sxs-lookup"><span data-stu-id="db51e-156">hello following examples show how toospecify hello scope for different levels:</span></span>

   * <span data-ttu-id="db51e-157">Előfizetés: /subscriptions/ {előfizetés-azonosító}</span><span class="sxs-lookup"><span data-stu-id="db51e-157">Subscription: /subscriptions/{subscription-id}</span></span>  
   * <span data-ttu-id="db51e-158">Erőforráscsoport: /subscriptions/ {előfizetés-azonosító} / resourceGroups/myresourcegroup1</span><span class="sxs-lookup"><span data-stu-id="db51e-158">Resource Group: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1</span></span>   
   * <span data-ttu-id="db51e-159">Erőforrás: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span><span class="sxs-lookup"><span data-stu-id="db51e-159">Resource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span></span>  
2. <span data-ttu-id="db51e-160">Cserélje le *{szerepkör-hozzárendelés-azonosító}* egy új GUID, amely válik hello új szerepkör-hozzárendelés hello GUID azonosítója.</span><span class="sxs-lookup"><span data-stu-id="db51e-160">Replace *{role-assignment-id}* with a new GUID, which becomes hello GUID identifier of hello new role assignment.</span></span>
3. <span data-ttu-id="db51e-161">Cserélje le *{api-version}* a 2015-07-01.</span><span class="sxs-lookup"><span data-stu-id="db51e-161">Replace *{api-version}* with 2015-07-01.</span></span>

<span data-ttu-id="db51e-162">Hello kérelemtörzset adja meg a hello hello formátuma a következő értékeket:</span><span class="sxs-lookup"><span data-stu-id="db51e-162">For hello request body, provide hello values in hello following format:</span></span>

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
    "principalId": "5ac84765-1c8c-4994-94b2-629461bd191b"
  }
}

```

| <span data-ttu-id="db51e-163">Elem neve</span><span class="sxs-lookup"><span data-stu-id="db51e-163">Element Name</span></span> | <span data-ttu-id="db51e-164">Szükséges</span><span class="sxs-lookup"><span data-stu-id="db51e-164">Required</span></span> | <span data-ttu-id="db51e-165">Típus</span><span class="sxs-lookup"><span data-stu-id="db51e-165">Type</span></span> | <span data-ttu-id="db51e-166">Leírás</span><span class="sxs-lookup"><span data-stu-id="db51e-166">Description</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="db51e-167">roledefinitionid-értékkel</span><span class="sxs-lookup"><span data-stu-id="db51e-167">roleDefinitionId</span></span> |<span data-ttu-id="db51e-168">Igen</span><span class="sxs-lookup"><span data-stu-id="db51e-168">Yes</span></span> |<span data-ttu-id="db51e-169">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="db51e-169">String</span></span> |<span data-ttu-id="db51e-170">hello szerepkör hello azonosítója</span><span class="sxs-lookup"><span data-stu-id="db51e-170">hello identifier of hello role.</span></span> <span data-ttu-id="db51e-171">hello hello azonosító formátuma:`{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id-guid}`</span><span class="sxs-lookup"><span data-stu-id="db51e-171">hello format of hello identifier is: `{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id-guid}`</span></span> |
| <span data-ttu-id="db51e-172">principalId</span><span class="sxs-lookup"><span data-stu-id="db51e-172">principalId</span></span> |<span data-ttu-id="db51e-173">Igen</span><span class="sxs-lookup"><span data-stu-id="db51e-173">Yes</span></span> |<span data-ttu-id="db51e-174">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="db51e-174">String</span></span> |<span data-ttu-id="db51e-175">az Azure AD hello rendszerbiztonsági tag (felhasználó, csoport vagy egyszerű szolgáltatásnév) objectId toowhich hello szerepkör van hozzárendelve.</span><span class="sxs-lookup"><span data-stu-id="db51e-175">objectId of hello Azure AD principal (user, group, or service principal) toowhich hello role is assigned.</span></span> |

### <a name="response"></a><span data-ttu-id="db51e-176">Válasz</span><span class="sxs-lookup"><span data-stu-id="db51e-176">Response</span></span>
<span data-ttu-id="db51e-177">Állapotkód: 201-et</span><span class="sxs-lookup"><span data-stu-id="db51e-177">Status code: 201</span></span>

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

## <a name="delete-a-role-assignment"></a><span data-ttu-id="db51e-178">Szerepkör-hozzárendelés törlése</span><span class="sxs-lookup"><span data-stu-id="db51e-178">Delete a Role Assignment</span></span>
<span data-ttu-id="db51e-179">Hello szerepkör-hozzárendelés törlése a megadott hatókörben.</span><span class="sxs-lookup"><span data-stu-id="db51e-179">Delete a role assignment at hello specified scope.</span></span>

<span data-ttu-id="db51e-180">szerepkör-hozzárendelés toodelete, rendelkeznie kell hozzáféréssel toohello `Microsoft.Authorization/roleAssignments/delete` műveletet.</span><span class="sxs-lookup"><span data-stu-id="db51e-180">toodelete a role assignment, you must have access toohello `Microsoft.Authorization/roleAssignments/delete` operation.</span></span> <span data-ttu-id="db51e-181">Hello beépített szerepkörök, csak *tulajdonos* és *felhasználói hozzáférés adminisztrátora* kapnak hozzáférést toothis műveletet.</span><span class="sxs-lookup"><span data-stu-id="db51e-181">Of hello built-in roles, only *Owner* and *User Access Administrator* are granted access toothis operation.</span></span> <span data-ttu-id="db51e-182">További információ a szerepkör-hozzárendelések és az Azure-erőforrások kezelése hozzáférés: [átruházásához hozzáférés-vezérlés](role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="db51e-182">For more information about role assignments and managing access for Azure resources, see [Azure Role-Based Access Control](role-based-access-control-configure.md).</span></span>

### <a name="request"></a><span data-ttu-id="db51e-183">Kérés</span><span class="sxs-lookup"><span data-stu-id="db51e-183">Request</span></span>
<span data-ttu-id="db51e-184">Használjon hello **törlése** hello URI a következő metódust:</span><span class="sxs-lookup"><span data-stu-id="db51e-184">Use hello **DELETE** method with hello following URI:</span></span>

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

<span data-ttu-id="db51e-185">Belül hello URI ellenőrizze a következő helyettesítések toocustomize hello a kérést:</span><span class="sxs-lookup"><span data-stu-id="db51e-185">Within hello URI, make hello following substitutions toocustomize your request:</span></span>

1. <span data-ttu-id="db51e-186">Cserélje le *{hatókör}* hello hatókörrel, amellyel kívánja toocreate hello szerepkör-hozzárendelések.</span><span class="sxs-lookup"><span data-stu-id="db51e-186">Replace *{scope}* with hello scope at which you wish toocreate hello role assignments.</span></span> <span data-ttu-id="db51e-187">hello a következő példák bemutatják, hogyan toospecify hello különböző szintű hatóköre:</span><span class="sxs-lookup"><span data-stu-id="db51e-187">hello following examples show how toospecify hello scope for different levels:</span></span>

   * <span data-ttu-id="db51e-188">Előfizetés: /subscriptions/ {előfizetés-azonosító}</span><span class="sxs-lookup"><span data-stu-id="db51e-188">Subscription: /subscriptions/{subscription-id}</span></span>  
   * <span data-ttu-id="db51e-189">Erőforráscsoport: /subscriptions/ {előfizetés-azonosító} / resourceGroups/myresourcegroup1</span><span class="sxs-lookup"><span data-stu-id="db51e-189">Resource Group: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1</span></span>  
   * <span data-ttu-id="db51e-190">Erőforrás: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span><span class="sxs-lookup"><span data-stu-id="db51e-190">Resource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span></span>  
2. <span data-ttu-id="db51e-191">Cserélje le *{szerepkör-hozzárendelés-azonosító}* a hello GUID azonosító.</span><span class="sxs-lookup"><span data-stu-id="db51e-191">Replace *{role-assignment-id}* with hello role assignment id GUID.</span></span>
3. <span data-ttu-id="db51e-192">Cserélje le *{api-version}* a 2015-07-01.</span><span class="sxs-lookup"><span data-stu-id="db51e-192">Replace *{api-version}* with 2015-07-01.</span></span>

### <a name="response"></a><span data-ttu-id="db51e-193">Válasz</span><span class="sxs-lookup"><span data-stu-id="db51e-193">Response</span></span>
<span data-ttu-id="db51e-194">Állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="db51e-194">Status code: 200</span></span>

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

## <a name="list-all-roles"></a><span data-ttu-id="db51e-195">Az összes szerepkör felsorolása</span><span class="sxs-lookup"><span data-stu-id="db51e-195">List all Roles</span></span>
<span data-ttu-id="db51e-196">Felsorolja az összes hello szerepkör-hozzárendelés megadott hello elérhető hatókör.</span><span class="sxs-lookup"><span data-stu-id="db51e-196">Lists all hello roles that are available for assignment at hello specified scope.</span></span>

<span data-ttu-id="db51e-197">toolist szerepkörök, hozzá kell férnie túl`Microsoft.Authorization/roleDefinitions/read` művelet hello hatókörben.</span><span class="sxs-lookup"><span data-stu-id="db51e-197">toolist roles, you must have access too`Microsoft.Authorization/roleDefinitions/read` operation at hello scope.</span></span> <span data-ttu-id="db51e-198">Minden hello beépített szerepkörök kapnak hozzáférést toothis műveletet.</span><span class="sxs-lookup"><span data-stu-id="db51e-198">All hello built-in roles are granted access toothis operation.</span></span> <span data-ttu-id="db51e-199">További információ a szerepkör-hozzárendelések és az Azure-erőforrások kezelése hozzáférés: [átruházásához hozzáférés-vezérlés](role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="db51e-199">For more information about role assignments and managing access for Azure resources, see [Azure Role-Based Access Control](role-based-access-control-configure.md).</span></span>

### <a name="request"></a><span data-ttu-id="db51e-200">Kérés</span><span class="sxs-lookup"><span data-stu-id="db51e-200">Request</span></span>
<span data-ttu-id="db51e-201">Használjon hello **beolvasása** hello URI a következő metódust:</span><span class="sxs-lookup"><span data-stu-id="db51e-201">Use hello **GET** method with hello following URI:</span></span>

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions?api-version={api-version}&$filter={filter}

<span data-ttu-id="db51e-202">Belül hello URI ellenőrizze a következő helyettesítések toocustomize hello a kérést:</span><span class="sxs-lookup"><span data-stu-id="db51e-202">Within hello URI, make hello following substitutions toocustomize your request:</span></span>

1. <span data-ttu-id="db51e-203">Cserélje le *{hatókör}* , amelynek kívánja toolist hello szerepkörök hello hatókörrel.</span><span class="sxs-lookup"><span data-stu-id="db51e-203">Replace *{scope}* with hello scope for which you wish toolist hello roles.</span></span> <span data-ttu-id="db51e-204">hello a következő példák bemutatják, hogyan toospecify hello különböző szintű hatóköre:</span><span class="sxs-lookup"><span data-stu-id="db51e-204">hello following examples show how toospecify hello scope for different levels:</span></span>

   * <span data-ttu-id="db51e-205">Előfizetés: /subscriptions/ {előfizetés-azonosító}</span><span class="sxs-lookup"><span data-stu-id="db51e-205">Subscription: /subscriptions/{subscription-id}</span></span>  
   * <span data-ttu-id="db51e-206">Erőforráscsoport: /subscriptions/ {előfizetés-azonosító} / resourceGroups/myresourcegroup1</span><span class="sxs-lookup"><span data-stu-id="db51e-206">Resource Group: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1</span></span>  
   * <span data-ttu-id="db51e-207">Erőforrás /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span><span class="sxs-lookup"><span data-stu-id="db51e-207">Resource /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span></span>  
2. <span data-ttu-id="db51e-208">Cserélje le *{api-version}* a 2015-07-01.</span><span class="sxs-lookup"><span data-stu-id="db51e-208">Replace *{api-version}* with 2015-07-01.</span></span>
3. <span data-ttu-id="db51e-209">Cserélje le *{szűrő}* hello feltétellel, hogy kívánja-e tooapply toofilter hello szerepkörök listája:</span><span class="sxs-lookup"><span data-stu-id="db51e-209">Replace *{filter}* with hello condition that you wish tooapply toofilter hello list of roles:</span></span>

   * <span data-ttu-id="db51e-210">Lista szerepkörökhöz rendelhető hozzá hello megadott hatókörrel és a gyermekhatókör bármelyikét:`atScopeAndBelow()`</span><span class="sxs-lookup"><span data-stu-id="db51e-210">List roles available for assignment at hello specified scope and any of its child scopes: `atScopeAndBelow()`</span></span>
   * <span data-ttu-id="db51e-211">Keresse meg egy szerepkör használatával pontos megjelenített név: `roleName%20eq%20'{role-display-name}'`.</span><span class="sxs-lookup"><span data-stu-id="db51e-211">Search for a role using exact display name: `roleName%20eq%20'{role-display-name}'`.</span></span> <span data-ttu-id="db51e-212">Képernyőn hello URL-kódolású hello pontos megjelenített névbe hello szerepkör.</span><span class="sxs-lookup"><span data-stu-id="db51e-212">Use hello URL encoded form of hello exact display name of hello role.</span></span> <span data-ttu-id="db51e-213">Például`$filter=roleName%20eq%20'Virtual%20Machine%20Contributor'` |</span><span class="sxs-lookup"><span data-stu-id="db51e-213">For instance, `$filter=roleName%20eq%20'Virtual%20Machine%20Contributor'` |</span></span>

### <a name="response"></a><span data-ttu-id="db51e-214">Válasz</span><span class="sxs-lookup"><span data-stu-id="db51e-214">Response</span></span>
<span data-ttu-id="db51e-215">Állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="db51e-215">Status code: 200</span></span>

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

## <a name="get-information-about-a-role"></a><span data-ttu-id="db51e-216">Egy szerepkör adatainak beolvasása</span><span class="sxs-lookup"><span data-stu-id="db51e-216">Get information about a Role</span></span>
<span data-ttu-id="db51e-217">Egyetlen szerepkör hello szerepkör-definíció azonosítója által megadott információ lekérése.</span><span class="sxs-lookup"><span data-stu-id="db51e-217">Gets information about a single role specified by hello role definition identifier.</span></span> <span data-ttu-id="db51e-218">a megjelenített név megadásával egyetlen szerepkör tooget információkat lásd: [listában az összes szerepkör](role-based-access-control-manage-access-rest.md#list-all-roles).</span><span class="sxs-lookup"><span data-stu-id="db51e-218">tooget information about a single role using its display name, see [List all roles](role-based-access-control-manage-access-rest.md#list-all-roles).</span></span>

<span data-ttu-id="db51e-219">egy szerepkör tooget információt, hozzá kell férnie túl`Microsoft.Authorization/roleDefinitions/read` műveletet.</span><span class="sxs-lookup"><span data-stu-id="db51e-219">tooget information about a role, you must have access too`Microsoft.Authorization/roleDefinitions/read` operation.</span></span> <span data-ttu-id="db51e-220">Minden hello beépített szerepkörök kapnak hozzáférést toothis műveletet.</span><span class="sxs-lookup"><span data-stu-id="db51e-220">All hello built-in roles are granted access toothis operation.</span></span> <span data-ttu-id="db51e-221">További információ a szerepkör-hozzárendelések és az Azure-erőforrások kezelése hozzáférés: [átruházásához hozzáférés-vezérlés](role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="db51e-221">For more information about role assignments and managing access for Azure resources, see [Azure Role-Based Access Control](role-based-access-control-configure.md).</span></span>

### <a name="request"></a><span data-ttu-id="db51e-222">Kérés</span><span class="sxs-lookup"><span data-stu-id="db51e-222">Request</span></span>
<span data-ttu-id="db51e-223">Használjon hello **beolvasása** hello URI a következő metódust:</span><span class="sxs-lookup"><span data-stu-id="db51e-223">Use hello **GET** method with hello following URI:</span></span>

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

<span data-ttu-id="db51e-224">Belül hello URI ellenőrizze a következő helyettesítések toocustomize hello a kérést:</span><span class="sxs-lookup"><span data-stu-id="db51e-224">Within hello URI, make hello following substitutions toocustomize your request:</span></span>

1. <span data-ttu-id="db51e-225">Cserélje le *{hatókör}* hello hatókörrel, amelynek kívánja toolist hello szerepkör-hozzárendelések.</span><span class="sxs-lookup"><span data-stu-id="db51e-225">Replace *{scope}* with hello scope for which you wish toolist hello role assignments.</span></span> <span data-ttu-id="db51e-226">hello a következő példák bemutatják, hogyan toospecify hello különböző szintű hatóköre:</span><span class="sxs-lookup"><span data-stu-id="db51e-226">hello following examples show how toospecify hello scope for different levels:</span></span>

   * <span data-ttu-id="db51e-227">Előfizetés: /subscriptions/ {előfizetés-azonosító}</span><span class="sxs-lookup"><span data-stu-id="db51e-227">Subscription: /subscriptions/{subscription-id}</span></span>  
   * <span data-ttu-id="db51e-228">Erőforráscsoport: /subscriptions/ {előfizetés-azonosító} / resourceGroups/myresourcegroup1</span><span class="sxs-lookup"><span data-stu-id="db51e-228">Resource Group: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1</span></span>  
   * <span data-ttu-id="db51e-229">Erőforrás: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span><span class="sxs-lookup"><span data-stu-id="db51e-229">Resource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span></span>  
2. <span data-ttu-id="db51e-230">Cserélje le *{szerepkör-definíció-azonosító}* hello szerepkör-definíció hello GUID azonosítója.</span><span class="sxs-lookup"><span data-stu-id="db51e-230">Replace *{role-definition-id}* with hello GUID identifier of hello role definition.</span></span>
3. <span data-ttu-id="db51e-231">Cserélje le *{api-version}* a 2015-07-01.</span><span class="sxs-lookup"><span data-stu-id="db51e-231">Replace *{api-version}* with 2015-07-01.</span></span>

### <a name="response"></a><span data-ttu-id="db51e-232">Válasz</span><span class="sxs-lookup"><span data-stu-id="db51e-232">Response</span></span>
<span data-ttu-id="db51e-233">Állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="db51e-233">Status code: 200</span></span>

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

## <a name="create-a-custom-role"></a><span data-ttu-id="db51e-234">Egyéni szerepkör létrehozása</span><span class="sxs-lookup"><span data-stu-id="db51e-234">Create a Custom Role</span></span>
<span data-ttu-id="db51e-235">Hozzon létre egy egyéni biztonsági szerepkört.</span><span class="sxs-lookup"><span data-stu-id="db51e-235">Create a custom role.</span></span>

<span data-ttu-id="db51e-236">toocreate egy egyéni biztonsági szerepkört, hozzá kell férnie túl`Microsoft.Authorization/roleDefinitions/write` összes hello művelet `AssignableScopes`.</span><span class="sxs-lookup"><span data-stu-id="db51e-236">toocreate a custom role, you must have access too`Microsoft.Authorization/roleDefinitions/write` operation on all hello `AssignableScopes`.</span></span> <span data-ttu-id="db51e-237">Hello beépített szerepkörök, csak *tulajdonos* és *felhasználói hozzáférés adminisztrátora* kapnak hozzáférést toothis műveletet.</span><span class="sxs-lookup"><span data-stu-id="db51e-237">Of hello built-in roles, only *Owner* and *User Access Administrator* are granted access toothis operation.</span></span> <span data-ttu-id="db51e-238">További információ a szerepkör-hozzárendelések és az Azure-erőforrások kezelése hozzáférés: [átruházásához hozzáférés-vezérlés](role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="db51e-238">For more information about role assignments and managing access for Azure resources, see [Azure Role-Based Access Control](role-based-access-control-configure.md).</span></span>

### <a name="request"></a><span data-ttu-id="db51e-239">Kérés</span><span class="sxs-lookup"><span data-stu-id="db51e-239">Request</span></span>
<span data-ttu-id="db51e-240">Használjon hello **PUT** hello URI a következő metódust:</span><span class="sxs-lookup"><span data-stu-id="db51e-240">Use hello **PUT** method with hello following URI:</span></span>

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

<span data-ttu-id="db51e-241">Belül hello URI ellenőrizze a következő helyettesítések toocustomize hello a kérést:</span><span class="sxs-lookup"><span data-stu-id="db51e-241">Within hello URI, make hello following substitutions toocustomize your request:</span></span>

1. <span data-ttu-id="db51e-242">Cserélje le *{hatókör}* hello az első *AssignableScope* hello egyéni szerepkör.</span><span class="sxs-lookup"><span data-stu-id="db51e-242">Replace *{scope}* with hello first *AssignableScope* of hello custom role.</span></span> <span data-ttu-id="db51e-243">hello a következő példák bemutatják, hogyan toospecify hello különböző szintű hatóköre.</span><span class="sxs-lookup"><span data-stu-id="db51e-243">hello following examples show how toospecify hello scope for different levels.</span></span>

   * <span data-ttu-id="db51e-244">Előfizetés: /subscriptions/ {előfizetés-azonosító}</span><span class="sxs-lookup"><span data-stu-id="db51e-244">Subscription: /subscriptions/{subscription-id}</span></span>  
   * <span data-ttu-id="db51e-245">Erőforráscsoport: /subscriptions/ {előfizetés-azonosító} / resourceGroups/myresourcegroup1</span><span class="sxs-lookup"><span data-stu-id="db51e-245">Resource Group: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1</span></span>  
   * <span data-ttu-id="db51e-246">Erőforrás: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span><span class="sxs-lookup"><span data-stu-id="db51e-246">Resource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span></span>  
2. <span data-ttu-id="db51e-247">Cserélje le *{szerepkör-definíció-azonosító}* egy új GUID, amely válik hello új egyéni szerepkör hello GUID azonosítója.</span><span class="sxs-lookup"><span data-stu-id="db51e-247">Replace *{role-definition-id}* with a new GUID, which becomes hello GUID identifier of hello new custom role.</span></span>
3. <span data-ttu-id="db51e-248">Cserélje le *{api-version}* a 2015-07-01.</span><span class="sxs-lookup"><span data-stu-id="db51e-248">Replace *{api-version}* with 2015-07-01.</span></span>

<span data-ttu-id="db51e-249">Hello kérelemtörzset adja meg a hello hello formátuma a következő értékeket:</span><span class="sxs-lookup"><span data-stu-id="db51e-249">For hello request body, provide hello values in hello following format:</span></span>

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

| <span data-ttu-id="db51e-250">Elem neve</span><span class="sxs-lookup"><span data-stu-id="db51e-250">Element Name</span></span> | <span data-ttu-id="db51e-251">Szükséges</span><span class="sxs-lookup"><span data-stu-id="db51e-251">Required</span></span> | <span data-ttu-id="db51e-252">Típus</span><span class="sxs-lookup"><span data-stu-id="db51e-252">Type</span></span> | <span data-ttu-id="db51e-253">Leírás</span><span class="sxs-lookup"><span data-stu-id="db51e-253">Description</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="db51e-254">név</span><span class="sxs-lookup"><span data-stu-id="db51e-254">name</span></span> |<span data-ttu-id="db51e-255">Igen</span><span class="sxs-lookup"><span data-stu-id="db51e-255">Yes</span></span> |<span data-ttu-id="db51e-256">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="db51e-256">String</span></span> |<span data-ttu-id="db51e-257">Egyéni szerepkör hello GUID azonosítója</span><span class="sxs-lookup"><span data-stu-id="db51e-257">GUID identifier of hello custom role.</span></span> |
| <span data-ttu-id="db51e-258">properties.roleName</span><span class="sxs-lookup"><span data-stu-id="db51e-258">properties.roleName</span></span> |<span data-ttu-id="db51e-259">Igen</span><span class="sxs-lookup"><span data-stu-id="db51e-259">Yes</span></span> |<span data-ttu-id="db51e-260">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="db51e-260">String</span></span> |<span data-ttu-id="db51e-261">Hello egyéni szerepkör nevét jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="db51e-261">Display name of hello custom role.</span></span> <span data-ttu-id="db51e-262">Legfeljebb 128 karakter.</span><span class="sxs-lookup"><span data-stu-id="db51e-262">Maximum size 128 characters.</span></span> |
| <span data-ttu-id="db51e-263">Properties.Description</span><span class="sxs-lookup"><span data-stu-id="db51e-263">properties.description</span></span> |<span data-ttu-id="db51e-264">Nem</span><span class="sxs-lookup"><span data-stu-id="db51e-264">No</span></span> |<span data-ttu-id="db51e-265">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="db51e-265">String</span></span> |<span data-ttu-id="db51e-266">Hello egyéni szerepkör leírása</span><span class="sxs-lookup"><span data-stu-id="db51e-266">Description of hello custom role.</span></span> <span data-ttu-id="db51e-267">Maximális méret 1024 karakternél.</span><span class="sxs-lookup"><span data-stu-id="db51e-267">Maximum size 1024 characters.</span></span> |
| <span data-ttu-id="db51e-268">Properties.Type</span><span class="sxs-lookup"><span data-stu-id="db51e-268">properties.type</span></span> |<span data-ttu-id="db51e-269">Igen</span><span class="sxs-lookup"><span data-stu-id="db51e-269">Yes</span></span> |<span data-ttu-id="db51e-270">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="db51e-270">String</span></span> |<span data-ttu-id="db51e-271">Állítsa be a túl "CustomRole."</span><span class="sxs-lookup"><span data-stu-id="db51e-271">Set too"CustomRole."</span></span> |
| <span data-ttu-id="db51e-272">Properties.permissions.Actions</span><span class="sxs-lookup"><span data-stu-id="db51e-272">properties.permissions.actions</span></span> |<span data-ttu-id="db51e-273">Igen</span><span class="sxs-lookup"><span data-stu-id="db51e-273">Yes</span></span> |<span data-ttu-id="db51e-274">String]</span><span class="sxs-lookup"><span data-stu-id="db51e-274">String[]</span></span> |<span data-ttu-id="db51e-275">A művelet tömbjét karakterláncok hello egyéni szerepkör által biztosított megadását hello műveletek.</span><span class="sxs-lookup"><span data-stu-id="db51e-275">An array of action strings specifying hello operations granted by hello custom role.</span></span> |
| <span data-ttu-id="db51e-276">properties.permissions.notActions</span><span class="sxs-lookup"><span data-stu-id="db51e-276">properties.permissions.notActions</span></span> |<span data-ttu-id="db51e-277">Nem</span><span class="sxs-lookup"><span data-stu-id="db51e-277">No</span></span> |<span data-ttu-id="db51e-278">String]</span><span class="sxs-lookup"><span data-stu-id="db51e-278">String[]</span></span> |<span data-ttu-id="db51e-279">Hello műveletek tooexclude hello egyéni szerepkör által biztosított hello műveletekből művelet karakterláncokkal tömbjét.</span><span class="sxs-lookup"><span data-stu-id="db51e-279">An array of action strings specifying hello operations tooexclude from hello operations granted by hello custom role.</span></span> |
| <span data-ttu-id="db51e-280">properties.assignableScopes</span><span class="sxs-lookup"><span data-stu-id="db51e-280">properties.assignableScopes</span></span> |<span data-ttu-id="db51e-281">Igen</span><span class="sxs-lookup"><span data-stu-id="db51e-281">Yes</span></span> |<span data-ttu-id="db51e-282">String]</span><span class="sxs-lookup"><span data-stu-id="db51e-282">String[]</span></span> |<span data-ttu-id="db51e-283">A mely hello egyéni biztonsági szerepkört is használható hatókörök tömbjét.</span><span class="sxs-lookup"><span data-stu-id="db51e-283">An array of scopes in which hello custom role can be used.</span></span> |

### <a name="response"></a><span data-ttu-id="db51e-284">Válasz</span><span class="sxs-lookup"><span data-stu-id="db51e-284">Response</span></span>
<span data-ttu-id="db51e-285">Állapotkód: 201-et</span><span class="sxs-lookup"><span data-stu-id="db51e-285">Status code: 201</span></span>

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

## <a name="update-a-custom-role"></a><span data-ttu-id="db51e-286">Frissítés egy egyéni biztonsági szerepkört</span><span class="sxs-lookup"><span data-stu-id="db51e-286">Update a Custom Role</span></span>
<span data-ttu-id="db51e-287">Módosítani egy egyéni biztonsági szerepkört.</span><span class="sxs-lookup"><span data-stu-id="db51e-287">Modify a custom role.</span></span>

<span data-ttu-id="db51e-288">toomodify egy egyéni biztonsági szerepkört, hozzá kell férnie túl`Microsoft.Authorization/roleDefinitions/write` összes hello művelet `AssignableScopes`.</span><span class="sxs-lookup"><span data-stu-id="db51e-288">toomodify a custom role, you must have access too`Microsoft.Authorization/roleDefinitions/write` operation on all hello `AssignableScopes`.</span></span> <span data-ttu-id="db51e-289">Hello beépített szerepkörök, csak *tulajdonos* és *felhasználói hozzáférés adminisztrátora* kapnak hozzáférést toothis műveletet.</span><span class="sxs-lookup"><span data-stu-id="db51e-289">Of hello built-in roles, only *Owner* and *User Access Administrator* are granted access toothis operation.</span></span> <span data-ttu-id="db51e-290">További információ a szerepkör-hozzárendelések és az Azure-erőforrások kezelése hozzáférés: [átruházásához hozzáférés-vezérlés](role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="db51e-290">For more information about role assignments and managing access for Azure resources, see [Azure Role-Based Access Control](role-based-access-control-configure.md).</span></span>

### <a name="request"></a><span data-ttu-id="db51e-291">Kérés</span><span class="sxs-lookup"><span data-stu-id="db51e-291">Request</span></span>
<span data-ttu-id="db51e-292">Használjon hello **PUT** hello URI a következő metódust:</span><span class="sxs-lookup"><span data-stu-id="db51e-292">Use hello **PUT** method with hello following URI:</span></span>

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

<span data-ttu-id="db51e-293">Belül hello URI ellenőrizze a következő helyettesítések toocustomize hello a kérést:</span><span class="sxs-lookup"><span data-stu-id="db51e-293">Within hello URI, make hello following substitutions toocustomize your request:</span></span>

1. <span data-ttu-id="db51e-294">Cserélje le *{hatókör}* hello az első *AssignableScope* hello egyéni szerepkör.</span><span class="sxs-lookup"><span data-stu-id="db51e-294">Replace *{scope}* with hello first *AssignableScope* of hello custom role.</span></span> <span data-ttu-id="db51e-295">hello a következő példák bemutatják, hogyan toospecify hello különböző szintű hatóköre:</span><span class="sxs-lookup"><span data-stu-id="db51e-295">hello following examples show how toospecify hello scope for different levels:</span></span>

   * <span data-ttu-id="db51e-296">Előfizetés: /subscriptions/ {előfizetés-azonosító}</span><span class="sxs-lookup"><span data-stu-id="db51e-296">Subscription: /subscriptions/{subscription-id}</span></span>  
   * <span data-ttu-id="db51e-297">Erőforráscsoport: /subscriptions/ {előfizetés-azonosító} / resourceGroups/myresourcegroup1</span><span class="sxs-lookup"><span data-stu-id="db51e-297">Resource Group: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1</span></span>  
   * <span data-ttu-id="db51e-298">Erőforrás: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span><span class="sxs-lookup"><span data-stu-id="db51e-298">Resource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span></span>  
2. <span data-ttu-id="db51e-299">Cserélje le *{szerepkör-definíció-azonosító}* hello egyéni szerepkör hello GUID azonosítója.</span><span class="sxs-lookup"><span data-stu-id="db51e-299">Replace *{role-definition-id}* with hello GUID identifier of hello custom role.</span></span>
3. <span data-ttu-id="db51e-300">Cserélje le *{api-version}* a 2015-07-01.</span><span class="sxs-lookup"><span data-stu-id="db51e-300">Replace *{api-version}* with 2015-07-01.</span></span>

<span data-ttu-id="db51e-301">Hello kérelemtörzset adja meg a hello hello formátuma a következő értékeket:</span><span class="sxs-lookup"><span data-stu-id="db51e-301">For hello request body, provide hello values in hello following format:</span></span>

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

| <span data-ttu-id="db51e-302">Elem neve</span><span class="sxs-lookup"><span data-stu-id="db51e-302">Element Name</span></span> | <span data-ttu-id="db51e-303">Szükséges</span><span class="sxs-lookup"><span data-stu-id="db51e-303">Required</span></span> | <span data-ttu-id="db51e-304">Típus</span><span class="sxs-lookup"><span data-stu-id="db51e-304">Type</span></span> | <span data-ttu-id="db51e-305">Leírás</span><span class="sxs-lookup"><span data-stu-id="db51e-305">Description</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="db51e-306">név</span><span class="sxs-lookup"><span data-stu-id="db51e-306">name</span></span> |<span data-ttu-id="db51e-307">Igen</span><span class="sxs-lookup"><span data-stu-id="db51e-307">Yes</span></span> |<span data-ttu-id="db51e-308">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="db51e-308">String</span></span> |<span data-ttu-id="db51e-309">Egyéni szerepkör hello GUID azonosítója</span><span class="sxs-lookup"><span data-stu-id="db51e-309">GUID identifier of hello custom role.</span></span> |
| <span data-ttu-id="db51e-310">properties.roleName</span><span class="sxs-lookup"><span data-stu-id="db51e-310">properties.roleName</span></span> |<span data-ttu-id="db51e-311">Igen</span><span class="sxs-lookup"><span data-stu-id="db51e-311">Yes</span></span> |<span data-ttu-id="db51e-312">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="db51e-312">String</span></span> |<span data-ttu-id="db51e-313">Hello frissített egyéni szerepkör nevét jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="db51e-313">Display name of hello updated custom role.</span></span> |
| <span data-ttu-id="db51e-314">Properties.Description</span><span class="sxs-lookup"><span data-stu-id="db51e-314">properties.description</span></span> |<span data-ttu-id="db51e-315">Nem</span><span class="sxs-lookup"><span data-stu-id="db51e-315">No</span></span> |<span data-ttu-id="db51e-316">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="db51e-316">String</span></span> |<span data-ttu-id="db51e-317">Hello leírása frissítve egyéni biztonsági szerepkört.</span><span class="sxs-lookup"><span data-stu-id="db51e-317">Description of hello updated custom role.</span></span> |
| <span data-ttu-id="db51e-318">Properties.Type</span><span class="sxs-lookup"><span data-stu-id="db51e-318">properties.type</span></span> |<span data-ttu-id="db51e-319">Igen</span><span class="sxs-lookup"><span data-stu-id="db51e-319">Yes</span></span> |<span data-ttu-id="db51e-320">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="db51e-320">String</span></span> |<span data-ttu-id="db51e-321">Állítsa be a túl "CustomRole."</span><span class="sxs-lookup"><span data-stu-id="db51e-321">Set too"CustomRole."</span></span> |
| <span data-ttu-id="db51e-322">Properties.permissions.Actions</span><span class="sxs-lookup"><span data-stu-id="db51e-322">properties.permissions.actions</span></span> |<span data-ttu-id="db51e-323">Igen</span><span class="sxs-lookup"><span data-stu-id="db51e-323">Yes</span></span> |<span data-ttu-id="db51e-324">String]</span><span class="sxs-lookup"><span data-stu-id="db51e-324">String[]</span></span> |<span data-ttu-id="db51e-325">Hello műveletek toowhich hello megadása művelet karakterláncok frissítése egyéni szerepkör engedélyezi a hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="db51e-325">An array of action strings specifying hello operations toowhich hello updated custom role grants access.</span></span> |
| <span data-ttu-id="db51e-326">properties.permissions.notActions</span><span class="sxs-lookup"><span data-stu-id="db51e-326">properties.permissions.notActions</span></span> |<span data-ttu-id="db51e-327">Nem</span><span class="sxs-lookup"><span data-stu-id="db51e-327">No</span></span> |<span data-ttu-id="db51e-328">String]</span><span class="sxs-lookup"><span data-stu-id="db51e-328">String[]</span></span> |<span data-ttu-id="db51e-329">Művelet tömbje karakterláncok megadását hello műveletek tooexclude hello műveletek, melyik hello frissítése egyéni szerepkör biztosít.</span><span class="sxs-lookup"><span data-stu-id="db51e-329">An array of action strings specifying hello operations tooexclude from hello operations which hello updated custom role grants.</span></span> |
| <span data-ttu-id="db51e-330">properties.assignableScopes</span><span class="sxs-lookup"><span data-stu-id="db51e-330">properties.assignableScopes</span></span> |<span data-ttu-id="db51e-331">Igen</span><span class="sxs-lookup"><span data-stu-id="db51e-331">Yes</span></span> |<span data-ttu-id="db51e-332">String]</span><span class="sxs-lookup"><span data-stu-id="db51e-332">String[]</span></span> |<span data-ttu-id="db51e-333">A tömb hatókörök mely hello a frissített egyéni biztonsági szerepkört is használható.</span><span class="sxs-lookup"><span data-stu-id="db51e-333">An array of scopes in which hello updated custom role can be used.</span></span> |

### <a name="response"></a><span data-ttu-id="db51e-334">Válasz</span><span class="sxs-lookup"><span data-stu-id="db51e-334">Response</span></span>
<span data-ttu-id="db51e-335">Állapotkód: 201-et</span><span class="sxs-lookup"><span data-stu-id="db51e-335">Status code: 201</span></span>

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

## <a name="delete-a-custom-role"></a><span data-ttu-id="db51e-336">Egyéni szerepkör törléséhez</span><span class="sxs-lookup"><span data-stu-id="db51e-336">Delete a Custom Role</span></span>
<span data-ttu-id="db51e-337">Egyéni szerepkör törléséhez.</span><span class="sxs-lookup"><span data-stu-id="db51e-337">Delete a custom role.</span></span>

<span data-ttu-id="db51e-338">toodelete egy egyéni biztonsági szerepkört, hozzá kell férnie túl`Microsoft.Authorization/roleDefinitions/delete` összes hello művelet `AssignableScopes`.</span><span class="sxs-lookup"><span data-stu-id="db51e-338">toodelete a custom role, you must have access too`Microsoft.Authorization/roleDefinitions/delete` operation on all hello `AssignableScopes`.</span></span> <span data-ttu-id="db51e-339">Hello beépített szerepkörök, csak *tulajdonos* és *felhasználói hozzáférés adminisztrátora* kapnak hozzáférést toothis műveletet.</span><span class="sxs-lookup"><span data-stu-id="db51e-339">Of hello built-in roles, only *Owner* and *User Access Administrator* are granted access toothis operation.</span></span> <span data-ttu-id="db51e-340">További információ a szerepkör-hozzárendelések és az Azure-erőforrások kezelése hozzáférés: [átruházásához hozzáférés-vezérlés](role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="db51e-340">For more information about role assignments and managing access for Azure resources, see [Azure Role-Based Access Control](role-based-access-control-configure.md).</span></span>

### <a name="request"></a><span data-ttu-id="db51e-341">Kérés</span><span class="sxs-lookup"><span data-stu-id="db51e-341">Request</span></span>
<span data-ttu-id="db51e-342">Használjon hello **törlése** hello URI a következő metódust:</span><span class="sxs-lookup"><span data-stu-id="db51e-342">Use hello **DELETE** method with hello following URI:</span></span>

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

<span data-ttu-id="db51e-343">Belül hello URI ellenőrizze a következő helyettesítések toocustomize hello a kérést:</span><span class="sxs-lookup"><span data-stu-id="db51e-343">Within hello URI, make hello following substitutions toocustomize your request:</span></span>

1. <span data-ttu-id="db51e-344">Cserélje le *{hatókör}* hello hatókörrel, amellyel toodelete hello szerepkör-definíció kívánja.</span><span class="sxs-lookup"><span data-stu-id="db51e-344">Replace *{scope}* with hello scope at which you wish toodelete hello role definition.</span></span> <span data-ttu-id="db51e-345">hello a következő példák bemutatják, hogyan toospecify hello különböző szintű hatóköre:</span><span class="sxs-lookup"><span data-stu-id="db51e-345">hello following examples show how toospecify hello scope for different levels:</span></span>

   * <span data-ttu-id="db51e-346">Előfizetés: /subscriptions/ {előfizetés-azonosító}</span><span class="sxs-lookup"><span data-stu-id="db51e-346">Subscription: /subscriptions/{subscription-id}</span></span>  
   * <span data-ttu-id="db51e-347">Erőforráscsoport: /subscriptions/ {előfizetés-azonosító} / resourceGroups/myresourcegroup1</span><span class="sxs-lookup"><span data-stu-id="db51e-347">Resource Group: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1</span></span>  
   * <span data-ttu-id="db51e-348">Erőforrás: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span><span class="sxs-lookup"><span data-stu-id="db51e-348">Resource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span></span>  
2. <span data-ttu-id="db51e-349">Cserélje le *{szerepkör-definíció-azonosító}* a hello GUID szerepkör-definíció azonosítója hello egyéni szerepkör.</span><span class="sxs-lookup"><span data-stu-id="db51e-349">Replace *{role-definition-id}* with hello GUID role definition id of hello custom role.</span></span>
3. <span data-ttu-id="db51e-350">Cserélje le *{api-version}* a 2015-07-01.</span><span class="sxs-lookup"><span data-stu-id="db51e-350">Replace *{api-version}* with 2015-07-01.</span></span>

### <a name="response"></a><span data-ttu-id="db51e-351">Válasz</span><span class="sxs-lookup"><span data-stu-id="db51e-351">Response</span></span>
<span data-ttu-id="db51e-352">Állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="db51e-352">Status code: 200</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="db51e-353">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="db51e-353">Next steps</span></span>

[!INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]
