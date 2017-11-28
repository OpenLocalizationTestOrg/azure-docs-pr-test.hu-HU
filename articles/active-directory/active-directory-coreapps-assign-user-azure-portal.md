---
title: "a felhasználó vagy csoport tooan vállalati alkalmazások az Azure Active Directoryban aaaAssign |} Microsoft Docs"
description: "Hogyan tooselect egy vállalati alkalmazás tooassign egy felhasználó vagy csoport tooit az Azure Active Directoryban"
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
ms.openlocfilehash: 86c11f19892b9c947a5331677c17759178ed2806
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="assign-a-user-or-group-tooan-enterprise-app-in-azure-active-directory"></a><span data-ttu-id="b9568-103">Rendelje hozzá a felhasználó vagy csoport tooan vállalati alkalmazások az Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="b9568-103">Assign a user or group tooan enterprise app in Azure Active Directory</span></span>
<span data-ttu-id="b9568-104">Fontos, hogy könnyen tooassign egy felhasználó vagy egy csoport tooyour vállalati alkalmazások az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="b9568-104">It's easy tooassign a user or a group tooyour enterprise applications in Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="b9568-105">Rendelkeznie kell hello megfelelő engedélyek toomanage hello vállalati alkalmazást, és hello címtár globális rendszergazdának kell lennie.</span><span class="sxs-lookup"><span data-stu-id="b9568-105">You must have hello appropriate permissions toomanage hello enterprise app, and you must be global admin for hello directory.</span></span>

## <a name="how-do-i-assign-user-access-tooan-enterprise-app"></a><span data-ttu-id="b9568-106">Hogyan oszthatok ki a felhasználói hozzáférés tooan vállalati alkalmazást?</span><span class="sxs-lookup"><span data-stu-id="b9568-106">How do I assign user access tooan enterprise app?</span></span>
1. <span data-ttu-id="b9568-107">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely hello címtár globális rendszergazdája.</span><span class="sxs-lookup"><span data-stu-id="b9568-107">Sign in toohello [Azure portal](https://portal.azure.com) with an account that's a global admin for hello directory.</span></span>
2. <span data-ttu-id="b9568-108">Válassza ki **további szolgáltatások**, Azure Active Directory hello mezőben adja meg, és válassza **Enter**.</span><span class="sxs-lookup"><span data-stu-id="b9568-108">Select **More services**, enter Azure Active Directory in hello text box, and then select **Enter**.</span></span>
3. <span data-ttu-id="b9568-109">A hello **Azure Active Directory - *directoryname***  (Ez azt jelenti, hogy az Azure AD hello panelen kezelt hello könyvtár) panelen válassza ki **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="b9568-109">On hello **Azure Active Directory - *directoryname*** blade (that is, hello Azure AD blade for hello directory you are managing), select **Enterprise applications**.</span></span>

    ![Vállalati alkalmazások megnyitásakor](./media/active-directory-coreapps-assign-user-azure-portal/open-enterprise-apps.png)
4. <span data-ttu-id="b9568-111">A hello **vállalati alkalmazások** panelen válassza **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="b9568-111">On hello **Enterprise applications** blade, select **All applications**.</span></span> <span data-ttu-id="b9568-112">Látni fogja a kezelhető hello alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="b9568-112">You'll see a list of hello apps you can manage.</span></span>
5. <span data-ttu-id="b9568-113">A hello **vállalati alkalmazások – összes alkalmazás** panelen, jelöljön ki egy alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="b9568-113">On hello **Enterprise applications - All applications** blade, select an app.</span></span>
6. <span data-ttu-id="b9568-114">A hello ***appname*** (Ez azt jelenti, hogy hello panelen kiválasztott alkalmazás hello hello címben hello nevű) panelen válassza ki **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="b9568-114">On hello ***appname*** blade (that is, hello blade with hello name of hello selected app in hello title), select **Users & Groups**.</span></span>

    ![Minden alkalmazások parancs kiválasztásával hello](./media/active-directory-coreapps-assign-user-azure-portal/select-app-users.png)
7. <span data-ttu-id="b9568-116">A hello ***appname*** **-felhasználó & csoport-hozzárendelés** panelen, jelölje be hello **Hozzáadás** parancsot.</span><span class="sxs-lookup"><span data-stu-id="b9568-116">On hello ***appname*** **- User & Group Assignment** blade, select hello **Add** command.</span></span>
8. <span data-ttu-id="b9568-117">A hello **hozzáadása hozzárendelés** panelen válassza **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="b9568-117">On hello **Add Assignment** blade, select **Users and groups**.</span></span>

    ![Egy felhasználó vagy csoport toohello alkalmazás hozzárendelése](./media/active-directory-coreapps-assign-user-azure-portal/assign-users.png)
9. <span data-ttu-id="b9568-119">A hello **felhasználók és csoportok** panelen, jelölje be egy vagy több felhasználók vagy csoportok hello a listában, és válassza a hello **válasszon** hello hello panel alsó részén gombra.</span><span class="sxs-lookup"><span data-stu-id="b9568-119">On hello **Users and groups** blade, select one or more users or groups from hello list and then select hello **Select** button at hello bottom of hello blade.</span></span>
10. <span data-ttu-id="b9568-120">A hello **hozzáadása hozzárendelés** panelen válassza **szerepkör**.</span><span class="sxs-lookup"><span data-stu-id="b9568-120">On hello **Add Assignment** blade, select **Role**.</span></span> <span data-ttu-id="b9568-121">Ezután a hello **Szerepkörválasztás** panelen, jelölje ki a szerepkör tooapply toohello kijelölt felhasználók vagy csoportok, és válassza ki hello **OK** hello hello panel alsó részén gombra.</span><span class="sxs-lookup"><span data-stu-id="b9568-121">Then, on hello **Select Role** blade, select a role tooapply toohello selected users or groups, and then select hello **OK** button at hello bottom of hello blade.</span></span>
11. <span data-ttu-id="b9568-122">A hello **hozzáadása hozzárendelés** panelen, jelölje be hello **hozzárendelése** hello hello panel alsó részén gombra.</span><span class="sxs-lookup"><span data-stu-id="b9568-122">On hello **Add Assignment** blade, select hello **Assign** button at hello bottom of hello blade.</span></span> <span data-ttu-id="b9568-123">hello hozzárendelt felhasználók vagy csoportok hello kijelölt szerepkör vállalati alkalmazás által meghatározott hello engedélyekkel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="b9568-123">hello assigned users or groups will have hello permissions defined by hello selected role for this enterprise app.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b9568-124">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b9568-124">Next steps</span></span>
* [<span data-ttu-id="b9568-125">Összes saját csoportok</span><span class="sxs-lookup"><span data-stu-id="b9568-125">See all of my groups</span></span>](active-directory-groups-view-azure-portal.md)
* [<span data-ttu-id="b9568-126">Egy felhasználó vagy csoport-hozzárendelés eltávolítása a vállalati alkalmazások</span><span class="sxs-lookup"><span data-stu-id="b9568-126">Remove a user or group assignment from an enterprise app</span></span>](active-directory-coreapps-remove-assignment-azure-portal.md)
* [<span data-ttu-id="b9568-127">Tiltsa le a felhasználói bejelentkezéseket a vállalati alkalmazás</span><span class="sxs-lookup"><span data-stu-id="b9568-127">Disable user sign-ins for an enterprise app</span></span>](active-directory-coreapps-disable-app-azure-portal.md)
* [<span data-ttu-id="b9568-128">Hello nevének módosítása vagy egy vállalati alkalmazás embléma</span><span class="sxs-lookup"><span data-stu-id="b9568-128">Change hello name or logo of an enterprise app</span></span>](active-directory-coreapps-change-app-logo-user-azure-portal.md)
