---
title: "aaaChange hello nevét vagy egy vállalati alkalmazás Azure Active Directoryban embléma |} Microsoft Docs"
description: "Hogyan toochange hello nevét vagy az Azure Active Directoryban egyéni vállalati alkalmazások embléma"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: d01303ce-e6cb-4f3b-a4d6-ec29dfd68146
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: curtand
ms.openlocfilehash: b660e1f0f6c7ffd626e17e2e3399e7169f6e8bc8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="change-hello-name-or-logo-of-an-enterprise-app-in-azure-active-directory"></a><span data-ttu-id="ed618-103">Hello nevének módosítása vagy egy vállalati alkalmazás Azure Active Directoryban embléma</span><span class="sxs-lookup"><span data-stu-id="ed618-103">Change hello name or logo of an enterprise app in Azure Active Directory</span></span>
<span data-ttu-id="ed618-104">Egyszerű toochange hello nevét vagy egy egyéni vállalati alkalmazás Azure Active Directory (Azure AD) embléma.</span><span class="sxs-lookup"><span data-stu-id="ed618-104">It's easy toochange hello name or logo for a custom enterprise application in Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="ed618-105">Rendelkeznie kell hello megfelelő engedélyek toomake ezeket a módosításokat, és be kell hello hello egyéni alkalmazás hozta létre.</span><span class="sxs-lookup"><span data-stu-id="ed618-105">You must have hello appropriate permissions toomake these changes, and you must be hello creator of hello custom app.</span></span>

## <a name="how-do-i-change-an-enterprise-apps-name-or-logo"></a><span data-ttu-id="ed618-106">Hogyan változtathatom meg egy vállalati alkalmazás nevét vagy embléma?</span><span class="sxs-lookup"><span data-stu-id="ed618-106">How do I change an enterprise app's name or logo?</span></span>
1. <span data-ttu-id="ed618-107">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely hello címtár globális rendszergazdája.</span><span class="sxs-lookup"><span data-stu-id="ed618-107">Sign in toohello [Azure portal](https://portal.azure.com) with an account that's a global admin for hello directory.</span></span>
2. <span data-ttu-id="ed618-108">Válassza ki **további szolgáltatások**, adja meg **Azure Active Directory** hello szövegmezőbe, és válassza ki **Enter**.</span><span class="sxs-lookup"><span data-stu-id="ed618-108">Select **More services**, enter **Azure Active Directory** in hello text box, and then select **Enter**.</span></span>
3. <span data-ttu-id="ed618-109">A hello **Azure Active Directory - *directoryname***  (Ez azt jelenti, hogy az Azure AD hello panelen kezelt hello könyvtár) panelen válassza ki **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="ed618-109">On hello **Azure Active Directory - *directoryname*** blade (that is, hello Azure AD blade for hello directory you are managing), select **Enterprise applications**.</span></span>

    ![Vállalati alkalmazások megnyitásakor](./media/active-directory-coreapps-change-app-logo-azure-portal/open-enterprise-apps.png)
4. <span data-ttu-id="ed618-111">A hello **vállalati alkalmazások** panelen válassza **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="ed618-111">On hello **Enterprise applications** blade, select **All applications**.</span></span> <span data-ttu-id="ed618-112">Látni fogja a kezelhető hello alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="ed618-112">You'll see a list of hello apps you can manage.</span></span>
5. <span data-ttu-id="ed618-113">A hello **vállalati alkalmazások – összes alkalmazás** panelen, jelöljön ki egy alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="ed618-113">On hello **Enterprise applications - All applications** blade, select an app.</span></span>
6. <span data-ttu-id="ed618-114">A hello ***appname*** (Ez azt jelenti, hogy hello panelen kiválasztott alkalmazás hello hello címben hello nevű) panelen válassza ki **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="ed618-114">On hello ***appname*** blade (that is, hello blade with hello name of hello selected app in hello title), select **Properties**.</span></span>

    ![Hello Tulajdonságok parancs kiválasztása](./media/active-directory-coreapps-change-app-logo-azure-portal/select-app.png)
7. <span data-ttu-id="ed618-116">A hello ***appname*** **-tulajdonságok** panelen, tallózással keresse meg a fájl toouse, mint egy új embléma, vagy hello alkalmazás neve, vagy mindkettőt szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="ed618-116">On hello ***appname*** **- Properties** blade, browse for a file toouse as a new logo, or edit hello app name, or both.</span></span>

    ![Hello app embléma vagy nameproperties parancs módosítása](./media/active-directory-coreapps-change-app-logo-azure-portal/change-logo.png)
8. <span data-ttu-id="ed618-118">Jelölje be hello **mentése** parancsot.</span><span class="sxs-lookup"><span data-stu-id="ed618-118">Select hello **Save** command.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ed618-119">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ed618-119">Next steps</span></span>
* [<span data-ttu-id="ed618-120">Összes saját csoportok</span><span class="sxs-lookup"><span data-stu-id="ed618-120">See all of my groups</span></span>](active-directory-groups-view-azure-portal.md)
* [<span data-ttu-id="ed618-121">Rendelje hozzá a felhasználó vagy csoport tooan vállalati alkalmazások</span><span class="sxs-lookup"><span data-stu-id="ed618-121">Assign a user or group tooan enterprise app</span></span>](active-directory-coreapps-assign-user-azure-portal.md)
* [<span data-ttu-id="ed618-122">Egy felhasználó vagy csoport-hozzárendelés eltávolítása a vállalati alkalmazások</span><span class="sxs-lookup"><span data-stu-id="ed618-122">Remove a user or group assignment from an enterprise app</span></span>](active-directory-coreapps-remove-assignment-azure-portal.md)
* [<span data-ttu-id="ed618-123">Tiltsa le a felhasználói bejelentkezéseket a vállalati alkalmazás</span><span class="sxs-lookup"><span data-stu-id="ed618-123">Disable user sign-ins for an enterprise app</span></span>](active-directory-coreapps-disable-app-azure-portal.md)
