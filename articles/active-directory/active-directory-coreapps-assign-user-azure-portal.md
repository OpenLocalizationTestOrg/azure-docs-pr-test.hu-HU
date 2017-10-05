---
title: "Egy felhasználó vagy csoport hozzárendelése az Azure Active Directory vállalati alkalmazások |} Microsoft Docs"
description: "Egy vállalati alkalmazás hozzárendelése egy felhasználóhoz vagy csoporthoz, az Azure Active Directory kiválasztása"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 5817ad48-d916-492b-a8d0-2ade8c50a224
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: curtand
ms.openlocfilehash: ee784704ada9238b5cd048f99aaa4cb192ec7d57
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="assign-a-user-or-group-to-an-enterprise-app-in-azure-active-directory"></a><span data-ttu-id="c4673-103">Egy felhasználó vagy csoport hozzárendelése egy vállalati alkalmazás Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="c4673-103">Assign a user or group to an enterprise app in Azure Active Directory</span></span>
<span data-ttu-id="c4673-104">Akkor is könnyen egy felhasználó vagy csoport hozzárendelése az Azure Active Directory (Azure AD) a vállalati alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="c4673-104">It's easy to assign a user or a group to your enterprise applications in Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="c4673-105">A vállalati alkalmazások kezelésére a megfelelő engedélyekkel kell rendelkeznie, és a címtár globális rendszergazdának kell lennie.</span><span class="sxs-lookup"><span data-stu-id="c4673-105">You must have the appropriate permissions to manage the enterprise app, and you must be global admin for the directory.</span></span>

## <a name="how-do-i-assign-user-access-to-an-enterprise-app"></a><span data-ttu-id="c4673-106">Hogyan oszthatok ki a felhasználói hozzáférés vállalati alkalmazások számára?</span><span class="sxs-lookup"><span data-stu-id="c4673-106">How do I assign user access to an enterprise app?</span></span>
1. <span data-ttu-id="c4673-107">Jelentkezzen be a [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely a címtár globális rendszergazdája.</span><span class="sxs-lookup"><span data-stu-id="c4673-107">Sign in to the [Azure portal](https://portal.azure.com) with an account that's a global admin for the directory.</span></span>
2. <span data-ttu-id="c4673-108">Válassza ki **további szolgáltatások**, a mezőben adja meg Azure Active Directoryban, és válassza **Enter**.</span><span class="sxs-lookup"><span data-stu-id="c4673-108">Select **More services**, enter Azure Active Directory in the text box, and then select **Enter**.</span></span>
3. <span data-ttu-id="c4673-109">Az a **Azure Active Directory - *directoryname***  (Ez azt jelenti, hogy az Azure AD panelen a kezelt könyvtár) panelen válassza ki **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="c4673-109">On the **Azure Active Directory - *directoryname*** blade (that is, the Azure AD blade for the directory you are managing), select **Enterprise applications**.</span></span>

    ![Vállalati alkalmazások megnyitásakor](./media/active-directory-coreapps-assign-user-azure-portal/open-enterprise-apps.png)
4. <span data-ttu-id="c4673-111">Az a **vállalati alkalmazások** panelen válassza **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="c4673-111">On the **Enterprise applications** blade, select **All applications**.</span></span> <span data-ttu-id="c4673-112">Kezelheti az alkalmazások listáját láthatja.</span><span class="sxs-lookup"><span data-stu-id="c4673-112">You'll see a list of the apps you can manage.</span></span>
5. <span data-ttu-id="c4673-113">Az a **vállalati alkalmazások – összes alkalmazás** panelen, jelöljön ki egy alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c4673-113">On the **Enterprise applications - All applications** blade, select an app.</span></span>
6. <span data-ttu-id="c4673-114">Az a ***appname*** (Ez azt jelenti, hogy a panel nevű, a kijelölt alkalmazást a címben) panelen válassza ki **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="c4673-114">On the ***appname*** blade (that is, the blade with the name of the selected app in the title), select **Users & Groups**.</span></span>

    ![Az összes alkalmazások paranccsal](./media/active-directory-coreapps-assign-user-azure-portal/select-app-users.png)
7. <span data-ttu-id="c4673-116">Az a ***appname*** **-felhasználó & csoport-hozzárendelés** panelen válassza a **Hozzáadás** parancsot.</span><span class="sxs-lookup"><span data-stu-id="c4673-116">On the ***appname*** **- User & Group Assignment** blade, select the **Add** command.</span></span>
8. <span data-ttu-id="c4673-117">Az a **hozzáadása hozzárendelés** panelen válassza **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="c4673-117">On the **Add Assignment** blade, select **Users and groups**.</span></span>

    ![Az alkalmazás egy felhasználó vagy csoport hozzárendelése](./media/active-directory-coreapps-assign-user-azure-portal/assign-users.png)
9. <span data-ttu-id="c4673-119">Az a **felhasználók és csoportok** panelen válasszon ki egy vagy több felhasználót vagy csoportot a listából, és válassza ki a **válasszon** gomb a panel alján.</span><span class="sxs-lookup"><span data-stu-id="c4673-119">On the **Users and groups** blade, select one or more users or groups from the list and then select the **Select** button at the bottom of the blade.</span></span>
10. <span data-ttu-id="c4673-120">Az a **hozzáadása hozzárendelés** panelen válassza **szerepkör**.</span><span class="sxs-lookup"><span data-stu-id="c4673-120">On the **Add Assignment** blade, select **Role**.</span></span> <span data-ttu-id="c4673-121">Ezt követően a **Szerepkörválasztás** panelen válassza ki a megfelelő szerepkört a kiválasztott felhasználókra vagy csoportokra alkalmazza, és válassza ki a **OK** gomb a panel alján.</span><span class="sxs-lookup"><span data-stu-id="c4673-121">Then, on the **Select Role** blade, select a role to apply to the selected users or groups, and then select the **OK** button at the bottom of the blade.</span></span>
11. <span data-ttu-id="c4673-122">Az a **hozzáadása hozzárendelés** panelen válassza a **hozzárendelése** gomb a panel alján.</span><span class="sxs-lookup"><span data-stu-id="c4673-122">On the **Add Assignment** blade, select the **Assign** button at the bottom of the blade.</span></span> <span data-ttu-id="c4673-123">A kijelölt felhasználók vagy csoportok a vállalati alkalmazás a kiválasztott szerepkör által meghatározott engedélyekkel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="c4673-123">The assigned users or groups will have the permissions defined by the selected role for this enterprise app.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c4673-124">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c4673-124">Next steps</span></span>
* [<span data-ttu-id="c4673-125">Összes saját csoportok</span><span class="sxs-lookup"><span data-stu-id="c4673-125">See all of my groups</span></span>](active-directory-groups-view-azure-portal.md)
* [<span data-ttu-id="c4673-126">Egy felhasználó vagy csoport-hozzárendelés eltávolítása a vállalati alkalmazások</span><span class="sxs-lookup"><span data-stu-id="c4673-126">Remove a user or group assignment from an enterprise app</span></span>](active-directory-coreapps-remove-assignment-azure-portal.md)
* [<span data-ttu-id="c4673-127">Tiltsa le a felhasználói bejelentkezéseket a vállalati alkalmazás</span><span class="sxs-lookup"><span data-stu-id="c4673-127">Disable user sign-ins for an enterprise app</span></span>](active-directory-coreapps-disable-app-azure-portal.md)
* [<span data-ttu-id="c4673-128">Módosítja a nevét, vagy egy vállalati alkalmazás embléma</span><span class="sxs-lookup"><span data-stu-id="c4673-128">Change the name or logo of an enterprise app</span></span>](active-directory-coreapps-change-app-logo-user-azure-portal.md)
