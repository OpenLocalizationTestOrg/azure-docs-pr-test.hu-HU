---
title: "Szerepköralapú hozzáférés-vezérlés REST - az Azure AD |} Microsoft Docs"
description: "A REST API szerepköralapú hozzáférés-vezérlés kezelése"
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
ms.openlocfilehash: a5c19fd87ce1ae3e199bf1dfc8cf82f5653baac2
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="manage-role-based-access-control-with-the-rest-api"></a><span data-ttu-id="aa388-103">A REST API szerepköralapú hozzáférés-vezérlés kezelése</span><span class="sxs-lookup"><span data-stu-id="aa388-103">Manage Role-Based Access Control with the REST API</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="aa388-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="aa388-104">PowerShell</span></span>](role-based-access-control-manage-access-powershell.md)
> * [<span data-ttu-id="aa388-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="aa388-105">Azure CLI</span></span>](role-based-access-control-manage-access-azure-cli.md)
> * [<span data-ttu-id="aa388-106">REST API</span><span class="sxs-lookup"><span data-stu-id="aa388-106">REST API</span></span>](role-based-access-control-manage-access-rest.md)

<span data-ttu-id="aa388-107">Szerepköralapú hozzáférés-vezérlés (RBAC) az Azure portálon, és az Azure Resource Manager API segítségével kezelhetők az előfizetés és a minden részletre kiterjedő szinten erőforrásokhoz való hozzáférését.</span><span class="sxs-lookup"><span data-stu-id="aa388-107">Role-Based Access Control (RBAC) in the Azure portal and Azure Resource Manager API helps you manage access to your subscription and resources at a fine-grained level.</span></span> <span data-ttu-id="aa388-108">Ez a szolgáltatás egyes szerepkörök hozzárendelése el egy adott hatókörhöz szerint engedélyezheti a hozzáférést az Active Directory felhasználók, csoportok vagy szolgáltatásnevekről.</span><span class="sxs-lookup"><span data-stu-id="aa388-108">With this feature, you can grant access for Active Directory users, groups, or service principals by assigning some roles to them at a particular scope.</span></span>

## <a name="list-all-role-assignments"></a><span data-ttu-id="aa388-109">Az összes szerepkör-hozzárendelések felsorolása</span><span class="sxs-lookup"><span data-stu-id="aa388-109">List all role assignments</span></span>
<span data-ttu-id="aa388-110">Megjeleníti a megadott hatókör és subscopes szerepkör-hozzárendelések.</span><span class="sxs-lookup"><span data-stu-id="aa388-110">Lists all the role assignments at the specified scope and subscopes.</span></span>

<span data-ttu-id="aa388-111">A szerepkör-hozzárendelések listáját hozzáféréssel kell rendelkeznie `Microsoft.Authorization/roleAssignments/read` műveletet a hatókörben.</span><span class="sxs-lookup"><span data-stu-id="aa388-111">To list role assignments, you must have access to `Microsoft.Authorization/roleAssignments/read` operation at the scope.</span></span> <span data-ttu-id="aa388-112">A beépített szerepkörök művelet számára hozzáférést kapnak.</span><span class="sxs-lookup"><span data-stu-id="aa388-112">All the built-in roles are granted access to this operation.</span></span> <span data-ttu-id="aa388-113">További információ a szerepkör-hozzárendelések és az Azure-erőforrások kezelése hozzáférés: [átruházásához hozzáférés-vezérlés](role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="aa388-113">For more information about role assignments and managing access for Azure resources, see [Azure Role-Based Access Control](role-based-access-control-configure.md).</span></span>

### <a name="request"></a><span data-ttu-id="aa388-114">Kérés</span><span class="sxs-lookup"><span data-stu-id="aa388-114">Request</span></span>
<span data-ttu-id="aa388-115">Használja a **beolvasása** metódus a következő URI-azonosítóhoz:</span><span class="sxs-lookup"><span data-stu-id="aa388-115">Use the **GET** method with the following URI:</span></span>

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments?api-version={api-version}&$filter={filter}

<span data-ttu-id="aa388-116">Az URI belül győződjön testre szabhatja a kérelem a következő helyettesítések hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="aa388-116">Within the URI, make the following substitutions to customize your request:</span></span>

1. <span data-ttu-id="aa388-117">Cserélje le *{hatókör}* a hatókörben, amelynek kívánja a szerepkör-hozzárendelések listáját.</span><span class="sxs-lookup"><span data-stu-id="aa388-117">Replace *{scope}* with the scope for which you wish to list the role assignments.</span></span> <span data-ttu-id="aa388-118">A következő példák bemutatják, hogyan adhatja meg a különböző szintű hatóköre:</span><span class="sxs-lookup"><span data-stu-id="aa388-118">The following examples show how to specify the scope for different levels:</span></span>

   * <span data-ttu-id="aa388-119">Előfizetés: /subscriptions/ {előfizetés-azonosító}</span><span class="sxs-lookup"><span data-stu-id="aa388-119">Subscription: /subscriptions/{subscription-id}</span></span>  
   * <span data-ttu-id="aa388-120">Erőforráscsoport: /subscriptions/ {előfizetés-azonosító} / resourceGroups/myresourcegroup1</span><span class="sxs-lookup"><span data-stu-id="aa388-120">Resource Group: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1</span></span>  
   * <span data-ttu-id="aa388-121">Erőforrás: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span><span class="sxs-lookup"><span data-stu-id="aa388-121">Resource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span></span>  
2. <span data-ttu-id="aa388-122">Cserélje le *{api-version}* a 2015-07-01.</span><span class="sxs-lookup"><span data-stu-id="aa388-122">Replace *{api-version}* with 2015-07-01.</span></span>
3. <span data-ttu-id="aa388-123">Cserélje le *{szűrő}* azzal a feltétellel, amelyet meg kíván alkalmazni a szerepkör-hozzárendelés csoportlista szűréséhez:</span><span class="sxs-lookup"><span data-stu-id="aa388-123">Replace *{filter}* with the condition that you wish to apply to filter the role assignment list:</span></span>

   * <span data-ttu-id="aa388-124">Szerepkör-hozzárendelések csak a megadott hatókör, a nem az a szerepkör-hozzárendelések subscopes, beleértve a listában:`atScope()`</span><span class="sxs-lookup"><span data-stu-id="aa388-124">List role assignments for only the specified scope, not including the role assignments at subscopes: `atScope()`</span></span>    
   * <span data-ttu-id="aa388-125">Egy adott felhasználó, csoport vagy alkalmazás szerepkör-hozzárendelések listában:`principalId%20eq%20'{objectId of user, group, or service principal}'`</span><span class="sxs-lookup"><span data-stu-id="aa388-125">List role assignments for a specific user, group, or application: `principalId%20eq%20'{objectId of user, group, or service principal}'`</span></span>  
   * <span data-ttu-id="aa388-126">Egy adott felhasználó, például csoportokból örökölt szerepkör-hozzárendelések lista |}`assignedTo('{objectId of user}')`</span><span class="sxs-lookup"><span data-stu-id="aa388-126">List role assignments for a specific user, including ones inherited from groups | `assignedTo('{objectId of user}')`</span></span>

### <a name="response"></a><span data-ttu-id="aa388-127">Válasz</span><span class="sxs-lookup"><span data-stu-id="aa388-127">Response</span></span>
<span data-ttu-id="aa388-128">Állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="aa388-128">Status code: 200</span></span>

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

## <a name="get-information-about-a-role-assignment"></a><span data-ttu-id="aa388-129">Szerepkör-hozzárendelés adatainak beolvasása</span><span class="sxs-lookup"><span data-stu-id="aa388-129">Get information about a role assignment</span></span>
<span data-ttu-id="aa388-130">A szerepkör-hozzárendelés azonosítója által megadott egyetlen szerepkör-hozzárendelés információ lekérése.</span><span class="sxs-lookup"><span data-stu-id="aa388-130">Gets information about a single role assignment specified by the role assignment identifier.</span></span>

<span data-ttu-id="aa388-131">Szerepkör-hozzárendelés adatainak beolvasása, hozzáféréssel kell rendelkeznie `Microsoft.Authorization/roleAssignments/read` műveletet.</span><span class="sxs-lookup"><span data-stu-id="aa388-131">To get information about a role assignment, you must have access to `Microsoft.Authorization/roleAssignments/read` operation.</span></span> <span data-ttu-id="aa388-132">A beépített szerepkörök művelet számára hozzáférést kapnak.</span><span class="sxs-lookup"><span data-stu-id="aa388-132">All the built-in roles are granted access to this operation.</span></span> <span data-ttu-id="aa388-133">További információ a szerepkör-hozzárendelések és az Azure-erőforrások kezelése hozzáférés: [átruházásához hozzáférés-vezérlés](role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="aa388-133">For more information about role assignments and managing access for Azure resources, see [Azure Role-Based Access Control](role-based-access-control-configure.md).</span></span>

### <a name="request"></a><span data-ttu-id="aa388-134">Kérés</span><span class="sxs-lookup"><span data-stu-id="aa388-134">Request</span></span>
<span data-ttu-id="aa388-135">Használja a **beolvasása** metódus a következő URI-azonosítóhoz:</span><span class="sxs-lookup"><span data-stu-id="aa388-135">Use the **GET** method with the following URI:</span></span>

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

<span data-ttu-id="aa388-136">Az URI belül győződjön testre szabhatja a kérelem a következő helyettesítések hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="aa388-136">Within the URI, make the following substitutions to customize your request:</span></span>

1. <span data-ttu-id="aa388-137">Cserélje le *{hatókör}* a hatókörben, amelynek kívánja a szerepkör-hozzárendelések listáját.</span><span class="sxs-lookup"><span data-stu-id="aa388-137">Replace *{scope}* with the scope for which you wish to list the role assignments.</span></span> <span data-ttu-id="aa388-138">A következő példák bemutatják, hogyan adhatja meg a különböző szintű hatóköre:</span><span class="sxs-lookup"><span data-stu-id="aa388-138">The following examples show how to specify the scope for different levels:</span></span>

   * <span data-ttu-id="aa388-139">Előfizetés: /subscriptions/ {előfizetés-azonosító}</span><span class="sxs-lookup"><span data-stu-id="aa388-139">Subscription: /subscriptions/{subscription-id}</span></span>  
   * <span data-ttu-id="aa388-140">Erőforráscsoport: /subscriptions/ {előfizetés-azonosító} / resourceGroups/myresourcegroup1</span><span class="sxs-lookup"><span data-stu-id="aa388-140">Resource Group: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1</span></span>  
   * <span data-ttu-id="aa388-141">Erőforrás: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span><span class="sxs-lookup"><span data-stu-id="aa388-141">Resource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span></span>  
2. <span data-ttu-id="aa388-142">Cserélje le *{szerepkör-hozzárendelés-azonosító}* azon szerepkör-hozzárendelés GUID azonosítóval.</span><span class="sxs-lookup"><span data-stu-id="aa388-142">Replace *{role-assignment-id}* with the GUID identifier of the role assignment.</span></span>
3. <span data-ttu-id="aa388-143">Cserélje le *{api-version}* a 2015-07-01.</span><span class="sxs-lookup"><span data-stu-id="aa388-143">Replace *{api-version}* with 2015-07-01.</span></span>

### <a name="response"></a><span data-ttu-id="aa388-144">Válasz</span><span class="sxs-lookup"><span data-stu-id="aa388-144">Response</span></span>
<span data-ttu-id="aa388-145">Állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="aa388-145">Status code: 200</span></span>

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

## <a name="create-a-role-assignment"></a><span data-ttu-id="aa388-146">Szerepkör-hozzárendelés létrehozása</span><span class="sxs-lookup"><span data-stu-id="aa388-146">Create a Role Assignment</span></span>
<span data-ttu-id="aa388-147">Szerepkör-hozzárendelés létrehozása a megadott rendszerbiztonsági tag a megadott szerepkör megadása a megadott hatókörben.</span><span class="sxs-lookup"><span data-stu-id="aa388-147">Create a role assignment at the specified scope for the specified principal granting the specified role.</span></span>

<span data-ttu-id="aa388-148">Szerepkör-hozzárendelés létrehozása, hozzáférést kell biztosítani `Microsoft.Authorization/roleAssignments/write` műveletet.</span><span class="sxs-lookup"><span data-stu-id="aa388-148">To create a role assignment, you must have access to `Microsoft.Authorization/roleAssignments/write` operation.</span></span> <span data-ttu-id="aa388-149">A beépített szerepkörök, csak *tulajdonos* és *felhasználói hozzáférés adminisztrátora* férhetnek hozzá ezt a műveletet.</span><span class="sxs-lookup"><span data-stu-id="aa388-149">Of the built-in roles, only *Owner* and *User Access Administrator* are granted access to this operation.</span></span> <span data-ttu-id="aa388-150">További információ a szerepkör-hozzárendelések és az Azure-erőforrások kezelése hozzáférés: [átruházásához hozzáférés-vezérlés](role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="aa388-150">For more information about role assignments and managing access for Azure resources, see [Azure Role-Based Access Control](role-based-access-control-configure.md).</span></span>

### <a name="request"></a><span data-ttu-id="aa388-151">Kérés</span><span class="sxs-lookup"><span data-stu-id="aa388-151">Request</span></span>
<span data-ttu-id="aa388-152">Használja a **PUT** metódus a következő URI-azonosítóhoz:</span><span class="sxs-lookup"><span data-stu-id="aa388-152">Use the **PUT** method with the following URI:</span></span>

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

<span data-ttu-id="aa388-153">Az URI belül győződjön testre szabhatja a kérelem a következő helyettesítések hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="aa388-153">Within the URI, make the following substitutions to customize your request:</span></span>

1. <span data-ttu-id="aa388-154">Cserélje le *{hatókör}* a hatókörben, ahol létre szeretne hozni a szerepkör-hozzárendeléseket.</span><span class="sxs-lookup"><span data-stu-id="aa388-154">Replace *{scope}* with the scope at which you wish to create the role assignments.</span></span> <span data-ttu-id="aa388-155">Szerepkör-hozzárendelés létrehozása a szülő hatókörben, minden gyermekhatókörében örökli a azonos szerepkör-hozzárendelés.</span><span class="sxs-lookup"><span data-stu-id="aa388-155">When you create a role assignment at a parent scope, all child scopes inherit the same role assignment.</span></span> <span data-ttu-id="aa388-156">A következő példák bemutatják, hogyan adhatja meg a különböző szintű hatóköre:</span><span class="sxs-lookup"><span data-stu-id="aa388-156">The following examples show how to specify the scope for different levels:</span></span>

   * <span data-ttu-id="aa388-157">Előfizetés: /subscriptions/ {előfizetés-azonosító}</span><span class="sxs-lookup"><span data-stu-id="aa388-157">Subscription: /subscriptions/{subscription-id}</span></span>  
   * <span data-ttu-id="aa388-158">Erőforráscsoport: /subscriptions/ {előfizetés-azonosító} / resourceGroups/myresourcegroup1</span><span class="sxs-lookup"><span data-stu-id="aa388-158">Resource Group: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1</span></span>   
   * <span data-ttu-id="aa388-159">Erőforrás: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span><span class="sxs-lookup"><span data-stu-id="aa388-159">Resource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span></span>  
2. <span data-ttu-id="aa388-160">Cserélje le *{szerepkör-hozzárendelés-azonosító}* egy új GUID, amely válik az új szerepkör-hozzárendelés GUID azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="aa388-160">Replace *{role-assignment-id}* with a new GUID, which becomes the GUID identifier of the new role assignment.</span></span>
3. <span data-ttu-id="aa388-161">Cserélje le *{api-version}* a 2015-07-01.</span><span class="sxs-lookup"><span data-stu-id="aa388-161">Replace *{api-version}* with 2015-07-01.</span></span>

<span data-ttu-id="aa388-162">A kérelem törzsében adja meg az értékeket a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="aa388-162">For the request body, provide the values in the following format:</span></span>

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
    "principalId": "5ac84765-1c8c-4994-94b2-629461bd191b"
  }
}

```

| <span data-ttu-id="aa388-163">Elem neve</span><span class="sxs-lookup"><span data-stu-id="aa388-163">Element Name</span></span> | <span data-ttu-id="aa388-164">Szükséges</span><span class="sxs-lookup"><span data-stu-id="aa388-164">Required</span></span> | <span data-ttu-id="aa388-165">Típus</span><span class="sxs-lookup"><span data-stu-id="aa388-165">Type</span></span> | <span data-ttu-id="aa388-166">Leírás</span><span class="sxs-lookup"><span data-stu-id="aa388-166">Description</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="aa388-167">roledefinitionid-értékkel</span><span class="sxs-lookup"><span data-stu-id="aa388-167">roleDefinitionId</span></span> |<span data-ttu-id="aa388-168">Igen</span><span class="sxs-lookup"><span data-stu-id="aa388-168">Yes</span></span> |<span data-ttu-id="aa388-169">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="aa388-169">String</span></span> |<span data-ttu-id="aa388-170">A szerepkör azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="aa388-170">The identifier of the role.</span></span> <span data-ttu-id="aa388-171">Az azonosító formátuma:`{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id-guid}`</span><span class="sxs-lookup"><span data-stu-id="aa388-171">The format of the identifier is: `{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id-guid}`</span></span> |
| <span data-ttu-id="aa388-172">principalId</span><span class="sxs-lookup"><span data-stu-id="aa388-172">principalId</span></span> |<span data-ttu-id="aa388-173">Igen</span><span class="sxs-lookup"><span data-stu-id="aa388-173">Yes</span></span> |<span data-ttu-id="aa388-174">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="aa388-174">String</span></span> |<span data-ttu-id="aa388-175">objectId az Azure AD rendszerbiztonsági tag (felhasználó, csoport vagy egyszerű szolgáltatásneve), amely a szerepkör hozzá van rendelve.</span><span class="sxs-lookup"><span data-stu-id="aa388-175">objectId of the Azure AD principal (user, group, or service principal) to which the role is assigned.</span></span> |

### <a name="response"></a><span data-ttu-id="aa388-176">Válasz</span><span class="sxs-lookup"><span data-stu-id="aa388-176">Response</span></span>
<span data-ttu-id="aa388-177">Állapotkód: 201-et</span><span class="sxs-lookup"><span data-stu-id="aa388-177">Status code: 201</span></span>

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

## <a name="delete-a-role-assignment"></a><span data-ttu-id="aa388-178">Szerepkör-hozzárendelés törlése</span><span class="sxs-lookup"><span data-stu-id="aa388-178">Delete a Role Assignment</span></span>
<span data-ttu-id="aa388-179">Szerepkör-hozzárendelés törlése a megadott hatókörből.</span><span class="sxs-lookup"><span data-stu-id="aa388-179">Delete a role assignment at the specified scope.</span></span>

<span data-ttu-id="aa388-180">Szerepkör-hozzárendelés törlése, hozzáféréssel kell rendelkeznie a `Microsoft.Authorization/roleAssignments/delete` műveletet.</span><span class="sxs-lookup"><span data-stu-id="aa388-180">To delete a role assignment, you must have access to the `Microsoft.Authorization/roleAssignments/delete` operation.</span></span> <span data-ttu-id="aa388-181">A beépített szerepkörök, csak *tulajdonos* és *felhasználói hozzáférés adminisztrátora* férhetnek hozzá ezt a műveletet.</span><span class="sxs-lookup"><span data-stu-id="aa388-181">Of the built-in roles, only *Owner* and *User Access Administrator* are granted access to this operation.</span></span> <span data-ttu-id="aa388-182">További információ a szerepkör-hozzárendelések és az Azure-erőforrások kezelése hozzáférés: [átruházásához hozzáférés-vezérlés](role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="aa388-182">For more information about role assignments and managing access for Azure resources, see [Azure Role-Based Access Control](role-based-access-control-configure.md).</span></span>

### <a name="request"></a><span data-ttu-id="aa388-183">Kérés</span><span class="sxs-lookup"><span data-stu-id="aa388-183">Request</span></span>
<span data-ttu-id="aa388-184">Használja a **törlése** metódus a következő URI-azonosítóhoz:</span><span class="sxs-lookup"><span data-stu-id="aa388-184">Use the **DELETE** method with the following URI:</span></span>

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

<span data-ttu-id="aa388-185">Az URI belül győződjön testre szabhatja a kérelem a következő helyettesítések hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="aa388-185">Within the URI, make the following substitutions to customize your request:</span></span>

1. <span data-ttu-id="aa388-186">Cserélje le *{hatókör}* a hatókörben, ahol létre szeretne hozni a szerepkör-hozzárendeléseket.</span><span class="sxs-lookup"><span data-stu-id="aa388-186">Replace *{scope}* with the scope at which you wish to create the role assignments.</span></span> <span data-ttu-id="aa388-187">A következő példák bemutatják, hogyan adhatja meg a különböző szintű hatóköre:</span><span class="sxs-lookup"><span data-stu-id="aa388-187">The following examples show how to specify the scope for different levels:</span></span>

   * <span data-ttu-id="aa388-188">Előfizetés: /subscriptions/ {előfizetés-azonosító}</span><span class="sxs-lookup"><span data-stu-id="aa388-188">Subscription: /subscriptions/{subscription-id}</span></span>  
   * <span data-ttu-id="aa388-189">Erőforráscsoport: /subscriptions/ {előfizetés-azonosító} / resourceGroups/myresourcegroup1</span><span class="sxs-lookup"><span data-stu-id="aa388-189">Resource Group: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1</span></span>  
   * <span data-ttu-id="aa388-190">Erőforrás: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span><span class="sxs-lookup"><span data-stu-id="aa388-190">Resource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span></span>  
2. <span data-ttu-id="aa388-191">Cserélje le *{szerepkör-hozzárendelés-azonosító}* a GUID azonosító.</span><span class="sxs-lookup"><span data-stu-id="aa388-191">Replace *{role-assignment-id}* with the role assignment id GUID.</span></span>
3. <span data-ttu-id="aa388-192">Cserélje le *{api-version}* a 2015-07-01.</span><span class="sxs-lookup"><span data-stu-id="aa388-192">Replace *{api-version}* with 2015-07-01.</span></span>

### <a name="response"></a><span data-ttu-id="aa388-193">Válasz</span><span class="sxs-lookup"><span data-stu-id="aa388-193">Response</span></span>
<span data-ttu-id="aa388-194">Állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="aa388-194">Status code: 200</span></span>

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

## <a name="list-all-roles"></a><span data-ttu-id="aa388-195">Az összes szerepkör felsorolása</span><span class="sxs-lookup"><span data-stu-id="aa388-195">List all Roles</span></span>
<span data-ttu-id="aa388-196">A megadott hatókörben kiosztására használható összes szerepköröket sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="aa388-196">Lists all the roles that are available for assignment at the specified scope.</span></span>

<span data-ttu-id="aa388-197">A lista szerepkörökhöz hozzáféréssel kell rendelkeznie `Microsoft.Authorization/roleDefinitions/read` műveletet a hatókörben.</span><span class="sxs-lookup"><span data-stu-id="aa388-197">To list roles, you must have access to `Microsoft.Authorization/roleDefinitions/read` operation at the scope.</span></span> <span data-ttu-id="aa388-198">A beépített szerepkörök művelet számára hozzáférést kapnak.</span><span class="sxs-lookup"><span data-stu-id="aa388-198">All the built-in roles are granted access to this operation.</span></span> <span data-ttu-id="aa388-199">További információ a szerepkör-hozzárendelések és az Azure-erőforrások kezelése hozzáférés: [átruházásához hozzáférés-vezérlés](role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="aa388-199">For more information about role assignments and managing access for Azure resources, see [Azure Role-Based Access Control](role-based-access-control-configure.md).</span></span>

### <a name="request"></a><span data-ttu-id="aa388-200">Kérés</span><span class="sxs-lookup"><span data-stu-id="aa388-200">Request</span></span>
<span data-ttu-id="aa388-201">Használja a **beolvasása** metódus a következő URI-azonosítóhoz:</span><span class="sxs-lookup"><span data-stu-id="aa388-201">Use the **GET** method with the following URI:</span></span>

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions?api-version={api-version}&$filter={filter}

<span data-ttu-id="aa388-202">Az URI belül győződjön testre szabhatja a kérelem a következő helyettesítések hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="aa388-202">Within the URI, make the following substitutions to customize your request:</span></span>

1. <span data-ttu-id="aa388-203">Cserélje le *{hatókör}* a hatókörben, amelynek meg szeretné össze a szerepkörök listáját.</span><span class="sxs-lookup"><span data-stu-id="aa388-203">Replace *{scope}* with the scope for which you wish to list the roles.</span></span> <span data-ttu-id="aa388-204">A következő példák bemutatják, hogyan adhatja meg a különböző szintű hatóköre:</span><span class="sxs-lookup"><span data-stu-id="aa388-204">The following examples show how to specify the scope for different levels:</span></span>

   * <span data-ttu-id="aa388-205">Előfizetés: /subscriptions/ {előfizetés-azonosító}</span><span class="sxs-lookup"><span data-stu-id="aa388-205">Subscription: /subscriptions/{subscription-id}</span></span>  
   * <span data-ttu-id="aa388-206">Erőforráscsoport: /subscriptions/ {előfizetés-azonosító} / resourceGroups/myresourcegroup1</span><span class="sxs-lookup"><span data-stu-id="aa388-206">Resource Group: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1</span></span>  
   * <span data-ttu-id="aa388-207">Erőforrás /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span><span class="sxs-lookup"><span data-stu-id="aa388-207">Resource /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span></span>  
2. <span data-ttu-id="aa388-208">Cserélje le *{api-version}* a 2015-07-01.</span><span class="sxs-lookup"><span data-stu-id="aa388-208">Replace *{api-version}* with 2015-07-01.</span></span>
3. <span data-ttu-id="aa388-209">Cserélje le *{szűrő}* azzal a feltétellel, akkor a szerepkörök szűréséhez alkalmazni kíván:</span><span class="sxs-lookup"><span data-stu-id="aa388-209">Replace *{filter}* with the condition that you wish to apply to filter the list of roles:</span></span>

   * <span data-ttu-id="aa388-210">Lista szerepkörökhöz rendelhető hozzá a megadott hatókörben és bármely gyermek hatóköréhez tartozó:`atScopeAndBelow()`</span><span class="sxs-lookup"><span data-stu-id="aa388-210">List roles available for assignment at the specified scope and any of its child scopes: `atScopeAndBelow()`</span></span>
   * <span data-ttu-id="aa388-211">Keresse meg egy szerepkör használatával pontos megjelenített név: `roleName%20eq%20'{role-display-name}'`.</span><span class="sxs-lookup"><span data-stu-id="aa388-211">Search for a role using exact display name: `roleName%20eq%20'{role-display-name}'`.</span></span> <span data-ttu-id="aa388-212">Az URL-kódolású képernyőn a pontos megjelenített névbe, a szerepkör.</span><span class="sxs-lookup"><span data-stu-id="aa388-212">Use the URL encoded form of the exact display name of the role.</span></span> <span data-ttu-id="aa388-213">Például`$filter=roleName%20eq%20'Virtual%20Machine%20Contributor'` |</span><span class="sxs-lookup"><span data-stu-id="aa388-213">For instance, `$filter=roleName%20eq%20'Virtual%20Machine%20Contributor'` |</span></span>

### <a name="response"></a><span data-ttu-id="aa388-214">Válasz</span><span class="sxs-lookup"><span data-stu-id="aa388-214">Response</span></span>
<span data-ttu-id="aa388-215">Állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="aa388-215">Status code: 200</span></span>

```
{
  "value": [
    {
      "properties": {
        "roleName": "Virtual Machine Contributor",
        "type": "BuiltInRole",
        "description": "Lets you manage virtual machines, but not access to them, and not the virtual network or storage account they\u2019re connected to.",
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

## <a name="get-information-about-a-role"></a><span data-ttu-id="aa388-216">Egy szerepkör adatainak beolvasása</span><span class="sxs-lookup"><span data-stu-id="aa388-216">Get information about a Role</span></span>
<span data-ttu-id="aa388-217">A szerepkör-definíció azonosítója által megadott egyetlen szerepkör információ lekérése.</span><span class="sxs-lookup"><span data-stu-id="aa388-217">Gets information about a single role specified by the role definition identifier.</span></span> <span data-ttu-id="aa388-218">Ahhoz, hogy a megjelenített név megadásával egyetlen szerepkör információt, lásd: [listában az összes szerepkör](role-based-access-control-manage-access-rest.md#list-all-roles).</span><span class="sxs-lookup"><span data-stu-id="aa388-218">To get information about a single role using its display name, see [List all roles](role-based-access-control-manage-access-rest.md#list-all-roles).</span></span>

<span data-ttu-id="aa388-219">Egy szerepkör adatainak beolvasása, hozzáféréssel kell rendelkeznie `Microsoft.Authorization/roleDefinitions/read` műveletet.</span><span class="sxs-lookup"><span data-stu-id="aa388-219">To get information about a role, you must have access to `Microsoft.Authorization/roleDefinitions/read` operation.</span></span> <span data-ttu-id="aa388-220">A beépített szerepkörök művelet számára hozzáférést kapnak.</span><span class="sxs-lookup"><span data-stu-id="aa388-220">All the built-in roles are granted access to this operation.</span></span> <span data-ttu-id="aa388-221">További információ a szerepkör-hozzárendelések és az Azure-erőforrások kezelése hozzáférés: [átruházásához hozzáférés-vezérlés](role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="aa388-221">For more information about role assignments and managing access for Azure resources, see [Azure Role-Based Access Control](role-based-access-control-configure.md).</span></span>

### <a name="request"></a><span data-ttu-id="aa388-222">Kérés</span><span class="sxs-lookup"><span data-stu-id="aa388-222">Request</span></span>
<span data-ttu-id="aa388-223">Használja a **beolvasása** metódus a következő URI-azonosítóhoz:</span><span class="sxs-lookup"><span data-stu-id="aa388-223">Use the **GET** method with the following URI:</span></span>

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

<span data-ttu-id="aa388-224">Az URI belül győződjön testre szabhatja a kérelem a következő helyettesítések hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="aa388-224">Within the URI, make the following substitutions to customize your request:</span></span>

1. <span data-ttu-id="aa388-225">Cserélje le *{hatókör}* a hatókörben, amelynek kívánja a szerepkör-hozzárendelések listáját.</span><span class="sxs-lookup"><span data-stu-id="aa388-225">Replace *{scope}* with the scope for which you wish to list the role assignments.</span></span> <span data-ttu-id="aa388-226">A következő példák bemutatják, hogyan adhatja meg a különböző szintű hatóköre:</span><span class="sxs-lookup"><span data-stu-id="aa388-226">The following examples show how to specify the scope for different levels:</span></span>

   * <span data-ttu-id="aa388-227">Előfizetés: /subscriptions/ {előfizetés-azonosító}</span><span class="sxs-lookup"><span data-stu-id="aa388-227">Subscription: /subscriptions/{subscription-id}</span></span>  
   * <span data-ttu-id="aa388-228">Erőforráscsoport: /subscriptions/ {előfizetés-azonosító} / resourceGroups/myresourcegroup1</span><span class="sxs-lookup"><span data-stu-id="aa388-228">Resource Group: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1</span></span>  
   * <span data-ttu-id="aa388-229">Erőforrás: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span><span class="sxs-lookup"><span data-stu-id="aa388-229">Resource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span></span>  
2. <span data-ttu-id="aa388-230">Cserélje le *{szerepkör-definíció-azonosító}* a szerepkör-definíció GUID azonosítóval.</span><span class="sxs-lookup"><span data-stu-id="aa388-230">Replace *{role-definition-id}* with the GUID identifier of the role definition.</span></span>
3. <span data-ttu-id="aa388-231">Cserélje le *{api-version}* a 2015-07-01.</span><span class="sxs-lookup"><span data-stu-id="aa388-231">Replace *{api-version}* with 2015-07-01.</span></span>

### <a name="response"></a><span data-ttu-id="aa388-232">Válasz</span><span class="sxs-lookup"><span data-stu-id="aa388-232">Response</span></span>
<span data-ttu-id="aa388-233">Állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="aa388-233">Status code: 200</span></span>

```
{
  "value": [
    {
      "properties": {
        "roleName": "Virtual Machine Contributor",
        "type": "BuiltInRole",
        "description": "Lets you manage virtual machines, but not access to them, and not the virtual network or storage account they\u2019re connected to.",
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

## <a name="create-a-custom-role"></a><span data-ttu-id="aa388-234">Egyéni szerepkör létrehozása</span><span class="sxs-lookup"><span data-stu-id="aa388-234">Create a Custom Role</span></span>
<span data-ttu-id="aa388-235">Hozzon létre egy egyéni biztonsági szerepkört.</span><span class="sxs-lookup"><span data-stu-id="aa388-235">Create a custom role.</span></span>

<span data-ttu-id="aa388-236">Hozzon létre egy egyéni biztonsági szerepkört, hozzáférést kell biztosítani `Microsoft.Authorization/roleDefinitions/write` minden műveletet a `AssignableScopes`.</span><span class="sxs-lookup"><span data-stu-id="aa388-236">To create a custom role, you must have access to `Microsoft.Authorization/roleDefinitions/write` operation on all the `AssignableScopes`.</span></span> <span data-ttu-id="aa388-237">A beépített szerepkörök, csak *tulajdonos* és *felhasználói hozzáférés adminisztrátora* férhetnek hozzá ezt a műveletet.</span><span class="sxs-lookup"><span data-stu-id="aa388-237">Of the built-in roles, only *Owner* and *User Access Administrator* are granted access to this operation.</span></span> <span data-ttu-id="aa388-238">További információ a szerepkör-hozzárendelések és az Azure-erőforrások kezelése hozzáférés: [átruházásához hozzáférés-vezérlés](role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="aa388-238">For more information about role assignments and managing access for Azure resources, see [Azure Role-Based Access Control](role-based-access-control-configure.md).</span></span>

### <a name="request"></a><span data-ttu-id="aa388-239">Kérés</span><span class="sxs-lookup"><span data-stu-id="aa388-239">Request</span></span>
<span data-ttu-id="aa388-240">Használja a **PUT** metódus a következő URI-azonosítóhoz:</span><span class="sxs-lookup"><span data-stu-id="aa388-240">Use the **PUT** method with the following URI:</span></span>

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

<span data-ttu-id="aa388-241">Az URI belül győződjön testre szabhatja a kérelem a következő helyettesítések hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="aa388-241">Within the URI, make the following substitutions to customize your request:</span></span>

1. <span data-ttu-id="aa388-242">Cserélje le *{hatókör}* az első *AssignableScope* az egyéni szerepkör.</span><span class="sxs-lookup"><span data-stu-id="aa388-242">Replace *{scope}* with the first *AssignableScope* of the custom role.</span></span> <span data-ttu-id="aa388-243">A következő példák bemutatják, hogyan határozhatja meg a különböző szintű hatókörét.</span><span class="sxs-lookup"><span data-stu-id="aa388-243">The following examples show how to specify the scope for different levels.</span></span>

   * <span data-ttu-id="aa388-244">Előfizetés: /subscriptions/ {előfizetés-azonosító}</span><span class="sxs-lookup"><span data-stu-id="aa388-244">Subscription: /subscriptions/{subscription-id}</span></span>  
   * <span data-ttu-id="aa388-245">Erőforráscsoport: /subscriptions/ {előfizetés-azonosító} / resourceGroups/myresourcegroup1</span><span class="sxs-lookup"><span data-stu-id="aa388-245">Resource Group: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1</span></span>  
   * <span data-ttu-id="aa388-246">Erőforrás: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span><span class="sxs-lookup"><span data-stu-id="aa388-246">Resource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span></span>  
2. <span data-ttu-id="aa388-247">Cserélje le *{szerepkör-definíció-azonosító}* egy új GUID, amely válik az új egyéni szerepkör GUID azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="aa388-247">Replace *{role-definition-id}* with a new GUID, which becomes the GUID identifier of the new custom role.</span></span>
3. <span data-ttu-id="aa388-248">Cserélje le *{api-version}* a 2015-07-01.</span><span class="sxs-lookup"><span data-stu-id="aa388-248">Replace *{api-version}* with 2015-07-01.</span></span>

<span data-ttu-id="aa388-249">A kérelem törzsében adja meg az értékeket a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="aa388-249">For the request body, provide the values in the following format:</span></span>

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

| <span data-ttu-id="aa388-250">Elem neve</span><span class="sxs-lookup"><span data-stu-id="aa388-250">Element Name</span></span> | <span data-ttu-id="aa388-251">Szükséges</span><span class="sxs-lookup"><span data-stu-id="aa388-251">Required</span></span> | <span data-ttu-id="aa388-252">Típus</span><span class="sxs-lookup"><span data-stu-id="aa388-252">Type</span></span> | <span data-ttu-id="aa388-253">Leírás</span><span class="sxs-lookup"><span data-stu-id="aa388-253">Description</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="aa388-254">név</span><span class="sxs-lookup"><span data-stu-id="aa388-254">name</span></span> |<span data-ttu-id="aa388-255">Igen</span><span class="sxs-lookup"><span data-stu-id="aa388-255">Yes</span></span> |<span data-ttu-id="aa388-256">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="aa388-256">String</span></span> |<span data-ttu-id="aa388-257">Az egyéni szerepkör GUID azonosítója</span><span class="sxs-lookup"><span data-stu-id="aa388-257">GUID identifier of the custom role.</span></span> |
| <span data-ttu-id="aa388-258">properties.roleName</span><span class="sxs-lookup"><span data-stu-id="aa388-258">properties.roleName</span></span> |<span data-ttu-id="aa388-259">Igen</span><span class="sxs-lookup"><span data-stu-id="aa388-259">Yes</span></span> |<span data-ttu-id="aa388-260">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="aa388-260">String</span></span> |<span data-ttu-id="aa388-261">Az egyéni szerepkör nevét jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="aa388-261">Display name of the custom role.</span></span> <span data-ttu-id="aa388-262">Legfeljebb 128 karakter.</span><span class="sxs-lookup"><span data-stu-id="aa388-262">Maximum size 128 characters.</span></span> |
| <span data-ttu-id="aa388-263">Properties.Description</span><span class="sxs-lookup"><span data-stu-id="aa388-263">properties.description</span></span> |<span data-ttu-id="aa388-264">Nem</span><span class="sxs-lookup"><span data-stu-id="aa388-264">No</span></span> |<span data-ttu-id="aa388-265">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="aa388-265">String</span></span> |<span data-ttu-id="aa388-266">Az egyéni szerepkör leírása</span><span class="sxs-lookup"><span data-stu-id="aa388-266">Description of the custom role.</span></span> <span data-ttu-id="aa388-267">Maximális méret 1024 karakternél.</span><span class="sxs-lookup"><span data-stu-id="aa388-267">Maximum size 1024 characters.</span></span> |
| <span data-ttu-id="aa388-268">Properties.Type</span><span class="sxs-lookup"><span data-stu-id="aa388-268">properties.type</span></span> |<span data-ttu-id="aa388-269">Igen</span><span class="sxs-lookup"><span data-stu-id="aa388-269">Yes</span></span> |<span data-ttu-id="aa388-270">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="aa388-270">String</span></span> |<span data-ttu-id="aa388-271">Állítsa be a "CustomRole."</span><span class="sxs-lookup"><span data-stu-id="aa388-271">Set to "CustomRole."</span></span> |
| <span data-ttu-id="aa388-272">Properties.permissions.Actions</span><span class="sxs-lookup"><span data-stu-id="aa388-272">properties.permissions.actions</span></span> |<span data-ttu-id="aa388-273">Igen</span><span class="sxs-lookup"><span data-stu-id="aa388-273">Yes</span></span> |<span data-ttu-id="aa388-274">String]</span><span class="sxs-lookup"><span data-stu-id="aa388-274">String[]</span></span> |<span data-ttu-id="aa388-275">Adja meg az egyéni szerepkör által biztosított műveletek művelet karakterláncokból álló tömb.</span><span class="sxs-lookup"><span data-stu-id="aa388-275">An array of action strings specifying the operations granted by the custom role.</span></span> |
| <span data-ttu-id="aa388-276">properties.permissions.notActions</span><span class="sxs-lookup"><span data-stu-id="aa388-276">properties.permissions.notActions</span></span> |<span data-ttu-id="aa388-277">Nem</span><span class="sxs-lookup"><span data-stu-id="aa388-277">No</span></span> |<span data-ttu-id="aa388-278">String]</span><span class="sxs-lookup"><span data-stu-id="aa388-278">String[]</span></span> |<span data-ttu-id="aa388-279">Ha szeretne kizárni a műveletek az egyéni szerepkör által biztosított műveletek művelet karakterláncokkal tömbjét.</span><span class="sxs-lookup"><span data-stu-id="aa388-279">An array of action strings specifying the operations to exclude from the operations granted by the custom role.</span></span> |
| <span data-ttu-id="aa388-280">properties.assignableScopes</span><span class="sxs-lookup"><span data-stu-id="aa388-280">properties.assignableScopes</span></span> |<span data-ttu-id="aa388-281">Igen</span><span class="sxs-lookup"><span data-stu-id="aa388-281">Yes</span></span> |<span data-ttu-id="aa388-282">String]</span><span class="sxs-lookup"><span data-stu-id="aa388-282">String[]</span></span> |<span data-ttu-id="aa388-283">A tömb hatókörök, amelyben az egyéni biztonsági szerepkört is használható.</span><span class="sxs-lookup"><span data-stu-id="aa388-283">An array of scopes in which the custom role can be used.</span></span> |

### <a name="response"></a><span data-ttu-id="aa388-284">Válasz</span><span class="sxs-lookup"><span data-stu-id="aa388-284">Response</span></span>
<span data-ttu-id="aa388-285">Állapotkód: 201-et</span><span class="sxs-lookup"><span data-stu-id="aa388-285">Status code: 201</span></span>

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

## <a name="update-a-custom-role"></a><span data-ttu-id="aa388-286">Frissítés egy egyéni biztonsági szerepkört</span><span class="sxs-lookup"><span data-stu-id="aa388-286">Update a Custom Role</span></span>
<span data-ttu-id="aa388-287">Módosítani egy egyéni biztonsági szerepkört.</span><span class="sxs-lookup"><span data-stu-id="aa388-287">Modify a custom role.</span></span>

<span data-ttu-id="aa388-288">Módosítja egy egyéni biztonsági szerepkört, hozzáféréssel kell rendelkeznie `Microsoft.Authorization/roleDefinitions/write` minden műveletet a `AssignableScopes`.</span><span class="sxs-lookup"><span data-stu-id="aa388-288">To modify a custom role, you must have access to `Microsoft.Authorization/roleDefinitions/write` operation on all the `AssignableScopes`.</span></span> <span data-ttu-id="aa388-289">A beépített szerepkörök, csak *tulajdonos* és *felhasználói hozzáférés adminisztrátora* férhetnek hozzá ezt a műveletet.</span><span class="sxs-lookup"><span data-stu-id="aa388-289">Of the built-in roles, only *Owner* and *User Access Administrator* are granted access to this operation.</span></span> <span data-ttu-id="aa388-290">További információ a szerepkör-hozzárendelések és az Azure-erőforrások kezelése hozzáférés: [átruházásához hozzáférés-vezérlés](role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="aa388-290">For more information about role assignments and managing access for Azure resources, see [Azure Role-Based Access Control](role-based-access-control-configure.md).</span></span>

### <a name="request"></a><span data-ttu-id="aa388-291">Kérés</span><span class="sxs-lookup"><span data-stu-id="aa388-291">Request</span></span>
<span data-ttu-id="aa388-292">Használja a **PUT** metódus a következő URI-azonosítóhoz:</span><span class="sxs-lookup"><span data-stu-id="aa388-292">Use the **PUT** method with the following URI:</span></span>

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

<span data-ttu-id="aa388-293">Az URI belül győződjön testre szabhatja a kérelem a következő helyettesítések hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="aa388-293">Within the URI, make the following substitutions to customize your request:</span></span>

1. <span data-ttu-id="aa388-294">Cserélje le *{hatókör}* az első *AssignableScope* az egyéni szerepkör.</span><span class="sxs-lookup"><span data-stu-id="aa388-294">Replace *{scope}* with the first *AssignableScope* of the custom role.</span></span> <span data-ttu-id="aa388-295">A következő példák bemutatják, hogyan adhatja meg a különböző szintű hatóköre:</span><span class="sxs-lookup"><span data-stu-id="aa388-295">The following examples show how to specify the scope for different levels:</span></span>

   * <span data-ttu-id="aa388-296">Előfizetés: /subscriptions/ {előfizetés-azonosító}</span><span class="sxs-lookup"><span data-stu-id="aa388-296">Subscription: /subscriptions/{subscription-id}</span></span>  
   * <span data-ttu-id="aa388-297">Erőforráscsoport: /subscriptions/ {előfizetés-azonosító} / resourceGroups/myresourcegroup1</span><span class="sxs-lookup"><span data-stu-id="aa388-297">Resource Group: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1</span></span>  
   * <span data-ttu-id="aa388-298">Erőforrás: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span><span class="sxs-lookup"><span data-stu-id="aa388-298">Resource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span></span>  
2. <span data-ttu-id="aa388-299">Cserélje le *{szerepkör-definíció-azonosító}* az egyéni szerepkör GUID azonosítóval.</span><span class="sxs-lookup"><span data-stu-id="aa388-299">Replace *{role-definition-id}* with the GUID identifier of the custom role.</span></span>
3. <span data-ttu-id="aa388-300">Cserélje le *{api-version}* a 2015-07-01.</span><span class="sxs-lookup"><span data-stu-id="aa388-300">Replace *{api-version}* with 2015-07-01.</span></span>

<span data-ttu-id="aa388-301">A kérelem törzsében adja meg az értékeket a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="aa388-301">For the request body, provide the values in the following format:</span></span>

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

| <span data-ttu-id="aa388-302">Elem neve</span><span class="sxs-lookup"><span data-stu-id="aa388-302">Element Name</span></span> | <span data-ttu-id="aa388-303">Szükséges</span><span class="sxs-lookup"><span data-stu-id="aa388-303">Required</span></span> | <span data-ttu-id="aa388-304">Típus</span><span class="sxs-lookup"><span data-stu-id="aa388-304">Type</span></span> | <span data-ttu-id="aa388-305">Leírás</span><span class="sxs-lookup"><span data-stu-id="aa388-305">Description</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="aa388-306">név</span><span class="sxs-lookup"><span data-stu-id="aa388-306">name</span></span> |<span data-ttu-id="aa388-307">Igen</span><span class="sxs-lookup"><span data-stu-id="aa388-307">Yes</span></span> |<span data-ttu-id="aa388-308">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="aa388-308">String</span></span> |<span data-ttu-id="aa388-309">Az egyéni szerepkör GUID azonosítója</span><span class="sxs-lookup"><span data-stu-id="aa388-309">GUID identifier of the custom role.</span></span> |
| <span data-ttu-id="aa388-310">properties.roleName</span><span class="sxs-lookup"><span data-stu-id="aa388-310">properties.roleName</span></span> |<span data-ttu-id="aa388-311">Igen</span><span class="sxs-lookup"><span data-stu-id="aa388-311">Yes</span></span> |<span data-ttu-id="aa388-312">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="aa388-312">String</span></span> |<span data-ttu-id="aa388-313">A frissített egyéni szerepkör nevét jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="aa388-313">Display name of the updated custom role.</span></span> |
| <span data-ttu-id="aa388-314">Properties.Description</span><span class="sxs-lookup"><span data-stu-id="aa388-314">properties.description</span></span> |<span data-ttu-id="aa388-315">Nem</span><span class="sxs-lookup"><span data-stu-id="aa388-315">No</span></span> |<span data-ttu-id="aa388-316">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="aa388-316">String</span></span> |<span data-ttu-id="aa388-317">A frissített egyéni szerepkör leírása</span><span class="sxs-lookup"><span data-stu-id="aa388-317">Description of the updated custom role.</span></span> |
| <span data-ttu-id="aa388-318">Properties.Type</span><span class="sxs-lookup"><span data-stu-id="aa388-318">properties.type</span></span> |<span data-ttu-id="aa388-319">Igen</span><span class="sxs-lookup"><span data-stu-id="aa388-319">Yes</span></span> |<span data-ttu-id="aa388-320">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="aa388-320">String</span></span> |<span data-ttu-id="aa388-321">Állítsa be a "CustomRole."</span><span class="sxs-lookup"><span data-stu-id="aa388-321">Set to "CustomRole."</span></span> |
| <span data-ttu-id="aa388-322">Properties.permissions.Actions</span><span class="sxs-lookup"><span data-stu-id="aa388-322">properties.permissions.actions</span></span> |<span data-ttu-id="aa388-323">Igen</span><span class="sxs-lookup"><span data-stu-id="aa388-323">Yes</span></span> |<span data-ttu-id="aa388-324">String]</span><span class="sxs-lookup"><span data-stu-id="aa388-324">String[]</span></span> |<span data-ttu-id="aa388-325">Adja meg a frissített egyéni biztonsági szerepkört, amelyhez engedélyezi a hozzáférést a műveletek művelet karakterláncokból álló tömb.</span><span class="sxs-lookup"><span data-stu-id="aa388-325">An array of action strings specifying the operations to which the updated custom role grants access.</span></span> |
| <span data-ttu-id="aa388-326">properties.permissions.notActions</span><span class="sxs-lookup"><span data-stu-id="aa388-326">properties.permissions.notActions</span></span> |<span data-ttu-id="aa388-327">Nem</span><span class="sxs-lookup"><span data-stu-id="aa388-327">No</span></span> |<span data-ttu-id="aa388-328">String]</span><span class="sxs-lookup"><span data-stu-id="aa388-328">String[]</span></span> |<span data-ttu-id="aa388-329">Adja meg a műveleteket, így a frissített egyéni szerepkörök műveletekről kizárása művelet karakterláncokból álló tömb.</span><span class="sxs-lookup"><span data-stu-id="aa388-329">An array of action strings specifying the operations to exclude from the operations which the updated custom role grants.</span></span> |
| <span data-ttu-id="aa388-330">properties.assignableScopes</span><span class="sxs-lookup"><span data-stu-id="aa388-330">properties.assignableScopes</span></span> |<span data-ttu-id="aa388-331">Igen</span><span class="sxs-lookup"><span data-stu-id="aa388-331">Yes</span></span> |<span data-ttu-id="aa388-332">String]</span><span class="sxs-lookup"><span data-stu-id="aa388-332">String[]</span></span> |<span data-ttu-id="aa388-333">A tömb hatókörök, amelyben a frissített egyéni biztonsági szerepkört is használható.</span><span class="sxs-lookup"><span data-stu-id="aa388-333">An array of scopes in which the updated custom role can be used.</span></span> |

### <a name="response"></a><span data-ttu-id="aa388-334">Válasz</span><span class="sxs-lookup"><span data-stu-id="aa388-334">Response</span></span>
<span data-ttu-id="aa388-335">Állapotkód: 201-et</span><span class="sxs-lookup"><span data-stu-id="aa388-335">Status code: 201</span></span>

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

## <a name="delete-a-custom-role"></a><span data-ttu-id="aa388-336">Egyéni szerepkör törléséhez</span><span class="sxs-lookup"><span data-stu-id="aa388-336">Delete a Custom Role</span></span>
<span data-ttu-id="aa388-337">Egyéni szerepkör törléséhez.</span><span class="sxs-lookup"><span data-stu-id="aa388-337">Delete a custom role.</span></span>

<span data-ttu-id="aa388-338">Egyéni szerepkör törléséhez hozzáféréssel kell rendelkeznie `Microsoft.Authorization/roleDefinitions/delete` minden műveletet a `AssignableScopes`.</span><span class="sxs-lookup"><span data-stu-id="aa388-338">To delete a custom role, you must have access to `Microsoft.Authorization/roleDefinitions/delete` operation on all the `AssignableScopes`.</span></span> <span data-ttu-id="aa388-339">A beépített szerepkörök, csak *tulajdonos* és *felhasználói hozzáférés adminisztrátora* férhetnek hozzá ezt a műveletet.</span><span class="sxs-lookup"><span data-stu-id="aa388-339">Of the built-in roles, only *Owner* and *User Access Administrator* are granted access to this operation.</span></span> <span data-ttu-id="aa388-340">További információ a szerepkör-hozzárendelések és az Azure-erőforrások kezelése hozzáférés: [átruházásához hozzáférés-vezérlés](role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="aa388-340">For more information about role assignments and managing access for Azure resources, see [Azure Role-Based Access Control](role-based-access-control-configure.md).</span></span>

### <a name="request"></a><span data-ttu-id="aa388-341">Kérés</span><span class="sxs-lookup"><span data-stu-id="aa388-341">Request</span></span>
<span data-ttu-id="aa388-342">Használja a **törlése** metódus a következő URI-azonosítóhoz:</span><span class="sxs-lookup"><span data-stu-id="aa388-342">Use the **DELETE** method with the following URI:</span></span>

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

<span data-ttu-id="aa388-343">Az URI belül győződjön testre szabhatja a kérelem a következő helyettesítések hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="aa388-343">Within the URI, make the following substitutions to customize your request:</span></span>

1. <span data-ttu-id="aa388-344">Cserélje le *{hatókör}* , amelyen a szerepkör-definíció törölni kívánt hatókörű.</span><span class="sxs-lookup"><span data-stu-id="aa388-344">Replace *{scope}* with the scope at which you wish to delete the role definition.</span></span> <span data-ttu-id="aa388-345">A következő példák bemutatják, hogyan adhatja meg a különböző szintű hatóköre:</span><span class="sxs-lookup"><span data-stu-id="aa388-345">The following examples show how to specify the scope for different levels:</span></span>

   * <span data-ttu-id="aa388-346">Előfizetés: /subscriptions/ {előfizetés-azonosító}</span><span class="sxs-lookup"><span data-stu-id="aa388-346">Subscription: /subscriptions/{subscription-id}</span></span>  
   * <span data-ttu-id="aa388-347">Erőforráscsoport: /subscriptions/ {előfizetés-azonosító} / resourceGroups/myresourcegroup1</span><span class="sxs-lookup"><span data-stu-id="aa388-347">Resource Group: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1</span></span>  
   * <span data-ttu-id="aa388-348">Erőforrás: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span><span class="sxs-lookup"><span data-stu-id="aa388-348">Resource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span></span>  
2. <span data-ttu-id="aa388-349">Cserélje le *{szerepkör-definíció-azonosító}* a GUID azonosító az egyéni szerepkör.</span><span class="sxs-lookup"><span data-stu-id="aa388-349">Replace *{role-definition-id}* with the GUID role definition id of the custom role.</span></span>
3. <span data-ttu-id="aa388-350">Cserélje le *{api-version}* a 2015-07-01.</span><span class="sxs-lookup"><span data-stu-id="aa388-350">Replace *{api-version}* with 2015-07-01.</span></span>

### <a name="response"></a><span data-ttu-id="aa388-351">Válasz</span><span class="sxs-lookup"><span data-stu-id="aa388-351">Response</span></span>
<span data-ttu-id="aa388-352">Állapotkód: 200</span><span class="sxs-lookup"><span data-stu-id="aa388-352">Status code: 200</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="aa388-353">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="aa388-353">Next steps</span></span>

[!INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]
