---
title: "Az Azure AD és az alkalmazások: az alkalmazás felhasználók hozzárendelése |} Microsoft Docs"
description: "Útmutató az Azure-alkalmazások felhasználó-hozzárendelés végrehajtásához."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: 
ms.assetid: 97ce69c1-4034-4e38-bd82-8caf984f6b98
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/12/2017
ms.author: kgremban
robots: noindex
ms.openlocfilehash: 29d63bd5781dc7ef9e84840dd4b1b70222cf6892
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-ad-and-applications-assigning-users-to-an-application"></a><span data-ttu-id="bc35b-103">Az Azure AD és az alkalmazások: az alkalmazás felhasználók hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="bc35b-103">Azure AD and Applications: Assigning Users to an Application</span></span>
<span data-ttu-id="bc35b-104">Mielőtt a felhasználók és csoportok rendelhet egy alkalmazás, felhasználó-hozzárendelés követelheti meg.</span><span class="sxs-lookup"><span data-stu-id="bc35b-104">Before you can assign users and groups to an application, you must require user assignment.</span></span>  <span data-ttu-id="bc35b-105">Igénylése felhasználói kiosztása elsajátításához lásd: a [igénylő felhasználó hozzárendelés](active-directory-applications-guiding-developers-requiring-user-assignment.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="bc35b-105">To learn how to require user assignment please see the [Requiring User Assignment](active-directory-applications-guiding-developers-requiring-user-assignment.md) article.</span></span>

## <a name="assigning-users-to-an-application"></a><span data-ttu-id="bc35b-106">Az alkalmazás felhasználók hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="bc35b-106">Assigning Users to an Application</span></span>
1. <span data-ttu-id="bc35b-107">Jelentkezzen be rendszergazdai fiókkal az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="bc35b-107">Log in to the Azure portal with an administrator account.</span></span>
2. <span data-ttu-id="bc35b-108">Kattintson a **összes elemet** a főmenü elemére.</span><span class="sxs-lookup"><span data-stu-id="bc35b-108">Click on the **All Items** item in the main menu.</span></span>
3. <span data-ttu-id="bc35b-109">Adja meg az alkalmazás használja.</span><span class="sxs-lookup"><span data-stu-id="bc35b-109">Choose the directory you are using for the application.</span></span>
4. <span data-ttu-id="bc35b-110">Kattintson a **alkalmazások** fülre.</span><span class="sxs-lookup"><span data-stu-id="bc35b-110">Click on the **APPLICATIONS** tab.</span></span>
5. <span data-ttu-id="bc35b-111">Az ebben a könyvtárban társított alkalmazások listájából válassza ki az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="bc35b-111">Select the application from the list of applications associated with this directory.</span></span>
6. <span data-ttu-id="bc35b-112">Kattintson a **felhasználók és csoportok** fülre.</span><span class="sxs-lookup"><span data-stu-id="bc35b-112">Click the **USERS AND GROUPS** tab.</span></span>
7. <span data-ttu-id="bc35b-113">Válassza ki az alkalmazáshoz hozzárendelni kívánt felhasználókat.</span><span class="sxs-lookup"><span data-stu-id="bc35b-113">Select the users you want to assign to the application.</span></span>
8. <span data-ttu-id="bc35b-114">Kattintson a **hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="bc35b-114">Click **ASSIGN**.</span></span>
9. <span data-ttu-id="bc35b-115">Kattintson a **Igen** megjelenésekor.</span><span class="sxs-lookup"><span data-stu-id="bc35b-115">Click **yes** when prompted.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bc35b-116">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bc35b-116">Next Steps</span></span>
[!INCLUDE [guiding-developers-for-lob-applications-toc.md](../../includes/active-directory-applications-guiding-developers-for-lob-applications-toc.md)]

