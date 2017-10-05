---
title: "Hogyan lehet elvégezni egy áttekintése |} Microsoft Docs"
description: "Miután az Azure AD Privileged Identity Management indította egy áttekintése, megtudhatja, hogyan befejeződését, és az eredmények megtekintése"
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: abc2d3dd-afd5-42cf-8a17-6c11f5674c35
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/06/2017
ms.author: kgremban
ms.custom: pim
ms.openlocfilehash: ca2a1c7c287e4cf6b1b50cfb0068861b6f525596
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-complete-an-access-review-in-azure-ad-privileged-identity-management"></a><span data-ttu-id="d6f27-103">Hogyan lehet elvégezni egy az Azure AD Privileged Identity Management áttekintése</span><span class="sxs-lookup"><span data-stu-id="d6f27-103">How to complete an access review in Azure AD Privileged Identity Management</span></span>
<span data-ttu-id="d6f27-104">Kiemelt szerepkörű rendszergazda egyszer emelt szintű hozzáférés is tekintheti a [biztonsági felülvizsgálat elindult](active-directory-privileged-identity-management-how-to-start-security-review.md).</span><span class="sxs-lookup"><span data-stu-id="d6f27-104">Privileged role administrators can review privileged access once a [security review has been started](active-directory-privileged-identity-management-how-to-start-security-review.md).</span></span> <span data-ttu-id="d6f27-105">Az Azure AD Privileged Identity Management (PIM) automatikusan elküld egy e-mailt arra kéri a felhasználót a hozzáférésüket áttekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="d6f27-105">Azure AD Privileged Identity Management (PIM) will automatically send an email prompting users to review their access.</span></span> <span data-ttu-id="d6f27-106">Ha a felhasználó nem kapott e-mailt, elküldheti azokat az utasításokat [egy biztonsági felülvizsgálat végrehajtása](active-directory-privileged-identity-management-how-to-perform-security-review.md).</span><span class="sxs-lookup"><span data-stu-id="d6f27-106">If a user did not get an email, you can send them the instructions in [how to perform a security review](active-directory-privileged-identity-management-how-to-perform-security-review.md).</span></span>

<span data-ttu-id="d6f27-107">Után a biztonsági felülvizsgálat időszak alatt, vagy a felhasználók végzett, az önálló tekintse át, kövesse a cikkben a felülvizsgálati kezelésére, és az eredmények megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="d6f27-107">After the security review period is over, or all the users have finished their self-review, follow the steps in this article to manage the review and see the results.</span></span>

## <a name="manage-security-reviews"></a><span data-ttu-id="d6f27-108">Biztonsági felülvizsgálat kezelése</span><span class="sxs-lookup"><span data-stu-id="d6f27-108">Manage security reviews</span></span>
1. <span data-ttu-id="d6f27-109">Lépjen a [Azure-portálon](https://portal.azure.com/) válassza ki a **Azure AD Privileged Identity Management** alkalmazás az irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="d6f27-109">Go to the [Azure portal](https://portal.azure.com/) and select the **Azure AD Privileged Identity Management** application on your dashboard.</span></span>
2. <span data-ttu-id="d6f27-110">Válassza ki a **értékelést hozzáférési** szakasza az irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="d6f27-110">Select the **Access reviews** section of the dashboard.</span></span>
3. <span data-ttu-id="d6f27-111">Válassza ki a kezelni kívánt áttekintése.</span><span class="sxs-lookup"><span data-stu-id="d6f27-111">Select the access review that you want to manage.</span></span>

<span data-ttu-id="d6f27-112">A hozzáférés felülvizsgálati részletei panel lehetőség áll rendelkezésre egy szám felülvizsgálat kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="d6f27-112">On the access review's detail blade there are a number options for managing that review.</span></span>

![PIM hozzáférés felülvizsgálati gombok – képernyőkép][1]

### <a name="remind"></a><span data-ttu-id="d6f27-114">Emlékeztesse</span><span class="sxs-lookup"><span data-stu-id="d6f27-114">Remind</span></span>
<span data-ttu-id="d6f27-115">Ha egy áttekintése be van állítva, így a felhasználók maguk, tekintse át a **emlékeztetése** gomb értesítést küld.</span><span class="sxs-lookup"><span data-stu-id="d6f27-115">If an access review is set up so that the users review themselves, the **Remind** button sends out a notification.</span></span> 

### <a name="stop"></a><span data-ttu-id="d6f27-116">Leállítás</span><span class="sxs-lookup"><span data-stu-id="d6f27-116">Stop</span></span>
<span data-ttu-id="d6f27-117">Hozzáférés az összes értékelést rendelkezik befejező dátumát, de használhatja a **leállítása** gomb korai befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="d6f27-117">All access reviews have an end date, but you can use the **Stop** button to finish it early.</span></span> <span data-ttu-id="d6f27-118">A felhasználók még nem ellenőrzött időpontig, ha azok nem fogja tudni a felülvizsgálati befejezése után.</span><span class="sxs-lookup"><span data-stu-id="d6f27-118">If any users haven't been reviewed by this time, they won't be able to after you stop the review.</span></span> <span data-ttu-id="d6f27-119">Nem indítható újra áttekintése után a rendszer leállt.</span><span class="sxs-lookup"><span data-stu-id="d6f27-119">You cannot restart a review after it's been stopped.</span></span>

### <a name="apply"></a><span data-ttu-id="d6f27-120">Jelentkezés</span><span class="sxs-lookup"><span data-stu-id="d6f27-120">Apply</span></span>
<span data-ttu-id="d6f27-121">A rendszer egy áttekintése után, mert elérte a záró dátum, vagy manuálisan, megszakította a **alkalmaz** gomb valósítja meg a felülvizsgálati eredményeit.</span><span class="sxs-lookup"><span data-stu-id="d6f27-121">After an access review is completed, either because you reached the end date or stopped it manually, the **Apply** button implements the outcome of the review.</span></span> <span data-ttu-id="d6f27-122">A felhasználói hozzáférés megtagadva a felülvizsgálat alatt, ha ez a lépés, amely a szerepkör-hozzárendelés, azzal eltávolítja az.</span><span class="sxs-lookup"><span data-stu-id="d6f27-122">If a user's access was denied in the review, this is the step that will remove their role assignment.</span></span>  

### <a name="export"></a><span data-ttu-id="d6f27-123">Exportálás</span><span class="sxs-lookup"><span data-stu-id="d6f27-123">Export</span></span>
<span data-ttu-id="d6f27-124">Ha a biztonsági ellenőrzés eredményének manuálisan alkalmazni kívánt, exportálhatja a felülvizsgálati.</span><span class="sxs-lookup"><span data-stu-id="d6f27-124">If you want to apply the results of the security review manually, you can export the review.</span></span> <span data-ttu-id="d6f27-125">A **exportálása** gomb indul el egy CSV-fájl letöltése.</span><span class="sxs-lookup"><span data-stu-id="d6f27-125">The **Export** button will start downloading a CSV file.</span></span> <span data-ttu-id="d6f27-126">Az eredményeket az Excel vagy más programok, nyissa meg a CSV-fájlok is kezelheti.</span><span class="sxs-lookup"><span data-stu-id="d6f27-126">You can manage the results in Excel or other programs that open CSV files.</span></span>

### <a name="delete"></a><span data-ttu-id="d6f27-127">Törlés</span><span class="sxs-lookup"><span data-stu-id="d6f27-127">Delete</span></span>
<span data-ttu-id="d6f27-128">Ha nem a felülvizsgálati további iránt érdeklődik, törölje azt.</span><span class="sxs-lookup"><span data-stu-id="d6f27-128">If you are not interested in the review any further, delete it.</span></span> <span data-ttu-id="d6f27-129">A **törlése** gomb a felülvizsgálati eltávolítja a PIM alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="d6f27-129">The **Delete** button removes the review from the PIM application.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d6f27-130">Akkor lesz nem megjelenik egy figyelmeztetés, még törlése előtt, ezért arról, hogy szeretné-e felülvizsgálat törlése.</span><span class="sxs-lookup"><span data-stu-id="d6f27-130">You will not get a warning before deletion occurs, so be sure that you want to delete that review.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="d6f27-131">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d6f27-131">Next steps</span></span>
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-complete-review/PIM_review_buttons.png
