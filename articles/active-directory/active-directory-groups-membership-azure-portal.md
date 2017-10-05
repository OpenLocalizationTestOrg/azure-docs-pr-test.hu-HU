---
title: "A csoportok a csoport beletartozik az Azure Active Directoryban kezelése |} Microsoft Docs"
description: "Csoportok tartalmazhatnak más csoportok az Azure Active Directoryban. Megtudhatja, hogyan kezelheti az adott tagságok."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: e785c2d0-7724-47d4-a56e-c58280c08a14
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 08e04a6590176c4084ca47b4bd6cbb22500eca2d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="manage-to-which-groups-a-group-belongs-in-your-azure-active-directory-tenant"></a><span data-ttu-id="06759-104">Kezelheti az Azure Active Directory-bérlőhöz tartozik egy csoport mely csoportok</span><span class="sxs-lookup"><span data-stu-id="06759-104">Manage to which groups a group belongs in your Azure Active Directory tenant</span></span>
<span data-ttu-id="06759-105">Csoportok tartalmazhatnak más csoportok az Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="06759-105">Groups can contain other groups in Azure Active Directory.</span></span> <span data-ttu-id="06759-106">Megtudhatja, hogyan kezelheti az adott tagságok.</span><span class="sxs-lookup"><span data-stu-id="06759-106">Here's how to manage those memberships.</span></span>

## <a name="how-do-i-find-the-groups-my-group-is-a-member-of"></a><span data-ttu-id="06759-107">Hogyan találhatom meg a csoportok a csoport egy tagja?</span><span class="sxs-lookup"><span data-stu-id="06759-107">How do I find the groups my group is a member of?</span></span>
1. <span data-ttu-id="06759-108">Jelentkezzen be a [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely a címtár globális rendszergazdája.</span><span class="sxs-lookup"><span data-stu-id="06759-108">Sign in to the [Azure portal](https://portal.azure.com) with an account that's a global admin for the directory.</span></span>
2. <span data-ttu-id="06759-109">Válassza ki **további szolgáltatások**, adja meg **felhasználók és csoportok** a szövegmezőbe, majd válassza ki azt a **Enter**.</span><span class="sxs-lookup"><span data-stu-id="06759-109">Select **More services**, enter **Users and groups** in the text box, and then select **Enter**.</span></span>

   ![Nyitó felhasználók kezelése](./media/active-directory-groups-membership-azure-portal/search-user-management.png)
3. <span data-ttu-id="06759-111">Az a **felhasználók és csoportok** panelen válassza **összes csoport**.</span><span class="sxs-lookup"><span data-stu-id="06759-111">On the **Users and groups** blade, select **All groups**.</span></span>

   ![A csoportok panel megnyitása](./media/active-directory-groups-membership-azure-portal/view-groups-blade.png)
4. <span data-ttu-id="06759-113">Az a **felhasználók és csoportok – minden csoport** panelen válasszon ki egy csoportot.</span><span class="sxs-lookup"><span data-stu-id="06759-113">On the **Users and groups - All groups** blade, select a group.</span></span>
5. <span data-ttu-id="06759-114">Az a **csoport - *groupname***  panelen válassza **csoporttagságok**.</span><span class="sxs-lookup"><span data-stu-id="06759-114">On the **Group - *groupname*** blade, select **Group memberships**.</span></span>

   ![A csoport tagságát panel megnyitása](./media/active-directory-groups-membership-azure-portal/group-membership-blade.png)
6. <span data-ttu-id="06759-116">A csoport tagja másik csoportnak, hozzáadja a a **Group - csoporttagságok** panelen válassza a **Hozzáadás** parancsot.</span><span class="sxs-lookup"><span data-stu-id="06759-116">To add your group as a member of another group, on the **Group - Group memberships** blade, select the **Add** command.</span></span>
7. <span data-ttu-id="06759-117">Válasszon csoportot a **csoport kijelölése** panelt, és válassza a **válasszon** gomb a panel alján.</span><span class="sxs-lookup"><span data-stu-id="06759-117">Select a group from the **Select Group** blade, and then select the **Select** button at the bottom of the blade.</span></span> <span data-ttu-id="06759-118">Egyszerre csak egy csoporthoz adhat hozzá a csoporthoz.</span><span class="sxs-lookup"><span data-stu-id="06759-118">You can add your group to only one group at a time.</span></span> <span data-ttu-id="06759-119">A **felhasználói** mezőben szűrése a képernyőt a megfelelő felhasználó vagy eszköz nevek a bejegyzést.</span><span class="sxs-lookup"><span data-stu-id="06759-119">The **User** box filters the display based on matching your entry to any part of a user or device name.</span></span> <span data-ttu-id="06759-120">Nem helyettesítő karakterek használata engedélyezett, hogy a mezőben.</span><span class="sxs-lookup"><span data-stu-id="06759-120">No wildcard characters are accepted in that box.</span></span>

   ![A csoporttagság hozzáadása](./media/active-directory-groups-membership-azure-portal/add-group-membership.png)
8. <span data-ttu-id="06759-122">A csoport eltávolításához egy másik csoport tagjaként a a **Group - csoporttagságok** panelen válasszon ki egy csoportot.</span><span class="sxs-lookup"><span data-stu-id="06759-122">To remove your group as a member of another group, on the **Group - Group memberships** blade, select a group.</span></span>
9. <span data-ttu-id="06759-123">Az a ***groupname*** panelen válassza a **eltávolítása** parancsot, és erősítse meg választását a parancssorba.</span><span class="sxs-lookup"><span data-stu-id="06759-123">On the ***groupname*** blade, select the **Remove** command, and confirm your choice at the prompt.</span></span>

   ![Távolítsa el a tagság parancs](./media/active-directory-groups-membership-azure-portal/remove-group-membership.png)
10. <span data-ttu-id="06759-125">Amikor befejezte a csoport csoporttagság módosítása, válassza ki a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="06759-125">When you finish changing group memberships for your group, select **Save**.</span></span>

## <a name="additional-information"></a><span data-ttu-id="06759-126">További információ</span><span class="sxs-lookup"><span data-stu-id="06759-126">Additional information</span></span>
<span data-ttu-id="06759-127">E cikkekben további információk találhatók az Azure Active Directoryval kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="06759-127">These articles provide additional information on Azure Active Directory.</span></span>

* [<span data-ttu-id="06759-128">Tekintse meg a meglévő csoportok</span><span class="sxs-lookup"><span data-stu-id="06759-128">See existing groups</span></span>](active-directory-groups-view-azure-portal.md)
* [<span data-ttu-id="06759-129">Hozzon létre egy új csoportot és tagok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="06759-129">Create a new group and adding members</span></span>](active-directory-groups-create-azure-portal.md)
* [<span data-ttu-id="06759-130">Egy csoport beállításainak kezelése</span><span class="sxs-lookup"><span data-stu-id="06759-130">Manage settings of a group</span></span>](active-directory-groups-settings-azure-portal.md)
* [<span data-ttu-id="06759-131">A csoport tagjai kezelése</span><span class="sxs-lookup"><span data-stu-id="06759-131">Manage members of a group</span></span>](active-directory-groups-members-azure-portal.md)
* [<span data-ttu-id="06759-132">A csoport dinamikus szabályok kezelése</span><span class="sxs-lookup"><span data-stu-id="06759-132">Manage dynamic rules for users in a group</span></span>](active-directory-groups-dynamic-membership-azure-portal.md)
