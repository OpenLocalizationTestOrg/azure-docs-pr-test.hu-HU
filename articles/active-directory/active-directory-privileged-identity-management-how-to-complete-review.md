---
title: "aaaHow toocomplete egy áttekintése |} Microsoft Docs"
description: "Miután az Azure AD Privileged Identity Management indította egy áttekintése, ismerje meg, hogyan toocomplete, és tekintse meg a hello eredmények"
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
ms.openlocfilehash: f99ddf3ebcf80b51110326064d584f33d8e1679a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocomplete-an-access-review-in-azure-ad-privileged-identity-management"></a><span data-ttu-id="94e2e-103">Hogyan toocomplete hozzáférés tekintse át az Azure AD Privileged Identity Management</span><span class="sxs-lookup"><span data-stu-id="94e2e-103">How toocomplete an access review in Azure AD Privileged Identity Management</span></span>
<span data-ttu-id="94e2e-104">Kiemelt szerepkörű rendszergazda egyszer emelt szintű hozzáférés is tekintheti a [biztonsági felülvizsgálat elindult](active-directory-privileged-identity-management-how-to-start-security-review.md).</span><span class="sxs-lookup"><span data-stu-id="94e2e-104">Privileged role administrators can review privileged access once a [security review has been started](active-directory-privileged-identity-management-how-to-start-security-review.md).</span></span> <span data-ttu-id="94e2e-105">Az Azure AD Privileged Identity Management (PIM) küld a felhasználók tooreview hozzáférésüket értesítése e-mailt.</span><span class="sxs-lookup"><span data-stu-id="94e2e-105">Azure AD Privileged Identity Management (PIM) will automatically send an email prompting users tooreview their access.</span></span> <span data-ttu-id="94e2e-106">Ha a felhasználó nem kapott e-mailt, elküldheti azokat hello utasításokat [hogyan tooperform biztonsági tekintse át](active-directory-privileged-identity-management-how-to-perform-security-review.md).</span><span class="sxs-lookup"><span data-stu-id="94e2e-106">If a user did not get an email, you can send them hello instructions in [how tooperform a security review](active-directory-privileged-identity-management-how-to-perform-security-review.md).</span></span>

<span data-ttu-id="94e2e-107">Után hello biztonsági felülvizsgálat időszak alatt, vagy minden hello felhasználó végzett, az önálló tekintse át, kövesse a cikk toomanage hello felülvizsgálat hello, és hello eredményeket.</span><span class="sxs-lookup"><span data-stu-id="94e2e-107">After hello security review period is over, or all hello users have finished their self-review, follow hello steps in this article toomanage hello review and see hello results.</span></span>

## <a name="manage-security-reviews"></a><span data-ttu-id="94e2e-108">Biztonsági felülvizsgálat kezelése</span><span class="sxs-lookup"><span data-stu-id="94e2e-108">Manage security reviews</span></span>
1. <span data-ttu-id="94e2e-109">Nyissa meg toohello [Azure-portálon](https://portal.azure.com/) és select hello **Azure AD Privileged Identity Management** alkalmazás az irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="94e2e-109">Go toohello [Azure portal](https://portal.azure.com/) and select hello **Azure AD Privileged Identity Management** application on your dashboard.</span></span>
2. <span data-ttu-id="94e2e-110">Jelölje be hello **értékelést hozzáférési** hello irányítópult szakasza.</span><span class="sxs-lookup"><span data-stu-id="94e2e-110">Select hello **Access reviews** section of hello dashboard.</span></span>
3. <span data-ttu-id="94e2e-111">Válassza ki a megjeleníteni kívánt toomanage hello áttekintése.</span><span class="sxs-lookup"><span data-stu-id="94e2e-111">Select hello access review that you want toomanage.</span></span>

<span data-ttu-id="94e2e-112">Hello hozzáférés felülvizsgálati részletei panel lehetőség áll rendelkezésre egy szám felülvizsgálat kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="94e2e-112">On hello access review's detail blade there are a number options for managing that review.</span></span>

![PIM hozzáférés felülvizsgálati gombok – képernyőkép][1]

### <a name="remind"></a><span data-ttu-id="94e2e-114">Emlékeztesse</span><span class="sxs-lookup"><span data-stu-id="94e2e-114">Remind</span></span>
<span data-ttu-id="94e2e-115">Ha egy áttekintése be van állítva, hogy önmagukat hello felhasználók tekintse át, hello **emlékeztetése** gomb értesítést küld.</span><span class="sxs-lookup"><span data-stu-id="94e2e-115">If an access review is set up so that hello users review themselves, hello **Remind** button sends out a notification.</span></span> 

### <a name="stop"></a><span data-ttu-id="94e2e-116">Leállítás</span><span class="sxs-lookup"><span data-stu-id="94e2e-116">Stop</span></span>
<span data-ttu-id="94e2e-117">Hozzáférés az összes értékelést rendelkeznek befejező dátumát, de használhatja hello **leállítása** gomb toofinish korai azt.</span><span class="sxs-lookup"><span data-stu-id="94e2e-117">All access reviews have an end date, but you can use hello **Stop** button toofinish it early.</span></span> <span data-ttu-id="94e2e-118">Bármely felhasználó még nem ellenőrzött időpontig, ha azok nem lesznek képesek tooafter hello felülvizsgálati le.</span><span class="sxs-lookup"><span data-stu-id="94e2e-118">If any users haven't been reviewed by this time, they won't be able tooafter you stop hello review.</span></span> <span data-ttu-id="94e2e-119">Nem indítható újra áttekintése után a rendszer leállt.</span><span class="sxs-lookup"><span data-stu-id="94e2e-119">You cannot restart a review after it's been stopped.</span></span>

### <a name="apply"></a><span data-ttu-id="94e2e-120">Jelentkezés</span><span class="sxs-lookup"><span data-stu-id="94e2e-120">Apply</span></span>
<span data-ttu-id="94e2e-121">Egy áttekintése után, vagy mert hello záró dátum érhető el, vagy manuálisan, megszakította hello **alkalmaz** gomb megvalósítja hello hello felülvizsgálati eredményeit.</span><span class="sxs-lookup"><span data-stu-id="94e2e-121">After an access review is completed, either because you reached hello end date or stopped it manually, hello **Apply** button implements hello outcome of hello review.</span></span> <span data-ttu-id="94e2e-122">Ha a felhasználó hozzáférése megtagadva hello felülvizsgálat alatt, akkor hello egy lépést, amely eltávolítja a szerepkör-hozzárendelés.</span><span class="sxs-lookup"><span data-stu-id="94e2e-122">If a user's access was denied in hello review, this is hello step that will remove their role assignment.</span></span>  

### <a name="export"></a><span data-ttu-id="94e2e-123">Exportálás</span><span class="sxs-lookup"><span data-stu-id="94e2e-123">Export</span></span>
<span data-ttu-id="94e2e-124">Ha manuálisan tooapply hello eredményét hello biztonsági felülvizsgálat, exportálhatja hello áttekintése.</span><span class="sxs-lookup"><span data-stu-id="94e2e-124">If you want tooapply hello results of hello security review manually, you can export hello review.</span></span> <span data-ttu-id="94e2e-125">Hello **exportálása** gomb indul el egy CSV-fájl letöltése.</span><span class="sxs-lookup"><span data-stu-id="94e2e-125">hello **Export** button will start downloading a CSV file.</span></span> <span data-ttu-id="94e2e-126">Hello elmulasztása az Excel vagy más programok, nyissa meg a CSV-fájlok is kezelheti.</span><span class="sxs-lookup"><span data-stu-id="94e2e-126">You can manage hello results in Excel or other programs that open CSV files.</span></span>

### <a name="delete"></a><span data-ttu-id="94e2e-127">Törlés</span><span class="sxs-lookup"><span data-stu-id="94e2e-127">Delete</span></span>
<span data-ttu-id="94e2e-128">Ha nem szeretné a hello további átnézni, törölje azt.</span><span class="sxs-lookup"><span data-stu-id="94e2e-128">If you are not interested in hello review any further, delete it.</span></span> <span data-ttu-id="94e2e-129">Hello **törlése** gomb hello felülvizsgálati eltávolítása hello PIM alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="94e2e-129">hello **Delete** button removes hello review from hello PIM application.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="94e2e-130">Akkor lesz nem megjelenik egy figyelmeztetés, még törlése előtt, ezért toodelete, tekintse át kívánja.</span><span class="sxs-lookup"><span data-stu-id="94e2e-130">You will not get a warning before deletion occurs, so be sure that you want toodelete that review.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="94e2e-131">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="94e2e-131">Next steps</span></span>
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-complete-review/PIM_review_buttons.png
