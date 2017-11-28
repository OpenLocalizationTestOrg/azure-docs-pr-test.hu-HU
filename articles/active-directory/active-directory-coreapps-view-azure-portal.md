---
title: "Az Azure Active Directoryban is kezelhető az összes vállalati alkalmazást megtekintése |} Microsoft Docs"
description: "A vállalati alkalmazásokat kezeléséhez az Azure Active Directoryban a engedélyekkel rendelkező megtekintése"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: c4fb6f94-34f8-4323-8bd7-a3ee44901f7d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: curtand
ms.openlocfilehash: 14b335d14d893640d469508d6f34b4e7ec6bee8b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="view-all-the-enterprise-apps-that-i-can-manage-in-azure-active-directory"></a><span data-ttu-id="f1e48-103">Az Azure Active Directoryban is kezelhető az összes vállalati alkalmazást megtekintése</span><span class="sxs-lookup"><span data-stu-id="f1e48-103">View all the enterprise apps that I can manage in Azure Active Directory</span></span>
<span data-ttu-id="f1e48-104">Kezelheti a vállalati alkalmazások az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="f1e48-104">You can manage your enterprise applications in the Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="f1e48-105">Ez magában foglalja, megtekintés, alkalmazásokat, kezelheti, hozzárendelése felhasználókat és csoportokat egy alkalmazáshoz, az alkalmazás például az alkalmazás neve/embléma tulajdonságok karbantartása és, hogy a felhasználók nem lehet bejelentkezni, még akkor is letiltása a kérelmet.</span><span class="sxs-lookup"><span data-stu-id="f1e48-105">This includes viewing the apps you can manage, assigning users or groups to an app, maintaining properties for the app such as the application name/logo, and even disabling an application so that no users can sign in to it.</span></span>

## <a name="how-do-i-view-all-my-apps"></a><span data-ttu-id="f1e48-106">Hogyan tekinthetem meg az alkalmazások?</span><span class="sxs-lookup"><span data-stu-id="f1e48-106">How do I view all my apps?</span></span>
1. <span data-ttu-id="f1e48-107">Jelentkezzen be a [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely a címtár globális rendszergazdája.</span><span class="sxs-lookup"><span data-stu-id="f1e48-107">Sign in to the [Azure portal](https://portal.azure.com) with an account that's a global admin for the directory.</span></span>
2. <span data-ttu-id="f1e48-108">Válassza ki **további szolgáltatások**, adja meg **Azure Active Directory** a szövegmezőbe, majd válassza ki azt a **Enter**.</span><span class="sxs-lookup"><span data-stu-id="f1e48-108">Select **More services**, enter **Azure Active Directory** in the text box, and then select **Enter**.</span></span>
3. <span data-ttu-id="f1e48-109">Az a **Azure Active Directory -** ***directoryname*** (Ez azt jelenti, hogy az Azure AD panelen a kezelt könyvtár) panelen válassza ki **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="f1e48-109">On the **Azure Active Directory -** ***directoryname*** blade (that is, the Azure AD blade for the directory you are managing), select **Enterprise applications**.</span></span>

    ![Vállalati alkalmazások megnyitásakor](./media/active-directory-coreapps-view-azure-portal/open-enterprise-apps.png)
4. <span data-ttu-id="f1e48-111">Az a **vállalati alkalmazások** panelen válassza **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="f1e48-111">On the **Enterprise applications** blade, select **All applications**.</span></span> <span data-ttu-id="f1e48-112">Ezen a panelen kiválaszthatja alkalmazásokat kezeléséhez, a megjelenített oszlopok sorrendjének megváltoztatásához vagy található (például csak a letiltott alkalmazások) kívánt alkalmazást a tulajdonságlista szűréséhez.</span><span class="sxs-lookup"><span data-stu-id="f1e48-112">From this blade you can select apps to manage, change the displayed columns, or filter the list to find that app you want (for example, to view only disabled apps).</span></span>

## <a name="next-steps"></a><span data-ttu-id="f1e48-113">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f1e48-113">Next steps</span></span>
* [<span data-ttu-id="f1e48-114">Egy felhasználó vagy csoport hozzárendelése egy vállalati alkalmazás</span><span class="sxs-lookup"><span data-stu-id="f1e48-114">Assign a user or group to an enterprise app</span></span>](active-directory-coreapps-assign-user-azure-portal.md)
* [<span data-ttu-id="f1e48-115">Egy felhasználó vagy csoport-hozzárendelés eltávolítása a vállalati alkalmazások</span><span class="sxs-lookup"><span data-stu-id="f1e48-115">Remove a user or group assignment from an enterprise app</span></span>](active-directory-coreapps-remove-assignment-azure-portal.md)
* [<span data-ttu-id="f1e48-116">Tiltsa le a felhasználói bejelentkezéseket a vállalati alkalmazás</span><span class="sxs-lookup"><span data-stu-id="f1e48-116">Disable user sign-ins for an enterprise app</span></span>](active-directory-coreapps-disable-app-azure-portal.md)
* [<span data-ttu-id="f1e48-117">Módosítja a nevét, vagy egy vállalati alkalmazás embléma</span><span class="sxs-lookup"><span data-stu-id="f1e48-117">Change the name or logo of an enterprise app</span></span>](active-directory-coreapps-change-app-logo-user-azure-portal.md)
