---
title: "A csoport az Azure Active Directoryban a tagokat kezelése |} Microsoft Docs"
description: "Hogyan lehet hozzáadni vagy a felhasználók és eszközök eltávolítása egy csoportból az Azure Active Directoryban"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: d399a97d-fd2a-4b2d-b73d-0975db83f41b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 044e88f95712e1cc5b5532f5492c78d711a8d858
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="manage-group-membership-for-users-in-your-azure-active-directory-tenant"></a><span data-ttu-id="78541-103">Az Azure Active Directory-bérlő felhasználók csoport tagságának kezelésére</span><span class="sxs-lookup"><span data-stu-id="78541-103">Manage group membership for users in your Azure Active Directory tenant</span></span>
<span data-ttu-id="78541-104">Ez a cikk azt ismerteti, hogyan kezelheti az Azure Active Directory (Azure AD) csoport tagjai.</span><span class="sxs-lookup"><span data-stu-id="78541-104">This article explains how to manage the members for a group in Azure Active Directory (Azure AD).</span></span>

## <a name="how-do-i-find-the-members-and-manage-them"></a><span data-ttu-id="78541-105">Hogyan keresse meg a tagok és kezelheti azokat?</span><span class="sxs-lookup"><span data-stu-id="78541-105">How do I find the members and manage them?</span></span>
1. <span data-ttu-id="78541-106">Jelentkezzen be a [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely a címtár globális rendszergazdája.</span><span class="sxs-lookup"><span data-stu-id="78541-106">Sign in to the [Azure portal](https://portal.azure.com) with an account that's a global admin for the directory.</span></span>
2. <span data-ttu-id="78541-107">Válassza ki **további szolgáltatások**, adja meg **felhasználók és csoportok** a szövegmezőbe, majd válassza ki azt a **Enter**.</span><span class="sxs-lookup"><span data-stu-id="78541-107">Select **More services**, enter **Users and groups** in the text box, and then select **Enter**.</span></span>

   ![Nyitó felhasználók kezelése](./media/active-directory-groups-members-azure-portal/search-user-management.png)
3. <span data-ttu-id="78541-109">Az a **felhasználók és csoportok** panelen válassza **összes csoport**.</span><span class="sxs-lookup"><span data-stu-id="78541-109">On the **Users and groups** blade, select **All groups**.</span></span>

   ![A csoportok panel megnyitása](./media/active-directory-groups-members-azure-portal/view-groups-blade.png)
4. <span data-ttu-id="78541-111">Az a **felhasználók és csoportok – minden csoport** panelen válasszon ki egy csoportot.</span><span class="sxs-lookup"><span data-stu-id="78541-111">On the **Users and groups - All groups** blade, select a group.</span></span>
5. <span data-ttu-id="78541-112">Az a **csoport - *groupname***  panelen válassza **tagok**.</span><span class="sxs-lookup"><span data-stu-id="78541-112">On the **Group - *groupname*** blade, select **Members**.</span></span>

   ![A tagok panel megnyitása](./media/active-directory-groups-members-azure-portal/view-group-members.png)
6. <span data-ttu-id="78541-114">Tagok hozzáadása a csoporthoz, az a **csoport - tagok** panelen válassza **tagok hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="78541-114">To add members to the group, on the **Group - Members** blade, select **Add Members**.</span></span>

   ![Tagok parancs hozzáadása](./media/active-directory-groups-members-azure-portal/add-group-members-command.png)
7. <span data-ttu-id="78541-116">Az a **tagok** panelen, jelölje be egy vagy több felhasználók vagy eszközök felvétele a csoportba, és válassza ki a **válasszon** gomb felvétele a csoportba a panel alján.</span><span class="sxs-lookup"><span data-stu-id="78541-116">On the **Members** blade, select one or more users or devices to add to the group and select the **Select** button at the bottom of the blade to add them to the group.</span></span> <span data-ttu-id="78541-117">A **felhasználói** mezőben szűrése a képernyőt a megfelelő felhasználó vagy eszköz nevek a bejegyzést.</span><span class="sxs-lookup"><span data-stu-id="78541-117">The **User** box filters the display based on matching your entry to any part of a user or device name.</span></span> <span data-ttu-id="78541-118">Nem helyettesítő karakterek használata engedélyezett, hogy a mezőben.</span><span class="sxs-lookup"><span data-stu-id="78541-118">No wildcard characters are accepted in that box.</span></span>
8. <span data-ttu-id="78541-119">Tagok eltávolítása a csoportból, a a **csoport - tagok** panelen válassza ki a megfelelő tag.</span><span class="sxs-lookup"><span data-stu-id="78541-119">To remove members from the group, on the **Group - Members** blade, select a member.</span></span>
9. <span data-ttu-id="78541-120">Az a ***membername*** panelen válassza a **eltávolítása** parancsot, és erősítse meg választását a parancssorba.</span><span class="sxs-lookup"><span data-stu-id="78541-120">On the ***membername*** blade, select the **Remove** command, and confirm your choice at the prompt.</span></span>

   ![Távolítsa el a tagok parancs](./media/active-directory-groups-members-azure-portal/remove-group-members-command.png)
10. <span data-ttu-id="78541-122">Ha befejezte a csoport tagjainak módosítása, válassza ki a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="78541-122">When you finish changing members for the group, select **Save**.</span></span>

## <a name="additional-information"></a><span data-ttu-id="78541-123">További információ</span><span class="sxs-lookup"><span data-stu-id="78541-123">Additional information</span></span>
<span data-ttu-id="78541-124">E cikkekben további információk találhatók az Azure Active Directoryval kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="78541-124">These articles provide additional information on Azure Active Directory.</span></span>

* [<span data-ttu-id="78541-125">Tekintse meg a meglévő csoportok</span><span class="sxs-lookup"><span data-stu-id="78541-125">See existing groups</span></span>](active-directory-groups-view-azure-portal.md)
* [<span data-ttu-id="78541-126">Hozzon létre egy új csoportot és tagok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="78541-126">Create a new group and adding members</span></span>](active-directory-groups-create-azure-portal.md)
* [<span data-ttu-id="78541-127">Egy csoport beállításainak kezelése</span><span class="sxs-lookup"><span data-stu-id="78541-127">Manage settings of a group</span></span>](active-directory-groups-settings-azure-portal.md)
* [<span data-ttu-id="78541-128">A csoport tagságát kezelése</span><span class="sxs-lookup"><span data-stu-id="78541-128">Manage memberships of a group</span></span>](active-directory-groups-membership-azure-portal.md)
* [<span data-ttu-id="78541-129">A csoport dinamikus szabályok kezelése</span><span class="sxs-lookup"><span data-stu-id="78541-129">Manage dynamic rules for users in a group</span></span>](active-directory-groups-dynamic-membership-azure-portal.md)
