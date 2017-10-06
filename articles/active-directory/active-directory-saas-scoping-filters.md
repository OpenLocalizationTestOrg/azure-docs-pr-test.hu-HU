---
title: "aaaProvisioning alkalmazások helyezése hatókörszűrőkkel |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hatókörének szűrők automatizált felhasználókiépítése a ténylegesen létre Ha az objektum nem elégíti ki az üzleti igényeknek támogató alkalmazások tooprevent objektumokat."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: bcfbda74-e4d4-4859-83bc-06b104df3918
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: markvi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f0299390dc3fdb70aa9d271e835069a08827d635
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="attribute-based-application-provisioning-with-scoping-filters"></a><span data-ttu-id="9c364-103">Alkalmazások Attribútumalapú üzembe helyezése hatókörszűrőkkel</span><span class="sxs-lookup"><span data-stu-id="9c364-103">Attribute-based application provisioning with scoping filters</span></span>
<span data-ttu-id="9c364-104">hello ebben a szakaszban célja tooexplain hogyan toouse hatókörének szűrők toodefine Attribútumalapú szabályok, amelyek meghatározzák, hogy mely felhasználók vannak kiépítve toohello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="9c364-104">hello objective of this section is tooexplain how toouse scoping filters toodefine attribute-based rules that determine which users are provisioned toohello application.</span></span>

## <a name="clauses-and-scope-groups"></a><span data-ttu-id="9c364-105">Feltételek és hatókör</span><span class="sxs-lookup"><span data-stu-id="9c364-105">Clauses and Scope Groups</span></span>
![Hatókör-beállítási szűrője][1] 

<span data-ttu-id="9c364-107">Hatókörként szűrők határozzák meg egy vagy több **csoportok hatókör**, minden egyes, amely tartalmaz egy vagy több **záradékok**.</span><span class="sxs-lookup"><span data-stu-id="9c364-107">Scoping filters are defined by one or more **scope groups**, each of which hold one or more **clauses**.</span></span> <span data-ttu-id="9c364-108">egy adott hatókör csoport toosee hello záradékok hello toohello balra nyíl hello csoportnév kattintva bontsa ki.</span><span class="sxs-lookup"><span data-stu-id="9c364-108">toosee hello clauses for a particular scope group, expand it by clicking hello arrow toohello left of hello group name.</span></span>

<span data-ttu-id="9c364-109">A **záradék** határozza meg, hogy mely felhasználók vannak toopass hatókör-beállítási szűrője minden felhasználói attribútumok kiértékelésével hello keresztül.</span><span class="sxs-lookup"><span data-stu-id="9c364-109">A **clause** determines which users are allowed toopass through hello scoping filter by evaluating each user’s attributes.</span></span> <span data-ttu-id="9c364-110">Például lehetséges, hogy egy feltételt, amely megköveteli, hogy a felhasználó a "state" attribútum egyenlő New York közt, így csak a győri felhasználók kiépített hello alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="9c364-110">For example, you might have one clause that requires that a user’s ‘state’ attribute equal New York, so only New York users are provisioned into hello application.</span></span>

![Hatókör neve][2] 

<span data-ttu-id="9c364-112">Minden egyes **hatókör csoport** kezdődik, egy kötelező **záradék**, a fenti hello képernyőfelvételen látható módon.</span><span class="sxs-lookup"><span data-stu-id="9c364-112">Each **scope group** starts with one mandatory **clause**, as shown in hello screenshot above.</span></span> <span data-ttu-id="9c364-113">Ehhez a záradékhoz egyszerűen szerint hello felhasználó először meg kell adni toohello alkalmazás által a tartalmazó szűrők értékeléséhez.</span><span class="sxs-lookup"><span data-stu-id="9c364-113">This clause simply states that hello user must first be assigned toohello application before it’s evaluated by your scoping filters.</span></span> <span data-ttu-id="9c364-114">Ehhez a záradékhoz nem törölték és nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="9c364-114">This clause cannot be deleted or modified.</span></span>

<span data-ttu-id="9c364-115">Hozzáadhat új kikötéseket vagy új hatókört csoportok hello megfelelő gomb lenyomásával.</span><span class="sxs-lookup"><span data-stu-id="9c364-115">You can add new clauses or new scope groups by pressing hello appropriate button.</span></span> <span data-ttu-id="9c364-116">Minden hatókör csoport egy nevet adhat szerkesztésével a **hatókör csoportnév** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="9c364-116">You can give each scope group a name by editing its **Scope Group Name** property.</span></span>

## <a name="how-scoping-filters-are-evaluated"></a><span data-ttu-id="9c364-117">Hogyan hatókörének szűrők kiértékelése</span><span class="sxs-lookup"><span data-stu-id="9c364-117">How Scoping Filters are Evaluated</span></span>
<span data-ttu-id="9c364-118">Telepítése során, teszteljük minden hozzárendelt felhasználó a tartalmazó szűrők toodetermine szemben ha, hogy a felhasználó érdemel access toohello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="9c364-118">During provisioning, we test every assigned user against your scoping filters toodetermine if that user deserves access toohello application.</span></span> <span data-ttu-id="9c364-119">Tulajdonképpen minden záradékot, hogy egy tesztet, amely ahhoz, hogy hello felhasználói tooavoid első kiszűri kell átadni.</span><span class="sxs-lookup"><span data-stu-id="9c364-119">You can think of each clause as being a test that must be passed in order for hello user tooavoid getting filtered out.</span></span> 

<span data-ttu-id="9c364-120">Ha több hatókör csoportok definiálva van, minden felhasználóhoz meg kell felelnie legalább egyik tooaccess hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="9c364-120">If you have multiple scope groups defined, each user must pass at least one of them tooaccess hello application.</span></span> <span data-ttu-id="9c364-121">Minden hatókör csoporton belül azonban hello felhasználónak meg kell felelnie minden záradék toopass adott hatókör csoporthoz.</span><span class="sxs-lookup"><span data-stu-id="9c364-121">Within each scope group, however, hello user must pass every clause toopass that specific scope group.</span></span> 

<span data-ttu-id="9c364-122">Más szóval az eltolásokat tekintheti, hogy a hatókört csoportok volna együtt, illetve, hogy bennük hello záradékok tulajdonképpen és volna együtt.</span><span class="sxs-lookup"><span data-stu-id="9c364-122">In other words, you can think of scope groups as being OR’d together, and you can think of hello clauses within them as being AND’d together.</span></span> <span data-ttu-id="9c364-123">Vegye figyelembe például a hatókör-beállítási szűrője az alábbi hello:</span><span class="sxs-lookup"><span data-stu-id="9c364-123">For example, consider hello scoping filter below:</span></span>

![Hatókör neve][3]  

<span data-ttu-id="9c364-125">Hatókör-beállítási szűrője toothis szerint, felhasználók meg kell felelnie a következő hello feltételek, a kiépített toobe:</span><span class="sxs-lookup"><span data-stu-id="9c364-125">According toothis scoping filter, users must satisfy hello following criteria, toobe provisioned:</span></span>

1. <span data-ttu-id="9c364-126">Ezek hozzá kell rendelni toohello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="9c364-126">They must be assigned toohello application.</span></span>
2. <span data-ttu-id="9c364-127">Hello mérnöki részleg kell működnek</span><span class="sxs-lookup"><span data-stu-id="9c364-127">They must work in hello Engineering department</span></span>
3. <span data-ttu-id="9c364-128">Munkahelyi kell lenniük a San Francisco vagy Kanada.</span><span class="sxs-lookup"><span data-stu-id="9c364-128">They must be work in either San Francisco or Canada.</span></span>

## <a name="related-articles"></a><span data-ttu-id="9c364-129">Kapcsolódó cikkek</span><span class="sxs-lookup"><span data-stu-id="9c364-129">Related Articles</span></span>
* [<span data-ttu-id="9c364-130">Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke</span><span class="sxs-lookup"><span data-stu-id="9c364-130">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="9c364-131">Felhasználói kiépítés és megszüntetés tooSaaS alkalmazások automatizálása</span><span class="sxs-lookup"><span data-stu-id="9c364-131">Automate User Provisioning and Deprovisioning tooSaaS Applications</span></span>](active-directory-saas-app-provisioning.md)
* [<span data-ttu-id="9c364-132">A felhasználók átadása attribútum-leképezésekhez testreszabása</span><span class="sxs-lookup"><span data-stu-id="9c364-132">Customizing Attribute Mappings for User Provisioning</span></span>](active-directory-saas-customizing-attribute-mappings.md)
* [<span data-ttu-id="9c364-133">Attribútum-leképezésekhez kifejezések írása</span><span class="sxs-lookup"><span data-stu-id="9c364-133">Writing Expressions for Attribute Mappings</span></span>](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [<span data-ttu-id="9c364-134">Alkalmazás-kiépítési értesítések</span><span class="sxs-lookup"><span data-stu-id="9c364-134">Account Provisioning Notifications</span></span>](active-directory-saas-account-provisioning-notifications.md)
* [<span data-ttu-id="9c364-135">SCIM tooenable automatikus kiépítés a felhasználók és csoportok az Azure Active Directory tooapplications használatával</span><span class="sxs-lookup"><span data-stu-id="9c364-135">Using SCIM tooenable automatic provisioning of users and groups from Azure Active Directory tooapplications</span></span>](active-directory-scim-provisioning.md)
* [<span data-ttu-id="9c364-136">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz</span><span class="sxs-lookup"><span data-stu-id="9c364-136">List of Tutorials on How tooIntegrate SaaS Apps</span></span>](active-directory-saas-tutorial-list.md)

<!--Image references-->
[1]: ./media/active-directory-saas-scoping-filters/ic782811.png
[2]: ./media/active-directory-saas-scoping-filters/ic782812.png
[3]: ./media/active-directory-saas-scoping-filters/ic782813.png
