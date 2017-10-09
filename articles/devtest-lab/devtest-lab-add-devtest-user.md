---
title: "aaaAdd tulajdonosa és a felhasználók a Azure DevTest Labs szolgáltatásban |} Microsoft Docs"
description: "Adja hozzá a tulajdonosok és a felhasználók a Azure DevTest Labs szolgáltatásban hello Azure-portálon vagy a PowerShell használatával"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 4f51d9a5-2702-45f0-a2d5-a3635b58c416
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2017
ms.author: tarcher
ms.openlocfilehash: 2a98f5fe1efbd7c23e0d97f58f47c37462aed3b6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-owners-and-users-in-azure-devtest-labs"></a><span data-ttu-id="2bbac-103">Adja hozzá a tulajdonosok és a felhasználók a Azure DevTest Labs szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="2bbac-103">Add owners and users in Azure DevTest Labs</span></span>
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/How-to-set-security-in-your-DevTest-Lab/player]
> 
> 

<span data-ttu-id="2bbac-104">Hozzáférés az Azure DevTest Labs vezérli [átruházásához hozzáférés-vezérlés (RBAC)](../active-directory/role-based-access-control-what-is.md).</span><span class="sxs-lookup"><span data-stu-id="2bbac-104">Access in Azure DevTest Labs is controlled by [Azure Role-Based Access Control (RBAC)](../active-directory/role-based-access-control-what-is.md).</span></span> <span data-ttu-id="2bbac-105">Szerepalapú használ, akkor is elkülönítse azon belül a csapat a *szerepkörök* ahol számára biztosítson hozzáférést szükséges toousers tooperform csak hello mennyisége a munkájukat.</span><span class="sxs-lookup"><span data-stu-id="2bbac-105">Using RBAC, you can segregate duties within your team into *roles* where you grant only hello amount of access necessary toousers tooperform their jobs.</span></span> <span data-ttu-id="2bbac-106">Közül három ezeket az RBAC-szerepkörök *tulajdonos*, *DevTest Labs felhasználói*, és *közreműködő*.</span><span class="sxs-lookup"><span data-stu-id="2bbac-106">Three of these RBAC roles are *Owner*, *DevTest Labs User*, and *Contributor*.</span></span> <span data-ttu-id="2bbac-107">Ebből a cikkből megismerheti milyen műveletek végezhetők az egyes hello három fő RBAC szerepkört.</span><span class="sxs-lookup"><span data-stu-id="2bbac-107">In this article, you learn what actions can be performed in each of hello three main RBAC roles.</span></span> <span data-ttu-id="2bbac-108">Ott, megismerheti, hogyan tooadd felhasználók tooa lab - mindkét keresztül hello portál és a PowerShell-parancsfájlt, és hogyan tooadd felhasználók hello előfizetés szintjéről.</span><span class="sxs-lookup"><span data-stu-id="2bbac-108">From there, you learn how tooadd users tooa lab - both via hello portal and via a PowerShell script, and how tooadd users at hello subscription level.</span></span>

## <a name="actions-that-can-be-performed-in-each-role"></a><span data-ttu-id="2bbac-109">Az egyes szerepkörökhöz végrehajtható műveletek</span><span class="sxs-lookup"><span data-stu-id="2bbac-109">Actions that can be performed in each role</span></span>
<span data-ttu-id="2bbac-110">Az a felhasználó hozzárendelheti három fő szerepkörök állnak rendelkezésre:</span><span class="sxs-lookup"><span data-stu-id="2bbac-110">There are three main roles that you can assign a user:</span></span>

* <span data-ttu-id="2bbac-111">Tulajdonos</span><span class="sxs-lookup"><span data-stu-id="2bbac-111">Owner</span></span>
* <span data-ttu-id="2bbac-112">DevTest Labs felhasználó</span><span class="sxs-lookup"><span data-stu-id="2bbac-112">DevTest Labs User</span></span>
* <span data-ttu-id="2bbac-113">Közreműködő</span><span class="sxs-lookup"><span data-stu-id="2bbac-113">Contributor</span></span>

<span data-ttu-id="2bbac-114">hello következő táblázat bemutatja, hogy ezek a szerepkörök a felhasználók által végrehajtható műveletek hello:</span><span class="sxs-lookup"><span data-stu-id="2bbac-114">hello following table illustrates hello actions that can be performed by users in each of these roles:</span></span>

| <span data-ttu-id="2bbac-115">**A szerepet betöltő felhasználók műveleteket hajthat végre.**</span><span class="sxs-lookup"><span data-stu-id="2bbac-115">**Actions users in this role can perform**</span></span> | <span data-ttu-id="2bbac-116">**DevTest Labs felhasználó**</span><span class="sxs-lookup"><span data-stu-id="2bbac-116">**DevTest Labs User**</span></span> | <span data-ttu-id="2bbac-117">**Tulajdonos**</span><span class="sxs-lookup"><span data-stu-id="2bbac-117">**Owner**</span></span> | <span data-ttu-id="2bbac-118">**Közreműködő**</span><span class="sxs-lookup"><span data-stu-id="2bbac-118">**Contributor**</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="2bbac-119">**Labor feladatok**</span><span class="sxs-lookup"><span data-stu-id="2bbac-119">**Lab tasks**</span></span> | | | |
| <span data-ttu-id="2bbac-120">Felhasználók tooa labor hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2bbac-120">Add users tooa lab</span></span> |<span data-ttu-id="2bbac-121">Nem</span><span class="sxs-lookup"><span data-stu-id="2bbac-121">No</span></span> |<span data-ttu-id="2bbac-122">Igen</span><span class="sxs-lookup"><span data-stu-id="2bbac-122">Yes</span></span> |<span data-ttu-id="2bbac-123">Nem</span><span class="sxs-lookup"><span data-stu-id="2bbac-123">No</span></span> |
| <span data-ttu-id="2bbac-124">Költség-beállítások frissítése</span><span class="sxs-lookup"><span data-stu-id="2bbac-124">Update cost settings</span></span> |<span data-ttu-id="2bbac-125">Nem</span><span class="sxs-lookup"><span data-stu-id="2bbac-125">No</span></span> |<span data-ttu-id="2bbac-126">Igen</span><span class="sxs-lookup"><span data-stu-id="2bbac-126">Yes</span></span> |<span data-ttu-id="2bbac-127">Igen</span><span class="sxs-lookup"><span data-stu-id="2bbac-127">Yes</span></span> |
| <span data-ttu-id="2bbac-128">**Alapszintű VM-feladatok**</span><span class="sxs-lookup"><span data-stu-id="2bbac-128">**VM base tasks**</span></span> | | | |
| <span data-ttu-id="2bbac-129">Hozzáadhat és eltávolíthat egyéni lemezképek</span><span class="sxs-lookup"><span data-stu-id="2bbac-129">Add and remove custom images</span></span> |<span data-ttu-id="2bbac-130">Nem</span><span class="sxs-lookup"><span data-stu-id="2bbac-130">No</span></span> |<span data-ttu-id="2bbac-131">Igen</span><span class="sxs-lookup"><span data-stu-id="2bbac-131">Yes</span></span> |<span data-ttu-id="2bbac-132">Igen</span><span class="sxs-lookup"><span data-stu-id="2bbac-132">Yes</span></span> |
| <span data-ttu-id="2bbac-133">Hozzáadása, módosítása és törlése képletek</span><span class="sxs-lookup"><span data-stu-id="2bbac-133">Add, update, and delete formulas</span></span> |<span data-ttu-id="2bbac-134">Igen</span><span class="sxs-lookup"><span data-stu-id="2bbac-134">Yes</span></span> |<span data-ttu-id="2bbac-135">Igen</span><span class="sxs-lookup"><span data-stu-id="2bbac-135">Yes</span></span> |<span data-ttu-id="2bbac-136">Igen</span><span class="sxs-lookup"><span data-stu-id="2bbac-136">Yes</span></span> |
| <span data-ttu-id="2bbac-137">Engedélyezett Azure piactéren elérhető rendszerkép</span><span class="sxs-lookup"><span data-stu-id="2bbac-137">Whitelist Azure Marketplace images</span></span> |<span data-ttu-id="2bbac-138">Nem</span><span class="sxs-lookup"><span data-stu-id="2bbac-138">No</span></span> |<span data-ttu-id="2bbac-139">Igen</span><span class="sxs-lookup"><span data-stu-id="2bbac-139">Yes</span></span> |<span data-ttu-id="2bbac-140">Igen</span><span class="sxs-lookup"><span data-stu-id="2bbac-140">Yes</span></span> |
| <span data-ttu-id="2bbac-141">**VM-feladatok**</span><span class="sxs-lookup"><span data-stu-id="2bbac-141">**VM tasks**</span></span> | | | |
| <span data-ttu-id="2bbac-142">Virtuális gépek létrehozása</span><span class="sxs-lookup"><span data-stu-id="2bbac-142">Create VMs</span></span> |<span data-ttu-id="2bbac-143">Igen</span><span class="sxs-lookup"><span data-stu-id="2bbac-143">Yes</span></span> |<span data-ttu-id="2bbac-144">Igen</span><span class="sxs-lookup"><span data-stu-id="2bbac-144">Yes</span></span> |<span data-ttu-id="2bbac-145">Igen</span><span class="sxs-lookup"><span data-stu-id="2bbac-145">Yes</span></span> |
| <span data-ttu-id="2bbac-146">Indítás, Leállítás, és törölje a virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="2bbac-146">Start, stop, and delete VMs</span></span> |<span data-ttu-id="2bbac-147">Csak virtuális gépek hello felhasználó által létrehozott</span><span class="sxs-lookup"><span data-stu-id="2bbac-147">Only VMs created by hello user</span></span> |<span data-ttu-id="2bbac-148">Igen</span><span class="sxs-lookup"><span data-stu-id="2bbac-148">Yes</span></span> |<span data-ttu-id="2bbac-149">Igen</span><span class="sxs-lookup"><span data-stu-id="2bbac-149">Yes</span></span> |
| <span data-ttu-id="2bbac-150">Virtuális gép házirendek frissítése</span><span class="sxs-lookup"><span data-stu-id="2bbac-150">Update VM policies</span></span> |<span data-ttu-id="2bbac-151">Nem</span><span class="sxs-lookup"><span data-stu-id="2bbac-151">No</span></span> |<span data-ttu-id="2bbac-152">Igen</span><span class="sxs-lookup"><span data-stu-id="2bbac-152">Yes</span></span> |<span data-ttu-id="2bbac-153">Igen</span><span class="sxs-lookup"><span data-stu-id="2bbac-153">Yes</span></span> |
| <span data-ttu-id="2bbac-154">Az adatlemezek és a virtuális gépek hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2bbac-154">Add/remove data disks to/from VMs</span></span> |<span data-ttu-id="2bbac-155">Csak virtuális gépek hello felhasználó által létrehozott</span><span class="sxs-lookup"><span data-stu-id="2bbac-155">Only VMs created by hello user</span></span> |<span data-ttu-id="2bbac-156">Igen</span><span class="sxs-lookup"><span data-stu-id="2bbac-156">Yes</span></span> |<span data-ttu-id="2bbac-157">Igen</span><span class="sxs-lookup"><span data-stu-id="2bbac-157">Yes</span></span> |
| <span data-ttu-id="2bbac-158">**Összetevő-feladatok**</span><span class="sxs-lookup"><span data-stu-id="2bbac-158">**Artifact tasks**</span></span> | | | |
| <span data-ttu-id="2bbac-159">Hozzáadhat és eltávolíthat összetevő adattárak</span><span class="sxs-lookup"><span data-stu-id="2bbac-159">Add and remove artifact repositories</span></span> |<span data-ttu-id="2bbac-160">Nem</span><span class="sxs-lookup"><span data-stu-id="2bbac-160">No</span></span> |<span data-ttu-id="2bbac-161">Igen</span><span class="sxs-lookup"><span data-stu-id="2bbac-161">Yes</span></span> |<span data-ttu-id="2bbac-162">Igen</span><span class="sxs-lookup"><span data-stu-id="2bbac-162">Yes</span></span> |
| <span data-ttu-id="2bbac-163">Az összetevők alkalmazása</span><span class="sxs-lookup"><span data-stu-id="2bbac-163">Apply artifacts</span></span> |<span data-ttu-id="2bbac-164">Igen</span><span class="sxs-lookup"><span data-stu-id="2bbac-164">Yes</span></span> |<span data-ttu-id="2bbac-165">Igen</span><span class="sxs-lookup"><span data-stu-id="2bbac-165">Yes</span></span> |<span data-ttu-id="2bbac-166">Igen</span><span class="sxs-lookup"><span data-stu-id="2bbac-166">Yes</span></span> |

> [!NOTE]
> <span data-ttu-id="2bbac-167">Amikor egy felhasználó létrehoz egy virtuális Gépet, a felhasználónak lesz automatikusan hozzárendelve toohello **tulajdonos** létrehozott virtuális gép hello szerepe.</span><span class="sxs-lookup"><span data-stu-id="2bbac-167">When a user creates a VM, that user is automatically assigned toohello **Owner** role of hello created VM.</span></span>
> 
> 

## <a name="add-an-owner-or-user-at-hello-lab-level"></a><span data-ttu-id="2bbac-168">Tulajdonos vagy felhasználó hozzáadása hello labor szinten</span><span class="sxs-lookup"><span data-stu-id="2bbac-168">Add an owner or user at hello lab level</span></span>
<span data-ttu-id="2bbac-169">Tulajdonosa és a felhasználók hello Azure-portálon keresztül hello labor szintjén adhatók meg.</span><span class="sxs-lookup"><span data-stu-id="2bbac-169">Owners and users can be added at hello lab level via hello Azure portal.</span></span> <span data-ttu-id="2bbac-170">Ez magában foglalja a külső felhasználók érvényes [Microsoft-fiók (msa-t)](devtest-lab-faq.md#what-is-a-microsoft-account).</span><span class="sxs-lookup"><span data-stu-id="2bbac-170">This includes external users with a valid [Microsoft account (MSA)](devtest-lab-faq.md#what-is-a-microsoft-account).</span></span>
<span data-ttu-id="2bbac-171">a lépéseket követve hello ismerteti egy tulajdonos vagy a felhasználó tooa labor hozzáadása az Azure DevTest Labs hello folyamatot:</span><span class="sxs-lookup"><span data-stu-id="2bbac-171">hello following steps guide you through hello process of adding an owner or user tooa lab in Azure DevTest Labs:</span></span>

1. <span data-ttu-id="2bbac-172">Jelentkezzen be toohello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="2bbac-172">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
2. <span data-ttu-id="2bbac-173">Válassza ki **további szolgáltatások**, majd válassza ki **DevTest Labs** hello listából.</span><span class="sxs-lookup"><span data-stu-id="2bbac-173">Select **More services**, and then select **DevTest Labs** from hello list.</span></span>
3. <span data-ttu-id="2bbac-174">Labs hello listában jelölje ki hello kívánt labor.</span><span class="sxs-lookup"><span data-stu-id="2bbac-174">From hello list of labs, select hello desired lab.</span></span>
4. <span data-ttu-id="2bbac-175">Hello labor paneljén válassza **konfigurációs**.</span><span class="sxs-lookup"><span data-stu-id="2bbac-175">On hello lab's blade, select **Configuration**.</span></span> 
5. <span data-ttu-id="2bbac-176">A hello **konfigurációs** panelen válassza **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="2bbac-176">On hello **Configuration** blade, select **Users**.</span></span>
6. <span data-ttu-id="2bbac-177">A hello **felhasználók** panelen válassza **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="2bbac-177">On hello **Users** blade, select **+Add**.</span></span>
   
    ![Felhasználó hozzáadása](./media/devtest-lab-add-devtest-user/devtest-users-blade.png)
7. <span data-ttu-id="2bbac-179">A hello **Szerepkörválasztás** panelen, jelölje be hello kívánt szerepkört.</span><span class="sxs-lookup"><span data-stu-id="2bbac-179">On hello **Select a role** blade, select hello desired role.</span></span> <span data-ttu-id="2bbac-180">a szakasz hello [szerepkörönként végrehajtott műveletekből](#actions-that-can-be-performed-in-each-role) listák hello különböző hello tulajdonos, DevTest felhasználói és közreműködő szerepkört felhasználók által végrehajtható műveleteket.</span><span class="sxs-lookup"><span data-stu-id="2bbac-180">hello section [Actions that can be performed in each role](#actions-that-can-be-performed-in-each-role) lists hello various actions that can be performed by users in hello Owner, DevTest User, and Contributor roles.</span></span>
8. <span data-ttu-id="2bbac-181">A hello **felhasználók hozzáadása az** panelen hello e-mail címet, vagy azt szeretné, hogy a megadott hello szerepkör tooadd hello felhasználó nevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="2bbac-181">On hello **Add users** blade, enter hello email address or name of hello user you want tooadd in hello role you specified.</span></span> <span data-ttu-id="2bbac-182">Hello a felhasználó nem található, egy hibaüzenet ismerteti, hogy ha hello probléma.</span><span class="sxs-lookup"><span data-stu-id="2bbac-182">If hello user can't be found, an error message explains hello issue.</span></span> <span data-ttu-id="2bbac-183">Hello felhasználó található, ha, hogy a felhasználó szerepel és van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="2bbac-183">If hello user is found, that user is listed and selected.</span></span> 
9. <span data-ttu-id="2bbac-184">Válassza ki **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="2bbac-184">Select **Select**.</span></span>
10. <span data-ttu-id="2bbac-185">Válassza ki **OK** tooclose hello **hozzáférés hozzáadása** panelen.</span><span class="sxs-lookup"><span data-stu-id="2bbac-185">Select **OK** tooclose hello **Add access** blade.</span></span>
11. <span data-ttu-id="2bbac-186">Amikor visszatér toohello **felhasználók** panelen hello felhasználó hozzá lett adva.</span><span class="sxs-lookup"><span data-stu-id="2bbac-186">When you return toohello **Users** blade, hello user has been added.</span></span>  

## <a name="add-an-external-user-tooa-lab-using-powershell"></a><span data-ttu-id="2bbac-187">Adja hozzá egy külső felhasználó tooa labor PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="2bbac-187">Add an external user tooa lab using PowerShell</span></span>
<span data-ttu-id="2bbac-188">Ezenkívül hello Azure-portálon a felhasználók tooadding, hozzáadhat egy külső felhasználó tooyour, amikor egy PowerShell-parancsfájlt használ.</span><span class="sxs-lookup"><span data-stu-id="2bbac-188">In addition tooadding users in hello Azure portal, you can add an external user tooyour lab using a PowerShell script.</span></span> <span data-ttu-id="2bbac-189">Hello a következő példában egyszerűen módosítsa hello paraméterértékeket a hello **értékek toochange** megjegyzés.</span><span class="sxs-lookup"><span data-stu-id="2bbac-189">In hello following example, simply modify hello parameter values under hello **Values toochange** comment.</span></span>
<span data-ttu-id="2bbac-190">Hello le `subscriptionId`, `labResourceGroup`, és `labName` hello labor panel az Azure-portálon hello értékeit.</span><span class="sxs-lookup"><span data-stu-id="2bbac-190">You can retrieve hello `subscriptionId`, `labResourceGroup`, and `labName` values from hello lab blade in hello Azure portal.</span></span>

> [!NOTE]
> <span data-ttu-id="2bbac-191">hello mintaparancsfájl azt feltételezi, hogy adott hello megadott felhasználó hozzá lett adva, a Vendég toohello Active Directory, és sikertelen lesz, ha a rendszer hello eset.</span><span class="sxs-lookup"><span data-stu-id="2bbac-191">hello sample script assumes that hello specified user has been added as a guest toohello Active Directory, and will fail if that is not hello case.</span></span> <span data-ttu-id="2bbac-192">egy felhasználó nem szerepel a hello Active Directory tooa labor tooadd hello Azure portál tooassign hello felhasználói tooa szerepkört használjon, hello szakaszban ismertetett módon [egy tulajdonos vagy a felhasználó hozzáadása hello labor szinten](#add-an-owner-or-user-at-the-lab-level).</span><span class="sxs-lookup"><span data-stu-id="2bbac-192">tooadd a user not in hello Active Directory tooa lab, use hello Azure portal tooassign hello user tooa role as illustrated in hello section, [Add an owner or user at hello lab level](#add-an-owner-or-user-at-the-lab-level).</span></span>   
> 
> 

    # Add an external user in DevTest Labs user role tooa lab
    # Ensure that guest users can be added toohello Azure Active directory:
    # https://azure.microsoft.com/en-us/documentation/articles/active-directory-create-users/#set-guest-user-access-policies

    # Values toochange
    $subscriptionId = "<Enter Azure subscription ID here>"
    $labResourceGroup = "<Enter lab's resource name here>"
    $labName = "<Enter lab name here>"
    $userDisplayName = "<Enter user's display name here>"

    # Log into your Azure account
    Login-AzureRmAccount

    # Select hello Azure subscription that contains hello lab. 
    # This step is optional if you have only one subscription.
    Select-AzureRmSubscription -SubscriptionId $subscriptionId

    # Retrieve hello user object
    $adObject = Get-AzureRmADUser -SearchString $userDisplayName

    # Create hello role assignment. 
    $labId = ('subscriptions/' + $subscriptionId + '/resourceGroups/' + $labResourceGroup + '/providers/Microsoft.DevTestLab/labs/' + $labName)
    New-AzureRmRoleAssignment -ObjectId $adObject.Id -RoleDefinitionName 'DevTest Labs User' -Scope $labId

## <a name="add-an-owner-or-user-at-hello-subscription-level"></a><span data-ttu-id="2bbac-193">Tulajdonos vagy felhasználó hozzáadása hello előfizetés szintjén</span><span class="sxs-lookup"><span data-stu-id="2bbac-193">Add an owner or user at hello subscription level</span></span>
<span data-ttu-id="2bbac-194">A hatókör toochild szülőhatókörben az Azure-ban Azure engedélyek továbbítja.</span><span class="sxs-lookup"><span data-stu-id="2bbac-194">Azure permissions are propagated from parent scope toochild scope in Azure.</span></span> <span data-ttu-id="2bbac-195">Ezért az Azure-előfizetéssel, amely tartalmazza a labs tulajdonosai a program automatikusan e labs tulajdonosai.</span><span class="sxs-lookup"><span data-stu-id="2bbac-195">Therefore, owners of an Azure subscription that contains labs are automatically owners of those labs.</span></span> <span data-ttu-id="2bbac-196">Emellett saját hello virtuális gépek és egyéb erőforrások hello labor felhasználók és hello Azure DevTest Labs szolgáltatás által létrehozott.</span><span class="sxs-lookup"><span data-stu-id="2bbac-196">They also own hello VMs and other resources created by hello lab's users, and hello Azure DevTest Labs service.</span></span> 

<span data-ttu-id="2bbac-197">Hello adhat hozzá további tulajdonosok tooa labor keresztül hello labor panel [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="2bbac-197">You can add additional owners tooa lab via hello lab's blade in hello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span> <span data-ttu-id="2bbac-198">Azonban hello hozzáadott, a tulajdonos felügyeleti hatóköre sokkal szűkebb hello előfizetés tulajdonosa hatókör-nál.</span><span class="sxs-lookup"><span data-stu-id="2bbac-198">However, hello added owner's scope of administration is more narrow than hello subscription owner's scope.</span></span> <span data-ttu-id="2bbac-199">Például hello hozzáadott tulajdonosok nem rendelkezik teljes körű hozzáférési toosome hello erőforrások hello DevTest Labs szolgáltatás által létrehozott hello előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="2bbac-199">For example, hello added owners do not have full access toosome of hello resources that are created in hello subscription by hello DevTest Labs service.</span></span> 

<span data-ttu-id="2bbac-200">tooadd egy tulajdonos tooan Azure-előfizetésre, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="2bbac-200">tooadd an owner tooan Azure subscription, follow these steps:</span></span>

1. <span data-ttu-id="2bbac-201">Jelentkezzen be toohello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="2bbac-201">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
2. <span data-ttu-id="2bbac-202">Válassza ki **több szolgáltatások**, majd válassza ki **előfizetések** hello listából.</span><span class="sxs-lookup"><span data-stu-id="2bbac-202">Select **More Services**, and then select **Subscriptions** from hello list.</span></span>
3. <span data-ttu-id="2bbac-203">Válassza ki a kívánt hello előfizetést.</span><span class="sxs-lookup"><span data-stu-id="2bbac-203">Select hello desired subscription.</span></span>
4. <span data-ttu-id="2bbac-204">Válassza ki **hozzáférés** ikonra.</span><span class="sxs-lookup"><span data-stu-id="2bbac-204">Select **Access** icon.</span></span> 
   
    ![Használó felhasználók](./media/devtest-lab-add-devtest-user/access-users.png)
5. <span data-ttu-id="2bbac-206">A hello **felhasználók** panelen válassza **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="2bbac-206">On hello **Users** blade, select **Add**.</span></span>
   
    ![Felhasználó hozzáadása](./media/devtest-lab-add-devtest-user/devtest-users-blade.png)
6. <span data-ttu-id="2bbac-208">A hello **Szerepkörválasztás** panelen válasszon ki **tulajdonos**.</span><span class="sxs-lookup"><span data-stu-id="2bbac-208">On hello **Select a role** blade, select **Owner**.</span></span>
7. <span data-ttu-id="2bbac-209">A hello **felhasználók hozzáadása** panelen adja meg az üdvözlő e-mail cím vagy név hello felhasználó tulajdonos tooadd kívánt.</span><span class="sxs-lookup"><span data-stu-id="2bbac-209">On hello **Add users** blade, enter hello email address or name of hello user you want tooadd as an owner.</span></span> <span data-ttu-id="2bbac-210">Ha hello a felhasználó nem található, hello probléma ismertető hibaüzenetet kap.</span><span class="sxs-lookup"><span data-stu-id="2bbac-210">If hello user can't be found, you get an error message explaining hello issue.</span></span> <span data-ttu-id="2bbac-211">Hello felhasználó található, ha a felhasználó megtalálható-e e hello **felhasználói** szövegmezőben.</span><span class="sxs-lookup"><span data-stu-id="2bbac-211">If hello user is found, that user is listed under hello **User** text box.</span></span>
8. <span data-ttu-id="2bbac-212">Hello található felhasználónév kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="2bbac-212">Select hello located user name.</span></span>
9. <span data-ttu-id="2bbac-213">Válassza ki **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="2bbac-213">Select **Select**.</span></span>
10. <span data-ttu-id="2bbac-214">Válassza ki **OK** tooclose hello **hozzáférés hozzáadása** panelen.</span><span class="sxs-lookup"><span data-stu-id="2bbac-214">Select **OK** tooclose hello **Add access** blade.</span></span>
11. <span data-ttu-id="2bbac-215">Amikor visszatér toohello **felhasználók** panelen hello felhasználó tulajdonos hozzáadva.</span><span class="sxs-lookup"><span data-stu-id="2bbac-215">When you return toohello **Users** blade, hello user has been added as an owner.</span></span> <span data-ttu-id="2bbac-216">Ez a felhasználó már létrehozott az előfizetéshez tartozó bármely labs tulajdonosa, és így képes tooperform tulajdonos feladatok.</span><span class="sxs-lookup"><span data-stu-id="2bbac-216">This user is now an owner of any labs created under this subscription, and thus be able tooperform owner tasks.</span></span> 

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

