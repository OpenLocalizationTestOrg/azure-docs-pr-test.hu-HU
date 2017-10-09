---
title: a csoport tartozik tooin Azure Active Directory aaaManage hello csoportok |} Microsoft Docs
description: "Csoportok tartalmazhatnak más csoportok az Azure Active Directoryban. Itt hogyan toomanage adott tagságok."
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
ms.openlocfilehash: d0a0a1967084de0968e1e802559f9cdfd7ca6ae4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-toowhich-groups-a-group-belongs-in-your-azure-active-directory-tenant"></a><span data-ttu-id="5ec68-104">Az Azure Active Directory-bérlőhöz tartozik egy csoport toowhich csoportok kezelése</span><span class="sxs-lookup"><span data-stu-id="5ec68-104">Manage toowhich groups a group belongs in your Azure Active Directory tenant</span></span>
<span data-ttu-id="5ec68-105">Csoportok tartalmazhatnak más csoportok az Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="5ec68-105">Groups can contain other groups in Azure Active Directory.</span></span> <span data-ttu-id="5ec68-106">Itt hogyan toomanage adott tagságok.</span><span class="sxs-lookup"><span data-stu-id="5ec68-106">Here's how toomanage those memberships.</span></span>

## <a name="how-do-i-find-hello-groups-my-group-is-a-member-of"></a><span data-ttu-id="5ec68-107">Hogyan találhatom hello csoportok a csoport egy tagja?</span><span class="sxs-lookup"><span data-stu-id="5ec68-107">How do I find hello groups my group is a member of?</span></span>
1. <span data-ttu-id="5ec68-108">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely hello címtár globális rendszergazdája.</span><span class="sxs-lookup"><span data-stu-id="5ec68-108">Sign in toohello [Azure portal](https://portal.azure.com) with an account that's a global admin for hello directory.</span></span>
2. <span data-ttu-id="5ec68-109">Válassza ki **további szolgáltatások**, adja meg **felhasználók és csoportok** hello szövegmezőbe, és válassza ki **Enter**.</span><span class="sxs-lookup"><span data-stu-id="5ec68-109">Select **More services**, enter **Users and groups** in hello text box, and then select **Enter**.</span></span>

   ![Nyitó felhasználók kezelése](./media/active-directory-groups-membership-azure-portal/search-user-management.png)
3. <span data-ttu-id="5ec68-111">A hello **felhasználók és csoportok** panelen válassza **összes csoport**.</span><span class="sxs-lookup"><span data-stu-id="5ec68-111">On hello **Users and groups** blade, select **All groups**.</span></span>

   ![Nyitó hello csoportok panelen](./media/active-directory-groups-membership-azure-portal/view-groups-blade.png)
4. <span data-ttu-id="5ec68-113">A hello **felhasználók és csoportok – minden csoport** panelen válasszon ki egy csoportot.</span><span class="sxs-lookup"><span data-stu-id="5ec68-113">On hello **Users and groups - All groups** blade, select a group.</span></span>
5. <span data-ttu-id="5ec68-114">A hello **csoport - *groupname***  panelen válassza **csoporttagságok**.</span><span class="sxs-lookup"><span data-stu-id="5ec68-114">On hello **Group - *groupname*** blade, select **Group memberships**.</span></span>

   ![Hello tagságát panel megnyitása](./media/active-directory-groups-membership-azure-portal/group-membership-blade.png)
6. <span data-ttu-id="5ec68-116">tooadd a csoport egy másik csoportot, a hello tagjaként **Group - csoporttagságok** panelen, jelölje be hello **Hozzáadás** parancsot.</span><span class="sxs-lookup"><span data-stu-id="5ec68-116">tooadd your group as a member of another group, on hello **Group - Group memberships** blade, select hello **Add** command.</span></span>
7. <span data-ttu-id="5ec68-117">Jelöljön ki egy csoportot hello **csoport kijelölése** panelen, és jelölje ki hello **válasszon** hello hello panel alsó részén gombra.</span><span class="sxs-lookup"><span data-stu-id="5ec68-117">Select a group from hello **Select Group** blade, and then select hello **Select** button at hello bottom of hello blade.</span></span> <span data-ttu-id="5ec68-118">A csoport tooonly egy csoport egyszerre is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="5ec68-118">You can add your group tooonly one group at a time.</span></span> <span data-ttu-id="5ec68-119">Hello **felhasználói** szűri hello megjelenítési megfelelő a bejegyzés tooany része egy felhasználó vagy eszköz nevét.</span><span class="sxs-lookup"><span data-stu-id="5ec68-119">hello **User** box filters hello display based on matching your entry tooany part of a user or device name.</span></span> <span data-ttu-id="5ec68-120">Nem helyettesítő karakterek használata engedélyezett, hogy a mezőben.</span><span class="sxs-lookup"><span data-stu-id="5ec68-120">No wildcard characters are accepted in that box.</span></span>

   ![A csoporttagság hozzáadása](./media/active-directory-groups-membership-azure-portal/add-group-membership.png)
8. <span data-ttu-id="5ec68-122">tooremove a csoport egy másik csoportot, a hello tagjaként **Group - csoporttagságok** panelen válasszon ki egy csoportot.</span><span class="sxs-lookup"><span data-stu-id="5ec68-122">tooremove your group as a member of another group, on hello **Group - Group memberships** blade, select a group.</span></span>
9. <span data-ttu-id="5ec68-123">A hello ***groupname*** panelen, jelölje be hello **eltávolítása** parancsot, és erősítse meg választását hello a parancssorból.</span><span class="sxs-lookup"><span data-stu-id="5ec68-123">On hello ***groupname*** blade, select hello **Remove** command, and confirm your choice at hello prompt.</span></span>

   ![Távolítsa el a tagság parancs](./media/active-directory-groups-membership-azure-portal/remove-group-membership.png)
10. <span data-ttu-id="5ec68-125">Amikor befejezte a csoport csoporttagság módosítása, válassza ki a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="5ec68-125">When you finish changing group memberships for your group, select **Save**.</span></span>

## <a name="additional-information"></a><span data-ttu-id="5ec68-126">További információ</span><span class="sxs-lookup"><span data-stu-id="5ec68-126">Additional information</span></span>
<span data-ttu-id="5ec68-127">E cikkekben további információk találhatók az Azure Active Directoryval kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="5ec68-127">These articles provide additional information on Azure Active Directory.</span></span>

* [<span data-ttu-id="5ec68-128">Tekintse meg a meglévő csoportok</span><span class="sxs-lookup"><span data-stu-id="5ec68-128">See existing groups</span></span>](active-directory-groups-view-azure-portal.md)
* [<span data-ttu-id="5ec68-129">Hozzon létre egy új csoportot és tagok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="5ec68-129">Create a new group and adding members</span></span>](active-directory-groups-create-azure-portal.md)
* [<span data-ttu-id="5ec68-130">Egy csoport beállításainak kezelése</span><span class="sxs-lookup"><span data-stu-id="5ec68-130">Manage settings of a group</span></span>](active-directory-groups-settings-azure-portal.md)
* [<span data-ttu-id="5ec68-131">A csoport tagjai kezelése</span><span class="sxs-lookup"><span data-stu-id="5ec68-131">Manage members of a group</span></span>](active-directory-groups-members-azure-portal.md)
* [<span data-ttu-id="5ec68-132">A csoport dinamikus szabályok kezelése</span><span class="sxs-lookup"><span data-stu-id="5ec68-132">Manage dynamic rules for users in a group</span></span>](active-directory-groups-dynamic-membership-azure-portal.md)
