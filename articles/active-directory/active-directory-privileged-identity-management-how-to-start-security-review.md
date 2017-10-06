---
title: "aaaHow toostart egy áttekintése |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate hozzáférés tekintse át a kiemelt identitásokat hello Azure Privileged Identity Management alkalmazás."
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
ms.openlocfilehash: 24feac307f77c69b5d68d6ae0623dbcb52416b01
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toostart-an-access-review-in-azure-ad-privileged-identity-management"></a><span data-ttu-id="d803c-103">Hogyan toostart hozzáférés tekintse át az Azure AD Privileged Identity Management</span><span class="sxs-lookup"><span data-stu-id="d803c-103">How toostart an access review in Azure AD Privileged Identity Management</span></span>
<span data-ttu-id="d803c-104">Szerepkör-hozzárendelések "elavult" válnak, amikor a felhasználók privilegizált hozzáférést, amelyek többé nem kell.</span><span class="sxs-lookup"><span data-stu-id="d803c-104">Role assignments become "stale" when users have privileged access that they don't need anymore.</span></span> <span data-ttu-id="d803c-105">Rendelés tooreduce hello kockázatának ezek elavult szerepkör-hozzárendelések, a kiemelt szerepkörű rendszergazda felhasználók kapott hello szerepkörtől rendszeresen tekintse át.</span><span class="sxs-lookup"><span data-stu-id="d803c-105">In order tooreduce hello risk associated with these stale role assignments, privileged role administrators should regularly review hello roles that users have been given.</span></span> <span data-ttu-id="d803c-106">Ez a dokumentum egy hozzáférés-ellenőrzés indítása az Azure AD Privileged Identity Management (PIM) hello lépéseit ismerteti.</span><span class="sxs-lookup"><span data-stu-id="d803c-106">This document covers hello steps for starting an access review in Azure AD Privileged Identity Management (PIM).</span></span>

## <a name="start-an-access-review"></a><span data-ttu-id="d803c-107">Hozzáférés-ellenőrzés indítása</span><span class="sxs-lookup"><span data-stu-id="d803c-107">Start an access review</span></span>
> [!NOTE]
> <span data-ttu-id="d803c-108">Ha hello PIM alkalmazás tooyour irányítópult hello Azure-portálon még nem adott hozzá, tekintse meg a hello lépéseit [Ismerkedés az Azure Privileged Identity Management szolgáltatással](active-directory-privileged-identity-management-getting-started.md)</span><span class="sxs-lookup"><span data-stu-id="d803c-108">If you haven't added hello PIM application tooyour dashboard in hello Azure portal, see hello steps in  [Getting Started with Azure Privileged Identity Management](active-directory-privileged-identity-management-getting-started.md)</span></span>
> 
> 

<span data-ttu-id="d803c-109">Hello PIM alkalmazás fő lapján nincsenek három módon toostart egy áttekintése:</span><span class="sxs-lookup"><span data-stu-id="d803c-109">From hello PIM application main page, there are three ways toostart an access review:</span></span>

* <span data-ttu-id="d803c-110">**Hozzáférési értékelést** > **hozzáadása**</span><span class="sxs-lookup"><span data-stu-id="d803c-110">**Access reviews** > **Add**</span></span>
* <span data-ttu-id="d803c-111">**Szerepkörök** > **felülvizsgálati** gomb</span><span class="sxs-lookup"><span data-stu-id="d803c-111">**Roles** > **Review** button</span></span>
* <span data-ttu-id="d803c-112">SELECT hello adott szerepkör toobe áttekintette hello szerepkörök listából > **felülvizsgálati** gomb</span><span class="sxs-lookup"><span data-stu-id="d803c-112">Select hello specific role toobe reviewed from hello roles list > **Review** button</span></span>

<span data-ttu-id="d803c-113">Amikor rákattint az hello **tekintse át** gomb hello **egy hozzáférés-ellenőrzés indítása** panel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="d803c-113">When you click on hello **Review** button, hello **Start an access review** blade appears.</span></span> <span data-ttu-id="d803c-114">A panel éppen folyamatban tooconfigure hello felülvizsgálati néven és határidő, választja a szerepkör tooreview, és döntse el, akik végrehajtják a hello áttekintése.</span><span class="sxs-lookup"><span data-stu-id="d803c-114">On this blade, you're going tooconfigure hello review with a name and time limit, choose a role tooreview, and decide who will perform hello review.</span></span>

![Indítsa el az áttekintése – képernyőkép][1]

### <a name="configure-hello-review"></a><span data-ttu-id="d803c-116">Hello felülvizsgálati konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d803c-116">Configure hello review</span></span>
<span data-ttu-id="d803c-117">toocreate hozzáférés tekintse át, akkor a tooname kell, és azt, és állítsa a kezdő és záró dátumát.</span><span class="sxs-lookup"><span data-stu-id="d803c-117">toocreate an access review, you need tooname it and set a start and end date.</span></span>

![Konfigurálja a felülvizsgálati – képernyőkép][2]

<span data-ttu-id="d803c-119">Ellenőrizze a hello hossza hello áttekintése amíg a felhasználók toocomplete.</span><span class="sxs-lookup"><span data-stu-id="d803c-119">Make hello length of hello review long enough for users toocomplete it.</span></span> <span data-ttu-id="d803c-120">Ha befejezte a hello záró dátum előtt, mindig le is hello felülvizsgálati korai.</span><span class="sxs-lookup"><span data-stu-id="d803c-120">If you finish before hello end date, you can always stop hello review early.</span></span>

### <a name="choose-a-role-tooreview"></a><span data-ttu-id="d803c-121">Válassza ki a szerepkör tooreview</span><span class="sxs-lookup"><span data-stu-id="d803c-121">Choose a role tooreview</span></span>
<span data-ttu-id="d803c-122">Minden egyes felülvizsgálati csak egy szerepkör összpontosít.</span><span class="sxs-lookup"><span data-stu-id="d803c-122">Each review focuses on only one role.</span></span> <span data-ttu-id="d803c-123">Ha hello áttekintése indította el egy adott szerepkör panel, toochoose szerepkör most lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="d803c-123">Unless you started hello access review from a specific role blade, you'll need toochoose a role now.</span></span>

1. <span data-ttu-id="d803c-124">Keresse meg a túl**tekintse át a szerepköri tagság**</span><span class="sxs-lookup"><span data-stu-id="d803c-124">Navigate too**Review role membership**</span></span>
   
    ![Tekintse át a szerepköri tagság – képernyőkép][3]
2. <span data-ttu-id="d803c-126">Hello listából válassza ki egy szerepkört.</span><span class="sxs-lookup"><span data-stu-id="d803c-126">Choose one role from hello list.</span></span>

### <a name="decide-who-will-perform-hello-review"></a><span data-ttu-id="d803c-127">Döntse el, akik végrehajtják a hello áttekintése</span><span class="sxs-lookup"><span data-stu-id="d803c-127">Decide who will perform hello review</span></span>
<span data-ttu-id="d803c-128">Ellenőrzés végrehajtása esetén három lehetőség áll rendelkezésre.</span><span class="sxs-lookup"><span data-stu-id="d803c-128">There are three options for performing a review.</span></span> <span data-ttu-id="d803c-129">Hello felülvizsgálati toosomeone rendelhet más toocomplete, akkor megteheti ezt, vagy beállíthatja, hogy minden felhasználó, tekintse át a saját hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="d803c-129">You can assign hello review toosomeone else toocomplete, you can do it yourself, or you can have each user review their own access.</span></span>

1. <span data-ttu-id="d803c-130">Keresse meg a túl**felülvizsgálók kiválasztása**</span><span class="sxs-lookup"><span data-stu-id="d803c-130">Navigate too**Select reviewers**</span></span>
   
    ![Válassza ki a felülvizsgálók – képernyőkép][4]
2. <span data-ttu-id="d803c-132">Hello lehetőségek közül választhat:</span><span class="sxs-lookup"><span data-stu-id="d803c-132">Choose one of hello options:</span></span>
   
   * <span data-ttu-id="d803c-133">**Válassza ki a felülvizsgáló**: használja ezt a beállítást, ha nem biztos benne, aki hozzá kell férnie.</span><span class="sxs-lookup"><span data-stu-id="d803c-133">**Select reviewer**: Use this option when you don't know who needs access.</span></span> <span data-ttu-id="d803c-134">Ezzel a lehetőséggel hello felülvizsgálati tooa erőforrás tulajdonosa vagy a csoport manager toocomplete rendelhet.</span><span class="sxs-lookup"><span data-stu-id="d803c-134">With this option, you can assign hello review tooa resource owner or group manager toocomplete.</span></span>
   * <span data-ttu-id="d803c-135">**A nekem**: hasznos, ha azt szeretné, toopreview hogyan tooreview kívánt nevében nem személyek vagy a hozzáférés ellenőrzi, hogy a munkahelyi.</span><span class="sxs-lookup"><span data-stu-id="d803c-135">**Me**: Useful if you want toopreview how access reviews work, or you want tooreview on behalf of people who can't.</span></span>
   * <span data-ttu-id="d803c-136">**Tagok áttekintése maguk**: használja ezt a beállítást toohave hello felhasználók tekintse át a saját szerepkör-hozzárendelések.</span><span class="sxs-lookup"><span data-stu-id="d803c-136">**Members review themselves**: Use this option toohave hello users review their own role assignments.</span></span>

### <a name="start-hello-review"></a><span data-ttu-id="d803c-137">Hello felülvizsgálat indítása</span><span class="sxs-lookup"><span data-stu-id="d803c-137">Start hello review</span></span>
<span data-ttu-id="d803c-138">Végül ott hello beállítás toorequire, hogy a felhasználók ha azok elfogadják a hozzáférését meg okot.</span><span class="sxs-lookup"><span data-stu-id="d803c-138">Finally, you have hello option toorequire that users provide a reason if they approve their access.</span></span> <span data-ttu-id="d803c-139">Ha szeretné adjon meg egy leírást, hello nézze át, és válassza ki **Start**.</span><span class="sxs-lookup"><span data-stu-id="d803c-139">Add a description of hello review if you like, and select **Start**.</span></span>

<span data-ttu-id="d803c-140">Győződjön meg arról, hogy engedélyezi a felhasználóknak, hogy egy entitás áttekintése, és a megjelenítésükhöz [hogyan tooperform hozzáférés tekintse át](active-directory-privileged-identity-management-how-to-perform-security-review.md).</span><span class="sxs-lookup"><span data-stu-id="d803c-140">Make sure you let your users know that there's an access review waiting for them, and show them [How tooperform an access review](active-directory-privileged-identity-management-how-to-perform-security-review.md).</span></span>

## <a name="manage-hello-access-review"></a><span data-ttu-id="d803c-141">Hello áttekintése kezelése</span><span class="sxs-lookup"><span data-stu-id="d803c-141">Manage hello access review</span></span>
<span data-ttu-id="d803c-142">Előrehaladásának hello módon hello felülvizsgálók végezze el az Azure AD PIM irányítópult hello áttekintette, a hello hozzáférési szakasz ellenőrzi.</span><span class="sxs-lookup"><span data-stu-id="d803c-142">You can track hello progress as hello reviewers complete their reviews in hello Azure AD PIM dashboard, in hello access reviews section.</span></span> <span data-ttu-id="d803c-143">Nincs hozzáférési jogosultsága amíg hello címtárban módosulnak [hello felülvizsgálati befejezése](active-directory-privileged-identity-management-how-to-complete-review.md).</span><span class="sxs-lookup"><span data-stu-id="d803c-143">No access rights will be changed in hello directory until [hello review completes](active-directory-privileged-identity-management-how-to-complete-review.md).</span></span>

<span data-ttu-id="d803c-144">Hello felülvizsgálati időszak felett van, akkor is emlékeztesse felhasználók toocomplete felülvizsgálatához, vagy ha stop hello felülvizsgálati korai hello hozzáférés ellenőrzi, hogy a szakasz.</span><span class="sxs-lookup"><span data-stu-id="d803c-144">Until hello review period is over, you can remind users toocomplete their review, or stop hello review early from hello access reviews section.</span></span>

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="pim-table-of-contents"></a><span data-ttu-id="d803c-145">A PIM tartalomjegyzék</span><span class="sxs-lookup"><span data-stu-id="d803c-145">PIM Table of Contents</span></span>
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-start-security-review/PIM_start_review.png
[2]: ./media/active-directory-privileged-identity-management-how-to-start-security-review/PIM_review_configure.png
[3]: ./media/active-directory-privileged-identity-management-how-to-start-security-review/PIM_review_role.png
[4]: ./media/active-directory-privileged-identity-management-how-to-start-security-review/PIM_review_reviewers.png
