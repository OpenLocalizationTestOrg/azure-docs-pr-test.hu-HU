---
title: "Azure szerepköralapú hozzáférés-vezérlés (RBAC) való hozzáférési jogosultságainak vezérlése létrehozásához és kezeléséhez a támogatási kérelmek |} Microsoft Docs"
description: "Azure szerepköralapú hozzáférés-vezérlés (RBAC) való hozzáférési jogosultságainak vezérlése létrehozásához és kezeléséhez a támogatási kérelmek"
author: ganganarayanan
ms.author: gangan
ms.date: 1/31/2017
ms.topic: article
ms.service: microsoft-docs
ms.assetid: 58a0ca9d-86d2-469a-9714-3b8320c33cf5
ms.openlocfilehash: 20ebd324cbf379980b43d255d468673de2b6d950
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-role-based-access-control-rbac-to-control-access-rights-to-create-and-manage-support-requests"></a><span data-ttu-id="1e996-103">Azure szerepköralapú hozzáférés-vezérlés (RBAC) való hozzáférési jogosultságainak vezérlése létrehozásához és kezeléséhez a támogatási kérelmek</span><span class="sxs-lookup"><span data-stu-id="1e996-103">Azure Role-Based Access Control (RBAC) to control access rights to create and manage support requests</span></span>

<span data-ttu-id="1e996-104">[Szerepköralapú hozzáférés-vezérlés (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is) lehetővé teszi a részletes hozzáféréskezelést az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="1e996-104">[Role-Based Access Control (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is) enables fine-grained access management for Azure.</span></span>
<span data-ttu-id="1e996-105">Támogatja a kérelem létrehozását az Azure-portálon [portal.azure.com](https://portal.azure.com), határozza meg, akik hozhat létre és támogatási kérelmek kezelése az Azure RBAC-modellben használatával.</span><span class="sxs-lookup"><span data-stu-id="1e996-105">Support request creation in the Azure portal, [portal.azure.com](https://portal.azure.com), uses Azure’s RBAC model to define who can create and manage support requests.</span></span>
<span data-ttu-id="1e996-106">Hozzáférés a megfelelő RBAC szerepkört rendel a felhasználók, csoportok és alkalmazások egy bizonyos hatókörhöz, amely lehet egy előfizetést, erőforráscsoport vagy egy erőforrás.</span><span class="sxs-lookup"><span data-stu-id="1e996-106">Access is granted by assigning the appropriate RBAC role to users, groups, and applications at a certain scope, which can be a subscription, resource group or a resource.</span></span>

<span data-ttu-id="1e996-107">Vegyünk egy példát: egy erőforrás csoport tulajdonosának olvasási engedély az előfizetés hatókörben, kezelheti az erőforráscsoportot, például webhelyekhez, virtuális gépek és alhálózatok tartozó összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="1e996-107">Let’s take an example: As a resource group owner with read permissions at the subscription scope, you can manage all the resources under the resource group, like websites, virtual machines, and subnets.</span></span>
<span data-ttu-id="1e996-108">Azonban amikor megpróbál létrehozni egy támogatási kérést, szemben a virtuálisgép-erőforrást, tapasztal a következő hiba</span><span class="sxs-lookup"><span data-stu-id="1e996-108">However, when you try to create a support request against the virtual machine resource, you encounter the following error</span></span>

![Előfizetés hiba](./media/create-manage-support-requests-using-access-control/subscription-error.png)

<span data-ttu-id="1e996-110">A támogatási kérelem beépülő modulban kell írási engedéllyel, vagy a támogatási művelet Microsoft.Support/* hozhatók létre és támogatási kérelmek kezelése az előfizetés hatókörben által betöltött szerepkör.</span><span class="sxs-lookup"><span data-stu-id="1e996-110">In support request management, you need write permission or a role that has the Support action Microsoft.Support/* at the Subscription scope to be able to create and manage support requests.</span></span>

<span data-ttu-id="1e996-111">A következő cikk ismerteti, hogyan használható Azure egyéni szerepköralapú hozzáférés-vezérlést (RBAC) létrehozása és kezelése az Azure portálon támogatási kérelmeket.</span><span class="sxs-lookup"><span data-stu-id="1e996-111">The following article explains how you can use Azure’s custom Role-Based Access Control (RBAC) to create and manage support requests in the Azure portal.</span></span>

## <a name="getting-started"></a><span data-ttu-id="1e996-112">Első lépések</span><span class="sxs-lookup"><span data-stu-id="1e996-112">Getting Started</span></span>

<span data-ttu-id="1e996-113">A fenti példa használ, akkor tudná egy támogatási kérést az erőforrás létrehozásához, ha a volt az előfizetésben egyedi RBAC szerepkört az előfizetés tulajdonosa.</span><span class="sxs-lookup"><span data-stu-id="1e996-113">Using the example above, you would be able to create a support request for your resource if you were assigned a custom RBAC role on the subscription by the subscription owner.</span></span>
<span data-ttu-id="1e996-114">[Egyéni RBAC-szerepkörök](https://azure.microsoft.com/documentation/articles/role-based-access-control-custom-roles/) Azure PowerShell, Azure parancssori felület (CLI) és a REST API használatával hozhatók létre.</span><span class="sxs-lookup"><span data-stu-id="1e996-114">[Custom RBAC roles](https://azure.microsoft.com/documentation/articles/role-based-access-control-custom-roles/) can be created using Azure PowerShell, Azure Command-Line Interface (CLI), and the REST API.</span></span>

<span data-ttu-id="1e996-115">Egy egyéni biztonsági szerepkört a műveletek tulajdonsága határozza meg, amelyhez a szerepkör hozzáférést biztosít az Azure műveletek.</span><span class="sxs-lookup"><span data-stu-id="1e996-115">The actions property of a custom role specifies the Azure operations to which the role grants access.</span></span>
<span data-ttu-id="1e996-116">A támogatási kérelem felügyeleti egyéni szerepkör létrehozása, a szerepkörnek rendelkeznie kell a művelet Microsoft.Support/*</span><span class="sxs-lookup"><span data-stu-id="1e996-116">To create a custom role for support request management, the role must have the action Microsoft.Support/*</span></span>

<span data-ttu-id="1e996-117">Íme egy példa egy egyéni biztonsági szerepkört hozhat létre és kezelhet a támogatási kérelmek.</span><span class="sxs-lookup"><span data-stu-id="1e996-117">Here’s an example of a custom role you can use to create and manage support requests.</span></span>
<span data-ttu-id="1e996-118">Jelenleg ez a szerepkör "Támogatási kérelem közreműködői" nevű, és hogyan az egyéni szerepkör ebben a cikkben hivatkozunk.</span><span class="sxs-lookup"><span data-stu-id="1e996-118">We’ve named this role “Support Request Contributor” and that’s how we refer to the custom role in this article.</span></span>

``` Json
{
    "Name":  "Support Request Contributor",
    "Id":  "1f2aad59-39b0-41da-b052-2fb070bd7942",
    "IsCustom":  true,
    "Description":  "Lets you create and manage support tickets.",
    "Actions":  [
                    "Microsoft.Support/*"
                ],
    "NotActions":  [
                   ],
    "AssignableScopes":  [
                             "/"
                         ]
}
```

<span data-ttu-id="1e996-119">Című rész lépéseit követve [Ez a videó](https://www.youtube.com/watch?v=-PaBaDmfwKI) megtudhatja, hogyan hozzon létre egy egyéni biztonsági szerepkört az előfizetés.</span><span class="sxs-lookup"><span data-stu-id="1e996-119">Follow the steps outlined in [this video](https://www.youtube.com/watch?v=-PaBaDmfwKI) to learn how to create a custom role for your subscription.</span></span>

## <a name="create-and-manage-support-requests-in-the-azure-portal"></a><span data-ttu-id="1e996-120">Hozzon létre, és az Azure portálon támogatási kérelmek kezelése</span><span class="sxs-lookup"><span data-stu-id="1e996-120">Create and manage support requests in the Azure portal</span></span>

<span data-ttu-id="1e996-121">Vegyünk egy példát – tulajdonosaként az előfizetés "Visual Studio MSDN-előfizetés."</span><span class="sxs-lookup"><span data-stu-id="1e996-121">Let’s take an example – you are the owner of Subscription "Visual Studio MSDN Subscription."</span></span>
<span data-ttu-id="1e996-122">Joe a partner, aki az erőforráscsoportok ebben az előfizetésben egyes erőforrás tulajdonosa és rendelkezik-e olvasási engedéllyel az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="1e996-122">Joe is your peer who is a resource owner to some of the resource groups in this subscription and has read permission to the subscription.</span></span>
<span data-ttu-id="1e996-123">Zónázással biztosítson hozzáférést a társ, Joe, hozzon létre, és az erőforrások az előfizetéshez tartozó támogatási jegyek kezelése kívánja.</span><span class="sxs-lookup"><span data-stu-id="1e996-123">You wish to give access to your peer, Joe, the ability to create and manage support tickets for the resources under this subscription.</span></span>

1. <span data-ttu-id="1e996-124">Az első lépés az, hogy nyissa meg az előfizetéshez, és a "Beállítások" felhasználók listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="1e996-124">The first step is to go to the subscription and under "Settings" you see a list of users.</span></span> <span data-ttu-id="1e996-125">A felhasználó az előfizetéshez olvasási hozzáféréssel rendelkező Joe kattintson, és most egy új egyéni biztonsági szerepkört rendel neki.</span><span class="sxs-lookup"><span data-stu-id="1e996-125">Click the user Joe who has reader access on the Subscription and let’s assign a new custom role to him.</span></span>

    ![Szerepkör hozzáadása](./media/create-manage-support-requests-using-access-control/add-role.png)

2. <span data-ttu-id="1e996-127">Kattintson a "Hozzáadás" a "Felhasználók" panel alatt.</span><span class="sxs-lookup"><span data-stu-id="1e996-127">Click "Add" under the "Users" blade.</span></span> <span data-ttu-id="1e996-128">Válassza ki az egyéni biztonsági szerepkört "Támogatási kérelem közreműködői" a szerepkörök listájáról</span><span class="sxs-lookup"><span data-stu-id="1e996-128">Select the custom role "Support Request Contributor" from the list of roles</span></span>

    ![Támogatási közreműködői szerepkör hozzáadása](./media/create-manage-support-requests-using-access-control/add-support-contributor-role.png)

3. <span data-ttu-id="1e996-130">Miután kiválasztotta a szerepkör nevét, a "Hozzáadás felhasználók", és adja meg a Joe e-mail hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="1e996-130">After selecting the role name, click "Add users" and enter the Joe's email credentials.</span></span> <span data-ttu-id="1e996-131">Kattintson a "Select"</span><span class="sxs-lookup"><span data-stu-id="1e996-131">Click "Select"</span></span>

    ![Felhasználók hozzáadása](./media/create-manage-support-requests-using-access-control/add-users.png)

4. <span data-ttu-id="1e996-133">Kattintson az "Ok" gombra. a folytatáshoz</span><span class="sxs-lookup"><span data-stu-id="1e996-133">Click "Ok" to proceed</span></span>

    ![Hozzáférés hozzáadása](./media/create-manage-support-requests-using-access-control/add-access.png)

5. <span data-ttu-id="1e996-135">Ekkor megjelenik-e a felhasználót az újonnan hozzáadott egyéni biztonsági szerepkört "Támogatási kérelem közreműködői", amelynek a tulajdonosa az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="1e996-135">Now you see the user with the newly added custom role "Support Request Contributor" under the Subscription for which you are the owner</span></span>

    ![A hozzáadott felhasználónak](./media/create-manage-support-requests-using-access-control/user-added.png)

    <span data-ttu-id="1e996-137">Amikor Joe bejelentkezik a portálra, az előfizetést, amelyhez ő hozzá látja.</span><span class="sxs-lookup"><span data-stu-id="1e996-137">When Joe logs in the portal, he sees the subscription to which he was added.</span></span>

7. <span data-ttu-id="1e996-138">Joe "Új támogatási kérelem" a "Súgó és támogatása" paneljéről kattint, és hozhat létre támogatási kérelmek "Visual Studio Ultimate az MSDN"</span><span class="sxs-lookup"><span data-stu-id="1e996-138">Joe clicks "New Support request" from the "Help and Support" blade and can create support requests for "Visual Studio Ultimate with MSDN"</span></span>

    ![Új támogatási kérelem](./media/create-manage-support-requests-using-access-control/new-support-request.png)

8. <span data-ttu-id="1e996-140">Kattintson az "Összes kérelmek támogatásához" Joe a listája látható a támogatási kérelmek létrehozása ehhez az előfizetéshez ![eset Részletek nézet](./media/create-manage-support-requests-using-access-control/case-details-view.png)</span><span class="sxs-lookup"><span data-stu-id="1e996-140">Clicking "All support requests" Joe can see the list of support requests created for this Subscription  ![Case details view](./media/create-manage-support-requests-using-access-control/case-details-view.png)</span></span>

## <a name="remove-support-request-access-in-the-azure-portal"></a><span data-ttu-id="1e996-141">Távolítsa el a támogatási kérelem hozzáférés az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="1e996-141">Remove support request access in the Azure portal</span></span>

<span data-ttu-id="1e996-142">Ugyanúgy, mint az is lehet, hogy hozzáférést biztosítson a felhasználó létrehozásához és kezeléséhez a támogatási kérelmek, lehetőség, valamint a felhasználó hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="1e996-142">Just as it is possible to grant access to a user to create and manage support requests, it's possible to remove access for the user as well.</span></span>
<span data-ttu-id="1e996-143">Hozzon létre és támogatási kérelmek kezelése, hogy az előfizetés olyan eltávolításához kattintson a "Beállítások", és kattintson a felhasználónak (jelen esetben Joe).</span><span class="sxs-lookup"><span data-stu-id="1e996-143">To remove the ability to create and manage support requests, go to the Subscription, click "Settings" and click the user (in this case, Joe).</span></span>
<span data-ttu-id="1e996-144">Kattintson a jobb gombbal a szerepkör nevét, a "Támogatási kérelem közreműködői", és kattintson az "Eltávolítás"</span><span class="sxs-lookup"><span data-stu-id="1e996-144">Right-click the role name, "Support Request Contributor" and click "Remove"</span></span>

![Támogatási kérelem hozzáférés](./media/create-manage-support-requests-using-access-control/remove-support-request-access.png)

<span data-ttu-id="1e996-146">Ha Joe jelentkezik be a portálra, és hozzon létre egy támogatási kérést próbálkozik, ezután a következő hibát tapasztal</span><span class="sxs-lookup"><span data-stu-id="1e996-146">When Joe logs in to the portal and tries to create a support request, he encounters the following error</span></span>

![Előfizetés hiba – 2](./media/create-manage-support-requests-using-access-control/subscription-error-2.png)

<span data-ttu-id="1e996-148">Joe nem jelennek meg kérelmek támogatásához, ha "Az összes kérelmek támogatásához" elemre kattint</span><span class="sxs-lookup"><span data-stu-id="1e996-148">Joe cannot see any support requests when he clicks "All support requests"</span></span>

![eset adatainak megtekintése – 2](./media/create-manage-support-requests-using-access-control/case-details-view-2.png)
