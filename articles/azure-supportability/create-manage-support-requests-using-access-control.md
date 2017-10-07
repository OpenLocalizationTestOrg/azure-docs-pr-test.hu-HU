---
title: "Szerepköralapú hozzáférés-vezérlést (RBAC) toocontrol aaaAzure hozzáférési jogok toocreate és támogatási kérelmek kezelése |} Microsoft Docs"
description: "Azure szerepköralapú hozzáférés-vezérlés (RBAC) toocontrol hozzáférési jogok toocreate és támogatási kérelmek kezelése"
author: ganganarayanan
ms.author: gangan
ms.date: 1/31/2017
ms.topic: article
ms.service: microsoft-docs
ms.assetid: 58a0ca9d-86d2-469a-9714-3b8320c33cf5
ms.openlocfilehash: c68a699ac870fa6bf371deb8ed0424848f39acf0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-role-based-access-control-rbac-toocontrol-access-rights-toocreate-and-manage-support-requests"></a><span data-ttu-id="64c34-103">Azure szerepköralapú hozzáférés-vezérlés (RBAC) toocontrol hozzáférési jogok toocreate és támogatási kérelmek kezelése</span><span class="sxs-lookup"><span data-stu-id="64c34-103">Azure Role-Based Access Control (RBAC) toocontrol access rights toocreate and manage support requests</span></span>

<span data-ttu-id="64c34-104">[Szerepköralapú hozzáférés-vezérlés (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is) lehetővé teszi a részletes hozzáféréskezelést az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="64c34-104">[Role-Based Access Control (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is) enables fine-grained access management for Azure.</span></span>
<span data-ttu-id="64c34-105">Támogatja a kérelem létrehozását az Azure-portálon hello [portal.azure.com](https://portal.azure.com), használja az Azure RBAC modell toodefine hozhat létre és kezelhet a támogatási kérelmek számára.</span><span class="sxs-lookup"><span data-stu-id="64c34-105">Support request creation in hello Azure portal, [portal.azure.com](https://portal.azure.com), uses Azure’s RBAC model toodefine who can create and manage support requests.</span></span>
<span data-ttu-id="64c34-106">A hozzáférés hello megfelelő RBAC szerepkör toousers, csoportok és alkalmazások egy bizonyos hatókörhöz, amely lehet egy előfizetést, erőforráscsoport vagy egy erőforrás hozzárendelésével.</span><span class="sxs-lookup"><span data-stu-id="64c34-106">Access is granted by assigning hello appropriate RBAC role toousers, groups, and applications at a certain scope, which can be a subscription, resource group or a resource.</span></span>

<span data-ttu-id="64c34-107">Vegyünk egy példát: egy erőforrás csoport tulajdonosának olvasási engedély hello előfizetés hatókörben, kezelheti a hello erőforráscsoport, például webhelyekhez, virtuális gépek és alhálózatok tartozó összes hello erőforrást.</span><span class="sxs-lookup"><span data-stu-id="64c34-107">Let’s take an example: As a resource group owner with read permissions at hello subscription scope, you can manage all hello resources under hello resource group, like websites, virtual machines, and subnets.</span></span>
<span data-ttu-id="64c34-108">Azonban ha toocreate egy támogatási kérést elleni hello virtuálisgép-erőforrást, tapasztal hello a következő hiba</span><span class="sxs-lookup"><span data-stu-id="64c34-108">However, when you try toocreate a support request against hello virtual machine resource, you encounter hello following error</span></span>

![Előfizetés hiba](./media/create-manage-support-requests-using-access-control/subscription-error.png)

<span data-ttu-id="64c34-110">A támogatási kérelem beépülő modulban kell írási engedélye, vagy hello által betöltött szerepkör támogatja a következő hello előfizetés hatókör toobe képes toocreate Microsoft.Support/* műveletet, és a támogatási kérelmek kezelése.</span><span class="sxs-lookup"><span data-stu-id="64c34-110">In support request management, you need write permission or a role that has hello Support action Microsoft.Support/* at hello Subscription scope toobe able toocreate and manage support requests.</span></span>

<span data-ttu-id="64c34-111">hello a következő cikk ismerteti, hogyan használhatja az Azure egyéni szerepköralapú hozzáférés-vezérlést (RBAC) toocreate és hello Azure-portálon a támogatási kérelmek kezelése.</span><span class="sxs-lookup"><span data-stu-id="64c34-111">hello following article explains how you can use Azure’s custom Role-Based Access Control (RBAC) toocreate and manage support requests in hello Azure portal.</span></span>

## <a name="getting-started"></a><span data-ttu-id="64c34-112">Első lépések</span><span class="sxs-lookup"><span data-stu-id="64c34-112">Getting Started</span></span>

<span data-ttu-id="64c34-113">A fenti hello példáját lehetővé válik képes toocreate egy támogatási kérést az erőforrás hello előfizetés tulajdonosa által hozzárendelt hello előfizetésben egyéni RBAC szerepkör volt.</span><span class="sxs-lookup"><span data-stu-id="64c34-113">Using hello example above, you would be able toocreate a support request for your resource if you were assigned a custom RBAC role on hello subscription by hello subscription owner.</span></span>
<span data-ttu-id="64c34-114">[Egyéni RBAC-szerepkörök](https://azure.microsoft.com/documentation/articles/role-based-access-control-custom-roles/) Azure PowerShell, az Azure parancssori felület (CLI) és a REST API hello segítségével hozhatók létre.</span><span class="sxs-lookup"><span data-stu-id="64c34-114">[Custom RBAC roles](https://azure.microsoft.com/documentation/articles/role-based-access-control-custom-roles/) can be created using Azure PowerShell, Azure Command-Line Interface (CLI), and hello REST API.</span></span>

<span data-ttu-id="64c34-115">az egy egyéni biztonsági szerepkört a hello műveletek tulajdonság határozza meg, hello Azure üzemeltetése toowhich hello szerepkör engedélyezi a hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="64c34-115">hello actions property of a custom role specifies hello Azure operations toowhich hello role grants access.</span></span>
<span data-ttu-id="64c34-116">egy egyéni biztonsági szerepkört a támogatási kérelem felügyeleti toocreate, hello szerepkörnek rendelkeznie kell a hello művelet Microsoft.Support/*</span><span class="sxs-lookup"><span data-stu-id="64c34-116">toocreate a custom role for support request management, hello role must have hello action Microsoft.Support/*</span></span>

<span data-ttu-id="64c34-117">Íme egy példa egy egyéni biztonsági szerepkört toocreate használja, és kezelheti a kérelmek támogatásához.</span><span class="sxs-lookup"><span data-stu-id="64c34-117">Here’s an example of a custom role you can use toocreate and manage support requests.</span></span>
<span data-ttu-id="64c34-118">Jelenleg ez a szerepkör "Támogatási kérelem közreműködői" nevű, és hogyan irányítjuk toohello egyéni szerepkör ebben a cikkben.</span><span class="sxs-lookup"><span data-stu-id="64c34-118">We’ve named this role “Support Request Contributor” and that’s how we refer toohello custom role in this article.</span></span>

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

<span data-ttu-id="64c34-119">Hello című rész lépéseit követve [Ez a videó](https://www.youtube.com/watch?v=-PaBaDmfwKI) toolearn hogyan toocreate egy egyéni biztonsági szerepkört az előfizetés.</span><span class="sxs-lookup"><span data-stu-id="64c34-119">Follow hello steps outlined in [this video](https://www.youtube.com/watch?v=-PaBaDmfwKI) toolearn how toocreate a custom role for your subscription.</span></span>

## <a name="create-and-manage-support-requests-in-hello-azure-portal"></a><span data-ttu-id="64c34-120">Hozzon létre és hello Azure-portálon a támogatási kérelmek kezelése</span><span class="sxs-lookup"><span data-stu-id="64c34-120">Create and manage support requests in hello Azure portal</span></span>

<span data-ttu-id="64c34-121">Vegyünk egy példát – áll hello tulajdonos előfizetés "Visual Studio MSDN-előfizetés."</span><span class="sxs-lookup"><span data-stu-id="64c34-121">Let’s take an example – you are hello owner of Subscription "Visual Studio MSDN Subscription."</span></span>
<span data-ttu-id="64c34-122">Joe a partner, aki egy erőforrás tulajdonosa toosome hello erőforráscsoportok ebben az előfizetésben és rendelkezik-e olvasási engedéllyel toohello előfizetés.</span><span class="sxs-lookup"><span data-stu-id="64c34-122">Joe is your peer who is a resource owner toosome of hello resource groups in this subscription and has read permission toohello subscription.</span></span>
<span data-ttu-id="64c34-123">Toogive hozzáférés tooyour társ, Joe, hello képességét toocreate kívánja és hello erőforrások az előfizetéshez tartozó támogatási jegyek kezelése.</span><span class="sxs-lookup"><span data-stu-id="64c34-123">You wish toogive access tooyour peer, Joe, hello ability toocreate and manage support tickets for hello resources under this subscription.</span></span>

1. <span data-ttu-id="64c34-124">hello első lépéseként toogo toohello előfizetés és a "Beállítások" felhasználók listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="64c34-124">hello first step is toogo toohello subscription and under "Settings" you see a list of users.</span></span> <span data-ttu-id="64c34-125">Kattintson a hello felhasználói Joe hello előfizetés olvasási hozzáféréssel rendelkező, és most rendelje hozzá egy új egyéni biztonsági szerepkört toohim.</span><span class="sxs-lookup"><span data-stu-id="64c34-125">Click hello user Joe who has reader access on hello Subscription and let’s assign a new custom role toohim.</span></span>

    ![Szerepkör hozzáadása](./media/create-manage-support-requests-using-access-control/add-role.png)

2. <span data-ttu-id="64c34-127">Kattintson a "Hozzáadás" hello "Felhasználók" panel alatt.</span><span class="sxs-lookup"><span data-stu-id="64c34-127">Click "Add" under hello "Users" blade.</span></span> <span data-ttu-id="64c34-128">Válassza ki a hello egyéni szerepkör "Támogatási kérelem közreműködői" a hello szerepkörök</span><span class="sxs-lookup"><span data-stu-id="64c34-128">Select hello custom role "Support Request Contributor" from hello list of roles</span></span>

    ![Támogatási közreműködői szerepkör hozzáadása](./media/create-manage-support-requests-using-access-control/add-support-contributor-role.png)

3. <span data-ttu-id="64c34-130">Miután kiválasztotta a hello szerepkör nevét, a "Hozzáadás felhasználók", és írja be a hello Joe e-mail hitelesítő adatokhoz.</span><span class="sxs-lookup"><span data-stu-id="64c34-130">After selecting hello role name, click "Add users" and enter hello Joe's email credentials.</span></span> <span data-ttu-id="64c34-131">Kattintson a "Select"</span><span class="sxs-lookup"><span data-stu-id="64c34-131">Click "Select"</span></span>

    ![Felhasználók hozzáadása](./media/create-manage-support-requests-using-access-control/add-users.png)

4. <span data-ttu-id="64c34-133">Kattintson az "Ok" tooproceed</span><span class="sxs-lookup"><span data-stu-id="64c34-133">Click "Ok" tooproceed</span></span>

    ![Hozzáférés hozzáadása](./media/create-manage-support-requests-using-access-control/add-access.png)

5. <span data-ttu-id="64c34-135">Ekkor megjelenik a hello felhasználói hello újonnan hozzáadott egyéni szerepkör "Támogatási kérelem közreműködői" hello előfizetésben, amelynek hello tulajdonosa</span><span class="sxs-lookup"><span data-stu-id="64c34-135">Now you see hello user with hello newly added custom role "Support Request Contributor" under hello Subscription for which you are hello owner</span></span>

    ![A hozzáadott felhasználónak](./media/create-manage-support-requests-using-access-control/user-added.png)

    <span data-ttu-id="64c34-137">Amikor Joe hello portal bejelentkezik, látja hello előfizetés toowhich ő hozzá lett adva.</span><span class="sxs-lookup"><span data-stu-id="64c34-137">When Joe logs in hello portal, he sees hello subscription toowhich he was added.</span></span>

7. <span data-ttu-id="64c34-138">Joe "Új támogatási kérelem" hello "Súgó és támogatása" paneljéről kattint, és hozhat létre támogatási kérelmek "Visual Studio Ultimate az MSDN"</span><span class="sxs-lookup"><span data-stu-id="64c34-138">Joe clicks "New Support request" from hello "Help and Support" blade and can create support requests for "Visual Studio Ultimate with MSDN"</span></span>

    ![Új támogatási kérelem](./media/create-manage-support-requests-using-access-control/new-support-request.png)

8. <span data-ttu-id="64c34-140">Kattintson az "Összes kérelmek támogatásához" Joe hello listája látható a támogatási kérelmek létrehozása ehhez az előfizetéshez ![eset Részletek nézet](./media/create-manage-support-requests-using-access-control/case-details-view.png)</span><span class="sxs-lookup"><span data-stu-id="64c34-140">Clicking "All support requests" Joe can see hello list of support requests created for this Subscription  ![Case details view](./media/create-manage-support-requests-using-access-control/case-details-view.png)</span></span>

## <a name="remove-support-request-access-in-hello-azure-portal"></a><span data-ttu-id="64c34-141">Távolítsa el a támogatási kérelem hozzáférés hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="64c34-141">Remove support request access in hello Azure portal</span></span>

<span data-ttu-id="64c34-142">Ugyanúgy, mint lehetséges toogrant hozzáférés tooa felhasználói toocreate és támogatási kérelmek kezelése, akkor lehetséges tooremove hozzáférést, valamint a hello felhasználó számára.</span><span class="sxs-lookup"><span data-stu-id="64c34-142">Just as it is possible toogrant access tooa user toocreate and manage support requests, it's possible tooremove access for hello user as well.</span></span>
<span data-ttu-id="64c34-143">tooremove képességét toocreate hello és támogatási kérelmek kezelése nyissa toohello előfizetés, kattintson a "Beállítások" és kattintson az hello felhasználónak (jelen esetben Joe).</span><span class="sxs-lookup"><span data-stu-id="64c34-143">tooremove hello ability toocreate and manage support requests, go toohello Subscription, click "Settings" and click hello user (in this case, Joe).</span></span>
<span data-ttu-id="64c34-144">Kattintson a jobb gombbal a hello szerepkör nevét, "Támogatási kérelem közreműködői", és kattintson az "Eltávolítás"</span><span class="sxs-lookup"><span data-stu-id="64c34-144">Right-click hello role name, "Support Request Contributor" and click "Remove"</span></span>

![Támogatási kérelem hozzáférés](./media/create-manage-support-requests-using-access-control/remove-support-request-access.png)

<span data-ttu-id="64c34-146">Ha Joe toohello portálon naplózza, és megpróbál toocreate egy támogatási kérést, ő észlel hello a következő hiba</span><span class="sxs-lookup"><span data-stu-id="64c34-146">When Joe logs in toohello portal and tries toocreate a support request, he encounters hello following error</span></span>

![Előfizetés hiba – 2](./media/create-manage-support-requests-using-access-control/subscription-error-2.png)

<span data-ttu-id="64c34-148">Joe nem jelennek meg kérelmek támogatásához, ha "Az összes kérelmek támogatásához" elemre kattint</span><span class="sxs-lookup"><span data-stu-id="64c34-148">Joe cannot see any support requests when he clicks "All support requests"</span></span>

![eset adatainak megtekintése – 2](./media/create-manage-support-requests-using-access-control/case-details-view-2.png)
