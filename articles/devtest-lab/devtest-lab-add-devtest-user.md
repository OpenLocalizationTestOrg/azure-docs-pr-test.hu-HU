---
title: "Adja hozzá a tulajdonosok és a felhasználók a Azure DevTest Labs szolgáltatásban |} Microsoft Docs"
description: "Adja hozzá a tulajdonosok és a felhasználók a Azure DevTest Labs szolgáltatásban az Azure-portálon vagy a PowerShell használatával"
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
ms.openlocfilehash: d67fa257574d6cb4ad4b18521900374fb51da290
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="add-owners-and-users-in-azure-devtest-labs"></a><span data-ttu-id="20bc7-103">Adja hozzá a tulajdonosok és a felhasználók a Azure DevTest Labs szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="20bc7-103">Add owners and users in Azure DevTest Labs</span></span>
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/How-to-set-security-in-your-DevTest-Lab/player]
> 
> 

<span data-ttu-id="20bc7-104">Hozzáférés az Azure DevTest Labs vezérli [átruházásához hozzáférés-vezérlés (RBAC)](../active-directory/role-based-access-control-what-is.md).</span><span class="sxs-lookup"><span data-stu-id="20bc7-104">Access in Azure DevTest Labs is controlled by [Azure Role-Based Access Control (RBAC)](../active-directory/role-based-access-control-what-is.md).</span></span> <span data-ttu-id="20bc7-105">Szerepalapú használ, akkor is elkülönítse azon belül a csapat a *szerepkörök* ahol engedélyezheti, hogy csak olyan mértékű hozzáférést a felhasználóknak a feladataik elvégzéséhez szükséges.</span><span class="sxs-lookup"><span data-stu-id="20bc7-105">Using RBAC, you can segregate duties within your team into *roles* where you grant only the amount of access necessary to users to perform their jobs.</span></span> <span data-ttu-id="20bc7-106">Közül három ezeket az RBAC-szerepkörök *tulajdonos*, *DevTest Labs felhasználói*, és *közreműködő*.</span><span class="sxs-lookup"><span data-stu-id="20bc7-106">Three of these RBAC roles are *Owner*, *DevTest Labs User*, and *Contributor*.</span></span> <span data-ttu-id="20bc7-107">Ebből a cikkből megismerheti milyen műveletek is elvégezhetők a három fő RBAC-szerepkörök.</span><span class="sxs-lookup"><span data-stu-id="20bc7-107">In this article, you learn what actions can be performed in each of the three main RBAC roles.</span></span> <span data-ttu-id="20bc7-108">Ott megismerheti a felhasználók felvétele egy tesztlabor - mind a portálon keresztül egy PowerShell-parancsfájlt, és felhasználók hozzáadása az előfizetés szintjén.</span><span class="sxs-lookup"><span data-stu-id="20bc7-108">From there, you learn how to add users to a lab - both via the portal and via a PowerShell script, and how to add users at the subscription level.</span></span>

## <a name="actions-that-can-be-performed-in-each-role"></a><span data-ttu-id="20bc7-109">Az egyes szerepkörökhöz végrehajtható műveletek</span><span class="sxs-lookup"><span data-stu-id="20bc7-109">Actions that can be performed in each role</span></span>
<span data-ttu-id="20bc7-110">Az a felhasználó hozzárendelheti három fő szerepkörök állnak rendelkezésre:</span><span class="sxs-lookup"><span data-stu-id="20bc7-110">There are three main roles that you can assign a user:</span></span>

* <span data-ttu-id="20bc7-111">Tulajdonos</span><span class="sxs-lookup"><span data-stu-id="20bc7-111">Owner</span></span>
* <span data-ttu-id="20bc7-112">DevTest Labs felhasználó</span><span class="sxs-lookup"><span data-stu-id="20bc7-112">DevTest Labs User</span></span>
* <span data-ttu-id="20bc7-113">Közreműködő</span><span class="sxs-lookup"><span data-stu-id="20bc7-113">Contributor</span></span>

<span data-ttu-id="20bc7-114">A következő táblázat bemutatja, ezek a szerepkörök a felhasználók által végrehajtható műveleteket:</span><span class="sxs-lookup"><span data-stu-id="20bc7-114">The following table illustrates the actions that can be performed by users in each of these roles:</span></span>

| <span data-ttu-id="20bc7-115">**A szerepet betöltő felhasználók műveleteket hajthat végre.**</span><span class="sxs-lookup"><span data-stu-id="20bc7-115">**Actions users in this role can perform**</span></span> | <span data-ttu-id="20bc7-116">**DevTest Labs felhasználó**</span><span class="sxs-lookup"><span data-stu-id="20bc7-116">**DevTest Labs User**</span></span> | <span data-ttu-id="20bc7-117">**Tulajdonos**</span><span class="sxs-lookup"><span data-stu-id="20bc7-117">**Owner**</span></span> | <span data-ttu-id="20bc7-118">**Közreműködő**</span><span class="sxs-lookup"><span data-stu-id="20bc7-118">**Contributor**</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="20bc7-119">**Labor feladatok**</span><span class="sxs-lookup"><span data-stu-id="20bc7-119">**Lab tasks**</span></span> | | | |
| <span data-ttu-id="20bc7-120">Felhasználók hozzáadása egy laborhoz</span><span class="sxs-lookup"><span data-stu-id="20bc7-120">Add users to a lab</span></span> |<span data-ttu-id="20bc7-121">Nem</span><span class="sxs-lookup"><span data-stu-id="20bc7-121">No</span></span> |<span data-ttu-id="20bc7-122">Igen</span><span class="sxs-lookup"><span data-stu-id="20bc7-122">Yes</span></span> |<span data-ttu-id="20bc7-123">Nem</span><span class="sxs-lookup"><span data-stu-id="20bc7-123">No</span></span> |
| <span data-ttu-id="20bc7-124">Költség-beállítások frissítése</span><span class="sxs-lookup"><span data-stu-id="20bc7-124">Update cost settings</span></span> |<span data-ttu-id="20bc7-125">Nem</span><span class="sxs-lookup"><span data-stu-id="20bc7-125">No</span></span> |<span data-ttu-id="20bc7-126">Igen</span><span class="sxs-lookup"><span data-stu-id="20bc7-126">Yes</span></span> |<span data-ttu-id="20bc7-127">Igen</span><span class="sxs-lookup"><span data-stu-id="20bc7-127">Yes</span></span> |
| <span data-ttu-id="20bc7-128">**Alapszintű VM-feladatok**</span><span class="sxs-lookup"><span data-stu-id="20bc7-128">**VM base tasks**</span></span> | | | |
| <span data-ttu-id="20bc7-129">Hozzáadhat és eltávolíthat egyéni lemezképek</span><span class="sxs-lookup"><span data-stu-id="20bc7-129">Add and remove custom images</span></span> |<span data-ttu-id="20bc7-130">Nem</span><span class="sxs-lookup"><span data-stu-id="20bc7-130">No</span></span> |<span data-ttu-id="20bc7-131">Igen</span><span class="sxs-lookup"><span data-stu-id="20bc7-131">Yes</span></span> |<span data-ttu-id="20bc7-132">Igen</span><span class="sxs-lookup"><span data-stu-id="20bc7-132">Yes</span></span> |
| <span data-ttu-id="20bc7-133">Hozzáadása, módosítása és törlése képletek</span><span class="sxs-lookup"><span data-stu-id="20bc7-133">Add, update, and delete formulas</span></span> |<span data-ttu-id="20bc7-134">Igen</span><span class="sxs-lookup"><span data-stu-id="20bc7-134">Yes</span></span> |<span data-ttu-id="20bc7-135">Igen</span><span class="sxs-lookup"><span data-stu-id="20bc7-135">Yes</span></span> |<span data-ttu-id="20bc7-136">Igen</span><span class="sxs-lookup"><span data-stu-id="20bc7-136">Yes</span></span> |
| <span data-ttu-id="20bc7-137">Engedélyezett Azure piactéren elérhető rendszerkép</span><span class="sxs-lookup"><span data-stu-id="20bc7-137">Whitelist Azure Marketplace images</span></span> |<span data-ttu-id="20bc7-138">Nem</span><span class="sxs-lookup"><span data-stu-id="20bc7-138">No</span></span> |<span data-ttu-id="20bc7-139">Igen</span><span class="sxs-lookup"><span data-stu-id="20bc7-139">Yes</span></span> |<span data-ttu-id="20bc7-140">Igen</span><span class="sxs-lookup"><span data-stu-id="20bc7-140">Yes</span></span> |
| <span data-ttu-id="20bc7-141">**VM-feladatok**</span><span class="sxs-lookup"><span data-stu-id="20bc7-141">**VM tasks**</span></span> | | | |
| <span data-ttu-id="20bc7-142">Virtuális gépek létrehozása</span><span class="sxs-lookup"><span data-stu-id="20bc7-142">Create VMs</span></span> |<span data-ttu-id="20bc7-143">Igen</span><span class="sxs-lookup"><span data-stu-id="20bc7-143">Yes</span></span> |<span data-ttu-id="20bc7-144">Igen</span><span class="sxs-lookup"><span data-stu-id="20bc7-144">Yes</span></span> |<span data-ttu-id="20bc7-145">Igen</span><span class="sxs-lookup"><span data-stu-id="20bc7-145">Yes</span></span> |
| <span data-ttu-id="20bc7-146">Indítás, Leállítás, és törölje a virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="20bc7-146">Start, stop, and delete VMs</span></span> |<span data-ttu-id="20bc7-147">Csak virtuális gépek a felhasználó által létrehozott</span><span class="sxs-lookup"><span data-stu-id="20bc7-147">Only VMs created by the user</span></span> |<span data-ttu-id="20bc7-148">Igen</span><span class="sxs-lookup"><span data-stu-id="20bc7-148">Yes</span></span> |<span data-ttu-id="20bc7-149">Igen</span><span class="sxs-lookup"><span data-stu-id="20bc7-149">Yes</span></span> |
| <span data-ttu-id="20bc7-150">Virtuális gép házirendek frissítése</span><span class="sxs-lookup"><span data-stu-id="20bc7-150">Update VM policies</span></span> |<span data-ttu-id="20bc7-151">Nem</span><span class="sxs-lookup"><span data-stu-id="20bc7-151">No</span></span> |<span data-ttu-id="20bc7-152">Igen</span><span class="sxs-lookup"><span data-stu-id="20bc7-152">Yes</span></span> |<span data-ttu-id="20bc7-153">Igen</span><span class="sxs-lookup"><span data-stu-id="20bc7-153">Yes</span></span> |
| <span data-ttu-id="20bc7-154">Az adatlemezek és a virtuális gépek hozzáadása</span><span class="sxs-lookup"><span data-stu-id="20bc7-154">Add/remove data disks to/from VMs</span></span> |<span data-ttu-id="20bc7-155">Csak virtuális gépek a felhasználó által létrehozott</span><span class="sxs-lookup"><span data-stu-id="20bc7-155">Only VMs created by the user</span></span> |<span data-ttu-id="20bc7-156">Igen</span><span class="sxs-lookup"><span data-stu-id="20bc7-156">Yes</span></span> |<span data-ttu-id="20bc7-157">Igen</span><span class="sxs-lookup"><span data-stu-id="20bc7-157">Yes</span></span> |
| <span data-ttu-id="20bc7-158">**Összetevő-feladatok**</span><span class="sxs-lookup"><span data-stu-id="20bc7-158">**Artifact tasks**</span></span> | | | |
| <span data-ttu-id="20bc7-159">Hozzáadhat és eltávolíthat összetevő adattárak</span><span class="sxs-lookup"><span data-stu-id="20bc7-159">Add and remove artifact repositories</span></span> |<span data-ttu-id="20bc7-160">Nem</span><span class="sxs-lookup"><span data-stu-id="20bc7-160">No</span></span> |<span data-ttu-id="20bc7-161">Igen</span><span class="sxs-lookup"><span data-stu-id="20bc7-161">Yes</span></span> |<span data-ttu-id="20bc7-162">Igen</span><span class="sxs-lookup"><span data-stu-id="20bc7-162">Yes</span></span> |
| <span data-ttu-id="20bc7-163">Az összetevők alkalmazása</span><span class="sxs-lookup"><span data-stu-id="20bc7-163">Apply artifacts</span></span> |<span data-ttu-id="20bc7-164">Igen</span><span class="sxs-lookup"><span data-stu-id="20bc7-164">Yes</span></span> |<span data-ttu-id="20bc7-165">Igen</span><span class="sxs-lookup"><span data-stu-id="20bc7-165">Yes</span></span> |<span data-ttu-id="20bc7-166">Igen</span><span class="sxs-lookup"><span data-stu-id="20bc7-166">Yes</span></span> |

> [!NOTE]
> <span data-ttu-id="20bc7-167">Amikor egy felhasználó létrehoz egy virtuális Gépet, a rendszer automatikusan hozzárendeli, hogy a felhasználó a **tulajdonos** a létrehozott virtuális gép szerepkör.</span><span class="sxs-lookup"><span data-stu-id="20bc7-167">When a user creates a VM, that user is automatically assigned to the **Owner** role of the created VM.</span></span>
> 
> 

## <a name="add-an-owner-or-user-at-the-lab-level"></a><span data-ttu-id="20bc7-168">A tesztkörnyezet szinten egy tulajdonos vagy a felhasználó hozzáadása</span><span class="sxs-lookup"><span data-stu-id="20bc7-168">Add an owner or user at the lab level</span></span>
<span data-ttu-id="20bc7-169">Tulajdonosa és a felhasználók az Azure-portálon a labor szintjén adhatók meg.</span><span class="sxs-lookup"><span data-stu-id="20bc7-169">Owners and users can be added at the lab level via the Azure portal.</span></span> <span data-ttu-id="20bc7-170">Ez magában foglalja a külső felhasználók érvényes [Microsoft-fiók (msa-t)](devtest-lab-faq.md#what-is-a-microsoft-account).</span><span class="sxs-lookup"><span data-stu-id="20bc7-170">This includes external users with a valid [Microsoft account (MSA)](devtest-lab-faq.md#what-is-a-microsoft-account).</span></span>
<span data-ttu-id="20bc7-171">A következő lépések végigvezetik egy tulajdonos vagy a felhasználó hozzáadása egy laborhoz a Azure DevTest Labs szolgáltatásban:</span><span class="sxs-lookup"><span data-stu-id="20bc7-171">The following steps guide you through the process of adding an owner or user to a lab in Azure DevTest Labs:</span></span>

1. <span data-ttu-id="20bc7-172">Jelentkezzen be az [Azure Portalra](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="20bc7-172">Sign in to the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
2. <span data-ttu-id="20bc7-173">Válassza a **További szolgáltatások**, majd a **DevTest Labs** elemet a listából.</span><span class="sxs-lookup"><span data-stu-id="20bc7-173">Select **More services**, and then select **DevTest Labs** from the list.</span></span>
3. <span data-ttu-id="20bc7-174">Válassza ki a kívánt labor labs listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="20bc7-174">From the list of labs, select the desired lab.</span></span>
4. <span data-ttu-id="20bc7-175">A labor paneljén válassza **konfigurációs**.</span><span class="sxs-lookup"><span data-stu-id="20bc7-175">On the lab's blade, select **Configuration**.</span></span> 
5. <span data-ttu-id="20bc7-176">Az a **konfigurációs** panelen válassza **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="20bc7-176">On the **Configuration** blade, select **Users**.</span></span>
6. <span data-ttu-id="20bc7-177">Az a **felhasználók** panelen válassza **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="20bc7-177">On the **Users** blade, select **+Add**.</span></span>
   
    ![Felhasználó hozzáadása](./media/devtest-lab-add-devtest-user/devtest-users-blade.png)
7. <span data-ttu-id="20bc7-179">Az a **Szerepkörválasztás** panelen válassza ki a kívánt szerepkört.</span><span class="sxs-lookup"><span data-stu-id="20bc7-179">On the **Select a role** blade, select the desired role.</span></span> <span data-ttu-id="20bc7-180">A szakasz [szerepkörönként végrehajtott műveletekből](#actions-that-can-be-performed-in-each-role) felsorolja azokat a különböző, a tulajdonosa, a DevTest felhasználói és a közreműködői szerepkör a felhasználók által végrehajtható műveleteket.</span><span class="sxs-lookup"><span data-stu-id="20bc7-180">The section [Actions that can be performed in each role](#actions-that-can-be-performed-in-each-role) lists the various actions that can be performed by users in the Owner, DevTest User, and Contributor roles.</span></span>
8. <span data-ttu-id="20bc7-181">Az a **felhasználók hozzáadása az** panelen adja meg az e-mail cím vagy a felhasználó a szerepkörben megadott hozzáadni kívánt nevét.</span><span class="sxs-lookup"><span data-stu-id="20bc7-181">On the **Add users** blade, enter the email address or name of the user you want to add in the role you specified.</span></span> <span data-ttu-id="20bc7-182">Ha a felhasználó nem található, egy hibaüzenet ismerteti a problémát.</span><span class="sxs-lookup"><span data-stu-id="20bc7-182">If the user can't be found, an error message explains the issue.</span></span> <span data-ttu-id="20bc7-183">Ha a felhasználó megtalálható, hogy a felhasználó a felsorolt és kiválasztott.</span><span class="sxs-lookup"><span data-stu-id="20bc7-183">If the user is found, that user is listed and selected.</span></span> 
9. <span data-ttu-id="20bc7-184">Válassza ki **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="20bc7-184">Select **Select**.</span></span>
10. <span data-ttu-id="20bc7-185">Válassza ki **OK** bezárásához a **hozzáférés hozzáadása** panelen.</span><span class="sxs-lookup"><span data-stu-id="20bc7-185">Select **OK** to close the **Add access** blade.</span></span>
11. <span data-ttu-id="20bc7-186">Amikor visszatér a **felhasználók** panelen, a felhasználó hozzá lett adva.</span><span class="sxs-lookup"><span data-stu-id="20bc7-186">When you return to the **Users** blade, the user has been added.</span></span>  

## <a name="add-an-external-user-to-a-lab-using-powershell"></a><span data-ttu-id="20bc7-187">Külső felhasználó hozzáadása egy laborhoz PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="20bc7-187">Add an external user to a lab using PowerShell</span></span>
<span data-ttu-id="20bc7-188">Mellett felhasználók hozzáadása az Azure portálon, a külső felhasználó adhat hozzá a labor egy PowerShell-parancsfájl használatával.</span><span class="sxs-lookup"><span data-stu-id="20bc7-188">In addition to adding users in the Azure portal, you can add an external user to your lab using a PowerShell script.</span></span> <span data-ttu-id="20bc7-189">A következő példában a paraméterértékeket a módosításához egyszerűen a **értékek módosítása** megjegyzés.</span><span class="sxs-lookup"><span data-stu-id="20bc7-189">In the following example, simply modify the parameter values under the **Values to change** comment.</span></span>
<span data-ttu-id="20bc7-190">Kérheti le a `subscriptionId`, `labResourceGroup`, és `labName` értékeit a labor panel az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="20bc7-190">You can retrieve the `subscriptionId`, `labResourceGroup`, and `labName` values from the lab blade in the Azure portal.</span></span>

> [!NOTE]
> <span data-ttu-id="20bc7-191">A parancsfájlpéldát feltételezi, hogy a megadott felhasználó az Active Directory vendégként hozzáadva, és sikertelen lesz, ha a feltételek nem teljesülnek.</span><span class="sxs-lookup"><span data-stu-id="20bc7-191">The sample script assumes that the specified user has been added as a guest to the Active Directory, and will fail if that is not the case.</span></span> <span data-ttu-id="20bc7-192">A felhasználó nem szerepel az Active Directory hozzáadása egy tesztkörnyezetet, az Azure-portál használatával rendelje hozzá a felhasználót a szerepkörhöz a szakaszban ismertetett módon [egy tulajdonos vagy a felhasználó hozzáadása a labor szinten](#add-an-owner-or-user-at-the-lab-level).</span><span class="sxs-lookup"><span data-stu-id="20bc7-192">To add a user not in the Active Directory to a lab, use the Azure portal to assign the user to a role as illustrated in the section, [Add an owner or user at the lab level](#add-an-owner-or-user-at-the-lab-level).</span></span>   
> 
> 

    # Add an external user in DevTest Labs user role to a lab
    # Ensure that guest users can be added to the Azure Active directory:
    # https://azure.microsoft.com/en-us/documentation/articles/active-directory-create-users/#set-guest-user-access-policies

    # Values to change
    $subscriptionId = "<Enter Azure subscription ID here>"
    $labResourceGroup = "<Enter lab's resource name here>"
    $labName = "<Enter lab name here>"
    $userDisplayName = "<Enter user's display name here>"

    # Log into your Azure account
    Login-AzureRmAccount

    # Select the Azure subscription that contains the lab. 
    # This step is optional if you have only one subscription.
    Select-AzureRmSubscription -SubscriptionId $subscriptionId

    # Retrieve the user object
    $adObject = Get-AzureRmADUser -SearchString $userDisplayName

    # Create the role assignment. 
    $labId = ('subscriptions/' + $subscriptionId + '/resourceGroups/' + $labResourceGroup + '/providers/Microsoft.DevTestLab/labs/' + $labName)
    New-AzureRmRoleAssignment -ObjectId $adObject.Id -RoleDefinitionName 'DevTest Labs User' -Scope $labId

## <a name="add-an-owner-or-user-at-the-subscription-level"></a><span data-ttu-id="20bc7-193">Az előfizetés szintjén egy tulajdonos vagy a felhasználó hozzáadása</span><span class="sxs-lookup"><span data-stu-id="20bc7-193">Add an owner or user at the subscription level</span></span>
<span data-ttu-id="20bc7-194">Az Azure engedélyek rendszer nem propagál származik gyermek hatókörben az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="20bc7-194">Azure permissions are propagated from parent scope to child scope in Azure.</span></span> <span data-ttu-id="20bc7-195">Ezért az Azure-előfizetéssel, amely tartalmazza a labs tulajdonosai a program automatikusan e labs tulajdonosai.</span><span class="sxs-lookup"><span data-stu-id="20bc7-195">Therefore, owners of an Azure subscription that contains labs are automatically owners of those labs.</span></span> <span data-ttu-id="20bc7-196">A virtuális gépek és egyéb erőforrások a labor felhasználók és az Azure DevTest Labs szolgáltatás által létrehozott is saját.</span><span class="sxs-lookup"><span data-stu-id="20bc7-196">They also own the VMs and other resources created by the lab's users, and the Azure DevTest Labs service.</span></span> 

<span data-ttu-id="20bc7-197">A labor panel az keresztül labor adhat hozzá további tulajdonosai a [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="20bc7-197">You can add additional owners to a lab via the lab's blade in the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span> <span data-ttu-id="20bc7-198">Azonban a hozzáadott tulajdonos felügyeleti hatóköre sokkal szűkebb, mint az előfizetés tulajdonosa hatókör.</span><span class="sxs-lookup"><span data-stu-id="20bc7-198">However, the added owner's scope of administration is more narrow than the subscription owner's scope.</span></span> <span data-ttu-id="20bc7-199">A hozzáadott tulajdonosai például nem rendelkezik teljes körű hozzáférés az egyes erőforrásokhoz az előfizetést a DevTest Labs szolgáltatás által létrehozott.</span><span class="sxs-lookup"><span data-stu-id="20bc7-199">For example, the added owners do not have full access to some of the resources that are created in the subscription by the DevTest Labs service.</span></span> 

<span data-ttu-id="20bc7-200">Tulajdonosa hozzá Azure-előfizetéssel, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="20bc7-200">To add an owner to an Azure subscription, follow these steps:</span></span>

1. <span data-ttu-id="20bc7-201">Jelentkezzen be az [Azure Portalra](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="20bc7-201">Sign in to the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
2. <span data-ttu-id="20bc7-202">Válassza ki **több szolgáltatások**, majd válassza ki **előfizetések** a listából.</span><span class="sxs-lookup"><span data-stu-id="20bc7-202">Select **More Services**, and then select **Subscriptions** from the list.</span></span>
3. <span data-ttu-id="20bc7-203">Válassza ki a kívánt előfizetést.</span><span class="sxs-lookup"><span data-stu-id="20bc7-203">Select the desired subscription.</span></span>
4. <span data-ttu-id="20bc7-204">Válassza ki **hozzáférés** ikonra.</span><span class="sxs-lookup"><span data-stu-id="20bc7-204">Select **Access** icon.</span></span> 
   
    ![Használó felhasználók](./media/devtest-lab-add-devtest-user/access-users.png)
5. <span data-ttu-id="20bc7-206">Az a **felhasználók** panelen válassza **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="20bc7-206">On the **Users** blade, select **Add**.</span></span>
   
    ![Felhasználó hozzáadása](./media/devtest-lab-add-devtest-user/devtest-users-blade.png)
6. <span data-ttu-id="20bc7-208">Az a **Szerepkörválasztás** panelen válasszon ki **tulajdonos**.</span><span class="sxs-lookup"><span data-stu-id="20bc7-208">On the **Select a role** blade, select **Owner**.</span></span>
7. <span data-ttu-id="20bc7-209">Az a **felhasználók hozzáadása az** panelen adja meg az e-mail cím vagy a tulajdonos hozzáadni kívánt felhasználó nevét.</span><span class="sxs-lookup"><span data-stu-id="20bc7-209">On the **Add users** blade, enter the email address or name of the user you want to add as an owner.</span></span> <span data-ttu-id="20bc7-210">Ha a felhasználó nem található, kap olyan hibaüzenetet, amely ismerteti, hogy a problémát.</span><span class="sxs-lookup"><span data-stu-id="20bc7-210">If the user can't be found, you get an error message explaining the issue.</span></span> <span data-ttu-id="20bc7-211">Ha a felhasználó megtalálható, a felhasználó megtalálható-e a **felhasználói** szövegmezőben.</span><span class="sxs-lookup"><span data-stu-id="20bc7-211">If the user is found, that user is listed under the **User** text box.</span></span>
8. <span data-ttu-id="20bc7-212">Válassza ki a található felhasználónév.</span><span class="sxs-lookup"><span data-stu-id="20bc7-212">Select the located user name.</span></span>
9. <span data-ttu-id="20bc7-213">Válassza ki **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="20bc7-213">Select **Select**.</span></span>
10. <span data-ttu-id="20bc7-214">Válassza ki **OK** bezárásához a **hozzáférés hozzáadása** panelen.</span><span class="sxs-lookup"><span data-stu-id="20bc7-214">Select **OK** to close the **Add access** blade.</span></span>
11. <span data-ttu-id="20bc7-215">Amikor visszatér a **felhasználók** panelen, a felhasználó tulajdonos hozzáadva.</span><span class="sxs-lookup"><span data-stu-id="20bc7-215">When you return to the **Users** blade, the user has been added as an owner.</span></span> <span data-ttu-id="20bc7-216">Ez a felhasználó már létrehozott az előfizetéshez tartozó bármely labs tulajdonosa, és így lesz tulajdonos feladatok elvégzéséhez.</span><span class="sxs-lookup"><span data-stu-id="20bc7-216">This user is now an owner of any labs created under this subscription, and thus be able to perform owner tasks.</span></span> 

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

