---
title: "kiépítés értesítések aaaAccount |} Microsoft Docs"
description: "Ismerje meg, hogyan tooensure, hogy értesítést küld a problémák kapcsolódó toouser kiépítés figyelmet igénylő fiók értesítések kiépítés engedélyezése."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: a637aac7-f06b-48ef-a66d-639835a8edec
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: markvi
ms.openlocfilehash: e33d1dd806fff43fc96f843a9dcddd7375d2e3c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="account-provisioning-notifications"></a><span data-ttu-id="0f309-103">Alkalmazáskiépítési értesítések</span><span class="sxs-lookup"><span data-stu-id="0f309-103">Account Provisioning Notifications</span></span>
<span data-ttu-id="0f309-104">A felhasználók átadása, automatizálhatja azokat hello folyamat a felhasználók a külső gyártótól származó SaaS-alkalmazások kezelése.</span><span class="sxs-lookup"><span data-stu-id="0f309-104">With user provisioning, you can automate hello process of managing users in third party SaaS applications.</span></span> <br>
<span data-ttu-id="0f309-105">Ez nem egy automatikus folyamat, ezt a folyamatot a beavatkozás szükséges idő tootime származik.</span><span class="sxs-lookup"><span data-stu-id="0f309-105">While this is an automated process, your interaction with this process is from time tootime required.</span></span> <br>
<span data-ttu-id="0f309-106">Ez helyzet, például hello, amikor hello jelszavát hello tooexchange adatok konfigurálása és a harmadik féltől származó SaaS-alkalmazás lejárt.</span><span class="sxs-lookup"><span data-stu-id="0f309-106">This is, for example hello case, when hello password of hello account you have configured tooexchange data with a third party SaaS application has expired.</span></span> 

<span data-ttu-id="0f309-107">Azáltal, hogy a fiók kiépítése értesítések, biztosíthatja, hogy értesítést küld a problémák kapcsolódó toouser kiépítés figyelmet igénylő.</span><span class="sxs-lookup"><span data-stu-id="0f309-107">By enabling account provisioning notifications, you can ensure that you are notified of issues related toouser provisioning that require your attention.</span></span>

<span data-ttu-id="0f309-108">Aktiválása vagy inaktiválása fiók értesítések kiépítése a felhasználók egy harmadik féltől származó SaaS-alkalmazás konfigurációja átadásához részeként.</span><span class="sxs-lookup"><span data-stu-id="0f309-108">You activate or deactivate account provisioning notifications as part of your user provisioning configuration for a third party SaaS application.</span></span>

![A felhasználók átadása][1] 

<span data-ttu-id="0f309-110">tooactivate fiók létesítési értesítések, válassza ki a hello kapcsolódó jelölőnégyzet hello **megerősítő** párbeszédpanel lapon, és a típus hello hello címzett e-mail aliasát.</span><span class="sxs-lookup"><span data-stu-id="0f309-110">tooactivate account provisioning notifications, select hello related checkbox on hello **Confirmation** dialog page, and then type hello email alias of hello recipient.</span></span>

![Alkalmazáskiépítési értesítések][2]

<span data-ttu-id="0f309-112">Megadhat terjesztési lista címzettjeként; azt azonban fontos, hogy hello értesítő e-mailt tartalmaz, amelyek csak az Azure AD hello rendszergazdák által elérhető hivatkozások tooreports toonote.</span><span class="sxs-lookup"><span data-stu-id="0f309-112">You can enter a distribution list as recipient; however, it is important toonote that hello notification email contains links tooreports that are only accessible by hello Azure AD administrators.</span></span>

<span data-ttu-id="0f309-113">Kiépítés értesítések engedélyezett fiókkal rendelkezik, ha kritikus problémákra, amelyek a kapcsolódó toouser kiépítés vonatkozó e-mailt fog kapni.</span><span class="sxs-lookup"><span data-stu-id="0f309-113">If you have account provisioning notifications enabled, you will receive emails about critical issues that are related toouser provisioning.</span></span> <span data-ttu-id="0f309-114">Azonban tooavoid e-mail túlterhelés, csak kapni fog egy e-mailben értesítést naponta minden szoftverszolgáltatások alkalmazás hello értesítő e-mailt engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="0f309-114">However, tooavoid an email overload, you will only receive one notification email per day for each SaaS application hello notification email is enabled for.</span></span>

## <a name="related-articles"></a><span data-ttu-id="0f309-115">Kapcsolódó cikkek</span><span class="sxs-lookup"><span data-stu-id="0f309-115">Related Articles</span></span>
* [<span data-ttu-id="0f309-116">Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke</span><span class="sxs-lookup"><span data-stu-id="0f309-116">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="0f309-117">Felhasználói kiépítési/megszüntetés tooSaaS alkalmazások automatizálása</span><span class="sxs-lookup"><span data-stu-id="0f309-117">Automate User Provisioning/Deprovisioning tooSaaS Apps</span></span>](active-directory-saas-app-provisioning.md)
* [<span data-ttu-id="0f309-118">A felhasználók átadása attribútum-leképezésekhez testreszabása</span><span class="sxs-lookup"><span data-stu-id="0f309-118">Customizing Attribute Mappings for User Provisioning</span></span>](active-directory-saas-customizing-attribute-mappings.md)
* [<span data-ttu-id="0f309-119">Attribútum-leképezésekhez kifejezések írása</span><span class="sxs-lookup"><span data-stu-id="0f309-119">Writing Expressions for Attribute Mappings</span></span>](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [<span data-ttu-id="0f309-120">Helyezése Hatókörszűrőkkel felhasználói történő üzembe helyezéséhez</span><span class="sxs-lookup"><span data-stu-id="0f309-120">Scoping Filters for User Provisioning</span></span>](active-directory-saas-scoping-filters.md)
* [<span data-ttu-id="0f309-121">SCIM tooenable automatikus kiépítés a felhasználók és csoportok az Azure Active Directory tooapplications használatával</span><span class="sxs-lookup"><span data-stu-id="0f309-121">Using SCIM tooenable automatic provisioning of users and groups from Azure Active Directory tooapplications</span></span>](active-directory-scim-provisioning.md)
* [<span data-ttu-id="0f309-122">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz</span><span class="sxs-lookup"><span data-stu-id="0f309-122">List of Tutorials on How tooIntegrate SaaS Apps</span></span>](active-directory-saas-tutorial-list.md)

<!--Image references-->
[1]: ./media/active-directory-saas-account-provisioning-notifications/ic766307.png
[2]: ./media/active-directory-saas-account-provisioning-notifications/ic766308.png
