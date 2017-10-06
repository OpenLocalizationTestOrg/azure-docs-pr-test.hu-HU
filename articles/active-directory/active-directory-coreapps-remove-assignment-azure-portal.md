---
title: "egy felhasználó vagy csoport-hozzárendelést egy vállalati alkalmazás Azure Active Directoryban aaaRemove |} Microsoft Docs"
description: "Hogyan tooremove hello férhetnek hozzá a felhasználó vagy csoport hozzárendelése egy vállalati alkalmazás Azure Active Directoryban"
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
ms.openlocfilehash: c067ecf59b4dedfe8f848357ca8bd545bdc610eb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="remove-a-user-or-group-assignment-from-an-enterprise-app-in-azure-active-directory"></a><span data-ttu-id="38fdf-103">Egy felhasználó vagy csoport-hozzárendelés eltávolítása egy vállalati alkalmazás Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="38fdf-103">Remove a user or group assignment from an enterprise app in Azure Active Directory</span></span>
<span data-ttu-id="38fdf-104">A felhasználó könnyen tooremove vagy folyik a hozzárendelése az Azure Active Directory (Azure AD) a vállalati alkalmazások hozzáférési tooone csoportot is.</span><span class="sxs-lookup"><span data-stu-id="38fdf-104">It's easy tooremove a user or a group from being assigned access tooone of your enterprise applications in Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="38fdf-105">Rendelkeznie kell hello megfelelő engedélyek toomanage hello vállalati alkalmazást, és hello címtár globális rendszergazdának kell lennie.</span><span class="sxs-lookup"><span data-stu-id="38fdf-105">You must have hello appropriate permissions toomanage hello enterprise app, and you must be global admin for hello directory.</span></span>

## <a name="how-do-i-remove-a-user-or-group-assignment"></a><span data-ttu-id="38fdf-106">Hogyan el egy felhasználó vagy csoport-hozzárendelést?</span><span class="sxs-lookup"><span data-stu-id="38fdf-106">How do I remove a user or group assignment?</span></span>
1. <span data-ttu-id="38fdf-107">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely hello címtár globális rendszergazdája.</span><span class="sxs-lookup"><span data-stu-id="38fdf-107">Sign in toohello [Azure portal](https://portal.azure.com) with an account that's a global admin for hello directory.</span></span>
2. <span data-ttu-id="38fdf-108">Válassza ki **további szolgáltatások**, adja meg **Azure Active Directory** hello szövegmezőbe, és válassza ki **Enter**.</span><span class="sxs-lookup"><span data-stu-id="38fdf-108">Select **More services**, enter **Azure Active Directory** in hello text box, and then select **Enter**.</span></span>
3. <span data-ttu-id="38fdf-109">A hello **Azure Active Directory - *directoryname***  (Ez azt jelenti, hogy az Azure AD hello panelen kezelt hello könyvtár) panelen válassza ki **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="38fdf-109">On hello **Azure Active Directory - *directoryname*** blade (that is, hello Azure AD blade for hello directory you are managing), select **Enterprise applications**.</span></span>

    ![Vállalati alkalmazások megnyitásakor](./media/active-directory-coreapps-remove-assignment-user-azure-portal/open-enterprise-apps.png)
4. <span data-ttu-id="38fdf-111">A hello **vállalati alkalmazások** panelen válassza **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="38fdf-111">On hello **Enterprise applications** blade, select **All applications**.</span></span> <span data-ttu-id="38fdf-112">Látni fogja a kezelhető hello alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="38fdf-112">You'll see a list of hello apps you can manage.</span></span>
5. <span data-ttu-id="38fdf-113">A hello **vállalati alkalmazások – összes alkalmazás** panelen, jelöljön ki egy alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="38fdf-113">On hello **Enterprise applications - All applications** blade, select an app.</span></span>
6. <span data-ttu-id="38fdf-114">A hello ***appname*** (Ez azt jelenti, hogy hello panelen kiválasztott alkalmazás hello hello címben hello nevű) panelen válassza ki **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="38fdf-114">On hello ***appname*** blade (that is, hello blade with hello name of hello selected app in hello title), select **Users & Groups**.</span></span>

    ![Felhasználók vagy csoportok kiválasztása](./media/active-directory-coreapps-remove-assignment-user-azure-portal/remove-app-users.png)
7. <span data-ttu-id="38fdf-116">A hello ***appname*** **-felhasználó & csoport-hozzárendelés** panelen válasszon egyet a további felhasználókat és csoportokat, és válassza a hello **eltávolítása** parancsot.</span><span class="sxs-lookup"><span data-stu-id="38fdf-116">On hello ***appname*** **- User & Group Assignment** blade, select one of more users or groups and then select hello **Remove** command.</span></span> <span data-ttu-id="38fdf-117">Erősítse meg a döntést, hello a parancssorból.</span><span class="sxs-lookup"><span data-stu-id="38fdf-117">Confirm your decision at hello prompt.</span></span>

    ![Hello eltávolítása paranccsal](./media/active-directory-coreapps-remove-assignment-user-azure-portal/remove-users.png)

## <a name="next-steps"></a><span data-ttu-id="38fdf-119">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="38fdf-119">Next steps</span></span>
* [<span data-ttu-id="38fdf-120">Összes saját csoportok</span><span class="sxs-lookup"><span data-stu-id="38fdf-120">See all of my groups</span></span>](active-directory-groups-view-azure-portal.md)
* [<span data-ttu-id="38fdf-121">Rendelje hozzá a felhasználó vagy csoport tooan vállalati alkalmazások</span><span class="sxs-lookup"><span data-stu-id="38fdf-121">Assign a user or group tooan enterprise app</span></span>](active-directory-coreapps-assign-user-azure-portal.md)
* [<span data-ttu-id="38fdf-122">Tiltsa le a felhasználói bejelentkezéseket a vállalati alkalmazás</span><span class="sxs-lookup"><span data-stu-id="38fdf-122">Disable user sign-ins for an enterprise app</span></span>](active-directory-coreapps-disable-app-azure-portal.md)
* [<span data-ttu-id="38fdf-123">Hello nevének módosítása vagy egy vállalati alkalmazás embléma</span><span class="sxs-lookup"><span data-stu-id="38fdf-123">Change hello name or logo of an enterprise app</span></span>](active-directory-coreapps-change-app-logo-user-azure-portal.md)
