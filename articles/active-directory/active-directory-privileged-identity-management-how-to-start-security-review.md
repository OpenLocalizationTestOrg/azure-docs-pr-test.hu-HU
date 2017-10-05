---
title: "Hogyan kell elindítani egy áttekintése |} Microsoft Docs"
description: "Megtudhatja, hogyan hozzon létre egy áttekintése a kiemelt jogosultságú identitások az Azure Privileged Identity Management alkalmazással."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 3e52b731-55f4-4c8a-ba87-9fd34033f52f
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/04/2017
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 2b516e2f05aa883c5e37f5864e5ee8a2b37d3a46
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-start-an-access-review-in-azure-ad-privileged-identity-management"></a><span data-ttu-id="6a7eb-103">Az Azure AD Privileged Identity Management egy hozzáférési felülvizsgálat indítása</span><span class="sxs-lookup"><span data-stu-id="6a7eb-103">How to start an access review in Azure AD Privileged Identity Management</span></span>
<span data-ttu-id="6a7eb-104">Szerepkör-hozzárendelések "elavult" válnak, amikor a felhasználók privilegizált hozzáférést, amelyek többé nem kell.</span><span class="sxs-lookup"><span data-stu-id="6a7eb-104">Role assignments become "stale" when users have privileged access that they don't need anymore.</span></span> <span data-ttu-id="6a7eb-105">Ahhoz, hogy ezek elavult szerepkör-hozzárendelések a kockázatának csökkentéséhez kiemelt szerepkörű rendszergazda rendszeresen tekintse át a szerepköröket, amelyek a felhasználóknak.</span><span class="sxs-lookup"><span data-stu-id="6a7eb-105">In order to reduce the risk associated with these stale role assignments, privileged role administrators should regularly review the roles that users have been given.</span></span> <span data-ttu-id="6a7eb-106">Ez a dokumentum egy hozzáférés-ellenőrzés indítása az Azure AD Privileged Identity Management (PIM) a lépéseket ismerteti.</span><span class="sxs-lookup"><span data-stu-id="6a7eb-106">This document covers the steps for starting an access review in Azure AD Privileged Identity Management (PIM).</span></span>

## <a name="start-an-access-review"></a><span data-ttu-id="6a7eb-107">Hozzáférés-ellenőrzés indítása</span><span class="sxs-lookup"><span data-stu-id="6a7eb-107">Start an access review</span></span>
> [!NOTE]
> <span data-ttu-id="6a7eb-108">Ha a PIM alkalmazást az irányítópulton nem adott meg az Azure portálon, olvassa el a [Ismerkedés az Azure Privileged Identity Management szolgáltatással](active-directory-privileged-identity-management-getting-started.md)</span><span class="sxs-lookup"><span data-stu-id="6a7eb-108">If you haven't added the PIM application to your dashboard in the Azure portal, see the steps in  [Getting Started with Azure Privileged Identity Management](active-directory-privileged-identity-management-getting-started.md)</span></span>
> 
> 

<span data-ttu-id="6a7eb-109">A PIM-alkalmazás fő lapján, a három módja van egy hozzáférés-ellenőrzés indítása:</span><span class="sxs-lookup"><span data-stu-id="6a7eb-109">From the PIM application main page, there are three ways to start an access review:</span></span>

* <span data-ttu-id="6a7eb-110">**Hozzáférési értékelést** > **hozzáadása**</span><span class="sxs-lookup"><span data-stu-id="6a7eb-110">**Access reviews** > **Add**</span></span>
* <span data-ttu-id="6a7eb-111">**Szerepkörök** > **felülvizsgálati** gomb</span><span class="sxs-lookup"><span data-stu-id="6a7eb-111">**Roles** > **Review** button</span></span>
* <span data-ttu-id="6a7eb-112">Válassza ki a megfelelő szerepkört a szerepkörök listából vizsgálni > **felülvizsgálati** gomb</span><span class="sxs-lookup"><span data-stu-id="6a7eb-112">Select the specific role to be reviewed from the roles list > **Review** button</span></span>

<span data-ttu-id="6a7eb-113">Elemre a **tekintse át** gombra, a **egy hozzáférés-ellenőrzés indítása** panel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="6a7eb-113">When you click on the **Review** button, the **Start an access review** blade appears.</span></span> <span data-ttu-id="6a7eb-114">A panel fog a felülvizsgálati állítson be egy nevet és a határidő, válassza ki a szerepkört, és döntse el, akik végrehajtják a felülvizsgálati.</span><span class="sxs-lookup"><span data-stu-id="6a7eb-114">On this blade, you're going to configure the review with a name and time limit, choose a role to review, and decide who will perform the review.</span></span>

![Indítsa el az áttekintése – képernyőkép][1]

### <a name="configure-the-review"></a><span data-ttu-id="6a7eb-116">A felülvizsgálati konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6a7eb-116">Configure the review</span></span>
<span data-ttu-id="6a7eb-117">Hozzon létre egy áttekintése, szüksége egy kezdő és záró dátumát, és adjon neki nevet.</span><span class="sxs-lookup"><span data-stu-id="6a7eb-117">To create an access review, you need to name it and set a start and end date.</span></span>

![Konfigurálja a felülvizsgálati – képernyőkép][2]

<span data-ttu-id="6a7eb-119">Ellenőrizze a elég hosszú a felhasználók számára meg ennek befejeződését, tekintse át a hosszát.</span><span class="sxs-lookup"><span data-stu-id="6a7eb-119">Make the length of the review long enough for users to complete it.</span></span> <span data-ttu-id="6a7eb-120">Ha befejezte a záró dátum előtt, mindig leállíthatja a felülvizsgálati korai.</span><span class="sxs-lookup"><span data-stu-id="6a7eb-120">If you finish before the end date, you can always stop the review early.</span></span>

### <a name="choose-a-role-to-review"></a><span data-ttu-id="6a7eb-121">Válassza ki a szerepkört áttekintése</span><span class="sxs-lookup"><span data-stu-id="6a7eb-121">Choose a role to review</span></span>
<span data-ttu-id="6a7eb-122">Minden egyes felülvizsgálati csak egy szerepkör összpontosít.</span><span class="sxs-lookup"><span data-stu-id="6a7eb-122">Each review focuses on only one role.</span></span> <span data-ttu-id="6a7eb-123">Kivéve, ha a áttekintése indította el egy adott szerepkör panel, szüksége lesz egy szerepkör most elemre.</span><span class="sxs-lookup"><span data-stu-id="6a7eb-123">Unless you started the access review from a specific role blade, you'll need to choose a role now.</span></span>

1. <span data-ttu-id="6a7eb-124">Navigáljon a **tekintse át a szerepköri tagság**</span><span class="sxs-lookup"><span data-stu-id="6a7eb-124">Navigate to **Review role membership**</span></span>
   
    ![Tekintse át a szerepköri tagság – képernyőkép][3]
2. <span data-ttu-id="6a7eb-126">Válassza ki egy szerepkört a listából.</span><span class="sxs-lookup"><span data-stu-id="6a7eb-126">Choose one role from the list.</span></span>

### <a name="decide-who-will-perform-the-review"></a><span data-ttu-id="6a7eb-127">Döntse el, akik végrehajtják a felülvizsgálati</span><span class="sxs-lookup"><span data-stu-id="6a7eb-127">Decide who will perform the review</span></span>
<span data-ttu-id="6a7eb-128">Ellenőrzés végrehajtása esetén három lehetőség áll rendelkezésre.</span><span class="sxs-lookup"><span data-stu-id="6a7eb-128">There are three options for performing a review.</span></span> <span data-ttu-id="6a7eb-129">A felülvizsgálati rendelhet más végezze, akkor megteheti ezt, vagy beállíthatja, hogy a minden felhasználó, tekintse át a saját hozzáférésüket.</span><span class="sxs-lookup"><span data-stu-id="6a7eb-129">You can assign the review to someone else to complete, you can do it yourself, or you can have each user review their own access.</span></span>

1. <span data-ttu-id="6a7eb-130">Navigáljon a **felülvizsgálók kiválasztása**</span><span class="sxs-lookup"><span data-stu-id="6a7eb-130">Navigate to **Select reviewers**</span></span>
   
    ![Válassza ki a felülvizsgálók – képernyőkép][4]
2. <span data-ttu-id="6a7eb-132">A lehetőségek közül választhat:</span><span class="sxs-lookup"><span data-stu-id="6a7eb-132">Choose one of the options:</span></span>
   
   * <span data-ttu-id="6a7eb-133">**Válassza ki a felülvizsgáló**: használja ezt a beállítást, ha nem biztos benne, aki hozzá kell férnie.</span><span class="sxs-lookup"><span data-stu-id="6a7eb-133">**Select reviewer**: Use this option when you don't know who needs access.</span></span> <span data-ttu-id="6a7eb-134">Ezzel a beállítással a felülvizsgálati hozzárendelése egy erőforrás tulajdonosa vagy a csoport kezelőjének befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="6a7eb-134">With this option, you can assign the review to a resource owner or group manager to complete.</span></span>
   * <span data-ttu-id="6a7eb-135">**A nekem**: hasznos, ha meg kívánja tekinteni, hogyan hozzáférést ellenőrzi, hogy a munkahelyi, vagy meg szeretné tekinteni a személyeket, akik nem nevében.</span><span class="sxs-lookup"><span data-stu-id="6a7eb-135">**Me**: Useful if you want to preview how access reviews work, or you want to review on behalf of people who can't.</span></span>
   * <span data-ttu-id="6a7eb-136">**Tagok áttekintése maguk**: használja ezt a beállítást szeretné, hogy a felhasználók, tekintse át a saját szerepkör-hozzárendelések.</span><span class="sxs-lookup"><span data-stu-id="6a7eb-136">**Members review themselves**: Use this option to have the users review their own role assignments.</span></span>

### <a name="start-the-review"></a><span data-ttu-id="6a7eb-137">Az ellenőrzés indítása</span><span class="sxs-lookup"><span data-stu-id="6a7eb-137">Start the review</span></span>
<span data-ttu-id="6a7eb-138">Végül lehetősége van a szükséges, hogy a felhasználók ha azok elfogadják a hozzáférését meg okot.</span><span class="sxs-lookup"><span data-stu-id="6a7eb-138">Finally, you have the option to require that users provide a reason if they approve their access.</span></span> <span data-ttu-id="6a7eb-139">Ha szeretné adjon meg egy leírást, a nézze át, és válassza ki **Start**.</span><span class="sxs-lookup"><span data-stu-id="6a7eb-139">Add a description of the review if you like, and select **Start**.</span></span>

<span data-ttu-id="6a7eb-140">Győződjön meg arról, hogy engedélyezi a felhasználóknak, hogy egy entitás áttekintése, és a megjelenítésükhöz [egy hozzáférési felülvizsgálat végrehajtása](active-directory-privileged-identity-management-how-to-perform-security-review.md).</span><span class="sxs-lookup"><span data-stu-id="6a7eb-140">Make sure you let your users know that there's an access review waiting for them, and show them [How to perform an access review](active-directory-privileged-identity-management-how-to-perform-security-review.md).</span></span>

## <a name="manage-the-access-review"></a><span data-ttu-id="6a7eb-141">Kezelheti a áttekintése</span><span class="sxs-lookup"><span data-stu-id="6a7eb-141">Manage the access review</span></span>
<span data-ttu-id="6a7eb-142">Nyomon követheti a folyamatban, a felülvizsgálók végezze el az Azure AD PIM irányítópultján, az access értékelést szakaszban az ellenőrzéseket.</span><span class="sxs-lookup"><span data-stu-id="6a7eb-142">You can track the progress as the reviewers complete their reviews in the Azure AD PIM dashboard, in the access reviews section.</span></span> <span data-ttu-id="6a7eb-143">Nincs hozzáférési jogosultsága módosulnak, amíg a címtár [a felülvizsgálati befejezése](active-directory-privileged-identity-management-how-to-complete-review.md).</span><span class="sxs-lookup"><span data-stu-id="6a7eb-143">No access rights will be changed in the directory until [the review completes](active-directory-privileged-identity-management-how-to-complete-review.md).</span></span>

<span data-ttu-id="6a7eb-144">Amíg a felülvizsgálati időszak alatt, emlékeztesse a felhasználók a felülvizsgálat befejezéséhez, vagy állítsa le a korai szakaszából hozzáférés értékelést áttekintése.</span><span class="sxs-lookup"><span data-stu-id="6a7eb-144">Until the review period is over, you can remind users to complete their review, or stop the review early from the access reviews section.</span></span>

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="pim-table-of-contents"></a><span data-ttu-id="6a7eb-145">A PIM tartalomjegyzék</span><span class="sxs-lookup"><span data-stu-id="6a7eb-145">PIM Table of Contents</span></span>
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-start-security-review/PIM_start_review.png
[2]: ./media/active-directory-privileged-identity-management-how-to-start-security-review/PIM_review_configure.png
[3]: ./media/active-directory-privileged-identity-management-how-to-start-security-review/PIM_review_role.png
[4]: ./media/active-directory-privileged-identity-management-how-to-start-security-review/PIM_review_reviewers.png
