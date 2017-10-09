---
title: "aaaGrant felhasználói engedélyek toospecific labor házirendeket |} Microsoft Docs"
description: "Ismerje meg, hogyan toogrant felhasználói engedélyek toospecific labor házirendeket a DevTest Labs szolgáltatásban minden egyes felhasználói igények alapján"
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
ms.openlocfilehash: 35647ab837243188f06566cdf365b67fe33a3865
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="grant-user-permissions-toospecific-lab-policies"></a><span data-ttu-id="dcb6f-103">Felhasználói engedélyek toospecific labor házirendek</span><span class="sxs-lookup"><span data-stu-id="dcb6f-103">Grant user permissions toospecific lab policies</span></span>
## <a name="overview"></a><span data-ttu-id="dcb6f-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="dcb6f-104">Overview</span></span>
<span data-ttu-id="dcb6f-105">Ez a cikk bemutatja, hogyan toouse PowerShell toogrant felhasználók engedélyek tooa adott labor házirend.</span><span class="sxs-lookup"><span data-stu-id="dcb6f-105">This article illustrates how toouse PowerShell toogrant users permissions tooa particular lab policy.</span></span> <span data-ttu-id="dcb6f-106">Ily módon engedélyek alkalmazhatók minden felhasználói igények alapján.</span><span class="sxs-lookup"><span data-stu-id="dcb6f-106">That way, permissions can be applied based on each user's needs.</span></span> <span data-ttu-id="dcb6f-107">Például érdemes toogrant egy adott felhasználói hello képességét toochange hello VM házirend-beállításokat, de nem hello házirendek költsége.</span><span class="sxs-lookup"><span data-stu-id="dcb6f-107">For example, you might want toogrant a particular user hello ability toochange hello VM policy settings, but not hello cost policies.</span></span>

## <a name="policies-as-resources"></a><span data-ttu-id="dcb6f-108">Erőforrásként házirendek</span><span class="sxs-lookup"><span data-stu-id="dcb6f-108">Policies as resources</span></span>
<span data-ttu-id="dcb6f-109">Hello leírtaknak megfelelően [Azure szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-configure.md) RBAC a cikkben lehetővé teszi, hogy az Azure-erőforrások részletes hozzáféréskezelést.</span><span class="sxs-lookup"><span data-stu-id="dcb6f-109">As discussed in hello [Azure Role-based Access Control](../active-directory/role-based-access-control-configure.md) article, RBAC enables fine-grained access management of resources for Azure.</span></span> <span data-ttu-id="dcb6f-110">Az RBAC használata, feladatokat elkülönítse a DevOps munkacsoporton belül, és hozzáférési toousers csak hello összegét adja meg, hogy be kell tooperform a feladatokat a.</span><span class="sxs-lookup"><span data-stu-id="dcb6f-110">Using RBAC, you can segregate duties within your DevOps team and grant only hello amount of access toousers that they need tooperform their jobs.</span></span>

<span data-ttu-id="dcb6f-111">A DevTest Labs szolgáltatásban, a házirend, amely lehetővé teszi a hello RBAC művelet erőforrástípus **Microsoft.DevTestLab/labs/policySets/policies/**.</span><span class="sxs-lookup"><span data-stu-id="dcb6f-111">In DevTest Labs, a policy is a resource type that enables hello RBAC action **Microsoft.DevTestLab/labs/policySets/policies/**.</span></span> <span data-ttu-id="dcb6f-112">Minden tesztkörnyezeti házirend hello házirend erőforrástípus az erőforráshoz, és hozzárendelhető hatókör tooan RBAC szerepkörként.</span><span class="sxs-lookup"><span data-stu-id="dcb6f-112">Each lab policy is a resource in hello Policy resource type, and can be assigned as a scope tooan RBAC role.</span></span>

<span data-ttu-id="dcb6f-113">Például a rendelés toogrant felhasználók olvasási/írási engedéllyel toohello **engedélyezett Virtuálisgép-méretek** házirend, akkor létre egy egyéni biztonsági szerepkört, amely kompatibilis a hello **Microsoft.DevTestLab/labs/policySets/policies/** * művelet, és rendelje hozzá hello megfelelő felhasználók toothis egyéni szerepkör hatóköre hello **Microsoft.DevTestLab/labs/policySets/policies/AllowedVmSizesInLab**.</span><span class="sxs-lookup"><span data-stu-id="dcb6f-113">For example, in order toogrant users read/write permission toohello **Allowed VM Sizes** policy, you would create a custom role that works with hello **Microsoft.DevTestLab/labs/policySets/policies/*** action, and then assign hello appropriate users toothis custom role in hello scope of **Microsoft.DevTestLab/labs/policySets/policies/AllowedVmSizesInLab**.</span></span>

<span data-ttu-id="dcb6f-114">További információ az egyéni szerepkörök az RBAC, toolearn, lásd: hello [egyéni szerepkörök hozzáférés-vezérlés](../active-directory/role-based-access-control-custom-roles.md).</span><span class="sxs-lookup"><span data-stu-id="dcb6f-114">toolearn more about custom roles in RBAC, see hello [Custom roles access control](../active-directory/role-based-access-control-custom-roles.md).</span></span>

## <a name="creating-a-lab-custom-role-using-powershell"></a><span data-ttu-id="dcb6f-115">PowerShell-lel labor egyéni szerepkör létrehozása</span><span class="sxs-lookup"><span data-stu-id="dcb6f-115">Creating a lab custom role using PowerShell</span></span>
<span data-ttu-id="dcb6f-116">A lépések sorrendben tooget, szüksége lesz a következő cikket, amely alapján meghatározható tooread hello hogyan tooinstall és Azure PowerShell-parancsmagok hello konfigurálása: [https://azure.microsoft.com/blog/azps-1-0-pre](https://azure.microsoft.com/blog/azps-1-0-pre).</span><span class="sxs-lookup"><span data-stu-id="dcb6f-116">In order tooget started, you’ll need tooread hello following article, which will explain how tooinstall and configure hello Azure PowerShell cmdlets: [https://azure.microsoft.com/blog/azps-1-0-pre](https://azure.microsoft.com/blog/azps-1-0-pre).</span></span>

<span data-ttu-id="dcb6f-117">Azure PowerShell-parancsmagok hello beállítása után hello a következő feladatokat végezheti el:</span><span class="sxs-lookup"><span data-stu-id="dcb6f-117">Once you’ve set up hello Azure PowerShell cmdlets, you can perform hello following tasks:</span></span>

* <span data-ttu-id="dcb6f-118">Egy erőforrás-szolgáltató minden hello műveletek/műveletek listázása</span><span class="sxs-lookup"><span data-stu-id="dcb6f-118">List all hello operations/actions for a resource provider</span></span>
* <span data-ttu-id="dcb6f-119">Egy adott szerepkörben műveletek listája:</span><span class="sxs-lookup"><span data-stu-id="dcb6f-119">List actions in a particular role:</span></span>
* <span data-ttu-id="dcb6f-120">Egyéni szerepkör létrehozása</span><span class="sxs-lookup"><span data-stu-id="dcb6f-120">Create a custom role</span></span>

<span data-ttu-id="dcb6f-121">a következő PowerShell-parancsfájl hello példákat szemlélteti tooperform ezeket a feladatokat:</span><span class="sxs-lookup"><span data-stu-id="dcb6f-121">hello following PowerShell script illustrates examples of how tooperform these tasks:</span></span>

    ‘List all hello operations/actions for a resource provider.
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

## <a name="assigning-permissions-tooa-user-for-a-specific-policy-using-custom-roles"></a><span data-ttu-id="dcb6f-122">Engedélyek tooa felhasználói egy adott házirend segítségével egyéni szerepkörök hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="dcb6f-122">Assigning permissions tooa user for a specific policy using custom roles</span></span>
<span data-ttu-id="dcb6f-123">Miután meghatározta a egyéni szerepkörök, hozzárendelheti azokat toousers.</span><span class="sxs-lookup"><span data-stu-id="dcb6f-123">Once you’ve defined your custom roles, you can assign them toousers.</span></span> <span data-ttu-id="dcb6f-124">A sorrend tooassign egy egyéni biztonsági szerepkört tooa felhasználói, be kell szereznie hello **ObjectId** képviselő, hogy a felhasználó.</span><span class="sxs-lookup"><span data-stu-id="dcb6f-124">In order tooassign a custom role tooa user, you must first obtain hello **ObjectId** representing that user.</span></span> <span data-ttu-id="dcb6f-125">használó, hello toodo **Get-AzureRmADUser** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="dcb6f-125">toodo that, use hello **Get-AzureRmADUser** cmdlet.</span></span>

<span data-ttu-id="dcb6f-126">A következő példa hello, hello **ObjectId** a hello *SomeUser* felhasználói 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3.</span><span class="sxs-lookup"><span data-stu-id="dcb6f-126">In hello following example, hello **ObjectId** of hello *SomeUser* user is 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3.</span></span>

    PS C:\>Get-AzureRmADUser -SearchString "SomeUser"

    DisplayName                    Type                           ObjectId
    -----------                    ----                           --------
    someuser@hotmail.com                                          05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3

<span data-ttu-id="dcb6f-127">Ha elvégezte a hello **ObjectId** hello felhasználói és egyéni szerepkör nevét, akkor is lehet rendelni a hello szerepkör toohello felhasználó **New-AzureRmRoleAssignment** parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="dcb6f-127">Once you have hello **ObjectId** for hello user and a custom role name, you can assign that role toohello user with hello **New-AzureRmRoleAssignment** cmdlet:</span></span>

    PS C:\>New-AzureRmRoleAssignment -ObjectId 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3 -RoleDefinitionName "Policy Contributor" -Scope /subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.DevTestLab/labs/<LabName>/policySets/policies/AllowedVmSizesInLab

<span data-ttu-id="dcb6f-128">Hello előző példában hello **AllowedVmSizesInLab** házirendet használja.</span><span class="sxs-lookup"><span data-stu-id="dcb6f-128">In hello previous example, hello **AllowedVmSizesInLab** policy is used.</span></span> <span data-ttu-id="dcb6f-129">Következő szabályzatok hello bármelyikét használhatja:</span><span class="sxs-lookup"><span data-stu-id="dcb6f-129">You can use any of hello following polices:</span></span>

* <span data-ttu-id="dcb6f-130">MaxVmsAllowedPerUser</span><span class="sxs-lookup"><span data-stu-id="dcb6f-130">MaxVmsAllowedPerUser</span></span>
* <span data-ttu-id="dcb6f-131">MaxVmsAllowedPerLab</span><span class="sxs-lookup"><span data-stu-id="dcb6f-131">MaxVmsAllowedPerLab</span></span>
* <span data-ttu-id="dcb6f-132">AllowedVmSizesInLab</span><span class="sxs-lookup"><span data-stu-id="dcb6f-132">AllowedVmSizesInLab</span></span>
* <span data-ttu-id="dcb6f-133">LabVmsShutdown</span><span class="sxs-lookup"><span data-stu-id="dcb6f-133">LabVmsShutdown</span></span>

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a><span data-ttu-id="dcb6f-134">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="dcb6f-134">Next steps</span></span>
<span data-ttu-id="dcb6f-135">Egyszer Ön már biztosított felhasználói engedélyek toospecific labor házirendek, az alábbiakban néhány következő lépések tooconsider:</span><span class="sxs-lookup"><span data-stu-id="dcb6f-135">Once you've granted user permissions toospecific lab policies, here are some next steps tooconsider:</span></span>

* <span data-ttu-id="dcb6f-136">[Biztonságos hozzáférés tooa labor](devtest-lab-add-devtest-user.md).</span><span class="sxs-lookup"><span data-stu-id="dcb6f-136">[Secure access tooa lab](devtest-lab-add-devtest-user.md).</span></span>
* <span data-ttu-id="dcb6f-137">[Laborházirendek megadása](devtest-lab-set-lab-policy.md).</span><span class="sxs-lookup"><span data-stu-id="dcb6f-137">[Set lab policies](devtest-lab-set-lab-policy.md).</span></span>
* <span data-ttu-id="dcb6f-138">[Laborsablon létrehozása](devtest-lab-create-template.md).</span><span class="sxs-lookup"><span data-stu-id="dcb6f-138">[Create a lab template](devtest-lab-create-template.md).</span></span>
* <span data-ttu-id="dcb6f-139">[Egyéni összetevők létrehozása a virtuális géphez](devtest-lab-artifact-author.md).</span><span class="sxs-lookup"><span data-stu-id="dcb6f-139">[Create custom artifacts for your VMs](devtest-lab-artifact-author.md).</span></span>
* <span data-ttu-id="dcb6f-140">[Adja hozzá a virtuális gép és az összetevők tooa labor](devtest-lab-add-vm-with-artifacts.md).</span><span class="sxs-lookup"><span data-stu-id="dcb6f-140">[Add a VM with artifacts tooa lab](devtest-lab-add-vm-with-artifacts.md).</span></span>

