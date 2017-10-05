---
title: "Felhasználó-hozzárendelés – az Azure AD szükséges |} Microsoft Docs"
description: "Használatának megkövetelése a felhasználó hozzárendelése az Azure-alkalmazások."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: 
ms.assetid: 30b78cba-1e0f-472f-8314-f2250a9b91c3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: kgremban
robots: noindex
ms.openlocfilehash: 079b806c041a4a21e9350342867aee581c57bf45
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-ad-and-applications-require-user-assignment"></a><span data-ttu-id="21550-103">Az Azure AD és az alkalmazások: igényelnek felhasználói kiosztása</span><span class="sxs-lookup"><span data-stu-id="21550-103">Azure AD and applications: Require user assignment</span></span>
## <a name="requiring-user-assignment"></a><span data-ttu-id="21550-104">Igénylő felhasználó-hozzárendelés</span><span class="sxs-lookup"><span data-stu-id="21550-104">Requiring User Assignment</span></span>
1. <span data-ttu-id="21550-105">Jelentkezzen be rendszergazdai fiókkal az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="21550-105">Log in to the Azure portal with an administrator account.</span></span>
2. <span data-ttu-id="21550-106">Kattintson a **összes elemet** a főmenü elemére.</span><span class="sxs-lookup"><span data-stu-id="21550-106">Click on the **All Items** item in the main menu.</span></span>
3. <span data-ttu-id="21550-107">Adja meg az alkalmazás használja.</span><span class="sxs-lookup"><span data-stu-id="21550-107">Choose the directory you are using for the application.</span></span>
4. <span data-ttu-id="21550-108">Kattintson a **alkalmazások** fülre.</span><span class="sxs-lookup"><span data-stu-id="21550-108">Click on the **APPLICATIONS** tab.</span></span>
5. <span data-ttu-id="21550-109">Az ebben a könyvtárban társított alkalmazások listájából válassza ki az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="21550-109">Select the application from the list of applications associated with this directory.</span></span>
6. <span data-ttu-id="21550-110">Kattintson a **KONFIGURÁLÁSA** fülre.</span><span class="sxs-lookup"><span data-stu-id="21550-110">Click the **CONFIGURE** tab.</span></span>
7. <span data-ttu-id="21550-111">Módosítsa a **felhasználói Access alkalmazásnak szükséges hozzárendelés** váltása igen.</span><span class="sxs-lookup"><span data-stu-id="21550-111">Change the **User Assignment Required to Access App** toggle to Yes.</span></span>
8. <span data-ttu-id="21550-112">Kattintson a **mentése** gomb a képernyő alján.</span><span class="sxs-lookup"><span data-stu-id="21550-112">Click the **Save** button at the bottom of the screen.</span></span>

<span data-ttu-id="21550-113">Felhasználók és/vagy csoportok hozzárendelése az alkalmazás most már rendelkezik lesz.</span><span class="sxs-lookup"><span data-stu-id="21550-113">You will now have to assign users and/or groups to the application.</span></span> <span data-ttu-id="21550-114">Lásd: [felhasználók hozzárendelése egy alkalmazás](active-directory-applications-guiding-developers-assigning-users.md) és [csoportok hozzárendelése egy alkalmazás](active-directory-applications-guiding-developers-assigning-groups.md).</span><span class="sxs-lookup"><span data-stu-id="21550-114">See [Assigning users to an application](active-directory-applications-guiding-developers-assigning-users.md) and [Assigning groups to an application](active-directory-applications-guiding-developers-assigning-groups.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="21550-115">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="21550-115">Next Steps</span></span>
[!INCLUDE [active-directory-applications-guiding-developers-for-lob-applications-toc.md](../../includes/active-directory-applications-guiding-developers-for-lob-applications-toc.md)]
