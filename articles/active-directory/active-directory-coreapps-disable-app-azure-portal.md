---
title: "Tiltsa le a felhasználói bejelentkezések Azure Active Directory vállalati alkalmazás |} Microsoft Docs"
description: "Egy vállalati alkalmazás letiltása, hogy egy felhasználó sem lehet, hogy jelentkezzen be az Azure Active Directoryban"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: a27562f9-18dc-42e8-9fee-5419566f8fd7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: curtand
ms.openlocfilehash: 5d27046370eada0c371c94fb573fa1bcf536f7cf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="disable-user-sign-ins-for-an-enterprise-app-in-azure-active-directory"></a><span data-ttu-id="e1ef9-103">Tiltsa le a felhasználói bejelentkezéseket a vállalati alkalmazás Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="e1ef9-103">Disable user sign-ins for an enterprise app in Azure Active Directory</span></span>
<span data-ttu-id="e1ef9-104">Akkor is könnyen tiltsa le a vállalati alkalmazás, így a felhasználók nem lehetséges, hogy jelentkezzen be azt az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="e1ef9-104">It's easy to disable an enterprise application so that no users may sign in to it in Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="e1ef9-105">A vállalati alkalmazások kezelésére a megfelelő engedélyekkel kell rendelkeznie, és a címtár globális rendszergazdának kell lennie.</span><span class="sxs-lookup"><span data-stu-id="e1ef9-105">You must have the appropriate permissions to manage the enterprise app, and you must be global admin for the directory.</span></span>

## <a name="how-do-i-disable-user-sign-ins"></a><span data-ttu-id="e1ef9-106">Hogyan tiltsa le a felhasználói bejelentkezéseket?</span><span class="sxs-lookup"><span data-stu-id="e1ef9-106">How do I disable user sign-ins?</span></span>
1. <span data-ttu-id="e1ef9-107">Jelentkezzen be a [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely a címtár globális rendszergazdája.</span><span class="sxs-lookup"><span data-stu-id="e1ef9-107">Sign in to the [Azure portal](https://portal.azure.com) with an account that's a global admin for the directory.</span></span>
2. <span data-ttu-id="e1ef9-108">Válassza ki **további szolgáltatások**, adja meg **Azure Active Directory** a szövegmezőbe, majd válassza ki azt a **Enter**.</span><span class="sxs-lookup"><span data-stu-id="e1ef9-108">Select **More services**, enter **Azure Active Directory** in the text box, and then select **Enter**.</span></span>
3. <span data-ttu-id="e1ef9-109">Az a **Azure Active Directory** -  ***directoryname*** (Ez azt jelenti, hogy az Azure AD panelen a kezelt könyvtár) panelen válassza ki **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="e1ef9-109">On the **Azure Active Directory** -  ***directoryname*** blade (that is, the Azure AD blade for the directory you are managing), select **Enterprise applications**.</span></span>

    ![Vállalati alkalmazások megnyitásakor](./media/active-directory-coreapps-disable-app-azure-portal/open-enterprise-apps.png)
4. <span data-ttu-id="e1ef9-111">Az a **vállalati alkalmazások** panelen válassza **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="e1ef9-111">On the **Enterprise applications** blade, select **All applications**.</span></span> <span data-ttu-id="e1ef9-112">Kezelheti az alkalmazások listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="e1ef9-112">You see a list of the apps you can manage.</span></span>
5. <span data-ttu-id="e1ef9-113">Az a **vállalati alkalmazások – összes alkalmazás** panelen, jelöljön ki egy alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="e1ef9-113">On the **Enterprise applications - All applications** blade, select an app.</span></span>
6. <span data-ttu-id="e1ef9-114">Az a ***appname*** (Ez azt jelenti, hogy a panel nevű, a kijelölt alkalmazást a címben) panelen válassza ki **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="e1ef9-114">On the ***appname*** blade (that is, the blade with the name of the selected app in the title), select **Properties**.</span></span>

    ![Az összes alkalmazások paranccsal](./media/active-directory-coreapps-disable-app-azure-portal/select-app.png)
7. <span data-ttu-id="e1ef9-116">Az a ***appname*** - **tulajdonságok** panelen válassza **nem** a **engedélyezve van, hogy a felhasználók bejelentkezési?**.</span><span class="sxs-lookup"><span data-stu-id="e1ef9-116">On the ***appname*** - **Properties** blade, select **No** for **Enabled for users to sign-in?**.</span></span>
8. <span data-ttu-id="e1ef9-117">Válassza ki a **mentése** parancsot.</span><span class="sxs-lookup"><span data-stu-id="e1ef9-117">Select the **Save** command.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e1ef9-118">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e1ef9-118">Next steps</span></span>
* [<span data-ttu-id="e1ef9-119">Összes csoport megtekintéshez</span><span class="sxs-lookup"><span data-stu-id="e1ef9-119">See all my groups</span></span>](active-directory-groups-view-azure-portal.md)
* [<span data-ttu-id="e1ef9-120">Egy felhasználó vagy csoport hozzárendelése egy vállalati alkalmazás</span><span class="sxs-lookup"><span data-stu-id="e1ef9-120">Assign a user or group to an enterprise app</span></span>](active-directory-coreapps-assign-user-azure-portal.md)
* [<span data-ttu-id="e1ef9-121">Egy felhasználó vagy csoport-hozzárendelés eltávolítása a vállalati alkalmazások</span><span class="sxs-lookup"><span data-stu-id="e1ef9-121">Remove a user or group assignment from an enterprise app</span></span>](active-directory-coreapps-remove-assignment-azure-portal.md)
* [<span data-ttu-id="e1ef9-122">Módosítja a nevét, vagy egy vállalati alkalmazás embléma</span><span class="sxs-lookup"><span data-stu-id="e1ef9-122">Change the name or logo of an enterprise app</span></span>](active-directory-coreapps-change-app-logo-user-azure-portal.md)
