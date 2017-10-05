---
title: "Módosítja a nevét, vagy egy vállalati alkalmazás Azure Active Directoryban embléma |} Microsoft Docs"
description: "Hogyan módosítja a nevét vagy az Azure Active Directoryban egyéni vállalati alkalmazások embléma"
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
ms.openlocfilehash: 3e44e876dcbac704a9809ae5b3957bf94be21c48
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="change-the-name-or-logo-of-an-enterprise-app-in-azure-active-directory"></a><span data-ttu-id="83fcb-103">Módosítja a nevét, vagy egy vállalati alkalmazás Azure Active Directoryban embléma</span><span class="sxs-lookup"><span data-stu-id="83fcb-103">Change the name or logo of an enterprise app in Azure Active Directory</span></span>
<span data-ttu-id="83fcb-104">Nem módosítja a nevét, vagy egy egyéni vállalati alkalmazás Azure Active Directory (Azure AD) emblémának.</span><span class="sxs-lookup"><span data-stu-id="83fcb-104">It's easy to change the name or logo for a custom enterprise application in Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="83fcb-105">Az ilyen jellegű változtatásokat a megfelelő engedélyekkel kell rendelkeznie, és be kell az egyéni alkalmazás hozta létre.</span><span class="sxs-lookup"><span data-stu-id="83fcb-105">You must have the appropriate permissions to make these changes, and you must be the creator of the custom app.</span></span>

## <a name="how-do-i-change-an-enterprise-apps-name-or-logo"></a><span data-ttu-id="83fcb-106">Hogyan változtathatom meg egy vállalati alkalmazás nevét vagy embléma?</span><span class="sxs-lookup"><span data-stu-id="83fcb-106">How do I change an enterprise app's name or logo?</span></span>
1. <span data-ttu-id="83fcb-107">Jelentkezzen be a [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely a címtár globális rendszergazdája.</span><span class="sxs-lookup"><span data-stu-id="83fcb-107">Sign in to the [Azure portal](https://portal.azure.com) with an account that's a global admin for the directory.</span></span>
2. <span data-ttu-id="83fcb-108">Válassza ki **további szolgáltatások**, adja meg **Azure Active Directory** a szövegmezőbe, majd válassza ki azt a **Enter**.</span><span class="sxs-lookup"><span data-stu-id="83fcb-108">Select **More services**, enter **Azure Active Directory** in the text box, and then select **Enter**.</span></span>
3. <span data-ttu-id="83fcb-109">Az a **Azure Active Directory - *directoryname***  (Ez azt jelenti, hogy az Azure AD panelen a kezelt könyvtár) panelen válassza ki **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="83fcb-109">On the **Azure Active Directory - *directoryname*** blade (that is, the Azure AD blade for the directory you are managing), select **Enterprise applications**.</span></span>

    ![Vállalati alkalmazások megnyitásakor](./media/active-directory-coreapps-change-app-logo-azure-portal/open-enterprise-apps.png)
4. <span data-ttu-id="83fcb-111">Az a **vállalati alkalmazások** panelen válassza **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="83fcb-111">On the **Enterprise applications** blade, select **All applications**.</span></span> <span data-ttu-id="83fcb-112">Kezelheti az alkalmazások listáját láthatja.</span><span class="sxs-lookup"><span data-stu-id="83fcb-112">You'll see a list of the apps you can manage.</span></span>
5. <span data-ttu-id="83fcb-113">Az a **vállalati alkalmazások – összes alkalmazás** panelen, jelöljön ki egy alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="83fcb-113">On the **Enterprise applications - All applications** blade, select an app.</span></span>
6. <span data-ttu-id="83fcb-114">Az a ***appname*** (Ez azt jelenti, hogy a panel nevű, a kijelölt alkalmazást a címben) panelen válassza ki **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="83fcb-114">On the ***appname*** blade (that is, the blade with the name of the selected app in the title), select **Properties**.</span></span>

    ![A Tulajdonságok parancs kiválasztása](./media/active-directory-coreapps-change-app-logo-azure-portal/select-app.png)
7. <span data-ttu-id="83fcb-116">Az a ***appname*** **-tulajdonságok** panelen, keresse meg egy fájlt egy új embléma használják, vagy szerkesztheti az alkalmazás neve, vagy mindkettőt.</span><span class="sxs-lookup"><span data-stu-id="83fcb-116">On the ***appname*** **- Properties** blade, browse for a file to use as a new logo, or edit the app name, or both.</span></span>

    ![Az alkalmazás embléma vagy nameproperties parancs módosítása](./media/active-directory-coreapps-change-app-logo-azure-portal/change-logo.png)
8. <span data-ttu-id="83fcb-118">Válassza ki a **mentése** parancsot.</span><span class="sxs-lookup"><span data-stu-id="83fcb-118">Select the **Save** command.</span></span>

## <a name="next-steps"></a><span data-ttu-id="83fcb-119">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="83fcb-119">Next steps</span></span>
* [<span data-ttu-id="83fcb-120">Összes saját csoportok</span><span class="sxs-lookup"><span data-stu-id="83fcb-120">See all of my groups</span></span>](active-directory-groups-view-azure-portal.md)
* [<span data-ttu-id="83fcb-121">Egy felhasználó vagy csoport hozzárendelése egy vállalati alkalmazás</span><span class="sxs-lookup"><span data-stu-id="83fcb-121">Assign a user or group to an enterprise app</span></span>](active-directory-coreapps-assign-user-azure-portal.md)
* [<span data-ttu-id="83fcb-122">Egy felhasználó vagy csoport-hozzárendelés eltávolítása a vállalati alkalmazások</span><span class="sxs-lookup"><span data-stu-id="83fcb-122">Remove a user or group assignment from an enterprise app</span></span>](active-directory-coreapps-remove-assignment-azure-portal.md)
* [<span data-ttu-id="83fcb-123">Tiltsa le a felhasználói bejelentkezéseket a vállalati alkalmazás</span><span class="sxs-lookup"><span data-stu-id="83fcb-123">Disable user sign-ins for an enterprise app</span></span>](active-directory-coreapps-disable-app-azure-portal.md)
