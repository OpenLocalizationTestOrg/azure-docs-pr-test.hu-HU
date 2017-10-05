---
title: "Csoportok hozzárendelése az Azure AD alkalmazásaiban |} Microsoft Docs"
description: "How Azure-alkalmazások csoport-hozzárendelés végrehajtásához."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: 
ms.assetid: 29b5ba89-a1c7-4f1f-a294-248a40106617
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2017
ms.author: kgremban
ms.custom: H1Hack27Feb2017
robots: noindex
ms.openlocfilehash: e0b0b87a454db96747f024e81882fe83d62fdbe2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="assign-azure-active-directory-groups-to-an-application"></a><span data-ttu-id="a7107-103">Az alkalmazás Azure Active Directory-csoportok hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="a7107-103">Assign Azure Active Directory groups to an application</span></span>
<span data-ttu-id="a7107-104">Mielőtt a felhasználók és csoportok rendelhet egy alkalmazás, felhasználó-hozzárendelés követelheti meg.</span><span class="sxs-lookup"><span data-stu-id="a7107-104">Before you can assign users and groups to an application, you must require user assignment.</span></span> <span data-ttu-id="a7107-105">Kötelező felhasználói kiosztása, lásd: a [igénylő felhasználó hozzárendelés](active-directory-applications-guiding-developers-requiring-user-assignment.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="a7107-105">To learn how to require user assignment, see the [Requiring User Assignment](active-directory-applications-guiding-developers-requiring-user-assignment.md) article.</span></span>

<span data-ttu-id="a7107-106">Ez a cikk feltételezi, hogy már létrehozott csoportok az active Directory, az alkalmazás használ.</span><span class="sxs-lookup"><span data-stu-id="a7107-106">This article assumes that you have already created groups in the active directory you are using for this application.</span></span>

## <a name="assigning-groups-to-an-application"></a><span data-ttu-id="a7107-107">Csoportok hozzárendelése az alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="a7107-107">Assigning Groups to an Application</span></span>
1. <span data-ttu-id="a7107-108">Jelentkezzen be rendszergazdai fiókkal az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="a7107-108">Log in to the Azure portal with an administrator account.</span></span>
2. <span data-ttu-id="a7107-109">Kattintson a **összes elemet** a főmenü elemére.</span><span class="sxs-lookup"><span data-stu-id="a7107-109">Click on the **All Items** item in the main menu.</span></span>
3. <span data-ttu-id="a7107-110">Adja meg az alkalmazás használja.</span><span class="sxs-lookup"><span data-stu-id="a7107-110">Choose the directory you are using for the application.</span></span>
4. <span data-ttu-id="a7107-111">Kattintson a **alkalmazások** fülre.</span><span class="sxs-lookup"><span data-stu-id="a7107-111">Click on the **APPLICATIONS** tab.</span></span>
5. <span data-ttu-id="a7107-112">Az ebben a könyvtárban társított alkalmazások listájából válassza ki az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="a7107-112">Select the application from the list of applications associated with this directory.</span></span>
6. <span data-ttu-id="a7107-113">Kattintson a **felhasználók és csoportok** fülre.</span><span class="sxs-lookup"><span data-stu-id="a7107-113">Click the **USERS AND GROUPS** tab.</span></span>
7. <span data-ttu-id="a7107-114">Az active Directoryban a csoportlista szűréséhez használatával a **csoportok** legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="a7107-114">Filter the list of groups in your active directory by using the **Groups** dropdown list.</span></span>
8. <span data-ttu-id="a7107-115">Válassza ki azt a csoportot.</span><span class="sxs-lookup"><span data-stu-id="a7107-115">Select the group.</span></span>
9. <span data-ttu-id="a7107-116">Kattintson a **hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="a7107-116">Click **ASSIGN**.</span></span>
10. <span data-ttu-id="a7107-117">Kattintson a **Igen** megjelenésekor.</span><span class="sxs-lookup"><span data-stu-id="a7107-117">Click **yes** when prompted.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a7107-118">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a7107-118">Next Steps</span></span>
[!INCLUDE [active-directory-applications-guiding-developers-for-lob-applications-toc.md](../../includes/active-directory-applications-guiding-developers-for-lob-applications-toc.md)]
