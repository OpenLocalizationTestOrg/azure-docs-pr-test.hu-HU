---
title: "aaaDisable felhasználói bejelentkezések Azure Active Directory vállalati alkalmazás |} Microsoft Docs"
description: "Hogyan toodisable egy vállalati alkalmazás, hogy a felhasználók nem lehetséges, hogy jelentkezzen be az Azure Active Directoryban tooit"
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
ms.openlocfilehash: 4c560b59359d433b0852a7606cc2cc0204866234
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="disable-user-sign-ins-for-an-enterprise-app-in-azure-active-directory"></a><span data-ttu-id="2d7a9-103">Tiltsa le a felhasználói bejelentkezéseket a vállalati alkalmazás Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="2d7a9-103">Disable user sign-ins for an enterprise app in Azure Active Directory</span></span>
<span data-ttu-id="2d7a9-104">Könnyen toodisable egy vállalati alkalmazás, hogy a felhasználók nem lehetséges, hogy jelentkezzen be az Azure Active Directory (Azure AD) tooit.</span><span class="sxs-lookup"><span data-stu-id="2d7a9-104">It's easy toodisable an enterprise application so that no users may sign in tooit in Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="2d7a9-105">Rendelkeznie kell hello megfelelő engedélyek toomanage hello vállalati alkalmazást, és hello címtár globális rendszergazdának kell lennie.</span><span class="sxs-lookup"><span data-stu-id="2d7a9-105">You must have hello appropriate permissions toomanage hello enterprise app, and you must be global admin for hello directory.</span></span>

## <a name="how-do-i-disable-user-sign-ins"></a><span data-ttu-id="2d7a9-106">Hogyan tiltsa le a felhasználói bejelentkezéseket?</span><span class="sxs-lookup"><span data-stu-id="2d7a9-106">How do I disable user sign-ins?</span></span>
1. <span data-ttu-id="2d7a9-107">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely hello címtár globális rendszergazdája.</span><span class="sxs-lookup"><span data-stu-id="2d7a9-107">Sign in toohello [Azure portal](https://portal.azure.com) with an account that's a global admin for hello directory.</span></span>
2. <span data-ttu-id="2d7a9-108">Válassza ki **további szolgáltatások**, adja meg **Azure Active Directory** hello szövegmezőbe, és válassza ki **Enter**.</span><span class="sxs-lookup"><span data-stu-id="2d7a9-108">Select **More services**, enter **Azure Active Directory** in hello text box, and then select **Enter**.</span></span>
3. <span data-ttu-id="2d7a9-109">A hello **Azure Active Directory** -  ***directoryname*** (Ez azt jelenti, hogy az Azure AD hello panelen kezelt hello könyvtár) panelen válassza ki **vállalatialkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="2d7a9-109">On hello **Azure Active Directory** -  ***directoryname*** blade (that is, hello Azure AD blade for hello directory you are managing), select **Enterprise applications**.</span></span>

    ![Vállalati alkalmazások megnyitásakor](./media/active-directory-coreapps-disable-app-azure-portal/open-enterprise-apps.png)
4. <span data-ttu-id="2d7a9-111">A hello **vállalati alkalmazások** panelen válassza **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="2d7a9-111">On hello **Enterprise applications** blade, select **All applications**.</span></span> <span data-ttu-id="2d7a9-112">Kezelheti a hello alkalmazások listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="2d7a9-112">You see a list of hello apps you can manage.</span></span>
5. <span data-ttu-id="2d7a9-113">A hello **vállalati alkalmazások – összes alkalmazás** panelen, jelöljön ki egy alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="2d7a9-113">On hello **Enterprise applications - All applications** blade, select an app.</span></span>
6. <span data-ttu-id="2d7a9-114">A hello ***appname*** (Ez azt jelenti, hogy hello panelen kiválasztott alkalmazás hello hello címben hello nevű) panelen válassza ki **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="2d7a9-114">On hello ***appname*** blade (that is, hello blade with hello name of hello selected app in hello title), select **Properties**.</span></span>

    ![Minden alkalmazások parancs kiválasztásával hello](./media/active-directory-coreapps-disable-app-azure-portal/select-app.png)
7. <span data-ttu-id="2d7a9-116">A hello ***appname*** - **tulajdonságok** panelen válassza **nem** a **toosign lévő felhasználók számára engedélyezett?**.</span><span class="sxs-lookup"><span data-stu-id="2d7a9-116">On hello ***appname*** - **Properties** blade, select **No** for **Enabled for users toosign-in?**.</span></span>
8. <span data-ttu-id="2d7a9-117">Jelölje be hello **mentése** parancsot.</span><span class="sxs-lookup"><span data-stu-id="2d7a9-117">Select hello **Save** command.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2d7a9-118">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2d7a9-118">Next steps</span></span>
* [<span data-ttu-id="2d7a9-119">Összes csoport megtekintéshez</span><span class="sxs-lookup"><span data-stu-id="2d7a9-119">See all my groups</span></span>](active-directory-groups-view-azure-portal.md)
* [<span data-ttu-id="2d7a9-120">Rendelje hozzá a felhasználó vagy csoport tooan vállalati alkalmazások</span><span class="sxs-lookup"><span data-stu-id="2d7a9-120">Assign a user or group tooan enterprise app</span></span>](active-directory-coreapps-assign-user-azure-portal.md)
* [<span data-ttu-id="2d7a9-121">Egy felhasználó vagy csoport-hozzárendelés eltávolítása a vállalati alkalmazások</span><span class="sxs-lookup"><span data-stu-id="2d7a9-121">Remove a user or group assignment from an enterprise app</span></span>](active-directory-coreapps-remove-assignment-azure-portal.md)
* [<span data-ttu-id="2d7a9-122">Hello nevének módosítása vagy egy vállalati alkalmazás embléma</span><span class="sxs-lookup"><span data-stu-id="2d7a9-122">Change hello name or logo of an enterprise app</span></span>](active-directory-coreapps-change-app-logo-user-azure-portal.md)
