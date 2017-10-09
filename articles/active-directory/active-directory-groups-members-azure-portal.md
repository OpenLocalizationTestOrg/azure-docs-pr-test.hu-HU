---
title: a csoport az Azure Active Directoryban aaaManage hello tagok |} Microsoft Docs
description: "Hogyan tooadd vagy a felhasználók és eszközök eltávolítása egy csoportból az Azure Active Directoryban"
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
ms.openlocfilehash: 4cb16ee63828003da251423a04736f7174dd4896
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-group-membership-for-users-in-your-azure-active-directory-tenant"></a><span data-ttu-id="1e8ed-103">Az Azure Active Directory-bérlő felhasználók csoport tagságának kezelésére</span><span class="sxs-lookup"><span data-stu-id="1e8ed-103">Manage group membership for users in your Azure Active Directory tenant</span></span>
<span data-ttu-id="1e8ed-104">Ez a cikk azt ismerteti, hogyan toomanage hello az Azure Active Directory (Azure AD) csoport tagjai.</span><span class="sxs-lookup"><span data-stu-id="1e8ed-104">This article explains how toomanage hello members for a group in Azure Active Directory (Azure AD).</span></span>

## <a name="how-do-i-find-hello-members-and-manage-them"></a><span data-ttu-id="1e8ed-105">Hogyan hello tagok keresse meg és kezelheti azokat?</span><span class="sxs-lookup"><span data-stu-id="1e8ed-105">How do I find hello members and manage them?</span></span>
1. <span data-ttu-id="1e8ed-106">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely hello címtár globális rendszergazdája.</span><span class="sxs-lookup"><span data-stu-id="1e8ed-106">Sign in toohello [Azure portal](https://portal.azure.com) with an account that's a global admin for hello directory.</span></span>
2. <span data-ttu-id="1e8ed-107">Válassza ki **további szolgáltatások**, adja meg **felhasználók és csoportok** hello szövegmezőbe, és válassza ki **Enter**.</span><span class="sxs-lookup"><span data-stu-id="1e8ed-107">Select **More services**, enter **Users and groups** in hello text box, and then select **Enter**.</span></span>

   ![Nyitó felhasználók kezelése](./media/active-directory-groups-members-azure-portal/search-user-management.png)
3. <span data-ttu-id="1e8ed-109">A hello **felhasználók és csoportok** panelen válassza **összes csoport**.</span><span class="sxs-lookup"><span data-stu-id="1e8ed-109">On hello **Users and groups** blade, select **All groups**.</span></span>

   ![Nyitó hello csoportok panelen](./media/active-directory-groups-members-azure-portal/view-groups-blade.png)
4. <span data-ttu-id="1e8ed-111">A hello **felhasználók és csoportok – minden csoport** panelen válasszon ki egy csoportot.</span><span class="sxs-lookup"><span data-stu-id="1e8ed-111">On hello **Users and groups - All groups** blade, select a group.</span></span>
5. <span data-ttu-id="1e8ed-112">A hello **csoport - *groupname***  panelen válassza **tagok**.</span><span class="sxs-lookup"><span data-stu-id="1e8ed-112">On hello **Group - *groupname*** blade, select **Members**.</span></span>

   ![Nyitó hello tagok panel](./media/active-directory-groups-members-azure-portal/view-group-members.png)
6. <span data-ttu-id="1e8ed-114">tooadd tagok toohello csoport, a hello **csoport - tagok** panelen válassza **tagok hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="1e8ed-114">tooadd members toohello group, on hello **Group - Members** blade, select **Add Members**.</span></span>

   ![Tagok parancs hozzáadása](./media/active-directory-groups-members-azure-portal/add-group-members-command.png)
7. <span data-ttu-id="1e8ed-116">A hello **tagok** panelen, válasszon ki egy vagy több felhasználók vagy eszközök tooadd toohello csoport és select hello **válasszon** gomb hello panel tooadd hello alján őket toohello csoport.</span><span class="sxs-lookup"><span data-stu-id="1e8ed-116">On hello **Members** blade, select one or more users or devices tooadd toohello group and select hello **Select** button at hello bottom of hello blade tooadd them toohello group.</span></span> <span data-ttu-id="1e8ed-117">Hello **felhasználói** szűri hello megjelenítési megfelelő a bejegyzés tooany része egy felhasználó vagy eszköz nevét.</span><span class="sxs-lookup"><span data-stu-id="1e8ed-117">hello **User** box filters hello display based on matching your entry tooany part of a user or device name.</span></span> <span data-ttu-id="1e8ed-118">Nem helyettesítő karakterek használata engedélyezett, hogy a mezőben.</span><span class="sxs-lookup"><span data-stu-id="1e8ed-118">No wildcard characters are accepted in that box.</span></span>
8. <span data-ttu-id="1e8ed-119">hello csoportból, a hello tooremove tagok **csoport - tagok** panelen válassza ki a megfelelő tag.</span><span class="sxs-lookup"><span data-stu-id="1e8ed-119">tooremove members from hello group, on hello **Group - Members** blade, select a member.</span></span>
9. <span data-ttu-id="1e8ed-120">A hello ***membername*** panelen, jelölje be hello **eltávolítása** parancsot, és erősítse meg választását hello a parancssorból.</span><span class="sxs-lookup"><span data-stu-id="1e8ed-120">On hello ***membername*** blade, select hello **Remove** command, and confirm your choice at hello prompt.</span></span>

   ![Távolítsa el a tagok parancs](./media/active-directory-groups-members-azure-portal/remove-group-members-command.png)
10. <span data-ttu-id="1e8ed-122">Amikor befejezte a hello csoport tagjainak módosítása, válassza ki a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="1e8ed-122">When you finish changing members for hello group, select **Save**.</span></span>

## <a name="additional-information"></a><span data-ttu-id="1e8ed-123">További információ</span><span class="sxs-lookup"><span data-stu-id="1e8ed-123">Additional information</span></span>
<span data-ttu-id="1e8ed-124">E cikkekben további információk találhatók az Azure Active Directoryval kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="1e8ed-124">These articles provide additional information on Azure Active Directory.</span></span>

* [<span data-ttu-id="1e8ed-125">Tekintse meg a meglévő csoportok</span><span class="sxs-lookup"><span data-stu-id="1e8ed-125">See existing groups</span></span>](active-directory-groups-view-azure-portal.md)
* [<span data-ttu-id="1e8ed-126">Hozzon létre egy új csoportot és tagok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="1e8ed-126">Create a new group and adding members</span></span>](active-directory-groups-create-azure-portal.md)
* [<span data-ttu-id="1e8ed-127">Egy csoport beállításainak kezelése</span><span class="sxs-lookup"><span data-stu-id="1e8ed-127">Manage settings of a group</span></span>](active-directory-groups-settings-azure-portal.md)
* [<span data-ttu-id="1e8ed-128">A csoport tagságát kezelése</span><span class="sxs-lookup"><span data-stu-id="1e8ed-128">Manage memberships of a group</span></span>](active-directory-groups-membership-azure-portal.md)
* [<span data-ttu-id="1e8ed-129">A csoport dinamikus szabályok kezelése</span><span class="sxs-lookup"><span data-stu-id="1e8ed-129">Manage dynamic rules for users in a group</span></span>](active-directory-groups-dynamic-membership-azure-portal.md)
