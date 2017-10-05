---
title: "Egy felhasználó vagy csoport-hozzárendelés eltávolítása egy vállalati alkalmazás Azure Active Directoryban |} Microsoft Docs"
description: "A hozzáférés hozzárendelése egy felhasználóhoz vagy csoporthoz eltávolítása a vállalati alkalmazások az Azure Active Directoryban"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 7b2d365b-ae92-477f-9702-353cc6acc5ea
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: curtand
ms.openlocfilehash: 02f122acfb53c2107e2b0af66c6195aa127a2c77
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="remove-a-user-or-group-assignment-from-an-enterprise-app-in-azure-active-directory"></a><span data-ttu-id="06608-103">Egy felhasználó vagy csoport-hozzárendelés eltávolítása egy vállalati alkalmazás Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="06608-103">Remove a user or group assignment from an enterprise app in Azure Active Directory</span></span>
<span data-ttu-id="06608-104">Nem lett hozzárendelve hozzáférés a vállalati alkalmazások az Azure Active Directory (Azure AD) egyik felhasználó vagy csoport eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="06608-104">It's easy to remove a user or a group from being assigned access to one of your enterprise applications in Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="06608-105">A vállalati alkalmazások kezelésére a megfelelő engedélyekkel kell rendelkeznie, és a címtár globális rendszergazdának kell lennie.</span><span class="sxs-lookup"><span data-stu-id="06608-105">You must have the appropriate permissions to manage the enterprise app, and you must be global admin for the directory.</span></span>

## <a name="how-do-i-remove-a-user-or-group-assignment"></a><span data-ttu-id="06608-106">Hogyan el egy felhasználó vagy csoport-hozzárendelést?</span><span class="sxs-lookup"><span data-stu-id="06608-106">How do I remove a user or group assignment?</span></span>
1. <span data-ttu-id="06608-107">Jelentkezzen be a [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely a címtár globális rendszergazdája.</span><span class="sxs-lookup"><span data-stu-id="06608-107">Sign in to the [Azure portal](https://portal.azure.com) with an account that's a global admin for the directory.</span></span>
2. <span data-ttu-id="06608-108">Válassza ki **további szolgáltatások**, adja meg **Azure Active Directory** a szövegmezőbe, majd válassza ki azt a **Enter**.</span><span class="sxs-lookup"><span data-stu-id="06608-108">Select **More services**, enter **Azure Active Directory** in the text box, and then select **Enter**.</span></span>
3. <span data-ttu-id="06608-109">Az a **Azure Active Directory - *directoryname***  (Ez azt jelenti, hogy az Azure AD panelen a kezelt könyvtár) panelen válassza ki **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="06608-109">On the **Azure Active Directory - *directoryname*** blade (that is, the Azure AD blade for the directory you are managing), select **Enterprise applications**.</span></span>

    ![Vállalati alkalmazások megnyitásakor](./media/active-directory-coreapps-remove-assignment-user-azure-portal/open-enterprise-apps.png)
4. <span data-ttu-id="06608-111">Az a **vállalati alkalmazások** panelen válassza **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="06608-111">On the **Enterprise applications** blade, select **All applications**.</span></span> <span data-ttu-id="06608-112">Kezelheti az alkalmazások listáját láthatja.</span><span class="sxs-lookup"><span data-stu-id="06608-112">You'll see a list of the apps you can manage.</span></span>
5. <span data-ttu-id="06608-113">Az a **vállalati alkalmazások – összes alkalmazás** panelen, jelöljön ki egy alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="06608-113">On the **Enterprise applications - All applications** blade, select an app.</span></span>
6. <span data-ttu-id="06608-114">Az a ***appname*** (Ez azt jelenti, hogy a panel nevű, a kijelölt alkalmazást a címben) panelen válassza ki **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="06608-114">On the ***appname*** blade (that is, the blade with the name of the selected app in the title), select **Users & Groups**.</span></span>

    ![Felhasználók vagy csoportok kiválasztása](./media/active-directory-coreapps-remove-assignment-user-azure-portal/remove-app-users.png)
7. <span data-ttu-id="06608-116">A a ***appname*** **-felhasználó & csoport-hozzárendelés** panelen válasszon egyet a további felhasználókat és csoportokat, és válassza ki a **eltávolítása** parancsot.</span><span class="sxs-lookup"><span data-stu-id="06608-116">On the ***appname*** **- User & Group Assignment** blade, select one of more users or groups and then select the **Remove** command.</span></span> <span data-ttu-id="06608-117">Erősítse meg a döntést, a parancssorba.</span><span class="sxs-lookup"><span data-stu-id="06608-117">Confirm your decision at the prompt.</span></span>

    ![A Remove parancsot kiválasztása](./media/active-directory-coreapps-remove-assignment-user-azure-portal/remove-users.png)

## <a name="next-steps"></a><span data-ttu-id="06608-119">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="06608-119">Next steps</span></span>
* [<span data-ttu-id="06608-120">Összes saját csoportok</span><span class="sxs-lookup"><span data-stu-id="06608-120">See all of my groups</span></span>](active-directory-groups-view-azure-portal.md)
* [<span data-ttu-id="06608-121">Egy felhasználó vagy csoport hozzárendelése egy vállalati alkalmazás</span><span class="sxs-lookup"><span data-stu-id="06608-121">Assign a user or group to an enterprise app</span></span>](active-directory-coreapps-assign-user-azure-portal.md)
* [<span data-ttu-id="06608-122">Tiltsa le a felhasználói bejelentkezéseket a vállalati alkalmazás</span><span class="sxs-lookup"><span data-stu-id="06608-122">Disable user sign-ins for an enterprise app</span></span>](active-directory-coreapps-disable-app-azure-portal.md)
* [<span data-ttu-id="06608-123">Módosítja a nevét, vagy egy vállalati alkalmazás embléma</span><span class="sxs-lookup"><span data-stu-id="06608-123">Change the name or logo of an enterprise app</span></span>](active-directory-coreapps-change-app-logo-user-azure-portal.md)
