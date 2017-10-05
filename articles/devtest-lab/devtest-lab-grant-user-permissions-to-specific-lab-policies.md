---
title: "Felhasználói engedélyek adott labor szabályzatokkal |} Microsoft Docs"
description: "Útmutató: a DevTest Labs szolgáltatásban minden egyes felhasználói igények alapján adott labor házirendek felhasználói engedélyeket"
services: devtest-lab,virtual-machines,visual-studio-online
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 5ca829f0-eb69-40a1-ae26-03a629db1d7e
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/25/2016
ms.author: tarcher
ms.openlocfilehash: 0bd9f83257834d9681479ba9117c48ffd6d6e166
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="grant-user-permissions-to-specific-lab-policies"></a><span data-ttu-id="18442-103">A konkrét labor házirendek felhasználói engedélyek</span><span class="sxs-lookup"><span data-stu-id="18442-103">Grant user permissions to specific lab policies</span></span>
## <a name="overview"></a><span data-ttu-id="18442-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="18442-104">Overview</span></span>
<span data-ttu-id="18442-105">Ez a cikk bemutatja, hogyan lehet egy adott labor házirend felhasználók engedélyeket a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="18442-105">This article illustrates how to use PowerShell to grant users permissions to a particular lab policy.</span></span> <span data-ttu-id="18442-106">Ily módon engedélyek alkalmazhatók minden felhasználói igények alapján.</span><span class="sxs-lookup"><span data-stu-id="18442-106">That way, permissions can be applied based on each user's needs.</span></span> <span data-ttu-id="18442-107">Például érdemes egy adott felhasználó megváltoztathatja a virtuális gép házirend-beállításokat, de nem a költségek házirendek megadását.</span><span class="sxs-lookup"><span data-stu-id="18442-107">For example, you might want to grant a particular user the ability to change the VM policy settings, but not the cost policies.</span></span>

## <a name="policies-as-resources"></a><span data-ttu-id="18442-108">Erőforrásként házirendek</span><span class="sxs-lookup"><span data-stu-id="18442-108">Policies as resources</span></span>
<span data-ttu-id="18442-109">A bemutatott a [Azure szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-configure.md) RBAC a cikkben lehetővé teszi, hogy az Azure-erőforrások részletes hozzáféréskezelést.</span><span class="sxs-lookup"><span data-stu-id="18442-109">As discussed in the [Azure Role-based Access Control](../active-directory/role-based-access-control-configure.md) article, RBAC enables fine-grained access management of resources for Azure.</span></span> <span data-ttu-id="18442-110">Az RBAC használata, feladatokat elkülönítse a DevOps munkacsoporton belül, és csak olyan mértékű hozzáférést biztosítania a felhasználók számára, hogy be kell elvégezni a munkájukat.</span><span class="sxs-lookup"><span data-stu-id="18442-110">Using RBAC, you can segregate duties within your DevOps team and grant only the amount of access to users that they need to perform their jobs.</span></span>

<span data-ttu-id="18442-111">A DevTest Labs szolgáltatásban, a házirend, amely lehetővé teszi a Szerepalapú művelet erőforrástípus **Microsoft.DevTestLab/labs/policySets/policies/**.</span><span class="sxs-lookup"><span data-stu-id="18442-111">In DevTest Labs, a policy is a resource type that enables the RBAC action **Microsoft.DevTestLab/labs/policySets/policies/**.</span></span> <span data-ttu-id="18442-112">Minden tesztkörnyezeti házirend házirend erőforrástípus az erőforráshoz, és hatóköreként RBAC szerepkörhöz is hozzárendelhető.</span><span class="sxs-lookup"><span data-stu-id="18442-112">Each lab policy is a resource in the Policy resource type, and can be assigned as a scope to an RBAC role.</span></span>

<span data-ttu-id="18442-113">Például ahhoz, hogy a felhasználók olvasási/írási engedélyeket a **engedélyezett Virtuálisgép-méretek** házirend, akkor létre egy egyéni biztonsági szerepkört, amely kompatibilis a **Microsoft.DevTestLab/labs/policySets/policies/*** a művelet, majd rendelje hozzá a megfelelő felhasználók az egyéni szerepkör hatókörébe tartozó **Microsoft.DevTestLab/labs/policySets/policies/AllowedVmSizesInLab**.</span><span class="sxs-lookup"><span data-stu-id="18442-113">For example, in order to grant users read/write permission to the **Allowed VM Sizes** policy, you would create a custom role that works with the **Microsoft.DevTestLab/labs/policySets/policies/*** action, and then assign the appropriate users to this custom role in the scope of **Microsoft.DevTestLab/labs/policySets/policies/AllowedVmSizesInLab**.</span></span>

<span data-ttu-id="18442-114">Egyéni szerepkörök az RBAC kapcsolatos további tudnivalókért tekintse meg a [egyéni szerepkörök hozzáférés-vezérlés](../active-directory/role-based-access-control-custom-roles.md).</span><span class="sxs-lookup"><span data-stu-id="18442-114">To learn more about custom roles in RBAC, see the [Custom roles access control](../active-directory/role-based-access-control-custom-roles.md).</span></span>

## <a name="creating-a-lab-custom-role-using-powershell"></a><span data-ttu-id="18442-115">PowerShell-lel labor egyéni szerepkör létrehozása</span><span class="sxs-lookup"><span data-stu-id="18442-115">Creating a lab custom role using PowerShell</span></span>
<span data-ttu-id="18442-116">Ahhoz, hogy az első lépések, olvassa el az alábbi cikket, amely fogja azt ismertetik, hogyan telepítse és konfigurálja az Azure PowerShell-parancsmagok szüksége: [https://azure.microsoft.com/blog/azps-1-0-pre](https://azure.microsoft.com/blog/azps-1-0-pre).</span><span class="sxs-lookup"><span data-stu-id="18442-116">In order to get started, you’ll need to read the following article, which will explain how to install and configure the Azure PowerShell cmdlets: [https://azure.microsoft.com/blog/azps-1-0-pre](https://azure.microsoft.com/blog/azps-1-0-pre).</span></span>

<span data-ttu-id="18442-117">Miután beállította az Azure PowerShell-parancsmagok, a következő feladatokat végezheti el:</span><span class="sxs-lookup"><span data-stu-id="18442-117">Once you’ve set up the Azure PowerShell cmdlets, you can perform the following tasks:</span></span>

* <span data-ttu-id="18442-118">A műveletek/műveleteket egy erőforrás-szolgáltató listázása</span><span class="sxs-lookup"><span data-stu-id="18442-118">List all the operations/actions for a resource provider</span></span>
* <span data-ttu-id="18442-119">Egy adott szerepkörben műveletek listája:</span><span class="sxs-lookup"><span data-stu-id="18442-119">List actions in a particular role:</span></span>
* <span data-ttu-id="18442-120">Egyéni szerepkör létrehozása</span><span class="sxs-lookup"><span data-stu-id="18442-120">Create a custom role</span></span>

<span data-ttu-id="18442-121">A következő PowerShell-parancsfájl a feladatok végrehajtásához példákat szemlélteti:</span><span class="sxs-lookup"><span data-stu-id="18442-121">The following PowerShell script illustrates examples of how to perform these tasks:</span></span>

    ‘List all the operations/actions for a resource provider.
    Get-AzureRmProviderOperation -OperationSearchString "Microsoft.DevTestLab/*"

    ‘List actions in a particular role.
    (Get-AzureRmRoleDefinition "DevTest Labs User").Actions

    ‘Create custom role.
    $policyRoleDef = (Get-AzureRmRoleDefinition "DevTest Labs User")
    $policyRoleDef.Id = $null
    $policyRoleDef.Name = "Policy Contributor"
    $policyRoleDef.IsCustom = $true
    $policyRoleDef.AssignableScopes.Clear()
    $policyRoleDef.AssignableScopes.Add("/subscriptions/<SubscriptionID> ")
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/policySets/policies/*")
    $policyRoleDef = (New-AzureRmRoleDefinition -Role $policyRoleDef)

## <a name="assigning-permissions-to-a-user-for-a-specific-policy-using-custom-roles"></a><span data-ttu-id="18442-122">Engedélyek hozzárendelése a felhasználó egy adott házirend segítségével egyéni szerepkörök</span><span class="sxs-lookup"><span data-stu-id="18442-122">Assigning permissions to a user for a specific policy using custom roles</span></span>
<span data-ttu-id="18442-123">Miután meghatározta a egyéni szerepkörök, hozzárendelheti azokat a felhasználókat.</span><span class="sxs-lookup"><span data-stu-id="18442-123">Once you’ve defined your custom roles, you can assign them to users.</span></span> <span data-ttu-id="18442-124">Ahhoz, hogy egy egyéni biztonsági szerepkört rendel egy felhasználói, be kell szereznie a **ObjectId** képviselő, hogy a felhasználó.</span><span class="sxs-lookup"><span data-stu-id="18442-124">In order to assign a custom role to a user, you must first obtain the **ObjectId** representing that user.</span></span> <span data-ttu-id="18442-125">Ehhez használja a **Get-AzureRmADUser** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="18442-125">To do that, use the **Get-AzureRmADUser** cmdlet.</span></span>

<span data-ttu-id="18442-126">A következő példában a **ObjectId** , a *SomeUser* felhasználói 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3.</span><span class="sxs-lookup"><span data-stu-id="18442-126">In the following example, the **ObjectId** of the *SomeUser* user is 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3.</span></span>

    PS C:\>Get-AzureRmADUser -SearchString "SomeUser"

    DisplayName                    Type                           ObjectId
    -----------                    ----                           --------
    someuser@hotmail.com                                          05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3

<span data-ttu-id="18442-127">Ha elvégezte a **ObjectId** a felhasználó és egy egyéni szerepkör nevét, a felhasználó számára, hogy szerepkört is rendelhet a **New-AzureRmRoleAssignment** parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="18442-127">Once you have the **ObjectId** for the user and a custom role name, you can assign that role to the user with the **New-AzureRmRoleAssignment** cmdlet:</span></span>

    PS C:\>New-AzureRmRoleAssignment -ObjectId 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3 -RoleDefinitionName "Policy Contributor" -Scope /subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.DevTestLab/labs/<LabName>/policySets/policies/AllowedVmSizesInLab

<span data-ttu-id="18442-128">Az előző példában a **AllowedVmSizesInLab** házirendet használja.</span><span class="sxs-lookup"><span data-stu-id="18442-128">In the previous example, the **AllowedVmSizesInLab** policy is used.</span></span> <span data-ttu-id="18442-129">Használhatja a következő házirendhez:</span><span class="sxs-lookup"><span data-stu-id="18442-129">You can use any of the following polices:</span></span>

* <span data-ttu-id="18442-130">MaxVmsAllowedPerUser</span><span class="sxs-lookup"><span data-stu-id="18442-130">MaxVmsAllowedPerUser</span></span>
* <span data-ttu-id="18442-131">MaxVmsAllowedPerLab</span><span class="sxs-lookup"><span data-stu-id="18442-131">MaxVmsAllowedPerLab</span></span>
* <span data-ttu-id="18442-132">AllowedVmSizesInLab</span><span class="sxs-lookup"><span data-stu-id="18442-132">AllowedVmSizesInLab</span></span>
* <span data-ttu-id="18442-133">LabVmsShutdown</span><span class="sxs-lookup"><span data-stu-id="18442-133">LabVmsShutdown</span></span>

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a><span data-ttu-id="18442-134">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="18442-134">Next steps</span></span>
<span data-ttu-id="18442-135">Egyszer már engedélyeket felhasználó adott labor házirendek, az alábbiakban néhány lépéseket érdemes figyelembe venni:</span><span class="sxs-lookup"><span data-stu-id="18442-135">Once you've granted user permissions to specific lab policies, here are some next steps to consider:</span></span>

* <span data-ttu-id="18442-136">[Biztonságos hozzáférés a laborokhoz](devtest-lab-add-devtest-user.md).</span><span class="sxs-lookup"><span data-stu-id="18442-136">[Secure access to a lab](devtest-lab-add-devtest-user.md).</span></span>
* <span data-ttu-id="18442-137">[Laborházirendek megadása](devtest-lab-set-lab-policy.md).</span><span class="sxs-lookup"><span data-stu-id="18442-137">[Set lab policies](devtest-lab-set-lab-policy.md).</span></span>
* <span data-ttu-id="18442-138">[Laborsablon létrehozása](devtest-lab-create-template.md).</span><span class="sxs-lookup"><span data-stu-id="18442-138">[Create a lab template](devtest-lab-create-template.md).</span></span>
* <span data-ttu-id="18442-139">[Egyéni összetevők létrehozása a virtuális géphez](devtest-lab-artifact-author.md).</span><span class="sxs-lookup"><span data-stu-id="18442-139">[Create custom artifacts for your VMs](devtest-lab-artifact-author.md).</span></span>
* <span data-ttu-id="18442-140">[Összetevőkkel rendelkező virtuális gép hozzáadása egy laborhoz](devtest-lab-add-vm-with-artifacts.md).</span><span class="sxs-lookup"><span data-stu-id="18442-140">[Add a VM with artifacts to a lab](devtest-lab-add-vm-with-artifacts.md).</span></span>

